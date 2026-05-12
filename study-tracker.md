# AI-102 — 6-Day Cram Plan

> Adapt to your own schedule — day 1 = 6 days before exam, day 6 = exam day.
> **METHOD:** Each day has 3 phases: READ → DRILL → RECALL.
> Use the [interactive app](https://demo-ninjas.github.io/pass_ai_102/) for flashcards and quizzes.

---

## Day 1: Plan & Manage Azure AI (20–25%)
This is the highest-weighted domain. Nail it.

### READ (cheat-sheets/1-plan-manage.md)
- [ ] Know when to pick each service (Vision vs Language vs Speech vs OpenAI vs Search vs Doc Intelligence)
- [ ] Multi-service vs single-service Azure AI resource — when to use each
- [ ] Authentication: keys, Entra ID (Azure AD), managed identities
- [ ] Key rotation and Key Vault integration
- [ ] Monitoring: diagnostic logs, metrics, cost management
- [ ] Container deployment for Azure AI services
- [ ] Responsible AI: content filters, blocklists, prompt shields, harm detection
- [ ] Content Safety service

### DRILL
**Do:** [MS Learn - Plan and Prepare to Develop AI Solutions](https://learn.microsoft.com/en-us/training/modules/prepare-azure-ai-development/)  
**Do:** [MS Learn - Secure Azure AI Services](https://learn.microsoft.com/en-us/training/modules/secure-cognitive-services/)  
**Do:** Take the [practice assessment](https://learn.microsoft.com/en-us/credentials/certifications/azure-ai-engineer/practice/assessment?assessment-type=practice&assessmentId=61&practice-assessment-type=certification) once to see your baseline

### RECALL (evening — 30 min)
- [ ] Open the app → Flashcards → filter **Plan & Manage** → drill all cards
- [ ] Star ★ the ones you got wrong
- [ ] Read traps-and-distinctions.md sections 1, 2, 5

---

## Day 2: Generative AI & Azure OpenAI (15–20%)
Second-highest weight. Very testable.

### READ (cheat-sheets/2-generative-ai.md)
- [ ] Provisioning Azure OpenAI resource
- [ ] Model deployment options (Standard, Global, Provisioned Throughput)
- [ ] Model selection: GPT-4o, GPT-4o-mini, GPT-4, DALL-E/gpt-image-1, Whisper
- [ ] API parameters: temperature, top_p, max_tokens, frequency_penalty, presence_penalty, stop sequences
- [ ] System message, user message, assistant message roles
- [ ] Prompt engineering: few-shot, chain-of-thought, grounding
- [ ] RAG pattern (Retrieval-Augmented Generation) — how it works with Azure AI Search + OpenAI
- [ ] Microsoft Foundry: hubs, projects, prompt flow, evaluations
- [ ] Fine-tuning concepts
- [ ] Image generation

### DRILL
**Do:** [MS Learn - Get started with Azure OpenAI](https://learn.microsoft.com/en-us/training/modules/get-started-openai/)  
**Do:** [MS Learn - Apply prompt engineering](https://learn.microsoft.com/en-us/training/modules/apply-prompt-engineering-azure-openai/)  
**Do:** [MS Learn - Implement RAG with Azure OpenAI](https://learn.microsoft.com/en-us/training/modules/use-own-data-azure-openai/)

### RECALL (evening — 30 min)
- [ ] Open the app → Flashcards → filter **Generative AI** → drill all cards
- [ ] Re-do ★ starred cards from Day 1
- [ ] Read traps-and-distinctions.md section 2 (Parameter Traps)

---

## Day 3: NLP — Language & Speech (15–20%)
Lots of services to know here.

### READ (cheat-sheets/5-nlp.md)
- [ ] Text Analytics: sentiment, key phrases, entities, language detection, PII detection
- [ ] Azure Translator: text translation, document translation, custom translator
- [ ] Speech-to-text (STT) and text-to-speech (TTS)
- [ ] SSML (Speech Synthesis Markup Language) — know the XML tags
- [ ] Custom speech models
- [ ] Intent recognition, keyword recognition
- [ ] Speech translation (speech-to-speech, speech-to-text)
- [ ] CLU (Conversational Language Understanding): intents, entities, utterances
- [ ] Custom question answering: QnA pairs, sources, multi-turn, chit-chat
- [ ] Training → evaluating → deploying → consuming language models

### DRILL
**Do:** [MS Learn - Analyze text](https://learn.microsoft.com/en-us/training/modules/analyze-text-with-text-analytics-service/)  
**Do:** [MS Learn - Create speech-enabled apps](https://learn.microsoft.com/en-us/training/modules/transcribe-speech-input-text/)  
**Do:** [MS Learn - Build CLU model](https://learn.microsoft.com/en-us/training/modules/build-language-understanding-model/)  
**Do:** [MS Learn - Create question answering solution](https://learn.microsoft.com/en-us/training/modules/create-question-answer-solution-ai-language/)

### RECALL (evening — 30 min)
- [ ] Open the app → Flashcards → filter **NLP & Speech** → drill all cards
- [ ] Re-do ALL ★ starred cards from Days 1-2
- [ ] Read traps-and-distinctions.md sections 7, 3 (NLP + Endpoint traps)

---

## Day 4: Knowledge Mining & Document Intelligence (15–20%)

### READ (cheat-sheets/6-knowledge-mining.md)
- [ ] Azure AI Search architecture: data source → indexer → skillset → index
- [ ] Built-in skills (OCR, entity recognition, key phrase extraction, etc.)
- [ ] Custom skills (Azure Function as a skill in the pipeline)
- [ ] Knowledge Store: projections (table, object, file)
- [ ] Index schema: fields, types, attributes (searchable, filterable, sortable, facetable)
- [ ] Query syntax: simple vs full Lucene, `$filter`, `$orderby`, `$select`, `search=*`
- [ ] Semantic search and vector search
- [ ] Document Intelligence: prebuilt models (invoice, receipt, ID document, business card, W-2)
- [ ] Custom models: train, test, publish
- [ ] Composed models (combine multiple custom models)
- [ ] Azure Content Understanding: OCR pipelines, document summarization, entity extraction

### DRILL
**Do:** [MS Learn - Create an Azure AI Search solution](https://learn.microsoft.com/en-us/training/modules/create-azure-cognitive-search-solution/)  
**Do:** [MS Learn - Create a custom skill](https://learn.microsoft.com/en-us/training/modules/create-enrichment-pipeline-azure-cognitive-search/)  
**Do:** [MS Learn - Extract data from forms](https://learn.microsoft.com/en-us/training/modules/work-form-recognizer/)

### RECALL (evening — 30 min)
- [ ] Open the app → Flashcards → filter **Knowledge Mining** → drill all cards
- [ ] Re-do ALL ★ starred cards from Days 1-3
- [ ] Read traps-and-distinctions.md sections 4, 8, 9 (Search, Doc Intelligence, Skillset traps)

---

## Day 5: Computer Vision + Agents + Full Recall

### READ (cheat-sheets/4-computer-vision.md + cheat-sheets/3-agentic.md)

#### Computer Vision (10–15%)
- [ ] Image Analysis 4.0: tags, objects, captions, dense captions, people, smart crops
- [ ] OCR (Read API) for printed and handwritten text
- [ ] Face API: detection_01/02/03, recognition_01-04 (MEMORIZE the table)
- [ ] Custom Vision: classification vs object detection, training, publishing, prediction
- [ ] Custom Vision metrics: precision, recall, AP (average precision)
- [ ] Video Indexer: faces, topics, keywords, sentiment, scenes, OCR from video
- [ ] Spatial Analysis: people counting, distance, zone dwell time

#### Agentic Solutions (5–10%)
- [ ] What is an AI agent, use cases
- [ ] Microsoft Foundry Agent Service (Thread, Run, Agent objects)
- [ ] Microsoft Agent Framework (Semantic Kernel concepts)
- [ ] Multi-agent orchestration patterns
- [ ] Testing and deploying agents

### DRILL
**Do:** [MS Learn - Analyze images](https://learn.microsoft.com/en-us/training/modules/analyze-images/)  
**Do:** [MS Learn - Read text with Vision](https://learn.microsoft.com/en-us/training/modules/read-text-images-documents-with-computer-vision-service/)  
**Do:** Re-take the [practice assessment](https://learn.microsoft.com/en-us/credentials/certifications/azure-ai-engineer/practice/assessment?assessment-type=practice&assessmentId=61&practice-assessment-type=certification)
- [ ] Review all wrong answers — read the explanations carefully

### RECALL (evening — FULL DECK)
- [ ] Open the app → Flashcards → filter **Computer Vision**, then **Agentic** → drill cards
- [ ] Then switch to **All** → run through the entire deck — time yourself
- [ ] Re-do ALL ★ starred cards until you get them right
- [ ] Read traps-and-distinctions.md sections 1, 6, 10 (Service confusions, Lifecycle, Numbers)

---

## Day 6 (Exam Day Eve / Morning)

- [ ] Read traps-and-distinctions.md ONLY — cover to cover (this is your highest-ROI file)
- [ ] Run through ★ starred flashcards one final time
- [ ] Skim REST API endpoint patterns in cheat-sheets/1-plan-manage.md
- [ ] Review practice assessment wrong answers one last time
- [ ] **NO new material** — only reinforce what you already studied
- [ ] Get good sleep, eat well, arrive early

---

## High-Yield Exam Tips

1. **Service selection questions are guaranteed** — know which service does what
2. **REST API patterns come up** — know endpoint formats for each service
3. **Responsible AI is a recurring theme** — content filters, blocklists, prompt shields
4. **RAG pattern is heavily tested** — understand Azure AI Search + OpenAI integration
5. **Know the difference** between prebuilt and custom models for Vision, Language, and Document Intelligence
6. **SSML syntax** appears in questions — know `<speak>`, `<voice>`, `<prosody>`, `<break>`
7. **Azure AI Search** — know indexer/skillset/index pipeline cold
8. **Parameters matter** — temperature vs top_p, what each does to output
9. **Read all answer choices** before selecting — many are close but differ in one detail
10. **Flag and return** to hard questions — don't burn time
