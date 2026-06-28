# Architect Agent V0.1

A design studio where you bring an idea and Claude operates as a **software architect / system
designer** — taking that idea from a one-line spark to a fully-specified system: features,
architecture, components, decisions, roadmap. It designs; it does **not** write the
implementation. The deliverable is a build-ready spec you hand to a builder.

> **New here?** Read `SETUP.md` — it covers install and how to drive the agent in ~5 minutes.

**Requirements:** [Claude Code](https://claude.com/claude-code) (CLI, desktop, or IDE extension) —
the only dependency. Everything here is plain markdown; there's nothing to install or build.

## The two-tier model

```
KERNEL (above every project — stable, shared)
  who the architect is · principles · methodology · routing
        │ routes you into ↓
PROJECT A      PROJECT B      PROJECT C     ← one isolated container per idea
(all its       ...            ...             only one open at a time
 context)
```

The kernel (`CLAUDE.md`) is small and always loaded. Only **one** project's context is pulled
in at a time. That isolation is deliberate — it's how the agent stays focused without losing the
plot.

## How to use it

Drop in and the agent runs its front-door ritual: orient → ask *"What are we designing
today?"* → route you into the right project (or the `design/` area, if the work is about
improving the agent itself) → and you work together inside that domain. The whole interface is
three words: **orient · checkpoint · diagnose** (see `SETUP.md`).

## Layout

| Path | Purpose |
|------|---------|
| `CLAUDE.md` | The kernel — operating contract, principles, routing. Always loaded. |
| `SETUP.md` | Install guide + how to drive the agent + why it's shaped this way. |
| `methodology/` | The repeatable design phases + the container/state/handoff formats. |
| `design/` | Where the agent itself is refined over time (ADRs = the "why"). |
| `projects/` | One container per real idea (none yet). |
