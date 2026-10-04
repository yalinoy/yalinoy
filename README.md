# Yali Noy

I build apps and tools, from the initial idea through development and testing. I'm an Electrical Engineering student at Tel Aviv University, with a longer-term interest in robotics, energy and marine technology.

Most of my recent work is on **Aretu**, a personal planning app, and **HomeRun**, a rental property management app.

## Aretu

A mobile app that brings goals, habits, tasks and weekly planning together. You can plan the week, see what needs doing today, and look back at what you actually completed.

The app uses SQLite on the device and syncs through Supabase. After signing in, everyday use works offline. It supports Hebrew and English.

<p align="center">
  <img src="https://raw.githubusercontent.com/yalinoy/yalinoy/main/assets/aretu-portfolio-2x2.png" alt="Aretu: Today, Planning, Goals and Progress screens" width="100%" />
</p>

**Status:** Preparing v0.10 for an iPhone pilot. Full device testing and TestFlight distribution are still ahead.

[Open web app](https://aretu-dev.vercel.app) · [About the project](projects/aretu.md)

React Native · Expo · TypeScript · SQLite · Supabase

## HomeRun

A web app for managing rental properties: ownership, leases, rent records, maintenance, documents and inspections.

A lot of the work is in handling changes over time. When a tenant leaves, rent changes or a property is sold, the old records need to stay accurate and each person should only see what they're allowed to access.

Recent additions include contract import, lease-renewal reminders, document expiry, maintenance photos, share links and an option to hide financial amounts on screen.

**Status:** The beta features are deployed. Final checks with signed-in users are still required before the closed beta starts.

[Open web app](https://homerun-v2-lake.vercel.app) · [About the project](projects/homerun-v2.md)

React · TypeScript · PostgreSQL · Supabase

## Atlas

A Python tool for researching U.S. stocks. It prepares research reports, checks for missing data and keeps a record of the analysis and human review. A Streamlit dashboard lets me inspect the results.

**Status:** Research and paper workflows only. It does not place trades. The next development task is to break up the larger modules and simplify testing and maintenance.

[About the project](projects/atlas-ibkr-agent.md)

Python · Streamlit · YAML

## Sugar Game

An online, four-player Israeli Whist game with rooms, accounts, friends and leaderboards. It handles reconnects so players can recover their session after refreshing or losing their connection.

**Status:** Working multiplayer app. The server and client need refactoring, along with more automated tests for the game rules.

[About the project](projects/sugar-game.md)

Node.js · Express · Socket.IO · PostgreSQL

## How I work

I work out the data model and main user flows first, then build and test in small steps. I pay particular attention to what happens when connections fail, users switch accounts or permissions change.

I use Claude Code and Codex during development, and review changes against the product requirements and test results. The project pages cover the implementation choices and remaining work.

The source repositories are private.

*Updated October 2026.*
