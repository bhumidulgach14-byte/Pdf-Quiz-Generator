# PDF-Based AI Quiz Generator

A clean, beginner-friendly web application developed using **Python, Django, SQLite, and Google Gemini AI**. It extracts text from any uploaded PDF and generates a 15-question multiple choice quiz divided into **Easy, Medium, and Hard** difficulty levels.

---

## 🌟 Key Features

1. **Clean Homepage & PDF Upload**
   - Drag-and-drop or file picker for uploading PDF files.
   - Strict validation: only valid `.pdf` files up to 10MB accepted.
   - Rejects empty, corrupted, or scanned PDFs with insufficient text (< 50 words).

2. **PyMuPDF Text Extraction**
   - High-performance text extraction using **PyMuPDF (`fitz`)** with automatic fallback to **`pypdf`**.

3. **AI-Powered 15-Question Generation**
   - Strict factual grounding: generates questions **only** from the uploaded PDF content.
   - 3 Difficulty Levels:
     - **Easy**: 15 questions focusing on direct facts, definitions, and terminology.
     - **Medium**: 15 questions focusing on conceptual relationships and comparisons.
     - **Hard**: 15 questions focusing on deep analytical reasoning and edge cases.
   - Each question has 4 options (A, B, C, D) with exactly one correct answer.

4. **One-Question-At-A-Time Quiz Interface**
   - Displays 1 question per screen.
   - Previous, Next, and Submit buttons.
   - Dynamic 1–15 Question Navigator pills showing answered/unanswered status.
   - Real-time progress bar.

5. **Instant Result & Scorecard**
   - Score percentage and status (Pass/Fail).
   - Metrics breakdown: Total (15), Correct, Incorrect, Unanswered.
   - Complete review: Shows chosen answer vs correct answer and source document explanation.
   - Stored in lightweight SQLite database (`QuizRecord` table).

---

## 📁 Project Structure

```text
pdf_quiz_generator/
│── manage.py                   # Django CLI management script
│── requirements.txt            # Python dependencies
│── .env.example                # Template for environment variables
│── .env                        # Local configuration (API Key & Secret Key)
│── db.sqlite3                  # Lightweight SQLite database
│── README.md                   # Complete documentation
│
├── quiz_project/               # Django Project Settings
│   ├── __init__.py
│   ├── settings.py             # App registration, SQLite, Session, Static setup
│   ├── urls.py                 # Root URL routing
│   ├── wsgi.py                 # WSGI entry point for web servers
│   └── asgi.py                 # ASGI entry point
│
└── quiz_app/                   # Main Django Application
    ├── __init__.py
    ├── apps.py                 # App configuration
    ├── models.py               # SQLite model: QuizRecord
    ├── admin.py                # Django Admin registration
    ├── forms.py                # PDF upload form validation
    ├── pdf_extractor.py        # PyMuPDF / fitz text extraction & validation
    ├── ai_generator.py         # Google Gemini AI quiz generator (strictly from PDF)
    ├── quiz_service.py         # Quiz session management & state flow
    ├── result_calculator.py    # Score, percentage, and review compiler
    ├── urls.py                 # Application routes (/quiz/, /submit/, /result/)
    ├── views.py                # Controllers for HTTP requests
    │
    ├── static/
    │   ├── css/
    │   │   └── style.css       # Clean, modern, responsive CSS
    │   └── js/
    │       └── quiz.js         # Single-question navigation & state
    │
    └── templates/
        └── quiz_app/
            ├── base.html       # Base layout with navbar & alerts
            ├── home.html       # Home page & PDF upload
            ├── quiz.html       # Quiz taking interface
            └── result.html     # Scorecard & answer explanations
```

---

## 🚀 Quick Setup & Run Instructions

### Step 1: Open Terminal in the Project Directory
```powershell
cd C:\Users\Prabh\.gemini\antigravity\scratch\pdf_quiz_generator
```

### Step 2: Install Dependencies
```bash
pip install -r requirements.txt
```

### Step 3: Configure Environment Variables
Copy `.env.example` to `.env` if not already present:
```bash
cp .env.example .env
```
Open `.env` and add your **Free-Tier Gemini API Key**:
```env
GEMINI_API_KEY=AIzaSy...your_gemini_key_here
```
*(Get a free Gemini API key in 30 seconds at: [Google AI Studio](https://aistudio.google.com/app/apikey))*

> **Note for Offline Viva / Demo:** If no API key is provided, the project includes an automatic built-in document heuristic fallback that still generates 15 valid questions from the PDF sentences without crashing!

### Step 4: Run Migrations
```bash
python manage.py migrate
```

### Step 5: Start the Development Server
```bash
python manage.py runserver
```

Open your browser and navigate to:
**http://127.0.0.1:8000/**

---

## 🎯 Viva & Presentation Guide (For MCA Semester Project)

- **Q: Why PyMuPDF over standard libraries?**
  *A:* PyMuPDF (`fitz`) is written in C (MuPDF engine) and is up to 10–20x faster than traditional pure-Python parsers. It accurately preserves text flow and handles multi-column layouts.
- **Q: How does the AI ensure questions are not generated from outside knowledge?**
  *A:* The system prompt strictly bounds the Gemini model with a closed-domain instruction ("Do NOT extrapolate or introduce external knowledge. Every question must be directly answerable from the provided text").
- **Q: Why use Django Sessions instead of complex user tables?**
  *A:* For a smooth, login-free user experience, Django sessions maintain state during the quiz, while finalized attempts are recorded in SQLite (`QuizRecord`) for auditing.
