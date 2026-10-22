# Agent instructions — apfs-qbo-legal

**This repo** is static HTML for **Intuit QuickBooks Online OAuth** connect / disconnect for A Place for Seniors. It is **not** the QBO→Airtable sync implementation.

Claude Code loads this file through `.claude/CLAUDE.md`; the rules in `.claude/rules/` load with it.

**Project wiki:** read `wiki/index.md` before non-trivial work; open pages it points to only when relevant.
After a repeated failure, check `wiki/gotchas.md`. Writing to the wiki: follow `wiki/SCHEMA.md`.

## Working rules

1. **Think before coding.** State assumptions, surface tradeoffs, ask before guessing.
2. **Simplicity first.** Minimum code that solves the problem; no speculative abstractions.
3. **Surgical changes.** Touch only what the task needs. Match the existing style.
4. **Read before you write.** Read the file's exports and its caller before adding to it.
5. **Fail loud.** If you can't be sure it worked, say so. Never report skipped work as done.
6. **Check after every change.** Run the type check / lint from "Commands" after each edit.
7. **Behavior changes ship with tests**, where a test runner exists (see "What can be verified").
8. **Never ratchet a test to match current output.** Find why it changed; decide whether the code or the test is wrong.

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
| Estate map | `~/Repos/APFS-Database/docs/APFS-ECOSYSTEM.md` |
| QBO ↔ Airtable ops | `~/Repos/APFS-Database/docs/INDEX.md` → QuickBooks |
| Retrieval notes for this repo | `docs/ai-retrieval.md` |

## Commands (tested)

Runtime: none to install: static HTML plus 3 LanceDB helper scripts. System `python3` 3.9, Node 22 (used by the gate). · Last verified: 2026-10-22 on `origin/main` (306b18e).

| Task | Command (run from) | Notes |
| ---- | ------------------ | ----- |
| Verify gate | `bash .verify.sh` (root) | Exit 0, PASS: Python parse 3, inline scripts compile, relative links resolve. |
| Workspace check | `~/Repos/apfs/bin/wt doctor` | Exit 1 on 2026-10-22 for another session's unclaimed worktree, not this repo. |
| Knowledge search | `env -u PYTHONPATH -u VIRTUAL_ENV .venv-lancedb/bin/python -u scripts/search_project_knowledge_lancedb.py "<q>" --top-k 8` (shared checkout) | Exit 0 with hits. `.venv-lancedb` is gitignored. |
| Install / test / build / dev server | none | No dependencies, no tests. GitHub Pages builds the site (Jekyll) on push; there is no local build. |

`bash .verify.sh` is the gate; run it before calling a change done. It is read-only and local: the
inline `<script>` in the HTML pages must compile (parsed, not run), every relative `.html` link must
point at a tracked page, and the `scripts/` Python must parse. It cannot check the Client ID or the
Redirect URI against Intuit, so also review HTML changes against the invariants above.

## Known baseline

None: clean. `.verify.sh` passes on `origin/main` as of 2026-10-22. No CI workflow.

## What can be verified

- Locally: the gate above. It cannot check the Client ID or Redirect URI against Intuit, or whether Jekyll builds.
- The real check is the live page after a push (GitHub Pages), which is a production deploy: only with Nicholas's OK.

## Running locally and logging in

- No login here: the pages are public. The OAuth flow itself (sign in to Intuit, get a code) is done by Nicholas;
  the code exchange runs in APFS-Database (`wiki/guides/reconnect-quickbooks.md`). Agents never run it.
- A local look at a page is a plain static server (`python3 -m http.server`), but it skips Jekyll; not tested in
  the 2026-10-22 rollout.

## Dev vs production gotchas

- Jekyll processes these files on GitHub Pages only: double curly braces or curly-brace-percent pairs can fail the
  build quietly while the old site keeps serving. A local file view won't show it (`wiki/gotchas.md`).

## Git and push safety

- Owner: `noidsoup/apfs-qbo-legal` (Nicholas's GitHub, public Pages site; client work for A Place for Seniors).
  Default branch `main`.
- **A push to `main` deploys production.** Push to origin only with Nicholas's OK.
- Work in a worktree (`~/Repos/apfs/bin/wt new qbo …`); guard hooks refuse commits in the shared checkout off `main`,
  and nightly jobs commit `wiki/` there.
- Root `CLAUDE.md` is local (written by `wt setup`/Origin) and excluded in `.git/info/exclude`. The repo config sets
  `core.hooksPath=~/.origin/git-hooks` (adds `Origin-*` trailers, then chains to the guard hooks in `.git/hooks`).
