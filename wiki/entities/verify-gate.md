---
title: Verify gate (.verify.sh)
type: entity
summary: What bash .verify.sh checks (inline scripts compile, .html links resolve, Python parses) and what it can't (Client ID, redirect URI).
status: active
created: 2026-10-19
updated: 2026-10-19
last_verified: 2026-10-19
sources: [.verify.sh, AGENTS.md, "commit 695279b", "commit fc07d6c", "commit 8d0f430", "commit 49b53ea", "~/Repos/apfs/FOLLOWUPS.md"]
tags: [verify, ci]
---

# Verify gate (.verify.sh)

`bash .verify.sh` is the only check in this repo. There is no CI here; nothing runs on push except the Pages build. It is read-only and local. Ran PASS on 2026-10-19.

## What it checks

1. `AGENTS.md` exists.
2. Every tracked `.py` file parses (`ast.parse`). These are only the three LanceDB helper scripts.
3. Every tracked `.html` file: each inline `<script>` compiles with Node's `vm.Script` (parsed, never run), and every relative `href="….html"` points at a tracked page.

Step 3 was hand-added in `fc07d6c` (2026-10-10). It is there because of a real bug: on 2026-04-06 `connect.html` shipped with a JavaScript syntax error and the `#app` element placed after the script, so the page did nothing until `49b53ea` fixed it.

## What it can't check

The Client ID and Redirect URI against Intuit, the OAuth round trip, and whether the legal text is current. Review HTML changes against the invariants in AGENTS.md, and test a `connect.html` change on the live Pages URL (there is no staging; see [[github-pages-site]]).

## History

- `695279b` (2026-07-29): generated stub that exits 1 on purpose ("no runnable checks") so a missing gate never looks green.
- `fc07d6c` (2026-10-10): real gate, PASS.
- `8d0f430` (2026-10-10): shared generator change for WPCode-style PHP snippets; no `.php` files here, so no effect.
- `~/Repos/apfs/FOLLOWUPS.md` still says this repo's verify "exits 1 by design". That line predates `fc07d6c` and is stale.

## Related

- [[overview]] · [[gotchas]]
