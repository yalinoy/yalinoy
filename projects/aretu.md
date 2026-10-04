# Aretu — Personal Operating System

> **Role:** Product architecture, system design, implementation and validation  
> **Status:** v0.10 pre-Apple release candidate · entry-flow and backend hardening merged · consolidated device QA and TestFlight pending\
> **Source:** Private

[Open Aretu web app ↗](https://aretu-dev.vercel.app)

Aretu is a mobile-first personal operating system for turning long-term goals into weekly priorities, daily execution and measurable review.

The hard part is not task entry. It is building a system that stays useful offline, preserves a coherent history across devices, and keeps product complexity from leaking into the core domain.

*Status reviewed: 4 October 2026.*

## Product in four screens

<p align="center">
  <img src="https://raw.githubusercontent.com/yalinoy/yalinoy/main/assets/aretu-portfolio-2x2.png" alt="Aretu core screens — Today, Planning, Goals and Progress" width="100%" />
</p>

**Today** turns the plan into execution. **Planning** limits the week to a small number of priorities. **Goals** connects work to longer-term outcomes. **Progress** turns recorded behavior into feedback for the next cycle.

![Aretu product loop](https://raw.githubusercontent.com/yalinoy/yalinoy/main/assets/aretu-product-loop.svg)

## At a glance

| | |
|---|---|
| **Core problem** | Turn long-term intent into a repeatable execution loop |
| **Product model** | Goals → weekly planning → daily execution → review → history |
| **Architecture** | Offline-first local SQLite with an isolated sync layer |
| **Reliability focus** | Sync boundaries, identity, bundle checks, multi-layer testing and device QA |
| **Stack** | TypeScript · React Native · Expo · SQLite · Supabase · PostgreSQL |

## What the system does

Aretu connects goal-setting to actual execution. Users can structure goals, plan a week, schedule tasks and habits, execute the day, review plan-versus-actual and preserve the result as a personal history.

The product is bilingual and supports Hebrew RTL and English LTR. An account is required: signed-out users see authentication, and initial sign-in needs the backend. After a successful sign-in, the persisted session supports ordinary SQLite reads and writes offline, including cold launches. First-time account setup and initial synchronization can still require connectivity.

## Architecture

![Aretu offline-first architecture](https://raw.githubusercontent.com/yalinoy/yalinoy/main/assets/aretu-architecture.svg)

The design invariant is simple: **after sign-in, the network is outside the ordinary product read/write path.**

## Key engineering decisions

### 1. Local-first instead of network-first
Normal reads and writes happen against SQLite. Synchronization is a separate layer, so poor connectivity does not become an application-wide failure mode.

### 2. Keep architecture boundaries executable
Domain logic, repository interfaces, SQLite, remote services, sync and platform-specific integrations are separated deliberately. Architecture tests help prevent those boundaries from eroding as features grow.

### 3. Treat release engineering as product engineering
The verification pipeline extends beyond unit tests: formatting, linting, strict types, domain tests, real SQLite integration tests, PostgreSQL integration tests, platform diagnostics and exported-bundle inspection all exist to catch different classes of failure.

### 4. Use real devices to challenge abstractions
Automated tests cannot see everything. Real iPhone QA exposed layout and interaction defects that the test suite missed; those failures were then turned into regression checks.

## Recent implementation progress

- **Account-required entry:** authentication gates the product; onboarding state belongs to the account rather than a device-wide flag.
- **Account isolation:** account switching and sign-out coordinate local identity, sync and reminder ownership. Failed notification cancellations are retained for retry.
- **Permission recovery:** notification denial preserves reminder settings; returning from system settings refreshes permission state and reconciles schedules.
- **Release preparation:** privacy-page generation, account-lifecycle protections, localized unknown-route handling and least-privilege backend grants are implemented.
- **Browser handling:** a dedicated Private Browsing state and updated Google sign-in presentation are merged.

## Recorded verification

The merged September entry-flow change records **1,760 unit tests, 421 SQLite integration tests and 292 PostgreSQL tests passing**, plus clean web/iOS exports and bundle scanning. These are recorded results for that change, not a new test run or proof of signed-device readiness.

## What this project demonstrates

- product thinking translated into system architecture
- offline-first mobile design
- sync and identity boundaries
- database/security discipline
- layered testing and release validation
- willingness to keep a release gate closed until physical QA is complete

## Current status

The pre-Apple release candidate, backend hardening, account-required entry flow and browser follow-up are merged. A preliminary Expo Go handset pass informed the fixes; the consolidated physical-device matrix remains pending.

Before a TestFlight pilot: complete the device and OAuth checks, Apple account/signing setup, Apple sign-in and account-lifecycle requirements, signed-build validation and App Store Connect submission work. No App Store or TestFlight launch is claimed.

---

The production source repository remains private. This case study documents the product model, architecture and engineering decisions without publishing source code, credentials or private configuration.
