# Chapter 11: Design it Twice

## Core Idea
Sketch at least two radically different designs for any major decision before committing. Comparing alternatives reveals weaknesses and surfaces a third approach that combines the best of both. The cost is small; the upside compounds.

## Frameworks Introduced
- **Design-it-twice**: for any major decision (interface, implementation, decomposition), produce 2+ rough alternatives, list pros/cons, then pick. Often the best option emerges by combining ideas from several.
- **Pick radical alternatives**: similar designs teach little. Force yourself to pick something genuinely different — even a "bad" alternative is instructive.
- **If none is attractive, design a third**: use the weaknesses you identified to drive a new design.
- **Apply at multiple levels**: interface design, implementation strategy, system decomposition, even UX features.

## Key Concepts
- **Sketching, not implementing**: rough out a few important methods; you don't need full detail.
- **Most important criterion (interfaces)**: ease of use for higher-level callers.
- **Other criteria**: simpler interface, more general, more efficient implementation.

## Mental Models
- For really smart people, this principle is hardest to embrace — they grew up where the first idea was good enough. As problems scale, first ideas stop being enough.
- "It's not that you aren't smart; it's that the problems are really hard."
- The act of designing twice trains design taste — you learn what makes designs better or worse.
- Cost is hours; benefit is days/weeks of clean implementation.

## Anti-patterns
- **Implementing the first idea that comes to mind** — common among smart, fast developers.
- **Sticking with a bad design because alternatives feel like admitting defeat** — design quality, not ego, is the metric.
- **Skipping the comparison because "all options look the same"** — that's a signal to design more aggressively different ones, or to design a third.

## Code Examples
*(No new code — uses the running text-storage example: line-oriented vs. character-oriented vs. range-oriented designs. The range-oriented design emerges from recognizing weaknesses in the first two.)*

## Reference Tables

| Level | Apply design-it-twice to |
|---|---|
| Class interface | What primitives to expose? |
| Implementation | Linked list vs. blocks vs. gap buffer |
| Module decomposition | Where do major boundaries go? |
| User-facing features | What does the UX flow look like? |

## Key Takeaways
1. For each major design decision, sketch at least two genuinely different alternatives.
2. List pros/cons explicitly; the best answer often combines features.
3. If no alternative is good, derive a third from the weaknesses of the first two.
4. Apply at every level: interface, implementation, decomposition.
5. Time investment is small (hours), benefit is large (cleaner code for the life of the system).
6. Design-it-twice is also a skill-builder — the comparisons teach you what makes designs work.

## Connects To
- **Ch 4 / Ch 6**: The text-storage example is used to illustrate depth and generality.
- **Ch 14**: Picking the best name is a mini design-it-twice exercise.
- **Ch 21**: Comparing alternatives clarifies what matters most.
