# Chapter 19: Software Trends

## Core Idea
Evaluate every popular paradigm by one criterion: does it reduce complexity in large software systems? Some help; some hurt; some help only when used carefully.

## Frameworks Introduced
- **The complexity test**: when meeting any new methodology, ask "Does it reduce complexity?" If not, it's a fashion, not progress.

## Per-trend verdicts

### 19.1 OOP and Inheritance
- **Interface inheritance**: GOOD. Multiple implementations of one API yield depth (more impls → deeper interface). Aligns with abstraction.
- **Implementation inheritance**: USE WITH CAUTION. Reduces change amplification but creates dependencies between parent and subclasses (information leakage through shared instance vars). Hierarchies tend toward high complexity.
- **Prefer composition** to implementation inheritance. If you must use it, separate parent-managed state from subclass state.
- OOP doesn't guarantee good design — shallow classes with public state are still bad.

### 19.2 Agile Development
- **Incremental development**: GOOD — matches the book's philosophy.
- **Risk**: agile can drift into tactical programming (focus on features, not abstractions; defer design decisions).
- **The increments of development should be abstractions, not features.** When you need an abstraction, design it cleanly and somewhat general-purpose.

### 19.3 Unit Tests
- **GOOD.** Tests enable refactoring; without them, refactoring is too risky and complexity accumulates. Unit tests > system tests for catching design-related bugs.
- **Tcl bytecode rewrite anecdote**: the existing test suite caught all but one bug after a massive engine rewrite.

### 19.4 Test-Driven Development
- **NEGATIVE.** Focuses on getting features working, not finding good designs. Encourages tactical, feature-by-feature thinking.
- **Right place for test-first**: bug fixing — write a failing test that captures the bug, then fix it.

### 19.5 Design Patterns
- **MOSTLY GOOD.** Apply existing patterns instead of reinventing — usually cleaner.
- **Risk: over-application.** Forcing a problem into a pattern when a custom approach is cleaner makes things worse.

### 19.6 Getters and Setters
- **Mostly NEGATIVE.** Shallow methods that expose implementation. Public state is the underlying problem; getters/setters encode it.
- The right answer is usually not to expose instance variables in the first place.

## Mental Models
- A trend is a tool. Pick it up if it reduces complexity in this case; put it down if it doesn't.
- Popularity is not validity — interrogate every claim.
- Watch for "more is always better" thinking (more classes, more patterns, more tests-first) — usually wrong.

## Anti-patterns
- **Implementation-inheritance towers** with shared parent state: high coupling, hidden dependencies.
- **TDD as default workflow**: feature-driven; lacks design checkpoints.
- **Patterns applied because they're popular**, not because they fit.
- **Getter/setter for every field by default** — leaks the implementation.

## Code Examples
*(No code in this chapter — it's evaluative.)*

## Reference Tables

| Trend | Verdict | When to use | When to avoid |
|---|---|---|---|
| Interface inheritance | Good | Multiple impls of same API | — |
| Implementation inheritance | Caution | When composition really won't work | Default; prefer composition |
| Agile | Good | Most projects | When it slides into tactical features-only |
| Unit tests | Good | Always | — |
| TDD | Limited | Bug fixes | Greenfield design |
| Design patterns | Good | Pattern fits | Force-fitting |
| Getters/setters | Negative | When you must expose state | Default — usually you shouldn't |

## Key Takeaways
1. Judge every trend by whether it reduces complexity in your system.
2. Prefer composition over implementation inheritance.
3. Build increments as abstractions, not features — even within agile.
4. Unit tests are essential for safe refactoring; TDD per se is a tactical trap (except for bugs).
5. Design patterns are wins when they fit; losses when forced.
6. Don't expose instance variables; getters/setters are usually a smell.

## Connects To
- **Ch 3**: Tactical vs strategic — TDD risks the tactical side.
- **Ch 5**: Implementation inheritance violates information hiding.
- **Ch 6**: Build abstractions — not features — as your design unit.
- **Clean Code (Robert C. Martin)** and the **Gang of Four (Design Patterns)** book are the implicit interlocutors.
