# Cheat Sheet 5: Implement Natural Language Processing Solutions (15–20%)

---

## Azure AI Language — Text Analytics Features

### API Endpoints (exact paths)
| Feature | Endpoint Path | Method |
|---------|--------------|--------|
| Sentiment | `/text/analytics/v3.1/sentiment` | POST |
| Key Phrases | `/text/analytics/v3.1/keyPhrases` | POST |
| Named Entities | `/text/analytics/v3.1/entities/recognition/general` | POST |
| Entity Linking | `/text/analytics/v3.1/entities/linking` | POST |
| PII Detection | `/text/analytics/v3.1/entities/recognition/pii` | POST |
| Language Detection | `/text/analytics/v3.1/languages` | POST |

### Request Format (all Text Analytics APIs)
```json
POST https://<endpoint>/text/analytics/v3.1/sentiment
Headers: Ocp-Apim-Subscription-Key: <key>

{
  "documents": [
    {"id": "1", "language": "en", "text": "I love this product!"},
    {"id": "2", "language": "en", "text": "Terrible experience."}
  ]
}
```

### Sentiment Analysis Response
```json
{
  "documents": [{
    "id": "1",
    "sentiment": "positive",
    "confidenceScores": { "positive": 0.98, "neutral": 0.01, "negative": 0.01 },
    "sentences": [{ "sentiment": "positive", "text": "I love this product!" }]
  }]
}
```

Sentiment values: `positive`, `negative`, `neutral`, `mixed`

### Key Phrases Response
Returns array of key phrases (strings). No confidence scores.

### Named Entity Types
| Entity Type | Examples |
|-------------|----------|
| Person | "John Smith" |
| Location | "Paris", "Mount Everest" |
| Organization | "Microsoft", "UN" |
| DateTime | "tomorrow", "June 5th" |
| Quantity | "50 kilograms", "3 miles" |
| URL | "https://example.com" |
| Email | "user@example.com" |
| Phone Number | "+1-555-0100" |

### PII Entity Categories
| Category | Examples |
|----------|----------|
| `Person` | Full names |
| `PhoneNumber` | Phone numbers |
| `Email` | Email addresses |
| `SSN / National ID` | Social security numbers |
| `CreditCardNumber` | Credit card numbers |
| `Address` | Physical addresses |
| `IPAddress` | IP addresses |
| `DateOfBirth` | Dates of birth |

PII response includes `redactedText` with entities replaced by `*****`.

### Entity Linking
- Links entities to **Wikipedia** articles
- Returns: entity name, Wikipedia URL, data source, matches in text
- Disambiguates: "Mars" (planet vs company vs chocolate)

### Language Detection
- Input: `documents[].text` (no `language` field needed)
- Output: `detectedLanguage.name`, `detectedLanguage.iso6391Name`, `confidenceScore`
- Can detect 120+ languages
- For ambiguous content, returns `unknown` with score 1.0 and `NaN` iso code

### Text Analytics Limits
| Limit | Value |
|-------|-------|
| Max documents per request | 1,000 |
| Max document size | 5,120 characters |
| Max request size | 1 MB |

> **Exam trap:** For **Language Detection**, the `language` field is NOT needed in the request (it auto-detects). For all other Text Analytics APIs, you should provide `language`.

---

## Azure AI Translator

### Text Translation API
```
POST https://api.cognitive.microsofttranslator.com/translate?api-version=3.0&to=fr&to=de

Headers:
  Ocp-Apim-Subscription-Key: <key>
  Ocp-Apim-Subscription-Region: <region>    ← REQUIRED for Translator!
  Content-Type: application/json

Body:
[{"Text": "Hello, how are you?"}]
```

**Exam trap:** Translator requires the `Ocp-Apim-Subscription-Region` header. Other services don't.

### Translation Features
| Feature | Parameter | Purpose |
|---------|-----------|---------|
| Translate | `to=fr` | Translate to French |
| Multiple targets | `to=fr&to=de&to=ja` | Translate to multiple languages at once |
| Source language | `from=en` | Specify source (optional; auto-detect if omitted) |
| Transliterate | `/transliterate` | Convert script (e.g., Japanese kanji → romaji) |
| Detect | `/detect` | Detect source language |
| Dictionary Lookup | `/dictionary/lookup` | Get alternative translations |
| Dictionary Examples | `/dictionary/examples` | Get example sentences |
| Document Translation | Async batch API | Translate entire documents (Word, PDF, etc.) |

### Custom Translator
- Train custom models with your own parallel text (source + target pairs)
- Use for: domain-specific terminology, brand names, technical content
- Train → Test → Publish → Use custom `category` parameter in API calls
- Requires **10,000+ sentence pairs** for good quality

---

## Azure AI Speech

### Speech-to-Text (STT) — SDK Pattern
```python
import azure.cognitiveservices.speech as speechsdk

speech_config = speechsdk.SpeechConfig(
    subscription="<key>",
    region="<region>"
)
speech_config.speech_recognition_language = "en-US"

# From microphone
recognizer = speechsdk.SpeechRecognizer(speech_config=speech_config)
result = recognizer.recognize_once()

# From file
audio_config = speechsdk.AudioConfig(filename="audio.wav")
recognizer = speechsdk.SpeechRecognizer(speech_config=speech_config, audio_config=audio_config)
result = recognizer.recognize_once()

# Continuous recognition
recognizer.recognized.connect(lambda evt: print(evt.result.text))
recognizer.start_continuous_recognition()
```

### Recognition Result Properties
| Property | Value |
|----------|-------|
| `result.reason` | `ResultReason.RecognizedSpeech`, `NoMatch`, `Canceled` |
| `result.text` | The recognized text |
| `result.offset` | Start time in ticks |
| `result.duration` | Duration in ticks |

### Text-to-Speech (TTS) — SDK Pattern
```python
speech_config = speechsdk.SpeechConfig(subscription="<key>", region="<region>")
speech_config.speech_synthesis_voice_name = "en-US-JennyNeural"

synthesizer = speechsdk.SpeechSynthesizer(speech_config=speech_config)
result = synthesizer.speak_text_async("Hello world").get()

# Or with SSML
result = synthesizer.speak_ssml_async(ssml_string).get()
```

### SSML — Speech Synthesis Markup Language (EXAM FAVORITE)
```xml
<speak version="1.0" xmlns="http://www.w3.org/2001/10/synthesis"
       xmlns:mstts="http://www.w3.org/2001/mstts" xml:lang="en-US">
  <voice name="en-US-JennyNeural">
    <prosody rate="slow" pitch="+10%" volume="loud">
      Welcome to the demo.
    </prosody>
    <break time="500ms"/>
    <emphasis level="strong">Important!</emphasis>
    <say-as interpret-as="date" format="mdy">1/29/2024</say-as>
    <say-as interpret-as="telephone">+1-555-0100</say-as>
    <say-as interpret-as="cardinal">12345</say-as>
    <mstts:express-as style="cheerful">That's great!</mstts:express-as>
  </voice>
</speak>
```

### SSML Tags — Know Each One
| Tag | Attributes | Purpose |
|-----|-----------|---------|
| `<speak>` | `version`, `xml:lang` | Root element |
| `<voice>` | `name` (e.g., `en-US-JennyNeural`) | Select voice |
| `<prosody>` | `rate`, `pitch`, `volume` | Control speech rate/pitch/volume |
| `<break>` | `time` (e.g., `500ms`, `2s`) or `strength` | Pause |
| `<emphasis>` | `level` (strong/moderate/reduced) | Emphasize word |
| `<say-as>` | `interpret-as` (date/telephone/cardinal/ordinal/spell-out) | Pronunciation hints |
| `<mstts:express-as>` | `style` (cheerful/sad/angry/excited) | Emotional style |
| `<phoneme>` | `alphabet` (ipa/sapi), `ph` | Exact pronunciation |
| `<sub>` | `alias` | Substitution ("UN" → "United Nations") |
| `<p>` / `<s>` | — | Paragraph / sentence |

### Prosody Values
| Attribute | Values |
|-----------|--------|
| `rate` | `x-slow`, `slow`, `medium`, `fast`, `x-fast`, `+50%`, `-30%` |
| `pitch` | `x-low`, `low`, `medium`, `high`, `x-high`, `+10%`, `-5Hz` |
| `volume` | `silent`, `x-soft`, `soft`, `medium`, `loud`, `x-loud`, `+20%` |

### Custom Speech
- Train custom STT models with your own audio + transcripts
- For: accents, domain-specific terminology, noisy environments
- Requires: audio files (.wav) + human-labeled transcripts
- Custom endpoint URL after deployment

### Intent Recognition
- Combines Speech-to-Text with **CLU** (Conversational Language Understanding)
- Recognizes spoken input AND extracts intent + entities
- Uses `IntentRecognizer` instead of `SpeechRecognizer`

```python
intent_recognizer = speechsdk.intent.IntentRecognizer(speech_config=speech_config)
model = speechsdk.intent.LanguageUnderstandingModel(app_id="<CLU-app-id>")
intent_recognizer.add_all_intents(model)
result = intent_recognizer.recognize_once()
# result.intent_id = "BookFlight"
```

### Keyword Recognition
- Detect a wake word / keyword (e.g., "Hey Computer")
- Uses `KeywordRecognizer` with a keyword model (.table file)
- Generated via Speech Studio

### Speech Translation
```python
translation_config = speechsdk.translation.SpeechTranslationConfig(
    subscription="<key>", region="<region>"
)
translation_config.speech_recognition_language = "en-US"
translation_config.add_target_language("fr")
translation_config.add_target_language("de")

recognizer = speechsdk.translation.TranslationRecognizer(
    translation_config=translation_config
)
result = recognizer.recognize_once()
# result.translations["fr"] = "Bonjour le monde"
# result.translations["de"] = "Hallo Welt"
```

- **Speech-to-text translation:** Spoken audio → translated text
- **Speech-to-speech translation:** Add `voice_name` to get synthesized output in target language

---

## CLU — Conversational Language Understanding

### Key Concepts
| Concept | Definition | Example |
|---------|-----------|---------|
| **Intent** | What the user wants | `BookFlight`, `GetWeather`, `Cancel` |
| **Entity** | Parameters/details | `destination=Paris`, `date=tomorrow` |
| **Utterance** | Example sentence | "Book me a flight to Paris tomorrow" |
| **None intent** | Fallback for unrecognized input | Always include; catches out-of-scope |

### CLU Workflow
1. **Create** a CLU project in Language Studio
2. **Define intents** (BookFlight, GetWeather, etc.)
3. **Define entities** (destination, date, etc.)
4. **Add utterances** (at least 5 per intent, labeled with entities)
5. **Train** the model
6. **Evaluate** (precision, recall, F1 per intent/entity)
7. **Deploy** to a named slot (e.g., "production")
8. **Query** via REST API or SDK

### CLU Entity Types
| Type | Description |
|------|-------------|
| **Learned** | ML-extracted from context (you label examples) |
| **List** | Fixed list of values with synonyms |
| **Prebuilt** | Built-in: datetime, number, email, URL, etc. |
| **Regex** | Pattern match (e.g., flight numbers) |

### CLU API Call
```
POST https://<endpoint>/language/:analyze-conversations?api-version=2023-04-01

{
  "kind": "Conversation",
  "analysisInput": {
    "conversationItem": {
      "id": "1",
      "text": "Book a flight to Paris",
      "participantId": "user1"
    }
  },
  "parameters": {
    "projectName": "<project>",
    "deploymentName": "production"
  }
}
```

---

## Custom Question Answering

### Sources
| Source Type | Description |
|------------|-------------|
| **URL** | Import from FAQ web pages |
| **File** | PDF, DOCX, XLSX, TXT, TSV |
| **Manual** | Add QnA pairs directly |
| **Chit-chat** | Pre-built personality sets |

### Features
| Feature | Purpose |
|---------|---------|
| **Multi-turn** | Follow-up prompts ("Did you mean A or B?") |
| **Alternate phrasing** | Multiple questions → same answer |
| **Chit-chat** | Pre-built small-talk (professional, friendly, witty, caring, enthusiastic) |
| **Active learning** | Suggests alternate questions from user traffic |
| **Metadata** | Filter QnA pairs by key-value metadata |
| **Precise answering** | Extract exact short answer from longer answer passage |
| **Multi-language** | Create projects in multiple languages |

### Question Answering Workflow
1. Create project in Language Studio
2. Add sources (URLs, files, manual QnA)
3. Review and edit QnA pairs
4. Add alternate phrasings
5. Add multi-turn follow-up prompts
6. Add chit-chat personality
7. Test in Language Studio
8. Deploy knowledge base
9. Query via REST API

### Query API
```
POST https://<endpoint>/language/:query-knowledgebases?api-version=2021-10-01

{
  "question": "How do I reset my password?",
  "top": 3,
  "confidenceScoreThreshold": 0.5,
  "projectName": "<project>",
  "deploymentName": "production"
}
```

---

## Custom Translation

- Train custom translation models with domain-specific parallel text
- Requires paired sentences (source language + target language)
- Minimum: **10,000 parallel sentences** for quality
- Publish model → use `category` parameter in Translator API
- Managed in Custom Translator portal (https://portal.customtranslator.azure.ai)
