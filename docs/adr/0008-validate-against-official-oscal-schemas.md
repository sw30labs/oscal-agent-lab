# 0008. Validate against official OSCAL v1.1.3 JSON schemas, cached locally, with LLM explanations

## Status
Accepted, retrospective, inferred 2026-11-12

## Context
ValidatorAgent (v0.4) checks OSCAL documents and explains failures in plain English.

## Decision
Detect the model type from the root key (catalog, profile, system-security-plan, component-definition, assessment-plan, assessment-results, plan-of-action-and-milestones). Load the matching schema from `data/schemas/oscal_<type>_schema.json`, downloading it from the usnistgov/OSCAL v1.1.3 GitHub release on first use and caching it. The profile and SSP schemas are committed (both draft-07, `$id` .../oscal/1.1.3/...). Validate with `jsonschema.Draft7Validator`, keep the first 20 errors, and have the LLM explain up to 10 of them. If iterating errors raises `re.error` (Python's `re` lacks the `\p{}` escapes the OSCAL schemas use), fall back to a root-key check and report the document valid when that passes.

## Consequences
Positive:
- Uses the official schemas, so results match NIST's definition of valid structure.
- Works offline for profiles and SSPs because those schemas are committed.

Negative:
- The `re.error` fallback can report invalid documents as valid (A-003).
- `error_count` is the capped count (max 20), but the CLI shows it as the total (A-011).
- Other model types need network on first use.

## Alternatives considered
The code comment names the Unicode-regex limitation, but no alternative regex engine or the NIST OSCAL CLI was adopted. Not otherwise recorded.

## Evidence
- src/oscal_agent_lab/agents/validator.py
- data/schemas/oscal_profile_schema.json, data/schemas/oscal_ssp_schema.json (vendored generated schemas, summarised by `$schema`/`$id` and 47/70 definitions)
- pyproject.toml (`jsonschema>=4.20.0`)
