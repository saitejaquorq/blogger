# Developer Guide

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start status -->
_Up to date._
<!-- sdlc:auto:end status -->

<!-- sdlc:auto:start source -->
Generated from commit `57b6059` · model `claude-sonnet-5`
<!-- sdlc:auto:end source -->

<!-- sdlc:auto:start overview -->
blogger is a generated monorepo from the Quorq AI SDLC platform containing two separate apps: a Vite + React 19 single-page frontend whose screens were design-approved and currently run entirely on seeded sample data, and a FastAPI 0.115 backend that has not yet had its architecture approved, so it currently exposes only a health check endpoint and typed stub infrastructure (SQLAlchemy models/Pydantic schemas/routers package) ready to be filled in. The backend defaults to a local SQLite database so it runs without any server, while docker-compose.yml provisions Postgres and Redis for when the real architecture is implemented.
<!-- sdlc:auto:end overview -->

<!-- sdlc:auto:start prerequisites -->
- Node.js (for Vite 6 / React 19 / TypeScript 5.6 toolchain)
- npm
- Python 3.12
- pip
- Docker and Docker Compose (optional, to run Postgres/Redis locally)
<!-- sdlc:auto:end prerequisites -->

<!-- sdlc:auto:start setup -->
1. Copy .env.example to .env and fill in any values it defines
2. cd backend && pip install -r requirements.txt
3. cd frontend && npm install
4. (optional) from the repo root run: docker compose up -d   # starts Postgres on 5432 and Redis on 6379 for when a real architecture uses them
<!-- sdlc:auto:end setup -->

<!-- sdlc:auto:start run -->
| Name | Command | Description |
|---|---|---|
| backend dev | `cd backend && uvicorn app.main:app --reload` | Starts the FastAPI service with autoreload; creates/upgrades the SQLite (or DATABASE_URL-pointed) schema on startup via the lifespan hook |
| frontend dev | `cd frontend && npm run dev` | Starts the Vite dev server for the React SPA (seeded sample data, no live API calls yet) |
| frontend build | `cd frontend && npm run build` | Bundles the app with Vite; does not typecheck |
| frontend preview | `cd frontend && npm run preview` | Serves the production build locally |
| docker compose | `docker compose up -d` | Starts the Postgres 16 and Redis 7 containers defined for the architecture |
<!-- sdlc:auto:end run -->

<!-- sdlc:auto:start structure -->
| Path | Purpose |
|---|---|
| `.devcontainer` | VS Code devcontainer configuration |
| `.quorq` | Quorq platform metadata (e.g. handoff.json) used by the generating platform, not application code |
| `backend` | FastAPI service: app package, Dockerfile, requirements, pytest/ruff config |
| `backend/app` | Application code: main.py entrypoint, database.py (engine/session), models.py (SQLAlchemy Base), schemas.py (Pydantic + StubResponse), services.py, routers/ package |
| `backend/tests` | Pytest suite split into unit/ and integration/ (integration includes the health check test) |
| `frontend` | Vite + React 19 SPA: approved screens running on seeded sample data |
| `frontend/src` | Application source: main.tsx entrypoint, index.css, lib/ (api.ts client, ui.tsx, icons.tsx, navigate.ts), test-setup.ts |
| `frontend/e2e` | Playwright end-to-end specs and support helpers |
| `docker-compose.yml` | Local Postgres 16 and Redis 7 services for the approved architecture |
<!-- sdlc:auto:end structure -->

<!-- sdlc:auto:start conventions -->
- Backend routes are organized one router per architecture component under backend/app/routers, each route a typed stub returning StubResponse until implemented
- Pydantic schemas are defined per entity in schemas.py; SQLAlchemy models inherit the shared Base from database.py and are imported in main.py before table creation
- Backend reads configuration from environment variables (DATABASE_URL, ALLOWED_ORIGINS) rather than hardcoding values, with sensible local defaults (SQLite, localhost CORS origins)
- Database schema is created via SQLAlchemy create_all until an alembic.ini is present, at which point main.py switches to running Alembic migrations automatically on startup
- Frontend screens live one-per-module under src/screens and are built against the shared component/icon vocabulary in src/lib/ui.tsx and src/lib/icons.tsx
- Frontend API access is centralized in src/lib/api.ts (apiFetch) with bearer auth via a project id or stored session token; screens do not call it directly yet
- Frontend code is linted with ESLint (flat config) and formatted with Prettier; backend is linted/formatted with ruff
<!-- sdlc:auto:end conventions -->

<!-- sdlc:auto:start testing -->
| Name | Command | Description |
|---|---|---|
| backend tests | `cd backend && pytest` | Runs unit and integration tests (pytest.ini sets pythonpath to backend/) |
| backend lint | `cd backend && ruff check .` | Lints the backend code with ruff |
| backend format check | `cd backend && ruff format .` | Formats backend code with ruff |
| frontend unit tests | `cd frontend && npm test` | Runs Vitest component tests located beside the components they cover |
| frontend typecheck | `cd frontend && npm run typecheck` | Runs tsc --noEmit, which the build step does not do |
| frontend lint | `cd frontend && npm run lint` | Runs ESLint over the whole frontend app |
| frontend e2e install | `cd frontend && npm run test:e2e:install` | Installs the Chromium browser Playwright needs |
| frontend e2e tests | `cd frontend && npm run test:e2e` | Runs Playwright specs from e2e/; boots the backend (via its .venv) and a built/served frontend unless E2E_BASE_URL is set |
<!-- sdlc:auto:end testing -->

<!-- sdlc:auto:start troubleshooting -->
| Problem | Solution |
|---|---|
| Frontend dev server calls to the API are blocked by CORS | Set ALLOWED_ORIGINS (comma-separated) in the backend's environment to include the frontend origin, e.g. http://localhost:5173; otherwise only the built-in dev origins are allowed |
| `npm run build` succeeds but deployment or CI fails on type errors | Run `npm run typecheck` (tsc --noEmit) locally and fix errors before relying on a successful build |
| Running `pytest` directly fails with `ModuleNotFoundError: app` | Run pytest from the backend/ directory so backend/pytest.ini's pythonpath setting applies, or run `cd backend && pytest` |
| Playwright e2e tests fail to launch a browser | Run `npm run test:e2e:install` first to fetch Chromium |
| Backend fails to connect to a database when DATABASE_URL points at Postgres | Ensure `docker compose up -d` has started the db service and that DATABASE_URL matches its host/port/credentials (defaults to a local SQLite file if DATABASE_URL is unset) |
<!-- sdlc:auto:end troubleshooting -->
