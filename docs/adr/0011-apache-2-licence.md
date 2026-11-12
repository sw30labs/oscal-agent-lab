# 0011. License the project under Apache-2.0

## Status
Accepted, retrospective, inferred 2026-11-12

## Context
The repo was created on GitHub with a LICENSE file (d62a888), and the README states the licence.

## Decision
Apache License 2.0, as in LICENSE (d62a888) and the README badge and header (90dcfb8). pyproject.toml declares `license = "MIT"` and an MIT classifier, which contradicts this.

## Consequences
Positive:
- Permissive licence with an explicit patent grant.

Negative:
- Package metadata (MIT) disagrees with the LICENSE file (Apache-2.0), so PyPI or SBOM tooling would report the wrong licence (A-001).

## Alternatives considered
MIT appears in pyproject.toml, probably from a template (inferred, A-001).

## Evidence
- LICENSE (commit d62a888)
- README.md (badge and "License: Apache 2.0", commit 90dcfb8)
- pyproject.toml (`license = "MIT"`, classifier)
