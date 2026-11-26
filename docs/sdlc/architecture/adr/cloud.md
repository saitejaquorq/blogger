<!-- sdlc:auto:start adr -->
# ADR-0004: Use AWS for Cloud

Status: Accepted  
Date: 2026-11-26

## Context and Problem Statement

Which Cloud technology does the project use? Decided in architecture v1.

## Considered Options

- AWS
- GCP
- Azure

## Decision Outcome

Chosen option: AWS, because Approved stack. Hosts the SPA, the FastAPI API and worker, RDS PostgreSQL, Redis, and S3 for original documents in one account, in a region near staff.

## Consequences

Single-cloud coupling; acceptable for an internal tool. Encryption at rest on RDS and S3 and TLS in transit protect the stored documents and credentials.
<!-- sdlc:auto:end adr -->