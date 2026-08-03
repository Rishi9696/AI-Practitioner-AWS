# AWS AI Practitioner (AIF-C01) — AWS Services Cheat Sheet

One-stop reference for every AWS service covered in the study notes: what it is, when/why to use it, and an example.

---

## A. Generative AI / Foundation Model Platform

| Service | What it is | When / why to use | Example |
|---|---|---|---|
| **Amazon Bedrock** | Fully managed, serverless API access to foundation models from multiple providers (Amazon, Anthropic, Meta, Mistral, Stability AI, Cohere, etc.). Gives you a private copy of the FM; your data never trains the base model. | Want GenAI capability without managing infrastructure. | Using an LLM without managing infrastructure; exam confirms Bedrock never trains on your data. |
| **Amazon Bedrock Knowledge Bases** | Managed RAG implementation — creates vector embeddings from your data in the vector DB of your choice, handles chunking/storage/retrieval. | FM needs to reference outside or proprietary knowledge without retraining. | HR policy Q&A bot over documents that change monthly. |
| **Amazon Bedrock Agents** | Manages multi-step tasks via pre-defined "action groups," task coordination, and integration with Knowledge Bases. | An agent needs to call APIs/tools in sequence to complete a task. | Automated customer service that looks up an order and processes a return. |
| **Amazon Bedrock AgentCore** | Managed runtime for deploying and operating AI agents securely at scale (identity, memory, tools, observability). | Running production-grade agents. | Running production agents at scale. |
| **AgentCore Identity / Policy** | Identity and permission boundaries specifically scoped for AI agents. | Controlling exactly what an agent is allowed to do. | Restricting an agent's access to only approved tools/actions. |
| **Amazon Bedrock Guardrails** | Filters harmful content, redacts PII, blocks denied topics, applies word filters, reduces hallucinations via contextual grounding checks. | Need to block toxic output, restrict topics, filter PII, or mitigate prompt injection. | Chatbot must refuse to discuss competitors, block toxic language, and redact PII; defending against a "template hijacking" prompt-injection attack. |
| **Amazon Bedrock Model Evaluation** | Automatic (BERTScore, F1, judge-model) or human evaluation of models for quality control. | Benchmarking model quality, accuracy, robustness, or toxicity before/after deployment. | Checking bias against a demographic group using a benchmark dataset before deploying. |
| **Amazon Bedrock Prompt Management** | Stores, versions, tests, and compares prompts instead of hard-coding them. | Teams need to version and reuse prompts across production apps. | Letting teams version, test, and reuse prompts instead of hard-coding them. |
| **Bedrock Model Distillation** | A "teacher" model transfers knowledge to a smaller "student" model — up to ~75% cheaper, some accuracy loss. | Need near-large-model quality at lower latency/cost. | Using Nova Premier as the teacher model to distill a smaller, cheaper model. |
| **Amazon Nova (family)** | AWS's own foundation models on Bedrock: Premier (most capable/best teacher), Pro (balanced), Lite (low-cost multimodal), Micro (text-only, lowest latency); Canvas (image), Reel (video), Sonic (speech-to-speech); Nova 2 variants add up to 1M-token context. | Choosing an AWS-native FM by cost/latency/modality tier. | Nova Micro for lowest-latency text-only tasks; Nova Sonic for a live voice assistant. |
| **Amazon Titan** | AWS's earlier FM family (text, image, multimodal, embeddings), customizable with your own data. | Need a customizable, cost-effective first-party model or embeddings. | Titan Embeddings used to produce chunk vectors for a RAG pipeline. |
| **PartyRock** | No-code GenAI app-building playground powered by Bedrock; no AWS account required. | Free, no-code experimentation with GenAI apps. | A team wants to build GenAI apps without an AWS account → PartyRock. |
| **Strands Agents** | Open-source SDK for building agents in code. | Need a code framework to build a custom agent. | Building an agent with full code-level control. |
| **Kiro** | Agentic IDE for spec-driven software development. | AI-assisted software development from specifications. | Generating an app from a written spec. |
| **Amazon Quick** | GenAI-powered business intelligence / analytics tool. | Asking natural-language questions over BI data. | Asking a plain-English question about quarterly sales data. |

---

## B. Ready-Made GenAI Assistants

| Service | What it is | When / why to use | Example |
|---|---|---|---|
| **Amazon Q Business** | Fully managed GenAI assistant for employees, grounded in company data. Built on Bedrock but the underlying FM is not user-selectable; integrates with IAM Identity Center; supports Q Apps. | Need a ready-made assistant that answers questions and performs actions over internal company data. | An assistant that answers employee questions over internal docs, summarizes, and handles time-off requests. |
| **Amazon Q Developer** | Coding assistant plus AWS expertise — answers documentation questions, gives CLI suggestions, analyzes bills, IDE extensions. | Developers need help writing code or managing AWS resources. | Assistant that answers AWS doc questions and helps write code → Amazon Q Developer. |
| **Amazon Q integrations** | Q embedded inside specific services: Q for QuickSight, Q for EC2, Q in AWS Chatbot, Q for Glue. | Need natural-language help inside a specific AWS service's console. | Asking QuickSight a natural-language question about a dashboard. |

---

## C. ML Platform / Custom Model Building

| Service | What it is | When / why to use | Example |
|---|---|---|---|
| **Amazon SageMaker AI** | Full-control platform to build, train, fine-tune, and host your own models. | Need a custom model with full control over training/infrastructure, or explainability/regulatory needs favor traditional ML. | Custom model with full control over training → SageMaker AI. |
| **SageMaker JumpStart** | ML hub of pre-trained FMs, CV, and NLP models (Hugging Face, Meta, Stability AI, Databricks), fully customizable and deployed on SageMaker. | Quickly deploy a pre-trained model into your own environment with full deployment control. | Deploying a pre-built fraud-detection or demand-forecasting solution. |
| **SageMaker Automatic Model Tuning (AMT)** | Automates hyperparameter tuning via grid/random search. | Hyperparameter tuning needed without manual trial and error. | Automatically finding the best learning rate/batch size combination. |
| **SageMaker Ground Truth** | Human data labeling; also used for RLHF and model alignment. | Producing labeled training data or performing RLHF. | 100,000 unlabeled images with budget to label only ~1,000 → Ground Truth for the labeling step. |
| **SageMaker Data Wrangler** | Fixes sampling bias by balancing datasets (e.g., augmenting underrepresented groups); also used for data prep. | Sampling bias needs correcting via data augmentation. | Algorithm flags only certain ethnic groups due to sampling bias → fix via Data Wrangler. |
| **SageMaker Feature Store** | Centralized store for engineered features. | Managing/reusing engineered features across multiple models. | Sharing a "customer lifetime value" feature across several models. |
| **SageMaker Clarify** | Detects/explains bias in datasets and models using statistical metrics; provides feature attribution (explainability); also evaluates FMs on accuracy, robustness, toxicity. | Need bias detection or "why did the model produce this output" explainability. | Service that detects/explains bias and shows which features drove a prediction → SageMaker Clarify. |
| **SageMaker Model Monitor** | Continuous/scheduled monitoring of model quality in production; alerts on drift. | Watching for production data or concept drift. | Fraud model accuracy declines over 6 months due to data drift → Model Monitor + retrain. |
| **SageMaker Role Manager, Model Cards, Model Dashboard** | Governance tools: access control, model documentation, and a central view of all models. | Need governance/documentation of custom models (Model Cards ≠ AWS Artifact — common exam trap). | Artifact documenting intended uses, risk rating, training details, dataset sources/licenses, known biases → SageMaker Model Cards. |
| **SageMaker Pipelines** | CI/CD automation for ML workflows. | Automating training/deployment pipelines. | Automatically retraining and redeploying a model on a schedule. |
| **SageMaker Model Registry** | Model versioning and approval workflow. | Version control of models in MLOps. | Approving a new model version before promoting it to production. |
| **MLflow on SageMaker** | Experiment tracking tool. | Tracking experiment runs/parameters/metrics. | Comparing metrics across multiple training runs. |
| **SageMaker network isolation mode** | Runs training containers with zero outbound internet access. | Training job must have no outbound internet access at all, not even to S3. | Explicit "no outbound internet access at all" requirement → network isolation mode. |
| **SageMaker endpoints** (real-time, serverless, async, batch transform) | Four inference-serving modes for a trained model. | Choosing serving infra by latency/payload/traffic pattern (real-time ~25MB/60s; serverless ~4MB/60s with cold starts; async up to 1GB/1hr; batch up to 100MB/mini-batch). | 20M records processed weekly with no latency requirement → Batch Transform; 800MB payload needing several minutes → Asynchronous. |

---

## D. Language / NLP AI Services (Purpose-Built, Managed)

| Service | What it is | When / why to use | Example |
|---|---|---|---|
| **Amazon Comprehend** | NLP: sentiment, entities, key phrases, language detection, PII detection; custom classification/entity recognition. | Analyzing text (e.g., reviews) without building a custom model. | Analyzing customer reviews for sentiment. |
| **Amazon Comprehend Medical** | Extracts medical information from unstructured clinical text. | Need medical entity extraction from clinical notes. | Pulling medication names and dosages out of a doctor's notes. |
| **Amazon Transcribe** | Speech-to-text with speaker identification and audio toxicity detection. | Converting call recordings/audio into searchable text. | Converting 50,000 hours of archived call recordings into searchable text. |
| **Amazon Transcribe Medical** | Medical speech-to-text. | Converting clinical conversations into text. | Transcribing a doctor-patient conversation. |
| **AWS HealthScribe** | Generates clinical documentation from patient conversations. | Automating clinical note-taking. | Generating a visit summary from a recorded patient conversation. |
| **Amazon Polly** | Converts text into lifelike speech. | Need voice output / reading content aloud. | Reading news articles aloud to users (the reverse of Transcribe). |
| **Amazon Translate** | Neural machine translation between languages. | Localizing content into multiple languages. | Auto-caption workflow: Transcribe (speech→text) then Translate (localize captions). |
| **Amazon Lex** | Conversational bots for voice and text (the tech behind Alexa); IVR. | Building a customer-service chatbot. | Building a voice-and-text customer-service chatbot. |

---

## E. Vision AI Services

| Service | What it is | When / why to use | Example |
|---|---|---|---|
| **Amazon Rekognition** | Image/video analysis: object/face detection, custom labels, content moderation, text-in-image. | Detecting inappropriate images or analyzing visual content. | Detecting inappropriate images uploaded by users. |
| **Amazon Textract** | Extracts text, forms, and tables from scanned documents (OCR + structure). | Digitizing paper invoices/forms. | Extracting printed/handwritten text, tables, and form fields from scanned documents. |

---

## F. Search, Recommendations, Forecasting, Fraud

| Service | What it is | When / why to use | Example |
|---|---|---|---|
| **Amazon Kendra** | Intelligent enterprise search over internal documents using natural language. | Employees need to search internal docs in plain English. | Natural-language search across wiki pages and PDFs. |
| **Amazon Personalize** | Real-time personalized recommendations using "recipes." | Need product recommendations like Amazon.com's. | "Customers who bought this also bought," updated in real time. |
| **Amazon Forecast** | Time-series forecasting. | Predicting future demand/inventory. | Predicting inventory demand for the next quarter. |
| **Amazon Fraud Detector** | Detects online fraud and fake accounts. | Flagging fraudulent signups or transactions. | Flagging a batch of suspicious new account signups. |

---

## G. Human-in-the-Loop / Responsible AI Tools

| Service | What it is | When / why to use | Example |
|---|---|---|---|
| **Amazon Augmented AI (A2I)** | Human review workflows for low-confidence or high-stakes ML predictions. | Routing predictions for human review before action is taken. | Medical imaging model flags uncertain predictions for radiologist review. |
| **Amazon Mechanical Turk** | Crowdsourced human task completion. | Need on-demand human workers for labeling/review tasks. | Crowdsourcing a large one-off labeling task. |

---

## H. Vector Stores / RAG Data Infrastructure

| Service | What it is | When / why to use | Example |
|---|---|---|---|
| **Amazon OpenSearch Service** | Search/analytics DB with fast k-NN similarity search; the most common RAG vector store. | Building scalable, real-time vector similarity search for RAG. | Which two services store vector embeddings for RAG? → OpenSearch and Neptune Analytics. |
| **Amazon Aurora PostgreSQL (pgvector)** | Relational DB with vector search support. | Want vector storage inside a relational DB you already use. | Adding vector search to an existing Aurora-backed app. |
| **Amazon RDS for PostgreSQL (pgvector)** | Same as above, RDS variant. | Same as above, for RDS users. | Adding vector search to an existing RDS-backed app. |
| **Amazon Neptune Analytics** | Graph database enabling GraphRAG (relationship-based retrieval). | Retrieval needs to traverse relationships, not just similarity. | GraphRAG scenario naming Neptune Analytics for relationship-aware retrieval. |
| **Amazon S3 Vectors** | Cost-effective, durable vector storage with sub-second queries. | Want cheap, durable vector storage. | Storing millions of embeddings cheaply for infrequent queries. |
| **Amazon DocumentDB / Amazon MemoryDB** | Also support vector search. | Need vector search alongside document or in-memory data models. | Adding similarity search to an existing DocumentDB collection. |
| **Amazon S3** | General object storage; RAG data source, required for Bedrock fine-tuning training data, and destination for Bedrock batch inference output. | Storing training/fine-tuning data, RAG source documents, batch job output. | Bedrock training data stored in an SSE-KMS-encrypted S3 bucket, requiring an IAM role with S3 access + KMS decrypt permission. |
| **AWS Glue (Data Catalog) / Glue DataBrew** | Data cataloging and data-prep/ETL tooling. | Organizing/documenting datasets, or preparing data in a pipeline. | Cataloging datasets so they can be discovered and reused. |

---

## I. Compute for AI

| Service | What it is | When / why to use | Example |
|---|---|---|---|
| **AWS Trainium** | Specialized chip for model training. | Cost-efficient large-scale training workloads. | Training a large foundation model cost-efficiently. |
| **AWS Inferentia** | Specialized chip for inference. | Cost-efficient inference at scale. | Serving high-volume inference requests cheaply. |

---

## J. Security Services

| Service | What it is | When / why to use | Example |
|---|---|---|---|
| **AWS IAM** | Users, Groups, Policies (JSON), Roles; identity verification and resource-level access control. | Controlling who can invoke a model or service. | Who configures IAM permissions controlling Bedrock model invocation? → the customer (shared responsibility model). |
| **AWS KMS** | Manages encryption keys; enables S3 SSE-KMS. | Customer-managed encryption keys are needed. | Bedrock training on SSE-KMS-encrypted S3 data requires an IAM role with S3 access AND KMS decrypt permission. |
| **Amazon Macie** | Finds sensitive data such as PII in S3 buckets. | Locating PII in a training-data bucket. | Finding/classifying PII in S3 buckets used for training. |
| **AWS PrivateLink / VPC Endpoints** | Private access to AWS services from inside a VPC; traffic never traverses the public internet. | Traffic must stay inside the VPC. | An app inside a VPC must call Bedrock without using the public internet → VPC endpoint via PrivateLink. |
| **Amazon Inspector** | Finds software vulnerabilities in EC2, ECR images, and Lambda functions. | Scanning workloads for vulnerabilities. | Scanning container images for known CVEs. |
| **AWS Secrets Manager** | Securely stores credentials and API keys. | Avoiding hard-coded secrets in application code. | Storing a database password instead of hard-coding it. |

---

## K. Governance, Compliance, and Monitoring Services

| Service | What it is | When / why to use | Example |
|---|---|---|---|
| **AWS CloudTrail** | Tracks API calls made by users in the account (who did what, when). | Auditing who invoked which Bedrock API and when. | Audit log of every API call, including who called Bedrock and when → CloudTrail. |
| **AWS Config** | Tracks configuration changes and evaluates compliance against rules. | Tracking configuration changes or evaluating compliance continuously. | Service that continuously evaluates whether resource configurations comply with defined rules → Config. |
| **AWS Audit Manager** | Automates evidence collection for audits of *your own* environment using prebuilt frameworks. | Assembling audit evidence for your own environment. | Collecting evidence for an internal SOC 2 readiness audit. |
| **AWS Artifact** | Self-service portal to download AWS's own compliance reports (SOC, ISO, PCI) and agreements. | An auditor requests AWS's own compliance reports. | Where to get AWS's SOC 2 report → AWS Artifact (not Audit Manager). |
| **AWS Trusted Advisor** | Best-practice recommendations across cost, performance, security, and service limits. | Seeking best-practice insights on your account setup. | Checking for underutilized resources or security best-practice gaps. |
| **Amazon CloudWatch** | Metrics, logs, and alarms for operational monitoring. | General operational monitoring (contrast with CloudTrail, which tracks user/API activity). | CloudWatch = metrics/logs vs. CloudTrail = API activity, in the same audit scenario. |
| **AWS Organizations / SCPs** | Multi-account governance and permission guardrails. | Enforcing permission boundaries across multiple accounts. | Blocking an entire business unit from using a specific AWS region or service. |

---

## Quick Decision Cheat Sheet

**Governance / compliance — which service?**
CloudTrail = who did what · Config = resource configuration compliance · Audit Manager = evidence for *your* audits · Artifact = *AWS's* compliance reports · Inspector = software vulnerabilities · Trusted Advisor = best-practice recommendations · Macie = PII in S3.

**Responsible AI — which tool?**
SageMaker Clarify = detect bias / explain predictions · Data Wrangler = balance a biased dataset · Model Monitor = watch production drift · Amazon A2I = human review of predictions · Ground Truth = labeling + RLHF · Bedrock Guardrails = filter output / block topics / redact PII · Model Cards = document the model.

**Picking a service for a task:**
Purpose-built managed service (Transcribe, Translate, Comprehend, Rekognition, Textract, Personalize, Lex, Polly, Kendra) for standard tasks · Bedrock for GenAI without infrastructure · SageMaker AI for full custom-model control · SageMaker JumpStart to deploy a pre-trained model into your own environment · Amazon Q Business for a ready-made company-data assistant · Amazon Q Developer for developer/AWS help · PartyRock for no-code, no-account experimentation.
