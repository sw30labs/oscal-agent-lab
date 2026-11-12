# 0002. Use LangGraph for orchestration, but wire only the Explainer into the graph for now

## Status
Accepted, retrospective, inferred 2026-11-12

## Context
The project pitches itself as a "LangGraph-powered multi-agent copilot" with four agents (Explainer, Diff, ProfileBuilder, Validator), built in stages v0.1 to v0.4.

## Decision
`graph.build_graph()` compiles a `StateGraph(MessagesState)` with a single linear path START -> explainer -> END. Each other agent exposes a `*_node` function with a LangGraph-compatible signature, but none is added to the graph; `build_multi_agent_graph()` raises `NotImplementedError("Multi-agent graph coming in v0.5")`. The CLI dispatches diff/profile/validate by command prefix and calls the agent functions directly (see 0009). A module-level `graph = build_graph()` is compiled at import time.

## Consequences
Positive:
- Agents are usable and testable today without an intent router.
- Node functions are ready to drop into a routed graph later.

Negative:
- Only Q&A actually flows through LangGraph; "multi-agent" is aspirational at v0.4 (A-002).
- `diff_node`, `profile_builder_node` and `validator_node` are dead code from the CLI's point of view.

## Alternatives considered
A router node with conditional edges is listed as a TODO in graph.py. Whether it was deferred on purpose is not recorded (A-002).

## Evidence
- src/oscal_agent_lab/graph.py
- src/oscal_agent_lab/agents/__init__.py, agents/diff.py, agents/profile_builder.py, agents/validator.py (`*_node` functions)
- src/oscal_agent_lab/cli.py (prefix dispatch before `graph.invoke`)
- README.md, QUICKSTART.md ("What's implemented" table)
