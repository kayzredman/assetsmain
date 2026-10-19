# ADR-0005: Single Asset backbone with typed detail tables

- Status: Accepted
- Date: 2026-10-19

## Context
Four teams need different views of overlapping data: servers, VMs, network devices, applications and purchased goods. Separate tables per type would duplicate common fields (owner, location, status, lifecycle) and make cross-type queries (for example, all obsolete assets) painful.

## Decision
Use one `asset` table for common fields, with 1:0..1 detail tables per type (`server_detail`, `vm_detail`, `network_device_detail`, `application_detail`) and a JSONB `custom_attributes` column for team-specific extras. Relationships between assets are modelled in `asset_relationship`.

## Consequences
- Cross-type reporting (Security dashboard, ownership, lifecycle) is a single query.
- New asset types add a detail table (or just use custom attributes) without touching the backbone.
- Some queries need a join to the detail table; acceptable at expected volumes.
- Validation is type-aware in the application layer.

## Alternatives considered
Table-per-type with no shared table (duplication), pure JSONB document per asset (weak integrity and slower filtering), EAV (complex and slow).
