# Architecture Decision Records

Retrospective ADRs for oscal-agent-lab, written 2026-11-12 from the code and git history at main@cf21ca2. Format: Title; Status; Context; Decision; Consequences; Alternatives considered; Evidence. Inferred rationale is logged in [../assumptions.md](../assumptions.md).

| # | Title | Status |
|---|-------|--------|
| [0001](0001-python-src-layout-package.md) | Ship as a hatchling-built Python package with a src layout and two console scripts | Accepted (retrospective) |
| [0002](0002-langgraph-orchestration-explainer-only-graph.md) | Use LangGraph for orchestration, but wire only the Explainer into the graph for now | Accepted (retrospective) |
| [0003](0003-nist-oscal-content-submodule.md) | Use NIST's oscal-content repo, as a git submodule, as the knowledge source | Accepted (retrospective) |
| [0004](0004-faiss-rag-one-document-per-control.md) | Local FAISS RAG over the catalog, one document per control or enhancement | Accepted (retrospective) |
| [0005](0005-openai-via-langchain-env-config.md) | OpenAI through langchain-openai, configured from env/.env, temperature 0, lazy clients | Accepted (retrospective) |
| [0006](0006-deterministic-ssp-diff-llm-summary.md) | Deterministic SSP diff by control ID, with the LLM used only for the narrative | Accepted (retrospective) |
| [0007](0007-profile-builder-baseline-plus-llm-delta.md) | Build profiles from a NIST baseline plus LLM-proposed add/remove deltas | Accepted (retrospective) |
| [0008](0008-validate-against-official-oscal-schemas.md) | Validate against official OSCAL v1.1.3 JSON schemas, cached locally, with LLM explanations | Accepted (retrospective) |
| [0009](0009-repl-cli-command-prefix-dispatch.md) | A REPL CLI that dispatches by command prefix, plus a standalone diff CLI | Accepted (retrospective) |
| [0010](0010-ci-smoke-tests-with-stubbed-graph.md) | GitHub Actions CI running offline smoke tests with a stubbed graph | Accepted (retrospective) |
| [0011](0011-apache-2-licence.md) | License the project under Apache-2.0 | Accepted (retrospective) |
