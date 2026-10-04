# HomeRun V2

A web app for managing residential rental properties, from purchase through day-to-day management and eventual sale.

**My work:** Product architecture, data modeling and implementation.

[Open web app](https://homerun-v2-lake.vercel.app) · [Back to profile](../README.md)

## What it does

HomeRun keeps property records, leases, rent, maintenance and documents in one place. Owners, managers and tenants get different views of the same property.

The main challenge is handling changes without losing the history. A rent increase should leave last month's rent intact. A tenant leaving should end their current access while preserving their lease record. Closing a maintenance ticket with a cost should create one expense, even if the request is retried.

The current app includes:

- Workspaces, invitations, Google sign-in and access to individual properties.
- Purchase records, ownership changes, sales and a Property Passport.
- Leases, roommates, rent terms, guarantees, renewals and tenant changes.
- Rent charges, settlement records, expenses and corrections.
- Maintenance tickets, quotes and follow-up work.
- Private documents, move-in and move-out inspections, and a property timeline.
- An Action Center for overdue rent, expiring guarantees, maintenance approvals and documents needing attention.

## Recent additions

**Contract import** reads purchase and rental contracts in the browser and proposes fields for review. The user can correct them before creating the property or lease. The source contract is saved through the existing private document flow.

**Lease and document reminders** now lead to renewal, end-of-lease or document-replacement actions. Replacing a document keeps the earlier version in the property's history.

**Maintenance reporting** supports structured tenant reports and photos. If a photo upload fails, retrying it doesn't create another ticket.

**Sharing** uses ordinary app links through WhatsApp or the device's share menu. Recipients still have to sign in and have permission to open the page.

**Finance Privacy Mode** hides amounts on the owner's screens without changing the underlying records. Onboarding also includes guidance based on the user's intended use and address prefill.

## Technical choices

| Part | Technology |
| --- | --- |
| Frontend | React, TypeScript, Vite and TanStack Query |
| UI | Tailwind CSS and shadcn/ui |
| Backend | Supabase Auth and PostgreSQL |
| Access control | PostgreSQL Row Level Security |
| Hosting | Vercel |
| Languages | Hebrew and English |

Ownership and app access are modeled separately. Someone can own part of a property without having a login; a manager can have access without owning it. Permissions come from workspace membership, property access and tenancy.

Ownership and rent terms have effective dates. Financial corrections add to the record rather than rewriting earlier entries. Operations such as ownership changes and maintenance closure run in database transactions, so the related records and audit entries are saved together.

Document downloads go through a server-side permission check before a short-lived link is issued. Draft inspection documents stay private to the owner side until the inspection is completed.

Occupancy, overdue rent and reminders are calculated from the underlying records. Keeping these calculations in one place reduces the risk of screens disagreeing.

## Testing

Tests cover permitted and denied access, financial operations, document privacy and the main owner, manager and tenant journeys. CI also rebuilds the database from its migrations and checks that the generated TypeScript types match.

The final beta feature batch recorded:

| Check | Result |
| --- | ---: |
| Application tests | 1,074 passed |
| Database and policy assertions | 2,803 passed |
| Browser tests | 130 passed, 2 intentionally skipped |

Browser coverage includes desktop and mobile in Hebrew and English. The final three implementation PRs passed all three CI jobs before merging. The 3 October deployment check confirmed that all 48 database migrations were applied.

## Where it stands

The beta features are merged and deployed. **The official closed beta has not started.** A final signed-in check on the deployed app is still needed for Google sign-in, links, document uploads and downloads, inspection privacy and historical access. The hosting-plan decision also needs to be settled before external invitations.

The planned cohort is 3–8 landlords. The target is a full month of use without incorrect, lost or improperly exposed data before moving on to launch.

HomeRun records financial activity. Payments, wallets, payouts and full accounting are outside the current scope.

*Updated October 2026. Source code is private.*
