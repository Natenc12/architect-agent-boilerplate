# Design Process

The repeatable path we walk an idea through. **Phases, not gates** — adapt the depth to the
idea. The point isn't ceremony; it's that every idea gets real rigor instead of depending on
mood. Announce which phase you're in, and don't race ahead before its exit criteria are met.

## Right-sizing rigor (depth control)
Flex **ceremony and document count**, never the **rigor of thought**. Two things never flex —
the floor:
- **The thinking** — problem and users nailed, real tradeoffs surfaced. Always.
- **The readiness gate** — every handoff passes it. It self-scales: a small system simply has
  fewer components to spec, so the same gate demands proportionally less.

At the **end of Discovery**, propose a rigor **tier** plus which artifacts apply or merge; the user
confirms or tunes; record it in `state.md`. Never silently go light — propose, then get buy-in.
Tiers are anchors, not rails — tune per idea.
- **Sketch** (small/simple) — merge vision+requirements; one combined architecture+components
  doc; minimal ADRs. Still gated.
- **Standard** (default) — full phases, separate artifacts, proportional depth.
- **Deep** (large / high-stakes / many components) — full rigor, per-component specs, thorough
  ADRs, revisit phases as needed.

---

## 0. Framing / Discovery
**Purpose:** understand the idea before shaping it.
**Ask:** What is it? What problem does it solve, for whom? Why now? What does great look like?
What is explicitly *out* of scope?
**Mode:** mostly interview/Socratic.
**Exit:** a crisp one-paragraph problem statement + a north-star vision both sides agree on,
**plus a proposed rigor tier** (see Right-sizing) confirmed with the user and recorded in `state.md`.
**Artifact:** `vision.md`.

## 1. Requirements
**Purpose:** pin down what the system must do — and must not.
**Ask:** Functional needs? Non-functional needs (scale, latency, cost, security, UX)?
Constraints? How do we know it's working (success criteria)?
**Exit:** a prioritized list of requirements with success criteria.
**Artifact:** `requirements.md`.

## 2. Architecture
**Purpose:** decide the high-level shape.
**Ask:** What are the major components and their responsibilities? How does data/control flow?
What are the big tradeoffs, and which did we pick and why?
**Mode:** strawman-heavy — draw it, react, revise.
**Exit:** a component map + data flow + the key tradeoffs named.
**Artifact:** `architecture.md`.

## 3. Component Design
**Purpose:** specify each component well enough to build.
**Ask:** For each: responsibility, interface/contract, internal approach, failure modes,
dependencies.
**Exit:** each major component has a spec someone could build from.
**Artifact:** per-component specs.

## 4. Decisions (continuous)
**Purpose:** never lose the "why."
**Practice:** whenever a real choice is made, log an ADR (`decisions/NNNN-title.md`) using
`decisions/0000-template.md`. Atomic, dated, with the alternatives considered.

## 5. Roadmap
**Purpose:** turn the design into a build order.
**Ask:** What's the smallest valuable slice? What's the dependency order? Phases/milestones?
**Exit:** a sequenced plan from first-build to full system.
**Artifact:** `roadmap.md`.

## 6. Handoff (terminal)
**Purpose:** package the design for a build-agent (with the user co-piloting) to implement.
**Practice:** produce `HANDOFF.md` — a brief + reading-order manifest pointing into the
artifacts (not a copy). It's only valid once the **readiness gate** passes (no load-bearing
questions open, every component spec'd with an interface, success criteria testable, tradeoffs
recorded, assumptions/non-goals stated). The gate is how "build-ready" is enforced.
**Pre-mortem first (required, before grading the gate):** run a deliberate *adversarial* pass —
assume the build failed and hunt for why. Where would a builder get stuck or be forced to make a
design call we should have made? What's underspecified, ambiguous, or quietly assumed? Which
requirements lack a testable criterion? Resolve each finding (design fix / ADR) or convert it
into an explicit "Open assumptions" entry. The gate is graded *only* after this. The pre-mortem
is what makes ticking the gate honest, not self-congratulatory (see `decisions/0010`).
**Exit:** a valid `HANDOFF.md`. Format lives in `methodology/project-container.md`.

---

## Diagnose — the reflexive loop (on demand)
Not a phase and not automatic. Runs only when the user says **"diagnose"** (one of the three
operating verbs: orient / checkpoint / diagnose). It's a deliberate self-review of the whole
operation — what's working, what isn't, where there's friction or drift — feeding findings into
`design/open-questions.md`, ADRs in `design/decisions/`, and durable fixes promoted into
`CLAUDE.md`. Full protocol lives in the kernel. Keep it light: observe → surface → decide →
promote.
