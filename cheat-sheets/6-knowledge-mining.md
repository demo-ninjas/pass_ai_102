# Cheat Sheet 6: Knowledge Mining & Information Extraction (15–20%)

---

## Azure AI Search — Architecture

```
Data Source → Indexer → [Skillset (AI enrichment)] → Index → Queries
                                  ↓
                           Knowledge Store
                        (tables, objects, files)
```

### Core Components — Know Each One
| Component | What It Is | Key Detail |
|-----------|------------|------------|
| **Data source** | Where your data lives | Blob, SQL, Cosmos DB, Table Storage |
| **Indexer** | Crawler that reads data source and populates index | Runs on schedule or on-demand |
| **Skillset** | Pipeline of AI enrichment steps | Built-in + custom skills |
| **Index** | The searchable data store | Schema of fields with attributes |
| **Knowledge store** | Side output of enriched data for analytics | Table, object, or file projections |

---

## Data Sources (exact supported types)

| Source | Type String |
|--------|-------------|
| Azure Blob Storage | `azureblob` |
| Azure Data Lake Storage Gen2 | `adlsgen2` |
| Azure Table Storage | `azuretable` |
| Azure SQL Database | `azuresql` |
| SQL Server on Azure VMs | `azuresql` |
| Azure Cosmos DB (SQL API) | `cosmosdb` |
| Azure Cosmos DB (MongoDB API) | `cosmosdb` |
| Azure MySQL | `mysql` |

### Data Source Connection String
```json
{
  "name": "my-datasource",
  "type": "azureblob",
  "credentials": { "connectionString": "<storage-connection-string>" },
  "container": { "name": "my-container", "query": "subfolder/" }
}
```

---

## Indexer

### Indexer Schedule
```json
{
  "name": "my-indexer",
  "dataSourceName": "my-datasource",
  "targetIndexName": "my-index",
  "skillsetName": "my-skillset",
  "schedule": { "interval": "PT2H" },
  "fieldMappings": [
    { "sourceFieldName": "metadata_storage_name", "targetFieldName": "title" }
  ],
  "outputFieldMappings": [
    { "sourceFieldName": "/document/keyPhrases", "targetFieldName": "keyPhrases" }
  ]
}
```

| Field | Purpose |
|-------|---------|
| `fieldMappings` | Map source fields → index fields (before enrichment) |
| `outputFieldMappings` | Map enrichment outputs → index fields (after skillset) |
| `schedule.interval` | ISO 8601 duration (e.g., `PT2H` = every 2 hours, `P1D` = daily) |

### Change Detection
- **High watermark** — tracks a timestamp/version column; only indexes changed docs
- **Soft delete** — marks deleted docs instead of removing (uses a flag column)

---

## Skillset — Built-in Skills (EXACT SKILL NAMES)

| Skill | `@odata.type` | Input → Output |
|-------|---------------|----------------|
| **OCR** | `#Microsoft.Skills.Vision.OcrSkill` | Image → text |
| **Image Analysis** | `#Microsoft.Skills.Vision.ImageAnalysisSkill` | Image → tags, description |
| **Key Phrase Extraction** | `#Microsoft.Skills.Text.KeyPhraseExtractionSkill` | Text → key phrases |
| **Entity Recognition** | `#Microsoft.Skills.Text.V3.EntityRecognitionSkill` | Text → entities (person, location, org) |
| **Entity Linking** | `#Microsoft.Skills.Text.V3.EntityLinkingSkill` | Text → Wikipedia-linked entities |
| **Sentiment** | `#Microsoft.Skills.Text.V3.SentimentSkill` | Text → sentiment + score |
| **Language Detection** | `#Microsoft.Skills.Text.LanguageDetectionSkill` | Text → detected language |
| **Text Translation** | `#Microsoft.Skills.Text.TranslationSkill` | Text → translated text |
| **Text Split** | `#Microsoft.Skills.Text.SplitSkill` | Text → pages/sentences |
| **Text Merge** | `#Microsoft.Skills.Text.MergeSkill` | Merge text from multiple inputs |
| **Shaper** | `#Microsoft.Skills.Util.ShaperSkill` | Reshape data for knowledge store |
| **Document Extraction** | `#Microsoft.Skills.Util.DocumentExtractionSkill` | Binary → text + images |
| **PII Detection** | `#Microsoft.Skills.Text.PIIDetectionSkill` | Text → PII entities + redacted text |
| **Custom Entity Lookup** | `#Microsoft.Skills.Text.CustomEntityLookupSkill` | Text → matched custom entities |
| **Conditional** | `#Microsoft.Skills.Util.ConditionalSkill` | If/then logic |

### Skill Input/Output Format
```json
{
  "@odata.type": "#Microsoft.Skills.Text.KeyPhraseExtractionSkill",
  "name": "keyphrases",
  "context": "/document",
  "inputs": [
    { "name": "text", "source": "/document/content" },
    { "name": "languageCode", "source": "/document/language" }
  ],
  "outputs": [
    { "name": "keyPhrases", "targetName": "keyPhrases" }
  ]
}
```

### Context Path Convention
- `/document` — the root document
- `/document/content` — the main text content
- `/document/normalized_images/*` — each extracted image
- `/document/pages/*` — each page (after text split)

---

## Custom Skills

Implemented as an **Azure Function** (HTTP trigger) or any Web API.

### Custom Skill Schema (Web API Skill)
```json
{
  "@odata.type": "#Microsoft.Skills.Custom.WebApiSkill",
  "name": "my-custom-skill",
  "uri": "https://my-function.azurewebsites.net/api/MySkill?code=<key>",
  "httpMethod": "POST",
  "timeout": "PT30S",
  "batchSize": 1,
  "context": "/document",
  "inputs": [
    { "name": "text", "source": "/document/content" }
  ],
  "outputs": [
    { "name": "category", "targetName": "customCategory" }
  ]
}
```

### Custom Skill Request/Response Format
**Request body (from Search):**
```json
{
  "values": [
    {
      "recordId": "1",
      "data": { "text": "Some document content" }
    }
  ]
}
```

**Response body (from your function):**
```json
{
  "values": [
    {
      "recordId": "1",
      "data": { "category": "Technology" },
      "errors": [],
      "warnings": []
    }
  ]
}
```

**Critical:** `recordId` in response MUST match request. Response must include `data`, `errors`, `warnings`.

---

## Index Schema — Field Attributes

| Attribute | Purpose | Default (strings) |
|-----------|---------|-------------------|
| `searchable` | Full-text search (analyzed/tokenized) | true |
| `filterable` | Use in `$filter` expressions | true |
| `sortable` | Use in `$orderby` | true |
| `facetable` | Faceted navigation (counts by category) | true |
| `retrievable` | Returned in search results | true |
| `stored` | Persisted on disk (can disable for vectors) | true |
| `key` | Unique document identifier | false (exactly one must be true) |

### Field Data Types
| Type | Description |
|------|-------------|
| `Edm.String` | Text |
| `Edm.Int32` | Integer |
| `Edm.Int64` | Long integer |
| `Edm.Double` | Floating point |
| `Edm.Boolean` | True/false |
| `Edm.DateTimeOffset` | Date/time |
| `Edm.GeographyPoint` | Lat/long coordinates |
| `Edm.ComplexType` | Nested object |
| `Collection(Edm.String)` | Array of strings |
| `Collection(Edm.Single)` | Vector field (float array) |

---

## Query Syntax — EXAM TESTED HEAVILY

### Simple Query (default)
```
GET /indexes/hotels/docs?search=pool wifi&$top=10
```
- Space between terms = OR
- `+` prefix = AND (must include)
- `-` prefix = NOT (must exclude)
- `"phrase"` = exact phrase

### Full Lucene Query
```
GET /indexes/hotels/docs?search=hotel~2 "ocean view"&queryType=full
```
- `queryType=full` required to enable Lucene syntax
- `~N` = fuzzy search (edit distance)
- `field:value` = field-scoped search
- `*` = wildcard (any characters)
- `?` = wildcard (single character)
- `/regex/` = regex search
- `term^boost` = boost a term

### OData Filter Expressions
```
$filter=rating ge 4 and city eq 'Seattle'
$filter=search.in(category, 'Budget,Luxury', ',')
$filter=geo.distance(location, geography'POINT(-122.12 47.67)') le 10
$filter=rooms/any(r: r/baseRate lt 100)
```

| Operator | Meaning |
|----------|---------|
| `eq` | Equals |
| `ne` | Not equals |
| `gt`, `ge` | Greater than, greater or equal |
| `lt`, `le` | Less than, less or equal |
| `and`, `or`, `not` | Logical operators |
| `search.in()` | Value in a list |
| `geo.distance()` | Geographic distance |
| `any()` / `all()` | Collection filters |

### Other Query Parameters
| Parameter | Purpose | Example |
|-----------|---------|---------|
| `$select` | Choose returned fields | `$select=name,rating,city` |
| `$orderby` | Sort results | `$orderby=rating desc` |
| `$top` | Limit results | `$top=10` |
| `$skip` | Pagination offset | `$skip=20` |
| `$count` | Include total count | `$count=true` |
| `$filter` | Filter results | `$filter=rating ge 4` |
| `facets` | Get facet counts | `facets=category,count:10` |
| `highlight` | Hit highlighting | `highlight=description` |
| `searchFields` | Limit which fields to search | `searchFields=title,content` |
| `scoringProfile` | Apply scoring profile | `scoringProfile=boostNew` |
| `queryType` | `simple` (default) or `full` (Lucene) | `queryType=full` |
| `searchMode` | `any` (OR) or `all` (AND) for terms | `searchMode=all` |

---

## Semantic Search & Vector Search

### Semantic Search
- Re-ranks results using deep learning language models
- Provides: **semantic captions**, **semantic answers**
- Enable in index: `semantic` configuration with `prioritizedFields`
- Query: `queryType=semantic&semanticConfiguration=my-config`

### Semantic Configuration
```json
"semantic": {
  "defaultConfiguration": "my-config",
  "configurations": [{
    "name": "my-config",
    "prioritizedFields": {
      "titleField": { "fieldName": "title" },
      "prioritizedContentFields": [{ "fieldName": "content" }],
      "prioritizedKeywordsFields": [{ "fieldName": "tags" }]
    }
  }]
}
```

### Vector Search
- Store embeddings in `Collection(Edm.Single)` fields
- Configure: algorithm (HNSW or exhaustive KNN), metric (cosine, euclidean, dotProduct)
- Query: pass `vectorQueries` with the embedding vector
- **HNSW** = fast approximate search (default, best for large indexes)
- **Exhaustive KNN** = slower exact search (brute force, 100% recall)

### Vector Search Metrics
| Metric | Best For |
|--------|----------|
| `cosine` | Most text embeddings (direction-based similarity) |
| `euclidean` | Distance-based similarity |
| `dotProduct` | When vectors are normalized |
| `hamming` | Binary/bit-packed vectors only |

### Hybrid Search
- Combines text search + vector search in a single query
- Results fused using Reciprocal Rank Fusion (RRF)
- Best of both worlds: keyword matching + semantic understanding

---

## Knowledge Store Projections

| Projection Type | Stored In | Use Case |
|----------------|-----------|----------|
| **Table** | Azure Table Storage | Structured data for Power BI / analytics |
| **Object** | Azure Blob Storage | JSON objects |
| **File** | Azure Blob Storage | Extracted images as files |

```json
"knowledgeStore": {
  "storageConnectionString": "<connection>",
  "projections": [{
    "tables": [{ "tableName": "keyPhrases", "source": "/document/keyPhrases/*" }],
    "objects": [{ "storageContainer": "enriched", "source": "/document" }],
    "files": [{ "storageContainer": "images", "source": "/document/normalized_images/*" }]
  }]
}
```

Use the **Shaper skill** to reshape data into the structure you need for projections.

---

## Scoring Profiles

Boost relevance based on field values.

### Scoring Functions
| Function | Boosts By |
|----------|----------|
| **magnitude** | Numeric field value (e.g., higher rating = higher score) |
| **freshness** | Date recency (newer = higher score) |
| **distance** | Geographic proximity |
| **tag** | Matching tags from a parameter list |

### Function Interpolation
| Type | Behavior |
|------|----------|
| `linear` | Linearly decreasing boost (default) |
| `constant` | Same boost regardless of value |
| `quadratic` | Slowly decreasing, then sharp drop (favor very recent; NOT allowed for tag) |
| `logarithmic` | Fast initial drop, then slow decrease (NOT allowed for tag) |

### Function Aggregation
| Value | Combines multiple scoring functions by... |
|-------|------------------------------------------|
| `sum` | Sum of all function scores (default) |
| `average` | Average |
| `minimum` | Minimum |
| `maximum` | Maximum |
| `firstMatching` | First applicable function only |

---

## Suggesters & Autocomplete

- A **suggester** enables typeahead (autocomplete + suggestions)
- One suggester per index, named (e.g., `"sg"`)
- `searchMode`: only value is `analyzingInfixMatching`
- Fields must be `Edm.String` and `searchable`
- Must use default or language analyzer (no custom analyzers)

```json
"suggesters": [{
  "name": "sg",
  "searchMode": "analyzingInfixMatching",
  "sourceFields": ["HotelName", "Category"]
}]
```

### Synonym Maps
- Define equivalent terms: `"hotel, motel, inn"` or `"US, United States => USA"`
- Assign to fields via `synonymMaps` property
- One synonym map per field
- Up to 5,000 rules per map
| `quadratic` | Slowly decreasing boost |
| `logarithmic` | Quickly decreasing boost |

---

## Azure Document Intelligence

### Prebuilt Models (exact model IDs)
| Model | Model ID | Extracts |
|-------|----------|----------|
| Invoice | `prebuilt-invoice` | Vendor, dates, line items, amounts |
| Receipt | `prebuilt-receipt` | Merchant, date, items, total, tax, tip |
| ID Document | `prebuilt-idDocument` | Name, DOB, address, doc number, expiry |
| Business Card | `prebuilt-businessCard` | Name, title, company, phone, email |
| W-2 (US Tax) | `prebuilt-tax.us.w2` | Employer, wages, tax withheld |
| Health Insurance | `prebuilt-healthInsuranceCard.us` | Member info, plan details |
| Read (OCR) | `prebuilt-read` | Text, paragraphs, lines, words, languages |
| Layout | `prebuilt-layout` | Tables, selection marks, text, structure |
| General Document | `prebuilt-document` | Key-value pairs, entities, tables |

### API Call
```
POST https://<endpoint>/formrecognizer/documentModels/<model-id>:analyze?api-version=2023-07-31

Headers:
  Ocp-Apim-Subscription-Key: <key>
  Content-Type: application/json

Body: { "urlSource": "https://example.com/invoice.pdf" }
```

**Response is async:** Returns `Operation-Location` header → poll for results.

### Custom Models
| Model Type | When To Use | Layout |
|------------|-------------|--------|
| **Template** | Fixed-layout forms (same positions every time) | Structured |
| **Neural** | Variable-layout forms (different vendors/formats) | Semi-structured |

Training: Minimum **5 labeled documents** per model.

### Composed Models
- Combine multiple custom models behind a single model ID
- Automatically **routes** incoming documents to the right sub-model
- Use when you have different form types (e.g., 3 different invoice formats)
- Max: 200 component models in a composed model

### Document Intelligence Studio
- Web UI for testing prebuilt models
- Labeling tool for custom model training
- URL: https://formrecognizer.appliedai.azure.com/

---

## Azure Content Understanding

### Capabilities
| Feature | What It Does |
|---------|--------------|
| **OCR Pipeline** | Extract text from images and documents |
| **Summarization** | Summarize document content |
| **Classification** | Classify documents into categories |
| **Attribute Detection** | Detect specific attributes/properties |
| **Entity Extraction** | Extract entities, tables, images from documents |
| **Multi-modal** | Process documents, images, videos, and audio |

### Key Differentiator
- **Document Intelligence** = structured extraction from forms/documents
- **Content Understanding** = broader: summarize, classify, detect attributes across multiple media types (docs, images, video, audio)

---

## Numbers to Remember

| Fact | Value |
|------|-------|
| AI Search free tier | 50 MB storage, 3 indexes |
| AI Search Basic tier | 2 GB storage, 15 indexes |
| Custom skill timeout default | 30 seconds |
| Custom skill timeout max | 230 seconds |
| Indexer max run time (free) | 3 minutes |
| Indexer max run time (standard) | 2 hours (24 hours with indexer config) |
| Max scoring profiles per index | 100 |
| Max synonym maps per index | 5,000 rules per map |
| Doc Intelligence min training docs | 5 labeled documents |
| Composed model max components | 200 |
| HNSW `m` parameter range | 4–10 (default 4) |
| HNSW `efConstruction` range | 100–1000 (default 400) |
| HNSW `efSearch` range | 100–1000 (default 500) |
