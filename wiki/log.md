# Log

Reverse-chronological change log. Newest on top.

## 2026-09-14

- No commits in the last 24 hours (last commit is `5c72254` from 2026-09-07).
  Working tree clean; no `docs/` changes, `AI_SESSION_MEMORY.md` still has no
  dated entries. Nothing changed for a second consecutive check — the last
  observable drift remains the weekly LanceDB rebuild under the gitignored
  `uncommitted/lancedb_project_knowledge/` (last 2026-09-08 ~03:02,
  `index_meta.json` content unchanged) per the [[lancedb-indexer]] cycle.

## 2026-09-07

- No commits in the last 24 hours, no `docs/` changes, `AI_SESSION_MEMORY.md` still
  empty, working tree clean (`.gitignore` mtime-only touch, content unchanged:
  `.loops` + `uncommitted/`). Nothing changed since the 2026-08-31 entry.

## 2026-08-31

- No commits in the last 24 hours. Working tree changes only: the LanceDB
  knowledge index under `uncommitted/lancedb_project_knowledge/` was rebuilt
  on the regular nightly cycle (2026-08-30 ~03:03; `index_meta.json`
  unchanged: `all-MiniLM-L6-v2`, 384-dim) — expected for [[lancedb-indexer]],
  no new docs. `.gitignore` was touched in the same window; content is intact
  and correct (`.loops` + `uncommitted/`).

## 2026-08-07

- **Commit `f2a7646`**: Noted `apfs-www` (marketing www companion) as a sibling
  repo in `AGENTS.md` and `.cursor/rules/apfs-ecosystem.mdc`, keeping QBO agent
  routing aligned with the five-repo estate map. See [[apfs-ecosystem]].

## 2026-08-06

- **Commit `2835fc2`**: Replaced the `AGENTS.md` stub with APFS QBO ecosystem
  context (sibling repos, invariants, topic routing) and added
  `.cursor/rules/apfs-ecosystem.mdc`. See [[apfs-ecosystem]].

## 2026-07-29

- **Commit `fcdcf29`** (2026-07-28): Added LanceDB project-knowledge indexing and
  search tooling (`scripts/index_project_knowledge_lancedb.py`,
  `scripts/search_project_knowledge_lancedb.py`,
  `scripts/project_knowledge_lancedb_common.py`, `requirements-lancedb.txt`)
  plus `.cursor/rules/project-knowledge-lancedb.mdc`. This implements
  **Option A (Python sidecar)** from `docs/ai-retrieval.md`.
- Created wiki scaffold (`wiki/`) per daily gardener bootstrap.

## Entities

- [[lancedb-indexer]] — `index_project_knowledge_lancedb.py`
- [[lancedb-search]] — `search_project_knowledge_lancedb.py`
