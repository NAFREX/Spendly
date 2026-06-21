# Spec: Login and Logout

## Overview
This step implements the login and logout flows for Spendly. The `/login` route currently renders the template but does not process the form — this step adds POST handling that authenticates the user against the `users` table and stores their identity in the Flask session. The `/logout` route clears the session and redirects to the landing page. Together these complete the auth loop started in Step 2 (registration).

## Depends on
- Step 1 — Database setup (`get_db`, `init_db`, `users` table)
- Step 2 — Registration (users table populated, session pattern established)

## Routes
- `GET /login` — render login form — public
- `POST /login` — validate credentials, set session, redirect to landing — public
- `GET /logout` — clear session, redirect to landing — public (safe to call even when logged out)

## Database changes
No database changes.

## Templates
- **Modify:** `templates/login.html` — add the POST form with email/password fields and an error display block (mirrors the pattern in `register.html`)

## Files to change
- `app.py` — replace the stub `login` route with GET+POST handler; replace the stub `logout` route with session-clearing logic
- `templates/login.html` — add form fields and error display

## Files to create
No new files.

## New dependencies
No new dependencies.

## Rules for implementation
- No SQLAlchemy or ORMs
- Parameterised queries only
- Passwords verified with `werkzeug.security.check_password_hash`
- Use CSS variables — never hardcode hex values
- All templates extend `base.html`
- On failed login, re-render `login.html` with a generic error ("Invalid email or password.") — do not reveal which field was wrong
- On successful login, set `session["user_id"]` and `session["user_name"]` to match the pattern used in `register`
- `logout` must call `session.clear()` then `redirect(url_for("landing"))`
- The login route accepts `GET` and `POST` — update `methods` accordingly

## Definition of done
- [ ] Visiting `/login` shows a form with email and password fields
- [ ] Submitting the form with `demo@spendly.com` / `demo123` redirects to the landing page and the user's name appears in the navbar
- [ ] Submitting with a wrong password shows "Invalid email or password." on the login page without redirecting
- [ ] Submitting with a non-existent email shows the same generic error
- [ ] Visiting `/logout` clears the session and redirects to `/`
- [ ] After logout, the navbar no longer shows the user's name
- [ ] All existing tests still pass (`pytest`)
