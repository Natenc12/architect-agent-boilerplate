# 0014. Sync earlier artifacts when a later phase resolves their open items

- **Date:** 2026-06-28
- **Status:** accepted

## Context
Flagged speculatively by the 2026-06-28 stress-test as a watch-item ("artifact drift on
backtrack"); **observed for real** in the first dogfood. In that project, `vision.md` listed
*modality* and *persistence* under "Open at this altitude," and Phase 1 (Requirements) resolved
both — but nothing updated `vision.md`, leaving it stating as open things that were settled. ADRs
have a "superseded by" story; vision/requirements/architecture have no change story, so resolved
items rot in earlier docs.

## Decision
Adopt a lightweight **backtrack-sync convention**: when a later phase resolves (or contradicts)
something an earlier artifact still lists as open or states differently, **update the earlier
artifact in the same checkpoint** — at minimum its "open/at-this-altitude" lists. Keep it light:
a one-line correction or deletion, not a rewrite. The `state.md` checkpoint is the natural moment
to sweep for this. No new tooling; it's a discipline, scaled to the change.

## Alternatives considered
- **Leave it (rely on `state.md` as the only live truth)** — rejected: earlier artifacts are read
  during handoff and re-orientation; stale "open" items mislead a builder and future-you.
- **Heavy change-log / versioning per artifact** — rejected: ceremony out of proportion; the "why"
  already survives in ADRs. A light sync at checkpoint is enough.

## Consequences
- Note added to `methodology/project-container.md` (the artifacts/record section).
- Graduates the watch-item from "watch" to a standing convention; `open-questions.md` updated.
- Doesn't need promotion to the kernel — it's methodology detail, not a prime directive.
