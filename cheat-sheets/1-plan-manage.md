# Cheat Sheet 1: Plan and Manage an Azure AI Solution (20–25%)

> Highest-weighted domain. Know service selection and security cold.

---

## Service Selection Matrix (MEMORIZE THIS)

| Need | Service | Key Detail |
|------|---------|------------|
| Chat, text gen, code | **Azure OpenAI** | GPT-4o, GPT-4o-mini, GPT-4 |
| Image generation | **Azure OpenAI** | DALL-E 3 / gpt-image-1 |
| Audio transcription | **Azure OpenAI** (Whisper) OR **Azure AI Speech** (STT) | Whisper = batch; Speech = real-time |
| Image analysis | **Azure AI Vision** | Image Analysis 4.0 API |
| OCR (text from images) | **Azure AI Vision** | Read API |
| Face detection/recognition | **Azure AI Face** | Requires limited access approval |
| Custom image classification | **Custom Vision** | Part of Azure AI Vision (retiring 9/2028 — migrate to Azure ML AutoML or Content Understanding) |
| Video analysis | **Azure AI Video Indexer** | Topics, keywords, faces, scenes |
| People counting in video | **Azure AI Vision** | Spatial Analysis (edge containers) |
| Sentiment, key phrases, entities, PII | **Azure AI Language** | Text Analytics features |
| Language detection | **Azure AI Language** | Up to 5,120 characters per doc |
| Custom Q&A / FAQ bot | **Azure AI Language** | Custom Question Answering |
| Intent recognition from text | **Azure AI Language** | CLU (Conversational Language Understanding) |
| Text translation | **Azure AI Translator** | 100+ languages |
| Document translation | **Azure AI Translator** | Batch translate entire documents |
| Speech-to-text / text-to-speech | **Azure AI Speech** | Neural voices, SSML |
| Speech translation | **Azure AI Speech** | Real-time speech-to-speech |
| Full-text search + AI enrichment | **Azure AI Search** | Skillsets, indexers, semantic/vector |
| Extract data from invoices/receipts | **Azure Document Intelligence** | Prebuilt models |
| Extract data from custom forms | **Azure Document Intelligence** | Custom + composed models |
| Content moderation | **Azure AI Content Safety** | Hate, sexual, violence, self-harm |
| End-to-end AI project mgmt | **Microsoft Foundry** | Hubs, projects, prompt flow |
| Build AI agents | **Microsoft Foundry Agent Service** | Tool-using autonomous agents |

---

## Azure AI Resource Types

| Type | Endpoint | Use When |
|------|----------|----------|
| **Multi-service** | `<name>.cognitiveservices.azure.com` | Multiple services, single key + endpoint |
| **Single-service** | `<name>.cognitiveservices.azure.com` | One service only; separate billing/tracking |

**Exam trap:** Multi-service resources use ONE key for all services. Single-service resources get their own key.

### When to use which:
- **Multi-service**: Simplicity, fewer keys to manage, single billing
- **Single-service**: Separate cost tracking per service, different regions, free tier (only available for single-service)

> **Exam trap:** The **Translator** uses a **global endpoint** (`api.cognitive.microsofttranslator.com`), not the resource-specific `.cognitiveservices.azure.com` endpoint. It also requires the `Ocp-Apim-Subscription-Region` header.

---

## Authentication — Exact Parameters

| Method | Header / Code | When |
|--------|---------------|------|
| **Subscription key** | `Ocp-Apim-Subscription-Key: <key>` header | Dev/test; quick prototyping |
| **Entra ID token** | `Authorization: Bearer <token>` header | Production; RBAC-based |
| **Managed Identity** | `DefaultAzureCredential()` in SDK | App → service calls; NO credentials in code |
| **Key Vault** | Store keys in Key Vault, retrieve at runtime | Secure key storage + rotation |

### Key Rotation Pattern
1. App uses **Key 1**
2. Regenerate **Key 2** (safely, since app uses Key 1)
3. Update app to use **Key 2**
4. Regenerate **Key 1**
5. Both keys are now fresh, app never went down

### RBAC Roles for Azure AI
| Role | Permissions |
|------|-------------|
| `Cognitive Services User` | Call APIs (read) |
| `Cognitive Services Contributor` | Manage resources + call APIs |
| `Cognitive Services OpenAI User` | Call OpenAI APIs specifically |
| `Cognitive Services OpenAI Contributor` | Manage OpenAI deployments + call APIs |

---

## Networking & Security

| Feature | Purpose |
|---------|---------|
| **Virtual Network (VNet)** | Restrict access to specific networks |
| **Private Endpoint** | Private IP within your VNet; no public internet |
| **Service Endpoint** | Optimized route from VNet to Azure service (still public IP) |
| **Firewall rules** | Allow specific IP ranges |

**Containers for disconnected/edge:**
- Many Azure AI services available as **Docker containers**
- Run on-premises or at the edge
- Still require billing connection (phone-home) unless using **disconnected containers** (special approval)
- Container images from: `mcr.microsoft.com/azure-cognitive-services/`

### When to Use Containers vs Cloud

| Scenario | Deploy As |
|----------|----------|
| Standard cloud app, good internet | **Cloud** (easiest) |
| On-premises, low latency, data residency | **Connected container** (needs billing phone-home) |
| Factory floor, no internet at all | **Disconnected container** (requires special approval) |
| Edge device with live camera (Spatial Analysis) | **Edge container** (NVIDIA GPU required) |

---

## Monitoring & Cost Management

| Tool | Purpose |
|------|---------|
| **Azure Monitor** | Metrics, logs, alerts for AI resources |
| **Diagnostic settings** | Route logs to Log Analytics, Storage, Event Hubs |
| **Application Insights** | End-to-end tracing of AI calls |
| **Cost Management** | Budgets, cost alerts, spending analysis |
| **Pricing tiers** | Free (F0) vs Standard (S0); free has rate limits |

### Key Metrics to Monitor
- **Total Calls** — API call volume
- **Total Errors** — Failed requests (4xx, 5xx)
- **Latency** — Response time
- **Blocked Calls** — Rate limiting (HTTP 429)

### Pricing Tiers
| Tier | Label | Note |
|------|-------|------|
| **Free** | F0 | Rate-limited; single-service only |
| **Standard** | S0 | Production use |

---

## Responsible AI — Content Safety Details

### Content Filter Categories (Azure OpenAI)
| Category | What It Catches |
|----------|----------------|
| **Hate** | Discrimination, slurs, stereotyping |
| **Sexual** | Sexual content, explicit material |
| **Violence** | Physical harm, weapons, cruelty |
| **Self-harm** | Suicide, self-injury, eating disorders |

### Severity Levels
`safe` → `low` → `medium` → `high`

Applied to BOTH **input** (prompt) and **output** (completion).

### Additional Protections (know each one)
| Feature | What It Does | When To Use |
|---------|--------------|-------------|
| **Blocklists** | Block specific terms/phrases you define | Custom banned words |
| **Prompt shields** | Detect prompt injection / jailbreak attempts | User-facing apps |
| **Groundedness detection** | Check if response is supported by provided context | RAG solutions |
| **Protected material detection** | Detect copyrighted text/code | Public-facing content generation |

### Responsible AI Principles (Microsoft's 6)
1. **Fairness** — AI should treat all people fairly
2. **Reliability & Safety** — AI should perform reliably and safely
3. **Privacy & Security** — AI should be secure and respect privacy
4. **Inclusiveness** — AI should empower and engage everyone
5. **Transparency** — AI should be understandable
6. **Accountability** — People should be accountable for AI

---

## Deployment Options for AI Models

| Deployment Type | Key Feature |
|----------------|-------------|
| **Standard** | Pay-per-token, shared infrastructure |
| **Global Standard** | Multi-region routing, highest availability |
| **Provisioned Throughput (PTU)** | Reserved capacity, guaranteed tokens/min |
| **Data Zone Standard** | Data stays in specified geography |

**Exam tip:** "Guaranteed minimum tokens per minute" = **Provisioned Throughput**

---

## CI/CD Integration
- Azure AI resources can be managed via **ARM templates**, **Bicep**, **Terraform**
- Model deployments can be automated via **Azure CLI** or **REST API**
- Use **Azure DevOps** or **GitHub Actions** for CI/CD pipelines
- **Microsoft Foundry SDK** for programmatic project management

---

## REST API Endpoint Patterns

```
# Multi-service resource
https://<resource>.cognitiveservices.azure.com/

# Azure OpenAI
https://<resource>.openai.azure.com/openai/deployments/<deployment>/chat/completions?api-version=2024-02-01

# Azure AI Search
https://<service>.search.windows.net/

# Document Intelligence
https://<resource>.cognitiveservices.azure.com/formrecognizer/

# Face API
https://<resource>.cognitiveservices.azure.com/face/v1.0/detect

# Speech
https://<region>.api.cognitive.microsoft.com/
# OR: https://<region>.tts.speech.microsoft.com/

# Translator
https://api.cognitive.microsofttranslator.com/
```

### Common Headers
| Header | Value |
|--------|-------|
| `Ocp-Apim-Subscription-Key` | Your subscription key |
| `Content-Type` | `application/json` (most APIs) |
| `Ocp-Apim-Subscription-Region` | Required for **Translator** only |

---

## Numbers to Remember

| Fact | Value |
|------|-------|
| Free tier label | F0 |
| Standard tier label | S0 |
| Content filter categories | 4 (Hate, Sexual, Violence, Self-harm) |
| Content filter severity levels | 4 (safe, low, medium, high) |
| Max stop sequences (OpenAI) | 4 |
| Passing score | 700/1000 |
| Exam time | 100 minutes |
