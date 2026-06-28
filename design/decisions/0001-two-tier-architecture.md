# 0001. Two-tier architecture: shared kernel + isolated project containers

- **Date:** 2026-06-28
- **Status:** accepted

## Context
The user wants to drop in, name an idea, and design it deeply — repeatedly, across many ideas over
time — without the agent losing focus or overflowing its context window. Specialization must
not mean ignorance; the agent has to stay both focused and capable.

## Decision
Structure the agent in two tiers:
- A **kernel** (`CLAUDE.md` + `methodology/`) holding principles, process, and routing —
  small, stable, always loaded.
- **Project containers** (`projects/<name>/`), one isolated context per idea, with only one
  open at a time.

On startup the agent runs a front-door ritual: orient → ask "what are we designing today?" →
route into the relevant container (or `design/` for agent-self work) → work in that domain.

## Alternatives considered
- **One flat workspace** — simplest, but context bleeds across ideas and overwhelms the window.
- **Everything inlined in the kernel** — no routing; defeats the focus goal.

## Consequences
- Context discipline becomes a first-class behavior (load kernel + one project only).
- The kernel must stay lean and act as a routing map, not an encyclopedia.
- Need to define project-container internals before the first real project (see open questions).
- Promoted into the kernel: the two-tier model + the front-door ritual.
