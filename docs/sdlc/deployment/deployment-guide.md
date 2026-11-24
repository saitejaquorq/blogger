# Deployment Guide

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start how-it-deploys -->
Pushes to `main` run `.github/workflows/deploy.yml`, which builds the API image (if there is a backend) and deploys the SPA to AWS Amplify. Commits touching only `docs/sdlc/**` don't deploy. Deployments can also be started from the platform's Deployment tab.
<!-- sdlc:auto:end how-it-deploys -->

<!-- sdlc:auto:start environments -->
| Name | Service | Runtime | Scaling | Environment |
|---|---|---|---|---|
| SPA Host | React 19 static build (S3 + CloudFront, its own origin) | Static assets | CDN-cached | dev |
| API Service | FastAPI 0.115 under Uvicorn (ECS Fargate / single container) | Python 3.12 | 1-2 small tasks for ~50 users | dev |
| Indexing Worker | RQ worker (same image, worker command) | Python 3.12 | 1 task | dev |
| PostgreSQL | Amazon RDS for PostgreSQL 16 with pgvector | PostgreSQL 16 | Single instance | dev |
| Redis | Redis 7 (redis:7-alpine) for the job queue | Redis 7 | Single node | dev |
| S3 Bucket | Amazon S3 document store | Managed | Managed | dev |
| Anthropic Claude | Claude API | External SaaS | Managed | dev |
| Voyage AI | Voyage embeddings API | External SaaS | Managed | dev |
| SMTP Server | SMTP relay for reset/invite email | External SaaS | Managed | dev |
<!-- sdlc:auto:end environments -->

<!-- sdlc:auto:start live -->
_Not deployed yet._
<!-- sdlc:auto:end live -->

<!-- sdlc:auto:start topology -->
```mermaid
graph LR
  n_SPA_Host["SPA Host<br/>React 19 static build (S3 + CloudFront, its own origin)"]
  n_API_Service["API Service<br/>FastAPI 0.115 under Uvicorn (ECS Fargate / single container)"]
  n_Indexing_Worker["Indexing Worker<br/>RQ worker (same image, worker command)"]
  n_PostgreSQL["PostgreSQL<br/>Amazon RDS for PostgreSQL 16 with pgvector"]
  n_Redis["Redis<br/>Redis 7 (redis:7-alpine) for the job queue"]
  n_S3_Bucket["S3 Bucket<br/>Amazon S3 document store"]
  n_Anthropic_Claude["Anthropic Claude<br/>Claude API"]
  n_Voyage_AI["Voyage AI<br/>Voyage embeddings API"]
  n_SMTP_Server["SMTP Server<br/>SMTP relay for reset/invite email"]
  n_SPA_Host -->|"HTTPS (Authorization: Bearer)"| n_API_Service
  n_API_Service -->|"TCP/TLS"| n_PostgreSQL
  n_API_Service -->|"TCP"| n_Redis
  n_API_Service -->|"HTTPS"| n_S3_Bucket
  n_API_Service -->|"HTTPS"| n_Anthropic_Claude
  n_API_Service -->|"HTTPS"| n_Voyage_AI
  n_API_Service -->|"SMTP/TLS"| n_SMTP_Server
  n_Indexing_Worker -->|"TCP"| n_Redis
  n_Indexing_Worker -->|"TCP/TLS"| n_PostgreSQL
  n_Indexing_Worker -->|"HTTPS"| n_S3_Bucket
  n_Indexing_Worker -->|"HTTPS"| n_Voyage_AI
```
<!-- sdlc:auto:end topology -->
