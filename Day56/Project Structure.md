# AI Interview Coach — Project Structure

**Version:** 1.4 | **Last updated:** Day 6 of 10

> **Day 6 update:** `pages/history.py` and `pages/dashboard.py` are fully implemented, along with `ui/dashboard_charts.py` and `ui/footer.py` (required footer added to all 5 pages). `core/db.py` gained `get_all_sessions()`, `get_session_full_detail()`, `get_dashboard_stats()`. `core/services.py` gained `get_session_history()`, `get_session_detail()`, `get_dashboard_stats()`, and was updated to read `GROQ_API_KEY` from Streamlit Secrets (for deployment) with a local `.env` fallback.
>
> **✅ RESOLVED (Day 7):** After extensive debugging (documented below), the app is confirmed **live and working** at `ai-interview-coach-faiquadil.streamlit.app`. Home page, sidebar navigation, all action buttons, and the required footer are all visually confirmed on the deployed site. Day 6 is now fully closed. See the Day 7 summary for the root cause and final fix.

## Final Folder Structure

```
ai-interview-coach/
├── app.py
├── core/
│   ├── ai_provider.py
│   ├── prompts.py
│   ├── services.py        ← implemented Day 4 (start_session orchestration)
│   ├── db.py
│   ├── models.py
│   └── resume_parser.py
├── ui/
│   ├── feedback_card.py
│   └── dashboard_charts.py
├── pages/
│   ├── new_session.py
│   ├── practice_session.py
│   ├── history.py
│   └── dashboard.py
├── data/
│   └── interview_coach.db      (created at runtime, not committed)
├── tests/
│   ├── test_db.py
│   └── test_ai_provider.py
├── screenshots/                (for README + portfolio images)
├── .streamlit/
│   └── config.toml             (theme, added Day 7)
├── .env.example
├── .gitignore
├── requirements.txt
└── README.md
```

## What Each Folder Is Responsible For

### `app.py`
The Streamlit entry point. Sets page config, renders the sidebar navigation, and routes to the Home view. Does not contain business logic — only page routing and layout.

### `core/` — the "brain" of the app
Everything here is plain Python, with zero Streamlit imports. This is deliberate: it means the AI logic, database logic, and orchestration logic could theoretically be tested or reused without Streamlit at all.

- `ai_provider.py` — defines the `AIProvider` interface. **As of Day 5:** `GroqProvider` (real, free AI via Groq's API, using Llama 3.3 70B) is the live implementation; `MockProvider` remains as a permanent offline fallback for fast local testing. The originally-planned `ClaudeProvider` was replaced with `GroqProvider` on Day 5 to keep the entire project free to run — see the Blueprint Addendum for full rationale. The `AIProvider` interface itself never changed, confirming the value of the Day 2 abstraction design.
- `prompts.py` — the two master prompt templates (question generation, answer evaluation), kept separate from the calling code so they're easy to iterate on.
- `services.py` — orchestration layer. `start_session()` (Day 4) generates + saves questions. **New Day 5:** `submit_answer()` saves an answer and gets real AI feedback; `complete_session()` computes and saves the overall score. `get_ai_provider()` now checks for `GROQ_API_KEY` instead of an Anthropic key.
- `db.py` — all SQLite reads/writes. No other file should import `sqlite3` directly.
- `models.py` — lightweight dataclasses (`Session`, `Question`, `Answer`, `Feedback`) used to pass structured data between layers instead of raw dicts.
- `resume_parser.py` — extracts text from an uploaded resume PDF using `pypdf`.

### `ui/` — reusable Streamlit components
Small, focused rendering functions used across multiple pages, so feedback cards and charts look identical everywhere they appear.

- `feedback_card.py` — renders one structured feedback object as a Streamlit card.
- `dashboard_charts.py` — renders the score trend chart and stat metrics.

### `pages/` — one file per screen
Each file corresponds to one screen from UI-WIREFRAMES.md. These files should be short: gather input, call one `services.py` function, render the result using `ui/` components.

### `data/`
Holds the SQLite database file at runtime. **Not committed to git** — the schema is created fresh via `db.py`'s init function on first run, so anyone cloning the repo gets a clean database automatically.

### `tests/`
Automated tests (added Day 8) for `db.py` and the JSON-parsing safety logic in `ai_provider.py`. AI-dependent behavior is tested with mocked responses, not live API calls, so tests run without needing a real API key.

### `screenshots/`
Final polished screenshots captured across Days 3–10, used in the README and for the LinkedIn/portfolio post.

### `.streamlit/config.toml`
Theme customization (colors, fonts) added during the Day 7 polish pass.

### Root-level files
- `.env.example` — documents the expected environment variable (`ANTHROPIC_API_KEY`) without exposing a real key.
- `.gitignore` — excludes `.env`, `data/*.db`, `__pycache__/`, and virtual environment folders.
- `requirements.txt` — pinned dependency versions for reproducible installs and deployment.
- `README.md` — project overview, setup instructions, live demo link (finalized Day 10).

## Why This Structure Was Chosen

1. **Layer separation mirrors the architecture** (UI / Service / AI / Data) — anyone reading the folder names understands the system design without opening a single file.
2. **`core/` has no Streamlit dependency** — this is a deliberate constraint that keeps business logic portable and unit-testable, which pays off directly on Day 8 (Testing).
3. **One file per screen in `pages/`** — matches Streamlit's natural multipage app conventions and keeps each file's job obvious.
4. **Nothing is over-engineered** — no `services/` package with dozens of files, no premature abstraction layers beyond what Day 2–9 actually need. This structure supports exactly the features in the PRD — nothing more.
