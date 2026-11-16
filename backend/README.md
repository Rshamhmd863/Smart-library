# Smart Library - Backend

Flask REST API, SQLite database, authentication and Gemini-powered chat.

## Run locally
```bash
python -m venv .venv
# Windows: .venv\\Scripts\\activate
# Linux/macOS: source .venv/bin/activate
pip install -r requirements.txt
# Copy .env.example to .env and fill in GEMINI_API_KEY and SECRET_KEY
python app.py
```

## Deploy on Render
1. Create a **Web Service** from this GitHub repository.
2. Set **Root Directory** to `backend` if using the combined repository.
3. Build Command: `pip install -r requirements.txt`
4. Start Command: `gunicorn app:app`
5. Add environment variables: `GEMINI_API_KEY`, `GEMINI_MODEL`, `SECRET_KEY`,
   `CORS_ORIGINS`, `ALLOW_LOCAL_FRONTEND=false`, and `DATABASE_PATH=/var/data/library.db`.
6. Attach a Render persistent disk mounted at `/var/data` so SQLite data survives deploys/restarts.
7. In `CORS_ORIGINS`, put the exact deployed frontend origin, e.g.
   `https://smart-library-ui.onrender.com` (no trailing slash).

`library.db` and `.env` must never be committed to GitHub.
