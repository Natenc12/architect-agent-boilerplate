# 0013. Triage component criticality within Component Design

- **Date:** 2026-06-28
- **Status:** accepted

## Context
Surfaced by the first dogfood project during `diagnose`. In Phase 3 the agent gave the
make-or-break artifact (the live-student system prompt — the project's stated #1 risk) and a
trivial one (a web-lookup wrapper) roughly the same treatment: one design round, then write the
spec. The rigor *tiers* (Sketch/Standard/Deep, ADR 0008) flex across the **whole project**, but
within a single project components vary wildly in stakes, and nothing in the process said to
proportion depth across them. The riskiest spec got under-invested relative to its stakes; the
trivial one got over-served.

## Decision
**Within Component Design, triage components by risk and spend adversarial depth accordingly.**
At the start of Phase 3, rank the components by how much the design's success rides on them and
how uncertain they are. Then:
- **Make-or-break / high-uncertainty components** get genuine stress-testing — adversarial review,
  alternatives, failure-mode hunting, possibly a draft-and-critique cycle — before the spec is
  called done.
- **Trivial / low-risk components** get a short spec and move on.
This is the "right-size rigor" principle (ADR 0008) applied at the **component grain**, not just
the project grain.

## Alternatives considered
- **Uniform depth per component** — what the dogfood did; over-serves the trivial and
  under-serves the critical. Rejected.
- **A new heavyweight per-component grading step** — rejected: that's ceremony; the floor is
  thinking proportioned to stakes, not more forms.

## Consequences
- Add a note to `methodology/design-process.md` Phase 3 (Component Design): triage by risk first;
  the make-or-break component(s) earn adversarial depth, trivial ones earn brevity.
- Reinforces the same "scrutiny where stakes are highest" theme as ADRs 0010 and 0012.
