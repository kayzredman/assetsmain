# Internal Assets Management System

A single, trusted registry of the organisation's assets (servers, VMs, network devices, applications, purchased goods) that the **Security**, **IT and Digital**, **Enterprise Business Services (Procurement)** and **Compliance** teams all work from.

> Status: **Phase 0 - blueprint, brand and mockups.** No application code yet.

## Repository map

| Path | Contents |
|---|---|
| [`docs/`](docs/README.md) | Vision, requirements, architecture, domain model, API, security, roadmap, ADRs |
| [`docs/design/brand/`](docs/design/brand/brand-guide.md) | Brand guide, design tokens, logo |
| [`mockups/`](mockups/index.html) | Static HTML/CSS mockup screens (open `mockups/index.html` in a browser) |
| `src/`, `tests/`, `web/` | Planned application code (Phase 1+), see [architecture](docs/03-architecture.md) |

## Chosen stack

.NET 10 (ASP.NET Core, EF Core) API, React + TypeScript SPA, PostgreSQL. Local accounts first, pluggable to Entra ID / AD later. See [ADRs](docs/adr/).

## Branching

- `main` - stable, reviewed, releasable.
- `dev` - integration branch; day-to-day work lands here.
- Flow: `dev` -> pull request -> `main`. Short-lived feature branches may be cut from `dev` and merged back into it.

## Viewing the mockups

Open `mockups/index.html` directly in a browser. No build step or network access is needed.
