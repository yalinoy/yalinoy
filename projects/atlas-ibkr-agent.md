# Atlas — Safety-First Research Agent

> **Role:** System design, guardrails, workflow architecture and implementation  
> **Status:** Research/paper workflow · no live trading · no order placement  
> **Source:** Private

Atlas explores a narrow but important systems problem: **how far should an autonomous workflow go before human authority must take over?**

It is a local research agent for disciplined U.S. equity research, built around explicit state boundaries, auditability and hard refusal paths instead of vague warnings.

![Atlas safety flow](../assets/atlas-safety-flow.svg)

## At a glance

| | |
|---|---|
| **Core problem** | Automate repeatable research without creating an accidental execution path |
| **Key constraint** | Research assistance must never silently become trade authority |
| **Architecture** | Local Python runner + structured config + append-only logs + human review |
| **Safety invariant** | There is no live-order execution path in the current system |
| **Stack** | Python · YAML · CLI workflows · scheduled execution · structured logs |

## What the system does

Atlas can turn approved or manually entered research inputs into structured artifacts: market snapshots, watchlist assessments, candidate research memos, human-review records and deeper research packs.

It can also run repeatably on a local schedule and keep operational history separate from explicitly committed research decisions.

What it **cannot** do is just as important: the current system does not connect to a live trading environment, does not place orders and does not treat a generated candidate or reviewed memo as trade approval.

## Key engineering decisions

### 1. Refuse unsafe states in code
A warning in documentation is not a control. Atlas validates its operating mode and rejects configurations that violate the research-only boundary.

### 2. Make states semantically explicit
**Candidate**, **research memo**, **human review** and **trade approval** are different concepts. The system keeps them separate so an agent cannot blur the meaning of progress through the workflow.

### 3. Preserve auditability
Operational runs and investment-research decisions are recorded separately. This prevents test runs and automation noise from masquerading as investment decisions.

### 4. Keep privilege escalation impossible by default
The architecture prepares validation logic for future read-only brokerage integration while keeping execution privileges outside the current system entirely.

## Why this project matters

The interesting part of Atlas is not the financial domain. It is the design pattern:

- automation with a hard capability ceiling
- explicit human authority
- refusal as a first-class system behavior
- append-only state and traceability
- safety boundaries that survive future feature growth

Those patterns transfer directly to higher-stakes agentic systems in operations, infrastructure and physical technology.

## Current status

Atlas remains intentionally constrained to research/paper workflows. Expanding its capabilities would require proving each new privilege boundary independently rather than enabling a broad “autonomous mode.”

---

The source repository remains private. This public case study focuses on system design and guardrails rather than investment strategy, credentials or private configuration.
