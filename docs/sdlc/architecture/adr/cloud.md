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

Chosen option: AWS, because A single account hosts the SPA, API, worker, RDS PostgreSQL, Redis and S3 for the original uploaded files in one region near staff, with encryption at rest and in transit.

## Consequences

Ties the deployment to AWS-managed services; acceptable for an internal tool with no stated multi-cloud or residency requirement.
<!-- sdlc:auto:end adr -->