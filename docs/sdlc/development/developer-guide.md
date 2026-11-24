# Developer Guide

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start status -->
_Up to date._
<!-- sdlc:auto:end status -->

<!-- sdlc:auto:start source -->
Generated from commit `7d28215` · model `claude-sonnet-5`
<!-- sdlc:auto:end source -->

<!-- sdlc:auto:start overview -->
Blogger is a Quorq-generated AI SDLC project scaffolded toward a RAG chatbot built on the Claude API. It is a two-app monorepo: a Vite + React 19 SPA in frontend/ containing the approved, reviewed screens (currently rendering seeded sample data, not real API calls), and a FastAPI 0.115 service in backend/ that has no approved architecture yet beyond a /health endpoint, using SQLAlchemy over a local SQLite file by default so it runs without a database server. A docker-compose.yml provisions Postgres 16 and Redis for when the real architecture lands.
<!-- sdlc:auto:end overview -->

<!-- sdlc:auto:start prerequisites -->
- Node.js (for npm/Vite/React 19, per frontend/package.json)
- Python 3.12 (per backend/Dockerfile)
- pip
- Docker and Docker Compose (optional, to run the Postgres/Redis services in docker-compose.yml)
<!-- sdlc:auto:end prerequisites -->

<!-- sdlc:auto:start setup -->
1. Clone the repository
2. cd frontend && npm install
3. cd backend && pip install -r requirements.txt
4. (Optional) copy .env.example to .env and set values such as DATABASE_URL, ALLOWED_ORIGINS, VITE_API_BASE_URL, VITE_API_PROJECT_ID as needed -- the backend defaults to a local SQLite file (sqlite:///./app.db) and the frontend defaults to http://localhost:8000 if these are unset
5. (Optional) docker-compose up -d to start Postgres and Redis if you want to point DATABASE_URL at a real database instead of the SQLite default
<!-- sdlc:auto:end setup -->

<!-- sdlc:auto:start run -->
| Name | Command | Description |
|---|---|---|
| frontend dev | `npm run dev` | Starts the Vite dev server for the React SPA (run from frontend/) |
| frontend build | `npm run build` | Bundles the frontend app with Vite (does not typecheck) |
| frontend preview | `npm run preview` | Serves the production build locally |
| backend dev | `uvicorn app.main:app --reload` | Starts the FastAPI app with auto-reload (run from backend/) |
| compose up | `docker-compose up -d` | Starts the Postgres 16 and Redis 7 service containers defined in docker-compose.yml |
<!-- sdlc:auto:end run -->

<!-- sdlc:auto:start structure -->
| Path | Purpose |
|---|---|
| `frontend` | Vite + React 19 SPA containing the approved, generated screens (currently run on seeded sample data) |
| `frontend/src` | App source: main.tsx entrypoint, lib/api.ts API client, lib/ui.tsx and lib/icons.tsx component vocabulary, index.css brand tokens |
| `frontend/e2e` | Playwright acceptance tests and support helpers |
| `backend` | FastAPI service: app package, Dockerfile, requirements, tests, and lint/test config |
| `backend/app` | FastAPI application code: main.py (entrypoint, CORS, lifespan, /health), database.py (engine/session), models.py (SQLAlchemy models, currently empty), schemas.py (Pydantic models and StubResponse), services.py, routers/ (one router per architecture component) |
| `backend/tests` | Pytest unit and integration tests (e.g. tests/integration/test_health.py) |
| `.devcontainer` | Dev container configuration for the repository |
| `.quorq` | Quorq platform metadata/handoff file for this generated project |
<!-- sdlc:auto:end structure -->

<!-- sdlc:auto:start conventions -->
- Backend routes are generated as one router per architecture component that owns endpoints, with each unimplemented route returning a typed StubResponse (status 'not_implemented') rather than raising, so the service and its OpenAPI doc stay complete before handlers are written
- SQLAlchemy models inherit from the shared Base in app.database; app.models is imported in main.py before create_all so tables register
- Database setup runs in the FastAPI lifespan handler (not at import time), via prepare_database(), which uses Base.metadata.create_all when no Alembic config exists, or alembic upgrade head once one does
- DATABASE_URL defaults to a local SQLite file so the backend runs without any database server; set DATABASE_URL to point at Postgres/another DB without code changes
- CORS origins come from the ALLOWED_ORIGINS env var (comma-separated), falling back to the Vite dev server origins (localhost:5173 / 127.0.0.1:5173)
- Frontend screens currently render seeded sample data; frontend/src/lib/api.ts is the prepared seam (apiFetch, API_BASE_URL, API_PROJECT_ID, session token helpers) for wiring screens to the real backend one at a time
- apiFetch sends a bearer Authorization header using a stored session token if present, else the API_PROJECT_ID shared secret
- Frontend and backend each have their own lint/format/test tooling (ESLint+Prettier / ruff) and are expected to be kept green as part of development checks
<!-- sdlc:auto:end conventions -->

<!-- sdlc:auto:start testing -->
| Name | Command | Description |
|---|---|---|
| frontend unit | `npm test` | Runs Vitest component tests located beside the components they cover (src/**/*.test.tsx) |
| frontend lint | `npm run lint` | Runs ESLint over the frontend app; used as a development gate |
| frontend typecheck | `npm run typecheck` | Runs tsc --noEmit (not run by npm run build) |
| frontend format check | `npm run format:check` | Checks formatting with Prettier |
| frontend e2e install | `npm run test:e2e:install` | Installs the Chromium browser Playwright needs |
| frontend e2e | `npm run test:e2e` | Runs Playwright acceptance tests in e2e/; boots the backend (via backend/.venv) and the built frontend locally unless E2E_BASE_URL is set |
| backend tests | `pytest` | Runs backend unit and integration tests (run from backend/); pytest.ini sets pythonpath=. and testpaths=tests |
| backend lint | `ruff check .` | Lints the backend code with ruff |
| backend format | `ruff format .` | Formats the backend code with ruff |
<!-- sdlc:auto:end testing -->

<!-- sdlc:auto:start troubleshooting -->
| Problem | Solution |
|---|---|
| Frontend API calls fail with a CORS error | Set ALLOWED_ORIGINS on the backend to include the frontend's origin, or run the frontend on the default Vite dev ports (5173) which are allowed out of the box |
| `pytest` fails with `ModuleNotFoundError: app` | Run pytest from the backend/ directory so pytest.ini's pythonpath=. setting applies |
| Backend won't start against Postgres from docker-compose | The backend defaults to SQLite (sqlite:///./app.db); after docker-compose up -d, explicitly set DATABASE_URL to the Postgres connection string (e.g. postgresql+psycopg://app:<database-password>@localhost:5432/app) before starting uvicorn |
| `npm run build` succeeds but the app is actually broken | build does not typecheck; run `npm run typecheck` (tsc --noEmit) before relying on a build |
| Playwright e2e tests fail to start the backend | The suite expects a backend virtualenv at backend/.venv with dependencies installed; create it and `pip install -r requirements.txt` there before running `npm run test:e2e` |
<!-- sdlc:auto:end troubleshooting -->
