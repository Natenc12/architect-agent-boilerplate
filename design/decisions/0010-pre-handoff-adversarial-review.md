# 0010. Pre-handoff adversarial review (pre-mortem before the readiness gate)

- **Date:** 2026-06-28
- **Status:** accepted

## Context
The readiness gate (ADR 0003) is the system's core quality promise — it's what makes
"build-ready" real instead of vibes. But the gate is a checklist the *same agent* ticks, and
that agent is motivated to declare the design done. A checklist graded by the party who wants to
pass it is easy to rationalize. Surfaced during the first stress-test (diagnose) of the agent:
the gate had no step that actively tries to *break* the design before grading it.

## Decision
Phase 6 (Handoff) gains a **mandatory pre-mortem** that must run *before* the readiness gate is
graded. It is a deliberate adversarial pass against the design, not a confirmation pass:

- Assume the build has failed — ask "what made it fail?" Hunt for the answers.
- Where would a builder get stuck or be forced to make a design decision we should have made?
- What's underspecified, ambiguous, or quietly assumed? What interfaces are hand-wavy?
- Which requirements have no testable success criterion? Which tradeoffs were never recorded?

Each finding is then either **resolved** (design fix / new ADR) or **converted into an explicit
entry in the handoff's "Open assumptions"** (a known, stated thing the builder may proceed on).
Only after the pre-mortem is worked through is the readiness gate graded. The pre-mortem is what
makes ticking the gate honest.

## Alternatives considered
- **Leave the gate as-is** — rejected: self-grading bias is exactly the failure the gate is
  supposed to prevent, turned inward.
- **External/second-agent review of the handoff** — deferred: heavier, and out of scope until
  dogfooding shows the self-administered pre-mortem isn't enough.

## Consequences
- `methodology/design-process.md` Phase 6 and `methodology/project-container.md` HANDOFF section
  document the pre-mortem as a required step gating the readiness checklist.
- Adds a little ceremony to every handoff — accepted, because handoff is terminal and rare, and
  it's the one place where rigor must not flex (the readiness gate is part of the floor).
