# Chapter 16: Modifying Existing Code

## Core Idea
Stay strategic when modifying code. Each change is an opportunity to improve the design, not just to add the smallest possible patch. Comments must be maintained alongside the code.

## Frameworks Introduced
- **Stay strategic** during modifications: ask "Is this the best I can do for the design, given my constraints?" Resist the impulse to make the smallest possible diff.
- **Leave the design at least slightly better than you found it** with every change.
- **Comment-maintenance rules**:
  1. **Keep comments near the code**. Far-away comments don't get updated.
  2. **Comments belong in the code, not the commit log**. Future devs won't dig through git history.
  3. **Avoid duplication** of documentation; reference a single source of truth.
  4. **Check the diffs** before committing — verify each change is reflected in comments.
  5. **Higher-level comments are easier to maintain** because they don't track tiny code changes.

## Key Concepts
- **Refactoring opportunity per change**: even if your task doesn't require it, look for small design improvements while you're in the area.
- **Investment mindset, applied to changes**: pay a small refactoring tax now to avoid spaghetti later.
- **Pre-commit scan**: helps catch stale comments, leftover debug code, unfixed TODOs.

## Mental Models
- "If you're not making the design better, you are probably making it worse."
- The mature design is determined more by changes during evolution than by the initial design — modify with care.
- The farther a comment is from the code, the more abstract it should be (less likely to be invalidated).
- Aim: when you're done with a change, the system should look like it was designed from the start with that change in mind.

## Anti-patterns
- **Smallest-possible-change mindset**: each minimal patch adds a special case; complexity accumulates fast.
- **Documenting code changes only in commit messages**: the info won't be where future devs look.
- **Putting all comments at the top of a method**: they drift from the code they describe.
- **Header-file-only interface comments** in C/C++: developers editing the body never see them.
- **Duplicating cross-module info**: copies drift apart.

## Code Examples

A method-level strategy comment plus per-phase comments:

```c
// We proceed in three phases:
// Phase 1: Find feasible candidates
// Phase 2: Assign each candidate a score
// Phase 3: Choose the best, and remove it
```

- **What it demonstrates**: top-of-method overview gives the strategy; details live above each phase's code, narrowest-scope-style.

## Reference Tables

| Where to put a comment | Reason |
|---|---|
| Code file, next to method body | Editors of the body see it; most likely to be updated |
| Header (.h) only | Bad — body editors don't see it |
| Top of a long method, all-in-one | Bad — drifts from individual code blocks |
| Above each major block | Good — stays close, updates with the block |
| External docs (URL/manual) | Reference, don't duplicate |
| `designNotes` file | Cross-module decisions with no obvious home |

| Maintenance technique | Goal |
|---|---|
| Comments near code | They get updated when code changes |
| No duplication | Less drift, fewer stale copies |
| Higher-level when far | Less affected by minor code changes |
| Pre-commit diff scan | Catch stale comments, debug code, TODOs |

## Key Takeaways
1. Modifying existing code is also a design activity — stay strategic, leave it better.
2. Refactor whenever practical; resist the smallest-diff temptation.
3. Comments belong in the code, not the commit log.
4. Position comments at the narrowest scope that includes the code they describe.
5. Avoid comment duplication — pick one home and reference it elsewhere.
6. Pre-commit, scan the diff and update comments + remove debug code.

## Connects To
- **Ch 3**: Strategic vs tactical — strategic also applies to modifications.
- **Ch 13**: How to write good comments; this chapter is about keeping them good.
- **Ch 15**: Comments-first reduces the maintenance load by getting comments right initially.
- **Ch 17**: Convention enforcement helps maintain consistency through evolution.
