---
title: GitHub Pages site
type: entity
summary: The public noidsoup.github.io/apfs-qbo-legal site; a legacy Jekyll build of main that also publishes AGENTS.md and this wiki.
status: active
created: 2026-10-19
updated: 2026-10-19
last_verified: 2026-10-19
sources: ["GitHub Pages API (repos/noidsoup/apfs-qbo-legal/pages)", "curl checks 2026-10-19", connect.html, "APFS-Database .github/workflows/qbo-oauth-exchange.yml"]
tags: [hosting, github-pages]
---

# GitHub Pages site

The only host for this repo. GitHub Pages builds `main` from the repo root with the "legacy" (Jekyll) build and serves it at `https://noidsoup.github.io/apfs-qbo-legal/` with HTTPS enforced. No custom domain (`cname: null`). Source: `gh api repos/noidsoup/apfs-qbo-legal/pages`, 2026-10-19.

## What this means in practice

- **Every push to `main` is a deploy.** There is no separate release step and no staging. A broken `connect.html` on `main` breaks the next QBO reconnect.
- **Everything tracked is public.** The repo is public, and on 2026-10-19 `AGENTS`, `wiki/index` and `wiki/concepts/apfs-ecosystem` all returned HTTP 200 from the Pages site. Jekyll renders the Markdown files to HTML. So `wiki/`, `AGENTS.md`, `.env.example` and the rules are on the open web. Write accordingly (see [[SCHEMA]] project notes).
- **Jekyll reads Liquid.** A page containing Liquid tags or a double curly brace can fail the Pages build. A failed build keeps serving the last good one, so it fails quietly. Last build status was `built` (2026-10-11).
- **The URL is part of the OAuth contract.** `connect.html`'s `REDIRECT_URI`, the Intuit app's Redirect URI list, and the default `redirect_uri` input of APFS-Database's `qbo-oauth-exchange.yml` all hard-code `https://noidsoup.github.io/apfs-qbo-legal/connect.html`. Renaming the repo, moving it to another owner, or adding a custom domain changes that URL and breaks reconnects until all three are updated. Inference from those three files; nothing documents a planned move.

## Related

- [[overview]] · [[intuit-developer-app]] · [[legal-site-source-of-truth]] · [[gotchas]]
