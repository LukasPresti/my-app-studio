# App Studio decisions

The durable agreement between learner and AI. Update only agreed decisions and
actual evidence; pending is a valid state. Keep explanations/help in
[LEARNING.md](LEARNING.md), not a second copy here.

## Current stage

- **Current feature:** APP-001.
- **Status:** Neutral foundation; learner scope and decisions pending.
- **Next step:** Confirm the local clone and SSH origin, run setup and try the page.
- **Implementation marker:** `studio.json` starts at `implemented_through: 0`.

This repository owns application code. Server/proxy changes and production
deployment are outside these three local-development lessons. Prepared helpers
are reusable foundations, not evidence that unfinished routes are implemented.

## APP-001 · Branded homepage

**Scope:** make the neutral FastAPI homepage express one chosen identity with
a simple original SVG mark, coherent copy, navigation and shared style tokens.
Keep account actions honest about their unfinished state.

| Decision | Agreement |
|---|---|
| Visitor and concrete use situation | Artisan bread enthusiasts, home cooks, and local community members seeking wholesome, handcrafted sourdough without shortcuts. |
| Reused IS117 brand, or name / promise / primary archetype | Brand: Bland Bred. Promise: Simple, unpretentious sourdough baked daily with slow fermentation and honest ingredients. Archetype: The Creator. |
| One visible choice and visitor benefit | Warm earthy palette (#F8F4EE wheat, #C45B28 crust amber, #2D241E ink) and artisan sourdough SVG emblem, conveying authentic craftsmanship and warmth over sterile commercial branding. |
| Own issue / branch / PR | Issue #1 (https://github.com/LukasPresti/my-app-studio/issues/1), Branch `feat/app-001-homepage` |

**Keep when:** the agreed identity is visible and usable at desktop/narrow widths;
full foundation + APP-001 checks pass; AI review and the learner's actual check
interpretation/browser observation/keep reason are recorded; reviewed evidence
and verified GitHub state are linked below.

## APP-002 · Signup, login and logout

**Scope:** add browser forms and REST behavior for member signup, login, a private
dashboard and logout backed by SQLite. Reuse the prepared password/JWT/CSRF/session
helpers; keep data local. New accounts cannot choose their role.

| Decision | Agreement |
|---|---|
| Expected public/private behavior and account flow | Public visitor accesses signup and login forms. Submitting signup creates member account; submitting login sets HttpOnly JWT session cookie and redirects to `/dashboard`. Unauthenticated access to `/dashboard` redirects to `/login`; unauthenticated `GET /api/me` returns 401. Logging out invalidates the session in SQLite and clears the cookie. |
| Valid and rejected cases to demonstrate | Valid signup and login return 201/200; duplicate usernames return 409; passwords under 15 characters return 422; invalid passwords return 401; missing CSRF tokens return 403; role injection at signup is ignored/rejected (new accounts are members). |
| Own issue / branch / PR | Issue #3 (https://github.com/LukasPresti/my-app-studio/issues/3), Branch `feat/app-002-accounts` |

**Keep when:** the agreed forms/API work; duplicate/invalid signup and wrong
credentials are rejected; passwords are hashed; authentication uses validated,
expiring JWTs with protected cookies and CSRF; private requests require a live
session and logout revokes it. Full cumulative checks, AI review, actual learner
contributions and GitHub state are recorded. This is not a production-security certification.

## APP-003 · Server-enforced roles

**Scope:** enforce a small member/admin boundary in the API and browser. Assign
roles only through the trusted local terminal command; use current server-side
permissions, including for an existing session after a role change.

| Visitor | Homepage | Member area | Admin area/API |
|---|---|---|---|
| Anonymous | Allowed | Login required | Login required |
| Member | Allowed | Allowed | Forbidden |
| Admin | Allowed | Allowed | Allowed |

| Decision | Agreement |
|---|---|
| Intended admin-only result | Restrict `/admin` and `/api/admin` to authenticated users with role `admin` in database; standard members receive 403 Forbidden with `access-denied.html`; anonymous users redirect to `/login` (or 401 for API). Roles are elevated/demoted only via trusted local CLI. |
| Actual allowed/denied request evidence | Anonymous: `/api/admin` -> 401, `/admin` -> 303 Redirect to `/login`. Member: `/api/admin` -> 403, `/admin` -> 403 Forbidden. Admin: `/api/admin` -> 200, `/admin` -> 200 OK. Dynamic role update: `studio.py user role <username> --role admin` immediately permits access on next request without relogin. Verified via [contracts/test_roles.py](file:///C:/Users/Lukas/IS373/my-app-studio/contracts/test_roles.py). |
| Own issue / branch / PR | Issue [#5](https://github.com/LukasPresti/my-app-studio/issues/5), Branch `feat/app-003-roles`, PR pending |

**Keep when:** a direct request cannot bypass the role boundary, role changes
take effect as specified, new signup remains member-only, and full cumulative
checks plus AI review pass. Link the learner's actual interpretation/observation,
reviewed record and verified GitHub result. Hidden navigation alone is insufficient.

## Evidence

Link actual files/checks and the student's own issues/PRs. No fabricated URLs,
customer research or claims that a timestamp proves learning.

| Feature | Files / own issue / PR | Actual checks and learner observations | Kept / GitHub state |
|---|---|---|---|
| APP-001 | [brand.json](file:///C:/Users/Lukas/IS373/my-app-studio/app/brand.json), [logo.svg](file:///C:/Users/Lukas/IS373/my-app-studio/app/static/logo.svg), Issue [#1](https://github.com/LukasPresti/my-app-studio/issues/1), PR [#2](https://github.com/LukasPresti/my-app-studio/pull/2) | Black, Pylint, 95 foundation tests, homepage contracts, Chromium journey passed | Merged to main |
| APP-002 | [accounts.py](file:///C:/Users/Lukas/IS373/my-app-studio/app/features/accounts.py), templates, Issue [#3](https://github.com/LukasPresti/my-app-studio/issues/3), PR [#4](https://github.com/LukasPresti/my-app-studio/pull/4) | Black, Pylint, 95 foundation tests, 14 contracts, Chromium journeys passed | Merged to main |
| APP-003 | [roles.py](file:///C:/Users/Lukas/IS373/my-app-studio/app/features/roles.py), [admin.html](file:///C:/Users/Lukas/IS373/my-app-studio/app/templates/admin.html), [access-denied.html](file:///C:/Users/Lukas/IS373/my-app-studio/app/templates/access-denied.html), Issue [#5](https://github.com/LukasPresti/my-app-studio/issues/5) | Black, Pylint, 95 foundation tests, 17 contracts, Chromium journeys passing | Branch `feat/app-003-roles` |

The instructor's reference branches show possible implementations. They do not
establish this repository's completion or the learner's own reasoning.
