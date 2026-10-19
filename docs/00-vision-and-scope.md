# 00 - Vision and Scope

## Problem

Asset information is scattered across spreadsheets, tools and people's heads. As a result:

- Security cannot reliably answer "what operating systems and versions are running, and which are obsolete?"
- IT and Digital keep separate, inconsistent lists of servers, VMs, applications and network devices.
- Procurement records purchases but cannot easily tie a purchase to the asset it produced.
- Compliance cannot show that registered assets meet internal policy.

## Vision

One authoritative asset registry, shared by every team, where each team sees the same records through the lens it needs. Data entered once is reused everywhere.

## Goals

1. A **single Asset backbone** with typed detail (server, VM, network device, application).
2. **Computed lifecycle health**: obsolete, nearing end-of-support, stale, ownerless.
3. **Role-based access** so each team has appropriate read/write rights.
4. **Full audit trail** of who changed what and when.
5. **Easy data in**: manual entry, CSV import, and an ingestion API for discovery tooling.
6. **Future-ready data**: PostgreSQL system of record with a clean path to BI and AI.

## Non-goals (for now)

- Replacing a full CMDB/ITSM suite (ticketing, change management).
- Agent-based automatic discovery (the ingestion API is the hook; agents come later).
- Financial accounting, depreciation, or general ledger integration.
- Public or customer-facing access. This is internal only.

## Scope by phase

| Phase | Scope |
|---|---|
| 0 | Blueprint docs, git, brand, static mockups |
| 1 | Foundation: solution scaffold, domain, database, local auth, RBAC, audit, generic asset CRUD, CSV |
| 2 | IT and Digital module (MVP) |
| 3 | Security module (MVP) |
| 4 | Procurement module |
| 5 | Compliance module |
| 6 | Platform: SSO, notifications, reporting schema and BI, AI search |

## Success measures

- 100% of in-scope production servers/VMs/network devices registered with an owner within 90 days of go-live.
- Security can produce the OS/version and obsolescence report in under one minute, without manual spreadsheet work.
- Every asset change is attributable to a user and timestamp.

## Assumptions

- Initial data comes from spreadsheets; discovery integration is later.
- Hosting target (on-prem vs cloud) is undecided; the system ships as containers.
- Expected volume is in the thousands to low tens of thousands of assets.
