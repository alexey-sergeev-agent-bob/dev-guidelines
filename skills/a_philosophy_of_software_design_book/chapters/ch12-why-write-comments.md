# Chapter 12: Why Write Comments? The Four Excuses

## Core Idea
Comments are essential to abstraction; they capture the design knowledge that code can't express. Refute the four excuses developers use to avoid them.

## Frameworks Introduced
- **The Four Excuses** (and their refutations):
  1. **"Good code is self-documenting"** → Code can't express the informal interface, design rationale, invariants, or "why". Without comments, abstraction is impossible.
  2. **"I don't have time"** → Writing comments is ≤ 5–10% of dev time and pays back quickly. Investment mindset (Ch 3).
  3. **"Comments get out of date"** → True but manageable; updates are small if updates are disciplined. Code reviews catch staleness.
  4. **"All comments I've seen are worthless"** → Most documentation IS mediocre, but it's a solvable problem. Chapters 13–16 teach how.

- **Comments are the only way to capture an abstraction**. The signature alone tells you nothing about behavior, side effects, preconditions, or invariants.
- **Comments help the two worst symptoms** of complexity (Ch 2): cognitive load (give readers what they need) and unknown unknowns (clarify dependencies).

## Key Concepts
- **Informal interface**: behavior, ordering constraints, error semantics — none expressible in signatures.
- **Method-name-as-comment** (Robert Martin's view): names like `isLeastRelevantMultipleOfNextLargerPrimeFactor` end up cryptic and require retyping the documentation at every call site.

## Mental Models
- An abstraction = signature + comments. Without comments, callers must read the implementation, which destroys the abstraction.
- "If users must read the code of a method to use it, then there is no abstraction."
- Comments are not a sign of failure; they encode design knowledge code can't.

## Anti-patterns
- **Skipping comments because "code is clean"** — clean code still needs interface comments.
- **Replacing comments with overly long method names** — verbose AND less informative.
- **Treating documentation as drudgery to defer** — guarantees it never gets written, or gets written badly.

## Code Examples
*(No code in this chapter — argumentative.)*

## Reference Tables

| Excuse | Author's response |
|---|---|
| Self-documenting code | Code expresses how, not why; signatures alone aren't abstractions |
| No time | <10% of dev effort, pays back quickly |
| Out of date | Manageable if comments live near code; reviews catch drift |
| Comments are worthless | Mostly true historically — fixable; Ch 13–16 |

## Key Takeaways
1. Without comments, you cannot define abstractions; users must read implementations, defeating the purpose.
2. Many things can't be expressed in code: rationale, invariants, conditions on calls, side effects, units.
3. Documentation belongs in the code, not in commit messages.
4. Bad documentation is a skill problem, not an inherent problem with comments.
5. Robert Martin's "comments are failures" stance is wrong: comments encode information code can't.

## Connects To
- **Ch 13**: How to write comments that aren't worthless.
- **Ch 15**: Write comments first — turns documentation into a design tool.
- **Ch 16**: Maintaining comments as the system evolves.
- **Clean Code (Robert C. Martin)**: explicitly disagreed with on the role of comments.
