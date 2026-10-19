---
title: Reconnect QuickBooks (invalid_grant)
type: guide
summary: Diagnose a QBO invalid_grant, then reconnect through connect.html and exchange the code locally while GitHub Actions is down.
status: active
created: 2026-10-19
updated: 2026-10-19
last_verified: 2026-10-19
sources: [connect.html, "APFS-Database scripts/qbo_exchange_code.py", "APFS-Database scripts/qbo_oauth_refresh.py", "APFS-Database docs/quickbooks-expenses-sync.md", "APFS-Database docs/hermes-crons-apfs.md", "APFS-Database PR #239"]
tags: [guide, oauth, quickbooks]
---

# Reconnect QuickBooks (invalid_grant)

Short version of APFS-Database `docs/quickbooks-expenses-sync.md` (the full, maintained doc), adjusted for the 2026-10-19 move of the QBO jobs to the Mac. Reconnecting changes production auth: it's Nicholas's call, and an APFS QBO admin has to sign in.

## 1. Make sure you really need to reconnect

In `~/Repos/APFS-Database` (APFS-Database's `.env` loaded):

```bash
python3 -u scripts/qbo_oauth_refresh.py --check
```

It prints which Commission Config row holds the token and whether **QBO OAuth Refresh Token** is set. `invalid_grant` has three common causes, and only the last needs this repo:

- wrong client keys in `.env` (a different Intuit app than the one that issued the token; happened 2026-10-19, see [[intuit-developer-app]]),
- a rotated token that never got saved (two runs racing, or a failed Airtable write),
- the token is revoked or expired (app disconnected in QBO, or unused too long).

## 2. Get a fresh code

1. Open `https://noidsoup.github.io/apfs-qbo-legal/connect.html` in one tab and click **Continue to Intuit to sign in**.
2. The QBO admin signs in and approves "APFS Database App".
3. Back on `connect.html`, copy the **Authorization code** and **Realm ID**. The code expires in minutes.

## 3. Exchange it (local, because Actions isn't running)

`connect.html` says to use the GitHub Actions workflow; GitHub-hosted runners haven't started jobs since 2026-10-14 (APFS-Database PR #239). Run the same script locally:

```bash
python3 -u scripts/qbo_exchange_code.py --code "<code>" \
  --redirect-uri "https://noidsoup.github.io/apfs-qbo-legal/connect.html" \
  --realm-id "<realm id>" --write-airtable
```

`--write-airtable` stores the token in Commission Config, which the Mac cron reads. The script prints the token for `.env`; keep that output out of chats and notes.

## 4. Confirm

Run `--check` again, then the next `apfs-qbo-sync` cron (07:00 daily) or `scripts/mac_local/run_scheduled_job.sh qbo-sync` by hand if Nicholas OKs a run. Log what you ran, with the UTC time, in `~/Repos/apfs/FOLLOWUPS.md`.

## Related

- [[qbo-oauth-token-lifecycle]] · [[0001-static-pages-oauth-redirect]] · [[gotchas]]
