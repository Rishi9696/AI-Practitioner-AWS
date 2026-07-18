# Domain 1: Fundamentals of AI and ML
**AIF-C01 · 20% of scored content · ~10 of 50 scored questions**

Three task statements: basic concepts (1.1), practical use cases (1.2), the AI/ML lifecycle (1.3).

---

## 1.1 Basic AI Concepts and Terminology

### The nesting doll

```
AI  ⊃  ML  ⊃  Deep Learning  ⊃  Generative AI (LLMs)
```

| Term | What it means | Quick example |
|---|---|---|
| **AI** | Any technique making machines mimic human intelligence — includes rule-based systems | A thermostat rule: "if temp > 25°C, turn on AC" |
| **ML** | Systems that *learn patterns from data* instead of being explicitly programmed | Spam filter trained on 100k labeled emails |
| **Deep Learning** | ML using multi-layer **neural networks**; learns features automatically | Recognizing cats in photos without anyone defining "whiskers" |
| **Generative AI** | Deep learning models that *create* new content (text, image, code) | Claude writing an email; Stable Diffusion making an image |
| **Agentic AI** | A model that plans multi-step actions and **calls tools** to achieve a goal | An agent that reads a ticket, queries a DB, and files a refund |

**Key distinction to memorize:** traditional ML *predicts a value or label*. GenAI *produces new content*. Agentic AI *takes actions*.

### Terms you must be able to define

- **Model** — the trained artifact (learned weights/parameters).
- **Algorithm** — the procedure used to produce the model (e.g., XGBoost, linear regression).
- **Training** — feeding data to learn parameters. Expensive, done once/periodically.
- **Inference** — using the trained model to make a prediction. Cheap-ish, done constantly.
- **LLM** — large language model; a transformer trained on massive text.
- **Computer vision** — AI on images/video. **NLP** — AI on human language.
- **Bias** — systematic error that unfairly favors/disfavors a group (or a bad assumption in the model).
- **Fairness** — the model treats groups equitably.

### Fit: the classic exam trap

| Condition | Training accuracy | Test accuracy | Fix |
|---|---|---|---|
| **Overfitting** | High | Low | More data, simpler model, regularization, early stopping |
| **Underfitting** | Low | Low | More complex model, more features, train longer |
| **Balanced (good fit)** | High | High | Ship it |

*Memory hook:* overfitting = the student who memorized the practice test. Underfitting = the student who didn't study.

**Bias–variance tradeoff:** high bias → underfitting. High variance → overfitting.

### Types of inference

| Type | When to use | AWS |
|---|---|---|
| **Real-time** | Need answer in milliseconds, steady traffic | SageMaker real-time endpoint |
| **Batch** | Large dataset, no rush, run overnight | SageMaker Batch Transform |
| **Asynchronous** | Large payloads (up to 1 GB) or long processing (minutes) | SageMaker Async Inference |
| **Serverless** | Intermittent/spiky traffic, tolerate cold starts, pay-per-use | SageMaker Serverless Inference |

*Exam cue:* "unpredictable traffic with idle periods, want to minimize cost" → **serverless**. "Score 10 million rows nightly" → **batch**. "Large video files, minutes of processing" → **async**.

### Types of data

- **Structured** — rows and columns (tabular in a database, CSV).
- **Unstructured** — text, images, audio, video. Roughly 80% of enterprise data.
- **Semi-structured** — JSON, XML.
- **Time-series** — data ordered by timestamp (stock prices, sensor readings, demand).
- **Labeled** — each example has the correct answer attached (email + "spam").
- **Unlabeled** — raw data with no answer key.

### Types of learning

| Type | Data needed | Goal | Example |
|---|---|---|---|
| **Supervised** | Labeled | Predict a label/value | Predict house price; classify email as spam |
| **Unsupervised** | Unlabeled | Discover structure | Customer segmentation, anomaly detection |
| **Reinforcement (RL)** | Reward signal from environment | Learn a policy by trial and error | Robot learning to walk; RLHF for aligning LLMs |
| **Semi-supervised** | Small labeled + large unlabeled | Cut labeling cost | Label 1k of 100k medical images |
| **Self-supervised** | Unlabeled, model creates its own labels | Pretrain FMs | LLM predicting the next word |

---

## 1.2 Practical Use Cases for AI

### When AI/ML *is* a good fit

- The pattern is complex and hard to write rules for (image recognition, fraud).
- You have lots of relevant historical data.
- You need scale/automation humans can't match.
- An approximate/probabilistic answer is acceptable.

### When AI/ML is **not** appropriate — high-yield

- **You need a deterministic, exact answer.** Calculating sales tax or interest owed → use code, not a model. *ML gives predictions, not guarantees.*
- **Simple rules already work.** "Flag transactions over $10,000" is a rule, not a model.
- **Cost exceeds benefit.** Building/labeling/hosting outweighs the value gained.
- **No quality data**, or the decision requires full explainability the model can't give.

### Choosing the technique

| Question | Technique | Learning type |
|---|---|---|
| "How much / how many?" (continuous number) | **Regression** | Supervised |
| "Which category?" (spam/not, cat/dog/bird) | **Classification** | Supervised |
| "What natural groupings exist?" | **Clustering** | Unsupervised |
| "What's abnormal here?" | **Anomaly detection** | Unsupervised |
| "What will this be next month?" | **Forecasting** (time-series) | Supervised |
| "What should we show this user next?" | **Recommendation** | Varies |

*Binary vs. multiclass vs. multilabel:* two options / one of many / several at once (a photo tagged "beach" + "sunset").

### AWS managed AI services (know them by one-line purpose)

| Service | Does what | Cue phrase |
|---|---|---|
| **Amazon Comprehend** | NLP: sentiment, entities, key phrases, PII detection, language | "analyze customer reviews for sentiment" |
| **Amazon Transcribe** | Speech → text | "call recordings into transcripts" |
| **Amazon Polly** | Text → lifelike speech | "read articles aloud" |
| **Amazon Translate** | Language translation | "localize content into 20 languages" |
| **Amazon Lex** | Conversational chatbots/IVR (voice + text) | "build a customer service bot" |
| **Amazon Rekognition** | Image/video analysis: objects, faces, moderation | "detect inappropriate images" |
| **Amazon Textract** | Extract text/tables/forms from documents | "digitize scanned invoices" |
| **Amazon Personalize** | Real-time recommendations | "product recommendations like Amazon.com" |
| **Amazon Forecast** | Time-series forecasting | "predict demand for inventory" |
| **Amazon Fraud Detector** | Detect fraudulent online activity | "flag fake account signups" |
| **Amazon Kendra** | Intelligent enterprise search | "search internal documents naturally" |
| **Amazon Bedrock** | Managed access to foundation models (GenAI) | "use an LLM without managing infra" |
| **Amazon Q** | GenAI assistant for business/developers | "chat over company data" |
| **Amazon SageMaker AI** | Build, train, deploy *your own* models end-to-end | "custom model, full control" |

*Exam heuristic:* if the question describes a common task (transcribe, translate, detect objects), pick the **purpose-built managed service** — it needs no ML expertise. Only pick **SageMaker AI** when the requirement is a custom model or full control over training.

### Traditional ML vs. Foundation Models

| Pick **traditional ML** when | Pick **foundation models** when |
|---|---|
| Task is narrow and numeric (credit scoring, churn) | Task involves open-ended language, content generation, or summarization |
| You need **explainability** for regulators | You need broad general capability with little/no training data |
| Tight latency/cost/compute constraints | You can accept higher cost and probabilistic output |
| You have good labeled data for the specific task | You have little labeled data — use prompting instead |

---

## 1.3 The AI/ML Development Lifecycle

### The pipeline (know the order)

1. **Business goal identification** — define the problem and success criteria in business terms.
2. **ML problem framing** — translate it into an ML task (this is a binary classification problem).
3. **Data collection** — gather from S3, databases, streams.
4. **Data preprocessing / EDA** — clean, handle missing values, deduplicate, label (SageMaker Ground Truth).
5. **Feature engineering** — create and select the input variables (SageMaker Feature Store).
6. **Model training and hyperparameter tuning**.
7. **Model evaluation** — test on held-out data against your metrics.
8. **Deployment** — expose as an endpoint or batch job.
9. **Monitoring** — watch for drift, then **retrain** and loop back.

*Data prep is typically the largest share of the effort.*

### Where foundation models come from

- **Pre-trained / open source** — use as-is (fastest, cheapest).
- **Customized** — prompt engineering → RAG → fine-tuning → continued pre-training (increasing cost and effort in that order).
- **Trained from scratch** — rarely justified; enormous cost and data requirements.

### Getting a model into production

- **Managed API service** — Amazon Bedrock, Amazon Q. No infrastructure, pay per token/request, fastest to launch.
- **Self-hosted** — SageMaker endpoint or EC2. More control and customization, more operational burden.

### MLOps in one paragraph

MLOps applies DevOps discipline to ML: version code, **data, and models**; make training repeatable and automated; run experiments trackably; deploy through CI/CD; monitor models in production for **drift** (data distribution changes) and degraded accuracy; retrain on a schedule or trigger. It exists to control technical debt and reach production readiness. *(SageMaker Pipelines, Model Registry, Model Monitor, Clarify.)*

### Model performance metrics

For a classification model, with TP/FP/TN/FN:

- **Accuracy** = correct / total. *Misleading on imbalanced data* — a model that calls every transaction legitimate is 99.9% accurate and useless.
- **Precision** = TP / (TP + FP) → "of the ones I flagged, how many were right?" **Use when false positives are costly** (e.g., wrongly blocking a customer's card).
- **Recall** = TP / (TP + FN) → "of all the real cases, how many did I catch?" **Use when false negatives are costly** (e.g., missing a cancer diagnosis or a fraudulent charge).
- **F1 score** = harmonic mean of precision and recall. Use when you need balance, especially on imbalanced classes.
- **AUC-ROC** — how well the model separates classes across all thresholds; 0.5 = random, 1.0 = perfect.
- **Regression metrics** — MSE / RMSE / MAE: lower is better (average size of the error). R² closer to 1 is better.

*Precision vs. recall trap:* read the scenario for which mistake hurts more. Spam filter → precision (don't lose a real email). Disease screening → recall (don't miss a sick patient).

### Business metrics

Cost per user/inference, development and training cost, time to market, **ROI**, customer feedback and satisfaction, conversion or retention lift. A model can have great accuracy and still fail the business case.

---

## Practice Questions

**1.** A company needs to convert 50,000 hours of archived call recordings into searchable text. Which service?

A. Amazon Polly · B. Amazon Transcribe · C. Amazon Comprehend · D. Amazon Translate

<details><summary>Answer</summary>

**B — Amazon Transcribe.** Speech → text. Polly is the reverse (text → speech). Comprehend analyzes text you already have; Translate converts between languages.
</details>

**2.** A model achieves 98% accuracy on training data but 61% on test data. What is happening?

A. Underfitting · B. Overfitting · C. Data leakage · D. Class imbalance

<details><summary>Answer</summary>

**B — Overfitting.** High training performance with a large drop on unseen data is the signature. Remedies: more training data, a simpler model, regularization, early stopping.
</details>

**3.** A retailer wants to group customers into segments for marketing, but has no predefined categories. Which approach?

A. Classification · B. Regression · C. Clustering · D. Forecasting

<details><summary>Answer</summary>

**C — Clustering** (unsupervised). No labels exist, and the goal is discovering natural groupings. Classification would require predefined labeled segments.
</details>

**4.** A hospital screening model must avoid missing patients who actually have a disease. Which metric should be prioritized?

A. Precision · B. Recall · C. Accuracy · D. Specificity

<details><summary>Answer</summary>

**B — Recall.** Missing a sick patient is a false negative; recall measures how many true cases were caught. Accuracy is misleading here because the disease is rare.
</details>

**5.** A company processes 20 million records once per week for scoring. There is no latency requirement. Which inference option is most cost-effective?

A. Real-time endpoint · B. Serverless inference · C. Batch transform · D. Asynchronous inference

<details><summary>Answer</summary>

**C — Batch transform.** Large volume, scheduled, no latency need. A real-time endpoint would run 24/7 and waste money.
</details>

**6.** Which scenario is **least** appropriate for a machine learning solution?

A. Predicting next quarter's demand · B. Calculating employee payroll tax owed · C. Detecting defects on a production line · D. Recommending products

<details><summary>Answer</summary>

**B — Payroll tax.** It requires an exact, deterministic, auditable result defined by rules. ML produces probabilistic predictions; use standard code.
</details>

**7.** Which statement correctly describes the relationship between AI, ML, and deep learning?

A. They are three independent fields · B. Deep learning ⊃ ML ⊃ AI · C. AI ⊃ ML ⊃ deep learning · D. ML ⊃ AI ⊃ deep learning

<details><summary>Answer</summary>

**C.** AI is the broadest field; ML is a subset of AI; deep learning is a subset of ML. Generative AI sits inside deep learning.
</details>

**8.** A fintech must explain to regulators exactly why each loan application was denied. Which is most appropriate?

A. A large language model on Bedrock · B. A traditional interpretable ML model · C. An agentic AI workflow · D. A multimodal foundation model

<details><summary>Answer</summary>

**B.** Regulatory explainability favors traditional, interpretable models (e.g., logistic regression, decision trees). Foundation models are difficult to explain decision-by-decision.
</details>

**9.** Which two are examples of unsupervised learning? (Choose two.)

A. Anomaly detection in network traffic · B. Predicting house prices from square footage · C. Customer segmentation · D. Classifying emails as spam · E. Forecasting revenue from labeled history

<details><summary>Answer</summary>

**A and C.** Both find structure in unlabeled data. B, D, and E all require labeled training examples, making them supervised.
</details>

**10.** A team needs a GenAI application using foundation models without provisioning or managing any infrastructure. Which service?

A. Amazon SageMaker AI · B. Amazon EC2 with GPUs · C. Amazon Bedrock · D. Amazon Comprehend

<details><summary>Answer</summary>

**C — Amazon Bedrock.** Fully managed, serverless API access to multiple foundation models. SageMaker AI and EC2 both involve managing infrastructure; Comprehend is traditional NLP, not GenAI.
</details>

**11.** Put the AI/ML pipeline stages in the correct order: *Deployment · Data preprocessing · Business goal identification · Model training · Monitoring · Model evaluation*

<details><summary>Answer</summary>

Business goal identification → Data preprocessing → Model training → Model evaluation → Deployment → Monitoring.
</details>

**12.** After six months in production, a fraud model's accuracy has steadily declined although the code is unchanged. What is the most likely cause and the appropriate response?

A. Overfitting; simplify the model · B. Data drift; retrain on recent data · C. Underfitting; add features · D. Hardware failure; redeploy the endpoint

<details><summary>Answer</summary>

**B — Data drift.** Real-world fraud patterns shift over time, so the live data no longer matches the training distribution. Monitor for drift and retrain on recent data (SageMaker Model Monitor).
</details>

---

## Quick Revision Checklist

- [ ] AI ⊃ ML ⊃ DL ⊃ GenAI; agentic AI = planning + tool use
- [ ] Overfitting vs. underfitting, and the fix for each
- [ ] Four inference types and the cue phrase for each
- [ ] Supervised / unsupervised / reinforcement / self-supervised
- [ ] Regression vs. classification vs. clustering
- [ ] One-line purpose for every managed AI service in the table
- [ ] When ML is the *wrong* tool (deterministic answers, simple rules, bad ROI)
- [ ] Precision vs. recall — decide by which error is costlier
- [ ] Pipeline order and what MLOps adds
- [ ] Bedrock (managed FM API) vs. SageMaker AI (build your own)

**Source:** [AWS Certified AI Practitioner (AIF-C01) Exam Guide — Domain 1](https://docs.aws.amazon.com/aws-certification/latest/ai-practitioner-01/ai-practitioner-01-domain1.html)
