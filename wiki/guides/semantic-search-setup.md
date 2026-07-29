# Semantic Search Setup

This repo hosts the LanceDB project-knowledge sidecar (**Option A** in
[[ai-retrieval-options]]).

## Setup

```bash
python3 -m venv .venv-lancedb
.venv-lancedb/bin/pip install -r requirements-lancedb.txt
```

## Index

```bash
.venv-lancedb/bin/python -u scripts/index_project_knowledge_lancedb.py --apply
```

- Dry-run by default (no model load); `--apply` writes the index.
- Index lands in `uncommitted/lancedb_project_knowledge/` unless
  `PROJECT_KNOWLEDGE_LANCEDB_DIR` is set.
- Uses `sentence-transformers` (`all-MiniLM-L6-v2` by default); fully offline
  after the model is cached.

## Search

```bash
.venv-lancedb/bin/python -u scripts/search_project_knowledge_lancedb.py "<question>"
```

See [[lancedb-indexer]] and [[lancedb-search]].
