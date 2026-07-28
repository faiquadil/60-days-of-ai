# AI Interview Coach — Project Structure

**Version:** 1.1 | **Last updated:** Day 3 of 10

> **Day 3 update:** `core/models.py`, `core/ai_provider.py` (with a working `MockProvider` and scaffolded `ClaudeProvider`), and `core/db.py` are now implemented and verified working. `core/services.py` remains reserved for Day 4, as originally planned on Day 2.

## Final Folder Structure

```
ai-interview-coach/
├── app.py
├── core/
│   ├── ai_provider.py
│   ├── prompts.py
│   ├── services.py        ← reserved (Day 2 design decision; not yet implemented — scheduled for Day 4)
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

- `ai_provider.py` — defines the `AIProvider` interface and the `ClaudeProvider` implementation. The only file that talks to the Anthropic API.
- `prompts.py` — the two master prompt templates (question generation, answer evaluation), kept separate from the calling code so they're easy to iterate on.
- `services.py` — **reserved, not yet implemented.** The internal "API" layer (see API.md), designed on Day 2. As of Day 3, `app.py`'s demo code calls `MockProvider` directly as a quick foundation check; this file will be created on Day 4 so all future UI pages go through it instead of calling `ai_provider.py`/`db.py` directly. This keeps `pages/*.py` thin and keeps business logic centralized and testable.
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
