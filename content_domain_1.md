# Content Domain 1: Fundamentals of AI and ML

**AWS Certified AI Practitioner (AIF-C01)** · Exam Guide v1.1 (April 2026)
**Weighting: 20% of scored content** (~10 of the 50 scored questions)

Domain 1 has three task statements:

- **1.1** — Explain basic AI concepts and terminologies
- **1.2** — Identify practical use cases for AI
- **1.3** — Describe the AI/ML development lifecycle

> This file follows the official objectives listed in the exam guide word-for-word, then expands each into study notes with exam cues. Practice questions (all four AWS question types: multiple choice, multiple response, ordering, matching) are at the end.

---

## Task Statement 1.1 — Explain basic AI concepts and terminologies

**Official objectives**

- Define basic AI terms (AI, ML, deep learning, neural networks, computer vision, NLP, model, algorithm, training and inferencing, bias, fairness, fit, LLM, generative AI, agentic AI).
- Describe the similarities and differences between AI, ML, GenAI, deep learning, and agentic AI.
- Describe various types of inferencing (batch, real-time, asynchronous, serverless).
- Describe the different types of data in AI models (labeled and unlabeled, tabular, time-series, image, text, structured and unstructured).
- Describe different types of AI/ML learning (supervised, unsupervised, reinforcement learning methods).

### 1.1.1 The AI hierarchy (nesting dolls)

```
AI  ⊃  ML  ⊃  Deep Learning  ⊃  Generative AI (LLMs / diffusion models)
```

| Term | Definition | One-line example |
|---|---|---|
| **AI** | Broadest field: any technique that makes machines mimic human intelligence, including hand-written rules | Thermostat rule: "if temp > 25 °C, turn on AC" |
| **ML** | Systems that *learn patterns from data* instead of being explicitly programmed | Spam filter trained on 100k labeled emails |
| **Neural network** | Layers of interconnected "neurons" (nodes) with weights, loosely modeled on the brain | The building block of deep learning |
| **Deep learning** | ML using *multi-layer* neural networks that learn features automatically | Recognizing cats in photos without anyone defining "whiskers" |
| **Generative AI** | Deep-learning models that *create* new content (text, image, audio, code) | Claude drafting an email; Stable Diffusion making an image |
| **Agentic AI** | A model that *plans multi-step actions* and **calls tools** to reach a goal | An agent that reads a ticket, queries a DB, and issues a refund |

**The distinction the exam loves:** traditional ML **predicts a value or label**; GenAI **produces new content**; agentic AI **takes actions** (plans + uses tools).

### 1.1.2 Core vocabulary you must be able to define

- **Model** — the trained artifact (the learned weights/parameters). The *output* of training.
- **Algorithm** — the procedure used to produce the model (e.g., XGBoost, linear regression, k-means).
- **Training** — feeding data to the algorithm so it learns parameters. Compute-heavy; done once or periodically.
- **Inference (inferencing)** — using the trained model to make a prediction on new data. Done constantly in production.
- **LLM (large language model)** — a transformer trained on massive text corpora (e.g., Amazon Nova, Anthropic Claude).
- **Computer vision** — AI applied to images/video (object detection, facial analysis, OCR).
- **NLP (natural language processing)** — AI applied to human language (sentiment, entities, translation).
- **Bias** — systematic error that unfairly favors/disfavors a group, or a wrong assumption baked into the model.
- **Fairness** — the model treats different groups equitably.
- **Fit** — how well the model generalizes from training data to unseen data (see overfitting/underfitting below).

#### Fit — the classic trap

| Condition | Training accuracy | Test accuracy | Fixes |
|---|---|---|---|
| **Overfitting** (high variance) | High | Low | More data, simpler model, regularization, early stopping, dropout |
| **Underfitting** (high bias) | Low | Low | More complex model, more/better features, train longer |
| **Good fit / balanced** | High | High | Ship it |

*Memory hook:* Overfitting = the student who memorized the practice test but fails the real one. Underfitting = the student who didn't study at all. **Bias–variance tradeoff:** high bias → underfitting; high variance → overfitting.

### 1.1.3 Types of inferencing

| Type | When to use | AWS mechanism |
|---|---|---|
| **Real-time** | Need an answer in milliseconds, steady/predictable traffic | SageMaker real-time endpoint |
| **Batch** | Large dataset, no urgency, run on a schedule | SageMaker Batch Transform |
| **Asynchronous** | Large payloads (up to ~1 GB) or long processing (minutes); queue requests | SageMaker Asynchronous Inference |
| **Serverless** | Intermittent/spiky traffic with idle periods; tolerate cold starts; pay-per-use | SageMaker Serverless Inference |

*Exam cues:* "unpredictable traffic with idle gaps, minimize cost" → **serverless**. "score 10 million rows nightly" → **batch**. "large video/audio files, minutes each" → **asynchronous**. "sub-second response, constant load" → **real-time**.

### 1.1.4 Types of data

- **Structured** — rows and columns; fits a table/database (CSV, SQL). Also called **tabular**.
- **Unstructured** — text, images, audio, video (~80% of enterprise data).
- **Semi-structured** — JSON, XML (has tags/structure but not a rigid schema).
- **Time-series** — data points ordered by timestamp (stock prices, sensor readings, demand).
- **Image / text** — common unstructured modalities named explicitly in the objectives.
- **Labeled** — each example carries the correct answer (email + "spam").
- **Unlabeled** — raw data with no answer key.

### 1.1.5 Types of AI/ML learning

| Type | Data needed | Goal | Example |
|---|---|---|---|
| **Supervised** | Labeled | Predict a label/value | Predict house price; classify email as spam |
| **Unsupervised** | Unlabeled | Discover hidden structure | Customer segmentation; anomaly detection |
| **Reinforcement (RL)** | Reward signal from an environment | Learn a policy by trial and error | Robot learning to walk; RLHF to align LLMs |
| **Semi-supervised** | Small labeled + large unlabeled | Cut labeling cost | Label 1k of 100k medical images |
| **Self-supervised** | Unlabeled; model creates its own labels | Pre-train foundation models | An LLM learning by predicting the next word |

---

## Task Statement 1.2 — Identify practical use cases for AI

**Official objectives**

- Recognize applications where AI/ML can provide value (assist human decision making, solution scalability, automation).
- Determine when AI/ML solutions are **not** appropriate (cost-benefit analyses; when a specific/exact outcome is needed instead of a prediction).
- Select the appropriate AI/ML techniques for a use case (regression, classification, clustering).
- Identify real-world AI applications (computer vision, NLP, speech recognition, recommendation systems, fraud detection, forecasting, knowledge bases, agentic AI).
- Explain the capabilities of AWS managed AI/ML services (SageMaker AI, Transcribe, Translate, Comprehend, Lex, Polly).
- Identify when **traditional ML** vs. **foundation models (FMs)** are appropriate (regulatory concerns, explainability requirements, operational constraints).

### 1.2.1 Where AI/ML provides value

- **Assist human decision making** — surface insights, rank options, flag risk (a doctor + a diagnostic model).
- **Scalability** — handle volumes no human team could (screen millions of transactions).
- **Automation** — remove repetitive manual effort (auto-tag images, route tickets).
- Good fit when: the pattern is complex/hard to hand-code, you have lots of relevant data, and an approximate/probabilistic answer is acceptable.

### 1.2.2 When AI/ML is NOT appropriate — high-yield

- **You need a deterministic, exact, auditable answer.** Calculating sales tax, payroll, or interest owed → use code, not a model. *ML gives predictions, not guarantees.*
- **Simple rules already solve it.** "Flag any transaction over $10,000" is a business rule, not ML.
- **Cost exceeds benefit.** Data collection, labeling, training, and hosting outweigh the value gained.
- **No quality data**, or a decision needs full explainability the model cannot provide.

### 1.2.3 Choosing the technique

| Question being asked | Technique | Learning type |
|---|---|---|
| "How much / how many?" (continuous number) | **Regression** | Supervised |
| "Which category?" (spam/not; cat/dog/bird) | **Classification** | Supervised |
| "What natural groupings exist?" | **Clustering** | Unsupervised |
| "What is abnormal here?" | **Anomaly detection** | Unsupervised |
| "What will the value be next month?" | **Forecasting** (time-series) | Supervised |
| "What should we show this user next?" | **Recommendation** | Varies |

*Binary vs. multiclass vs. multilabel:* two classes / one of many / several at once (a photo tagged both "beach" and "sunset").

### 1.2.4 Real-world AI application categories

Computer vision · NLP · speech recognition · recommendation systems · fraud detection · forecasting · **knowledge bases** (searchable enterprise info, often for RAG) · **agentic AI** (autonomous multi-step, tool-using workflows).

### 1.2.5 AWS managed AI services (know each by its one-line purpose)

| Service | Does what | Cue phrase |
|---|---|---|
| **Amazon Comprehend** | NLP: sentiment, entities, key phrases, PII, language detection | "analyze reviews for sentiment" |
| **Amazon Transcribe** | Speech → text | "turn call recordings into transcripts" |
| **Amazon Polly** | Text → lifelike speech | "read articles aloud" |
| **Amazon Translate** | Machine translation between languages | "localize content into 20 languages" |
| **Amazon Lex** | Conversational chatbots / IVR (voice + text) | "build a customer-service bot" |
| **Amazon Rekognition** | Image/video analysis: objects, faces, moderation | "detect inappropriate images" |
| **Amazon Textract** | Extract text/tables/forms from documents | "digitize scanned invoices" |
| **Amazon Personalize** | Real-time recommendations | "product recommendations like Amazon.com" |
| **Amazon Kendra** | Intelligent enterprise search (knowledge bases) | "search internal documents in natural language" |
| **Amazon Bedrock** | Managed access to foundation models (GenAI) | "use an LLM without managing infrastructure" |
| **Amazon Q** | GenAI assistant for business and developers | "chat over company data / help me code" |
| **Amazon Nova** | AWS's own family of foundation models (on Bedrock) | "AWS first-party FM" |
| **Amazon SageMaker AI** | Build, train, deploy **your own** models end-to-end | "custom model, full control" |

*Heuristic:* if a question describes a common task (transcribe, translate, detect objects, extract text), pick the **purpose-built managed service** — it needs no ML expertise. Choose **SageMaker AI** only when a **custom model or full control** is required.

### 1.2.6 Traditional ML vs. Foundation Models (added objective)

| Pick **traditional ML** when… | Pick **foundation models (FMs)** when… |
|---|---|
| Task is narrow and numeric (credit scoring, churn) | Task is open-ended language, content generation, or summarization |
| You need **explainability** for regulators | You need broad, general capability with little/no task-specific data |
| Tight latency/cost/compute constraints (operational) | You can accept higher cost and probabilistic output |
| You have good labeled data for the specific task | You have little labeled data → use prompting/RAG instead |

*Exam cue:* "must explain each decision to regulators / strict operational constraints" → **traditional, interpretable ML**. "general-purpose text generation with minimal data" → **FM**.

---

## Task Statement 1.3 — Describe the AI/ML development lifecycle

**Official objectives**

- Describe and differentiate components of an AI/ML pipeline.
- Describe sources of FM models (open-source pre-trained models, training custom models).
- Describe methods to use a model in production (managed API service, self-hosted API).
- Identify relevant AWS services/features for each pipeline stage (Bedrock, Amazon Q, Amazon Quick, Kiro, SageMaker AI).
- Describe fundamental MLOps concepts (experimentation, repeatable processes, scalable systems, managing technical debt, production readiness, model monitoring, model re-training).
- Describe model performance metrics (accuracy, precision, recall, F1) and business metrics (cost per user, development costs, customer feedback, ROI).

### 1.3.1 The AI/ML pipeline (know the order)

1. **Business goal identification** — define the problem and success criteria in business terms.
2. **ML problem framing** — translate it into an ML task ("this is a binary classification problem").
3. **Data collection** — gather from S3, databases, streams.
4. **Data preprocessing / EDA** — clean, handle missing values, deduplicate, label (SageMaker Ground Truth).
5. **Feature engineering** — create and select input variables (SageMaker Feature Store).
6. **Model training & hyperparameter tuning**.
7. **Model evaluation** — test on held-out data against chosen metrics.
8. **Deployment** — expose as an endpoint or batch job.
9. **Monitoring** — watch for **drift** and degraded accuracy, then **retrain** and loop back.

*Data preparation (steps 3–5) is typically the largest share of the effort.*

### 1.3.2 Sources of foundation models

- **Pre-trained / open source** — use as-is (fastest, cheapest). Access via Amazon Bedrock or SageMaker JumpStart.
- **Customized** — increasing cost/effort order: **prompt engineering → RAG → fine-tuning → continued pre-training**.
- **Trained from scratch (custom)** — enormous cost and data requirements; rarely justified.

### 1.3.3 Getting a model into production

- **Managed API service** — Amazon Bedrock, Amazon Q. No infrastructure to manage, pay per token/request, fastest to launch.
- **Self-hosted API** — SageMaker endpoint or EC2. More control/customization, more operational burden.

### 1.3.4 AWS services by pipeline stage (v1.1 names)

| Stage | Representative AWS services |
|---|---|
| Data prep & labeling | SageMaker Data Wrangler, SageMaker Ground Truth, AWS Glue / DataBrew |
| Feature management | SageMaker Feature Store |
| Build / train / tune | Amazon SageMaker AI, SageMaker JumpStart |
| GenAI app building | Amazon Bedrock, Amazon Q, **Kiro** (AI IDE / dev tool), **Strands Agents**, Bedrock AgentCore |
| BI / analytics over results | **Amazon Quick** (business intelligence) |
| Deploy & serve | SageMaker endpoints, Bedrock managed APIs |
| Monitor | SageMaker Model Monitor, SageMaker Clarify, Amazon CloudWatch |

### 1.3.5 MLOps in one paragraph

MLOps applies DevOps discipline to ML: version **code, data, and models**; make training **repeatable** and automated; run **experiments** trackably; build **scalable** systems; deploy through CI/CD; **monitor** production models for drift and degraded accuracy; **retrain** on a schedule or trigger. Its purpose is to control **technical debt** and reach **production readiness**. (Tooling: SageMaker Pipelines, Model Registry, Model Monitor, Clarify.)

### 1.3.6 Model performance metrics

For classification (with TP/FP/TN/FN):

- **Accuracy** = correct / total. *Misleading on imbalanced data* — a model that calls every transaction legitimate can be 99.9% accurate yet useless.
- **Precision** = TP / (TP + FP) → "of the ones I flagged, how many were right?" **Use when false positives are costly** (wrongly blocking a good customer's card).
- **Recall (sensitivity)** = TP / (TP + FN) → "of all the real cases, how many did I catch?" **Use when false negatives are costly** (missing a cancer diagnosis or a fraudulent charge).
- **F1 score** = harmonic mean of precision and recall. Use when you need balance, especially with imbalanced classes.
- **AUC-ROC** — how well the model separates classes across all thresholds; 0.5 = random, 1.0 = perfect.
- **Regression metrics** — MSE / RMSE / MAE (lower is better); R² closer to 1 is better.

*Precision vs. recall trap:* decide by which mistake hurts more. Spam filter → **precision** (don't lose a real email). Disease screening → **recall** (don't miss a sick patient).

### 1.3.7 Business metrics

Cost per user / per inference, development and training cost, time to market, **ROI**, customer feedback and satisfaction, conversion/retention lift. A model can have great accuracy and still fail the business case.

---

## Practice Questions

### Multiple choice (one correct answer)

**1.** A company needs to convert 50,000 hours of archived call recordings into searchable text. Which service?

A. Amazon Polly · B. Amazon Transcribe · C. Amazon Comprehend · D. Amazon Translate

<details><summary>Answer</summary>

**B — Amazon Transcribe.** Speech → text. Polly is the reverse (text → speech); Comprehend analyzes text you already have; Translate converts between languages.
</details>

**2.** A model achieves 98% accuracy on training data but 61% on test data. What is happening?

A. Underfitting · B. Overfitting · C. Data leakage · D. Class imbalance

<details><summary>Answer</summary>

**B — Overfitting.** High training performance with a large drop on unseen data is the signature. Fixes: more data, simpler model, regularization, early stopping.
</details>

**3.** A retailer wants to group customers into segments for marketing but has no predefined categories. Which technique?

A. Classification · B. Regression · C. Clustering · D. Forecasting

<details><summary>Answer</summary>

**C — Clustering** (unsupervised). No labels exist and the goal is discovering natural groupings. Classification would require predefined labeled segments.
</details>

**4.** A hospital screening model must avoid missing patients who actually have a disease. Which metric should be prioritized?

A. Precision · B. Recall · C. Accuracy · D. Specificity

<details><summary>Answer</summary>

**B — Recall.** Missing a sick patient is a false negative; recall measures how many true cases were caught. Accuracy is misleading because the disease is rare.
</details>

**5.** A company scores 20 million records once per week. There is no latency requirement. Which inference option is most cost-effective?

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

A. Three independent fields · B. Deep learning ⊃ ML ⊃ AI · C. AI ⊃ ML ⊃ deep learning · D. ML ⊃ AI ⊃ deep learning

<details><summary>Answer</summary>

**C.** AI is the broadest field; ML is a subset of AI; deep learning is a subset of ML; generative AI sits inside deep learning.
</details>

**8.** A fintech must explain to regulators exactly why each loan application was denied. Which is most appropriate?

A. A large language model on Bedrock · B. A traditional interpretable ML model · C. An agentic AI workflow · D. A multimodal foundation model

<details><summary>Answer</summary>

**B.** Regulatory explainability favors traditional, interpretable models (logistic regression, decision trees). Foundation models are hard to explain decision-by-decision.
</details>

**9.** A team needs a GenAI application using foundation models without provisioning or managing any infrastructure. Which service?

A. Amazon SageMaker AI · B. Amazon EC2 with GPUs · C. Amazon Bedrock · D. Amazon Comprehend

<details><summary>Answer</summary>

**C — Amazon Bedrock.** Fully managed, serverless API access to multiple FMs. SageMaker AI and EC2 involve managing infrastructure; Comprehend is traditional NLP, not GenAI.
</details>

**10.** After six months in production, a fraud model's accuracy steadily declines although the code is unchanged. Most likely cause and response?

A. Overfitting; simplify the model · B. Data drift; retrain on recent data · C. Underfitting; add features · D. Hardware failure; redeploy

<details><summary>Answer</summary>

**B — Data drift.** Real-world fraud patterns shift over time, so live data no longer matches the training distribution. Monitor for drift and retrain (SageMaker Model Monitor).
</details>

**11.** Which describes **agentic AI** rather than a standard generative model?

A. It generates a paragraph of text from a prompt · B. It classifies an image as cat or dog · C. It plans multiple steps and calls external tools to complete a goal · D. It forecasts next month's sales

<details><summary>Answer</summary>

**C.** Agentic AI plans multi-step actions and invokes tools/APIs to act, going beyond generating a single response.
</details>

**12.** Which is the correct definition of **inference** in ML?

A. Feeding labeled data to learn parameters · B. Using a trained model to make predictions on new data · C. Selecting hyperparameters · D. Cleaning and labeling raw data

<details><summary>Answer</summary>

**B.** Inference = using the trained model to predict. Learning parameters from data is **training**.
</details>

### Multiple response (choose all that apply)

**13.** Which two are examples of **unsupervised** learning? (Choose two.)

A. Anomaly detection in network traffic · B. Predicting house prices from square footage · C. Customer segmentation · D. Classifying emails as spam · E. Forecasting revenue from labeled history

<details><summary>Answer</summary>

**A and C.** Both find structure in unlabeled data. B, D, and E require labeled examples → supervised.
</details>

**14.** Which two situations indicate a **purpose-built AWS managed AI service** is a better fit than building a custom SageMaker model? (Choose two.)

A. The team has no ML expertise and needs sentiment analysis of reviews · B. The company needs a novel model architecture for proprietary sensor data · C. The task is standard document text extraction · D. Regulators require a fully custom, from-scratch algorithm · E. The team needs full control over the training loop

<details><summary>Answer</summary>

**A and C.** Comprehend (sentiment) and Textract (document extraction) are common tasks solved by managed services with no ML expertise. B, D, and E call for SageMaker AI / custom models.
</details>

**15.** Which two are **business metrics** (as opposed to model performance metrics)? (Choose two.)

A. F1 score · B. Return on investment (ROI) · C. Recall · D. Cost per user · E. AUC-ROC

<details><summary>Answer</summary>

**B and D.** ROI and cost per user are business metrics. F1, recall, and AUC-ROC are model performance metrics.
</details>

**16.** Which two data types are considered **unstructured**? (Choose two.)

A. A relational table of transactions · B. Customer support call audio · C. Scanned invoice images · D. A CSV of temperatures · E. A JSON configuration file

<details><summary>Answer</summary>

**B and C.** Audio and images are unstructured. A and D are structured/tabular; E is semi-structured.
</details>

### Ordering

**17.** Place these AI/ML pipeline stages in the correct order:
*Deployment · Data preprocessing · Business goal identification · Model training · Monitoring · Model evaluation*

<details><summary>Answer</summary>

1. Business goal identification
2. Data preprocessing
3. Model training
4. Model evaluation
5. Deployment
6. Monitoring
</details>

**18.** Order these FM customization approaches from **lowest to highest** cost/effort:
*Fine-tuning · Prompt engineering · Continued pre-training · Retrieval Augmented Generation (RAG)*

<details><summary>Answer</summary>

1. Prompt engineering
2. RAG
3. Fine-tuning
4. Continued pre-training
</details>

### Matching

**19.** Match each scenario to the correct **inference type** (real-time, batch, asynchronous, serverless):

| # | Scenario |
|---|---|
| a | Sub-second fraud check on every card swipe, constant traffic |
| b | Score 10 million rows overnight on a schedule |
| c | Transcribe/analyze large video files that take minutes each |
| d | Intermittent, spiky requests with long idle periods; minimize cost |

<details><summary>Answer</summary>

a → **Real-time** · b → **Batch** · c → **Asynchronous** · d → **Serverless**
</details>

**20.** Match each task to the correct **AWS managed service**:

| # | Task |
|---|---|
| a | Detect the sentiment and entities in product reviews |
| b | Convert written articles into lifelike spoken audio |
| c | Extract tables and form fields from scanned PDFs |
| d | Build a voice + text customer-service chatbot |

<details><summary>Answer</summary>

a → **Amazon Comprehend** · b → **Amazon Polly** · c → **Amazon Textract** · d → **Amazon Lex**
</details>

**21.** Match each term to its definition:

| # | Term |
|---|---|
| a | Model |
| b | Algorithm |
| c | Training |
| d | Inference |

| Definition |
|---|
| 1. Using a trained artifact to make predictions on new data |
| 2. The trained artifact holding learned parameters |
| 3. The procedure used to produce the model |
| 4. Feeding data so the model learns its parameters |

<details><summary>Answer</summary>

a → 2 · b → 3 · c → 4 · d → 1
</details>

---

## Additional Practice Questions (Exam-Style)

### Multiple choice (one correct answer)

**22.** A media company wants to automatically add captions to thousands of user-uploaded videos. Which combination of services best fits an automated captioning workflow?

A. Amazon Polly then Amazon Translate · B. Amazon Transcribe then Amazon Translate · C. Amazon Comprehend then Amazon Polly · D. Amazon Rekognition then Amazon Lex

<details><summary>Answer</summary>

**B.** Transcribe converts the audio track to text (captions); Translate then localizes those captions into other languages. Polly is text→speech (the wrong direction).
</details>

**23.** A developer wants a chat-based assistant that can answer employee questions using the company's internal documents and help write code. Which AWS service is purpose-built for this?

A. Amazon Lex · B. Amazon Q · C. Amazon Polly · D. Amazon Personalize

<details><summary>Answer</summary>

**B — Amazon Q.** A GenAI assistant for business (chat over company data) and developers (coding help). Lex builds task-oriented bots but isn't a GenAI knowledge assistant.
</details>

**24.** Which learning approach is used to train a robot to walk by rewarding forward progress and penalizing falls?

A. Supervised learning · B. Unsupervised learning · C. Reinforcement learning · D. Self-supervised learning

<details><summary>Answer</summary>

**C — Reinforcement learning.** An agent learns a policy through trial and error guided by a reward signal from the environment.
</details>

**25.** A team has 100,000 product images but only 2,000 are labeled with categories. They want to reduce labeling cost while still training a classifier. Which approach fits best?

A. Fully supervised learning · B. Semi-supervised learning · C. Reinforcement learning · D. Clustering only

<details><summary>Answer</summary>

**B — Semi-supervised learning.** It combines a small labeled set with a large unlabeled set to cut labeling cost while still producing a classifier.
</details>

**26.** Which technique answers the question "how many units will we sell next month?"

A. Classification · B. Clustering · C. Forecasting (regression on time-series) · D. Anomaly detection

<details><summary>Answer</summary>

**C — Forecasting.** Predicting a future continuous value from time-ordered history is time-series forecasting (a supervised, regression-style task).
</details>

**27.** A bank's fraud model flags 100 transactions as fraudulent; only 60 actually were. Of all real fraud in the period (150 cases), it caught 60. What is the model's precision and recall?

A. Precision 60%, Recall 40% · B. Precision 40%, Recall 60% · C. Precision 60%, Recall 60% · D. Precision 40%, Recall 40%

<details><summary>Answer</summary>

**A.** Precision = TP/(TP+FP) = 60/100 = **60%**. Recall = TP/(TP+FN) = 60/150 = **40%**.
</details>

**28.** A model correctly predicts the majority class 95% of the time on a dataset where 95% of samples belong to that class. Why is accuracy a poor metric here?

A. Accuracy cannot be computed on imbalanced data · B. High accuracy can be achieved by always predicting the majority class, hiding poor minority-class performance · C. Accuracy only applies to regression · D. Accuracy requires labeled data

<details><summary>Answer</summary>

**B.** On imbalanced data, a trivial "always majority" model scores high accuracy while failing the class that matters. Use precision/recall/F1 instead.
</details>

**29.** Which scenario is the best fit for **asynchronous** inference on SageMaker?

A. Sub-millisecond responses for an ad-bidding system · B. Processing large documents that each take several minutes, with large payloads · C. A one-time nightly scoring of 50 million rows · D. Occasional requests with long idle gaps where cost must be minimized

<details><summary>Answer</summary>

**B.** Async inference queues requests and suits large payloads and long (minutes-scale) processing. A is real-time, C is batch, D is serverless.
</details>

**30.** A company wants recommendations like "customers who bought this also bought" updated in real time. Which managed service is purpose-built for this?

A. Amazon Forecast · B. Amazon Personalize · C. Amazon Comprehend · D. Amazon Kendra

<details><summary>Answer</summary>

**B — Amazon Personalize.** Real-time personalized recommendations. Kendra is enterprise search; Comprehend is NLP; Forecast is time-series forecasting.
</details>

**31.** Which statement about **training** vs. **inference** cost/frequency is generally correct?

A. Training is cheap and continuous; inference is expensive and rare · B. Training is compute-intensive and periodic; inference is comparatively lighter and frequent · C. Both are equally expensive and run continuously · D. Inference must be re-run every time to update model weights

<details><summary>Answer</summary>

**B.** Training learns parameters (heavy, periodic). Inference applies the trained model to new inputs (lighter, runs constantly). Inference does not change weights.
</details>

**32.** A healthcare startup needs a general-purpose model to summarize clinical notes but has very little labeled training data. Which approach is most appropriate?

A. Train a model from scratch · B. Use a foundation model with prompting/RAG · C. Build a rule-based system · D. Use k-means clustering

<details><summary>Answer</summary>

**B.** With little labeled data and an open-ended language task (summarization), a foundation model accessed via prompting or RAG is the practical choice.
</details>

**33.** Which AWS service would you use to build, train, and deploy a **custom** ML model with full control over the training process?

A. Amazon Rekognition · B. Amazon Comprehend · C. Amazon SageMaker AI · D. Amazon Textract

<details><summary>Answer</summary>

**C — Amazon SageMaker AI.** It's the end-to-end platform for custom models. The others are pre-built managed AI services for specific tasks.
</details>

**34.** A retailer must produce an exact, auditable calculation of each customer's loyalty-points balance. What should they use?

A. A regression model · B. A classification model · C. Deterministic business logic (code), not ML · D. A foundation model

<details><summary>Answer</summary>

**C.** Exact, rule-defined, auditable results call for deterministic code. ML produces probabilistic predictions, not guaranteed exact answers.
</details>

**35.** In the ML lifecycle, which stage typically consumes the **largest** share of effort?

A. Model deployment · B. Data collection and preprocessing · C. Business goal identification · D. Monitoring

<details><summary>Answer</summary>

**B.** Data collection, cleaning, and preparation usually dominate the effort in real ML projects.
</details>

**36.** What is the primary purpose of **MLOps**?

A. To eliminate the need for data preprocessing · B. To make ML workflows repeatable, scalable, and production-ready while managing technical debt · C. To replace data scientists with automation · D. To guarantee 100% model accuracy

<details><summary>Answer</summary>

**B.** MLOps applies DevOps discipline (versioning, CI/CD, monitoring, retraining) to control technical debt and reach production readiness.
</details>

**37.** A GenAI application is built by accessing foundation models through a fully managed API with pay-per-token pricing and no servers to manage. This is an example of which deployment method?

A. Self-hosted API on EC2 · B. Managed API service · C. On-premises inference · D. Edge deployment

<details><summary>Answer</summary>

**B — Managed API service** (e.g., Amazon Bedrock). No infrastructure to manage; pay per request/token.
</details>

**38.** Which is the best example of using AI/ML to **assist human decision making** rather than fully automate it?

A. Auto-rejecting all loan applications below a score · B. A radiologist reviewing an AI-flagged region on a scan before diagnosing · C. A scheduled batch job that deletes old files · D. A fixed rule that blocks logins after 3 failures

<details><summary>Answer</summary>

**B.** The model surfaces insight (a flagged region) and a human makes the final call — decision support, not full automation. C and D are deterministic rules, not ML.
</details>

**39.** A company uses a model whose live accuracy drops after a competitor changes the market, shifting the input data distribution. What is this phenomenon called?

A. Overfitting · B. Data (concept) drift · C. Underfitting · D. Data leakage

<details><summary>Answer</summary>

**B — Data/concept drift.** The production data no longer matches the training distribution. Response: monitor and retrain on recent data.
</details>

**40.** Which of the following is an example of **structured (tabular)** data?

A. A folder of scanned contracts · B. A table of customers with columns for age, income, and region · C. A collection of podcast audio files · D. A set of product photos

<details><summary>Answer</summary>

**B.** Rows and columns with defined fields = structured/tabular. The others are unstructured.
</details>

**41.** Which AWS service extracts printed and handwritten text, tables, and form fields from scanned documents?

A. Amazon Comprehend · B. Amazon Textract · C. Amazon Translate · D. Amazon Kendra

<details><summary>Answer</summary>

**B — Amazon Textract.** OCR plus structure (tables/forms). Comprehend analyzes text meaning; Kendra searches; Translate localizes.
</details>

**42.** A team needs natural-language search across thousands of internal wiki pages and PDFs so employees can ask questions and get relevant passages. Which service fits best?

A. Amazon Kendra · B. Amazon Polly · C. Amazon Rekognition · D. Amazon Forecast

<details><summary>Answer</summary>

**A — Amazon Kendra.** Intelligent enterprise search over documents (a knowledge-base use case).
</details>

### Multiple response (choose all that apply)

**43.** Which two are characteristics of **overfitting**? (Choose two.)

A. High training accuracy · B. Low training accuracy · C. Poor generalization to unseen data · D. High bias · E. It is fixed by adding more model complexity

<details><summary>Answer</summary>

**A and C.** Overfitting = high training accuracy but poor performance on new data (high variance). Adding complexity makes it worse; the fixes are more data, regularization, simpler models, early stopping.
</details>

**44.** Which two are valid reasons to choose a **foundation model** over a traditional ML model? (Choose two.)

A. You need decision-by-decision explainability for regulators · B. The task is open-ended text generation · C. You have little task-specific labeled data · D. You need the lowest possible latency and compute cost · E. The output must be exact and deterministic

<details><summary>Answer</summary>

**B and C.** FMs excel at open-ended generation and work with little labeled data via prompting/RAG. A, D, and E favor traditional models or plain code.
</details>

**45.** Which two tasks are best solved with **supervised learning**? (Choose two.)

A. Predicting tomorrow's temperature from historical labeled weather data · B. Grouping unlabeled news articles by topic · C. Classifying emails as spam or not spam · D. Detecting previously unseen anomalies with no labels · E. Segmenting customers with no predefined groups

<details><summary>Answer</summary>

**A and C.** Both use labeled data to predict a value/label. B, D, and E are unsupervised.
</details>

**46.** Which two AWS services are primarily for **natural language processing / language tasks**? (Choose two.)

A. Amazon Comprehend · B. Amazon Rekognition · C. Amazon Translate · D. Amazon Personalize · E. Amazon Forecast

<details><summary>Answer</summary>

**A and C.** Comprehend (sentiment/entities/PII) and Translate (language translation) are NLP/language services. Rekognition is vision; Personalize is recommendations; Forecast is time-series.
</details>

**47.** Which two are **model performance** metrics for classification? (Choose two.)

A. F1 score · B. Cost per inference · C. Recall · D. Return on investment · E. Time to market

<details><summary>Answer</summary>

**A and C.** F1 and recall measure model performance. The others are business metrics.
</details>

**48.** Which two conditions suggest AI/ML is a **good fit** for a problem? (Choose two.)

A. The pattern is complex and hard to express as fixed rules · B. A single deterministic correct answer is legally required · C. Large volumes of relevant historical data exist · D. A simple threshold rule already solves it · E. No labeled or usable data is available

<details><summary>Answer</summary>

**A and C.** Complex patterns plus ample relevant data favor ML. B, D, and E argue against it.
</details>

### Ordering

**49.** Order these steps of a typical supervised ML workflow from first to last:
*Model evaluation · Data labeling · Feature engineering · Model training · Data collection*

<details><summary>Answer</summary>

1. Data collection
2. Data labeling
3. Feature engineering
4. Model training
5. Model evaluation
</details>

**50.** A model is being customized. Order these from **least to most** data and compute required:
*Fine-tuning · Training from scratch · Prompt engineering · Continued pre-training*

<details><summary>Answer</summary>

1. Prompt engineering
2. Fine-tuning
3. Continued pre-training
4. Training from scratch
</details>

### Matching

**51.** Match each learning type to its example:

| # | Learning type |
|---|---|
| a | Supervised |
| b | Unsupervised |
| c | Reinforcement |
| d | Self-supervised |

| Example |
|---|
| 1. An LLM pre-trained by predicting the next word in text |
| 2. Predicting house price from labeled sales records |
| 3. Grouping customers into segments with no labels |
| 4. An agent learning to play a game via rewards |

<details><summary>Answer</summary>

a → 2 · b → 3 · c → 4 · d → 1
</details>

**52.** Match each metric to what it best measures:

| # | Metric |
|---|---|
| a | Precision |
| b | Recall |
| c | Accuracy |
| d | F1 score |

| Meaning |
|---|
| 1. Overall fraction of correct predictions (misleading on imbalanced data) |
| 2. Of all actual positives, how many were caught |
| 3. Harmonic mean balancing precision and recall |
| 4. Of all predicted positives, how many were correct |

<details><summary>Answer</summary>

a → 4 · b → 2 · c → 1 · d → 3
</details>

**53.** Match each business need to the correct **inference type**:

| # | Need |
|---|---|
| a | Steady traffic needing millisecond responses |
| b | Nightly scoring of a huge dataset, no rush |
| c | Spiky, intermittent traffic; pay only when used |
| d | Large payloads needing minutes of processing |

<details><summary>Answer</summary>

a → **Real-time** · b → **Batch** · c → **Serverless** · d → **Asynchronous**
</details>

**54.** Match each AWS service to its primary purpose:

| # | Service |
|---|---|
| a | Amazon Bedrock |
| b | Amazon SageMaker AI |
| c | Amazon Rekognition |
| d | Amazon Transcribe |

| Purpose |
|---|
| 1. Analyze images/video for objects, faces, moderation |
| 2. Managed access to foundation models (GenAI) |
| 3. Convert speech to text |
| 4. Build, train, and deploy custom ML models |

<details><summary>Answer</summary>

a → 2 · b → 4 · c → 1 · d → 3
</details>

---

## Quick Revision Checklist

- [ ] AI ⊃ ML ⊃ deep learning ⊃ GenAI; **agentic AI = planning + tool use**
- [ ] Define every term in objective 1.1.1 (model vs. algorithm; training vs. inference; bias vs. fairness; fit)
- [ ] Overfitting vs. underfitting — signature and fixes; bias–variance tradeoff
- [ ] Four inference types (real-time, batch, asynchronous, serverless) and the cue phrase for each
- [ ] Data types: structured/tabular, unstructured (image/text/audio), semi-structured, time-series, labeled/unlabeled
- [ ] Learning types: supervised, unsupervised, reinforcement (plus semi- and self-supervised)
- [ ] Regression vs. classification vs. clustering vs. forecasting vs. anomaly detection
- [ ] When ML is the **wrong** tool (deterministic answers, simple rules, poor ROI, no data)
- [ ] One-line purpose for every managed AI service; managed service vs. SageMaker AI heuristic
- [ ] Traditional ML vs. FMs — decide by explainability, regulation, operational constraints, data availability
- [ ] Pipeline order; sources of FMs; managed API vs. self-hosted
- [ ] MLOps essentials (experimentation, repeatability, scalability, technical debt, monitoring, retraining)
- [ ] Precision vs. recall — decide by which error is costlier; accuracy trap on imbalanced data
- [ ] Model metrics vs. business metrics (ROI, cost per user)

---

**Source:** *AWS Certified AI Practitioner (AIF-C01) Exam Guide, v1.1 (April 2026)* — [Official PDF](https://docs.aws.amazon.com/pdfs/aws-certification/latest/ai-practitioner-01/ai-practitioner-01.pdf) · Content Domain 1: Fundamentals of AI and ML (20% of scored content).
