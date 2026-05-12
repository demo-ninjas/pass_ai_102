# Cheat Sheet 4: Implement Computer Vision Solutions (10–15%)

> This domain tests EXACT model names and API parameters. Memorize the tables.

---

## Azure AI Vision — Image Analysis 4.0

### Visual Features (exact API values)
| Feature | API Value | Returns |
|---------|-----------|---------|
| Caption | `caption` | Single sentence description + confidence |
| Dense Captions | `denseCaptions` | Multiple captions for regions + bounding boxes |
| Tags | `tags` | Content tags + confidence scores |
| Objects | `objects` | Detected objects + bounding boxes |
| People | `people` | Detected people + bounding boxes |
| Smart Crops | `smartCrops` | Suggested crop regions by aspect ratio |
| OCR / Read | `read` | Extracted text + bounding polygons |

### Image Analysis API Call
```
POST https://<endpoint>/computervision/imageanalysis:analyze?api-version=2024-02-01
  &features=caption,tags,objects,read
  &language=en
  &gender-neutral-caption=true
  &smartcrops-aspect-ratios=0.9,1.33
```

### Image Analysis Options
| Option | Values | Purpose |
|--------|--------|---------|
| `language` | `en`, `ja`, `zh`, etc. | Language of returned text |
| `gender-neutral-caption` | `true` / `false` | Replace "man/woman" with "person", "boy/girl" with "child" |
| `smartcrops-aspect-ratios` | `0.75` – `1.8` | Desired crop ratios |
| `model-version` | `latest`, `2024-02-01` | API model version |

### Region Availability
- `caption` and `denseCaptions` are **only available in certain Azure regions**
- Check region support before deploying

---

## Face API — EXACT Model Versions (EXAM FAVORITE)

### Detection Models
| Model | Released | Best For | Landmarks? | Attributes? |
|-------|----------|----------|------------|-------------|
| `detection_01` | Default | General purpose | Yes | Yes (all) |
| `detection_02` | May 2019 | **Small, side-view, blurry faces** | **No** | **No** |
| `detection_03` | Feb 2021 | **Best accuracy, small faces (64x64), rotated** | Yes | Yes (most) |

### Recognition Models
| Model | Released | Best For |
|-------|----------|----------|
| `recognition_01` | 2017 | Legacy (default if not specified) |
| `recognition_02` | 2019 | Improved accuracy |
| `recognition_03` | 2020 | Better accuracy |
| `recognition_04` | 2021 | **Best accuracy; works with face masks** |

### CRITICAL Exam Rules
- **Blurry/small/side-view faces?** → Use `detection_02` or `detection_03`
- **Best overall detection?** → `detection_03`
- **Need face landmarks?** → Use `detection_01` or `detection_03` (NOT `detection_02`)
- **Best recognition?** → `recognition_04` (handles face masks)
- **detection_02 returns NO landmarks and NO attributes**
- **Default models if unspecified:** `detection_01` and `recognition_01`
- **Models must be consistent:** A PersonGroup created with `recognition_04` requires faces detected with `recognition_04`
- **Can't change model** after creating a PersonGroup or FaceList

### Face API Parameters
| Parameter | Purpose |
|-----------|---------|
| `detectionModel` | `detection_01`, `detection_02`, `detection_03` |
| `recognitionModel` | `recognition_01` – `recognition_04` |
| `returnFaceId` | Must be `true` for recognition scenarios |
| `returnFaceLandmarks` | `true` to get 27 facial landmark points |
| `returnFaceAttributes` | Comma list: `blur,exposure,glasses,headPose,mask,noise,occlusion,qualityForRecognition` |
| `faceIdTimeToLive` | Seconds to keep face ID (default 86400 = 24 hours) |
| `returnRecognitionModel` | Return which recognition model was used |

### Face Attributes (know what each returns)
| Attribute | Values | Detection Model |
|-----------|--------|-----------------|
| `blur` | low / medium / high + score (0–1) | `detection_01`, `detection_03` |
| `exposure` | underExposure / goodExposure / overExposure | `detection_01`, `detection_03` |
| `glasses` | NoGlasses / ReadingGlasses / Sunglasses / SwimmingGoggles | `detection_01`, `detection_03` |
| `headPose` | roll, yaw, pitch (degrees) | `detection_01`, `detection_03` |
| `mask` | mask type + noseAndMouthCovered boolean | `detection_03` only |
| `noise` | low / medium / high + score (0–1) | `detection_01`, `detection_03` |
| `occlusion` | eyeOccluded, foreheadOccluded, mouthOccluded | `detection_01`, `detection_03` |
| `qualityForRecognition` | low / medium / high | `detection_01` or `detection_03` with `recognition_03`/`04` |

### QualityForRecognition Rules
- **High** quality recommended for person **enrollment**
- **Medium** or better recommended for **identification**
- Only available with `detection_01`/`detection_03` + `recognition_03`/`recognition_04`

### Face API Operations
| Operation | What It Does |
|-----------|--------------|
| **Detect** | Find faces in image; returns face rectangles, optional landmarks/attributes |
| **Identify** | Match detected face against a PersonGroup |
| **Verify** | Check if two faces are the same person (1:1) |
| **Find Similar** | Find similar faces in a FaceList |
| **Group** | Group similar faces together |

### PersonGroup / FaceList
| Object | Purpose | Recognition Model |
|--------|---------|-------------------|
| **PersonGroup** | Collection of Persons for identification | Set at creation, immutable |
| **LargePersonGroup** | Same but for >1000 persons | Set at creation, immutable |
| **FaceList** | Collection of faces for Find Similar | Set at creation, immutable |
| **LargeFaceList** | Same but for >1000 faces | Set at creation, immutable |

Workflow: Create group → Add persons → Add faces → **Train** → Identify

### Face API — Limited Access
- Face **recognition** features require **limited access approval**
- Apply via the [Face Recognition intake form](https://aka.ms/facerecognition)
- Detection (finding faces) is generally available
- Identification/verification requires approval

---

## Service Selection — Image Analysis vs Face API vs Custom Vision

| Need | Use | NOT |
|------|-----|-----|
| "Detect people in an image" (bounding boxes only) | **Image Analysis 4.0** (`people`) | Not Face API (that's for identity) |
| "Identify WHO a person is" | **Face API** (Identify) | Not Image Analysis (no identity) |
| "Is this a cat or dog?" | **Custom Vision** (classification) | Not Image Analysis tags (not your custom categories) |
| "WHERE are the cats?" (bounding boxes) | **Custom Vision** (object detection) | Not Image Analysis objects (generic, not your data) |
| "Extract text from a document" | **Read API** or **Document Intelligence** | Not Custom Vision |

---

## Custom Vision

> **WARNING:** Custom Vision is **retiring September 25, 2028**. For new projects, Microsoft recommends Azure ML AutoML or Azure Content Understanding. However, Custom Vision is still on the AI-102 exam.

### Model Types
| Type | Task | Output |
|------|------|--------|
| **Classification** | What is in the image? | Tag + confidence |
| **Multilabel classification** | Multiple tags per image | Multiple tags |
| **Object Detection** | Where are objects? | Tag + bounding box + confidence |

### Training Requirements
| Type | Minimum Images Per Tag | Recommended |
|------|----------------------|-------------|
| Classification | **5** images per tag (absolute minimum) | **30+** per tag |
| Object Detection | **15** images per tag (with bounding boxes) | **50+** per tag |

> **Exam trap:** The absolute minimums are 5 (classify) and 15 (detect), but MS docs recommend **at least 30** images per tag for quality results. Know both numbers.

### Evaluation Metrics
| Metric | What It Measures |
|--------|-----------------|
| **Precision** | Of predicted positives, how many are correct? (low = too many false positives) |
| **Recall** | Of actual positives, how many were found? (low = too many false negatives) |
| **AP (Average Precision)** | Area under precision-recall curve; overall quality |
| **mAP** | Mean AP across all classes |

### Custom Vision Workflow
1. Create project (classification or object detection)
2. Upload and **tag/label** images
3. **Train** (quick training or advanced)
4. **Evaluate** (precision, recall, AP)
5. **Publish** to a prediction resource
6. **Consume** via prediction endpoint

### Custom Vision Domains (Compact vs General)
| Domain | Use Case |
|--------|----------|
| **General** | Best accuracy, runs in cloud |
| **General (compact)** | Export to ONNX/TensorFlow/CoreML for edge |
| **Food** | Optimized for food photos |
| **Landmarks** | Optimized for recognizable landmarks |
| **Retail** | Optimized for retail/shopping |

**Compact models** can be exported for offline/edge use (mobile, IoT).

### Prediction API
```
POST https://<endpoint>/customvision/v3.0/Prediction/<project-id>/classify/iterations/<published-name>/image
Headers: Prediction-Key: <key>
Body: image binary
```

---

## Azure AI Video Indexer

### What It Extracts
| Insight | Description |
|---------|-------------|
| **Faces** | Detect and identify people (with Face API) |
| **OCR** | Text visible in video frames |
| **Topics** | Main topics discussed |
| **Keywords** | Key terms mentioned |
| **Sentiment** | Positive/negative/neutral sentiment |
| **Scenes** | Scene boundaries and keyframes |
| **Shots** | Camera shot detection |
| **Transcript** | Speech-to-text transcription |
| **Translation** | Translate transcript to other languages |
| **Labels** | Visual objects and actions detected |
| **Brands** | Brand logos and mentions |
| **Named entities** | People, places, organizations |
| **Observed people** | Track individuals across frames |

### Video Indexer Access
- Web portal: https://www.videoindexer.ai/
- REST API available
- Can index from URL, upload, or Azure Blob

---

## Spatial Analysis (Azure AI Vision)

| Operation | What It Detects |
|-----------|----------------|
| **Person counting** | Count people in a zone |
| **Person distance** | Measure distance between people |
| **Person zone crossing** | Detect when someone enters/exits a zone |
| **Person line crossing** | Detect when someone crosses a virtual line |
| **Person dwell time** | How long someone stays in a zone |

### Key Details
- Runs on **edge** devices (requires NVIDIA GPU)
- Deployed as a **Docker container**
- Processes **video streams** (RTSP)
- Used for: retail analytics, workplace safety, occupancy monitoring
- Requires spatial analysis container from `mcr.microsoft.com`

---

## Numbers to Remember

| Fact | Value |
|------|-------|
| Face API min detectable face | 36 × 36 pixels |
| Face API max detectable face | 4096 × 4096 pixels |
| Face API max image size | 6 MB |
| Face API image formats | JPEG, PNG, GIF (first frame), BMP |
| Face ID time-to-live default | 86400 seconds (24 hours) |
| Face landmarks count | 27 points |
| Custom Vision min images (classify) | 5 per tag (30+ recommended) |
| Custom Vision min images (detect) | 15 per tag (50+ recommended) |
| Image Analysis 4.0 smart crop range | 0.75 – 1.8 aspect ratio |
