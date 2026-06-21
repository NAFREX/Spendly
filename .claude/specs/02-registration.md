# Spec: Registration

## Overview
Implement user registration for Spendly. The `/register` route currently only handles `GET` and renders the form. This step wires up the `POST` handler to validate the submitted data, hash the password, insert a new user into the `users` table, start a session, and redirect to the dashboard (or re-render the form with an error on failure). This is the first feature that writes user-supplied data to the database and establishes the session pattern all future authenticated routes depend on.

## Depends on
- Step 1 — Database setup (`database/db.py` must be implemented: `get_db()`, `init_db()`, `seed_db()`)

## Routes
- `GET  /register` — render the registration form — public (already exists, no change needed)
- `POST /register` — validate form data, insert user, start session, redirect — public

## Database changes
No new tables or columns. Uses the existing `users` table:
- `name`, `email`, `password_hash`, `created_at`

## Templates
- **Modify:** `templates/register.html` — already renders `{{ error }}`; no structural change needed. Ensure `value` attributes re-populate `name` and `email` fields on validation failure so the user does not lose their input.

## Files to change
- `app.py` — add `POST` method to `/register` route; add `secret_key` to app config; import `session`, `redirect`, `url_for`, `request` from flask; import `generate_password_hash` from werkzeug

## Files to create
None

## New dependencies
No new dependencies. Uses:
- `flask.session` (built-in to Flask)
- `werkzeug.security.generate_password_hash` (already installed)

## Rules for implementation
- No SQLAlchemy or ORMs — raw `sqlite3` via `get_db()` only
- Parameterised queries only — never use string formatting in SQL
- Passwords hashed with `werkzeug.security.generate_password_hash` before insert
- Use CSS variables — never hardcode hex values
- All templates extend `base.html`
- `app.secret_key` must be set before sessions work — use a hard-coded dev string for now (e.g. `"spendly-dev-secret"`)
- Store `user_id` and `user_name` in `session` after successful registration
- On duplicate email, catch the `sqlite3.IntegrityError` and re-render the form with `error="An account with that email already exists."`
- Validate server-side: name non-empty, email non-empty, password at least 8 characters — re-render with a specific error message for each failure
- After successful registration redirect to `/` (landing) until a dashboard route exists

## Definition of done
- [ ] Submitting valid data creates a new row in `users` with a hashed password
- [ ] `session["user_id"]` and `session["user_name"]` are set after registration
- [ ] Duplicate email shows the error message on the form without crashing
- [ ] Password shorter than 8 characters shows a validation error
- [ ] Empty name or email shows a validation error
- [ ] `name` and `email` field values are preserved on validation failure (no blank form)
- [ ] Successful registration redirects (302) rather than re-rendering
- [ ] Existing `GET /register` still renders the empty form correctly
- [ ] App starts without errors after changes to `app.py`
