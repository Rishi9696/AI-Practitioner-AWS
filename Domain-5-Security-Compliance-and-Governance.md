# Domain 5: Security, Compliance, and Governance for AI Solutions
**AIF-C01 · 14% of scored content · ~7 of 50 scored questions**

Two task statements: securing AI systems (5.1), governance and compliance (5.2).

---

## 5.1 Securing AI Systems

### The shared responsibility model applied to AI

- **AWS secures the cloud** — physical infrastructure, hypervisor, managed service infrastructure (the Bedrock service itself).
- **You secure what's in the cloud** — your data, IAM permissions, encryption choices, network configuration, prompts, guardrails, and how you use model outputs.

*Exam framing:* who is responsible for controlling access to a Bedrock model or for the data you put in S3? **You are.**

### Security services to know

| Service | Purpose | Cue phrase |
|---|---|---|
| **AWS IAM** | Roles, policies, least-privilege access to models, data, and endpoints | "control who can invoke the model" |
| **AWS KMS** | Manages encryption keys for data at rest | "customer-managed encryption keys" |
| **Amazon Macie** | Discovers and classifies **sensitive data/PII in S3** | "find PII in our training data bucket" |
| **AWS PrivateLink** | Private connectivity to services without traversing the public internet | "traffic must not leave the AWS network" |
| **Amazon Bedrock Guardrails** | Filters harmful content, denied topics, PII redaction, grounding checks | "block toxic output / prevent prompt attacks" |
| **AgentCore Identity / Policy** | Identity and permission boundaries for AI agents | "control what an agent is allowed to do" |
| **AWS Secrets Manager** | Store credentials and API keys | "don't hard-code secrets" |
| **Amazon GuardDuty / Inspector** | Threat detection / vulnerability scanning | "detect suspicious activity / scan workloads" |

**Encryption:** at rest (KMS, S3 SSE, encrypted EBS) and in transit (TLS). Assume both are expected in any correct answer.

### Data lineage and source citation

- **Data lineage** — record of where data came from and every transformation applied to it.
- **Data cataloging** — inventory of datasets with metadata and ownership (AWS Glue Data Catalog).
- **Source citation** — showing which documents an FM answer was based on. RAG makes this possible and is the main defense against "where did this claim come from?"
- **SageMaker Model Cards** — documents the model's data sources, intended use, and limitations.

### Secure data engineering practices

Assess **data quality** (accuracy, completeness, consistency) · enforce **access control** (least privilege, role-based, S3 bucket policies) · protect **data integrity** (versioning, checksums, immutability) · apply **privacy-enhancing technologies** (anonymization, pseudonymization, tokenization, masking, differential privacy) · classify data by sensitivity · encrypt everywhere · log all access.

### AI-specific security threats

| Threat | Description | Mitigation |
|---|---|---|
| **Prompt injection** | Malicious instructions embedded in user input or retrieved documents | Guardrails, input sanitization, delimiting, least-privilege tools |
| **Data leakage** | Sensitive data exposed via prompts, logs, or model output | PII redaction, output filtering, log scrubbing |
| **Model poisoning** | Corrupt training data or RAG sources to alter behavior | Validate and control data sources, integrity checks |
| **Model inversion / extraction** | Reconstructing training data or cloning the model via queries | Rate limiting, access control, monitoring |
| **Toxicity / harmful output** | Offensive or unsafe generated content | Guardrails content filters, human review |
| **Insecure agent tool use** | An agent with excessive permissions takes damaging actions | Scoped IAM roles, human approval for high-impact actions |

Also expected: application security, threat detection, vulnerability management, infrastructure protection, and **audit trails/logging of all AI interactions** (prompts, responses, who called what — CloudTrail plus application logging).

### Hallucination detection and grounding

1. **RAG grounding** — force answers to come from retrieved source documents.
2. **Contextual grounding check** (Bedrock Guardrails) — automatically scores whether a response is supported by the provided context and blocks it if not.
3. **Output validation** — schema checks, fact-checking against a trusted system, regex/business-rule validation.
4. **Confidence scoring** — flag low-confidence responses for human review.
5. **Citations** — require the model to cite sources so users can verify.
6. **Human-in-the-loop** for high-stakes outputs.

---

## 5.2 Governance and Compliance

### Governance and audit services — memorize the one-liners

| Service | One-line purpose |
|---|---|
| **AWS CloudTrail** | Records **who did what, when** — API call audit log |
| **AWS Config** | Records **resource configurations** and evaluates them against compliance rules |
| **AWS Audit Manager** | **Automates evidence collection** for audits and maps it to frameworks |
| **AWS Artifact** | **Downloads AWS compliance reports** (SOC, ISO, PCI) and agreements |
| **Amazon Inspector** | Automated **vulnerability scanning** of workloads |
| **AWS Trusted Advisor** | Best-practice **recommendations** (cost, performance, security, limits) |
| **AWS CloudWatch** | Metrics, logs, alarms — operational monitoring |
| **AWS Organizations / SCPs** | Multi-account governance and permission guardrails |

*The four most-confused:* **CloudTrail = actions. Config = configurations. Audit Manager = audit evidence. Artifact = AWS's own compliance documents.*

### Data governance strategies

- **Data lifecycle** — creation → storage → use → archival → deletion, with defined rules at each stage.
- **Data residency / sovereignty** — keeping data in a required region to satisfy laws like GDPR. Choose the AWS Region accordingly.
- **Retention policies** — how long data and logs are kept (and when they must be deleted).
- **Logging and monitoring** — comprehensive, tamper-resistant, retained per policy.
- **Observation** — ongoing review of model behavior and data quality in production.
- **Classification and ownership** — every dataset has a sensitivity label and an accountable owner.

### Governance processes

- Written **policies and standards** for AI development and use.
- **Review cadence** — periodic model, risk, and bias reviews; approval gates before deployment.
- **Governance frameworks** — e.g., the **Generative AI Security Scoping Matrix**, which classifies GenAI workloads into five scopes (from consumer apps through self-trained models) and prescribes security controls for each. Also relevant: the NIST AI Risk Management Framework and ISO/IEC 42001.
- **Transparency standards** — model cards, service cards, user disclosure.
- **Team training** — everyone building or using AI understands the policies and risks.
- **Roles and accountability** — clear ownership for each model in production.

---

## Practice Questions

**1.** Under the AWS shared responsibility model, who is responsible for configuring IAM permissions that control who can invoke a Bedrock model?

A. AWS · B. The customer · C. The model provider · D. Shared equally

<details><summary>Answer</summary>

**B — The customer.** AWS secures the infrastructure *of* the cloud; the customer controls access, data, and configuration *in* the cloud.
</details>

**2.** A company must find and classify PII sitting in S3 buckets used for model training. Which service?

A. Amazon Macie · B. AWS Config · C. Amazon Inspector · D. AWS Artifact

<details><summary>Answer</summary>

**A — Amazon Macie.** Purpose-built for discovering and classifying sensitive data in S3.
</details>

**3.** A security team needs an audit log of every API call made in the account, including who called Bedrock and when. Which service?

A. Amazon CloudWatch · B. AWS CloudTrail · C. AWS Config · D. AWS Trusted Advisor

<details><summary>Answer</summary>

**B — CloudTrail.** Records API activity — the "who did what, when" log. CloudWatch is metrics and logs; Config tracks resource configuration state.
</details>

**4.** An auditor requests AWS's SOC 2 report. Where do you get it?

A. AWS Audit Manager · B. AWS Artifact · C. AWS Config · D. Amazon Inspector

<details><summary>Answer</summary>

**B — AWS Artifact.** The self-service portal for AWS compliance reports and agreements. Audit Manager collects *your* evidence, not AWS's reports.
</details>

**5.** Which two measures most directly reduce hallucinations in a production GenAI application? (Choose two.)

A. RAG grounding with source citations · B. Increasing temperature · C. Contextual grounding checks in Bedrock Guardrails · D. Adding more output tokens · E. Disabling logging

<details><summary>Answer</summary>

**A and C.** Ground answers in retrieved documents and automatically verify that the response is supported by that context.
</details>

**6.** Traffic between an application in a VPC and Amazon Bedrock must never traverse the public internet. Which service?

A. AWS PrivateLink · B. Amazon CloudFront · C. AWS Direct Connect · D. Internet Gateway

<details><summary>Answer</summary>

**A — AWS PrivateLink.** Private connectivity to AWS services over the AWS network.
</details>

**7.** A user submits input designed to override the application's system instructions. Which control best mitigates this?

A. Encryption at rest · B. Amazon Bedrock Guardrails with input filtering · C. Increasing the context window · D. AWS Trusted Advisor

<details><summary>Answer</summary>

**B — Guardrails.** Combined with input sanitization, clear delimiting of user content, and least-privilege permissions for any tools the model can call.
</details>

**8.** GDPR requires a company's EU customer data to remain in Europe. Which governance concept is this?

A. Data lineage · B. Data residency · C. Data cataloging · D. Data retention

<details><summary>Answer</summary>

**B — Data residency (sovereignty).** Satisfied by choosing an EU AWS Region and restricting cross-region movement.
</details>

**9.** Which service continuously evaluates whether AWS resource configurations comply with defined rules?

A. AWS CloudTrail · B. AWS Config · C. Amazon Macie · D. AWS Artifact

<details><summary>Answer</summary>

**B — AWS Config.** Configuration recording plus compliance rule evaluation.
</details>

**10.** What is the purpose of the Generative AI Security Scoping Matrix?

A. Pricing GenAI workloads · B. Classifying GenAI use cases into scopes and prescribing appropriate security controls · C. Measuring model accuracy · D. Selecting a vector database

<details><summary>Answer</summary>

**B.** It defines five scopes — from using consumer apps to training your own models — and the security, governance, and compliance controls appropriate to each.
</details>

**11.** Which two practices support secure data engineering for AI systems? (Choose two.)

A. Granting all users admin access for convenience · B. Encrypting data at rest and in transit · C. Applying least-privilege access controls · D. Storing credentials in application source code · E. Disabling audit logging

<details><summary>Answer</summary>

**B and C.** Encryption everywhere plus least privilege. The other options are all anti-patterns.
</details>

**12.** A company needs to record where each training dataset came from and every transformation applied to it. What is this called?

A. Data lineage · B. Data residency · C. Model distillation · D. Data drift

<details><summary>Answer</summary>

**A — Data lineage.** Often paired with data cataloging (AWS Glue Data Catalog) and documented in SageMaker Model Cards.
</details>

---

## Quick Revision Checklist

- [ ] Shared responsibility: AWS = *of* the cloud, you = *in* the cloud
- [ ] IAM, KMS, Macie, PrivateLink, Guardrails, Secrets Manager — one line each
- [ ] CloudTrail (actions) vs. Config (configurations) vs. Audit Manager (evidence) vs. Artifact (AWS reports)
- [ ] Prompt injection, data leakage, poisoning, model inversion, toxicity — and mitigations
- [ ] Grounding techniques: RAG, contextual grounding checks, output validation, confidence scoring, human review
- [ ] Data lineage, cataloging, residency, retention, lifecycle
- [ ] Generative AI Security Scoping Matrix
- [ ] Encryption at rest **and** in transit is always part of the right answer

**Source:** [AIF-C01 Exam Guide — Domain 5](https://docs.aws.amazon.com/aws-certification/latest/ai-practitioner-01/ai-practitioner-01-domain5.html)
