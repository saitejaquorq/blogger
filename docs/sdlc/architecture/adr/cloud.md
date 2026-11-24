<!-- sdlc:auto:start adr -->
# ADR-0004: Use AWS for Cloud

Status: Accepted  
Date: 2026-11-24

## Context and Problem Statement

Which Cloud technology does the project use? Decided in architecture v1.

## Considered Options

- AWS
- Azure
- GCP

## Decision Outcome

Chosen option: AWS, because The approved cloud; one account provides ECS/Fargate for the API and worker, RDS PostgreSQL, ElastiCache Redis, and S3 for original documents, in a region near staff since no data-residency rule is stated.

## Consequences

All-in on AWS; no multi-cloud portability, which is acceptable for an internal tool.
<!-- sdlc:auto:end adr -->