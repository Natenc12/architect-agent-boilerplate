# 0004. Project container structure: phase-mirrored, grow-as-you-go

- **Date:** 2026-06-28
- **Status:** accepted

## Context
Each idea gets an isolated `projects/<name>/` container. We need its internal structure — what
files exist, when they appear, and how an idea's context is reloaded on return — defined before
the first real project. The deliverable is a build-ready spec (`decisions/0003`) walked through
the methodology phases (`methodology/design-process.md`).

## Decision
- The container **mirrors the methodology phases** — one artifact (or folder) per phase:
  `vision.md`, `requirements.md`, `architecture.md`, `components/`, `decisions/`, `roadmap.md`,
  terminating in `HANDOFF.md`.
- **Grow as you go:** a new project starts with only `state.md` + `vision.md`; later files are
  created when their phase begins, not seeded empty.
- `state.md` is the **resume anchor** — read first on every project orient (see `0005`).

## Alternatives considered
- **Seed all files empty up front** — clutter; fights lean-context; less self-teaching.
- **One flat document per project** — no routing; everything loads at once; defeats focus.

## Consequences
- Structure is documented canonically in `methodology/project-container.md`.
- Per-project `decisions/` reuses the kernel's ADR format (`design/decisions/0000-template.md`).
- Open: the `HANDOFF.md` package format (tracked in `open-questions.md`).
