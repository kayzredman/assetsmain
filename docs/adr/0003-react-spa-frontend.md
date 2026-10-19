# ADR-0003: React + TypeScript single-page app for the frontend

- Status: Accepted
- Date: 2026-10-19

## Context
The UI is data-dense (tables, filters, dashboards, forms) and used by internal staff on desktop browsers.

## Decision
Build a React + TypeScript SPA with Vite, TanStack Query for server state, and a typed API client generated from the OpenAPI spec. Theme driven by the design tokens in `docs/design/brand/design-tokens.css`.

## Consequences
- Clear separation between UI and API; the API can serve future clients (scripts, BI, other apps).
- Rich component ecosystem for tables and charts.
- Requires CORS/cookie configuration and a CI step to regenerate the client.

## Alternatives considered
Blazor (single language, but smaller ecosystem for dashboard/table components), server-rendered Razor/HTMX (simpler, less interactive).
