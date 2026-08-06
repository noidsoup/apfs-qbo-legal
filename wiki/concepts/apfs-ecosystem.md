# APFS Ecosystem Context

Context for where this repo sits in the A Place for Seniors (APFS) workspace,
per `AGENTS.md` and `.cursor/rules/apfs-ecosystem.mdc` (commit `2835fc2`).

## This repo

Static HTML for Intuit QuickBooks Online (QBO) OAuth connect / disconnect
(`connect.html`, `disconnect.html`) plus legal pages (`privacy.html`,
`terms.html`) for the Intuit app listing. It is **not** the QBO→Airtable sync.

## Invariants

1. Client secret never goes in HTML — production Client ID and exact Redirect
   URI only.
2. Redirect URI must match Intuit app settings exactly.
3. QBO→Airtable sync bugs belong in the sibling **APFS-Database** repo
   (`docs/` QuickBooks topics, GHA `qbo-*-sync.yml`).

## Sibling repos

- `APFS-Database` — QBO expense/revenue sync workflows, Integrately docs, Airtable
- `APlaceForSeniorsFrontEnd` — communities site (unrelated to QBO OAuth)
- `senior-scraper` — listing scrape (unrelated)

Estate map: `APFS-Database/docs/APFS-ECOSYSTEM.md`.
