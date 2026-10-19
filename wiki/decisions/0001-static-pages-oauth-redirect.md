---
title: "0001: Static GitHub Pages redirect, secret-side code exchange elsewhere"
type: decision
summary: QBO OAuth uses a static connect.html on GitHub Pages that only shows the code; the secret-using exchange runs in APFS-Database.
status: active
created: 2026-10-19
updated: 2026-10-19
last_verified: 2026-10-19
sources: [connect.html, AGENTS.md, "commit 796d0fc", "APFS-Database commit d90a9fa4", "APFS-Database docs/quickbooks-api-integration.md", "APFS-Database scripts/qbo_exchange_code.py"]
tags: [decision, oauth, quickbooks]
---

# 0001: Static GitHub Pages redirect, secret-side code exchange elsewhere

**Status:** active (since 2026-04-06). Reconstructed from the code and commit history; there is no written ADR from the time.

## Context

APFS-Database moved QBO data into Airtable with its own Python scripts instead of a no-code connector (APFS-Database `docs/quickbooks-api-integration.md`: "Alternative to connecting QuickBooks via Integrately"). That needs a one-time OAuth sign-in with a production HTTPS redirect URI, and Intuit wants public EULA/privacy/disconnect URLs for the app. APFS had no server to host either.

## Decision

- Host the redirect and legal pages as static HTML on GitHub Pages (`noidsoup/apfs-qbo-legal`).
- `connect.html` only starts OAuth and displays the returned `code` and `realmId`. It holds the public Client ID and never the client secret (AGENTS.md invariant 1; comment in `connect.html`).
- Exchange the code where the secret already lives: `scripts/qbo_exchange_code.py` in APFS-Database, wrapped in the manual **QBO OAuth code exchange** workflow (both added 2026-04-06, APFS-Database commit `d90a9fa4`, the same day `connect.html` got its OAuth flow here in `796d0fc`).

## Alternatives (as documented)

- **Integrately** (no-code connector): the reason the API route exists is that its OAuth connection could fail and offered little control (`docs/quickbooks-api-integration.md`, "When to use API instead of Integrately").
- **Local callback server** (`localhost` redirect, the "Suggested flow" in the same doc): fine for one-time auth, but production Intuit keys need an HTTPS redirect and the legal pages need a public host anyway.
- **Serverless function holding the secret** (e.g. Netlify): not chosen *(unverified; no record of it being considered)*. It would put the secret on another host.

## Consequences

- The person reconnecting must copy the code into a second tool within minutes. That manual hop is why [[reconnect-quickbooks]] exists.
- The site URL is baked into three places ([[github-pages-site]]).
- When GitHub Actions stopped starting jobs (2026-10-14), the documented exchange path went with it; the local script still works ([[qbo-oauth-token-lifecycle]]).

## Related

- [[intuit-developer-app]] · [[legal-site-source-of-truth]]
