# LanceDB Indexer

`scripts/index_project_knowledge_lancedb.py`

Builds a local LanceDB index of repo markdown/rules for semantic search.
Dry-run by default; `--apply` writes the index under
`uncommitted/lancedb_project_knowledge/` (override with
`PROJECT_KNOWLEDGE_LANCEDB_DIR`). Embeddings via sentence-transformers,
default `all-MiniLM-L6-v2`, offline after model cache. Shared helpers live in
`scripts/project_knowledge_lancedb_common.py`. Companion: [[lancedb-search]].
See [[semantic-search-setup]].
