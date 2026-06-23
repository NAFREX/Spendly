# Spec: Profile Page

## Overview
This step implements the `/profile` route, replacing the stub string with a real page that displays the logged-in user's account details and a summary of their expense activity. It gives users a central place to see who they are in the app and a snapshot of their spending, reinforcing the session and database query patterns established in the auth steps.

## Depends on
- Step 1 — Database setup (`get_db`, `users` and `expenses` tables)
- Steps 2 & 3 — Registration and Login/Logout (session with `user_id` and `user_name`)

## Routes
- `GET /profile` — render the profile page with user info and expense summary — logged-in only (redirect to `/login` if no session)

## Database changes
No database changes. The existing `users` and `expenses` tables are sufficient.

## Templates
- **Create:** `templates/profile.html` — displays name, email, member-since date, and expense summary stats
- **Modify:** `templates/base.html` — ensure the navbar "Profile" link points to `url_for('profile')` (add if missing)

## Files to change
- `app.py` — replace the stub `profile()` route with a real implementation that queries the DB and renders `profile.html`
- `templates/base.html` — verify/add Profile nav link

## Files to create
- `templates/profile.html`

## New dependencies
No new dependencies.

## Rules for implementation
- No SQLAlchemy or ORMs — use `get_db()` and raw SQL only
- Parameterised queries only — never format user data into SQL strings
- Passwords hashed with werkzeug (already done at registration; do not expose `password_hash` in the template)
- Use CSS variables — never hardcode hex values in inline styles or new CSS rules
- All templates extend `base.html`
- Redirect unauthenticated users to `/login` (check `session.get("user_id")` at the top of the route)
- Query user row by `session["user_id"]`; if the row is missing (deleted account edge case), clear session and redirect to landing
- Pass to the template: `user` (the full row), `total_spent` (SUM of all user expenses), `expense_count` (COUNT), `top_category` (category with highest total spend, or `None` if no expenses)

## Definition of done
- [ ] Visiting `/profile` while logged out redirects to `/login`
- [ ] Visiting `/profile` while logged in renders a page (not a plain string)
- [ ] The page shows the logged-in user's name and email
- [ ] The page shows the account creation date (`created_at` from the `users` table)
- [ ] The page shows the total amount spent and number of expenses
- [ ] The page shows the top spending category (or a graceful "No expenses yet" message)
- [ ] The Profile link in the navbar navigates to `/profile`
- [ ] All tests pass: `pytest`
