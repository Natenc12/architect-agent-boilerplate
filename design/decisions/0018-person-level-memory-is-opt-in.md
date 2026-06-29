# 0018. Person-level memory is opt-in here (ask-first, never auto-write)

- **Date:** 2026-06-30
- **Status:** accepted

## Context
ADR 0009 set the memory boundary: persistent auto-memory is **person-level only**. But auto-memory
is **global across all of the user's Claude Code sessions**, not scoped to the architect agent. During
a strategy/plan project, genuinely person-level facts surfaced (values filter, work-style,
history). The user declined to save any of them, with a clear reason: *they don't want facts learned
inside this design work bleeding into their unrelated Claude Code uses — they want this context kept
separate and non-personal.* So "person-level and durable" is **necessary but not sufficient** licence
to write — the global blast radius makes silent auto-writing a leak risk the user may not want.

## Decision
Inside the architect agent, person-level memory is **opt-in: ask first, never auto-write.** Even a
fact that clearly qualifies under ADR 0009 is only saved after the user explicitly approves it (showing
the proposed text first is the courtesy). The default is **don't write.** Amend kernel directive #7
to carry this.

## Alternatives considered
- **Keep auto-writing person-level facts (status quo of 0009).** Rejected — ignores that auto-memory
  is global; risks leaking this work into unrelated sessions against the user's wishes.
- **Project-scoped memory instead.** Already the rule for idea-tied facts (project container); doesn't
  solve the *person-level* case, which is legitimately cross-project but still shouldn't be silent.
- **Handle it case-by-case without a rule.** Rejected — the user asked for a durable tweak; behavior
  governance belongs in the kernel, not re-litigated each time.

## Consequences
- Refines, doesn't overturn, ADR 0009: the *categories* stand; the *person-level write* becomes
  ask-first. Kernel directive #7 updated.
- One more confirmation step before any auto-memory write — a deliberate, cheap cost.
