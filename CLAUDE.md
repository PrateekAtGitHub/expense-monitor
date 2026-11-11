# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Spendly — a Flask + SQLite personal expense tracker (amounts in ₹). It is a step-by-step teaching project: much of the app is intentionally stubbed, with comments like "students will implement this in Step N". Build features in that step order and keep the existing stub structure unless asked to change it.

## Commands

The virtualenv lives in `expenseEnv/` (Python 3.14, gitignored).

```bash
source expenseEnv/bin/activate
pip install -r requirements.txt
python app.py                     # dev server with debug on, http://localhost:5001 (not Flask's default 5000)
pytest                            # run all tests (pytest + pytest-flask installed; no tests exist yet)
pytest tests/test_x.py::test_name # run a single test
```

## Architecture

- `app.py` — the single Flask app module; all routes live here. Implemented routes only render templates (`/`, `/login`, `/register`, `/terms`, `/privacy`). Placeholder routes return strings and map to the build plan:
  - Step 1: `database/db.py` — `get_db()` (SQLite connection with `row_factory` and foreign keys enabled), `init_db()` (`CREATE TABLE IF NOT EXISTS`), `seed_db()` (sample data)
  - Step 3: `/logout` · Step 4: `/profile` · Step 7: `/expenses/add` · Step 8: `/expenses/<id>/edit` · Step 9: `/expenses/<id>/delete`
- The SQLite file is `expense_tracker.db` (gitignored).
- `templates/` — Jinja templates that all extend `base.html` (navbar, footer, `title`/`head`/`content`/`scripts` blocks). `login.html` and `register.html` already contain `POST` forms and render an `error` variable, but their routes currently accept only GET; auth handling has to be added in `app.py`.
- `static/css/style.css` — the single stylesheet for every page (DM Serif Display / DM Sans fonts loaded in `base.html`). `static/js/main.js` is an empty placeholder.
- Use `url_for(...)` for internal links in templates. The footer's `/terms` and `/privacy` links are currently hardcoded.
