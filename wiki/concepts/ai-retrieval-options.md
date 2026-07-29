# AI Retrieval Options

Source: `docs/ai-retrieval.md`.

This project is not Python-capable at the top level, so semantic doc search was
offered three ways:

- **Option A — Python sidecar (chosen):** tiny `scripts/` + venv hosting the
  standard LanceDB scaffold scripts. Reads markdown only. See
  [[semantic-search-setup]].
- **Option B — cross-repo vault recall:** rely on centrally indexed committed
  markdown (e.g. ghembed / vault recall).
- **Option C — platform-native retrieval:** use the agent platform's own
  retrieval layer instead of a per-repo vector index.

Option A was implemented in commit `fcdcf29` (2026-07-28).
