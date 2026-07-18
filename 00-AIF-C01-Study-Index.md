# AWS Certified AI Practitioner (AIF-C01) — Study Index

## Exam facts

- **50 scored questions** + 15 unscored (unidentified) = 65 total
- Scaled score **100–1000**, pass at **700**
- **Compensatory scoring** — no minimum per domain, only the overall score matters
- Question types: multiple choice, multiple response, ordering, matching
- No penalty for guessing — never leave a question blank
- Target candidate: ~6 months exposure; **uses** AI/ML on AWS rather than builds it

## Domains and notes

| Domain | Weight | Notes | Questions |
|---|---|---|---|
| 1 — Fundamentals of AI and ML | 20% | [Domain-1-Fundamentals-of-AI-and-ML.md](Domain-1-Fundamentals-of-AI-and-ML.md) | 18 |
| 2 — Fundamentals of GenAI | 24% | [Domain-2-Fundamentals-of-GenAI.md](Domain-2-Fundamentals-of-GenAI.md) | 16 |
| 3 — Applications of Foundation Models | 28% | [Domain-3-Applications-of-Foundation-Models.md](Domain-3-Applications-of-Foundation-Models.md) | 16 |
| 4 — Guidelines for Responsible AI | 14% | [Domain-4-Guidelines-for-Responsible-AI.md](Domain-4-Guidelines-for-Responsible-AI.md) | 14 |
| 5 — Security, Compliance, and Governance | 14% | [Domain-5-Security-Compliance-and-Governance.md](Domain-5-Security-Compliance-and-Governance.md) | 15 |

Domains 2 + 3 are **52% of the exam** — spend your time there. Every practice question sits in the domain it belongs to.

---

## Cross-domain decision rules (the highest-yield page)

**Which customization approach?**
Knowledge the model lacks → **RAG**. Behavior, style, or format → **instruction fine-tuning**. Deep domain specialization → **domain-adaptation fine-tuning**. Cheaper/faster copy of a big model → **distillation**. Try **prompt engineering** first, always.
*Cost order:* prompt engineering < RAG < instruction fine-tuning < domain-adaptation fine-tuning < from scratch.

**Which service?**
Standard task, no custom model → **purpose-built managed service** (Transcribe, Translate, Comprehend, Rekognition, Textract, Personalize, Lex, Polly, Kendra).
GenAI without infrastructure → **Bedrock**.
Custom model, full control → **SageMaker AI**.
Pre-trained model deployed into your own environment → **SageMaker JumpStart**.
Ready-made assistant over company data → **Amazon Q Business**. Developer/AWS help → **Amazon Q Developer**.
No-code experimentation, no AWS account → **PartyRock**.

**Which inference type?**
Milliseconds, steady traffic → real-time (~25 MB, 60 s). Spiky/idle traffic, minimize cost → serverless (~4 MB, 60 s, cold starts). Payloads up to 1 GB or processing up to 1 hour → asynchronous. Huge offline volume on a schedule → batch. No connectivity, on-device → edge with a small model.

**Which metric?**
False positives costly → precision. False negatives costly → recall. Imbalanced / need balance → F1. Balanced dataset → accuracy. Threshold comparison → AUC-ROC. Regression error → MAE/MAPE/RMSE (lower better), R² (closer to 1 better). Summarization → ROUGE. Translation → BLEU. Semantic similarity → BERTScore. Next-token quality → perplexity (lower better).

**Which governance service?**
Who did what → CloudTrail. Resource configuration compliance → Config. Audit evidence → Audit Manager. AWS's own compliance reports → Artifact. Software vulnerabilities → Inspector. Best-practice recommendations → Trusted Advisor. PII in S3 → Macie.

**Which responsible-AI tool?**
Detect bias / explain predictions → **SageMaker Clarify**. Balance a biased dataset → **Data Wrangler**. Watch production drift → **Model Monitor**. Human review of predictions → **Amazon A2I**. Labeling and RLHF → **Ground Truth**. Filter output, block topics, redact PII → **Bedrock Guardrails**. Document the model → **Model Cards**.

---

## Common traps

- **Accuracy is misleading on imbalanced data** — a 99.9% accurate fraud model can catch zero fraud. Accuracy is best only for **balanced** datasets.
- **ML is the wrong tool** for deterministic problems with a computable exact answer, or when a simple rule works.
- **Temperature, Top P, and Top K affect neither price nor latency.** Cost and latency are driven by model size and token counts.
- **Provisioned Throughput is not a cost-saving measure** — it reserves capacity. **Batch** is the discount (up to 50%).
- **Custom (fine-tuned) models cost more to run**, via on-demand tokens or provisioned throughput.
- **Model Cards ≠ AWS Artifact.** One documents your model; the other downloads AWS compliance reports.
- **Interpretability ≠ explainability ≠ transparency.**
- **RAG failures are usually retrieval failures**, not generation failures.
- **Lower temperature** fixes inconsistent output — not a bigger model.
- **Sampling bias** is fixed with **data augmentation for underrepresented classes**.
- **Bigger model ≠ better answer.** Right-size for cost, latency, and sustainability.

---

## Study plan (2–3 weeks)

1. Read Domain 1 and 2 notes; attempt the practice questions without peeking.
2. Read Domain 3 twice — largest domain, most detail.
3. Read Domains 4 and 5; drill the service one-liner tables until automatic.
4. Re-do all practice questions across the five files; log every miss.
5. Take AWS's official practice question set and a full-length practice exam.
6. Final pass: only the Quick Revision Checklists and this index page.

**Sources:** [AWS Certified AI Practitioner (AIF-C01) Exam Guide](https://docs.aws.amazon.com/aws-certification/latest/ai-practitioner-01/ai-practitioner-01.html) · *AWS Certified AI Practitioner* course slides (Stéphane Maarek), used as a reference for scope and terminology. The slide deck is licensed for personal use only — these notes are original write-ups, not a copy of it.
