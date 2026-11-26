# Developer Guide

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start status -->
_Up to date._
<!-- sdlc:auto:end status -->

<!-- sdlc:auto:start source -->
Generated from commit `e91e192` · model `claude-sonnet-5`
<!-- sdlc:auto:end source -->

<!-- sdlc:auto:start overview -->
blogger is a generated two-app repository from the Quorq AI SDLC platform: a React 19 + Vite single-page frontend whose screens were approved against seeded sample data, and a FastAPI 0.115 backend that currently exposes only a health check (no architecture had been approved yet when it was generated). The backend uses SQLAlchemy against a SQLite file by default (swap in Postgres via DATABASE_URL), and docker-compose provides Postgres 16 and Redis containers for local infrastructure. The frontend talks to the backend only through the `apiFetch` helper in `src/lib/api.ts`, which is a seam left for future screens to wire up — none use it yet.
<!-- sdlc:auto:end overview -->

<!-- sdlc:auto:start prerequisites -->
- Node.js (compatible with Vite 6 / React 19, e.g. Node 20+)
- Python 3.12
- pip
- Docker and Docker Compose (optional, for Postgres/Redis via docker-compose.yml)
<!-- sdlc:auto:end prerequisites -->

<!-- sdlc:auto:start setup -->
1. Clone the repository
2. Copy the environment template: cp .env.example .env, and fill in values as needed
3. Backend: cd backend && pip install -r requirements.txt
4. (Optional) Start Postgres/Redis: docker compose up -d
5. Frontend: cd frontend && npm install
<!-- sdlc:auto:end setup -->

<!-- sdlc:auto:start run -->
| Name | Command | Description |
|---|---|---|
| backend dev | `cd backend && uvicorn app.main:app --reload` | Runs the FastAPI app locally with auto-reload; defaults to a local SQLite database unless DATABASE_URL is set. |
| frontend dev | `cd frontend && npm run dev` | Starts the Vite dev server for the React SPA, which renders the approved screens on seeded sample data. |
| frontend build | `cd frontend && npm run build` | Bundles the app for production (does not typecheck). |
| frontend preview | `cd frontend && npm run preview` | Serves the production build locally. |
| infra up | `docker compose up -d` | Starts Postgres 16 and Redis containers defined in docker-compose.yml. |
<!-- sdlc:auto:end run -->

<!-- sdlc:auto:start structure -->
| Path | Purpose |
|---|---|
| `backend/app` | FastAPI application code: main.py entrypoint/lifespan, database.py engine/session, models.py SQLAlchemy models, schemas.py Pydantic schemas, services.py, routers/ for per-component endpoint routers. |
| `backend/tests` | pytest suite split into unit/ and integration/ (e.g. test_health.py), with conftest.py fixtures. |
| `frontend/src` | React SPA source: App.tsx navigation shell, lib/ (api client, UI/icon vocabulary, navigate helpers), index.css brand tokens, main.tsx entrypoint, test-setup.ts for Vitest. |
| `frontend/e2e` | Playwright acceptance tests and support helpers (e.g. admin.ts), run against a locally built app or a deployed URL. |
| `.devcontainer` | Dev container configuration for a consistent local development environment. |
| `.quorq` | Platform metadata (handoff.json) used by the Quorq AI SDLC tooling. |
<!-- sdlc:auto:end structure -->

<!-- sdlc:auto:start conventions -->
- Backend routes are generated one router per architecture component, each endpoint currently a stub returning the typed StubResponse model until implemented.
- SQLAlchemy models all inherit from the shared Base in app/database.py; models.py is imported in main.py before table creation so tables register.
- Request/response shapes are defined as Pydantic schemas in app/schemas.py, one pair per entity.
- Backend tests are split into tests/unit and tests/integration.
- Database bootstrap runs in the FastAPI lifespan handler (not at import time) so importing the app never opens a connection; Alembic migrations are used automatically if an alembic.ini is present, otherwise Base.metadata.create_all runs.
- Frontend uses a '@/' path alias to src/, Tailwind CSS via @tailwindcss/vite, and ESLint + Prettier for linting/formatting.
- Frontend screens currently render seeded sample data; the api.ts client (apiFetch, session token helpers) is the designated integration seam, not yet used by screens.
- CORS origins are controlled via the ALLOWED_ORIGINS env var (comma-separated), defaulting to the local Vite dev server origins.
<!-- sdlc:auto:end conventions -->

<!-- sdlc:auto:start testing -->
| Name | Command | Description |
|---|---|---|
| backend tests | `cd backend && pytest` | Runs the pytest suite (unit + integration) with pythonpath configured via pytest.ini. |
| backend lint | `cd backend && ruff check .` | Lints backend Python code with Ruff. |
| backend format | `cd backend && ruff format .` | Formats backend Python code with Ruff. |
| frontend unit tests | `cd frontend && npm test` | Runs Vitest component tests under src/. |
| frontend lint | `cd frontend && npm run lint` | Runs ESLint over the whole frontend app; this is the gate the platform runs during development. |
| frontend typecheck | `cd frontend && npm run typecheck` | Runs tsc --noEmit (not run by the build). |
| frontend e2e | `cd frontend && npm run test:e2e` | Runs Playwright acceptance tests from e2e/; boots the backend (from backend/.venv) and a built frontend preview unless E2E_BASE_URL is set. |
<!-- sdlc:auto:end testing -->

<!-- sdlc:auto:start troubleshooting -->
| Problem | Solution |
|---|---|
| Frontend screens show only sample data and calls to the backend fail or are not attempted | This is expected: generated screens render seeded sample data and have not been wired to the API yet; use src/lib/api.ts's apiFetch as the integration point when connecting a screen. |
| Browser blocks requests from the frontend to the backend (CORS error) | Set ALLOWED_ORIGINS on the backend to a comma-separated list including the frontend's origin; it defaults to http://localhost:5173 and http://127.0.0.1:5173 for local dev. |
| Backend fails to find a Postgres/Redis server | The backend defaults to a local SQLite file (app.db) via DATABASE_URL, so no server is required by default; run docker compose up -d to start Postgres 16 and Redis if DATABASE_URL points at them. |
| Playwright e2e tests fail to start the backend | Ensure backend dependencies are installed into backend/.venv before running npm run test:e2e, since the Playwright config launches uvicorn from that virtualenv. |
| `pytest` run from backend/ fails to import app | Run pytest from the backend/ directory so pytest.ini's pythonpath = . takes effect, making `app` importable. |
<!-- sdlc:auto:end troubleshooting -->
