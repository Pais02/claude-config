---
name: project-overview
description: "Couples app — what it is, stack, spec location, current stage"
metadata: 
  node_type: memory
  type: project
  originSessionId: 3895e366-585e-4793-bbf5-b75d9c3c3907
  modified: 2026-08-28T14:04:10.702Z
---

Building an iOS app for couples (working title **"Alcove"** — placeholder, not decided). Private shared space inspired by real Telegram usage between a couple: wishlist, store loyalty cards, shared plans/trips, events, subscriptions, reminders, and an end-to-end encrypted document vault.

Stack: Swift + SwiftUI (iOS-only at start), Supabase (Postgres + RLS, EU/Frankfurt region for GDPR), CryptoKit for on-device vault encryption, VNDocumentCameraViewController for document scanning.

Full spec lives at `SPEC.md` in the project root (copied from `~/Downloads/couple-app-spec.md` on 2026-08-27). Build order (from spec section 5): 1) Pairing skeleton, 2) Wishlist + Events, 3) Subscriptions + Reminders, 4) Store cards + Plans, 5) Vault (last, most sensitive).

**Current stage (as of 2026-08-28):** ALL modules are wireframed — 27 artboards across 4 canvas pages. Working `.dc.html` files live in `design/sprint1/` (folder name is a misnomer — it holds every sprint). **The backend is DONE** — migrations `0001`–`0006` cover every module in the spec, applied to the live project and verified against it, with 101 PGlite assertions plus 47 live Storage ones. **SwiftUI has not started, and is the only remaining work.** Next up is Sprint 1 from the spec: the pairing flow (create household → invite → redeem), against `create_household()` / `create_invite()` / `redeem_invite()`.

**The machine this repo sits on is Windows — no Xcode, no Swift, no Docker.** As of 2026-08-28 the user confirmed they now have a Mac and can build there, so iOS is no longer blocked; it just does not happen on this machine. Backend work runs here: `db-tests/` runs migrations in PGlite (Postgres in WASM) with `auth.users`/`auth.uid()`/`authenticated` and `storage.objects` stubbed. `npm test` in `db-tests/`.

**Repo:** `https://github.com/Pais02/alcove` — private, GitHub account `Pais02`, default branch `master`. Pushed 2026-08-28 so the work can continue on the user's Mac, where Xcode is. `.env` is gitignored and deliberately did NOT travel: on a new machine recreate it from `.env.example` with keys from the Supabase dashboard.

**Supabase project is live**: ref `drkwsdfkzawsyipfhujv`, EU (Frankfurt), Magic Link auth. Migrations `0001`–`0006` applied through the dashboard SQL Editor (paste `supabase/apply-all.sql`, which is generated and gitignored). `supabase/verify.sql` is a read-only post-apply sanity check.

**Supabase trap worth remembering:** a new project runs `grant all on all tables in schema public to anon, authenticated` and keeps doing it for new tables via ALTER DEFAULT PRIVILEGES. Any column-level `grant select (cols)` is therefore additive and restricts nothing until you `revoke all` first. This silently defeated the wishlist surprise; `0006` fixes it and `db-tests/` now reproduces those defaults so it cannot recur.

**The backend is now fully verified live (2026-08-28).** The Storage policies in `0003`/`0005` were the last gap and are closed: `db-tests/live-storage.mjs` (`npm run test:live`) mints two throwaway users via the Admin API, gives them separate households, drives `vault` and `plan-files` from both sides and signed-out, and cleans up in a `finally`. 47 assertions passing. Needs `SUPABASE_URL`/`SUPABASE_ANON_KEY`/`SUPABASE_SERVICE_ROLE_KEY` in `.env` at the repo root (gitignored; `.env.example` is the template). Note this disproved an earlier belief written into the README — a live storage check does NOT need the app, because the Admin API can create and sign in users over plain HTTP.

**Supabase trap #2 (cost an hour):** for service-role calls the `apikey` header must carry the *same* key as the `Authorization: Bearer`. GoTrue reads the role off `apikey` and then tries to parse the bearer as a user JWT, so a publishable `apikey` with a secret bearer fails as `bad_jwt: token contains an invalid number of segments`. For user calls it is `apikey: anon` + the user's JWT as bearer, as normal.

**Navigation decision made during design (not in SPEC.md):** the bottom tab bar carries only Wishlist, Events, Vault, Plans and More. Store Cards, Subscriptions and Reminders live one level down under "More" — iOS collapses anything past 5 tabs anyway, so this was chosen deliberately rather than left to the system.

**Why:** [[design-workflow]] for the reasoning behind designing before coding.
**How to apply:** Before writing SwiftUI for any screen, open the matching wireframe in `design/sprint1/` and build to match it rather than inventing layout. Screens deliberately NOT designed: subscription detail (near-duplicate of the add form) — draw it before building if it's actually needed.
