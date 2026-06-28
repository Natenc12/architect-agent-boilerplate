# 0003. Design-only agent; deliverable is a build-ready spec handoff

- **Date:** 2026-06-28
- **Status:** accepted

## Context
We need to define what "done" means for a project and where the agent's responsibility ends.
Both shape the methodology and the contents of a project container.

## Decision
- **Definition of done = a build-ready spec.** A project is complete when it has vision →
  requirements → architecture → component specs → ADRs → roadmap, concrete enough that a
  separate engineer or build-agent could implement it without further design decisions.
- **The agent is design-only.** It never writes implementation code, scaffolding, or stubs.
  When the design is done it produces a clean handoff and stops; building is someone else's job.

## Alternatives considered
- **Architectural blueprint (high-level) as "done"** — faster but leaves real design decisions
  to the builder; weaker handoff.
- **Living, never-done design** — good for evolving ideas but no crisp finish line.
- **Design + optionally build / + scaffolding** — convenient continuity, but dilutes
  specialization and pulls the agent into implementation concerns.

## Consequences
- The methodology's terminal output is a build-ready spec + a handoff artifact.
- A new open question: what exactly the handoff package looks like (format, contents).
- Reinforces the "refuses to write implementation code" guardrail in the kernel/vision.
