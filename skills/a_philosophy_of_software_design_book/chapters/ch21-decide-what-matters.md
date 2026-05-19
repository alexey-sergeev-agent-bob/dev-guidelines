# Chapter 21: Decide What Matters

## Core Idea
Software design is the art of separating what matters from what doesn't. Structure systems around the things that matter; minimize and hide the rest. Most book principles are facets of this idea.

## Frameworks Introduced
- **Look for leverage**: things that matter most are those that, when known/solved, illuminate or solve many other things. A good general-purpose API is leverage; an invariant is leverage.
- **Minimize what matters**: fewer parameters, more defaults, more hiding, more automation. The less that matters to outsiders, the simpler the system.
- **Three ways to emphasize what matters**:
  1. **Prominence** — put it in interface docs, names, parameters of common methods.
  2. **Repetition** — key ideas recur across the system.
  3. **Centrality** — it's at the heart of the design; other things depend on it (e.g., device-driver interface).
- **Two kinds of mistakes**:
  - Treating too many things as important → cluttered, shallow design.
  - Failing to recognize something is important → unknown unknowns, hidden critical info.

## Key Concepts
- **Hypothesis-driven design**: when unsure what matters, commit to a hypothesis, build, then evaluate. Right or wrong, you learn.
- **Good taste**: the ability to distinguish what's important from what isn't. A defining trait of good designers.
- **Most chapters in disguise**: abstractions hide what doesn't matter; names emphasize what does; hiding info means it doesn't matter; performance design centers what matters.

## Mental Models
- For each design decision, ask: "What matters here? What doesn't?"
- If something is repeated, prominently named, or structurally central → it should matter. If it doesn't, you have an emphasis problem.
- "If an idea impacts a system's structure significantly, then that idea matters" — the converse holds too.

## Anti-patterns
- **Java I/O** forcing every developer to know about buffered vs unbuffered when the difference almost never matters.
- **Methods with many irrelevant parameters** to most callers.
- **Hiding what's important** (no error reporting where users actually need to know).
- **Clutter from unimportant features**: shallow classes built around what shouldn't matter.

## Code Examples
*(No new code — uses prior examples: text-class API, Java I/O, RAMCloud Buffer.)*

## Reference Tables

| To emphasize | Technique |
|---|---|
| Prominence | Place in interface docs, names, common parameters |
| Repetition | Key ideas recur across modules |
| Centrality | Make it the structural backbone (e.g., device-driver interface) |

| To de-emphasize | Technique |
|---|---|
| Hide | Inside module; not in interface |
| Default | Auto-compute or default; don't force a choice |
| Move off the critical path | Special cases handled rarely, far from the main flow |

## Key Takeaways
1. Identify what matters most; structure the system around it.
2. Minimize what matters; provide defaults, hide internals, automate decisions.
3. Emphasize what matters via prominence, repetition, and centrality.
4. Two failure modes: too much marked important (clutter), too little (unknown unknowns).
5. When unsure, hypothesize, build, and learn from being right or wrong.
6. "Good taste" = the trained ability to make this distinction quickly.

## Connects To
- **Ch 4**: Abstractions hide what doesn't matter.
- **Ch 5**: Information hiding is "what doesn't matter to others".
- **Ch 8**: Auto-determining values pulls "doesn't matter to users" downward.
- **Ch 14**: Names emphasize the most important aspects of an entity.
- **Ch 20**: Performance design structures around what matters when speed matters.
