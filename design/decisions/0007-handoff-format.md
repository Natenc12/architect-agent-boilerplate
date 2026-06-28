# 0007. Handoff format: pointer-based brief + manifest with a readiness gate

- **Date:** 2026-06-28
- **Status:** accepted

## Context
The deliverable is a build-ready spec (`decisions/0003`). We need to define the terminal
`HANDOFF.md`. Its primary reader is a **build-agent** (Claude in a build environment) that
implements the design, with the user following along and making the rare load-bearing decision that
surfaces — though most such decisions are resolved during the design process itself.

## Decision
`HANDOFF.md` is **pointer-based**, not a re-copy of the spec:
- A **build brief** (60-second version: what, the shape, hard constraints, where to start).
- A **reading-order manifest** into the artifacts (vision → requirements → architecture →
  components → roadmap → decisions).
- A **build order** (smallest valuable slice → full system).
- A **readiness gate**: a checklist that must all be true for the handoff to be valid. The
  agent does not emit a valid handoff until it passes — this is how "build-ready" is enforced.
- **Open assumptions** the builder may proceed on.

Optimized to be followable by a build-agent and navigable by the user; one source of truth.

## Alternatives considered
- **Fully self-contained doc** — portable in isolation, but duplicates artifacts and drifts.
- **Human-engineer-first prose** — heavier onboarding narrative; unnecessary since the primary
  consumer is an agent and the user already holds the design context.

## Consequences
- Format documented in `methodology/project-container.md`; terminal step added to the
  methodology (`design-process.md` §6).
- The readiness gate operationalizes `decisions/0003` — "done" becomes checkable, not vibes.
- Resolves the "handoff package" open question.
