# Developer Guide

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start status -->
_Up to date._
<!-- sdlc:auto:end status -->

<!-- sdlc:auto:start source -->
Generated from commit `0ac14b4` · model `claude-sonnet-5`
<!-- sdlc:auto:end source -->

<!-- sdlc:auto:start overview -->
Blogger is a Quorq-generated scaffold for a chatbot-with-RAG product, split into two independently runnable apps in one repo. The frontend is a Vite + React 19 SPA whose screens were already reviewed and approved, and currently render seeded sample data rather than calling a real API. The backend is a FastAPI 0.115 service that, since no architecture was approved yet, only implements a `/health` liveness check; it uses SQLAlchemy models against a SQLite file by default (swappable via `DATABASE_URL` to the Postgres 16 the approved stack names) and Pydantic schemas, with a stubbed per-component router pattern ready to be filled in. A docker-compose file provisions Postgres and Redis for later use, but neither is required to run the apps as they stand today.
<!-- sdlc:auto:end overview -->

<!-- sdlc:auto:start prerequisites -->
- Node.js (for Vite 6 / React 19 / npm scripts)
- npm
- Python 3.12
- pip
- Docker and Docker Compose (optional, for the Postgres/Redis services in docker-compose.yml)
<!-- sdlc:auto:end prerequisites -->

<!-- sdlc:auto:start setup -->
1. Clone the repository
2. cd frontend && npm install
3. cd backend && pip install -r requirements.txt
4. (Optional) docker compose up -d to start the Postgres and Redis containers defined in docker-compose.yml
5. (Optional) set DATABASE_URL to point the backend at Postgres instead of its default sqlite:///./app.db
6. (Optional) set ALLOWED_ORIGINS on the backend and VITE_API_BASE_URL / VITE_API_PROJECT_ID on the frontend when running against a deployed API
<!-- sdlc:auto:end setup -->

<!-- sdlc:auto:start run -->
| Name | Command | Description |
|---|---|---|
| frontend dev | `npm run dev` | Starts the Vite dev server for the React SPA (run from frontend/) |
| frontend build | `npm run build` | Bundles the app with Vite (no typechecking) |
| frontend preview | `npm run preview` | Serves the production build locally |
| backend dev | `uvicorn app.main:app --reload` | Runs the FastAPI service with autoreload (run from backend/) |
| compose up | `docker compose up -d` | Starts the Postgres 16 and Redis 7 containers used by the approved architecture |
<!-- sdlc:auto:end run -->

<!-- sdlc:auto:start structure -->
| Path | Purpose |
|---|---|
| `frontend` | Vite + React SPA; approved, generated screens running on seeded sample data |
| `frontend/src` | App source: lib/api.ts (API client seam), lib/ui.tsx and lib/icons.tsx (component vocabulary), main.tsx (entry), index.css (brand tokens) |
| `frontend/e2e` | Playwright acceptance tests and support fixtures (e.g. admin.ts) |
| `backend` | FastAPI service: app package, tests, Dockerfile, requirements |
| `backend/app` | Application code: main.py (entrypoint, lifespan, CORS, /health), database.py (engine/session), models.py (SQLAlchemy models), schemas.py (Pydantic schemas incl. StubResponse), services.py, routers/ (per-component routers) |
| `backend/tests` | Unit and integration tests (pytest), including test_health.py |
| `.devcontainer` | Dev container configuration for this repo |
| `.quorq` | Platform metadata (handoff.json) from the Quorq AI SDLC platform |
<!-- sdlc:auto:end structure -->

<!-- sdlc:auto:start conventions -->
- Backend routes are generated one router per architecture component, one route per spec endpoint, each returning a typed StubResponse until implemented
- SQLAlchemy models all inherit from the shared Base in app/database.py; main.py imports models before create_all so tables register
- Database is SQLite by default for zero-dependency startup; DATABASE_URL env var swaps in Postgres without code changes
- Database setup/migration happens in the FastAPI lifespan handler (prepare_database), not at import time, so importing the app never opens a connection
- CORS origins come from the ALLOWED_ORIGINS env var (comma-separated), falling back to the local Vite dev server origins
- Frontend component tests live beside the components they cover and are collected only from src/**/*.{test,spec}.{ts,tsx} by Vitest
- Playwright e2e specs live in e2e/ outside src/, run with workers=1, and either hit a deployed URL via E2E_BASE_URL or boot backend+frontend locally
- Frontend API access is centralized in src/lib/api.ts (apiFetch), which attaches a bearer token (session token or API_PROJECT_ID) to every call; screens don't call fetch directly
- Path alias @/* maps to frontend/src in both tsconfig and Vite config
<!-- sdlc:auto:end conventions -->

<!-- sdlc:auto:start testing -->
| Name | Command | Description |
|---|---|---|
| frontend unit tests | `npm run test` | Runs Vitest component tests in frontend/src |
| frontend lint | `npm run lint` | Runs ESLint over the frontend app (CI gate) |
| frontend typecheck | `npm run typecheck` | Runs tsc --noEmit (not run by build) |
| frontend e2e install | `npm run test:e2e:install` | Installs the Chromium browser Playwright needs |
| frontend e2e | `npm run test:e2e` | Runs Playwright acceptance tests from e2e/, booting backend and frontend if E2E_BASE_URL is unset |
| backend tests | `pytest` | Runs unit and integration tests (pythonpath=. set in pytest.ini) from backend/ |
| backend lint | `ruff check .` | Lints the backend code |
| backend format | `ruff format .` | Formats the backend code |
<!-- sdlc:auto:end testing -->

<!-- sdlc:auto:start troubleshooting -->
| Problem | Solution |
|---|---|
| Frontend can't reach the backend / CORS errors in the browser console | Set ALLOWED_ORIGINS on the backend to include the frontend's origin, or run the frontend on the default Vite dev ports (5173) which are allowed by default |
| `npm run test:e2e` fails because no browser is installed | Run `npm run test:e2e:install` first to fetch Chromium |
| `npm run build` succeeds but the app doesn't actually typecheck | Run `npm run typecheck` separately; build does not run tsc |
| `pytest` run directly fails with an import error for `app` | Run it from backend/ (pytest.ini sets pythonpath = . for that directory) or use `cd backend && pytest` |
| Backend Docker container reports unhealthy | Confirm the app answers on http://localhost:8000/health inside the container; the HEALTHCHECK depends on that endpoint |
<!-- sdlc:auto:end troubleshooting -->
