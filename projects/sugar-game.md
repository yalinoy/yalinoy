# Sugar Game

An online, four-player Israeli Whist game with rooms, accounts, friends and leaderboards.

**My work:** Product design, full-stack implementation and architecture refactor.

[Back to profile](../README.md)

## Playing

Players can create or join a room, play live rounds and recover their session after a refresh or dropped connection. Accounts keep points, friends and leaderboard history across games. QR codes make it easier to join from another device.

The app supports local-network play and hosted deployment.

## At a glance

| | |
| --- | --- |
| **Server** | Node.js · Express |
| **Realtime** | Socket.IO |
| **Persistence** | PostgreSQL with local-file development fallback |
| **Game core** | Pure deterministic state machine |
| **Tests** | 231 total · 174 pure-engine · 9 real-server integration |
| **Quality gate** | ESLint + GitHub Actions CI on Node 20 |

## Architecture

The key refactor was to separate deterministic game rules from realtime orchestration and transport.

```text
Browser clients
      ↓ Socket.IO
Transport — server.js
HTTP · auth · friends · leaderboards · admin
      ↓ actions
Orchestrator — src/server/
rooms · seats · timers · bots · emissions · RNG
      ↓ state + action
Pure engine — src/game/
applyAction(state, action) → { state, events, error? }
```

The engine has no Express, Socket.IO, database, filesystem, timers, clock or randomness. Shuffling and timing stay in the orchestrator. The browser renders server-authoritative state instead of reimplementing game rules.

## Why the split matters

Before the refactor, game rules, room lifecycle and socket handling lived together in a large server file. The system worked, but changing one concern could destabilize another.

After the refactor:

- the entire rules engine can run without a server or network
- state transitions are deterministic and non-mutating
- Socket.IO is transport rather than the rules authority
- room lifecycle, bots and timers belong to the orchestrator
- duplicate client-side bid/trick rules were removed
- the wire protocol remained backward-compatible
- `server.js` dropped from 2,631 to about 1,400 lines

## Reliability and reconnects

Reconnect handling links a returning connection to the player's existing identity, seat and room.

The refactor surfaced a real reliability bug in solo games: refreshing could arm a stale abandonment timer that destroyed the room after the player had already reclaimed the seat. The fix gave abandonment timers explicit ownership and added both orchestrator and real Socket.IO regression coverage.

A reconnect now preserves the same seat, hand, round state and score, and a stale timer cannot later remove the room.

## Verification

The repository currently has **231 automated tests**:

- **174 pure-engine tests** for bidding, legal play, trick resolution, scoring, lifecycle and regression behavior
- orchestrator tests using fake Socket.IO, mocked timers and deterministic RNG
- **9 integration tests** that boot the real server on an ephemeral port and exercise websocket clients

The local quality gate is `npm run verify`: ESLint followed by the complete test suite. GitHub Actions runs the same checks from a clean `npm ci` checkout on Node 20 for pushes to `main` and pull requests.

The extraction also preserved known behavioral quirks intentionally rather than silently changing gameplay during a structural refactor. Those are documented and characterized by tests.

## Persistence and deployment

Live room/game state remains in memory for fast coordination. Accounts, points, friendships and leaderboard data are persisted separately in PostgreSQL.

The same server can run on a LAN or behind a hosted proxy, and a local-file persistence fallback keeps development usable when PostgreSQL is unavailable.

## Remaining work

The core game architecture and automated quality gate are in place. The remaining debt is narrower:

- continue decomposing non-game HTTP/account/social responsibilities from `server.js`
- eventually split the large browser client and orchestrator further
- in-progress games are intentionally in-memory only, so a server restart ends the active game

*Updated October 2026. Source code is private.*
