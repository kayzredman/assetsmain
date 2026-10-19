# ADR-0004: Local authentication first, pluggable to Entra ID / AD

- Status: Accepted
- Date: 2026-10-19

## Context
SSO with the corporate directory is the eventual goal, but requires tenant registration and group design that are not ready. Delivery should not wait on it.

## Decision
Ship Phase 1 with ASP.NET Core Identity local accounts. Authorization is permission-based and independent of how a user signed in. An identity abstraction allows adding an OIDC scheme (Entra ID) and mapping directory groups to roles later. A break-glass local admin is retained after SSO.

## Consequences
- Fast start; no external dependency for MVP.
- Temporary password-management burden (policy, lockout, reset) until SSO.
- Migration to SSO is configuration plus a user-linking step, not a rewrite.

## Alternatives considered
SSO from day one (blocked on directory setup), LDAP bind (viable for on-prem AD but less modern than OIDC).
