# 0001. Ship as a hatchling-built Python package with a src layout and two console scripts

## Status
Accepted, retrospective, inferred 2026-11-12

## Context
The lab started as a single "Initial Commit" of a full scaffold (fafccda) built from a design plan ("site structure.md", added in fafccda and removed in 61240ae). It needed to be installable for local hacking and to expose a REPL plus a standalone diff tool.

## Decision
Package the code as `oscal-agent-lab` (version 0.4.0) under `src/oscal_agent_lab`, built with hatchling, requiring Python >=3.10, with console scripts `oscal-lab` (`cli:main`) and `oscal-diff` (`diff_cli:main`). Dev extras pin pytest, pytest-asyncio, ruff and mypy; ruff targets py310 with E/F/I/UP/B/SIM rules.

## Consequences
Positive:
- `pip install -e .[dev]` gives a working dev environment (used by CI, see 0010).
- The src layout keeps tests honest about importing the installed package.

Negative:
- pyproject still carries template placeholders (author "Your Name", `yourusername` URLs) and declares MIT, contradicting the Apache-2.0 LICENSE (see 0011, A-001).
- `config.BASE_DIR = parents[2]` assumes an editable/source install; a wheel install would resolve data paths inside site-packages (inferred, A-012).

## Alternatives considered
Not recorded. A plain script folder or Poetry/uv would have worked; no evidence they were weighed.

## Evidence
- pyproject.toml (build-system, [project.scripts], [tool.hatch.build.targets.wheel])
- src/oscal_agent_lab/__init__.py (`__version__ = "0.4.0"`)
- src/oscal_agent_lab/config.py (`BASE_DIR`)
- commits fafccda, 61240ae
