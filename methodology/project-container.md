# Project Container

What lives inside a `projects/<name>/`, how it grows, and how sessions start and stop. This is
the day-to-day mechanism of using the agent. Read it when creating or resuming a project.

## Control plane vs. record
Two different things, kept separate on purpose:
- **`state.md` — the control plane.** A lean, overwritten *snapshot* of where we are right now.
  Read first on every project orient. ~1 screen. Never an append-only log.
- **The artifacts — the record.** The accumulated design work (vision, requirements,
  architecture, component specs, decisions, roadmap). The source of truth; grows over time.
  Only the artifact(s) relevant to the current phase are opened — `state.md` says which.

Why split them: it keeps reorientation cheap (read one small file), keeps `state.md` always
relevant (no stale noise), and keeps the heavy record out of context until a phase needs it.

**One-level undo.** At each checkpoint, `state.md` is copied to `state.prev.md` *before* being
overwritten, so a botched checkpoint is always recoverable. `state.prev.md` is a recovery
artifact only — never read on orient, so it costs nothing on the normal path. Full restorable
history stays deferred to the git-substrate question (`decisions/0011`, `decisions/0005`).

## Layout
```
projects/<name>/
├── state.md          # lean snapshot — read FIRST on orient, overwritten at each checkpoint
├── state.prev.md     # last snapshot — recovery-only one-level undo, never read on orient (ADR 0011)
├── vision.md         # phase 0 — problem, users, north star, scope
├── requirements.md   # phase 1 — functional + non-functional, success criteria
├── architecture.md   # phase 2 — component map, data flow, tradeoffs
├── components/        # phase 3 — one spec per major component
├── decisions/         # ADRs, continuous (uses design/decisions/0000-template.md format)
├── roadmap.md        # phase 5 — build order / milestones
└── HANDOFF.md        # terminal build-ready handoff — brief + manifest + readiness gate
```

## Grow as you go
A new project starts with **only `state.md` + `vision.md`.** The other files are created when
their phase actually begins — not seeded empty up front. Seeding eight blank files is clutter
that fights the lean-context principle; the files appearing as phases happen also makes the
structure self-teaching. (See `design/decisions/0004`.)

## `state.md` format
```markdown
# State — <project name>

> Lean snapshot. Overwritten at each checkpoint. Read FIRST on project orient.

**Idea (one line):** ...
**Rigor:** <Sketch | Standard | Deep + any tuning>
**Current phase:** <0–5 + name>      **Status:** <e.g. mid-architecture>

## Done
- ...

## Next (immediate)
- ...

## Open threads / blockers
- ...

## Where we left off
<enough detail to resume mid-thought — the last thing we were deciding or doing>

## Active artifacts (open these next)
- `architecture.md` — <why it's active>
```

## The handoff (`HANDOFF.md`)
The terminal artifact — what makes a project "done." Its primary reader is a **build-agent**
(Claude in a build environment) that implements the design, with the user following along and
adjudicating any rare load-bearing decision that surfaces (most are already made during design).
It is **pointer-based, not a copy**: a tight brief + a reading-order manifest into the
artifacts, so there's one source of truth and nothing drifts.

The agent does **not** emit a valid handoff until the readiness gate passes — that gate is how
"build-ready" (`decisions/0003`) is enforced rather than vibes. And the gate is graded **only
after** a required pre-mortem: a deliberate adversarial pass that assumes the build failed and
hunts for why, converting each finding into a design fix, an ADR, or an explicit "Open
assumptions" entry. The pre-mortem is what keeps the self-administered gate honest
(`decisions/0010`).

```markdown
# HANDOFF — <project name>

## Build brief (the 60-second version)
- What we're building, in 2–3 sentences
- The shape: each core component in one line
- Hard constraints the builder must respect (non-functional, tech, budget)
- Where to start

## Reading order (manifest — pointers into the artifacts)
1. vision.md        — why this exists, scope
2. requirements.md  — what it must do + success criteria
3. architecture.md  — system shape & data flow
4. components/*.md   — build specs, in this order: ...
5. roadmap.md       — build sequence / milestones
6. decisions/       — the "why"; consult when a choice looks arbitrary

## Build order (first slice → full system)
- Phase 1: the smallest valuable, runnable slice
- Phase 2: ...

## Readiness gate ✅  (all must be true for the handoff to be valid)
- [ ] No load-bearing design questions left open
- [ ] Every component has a build spec with a defined interface
- [ ] Success criteria are testable
- [ ] Key tradeoffs are recorded as ADRs
- [ ] Assumptions and explicit non-goals are stated

## Open assumptions (the builder may proceed on these)
- ...
```

## Session lifecycle
**orient → work → checkpoint → clear → re-orient.** The full protocol lives in the kernel
(`CLAUDE.md` → "Checkpoint & resume"). In short: checkpoint flushes everything important to
disk (ADRs, active artifacts, then an overwritten `state.md`) so a context clear is safe; a
re-orient reads `state.md` and reopens only the artifacts it names.

## Deferred: durable checkpoint history
History of past states is intentionally *not* kept in `state.md` (that's what bloats a log).
The "why" of decisions already survives in `decisions/`. If we later want full restorable
checkpoint history, the option on the table is making this directory a **git repo** (a
checkpoint = a commit). Deferred for now — left for the reflexive loop to diagnose whether it's
worth the ceremony, or whether a fleshed-out `/checkpoint` skill is the better move
(`design/decisions/0005`).
