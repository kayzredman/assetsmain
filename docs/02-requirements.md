# 02 - Requirements

Priority: **M** = must (MVP), **S** = should (soon after MVP), **C** = could (later).

## Functional requirements

### Core registry (all teams)

| ID | Requirement | Pri |
|---|---|---|
| FR-01 | Create, view, edit and retire assets with a unique asset tag | M |
| FR-02 | Asset types: Server, VM, Network Device, Application, Other (extensible) | M |
| FR-03 | Asset status: Planned, Active, Retired, Decommissioned | M |
| FR-04 | Assign owner team, location, environment (Prod/UAT/Dev), criticality | M |
| FR-05 | Search, filter, sort and paginate the asset list | M |
| FR-06 | Link assets (runs-on, hosted-by, depends-on) and show a relationship view | M |
| FR-07 | Custom attributes per asset (key/value) | S |
| FR-08 | Audit trail per asset (who, what, when, old/new values) | M |
| FR-09 | CSV import with dry-run preview and error report; CSV export | M |
| FR-10 | Location hierarchy: site > building > room > rack | M |
| FR-11 | Ingestion API so tools/scripts can push inventory | S |

### IT and Digital

| ID | Requirement | Pri |
|---|---|---|
| FR-20 | Server details: hostname, IP(s), OS, version, role, CPU/RAM/storage | M |
| FR-21 | VM details: hypervisor, host, vCPU/RAM/disk, OS, version | M |
| FR-22 | Network device details: vendor, model, firmware, serial, management IP, ports | M |
| FR-23 | Application details: stack, repo URL, owner, environment, URL, hosting asset | M |
| FR-24 | "What runs on this host" and "what does this app depend on" views | M |

### Security

| ID | Requirement | Pri |
|---|---|---|
| FR-30 | Dashboard: asset count by OS and version | M |
| FR-31 | Obsolete (past EOL) and nearing-EOL (configurable window, default 90 days) counts and lists | M |
| FR-32 | Unsupported assets broken down by environment and criticality | M |
| FR-33 | Stale assets (not updated/seen in N days) and assets with no owner | M |
| FR-34 | Health/aging score per asset and per team | S |
| FR-35 | Drill-down lists with CSV export and saved filters | M |
| FR-36 | OS/version EOL catalog maintained by Admin or Security | M |

### Procurement (Phase 4)

| ID | Requirement | Pri |
|---|---|---|
| FR-40 | Vendor register | M |
| FR-41 | Purchase request, order and invoice records | M |
| FR-42 | Approval workflow for requests | S |
| FR-43 | Warranty and contract dates with expiry alerts | S |
| FR-44 | Link a purchase line to the asset(s) it produced | M |

### Compliance (Phase 5)

| ID | Requirement | Pri |
|---|---|---|
| FR-50 | Policy and control library | M |
| FR-51 | Map controls to asset types | M |
| FR-52 | Record check results/attestations per asset | M |
| FR-53 | Exceptions with owner, reason and expiry | M |
| FR-54 | Compliance status reports by asset, type and team | M |

### Platform

| ID | Requirement | Pri |
|---|---|---|
| FR-60 | Local account sign-in, user and role administration | M |
| FR-61 | Entra ID / AD single sign-on with group-to-role mapping | S |
| FR-62 | Email/Teams notifications for expiry events | C |
| FR-63 | Reporting schema and BI connectivity | C |
| FR-64 | AI-assisted search over assets and docs | C |

## Non-functional requirements

| ID | Area | Requirement |
|---|---|---|
| NFR-01 | Security | All traffic over TLS; passwords hashed (ASP.NET Core Identity, PBKDF2/Argon2 policy); lockout on repeated failures |
| NFR-02 | Security | Role checks enforced server-side on every endpoint; UI hiding is cosmetic only |
| NFR-03 | Audit | Every create/update/delete on assets and reference data is audit-logged and immutable to non-admins |
| NFR-04 | Performance | List and dashboard queries under 1 second at 50,000 assets (p95) |
| NFR-05 | Availability | Business-hours availability target 99.5%; nightly database backups with tested restore |
| NFR-06 | Scalability | Stateless API, horizontally scalable; database is the only stateful component |
| NFR-07 | Portability | Runs as containers; configuration via environment variables |
| NFR-08 | Maintainability | Layered architecture, automated tests on domain rules and APIs, typed API client generated from OpenAPI |
| NFR-09 | Accessibility | UI targets WCAG 2.1 AA (contrast, keyboard navigation, labels) |
| NFR-10 | Observability | Structured logging, health endpoints, request correlation IDs |
| NFR-11 | Data | Soft-delete for assets; retention policy for audit log (default 7 years, configurable) |
