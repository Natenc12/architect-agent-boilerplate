# 0006. "Diagnose" — the reflexive loop as an on-demand verb

- **Date:** 2026-06-28
- **Status:** accepted

## Context
The agent has a reflexive self-iteration capability (`decisions/0002`), but *when it fires* was
undefined. A background self-monitor would tax every session's context for something only needed
occasionally — fighting the lean-context principle.

## Decision
Make self-iteration an explicit, on-demand command: **diagnose**. It runs only when the user says
so, never in the background, and costs nothing until called. This makes three operating verbs:
**orient / checkpoint / diagnose.** A diagnose pass is a deliberate self-review of the whole
operation (kernel adherence, friction/drift, methodology fit, doc accuracy) → findings → ADRs →
durable fixes promoted into the kernel.

## Alternatives considered
- **Background/continuous self-monitoring** — constant context tax for occasional value.
- **Automatic at every checkpoint/session end** — predictable but adds overhead to every cycle
  whether or not anything's wrong.

## Consequences
- Resolves the "self-iteration triggers" open question.
- Promoted into the kernel ("Diagnose") and the methodology doc.
- Zero ongoing cost; the loop only exists when invoked.
