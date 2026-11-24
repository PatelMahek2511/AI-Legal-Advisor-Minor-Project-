# AI Legal Advisor — Full-Stack Starter

An improved frontend + backend starter for a final-year B.Tech CSE project.

## Features in this starter
- Responsive HTML/CSS/JavaScript frontend
- FastAPI backend with `/api/health`, `/api/chat`, `/api/history`, and `/api/documents/extract`
- Local SQLite persistence for question/answer history
- Optional OpenAI-compatible chat model configuration via backend environment variables
- PDF and TXT text extraction (PDF extraction requires a text-based PDF; scanned PDFs need OCR)
- Legal-information disclaimer and basic upload limits

## Important limitation
This is a starter prototype, not a verified legal advice system. If no LLM is configured, the API returns a clearly labelled demo response. The RAG/legal-source retrieval pipeline is **not yet implemented**, and the `sources` array will remain empty until verified-source retrieval is added. Do not present the app as giving authoritative legal advice.

## Requirements
- Python 3.10 or newer
- A modern browser
- Optional: API credentials for a compatible LLM provider

## 1. Start the backend on Windows

Open PowerShell in the `backend` folder:

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
Copy-Item .env.example .env
uvicorn main:app --reload
```

If PowerShell blocks activation, you can run the backend using:

```powershell
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\python.exe -m uvicorn main:app --reload
```

API docs: http://127.0.0.1:8000/docs

## 2. Open the frontend

Keep the backend running. In a second PowerShell terminal, open the `frontend` folder and run:

```powershell
py -m http.server 5500
```

Then open http://127.0.0.1:5500

If you open `index.html` directly as a `file://` URL, browser requests to the backend may be blocked. Use the local HTTP server above.

## 3. Optional AI provider setup

Edit `backend/.env` and fill in the values supported by your chosen OpenAI-compatible provider:

```env
LLM_API_KEY=your_key_here
LLM_BASE_URL=
LLM_MODEL=your_model_name
CORS_ORIGINS=http://127.0.0.1:5500,http://localhost:5500,http://127.0.0.1:8000,http://localhost:8000
```

- `LLM_BASE_URL` can be left blank for a provider using the default OpenAI API endpoint.
- For another compatible provider, use its documented base URL and model name.
- Keep the key in `backend/.env`; never place it in frontend JavaScript or upload it to GitHub.
- Restart Uvicorn after changing `.env`.

Without the three required AI settings (`LLM_API_KEY`, `LLM_MODEL`, and the installed SDK), the app uses demo mode. Demo mode is not an AI answer.

## 4. API endpoints
- `GET /api/health` — backend status and whether AI settings are present
- `POST /api/chat` — JSON body: `{"question":"...","jurisdiction":"India"}`
- `GET /api/history?limit=20` — saved local history
- `DELETE /api/history` — clears all local history
- `POST /api/documents/extract` — multipart upload of PDF/TXT

## 5. Before public deployment
1. Configure CORS with only your actual frontend domain.
2. Add authentication and authorization before storing real user data.
3. Use HTTPS, secure secrets, request limits, monitoring, and backups.
4. Add curated legal documents, embeddings, vector retrieval, source citations, and tests.
5. Never treat AI output as legal advice; have a qualified professional review high-risk content.
6. Do not upload confidential legal documents during this prototype stage.

## Folder structure

```text
ai_legal_advisor_fullstack/
├── backend/
│   ├── main.py
│   ├── requirements.txt
│   ├── .env.example
│   └── .gitignore
├── frontend/
│   └── index.html
└── README.md
```
