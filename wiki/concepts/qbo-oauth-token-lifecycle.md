---
title: QBO OAuth token lifecycle
type: concept
summary: How a click on connect.html becomes a rotating refresh token in Airtable that the Mac's apfs-qbo-sync cron uses, and where it breaks.
status: active
created: 2026-10-19
updated: 2026-10-19
last_verified: 2026-10-19
sources: [connect.html, "APFS-Database scripts/qbo_exchange_code.py", "APFS-Database .github/workflows/qbo-oauth-exchange.yml", "APFS-Database docs/quickbooks-expenses-sync.md", "APFS-Database docs/hermes-crons-apfs.md", "APFS-Database PR #239", "APFS-Database commits dab51deb, 2c947af6, 273ad38f", "~/Repos/apfs/FOLLOWUPS.md 2026-10-19"]
tags: [oauth, quickbooks, tokens]
---

# QBO OAuth token lifecycle

This repo does step 1 and 2 only. Everything after the browser hands over the `code` happens in APFS-Database.

## The chain

1. **Start.** An APFS QBO admin opens `connect.html` and clicks "Continue to Intuit". The page stores a random `state` in `sessionStorage` (per tab) and redirects to Intuit's authorize URL with the public Client ID, the redirect URI and the accounting scope.
2. **Redirect back.** Intuit returns to `connect.html?code=…&realmId=…&state=…`. The page checks `state`, shows the `code` and `realmId` with copy buttons, then strips the query string from the address bar. It cannot exchange the code itself: that needs the client secret ([[0001-static-pages-oauth-redirect]]).
3. **Exchange (minutes, not hours).** Someone runs `scripts/qbo_exchange_code.py --code … --redirect-uri <connect.html URL> [--realm-id …] --write-airtable` in APFS-Database. Two ways:
   - GitHub Actions workflow **QBO OAuth code exchange** (`qbo-oauth-exchange.yml`, manual). This is what `connect.html` tells the user to do.
   - Locally on the Mac with APFS-Database's `.env`. Since 2026-10-14 GitHub-hosted runners don't start jobs (APFS-Database PR #239), so this is the path that works today.
4. **Store.** With `--write-airtable`, the refresh token is saved to Airtable **Commission Config → QBO OAuth Refresh Token** (APFS-Database commit `dab51deb`). Optionally also the `QBO_REFRESH_TOKEN` GitHub secret when `REPO_SECRETS_WRITE_PAT` exists (`2c947af6`).
5. **Use and rotate.** The daily Hermes cron `apfs-qbo-sync` (07:00, `run_scheduled_job.sh qbo-sync`) runs expenses, journal entries, P&L detail preview and revenue in one process. Intuit returns a **new** refresh token on refresh; `scripts/qbo_oauth_refresh.py` writes it back to Commission Config. They run in one job on purpose: two runs refreshing the same pre-rotation token at once produce `invalid_grant` (`docs/hermes-crons-apfs.md`, `docs/quickbooks-expenses-sync.md`).

## When you need this repo again

Only when the refresh token is revoked or truly expired (QBO admin disconnected the app, Intuit expiry, or a rotated token was lost). The symptom is `invalid_grant` / "Incorrect or invalid refresh token" in the QBO sync log. Before reconnecting, run `python3 -u scripts/qbo_oauth_refresh.py --check` in APFS-Database: often the token is fine and the wrong client keys or a lost write is the cause. Steps: [[reconnect-quickbooks]].

## Traps

- The `connect.html` success screen is partly out of date: it sends you to GitHub Actions (not running since 2026-10-14) and says to re-run "QBO Sales → Airtable Sync", which was retired on 2026-04-06 (APFS-Database PR #101). Its last step (copy `QBO_REFRESH_TOKEN` from the job log) is now only a fallback: since APFS-Database commit `273ad38f` the Actions job prints the token only when neither Airtable nor the repo secret could be written.
- A **local** exchange still prints the refresh token to the terminal (for `.env`). Don't paste that output into a chat log, issue, or note.
- `state` check is lenient: it only fails when both values exist and differ. Start the flow and finish it in the same tab.
- Never paste a `code`, refresh token or realm ID into this repo, the wiki, or a commit. They're public ([[github-pages-site]]).

## Related

- [[intuit-developer-app]] · [[reconnect-quickbooks]] · [[gotchas]]
