<!-- sdlc:auto:start adr -->
# ADR-0001: Use React 19 for Frontend

Status: Accepted  
Date: 2026-11-26

## Context and Problem Statement

Which Frontend technology does the project use? Decided in architecture v2.

## Considered Options

- React 19
- Vue
- Svelte

## Decision Outcome

Chosen option: React 19, because Delivered as a single responsive SPA for the streaming chat and admin document screens; stores the opaque session token and sends it as Authorization: Bearer, reading the API base URL from VITE_API_BASE_URL since the SPA and API are separate origins.

## Consequences

A separate SPA origin means CORS and token handling in the client rather than cookie sessions; acceptable and required by the two-origin deployment.
<!-- sdlc:auto:end adr -->