# Domain 4: Guidelines for Responsible AI
**AIF-C01 · 14% of scored content · ~7 of 50 scored questions**

Two task statements: developing responsible AI systems (4.1), transparency and explainability (4.2).

---

## 4.1 Developing Responsible AI Systems

### The core dimensions of responsible AI

AWS defines eight. Learn the one-line meaning of each.

| Dimension | Meaning | Example failure |
|---|---|---|
| **Fairness** | Promote inclusion and prevent discrimination | A hiring model favors one gender |
| **Explainability** | Understand how and why outputs are produced | A loan denial with no reason given |
| **Privacy and security** | Individuals control when and if their data is used | Training data leaks through model outputs |
| **Transparency** | Stakeholders understand what the system is, how it was built, and its limits | Users don't know they're talking to AI |
| **Veracity and robustness** | Reliable and correct **even in unexpected situations** | Hallucinated citations; breaks on unusual input |
| **Governance** | Define, implement, and enforce responsible AI practices | No approval process before deployment |
| **Safety** | Algorithms are safe and beneficial for individuals and society | A chatbot giving dangerous instructions |
| **Controllability** | Ability to align the system to human values and intent | No way to steer or disable a misbehaving feature |

Also tested: **inclusivity** — the system works for all user groups, including different languages and accessibility needs.

**Expanded explanations:**

- **Fairness** — predictions shouldn't systematically disadvantage a group based on protected characteristics (gender, race, age). Training data reflects historical human bias, and models reproduce and can amplify it at scale. E.g. a hiring model learns from mostly-male past engineers and scores women lower even when nothing in the job description calls for that.
- **Explainability** — the ability to answer "why did the model produce *this* output" for one individual decision, not just "does it perform well overall." Matters most in high-stakes individual decisions (credit, hiring, healthcare, justice) where someone has a right to know the basis of a decision about them.
- **Privacy and security** — whether individuals control if/how their data is used, and whether the system protects it from exposure. Distinct risk from normal security: models can memorize and later leak snippets of training data. E.g. a clever prompt makes an LLM reproduce a real phone number from training data.
- **Transparency** — a *system-level* property: do people know they're dealing with AI, and do they understand roughly how it works and its limits. Different from explainability (single decision) — transparency is honest disclosure of the system as a whole. E.g. a support chatbot that doesn't disclose it's automated.
- **Veracity and robustness** — veracity = factually correct outputs; robustness = stays reliable under noisy/unusual/adversarial input rather than breaking or confabulating. Grouped together because both describe reliability under real-world (not curated) conditions. E.g. an LLM inventing a fake legal citation, or a vision model misclassifying after small added noise.
- **Governance** — the organizational layer: policies, review boards, documentation, accountability that actually enforce the other seven principles rather than leaving them aspirational. E.g. a feature ships to production with no risk assessment or owner.
- **Safety** — preventing physical, psychological, or societal harm from outputs/actions, including scenarios the designers didn't anticipate. Distinct from robustness: safety is about harmful consequences even when the system is "working as designed." E.g. a chatbot giving self-harm or weapon-building instructions.
- **Controllability** — whether humans can monitor and intervene (adjust, override, throttle, shut down) when the system misbehaves. The safety net underneath every other dimension. E.g. a recommendation engine pushing extreme content with no way to retune or disable it.

### Bias — the exam's favorite topic

**Formal definitions of bias types:**

| Type | Definition | Example and fix |
|---|---|---|
| **Sampling bias** | The training data does not represent the full population fairly, so the model over-represents or disproportionately affects certain groups | An algorithm flags only people from specific ethnic groups. **Fix: data augmentation for the imbalanced/underrepresented classes.** |
| **Measurement bias** | The tools or measurements used to collect data are flawed or skewed | Using arrests as a proxy for crime rate |
| **Observer bias** | The person collecting or interpreting the data has personal biases that affect the results | An annotator's assumptions leak into the labels |
| **Confirmation bias** | Interpreting or favoring information that confirms pre-existing beliefs — applies more to **human decision-making** than to automated model output | A reviewer accepts results that match their expectations |
| **Historical bias** | Past human decisions embedded in the data become learned patterns | A resume screener trained on a decade of biased hiring |
| **Algorithmic bias** | Arises from model design or applying one model to groups that behave differently | A single risk score used across very different populations |

**Expanded explanations — where each bias enters the pipeline:**

- **Sampling bias** — *Definition:* enters at data collection. The dataset doesn't represent the real-world population fairly, so one group is over- or under-represented, and the model learns patterns that fit the majority well and everyone else poorly.
  *Example:* a facial recognition system trained mostly on lighter-skinned faces performs worse on darker-skinned faces, since that group was underrepresented in training data.

- **Measurement bias** — *Definition:* the metric or tool used to collect data doesn't accurately capture the real-world concept it's meant to represent — collection is consistent, but it's measuring the wrong (or a distorted) thing.
  *Example:* a predictive policing model uses arrest rate as a proxy for crime rate. Arrest rate isn't a clean measurement of actual crime — it reflects where police patrol more, not where crime actually happens — so the model learns policing patterns rather than crime patterns.

- **Observer bias** — *Definition:* enters at labeling/annotation — a human-in-the-loop problem where the person collecting or interpreting the data lets their own assumptions shape ambiguous cases.
  *Example:* an annotator labeling "aggressive" tone in text unconsciously marks certain dialects or phrasing styles as more aggressive based on their own cultural assumptions, and that assumption leaks into the labels.

- **Confirmation bias** — *Definition:* enters at human evaluation of results, not in the data or model itself — an evaluator favors information that confirms what they already expected, and scrutinizes disconfirming results less.
  *Example:* a data scientist expects a new model to outperform the old one, sees a few favorable metrics, and declares success without checking subgroup performance that would have told a different story.

- **Historical bias** — *Definition:* enters even with a perfectly accurate, representative dataset — the data faithfully reflects a world that was itself unfair, so the model learns to reproduce past inequities.
  *Example:* a resume-screening model trained on ten years of a company's hiring decisions learns that most past engineering hires were men, so it downgrades resumes with signals correlated with being a woman, even though the model itself was never told to consider gender.

- **Algorithmic bias** — *Definition:* enters at model design — either the architecture doesn't generalize across subgroups, or one model/threshold is applied uniformly to populations that behave differently.
  *Example:* a single credit-risk score threshold is applied identically to two demographic groups with genuinely different financial behavior patterns, producing unequal outcomes across groups even though the underlying data wasn't biased.

Memory aid: sampling & historical bias live **in the data**; measurement & observer bias live **in how data is collected/labeled**; confirmation bias lives **in the human reviewing results**; algorithmic bias lives **in the model/design applied afterward**.

**Effects of bias:** discriminatory outcomes for demographic groups, reduced accuracy for underrepresented users, legal liability, loss of customer trust, reputational damage.

**Bias and variance also describe model fit** (linking back to Domain 1):

- **High bias → underfitting** — the model is too simple and is wrong for everyone.
- **High variance → overfitting** — the model memorizes training data and fails on new data.
- A model can be accurate overall and still biased against a subgroup — which is why **subgroup analysis** matters.

### Characteristics of good datasets

**Inclusive · diverse · balanced across classes and demographic groups · curated from trustworthy sources · accurately and consistently labeled · large enough · documented (lineage, licences, known quality issues) · refreshed regularly.**

### AWS tools for responsible AI

| Tool | What it does |
|---|---|
| **Amazon Bedrock Guardrails** | Controls the interaction between users and FMs: **filters undesirable and harmful content**, **removes PII**, **blocks denied topics**, applies word filters, and **reduces hallucinations** via contextual grounding checks. You can create multiple guardrails and monitor inputs that violate them. |
| **Amazon Bedrock Model Evaluation** | Human or automatic evaluation of models — including for accuracy, robustness, and toxicity |
| **SageMaker Clarify** | **Detects and explains bias** in datasets and models using statistical metrics; provides **model explainability** (feature attribution) both before deployment and to debug predictions afterwards. Also evaluates FMs on accuracy, robustness, and toxicity, and on human factors such as friendliness or humor. |
| **SageMaker Data Wrangler** | **Fixes bias by balancing the dataset** — for example augmenting data to generate new instances for underrepresented groups |
| **SageMaker Model Monitor** | Continuous or scheduled monitoring of model quality in production, with alerts on deviation (drift), so you can fix data and retrain |
| **SageMaker Ground Truth** | Human labeling, RLHF, model review and evaluation, aligning models to human preferences |
| **Amazon Augmented AI (A2I)** | **Human review of ML predictions** — route low-confidence or high-stakes predictions to people |
| **SageMaker Role Manager, Model Cards, Model Dashboard** | Governance: access control, documentation, and a central view of all models |
| **Human audits, subgroup analysis, label-quality review** | Non-tool practices — measure performance separately per demographic group |

*Guardrails are the answer whenever a question mentions blocking toxic content, restricting topics, filtering PII, or reducing hallucinations at the application layer.*

**Four extra exam notes for this domain:**

1. Expect questions pairing a responsible-AI dimension with the AWS service that addresses it — e.g. fairness/bias → Clarify, drift → Model Monitor, application-layer safety → Bedrock Guardrails, documentation/transparency → Model Cards / AI Service Cards.
2. Fairness and bias questions often reference the specific bias types above (sampling, measurement, observer, confirmation, historical, algorithmic) — know they originate in data collection, labeling, human evaluation, or model design respectively, not interchangeably.
3. Explainability questions are frequently framed as "black box" vs. "interpretable" — simpler models (linear/logistic regression, decision trees) are more explainable; deep neural networks and ensembles are less so, and that's a real accuracy-vs-transparency tradeoff, not a flaw to "fix."
4. Governance questions often reference **AWS AI Service Cards** and **SageMaker Model Cards** by name — these are AWS's concrete documentation artifacts for transparency and governance, and the exam likes to test the distinction between the two (AI Service Cards = AWS's own services; Model Cards = your models).

### GenAI challenges and their mitigations

**Toxicity** — generating offensive, disturbing, or inappropriate content. Defining "toxicity" is itself difficult: where is the boundary between restricting harmful content and censorship, and how do you treat quotations of someone else's toxic words?
*Mitigation:* curate training data to identify and remove offensive phrases in advance; use guardrail models to detect and filter unwanted content.

**Hallucinations** — assertions that sound true but are incorrect, caused by the **next-word probability sampling** LLMs use. The content may not exist at all while seeming entirely plausible.
*Mitigation:* educate users that generated content must be checked; verify against independent sources; **mark generated content as unverified** so users know verification is needed; ground with RAG and citations.

**Plagiarism and cheating** — GenAI used to write essays, job applications, and other work. It is difficult to trace a specific output back to its source, which has driven the rise of AI-detection tooling and ongoing debate about acceptance versus prohibition.

### Legal and business risks of GenAI

- **Intellectual property infringement claims** — outputs may resemble copyrighted training material; ownership of generated content is unsettled.
- **Biased model outputs** → discrimination claims and regulatory exposure.
- **Hallucinations** → users act on false information; liability for the business.
- **Data privacy** — sensitive data in prompts; regulated data crossing borders.
- **Loss of customer trust** — the hardest damage to repair.
- **End-user risk** — harm from misuse or over-reliance on outputs.

**Expanded explanations:**

- **Intellectual property infringement claims** — *Definition:* GenAI models are trained on huge datasets that often include copyrighted material (text, images, code), and outputs can end up resembling or reproducing pieces of that material closely enough to raise infringement concerns; who owns AI-generated content (the prompter, the model builder, or no one) is still legally unresolved in most jurisdictions.
  *Example:* a business publishes AI-generated marketing copy that closely echoes a copyrighted article, and faces both an infringement claim and uncertainty over whether it even owns the copy it published.

- **Biased model outputs → discrimination claims and regulatory exposure** — *Definition:* when a model's outputs systematically disadvantage a protected group, that becomes a legal issue, not just a technical one — affected individuals can bring discrimination claims, and regulators in sectors like finance and employment can impose fines or sanctions.
  *Example:* a lending model that scores applicants from a certain zip code lower triggers a fair-lending investigation, since biased automated decisions are treated the same as biased human decisions under the law.

- **Hallucinations → liability for the business** — *Definition:* hallucinated outputs look confident and plausible even when false, so users may act on them without verifying; if the deploying business's customer suffers harm relying on that false output, the business can be held liable even though the model invented the error.
  *Example:* a legal-assistant chatbot cites a fabricated case, a lawyer submits it in court, and the firm faces sanctions and reputational damage.

- **Data privacy** — *Definition:* users often paste sensitive information into prompts (personal data, trade secrets, health records) without realizing where it's stored or how it's used afterward, and infrastructure that processes data across borders can violate laws restricting international transfer of personal data (e.g. GDPR).
  *Example:* an employee pastes a customer's medical record into a prompt to summarize it, and that data is now stored on servers in a different country than regulations permit.

- **Loss of customer trust** — *Definition:* trust erodes quickly from a single bad, biased, or leaked output, but doesn't rebuild through a technical fix alone — it typically requires sustained transparency and a track record of reliability over time, making it the hardest kind of damage to repair.
  *Example:* after a chatbot gives a customer harmful financial advice, the company patches the model within a day, but customers keep avoiding the feature for months.

- **End-user risk** — *Definition:* harm to the person actually using or affected by the system — either through misuse of the tool for a purpose it wasn't designed for, or through over-reliance on its outputs as if they were guaranteed correct (automation bias).
  *Example:* a student submits an AI-generated answer containing a hallucinated fact without checking it, because they assumed the tool "wouldn't just make something up."

### Compliance challenges specific to AI

| Challenge | Why it's hard |
|---|---|
| **Complexity and opacity** | It's challenging to audit how AI systems reach decisions |
| **Dynamism and adaptability** | AI systems change over time — they aren't static software |
| **Emergent capabilities** | Systems may develop unintended capabilities |
| **Unique risks** | Algorithmic bias, privacy violations, misinformation |
| **Algorithmic bias** | Unrepresentative data causes the model to perpetuate bias |
| **Human bias** | The people who build the system introduce bias too |
| **Algorithm accountability** | Algorithms should be transparent and explainable |

**Regulation:** the **EU Artificial Intelligence Act**, plus rules in several US states and cities, promoting fairness, non-discrimination, and human rights.

**Regulated workloads** — some industries (financial services, healthcare, aerospace) require an extra level of compliance: regular reporting to federal agencies, regulated outcomes such as mortgage and credit decisions, audit trails, archival, and special security requirements. If you must comply with such frameworks, you have a regulated workload.

### Responsible model selection

Pick the **smallest model that satisfies the requirement** — this reduces **energy consumption and carbon footprint** as well as cost. Also consider the provider's transparency documentation, licensing terms, whether the model's intended use matches yours, and its safety record.

---

## 4.2 Transparency and Explainability

### Interpretability vs. explainability

**Interpretability** — the degree to which a **human can understand the cause of a decision**; access into the system so a human can interpret its output. It answers **"why and how."**

**Explainability** — understanding the **nature and behavior** of the model: being able to look at inputs and outputs and explain the relationship **without necessarily understanding exactly how** the model reached the conclusion. Often, **explainability is enough**.

**Transparency** — openness *about* the system: what data it was trained on, its intended use, its limitations, who is accountable, and disclosing to users that AI is involved.

### The interpretability trade-off

**High transparency ⇒ high interpretability ⇒ lower performance.**

Ordered from most interpretable / least performant to least interpretable / most performant:

```
Linear Regression → Decision Tree → Logistic Regression → Naïve Bayes
   → K-Nearest Neighbors → Support Vector Machines → Ensemble Methods → Neural Networks
```

| Interpretable ("white box") | Black box |
|---|---|
| Linear/logistic regression, decision trees, Naïve Bayes | Ensemble methods, deep neural networks, LLMs |
| Decision logic can be read directly | Reasoning is not humanly traceable |
| Generally lower accuracy on complex problems | Generally higher accuracy |
| Required in regulated settings (lending, hiring, healthcare) | Fine where explanations aren't legally required |

**Decision trees** are the canonical high-interpretability model — you can literally follow the branches. *Post-hoc* techniques such as **SHAP**, **LIME**, and **partial dependence plots (PDP)** add partial explanations to black-box models without making them truly transparent.

### Tools for transparency and explainability

| Tool | Purpose |
|---|---|
| **SageMaker Model Cards** | Standardized documentation of a model's key details: **intended uses, risk rating, training details and metrics**, datasets used, their sources and licences, and any known biases or quality issues. In GenAI they can include **source citations and data-origin documentation**. Supports audit activities. |
| **AWS AI Service Cards** | AWS's own responsible-AI documentation for its AI services: intended use cases and **limitations**, responsible-AI design choices, and deployment/performance best practices |
| **SageMaker Model Dashboard** | Central view of all models and their insights |
| **SageMaker Clarify** | Feature attribution — which inputs drove a prediction; answers "why did the model reject this applicant?" |
| **Amazon Bedrock Model Evaluation** | Compare models on accuracy, robustness, and toxicity |
| **Open-source models, open data, clear licensing** | Inherently more transparent than closed models |

### Human-Centered Design (HCD) for explainable AI

Designing AI systems around human needs:

- **Design for amplified decision-making** — support the human, especially in stressful or high-pressure environments, and minimize risk and errors.
- **Design for clarity, simplicity, and usability.**
- **Design for reflexivity and accountability** — let people reflect on the decision-making process and assign responsibility.
- **Design for unbiased decision-making** — keep the decision process free from bias and **train decision-makers to recognize and mitigate their own biases**.
- **Design for human and AI learning** — *cognitive apprenticeship* (AI learns from human instructors and experts), *personalization* to the learner's needs, and *user-centered design* accessible to a wide range of users.

In practice this also means: disclose when a user is interacting with AI, show sources behind an answer, provide **feedback mechanisms**, keep a human in the loop for high-stakes decisions, and set honest expectations about capabilities and limits.

---

## Practice Questions — Domain 4 only

**1.** An algorithm flags people from only certain ethnic groups because those groups were over-represented in the training data. What type of bias is this, and what is the fix?

A. Measurement bias; recalibrate the sensors · B. Sampling bias; data augmentation for imbalanced classes · C. Confirmation bias; retrain the reviewers · D. Observer bias; change the labeling team

<details><summary>Answer</summary>

**B — Sampling bias.** The training data doesn't represent the full population fairly. The standard remedy is data augmentation to balance underrepresented classes (SageMaker Data Wrangler can do this).
</details>

**2.** Which AWS service detects and explains bias in datasets and models, and shows which features drove a prediction?

A. Amazon Macie · B. SageMaker Clarify · C. AWS CloudTrail · D. Amazon Comprehend

<details><summary>Answer</summary>

**B — SageMaker Clarify.** Bias detection using statistical metrics, plus model explainability before deployment and for debugging predictions afterwards.
</details>

**3.** A company needs its customer chatbot to refuse to discuss competitors, block toxic language, and redact customer PII. Which feature?

A. SageMaker Model Monitor · B. Amazon Bedrock Guardrails · C. AWS Config · D. Amazon A2I

<details><summary>Answer</summary>

**B — Bedrock Guardrails.** Blocked topics, harmful-content filters, PII removal, and reduced hallucinations, with monitoring of violating inputs.
</details>

**4.** Which model type sits at the **high interpretability, lower performance** end of the trade-off?

A. Neural networks · B. Ensemble methods · C. Decision trees · D. Support vector machines

<details><summary>Answer</summary>

**C — Decision trees.** The interpretability ordering runs roughly: linear regression → decision tree → logistic regression → Naïve Bayes → k-NN → SVM → ensembles → neural networks, with performance increasing and interpretability falling along that line.
</details>

**5.** Which artifact documents a model's intended uses, risk rating, training details, dataset sources and licences, and known biases?

A. AWS Artifact · B. SageMaker Model Cards · C. CloudWatch Logs · D. IAM policy

<details><summary>Answer</summary>

**B — SageMaker Model Cards.** (AWS Artifact provides AWS's own compliance reports such as SOC and ISO — a common distractor.) AWS AI Service Cards are the equivalent documentation for AWS AI services.
</details>

**6.** A medical imaging model flags uncertain predictions for review by a radiologist. Which service supports this?

A. Amazon A2I · B. Amazon Polly · C. AWS Glue · D. Amazon Kendra

<details><summary>Answer</summary>

**A — Amazon Augmented AI (A2I).** Human review workflows for low-confidence or high-stakes ML predictions.
</details>

**7.** Which two are recommended mitigations for hallucinations? (Choose two.)

A. Increase temperature · B. Educate users that generated content must be verified · C. Mark generated content as unverified · D. Remove the validation set · E. Disable guardrails

<details><summary>Answer</summary>

**B and C.** Because hallucinations stem from next-word probability sampling, mitigation focuses on user awareness, independent verification, and flagging content as unverified — alongside RAG grounding.
</details>

**8.** A model has 94% overall accuracy but only 61% accuracy for one demographic group. Which practice would have revealed this?

A. Increasing the learning rate · B. Subgroup analysis · C. Reducing the model size · D. Batch inference

<details><summary>Answer</summary>

**B — Subgroup analysis.** Aggregate accuracy hides disparities; evaluate performance separately per group.
</details>

**9.** What is the difference between interpretability, explainability, and transparency?

<details><summary>Answer</summary>

**Interpretability** is the degree to which a human can understand the *cause* of a specific decision — it answers "why and how." **Explainability** is understanding the model's nature and behavior from inputs and outputs, without necessarily knowing the internal mechanism; often this is sufficient. **Transparency** is openness about the system overall — training data, intended use, limitations, accountability, and disclosing AI involvement to users.
</details>

**10.** Which choice reflects sustainability considerations when selecting a model?

A. Always choose the largest available model · B. Choose the smallest model meeting requirements, reducing compute and energy use · C. Train a new model from scratch per task · D. Disable monitoring to save resources

<details><summary>Answer</summary>

**B.** Right-sizing reduces energy consumption, carbon footprint, and cost at the same time.
</details>

**11.** Which service continuously watches a deployed model for quality deviations such as drift, so you can fix the data and retrain?

A. SageMaker Model Monitor · B. AWS Trusted Advisor · C. Amazon Inspector · D. Amazon Macie

<details><summary>Answer</summary>

**A — SageMaker Model Monitor.** Continuous or scheduled monitoring with alerts — for example, a loan model that begins approving applicants who don't meet the credit criteria.
</details>

**12.** Which two are principles of Human-Centered Design for explainable AI? (Choose two.)

A. Hide AI involvement to avoid confusing users · B. Design for amplified decision-making · C. Design for reflexivity and accountability · D. Remove human review to increase speed · E. Maximize model complexity

<details><summary>Answer</summary>

**B and C.** HCD emphasizes amplifying human decision-making, clarity and usability, reflexivity and accountability, unbiased decision-making, and human/AI learning. Hiding AI involvement and removing oversight are the opposite of responsible design.
</details>

**13.** Which two are AI-specific standard compliance challenges? (Choose two.)

A. AI systems are static and never change · B. Complexity and opacity make decisions hard to audit · C. Emergent capabilities may be unintended · D. AI cannot process structured data · E. Algorithms are always fully transparent

<details><summary>Answer</summary>

**B and C.** AI compliance is hard precisely because systems are opaque, dynamic and adaptive, and may develop unintended capabilities — alongside algorithmic and human bias.
</details>

**14.** A bank's mortgage approval system must report regularly to federal agencies and maintain audit archives. What is this called?

A. A serverless workload · B. A regulated workload · C. An edge workload · D. A batch workload

<details><summary>Answer</summary>

**B — A regulated workload.** Industries such as financial services, healthcare, and aerospace require extra compliance: regular regulatory reporting, regulated outcomes such as credit decisions, audit, archival, and special security requirements.
</details>

---

## Quick Revision Checklist

- [ ] Eight responsible-AI dimensions: fairness, explainability, privacy & security, transparency, veracity & robustness, governance, safety, controllability
- [ ] Bias types with definitions: **sampling, measurement, observer, confirmation** (+ historical, algorithmic)
- [ ] Sampling bias → fix with **data augmentation for imbalanced classes** (Data Wrangler)
- [ ] High bias → underfitting; high variance → overfitting
- [ ] Clarify (detect bias + explain) · Data Wrangler (balance data) · Model Monitor (drift) · A2I (human review) · Guardrails (filter output) · Ground Truth (labeling + RLHF)
- [ ] Model Cards = your model's documentation; AI Service Cards = AWS service documentation
- [ ] Interpretability vs. explainability vs. transparency
- [ ] Interpretability ladder: linear regression → decision tree → … → ensembles → neural networks
- [ ] Toxicity, hallucination, plagiarism — and the specific mitigations for each
- [ ] GenAI legal risks: IP infringement, biased output, hallucination, privacy, lost trust
- [ ] Compliance challenges: complexity/opacity, dynamism, emergent capabilities, algorithmic and human bias
- [ ] EU AI Act; regulated workloads (finance, healthcare, aerospace)
- [ ] HCD principles: amplified decision-making, clarity, reflexivity & accountability, unbiased decisions, human and AI learning
- [ ] Smaller model = less energy = the sustainability answer

**Sources:** [AIF-C01 Exam Guide — Domain 4](https://docs.aws.amazon.com/aws-certification/latest/ai-practitioner-01/ai-practitioner-01-domain4.html) · *AWS Certified AI Practitioner* course slides (Stéphane Maarek), used as a reference for scope and terminology.
