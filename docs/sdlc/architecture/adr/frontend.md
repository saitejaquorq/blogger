<!-- sdlc:auto:start adr -->
# ADR-0001: Use React 19 for Frontend

Status: Accepted  
Date: 2026-11-24

## Context and Problem Statement

Which Frontend technology does the project use? Decided in architecture v1.

## Considered Options

- React 19
- Vue
- Svelte

## Decision Outcome

Chosen option: React 19, because Delivered as a responsive SPA on its own origin that stores the opaque session token and streams Claude answers with citations into the chat thread, plus the admin document screen.

## Consequences

A SPA needs its own build/host and cannot share cookies with the API, so sessions ride as Authorization: Bearer tokens rather than browser cookies.
<!-- sdlc:auto:end adr -->