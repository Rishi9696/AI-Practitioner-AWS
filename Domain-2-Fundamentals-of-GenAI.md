# Domain 2: Fundamentals of GenAI
**AIF-C01 · 24% of scored content · ~12 of 50 scored questions**

Three task statements: core GenAI concepts (2.1), capabilities and limitations (2.2), AWS GenAI infrastructure (2.3).

---

## 2.1 Core GenAI Concepts

### How an LLM actually sees your text

```
"AWS is great"  →  tokens ["AWS", " is", " great"]  →  embeddings [0.23, -0.81, ...]  →  transformer  →  next token
```

| Term | Meaning | Rule of thumb |
|---|---|---|
| **Token** | The unit a model reads/writes — a word, part of a word, or punctuation | ~1 token ≈ 4 characters ≈ ¾ of an English word; 1,000 tokens ≈ 750 words |
| **Embedding** | A numeric vector representing meaning | "king" and "queen" land close together in vector space |
| **Vector** | The list of numbers itself | Stored in a vector database for similarity search |
| **Chunking** | Splitting a long document into smaller pieces before embedding | A 200-page PDF → 500-token chunks so retrieval returns only relevant parts |
| **Context window** | Max tokens the model can consider at once (prompt + response) | Exceed it and earlier content is truncated |

### Model families

- **Transformer LLMs** — the architecture behind modern text models. The **self-attention** mechanism lets the model weigh every token against every other token, and it processes tokens **in parallel** (unlike older RNNs, which were sequential). This is why LLMs scale.
- **Foundation model (FM)** — a large model pre-trained on broad data, adaptable to many downstream tasks. One model, many jobs.
- **Multimodal model** — accepts/produces more than one modality (text + image + audio). *Example:* upload a chart image and ask "what's the trend?"
- **Diffusion model** — generates images by starting from noise and iteratively denoising toward the prompt. *This is the exam's image-generation answer* (Stable Diffusion, Amazon Nova Canvas).

### GenAI use cases

Text summarization · content generation (marketing copy, emails) · chatbots and AI assistants · code generation · translation · semantic search · image/video/audio generation · question answering over documents · sentiment and intent analysis · recommendation engines · customer service agents.

### The FM lifecycle

1. **Data selection** — assemble massive, diverse, curated corpora.
2. **Model selection** — pick architecture, or pick an existing FM to build on.
3. **Pre-training** — self-supervised learning on unlabeled data. *Most expensive step by far* (millions of dollars, weeks of GPU time).
4. **Fine-tuning** — adapt to specific tasks/domains with smaller labeled datasets.
5. **Evaluation** — benchmarks, human review, task-specific metrics.
6. **Deployment** — serve via managed API or self-hosted endpoint.
7. **Feedback** — collect real usage, then loop back (RLHF, retraining).

### Token-based pricing

You pay per **input token + output token**, usually at different rates. Practical consequences:

- Long prompts, big RAG context, and verbose outputs all directly raise cost.
- **Latency scales with output tokens** — generation is sequential, so capping max output length is the main latency lever.
- **Prompt caching** reuses a repeated prefix (e.g., a long system prompt or document) across calls at reduced cost and latency.
- Larger models = higher per-token price. Right-size the model to the task; use a small model for classification, a large one for complex reasoning.

**Bedrock pricing modes:**

| Mode | How it works | When |
|---|---|---|
| **On-demand** | Pay per token, no commitment | Variable/low volume, experimentation |
| **Batch** | Submit jobs asynchronously, discounted | Large offline workloads |
| **Provisioned Throughput** | Buy dedicated capacity for a time commitment | Steady high volume, guaranteed throughput, **required for custom models** |

### Context engineering

Broader than prompt engineering: deciding **what information the model sees** at inference time and how it is organized — system instructions, retrieved documents, conversation history, tool definitions, and output format. Since the context window is finite and every token costs money, the goal is getting the *right* context in, not the *most* context. Poor context engineering causes hallucinations, cost blowout, and the model losing track of long conversations.

### Agentic AI concepts

An **AI agent** perceives a goal, **plans** the steps, **uses tools** to act, observes results, and iterates until done.

| Concept | What it is |
|---|---|
| **Tool use** | The agent calls external functions/APIs (search a DB, send an email, run code) |
| **Memory** | Short-term (current conversation) vs. long-term (persisted facts across sessions) |
| **Orchestration** | Managing the loop: plan → act → observe → repeat, with error handling |
| **MCP (Model Context Protocol)** | An open standard for connecting agents to external tools and data sources — write the integration once, any MCP-compatible agent can use it |
| **Multi-agent systems** | Several specialized agents collaborating. Common pattern: a **supervisor/orchestrator** agent routes subtasks to worker agents, then merges results |

*Business examples:* an IT support agent that resets passwords and opens tickets; a travel agent that checks calendars, searches flights, and books; a research agent that queries multiple sources and writes a briefing.

---

## 2.2 Capabilities and Limitations

### Advantages

- **Adaptability** — one model handles many tasks with no retraining, just new prompts.
- **Responsiveness** — real-time, conversational, natural language interface.
- **Content generation** at scale — drafts, summaries, code, images.
- **Simplicity** — no ML expertise or labeled dataset required to start.
- **Speed to market** — days rather than months.

### Disadvantages — heavily tested

| Limitation | What it means | Mitigation |
|---|---|---|
| **Hallucination** | Confidently stated but false output | RAG grounding, citations, output validation, human review |
| **Nondeterminism** | Same prompt can give different answers | Lower temperature; not suitable where exact repeatability is required |
| **Interpretability** | Can't explain *why* it produced a given answer | Use traditional ML where regulators demand explanations |
| **Inaccuracy / knowledge cutoff** | Doesn't know recent events or your private data | RAG, tool use |
| **Bias and toxicity** | Reproduces bias in training data | Guardrails, bias evaluation, diverse data |
| **Cost and latency** | Token pricing adds up; large models are slow | Right-size the model, cap output, cache prompts |
| **Data privacy / IP risk** | Prompts may contain sensitive data; outputs may raise copyright questions | Encryption, PII filtering, review policies |

### Choosing a model — selection factors

Modality (text/image/multimodal) · cost per token · latency · context window size · accuracy on your task · multilingual support · model size and complexity · customization support (can it be fine-tuned?) · licensing and compliance · regional availability.

*Exam heuristic:* the "best" model is the **smallest and cheapest one that meets the requirement**, not the largest.

### Business value metrics

**Technical:** accuracy, cross-domain performance, latency, task completion rate.
**Business:** ROI, cost per interaction/user, efficiency and time saved, conversion rate, average revenue per user (ARPU), customer lifetime value (CLV), customer satisfaction (CSAT), user engagement and retention.

*Exam framing:* when asked how to prove a GenAI project's value, the answer is a **business metric tied to the original objective**, not model accuracy alone.

---

## 2.3 AWS GenAI Services and Infrastructure

| Service | What it does | Cue phrase |
|---|---|---|
| **Amazon Bedrock** | Fully managed, serverless API access to FMs from multiple providers (Anthropic, Meta, Mistral, Amazon Nova, Stability AI). Includes Knowledge Bases, Agents, Guardrails, Model Evaluation, Prompt Management. | "build GenAI apps without managing infrastructure" |
| **Amazon SageMaker AI** | Full control: build, train, fine-tune, and host your own models | "custom model, own infrastructure" |
| **SageMaker JumpStart** | Hub of pre-trained open-source and proprietary models, deployable in a few clicks | "quickly deploy a pre-trained model into my VPC" |
| **Amazon Bedrock AgentCore** | Managed runtime for deploying and operating AI agents securely at scale (identity, memory, tools, observability) | "run production agents" |
| **Strands Agents** | Open-source SDK for building agents in code | "framework to build an agent" |
| **Amazon Q** | Ready-made GenAI assistant — Q Business (chat over enterprise data) and Q Developer (coding assistant) | "assistant for employees / developers" |
| **Kiro** | Agentic IDE for spec-driven development | "AI-assisted software development" |
| **Amazon Quick** | GenAI-powered business intelligence and analytics | "natural-language questions over BI data" |
| **PartyRock** | No-code playground for building GenAI apps | "experiment without writing code" |

### Why build GenAI on AWS

- **Lower barrier to entry** — no need to train or host models; a single API.
- **Choice of models** — swap providers without rewriting the application.
- **Speed to market** and pay-as-you-go economics.
- **Security and privacy** — your data is not used to train the base models; encrypted in transit and at rest with KMS; **PrivateLink** keeps traffic off the public internet; IAM controls access.
- **Compliance** — Bedrock supports HIPAA eligibility, GDPR, SOC, ISO; AWS Artifact provides audit reports.
- **Responsibility and safety** — Guardrails filter harmful content and PII; AWS AI Service Cards document intended use and limitations.

### Cost tradeoffs to reason about

More capable model → better quality, higher cost and latency. Provisioned Throughput → predictable performance, but you pay for idle capacity. Custom/fine-tuned models → better task fit, but training cost plus mandatory provisioned hosting. Multi-region redundancy → higher availability, higher cost. Larger RAG context → more grounded, more tokens billed.

---

## Practice Questions

**1.** A prompt contains roughly 4,000 English words. Approximately how many input tokens is that?

A. ~1,000 · B. ~2,700 · C. ~5,300 · D. ~16,000

<details><summary>Answer</summary>

**C — ~5,300.** Roughly 1 token ≈ ¾ of a word, so words ÷ 0.75. Useful for estimating cost.
</details>

**2.** Which model type is used to generate images from text prompts?

A. Transformer LLM · B. Diffusion model · C. Embedding model · D. Regression model

<details><summary>Answer</summary>

**B — Diffusion model.** It starts from random noise and iteratively denoises toward the prompt. Transformers handle text; embedding models convert data to vectors.
</details>

**3.** A company must generate identical, reproducible outputs for a compliance calculation. Why is a foundation model a poor choice?

A. FMs cannot process numbers · B. FMs are nondeterministic and may hallucinate · C. FMs are too cheap · D. FMs cannot be deployed on AWS

<details><summary>Answer</summary>

**B.** GenAI output is probabilistic and can vary between runs, and may be confidently wrong. Compliance calculations need deterministic, auditable logic.
</details>

**4.** An application sends the same 8,000-token policy document with every request, followed by a short user question. Which feature most reduces cost and latency?

A. Increasing temperature · B. Prompt caching · C. Provisioned Throughput · D. Batch inference

<details><summary>Answer</summary>

**B — Prompt caching.** It reuses the repeated prefix across calls instead of reprocessing it every time.
</details>

**5.** Which Bedrock pricing option suits a steady, high-volume production workload that requires guaranteed throughput?

A. On-demand · B. Batch · C. Provisioned Throughput · D. Spot

<details><summary>Answer</summary>

**C — Provisioned Throughput.** Dedicated capacity for a commitment term. It's also required for hosting custom (fine-tuned) models.
</details>

**6.** Which two are limitations of generative AI? (Choose two.)

A. Inability to process text · B. Hallucinations · C. Limited interpretability · D. Requires labeled training data for every task · E. Cannot generate images

<details><summary>Answer</summary>

**B and C.** FMs may produce plausible falsehoods and cannot readily explain their reasoning. D is wrong — the advantage of FMs is working without task-specific labeled data.
</details>

**7.** What is the primary role of the Model Context Protocol (MCP)?

A. Compressing prompts · B. A standard way to connect agents to external tools and data sources · C. Encrypting model weights · D. Measuring token cost

<details><summary>Answer</summary>

**B.** MCP is an open standard for tool/data integration — build the connector once and any MCP-compatible agent can use it.
</details>

**8.** A team wants to deploy an open-source pre-trained model quickly into their own SageMaker environment. Which feature?

A. Bedrock Guardrails · B. SageMaker JumpStart · C. Amazon Q Developer · D. SageMaker Clarify

<details><summary>Answer</summary>

**B — SageMaker JumpStart.** A hub of pre-trained models that can be deployed with minimal configuration.
</details>

**9.** Which step of the foundation model lifecycle is the most computationally expensive?

A. Fine-tuning · B. Pre-training · C. Evaluation · D. Deployment

<details><summary>Answer</summary>

**B — Pre-training.** Self-supervised training on massive unlabeled corpora, costing millions and taking weeks. Fine-tuning is orders of magnitude cheaper.
</details>

**10.** A retailer launches a GenAI shopping assistant. Which metric best demonstrates business value to executives?

A. Model parameter count · B. Tokens processed per day · C. Conversion rate and average revenue per user · D. GPU utilization

<details><summary>Answer</summary>

**C.** Business value is measured against the business objective. Token counts and GPU metrics are operational, not value indicators.
</details>

**11.** In a multi-agent system, what does a supervisor (orchestrator) agent do?

A. Encrypts agent traffic · B. Decomposes the task, routes subtasks to specialized agents, and combines results · C. Stores embeddings · D. Filters toxic content

<details><summary>Answer</summary>

**B.** It coordinates specialized worker agents — the standard multi-agent pattern.
</details>

**12.** Which statement about data privacy in Amazon Bedrock is correct?

A. Prompts are used to retrain the base foundation models · B. Customer data is shared with model providers by default · C. Customer data is not used to train the base models and can stay off the public internet using PrivateLink · D. Bedrock does not support encryption

<details><summary>Answer</summary>

**C.** Your inputs and outputs are not used to train the underlying models, data is encrypted in transit and at rest, and PrivateLink keeps traffic within the AWS network.
</details>

---

## Quick Revision Checklist

- [ ] Token ≈ ¾ word; cost = input + output tokens; latency driven by output length
- [ ] Embeddings, vectors, chunking, context window
- [ ] Transformer = self-attention + parallel processing; diffusion = images
- [ ] FM lifecycle order, with pre-training as the expensive step
- [ ] On-demand vs. Batch vs. Provisioned Throughput
- [ ] Context engineering ≠ just prompt wording
- [ ] Agent = plan + tools + memory + orchestration; MCP = tool-connection standard
- [ ] Hallucination, nondeterminism, interpretability — and their mitigations
- [ ] Bedrock vs. SageMaker AI vs. JumpStart vs. Amazon Q
- [ ] Business metrics (ROI, conversion, CLV) over model metrics for value questions

**Source:** [AIF-C01 Exam Guide — Domain 2](https://docs.aws.amazon.com/aws-certification/latest/ai-practitioner-01/ai-practitioner-01-domain2.html)
