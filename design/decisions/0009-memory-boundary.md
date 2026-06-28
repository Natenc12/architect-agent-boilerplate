# 0009. Memory boundary — three-store routing

- **Date:** 2026-06-28
- **Status:** accepted

## Context
Durable facts can now persist in three places, and we never decided what belongs where:

1. **Kernel** (`CLAUDE.md` + `design/`) — how the agent behaves; always loaded; governs everything.
2. **Project container** (`projects/<name>/`) — everything about one idea; isolated, one open at a time.
3. **Claude's persistent auto-memory** (`~/.claude/.../memory/`) — file-based memory the *harness*
   injects into **every** session automatically, regardless of what's being worked on. It lives
   outside the repo, is already active, and writes itself as I work.

The overlap is a real leak risk, not bookkeeping:
- **Isolation breach.** The two-tier model (ADR 0001) guarantees exactly one project's context
  loads at a time. Auto-memory injects unconditionally, so a project fact saved there bleeds into
  every other project — a hole in the isolation wall.
- **Two sources of truth for behavior.** Behavior is supposed to live in the kernel as auditable,
  versioned ADRs (directive #5). Auto-memory also captures "how I should work" facts, in a store
  the user mostly can't see or review.
- **Silent erosion.** I write to auto-memory autonomously. Without a rule I'll keep dumping things
  there that should have been an ADR or a `state.md` line.

## Decision
Each store owns a distinct class of fact, with strict bans:

| Fact class | Owner | Banned from |
|---|---|---|
| Who the user is; durable cross-project preferences *about him* | **auto-memory** | — |
| How the agent behaves / methodology | **kernel** (as ADRs) | auto-memory |
| Anything tied to one idea | **project container** | auto-memory |

Rule of thumb: **auto-memory is person-level only.** If a fact is about the agent's behavior or
about a specific idea, it has a visible home in the repo and must go there instead.

## Alternatives considered
- **Let auto-memory hold whatever's convenient** — rejected: breaks project isolation and creates a
  hidden second source of truth for behavior.
- **Ban auto-memory entirely** — rejected: genuinely person-level preferences are legitimately
  global, and the harness will keep offering the store anyway. Better to scope it than fight it.

## Consequences
- Kernel gains a governing directive so the rule is enforced every session (it's always loaded).
- The two existing auto-memories both violate the rule and are **deleted** — their content already
  lives in the kernel (`CLAUDE.md` directives #2/#3) and `vision.md`. No migration needed.
- Auto-memory ends up empty of any work-specific fact. That is the correct end state, not a gap.
