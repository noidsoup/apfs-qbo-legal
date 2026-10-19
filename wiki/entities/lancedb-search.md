---
title: LanceDB search
type: entity
summary: "scripts/search_project_knowledge_lancedb.py: queries the local LanceDB project-knowledge index."
status: active
created: 2026-07-29
updated: 2026-10-19
last_verified: 2026-10-19
sources: [scripts/search_project_knowledge_lancedb.py]
tags: [retrieval, lancedb]
---

# LanceDB Search

`scripts/search_project_knowledge_lancedb.py`

Queries the local LanceDB project-knowledge index built by [[lancedb-indexer]]:

```bash
.venv-lancedb/bin/python -u scripts/search_project_knowledge_lancedb.py "<question>"
```

See [[semantic-search-setup]].
