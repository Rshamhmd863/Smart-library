# Smart Library - GitHub + Render deployment

This package is a single repository with two deployable folders:
- `frontend/` -> Render Static Site
- `backend/` -> Render Web Service

## Recommended GitHub layout
Create one repository named `smart-library`, upload both folders, and push the whole project.
This is a monorepo; Render can deploy each folder separately by setting its Root Directory.

## Deployment order
1. Push this project to GitHub.
2. Deploy `backend` as a Render Web Service:
   - Root Directory: `backend`
   - Build: `pip install -r requirements.txt`
   - Start: `gunicorn app:app`
   - Environment variables:
     - `GEMINI_API_KEY` = your private Gemini key
     - `GEMINI_MODEL` = `gemini-2.5-flash`
     - `SECRET_KEY` = a long random secret
     - `ALLOW_LOCAL_FRONTEND` = `false`
     - `CORS_ORIGINS` = exact frontend URL, e.g. `https://smart-library-ui.onrender.com`
     - `DATABASE_PATH` = `/var/data/library.db`
   - Attach a persistent disk mounted at `/var/data`.
3. Deploy `frontend` as a Render Static Site:
   - Root Directory: `frontend`
   - Build Command: leave empty
   - Publish Directory: `.`
4. Copy the actual backend URL from Render and update `frontend/config.js`:
   `window.API_BASE = "https://YOUR-ACTUAL-BACKEND.onrender.com";`
   Commit and push the change.
5. Once the frontend URL is known, set that exact origin in backend `CORS_ORIGINS` and redeploy the backend.

## Important notes
- Never upload `.env`, `library.db`, API keys, or passwords to GitHub.
- `.env.example` is only a template; it contains no real key.
- Render's local filesystem is not persistent by default. The persistent disk is important for this project's SQLite database.
- `app.py` still contains the original page-serving routes, but the separate frontend deployment uses the Static Site; the browser API calls go to the backend URL configured in `frontend/config.js`.
- The video file is kept in the frontend root because `home.html` references it by filename.
