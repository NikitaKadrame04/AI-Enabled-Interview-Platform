<div align="center">

# 🎤 AI-Enabled Interview Performance & Soft Skills Evaluation Platform

*Practice interviews with an AI interviewer, then get explainable feedback on what you said, how you said it, and how you came across.*

![status](https://img.shields.io/badge/status-in%20development-orange)
![license](https://img.shields.io/badge/license-MIT-green)

</div>

---

## Table of Contents
1. [About the Project](#1-about-the-project)
2. [The Problem](#2-the-problem)
3. [How It Works](#3-how-it-works)
4. [Features and Modules](#4-features-and-modules)
5. [Technologies Used](#5-technologies-used)
6. [System Architecture](#6-system-architecture)
7. [How Scoring Works](#7-how-scoring-works)
8. [Scope](#8-scope)
9. [Getting Started](#9-getting-started)
10. [Privacy, Ethics and Limitations](#10-privacy-ethics-and-limitations)
11. [Project Status](#11-project-status)
12. [License](#12-license)

---

## 1. About the Project

This is a web platform that acts as a personal interview coach for students. It reads a candidate's resume
and a job description, conducts a **spoken mock interview** with role-specific questions and **adaptive
follow-ups**, analyses each answer across **content, speech and facial behaviour**, and produces an
**AI-estimated confidence score** with a transparent breakdown, plain-language explanations and a
**personalised improvement roadmap**.

> ⚠️ Confidence and nervousness scores are AI-estimated behavioural indicators meant for practice.
> They are **not** psychological assessments and must not be used for hiring decisions.

## 2. The Problem

Many students struggle in interviews not because they lack knowledge, but because they lack practice and
specific feedback on communication and confidence. Human mock interviews are hard to arrange. Existing
practice tools often give only a single score or generic advice such as "be more confident", which does
not show what to change. This platform aims to give detailed, explainable feedback instead: which
measurements led to each score, and which exercise improves them.

## 3. How It Works

```
1.  Sign up and set a profile (target role, experience level, preferred interview type)
2.  Upload a resume (PDF or DOCX) and optionally paste a job description
3.  The system extracts skills, scores the resume-JD match and finds skill gaps
4.  AI generates role-specific questions (technical, HR, behavioural, situational)
5.  In the interview room, the AI asks each question aloud and the candidate answers on camera and mic
6.  After each answer, the AI may ask a follow-up based on what was said
7.  Each answer is analysed in three ways:
      Speech   : transcript, pace, pauses, filler words, tone
      Content  : relevance to the question, vocabulary, structure
      Face     : eye contact, expressions, head movement
8.  The signals are combined into a confidence score using documented weights
9.  The report explains each score, lists strengths and weaknesses, and suggests exercises
10. A dashboard tracks progress across sessions
```

## 4. Features and Modules

| # | Module | What it does |
|---|--------|--------------|
| 1 | User & Candidate Profile | Registration, login, profile, resume upload, interview history |
| 2 | Resume & JD Analysis | Resume parsing, skill extraction, resume-JD match score, skill-gap detection |
| 3 | AI Mock Interview | Role-specific questions, adaptive difficulty, spoken follow-ups |
| 4 | Multimodal Analysis | Speech-to-text, pace, pauses, filler words, tone, answer relevance, eye contact, expressions |
| 5 | Confidence & Nervousness Assessment | Transparent weighted scoring that combines all signals |
| 6 | Explainable AI Feedback | Per-measurement score breakdown, strengths, weaknesses, plain-language reasons |
| 7 | Personalised Improvement Roadmap | Targeted exercises, trend tracking, progress dashboard |

## 5. Technologies Used

| Area | Technologies |
|------|--------------|
| Frontend | React, Vite, Tailwind CSS, Recharts |
| Backend | Python 3.11, FastAPI, Uvicorn, SQLAlchemy |
| Database | SQLite (development), PostgreSQL (optional for deployment) |
| Authentication | JWT, bcrypt |
| Resume parsing | pdfplumber, python-docx |
| NLP | sentence-transformers, spaCy, scikit-learn, NLTK (VADER), LanguageTool |
| Speech | faster-whisper (Whisper), librosa, parselmouth, Web Speech API (live captions) |
| Computer vision | MediaPipe Face Landmarker, OpenCV |
| LLM | Google Gemini free tier (swappable with Groq or Ollama through one wrapper file) |
| Text-to-speech | Browser Speech Synthesis API |
| Tooling | Git, GitHub (Issues, Projects, Pull Requests), pytest, FFmpeg |

All technologies are free or open source.

## 6. System Architecture

```
  Browser (React)                                     Server (FastAPI)
 ┌──────────────────────────────┐                  ┌───────────────────────────────┐
 │ Camera + Microphone          │                  │ Auth, profile, history        │
 │  ├─ MediaPipe (face metrics) │   transcript +   │ Resume and JD analysis        │
 │  ├─ Web Audio (tone, pauses) │   metrics + audio│ LLM: questions and follow-ups │
 │  ├─ Live speech-to-text      │ ───────────────► │ faster-whisper + librosa      │
 │  └─ Text-to-speech           │                  │ Scoring and explanations      │
 │ Interview state machine      │ ◄─────────────── │ Roadmap and dashboard data    │
 └──────────────────────────────┘  follow-up/next  └───────────────┬───────────────┘
                                                                    ▼
                                                           SQLite / PostgreSQL
```

Live measurements in the browser drive the interface and the follow-up questions. After each answer the
server re-analyses the recorded audio with faster-whisper and librosa, and those more accurate numbers
are used in the final report.

## 7. How Scoring Works

Each answer is scored from 0 to 100 on six components, then combined with fixed, documented weights:

| Component | Weight | Based on |
|-----------|--------|----------|
| Content relevance and depth | 30% | Similarity to the ideal answer points, structure score |
| Speech fluency | 25% | Pace, filler words, pauses |
| Clarity and vocabulary | 15% | Clarity score, vocabulary richness |
| Eye contact and engagement | 15% | Eye-contact percentage, face visibility |
| Voice modulation | 10% | Pitch and volume variation |
| Composure | 5% | Blink rate, head movement, brow tension |

If a signal is unavailable (for example in text-only mode), that component is removed and the remaining
weights are renormalised. Readiness bands: 0-39 Needs significant practice, 40-59 Developing,
60-79 Good, 80-100 Interview-ready.

A transparent weighted model was chosen over a black-box model on purpose. Every score can be traced to
named measurements, which is the core requirement for explainable feedback.

## 8. Scope

**In scope:** web application, English-language interviews, student users, resume and JD analysis,
spoken mock interviews, multimodal analysis, explainable scoring, roadmap and progress tracking.

**Out of scope for now (future work):** mobile app, languages other than English, and an admin dashboard
for institutions and placement cells.

## 9. Getting Started

> 🚧 The project is at an early stage. Setup and run instructions will be added here as each part
> of the application is built. The sections below list what will be needed.

**Requirements**
- Python 3.11 (newer versions may not support some ML libraries)
- Node.js 18 or newer
- Git
- FFmpeg
- Chrome or Edge (live speech recognition needs a Chromium browser)
- A webcam, a microphone and at least 8 GB RAM
- A free LLM API key, for example from Google AI Studio

**Setup steps (to be filled in as the project grows)**
1. Clone the repository
2. Set up the backend (virtual environment, `pip install -r requirements.txt`, `.env` file, run the server)
3. Set up the frontend (`npm install`, `npm run dev`)

## 10. Privacy, Ethics and Limitations

- Facial analysis runs in the browser. Video is not uploaded or stored.
- Users give explicit consent before recording and can delete their data at any time.
- Accents, background noise and poor microphones reduce transcription accuracy.
- Lighting, camera quality and individual differences affect face tracking. People express confidence in
  different ways, so scores are guidance and not judgement.
- "Pronunciation clarity" is a proxy based on recognition confidence, not a phoneme-level assessment.
- Live speech recognition in Chrome may send audio to the browser vendor's service.

## 11. Project Status

- [ ] Module 1: User & Candidate Profile
- [ ] Module 2: Resume & JD Analysis
- [ ] Module 3: AI Mock Interview
- [ ] Module 4: Multimodal Analysis
- [ ] Module 5: Confidence & Nervousness Assessment
- [ ] Module 6: Explainable AI Feedback
- [ ] Module 7: Personalised Improvement Roadmap

## 12. License

Released under the MIT License. See [`LICENSE`](LICENSE).