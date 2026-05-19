# Chapter 18: Code Should Be Obvious

## Core Idea
Obvious code is code where a reader's first guess about behavior is correct, with little thought. Obviousness is judged by readers, not writers. Combine good names, consistency, and a few tactical techniques.

## Frameworks Introduced
- **Three sources of obviousness** (8.3):
  1. **Reduce information needed** — better abstractions, fewer special cases.
  2. **Use information readers already have** — follow conventions and reader expectations.
  3. **Present information explicitly** — good names, well-placed comments.
- **Two main techniques (recap)**:
  - Good names (Ch 14)
  - Consistency (Ch 17)
- **General-purpose helpers**:
  - **Judicious whitespace** — separate logical blocks; visually structure documentation.
  - **Strategic comments** — when nonobviousness is unavoidable, document the missing information.

## Key Concepts
- **Obvious is reader-defined**: if a reviewer says it's not obvious, it's not obvious. Don't argue.
- **Generic containers** (`Pair<K,V>`, `std::pair`) — give grouped values opaque names like `getKey()`, `getValue()`. Define a specific container instead.
- **Ease of reading > ease of writing**: spend a few extra minutes for the reader.
- **Match declaration type to allocation type**: `List<Message> incomingMessageList = new ArrayList<>()` confuses readers; if it's an ArrayList, declare as ArrayList.
- **Conform to reader expectations**: if your `main` returns but the program keeps running (background threads), document it conspicuously.

## Mental Models
- "Obvious" code = first guess is correct, made without much thought.
- Obscurity is one of the two root causes of complexity (Ch 2). This chapter is its direct attack.
- A good code review tests obviousness; learn from confused reviewers.

## Anti-patterns
- **Red Flag — Nonobvious Code**: meaning/behavior cannot be understood with a quick reading; important info is missing.
- **Generic containers as return types** (`return new Pair<>(currentTerm, false)` → caller writes `result.getKey()`).
- **Event-driven programming without comments**: control flow is invisible; mitigate with handler-invocation comments.
- **Different declared and allocated types** without reason — readers may be misled about behavior.
- **Code that violates conventions silently**: e.g., `main` that returns while threads keep running; reader assumes program exits.

## Code Examples

Bad — squeezed-out whitespace makes parameter docs unscannable:

```java
/**
 * @param numThreads The number of threads ... (10 lines run together)
 * @param handler Used as a callback ...
 */
```

Good — whitespace makes structure obvious:

```java
/**
 * @param numThreads
 *        The number of threads that this manager should spin up
 *        in order to manage ongoing connections. ...
 *
 * @param handler
 *        Used as a callback in order to handle incoming messages
 *        on this MessageManager's open connections.
 */
```

Bad — `Pair` hides what's actually returned:

```java
return new Pair<Integer, Boolean>(currentTerm, false);
// caller: result.getKey(), result.getValue() — no clue what these are
```

Good — define a specific result type with named fields and documentation.

Event-driven handler — must document when invoked:

```java
/**
 * This method is invoked in the dispatch thread by a transport if a
 * transport-level error prevents an RPC from completing.
 */
void Transport::RpcNotifier::failed() { ... }
```

- **What it demonstrates**: event handlers are never called directly — the comment supplies the missing context.

## Reference Tables

| Make code more obvious | How |
|---|---|
| Good names | Precise, consistent, predicate booleans |
| Consistency | "When in Rome", style guides, automated checkers |
| Whitespace | Blank lines between logical blocks; align doc fields |
| Strategic comments | Where unavoidably nonobvious, fill the info gap |
| Reduce information needed | Better abstractions, fewer special cases |
| Use reader's existing knowledge | Conventions, expected idioms |

| Avoid | Replace with |
|---|---|
| `Pair<K,V>` return types | Custom result class with named fields |
| Generic-named local variables in long methods | Descriptive names |
| `List<X> v = new ArrayList<>()` (mismatch matters) | Match declared and allocated types when behavior depends on it |
| Constructor that quietly spawns threads | Document; add a note where main returns |

## Key Takeaways
1. Obviousness is reader-defined; if a reviewer is confused, the code is wrong.
2. The two big techniques: good names and consistency.
3. Whitespace is design — group related code, separate phases.
4. Avoid generic containers; define purpose-built types.
5. Match declarations to allocations when the actual subtype matters.
6. When code violates expectations (e.g., event-driven, threads-keep-running), document loudly.

## Connects To
- **Ch 2**: Obscurity is one of the root causes of complexity; this chapter attacks it.
- **Ch 14**: Good names — the first technique.
- **Ch 17**: Consistency — the second technique.
- **Ch 13 / Ch 15**: Strategic commenting fills the unavoidable gaps.
