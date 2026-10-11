---
description: APFS cross-repo ecosystem — read before spanning sites/syncs
---

# APFS ecosystem (always)

A Place for Seniors spans five repos and three public web surfaces. Do not treat any single folder as the whole product.

**This repo:** apfs-qbo-legal, static Intuit OAuth connect/disconnect pages for APFS QuickBooks (`connect.html`, `disconnect.html`, and the `privacy.html` / `terms.html` legal pages). QBO → Airtable sync jobs live in APFS-Database (GitHub Actions), not here. Never put the Intuit client secret in these HTML pages: Client ID and redirect URI only.

**Full map:** `/Users/thedao/Repos/APFS-Database/docs/APFS-ECOSYSTEM.md`. Read it before a task that might touch the listings UI, marketing www, the listing CMS, scrapes, Airtable/FUB/SeniorPlace, or QuickBooks.

**Several sessions at once:** the hub `~/Repos/apfs/` (not a git repo) holds the session rules (`AGENTS.md`), `bin/wt` (`doctor`, `ls`, `new`) and the cross-repo `FOLLOWUPS.md` / `AI_SESSION_MEMORY.md`.

| Task smells like… | Repo | Local clone |
|---|---|---|
| Airtable fields, invoices, commissions, FUB and SeniorPlace CRM sync, QBO → Airtable sync, Hermes crons | APFS-Database | `/Users/thedao/Repos/APFS-Database` |
| communities.aplaceforseniors.org: listing UI, quiz, Netlify functions (`submit-to-fub`) | APlaceForSeniorsFrontEnd | `/Users/thedao/Repos/APlaceForSeniorsFrontEnd` |
| www.aplaceforseniors.org: homepage, Contact Page, Elementor, Code Snippets / FUB forms | apfs-www | `/Users/thedao/Repos/apfs-www` |
| SeniorPlace website scrape → WordPress listing drafts, scraper dashboard | senior-scraper | `/Users/thedao/Repos/senior-scraper` |
| Intuit OAuth connect/disconnect pages for QuickBooks | apfs-qbo-legal | `/Users/thedao/Repos/apfs-qbo-legal` |
| Listing CPT content in the listing CMS (Kinsta WordPress) | no clone: WP admin, REST read | — |

## Invariants

- Only Airtable is writable by default. WordPress (www and the listing CMS) and SeniorPlace are read-only unless Nicholas asks for a specific change.
- Production is Nicholas's call: live WordPress changes, merging the frontend's `main` (Netlify deploys it), Hermes cron edits, Airtable automation pastes.
- Do not conflate marketing www, the communities Next.js app, and the listing CMS.
- A cross-repo change is one PR per repo, linked, saying which ships first.
- After this map, follow the repo's own `AGENTS.md`.
