# Legacy Shop (Sample Debt Repo)

**Purpose:** Intentionally bad codebase for testing an AI Technical Debt & Modernisation Planner.

Use this folder as:
- a zip upload, or
- a git subdirectory / cloned repo target

Do **not** use this app in production. Secrets and insecure patterns are deliberate.

## How to point your analyzer at it

```bash
# Option A: zip
cd /path/to/workspace
zip -r sample-legacy-shop.zip sample-legacy-shop -x "*.git*"

# Option B: git URL after you push this project
# use the repo URL and set root path to sample-legacy-shop if supported
```

## Expected findings by AC category

| AC output | Planted issues in this repo |
|---|---|
| Technical Debt Inventory | TODOs/FIXMEs, duplicated order logic, dead `old_checkout.py`, commented-out blocks |
| Code Quality Findings | 400+ line god module `app/main.py`, no tests, magic numbers, poor naming |
| Architecture Issues | UI + DB + business logic in one file, global mutable state, no service layer |
| Dependency Risks | Ancient Flask/Pillow/requests pins, unused `django==2.2`, vulnerable `pyyaml==5.1` |
| Security Findings | Hardcoded AWS/DB secrets, SQL injection, debug=True, open CORS, weak JWT secret |
| Performance Concerns | N+1 queries in loop, loading full table into memory, sync requests in request path |
| Modernization Opportunities | Python 3.7 target, no Docker/CI, sync Flask vs async, no typed models, no OpenAPI |
| Priority / Effort / Roadmap | Mix of quick wins (remove secrets, upgrade PyYAML) and strategic (modularize monolith) |

## Quick Wins (should rank high, low effort)
1. Remove secrets from source; use env vars
2. Upgrade/remove vulnerable deps (`pyyaml==5.1`, `django==2.2`)
3. Turn off `debug=True` in production path
4. Delete dead `old_checkout.py`

## Medium-Term
1. Split `app/main.py` into routes/services/repos
2. Add tests and CI
3. Parameterize SQL queries
4. Cache product listing

## Strategic
1. Migrate Flask monolith toward modular/API gateway style
2. Replace sync blocking I/O with async or background jobs
3. Introduce proper auth (not hard-coded JWT secret)
4. Containerize + deploy pipeline

## Run locally (optional; not required for analyzer tests)

```bash
cd sample-legacy-shop
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python app/main.py
```
