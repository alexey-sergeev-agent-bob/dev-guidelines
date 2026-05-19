# Cheatsheet — Ousterhout, *A Philosophy of Software Design*

## Core Principles (verbatim, from the book's appendix)

1. Complexity is incremental: you have to sweat the small stuff.
2. Working code isn't enough.
3. Make continual small investments to improve system design.
4. Modules should be deep.
5. Interfaces should be designed to make the most common usage as simple as possible.
6. It's more important for a module to have a simple interface than a simple implementation.
7. General-purpose modules are deeper.
8. Separate general-purpose and special-purpose code.
9. Different layers should have different abstractions.
10. Pull complexity downward.
11. Define errors out of existence.
12. Design it twice.
13. Comments should describe things that are not obvious from the code.
14. Software should be designed for ease of reading, not ease of writing.
15. The increments of software development should be abstractions, not features.
16. Separate what matters from what doesn't matter and emphasize the things that matter.

---

## All Red Flags (book's complete summary)

| Red Flag | Definition | Chapter |
|---|---|---|
| Shallow Module | Interface ≈ implementation in complexity | 4 |
| Information Leakage | A design decision reflected in multiple modules | 5 |
| Temporal Decomposition | Code structured by execution order, not knowledge | 5 |
| Overexposure | API for common feature forces awareness of rare features | 5 |
| Pass-Through Method | A method does almost nothing but call another with the same signature | 7 |
| Repetition | Non-trivial code repeated over and over | 9 |
| Special-General Mixture | Special-purpose code mixed into general-purpose mechanism | 9 |
| Conjoined Methods | Can't understand one method without the other | 9 |
| Comment Repeats Code | Info in comment is obvious from adjacent code | 13 |
| Implementation Documentation Contaminates Interface | Interface comment describes implementation details | 13 |
| Vague Name | Name is too imprecise to convey meaning | 14 |
| Hard to Pick Name | Can't find a precise short name | 14 |
| Hard to Describe | Comment must be long-and-complete to cover variable/method | 15 |
| Nonobvious Code | Behavior/meaning not understandable at a glance | 18 |

---

## Decision Tables

### When designing a method

| Question | Yes → | No → |
|---|---|---|
| Hides significant complexity? | Deep — keep | Shallow — combine or remove |
| Used in multiple call sites? | Probably worth it | Possibly too specialized |
| Signature near-identical to another method? | Possible pass-through | OK |
| Interface comment is short and complete? | Good abstraction | Probably ill-defined |

### When deciding combine vs split

| Signal | Action |
|---|---|
| Two pieces share a fact (format, protocol) | Combine |
| Used together bidirectionally | Combine |
| Repeated code | Combine (extract abstraction) |
| Specialization of a general mechanism | Separate |
| Conjoined methods | Combine |
| Two methods truly do unrelated things | Split |

### Exception handling priority

| Option | Priority | Use when |
|---|---|---|
| Define out of existence | 1 | Caller doesn't really need to know |
| Mask | 2 | Library handles it; callers don't care |
| Aggregate | 3 | Many call sites with same handling |
| Crash | 4 | Rare, unrecoverable, indicates a bug |

### Performance triage

| Step | Action |
|---|---|
| 1 | Measure — find hotspots |
| 2 | Re-design around the critical path; ideal-code first |
| 3 | One single check at top dispatches all special cases off path |
| 4 | Re-measure; back out changes that don't help |

---

## Quick Heuristics

- **Names**: predicate booleans (`cursorVisible`); precise nouns; consistent everywhere; different names for different concepts (Sprite block bug).
- **Comments**: write first; describe non-obvious things; nouns for variables; what/why (not how) for impl; near the code; designNotes for cross-module.
- **Abstractions, not features**: agile/incremental fine, but each increment should be an abstraction.
- **Investment**: 10–20% of dev time on design quality; payback 6–18 months.
- **Whitespace and blank lines** are design — separate logical blocks.
- **Custom result types** beat `Pair`/tuple-as-return.
- **Match declared and allocated types** when behavior depends on subclass.
- **"When in Rome"** — follow local conventions; don't introduce inconsistency.

---

## Authors-the-book-disagrees-with

| Source | Disagreement |
|---|---|
| Robert C. Martin, *Clean Code* | Function-length rule; "comments are failures" |
| Andrew Gerrand, Go style | Single-letter names; same name for different things |
| TDD orthodoxy | Test-first as default workflow (instead: bug fixes only) |
| Classitis (Java culture) | "Classes should be small, not deep" |
