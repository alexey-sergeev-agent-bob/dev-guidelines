# Chapter 9: Better Together Or Better Apart?

## Core Idea
Subdivision is not free: it adds interfaces, separation, possibly duplication. Combine code that is closely related; separate it only when independence is real. Decide based on whether the result reduces complexity overall.

## Frameworks Introduced
- **Four signs to combine**:
  1. **Shared information** (both pieces depend on the same fact, e.g., file format).
  2. **Used together** (using one almost always means using the other; bidirectional).
  3. **Conceptual overlap** (they fall under the same higher-level category).
  4. **Hard to understand independently** (you keep flipping between them).
- **Four reasons subdivision adds cost**: more components to track, more management code, separation hurts when there are dependencies, possible duplication.
- **Methods: split or join?** — split only if it produces cleaner abstractions, NOT for length alone.
  - **Subtask extraction (good)**: when a self-contained subtask is cleanly separable.
  - **Functional split (rarer)**: when one method really does two unrelated things.
  - **Joining**: when methods are conjoined or shallow.

## Key Concepts
- **Repetition**: a red flag indicating the right abstraction is missing.
- **Conjoined methods**: cannot understand one without reading the other; a sign that splitting was wrong (or both should fold back into one).
- **Logging-class anti-pattern**: extracting one-line logging helpers per call site adds interface, splits related code, and forces flipping back and forth.

## Mental Models
- "Pick the structure that results in the best information hiding, the fewest dependencies, and the deepest interfaces."
- Combining can simplify the interface (fewer methods, automatic behaviors like default buffering).
- Length alone is a bad reason to split. Depth matters more than length.

## Anti-patterns
- **Red Flag — Repetition**: same code/structure copied; signals missing abstraction.
- **Red Flag — Special-General Mixture**: a general mechanism with code specialized for one use of it.
- **Red Flag — Conjoined Methods**: can't understand one without the other.
- **Splitting based on line count rules** ("any function over 20 lines must split").
- **Selection + cursor combined into one object** when callers manipulate them separately — combining didn't simplify anything.

## Code Examples

`goto`-as-cleanup pattern (one of the few legitimate uses), drawn from Section 9.3 (eliminating duplicated cleanup at multiple error returns):

```c
result = malloc(...);
if (!result) goto cleanup;
if (something_failed()) goto cleanup;
...
return ok;

cleanup:
    free(...);
    return error;
```

- **What it demonstrates**: Joining duplicated cleanup snippets at one site, rather than repeating them. Goto is acceptable here (escape-from-nesting pattern).

A different opinion — Robert Martin (Clean Code) argues for tiny functions; the author counters: "Depth is more important than length: first make functions deep, then try to make them short enough to be easily read. Don't sacrifice depth for length."

## Reference Tables

| Signal | Action |
|---|---|
| Both pieces share the same knowledge | Combine |
| One always implies the other | Combine |
| They overlap conceptually | Probably combine |
| Hard to understand one without the other | Combine |
| One is general-purpose, the other is its specialization | Separate |
| Subdivision creates repetition | Combine |

| Method split | OK? | Why |
|---|---|---|
| Extract clean subtask | Yes | Resulting child is independent and reusable |
| Split into two methods callers must invoke together | No | Adds interface without simplification |
| Split because the function is "too long" | No | Length alone isn't the criterion |
| Join shallow methods that callers always use together | Yes | Deepens, simplifies interface |

## Key Takeaways
1. The decision is about overall complexity — information hiding, dependencies, depth — not file or function size.
2. Combine if information is shared, if combining simplifies the interface, or to eliminate duplication.
3. Separate general-purpose from special-purpose code (but combine special-purpose code with the relevant general module if it's about that module).
4. Don't split methods by length; split only to produce cleaner abstractions.
5. Extracting code into a single-call-site helper class often makes things worse — see the logging anti-example.
6. If you keep flipping between two functions to understand either, they were split incorrectly.

## Connects To
- **Ch 4**: Joining shallow methods produces deeper ones.
- **Ch 5 / Ch 6**: Shared information and special-general separation are major drivers.
- **Ch 7**: Pass-through methods often signal the wrong split.
- **Clean Code (Robert C. Martin)**: explicitly disagrees with the "small functions" rule.
