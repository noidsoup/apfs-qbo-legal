---
title: Gotchas
type: overview
summary: Known traps in apfs-qbo-legal. Check here after a repeated failure.
status: active
created: 2026-10-19
updated: 2026-10-19
last_verified: 2026-10-19
sources: [AGENTS.md, connect.html, .gitignore, "commits 49b53ea, 2269df9, 6e3d3dc, c209780", "APFS-Database PRs #101, #239, #240", "APFS-Database .github/workflows/deploy-intuit-legal-site.yml", "~/Repos/apfs/AGENTS.md"]
tags: [gotchas]
---

# Gotchas

One line each: the trap, then the fix or the page with details.

## Public and live

- **Everything here is public, including this wiki.** Pages publishes `main` from the repo root and renders the Markdown. No realm ID, auth code, token, secret, record ID or client data in any file or commit. → [[github-pages-site]]
- **A push to `main` is a production deploy.** No staging; a broken `connect.html` breaks the next reconnect. Run `bash .verify.sh`, then load the live page after the push. → [[verify-gate]]
- **Liquid breaks the Pages build quietly.** Jekyll processes the files; double curly braces or curly-brace-percent pairs in any page can fail the build, and the old site keeps serving. → [[github-pages-site]]
- **The site URL is the OAuth redirect URI.** Renaming or moving the repo, or adding a custom domain, breaks reconnects until `connect.html`, the Intuit app settings and APFS-Database's `qbo-oauth-exchange.yml` default all change. → [[github-pages-site]]

## OAuth and QuickBooks

- **Client secret never goes in HTML.** Client ID and redirect URI only (AGENTS.md invariant 1). The exchange runs in APFS-Database. → [[0001-static-pages-oauth-redirect]]
- **`connect.html`'s "next steps" are out of date.** They point at GitHub Actions (no hosted jobs since 2026-10-14) and a "QBO Sales" sync retired 2026-04-06 (APFS-Database PR #101). Exchange locally. → [[reconnect-quickbooks]]
- **`invalid_grant` usually isn't a reconnect.** Check client keys and the stored token first (`qbo_oauth_refresh.py --check`); on 2026-10-19 it was the wrong Intuit app's keys in the Mac `.env`. → [[intuit-developer-app]]
- **Auth codes expire in minutes, and `state` lives in one tab.** Start and finish the connect in the same tab and exchange right away. → [[qbo-oauth-token-lifecycle]]
- **Sync bugs don't belong here.** QBO → Airtable code, token storage and crons are in APFS-Database (AGENTS.md invariant 3). → [[apfs-ecosystem]]

## Two copies, one dangerous workflow

- **The pages also live in APFS-Database `docs/intuit-legal-site/`.** Change both, or they drift. → [[legal-site-source-of-truth]]
- **Never run APFS-Database's `deploy-intuit-legal-site.yml` as is.** It uses `clean: true` and would delete `AGENTS.md`, `.claude/`, `scripts/` and `wiki/` from this repo's `main`. → [[legal-site-source-of-truth]]

## Repo hygiene

- **Shared checkout stays on `main`.** Work in a worktree (`~/Repos/apfs/bin/wt new qbo …`); guard hooks refuse commits in the shared checkout off `main`. Nightly gardener/close-out jobs commit `wiki/` on `main` there, so don't leave half-edited wiki files in it (`~/Repos/apfs/AGENTS.md`).
- **Close-out auto-commits once committed bytecode** (`2269df9`, a `.pyc`); `6e3d3dc` untracked it and `.gitignore` now has `__pycache__/`. Check `git status` before an auto-commit.
- **Agent setup loads from `.claude/`, not `.cursor/`.** Cursor was retired 2026-10-02; `.claude/CLAUDE.md` imports `AGENTS.md` and the rules moved to `.claude/rules/` (`c209780`). The root `CLAUDE.md` is local and git-excluded. Only `.cursor/context7-libraries.md` is still used.
- **The FOLLOWUPS note that verify "exits 1 by design" is stale.** The real gate landed in `fc07d6c` and passes. → [[verify-gate]]
