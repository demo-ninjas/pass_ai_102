# Cheat Sheet 2: Implement Generative AI Solutions (15–20%)

---

## Azure OpenAI Models — Know Which To Pick

| Model | Best For | Context Window |
|-------|----------|----------------|
| **GPT-4o** | Multimodal (text + images + audio), fastest GPT-4 class | 128K input / 16K output |
| **GPT-4o-mini** | Fast, cheap replacement for GPT-3.5 | 128K input / 16K output |
| **GPT-4** | Complex reasoning, highest accuracy | 128K input / 4K output |
| **GPT-4 Turbo** | Faster/cheaper than GPT-4, with vision | 128K input / 4K output |
| **GPT-3.5-Turbo** | Legacy; fast, cheap, simple tasks | 16K input / 4K output |
| **gpt-image-1** | Image generation from text (replaces DALL-E) | N/A |
| **Whisper** | Audio transcription and translation | 25 MB audio file |
| **text-embedding-ada-002** | Text embeddings (legacy) | 8K tokens |
| **text-embedding-3-small** | Text embeddings (newer, cheaper) | 8K tokens |
| **text-embedding-3-large** | Text embeddings (newer, best quality) | 8K tokens |

---

## API Parameters — Exact Names and Ranges

| Parameter | What It Controls | Default | Range |
|-----------|-----------------|---------|-------|
| `temperature` | Randomness/creativity | 1 | 0.0 – 2.0 |
| `top_p` | Nucleus sampling (cumulative probability) | 1 | 0.0 – 1.0 |
| `max_tokens` | Maximum output length | Model-dependent | 1 – model max |
| `frequency_penalty` | Penalize repeated tokens (based on frequency) | 0 | -2.0 – 2.0 |
| `presence_penalty` | Penalize tokens that appeared at all (encourage new topics) | 0 | -2.0 – 2.0 |
| `stop` | Stop generating at these sequences | null | Up to 4 strings |
| `n` | Number of completions to generate | 1 | 1 – 128 |
| `logprobs` | Return log probabilities of tokens | false | true/false |

### Parameter Cheat Rules
- **Low temperature (0.0–0.3)** = deterministic, factual → use for code, data extraction
- **High temperature (0.7–1.5)** = creative, varied → use for marketing copy, stories
- **Don't set both** `temperature` AND `top_p` — pick one to modify
- **frequency_penalty > 0** = reduce word repetition
- **presence_penalty > 0** = encourage covering new topics

---

## Chat Completions API — Request Format

```json
POST https://<resource>.openai.azure.com/openai/deployments/<deployment>/chat/completions?api-version=2024-02-01

{
  "messages": [
    {"role": "system", "content": "You are a helpful assistant."},
    {"role": "user", "content": "What is Azure?"},
    {"role": "assistant", "content": "Azure is Microsoft's cloud platform."},
    {"role": "user", "content": "Tell me more about AI services."}
  ],
  "temperature": 0.7,
  "max_tokens": 800
}
```

### Message Roles
| Role | Purpose | Exam Tip |
|------|---------|----------|
| `system` | Sets behavior, persona, constraints, grounding data | First message; defines the AI's behavior |
| `user` | The human's input | What the end-user types |
| `assistant` | AI's previous responses | Also used to provide **few-shot examples** |
| `tool` / `function` | Tool/function call results | For function calling / agents |

---

## Prompt Engineering Techniques

| Technique | Description | Example |
|-----------|-------------|---------|
| **Zero-shot** | No examples, just instruction | "Classify this as positive or negative" |
| **One-shot** | One example provided | "positive: I love it! → Now classify: ..." |
| **Few-shot** | Multiple examples | 3-5 examples in the prompt |
| **Chain-of-thought** | Ask model to reason step-by-step | "Think step by step before answering" |
| **Grounding** | Provide reference docs in prompt | "Based on the following article: ..." |
| **Meta-prompting** | Prompt about how to prompt | "You are an expert prompt engineer..." |

### Prompt Engineering Best Practices
- Be **specific** and **clear** in instructions
- Use **delimiters** (```, """, ---) to separate instructions from content
- Specify the **output format** (JSON, bullet points, etc.)
- Provide **examples** of desired output
- Use **system message** for persistent behavior rules

---

## RAG (Retrieval-Augmented Generation) Pattern

```
User query
    ↓
Azure AI Search (vector/semantic/hybrid search)
    ↓
Top K relevant documents retrieved
    ↓
Documents injected into system message as context
    ↓
Azure OpenAI generates response grounded in documents
    ↓
Response returned to user
```

### RAG Key Components
| Component | Service | Role |
|-----------|---------|------|
| **Knowledge store** | Azure AI Search | Index your documents |
| **Embedding model** | Azure OpenAI (text-embedding-3-*) | Convert text → vectors |
| **Vector search** | Azure AI Search | Find similar documents by vector |
| **Semantic ranker** | Azure AI Search | Re-rank results by semantic relevance |
| **LLM** | Azure OpenAI (GPT-4o) | Generate answer from context |

### "On Your Data" Feature
- Built-in Azure OpenAI feature for quick RAG setup
- Connects directly to: Azure AI Search, Azure Blob Storage, uploaded files
- No code required — configure in Azure OpenAI Studio
- Automatically cites sources in response

---

## Microsoft Foundry (AI Foundry)

### Key Concepts
| Concept | What It Is |
|---------|------------|
| **Hub** | Top-level resource; shared infrastructure (compute, connections, policies) |
| **Project** | A workspace within a hub; isolated for a team/use case |
| **Connections** | Links to Azure OpenAI, AI Search, storage, etc. |
| **Prompt flow** | Visual tool to build LLM orchestration pipelines |
| **Evaluation** | Measure model/flow quality (groundedness, relevance, coherence, fluency) |
| **Tracing** | Track requests through the pipeline for debugging |
| **Content safety** | Built-in content filtering configuration |

### Prompt Flow Node Types
| Node | Purpose |
|------|---------|
| **LLM** | Call a language model |
| **Python** | Run custom Python code |
| **Prompt** | Define a prompt template with variables |
| **Tool** | Call an external tool/API |

### Evaluation Metrics
| Metric | What It Measures |
|--------|-----------------|
| **Groundedness** | Is the answer supported by the provided context? |
| **Relevance** | Does the answer address the question? |
| **Coherence** | Is the answer logically consistent and well-structured? |
| **Fluency** | Is the answer grammatically correct and natural? |
| **Similarity** | How close is the answer to the ground truth? |

---

## Image Generation (DALL-E / gpt-image-1)

> Note: DALL-E 3 was the original model. Azure has transitioned to `gpt-image-1` series. Know BOTH names for the exam.

```json
POST https://<resource>.openai.azure.com/openai/deployments/<model>/images/generations?api-version=2024-02-01

{
  "prompt": "A sunset over mountains in watercolor style",
  "n": 1,
  "size": "1024x1024"
}
```

### Image Sizes (DALL-E 3)
- `1024x1024` (square)
- `1792x1024` (landscape)
- `1024x1792` (portrait)

### Image Quality
- `standard` — faster, cheaper
- `hd` — more detailed, better consistency

---

## Fine-Tuning

| Aspect | Detail |
|--------|--------|
| **When** | When prompt engineering alone isn't enough for your use case |
| **Format** | JSONL file with `{"messages": [...]}` per line |
| **Min data** | 10 examples minimum (50-100+ recommended) |
| **Models** | GPT-4o, GPT-4o-mini, GPT-4.1 series, GPT-3.5-Turbo (fine-tunable) |
| **Result** | Custom model deployed as a new deployment |
| **Cost** | Training cost + hosting cost (higher than base model) |

**Exam tip:** Fine-tuning ≠ RAG. Use RAG for grounding in dynamic data. Use fine-tuning for teaching the model a specific style/format/domain behavior.

---

## Deployment & Operationalization

### Model Deployment Types
| Type | Capacity | Use Case |
|------|----------|----------|
| **Standard** | Shared, pay-per-token | General use |
| **Global Standard** | Multi-region, highest availability | Global apps |
| **Provisioned (PTU)** | Reserved tokens/min | Guaranteed throughput |
| **Data Zone** | Data stays in geography | Compliance |

### Monitoring
- **Azure Monitor** — metrics and logs
- **Application Insights** — end-to-end tracing
- **Diagnostic settings** — route to Log Analytics
- **Content Safety dashboard** — view filter activations
- **Token usage metrics** — track consumption

### Container Deployment (Edge/Local)
- Deploy Azure OpenAI models in containers for edge/disconnected scenarios
- Requires special approval for disconnected containers
- Still needs periodic billing connection unless fully disconnected
