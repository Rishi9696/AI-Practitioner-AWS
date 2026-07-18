# Domain 2: Fundamentals of GenAI
**AIF-C01 · 24% of scored content · ~12 of 50 scored questions**

Three task statements: core GenAI concepts (2.1), capabilities and limitations (2.2), AWS GenAI infrastructure (2.3).

---

## 2.1 Core GenAI Concepts

### What generative AI actually is

Generative AI relies on a **foundation model (FM)**: a very large model **pre-trained on a huge amount of unlabeled data** using self-supervised learning, which can then be **adapted** to a broad range of general tasks — text generation, summarization, information extraction, question answering, chatbots, image generation.

```
Unlabeled data  ──pre-train──▶  Foundation Model  ──adapt──▶  many downstream tasks
```

Foundation models can cost **tens of millions of dollars to train**, which is why almost nobody builds one — you consume an existing one. Providers include Amazon, Anthropic, Meta, Mistral, Stability AI, OpenAI, and Google. Some are **open-source** (Meta's Llama, Google's BERT); others are under **commercial licence**.

### How text is generated

An LLM predicts **one token at a time**, choosing the next token based on probability. That single mechanism explains most of its behavior — including hallucinations, which come from probability-based sampling rather than from any lookup of facts.

```
"AWS is great"  →  tokens ["AWS", " is", " great"]  →  token IDs  →  embeddings [0.23, -0.81, …]  →  transformer  →  next token
```

### The vocabulary

**Tokenization** — converting raw text into a sequence of **tokens**, the units a model actually reads and writes.
- **Word-based tokenization** splits text into whole words.
- **Subword tokenization** splits longer or rarer words into pieces, which keeps the vocabulary manageable.
*Rule of thumb:* 1 token ≈ 4 characters ≈ ¾ of an English word. 1,000 tokens ≈ 750 words.

**Context window** — the **number of tokens the model can consider at once** (prompt + response). A larger window means more information and better coherence, but requires more memory and processing. AWS calls this the **first factor to look at** when choosing a model. Exceed it and earlier content is dropped.

**Embeddings** — **vectors** (arrays of numbers) created from text, images, or audio. They are high-dimensional so they can capture many features of a single input at once — semantic meaning, syntactic role, sentiment. Words with related meanings end up with similar vectors, which is what makes semantic search and RAG possible: "dog" and "puppy" sit close together, "houses" sits far away.

**Chunking** — splitting a long document into smaller pieces before embedding, so retrieval returns only the relevant passage rather than a whole 200-page PDF.

**Vector database** — where embeddings are stored and searched by similarity.

### Model families

- **Transformer LLMs** — the architecture behind modern text models. **Self-attention** lets the model weigh every token against every other token, and tokens are processed **in parallel** (unlike RNNs, which are sequential). This is why LLMs scale.
- **Multimodal models** — accept and/or produce more than one type of input/output (text, image, audio, video). *Example:* upload a photo of apples and ask "how many are there?"
- **Diffusion models** — the text-to-image approach. In **training (forward diffusion)** the model progressively adds noise to real pictures. In **generation (reverse diffusion)** it starts from pure noise and removes it step by step, steered by the prompt, until an image matching the text appears. *This is the exam's image-generation answer* (Stable Diffusion, Amazon Nova Canvas).

### GenAI use cases

Text generation · summarization · information extraction · chatbots and AI assistants · question answering · code generation · translation · semantic search · recommendation engines · image, video, and audio generation · customer service agents.

### The FM lifecycle

1. **Data selection** — assemble massive, diverse, curated corpora.
2. **Model selection** — pick an architecture, or pick an existing FM to build on.
3. **Pre-training** — self-supervised learning on unlabeled data. **By far the most expensive step** (millions of dollars, weeks of compute).
4. **Fine-tuning** — adapt to specific tasks or domains with much smaller datasets.
5. **Evaluation** — benchmarks, automatic metrics, human review.
6. **Deployment** — serve via managed API or self-hosted endpoint.
7. **Feedback** — collect real usage and loop back (RLHF, retraining).

### Token-based pricing and its consequences

You pay per **input token and output token**. What this means in practice:

- **The number of input and output tokens is the main driver of cost.** Long prompts, large RAG context, and verbose answers all cost more.
- **Latency scales with tokens and model size** — generation is sequential. Latency is affected by model size, model type, input length, and output length. It is **not** affected by temperature, Top P, or Top K.
- **Temperature, Top K, and Top P have no impact on price.**
- **Smaller models are usually cheaper** (and faster). Right-size the model: a small model for classification, a large one for complex reasoning.
- **Prompt caching** reuses a repeated prefix (a long system prompt or document) across calls at reduced cost and latency.

**Amazon Bedrock pricing modes:**

| Mode | How it works | When to use |
|---|---|---|
| **On-demand** | Pay-as-you-go, no commitment. Text models charged per input/output token; embedding models per input token; image models per image generated. Works with base and custom models. | Unpredictable or low volume, experimentation |
| **Batch** | Many predictions at once, output written as a single file to S3. **Discounts up to 50%.** | Large offline workloads |
| **Provisioned Throughput** | Buy model units for a term (1 month, 6 months). Guarantees max tokens processed per minute. Works with base, fine-tuned, and custom models. | Steady high volume; **required to host custom models** |

*Note:* Provisioned Throughput is usually **not** a cost-saving measure — it's a way to reserve capacity and guarantee performance.

### Context engineering

Broader than prompt engineering: deciding **what information the model sees at inference time** and how it's organized — system instructions, retrieved documents, conversation history, tool definitions, output format. Because the context window is finite and every token costs money, the goal is getting the *right* context in, not the *most*. Poor context engineering causes hallucinations, cost blowout, and models losing track of long conversations.

### Agentic AI concepts

An **AI agent** perceives a goal, **plans** the steps, **uses tools** to act, observes results, and iterates until done. Agents coordinate tasks in the correct order and pass information correctly between steps.

| Concept | What it is |
|---|---|
| **Tool use / action groups** | The agent calls external APIs, databases, or functions to fetch data or take action |
| **Memory** | Short-term (current conversation history) vs. long-term (facts persisted across sessions) |
| **Orchestration** | Managing the loop — plan → act → observe → repeat — with error handling |
| **Chain of thought** | The agent reasons through steps internally before acting |
| **MCP (Model Context Protocol)** | An open standard for connecting agents to external tools and data sources: build the integration once, and any MCP-compatible agent can use it |
| **Multi-agent systems** | Several specialized agents collaborating; the common pattern is a **supervisor/orchestrator** agent that decomposes the task, routes subtasks to workers, and merges results |

Agents also **use RAG** to retrieve information when needed.

*Business examples:* IT support agent that resets passwords and files tickets; travel agent that checks calendars, searches flights, and books; research agent that queries multiple sources and writes a briefing; infrastructure provisioning and deployment automation.

---

## 2.2 Capabilities and Limitations

### Capabilities of generative AI

| Capability | What it means |
|---|---|
| **Adaptability** | One model handles many tasks with no retraining — just new prompts |
| **Responsiveness** | Real-time, conversational, natural-language interaction |
| **Simplicity** | No ML expertise or labeled dataset needed to start |
| **Creativity and exploration** | Generates novel content and variations |
| **Data efficiency** | Works with little or no task-specific training data |
| **Personalization** | Tailors output to individual users |
| **Scalability** | Produces content and answers at volumes humans cannot |

### Challenges and limitations — heavily tested

| Limitation | What it means | Mitigation |
|---|---|---|
| **Hallucinations** | Claims that sound true but are false, caused by next-token probability sampling. Content can seem plausible while not existing at all. | Educate users that output must be checked · verify against independent sources · mark generated content as unverified · RAG grounding and citations |
| **Toxicity** | Offensive, disturbing, or inappropriate content. Defining "toxic" is itself hard — where is the line between filtering and censorship, and what about quoting someone else's toxic words? | Curate training data to remove offensive material · use guardrail models to detect and filter output |
| **Interpretability** | Cannot explain *why* it produced a given answer | Use traditional ML where regulators demand explanations |
| **Nondeterminism** | The same prompt can produce different answers | Lower temperature; unsuitable where exact repeatability is required |
| **Plagiarism and cheating** | Essays, applications, and assignments generated by AI; hard to trace an output back to its source | AI-detection tooling; policy |
| **Data security and privacy** | Sensitive data in prompts; risk of exposure | Encryption, PII filtering, guardrails, access control |
| **Regulatory violations** | Output that breaches sector rules | Governance framework, human review |
| **Social risks** | Bias, misinformation, and downstream harm | Bias evaluation, diverse data, guardrails |

### Choosing a model — selection factors

Model type and **modality** (text / image / multimodal) · performance requirements · capabilities · constraints · **compliance** · level of customization supported · model size · inference options · **licensing agreements** · **context window** · **latency** · cost.

*Exam heuristic:* the "best" model is the **smallest and cheapest one that meets the requirement**, not the largest.

### Business value metrics

**Technical:** accuracy, cross-domain performance, latency, task completion rate.

**Business:**

| Metric | What it captures |
|---|---|
| **User satisfaction** | Direct user feedback on response quality |
| **Average revenue per user (ARPU)** | Revenue attributable to the GenAI application |
| **Cross-domain performance** | Ability to handle tasks across different domains |
| **Conversion rate** | Whether the app drives desired outcomes such as purchases |
| **Efficiency** | Computation and resource utilization; process or production efficiency |
| **ROI / cost per interaction** | Whether the value exceeds the token and development cost |
| **Customer lifetime value (CLV)** | Long-term value of retained customers |

*Exam framing:* when asked how to prove a GenAI project's value, choose a **business metric tied to the original objective**, not model accuracy alone.

---

## 2.3 AWS GenAI Services and Infrastructure

| Service | What it does | Cue phrase |
|---|---|---|
| **Amazon Bedrock** | Fully managed, serverless API access to FMs from many providers. Includes an interactive playground, Knowledge Bases (RAG), Agents, Guardrails, Model Evaluation, and Prompt Management. **Bedrock makes a private copy of the FM available only to you, and none of your data is used to train the base model.** | "build GenAI apps without managing infrastructure" |
| **Amazon SageMaker AI** | Full control: build, train, fine-tune, and host your own models | "custom model, own infrastructure" |
| **SageMaker JumpStart** | ML hub of pre-trained FMs, CV models, and NLP models from Hugging Face, Meta, Stability AI, Databricks; fully customizable and deployed on SageMaker with full control of deployment options. Also offers pre-built solutions for demand forecasting, credit prediction, fraud detection. | "quickly deploy a pre-trained model into my own environment" |
| **Amazon Bedrock AgentCore** | Managed runtime for deploying and operating AI agents securely at scale (identity, memory, tools, observability) | "run production agents" |
| **Strands Agents** | Open-source SDK for building agents in code | "framework to build an agent" |
| **Amazon Q Business** | Fully managed GenAI assistant for employees, grounded in company data. Answers questions, summarizes, generates content, and performs routine actions (submit time-off requests, send invites). Built on Bedrock, **but you cannot choose the underlying FM.** Integrates with IAM Identity Center; supports admin controls and Q Apps. | "assistant over our internal company data" |
| **Amazon Q Developer** | Coding assistant plus AWS expertise — answers questions about AWS documentation and about resources in your account, suggests CLI commands, helps with bill analysis and troubleshooting. IDE extensions available. | "help developers write code and manage AWS" |
| **Amazon Q integrations** | Q for QuickSight, Q for EC2, Q in AWS Chatbot, Q for Glue | "natural-language help inside an AWS service" |
| **Kiro** | Agentic IDE for spec-driven development | "AI-assisted software development" |
| **Amazon Quick** | GenAI-powered business intelligence and analytics | "natural-language questions over BI data" |
| **PartyRock** | GenAI app-building playground powered by Bedrock — **no coding and no AWS account required** | "experiment with GenAI apps for free" |

### Amazon Nova — AWS's own foundation models

AWS's family of FMs, accessible through Bedrock: fast, cost-effective, enterprise-ready.

**Understanding models:**

| Model | Positioning |
|---|---|
| **Nova Premier** | Most capable multimodal model, for complex reasoning; also the best **teacher** for distilling custom models |
| **Nova Pro** | Highly capable multimodal model with the best balance of accuracy, speed, and cost |
| **Nova Lite** | Very low-cost multimodal model, extremely fast on image, video, and text input |
| **Nova Micro** | Text-only, **lowest latency** at very low cost |

**Creative and speech:** **Nova Canvas** (image generation) · **Nova Reel** (video generation) · **Nova Sonic** (conversational speech understanding and generation).

**Nova 2** adds up to **1M tokens of context** and advanced reasoning: **Nova 2 Lite** (fast reasoning over text, images, video, documents), **Nova 2 Sonic** (real-time speech-to-speech), **Nova 2 Multimodal Embeddings** (for agentic RAG and semantic search), **Nova 2 Omni** (multimodal reasoning plus image generation).

**Amazon Titan** — AWS's earlier high-performing FM family (text, image, multimodal, and embedding models) via fully managed APIs, customizable with your own data. Smaller Titan models are more cost-effective.

### Why build GenAI on AWS

- **Lower barrier to entry** — no training, no hosting, a single API.
- **Choice of models** — swap providers without rewriting the application.
- **Speed to market** and pay-as-you-go economics.
- **Responsiveness and availability** — deployed across multiple Availability Zones and Regions, giving redundancy and regional coverage.
- **Performance** — specialized CPUs and GPUs (AWS Trainium for training, AWS Inferentia for inference) chosen per use case for cost savings.
- **Security and privacy** — your data is **not** used to train the base models; encrypted at rest and in transit with KMS; **PrivateLink** keeps API calls inside your VPC; IAM controls access; CloudTrail records API activity.
- **Compliance** — Bedrock supports frameworks such as HIPAA, GDPR, SOC, and ISO; AWS Artifact provides audit reports.
- **Responsibility and safety** — Bedrock Guardrails filter harmful content and PII; AWS AI Service Cards document intended use and limitations.

### Cost tradeoffs to reason about

More capable model → better quality, higher cost and latency. **Batch** → up to 50% cheaper but not immediate. **Provisioned Throughput** → predictable performance, but you pay for reserved capacity. **Custom/fine-tuned models** → better task fit, but training cost plus more expensive hosting. **Multi-region redundancy** → higher availability, higher cost. **Larger RAG context** → more grounded answers, more tokens billed.

---

## Practice Questions — Domain 2 only

**1.** A prompt contains roughly 4,000 English words. Approximately how many input tokens is that?

A. ~1,000 · B. ~2,700 · C. ~5,300 · D. ~16,000

<details><summary>Answer</summary>

**C — ~5,300.** Roughly 1 token ≈ ¾ of a word, so divide words by 0.75. Useful for estimating cost, since token count is the main cost driver.
</details>

**2.** Which model type generates images from text prompts, and how does it work?

A. Transformer LLM — predicts the next token · B. Diffusion model — reverses a noise process guided by the prompt · C. Embedding model — converts text to vectors · D. RNN — processes pixels sequentially

<details><summary>Answer</summary>

**B — Diffusion model.** Training adds noise to real images (forward diffusion); generation starts from pure noise and removes it step by step (reverse diffusion), steered by the prompt.
</details>

**3.** A company must produce identical, reproducible outputs for a compliance calculation. Why is a foundation model a poor choice?

A. FMs cannot process numbers · B. FMs are nondeterministic and may hallucinate · C. FMs are too cheap · D. FMs are unavailable in most Regions

<details><summary>Answer</summary>

**B.** GenAI output is probabilistic and can vary between runs, and may be confidently wrong. Compliance calculations require deterministic, auditable logic.
</details>

**4.** Which two factors increase Amazon Bedrock inference latency? (Choose two.)

A. Higher temperature · B. Larger model size · C. More output tokens · D. Higher Top K · E. Higher Top P

<details><summary>Answer</summary>

**B and C.** Latency is driven by model size, model type, and the number of input and output tokens. Temperature, Top K, and Top P affect *what* is generated, not how fast — and they have no effect on price either.
</details>

**5.** Which Bedrock pricing option offers discounts of up to 50% for large, non-urgent workloads?

A. On-demand · B. Batch · C. Provisioned Throughput · D. Spot

<details><summary>Answer</summary>

**B — Batch.** Multiple predictions processed together with the output written as a single file to S3. Provisioned Throughput reserves capacity but is generally not a cost-saving measure.
</details>

**6.** A company wants to host a fine-tuned custom model on Bedrock with guaranteed throughput. Which pricing mode is required?

A. On-demand only · B. Batch · C. Provisioned Throughput · D. Serverless

<details><summary>Answer</summary>

**C — Provisioned Throughput.** You buy model units for a term and get a guaranteed maximum tokens-per-minute. It works with base, fine-tuned, and custom models.
</details>

**7.** Which two are challenges of generative AI? (Choose two.)

A. Inability to process text · B. Hallucinations · C. Nondeterminism · D. Requires labeled training data for every task · E. Cannot generate images

<details><summary>Answer</summary>

**B and C.** Plausible-sounding falsehoods and variable output are core GenAI challenges. D is wrong — data efficiency (working without task-specific labeled data) is a *capability* of FMs.
</details>

**8.** What is the primary role of the Model Context Protocol (MCP)?

A. Compressing prompts · B. A standard way to connect agents to external tools and data sources · C. Encrypting model weights · D. Measuring token cost

<details><summary>Answer</summary>

**B.** MCP is an open standard for tool and data integration — build the connector once and any MCP-compatible agent can use it.
</details>

**9.** Which Amazon Nova model would you choose for the **lowest-latency, text-only** responses at very low cost?

A. Nova Premier · B. Nova Pro · C. Nova Micro · D. Nova Canvas

<details><summary>Answer</summary>

**C — Nova Micro.** Text-only and optimized for the lowest latency. Premier is the most capable (and the best distillation teacher), Pro balances accuracy/speed/cost, and Canvas generates images.
</details>

**10.** Which step of the foundation model lifecycle is the most computationally expensive?

A. Fine-tuning · B. Pre-training · C. Evaluation · D. Deployment

<details><summary>Answer</summary>

**B — Pre-training.** Self-supervised training on massive unlabeled corpora can cost tens of millions of dollars. Fine-tuning is orders of magnitude cheaper.
</details>

**11.** A retailer launches a GenAI shopping assistant. Which metric best demonstrates business value to executives?

A. Model parameter count · B. Tokens processed per day · C. Conversion rate and average revenue per user · D. GPU utilization

<details><summary>Answer</summary>

**C.** Business value is measured against the business objective. Token counts and GPU metrics are operational, not value indicators.
</details>

**12.** Which statement about data privacy in Amazon Bedrock is correct?

A. Prompts are used to retrain the base foundation models · B. Customer data is shared with model providers by default · C. Bedrock provides a private copy of the model, and customer data is not used to train the base FM · D. Bedrock does not support encryption

<details><summary>Answer</summary>

**C.** Bedrock makes a copy of the FM available only to you, which you may fine-tune with your own data. None of your data trains the base model.
</details>

**13.** A team wants to experiment with building GenAI apps with no coding and **without an AWS account**. What should they use?

A. SageMaker Studio · B. PartyRock · C. Bedrock Playground · D. Amazon Q Developer

<details><summary>Answer</summary>

**B — PartyRock.** A GenAI app-building playground powered by Bedrock that requires no coding and no AWS account.
</details>

**14.** Which statement about Amazon Q Business is correct?

A. You choose which foundation model powers it · B. It is a fully managed assistant grounded in your company's data, built on Bedrock, with no choice of underlying FM · C. It only works with public web data · D. It replaces SageMaker for model training

<details><summary>Answer</summary>

**B.** Q Business answers questions, summarizes, generates content, and performs routine actions over company data. It's built on Bedrock, but the underlying FM is not selectable.
</details>

**15.** What does a supervisor (orchestrator) agent do in a multi-agent system?

A. Encrypts agent traffic · B. Decomposes the task, routes subtasks to specialized agents, and combines results · C. Stores embeddings · D. Filters toxic content

<details><summary>Answer</summary>

**B.** It coordinates specialized worker agents — the standard multi-agent pattern.
</details>

**16.** Why is the context window described as the first factor to consider when choosing a model?

<details><summary>Answer</summary>

It sets the **maximum number of tokens the model can consider at once** (prompt + response). A larger window allows more information and better coherence, but demands more memory and processing power — and costs more. If your documents or conversations exceed it, the model simply cannot see the earlier content.
</details>

---

## Quick Revision Checklist

- [ ] FM = pre-trained on unlabeled data, adaptable to many tasks; costs tens of millions to train
- [ ] Token ≈ ¾ word; **input + output token count is the main cost driver**
- [ ] Latency ← model size, model type, input/output tokens. **Not** temperature/Top K/Top P
- [ ] Temperature, Top K, Top P have **no** effect on price
- [ ] Context window = first factor when choosing a model
- [ ] Tokenization (word vs. subword), embeddings, chunking, vector DB
- [ ] Transformer = self-attention + parallel; diffusion = forward/reverse noise → images
- [ ] FM lifecycle order, with pre-training as the expensive step
- [ ] On-demand vs. **Batch (up to 50% off)** vs. **Provisioned Throughput (reserve capacity, required for custom models)**
- [ ] Capabilities: adaptability, responsiveness, simplicity, creativity, data efficiency, personalization, scalability
- [ ] Challenges: hallucination, toxicity, interpretability, nondeterminism, plagiarism, privacy, regulatory, social
- [ ] Agent = plan + tools/action groups + memory + orchestration; MCP = tool-connection standard
- [ ] Nova family (Premier / Pro / Lite / Micro / Canvas / Reel / Sonic; Nova 2 with 1M context) and Titan
- [ ] Bedrock vs. SageMaker AI vs. JumpStart vs. Q Business vs. Q Developer vs. PartyRock
- [ ] Business metrics: user satisfaction, ARPU, cross-domain performance, conversion rate, efficiency

**Sources:** [AIF-C01 Exam Guide — Domain 2](https://docs.aws.amazon.com/aws-certification/latest/ai-practitioner-01/ai-practitioner-01-domain2.html) · *AWS Certified AI Practitioner* course slides (Stéphane Maarek), used as a reference for scope and terminology.
