---
title: Two copies of the pages (APFS-Database mirror)
type: concept
summary: The HTML pages also live in APFS-Database docs/intuit-legal-site/; a dormant deploy workflow there would overwrite this repo with clean:true.
status: active
created: 2026-10-19
updated: 2026-10-19
last_verified: 2026-10-19
sources: ["APFS-Database docs/intuit-legal-site/", "APFS-Database .github/workflows/deploy-intuit-legal-site.yml", "APFS-Database commits 7941a468, 0076b73d, 819a7596, f7f268a7, 7d4c582b", "commits 796d0fc, d27e67e, 49b53ea", "diff 2026-10-19"]
tags: [deploy, source-of-truth]
---

# Two copies of the pages (APFS-Database mirror)

The five HTML pages exist twice: here, and in APFS-Database `docs/intuit-legal-site/`. They were written there first (APFS-Database PR #34, 2026-03-16) and copied here. On 2026-10-19 all five files were byte-identical (`diff`).

## History

| Date | APFS-Database | This repo |
|---|---|---|
| 2026-03-16 | Pages added in PR #34 (`7941a468`) | Same five pages committed one by one (`f7bb96b` … `f1e4b94`) |
| 2026-04-06 | Deploy workflow added (`0076b73d`), then made manual-only until a deploy token exists (`819a7596`) | — |
| 2026-04-06 | Production Client ID set (`f7f268a7`); JS parse error fixed (`7d4c582b`) | Same changes copied by hand: `796d0fc` "Deploy connect.html OAuth flow from APFS-Database", `d27e67e`, `49b53ea` |

So far every change was made in APFS-Database and copied here by a manual push the same day.

## The deploy workflow (dormant, dangerous)

`deploy-intuit-legal-site.yml` in APFS-Database would push `docs/intuit-legal-site/` to this repo's `main` with `JamesIves/github-pages-deploy-action` and **`clean: true`**. It needs the secret `APFS_QBO_LEGAL_DEPLOY_TOKEN`, which the workflow comments say doesn't exist yet, and it is `workflow_dispatch` only.

`clean: true` deletes files in the target that aren't in the source folder. That folder has only the five HTML pages, so a run would remove `AGENTS.md`, `.claude/`, `.verify.sh`, `scripts/`, `docs/` and this `wiki/` from `main` (git history keeps them). Don't run it or enable its `push:` trigger without adding a `clean-exclude` list or dropping `clean: true` first. *(Inference from the workflow file; it has never run against the current repo layout.)*

## Working rule

Change a page in **both** places in the same session, one PR/commit per repo, and say which ships first (`~/Repos/apfs/AGENTS.md` cross-repo rule). This repo is what's live; APFS-Database's copy is what a future deploy would publish.

## Related

- [[github-pages-site]] · [[overview]] · [[gotchas]]
