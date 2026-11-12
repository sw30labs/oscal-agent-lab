# 0006. Deterministic SSP diff by control ID, with the LLM used only for the narrative

## Status
Accepted, retrospective, inferred 2026-11-12

## Context
DiffAgent (v0.2) compares two SSPs and explains what changed.

## Decision
`compare_ssps` loads both SSPs, keys `implemented-requirements` by `control-id`, and uses set operations for added/removed controls. For shared controls it compares a concatenated description string (statement by-component descriptions, control-level by-component descriptions, remarks); any text difference marks the control as changed. The LLM only writes the security-focused summary in `generate_diff_summary`, which sends at most 10 changed controls with before/after text cut to 200 chars. `oscal-diff` can skip the LLM call with `--no-summary` and emit machine-readable output with `--json`.

## Consequences
Positive:
- Counts and lists are reproducible and don't depend on the model.
- The diff works offline with `--no-summary`.

Negative:
- Changes to props, parameters, responsible roles or non-description fields aren't detected (`props` is captured but not compared).
- Whitespace-only edits count as changes.
- The summary can miss changes beyond the first 10.

## Alternatives considered
Not recorded.

## Evidence
- src/oscal_agent_lab/agents/diff.py
- src/oscal_agent_lab/diff_cli.py
- data/test-ssps/ssp_v1.json (ac-2, ac-3, au-2, sc-7) vs ssp_v2.json (ac-2, ac-3, au-2, au-6, ir-4)
