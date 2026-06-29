# 0015. Durable pre-mortem capture (survive a context clear into the gate)

- **Date:** 2026-06-29
- **Status:** accepted

## Context
ADR 0010 makes a pre-mortem mandatory *before* the readiness gate is graded — it's what keeps the
self-administered gate honest. But in the first dogfood's back half, the pre-mortem
ran in one session, was compressed to one-liners in `state.md`, then context was cleared. When the
gate was graded a session later, it could only check against that lossy summary — not the original
findings or how each was disposed. The gate depends on the pre-mortem, yet nothing durably bridges
the two across a context clear. The substance of each finding survived (each became an ADR, an Open
Assumption, or a spec edit), but the *audit trail* — "here are the things we stress-tested and how
each was resolved" — evaporated, which is exactly the evidence that makes ticking the gate honest.

## Decision
The Phase 6 pre-mortem **emits a durable artifact** — `projects/<name>/pre-mortem.md` — listing
each finding and its **disposition** (resolved → which design fix / ADR; or carried → which Open
Assumption). It is written when the pre-mortem runs and read when the gate is graded, so the gate is
verifiable against the actual pass even when the two are separated by a context clear. Keep it lean:
a one-line finding + one-line disposition each, not a narrative. The handoff's "Open assumptions"
section remains the *survivors*; `pre-mortem.md` is the full ledger behind it.

## Alternatives considered
- **Lighter: keep findings in `state.md` until the gate is graded, then drop** — rejected: `state.md`
  is a lean overwritten control-plane snapshot, not a record; parking a finding ledger there fights
  its purpose and still dies on a clear mid-way.
- **Drop it (rely on ADRs/assumptions/spec edits)** — rejected: those preserve each finding's
  *substance* but not the *enumeration*, and the enumeration is the honesty evidence 0010 exists for.

## Consequences
- `methodology/design-process.md` Phase 6: the pre-mortem produces `pre-mortem.md`; the gate is
  graded against it.
- `methodology/project-container.md`: `pre-mortem.md` added to the layout and the HANDOFF section.
- Adds one small file per handoff — accepted, same rationale as 0010: handoff is terminal and rare,
  and the gate is part of the rigor floor that must not flex.
- Kernel unchanged: this is a methodology-grain detail, not a new principle (cf. how 0013/0014 landed).
