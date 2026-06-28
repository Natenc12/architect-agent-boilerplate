# Setup — Get Your Own Architect Agent Running

This is a **boilerplate**. It's pure markdown — there's no app, no install step, no dependencies.
The "agent" is Claude Code reading these files and behaving the way they prescribe. Getting it
running is mostly: put the folder somewhere and open it in Claude Code.

---

## 1. Install (2 minutes)

1. Copy this whole folder to wherever you want your design studio to live, and rename it to
   whatever you like (e.g. `~/Documents/Architect`).
2. Open that folder in **Claude Code** (CLI, desktop, or an IDE extension — any works). Claude
   automatically reads `CLAUDE.md` (the kernel) on entry, which is what turns it into the
   architect.
3. Type: **`orient`** (or just "let's get started"). The agent reads its kernel, scans the
   folder, and asks *"What are we designing today?"*

That's it. There's nothing to build or configure.

> Optional: the kernel refers to the operator generically as "you/the user." If you'd like it to
> use your name, do a find-and-replace in `CLAUDE.md`, or just tell the agent your name in-session.

---

## 2. How you drive it — the three verbs

The entire interface is three words. You don't have to say them literally; the agent picks up
the intent ("save our spot" = checkpoint), but knowing them is the whole skill:

| Say… | And the agent will… | Use it when… |
|------|---------------------|--------------|
| **orient** | Read its kernel, ask what you're designing, load just that one project's context | Starting or resuming a session |
| **checkpoint** | Flush everything important to disk (decisions, artifacts, an updated `state.md`) so it's safe to clear context | Before you clear, or after a chunk of work |
| **diagnose** | Step back and self-review the whole operation — what's working, what's drifting — and propose fixes | Things feel off, repetitive, or it's lost the plot |

**The core loop:** `orient → work → checkpoint → clear context → re-orient → continue…`

> ⚠️ **The one rule that matters most:** only what's been checkpointed survives a context clear.
> Great thinking + a clear *without* checkpointing = it's gone. When in doubt, checkpoint first.

---

## 3. Designing your first idea

Just describe it. The agent will:
1. Spin up a new `projects/<name>/` container.
2. Run **Discovery** — sharp questions until the problem, users, and scope are clear.
3. Propose a **rigor tier** — your cue to weigh in:
   - **Sketch** — small/simple; fewer, merged docs.
   - **Standard** — the default; full process, proportional depth.
   - **Deep** — large or high-stakes; full rigor, every component spec'd.
4. Walk the phases (vision → requirements → architecture → components → roadmap), capturing
   decisions as ADRs, until it can emit a gated **`HANDOFF.md`** — the build-ready spec.

You make the rare load-bearing call when one surfaces; most get settled during the design.

---

## 4. Why it's shaped this way (the design rationale)

You don't need this to use it, but it helps if you want to modify it. The full reasoning lives in
`design/decisions/` (12 ADRs). The headlines:

- **Two-tier model** (ADR 0001) — a lean always-on kernel + isolated per-idea containers. The
  kernel stays small so the context window stays sharp; isolation keeps focus on one idea.
- **Design-only, build-ready handoff** (ADR 0003) — the agent's job *ends at the spec*; it never
  writes implementation code. That boundary is what keeps it specialized.
- **Three operating verbs** (ADRs 0005, 0006) — orient / checkpoint / diagnose. Checkpoint exists
  because only disk survives a context clear; diagnose is on-demand self-improvement.
- **`state.md` as control plane** (ADRs 0004, 0005, 0011) — a lean, overwritten snapshot read
  first on every resume; the heavy artifacts are the record, opened only when a phase needs them.
- **Readiness gate + pre-mortem** (ADRs 0007, 0010) — "build-ready" is enforced by a checklist,
  and a mandatory adversarial pass keeps the self-graded gate honest.
- **Depth tiers** (ADR 0008) — flex ceremony per idea; never flex the thinking or the gate.
- **Memory boundary** (ADR 0009) — see below.

---

## 5. About persistent memory (important)

Claude Code's auto-memory persists across your sessions. This system has a strict rule (kernel
directive #7 / ADR 0009): **auto-memory is person-level only** — durable facts about *you*. Two
things must **never** go there:
- *How the agent behaves / methodology* → belongs in the kernel (`CLAUDE.md`) as ADRs.
- *Anything tied to one idea* → belongs in that idea's `projects/<name>/` container.

This keeps project isolation intact and keeps the agent's behavior auditable in the repo instead
of hidden in memory. If you watch the agent try to save a behavior rule or project fact to
memory, redirect it.

---

## 6. Customizing the agent itself

To change how the agent behaves, the intended path is: run **diagnose**, describe what you'd like
different, and let it propose and (with your OK) update its own kernel/methodology — recording an
ADR in `design/decisions/` so there's always a trail of *why*. The `design/` folder is the agent's
own workshop; the ADRs you inherited explain every choice, so you can adopt, override, or extend
them as your own.
