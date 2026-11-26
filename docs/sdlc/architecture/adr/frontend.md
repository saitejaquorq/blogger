<!-- sdlc:auto:start adr -->
# ADR-0001: Use React 19 for Frontend

Status: Accepted  
Date: 2026-11-26

## Context and Problem Statement

Which Frontend technology does the project use? Decided in architecture v1.

## Considered Options

- React 19
- Vue
- Svelte

## Decision Outcome

Chosen option: React 19, because Approved stack. Delivered as a responsive single-page app for the streaming chat, history view, and admin document screens; reads the API base URL from VITE_API_BASE_URL and stores the opaque session token for Authorization: Bearer calls.

## Consequences

A SPA needs client-side routing and auth guards; it is served from its own origin separate from the API, so all auth goes through the Bearer token rather than cookies.
<!-- sdlc:auto:end adr -->