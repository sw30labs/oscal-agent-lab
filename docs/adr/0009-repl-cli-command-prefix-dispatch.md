# 0009. A REPL CLI that dispatches by command prefix, plus a standalone diff CLI

## Status
Accepted, retrospective, inferred 2026-11-12

## Context
The tool is a local "copilot" for one user at a terminal.

## Decision
`cli.main()` prints a banner, builds the graph (setup hints on FileNotFoundError), then loops on `input()`: `exit/quit/q`, `clear`, `help`, `validate <file>`, `profile [--baseline X] <desc>` and `diff <a> <b>` are handled inline by importing the agent functions; anything else is appended as a `HumanMessage` and sent through `graph.invoke`, keeping conversation history in memory. `diff_cli.py` provides an argparse tool with `--no-summary` and `--json`. Commit cf21ca2 removed a local `from pathlib import Path` in the diff branch that shadowed the module-level import.

## Consequences
Positive:
- Simple, scriptable through stdin, and easy to stub in tests (0010).

Negative:
- Help text omits the `--baseline` choices validation; an unknown baseline gives an empty base set silently.
- The whole message history is resent on each graph call, but the Explainer only reads the last message.

## Alternatives considered
Not recorded.

## Evidence
- src/oscal_agent_lab/cli.py
- src/oscal_agent_lab/diff_cli.py
- commit cf21ca2 ("fix CLI path shadowing bug")
