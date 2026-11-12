# 0010. GitHub Actions CI running offline smoke tests with a stubbed graph

## Status
Accepted, retrospective, inferred 2026-11-12

## Context
Until cf21ca2 (2026-02-23) the repo had no tests or CI.

## Decision
Add `.github/workflows/ci.yml`: on every push and pull request, Ubuntu with Python 3.11 installs `-e .[dev]`, runs `ruff check tests` (tests only, not src), and `pytest -q`. `tests/test_cli_smoke.py` monkeypatches `cli.build_graph` with a `FakeGraph` and `builtins.input` with scripted lines, covering help output, a Q&A round trip, and the validate missing-file path. Dummy env `OPENAI_API_KEY=test-key` keeps imports happy with no network calls.

## Consequences
Positive:
- Cheap, deterministic, no secrets in CI.

Negative:
- No coverage of the loader, diff logic, profile builder or validator; ruff doesn't lint src/.

## Alternatives considered
Not recorded.

## Evidence
- .github/workflows/ci.yml
- tests/test_cli_smoke.py
- commit cf21ca2
