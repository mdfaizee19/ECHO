# ECHO

## Multimodal Misinformation & AI-Generated Content Detection Platform

ECHO is a full-stack AI system that detects misinformation, synthetic media, and AI-generated content across text, images, and short videos.

It operates in two modes:

1. Manual Analysis Mode (API / Web Input)
2. Browser Extension Mode (Real-time automatic detection)

The system produces a structured credibility score with risk classification and explanation.

---

## Operating Modes

### 1. Manual Analysis Mode

Users can directly submit:

- Raw text
- Article URLs
- Images
- Short videos

The system returns:

- Risk score
- Risk label (LOW / MEDIUM / HIGH)
- AI-generation probability
- Similarity score
- Signal-level breakdown

This mode is suitable for structured verification workflows.

---

### 2. Browser Extension Mode

ECHO provides a Chrome browser extension that:

- Automatically extracts active webpage content
- Sends it to the backend
- Displays a real-time credibility popup

Example Output:

```
Risk Score: 0.72
Label: HIGH
Likely AI Generated: Yes
```

This mode enables frictionless, real-time content verification during browsing.

---

## Problem Statement

Misinformation spreads rapidly across:

- News websites
- Blogs
- Social media platforms
- Image-based quote graphics
- Short-form video platforms

There is no unified system that simultaneously detects:

- Fake or misleading text
- AI-generated text
- AI-generated images
- Misinformation embedded inside images
- Misinformation spoken inside videos

ECHO solves this using a modular multimodal detection pipeline.

---

## System Architecture

```
User Input (Text / URL / Image / Video)
                    ↓
        Ingestion & Normalization Layer
                    ↓
        Multimodal Detection Engines
                    ↓
        Vector Similarity Engine (pgvector)
                    ↓
        Risk Aggregation Engine
                    ↓
        Structured Credibility Output
```

Browser Extension Mode:

```
Webpage → Chrome Extension → FastAPI Backend → Detection Pipeline → Popup Output
```

---

## Core Components

### URL Scraper / Content Extractor

- Extract article text from URLs
- Clean HTML
- Identify domain metadata

Libraries:
- requests
- BeautifulSoup
- newspaper3k

---

### Text Analysis Engine

Detects:
- Fake or misleading content
- AI-generated text

Models:
- Transformer-based classifiers (RoBERTa / DeBERTa)
- Sentence-Transformer embeddings

Framework:
- PyTorch
- Hugging Face Transformers

---

### Image Analysis Engine

Pipeline:
- Generate CLIP embeddings
- Detect AI-generated image artifacts
- Extract embedded text using OCR
- Classify extracted text

Libraries:
- torchvision
- timm
- open_clip
- easyocr

---

### Video Analysis Engine

Pipeline:
- Extract audio from video
- Transcribe using Whisper
- Run transcript through fake classifier
- Sample frames for AI-image detection
- Aggregate multimodal signals

Libraries:
- moviepy
- opencv-python
- faster-whisper

---

### Vector Similarity Engine

Purpose:
- Compare content against known misinformation corpus
- Compare against trusted verified sources

Technology:
- Sentence-Transformers
- CLIP embeddings
- PostgreSQL with pgvector
- Cosine similarity search

---

### Risk Aggregation Engine

Final risk score calculation:

```
risk =
0.35 * fake_score +
0.20 * ai_text_score +
0.20 * ai_image_score +
0.25 * similarity_score
```

Risk Classification:

- < 0.3 → LOW
- 0.3–0.6 → MEDIUM
- > 0.6 → HIGH

---

## Tech Stack

Backend:
- Python 3.10+
- FastAPI
- Uvicorn
- SQLAlchemy
- Alembic

Database:
- PostgreSQL 15
- pgvector extension

Machine Learning:
- torch
- transformers
- sentence-transformers

Vision:
- torchvision
- timm
- open_clip

OCR:
- easyocr

Video Processing:
- moviepy
- opencv-python

Speech-to-Text:
- faster-whisper

Scraping:
- requests
- beautifulsoup4
- newspaper3k

Infrastructure:
- Docker
- Docker Compose
- Render (deployment)

Frontend:
- HTML
- CSS
- JavaScript
- Chrome Extension APIs

---

## Who Can Use ECHO

- Investigative journalists
- Newsrooms and editorial teams
- Fact-checking organizations
- Social media users
- Students and academic researchers
- Media literacy educators
- Government and policy analysts
- Cybersecurity and threat intelligence teams
- Legal and compliance teams
- Corporate communications teams
- Platform moderation teams
- AI safety researchers

---

## Use Cases

- News authenticity verification
- Detection of AI-generated propaganda
- Image-based misinformation screening
- Deepfake video pre-screening
- Brand misinformation monitoring
- Academic source validation
- Moderation queue prioritization

---

## Installation

Clone repository:

```
git clone https://github.com/yourusername/ECHO.git
cd ECHO
```

Run locally with Docker:

```
docker-compose up --build
```

---

## Deployment

Platform: Render

Steps:
1. Push repository to GitHub
2. Create Web Service
3. Attach PostgreSQL
4. Set environment variables
5. Deploy

Start command:

```
uvicorn app.main:app --host 0.0.0.0 --port $PORT
```

---

## Project Structure

```
ECHO/
│
├── app/
├── extension/
├── ml_models/
├── alembic/
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
└── README.md
```

---

## API

POST /analyze

Accepts:
- text
- image file
- video file
- url

Returns:

```
{
  "risk_score": 0.72,
  "risk_label": "HIGH",
  "fake_score": 0.81,
  "ai_text_score": 0.65,
  "ai_image_score": 0.49,
  "similarity_score": 0.76
}
```
