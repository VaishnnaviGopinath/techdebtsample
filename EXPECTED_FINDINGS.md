# Test Oracle — what your analyzer should roughly surface

Use this checklist when validating AC outputs. Exact wording can differ; categories should match.

## Technical Debt Inventory
- [ ] TODO/FIXME comments in `app/main.py` checkout
- [ ] Dead file `app/old_checkout.py`
- [ ] Commented-out legacy functions in `app/main.py`
- [ ] Duplicated tax/pricing logic (`main.py`, `old_checkout.py`, `scripts/reprice.py`)
- [ ] Unused Django dependency
- [ ] Committed `.env`

## Code Quality Findings
- [ ] God file `app/main.py` (routes + SQL + payments + HTML)
- [ ] Poor names in `app/utils/helpers.py` (`do_stuff`, global `x`)
- [ ] MD5 password hashing
- [ ] No automated tests directory
- [ ] Magic numbers (`1.08` tax)

## Architecture Issues
- [ ] Monolith vs `docs/architecture.md` microservices claim
- [ ] Unused `app/models/user.py`
- [ ] Global mutable `CARTS` / `PRODUCT_CACHE`
- [ ] Celery/Redis in requirements but not wired
- [ ] Mixed SQLite runtime vs MySQL URL config

## Dependency Risks
- [ ] Flask 1.1.2 / React 16 / Node 10 era pins
- [ ] `PyYAML==5.1` known risk class
- [ ] `django==2.2.28` unused + EOL era
- [ ] `axios@0.18.0`, `jquery@2.2.4`, `lodash@4.17.15`
- [ ] `Pillow==6.2.0`, `requests==2.22.0`

## Security Findings
- [ ] Hardcoded AWS/Stripe/DB credentials in source and `.env`
- [ ] SQL injection in login/orders/search paths
- [ ] `debug=True` / `FLASK_DEBUG=1`
- [ ] `/admin/users` without authz
- [ ] XSS in `/search` and `static/admin.js`
- [ ] Weak JWT/secret values
- [ ] Health endpoint leaks `DATABASE_URL`

## Performance Concerns
- [ ] N+1 queries in `/orders/<user_id>`
- [ ] Blocking `requests.get` delay inside `/products`
- [ ] Full table load + never-invalidated cache
- [ ] `time.sleep` inside order loop
- [ ] Row-by-row updates in `scripts/reprice.py`

## Modernization Opportunities
- [ ] Upgrade Python beyond 3.7 (`runtime.txt`)
- [ ] Replace Flask 1.x patterns / add typing / OpenAPI
- [ ] Remove jQuery admin or rebuild in modern React
- [ ] Add Docker + CI (none present)
- [ ] Env-based secrets, parameterized SQL, real auth
- [ ] Align architecture docs with modular services or delete fiction

## Suggested prioritization buckets

### Quick Wins
- Remove secrets from repo; use environment variables
- Delete `old_checkout.py` and unused Django pin
- Disable debug in non-local config
- Upgrade or remove `PyYAML==5.1`

### Medium-Term
- Parameterize SQL; add authz on admin routes
- Split `main.py` into routes/services/repositories
- Add pytest + CI
- Fix N+1 and request-path network calls

### Strategic
- Modularize toward real service boundaries (or rewrite docs)
- Modern frontend admin
- Containerization + deployment pipeline
- Proper payment/auth architecture
