# Yali Noy

**Technical Product Builder · Systems Thinker · Electrical Engineering**

I build products where software, systems and real-world constraints meet — taking ambiguous problems from product definition to architecture, implementation, testing and release decisions.

## Featured project — Aretu

[**Aretu — Personal Operating System →**](projects/aretu.md)  
*Release candidate · mobile · offline-first*

![Aretu core product screens](assets/aretu-core-screens.webp)

Aretu turns long-term goals into weekly priorities, daily execution, review and history. The product is designed around a strict local-first invariant: ordinary use should continue even when the backend disappears.

**Engineering signal:** offline-first architecture · SQLite/Supabase sync boundary · identity · bilingual RTL/LTR · multi-layer testing · bundle/release validation · real-device QA

## Selected systems

### [HomeRun V2 — Property Lifecycle System](projects/homerun-v2.md)
*Core domain through leases & tenants implemented · financial core next*

A property-management system designed around historical truth: effective-dated ownership and rent, relationship-derived capabilities, Row Level Security, auditability and atomic lifecycle operations.

`React` · `TypeScript` · `PostgreSQL` · `Supabase`

### [Atlas — Safety-First Research Agent](projects/atlas-ibkr-agent.md)
*Research/paper workflow only · no live execution path*

An agentic research workflow built around explicit authority boundaries: hard refusal states, append-only logs, human review and research artifacts that cannot silently become execution permission.

`Python` · `YAML` · local automation

### [Sugar Game — Real-Time Multiplayer System](projects/sugar-game.md)
*Working multiplayer application*

A four-player realtime card game that separates fast in-memory room state from persistent identity, points, friendships and leaderboards, with reconnect-aware session handling.

`Node.js` · `Socket.IO` · `PostgreSQL`

## Engineering themes

- **Product architecture:** reduce the problem before adding structure; make the domain model carry real product meaning.
- **Failure modes:** offline behavior, reconnects, permission boundaries, degraded storage and unsafe states are designed explicitly.
- **Evidence over ceremony:** use automated tests, integration tests, real devices and release checks to challenge assumptions.
- **Boundaries that hold:** keep domain logic, persistence, external services and authority separated so complexity stays controllable.

## Current direction

Building deeper capability across **electrical engineering, product systems, automation and real-world technology**, with long-term interest in robotics, energy and physical systems.

Production source repositories are intentionally private where appropriate. The case studies above document what was built, the architecture, engineering decisions, limitations and current status without exposing credentials, private data or proprietary source.
