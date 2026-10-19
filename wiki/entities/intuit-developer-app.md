---
title: Intuit developer app ("APFS Database App")
type: entity
summary: The Intuit QuickBooks app these pages belong to; public Client ID in connect.html, secret only in APFS-Database, accounting scope, legal URLs here.
status: active
created: 2026-10-19
updated: 2026-10-19
last_verified: 2026-10-19
sources: [connect.html, index.html, privacy.html, terms.html, disconnect.html, AGENTS.md, "commit d27e67e", "APFS-Database scripts/qbo_exchange_code.py", "APFS-Database PR #239", "APFS-Database PR #240"]
tags: [intuit, quickbooks, oauth]
---

# Intuit developer app ("APFS Database App")

The Intuit developer app that lets APFS-Database read the APFS QuickBooks Online company. This repo holds its public face: the legal pages Intuit links to, and the OAuth redirect page.

## Facts an agent needs

- **Name shown to users:** "APFS Database App" (`index.html`, `terms.html`, and the QBO "Apps" list in `disconnect.html`).
- **Keys:** the **Production** Client ID is hard-coded in `connect.html` (`INTUIT_CLIENT_ID`, set in commit `d27e67e`, 2026-04-06). A Client ID is public in OAuth, so that is fine. The **client secret** is never in this repo (AGENTS.md invariant 1). It lives only where the token exchange runs: APFS-Database's `.env` (`QBO_CLIENT_ID` / `QBO_CLIENT_SECRET`, or the older `QB_*` names) and its GitHub repo secrets.
- **Same app on both sides.** The refresh token only works with the client ID/secret of the app that issued it. On 2026-10-19 the Mac's QBO jobs failed with `invalid_grant` because its local `QB_CLIENT_ID`/`QB_CLIENT_SECRET` were a different Intuit app; the Production keys were then copied in (APFS-Database PRs #239, #240). If you rotate or replace keys, change the exchange side and keep `connect.html`'s Client ID in step.
- **Scope:** `com.intuit.quickbooks.accounting` only (`connect.html` `SCOPE`).
- **Redirect URI:** `https://noidsoup.github.io/apfs-qbo-legal/connect.html`; must match the Intuit app's settings exactly (AGENTS.md invariant 2). See [[github-pages-site]].
- **Legal and support URLs:** `terms.html` (EULA), `privacy.html`, `disconnect.html`, plus `index.html` as the app home. The support contact on all of them is the APFS support mailbox.

## Policy text vs what the integration does

`privacy.html` (last updated 2026-03-16) and `index.html` describe syncing QBO **expense** data. Since then APFS-Database also syncs revenue transactions, journal entries and P&L detail (`scripts/mac_local/run_scheduled_job.sh` `qbo-sync`). The privacy page's "Data We Access" list is general enough to cover these, but the wording is expense-only. Changing legal text is APFS's and Nicholas's call, not an agent's. *(Observation, 2026-10-19.)*

## Related

- [[qbo-oauth-token-lifecycle]] · [[0001-static-pages-oauth-redirect]] · [[reconnect-quickbooks]] · [[overview]]
