# ResearchRadar — Windows setup (Python 3.14.8)

## 1. Requirements
- Python 3.14.8
- Node.js 20 or newer
- MySQL Server 8.0 or newer, running locally
- A valid Groq API key for AI recommendations, assistant, and PDF AI analysis

## 2. Configure MySQL and Groq
1. Copy `backend/.env.example` to `backend/.env`.
2. Enter your MySQL password in `DB_PASSWORD`.
3. Paste your Groq key into `GROQ_API_KEY`. Never share this file or upload it publicly.
4. Keep `DB_NAME=researchhub` unless you intentionally use another database.

## 3. Create the Python environment and database
Open PowerShell in the project folder and run:

```powershell
cd backend
py -3.14 -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
python setup_database.py
python run.py
```

If PowerShell blocks activation, use this command in the same terminal (it affects only that terminal):

```powershell
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
.\.venv\Scripts\Activate.ps1
```

The API runs at `http://127.0.0.1:5000`. Open `http://127.0.0.1:5000/api/health` to check database and AI-key configuration.

## 4. Start the existing frontend
Open a second PowerShell window in the project folder:

```powershell
cd frontend
npm install
npm run dev
```

Open the local URL printed by Vite (normally `http://127.0.0.1:5173`). The frontend source and styling are retained; Rollup is configured to use its WebAssembly build instead of the Windows native `.node` binary that your device policy blocked.

## 5. Collect real papers
1. Sign up or log in.
2. Open **Paper collection**.
3. Click **Run paper collection**.
4. Wait for arXiv, OpenAlex, Crossref, and PubMed results to be imported into MySQL.

Collection needs working internet access and a running MySQL server. Some sources may temporarily fail or rate-limit; the collection report records each source separately.

## Feature requirements
- Search, similar-paper discovery, saved papers, and library: require MySQL records.
- AI recommendations and AI Research Assistant: require a valid Groq key and available model quota.
- PDF reader: text extraction/OCR happens in the browser; AI paper analysis requires login, MySQL, and Groq. The personal PDF library is stored in the browser's IndexedDB, not in MySQL.
- Help Desk: the selected frontend currently provides built-in help information, not a ticket-submission system.

## Troubleshooting
- `Database: Not connected`: start MySQL, verify `DB_HOST`, `DB_PORT`, `DB_USER`, `DB_PASSWORD`, then rerun `python setup_database.py`.
- `GROQ_API_KEY is missing`: set it in `backend/.env`, stop and restart the backend.
- Paper collection source errors: inspect `backend/data/last_collection.json`; a source may be temporarily unavailable or rate-limited.
- If your organization's application-control policy blocks a file, do not disable the policy. The frontend package uses the Rollup WebAssembly implementation to avoid the blocked native Rollup binary.
