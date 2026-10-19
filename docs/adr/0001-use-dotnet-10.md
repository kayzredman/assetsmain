# ADR-0001: Use ASP.NET Core on .NET 10 for the backend

- Status: Accepted
- Date: 2026-10-19

## Context
The organisation runs a Microsoft-centric domain (Active Directory). The system needs strong identity integration, long-term support, and a path to analytics/AI.

## Decision
Build the backend with ASP.NET Core on .NET 10 (LTS), using EF Core for data access.

## Consequences
- First-class Entra ID / AD and policy-based authorization support.
- Strong typing, mature tooling, good performance, LTS support window.
- Analytics and AI are not constrained: they work against the database and API, and Python services can be added alongside.
- Team needs .NET skills; mitigated by a conventional layered structure.

## Alternatives considered
Next.js full-stack (fast, but weaker fit for AD-centric environment), Django (strong data tooling, less natural AD fit), separate Node/NestJS API.
