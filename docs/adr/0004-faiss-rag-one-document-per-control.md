# 0004. Local FAISS RAG over the catalog, one document per control or enhancement

## Status
Accepted, retrospective, inferred 2026-11-12

## Context
The Explainer answers questions about 800-53 controls and must stay grounded in the catalog text.

## Decision
`oscal_loader.catalog_to_documents` flattens the catalog into one LangChain `Document` per control and per enhancement (recursing into nested `controls`), with title, family, label prop and all part prose, and metadata `control_id`, `group_id`, `group_title`, `parent_control`. `vectorstore.py` embeds these with `OpenAIEmbeddings(model=OPENAI_EMBED_MODEL)` (default text-embedding-3-small) into a FAISS index, caches it in-process, and persists it to `data/vectorstore/faiss_index` (gitignored), loading with `allow_dangerous_deserialization=True`. The Explainer retrieves k=6; the profile builder retrieves k=10 and truncates each snippet to 500 chars.

## Consequences
Positive:
- Control-sized chunks keep citations aligned with control IDs.
- The index is built once and reused across runs; no external vector DB.

Negative:
- Loading a pickled index with dangerous deserialization trusts whatever sits in `data/vectorstore/` (A-005).
- Changing the embedding model does not invalidate the cached index; a manual rebuild (`force_rebuild`) is needed.
- Building the index needs network access and an OpenAI key.
- `ssp_to_documents`, `profile_to_documents` and `add_documents_to_store` exist but nothing calls them.

## Alternatives considered
Not recorded.

## Evidence
- src/oscal_agent_lab/oscal_loader.py
- src/oscal_agent_lab/vectorstore.py
- src/oscal_agent_lab/agents/explainer.py (`get_retriever(k=6)`), agents/profile_builder.py (`get_retriever(k=10)`)
- .gitignore (`data/vectorstore/`)
- QUICKSTART.md ("1,196 control documents")
