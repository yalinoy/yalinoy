# HomeRun V2 — Property Lifecycle System

> **Role:** Product architecture, domain modeling and implementation  
> **Status:** V1 scope + Beta Product Pack merged and deployed · authenticated production smoke pending · official Closed Beta not started\
> **Source:** Private

[Open HomeRun web app ↗](https://homerun-v2-lake.vercel.app)

HomeRun V2 is a property lifecycle management platform for residential rental properties — a system of record for the life of a property from acquisition to sale.

The hard part is not CRUD. It is preserving **historical truth** across years while different people gain and lose different kinds of access, money changes hands, maintenance work becomes financial history and documents remain visible only to the right people.

A property can change owners without changing managers. A tenant can leave without erasing the lease they were part of. Rent can change without rewriting what was true last month. A maintenance ticket can become an expense without double-counting. A former tenant, current tenant, co-owner and delegated manager can all need different views of the same asset.

*Status reviewed: 4 October 2026; latest recorded hosted verification: 3 October 2026.*

## At a glance

| | |
|---|---|
| **Frontend** | React · TypeScript · Vite · TanStack Query |
| **Backend** | Supabase Auth · PostgreSQL · Row Level Security |
| **Architecture** | Domain-owned features + isolated provider boundary |
| **Security model** | Capabilities derived from relationships |
| **Historical model** | Effective-dated records + append/supersede semantics |
| **Implemented domains** | Identity/access · property lifecycle · leases/tenants · finance · maintenance · documents · inspections · Action Center |
| **Reliability focus** | Atomic operations · adversarial allow/deny tests · schema reproducibility · storage boundary checks |
| **Current gate** | Authenticated production smoke and invitation-day revalidation before the official beta cohort |

## Product model

```mermaid
flowchart LR
    A[Workspace] --> P[Property]
    P --> O[Ownership history]
    P --> L[Leases]
    L --> M[Lease members]
    L --> R[Rent terms]
    L --> AL[Allocation terms]
    P --> F[Financial core]
    P --> MT[Maintenance]
    P --> D[Documents]
    P --> I[Inspections]
    P --> AC[Action Center]
    P --> T[Timeline]

    U[Profile] -. membership / delegation / tenancy .-> P
```

The core principle is simple:

**economic ownership, operational access and tenancy are different relationships.**

They should not be collapsed into one global `user.role`.

## What is implemented

### Identity and access

- Supabase Auth profile foundation and Google OAuth
- workspaces and membership administration
- role-to-capability model
- invitations and revocation flows
- property-specific access grants
- effective-access reporting
- audit records for sensitive operations
- adversarial allow/deny RLS tests

### Property lifecycle

- properties and acquisition records
- person and legal-entity owner holders
- effective-dated ownership history
- ownership-change and sale operations
- portfolio and Property Passport surfaces
- timeline events and audit records emitted with domain operations

### Leases and tenants

- leases, lease members and tenant invitations
- effective-dated rent and allocation terms
- guarantees
- activation, renewal, ending and cancellation
- tenant replacement and participation history
- current and past tenancy read models
- occupancy derived from active leases
- tenant access that expires when participation ends

### Financial core

- rent-charge generation on settled temporal facts
- append-only financial corrections instead of destructive edits
- owner-side expense recording and lifecycle integration
- maintenance closure that creates exactly one linked expense
- database-side idempotency and auditability around financially meaningful operations

### Maintenance

- service tickets and explicit lifecycle transitions
- tenant-safe read surfaces that do not expose quotes or owner costs
- professional contacts and manual quote workflow
- follow-up behavior that preserves closed-ticket financial history

### Documents, inspections and Action Center

- private document storage behind a trusted server boundary
- explicit document visibility and recipients rather than a general-purpose ACL engine
- move-in / move-out inspection capture and tenant acknowledgement
- property timeline with reader-specific payload filtering
- derived Action Center items for expiring guarantees, overdue rent, maintenance approvals and document attention
- dismiss/snooze state keyed to the underlying domain state so stale alerts naturally return when facts change

## Beta Product Pack — implemented

- **Review-first Smart Import:** purchase and rental contracts become editable proposals before canonical property or lease creation. Extraction runs in the browser; the source document is retained through the existing private document flow.
- **Lease Renewal Actions:** expiry reminders lead to renewal, end or defer decisions while preserving predecessor history.
- **Document expiry and replacement:** warnings derive from canonical dates; replacing a document preserves its historical record.
- **Structured maintenance intake:** tenant reports and authorized photos converge on one ticket even when an attachment needs a retry.
- **Share links:** lightweight WhatsApp/native-share entry points return users through normal authentication and grant no additional access.
- **Finance Privacy Mode:** owner-side financial values can be masked without changing stored amounts, calculations or authorization.
- **Onboarding:** persona guidance and address-first prefill simplify entry without changing permissions or creating a separate property model.

## Recorded verification

The final product-pack stack records **1,074 application tests, 2,803 database/policy assertions and 130 passing browser tests across desktop/mobile and Hebrew/English**; two browser cases were intentionally skipped. These are recorded stack results, not a fresh run performed for this portfolio update.

The final three implementation PRs passed all three CI jobs before merging. The 3 October verification records the production deployment ready and hosted migration parity at **48/48**. Authenticated production smoke is still an explicit release gate.

## Engineering decisions

### 1. History is a first-class product requirement

A property exists for years. Important facts are closed, superseded or appended rather than silently overwritten. The system should be able to answer **what was true on a specific date?**

### 2. Capabilities come from relationships

Access is derived from workspace membership, property delegation and tenancy. This supports the real-world case where one person can be an owner in one place and a tenant somewhere else.

### 3. High-value operations are atomic

Property creation, ownership changes, sale, financial corrections and maintenance closure are database-side operations where required state, timeline and audit records succeed or fail together.

### 4. RLS is treated as a testable security boundary

Sensitive access rules ship with explicit **allow and deny** tests. CI rebuilds the database from committed migrations and fails if generated application types drift from the schema.

### 5. Files are authorized by records, not paths

The browser does not get general Storage authority. Document access is decided from the database record and a trusted boundary issues the short-lived file access required for that one authorized object.

### 6. Derived state stays derived

Occupancy, overdue state and Action Center items are computed from domain truth instead of duplicated into writable status fields that can silently become stale.

## Architecture

```text
src/
  features/
    identity/
    accounts/
    properties/
    leases/
    finance/
    maintenance/
    documents/
    inspections/
    action-center/
  infrastructure/       Provider-specific boundary
  shared/               Shared UI and cross-cutting primitives

supabase/
  migrations/           Schema, RLS, predicates and atomic operations
  functions/            Trusted document-storage boundary
  tests/                Security / migration verification

docs/                   Product and engineering contracts
```

The import boundary is enforced rather than merely documented: provider-specific infrastructure stays isolated from feature-owned domain code.

## What this project demonstrates

- complex domain modeling before premature feature expansion
- temporal / effective-dated data design
- capability-based authorization
- PostgreSQL and RLS security discipline
- financially meaningful atomic operations and idempotency
- private-file authorization design
- derived workflows that avoid stale duplicated state
- product scope discipline across a long-lived system of record

## Current status

The functional V1 scope, engineering hardening and all eight Beta Product Pack slices are merged. The final implementation stack was deployed and its hosted configuration rechecked on 3 October 2026.

**The official Closed Beta has not started.** The remaining gate is a recorded authenticated production smoke: sign-in and deep-link return, document upload/download, draft-inspection privacy, historical access and browser checks. External invitations also depend on the documented hosting-plan decision.

After those gates close, the target is a small cohort of **3–8 landlords** and a full month with **zero data-integrity or access incidents**. That is an acceptance criterion, not an achieved usage metric.

HomeRun records financial history; money movement, wallets, payouts and full accounting remain outside this release.

---

The production repository remains private. The public case study focuses on the domain model and engineering decisions without exposing private configuration or data.
