# Chapter 5: Information Hiding (and Leakage)

## Core Idea
Each module should encapsulate a few pieces of design knowledge — data structures, algorithms, conventions — that don't appear in its interface. Information hiding is the most important technique for making modules deep.

## Frameworks Introduced
- **Information hiding** (Parnas 1972): each module encapsulates design decisions whose details don't appear in its interface. Examples: B-tree internals, TCP protocol, JSON parsing, thread scheduling.
  - Reduces complexity two ways: simpler interface (lower cognitive load) AND easier evolution (no external dependencies on the hidden info).
- **Information leakage**: a design decision is reflected in multiple modules. The opposite of information hiding. The most important red flag in software design.
- **Temporal decomposition**: structuring code by the order in which operations happen ("first read, then parse, then write"). Almost always leaks information because the same knowledge gets used at different times.

## Key Concepts
- **Private vs hidden**: declaring fields `private` is not the same as information hiding. Public getters/setters can leak everything anyway.
- **Partial information hiding**: rare features accessed through separate methods → still mostly hidden from common-case callers.
- **Back-door leakage**: two classes share knowledge of, e.g., a file format even if neither exposes it via interface. Worse than interface leakage because invisible.
- **Defaults** as partial hiding: don't make callers learn about something they almost never override.

## Mental Models
- Ask "What knowledge does this module own that no other module needs to know?" That's what to hide.
- "Information hiding can often be improved by making a class slightly larger" — combine if both pieces share knowledge of the same thing.
- When designing modules, focus on the **knowledge needed** to perform tasks, not the **order** in which tasks happen.
- Think of leakage as gravity: information wants to spread; design must actively contain it.

## Anti-patterns
- **Red Flag — Information Leakage**: same knowledge used in multiple places (e.g., two classes both parsing the same file format).
- **Red Flag — Temporal Decomposition**: code structured by execution order; same knowledge appears in different stages.
- **Red Flag — Overexposure**: API for a common feature forces users to learn about rarely-used features.
- **Returning internal data structures from getters** (e.g., `getParams()` returning the underlying Map) — exposes representation, locks future evolution.
- **Requiring callers to know obvious values** (e.g., HTTP version, response Date header) — should default automatically.

## Code Examples

Bad — exposes internal Map representation:

```java
public Map<String, String> getParams() {
    return this.params;
}
```

Good — deeper, hides representation, handles type conversion:

```java
public String getParameter(String name) { ... }
public int    getIntParameter(String name) { ... }
```

- **What it demonstrates**: Replace shallow accessor that returns internal collection with focused, typed lookups; hide the data structure.

## Reference Tables

| Symptom | Cause | Fix |
|---|---|---|
| Same format/protocol referenced in 2+ modules | Information leakage | Merge, or extract one class that owns the knowledge |
| Pipeline stages all parse the same data | Temporal decomposition | Combine reading + parsing in one class |
| Callers must know fields/structures internal to a class | Overexposure | Provide focused methods; hide the structure |
| Caller has to specify obvious values (HTTP version, Date) | Missing defaults | Default and offer override |

## Key Takeaways
1. Hide design decisions — their data structures, algorithms, formats, conventions.
2. Information leakage is the most important red flag; develop high sensitivity to it.
3. If two classes share knowledge of one thing, consider merging them — even at the cost of a bigger class.
4. Avoid temporal decomposition; organize by knowledge, not by execution order.
5. Make the common case simple via good defaults; hide rarely-needed features behind separate methods.
6. `private` ≠ hidden. Don't accidentally re-export fields via getters/setters.

## Connects To
- **Ch 4**: Hiding more information makes modules deeper.
- **Ch 6**: General-purpose interfaces hide more than special-purpose ones.
- **Ch 7**: Pass-through methods/decorators are signs that information is being shuttled across layers without hiding.
- **David Parnas, "On the Criteria to be Used in Decomposing Systems into Modules"** (1972) — the canonical paper introducing this idea.
