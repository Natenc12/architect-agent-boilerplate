# 0012. Guard against push-back decay on agreement

- **Date:** 2026-06-28
- **Status:** accepted

## Context
Surfaced by the first real dogfood project during a `diagnose` pass. Early in
the session the agent challenged hard — reframed the core concept, forced a real fork into open
discussion. But once the user shifted into rapid approval ("agree", "let's move forward"), the agent
slid from thinking-partner toward competent-producer-seeking-sign-off: it proposed and the user
rubber-stamped. The design was good, so this *might* have been genuine convergence — but the
agent could not distinguish genuine convergence from coasting on agreement. That ambiguity is
the problem. Directive #3 ("push back — you're a thinking partner, not a stenographer") is most
likely to fail silently exactly when the user is most agreeable.

## Decision
Treat **a run of fast approvals as a signal to increase scrutiny, not a green light to stop
surfacing tension.** Concretely, the agent should:
- Not let agreement velocity substitute for having surfaced the real tradeoffs. When the user is
  agreeing quickly, periodically name the **strongest remaining objection** to the current
  direction even if unprompted — and make the user actually choose, rather than nodding it
  through.
- Distinguish **earned** agreement (the user engaged with a tradeoff and chose) from **passive**
  agreement (the user waved it through), and deliberately re-introduce friction when the recent
  pattern is the latter — especially before a phase exit or a load-bearing decision.
- Proportion this to stakes: a trivial choice doesn't need manufactured debate; a make-or-break
  decision does, agreement or not.

## Alternatives considered
- **Do nothing / rely on directive #3 as written** — rejected: this dogfood showed #3 degrades
  precisely under fast approval, which is when it matters most.
- **Hard ceremony (force an explicit objection every turn)** — rejected: manufactured friction is
  noise and erodes trust; the fix is judgment proportioned to stakes, not a ritual.

## Consequences
- Promote a sentence into the kernel under Prime Directive #3 (push back) so it governs all work,
  not just this project.
- Pairs with the component-criticality triage (`0013`) and the pre-handoff pre-mortem (`0010`):
  all three are mechanisms that force scrutiny where stakes are highest.
- Risk to watch: over-correction into contrarianism. The guard is *stakes-proportioned* scrutiny,
  not reflexive disagreement.
