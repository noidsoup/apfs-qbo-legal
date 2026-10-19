---
title: Overview
type: overview
summary: What apfs-qbo-legal is (the public Intuit app pages and OAuth redirect for APFS QuickBooks), who uses it, where it runs.
status: active
created: 2026-10-19
updated: 2026-10-19
last_verified: 2026-10-19
sources: [AGENTS.md, connect.html, index.html, "GitHub Pages API (repos/noidsoup/apfs-qbo-legal/pages)", "APFS-Database .github/workflows/qbo-oauth-exchange.yml", "APFS-Database PR #239"]
tags: [overview]
---

# apfs-qbo-legal

Five static HTML pages, served by GitHub Pages, that give A Place for Seniors' QuickBooks Online (QBO) integration two things Intuit requires: public legal/support URLs for the Intuit app, and an HTTPS redirect page for the OAuth sign-in. Nothing here syncs data; the sync lives in APFS-Database.

## What and why

- **The Intuit app** is called "APFS Database App" on the pages (`index.html`, `terms.html`). It reads QBO accounting data so APFS-Database can copy it into Airtable (see [[intuit-developer-app]]).
- **The legal pages** (`terms.html` EULA, `privacy.html`, `disconnect.html`) exist because Intuit asks for these URLs for an app listing. They were added together on 2026-03-16 as "Intuit compliance pages" (commits `76503b9`, `23a8b4e`, `a2005c3`, `f1e4b94`, `f7bb96b`).
- **The connect page** (`connect.html`) starts Intuit OAuth and receives the redirect. It shows the one-time `code` and `realmId` so an admin can hand them to the secret-holding exchange step in APFS-Database. The browser never sees the client secret. Full flow: [[qbo-oauth-token-lifecycle]]; why it's built this way: [[0001-static-pages-oauth-redirect]].
- The pages were first written in APFS-Database `docs/intuit-legal-site/` and copied here; both copies are still identical (checked 2026-10-19). See [[legal-site-source-of-truth]].

## People and roles

| Role | Who | Notes |
|---|---|---|
| Client / data owner | A Place for Seniors (APFS) | Owns the QBO company and the Airtable base. |
| Developer, repo owner | Nicholas (Cooper Bold) | Owns the GitHub repo and Pages site; production changes are his call (`~/Repos/apfs/AGENTS.md`). |
| QBO admin | an APFS QuickBooks admin | Needed to authorize (connect) or disconnect the app in QBO (`disconnect.html`). |

## Where it runs

| Environment | URL / host | Deploys from |
|---|---|---|
| Production (only one) | `https://noidsoup.github.io/apfs-qbo-legal/` | GitHub Pages "legacy" Jekyll build of `main`, repo root. Every push to `main` republishes. See [[github-pages-site]]. |
| OAuth redirect URI | `https://noidsoup.github.io/apfs-qbo-legal/connect.html` | Same site; must match the Intuit app settings exactly (AGENTS.md invariant 2). |
| Token exchange + syncs | APFS-Database on the Mac Studio (Hermes cron `apfs-qbo-sync`) | Moved off GitHub Actions on 2026-10-19 (APFS-Database PR #239). |

There is no staging site. The repo is public, so the wiki you are reading is public too.

## Key links

- Repo: <https://github.com/noidsoup/apfs-qbo-legal>
- Estate map: `~/Repos/APFS-Database/docs/APFS-ECOSYSTEM.md`; workspace rules: `~/Repos/apfs/AGENTS.md`; workspace wiki: `~/Repos/apfs/wiki`
- QBO sync and token docs: APFS-Database `docs/quickbooks-expenses-sync.md`, `docs/quickbooks-api-integration.md`, `docs/hermes-crons-apfs.md`
- Reconnect steps: [[reconnect-quickbooks]]

## Related

- [[gotchas]] · [[apfs-ecosystem]] · [[verify-gate]]
