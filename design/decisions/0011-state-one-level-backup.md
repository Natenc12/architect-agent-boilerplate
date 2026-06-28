# 0011. One-level `state.md` backup at checkpoint

- **Date:** 2026-06-28
- **Status:** accepted

## Context
`state.md` is the single most load-bearing file in a container and is **overwritten** at every
checkpoint with no history (full restorable history was deferred to a possible git substrate —
ADR 0005). The failure mode: one botched or incomplete checkpoint, then a context clear, and the
prior snapshot is gone with no way back. Surfaced during the first stress-test (diagnose).

## Decision
A minimal, ceremony-free safety net — **not** the full git history, which stays deferred. As the
first step of the checkpoint ritual, before overwriting `state.md`, copy the current file to
`state.prev.md` (one-level undo). That single previous snapshot is enough to recover from a bad
checkpoint without reintroducing the append-log bloat we explicitly avoid.

`state.prev.md` is a recovery artifact only: it is **not** read during orient (orient still reads
`state.md` first, then the artifacts it names), so it adds zero cost to the normal path.

## Alternatives considered
- **Nothing / wait for git** — rejected: clears are routine, so the unprotected overwrite is a
  live risk now, and the fix is nearly free.
- **Multi-level rolling history in-tree** — rejected as gold-plating; that's what the deferred
  git substrate is for if we ever need it.
- **Keep history inside `state.md`** — rejected: that's the append-log bloat ADR 0005 forbids.

## Consequences
- Kernel "Checkpoint & resume" ritual and `methodology/project-container.md` document the
  copy-then-overwrite step.
- Full restorable checkpoint history remains deferred to the git-substrate question (ADR 0005).
