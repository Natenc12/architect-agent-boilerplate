# Open Questions

Live list of unresolved design questions about the agent itself. Resolve → move the answer into
the right doc and log an ADR (`design/decisions/`) if it's a real decision.

## Open
- _(none yet)_

## Resolved
- The core design decisions are recorded as ADRs 0001–0018 in `design/decisions/`.

## Change history
What changed in each sync of this boilerplate from the upstream agent. Newest first.
- **V0.2 (2026-06-30)** — Synced to agent ADRs 0001–0018. Added ADRs 0012–0018: push-back-decay guard,
  component-criticality triage, backtrack artifact sync, durable pre-mortem ledger, amendment
  re-gate loop, Discovery diverge-before-converge, person-level memory opt-in. Kernel directives
  #2/#3/#7 updated; methodology gained component triage, the pre-mortem ledger +
  buildable-not-validated caveat + amendment re-gate loop, and backtrack artifact sync.
- **Initial snapshot** — agent ADRs 0001–0011: kernel, methodology, container model, and the three
  operating verbs (orient / checkpoint / diagnose).
