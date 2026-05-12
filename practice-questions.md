# AI-102 Practice Questions — Self-Test

> Answers at the bottom. Try to answer before looking!

---

## Domain 1: Plan and Manage an Azure AI Solution

**Q1.** You need to use Azure AI Language and Azure AI Vision in the same application with a single endpoint. What type of resource should you create?

A) Azure AI Language resource  
B) Azure AI Vision resource  
C) Azure AI Services multi-service resource  
D) Two separate resources with a load balancer  

---

**Q2.** You want to ensure that API keys for your Azure AI service are stored securely and can be rotated without downtime. What should you use?

A) Store keys in app settings  
B) Use Azure Key Vault with managed identity  
C) Use SAS tokens  
D) Hardcode the secondary key as a backup  

---

**Q3.** Your company requires that Azure AI services run in an isolated network without internet access. What should you deploy?

A) A virtual network with service endpoints  
B) Azure AI services in a Docker container  
C) A private endpoint for the Azure AI resource  
D) Both B and C are valid approaches  

---

**Q4.** You need to block specific words from appearing in Azure OpenAI responses. What should you configure?

A) Content filters with high severity  
B) A custom blocklist  
C) Prompt shields  
D) A system message instructing the model  

---

## Domain 2: Implement Generative AI Solutions

**Q5.** You want Azure OpenAI to generate highly creative marketing copy. Which parameter should you increase?

A) max_tokens  
B) frequency_penalty  
C) temperature  
D) presence_penalty  

---

**Q6.** You are building a RAG solution. The model sometimes generates answers not found in the provided documents. What should you implement to detect this?

A) Content filters  
B) Groundedness detection  
C) Prompt shields  
D) Fine-tuning  

---

**Q7.** In an Azure OpenAI chat completion request, which message role defines the AI assistant's behavior and constraints?

A) user  
B) assistant  
C) system  
D) function  

---

**Q8.** You need to deploy a generative AI model that must handle a guaranteed minimum number of tokens per minute. Which deployment type should you use?

A) Standard  
B) Global Standard  
C) Provisioned Throughput  
D) Free tier  

---

**Q9.** You are using prompt flow in Microsoft Foundry. What is the primary purpose of an evaluation flow?

A) Deploy the model to production  
B) Measure the quality of a flow's outputs against ground truth  
C) Fine-tune the model weights  
D) Generate training data  

---

## Domain 3: Implement an Agentic Solution

**Q10.** You need to build an AI agent that can search the web, query a database, and generate reports autonomously. Which service should you use?

A) Azure AI Language  
B) Microsoft Foundry Agent Service  
C) Azure AI Search  
D) Azure Logic Apps  

---

## Domain 4: Implement Computer Vision Solutions

**Q11.** You need to detect and return bounding box coordinates for objects in an image. Which Azure AI Vision feature should you use?

A) Tags  
B) Caption  
C) Objects  
D) Categories  

---

**Q12.** You are training a Custom Vision model for classifying types of flowers. After training, the model has high precision but low recall for "roses." What does this mean?

A) The model identifies many roses correctly but also misidentifies other flowers as roses  
B) The model rarely misidentifies other flowers as roses, but misses many actual roses  
C) The model performs perfectly on roses  
D) The training data has too many rose images  

---

**Q13.** Which API should you use to extract printed AND handwritten text from a scanned document?

A) Image Analysis - Tags  
B) Image Analysis - Read (OCR)  
C) Custom Vision  
D) Face API  

---

**Q14.** You need to analyze a video to extract topics, keywords, and detect scenes. Which service should you use?

A) Azure AI Vision - Spatial Analysis  
B) Azure AI Video Indexer  
C) Azure AI Vision - Image Analysis  
D) Azure Media Services  

---

## Domain 5: Implement NLP Solutions

**Q15.** You need to detect if a customer review is positive, negative, or neutral. Which Azure AI Language feature should you use?

A) Key phrase extraction  
B) Entity recognition  
C) Sentiment analysis  
D) Language detection  

---

**Q16.** In a CLU (Conversational Language Understanding) model, a user says "Book me a flight to Paris tomorrow." What are the intents and entities?

A) Intent: "Paris", Entities: "Book flight", "tomorrow"  
B) Intent: "BookFlight", Entities: "Paris" (destination), "tomorrow" (date)  
C) Intent: "tomorrow", Entities: "Paris", "flight"  
D) Intent: "travel", Entities: "BookFlight"  

---

**Q17.** You want Azure AI Speech to read text aloud with a slower pace and higher pitch. What technology should you use?

A) Custom Speech  
B) SSML with `<prosody>` tag  
C) Speech Translation  
D) Intent Recognition  

---

**Q18.** You are building a FAQ bot using custom question answering. A user asks a question, and you want the bot to ask a follow-up clarification question. What feature should you configure?

A) Chit-chat  
B) Alternate phrasing  
C) Multi-turn conversation  
D) Active learning  

---

**Q19.** You need to detect and redact social security numbers and email addresses from text. Which feature should you use?

A) Entity recognition  
B) PII detection  
C) Key phrase extraction  
D) Sentiment analysis  

---

## Domain 6: Knowledge Mining & Information Extraction

**Q20.** In Azure AI Search, you want to enrich documents with AI-generated key phrases and entity recognition during indexing. What should you configure?

A) A data source  
B) An indexer  
C) A skillset  
D) A synonym map  

---

**Q21.** You need to implement a custom AI enrichment step in Azure AI Search that calls your own API. What should you create?

A) A built-in skill  
B) A custom skill backed by an Azure Function  
C) A knowledge store projection  
D) A scoring profile  

---

**Q22.** You are querying an Azure AI Search index and need to return only hotels with a rating of 4 or higher, sorted by price. Which query parameters do you need?

A) `$filter=rating ge 4&$orderby=price asc`  
B) `search=rating:4&sort=price`  
C) `$select=rating,price&$top=4`  
D) `queryType=full&search=rating:[4 TO *]`  

---

**Q23.** You need to extract data from invoices that have varying layouts from different vendors. Which Document Intelligence approach should you use?

A) Prebuilt Invoice model  
B) Custom template model  
C) Custom neural model  
D) Prebuilt Receipt model  

---

**Q24.** You have trained three custom Document Intelligence models for three different form types. You want a single endpoint that automatically routes to the correct model. What should you create?

A) A custom skill  
B) A composed model  
C) A prebuilt model  
D) An indexer  

---

**Q25.** In Azure AI Search, you want to store enriched data in Azure Table Storage for analytics. What should you configure?

A) An index  
B) A data source  
C) A knowledge store with table projections  
D) A custom skill  

---

---

## Answers

| Q | Answer | Explanation |
|---|--------|-------------|
| 1 | **C** | Multi-service resource provides a single endpoint and key for multiple Azure AI services |
| 2 | **B** | Key Vault + managed identity = secure key storage with automatic rotation, no credentials in code |
| 3 | **D** | Both containers (disconnected) and private endpoints (network-isolated) work for isolation |
| 4 | **B** | Blocklists let you define specific terms to block; content filters work on categories/severity |
| 5 | **C** | Higher temperature = more random/creative output |
| 6 | **B** | Groundedness detection checks if the model's response is supported by the provided context |
| 7 | **C** | The system message defines the assistant's behavior, persona, and constraints |
| 8 | **C** | Provisioned Throughput guarantees a reserved amount of model processing capacity |
| 9 | **B** | Evaluation flows measure output quality against expected results/ground truth |
| 10 | **B** | Microsoft Foundry Agent Service is designed for building autonomous AI agents with tool use |
| 11 | **C** | Objects feature returns detected objects with bounding box coordinates |
| 12 | **B** | High precision = few false positives; low recall = many false negatives (misses real roses) |
| 13 | **B** | The Read API (OCR) handles both printed and handwritten text |
| 14 | **B** | Video Indexer extracts topics, keywords, transcript, scenes, faces, and more from video |
| 15 | **C** | Sentiment analysis returns positive, negative, neutral, or mixed sentiment |
| 16 | **B** | "BookFlight" is the intent (action); "Paris" and "tomorrow" are entities (parameters) |
| 17 | **B** | SSML `<prosody rate="slow" pitch="+5%">` controls speech rate and pitch |
| 18 | **C** | Multi-turn conversations enable follow-up prompts for clarification |
| 19 | **B** | PII detection identifies and can redact personal data like SSNs and emails |
| 20 | **C** | A skillset defines the AI enrichment pipeline applied during indexing |
| 21 | **B** | Custom skills are implemented as Azure Functions with the Web API skill type |
| 22 | **A** | `$filter` for conditions, `$orderby` for sorting — this is OData query syntax |
| 23 | **A** | The prebuilt Invoice model handles varying invoice layouts out of the box |
| 24 | **B** | A composed model combines multiple custom models and routes automatically |
| 25 | **C** | Knowledge store with table projections saves enriched data to Azure Table Storage |
