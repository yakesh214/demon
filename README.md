# LegalEase — AI-Powered Legal Document Generator

A complete implementation of the supplied LegalEase specification: Streamlit frontend, FastAPI backend, Gemini AI generation, editable preview, optional logo branding, and TXT/DOCX/PDF export.

## Important compatibility note

The supplied PDF specifies `gemini-1.5-pro` and the older `google-generativeai` package. Google's current documentation recommends the unified `google-genai` SDK. This project therefore uses `google-genai` and keeps `GEMINI_MODEL` configurable in `.env`.

## VS Code / Windows setup

Open this folder in VS Code, then Terminal → New Terminal:

```powershell
py -3.10 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
Copy-Item .env.example .env
```

Edit `.env` and set your Gemini key:

```env
GEMINI_API_KEY=your_real_key_here
GEMINI_MODEL=gemini-3.8-flash
```

### Start backend

Terminal 1:

```powershell
.\.venv\Scripts\Activate.ps1
uvicorn backend.main:app --reload
```

Open `http://127.0.0.1:8000/docs` for Swagger and `http://127.0.0.1:8000/health` for health status.

### Start frontend

Terminal 2:

```powershell
.\.venv\Scripts\Activate.ps1
streamlit run frontend/app.py
```

Open `http://localhost:8501` if the browser does not open automatically.

## Test

```powershell
pytest -q
```

Tests cover root/health and TXT/DOCX/PDF exports without consuming Gemini API calls.

## Workflow

1. Enter document type, parties, terms separated by semicolons, and effective date.
2. Optionally upload a PNG/JPEG/WEBP logo.
3. Generate with Gemini.
4. Edit the generated text in the browser.
5. Download TXT, DOCX or PDF.

## Docker

Create `.env`, then:

```powershell
docker compose up --build
```

Backend: `http://localhost:8000`  
Frontend: `http://localhost:8501`

## API

`POST /generate` accepts `document_type`, `parties`, `terms`, and `effective_date` and returns generated document text.

`POST /export/txt`, `/export/docx`, and `/export/pdf` export edited text.

`POST /upload-logo` validates a logo upload and returns base64 data for branding.

## Legal disclaimer

LegalEase is an AI-assisted drafting and information tool, not a law firm or substitute for qualified legal advice. Generated documents may contain errors, omissions, or jurisdiction-specific issues and should be reviewed before signing or relying on them.
