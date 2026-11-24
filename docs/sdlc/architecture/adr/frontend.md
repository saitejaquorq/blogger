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

Chosen option: React 19, because The approved frontend; delivered as a responsive Vite SPA that renders the streaming chat, citations and admin document screen, reading the API base URL from VITE_API_BASE_URL and sending Authorization: Bearer on every call.

## Consequences

A SPA on its own origin cannot use cookies across to the API origin, so sessions use an opaque bearer token stored by the frontend rather than an HttpOnly cookie.
<!-- sdlc:auto:end adr -->