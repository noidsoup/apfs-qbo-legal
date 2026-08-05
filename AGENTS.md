# Agent instructions — apfs-qbo-legal

**This repo** is static HTML for **Intuit QuickBooks Online OAuth** connect / disconnect for A Place for Seniors. It is **not** the QBO→Airtable sync implementation.

## Ecosystem

→ `/Users/thedao/Repos/APFS-Database/docs/APFS-ECOSYSTEM.md`  
Workspace: `/Users/thedao/Repos/APFS-Database/apfs.code-workspace`

| Sibling | Role |
|---------|------|
| APFS-Database | QBO expense/revenue sync workflows, Integrately docs, Airtable |
| APlaceForSeniorsFrontEnd | Communities site (unrelated to QBO OAuth) |
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
