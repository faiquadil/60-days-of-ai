# AI Interview Coach — Environment & Configuration

**Version:** 1.0 | **Day:** 3 of 10

## Environment Variables

| Variable | Required? | Where Used | Notes |
|---|---|---|---|
| `ANTHROPIC_API_KEY` | Not yet (deferred) | `core/ai_provider.py` → `ClaudeProvider` | Not required while running on `MockProvider`. Add to `.env` (local) or Streamlit Secrets (deployed) once available. |

`.env.example` documents this variable without exposing a real value. The real `.env` file is git-ignored and never committed.

## AI Provider Configuration (Day 3 Decision)

Because the Anthropic Console now requires a small prepaid credit balance before issuing a new API key (no guaranteed free trial for all accounts), we deferred real Claude API integration to avoid blocking today's foundation work.

**Current state:**
- `core/ai_provider.py` defines the `AIProvider` interface with two implementations:
  - `MockProvider` — **active today.** Returns realistic, hardcoded question/feedback data with zero network calls or API key.
  - `ClaudeProvider` — implemented but not yet wired into `app.py`; its methods currently raise `NotImplementedError` as placeholders for Day 4/5.
- `app.py` currently instantiates `MockProvider()` directly.

**To activate real Claude integration later (Day 4/5 or beyond):**
1. Add `ANTHROPIC_API_KEY` to `.env`.
2. In `app.py` (and later `core/services.py`), change `MockProvider()` to `ClaudeProvider(api_key=os.environ["ANTHROPIC_API_KEY"])`.
3. Implement the real logic inside `ClaudeProvider.generate_questions()` and `evaluate_answer()` using the prompt templates from `core/prompts.py` (per ARCHITECTURE.md Section 5).
4. No other file needs to change — this is exactly the benefit of the provider-abstraction pattern designed on Day 2.

## Tools Installed

| Tool | Purpose |
|---|---|
| Python 3.13.4 | Runtime |
| VS Code | Editor |
| VS Code Python extension (Microsoft) | IntelliSense, linting, debugging |
| Pylance | Fast type-checking/autocomplete |
| Git | Version control |
| Virtual environment (`venv/`) | Isolated dependency management |

## Python Package Versions (pinned in `requirements.txt`)

```
streamlit==1.38.0
anthropic==0.34.2
python-dotenv==1.0.1
pandas==2.2.3
pypdf==4.3.1
pytest==8.3.2
```

**Note:** `pandas` was bumped from `2.2.2` to `2.2.3` on Day 3 to fix a Windows/Python 3.13 build failure (see SETUP.md Troubleshooting).

## Configuration Files

| File | Purpose | Committed to Git? |
|---|---|---|
| `.env.example` | Documents expected env vars | Yes |
| `.env` | Real secrets (API key) | **No** — git-ignored |
| `.gitignore` | Excludes `.env`, `venv/`, `__pycache__/`, `data/*.db` | Yes |
| `requirements.txt` | Pinned dependencies | Yes |
| `.streamlit/config.toml` | UI theme (added Day 7) | Yes, once created |

## Database Configuration

- Engine: SQLite
- File location: `data/interview_coach.db`
- Created automatically at runtime by `core/db.py`'s `init_db()` function — not committed to git, so every fresh clone starts with a clean database.
