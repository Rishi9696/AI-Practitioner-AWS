# Domain 1: Fundamentals of AI and ML
**AIF-C01 · 20% of scored content · ~10 of 50 scored questions**

Three task statements: basic concepts (1.1), practical use cases (1.2), the AI/ML lifecycle (1.3).

---

## 1.1 Basic AI Concepts and Terminology

### The nesting doll

Every layer sits inside the one before it. This is a guaranteed exam question.

```
AI  ⊃  ML  ⊃  Deep Learning  ⊃  Generative AI (LLMs)
```

**Artificial Intelligence (AI)**
A field of computer science dedicated to solving problems that normally require human intelligence — reasoning, perceiving, understanding language, making decisions. Crucially, **AI does not require learning**. A hand-written rule engine is AI. The classic example is **MYCIN**, a 1970s rule-based expert system that diagnosed bacterial infections using if-then rules written by doctors — pure AI, zero machine learning. This distinction matters because the exam asks when you *don't* need ML.

**Machine Learning (ML)**
A subset of AI where the system **learns patterns from data instead of being explicitly programmed with rules**. You don't write "spam emails contain the word 'lottery'." You show the algorithm 100,000 emails already labeled spam or not-spam and it derives the patterns itself — including ones no human would think to write down. If a person had to hand-write the logic, it isn't ML.

**Deep Learning (DL)**
A subset of ML that uses **neural networks with many layers** ("deep" = many layers). Its defining advantage is **automatic feature extraction**. With traditional ML, a human decides which input variables matter (for house prices: square footage, bedrooms, ZIP code). With deep learning on images, nobody defines "whiskers" or "pointy ears" — the network discovers those features across its layers. The tradeoff is that DL needs far more data and compute.

**Neural Network**
The structure behind deep learning: layers of interconnected nodes ("neurons") loosely inspired by the brain. Each connection carries a **weight**. Data enters the input layer, flows through hidden layers where each neuron combines its inputs and applies a transformation, and emerges at the output layer. Training adjusts the weights until outputs match the correct answers. The textbook example is recognizing hand-written digits.

**Generative AI (GenAI)**
A subset of deep learning focused on **creating new content** rather than predicting a label or number — text, images, audio, video, code. Traditional ML looks at an email and outputs "spam." Generative AI looks at the email and *writes the reply*. It's built on **foundation models**: models pre-trained on huge amounts of **unlabeled** data that can then be adapted to a broad range of tasks (summarization, Q&A, chat, image generation).

**Agentic AI**
An AI system that doesn't just answer — it **plans a sequence of steps, uses tools to act, observes the results, and iterates until the goal is met**. Regular GenAI explains how to process a refund. An agent looks up the order, checks the policy, issues the refund, and emails the customer. Three ingredients: planning, tool use, iteration.

**The one-line distinction to memorize:** traditional ML **predicts**. Generative AI **creates**. Agentic AI **acts**.

| | Traditional ML | Generative AI | Agentic AI |
|---|---|---|---|
| Output | A label or number | New content | Completed actions |
| Example | "This transaction is fraudulent" | Writes the fraud-alert email | Freezes the card and notifies the customer |
| Needs | Labeled task data | A pre-trained FM + prompt | An FM + tools + orchestration |

---

### Core vocabulary — full definitions

**Model**
The **output of training** — the saved artifact containing everything the system learned, stored as numeric **parameters (weights)**. It's the "brain" produced at the end of training: a file you can save, version, copy, and load. When you deploy something to make predictions, you deploy the model. Sizes range from kilobytes (linear regression) to hundreds of gigabytes (a large LLM).

**Algorithm**
The **procedure used to produce a model** from data. The algorithm is the method; the model is the result. Examples: linear regression, decision trees, k-nearest neighbors, XGBoost, k-means, transformers. The same algorithm run on different data produces a different model.
*Analogy:* algorithm = recipe, training data = ingredients, model = the cake you actually eat.

**Parameters vs. hyperparameters**
**Parameters** are learned *by* the model during training (the weights). **Hyperparameters** are set *by a human before* training and control how training happens:

- **Learning rate** — how big a step the model takes when updating weights. Too high overshoots the best solution; too low is precise but slow.
- **Batch size** — how many examples are used per weight update. Small = more stable but slower; large = faster but less stable.
- **Epochs** — how many complete passes over the training data. Too few → underfitting; too many → overfitting.
- **Regularization** — a penalty that discourages complexity. Increase it to reduce overfitting.

*Tuning* hyperparameters is out of scope for this exam, but recognizing what they are is not. **SageMaker Automatic Model Tuning (AMT)** does it for you via grid or random search.

**Training**
Feeding data to an algorithm so it learns the parameters that best map inputs to correct outputs. The model predicts, compares to the known answer, measures the error, and adjusts weights to reduce it — repeated many times. Training is **computationally expensive, slow, and done occasionally**.

Training data is split three ways:

| Split | Typical share | Purpose |
|---|---|---|
| **Training set** | 60–80% | The model actually learns from this |
| **Validation set** | 10–20% | Tune hyperparameters and check progress during development |
| **Test set** | 10–20% | Final, unbiased evaluation on data never seen before |

*"Garbage in, garbage out"* — data quality is the single biggest determinant of model quality.

**Inference**
**Using the already-trained model to make a prediction on new, unseen data.** No learning happens; the weights are frozen. Inference is fast and cheap per request, but it runs constantly in production, so at scale it usually dominates total cost.
*In short: training is when the model learns; inference is when the model works.*

**Feature**
An individual input variable the model learns from — a column in your data. **Feature engineering** is using domain knowledge to select and transform raw data into meaningful features, and it's often the single biggest lever on model quality. Three techniques:

- **Feature extraction** — derive new information from raw data (compute *age* from *date of birth*).
- **Feature selection** — keep only the relevant predictors and drop the rest.
- **Feature transformation** — reshape values for better learning (normalizing or scaling numbers).

Feature engineering matters most for **supervised learning on structured data**; deep learning largely does it automatically.

**Large Language Model (LLM)**
A very large neural network — almost always a **transformer** — trained on an enormous corpus of text (books, articles, websites) to predict the next token. From that single objective it develops broad language ability: writing, summarizing, translating, answering questions, generating code. "Large" refers to the parameter count (billions+) and the training data size.

**Computer Vision (CV)**
AI that enables machines to **interpret visual information** — images and video. Tasks: image classification, object detection, facial recognition, OCR, content moderation. *AWS:* Amazon Rekognition, Amazon Textract.

**Natural Language Processing (NLP)**
AI that enables machines to **understand, interpret, and generate human language**. Tasks: sentiment analysis, entity extraction, translation, summarization, intent detection. *AWS:* Amazon Comprehend, Translate, Lex, Transcribe, Polly.

**Bias**
A **systematic, repeatable error** — the difference between predicted and actual values caused by a wrong choice somewhere in the ML process. Two related meanings:

1. **Statistical bias** — the model is too simple to match the data (fitting a straight line to a curved relationship). High bias = **underfitting**. Reduce it with a more complex model or more features.
2. **Societal/data bias** — the model produces **unfair outcomes for particular groups**, usually because training data was unrepresentative or reflected historical discrimination.

Bias is *systematic*, not random — it happens the same way every time, which is why it doesn't average out.

**Variance**
How much a model's performance **changes if trained on a different dataset** with a similar distribution. High variance means the model is sensitive to the specific training data — chasing noise rather than signal. High variance = **overfitting**. Reduce it with feature selection (fewer, more important features) and repeated train/test splitting (cross-validation).

**Fairness**
The principle that outcomes should be **equitable across groups**, without systematically advantaging or disadvantaging people by gender, race, age, or geography. **Overall accuracy hides unfairness**: a model can be 95% accurate overall and 60% accurate for one group. Evaluate **per subgroup**. *AWS tool:* SageMaker Clarify.

---

### Model architectures and acronyms you may see

These appear as distractors and occasionally as answers.

| Acronym | What it is | Used for |
|---|---|---|
| **GPT** (Generative Pre-trained Transformer) | Transformer LLM that generates text/code from prompts | Text and code generation |
| **BERT** (Bidirectional Encoder Representations from Transformers) | Transformer that reads text in **both directions** | Understanding tasks, embeddings, search |
| **RNN** (Recurrent Neural Network) | Processes **sequential** data one step at a time | Time-series prediction, speech recognition |
| **CNN / ResNet** (Convolutional / Residual Network) | Deep networks specialized for images | Image recognition, object detection |
| **GAN** (Generative Adversarial Network) | Two networks compete to produce realistic synthetic data | Synthetic images/video, **data augmentation** |
| **SVM** (Support Vector Machine) | Classic ML algorithm | Classification and regression |
| **WaveNet** | Generates raw audio waveforms | Speech synthesis |
| **XGBoost** (Extreme Gradient Boosting) | Gradient-boosted decision trees | Top performer on **tabular/structured** data |
| **Diffusion model** | Adds noise to images in training, then reverses the process to generate | **Text-to-image generation** (Stable Diffusion) |

---

### Fit: overfitting and underfitting

**Overfitting** — the model learned the training data **too well, including its noise**. It memorized rather than generalized: excellent on seen data, poor on new data. The more common failure.
*Causes:* training set too small or unrepresentative, training too long on one dataset, model too complex.
*Fixes:* increase training data size · **early stopping** · **data augmentation** (generate more varied examples) · adjust hyperparameters / increase regularization · **ensembling** (combine multiple models) · feature selection.

**Underfitting** — the model is **too simple to capture the real pattern**, so it performs poorly on both training and new data.
*Fixes:* use a more complex model, add or improve features, train longer.

**Balanced** — performs well on both training and unseen data. The goal.

| Condition | Training | Test | Root cause | Fix |
|---|---|---|---|---|
| **Overfitting** | High | Low | High **variance** | More data, simpler model, regularization, early stopping, augmentation, ensembling |
| **Underfitting** | Low | Low | High **bias** | More complex model, more/better features, train longer |
| **Balanced** | High | High | Low bias, low variance | Ship it |

*Memory hook:* overfitting is the student who memorized last year's paper and freezes when the questions change. Underfitting is the student who never opened the book and fails both papers equally.

**The bias–variance quadrant:** low bias + low variance = balanced. High bias + low variance = underfitting. Low bias + high variance = overfitting. Reducing one tends to raise the other.

---

### Types of inference

The **same trained model** can be served four ways. The question is always about **latency needs, payload size, and traffic pattern**.

| Type | Latency | Max payload | Max processing | Best for |
|---|---|---|---|---|
| **Real-time** | Low (ms–sec) | ~25 MB per record | ~60 sec | Fast, near-instant predictions for web/mobile apps; chatbots |
| **Serverless** | Low (ms–sec), plus cold starts | ~4 MB per record | ~60 sec | Sporadic/intermittent traffic; no infrastructure to manage; scales to zero |
| **Asynchronous** | Medium–high ("near real-time") | Up to **1 GB** | Up to **1 hour** | Large payloads and long processing — video, big documents |
| **Batch transform** | High (minutes–hours) | ~100 MB per mini-batch | Up to 1 hour | Bulk scoring of large datasets on a schedule |

**Real-time** keeps a persistent endpoint running 24/7 — you pay whether or not requests arrive. Speed is prioritized over perfect accuracy.
**Batch** analyzes a large dataset all at once; speed doesn't matter, accuracy does.
**Serverless** trades cold-start latency for paying nothing while idle.
**Asynchronous** queues the request and returns the result when ready.

| Cue phrase in the question | Answer |
|---|---|
| "millisecond response, consistent traffic" | Real-time |
| "score millions of records nightly, no rush" | Batch |
| "1 GB files, several minutes to process" | Asynchronous |
| "unpredictable traffic with long idle periods, minimize cost" | Serverless |

**Inference at the edge**
Running the model on a device close to where data is generated (a Raspberry Pi, a camera, a car) instead of in the cloud.

- **Small Language Model (SLM) on the edge device** — very low latency, small compute footprint, **works offline**. Less capable.
- **Large Language Model (LLM) on a remote server** — more powerful, higher latency, **requires connectivity**.

*Exam cue:* "limited or no internet connectivity" or "lowest possible latency on-device" → edge inference with a small model.

---

### Types of data

**Structured data** — organized in a predefined schema of rows and columns, like a spreadsheet or database table. Immediately queryable with SQL.
- **Tabular** — rows are records, columns are features (a customer table with name, age, purchase amount).
- **Time-series** — data points recorded at successive times, where **the order carries meaning** and cannot be shuffled (daily stock prices, IoT sensor readings). Used for forecasting.

**Unstructured data** — no predefined structure; typically text-heavy or multimedia. *Examples:* product reviews, articles, social posts, images, audio, video. Roughly 80% of enterprise data, and what deep learning and foundation models excel at.

**Semi-structured data** — not a rigid table, but carries tags giving partial organization. *Examples:* JSON, XML, logs.

**Labeled data** — includes both **input features and the corresponding output label**: an image *plus* the tag "cat"; an email *plus* "spam." Required for **supervised learning**. Producing labels is usually the slowest, most expensive part of a project. *AWS tool:* SageMaker Ground Truth.

**Unlabeled data** — input features only, no answers attached. Abundant and cheap. Used by **unsupervised** and **self-supervised** learning.

---

### Types of machine learning

**Supervised learning**
Learns a mapping function from **labeled data** so it can predict outputs for new inputs. It's "supervised" because the correct answers act as a teacher. Very powerful, but hard to scale to millions of data points because everything must be labeled.

- **Regression** — predicts a **continuous numeric value** that can fall anywhere in a range. *Examples:* house prices from size and location; stock price prediction; weather forecasting.
- **Classification** — predicts a **discrete category**. *Binary:* spam / not spam. *Multiclass:* mammal / bird / reptile. *Multi-label:* a movie tagged both "action" and "comedy." *Common algorithm:* k-nearest neighbors (k-NN).

**Unsupervised learning**
Discovers inherent patterns, structures, or relationships in **unlabeled data**. The machine creates the groups; **humans still interpret and label the resulting groups**. Three techniques:

| Technique | What it does | Example | Typical algorithm |
|---|---|---|---|
| **Clustering** | Groups similar data points | Customer segmentation for targeted marketing | **K-means** |
| **Association rule learning** | Finds items that occur together | Market basket analysis — bread and butter bought together | **Apriori** |
| **Anomaly detection** | Flags points that deviate from normal | Fraudulent credit card transactions | **Isolation Forest** |

**Reinforcement learning (RL)**
An **agent** learns to make decisions by taking **actions** in an **environment** to maximize cumulative **reward**. No labeled dataset — it learns by trial and error across many simulations. Six terms to know:

| Term | Meaning |
|---|---|
| **Agent** | The learner / decision-maker |
| **Environment** | The external system it interacts with |
| **Action** | A choice the agent makes |
| **Reward** | Feedback (positive or negative) from the environment |
| **State** | The current situation of the environment |
| **Policy** | The strategy mapping states to actions |

*Applications:* robotics, game playing, autonomous vehicles, resource optimization.

**RLHF (Reinforcement Learning from Human Feedback)**
RL where **human judgment is built into the reward function**, aligning the model with human goals and preferences. Humans compare and rate model responses; those ratings train a reward model; the model is then optimized against it. This is how LLMs are made helpful and harmless, and how translations move from "technically correct" to "sounds human." *AWS tool:* SageMaker Ground Truth.

**Semi-supervised learning**
Uses a **small labeled set plus a large unlabeled set**. The partially trained model labels the unlabeled data itself — called **pseudo-labeling** — and is then retrained on the combined mix. Practical when labeling everything is too expensive.

**Self-supervised learning**
The model **generates its own pseudo-labels from raw data**, with no human labeling at all. Hide the next word in a sentence and make the model predict it — the text itself supplies the answer. This is how BERT and GPT-style foundation models are pre-trained, and it's why internet-scale training is possible.

| Type | Data needed | Goal | Example |
|---|---|---|---|
| Supervised | Labeled | Predict a known target | Spam detection, price prediction |
| Unsupervised | Unlabeled | Discover hidden structure | Segmentation, anomaly detection |
| Reinforcement | Reward signal | Learn an optimal policy | Robotics, game AI, RLHF |
| Semi-supervised | Small labeled + large unlabeled | Cut labeling cost | Pseudo-labeling medical images |
| Self-supervised | Unlabeled (self-generated labels) | Pre-train foundation models | LLM next-token prediction |

---

## 1.2 Practical Use Cases for AI

### When AI/ML is a good fit

- **The pattern is complex and hard to express as rules** — nobody can write reliable rules to identify a cat in a photo.
- **You have enough relevant historical data.**
- **The task needs scale or speed humans can't match** — screening millions of transactions daily.
- **An approximate, probabilistic answer is acceptable.**
- **The pattern shifts over time**, so hand-written rules would need constant rewriting.

The three value patterns the exam names: **assisting human decision-making**, **scalability**, and **automation**.

### When AI/ML is NOT appropriate — high-yield

- **The problem is deterministic and can simply be computed.** *"A deck has five red, three blue, and two yellow cards — what's the probability of drawing blue?"* The answer is 3/10, exactly, every time. Writing code is better than training a model, which would only ever produce an *approximation*. (Even LLMs with reasoning ability are a worse solution here.)
- **Simple rules already work.** "Flag any transaction over $10,000" is one line of code.
- **Cost outweighs benefit.** Data collection, labeling, training, hosting, and monitoring all cost money.
- **You lack sufficient quality data.**
- **Full explainability is legally required** and only black-box models are accurate enough.
- **The identical output is required every time** — ML output varies.

*Exam tip:* if a scenario has one provably correct answer defined by fixed rules, the answer is "ML is not appropriate."

### Choosing the right technique

| The question being asked | Technique | Learning type |
|---|---|---|
| "How much / how many?" (continuous number) | **Regression** | Supervised |
| "Which category is this?" | **Classification** | Supervised |
| "What natural groups exist?" | **Clustering** | Unsupervised |
| "What's unusual here?" | **Anomaly detection** | Unsupervised |
| "What products are bought together?" | **Association rule learning** | Unsupervised |
| "What will this value be in future?" | **Forecasting** (time-series) | Supervised |
| "What should this user see next?" | **Recommendation** | Varies |

### Real-world AI applications

Computer vision (quality inspection, medical imaging, autonomous vehicles) · NLP and document processing · speech recognition and generation · recommendation systems · fraud detection · demand and financial forecasting · predictive maintenance · knowledge bases and intelligent search · agentic AI for automated workflows.

**Intelligent Document Processing** is the classic multi-technique example: a scanned PDF invoice comes in, **computer vision** reads the image, **deep learning + NLP** extract the seller, buyer, items, and prices, and structured records are inserted into a database.

### AWS managed AI services

These are **pre-trained services you call through an API** — no ML expertise, no training data, no infrastructure. AWS highlights their responsiveness, availability, multi-AZ/multi-region redundancy, specialized hardware, and pay-for-what-you-use pricing.

| Service | What it does | Cue phrase |
|---|---|---|
| **Amazon Comprehend** | NLP: sentiment, entities, key phrases, language, PII; custom classification and custom entity recognition | "analyze customer reviews for sentiment" |
| **Amazon Transcribe** | Speech → text; speaker identification; toxicity detection in audio | "turn call recordings into transcripts" |
| **Amazon Polly** | Text → lifelike speech | "read articles aloud" |
| **Amazon Translate** | Neural machine translation | "localize the site into 20 languages" |
| **Amazon Lex** | Conversational bots for voice and text (the tech behind Alexa) | "build a customer service chatbot" |
| **Amazon Rekognition** | Image/video analysis: objects, faces, text, custom labels, content moderation | "detect inappropriate images" |
| **Amazon Textract** | Extract text, forms, and tables from scanned documents | "digitize paper invoices" |
| **Amazon Personalize** | Real-time personalized recommendations (uses "recipes") | "product recommendations like Amazon.com" |
| **Amazon Forecast** | Time-series forecasting | "predict inventory demand" |
| **Amazon Fraud Detector** | Detect online fraud and fake accounts | "flag fraudulent signups" |
| **Amazon Kendra** | Intelligent enterprise search over internal documents | "search internal docs in plain English" |
| **Amazon Bedrock** | Managed API access to foundation models (GenAI) | "use an LLM without managing infrastructure" |
| **Amazon Q** | Ready-made GenAI assistant (Q Business, Q Developer) | "AI assistant over our company data" |
| **Amazon SageMaker AI** | Build, train, tune, deploy, monitor **your own** models | "custom model with full control" |

**Specialized/healthcare variants:** Amazon Transcribe Medical (medical speech-to-text), Amazon Comprehend Medical (extract medical information from text), AWS HealthScribe (clinical documentation from patient conversations).

**Human-in-the-loop services:** Amazon Mechanical Turk (crowdsourced human tasks), Amazon Augmented AI / A2I (human review of low-confidence ML predictions), SageMaker Ground Truth (data labeling and RLHF).

*Decision rule:* a **standard task** (transcribe, translate, detect objects, extract text) → the **purpose-built managed service**. A **custom model** or control over training → **SageMaker AI**.

### Traditional ML vs. foundation models

| Choose **traditional ML** when | Choose a **foundation model** when |
|---|---|
| The task is narrow and numeric (credit scoring, churn, demand) | The task involves open-ended language, generation, or summarization |
| **Explainability is legally required** (lending, insurance, hiring) | Broad general capability matters more than traceability |
| Latency, cost, or compute is tightly constrained | You can absorb higher cost and latency |
| You already have good labeled data for the task | You have little or no labeled data — prompt instead of train |
| Output must be consistent and reproducible | Some variability is acceptable |

---

## 1.3 The AI/ML Development Lifecycle

### Phases of an ML project

1. **Business problem / goals** — stakeholders define the value, budget, and success criteria. **Defining KPIs is critical.**
2. **ML problem framing** — convert the business problem into an ML problem and **decide whether ML is even appropriate**. Data scientists, data engineers, ML architects, and subject-matter experts collaborate.
3. **Data collection and integration** — gather data and make it centrally accessible.
4. **Data preprocessing and EDA** — convert to a usable format, clean it, visualize it. **Exploratory Data Analysis** uses graphs and a **correlation matrix** to see how strongly variables relate, which guides feature choice.
5. **Feature engineering** — create, transform, and extract the variables the model learns from.
6. **Model training and hyperparameter tuning** — iterative; often loops back to more feature engineering.
7. **Model evaluation** — measure on the **held-out test set** against the KPIs from step 1. Ask: *are business goals met?* If not, add data, augment features, and retrain.
8. **Deployment** — choose a deployment mode: real-time, serverless, asynchronous, batch, or on-premises/edge.
9. **Monitoring and debugging** — detect degradation early, understand model behavior, and iterate as new data arrives and requirements change.

*Data collection and preparation typically consume the majority of project effort — and data quality is the most critical factor in the outcome.*

### Sources of foundation models

- **Open-source / pre-trained models** — use as-is. Fastest and cheapest, available via Amazon Bedrock and SageMaker JumpStart (models from Hugging Face, Meta, Stability AI, Databricks and others).
- **Customized models** — start from a pre-trained FM and adapt it. Increasing cost: prompt engineering → RAG → fine-tuning → continued pre-training.
- **Trained from scratch** — building a new FM can cost **tens of millions of dollars**. Almost never the right answer on this exam.

### Using a model in production

**Managed API service** — call a hosted model over an API (Amazon Bedrock, Amazon Q). No servers, pay per token or request, fastest path to production, less control over internals.

**Self-hosted** — deploy onto infrastructure you control (SageMaker endpoints, EC2). Full control over model, hardware, and data residency; you manage scaling, patching, availability, and cost.

### MLOps

MLOps ensures models aren't just *developed* but **deployed, monitored, and retrained systematically and repeatedly**. It extends DevOps, with the crucial difference that you version **data and models**, not just code.

Key principles:

- **Version control** — data, code, and models, so anything can be rolled back.
- **Automation** — of every stage: ingestion, preprocessing, training, deployment.
- **Continuous integration** — test models consistently.
- **Continuous delivery** — ship models to production reliably.
- **Continuous retraining** — refresh the model as data changes.
- **Continuous monitoring** — watch live quality and detect drift.

A typical MLOps setup chains four pipelines — **data → build & test → deployment → monitoring** — backed by a data repository, code repository, and model repository.

*AWS tools:* SageMaker Pipelines (CI/CD for ML), Model Registry (versioning and approval), Model Monitor (drift alerts), Model Dashboard, Clarify, MLflow on SageMaker.

### Model performance metrics

Classification metrics come from the **confusion matrix**, which cross-tabulates predicted vs. actual values into **TP** (correctly flagged), **FP** (false alarm), **TN** (correctly ignored), and **FN** (missed case). It can be multi-dimensional for multiclass problems and is the best way to evaluate a classifier.

| Metric | Formula | Use it when |
|---|---|---|
| **Precision** | TP / (TP + FP) | **False positives are costly** — wrongly blocking a good customer's card |
| **Recall** (sensitivity) | TP / (TP + FN) | **False negatives are costly** — missing a cancer diagnosis or a fraud |
| **F1 score** | 2 × (P × R) / (P + R) | You need **balance**, especially on **imbalanced datasets** |
| **Accuracy** | (TP + TN) / all | Only for **balanced datasets** — rarely the right answer |

**Why accuracy is a trap:** if 0.1% of transactions are fraudulent, a model that labels everything "legitimate" scores 99.9% accuracy and catches zero fraud.

**AUC-ROC** — plots the true positive rate against the false positive rate across all thresholds. Ranges 0 to 1; 0.5 = random guessing, 1.0 = perfect. Used to compare models and pick the threshold that fits the business need.

**Regression metrics** — all measure error, so **lower is better**:
- **MAE** (mean absolute error) — average absolute gap between predicted and actual.
- **MAPE** (mean absolute percentage error) — the same, expressed as a percentage.
- **RMSE** (root mean squared error) — penalizes large errors more heavily. *"RMSE of 5" means predictions are off by about 5 points on average.*
- **R² (R-squared)** — the share of variance explained by the model; **closer to 1 is better**. *"R² of 0.8" means 80% of the variation is explained by your inputs.*

*How to answer precision-vs-recall questions:* ask which mistake does more damage. Missing something bad → **recall**. Falsely accusing something good → **precision**.

### Business metrics

A model with excellent technical metrics can still fail commercially. Business measures include **ROI**, **cost per user or per inference**, development and training cost, time to market, **customer feedback and satisfaction**, adoption, conversion or retention lift, and hours of manual work saved. Always tie evaluation back to the KPIs defined in phase 1.

---

## Practice Questions — Domain 1 only

**1.** A company needs to convert 50,000 hours of archived call recordings into searchable text. Which service?

A. Amazon Polly · B. Amazon Transcribe · C. Amazon Comprehend · D. Amazon Translate

<details><summary>Answer</summary>

**B — Amazon Transcribe.** Speech → text. Polly is the reverse (text → speech), Comprehend analyzes text you already have, and Translate converts between languages.
</details>

**2.** A model achieves 98% accuracy on training data but 61% on test data. What is happening, and what is the underlying cause?

A. Underfitting; high bias · B. Overfitting; high variance · C. Underfitting; high variance · D. Overfitting; high bias

<details><summary>Answer</summary>

**B — Overfitting caused by high variance.** Strong training performance with a large drop on unseen data is the signature: the model memorized the training set. Fixes: more data, simpler model, regularization, early stopping, data augmentation, ensembling.
</details>

**3.** A retailer wants to group customers into segments for marketing but has no predefined categories. Which approach and algorithm?

A. Classification with k-NN · B. Regression with linear regression · C. Clustering with K-means · D. Anomaly detection with Isolation Forest

<details><summary>Answer</summary>

**C — Clustering with K-means** (unsupervised). No labels exist and the goal is discovering natural groupings. Classification would require predefined labeled segments.
</details>

**4.** A hospital screening model must avoid missing patients who actually have a disease. Which metric should be prioritized?

A. Precision · B. Recall · C. Accuracy · D. R²

<details><summary>Answer</summary>

**B — Recall.** Missing a sick patient is a false negative, and recall measures how many true cases were caught. Accuracy is misleading because the disease is rare.
</details>

**5.** A company scores 20 million records once per week with no latency requirement. Which inference option is most cost-effective?

A. Real-time endpoint · B. Serverless inference · C. Batch transform · D. Asynchronous inference

<details><summary>Answer</summary>

**C — Batch transform.** Large volume, scheduled, no latency need. A real-time endpoint would run 24/7 and sit idle almost all week.
</details>

**6.** Which scenario is **least** appropriate for a machine learning solution?

A. Predicting next quarter's demand · B. Calculating the exact probability of drawing a blue card from a known deck · C. Detecting defects on a production line · D. Recommending products

<details><summary>Answer</summary>

**B.** This is a deterministic problem with a computable exact answer. ML would only ever produce an approximation — write code instead.
</details>

**7.** Which statement correctly describes the relationship between AI, ML, and deep learning?

A. They are three independent fields · B. Deep learning ⊃ ML ⊃ AI · C. AI ⊃ ML ⊃ deep learning · D. ML ⊃ AI ⊃ deep learning

<details><summary>Answer</summary>

**C.** AI is broadest, ML is a subset of AI, deep learning is a subset of ML, and generative AI sits inside deep learning. A rule-based expert system like MYCIN is AI but not ML.
</details>

**8.** A fintech must explain to regulators exactly why each loan application was denied. Which is most appropriate?

A. A large language model · B. A traditional interpretable ML model · C. An agentic AI workflow · D. A diffusion model

<details><summary>Answer</summary>

**B.** Regulatory explainability favors interpretable models such as logistic regression or decision trees, where the decision logic can be traced. Foundation models cannot readily justify individual decisions.
</details>

**9.** Which two are examples of unsupervised learning? (Choose two.)

A. Anomaly detection in network traffic · B. Predicting house prices from square footage · C. Market basket analysis of products bought together · D. Classifying emails as spam · E. Forecasting revenue from labeled history

<details><summary>Answer</summary>

**A and C.** Anomaly detection and association rule learning both find structure in unlabeled data. B, D, and E require labeled training examples, making them supervised.
</details>

**10.** A payload of 800 MB must be processed by a model, and processing takes several minutes per request. Which inference option?

A. Real-time endpoint · B. Serverless inference · C. Asynchronous inference · D. Batch transform

<details><summary>Answer</summary>

**C — Asynchronous inference.** It supports payloads up to 1 GB and processing up to an hour. Real-time and serverless are capped at roughly 25 MB and 4 MB with ~60-second limits.
</details>

**11.** Put the ML project phases in the correct order: *Deployment · Data collection & preparation · Business goal definition · Model training · Monitoring · ML problem framing · Model evaluation*

<details><summary>Answer</summary>

Business goal definition → ML problem framing → Data collection & preparation → Model training → Model evaluation → Deployment → Monitoring.
</details>

**12.** After six months in production, a fraud model's accuracy has steadily declined although the code is unchanged. What is the most likely cause and response?

A. Overfitting; simplify the model · B. Data drift; retrain on recent data · C. Underfitting; add features · D. Hardware failure; redeploy the endpoint

<details><summary>Answer</summary>

**B — Data drift.** Real-world fraud tactics change, so live data no longer matches the training distribution. Detect with SageMaker Model Monitor and retrain on recent data.
</details>

**13.** What is the difference between a model and an algorithm?

<details><summary>Answer</summary>

The **algorithm** is the procedure used to learn from data (linear regression, XGBoost, k-means). The **model** is the artifact produced by running that algorithm on data — the learned parameters/weights you deploy. Same algorithm + different data = different model.
</details>

**14.** A company has 100,000 unlabeled medical images and budget to label only about 1,000. Which learning approach fits best, and what is the technique called?

A. Fully supervised learning; cross-validation · B. Semi-supervised learning; pseudo-labeling · C. Reinforcement learning; reward shaping · D. Unsupervised learning; clustering

<details><summary>Answer</summary>

**B — Semi-supervised learning using pseudo-labeling.** The model trains on the small labeled set, labels the unlabeled data itself, and retrains on the combined mix.
</details>

**15.** In reinforcement learning, what is the **policy**?

A. The reward received after an action · B. The current situation of the environment · C. The strategy the agent uses to choose actions based on state · D. The external system the agent interacts with

<details><summary>Answer</summary>

**C.** Policy = the strategy mapping states to actions. Reward is the feedback, state is the current situation, and environment is the external system.
</details>

**16.** A regression model reports RMSE = 5 and R² = 0.8. What does this mean?

<details><summary>Answer</summary>

Predictions are off by roughly **5 units on average**, and the model's inputs explain about **80% of the variation** in the target — the remaining 20% comes from factors not captured. For error metrics (MAE, MAPE, RMSE) lower is better; for R², closer to 1 is better.
</details>

**17.** Which two hyperparameter changes would you consider to address overfitting? (Choose two.)

A. Increase regularization · B. Increase the number of epochs · C. Use early stopping · D. Increase the learning rate · E. Remove the validation set

<details><summary>Answer</summary>

**A and C.** More regularization penalizes complexity, and early stopping halts training before the model starts memorizing noise. Training for *more* epochs worsens overfitting.
</details>

**18.** A manufacturer needs image-based defect detection on factory-floor devices with unreliable internet. What is the appropriate approach?

A. A large model on a real-time cloud endpoint · B. A small model deployed for inference at the edge · C. Batch transform in the cloud overnight · D. Serverless inference

<details><summary>Answer</summary>

**B — Edge inference with a small model.** Edge deployment gives very low latency, a small compute footprint, and **offline capability**. Cloud options all require connectivity.
</details>

---

## Quick Revision Checklist

- [ ] AI ⊃ ML ⊃ DL ⊃ GenAI; rule-based systems (MYCIN) are AI but not ML
- [ ] ML predicts · GenAI creates · agentic AI acts
- [ ] Model vs. algorithm; parameters vs. hyperparameters (learning rate, batch size, epochs, regularization)
- [ ] Train / validation / test split ≈ 80 / 10 / 10
- [ ] Feature engineering: extraction, selection, transformation
- [ ] Overfitting (high variance) vs. underfitting (high bias) and every fix for each
- [ ] Four inference types **with payload and processing limits**; plus edge inference (SLM vs. LLM)
- [ ] Structured (tabular, time-series) / unstructured / semi-structured; labeled vs. unlabeled
- [ ] Supervised · unsupervised (clustering / association / anomaly) · reinforcement · semi-supervised · self-supervised
- [ ] RL vocabulary: agent, environment, action, reward, state, policy; RLHF
- [ ] Model acronyms: GPT, BERT, RNN, CNN/ResNet, GAN, SVM, WaveNet, XGBoost, diffusion
- [ ] When ML is the *wrong* tool — deterministic problems above all
- [ ] Confusion matrix; precision vs. recall vs. F1 vs. accuracy; AUC-ROC
- [ ] Regression metrics: MAE, MAPE, RMSE (lower better), R² (closer to 1 better)
- [ ] ML project phases in order; MLOps principles; drift and retraining
- [ ] Managed AI service one-liners; Bedrock vs. SageMaker AI

**Sources:** [AIF-C01 Exam Guide — Domain 1](https://docs.aws.amazon.com/aws-certification/latest/ai-practitioner-01/ai-practitioner-01-domain1.html) · *AWS Certified AI Practitioner* course slides (Stéphane Maarek), used as a reference for scope and terminology.
