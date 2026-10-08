# Agent instructions — apfs-qbo-legal

**This repo** is static HTML for **Intuit QuickBooks Online OAuth** connect / disconnect for A Place for Seniors. It is **not** the QBO→Airtable sync implementation.

## Workspace (several sessions at once)

This repo is one of five A Place for Seniors repos, and several Claude sessions often work on them
at the same time. The workspace rules are in `~/Repos/apfs/AGENTS.md` (a hub folder, not part of
this repo). The short version:

- **Start with** `~/Repos/apfs/bin/wt doctor` and `~/Repos/apfs/bin/wt ls`: who is working on
  what, in which worktree, on which PR, and what jobs are running. If a session already covers
  your topic, message it (ListAgents, then SendMessage) before starting.
- **Work in a worktree, not the shared checkout** (`~/Repos/apfs-qbo-legal`):
  `~/Repos/apfs/bin/wt new qbo <type>/<topic> --note "what this is for" --session "<your session name>"`.
  If the Claude app put you in `.claude/worktrees/<name>`, run `~/Repos/apfs/bin/wt claim . --note "…"`.
  A small close-out commit on `main` in the shared checkout is fine; switching branches there is
  not, and the guard hooks refuse commits when it's off `main`.
- **Static pages only.** The QBO → Airtable sync that uses this OAuth connection lives in
  APFS-Database.
- **Siblings:** APFS-Database (Airtable hub, FUB/SeniorPlace syncs, commissions, Hermes crons),
  APlaceForSeniorsFrontEnd (communities site), apfs-www (marketing WordPress), senior-scraper
  (SeniorPlace website → WordPress drafts), apfs-qbo-legal (QBO OAuth pages). How they connect:
  `~/Repos/APFS-Database/docs/APFS-ECOSYSTEM.md`. A change that crosses repos is one PR per repo,
  linked, saying which ships first.
- **Cross-repo state:** `~/Repos/apfs/FOLLOWUPS.md` (in flight, blockers, live writes with UTC
  times) and `~/Repos/apfs/AI_SESSION_MEMORY.md` (cross-repo session log). Re-check any state
  claim before acting on it.

## Ecosystem

→ `/Users/thedao/Repos/APFS-Database/docs/APFS-ECOSYSTEM.md`  
Workspace: `/Users/thedao/Repos/APFS-Database/apfs.code-workspace`

| Sibling | Role |
|---------|------|
| APFS-Database | QBO expense/revenue sync workflows, Integrately docs, Airtable |
| APlaceForSeniorsFrontEnd | Communities site (unrelated to QBO OAuth) |
| apfs-www | Marketing www companion (unrelated to QBO OAuth) |
| senior-scraper | Listing scrape (unrelated) |

## Invariants

1. **Client secret never goes in HTML.** Production Client ID + exact Redirect URI only (`connect.html` / `disconnect.html`).
2. Redirect URI must match Intuit app settings exactly.
3. For sync bugs or Airtable QBO tables, work in **APFS-Database** (`docs/` QuickBooks topics, GHA `qbo-*-sync.yml`).

## Files

| File | Purpose |
|------|---------|
| `connect.html` | Connect / reconnect QBO |
| `disconnect.html` | Disconnect flow |
| `privacy.html` / `terms.html` | Legal pages for the Intuit app listing |

## Topic → where

| Need | Open |
|------|------|
| Estate map | APFS-Database `docs/APFS-ECOSYSTEM.md` |
| QBO ↔ Airtable ops | APFS-Database `docs/INDEX.md` → QuickBooks |
| Retrieval notes for this repo | `docs/ai-retrieval.md` |
