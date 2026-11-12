# 0003. Use NIST's oscal-content repo, as a git submodule, as the knowledge source

## Status
Accepted, retrospective, inferred 2026-11-12

## Context
The agents need authoritative OSCAL content: the SP 800-53 Rev 5 catalog, the low/moderate/high baseline profiles, and example SSPs.

## Decision
Add `usnistgov/oscal-content` as a git submodule at `data/oscal-content` (pinned to bc8a528770033611df899b3d52703fb3dc91a20d on main). `config.py` hard-codes paths into it for the rev5 catalog JSON, the three baseline profile JSONs, the SSP examples directory and the profiles directory. Small hand-written SSP fixtures (`data/test-ssps/ssp_v1.json`, `ssp_v2.json`, oscal-version 1.1.1) live in the repo for diff demos.

## Consequences
Positive:
- Uses official NIST content rather than scraped or paraphrased controls.
- Pinning gives reproducible indexes for a given commit.

Negative:
- The README tells users to run `git submodule add`, which fails in a clone where the submodule already exists; `git submodule update --init` is the right step (A-008).
- If the submodule isn't checked out, the vectorstore build raises FileNotFoundError and baseline lookups silently return [] (`get_baseline_controls`).
- `OSCAL_SSP_EXAMPLES_DIR` and `OSCAL_PROFILES_DIR` are defined but unused.

## Alternatives considered
Not recorded. Downloading the catalog at runtime (as is done for schemas, 0008) was an option; no evidence it was weighed.

## Evidence
- .gitmodules; `git ls-tree HEAD data/oscal-content` -> 160000 commit bc8a528
- src/oscal_agent_lab/config.py
- data/.gitkeep, README.md (Install), src/oscal_agent_lab/cli.py (setup instructions)
- data/test-ssps/ssp_v1.json, data/test-ssps/ssp_v2.json
