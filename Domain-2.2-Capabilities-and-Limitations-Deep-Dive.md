# Domain 2.2: Capabilities and Limitations — Deep Dive (Q&A Study Notes)

**AIF-C01 · Companion to Domain-2-Fundamentals-of-GenAI.md, section 2.2**

This file expands on 2.2 with analogies, worked examples, and exam traps for every capability, limitation, model-selection factor, and business-value metric in the syllabus.

---

## Capabilities — why a business reaches for GenAI at all

Each capability below is really answering "why GenAI instead of a traditional ML model, a rules engine, or a human doing it manually?"

| Capability | What it means | Concrete example |
|---|---|---|
| **Adaptability** | One model handles many different tasks, with no retraining — you just change the prompt | The same Claude/Nova model drafts a marketing email, summarizes a contract, and answers a support ticket. A traditional ML model would need a separate model trained for each of those three jobs. |
| **Responsiveness** | Real-time, conversational, natural-language interaction | A customer types a question into a chat widget and gets a coherent answer in seconds — no form fields, no waiting for a batch report. |
| **Simplicity** | No ML expertise or labeled dataset required to get started | A small business owner writes a prompt to generate product descriptions the same afternoon, versus needing a data scientist and thousands of labeled examples to train a custom text generator. |
| **Creativity and exploration** | Generates novel content and variations beyond a single fixed answer | Ask for a tagline and get five different options with different tones (playful, formal, punchy) — a rules engine can't improvise like this. |
| **Data efficiency** | Produces useful output with little or no *task-specific* training data | A model can classify sentiment in a niche industry's reviews from a single example prompt (few-shot), where a traditional classifier would need thousands of labeled reviews first. Note: the model still relies on its massive pre-training corpus — "data efficient" means it doesn't need new labeled data for *this* task, not that it needs zero data ever. |
| **Personalization** | Tailors output to an individual user from context | A shopping assistant references a specific customer's past orders and preferences to recommend products, using the same base model as every other customer. |
| **Scalability** | Produces output/handles volume at a scale humans cannot | Answering a million customer questions overnight, or generating personalized emails for an entire customer base simultaneously. |

**Exam trap:** don't mix up "data efficiency" (a *capability* — works with little task-specific data) with "requires labeled training data for every task" (a false statement often planted as a wrong answer about limitations). Foundation models are the opposite of that.

---

## Limitations — heavily tested, and almost all trace back to one root cause

**Root cause to remember first:** an LLM is a next-token probability machine. It is not a fact database, not a calculator, and not deterministic by design. Nearly every limitation below is a direct consequence of that one fact.

| Limitation | What it means | Concrete example | Mitigation |
|---|---|---|---|
| **Hallucination** | Confidently generates plausible-sounding claims that are false, because generation is probability-based sampling, not fact lookup | A model cites a court case that doesn't exist, or invents a Python method that was never in the library, phrased with total confidence | RAG grounding with citations to real sources · human review · tell users output must be verified · never treat GenAI output as a source of truth on its own |
| **Toxicity** | Offensive, disturbing, or inappropriate content — and the line itself is genuinely hard to draw | A model summarizing a historical document containing a slur has to decide whether to reproduce the quote or sanitize it — where's the line between filtering and censorship? | Curate training data to remove offensive material · guardrail models that detect and filter output (e.g., Bedrock Guardrails) |
| **Interpretability** | Cannot explain *why* it produced a specific answer the way a decision tree or regression model can show its logic | A loan-denial decision needs to be explainable to a regulator; a GenAI model can't produce a defensible "these three factors, weighted this way" explanation | Use traditional, interpretable ML where regulators demand an explanation, not a GenAI model |
| **Nondeterminism** | The same prompt can produce different answers on different runs, because token selection is probabilistic sampling | Running the exact same compliance calculation prompt twice gives two different numeric answers | Lower temperature narrows variability but doesn't eliminate it; use deterministic traditional logic wherever exact repeatability is legally or operationally required |
| **Plagiarism and cheating** | AI-generated essays, applications, or assignments submitted as original human work; hard to trace an output back to its source | A student submits an AI-written essay indistinguishable from their own writing; a job applicant's cover letter is entirely AI-generated | AI-detection tooling (imperfect) · institutional policy |
| **Data security and privacy** | Sensitive data entered into a prompt is now outside its normal protective boundary | An employee pastes a customer's SSN or a company's unreleased financials into a prompt "to help draft a response" | Encryption in transit/at rest · PII filtering and redaction · guardrails · access control (e.g., IAM, PrivateLink so calls stay inside your VPC) |
| **Regulatory violations** | Output that breaches sector-specific rules the model has no inherent awareness of | A model in a healthcare chatbot gives medical advice that violates HIPAA-adjacent disclosure rules, because it doesn't "know" your compliance obligations | Governance framework · human review · compliance-aware guardrails |
| **Social risks** | Bias in training data surfacing in output, causing downstream harm or misinformation | A resume-screening prompt systematically favors certain names or schools because that pattern existed in training data | Bias evaluation, diverse training data, guardrails, human oversight of high-stakes decisions |

**Exam trap:** a question offering "inability to process text" or "cannot generate images" as a limitation is always a wrong-answer distractor — those describe things GenAI *can* do. The real limitations are about trustworthiness, repeatability, and explainability, not raw capability.

---

## Choosing a model — selection factors, each with a scenario

The exam's favorite twist: "biggest and most capable" is almost never the right answer. The correct instinct is *smallest/cheapest model that still clears the bar for this specific task* — every factor below exists to help you rule models out, not to justify picking the biggest one.

| Factor | What to check | Scenario |
|---|---|---|
| **Model type and modality** (text / image / multimodal) | Does the model accept/produce the input and output types you need | A "describe this product photo" feature needs a multimodal model (e.g., Nova Lite/Pro) — a text-only model like Nova Micro literally cannot accept an image |
| **Performance requirements** | How good does it actually need to be at *this* task | A legal-document summarizer needs strong reasoning; a "billing vs. technical vs. other" ticket classifier just needs to get the label right most of the time |
| **Capabilities** | Does it support specific features you depend on, e.g. function/tool calling | An agent that books flights needs a model with reliable tool-use support — this narrows the field before cost even enters the conversation |
| **Constraints** | Practical limits like region availability or throughput ceilings | An app needs a model hosted in the EU for data-residency — this eliminates any model not offered there, regardless of quality |
| **Compliance** | Regulatory frameworks the service/model must support | A healthcare app needs HIPAA support — Bedrock supports this at the platform level, and it can rule out an otherwise-appealing option that doesn't |
| **Level of customization supported** | Can you fine-tune it, or only prompt it | A company needing thousands of documents written in its exact house tone benefits from fine-tuning, not prompting alone — and fine-tuned models on Bedrock require Provisioned Throughput to host |
| **Model size** | Bigger = slower and pricier, not automatically better | Classifying "is this email spam" doesn't need Nova Premier — that's like hiring a surgeon for a Band-Aid |
| **Inference options** | Real-time/on-demand vs. batch | A live chatbot needs on-demand, low-latency responses; overnight sentiment analysis on 500,000 reviews is a batch job |
| **Licensing agreements** | Open-source vs. commercial license, and what that permits | A startup planning to resell their product needs to confirm the model's license allows commercial redistribution, not just internal use |
| **Context window** | How much text/history it can hold at once | Summarizing one support ticket needs very little; summarizing a 300-page contract or running a long multi-turn agent needs a large window (e.g., Nova 2's ~1M tokens) — exceed it and earlier content is silently dropped |
| **Latency** | How fast it must respond | A live voice assistant (Nova Sonic) needs near-instant responses; a nightly batch report can tolerate more time for a cheaper model |
| **Cost** | Token pricing, scaled by volume | Classifying 10 million short messages/day cost-sensitively — the correct answer is the smallest model that hits acceptable accuracy (e.g., Nova Micro/Lite), not the most capable one, because cost scales linearly with tokens at that volume |

**Exam heuristic in one line:** when a question lists a pile of requirements, treat them as filters that eliminate the smaller/cheaper models one by one — the answer is the cheapest model still standing after every requirement is applied, not the most powerful model on the list.

---

## Business value metrics — technical vs. business, and why the exam cares

**The trap to know cold:** "How do we prove this GenAI project was worth it?" The wrong answer is "measure model accuracy." The right answer is "tie it to the business objective the project was built for." Accuracy is what engineers watch; revenue, retention, and cost are what executives watch.

**Technical metrics** — the engineering-facing view:

| Metric | What it captures |
|---|---|
| **Accuracy** | How often the model's output is correct for the task |
| **Cross-domain performance** | Whether the model holds up across different subject areas, not just the one it was tuned on |
| **Latency** | Response speed |
| **Task completion rate** | Whether the model/agent actually finishes the job end-to-end, not just produces a plausible-looking partial answer |

**Business metrics** — the executive-facing view:

| Metric | What it captures | Example |
|---|---|---|
| **User satisfaction** | Direct user feedback on response quality | Post-chat thumbs-up/down ratings on a support bot |
| **Average revenue per user (ARPU)** | Revenue attributable to the GenAI feature | A subscription app's AI-powered upsell assistant is judged by whether ARPU rises after launch |
| **Cross-domain performance** | Same idea as the technical metric, viewed through "does this generalize enough to be useful across our whole product," not just a lab benchmark |
| **Conversion rate** | Whether the app drives the desired outcome (e.g., a purchase) | A retail shopping assistant's success is measured by whether more browsing sessions turn into checkouts |
| **Efficiency** | Computation/resource utilization, or process/production efficiency | Fewer human-hours needed per support ticket after deploying the assistant |
| **ROI / cost per interaction** | Whether the value generated exceeds token cost and development cost | A support-deflection bot is worth it only if the cost of running it is lower than the human-agent cost it replaces |
| **Customer lifetime value (CLV)** | Long-term value of retained customers | A personalization assistant is judged by whether personalized customers stick around and spend more over years, not just in the first session |

**Worked example:** a retailer launches a GenAI shopping assistant. A question might offer "model parameter count," "tokens processed per day," "conversion rate and ARPU," and "GPU utilization" as options. The correct answer is conversion rate and ARPU — the other three are operational/technical signals that don't by themselves prove the business is better off.

---

**Companion file:** [Domain-2-Fundamentals-of-GenAI.md](Domain-2-Fundamentals-of-GenAI.md) — the main 2.1–2.3 reference and practice questions.
