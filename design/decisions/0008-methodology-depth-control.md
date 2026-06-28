# 0008. Methodology depth control: tiers as anchors, tuned per idea

- **Date:** 2026-06-28
- **Status:** accepted

## Context
Walking all six phases at full ceremony suits a large idea but over-engineers a small one —
which contradicts the "don't gold-plate" guardrail. Yet the agent also refuses to skip the
thinking. We need to right-size without losing rigor.

## Decision
Right-sizing flexes **ceremony and document count**, never the **rigor of thought**. A floor
never flexes: (1) the thinking (problem/users nailed, tradeoffs surfaced) and (2) the readiness
gate (every handoff passes it — and it self-scales, since a small system has fewer components).

Mechanism: **named tiers as anchors, tuned per idea.** At the end of Discovery the agent
proposes a tier and which artifacts apply/merge; the user confirms or tunes; it's recorded in
`state.md` so later sessions honor it. Never silently go light — propose, then get buy-in.
- **Sketch** — merge vision+requirements; one combined architecture+components doc; minimal ADRs.
- **Standard** (default) — full phases, separate artifacts, proportional depth.
- **Deep** — full rigor, per-component specs, thorough ADRs, revisit phases as needed.

## Alternatives considered
- **Pure per-idea plan (no tiers)** — flexible but less predictable, more decisions each time.
- **Rigid tiers (no tuning)** — predictable but can't adapt to an idea's quirks.
- **No depth control** — over-ceremony on small ideas, the problem we're solving.

## Consequences
- Discovery's exit now includes a confirmed tier; `state.md` carries a **Rigor** field.
- The readiness gate doubles as the rigor floor regardless of tier.
- Promoted into the kernel (directive 6) and the methodology (Right-sizing section).
- Resolves the "methodology depth control" open question.
