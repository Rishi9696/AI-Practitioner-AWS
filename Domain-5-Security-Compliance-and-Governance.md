# Domain 5: Security, Compliance, and Governance for AI Solutions
**AIF-C01 · 14% of scored content · ~7 of 50 scored questions**

Two task statements: securing AI systems (5.1), governance and compliance (5.2).

---

## 5.1 Securing AI Systems

### The shared responsibility model applied to AI

- **AWS secures the cloud** — physical infrastructure, hardware, the hypervisor, and the managed service itself (the Bedrock service, the SageMaker platform).
- **You secure what's in the cloud** — your data, IAM users/roles/permissions, encryption choices, network configuration, prompts, guardrails, and how you use model outputs.

*Exam framing:* who controls access to a Bedrock model, or protects the data you put in S3? **You do.**

### Core security services

| Service | Purpose | Cue phrase |
|---|---|---|
| **AWS IAM** | **Users** (a physical person, console password), **Groups** (contain users only), **Policies** (JSON documents defining permissions), **Roles** (for services and EC2 instances). Implement identity verification and resource-level access control. | "control who can invoke the model" |
| **AWS KMS** | Manages encryption keys; S3 server-side encryption with SSE-KMS | "customer-managed encryption keys" |
| **Amazon Macie** | Finds **sensitive data such as PII in Amazon S3 buckets** | "find PII in our training data bucket" |
| **AWS PrivateLink / VPC Endpoints** | Private access to AWS services from within your VPC — API calls never traverse the public internet. (An **S3 Gateway Endpoint** gives private access to S3.) | "traffic must stay inside the VPC" |
| **Amazon Bedrock Guardrails** | Restrict topics, filter harmful content, redact PII, analyze user inputs for safety-policy compliance | "block toxic output / prevent prompt attacks" |
| **Amazon Inspector** | Finds **software vulnerabilities** in EC2, ECR images, and Lambda functions | "scan workloads for vulnerabilities" |
| **AgentCore Identity / Policy** | Identity and permission boundaries for AI agents | "control what an agent is allowed to do" |
| **AWS Secrets Manager** | Store credentials and API keys securely | "don't hard-code secrets" |

**Encryption** — encrypt data **at rest and in transit**, and manage the keys properly so they're protected from unauthorized access. Assume both are part of any correct answer.

### Bedrock security patterns worth knowing

| Requirement | Solution |
|---|---|
| Bedrock must train a custom model from data in an **encrypted S3 bucket** | Give Bedrock an **IAM role** with access to the S3 bucket **and decrypt permission on the KMS key** |
| An application in a VPC must call a Bedrock model **without using the internet** | **VPC endpoint powered by PrivateLink**, with a security group and endpoint policy |
| Audit who invoked which Bedrock API | **AWS CloudTrail** — every API call is recorded as a CloudTrail event with the calling identity |
| Track configuration changes within Bedrock | **AWS Config** |
| Restrict topics and filter harmful content | **Bedrock Guardrails** |
| Run SageMaker training containers with **no outbound internet at all** | SageMaker **network isolation mode** |

### Data lineage and source citation

- **Data lineage** — the record of where data came from and every transformation applied. It covers **source citation** (attributing datasets, databases, and their **licences, terms of use, or permissions**) and **documenting data origins** (how the data was collected, how it was cleaned and curated, and what pre-processing and transformations were applied).
- **Cataloging** — organizing and documenting datasets so they can be found and understood (AWS Glue Data Catalog).
- Together these deliver **transparency, traceability, and accountability**.
- **SageMaker Model Cards** document a model's data sources, intended use, and limitations — and in GenAI can include source citations.

### Secure data engineering best practices

**Assess data quality** on four axes:

| Axis | Meaning |
|---|---|
| **Completeness** | Covers a diverse and comprehensive range of scenarios |
| **Accuracy** | Correct, up to date, and representative |
| **Timeliness** | The age of the data in the data store is appropriate |
| **Consistency** | Coherent and consistent throughout the data lifecycle |

**Privacy-enhancing technologies** — data **masking** and **obfuscation** to minimize breach impact; **encryption** and **tokenization** to protect data during processing and use; anonymization and pseudonymization.

**Data access control** — a comprehensive data governance framework with clear policies · **role-based access control** and fine-grained permissions · single sign-on, MFA, and IAM · monitor and log all data access · regularly review access rights under **least privilege**.

**Data integrity** — data is complete, consistent, and error-free · robust backup and recovery · maintain data lineage and audit trails · monitor and test the integrity controls.

Plus **data profiling and monitoring** to catch quality problems early.

### Security and privacy considerations for AI systems

| Area | What to do |
|---|---|
| **Threat detection** | Watch for fake content, manipulated data, and automated attacks; deploy AI-based detection analyzing network traffic and user behavior |
| **Vulnerability management** | Identify software bugs and model weaknesses; run security assessments, penetration tests, and code reviews; keep patching current |
| **Infrastructure protection** | Secure the cloud platform, edge devices, and data stores with access control, network segmentation, and encryption; withstand system failures |
| **Prompt injection** | Manipulated input prompts producing malicious or undesirable output — mitigate with guardrails, prompt filtering, sanitization, and validation |
| **Data encryption** | At rest and in transit, with properly managed keys |
| **Data leakage prevention** | Output filtering, PII redaction, log scrubbing |
| **Audit trails and logging** | Log all AI interactions — prompts, responses, and who called what (CloudTrail plus application logging) |
| **Toxicity** | Content filters and human review |

**Monitoring AI systems** covers three layers:

1. **Performance metrics** — accuracy, precision, recall, F1, and **latency** (time to make a prediction).
2. **Infrastructure monitoring** — CPU and GPU usage, network performance, storage, and system logs, to catch bottlenecks and failures.
3. **Bias and fairness, compliance, and responsible AI** monitoring.

### Hallucination detection and grounding

1. **RAG grounding** — force answers to come from retrieved source documents.
2. **Contextual grounding checks** (Bedrock Guardrails) — automatically score whether a response is supported by the provided context and block it if not.
3. **Output validation** — schema checks, business-rule validation, fact-checking against a trusted system.
4. **Confidence scoring** — flag low-confidence responses for human review.
5. **Citations** — require the model to cite sources so users can verify.
6. **Human-in-the-loop** (Amazon A2I) for high-stakes outputs.

---

## 5.2 Governance and Compliance

### Governance and audit services — memorize the one-liners

| Service | One-line purpose |
|---|---|
| **AWS CloudTrail** | Tracks **API calls made by users** in the account — who did what, when |
| **AWS Config** | Tracks **configuration changes** and compliance against rules |
| **AWS Audit Manager** | **Automates evidence collection** for audits; select a prebuilt framework with included controls |
| **AWS Artifact** | Self-service portal to **download AWS compliance reports** (SOC, ISO, PCI) and agreements, including third-party reports |
| **Amazon Inspector** | Finds **software vulnerabilities** in EC2, ECR images, and Lambda |
| **AWS Trusted Advisor** | Best-practice **insights and recommendations** (cost, performance, security, service limits) |
| **Amazon CloudWatch** | Metrics, logs, and alarms — operational monitoring |
| **AWS Organizations / SCPs** | Multi-account governance and permission guardrails |

*The four most-confused:* **CloudTrail = actions. Config = configurations. Audit Manager = audit evidence. Artifact = AWS's own compliance documents.**

### Why governance matters

Governance manages risk, ensures regulatory compliance, builds trust, and keeps AI aligned with organizational values. A typical framework:

1. **Establish an AI governance board or committee** — with representatives from legal, compliance, data privacy, and AI subject-matter experts.
2. **Define roles and responsibilities** — oversight, policy-making, risk assessment, decision-making.
3. **Implement policies and procedures** covering the entire AI lifecycle, from data management through deployment and monitoring.

### Governance strategies

**Policies** — principles, guidelines, and responsible-AI considerations covering data management, model training, output validation, safety, human oversight, intellectual property, bias mitigation, and privacy protection.

**Review cadence** — a combination of technical, legal, and responsible-AI review on a clear timeline (monthly, quarterly, annually), including SMEs, legal and compliance teams, and end users.

**Review strategies** — **technical reviews** of model performance, data quality, and algorithm robustness; **non-technical reviews** of policies, responsible-AI principles, and regulatory requirements; testing and validation of outputs before deploying a new model; clear decision-making frameworks for acting on results.

**Transparency standards** — publish information about models, training data, and key decisions; document limitations, capabilities, and use cases; provide channels for users and stakeholders to raise concerns.

**Team training requirements** — train staff on policies, guidelines, and best practices, including bias mitigation and responsible AI; encourage cross-functional collaboration; run a training and certification program.

**Data governance structure** — establish a data governance council; define clear roles for **data stewards, data owners, and data custodians**; support AI/ML practitioners with training; put **data sharing agreements** in place, and consider **data virtualization or federation** to grant access without compromising ownership.

### Data management concepts

| Concept | Meaning |
|---|---|
| **Data lifecycle** | Collection → processing → storage → consumption → archival |
| **Data logging** | Tracking inputs, outputs, performance metrics, and system events |
| **Data residency** | Where data is processed and stored — driven by regulations, privacy requirements, and proximity of compute to data |
| **Data monitoring** | Data quality, anomalies, and **data drift** |
| **Data analysis** | Statistical analysis, visualization, exploration |
| **Data retention** | How long data is kept — regulatory requirements, historical training value, and cost |

### The Generative AI Security Scoping Matrix

A framework to identify and manage the security risks of deploying GenAI applications. It classifies your app into **five scopes, from low to high ownership**:

| Scope | Description | Example |
|---|---|---|
| **1 — Consumer app** | Using public GenAI services | ChatGPT, Midjourney |
| **2 — Enterprise app** | Using an app or SaaS with GenAI features | Salesforce Einstein GPT, Amazon Q Developer |
| **3 — Pre-trained models** | Building your app on a versioned model | Amazon Bedrock base models |
| **4 — Fine-tuned models** | Fine-tuning a model on your data | Bedrock customized models, SageMaker JumpStart |
| **5 — Self-trained models** | Training a model from scratch on your data | SageMaker |

Across all five scopes it addresses **governance and compliance, legal and privacy, risk management, controls, and resilience**. The more ownership you take, the more security responsibility falls to you.

Other frameworks to recognize: the **NIST AI Risk Management Framework** and **ISO/IEC 42001**.

---

## Practice Questions — Domain 5 only

**1.** Under the AWS shared responsibility model, who is responsible for configuring IAM permissions controlling who can invoke a Bedrock model?

A. AWS · B. The customer · C. The model provider · D. Shared equally

<details><summary>Answer</summary>

**B — The customer.** AWS secures the infrastructure *of* the cloud; the customer controls access, data, and configuration *in* the cloud.
</details>

**2.** A company must find and classify PII sitting in S3 buckets used for model training. Which service?

A. Amazon Macie · B. AWS Config · C. Amazon Inspector · D. AWS Artifact

<details><summary>Answer</summary>

**A — Amazon Macie.** Purpose-built to discover and classify sensitive data such as PII in Amazon S3.
</details>

**3.** A security team needs an audit log of every API call made in the account, including who called Bedrock and when. Which service?

A. Amazon CloudWatch · B. AWS CloudTrail · C. AWS Config · D. AWS Trusted Advisor

<details><summary>Answer</summary>

**B — CloudTrail.** It records API activity with the calling identity. CloudWatch handles metrics and logs; Config tracks resource configuration state.
</details>

**4.** An auditor requests AWS's SOC 2 report. Where do you get it?

A. AWS Audit Manager · B. AWS Artifact · C. AWS Config · D. Amazon Inspector

<details><summary>Answer</summary>

**B — AWS Artifact.** The self-service portal for AWS compliance reports (SOC, ISO, PCI) and agreements. Audit Manager collects evidence about *your* environment, not AWS's reports.
</details>

**5.** Bedrock must train a custom model from data in an S3 bucket encrypted with SSE-KMS. What must be configured?

A. A public S3 bucket policy · B. An IAM role granting Bedrock access to S3 and decrypt permission on the KMS key · C. An internet gateway · D. A CloudFront distribution

<details><summary>Answer</summary>

**B.** Bedrock needs an IAM role with both S3 access and KMS decrypt permission to read the encrypted training data.
</details>

**6.** Traffic between an application in a VPC and Amazon Bedrock must never traverse the public internet. Which solution?

A. VPC endpoint powered by AWS PrivateLink · B. Amazon CloudFront · C. AWS Direct Connect · D. Internet gateway

<details><summary>Answer</summary>

**A — VPC endpoint with PrivateLink**, secured with a security group and endpoint policy. This keeps all Bedrock API calls inside the private VPC.
</details>

**7.** A user submits input designed to override the application's system instructions. Which control best mitigates this?

A. Encryption at rest · B. Bedrock Guardrails with prompt filtering, sanitization, and validation · C. Increasing the context window · D. AWS Trusted Advisor

<details><summary>Answer</summary>

**B.** Prompt injection is mitigated with guardrails plus input filtering, sanitization, and validation — reinforced by least-privilege permissions on any tools the model can call.
</details>

**8.** GDPR requires a company's EU customer data to remain in Europe. Which data management concept is this?

A. Data lineage · B. Data residency · C. Data cataloging · D. Data retention

<details><summary>Answer</summary>

**B — Data residency.** Where data is processed and stored, driven by regulations, privacy requirements, and proximity of compute to data. Satisfied by choosing an EU Region.
</details>

**9.** Which service continuously evaluates whether AWS resource configurations comply with defined rules?

A. AWS CloudTrail · B. AWS Config · C. Amazon Macie · D. AWS Artifact

<details><summary>Answer</summary>

**B — AWS Config.** Configuration change tracking plus compliance rule evaluation.
</details>

**10.** A company fine-tunes a Bedrock base model on its own data. Which scope of the Generative AI Security Scoping Matrix is this?

A. Scope 1 · B. Scope 2 · C. Scope 3 · D. Scope 4

<details><summary>Answer</summary>

**D — Scope 4 (fine-tuned models).** Scope 1 is consumer apps, Scope 2 enterprise SaaS with GenAI features, Scope 3 building on a pre-trained versioned model, and Scope 5 training from scratch.
</details>

**11.** Which two are dimensions of assessing data quality in secure data engineering? (Choose two.)

A. Completeness · B. Compression ratio · C. Timeliness · D. Instance type · E. Region count

<details><summary>Answer</summary>

**A and C.** The four axes are completeness, accuracy, timeliness, and consistency.
</details>

**12.** A company needs to record where each training dataset came from, its licence, and every transformation applied to it. What is this called?

A. Data lineage · B. Data residency · C. Model distillation · D. Data drift

<details><summary>Answer</summary>

**A — Data lineage**, which includes source citation (attribution and licences) and documenting data origins (collection, cleaning, curation, and transformations). Paired with cataloging and documented in Model Cards.
</details>

**13.** Which two privacy-enhancing technologies protect data during processing and use? (Choose two.)

A. Data masking · B. Increasing batch size · C. Tokenization · D. Raising temperature · E. Disabling logging

<details><summary>Answer</summary>

**A and C.** Masking and obfuscation minimize breach impact; encryption and tokenization protect data during processing and usage.
</details>

**14.** A SageMaker training job must run with **no outbound internet access at all**, not even to Amazon S3. What should be enabled?

A. PrivateLink · B. Network isolation mode · C. A NAT gateway · D. Guardrails

<details><summary>Answer</summary>

**B — Network isolation mode.** SageMaker job containers run with no outbound internet connectivity whatsoever.
</details>

**15.** Which two practices support secure data access control for AI systems? (Choose two.)

A. Grant all users admin access for convenience · B. Role-based access control with fine-grained permissions · C. Hard-code credentials in source code · D. Regularly review access rights under least privilege · E. Disable audit logging

<details><summary>Answer</summary>

**B and D.** RBAC with fine-grained permissions, SSO/MFA, monitoring and logging all access, and periodic least-privilege reviews. The other options are anti-patterns.
</details>

---

## Quick Revision Checklist

- [ ] Shared responsibility: AWS = security *of* the cloud, you = security *in* the cloud
- [ ] IAM users/groups/policies/roles · KMS · Macie (PII in S3) · PrivateLink/VPC endpoints · Guardrails · Inspector · Secrets Manager
- [ ] Bedrock patterns: IAM role + KMS decrypt for encrypted S3 · PrivateLink for private access · CloudTrail for API audit · Config for configuration changes
- [ ] SageMaker network isolation mode = no outbound internet
- [ ] **CloudTrail (API calls) vs. Config (configurations) vs. Audit Manager (evidence) vs. Artifact (AWS reports) vs. Inspector (vulnerabilities) vs. Trusted Advisor (recommendations)**
- [ ] Data quality axes: completeness, accuracy, timeliness, consistency
- [ ] Privacy-enhancing tech: masking, obfuscation, encryption, tokenization
- [ ] Data lineage = source citation + documenting origins; plus cataloging
- [ ] Data management: lifecycle, logging, residency, monitoring, analysis, retention
- [ ] Prompt injection, data leakage, poisoning, toxicity — and their mitigations
- [ ] Grounding: RAG, contextual grounding checks, output validation, confidence scoring, human review
- [ ] **GenAI Security Scoping Matrix — all five scopes with their examples**
- [ ] Governance: board/committee, roles, policies, review cadence, transparency standards, team training
- [ ] Encryption at rest **and** in transit is always part of the right answer

**Sources:** [AIF-C01 Exam Guide — Domain 5](https://docs.aws.amazon.com/aws-certification/latest/ai-practitioner-01/ai-practitioner-01-domain5.html) · *AWS Certified AI Practitioner* course slides (Stéphane Maarek), used as a reference for scope and terminology.
