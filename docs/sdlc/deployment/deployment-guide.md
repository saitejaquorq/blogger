# Deployment Guide

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start how-it-deploys -->
Pushes to `main` run `.github/workflows/deploy.yml`, which builds the API image (if there is a backend) and deploys the SPA to AWS Amplify. Commits touching only `docs/sdlc/**` don't deploy. Deployments can also be started from the platform's Deployment tab.
<!-- sdlc:auto:end how-it-deploys -->

<!-- sdlc:auto:start environments -->
| Name | Service | Runtime | Scaling | Environment |
|---|---|---|---|---|
| Web SPA | React static site (S3 + CloudFront) | Static assets | CDN-served | dev |
| API Service | FastAPI on ECS Fargate | Python 3.12 / Uvicorn | 1-2 tasks (sized for ~50 users) | dev |
| Indexing Worker | RQ worker on ECS Fargate | Python 3.12 | 1 task | dev |
| Database | Amazon RDS for PostgreSQL 16 with pgvector | PostgreSQL 16 | Single instance | dev |
| Redis | Amazon ElastiCache for Redis (or redis:7-alpine container) | Redis 7 | Single node | dev |
| Document Store | Amazon S3 | Object storage | Managed | dev |
| Claude API | Anthropic (Claude) | SaaS | Managed | dev |
| Voyage AI | Voyage AI embeddings | SaaS | Managed | dev |
| SMTP | SMTP server | SaaS | Managed | dev |
<!-- sdlc:auto:end environments -->

<!-- sdlc:auto:start live -->
_Not deployed yet._
<!-- sdlc:auto:end live -->

<!-- sdlc:auto:start topology -->
```mermaid
graph LR
  n_Web_SPA["Web SPA<br/>React static site (S3 + CloudFront)"]
  n_API_Service["API Service<br/>FastAPI on ECS Fargate"]
  n_Indexing_Worker["Indexing Worker<br/>RQ worker on ECS Fargate"]
  n_Database["Database<br/>Amazon RDS for PostgreSQL 16 with pgvector"]
  n_Redis["Redis<br/>Amazon ElastiCache for Redis (or redis:7-alpine container)"]
  n_Document_Store["Document Store<br/>Amazon S3"]
  n_Claude_API["Claude API<br/>Anthropic (Claude)"]
  n_Voyage_AI["Voyage AI<br/>Voyage AI embeddings"]
  n_SMTP["SMTP<br/>SMTP server"]
  n_Web_SPA -->|"HTTPS (Bearer token)"| n_API_Service
  n_API_Service -->|"TLS (PostgreSQL)"| n_Database
  n_API_Service -->|"TCP"| n_Redis
  n_API_Service -->|"HTTPS"| n_Document_Store
  n_API_Service -->|"HTTPS"| n_Claude_API
  n_API_Service -->|"HTTPS"| n_Voyage_AI
  n_API_Service -->|"SMTP/TLS"| n_SMTP
  n_Indexing_Worker -->|"TCP"| n_Redis
  n_Indexing_Worker -->|"TLS (PostgreSQL)"| n_Database
  n_Indexing_Worker -->|"HTTPS"| n_Document_Store
  n_Indexing_Worker -->|"HTTPS"| n_Voyage_AI
```
<!-- sdlc:auto:end topology -->
