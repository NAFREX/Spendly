# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**Spendly** is a Flask-based personal expense tracker. The project is structured as a step-by-step student exercise — many routes in `app.py` are stubs returning placeholder strings, and `database/db.py` is intentionally empty for students to implement.

## Commands

```bash
# Activate the virtual environment (Windows)
venv\Scripts\activate

# Run the development server (port 5001)
python app.py

# Run all tests
pytest

# Run a single test file
pytest tests/test_auth.py

# Run a single test by name
pytest tests/test_auth.py::test_login_success
```

## Architecture

**`app.py`** — single-file Flask app. All routes live here. Completed routes render Jinja2 templates; stub routes return plain strings marking which step implements them.

**`database/db.py`** — SQLite layer (to be implemented). Should expose three functions:
- `get_db()` — connection with `row_factory` and foreign keys enabled
- `init_db()` — `CREATE TABLE IF NOT EXISTS` for all tables
- `seed_db()` — sample data for development

**`templates/`** — Jinja2 templates. `base.html` is the shared layout (navbar + footer). All other templates extend it via `{% block content %}`.

**`static/css/style.css`** / **`static/js/main.js`** — single CSS file and single JS file; JS is currently empty and grows as features are added.

## Step Sequence

The project is built incrementally across numbered steps:

| Step | Feature |
|------|---------|
| 1 | Database setup (`database/db.py`) |
| 3 | Logout |
| 4 | Profile page |
| 7 | Add expense |
| 8 | Edit expense |
| 9 | Delete expense |

Auth (register/login) routes already render templates. All stub routes use `GET` only until their step is reached.
