# Yali Noy

**Technical Product Builder · Product Systems · Electrical Engineering**

I build working systems from ambiguous product problems — defining the domain, designing the architecture, shipping the product and testing the failure modes.

*Portfolio status reviewed: 4 October 2026.*

**Current focus:** offline-first mobile · property lifecycle & security · safety-first automation · real-time systems

## Featured project — Aretu

[**Aretu — Personal Operating System →**](projects/aretu.md)

[Open Aretu web app ↗](https://aretu-dev.vercel.app)  
*v0.10 pre-Apple release candidate · account required · offline after sign-in · device QA pending*

<p align="center">
  <img src="https://raw.githubusercontent.com/yalinoy/yalinoy/main/assets/aretu-portfolio-2x2.png" alt="Aretu core product screens — Today, Planning, Goals and Progress" width="100%" />
</p>

Aretu turns long-term goals into weekly priorities, daily execution, review and history. Its core engineering constraint is strict local-first behavior: after a successful sign-in, ordinary reads and writes remain local through network outages.

**Signal:** SQLite-first mobile architecture · isolated Supabase sync · identity · bilingual RTL/LTR · multi-layer testing · bundle/release validation · account isolation · notification recovery

## Selected systems

### [HomeRun V2 — Property Lifecycle System](projects/homerun-v2.md)

[Open HomeRun web app ↗](https://homerun-v2-lake.vercel.app)

*V1 scope + Beta Product Pack merged and deployed · authenticated production smoke pending before official Closed Beta*

A property lifecycle system built around historical truth: effective-dated ownership and rent, relationship-derived capabilities, Row Level Security, atomic domain operations, auditable finance and private document access.

Recent additions: review-first contract import, lease-renewal actions, document-expiry reminders, structured maintenance intake, permission-respecting share links and Finance Privacy Mode.

`React` · `TypeScript` · `PostgreSQL` · `Supabase`

### [Atlas — Safety-First Research Agent](projects/atlas-ibkr-agent.md)
*Research/paper workflow · CLI + read-only dashboard · no live execution path*

An agentic research workflow built around explicit authority boundaries: hard refusal states, auditable artifacts, human review and a capability ceiling that prevents research from silently becoming execution permission.

`Python` · `YAML` · `Streamlit`

### [Sugar Game — Real-Time Multiplayer System](projects/sugar-game.md)
*Working multiplayer application · state-machine refactor next*

A four-player real-time card game that separates fast in-memory room state from persistent identity, points, friendships and leaderboards, with reconnect-aware session handling across local and hosted environments.

`Node.js` · `Socket.IO` · `PostgreSQL`

## How I build

- **Model the real problem first.** Domain rules and invariants come before feature volume.
- **Design failure modes explicitly.** Offline use, reconnects, permission boundaries, degraded storage and unsafe states are product behavior.
- **Use evidence, not ceremony.** Automated tests, integration tests, security checks, real devices and release gates challenge assumptions.
- **Keep boundaries enforceable.** Domain logic, persistence, infrastructure and authority stay separated so complexity remains controllable.

## Current direction

Preparing to begin a B.Sc. in Electrical Engineering at Tel Aviv University in October 2026.

Building deeper capability across **electrical engineering, product systems, automation and real-world technology**, with long-term interest in robotics, energy and physical systems.

Production source repositories remain private where appropriate. The case studies above document what was built, the architecture, engineering decisions, limitations and current status without exposing credentials, private data or proprietary source.
