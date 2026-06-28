# 0005. Session lifecycle: orient / checkpoint / clear, with state.md as a lean snapshot

- **Date:** 2026-06-28
- **Status:** accepted

## Context
An established design workflow is: orient → work → checkpoint →
clear context → re-orient → continue. This keeps context windows short and the agent
specialized. The mechanism that makes it safe is the reorientation anchor, `state.md`. We must
decide what `state.md` holds and how history is handled.

## Decision
- **Two-step orient:** kernel orient (read `CLAUDE.md`, scan projects) → project orient (read
  `projects/<name>/state.md` first, then only the artifacts it names). Keeps each session
  loaded with one project's context, not everything.
- **`state.md` = lean, overwritten snapshot** (control plane): phase, done, next, open threads,
  where we left off, active artifacts. ~1 screen. The artifacts are the record (data plane);
  the "why" of decisions lives in `decisions/`.
- **Checkpoint is an explicit ritual:** flush ADRs → update active artifacts → overwrite
  `state.md` → confirm safe to clear. Only persisted state survives a clear.

## Alternatives considered
- **Append-only log `state.md`** — keeps narrative but grows unbounded, gets expensive to read,
  fills with stale noise.
- **Git as checkpoint substrate (commit = checkpoint)** — gives lean snapshot *and* full
  restorable history, but adds ceremony. **Deferred** — left for the reflexive loop to decide
  whether it (or a `/checkpoint` skill) is worth it once the doc-based version is in use.

## Consequences
- Checkpoint discipline is the single load-bearing habit; if it ever feels unreliable, that's
  the trigger to adopt git or formalize a checkpoint skill.
- Protocol promoted into the kernel ("Checkpoint & resume"); details in
  `methodology/project-container.md`.
