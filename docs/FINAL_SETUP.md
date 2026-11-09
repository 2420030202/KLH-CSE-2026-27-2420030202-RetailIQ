# RetailIQ V1.0 — Final Frontend Setup

This package is a **complete RetailIQ project**, not a frontend-only folder.

## Structure

- `app.py` — premium Streamlit frontend
- `src/` — existing RetailIQ analytics/ML engines
- `assets/retailiq_logo.svg` — new branding
- `requirements.txt` — dependencies
- `Dockerfile` — container build
- `RUN_RETAILIQ.bat` — Windows launcher

## Run in VS Code

Open the repository root as the VS Code workspace. It contains both `app.py` and the backend package in `src/`.

Then run:

```powershell
python -m pip install -r requirements.txt
python -m streamlit run app.py
```

On Windows, `RUN_RETAILIQ.bat` can also be started from the repository root.

## Important

Do **not** copy only `app.py` into a separate empty folder. The frontend imports the existing backend through `src.*`.

Launch the application from the repository root so its data and results paths resolve consistently.
