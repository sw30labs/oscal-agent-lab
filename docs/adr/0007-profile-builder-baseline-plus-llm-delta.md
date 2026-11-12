# 0007. Build profiles from a NIST baseline plus LLM-proposed add/remove deltas

## Status
Accepted, retrospective, inferred 2026-11-12

## Context
ProfileBuilderAgent (v0.3) turns a natural-language system description into an OSCAL profile.

## Decision
Start from the control IDs in the chosen NIST rev5 baseline profile (low, moderate (default), or high). Ask the LLM, with retrieved catalog snippets, a family list and up to 50 sample baseline IDs, to return strict JSON `{baseline, additional_controls, removed_controls, rationale}`; code fences are stripped and a parse failure falls back to "no changes". Apply the deltas as set operations and emit a minimal OSCAL profile (new UUID, oscal-version "1.1.1", one import of the NIST catalog's raw GitHub URL with `include-controls.with-ids`). The CLI writes it to `generated_profile.json` in the current working directory.

## Consequences
Positive:
- The baseline anchors the result, so the LLM only proposes deltas.
- The output is OSCAL JSON that can go straight into the validator.

Negative:
- LLM-proposed IDs aren't checked against the catalog; `get_all_control_ids()` exists but is never called (A-007).
- The profile declares oscal-version 1.1.1 while the validator uses 1.1.3 schemas (A-007).
- Profiles don't carry `modify`/parameter settings, only selection.
- Each run overwrites `generated_profile.json` in the CWD.

## Alternatives considered
Not recorded.

## Evidence
- src/oscal_agent_lab/agents/profile_builder.py (`get_baseline_controls`, `analyze_system_for_controls`, `generate_oscal_profile`, `build_profile`)
- src/oscal_agent_lab/cli.py (profile command, `generated_profile.json`)
- README.md (baseline sizes 149/287/370)
