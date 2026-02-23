ECHO

Multimodal Misinformation & AI-Generated Content Detection System

ECHO is a full-stack AI system that detects misinformation and synthetic media across text, images, and short videos.

It consists of:

A Python FastAPI backend

PostgreSQL with pgvector for semantic similarity

Multimodal ML pipelines (text, image, video)

A Chrome browser extension for real-time webpage detection

Problem

Misinformation spreads rapidly across:

News websites

Blogs

Social media posts

Image-based quotes

Short-form video platforms

There is no unified system that detects:

Fake text

AI-generated text

AI-generated images

Misinformation inside images

Misinformation spoken inside videos

ECHO solves this using a multimodal detection pipeline.

Solution Overview

ECHO analyzes:

Raw text

Article URLs

Images

Short videos

Active webpage content via browser extension

It outputs a structured risk score with classification and explanation.

System Architecture
Chrome Extension
        ↓
FastAPI Backend
        ↓
--------------------------------------------------
| URL Scraper / Content Extractor               |
| Text Analysis Engine                          |
| Image Analysis Engine                         |
| Video (Audio → Text) Engine                   |
| Vector Similarity Engine (pgvector)           |
| Risk Aggregation Engine                       |
--------------------------------------------------
        ↓
PostgreSQL + pgvector
Core Components
1. URL Scraper / Content Extractor

Purpose:

Extract article text from webpage URLs

Clean HTML

Identify domain

Libraries:

requests

BeautifulSoup

newspaper3k

This enables automatic analysis of live web articles.

2. Text Analysis Engine

Detects:

Fake or misleading content

AI-generated text

Models:

Transformer-based classifier (RoBERTa / DeBERTa)

Sentence-Transformer embeddings

Framework:

PyTorch

Hugging Face Transformers

3. Image Analysis Engine

Pipeline:

Generate CLIP embedding

Detect AI-generated image patterns

Extract embedded text via OCR

Run extracted text through fake classifier

Libraries:

torchvision

timm

open_clip

easyocr

4. Video / Reel Analysis Engine

Pipeline:

Extract audio from video

Transcribe using Whisper

Run transcript through fake classifier

Sample keyframes

Run AI-image detection on frames

Aggregate results

Libraries:

moviepy

opencv-python

faster-whisper

5. Vector Similarity Engine

Purpose:

Compare content against known fake corpus

Compare against trusted sources

Technology:

Sentence-Transformers

CLIP embeddings

PostgreSQL with pgvector

Cosine similarity search

6. Risk Aggregation Engine

Final risk calculation:

risk =
0.35 * fake_score +
0.20 * ai_text_score +
0.20 * ai_image_score +
0.25 * similarity_score

Risk Levels:

< 0.3 → LOW

0.3–0.6 → MEDIUM

0.6 → HIGH

Chrome Extension

The browser extension allows users to detect misinformation directly on webpages.

Functionality:

Captures current page text

Sends data to backend

Displays risk score

Shows AI-detection probability

Popup UI:

Detect the False!

Risk Score: 0.72
Label: HIGH
Likely AI Generated: Yes

The extension communicates with the FastAPI backend via REST API.

Tech Stack
Backend

Python 3.10+

FastAPI

Uvicorn

SQLAlchemy

Alembic

Database

PostgreSQL 15

pgvector extension

Machine Learning

torch

transformers

sentence-transformers

Vision

torchvision

timm

open_clip

OCR

easyocr

Video Processing

moviepy

opencv-python

Speech-to-Text

faster-whisper

Scraping

requests

beautifulsoup4

newspaper3k

Infrastructure

Docker

Docker Compose

Render (deployment)

Frontend

HTML

CSS

JavaScript

Chrome Extension APIs

Project Structure
ECHO/
│
├── app/
│   ├── main.py
│   ├── api/
│   ├── services/
│   │   ├── scraper.py
│   │   ├── text_engine.py
│   │   ├── image_engine.py
│   │   ├── video_engine.py
│   │   ├── similarity_engine.py
│   │   └── risk_engine.py
│   ├── models/
│   ├── db/
│   └── core/
│
├── extension/
│   ├── manifest.json
│   ├── popup.html
│   ├── popup.js
│   └── content.js
│
├── ml_models/
├── alembic/
├── Dockerfile
├── docker-compose.yml
├── requirements.txt
└── README.md
API Design
POST /analyze

Accepts:

text

image file

video file

url

Returns:

{
  "risk_score": 0.72,
  "risk_label": "HIGH",
  "fake_score": 0.81,
  "ai_text_score": 0.65,
  "ai_image_score": 0.49,
  "similarity_score": 0.76
}
How This Project Was Built (Step-by-Step)

Designed multimodal architecture.

Implemented FastAPI backend.

Integrated PostgreSQL with pgvector.

Added text fake detection using Transformers.

Added AI-text detection.

Integrated CLIP for image embeddings.

Added OCR pipeline for image-based misinformation.

Integrated Whisper for video speech detection.

Implemented similarity search using pgvector.

Designed unified risk scoring engine.

Dockerized backend and database.

Deployed to Render.

Built Chrome extension to connect to API.

Installation
Clone Repository
git clone https://github.com/yourusername/ECHO.git
cd ECHO
Run Locally with Docker
docker-compose up --build
Deployment

Platform: Render

Push repository to GitHub

Create Web Service

Attach PostgreSQL

Set environment variables

Deploy

Start command:

uvicorn app.main:app --host 0.0.0.0 --port $PORT
Use Cases

Journalists verifying content authenticity

Students checking article credibility

Social media users detecting AI-generated posts

Fact-checking workflows

Media literacy tools
