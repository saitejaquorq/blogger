<!-- sdlc:auto:start adr -->
# ADR-0004: Use AWS for Cloud

Status: Accepted  
Date: 2026-11-26

## Context and Problem Statement

Which Cloud technology does the project use? Decided in architecture v2.

## Considered Options

- AWS
- Azure
- GCP

## Decision Outcome

Chosen option: AWS, because Hosts the SPA, API, worker, RDS PostgreSQL, Redis and S3 document storage in one account and region near staff; S3 durably holds the original uploads behind citations.

## Consequences

Single-cloud lock-in is acceptable for an internal tool; no data-residency constraint was stated so region is engineering's choice.
<!-- sdlc:auto:end adr -->