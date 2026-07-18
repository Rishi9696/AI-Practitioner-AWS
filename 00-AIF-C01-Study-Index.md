# AWS Certified AI Practitioner (AIF-C01) — Study Index

## Exam facts

- **50 scored questions** + 15 unscored (unidentified) = 65 total
- Scaled score **100–1000**, pass at **700**
- **Compensatory scoring** — no minimum per domain, only the overall score matters
- Question types: multiple choice, multiple response, ordering, matching
- No penalty for guessing — never leave a question blank
- Target candidate: ~6 months exposure; **uses** AI/ML on AWS rather than builds it

## Domains and notes

| Domain | Weight | Notes |
|---|---|---|
| 1 — Fundamentals of AI and ML | 20% | [Domain-1-Fundamentals-of-AI-and-ML.md](Domain-1-Fundamentals-of-AI-and-ML.md) |
| 2 — Fundamentals of GenAI | 24% | [Domain-2-Fundamentals-of-GenAI.md](Domain-2-Fundamentals-of-GenAI.md) |
| 3 — Applications of Foundation Models | 28% | [Domain-3-Applications-of-Foundation-Models.md](Domain-3-Applications-of-Foundation-Models.md) |
| 4 — Guidelines for Responsible AI | 14% | [Domain-4-Guidelines-for-Responsible-AI.md](Domain-4-Guidelines-for-Responsible-AI.md) |
| 5 — Security, Compliance, and Governance | 14% | [Domain-5-Security-Compliance-and-Governance.md](Domain-5-Security-Compliance-and-Governance.md) |

Domains 2 + 3 are **52% of the exam** — spend your time there.

---

## Cross-domain decision rules (the highest-yield page)

**Which customization approach?**
Knowledge the model lacks → **RAG**. Behavior, style, or format → **fine-tuning**. Domain vocabulary from unlabeled text → **continued pre-training**. Cheaper/faster copy of a big model → **distillation**. Try **prompt engineering** first, always.

**Which service?**
Common task with no custom model needed → **purpose-built managed service** (Transcribe, Translate, Comprehend, Rekognition, Textract, Personalize, Lex, Polly).
Generative AI without infrastructure → **Bedrock**.
Custom model, full control → **SageMaker AI**.
Ready-made assistant over company data → **Amazon Q**.

**Which inference type?**
Milliseconds, steady traffic → real-time. Huge offline volume → batch. Big payloads / long processing → async. Spiky or idle traffic, minimize cost → serverless.

**Which metric?**
False positives costly → precision. False negatives costly → recall. Both matter / imbalanced → F1. Summarization → ROUGE. Translation → BLEU. Semantic similarity → BERTScore.

**Which governance service?**
Who did what → CloudTrail. Resource configuration compliance → Config. Audit evidence → Audit Manager. AWS's own compliance reports → Artifact. PII in S3 → Macie.

**Which responsible-AI tool?**
Detect bias / explain predictions → SageMaker Clarify. Watch production drift → Model Monitor. Human review of predictions → Amazon A2I. Filter output, block topics, catch hallucinations → Bedrock Guardrails. Document the model → Model Cards.

---

## Common traps

- **Accuracy is misleading on imbalanced data** — a 99.9% accurate fraud model can catch zero fraud.
- **ML is the wrong tool** when an exact deterministic answer is required, or a simple rule works.
- **Bigger model ≠ better answer.** Right-size for cost, latency, and sustainability.
- **Model Cards ≠ AWS Artifact.** One documents your model; the other downloads AWS compliance reports.
- **Transparency ≠ explainability.**
- **RAG failures are usually retrieval failures**, not generation failures.
- **Lower temperature** fixes inconsistent/random output — not a bigger model.
- Custom (fine-tuned) models on Bedrock require **Provisioned Throughput** to host.

---

## Study plan (2–3 weeks)

1. Read Domain 1 and 2 notes; do the practice questions without looking.
2. Read Domain 3 twice — it's the largest and most detailed domain.
3. Read Domains 4 and 5; drill the service one-liner tables until automatic.
4. Re-do all practice questions across the five files; note every miss.
5. Take AWS's official practice question set and any full-length practice exam.
6. Final pass: only the Quick Revision Checklists and this index page.

**Source:** [AWS Certified AI Practitioner (AIF-C01) Exam Guide](https://docs.aws.amazon.com/aws-certification/latest/ai-practitioner-01/ai-practitioner-01.html)
