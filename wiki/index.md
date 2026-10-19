---
title: Index
type: overview
summary: Start here. Must-know facts, then one line per page.
status: active
created: 2026-07-29
updated: 2026-10-19
last_verified: 2026-10-19
sources: [AGENTS.md, connect.html, "GitHub Pages API (repos/noidsoup/apfs-qbo-legal/pages)"]
tags: [meta]
---

# apfs-qbo-legal wiki

## Must know

- This repo and this wiki are **public**, and every push to `main` redeploys the GitHub Pages site. No IDs, tokens, secrets or client data anywhere.
- `connect.html` URL = the Intuit OAuth redirect URI. The client secret never goes in HTML; the code exchange runs in APFS-Database.
- QBO sync, token storage and crons live in **APFS-Database** (Mac Hermes cron `apfs-qbo-sync` since 2026-10-19), not here.
- The pages are mirrored in APFS-Database `docs/intuit-legal-site/`; its deploy workflow uses `clean: true` and would wipe this repo's extras.
- `connect.html`'s post-auth instructions are stale (GitHub Actions down, Sales sync retired): use [[reconnect-quickbooks]].

## Overview

- [[overview]] — what this is, who it's for, where it runs
- [[gotchas]] — known traps; check after a repeated failure

## Entities

- [[github-pages-site]] — the public Pages site: legacy Jekyll build of `main`, publishes the wiki too
- [[intuit-developer-app]] — "APFS Database App": public Client ID, secret elsewhere, scope, legal URLs
- [[verify-gate]] — what `.verify.sh` checks and can't check
- [[lancedb-indexer]] — `index_project_knowledge_lancedb.py`, offline semantic index
- [[lancedb-search]] — `search_project_knowledge_lancedb.py`

## Concepts

- [[qbo-oauth-token-lifecycle]] — connect click → code → exchange → rotating token in Airtable → Mac cron
- [[legal-site-source-of-truth]] — the APFS-Database mirror and its dangerous deploy workflow
- [[apfs-ecosystem]] — where this repo sits among the five APFS repos
- [[ai-retrieval-options]] — semantic doc-search options; Option A (LanceDB sidecar) chosen

## Decisions

- [[0001-static-pages-oauth-redirect]] — static Pages redirect; secret-side exchange in APFS-Database

## Guides

- [[reconnect-quickbooks]] — diagnose `invalid_grant`, reconnect, exchange the code locally
- [[semantic-search-setup]] — build and query the LanceDB project-knowledge index

## Sources

- (none yet)

## Meta

- [[SCHEMA]] — how to write here · [[log]] — change history
