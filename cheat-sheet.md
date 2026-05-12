# AI-102 Cheat Sheet — Quick Reference

## Service Selection Matrix (MEMORIZE THIS)

| Need | Service |
|------|---------|
| Chat, text generation, code | **Azure OpenAI** (GPT-4o, GPT-4, GPT-3.5-Turbo) |
| Image generation | **Azure OpenAI** (DALL-E) |
| Audio transcription | **Azure OpenAI** (Whisper) OR **Azure AI Speech** (STT) |
| Image analysis (tags, objects, captions) | **Azure AI Vision** (Image Analysis 4.0) |
| OCR (extract text from images/docs) | **Azure AI Vision** (Read API) |
| Custom image classification / object detection | **Custom Vision** (part of Azure AI Vision) |
| Video analysis | **Azure AI Video Indexer** |
| People counting in video | **Azure AI Vision** (Spatial Analysis) |
| Sentiment, key phrases, entities, PII | **Azure AI Language** (Text Analytics) |
| Language detection | **Azure AI Language** |
| Custom Q&A / FAQ bot | **Azure AI Language** (Custom Question Answering) |
| Intent recognition from text | **Azure AI Language** (CLU) |
| Text translation | **Azure AI Translator** |
| Speech-to-text / text-to-speech | **Azure AI Speech** |
| Speech translation | **Azure AI Speech** (Translation) |
| Full-text search + AI enrichment | **Azure AI Search** |
| Extract data from invoices, receipts, IDs | **Azure Document Intelligence** (prebuilt) |
| Extract data from custom forms | **Azure Document Intelligence** (custom) |
| Content moderation | **Azure AI Content Safety** |
| End-to-end AI project management | **Microsoft Foundry** (AI Foundry) |
| Build AI agents | **Microsoft Foundry Agent Service** |

---

## Azure AI Resource Types

| Type | Use When |
|------|----------|
| **Multi-service resource** | Need multiple AI services; single endpoint & key |
| **Single-service resource** | Need only one service; separate billing/tracking |

---

## Authentication Methods

| Method | When |
|--------|------|
| **Subscription key** | Simple dev/test; pass in `Ocp-Apim-Subscription-Key` header |
| **Entra ID (Azure AD) token** | Production; RBAC-based; use `DefaultAzureCredential` |
| **Managed Identity** | App-to-service calls; no credentials in code |
| **Key Vault** | Store & rotate keys securely |

---

## Azure OpenAI — Key Parameters

| Parameter | Effect | Range |
|-----------|--------|-------|
| `temperature` | Randomness/creativity | 0.0–2.0 (lower = deterministic) |
| `top_p` | Nucleus sampling | 0.0–1.0 (lower = focused) |
| `max_tokens` | Max output length | Depends on model |
| `frequency_penalty` | Reduce repetition of tokens | -2.0–2.0 |
| `presence_penalty` | Encourage new topics | -2.0–2.0 |
| `stop` | Stop sequences | Up to 4 sequences |

> **Tip:** Don't modify both `temperature` AND `top_p` — pick one.

### Message Roles
- **system** — Sets behavior/persona
- **user** — The user's prompt
- **assistant** — Model's response (also used for few-shot examples)

### Prompt Engineering Techniques
- **Zero-shot:** No examples, just instruction
- **Few-shot:** Provide examples in the prompt
- **Chain-of-thought:** "Think step by step"
- **Grounding:** Provide context/documents for the model to reference (RAG)

---

## RAG Pattern (Retrieval-Augmented Generation)

```
User query → Azure AI Search (retrieves relevant docs) → Docs + query → Azure OpenAI → Response
```

Key components:
- **Azure AI Search** index with your data
- **Vector search** or **semantic search** for retrieval
- **Azure OpenAI** uses retrieved docs as context in system message
- "On your data" feature in Azure OpenAI for easy setup

---

## Azure AI Search Architecture

```
Data Source → Indexer → [Skillset (AI enrichment)] → Index → Queries
                                ↓
                         Knowledge Store (optional)
```

### Skillset Built-in Skills
- OCR, Image Analysis, Key Phrase Extraction, Entity Recognition
- Language Detection, Sentiment Analysis, Text Translation, Text Split
- Shaper (reshape data), Document Extraction

### Custom Skills
- Implemented as **Azure Function** (Web API skill)
- Input: `values[].data` → Output: `values[].data`
- Must return JSON in the expected schema

### Index Field Attributes
| Attribute | Purpose |
|-----------|---------|
| `searchable` | Full-text search |
| `filterable` | Use in `$filter` |
| `sortable` | Use in `$orderby` |
| `facetable` | Faceted navigation |
| `retrievable` | Returned in results |
| `key` | Unique document ID |

### Query Syntax
- Simple: `search=hotel pool` (default)
- Full Lucene: `queryType=full`, supports `"phrase"`, `field:value`, `*`, `?`, `~fuzzy`, regex
- Filters: `$filter=rating ge 4`
- Select: `$select=name,rating`
- Order: `$orderby=rating desc`

---

## Azure AI Vision

### Image Analysis 4.0 Features
`caption`, `denseCaptions`, `tags`, `objects`, `people`, `smartCrops`, `read` (OCR)

### Custom Vision
| Model Type | Use Case |
|------------|----------|
| **Classification** | "Is this a cat or dog?" |
| **Object Detection** | "Where are the cats in this image?" (bounding boxes) |

Training → Evaluate (precision / recall / AP) → Publish → Predict endpoint

---

## Azure AI Language

### Text Analytics Capabilities
| Feature | Endpoint |
|---------|----------|
| Sentiment | `/text/analytics/v3.x/sentiment` |
| Key Phrases | `/text/analytics/v3.x/keyPhrases` |
| Entities | `/text/analytics/v3.x/entities/recognition/general` |
| PII | `/text/analytics/v3.x/entities/recognition/pii` |
| Language | `/text/analytics/v3.x/languages` |
| Linked Entities | `/text/analytics/v3.x/entities/linking` |

### CLU (Conversational Language Understanding)
- **Intents** — What the user wants to do (e.g., "BookFlight")
- **Entities** — Parameters (e.g., "destination=Paris", "date=tomorrow")
- **Utterances** — Example sentences for training
- Workflow: Create project → Add intents/entities/utterances → Train → Evaluate → Deploy → Query

### Custom Question Answering
- Sources: FAQ URLs, PDF files, manual QnA pairs
- **Multi-turn:** Follow-up prompts to drill deeper
- **Chit-chat:** Pre-built personality (friendly, professional, etc.)
- **Alternate phrasing:** Multiple ways to ask same question
- Deploy as a knowledge base → query via REST

---

## Azure AI Speech

### Speech-to-Text (STT)
```python
speech_config = speechsdk.SpeechConfig(subscription=key, region=region)
recognizer = speechsdk.SpeechRecognizer(speech_config=speech_config)
result = recognizer.recognize_once()
```

### Text-to-Speech (TTS)
```python
synthesizer = speechsdk.SpeechSynthesizer(speech_config=speech_config)
synthesizer.speak_text("Hello world")
```

### SSML (Speech Synthesis Markup Language)
```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis" xml:lang="en-US">
  <voice name="en-US-JennyNeural">
    <prosody rate="slow" pitch="+5%">Hello!</prosody>
    <break time="500ms"/>
    Welcome.
  </voice>
</speak>
```
Key SSML tags: `<speak>`, `<voice>`, `<prosody>` (rate, pitch, volume), `<break>`, `<emphasis>`, `<say-as>`

---

## Azure Document Intelligence

### Prebuilt Models
| Model | Extracts |
|-------|----------|
| Invoice | Vendor, dates, line items, totals |
| Receipt | Merchant, date, items, total, tax |
| ID Document | Name, DOB, address, document number |
| Business Card | Name, company, phone, email |
| W-2 | Tax form fields |

### Custom Models
- Train with at least 5 labeled documents
- **Template model** — fixed layout forms
- **Neural model** — variable layout forms
- **Composed model** — routes to appropriate sub-model

---

## Azure Content Safety / Responsible AI

### Content Filters (Azure OpenAI)
- Categories: **Hate**, **Sexual**, **Violence**, **Self-harm**
- Severity levels: safe, low, medium, high
- Applied to both input (prompt) and output (completion)

### Additional Protections
- **Blocklists** — Custom blocked terms
- **Prompt shields** — Detect prompt injection attempts
- **Groundedness detection** — Check if response is grounded in provided data
- **Protected material detection** — Detect copyrighted content

---

## Common REST API Endpoint Patterns

```
# Azure AI Services (multi-service)
https://<resource>.cognitiveservices.azure.com/

# Azure OpenAI
https://<resource>.openai.azure.com/openai/deployments/<deployment>/chat/completions?api-version=2024-02-01

# Azure AI Search
https://<service>.search.windows.net/indexes/<index>/docs?search=*&api-version=2024-07-01

# Document Intelligence
https://<resource>.cognitiveservices.azure.com/formrecognizer/documentModels/<model>:analyze?api-version=2023-07-31

# Azure AI Language
https://<resource>.cognitiveservices.azure.com/text/analytics/v3.1/<operation>

# Azure AI Speech
Region-based: https://<region>.api.cognitive.microsoft.com/
```

---

## Numbers to Remember

- Custom Vision: minimum **5 images per tag** for classification, **15 per tag** for object detection
- Document Intelligence custom: minimum **5 labeled documents**
- Azure AI Search: free tier = **50 MB storage**, **3 indexes**
- Azure OpenAI GPT-4: **128K token** context window
- Passing score: **700/1000**
- Exam time: **100 minutes**
