# 05 - Data Dictionary

Conventions: snake_case names, `uuid` primary keys (`id`), `created_at`/`updated_at` (timestamptz, UTC) on every table, soft delete via `is_deleted` where noted. Phase 1 covers everything in this file except the "Later phases" section.

## Enumerations

| Enum | Values |
|---|---|
| `asset_status` | Planned, Active, Retired, Decommissioned |
| `environment` | Production, UAT, Development, DR, NotApplicable |
| `criticality` | Low, Medium, High, Critical |
| `asset_type` (reference table) | Server, VirtualMachine, NetworkDevice, Application, Other |
| `relationship_type` | RunsOn, HostedBy, DependsOn, ConnectedTo |
| `location_kind` | Site, Building, Room, Rack |
| `audit_action` | Create, Update, Delete, StatusChange, Login, Import |

## Tables

### asset
| Column | Type | Notes |
|---|---|---|
| id | uuid PK | |
| asset_tag | varchar(40) | Unique, human-readable (for example `SRV-000123`) |
| name | varchar(200) | Required |
| asset_type_id | int FK | Required |
| status | asset_status | Default Planned |
| environment | environment | |
| criticality | criticality | Default Medium |
| owner_team_id | uuid FK -> team | Required when Active |
| location_id | uuid FK -> location | |
| os_version_id | uuid FK -> os_version | |
| external_id | varchar(200) | Source system key for ingestion upsert |
| source | varchar(60) | Manual, CSV, Ingest:<system> |
| acquired_at | date | |
| end_of_support_at | date | Optional override of OS-derived date |
| retired_at | date | |
| last_seen_at | timestamptz | Updated by ingestion or edits |
| notes | text | |
| custom_attributes | jsonb | Team-specific key/values |
| is_deleted | bool | Soft delete |

Indexes: unique `asset_tag`; `(asset_type_id, status)`; `(owner_team_id)`; `(os_version_id)`; `(environment, criticality)`; `(last_seen_at)`; unique partial `(source, external_id)` where `external_id` is not null; GIN on `custom_attributes`; trigram on `name` for search.

### server_detail (asset_id PK/FK)
hostname, ip_addresses (text[]), server_role, cpu_cores, ram_gb, storage_gb, manufacturer, model, serial_number.

### vm_detail (asset_id PK/FK)
hypervisor, cluster, host_asset_id (FK -> asset), vcpu, ram_gb, disk_gb, hostname, ip_addresses (text[]).

### network_device_detail (asset_id PK/FK)
device_role (Switch, Router, Firewall, AccessPoint, Other), vendor, model, firmware_version, serial_number, management_ip, port_count.

### application_detail (asset_id PK/FK)
stack, repo_url, app_url, business_owner, technical_owner, data_classification (Public, Internal, Confidential, Restricted), hosting_notes.

### os_product
| Column | Type |
|---|---|
| id | int PK |
| vendor | varchar(100) |
| family | varchar(100) |

### os_version
| Column | Type | Notes |
|---|---|---|
| id | uuid PK | |
| os_product_id | int FK | |
| version_label | varchar(100) | For example `Windows Server 2016` |
| released_at | date | |
| mainstream_end_at | date | |
| extended_end_at | date | Treated as end-of-support |

Unique `(os_product_id, version_label)`.

### asset_relationship
id, source_asset_id, target_asset_id, relationship_type. Unique `(source, target, type)`; check `source <> target`.

### location
id, name, kind, parent_id (self FK), address_notes. Index on `parent_id`.

### team
id, name (unique), code, description.

### app_user (ASP.NET Core Identity)
Identity columns plus `display_name`, `team_id`, `is_active`, `last_login_at`.

### app_role / user_role
Roles: Admin, ITDigital, Security, Procurement, Compliance, Viewer. Many-to-many via `user_role`.

### audit_log
| Column | Type |
|---|---|
| id | bigint identity PK |
| occurred_at | timestamptz |
| user_id | uuid (nullable for system) |
| action | audit_action |
| entity_type | varchar(80) |
| entity_id | varchar(80) |
| changes | jsonb (old/new per field) |
| correlation_id | varchar(64) |
| reason | text |

Indexes: `(entity_type, entity_id)`, `(occurred_at)`, `(user_id)`.

### saved_filter
id, user_id, name, scope (assets/security), definition jsonb.

### api_key (ingestion)
id, name, key_hash, service_user_id, created_at, expires_at, last_used_at.

## Later phases (outline only)

- **Procurement:** vendor, purchase_request, purchase_order, purchase_line, invoice, purchase_line_asset (link), warranty fields.
- **Compliance:** policy, control, control_asset_type, compliance_check, compliance_exception.

## Reporting views (planned, `reporting` schema)

`reporting.asset_health` (asset with computed health state), `reporting.os_summary`, `reporting.team_scorecard`. Read-only role `reporting_reader` only.
