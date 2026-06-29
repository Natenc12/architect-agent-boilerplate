# 0016. Amending a handed-off design re-opens the gate (proportionally)

- **Date:** 2026-06-29
- **Status:** accepted

## Context
Surfaced by the 2026-06-29 diagnose. Phase 6 (Handoff) is written as **terminal** — a project
reaches a passed readiness gate and a valid `HANDOFF.md`, and design is "done." But real design
work reopens closed projects: one session reopened an already-gated project to add a new
self-check (that project's ADR 0006).

What happened exposed the gap: I made the change, logged the ADR, updated the specs, checkpointed,
and **declared the project "design-complete again" — without re-running the pre-mortem or
re-grading the gate.** The gate only got re-run because the user explicitly invoked Phase 6
afterward. Nothing in the methodology required it. So a build-ready design was momentarily
re-closed on a checkpoint alone, and correctness depended on the user knowing to ask. A checkpoint
records state; it does not certify build-readiness — only the gate does (directive #6: the gate
never flexes).

## Decision
A **design-amendment re-gate loop**: any change that touches an already-gated design **re-opens
Phase 6, proportionally to the change**. Concretely:
1. **Re-pre-mortem the delta** — an adversarial pass scoped to the change and its ripples (not the
   whole project, which already passed). Whole-project findings stand unless the change disturbs them.
2. **Append to the existing `pre-mortem.md` ledger** as a dated increment pass (ADR 0015), each
   finding + disposition (resolved → fix/ADR, or carried → Open Assumption).
3. **Re-grade the gate against the updated ledger** and update `HANDOFF.md` (manifest, constraints,
   Open Assumptions, the gate's date/ADR list).
4. **A checkpoint alone never re-closes a gated design.** Only a re-graded gate restores
   "build-ready." Until then the project is "reopened," not "complete."

Proportionality is the depth lever (ADR 0008 at the amendment grain): a one-line spec tweak is a
quick delta pre-mortem; a structural change re-examines more. The gate itself never flexes.

## Alternatives considered
- **Treat Phase 6 as truly terminal; amendments are new projects** — wrong: an amendment shares the
  whole design context; forcing a new container loses it and is pure ceremony.
- **Checkpoint is enough to re-close** — the status quo that failed here: a checkpoint isn't a gate,
  so a changed design could ship "complete" with an ungraded delta.
- **Always re-pre-mortem the entire project on any change** — over-built; the project already passed.
  Scope the adversarial pass to the delta + its ripples (proportional).

## Consequences
- `methodology/design-process.md` Phase 6 gains an **Amendments** note (re-entry loop above).
- `methodology/project-container.md` lifecycle notes that "build-ready" is reversible: an amendment
  reopens it until re-gated; `state.md` should read "reopened," not "complete," mid-amendment.
- The kernel is **not** expanded — it already delegates process to `design-process.md` and already
  forbids flexing the gate (directive #6); this is that rule applied to amendments, so it lives in
  the methodology, keeping the kernel lean.
- Minor adjacent convention: when cross-referencing an ADR across namespaces (kernel vs. a project),
  qualify it ("the kernel's ADR 0006" vs. "a project's ADR 0006") — numbers collide between containers.
