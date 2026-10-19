# 07 - RBAC and Security

## Roles

| Role | Typical user |
|---|---|
| Admin | Platform administrators |
| ITDigital | IT and Digital staff |
| Security | Security team |
| Procurement | Enterprise Business Services |
| Compliance | Compliance team |
| Viewer | Management / read-only |

A user may hold several roles. Permissions are the union of the roles.

## Permission matrix

R = read, W = write (create/update), D = delete/retire, M = manage, - = no access, s = summary only.

| Permission area | Admin | ITDigital | Security | Procurement | Compliance | Viewer |
|---|---|---|---|---|---|---|
| Assets (`assets.read/write/delete`) | R W D | R W D | R | R | R | R |
| Asset import/export (`assets.import`) | W | W | R (export) | R (export) | R (export) | - |
| Relationships | W | W | R | R | R | R |
| Reference data: locations | M | W | R | R | R | R |
| Reference data: OS catalog | M | R | W | R | R | R |
| Security module (`security.read`) | R | R | R W | - | R | s |
| Saved filters (own) | W | W | W | W | W | W |
| Procurement data (Phase 4) | R | R | - | R W | R | - |
| Compliance data (Phase 5) | R | R | R | - | R W | s |
| Audit log | R | - | R | - | R | - |
| Users, roles, teams (`users.manage`) | M | - | - | - | - | - |
| Ingestion API (service account) | - | W | - | - | - | - |

Enforcement: ASP.NET Core authorization policies per permission, applied on every endpoint and re-checked in the application layer for sensitive operations. The SPA hides unavailable actions for usability only.

## Authentication

- Local accounts via ASP.NET Core Identity: strong password policy (min 12 chars, breached-password check where feasible), lockout after 5 failed attempts, optional TOTP MFA planned.
- Session: HTTP-only, Secure, SameSite=Strict cookie with sliding expiry and absolute cap; CSRF protection via anti-forgery token for state-changing requests.
- Ingestion uses API keys: stored hashed, scoped to a service account, rotatable, expiring.
- Future: Entra ID / AD via OIDC with group-to-role mapping; local accounts retained only for a break-glass admin.

## Audit

- All writes to assets, relationships, reference data, users and roles are logged with user, time, correlation ID and field-level old/new values.
- Sign-in successes and failures are logged.
- Audit rows are append-only; the application database role has no UPDATE/DELETE privilege on `audit_log`.
- Default retention 7 years; export available to Security and Compliance.

## Data classification and protection

- Asset inventory is **Internal/Confidential**: IPs, topology and OS versions are valuable to an attacker. Access follows least privilege.
- TLS everywhere; database encrypted at rest (disk or managed-service encryption); backups encrypted.
- No secrets, passwords or credentials are stored in asset records or custom attributes (validated: field-name heuristics warn on `password`, `secret`, `token`).
- Application-level logging excludes personal data and secrets.

## Threat notes (STRIDE-lite)

| Threat | Mitigation |
|---|---|
| Spoofing: stolen session | Secure cookies, short sliding expiry, lockout, MFA planned |
| Tampering: unauthorised inventory edits | RBAC, audit log, optimistic concurrency |
| Repudiation | Immutable audit log tied to user identity |
| Information disclosure: topology leak | Least-privilege roles, export permissions, CSV export logged |
| Denial of service | Pagination caps, rate limiting on login and ingestion, request size limits |
| Elevation of privilege | Server-side policy checks, tests that assert denied access per role |
| Malicious CSV | Size/row limits, strict parsing, no formula injection on export (prefix `=+-@` cells) |

## Secure development practices

- Dependency and container scanning in CI; pinned base images.
- Secrets via environment/secret store; `.env` files are git-ignored.
- Mandatory pull request review for `main`.
- Security-focused integration tests: every endpoint tested for unauthenticated and wrong-role access.
