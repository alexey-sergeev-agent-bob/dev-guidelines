# Chapter 17: Consistency

## Core Idea
Consistency means similar things are done in similar ways and dissimilar things in different ways. It creates cognitive leverage: knowledge from one place transfers safely to another. It's an investment that pays back forever.

## Frameworks Introduced
- **Levels of consistency**:
  - **Names** (Ch 14)
  - **Coding style** (indentation, braces, naming, ordering)
  - **Interfaces** (multiple implementations of one API)
  - **Design patterns** (MVC, observer, etc. — Ch 19.5)
  - **Invariants** (always-true properties of a structure)
- **Tactics for ensuring consistency**:
  1. **Document** — write down conventions where developers will see them.
  2. **Enforce** — automated checkers gating commits work best.
  3. **Code reviews** — train newcomers; nit-picky reviewers create cleaner code faster.
  4. **"When in Rome"** — when in a new file, look at how it's done locally and follow.
  5. **Don't change established conventions** unless the gain is enormous AND you can update all existing uses.

## Key Concepts
- **Invariant**: a property always true of a variable/structure — e.g., "every line ends with \n", "the list is non-empty". Reduces special cases.
- **Cognitive leverage**: once you've learned how something works in one place, you can safely assume the same elsewhere.
- **The line-ending example**: Unix newline vs Windows CR/LF. Convention enforced by a pre-commit hook eliminated mass false-diff problems.

## Mental Models
- "If it looks like an x, it really is an x" — that's the safety consistency provides.
- A "better idea" is rarely worth the cost of inconsistency. Only upgrade if the new idea is significantly better AND you replace all old uses.
- New developers will violate conventions unintentionally — automated enforcement is the cure.

## Anti-patterns
- **Forcing dissimilar things into the same approach** for the sake of consistency — destroys cognitive leverage.
- **Rolling-your-own variant** of a project's existing convention because you have a "better idea" — pure cost.
- **Relying on memory + reviews** alone — automated enforcement scales better.

## Code Examples
*(No new code — example: a pre-commit script that rejects files containing carriage returns when the project is newline-only.)*

## Reference Tables

| Mechanism | When best | Cost |
|---|---|---|
| Style guide doc | Anchoring shared rules | Time to write/maintain |
| Automated checker (pre-commit hook) | Mechanical, low-level rules | Time to build once |
| Code review nitpicks | Style and design alike | Reviewer time |
| "When in Rome" | Local conventions you might not know | Looking around before writing |

| Question before introducing inconsistency | If "no" anywhere → don't |
|---|---|
| Significant new info justifies the change? |
| New approach is *much* better than the old? |
| Will you update ALL old uses? |

## Key Takeaways
1. Consistency reduces cognitive load and prevents mistakes.
2. Document conventions where they'll actually be seen; encode them in tooling when possible.
3. Code reviews are training opportunities — be nit-picky.
4. "When in Rome" is the most important convention — follow what's already there.
5. Don't introduce inconsistency for a marginal improvement.
6. Don't force dissimilar things into the same pattern just for consistency's sake.

## Connects To
- **Ch 14**: Consistent naming is one face of consistency.
- **Ch 18**: Consistency is one of the two main techniques for making code obvious (along with good names).
- **Ch 19.5**: Design patterns leverage consistency, but over-application breaks it.
