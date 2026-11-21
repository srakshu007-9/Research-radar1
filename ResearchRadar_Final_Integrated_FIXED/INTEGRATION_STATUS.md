# ResearchRadar integration status (read before running)

## Main frontend selected
The frontend in `frontend/` is copied from the user's selected `ResearchRadar_AI_PDF_Reader_Improved` source. Its PDF extraction/OCR and browser IndexedDB library code are retained.

## Main backend
The Flask backend in `backend/` provides these routes:
- `GET /api/health`
- `POST /api/auth/signup`, `POST /api/auth/login`, `GET /api/auth/me`, `POST /api/auth/logout`
- `GET /api/papers`, `GET /api/papers/<id>`, `/api/saved`
- `GET /api/recommendations`
- `POST /api/assistant/chat`, `GET /api/assistant/history`
- `POST /api/reader/analyze`, `GET /api/reader/history`
- `POST /api/collection/run`, `GET /api/collection/status`

The Vite dev server proxies `/api` to the Flask backend at port 5000.

## Important limitations — do not mistake these for verified features
The selected frontend currently contains demonstration paper records and mock login behavior in `src/App.jsx`. Its visible similar-paper search, saved-paper interactions and login screen are not yet wired to the Flask routes. The PDF reader currently calls `/api/analyze-paper`, while the Flask route is `/api/reader/analyze`; that endpoint mismatch must be fixed before PDF AI analysis works. The selected frontend does not currently contain a connected AI Research Assistant, Help Desk, or live collection screen. The backend routes exist for several of these capabilities, but route existence does not mean the selected UI is connected.

The Groq helper uses the OpenAI-compatible endpoint and defaults to `openai/gpt-oss-20b`; a valid key, available model access, internet, and quota are required. Live Groq calls have not been tested here.

## Preserve originals
`source_modules/` contains preserved source modules from the supplied project versions. They are retained for integration/reference; they are not all active runtime services. In particular, the standalone AI recommendation project is not automatically connected to the Flask runtime.

## Verification performed
- Python syntax compilation for backend `.py` files.
- ZIP archive integrity.
- No live MySQL, Groq, external scraping, browser UI, or end-to-end acceptance test was possible in this environment.
