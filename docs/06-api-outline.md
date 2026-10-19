# 06 - API Outline

Base path `/api`, JSON over HTTPS, OpenAPI at `/openapi/v1.json`. All endpoints require authentication except login and health.

## Conventions

- **Pagination:** `?page=1&pageSize=50` (max 200). Response: `{ items, page, pageSize, totalCount }`.
- **Sorting:** `?sort=name` or `?sort=-updatedAt` (leading `-` is descending).
- **Filtering:** query parameters named after fields, for example `?type=Server&status=Active&environment=Production&osVersionId=...`. Free text via `?q=`.
- **Errors:** RFC 9457 `application/problem+json` with `type`, `title`, `status`, `detail`, `traceId`, and `errors` for validation failures.
- **Concurrency:** assets carry a `rowVersion`; updates send it back and a mismatch returns `409`.
- **IDs:** GUIDs. Dates are ISO 8601 UTC.
- **Versioning:** URL-stable for MVP; breaking changes introduce `/api/v2`.

## Auth and users

| Method | Path | Permission |
|---|---|---|
| POST | `/api/auth/login` | anonymous |
| POST | `/api/auth/logout` | authenticated |
| GET | `/api/auth/me` | authenticated |
| GET/POST | `/api/users` | `users.manage` |
| GET/PUT/DELETE | `/api/users/{id}` | `users.manage` |
| PUT | `/api/users/{id}/roles` | `users.manage` |
| GET | `/api/teams` | authenticated |

## Assets (core)

| Method | Path | Permission |
|---|---|---|
| GET | `/api/assets` | `assets.read` |
| POST | `/api/assets` | `assets.write` |
| GET | `/api/assets/{id}` | `assets.read` |
| PUT | `/api/assets/{id}` | `assets.write` |
| POST | `/api/assets/{id}/status` | `assets.write` (body: target status, reason) |
| DELETE | `/api/assets/{id}` | `assets.delete` (soft delete) |
| GET | `/api/assets/{id}/relationships` | `assets.read` |
| POST/DELETE | `/api/assets/{id}/relationships` | `assets.write` |
| GET | `/api/assets/{id}/history` | `assets.read` |
| GET | `/api/assets/export` | `assets.read` (CSV) |
| POST | `/api/assets/import?dryRun=true` | `assets.import` |

Type-specific detail is nested in the asset payload under `detail` and validated per type.

## Typed convenience views (IT and Digital)

`GET /api/servers`, `/api/vms`, `/api/network-devices`, `/api/applications` are filtered projections of `/api/assets` with their detail fields.

## Reference data

| Method | Path | Permission |
|---|---|---|
| GET/POST/PUT | `/api/locations` | read: authenticated; write: `reference.manage` |
| GET/POST/PUT | `/api/os-products`, `/api/os-versions` | read: authenticated; write: `reference.manage` or Security |
| GET | `/api/asset-types` | authenticated |

## Security module

| Method | Path | Purpose |
|---|---|---|
| GET | `/api/security/summary` | Totals: obsolete, nearing EOL, stale, ownerless, unknown OS |
| GET | `/api/security/os-breakdown` | Counts by OS product and version, with health state |
| GET | `/api/security/unsupported` | Unsupported assets grouped by environment and criticality |
| GET | `/api/security/aging` | EOL timeline buckets (0-30, 31-90, 91-180, 180+ days) |
| GET | `/api/security/scorecard` | Health score per team (S) |
| GET | `/api/security/assets` | Drill-down list; filter by `health=Obsolete` etc.; supports CSV export |
| GET/POST/DELETE | `/api/security/saved-filters` | Personal saved filters |

Permission: `security.read` (Security, Admin; read-only Viewer gets summary only).

## Ingestion (Phase 2+)

`POST /api/ingest/assets` with `X-Api-Key`.

```json
{
  "source": "vcenter",
  "items": [
    {
      "externalId": "vm-1042",
      "type": "VirtualMachine",
      "name": "app-prod-07",
      "environment": "Production",
      "osName": "Windows Server 2016",
      "lastSeenAt": "2026-10-19T08:00:00Z",
      "detail": { "hypervisor": "vSphere", "vcpu": 4, "ramGb": 16 }
    }
  ]
}
```

Response: per-item `created | updated | skipped | error` with messages. Upsert key is `(source, externalId)`.

## Phase 4 and 5 (outline)

`/api/vendors`, `/api/purchase-requests`, `/api/purchase-orders`, `/api/invoices`; `/api/policies`, `/api/controls`, `/api/compliance/checks`, `/api/compliance/exceptions`, `/api/compliance/summary`.

## Health and ops

`GET /health/live`, `GET /health/ready` (anonymous, no sensitive data).
