# AI-102 Exam Traps & Tricky Distinctions

> These are the questions designed to trip you up. The wrong answers SOUND right.
> Study this file the night before the exam.

---

## 1. "Sounds the Same" Service Confusions

### Blocklist vs Content Filter vs Prompt Shield
| Feature | What It Does | When the Exam Asks |
|---------|-------------|-------------------|
| **Content filter** | Blocks by CATEGORY (hate/sexual/violence/self-harm) at severity levels | "Filter inappropriate content by category" |
| **Blocklist** | Blocks SPECIFIC TERMS you define | "Block the word [specific-term] from responses" |
| **Prompt shield** | Detects PROMPT INJECTION / jailbreak attempts | "Prevent users from manipulating the system message" |
| **Groundedness detection** | Checks if response is SUPPORTED BY CONTEXT | "Ensure the model only uses provided documents" |

**Trap:** "You need to prevent the model from saying competitor names" → **Blocklist** (not content filter — there's no "competitor" category).

### Face Detection Models — The Classic Trap
| Scenario | Answer | Why NOT the other? |
|----------|--------|-------------------|
| "Blurry images" | `detection_02` or `detection_03` | `detection_01` is NOT optimized for blur |
| "Need face landmarks from blurry images" | `detection_03` ONLY | `detection_02` handles blur but returns NO landmarks |
| "Best overall detection" | `detection_03` | `detection_02` is good for blur but limited features |
| "Need all attributes including mask detection" | `detection_03` | `detection_01` has most attributes but NOT mask |
| "Default behavior when model not specified" | `detection_01` + `recognition_01` | These are defaults but NOT the best |
| "Can I change a PersonGroup's recognition model?" | **No, never** | Must recreate the group |

### Image Analysis vs Face API vs Custom Vision
| Need | Service | NOT |
|------|---------|-----|
| "Detect people in an image" (just bounding boxes) | **Image Analysis 4.0** (`people` feature) | Not Face API (that's for identity) |
| "Identify WHO a person is" | **Face API** (Identify) | Not Image Analysis (no identity) |
| "Is this image a cat or dog?" | **Custom Vision** (classification) | Not Image Analysis (it does tags, not your custom categories) |
| "WHERE are the cats in this image?" | **Custom Vision** (object detection) | Not Image Analysis objects (those are generic, not trained on your data) |
| "Extract text from a scanned document" | **Image Analysis Read API** or **Document Intelligence** | Not Custom Vision |

---

## 2. Parameter Traps

### temperature vs top_p
- Both control randomness — **never set both**
- Exam will offer answers that set both → those are WRONG
- `temperature=0` = completely deterministic
- `top_p=1` = consider all tokens (default, no restriction)

### frequency_penalty vs presence_penalty
| Parameter | Meaning | Positive Value Effect |
|-----------|---------|----------------------|
| `frequency_penalty` | Penalizes tokens based on HOW OFTEN they appeared | Reduces repetitive phrases |
| `presence_penalty` | Penalizes tokens based on WHETHER they appeared AT ALL | Encourages new topics |

**Trap:** "Model keeps repeating the same paragraph" → `frequency_penalty`. "Model won't explore new topics" → `presence_penalty`.

### faceIdTimeToLive
- Default: **86400 seconds** (24 hours)
- Exam may ask: "Face IDs expire after [time]. What should you change?" → increase `faceIdTimeToLive`
- Or: "You need face IDs to persist longer for a multi-step process" → same answer

---

## 3. API Endpoint Traps

### Translator is DIFFERENT from all other services
| Other AI Services | Translator |
|-------------------|-----------|
| `https://<resource>.cognitiveservices.azure.com/` | `https://api.cognitive.microsofttranslator.com/` |
| Only needs `Ocp-Apim-Subscription-Key` header | ALSO needs `Ocp-Apim-Subscription-Region` header |

**Trap:** Any answer that puts Translator at a `.cognitiveservices.azure.com` URL is WRONG.

### Azure OpenAI endpoint includes deployment name
```
https://<resource>.openai.azure.com/openai/deployments/<DEPLOYMENT-NAME>/chat/completions
```
- NOT the model name — the **deployment** name
- A single model can have multiple deployments with different settings

---

## 4. Search Query Traps

### Simple vs Full Lucene
| Syntax | Simple? | Full Lucene? |
|--------|---------|-------------|
| `search=pool wifi` (space = OR) | ✅ | ✅ |
| `search="ocean view"` (phrase) | ✅ | ✅ |
| `search=hotel~2` (fuzzy) | ❌ | ✅ |
| `search=ho*` (wildcard) | ❌ | ✅ |
| `search=title:pool` (field-scoped) | ❌ | ✅ |
| `search=/[a-z]+/` (regex) | ❌ | ✅ |

**Trap:** If a question uses fuzzy, wildcard, field-scoped, or regex syntax, the answer MUST include `queryType=full`.

### $filter vs search
- `search` = full-text search (analyzed, tokenized, relevance-scored)
- `$filter` = exact match / comparison (not analyzed, boolean result)
- `$filter=city eq 'Seattle'` — exact string match, case-sensitive
- `search=Seattle` — will match "Seattle", "seattle", "SEATTLE" etc.

**Trap:** "Find hotels where city is exactly 'Seattle'" → `$filter`, not `search`.

### searchMode=any vs searchMode=all
- `searchMode=any` (default): `search=pool wifi` → matches docs with pool OR wifi
- `searchMode=all`: `search=pool wifi` → matches docs with pool AND wifi

**Trap:** "Return only documents containing ALL search terms" → `searchMode=all`

---

## 5. Authentication Traps

### When to use what
| Scenario | Answer | Why NOT the other? |
|----------|--------|-------------------|
| "Quick prototype, simplest auth" | Subscription key | Not Entra ID (overkill) |
| "Production app, no credentials in code" | Managed Identity + `DefaultAzureCredential` | Not subscription key (insecure) |
| "Secure key storage with rotation" | Azure Key Vault | Not app settings (not secure enough) |
| "RBAC-based access control" | Entra ID (Azure AD) | Not keys (keys = all-or-nothing) |

### Key Rotation — the EXACT order matters
1. App uses **Key 1**
2. Regenerate **Key 2** (safe — app doesn't use it)
3. Switch app to **Key 2**
4. NOW regenerate **Key 1** (safe — app now uses Key 2)

**Trap:** Any answer that regenerates the key the app is currently using = **WRONG** (causes downtime).

---

## 6. Model/Resource Lifecycle Traps

### Can't change after creation
| Thing | Immutable? |
|-------|-----------|
| PersonGroup recognition model | ✅ Can't change |
| FaceList recognition model | ✅ Can't change |
| AI Search index key field | ✅ Can't change |
| AI Search similarity algorithm | ✅ Can't change (set at creation) |
| Custom Vision project type (classify/detect) | ❌ CAN change |
| Scoring profile on an index | ❌ CAN add/update without rebuild |

### Requires rebuild vs doesn't
| Change | Rebuild? |
|--------|----------|
| Add a scoring profile | No |
| Add a synonym map | No |
| Add a suggester to existing fields | **Yes** (prefixes must be re-indexed) |
| Add a new field to index | No (just add it) |
| Change a field's analyzer | **Yes** |
| Add a new data source | No |

---

## 7. NLP Traps

### CLU vs Custom Question Answering — When to use which
| Scenario | Service |
|----------|---------|
| "User says 'Book a flight to Paris'" → extract intent + entities | **CLU** |
| "User asks 'How do I reset my password?'" → return FAQ answer | **Custom Question Answering** |
| "Build a chatbot that understands commands" | **CLU** |
| "Build a FAQ bot from existing documentation" | **Custom Question Answering** |

### Language Detection gotcha
- Input does NOT need a `language` field (it's detecting the language!)
- Returns `unknown` with confidence `1.0` for ambiguous content (not `0.0`!)

### SSML namespace trap
- `<mstts:express-as>` requires the Microsoft namespace: `xmlns:mstts="http://www.w3.org/2001/mstts"`
- If the namespace is missing in a code snippet, the SSML is invalid

---

## 8. Document Intelligence Traps

### Prebuilt vs Custom
| Scenario | Answer |
|----------|--------|
| "Extract data from any vendor's invoices" | **Prebuilt invoice** (handles variable layouts) |
| "Extract data from YOUR company's unique form" | **Custom model** |
| "Multiple custom form types, one endpoint" | **Composed model** |
| "Forms with fixed, identical layouts" | **Custom template** model |
| "Forms with variable layouts from different sources" | **Custom neural** model |

**Trap:** "Invoices from multiple vendors with different formats" → this is still **prebuilt-invoice** (it handles varied layouts). Only use custom if the prebuilt doesn't extract what you need.

---

## 9. Skillset Traps

### fieldMappings vs outputFieldMappings
| Mapping | When | Direction |
|---------|------|-----------|
| `fieldMappings` | BEFORE enrichment | Source data → index field |
| `outputFieldMappings` | AFTER enrichment | Enrichment output → index field |

**Trap:** "Map the OCR output to an index field" → `outputFieldMappings` (it's an enrichment output, not source data).

### Custom skill input/output context
- `/document` = the whole document
- `/document/content` = the text content
- `/document/normalized_images/*` = each image (need `/*` for arrays)
- `/document/pages/*` = each page after text split

---

## 10. Numbers That Trip People Up

| What | Number | Trap |
|------|--------|------|
| Custom Vision min (classify) | 5 per tag | Docs recommend 30+; exam may test the minimum |
| Custom Vision min (detect) | 15 per tag | Not 5 — detection needs more |
| Doc Intelligence min training | 5 labeled documents | Not 10, not 50 |
| Fine-tuning min examples | 10 | 50-100+ recommended |
| Face min detectable size | 36 × 36 px | Not 64 × 64 |
| Composed model max components | 200 | |
| Content filter severity levels | 4 (safe/low/medium/high) | Not 3, not 5 |
| Scoring profiles per index max | 100 | |
| Max stop sequences | 4 | Not 1, not unlimited |
| Text Analytics max doc size | 5,120 chars | Not 5,000 or 10,000 |
