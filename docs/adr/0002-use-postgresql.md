# ADR-0002: Use PostgreSQL as the system of record

- Status: Accepted
- Date: 2026-10-19

## Context
We need a relational store with strong integrity, flexible semi-structured attributes, and good analytics/AI options without licensing cost.

## Decision
Use PostgreSQL via Npgsql/EF Core. Use JSONB for custom attributes, trigram indexes for search, a `reporting` schema for BI, and `pgvector` later for AI search.

## Consequences
- No licence cost; broad tooling (Power BI, Metabase, Python, dbt).
- Single database serves OLTP now and reporting later; a read replica can be added if load requires.
- Team must operate PostgreSQL (backups, upgrades) unless a managed service is used.

## Alternatives considered
SQL Server (best if the organisation already standardises on it; licensing cost, newer/less mature vector features), MySQL, document databases (weaker relational integrity for asset relationships).
