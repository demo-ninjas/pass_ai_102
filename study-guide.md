# AI-102 Study Guide — Detailed Objectives & Resources

> Based on skills measured as of December 23, 2025

---

## Domain 1: Plan and Manage an Azure AI Solution (20–25%)

### 1.1 Select the Appropriate Microsoft Foundry Services

| Task | Key Service |
|------|-------------|
| Generative AI solution | Azure OpenAI, Microsoft Foundry |
| Computer vision solution | Azure AI Vision (Image Analysis 4.0) |
| NLP solution | Azure AI Language |
| Speech solution | Azure AI Speech |
| Information extraction | Azure Document Intelligence |
| Knowledge mining | Azure AI Search |

**Study:** [Plan and Prepare to Develop AI Solutions on Azure](https://learn.microsoft.com/en-us/training/modules/prepare-azure-ai-development/)

### 1.2 Plan, Create and Deploy a Microsoft Foundry Service

- Plan for Responsible AI principles
- Create an Azure AI resource (multi-service vs single-service)
- Choose appropriate AI models for your solution
- Deploy AI models (deployment options: Standard, Global, Provisioned)
- Install and use SDKs and REST APIs
- Determine default endpoints
- Integrate into CI/CD pipelines
- Plan and implement container deployment

**Study:** [Create and manage Azure AI Services](https://learn.microsoft.com/en-us/training/modules/create-manage-cognitive-services/)

### 1.3 Manage, Monitor, and Secure a Microsoft Foundry Service

- Monitor an Azure AI resource (diagnostic logging, metrics)
- Manage costs (pricing tiers, budgets, cost alerts)
- Manage and protect account keys (key rotation, Key Vault)
- Manage authentication (Azure AD/Entra ID, managed identities, SAS)

**Study:** [Secure Azure AI Services](https://learn.microsoft.com/en-us/training/modules/secure-cognitive-services/)

### 1.4 Implement AI Solutions Responsibly

- Content moderation solutions
- Responsible AI insights, content safety
- Content filters and blocklists
- Prompt shields and harm detection
- Responsible AI governance framework

**Study:** [Implement content moderation with Azure AI Content Safety](https://learn.microsoft.com/en-us/azure/ai-services/content-safety/)

---

## Domain 2: Implement Generative AI Solutions (15–20%)

### 2.1 Build Generative AI Solutions with Microsoft Foundry

- Plan and prepare for a generative AI solution
- Deploy hub, project, and resources with Microsoft Foundry
- Deploy appropriate generative AI model
- Implement prompt flow
- Implement RAG (Retrieval-Augmented Generation) — ground model in your data
- Evaluate models and flows
- Integrate project with Microsoft Foundry SDK
- Use prompt templates

**Study:**
- [Get started with Azure AI Foundry](https://learn.microsoft.com/en-us/training/modules/introduction-to-azure-ai-studio/)
- [Build a RAG-based copilot](https://learn.microsoft.com/en-us/training/modules/build-copilot-ai-studio/)

### 2.2 Use Azure OpenAI in Foundry Models to Generate Content

- Provision Azure OpenAI resource
- Select and deploy models (GPT-4, GPT-4o, GPT-3.5-Turbo, DALL-E)
- Submit prompts for code and natural language
- Generate images with DALL-E
- Integrate into applications
- Use large multimodal models

**Study:**
- [Get started with Azure OpenAI Service](https://learn.microsoft.com/en-us/training/modules/get-started-openai/)
- [Apply prompt engineering with Azure OpenAI](https://learn.microsoft.com/en-us/training/modules/apply-prompt-engineering-azure-openai/)

### 2.3 Optimize and Operationalize a Generative AI Solution

- Configure parameters (temperature, top_p, max_tokens, frequency_penalty, presence_penalty)
- Model monitoring and diagnostics
- Manage resources for deployment (scaling, model updates)
- Enable tracing and collect feedback
- Model reflection
- Deploy containers for local/edge
- Orchestrate multiple generative models
- Prompt engineering techniques
- Fine-tune a generative model

**Study:**
- [Fine-tune Azure OpenAI models](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/fine-tuning)

---

## Domain 3: Implement an Agentic Solution (5–10%)

### 3.1 Create Custom Agents

- Role and use cases of an agent
- Configure resources to build an agent
- Create agent with Microsoft Foundry Agent Service
- Implement complex agents with Microsoft Agent Framework
- Complex workflows: orchestration, multi-agent, multi-user, autonomous
- Test, optimize, and deploy an agent

**Study:**
- [Azure AI Agent Service documentation](https://learn.microsoft.com/en-us/azure/ai-services/agents/)

---

## Domain 4: Implement Computer Vision Solutions (10–15%)

### 4.1 Analyze Images

- Select visual features for image processing
- Detect objects, generate image tags
- Image analysis features in requests
- Interpret image processing responses
- Extract text (OCR) with Azure Vision in Foundry Tools
- Convert handwritten text

**Study:**
- [Analyze images with Azure AI Vision](https://learn.microsoft.com/en-us/training/modules/analyze-images/)
- [Read text with Azure AI Vision](https://learn.microsoft.com/en-us/training/modules/read-text-images-documents-with-computer-vision-service/)

### 4.2 Implement Custom Vision Models

- Image classification vs object detection
- Label images
- Train custom image model
- Evaluate metrics (precision, recall, AP)
- Publish and consume model
- Build model code-first

**Study:**
- [Custom Vision documentation](https://learn.microsoft.com/en-us/azure/ai-services/custom-vision-service/)

### 4.3 Analyze Videos

- Azure AI Video Indexer: extract insights
- Spatial Analysis: detect presence/movement of people

**Study:**
- [Analyze video with Video Indexer](https://learn.microsoft.com/en-us/training/modules/analyze-video/)

---

## Domain 5: Implement Natural Language Processing Solutions (15–20%)

### 5.1 Analyze and Translate Text

- Extract key phrases and entities
- Determine sentiment
- Detect language
- Detect PII (personally identifiable information)
- Translate text and documents (Azure Translator)

**Study:**
- [Analyze text with Azure AI Language](https://learn.microsoft.com/en-us/training/modules/analyze-text-with-text-analytics-service/)
- [Translate text with Azure AI Translator](https://learn.microsoft.com/en-us/training/modules/translate-text-with-translator-service/)

### 5.2 Process and Translate Speech

- Generative AI speaking capabilities
- Text-to-speech (TTS) and speech-to-text (STT)
- Improve TTS with SSML (Speech Synthesis Markup Language)
- Custom speech solutions
- Intent and keyword recognition
- Speech-to-speech and speech-to-text translation

**Study:**
- [Create speech-enabled apps with Azure AI Speech](https://learn.microsoft.com/en-us/training/modules/transcribe-speech-input-text/)
- [Translate speech with Azure AI Speech](https://learn.microsoft.com/en-us/training/modules/translate-speech-speech-service/)

### 5.3 Implement Custom Language Models

- Create intents, entities, add utterances (CLU - Conversational Language Understanding)
- Train, evaluate, deploy, test language understanding model
- Optimize, backup, recover models
- Consume from client application
- Custom question answering: create project, add QnA pairs, import sources
- Train, test, publish knowledge base
- Multi-turn conversations
- Alternate phrasing, chit-chat
- Export knowledge base
- Multi-language question answering
- Custom translation

**Study:**
- [Build a conversational language understanding model](https://learn.microsoft.com/en-us/training/modules/build-language-understanding-model/)
- [Create a question answering solution](https://learn.microsoft.com/en-us/training/modules/create-question-answer-solution-ai-language/)

---

## Domain 6: Implement Knowledge Mining & Information Extraction (15–20%)

### 6.1 Implement an Azure AI Search Solution

- Provision Azure AI Search, create index, define skillset
- Create data sources and indexers
- Implement custom skills in a skillset
- Create and run indexer
- Query index (syntax, sorting, filtering, wildcards)
- Knowledge Store projections (file, object, table)
- Semantic and vector store solutions

**Study:**
- [Create an Azure AI Search solution](https://learn.microsoft.com/en-us/training/modules/create-azure-cognitive-search-solution/)
- [Create a custom skill for Azure AI Search](https://learn.microsoft.com/en-us/training/modules/create-enrichment-pipeline-azure-cognitive-search/)

### 6.2 Implement Azure Document Intelligence Solution

- Provision Document Intelligence resource
- Prebuilt models (invoice, receipt, ID, business card)
- Custom document intelligence model
- Train, test, publish custom model
- Composed document intelligence model

**Study:**
- [Extract data from forms with Azure Document Intelligence](https://learn.microsoft.com/en-us/training/modules/work-form-recognizer/)

### 6.3 Extract Information with Azure Content Understanding

- OCR pipeline for text extraction
- Summarize, classify, detect document attributes
- Extract entities, tables, images
- Process documents, images, videos, audio

**Study:**
- [Azure Content Understanding documentation](https://learn.microsoft.com/en-us/azure/ai-services/content-understanding/)

---

## Documentation Quick Links

| Service | Docs |
|---------|------|
| Azure AI Services | https://learn.microsoft.com/en-us/azure/ai-services/ |
| Azure AI Vision | https://learn.microsoft.com/en-us/azure/ai-services/computer-vision/ |
| Azure AI Language | https://learn.microsoft.com/en-us/azure/ai-services/language-service/ |
| Azure AI Speech | https://learn.microsoft.com/en-us/azure/ai-services/speech-service/ |
| Azure OpenAI | https://learn.microsoft.com/en-us/azure/ai-services/openai/ |
| Azure AI Search | https://learn.microsoft.com/en-us/azure/search/ |
| Azure Document Intelligence | https://learn.microsoft.com/en-us/azure/ai-services/document-intelligence/ |
| Azure AI Video Indexer | https://learn.microsoft.com/en-us/azure/azure-video-indexer/ |
| Azure AI Foundry | https://learn.microsoft.com/en-us/azure/ai-studio/ |
