# Documentation Index

Read in order for the full picture, or jump to what you need.

| # | Document | Purpose |
|---|---|---|
| 00 | [Vision and Scope](00-vision-and-scope.md) | Why we are building this, what is in and out |
| 01 | [Stakeholders and Personas](01-stakeholders-and-personas.md) | The four teams and what they need |
| 02 | [Requirements](02-requirements.md) | Functional and non-functional requirements |
| 03 | [Architecture](03-architecture.md) | C4 diagrams, layering, auth, ingestion, analytics path |
| 04 | [Domain Model](04-domain-model.md) | Entities, relationships, lifecycle, obsolescence rules |
| 05 | [Data Dictionary](05-data-dictionary.md) | Tables, fields, enums, indexes |
| 06 | [API Outline](06-api-outline.md) | Endpoints and conventions |
| 07 | [RBAC and Security](07-rbac-and-security.md) | Roles, permission matrix, audit, threats |
| 08 | [Roadmap](08-roadmap.md) | Phases, milestones, risks |
| 09 | [Build Plan](09-build-plan.md) | The approved execution plan |
| - | [Brand Guide](design/brand/brand-guide.md) | Colors, type, logo, tokens |
| - | [ADRs](adr/) | Recorded architecture decisions |

Diagrams are written in [Mermaid](https://mermaid.js.org/) and render in GitHub, GitLab and most editors.

## Conventions

- Docs are the source of truth until code exists; when they diverge, update the doc in the same pull request.
- Decisions that are hard to reverse get an ADR in [`adr/`](adr/).
