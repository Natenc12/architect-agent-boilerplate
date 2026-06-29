# 0017. Discovery diverges before it converges (anti-premature-convergence guard)

- **Date:** 2026-06-30
- **Status:** accepted

## Context
On the **2nd real project** (also the *first strategy/plan* project), the
agent converged too early in Discovery **twice**: it narrowed to a few traditional buckets,
then proposed a strawman, before the possibility space had genuinely been explored.
The user had to redirect both times — *"you're being narrow/traditional"* and *"you're committing too
fast"* — and explicitly flagged that **Discovery is the time to expand and think of possibilities;
finding answers too quickly forecloses potential.** The agent self-corrected each time and the
outcome was good, but the *pattern* (it took the user to catch it) is the failure.

This is distinct from **ADR 0012** (push-back decay = *under*-challenging under a run of approvals).
This is the opposite: the agent *over-converging* during Discovery. The two are complementary guards.

## Decision
Make **divergence-before-convergence** an explicit Discovery discipline.
- Don't switch into strawman / proposing-a-direction mode until several **distinct framings** have
  been surfaced and weighed. A single early-plausible option is **not** concreteness.
- Directive #2's "propose strawmen when concrete" requires concreteness **earned by exploration**,
  not assumed because the first option looks reasonable.
- **Tripwire:** if the user signals the agent is narrowing or rushing, treat it as confirmation it
  converged early — *widen, don't defend the strawman.*
Promote a lean clause into **kernel directive #2** and a Phase-0 discipline into
`methodology/design-process.md`.

## Alternatives considered
- **Leave as a watch-item.** Rejected — the user explicitly wants Discovery protected from rushing; they
  consider premature convergence a real problem, not a speculative one.
- **A blanket "slow down / don't commit" rule across all phases.** Rejected — convergence is
  *correct* in later phases ("act when you can act"; strawman-heavy architecture). The fix is
  Discovery-specific; over-applying it would make the agent waffle where it should decide.

## Consequences
- Discovery may run a little longer before a tier is proposed — that's the intended trade
  (expansion > speed in Phase 0).
- Pairs with ADR 0012 as the two-sided scrutiny guard: 0012 stops under-challenging on approvals;
  0017 stops over-converging in Discovery.
- Kernel directive #2 + `design-process.md` Phase 0 updated. Nothing for auto-memory (methodology).
