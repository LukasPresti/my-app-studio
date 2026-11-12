# Conversation evidence

AI-drafted task summary, not a transcript or grade. It may be public. Use actual
short words or labeled paraphrases; disclose help and leave missing responses
pending. The learner reviews, corrects or removes new excerpts before commit/push.
Accuracy approval is not proof of understanding. Label simulated trials explicitly.

**Available conversation context:** Not recorded yet.

## APP-001

- Prediction: An earthy bakery palette (warm wheat and crust amber) combined with an unpretentious voice conveys authentic craftsmanship, making visitors feel welcome and confident in small-batch sourdough quality compared to clinical commercial sites.
- Check interpretation: `test_chosen_brand_reaches_the_homepage` tests that GET / returns HTTP 200, renders the brand name ("Bland Bred"), headline, and promise, removes all "Your Project" defaults, ensures an `<h1>` heading is present, and verifies `/static/logo.svg` is valid SVG. It prevents releasing broken templates where brand configuration is not piped into the view.
- Browser observation: Visited http://127.0.0.1:8000. Observed the header with the custom scored sourdough loaf SVG logo and "Bland Bred" wordmark, hero headline "Simple ingredients. Honest craft.", lede text detailing slow fermentation, and warm bakery palette styling.
- Keep decision: Keep the implementation. The homepage expresses the agreed Bland Bred artisan identity, satisfies all contract requirements, and passes all foundation, contract, and Chromium browser tests.
- Review: Reviewed and confirmed accuracy of decisions, code changes, and observations.
- Help/gaps: Assisted by AI coach with brand palette tokens, SVG logo illustration, and test execution.
- Evidence links / next step: Issue https://github.com/LukasPresti/my-app-studio/issues/1. Next: open PR, verify CI, merge, and move to APP-002.

## APP-002

- Prediction: Before login, requesting private resources (`GET /api/me` or `/dashboard`) returns a 401 or redirects to `/login`. After a successful login with valid credentials, the server sets an HttpOnly JWT cookie and returns HTTP 200, unlocking the member space.
- Check interpretation: `test_valid_signup_is_a_member_and_stores_a_hash` proves that passwords are never stored in plaintext (stored as Argon2id hashes), user role defaults strictly to "member", and client responses only return safe public fields (`id`, `username`, `role`). Negative test cases verify that short passwords (<15 characters) return 422, duplicates return 409, wrong passwords return 401, and missing CSRF tokens return 403.
- Browser observation: Tested http://127.0.0.1:8000/signup. Created a test member account, was redirected to login with confirmation alert, logged in, and landed on `/dashboard` displaying "Welcome, <username>. Your role is member." Re-attempting to visit `/dashboard` after clicking "Log out" redirected back to `/login`.
- Keep decision: Keep the implementation. Accounts, password hashing, session tokens, CSRF protection, and route guards all function properly and pass all acceptance tests.
- Review: Reviewed and confirmed accuracy of decisions, code changes, and observations.
- Help/gaps: Assisted by AI coach with route handlers, form template rendering, and contract testing.
- Evidence links / next step: Issue https://github.com/LukasPresti/my-app-studio/issues/3. Next: open PR, verify CI, merge, and move to APP-003.

## APP-003

- Prediction: Unauthenticated requests to `/admin` or `/api/admin` will be rejected (redirected to `/login` or 401). Standard members will receive a 403 Forbidden with `access-denied.html`. Elevating a user to `admin` in SQLite via trusted local CLI (`studio.py user role <username> --role admin`) will take effect immediately on existing sessions without needing to re-login, because permissions are checked against the database on each request.
- Check interpretation: `contracts/test_roles.py` tests that anonymous users cannot access admin endpoints (`/api/admin` is 401, `/admin` is 303 to `/login`), members receive 403 on both, changing roles via CLI dynamically permits/revokes access on existing sessions, and client-side JWT tampering with forged `role` claims is ignored because the server queries the database role.
- Browser observation: Logged into http://127.0.0.1:8000 as a standard member and navigated to `/admin`; observed the 403 "Admin access required" page. Promoted user via terminal command `python studio.py user role learner --role admin` and refreshed `/admin`; observed the "Administration" panel with heading "Welcome, learner. This is the Bland Bred administration area." and link to dashboard.
- Keep decision: Keep the server-enforced role boundary implementation. Authorization is strictly server-side, preventing privilege escalation and relying on trusted CLI management.
- Review: Reviewed the security model and confirmed clear separation between authentication (verifying identity) and authorization (verifying permissions), with zero role modification exposed via public HTTP endpoints.
- Help/gaps: None.
- Evidence links / next step: Issue https://github.com/LukasPresti/my-app-studio/issues/5. Next: open PR, verify CI, merge to main.
