# AI Interview Coach — Setup Guide

**Version:** 1.0 | **Day:** 3 of 10

This guide lets anyone (including a future you, or a recruiter reviewing your GitHub repo) set up and run AI Interview Coach locally from scratch.

## Prerequisites

| Tool | Version | Why |
|---|---|---|
| Python | 3.11+ (tested on 3.13.4) | Runtime for the whole app |
| Git | Any recent version | Version control |
| VS Code (recommended) | Any recent version | Editor, with Python + Pylance extensions |

## 1. Clone the Repository

```
git clone https://github.com/faiquadil/ai-interview-coach.git
cd ai-interview-coach
```

## 2. Create and Activate a Virtual Environment

**Why:** Isolates this project's dependencies from your system Python.

Windows:
```
python -m venv venv
venv\Scripts\activate
```

Mac/Linux:
```
python3 -m venv venv
source venv/bin/activate
```

You should see `(venv)` at the start of your terminal prompt when active.

## 3. Install Dependencies

```
pip install -r requirements.txt
```

**Known issue (Windows + Python 3.13):** `pandas==2.2.2` fails to install because it lacks a pre-built wheel for Python 3.13, and tries to compile from source (requiring Visual Studio build tools). **Fix:** the pinned version in this repo is already `pandas==2.2.3`, which has a pre-built Windows wheel. If you see a `meson`/`vswhere.exe` error, confirm your `requirements.txt` has `pandas==2.2.3`, not `2.2.2`.

## 4. Configure Environment Variables

1. Copy the example file:
   ```
   copy .env.example .env        (Windows)
   cp .env.example .env          (Mac/Linux)
   ```
2. Open `.env` and add your Anthropic API key:
   ```
   ANTHROPIC_API_KEY=your-real-key-here
   ```

> **No API key yet?** That's fine — the app currently runs on `MockProvider` (see ENVIRONMENT.md), which needs no key at all. You can develop the entire UI, database, and analytics layers before ever touching the real Claude API. Swap to `ClaudeProvider` in `app.py` once you have a key.

## 5. Run the App

```
streamlit run app.py
```

This opens `http://localhost:8501` in your browser. You should see the AI Interview Coach home page with a sidebar for navigation.

## 6. Verify Everything Works

- [ ] Home page loads without errors
- [ ] Sidebar shows navigation options
- [ ] "New Session (preview)" generates 5 mock questions when you click the button
- [ ] No red error tracebacks appear in the terminal or browser

If all four are true, your environment is fully set up and ready for feature development.

## Troubleshooting

| Problem | Likely Cause | Fix |
|---|---|---|
| `streamlit: command not found` | Virtual environment not activated | Run `venv\Scripts\activate` again |
| `pandas` build error | Wrong pandas version pinned | Ensure `requirements.txt` has `pandas==2.2.3` |
| Blank page in browser | App still starting | Wait a few seconds, refresh |
| `ModuleNotFoundError` | Dependencies not installed in active venv | Re-run `pip install -r requirements.txt` with `(venv)` active |
