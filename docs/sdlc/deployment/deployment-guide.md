# Deployment Guide

> Maintained by QuorQ AI SDLC. Text between `sdlc:auto` markers is regenerated; anything outside them is yours and is kept.

<!-- sdlc:auto:start how-it-deploys -->
Pushes to `main` run `.github/workflows/deploy.yml`, which builds the API image (if there is a backend) and deploys the SPA to AWS Amplify. Commits touching only `docs/sdlc/**` don't deploy. Deployments can also be started from the platform's Deployment tab.
<!-- sdlc:auto:end how-it-deploys -->

<!-- sdlc:auto:start environments -->
| Name | Service | Runtime | Scaling | Environment |
|---|---|---|---|---|
| Web SPA | React 19 static build on S3 + CloudFront (SPA origin) | Static assets | CDN-cached, single bucket | dev |
| API Service | FastAPI 0.115 on ECS Fargate (API origin, separate host) | Python 3.12 / uvicorn | 1-2 small tasks for ~50 users | dev |
| Indexing Worker | FastAPI 0.115 arq worker on ECS Fargate | Python 3.12 | 1 task | dev |
| PostgreSQL | Amazon RDS for PostgreSQL 16 with pgvector | PostgreSQL 16 | single small instance, encryption at rest enabled | dev |
| Redis | Redis 7 (redis:7-alpine) on ECS / ElastiCache | Redis 7 | single node | dev |
| Document Store | Amazon S3 bucket | Object storage | managed, private bucket, server-side encryption | dev |
<!-- sdlc:auto:end environments -->

<!-- sdlc:auto:start live -->
_Not deployed yet._
<!-- sdlc:auto:end live -->

<!-- sdlc:auto:start topology -->
```mermaid
graph LR
  n_Web_SPA["Web SPA<br/>React 19 static build on S3 + CloudFront (SPA origin)"]
  n_API_Service["API Service<br/>FastAPI 0.115 on ECS Fargate (API origin, separate host)"]
  n_Indexing_Worker["Indexing Worker<br/>FastAPI 0.115 arq worker on ECS Fargate"]
  n_PostgreSQL["PostgreSQL<br/>Amazon RDS for PostgreSQL 16 with pgvector"]
  n_Redis["Redis<br/>Redis 7 (redis:7-alpine) on ECS / ElastiCache"]
  n_Document_Store["Document Store<br/>Amazon S3 bucket"]
  n_Web_SPA -->|"HTTPS (Authorization: Bearer)"| n_API_Service
  n_API_Service -->|"TLS/5432"| n_PostgreSQL
  n_API_Service -->|"TCP/6379"| n_Redis
  n_API_Service -->|"HTTPS (S3 API)"| n_Document_Store
  n_Indexing_Worker -->|"TCP/6379"| n_Redis
  n_Indexing_Worker -->|"TLS/5432"| n_PostgreSQL
  n_Indexing_Worker -->|"HTTPS (S3 API)"| n_Document_Store
```
<!-- sdlc:auto:end topology -->
