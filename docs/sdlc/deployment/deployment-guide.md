# Deployment Guide

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start how-it-deploys -->
Pushes to `main` run `.github/workflows/deploy.yml`, which builds the API image (if there is a backend) and deploys the SPA to AWS Amplify. Commits touching only `docs/sdlc/**` don't deploy. Deployments can also be started from the platform's Deployment tab.
<!-- sdlc:auto:end how-it-deploys -->

<!-- sdlc:auto:start environments -->
| Name | Service | Runtime | Scaling | Environment |
|---|---|---|---|---|
| Web SPA | React static site (S3 + CloudFront, SPA origin) | Static assets | CDN-served, no app scaling | dev |
| RAG API | FastAPI on ECS Fargate (API origin) | Python 3.12 / Uvicorn | 1-2 tasks, sized for ~50 users | dev |
| Indexing Worker | RQ worker on ECS Fargate | Python 3.12 | 1 task | dev |
| PostgreSQL | Amazon RDS for PostgreSQL 16 (pgvector enabled) | PostgreSQL 16 | Single instance | dev |
| Redis | Redis 7 container (ECS) or ElastiCache | redis:7-alpine | Single node | dev |
| Document Storage | Amazon S3 bucket | S3 | Managed | dev |
| Anthropic Claude API | Anthropic SaaS | External | Managed by vendor | dev |
| Voyage AI API | Voyage AI SaaS | External | Managed by vendor | dev |
| SMTP Email | SMTP server | External | Managed | dev |
<!-- sdlc:auto:end environments -->

<!-- sdlc:auto:start live -->
_Not deployed yet._
<!-- sdlc:auto:end live -->

<!-- sdlc:auto:start topology -->
```mermaid
graph LR
  n_Web_SPA["Web SPA<br/>React static site (S3 + CloudFront, SPA origin)"]
  n_RAG_API["RAG API<br/>FastAPI on ECS Fargate (API origin)"]
  n_Indexing_Worker["Indexing Worker<br/>RQ worker on ECS Fargate"]
  n_PostgreSQL["PostgreSQL<br/>Amazon RDS for PostgreSQL 16 (pgvector enabled)"]
  n_Redis["Redis<br/>Redis 7 container (ECS) or ElastiCache"]
  n_Document_Storage["Document Storage<br/>Amazon S3 bucket"]
  n_Anthropic_Claude_API["Anthropic Claude API<br/>Anthropic SaaS"]
  n_Voyage_AI_API["Voyage AI API<br/>Voyage AI SaaS"]
  n_SMTP_Email["SMTP Email<br/>SMTP server"]
  n_Web_SPA -->|"HTTPS (Authorization: Bearer)"| n_RAG_API
  n_RAG_API -->|"TLS (psycopg)"| n_PostgreSQL
  n_RAG_API -->|"TCP"| n_Redis
  n_RAG_API -->|"HTTPS"| n_Document_Storage
  n_RAG_API -->|"HTTPS"| n_Anthropic_Claude_API
  n_RAG_API -->|"HTTPS"| n_Voyage_AI_API
  n_RAG_API -->|"SMTP/TLS"| n_SMTP_Email
  n_Indexing_Worker -->|"TCP"| n_Redis
  n_Indexing_Worker -->|"TLS (psycopg)"| n_PostgreSQL
  n_Indexing_Worker -->|"HTTPS"| n_Document_Storage
  n_Indexing_Worker -->|"HTTPS"| n_Voyage_AI_API
```
<!-- sdlc:auto:end topology -->
