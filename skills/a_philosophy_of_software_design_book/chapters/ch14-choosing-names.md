# Chapter 14: Choosing Names

## Core Idea
Good names are documentation: they make code obvious and reduce the need for other comments. A great name creates the right image in the reader's mind. Mediocre names cost hours of debugging and design rework.

## Frameworks Introduced
- **Two properties of good names**:
  1. **Precise** — narrow enough that readers know what it refers to (and what it doesn't).
  2. **Consistent** — used the same way everywhere; never reused for a different purpose.
- **Names are abstractions**: they pick the few important features and omit the rest.
- **Three requirements for consistency**:
  1. Always use the common name for that purpose.
  2. Never use that name for anything else.
  3. The purpose must be narrow — all variables with the name must have identical behavior.
- **"When in Rome"**: when in doubt, follow existing project conventions.

## Key Concepts
- **Sprite `block` bug**: the same name `block` was used for both physical disk blocks and logical file blocks → file corruption took six months to find. Different concepts → different names (e.g., `fileBlock`, `diskBlock`).
- **Boolean names should be predicates**: `cursorVisible`, not `blinkStatus`.
- **Distinguishing prefixes** for related variables: `srcFileBlock`, `dstFileBlock`.
- **Loop convention**: `i` for outer, `j` for nested. Short loop variable names are fine when the loop fits on a screen.

## Mental Models
- "If someone sees this name in isolation, how closely will they guess what it is?"
- A name should create an image of what the entity IS and what it IS NOT.
- "The greater the distance between a name's declaration and its uses, the longer the name should be." (Andrew Gerrand, agreed by author.)
- Readability is determined by readers, not writers.

## Anti-patterns
- **Red Flag — Vague Name**: `count`, `status`, `result` (when not actually a return value), `x`/`y` for character positions instead of pixels, `block` for two unrelated concepts.
- **Red Flag — Hard to Pick Name**: if you can't find a precise short name, the underlying entity may not have a clean design — consider alternative factorings.
- **Adding generic suffixes** like `Object`, `Field` — usually empty calories.
- **Hungarian Notation**: encoding type in name; unnecessary with modern IDEs.
- **Repeating the class name in instance variables**: `File.fileBlock` → just `File.block`.
- **Names too specific** (`selection` for a parameter that's actually any range) — overconstrains the abstraction.
- **Reusing common short names for different things** (Go's `ch` for character/channel, `d` for data/distance/...).

## Code Examples

Bad — vague status booleans:

```java
// Blink state: true when cursor visible.
private boolean blinkStatus = true;
```

Good — predicate name, says what true means without consulting docs:

```java
// Controls cursor blinking: true means the cursor is visible,
// false means the cursor is not displayed.
private boolean cursorVisible = true;
```

Bad — sentinel name that hides meaning:

```java
private static final String VOTED_FOR_SENTINEL_VALUE = "null";
```

Good — name says what the sentinel represents:

```java
private static final String NOT_YET_VOTED = "null";
```

## Reference Tables

| Name pattern | Verdict |
|---|---|
| `i`, `j` in short loops | OK (short distance, idiomatic) |
| `x`, `y` for character positions | Vague — confuses with pixel coords |
| `count` (no qualifier) | Too generic |
| `numActiveIndexlets` | Precise |
| `result` in a void method | Misleading |
| `status`, `data`, `info` (alone) | Vague |
| `cursorVisible` (predicate) | Good for a bool |
| `fileBlock` / `diskBlock` (different concepts) | Good — distinguishable |

## Key Takeaways
1. Spend a little extra time on every name — the payback compounds across the system.
2. Names should create an image; they're abstractions and should focus on what's most important.
3. Precision beats brevity; consistency beats cleverness.
4. Different concepts → different names, even if they're "kind of similar" (Sprite block bug).
5. Boolean names are predicates; sentinel names convey their meaning.
6. If a name is hard to pick, the underlying design may be muddled — refactor.
7. Avoid Hungarian-style type prefixes; modern tools render them obsolete.

## Connects To
- **Ch 13**: Good names reduce, but don't eliminate, the need for comments.
- **Ch 17**: Consistent naming is one face of system-wide consistency.
- **Ch 18**: Good names are one of the two main techniques for making code obvious.
- **Go style guide (Andrew Gerrand)**: explicitly disagreed with on short names.
