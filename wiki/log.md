# Log

Reverse-chronological change log. Newest on top.

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
