# AI-102 Flashcard Deck — Active Recall

> **How to use:** Cover the Answer column. Say your answer OUT LOUD, then check.
> Do 3 passes per day: morning, afternoon, evening. Star (★) the ones you get wrong — those are your priority next pass.
> Shuffle domains each session — don't study one domain front-to-back.

---

## Domain 1: Plan & Manage (20-25%)

| # | Question | Answer |
|---|----------|--------|
| 1 | You need Azure AI Language AND Vision with one key. What resource type? | **Multi-service** resource |
| 2 | You need separate billing per AI service. What resource type? | **Single-service** resource |
| 3 | Which resource type offers a free tier (F0)? | **Single-service** only |
| 4 | What header passes a subscription key to Azure AI APIs? | `Ocp-Apim-Subscription-Key` |
| 5 | What header passes an Entra ID token? | `Authorization: Bearer <token>` |
| 6 | What SDK class auto-handles Entra ID, managed identity, and CLI credentials? | `DefaultAzureCredential` |
| 7 | You're rotating keys with zero downtime. What's the correct order? | App uses Key1 → regen Key2 → switch app to Key2 → regen Key1 |
| 8 | Where should you store Azure AI keys in production? | **Azure Key Vault** |
| 9 | What RBAC role lets you call Azure AI APIs but NOT manage resources? | `Cognitive Services User` |
| 10 | What RBAC role lets you manage AND call Azure OpenAI? | `Cognitive Services OpenAI Contributor` |
| 11 | What's the difference between a Private Endpoint and a Service Endpoint? | Private Endpoint = private IP in your VNet. Service Endpoint = optimized route but still public IP. |
| 12 | Where do Azure AI container images come from? | `mcr.microsoft.com/azure-cognitive-services/` |
| 13 | Do Azure AI containers need internet access? | Yes for billing (phone-home), unless **disconnected container** (special approval) |
| 14 | What deployment type guarantees minimum tokens/minute? | **Provisioned Throughput (PTU)** |
| 15 | Name Microsoft's 6 Responsible AI principles. | Fairness, Reliability & Safety, Privacy & Security, Inclusiveness, Transparency, Accountability |
| 16 | What blocks specific terms YOU define in Azure OpenAI responses? | **Blocklist** (not content filter — that's category-based) |
| 17 | What detects prompt injection / jailbreak attempts? | **Prompt shields** |
| 18 | What checks if an AI response is supported by the provided context? | **Groundedness detection** |
| 19 | Content filters apply to input only, output only, or both? | **Both** input (prompt) AND output (completion) |
| 20 | Name the 4 content filter categories. | Hate, Sexual, Violence, Self-harm |
| 21 | What are the content filter severity levels in order? | safe → low → medium → high |
| 22 | Azure Translator uses what unique endpoint (not resource-specific)? | `api.cognitive.microsofttranslator.com` |
| 23 | What extra header does Azure Translator require? | `Ocp-Apim-Subscription-Region` |

---

## Domain 2: Generative AI (15-20%)

| # | Question | Answer |
|---|----------|--------|
| 24 | What parameter controls randomness? Range? | `temperature`, 0.0–2.0 |
| 25 | What parameter does nucleus sampling? Range? | `top_p`, 0.0–1.0 |
| 26 | Should you set both temperature AND top_p? | **No** — modify one, leave the other at default |
| 27 | Low temperature (0.0–0.3) is best for what? | Deterministic, factual: code generation, data extraction |
| 28 | High temperature (0.7–1.5) is best for what? | Creative: marketing copy, stories, brainstorming |
| 29 | What parameter reduces word repetition based on frequency? | `frequency_penalty` (-2.0 to 2.0) |
| 30 | What parameter encourages new topics? | `presence_penalty` (-2.0 to 2.0) |
| 31 | What is the max number of stop sequences? | **4** |
| 32 | What message role defines the AI's behavior/persona? | `system` |
| 33 | What message role is used for few-shot examples? | `assistant` (paired with user messages) |
| 34 | What's the RAG pattern in one sentence? | User query → Search retrieves docs → docs injected as context → LLM generates grounded answer |
| 35 | In RAG, what service retrieves relevant documents? | **Azure AI Search** (vector/semantic/hybrid) |
| 36 | In RAG, what converts text to vectors? | **Embedding model** (text-embedding-3-small/large) |
| 37 | GPT-4o context window: input? output? | 128K input / 16K output |
| 38 | GPT-4 Turbo context window: input? output? | 128K input / 4K output |
| 39 | What's the Foundry concept for a top-level shared resource? | **Hub** |
| 40 | What's the Foundry concept for an isolated workspace within a hub? | **Project** |
| 41 | What Foundry tool visually builds LLM orchestration pipelines? | **Prompt flow** |
| 42 | Name 4 Foundry evaluation metrics. | Groundedness, Relevance, Coherence, Fluency |
| 43 | DALL-E 3 landscape size? | `1792x1024` |
| 44 | Fine-tuning vs RAG: when use which? | RAG for dynamic/updatable data grounding. Fine-tuning for teaching style/format/domain behavior. |
| 45 | What's the minimum fine-tuning data? | 10 examples (50-100+ recommended) |
| 46 | Fine-tuning data format? | JSONL with `{"messages": [...]}` per line |
| 47 | What Azure OpenAI feature does quick RAG with no code? | **"On your data"** feature |

---

## Domain 3: Agentic Solutions (5-10%)

| # | Question | Answer |
|---|----------|--------|
| 48 | Agent vs chatbot: key difference? | Agent plans, uses tools, and iterates autonomously. Chatbot just responds. |
| 49 | Name 3 built-in Foundry Agent Service tools. | Code Interpreter, File Search, Function Calling (also: Azure AI Search, Bing) |
| 50 | What Agent API object holds the conversation? | **Thread** |
| 51 | What Agent API object triggers the agent to process a thread? | **Run** |
| 52 | Name 3 multi-agent orchestration patterns. | Sequential, Parallel, Hierarchical, Collaborative (any 3) |
| 53 | In Semantic Kernel, what auto-creates a plan from available functions? | **Planner** |
| 54 | What's a Semantic Kernel Plugin? | A collection of functions the agent can call |

---

## Domain 4: Computer Vision (10-15%)

| # | Question | Answer |
|---|----------|--------|
| 55 | Name all 7 Image Analysis 4.0 visual features. | caption, denseCaptions, tags, objects, people, smartCrops, read |
| 56 | Which Face detection model is best for blurry/small faces? | `detection_02` (or `detection_03` for best overall) |
| 57 | Which detection model returns NO landmarks and NO attributes? | `detection_02` |
| 58 | Which is the best overall face detection model? | `detection_03` |
| 59 | Which is the best face recognition model? What's special about it? | `recognition_04` — works with face masks |
| 60 | Default detection model if unspecified? | `detection_01` |
| 61 | Default recognition model if unspecified? | `recognition_01` |
| 62 | Can you change a PersonGroup's recognition model after creation? | **No** — immutable once created |
| 63 | What `qualityForRecognition` level is needed for enrollment? | **High** |
| 64 | What level for identification? | **Medium** or better |
| 65 | Face ID time-to-live default? | 86400 seconds (24 hours) |
| 66 | What parameter extends how long a face ID is kept? | `faceIdTimeToLive` |
| 67 | Custom Vision: classification minimum images per tag? | **5** (30+ recommended) |
| 68 | Custom Vision: object detection minimum images per tag? | **15** (50+ recommended) |
| 69 | Precision measures what? | Of predicted positives, how many are correct (false positive rate) |
| 70 | Recall measures what? | Of actual positives, how many were found (false negative rate) |
| 71 | High precision + low recall means what? | Model is rarely wrong when it says "yes", but misses many actual positives |
| 72 | What does Spatial Analysis run on? | **Edge devices** (Docker container, NVIDIA GPU, RTSP video) |
| 73 | Can Custom Vision compact models be exported? To what? | Yes — ONNX, TensorFlow, CoreML (for mobile/edge) |
| 74 | Minimum detectable face size? | **36 × 36 pixels** |
| 75 | Does Face recognition require limited access approval? | **Yes** — apply via intake form |

---

## Domain 5: NLP & Speech (15-20%)

| # | Question | Answer |
|---|----------|--------|
| 76 | What values can sentiment analysis return? | positive, negative, neutral, mixed |
| 77 | Does key phrase extraction return confidence scores? | **No** — just strings |
| 78 | What feature links entities to Wikipedia? | **Entity Linking** |
| 79 | PII detection returns `redactedText` — what are entities replaced with? | `*****` (asterisks) |
| 80 | For language detection, do you need to specify `language` in the request? | **No** — it auto-detects (that's the whole point) |
| 81 | Max document size for Text Analytics? | **5,120 characters** |
| 82 | Azure Translator: can you translate to multiple languages in one call? | **Yes** — `to=fr&to=de&to=ja` |
| 83 | What's transliteration? | Converting script (e.g., Japanese kanji → romaji), not translating meaning |
| 84 | Custom Translator minimum parallel sentences? | **10,000+** for good quality |
| 85 | What SDK class for speech-to-text? | `SpeechRecognizer` |
| 86 | What SDK class for text-to-speech? | `SpeechSynthesizer` |
| 87 | What SSML tag controls rate, pitch, volume? | `<prosody>` |
| 88 | What SSML tag adds a pause? | `<break time="500ms"/>` |
| 89 | What SSML tag makes "12/25" read as a date? | `<say-as interpret-as="date">` |
| 90 | What SSML tag adds emotional style like "cheerful"? | `<mstts:express-as style="cheerful">` |
| 91 | What SDK class for speech + intent recognition? | `IntentRecognizer` (not SpeechRecognizer) |
| 92 | What SDK class for speech translation? | `TranslationRecognizer` |
| 93 | In CLU, what is an "intent"? Give an example. | What the user wants to do. E.g., `BookFlight` |
| 94 | In CLU, what is an "entity"? Give an example. | A parameter/detail. E.g., `destination=Paris` |
| 95 | Name the 4 CLU entity types. | Learned, List, Prebuilt, Regex |
| 96 | What CLU intent must you always include? | **None** intent (catches out-of-scope input) |
| 97 | Question Answering: what's multi-turn? | Follow-up prompts to drill deeper ("Did you mean A or B?") |
| 98 | Name the chit-chat personality options. | Professional, Friendly, Witty, Caring, Enthusiastic |
| 99 | Speech-to-speech translation requires what extra config? | Add `voice_name` for target language to get synthesized output |

---

## Domain 6: Knowledge Mining (15-20%)

| # | Question | Answer |
|---|----------|--------|
| 100 | What are the 5 core Azure AI Search components? | Data source, Indexer, Skillset, Index, Knowledge Store |
| 101 | What's a skillset? | Pipeline of AI enrichment skills applied during indexing |
| 102 | `fieldMappings` vs `outputFieldMappings`? | fieldMappings = source → index (before enrichment). outputFieldMappings = enrichment output → index (after skillset) |
| 103 | What type of Azure resource implements a custom skill? | **Azure Function** (HTTP trigger, Web API skill) |
| 104 | Custom skill response MUST include what 3 fields per record? | `data`, `errors`, `warnings` |
| 105 | Custom skill `recordId` in response must do what? | **Match** the `recordId` from the request |
| 106 | What field attribute is needed for `$filter`? | `filterable` |
| 107 | What field attribute is needed for `$orderby`? | `sortable` |
| 108 | What field attribute is needed for faceted navigation? | `facetable` |
| 109 | Simple query: space between terms means what? | **OR** |
| 110 | How do you enable full Lucene query syntax? | `queryType=full` |
| 111 | Lucene `~2` after a term means what? | Fuzzy search with edit distance 2 |
| 112 | `$filter=rooms/any(r: r/baseRate lt 100)` does what? | Filters to documents where ANY room has baseRate < 100 |
| 113 | `searchMode=all` vs `searchMode=any`? | `all` = AND (all terms must match). `any` = OR (default) |
| 114 | Name the 4 scoring profile function types. | magnitude, freshness, distance, tag |
| 115 | Which interpolations are NOT allowed for tag scoring? | `quadratic` and `logarithmic` |
| 116 | What's a suggester's only searchMode value? | `analyzingInfixMatching` |
| 117 | Knowledge store table projections go where? | **Azure Table Storage** |
| 118 | Knowledge store object projections go where? | **Azure Blob Storage** (JSON) |
| 119 | What skill reshapes data for knowledge store projections? | **Shaper** skill |
| 120 | HNSW vs Exhaustive KNN? | HNSW = fast approximate (default). Exhaustive KNN = slow exact (brute force, 100% recall) |
| 121 | Best vector metric for most text embeddings? | `cosine` |
| 122 | Document Intelligence: prebuilt invoice model ID? | `prebuilt-invoice` |
| 123 | Minimum training docs for custom Document Intelligence? | **5** labeled documents |
| 124 | What's a composed model in Document Intelligence? | Combines multiple custom models; auto-routes to the right sub-model |
| 125 | Template model vs Neural model? | Template = fixed layout. Neural = variable layout (different vendors/formats). |
| 126 | Semantic search provides what two extras beyond results? | **Semantic captions** and **semantic answers** |
| 127 | What query parameter triggers semantic search? | `queryType=semantic&semanticConfiguration=<name>` |
| 128 | How does hybrid search combine text + vector results? | **Reciprocal Rank Fusion (RRF)** |
| 129 | What's the `@odata.type` for a custom Web API skill? | `#Microsoft.Skills.Custom.WebApiSkill` |
| 130 | Context path for extracted images in a skillset? | `/document/normalized_images/*` |
