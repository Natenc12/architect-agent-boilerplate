# 0002. Lightweight reflexive self-iteration

- **Date:** 2026-06-28
- **Status:** accepted

## Context
The environment exists to build and refine the agent itself. Rather than relying only on the user
to spot what's working, the agent should carry a small ability to diagnose and improve itself —
without turning into a complex or runaway self-modifying system.

## Decision
Give the agent a lightweight reflexive loop, housed in the `design/` area (which sits outside
`projects/`): observe friction/drift → surface it in `design/open-questions.md` → propose a
change as an ADR → once agreed, promote the durable fix into the kernel.

## Alternatives considered
- **No self-iteration** — simpler, but improvement depends entirely on the user noticing.
- **Heavy autonomous self-modification** — overkill, risky, against the "keep it simple" intent.

## Consequences
- `design/` is the home of agent-self work, deliberately separate from real-idea projects.
- The methodology includes a retrospective/self-diagnosis step.
- Open: exactly when the loop fires and how light it can stay (tracked in open questions).
