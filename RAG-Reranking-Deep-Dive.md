# RAG Retrieval & Reranking — Deep Dive (Interview / Practical Notes)

**Companion to:** Domain-2.1-Core-GenAI-Concepts-Deep-Dive.md (see its "Context assembly & ordering, function context, relevance filtering/re-ranking" section for the exam-level summary) and Domain-3-Applications-of-Foundation-Models.md (RAG basics, prompt engineering).

This file goes beyond AIF-C01 exam scope into practical/interview-level depth on how real RAG retrieval and reranking pipelines are built — bi-encoders vs. cross-encoders, reranking technique families, and worked examples of the trickier mechanics (tournament sorting, ColBERT, RRF).

---

## 1. Bi-encoder vs. cross-encoder — the foundational distinction

**Bi-encoder** — encodes the query and each document **separately and independently** into their own vectors. Similarity is computed *afterward* via cosine similarity / dot product.

```
Query  → Encoder → Vector Q
Doc 1  → Encoder → Vector D1        similarity(Q, D1)
Doc 2  → Encoder → Vector D2        similarity(Q, D2)
Doc 3  → Encoder → Vector D3        similarity(Q, D3)
```

Because documents are embedded once, independent of any future query, their vectors can be **precomputed and stored** in a vector DB ahead of time. At query time you only embed the query and run a fast approximate-nearest-neighbor (ANN) search against millions of stored vectors. Fast, scalable — but the model never sees query and document together, so it can miss subtle relevance signals.

Examples: OpenAI `text-embedding-3-small`/`text-embedding-3-large`, Sentence-BERT, Cohere Embed.

**OpenAI's embeddings are bi-encoders.** `embed("chunk text")` and `embed("query text")` are the exact same API call — the model has no idea, at embedding time, what it will later be compared against. That's what makes it a bi-encoder by definition, not a design choice you can toggle.

**Cross-encoder** — feeds the query and **one** document **together, concatenated**, into a single model pass. The model attends across both texts at once and outputs one relevance score for that pair.

```
[Query + Document] → one model (reads both together) → relevance score (e.g. 0.92)
```

Far more accurate (it directly compares specific words/phrases across query and document via self-attention), but nothing can be precomputed — every query-document pair needs a fresh forward pass. Too slow to run against millions of documents.

Examples: Cohere Rerank, BGE-reranker, MS MARCO cross-encoders.

**Why RAG uses both, in two stages:**

```
Stage 1 — Retrieval (bi-encoder, cheap, recall-oriented)
  Query → embed → vector search over WHOLE corpus (millions of chunks) → top ~20-50 candidates

Stage 2 — Reranking (cross-encoder, expensive, precision-oriented)
  Query + each of the ~20-50 candidates → cross-encoder scores each pair → keep top 3-5
```

**Interview one-liner:** "A bi-encoder embeds query and document independently and compares vectors afterward — fast, scalable, used for initial retrieval. A cross-encoder processes query and document together in one pass and outputs a direct relevance score — slower but far more accurate, so it's only applied to the small shortlist from the bi-encoder, as the reranking step."

---

## 2. Practical RAG pipeline using both

```python
from openai import OpenAI
from sentence_transformers import CrossEncoder

client = OpenAI()
reranker = CrossEncoder("cross-encoder/ms-marco-MiniLM-L-6-v2")  # or BAAI/bge-reranker-base

def embed(text):
    return client.embeddings.create(model="text-embedding-3-small", input=text).data[0].embedding

# --- Ingestion (once) ---
for chunk in chunks:
    vector = embed(chunk.text)
    pinecone_index.upsert([(chunk.id, vector, {"text": chunk.text})])

# --- Query time ---
query_vector = embed(user_question)
results = pinecone_index.query(vector=query_vector, top_k=20, include_metadata=True)   # stage 1: wide net
candidates = [r.metadata["text"] for r in results.matches]

pairs = [[user_question, c] for c in candidates]
scores = reranker.predict(pairs)                                                       # stage 2: precise rescore
top_chunks = [c for c, s in sorted(zip(candidates, scores), key=lambda x: x[1], reverse=True)[:5]]

context = "\n\n".join(top_chunks)
prompt = f"Context:\n{context}\n\nQuestion: {user_question}\nAnswer:"
answer = llm.generate(prompt)
```

**Tuning notes:** retrieve wider at stage 1 (top 15-30) precisely because stage 2 exists to clean it up. Only rerank the shortlist, never the whole corpus. Add reranking when you observe wrong-but-similar chunks winning; skip it if latency matters more than that last bit of precision. LangChain/LlamaIndex ship this as a built-in "compression retriever" / reranker wrapper.

---

## 3. Reranking technique families (beyond plain cross-encoders)

| Technique | How it works | Cost | When to use |
|---|---|---|---|
| **Cross-encoder** | Small dedicated model scores query+doc pairs directly | Low-medium | Default choice for most production RAG |
| **LLM reranking (pointwise/pairwise/listwise)** | General LLM judges relevance | High | Quality-critical domains (legal, medical) |
| **ColBERT (late interaction)** | Per-token vectors + MaxSim at search time | Medium | Want cross-encoder-like accuracy, bi-encoder-like speed |
| **Reciprocal Rank Fusion (RRF)** | Merge rankings from multiple retrievers via formula | Very low (no model) | Hybrid search (vector + keyword/BM25) |
| **Learning-to-rank (LTR)** | Trained model (XGBoost) on business features | Low at inference | Mature search with business signals (recency, authority) |
| **Rule-based / metadata boosting** | Deterministic rules on top of any ranker | Free | Layer on top of any of the above |

### 3a. LLM-based reranking — pointwise, pairwise, listwise

**Pointwise:** LLM scores each candidate individually (e.g., "Rate relevance 0-10"), sort by score. Simple, but N separate LLM calls — slow/costly at scale.

**Pairwise:** LLM compares two chunks at a time, picks the more relevant one; ranking is built from many such comparisons.

**Pairwise, worked example — the tie problem:**

4 candidates (A, B, C, D) for query "How do I reset my AWS root account password?" Round-robin, every pair compared once (O(N²), 6 comparisons for N=4):

```
A vs B → B wins
A vs C → A wins
A vs D → A wins
B vs C → C wins
B vs D → B wins
C vs D → C wins
```

Tally wins: **A=2, B=2, C=2, D=0.** D is clearly last, but A, B, C are tied. Looking closer: A beats C, C beats B, B beats A — a **cycle** (intransitivity), like rock-paper-scissors. There is no true "best" among the three; simple win-counting cannot resolve it because the judgments themselves are cyclic, not because of a tie-breaking bug.

**How this actually gets resolved in practice:**
- Fall back to the original bi-encoder similarity score as a tiebreaker.
- Run repeated comparisons (majority vote across multiple LLM calls) — cycles are often just judge noise/inconsistency, not genuine circular preference.
- Use **Elo rating** or the **Bradley-Terry model** instead of raw win-counting — these estimate a continuous "strength" score from pairwise outcomes and produce a best-fit ranking even when raw results are inconsistent.
- This fragility is exactly why pairwise reranking is rare in production — it needs either a lot of extra comparisons or a smarter aggregator to be reliable.

**Merge-sort-style tournament (reduces comparisons from O(N²) to ~O(N log N)):**

For N=8, number of rounds = **log₂(8) = 3**:

```
Round 1 (merge singles → pairs):
  [A] vs [B] → 1 cmp   [C] vs [D] → 1 cmp   [E] vs [F] → 1 cmp   [G] vs [H] → 1 cmp
  Total: 4 comparisons

Round 2 (merge pairs → groups of 4):
  [A,B]+[C,D] → up to 3 cmp    [E,F]+[G,H] → up to 3 cmp
  Total: up to 6 comparisons

Round 3 (merge groups of 4 → full list of 8):
  [A,B,C,D] + [E,F,G,H] → up to 7 comparisons

Grand total: 3 rounds, ~17 comparisons  (vs. N²=64 for full round-robin, or N(N-1)/2=28 for round-robin without duplicates)
```

Note: a **single-elimination bracket** (only finding the #1 item) needs just N−1 = 7 comparisons over the same 3 rounds — cheaper, but it doesn't produce a full ranking, only a winner. Merge-sort style is needed when the full order matters (as it does for reranking, where you want top-5, not just top-1).

**Listwise (most common in practice — RankGPT):**

Instead of scoring or pairwise-comparing, give the LLM the **entire candidate list in one prompt** and ask it to output the full ranked order directly.

```
Prompt:
Query: "How do I reset my AWS root account password?"
Passages:
[1] IAM user permissions overview
[2] Root account recovery steps
[3] MFA device setup
[4] Billing alert configuration

Rank these passages by relevance to the query, most relevant first.
Output: [2] > [3] > [1] > [4]
```

**RankGPT** is exactly this technique (2023 paper) — using a general LLM (GPT-3.5/4) with zero fine-tuning as a listwise reranker. Since long candidate lists don't fit one context window, RankGPT uses a **sliding window**: rank ~10-20 passages at a time, slide across the full list, merge partial rankings — similar in spirit to a bubble-sort pass with a moving window. It has shown competitive or better results than specialized cross-encoders on some benchmarks, at the cost of much higher latency/expense per query (full LLM call(s) vs. one lightweight forward pass).

### 3b. ColBERT — late interaction

**Full form: ColBERT = Contextualized Late Interaction over BERT.**

Regular bi-encoders compress a whole chunk into **one** vector, losing granularity. ColBERT keeps **one vector per token**, for both query and document, and only compares them at search time ("late interaction") instead of upfront.

```
Query: "root account password reset"
  → tokenize + embed each token → [v_root, v_account, v_password, v_reset]

Document: "To recover your root account, reset the password via..."
  → tokenize + embed each token → [v_to, v_recover, v_your, v_root, v_account, v_reset, v_the, v_password, v_via, ...]
```

**MaxSim** — for each query token, find its single best-matching token anywhere in the document, then sum those best-match scores across all query tokens:

```
MaxSim(v_root)     = best similarity(v_root, any doc token)      → matches doc's "root"
MaxSim(v_account)  = best similarity(v_account, any doc token)   → matches doc's "account"
MaxSim(v_password) = best similarity(v_password, any doc token)  → matches doc's "password"
MaxSim(v_reset)    = best similarity(v_reset, any doc token)     → matches doc's "reset"

Total score = sum of the four MaxSim values
```

**Cost tradeoff — where ColBERT sits between bi-encoder and cross-encoder:**

```
Bi-encoder:    1 vector vs 1 vector             → cheapest, least accurate
ColBERT:       N query tokens × M doc tokens    → heavier compute, but doc token vectors PRECOMPUTED
Cross-encoder: fresh transformer pass per pair  → most expensive, most accurate, nothing precomputed
```

Yes — ColBERT is more expensive than a bi-encoder, because instead of one dot product per document, you're doing up to N×M token-level similarity computations (e.g., 5 query tokens × 50 doc tokens = 250 comparisons for one document). But it's still cheaper than a true cross-encoder because the expensive part — running the transformer — happens **offline, once, per document, at ingestion time**. At query time you only run the transformer once (on the query) and do fast vector math (MaxSim) against already-stored document token vectors. A cross-encoder, by contrast, must run a full live transformer pass on query+document together, for every candidate, every query, with zero precomputation possible.

### 3c. Reciprocal Rank Fusion (RRF)

Used to merge rankings from **multiple different retrievers** (e.g., vector/semantic search + BM25/keyword search) into one final ranking, with no trained model needed at all.

```
RRF_score(doc) = Σ  1 / (k + rank_i(doc))     [summed across every ranker i the doc appears in, k≈60]
```

**Worked example** — query "AWS root password reset":

```
Vector search:  [1] Root recovery doc   [2] IAM overview       [3] MFA setup
Keyword (BM25): [1] MFA setup           [2] Root recovery doc  [3] Billing FAQ

RRF(Root recovery doc) = 1/(60+1) + 1/(60+2) = 0.0164 + 0.0161 = 0.0325   ← ranks well in BOTH
RRF(MFA setup)          = 1/(60+3) + 1/(60+1) = 0.0159 + 0.0164 = 0.0323   ← also strong
RRF(IAM overview)       = 1/(60+2) + 0                          = 0.0161   ← only in one list
```

Sort by RRF score for the final fused ranking. Key idea: a document ranked well by *multiple independent signals* rises to the top even if no single retriever put it at #1 — often more robust than trusting one retriever type. Popular because it costs nothing but arithmetic on rankings you already have.

### 3d. BGE reranker

**BGE (BAAI General Embedding)** is an open-source model family from the Beijing Academy of Artificial Intelligence. **BGE-reranker** (`bge-reranker-base`, `bge-reranker-large`, `bge-reranker-v2-m3`) is a **cross-encoder** — same role as `ms-marco-MiniLM`, different training/model family.

```python
from sentence_transformers import CrossEncoder
reranker = CrossEncoder("BAAI/bge-reranker-base")
scores = reranker.predict([[query, chunk] for chunk in candidates])
```

Why people choose it: free, open-source, self-hostable (no API dependency/cost like Cohere Rerank), multilingual variants (`bge-reranker-v2-m3` covers 100+ languages), and competitive with commercial rerankers on standard benchmarks (MTEB, MS MARCO). Good default when you want reranking on your own infrastructure.

### 3e. Learning-to-rank (LTR) and rule-based reranking

**LTR** — train a lightweight model (XGBoost/LightGBM) on features beyond text similarity: recency, source authority, click-through history, document length, exact keyword overlap. Common in mature search systems (e-commerce, enterprise search) where relevance depends on business signals, not just semantics.

**Rule-based/metadata boosting** — simple deterministic rules layered on top of any ranker above: boost recent documents, deprioritize deprecated docs, boost exact keyword matches. Cheap, predictable, usually combined with a model-based reranker rather than replacing it.

---

## 4. Practical recommendation

Start with a **cross-encoder** (Cohere Rerank hosted, or BGE-reranker self-hosted) — best cost/quality ratio for most RAG. Move to **listwise LLM reranking (RankGPT-style)** only if you've measured that cross-encoders miss nuance the LLM would catch — it costs meaningfully more per query. Add **RRF** only if combining vector + keyword search. Reach for **ColBERT** if you want cross-encoder-level nuance at bi-encoder-like query-time cost and can afford the extra storage (per-token vectors instead of one vector per chunk).

---

## 5. Bonus — encoder-only / decoder-only / encoder-decoder, tied to reranking and generation

(Full version lives in Domain-2.1's "Three transformer architecture families" section — summarized here for RAG-specific relevance.)

- **Encoder-only** (BERT-family) → powers embeddings (bi-encoder) and cross-encoder reranking. Understands, doesn't generate long text.
- **Decoder-only** (GPT/Llama-family) → the generation model in most modern RAG stacks: `question + retrieved context → answer`, generated token-by-token (autoregressive).
- **Encoder-decoder** (T5/FLAN-T5/BART) → an alternative generation model for RAG, well suited to constrained seq2seq answer generation, translation, summarization. `Encoder reads question+context → Decoder generates answer.`

```
RAG = Retrieval (chunk → embed [bi-encoder] → vector DB → retrieve → optionally rerank [cross-encoder/ColBERT/LLM])
      + Generation (question + context → Decoder-only LLM  OR  Encoder-Decoder model → answer)
```
