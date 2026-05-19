# Chapter 13: Comments Should Describe Things that Aren't Obvious from the Code

## Core Idea
The guiding rule: comments should describe what cannot be seen in the code. Use lower-level comments to add **precision**; use higher-level comments to provide **intuition**. Same-level comments just repeat the code.

## Frameworks Introduced
- **Four comment categories**:
  1. **Interface** — precedes a class/method/struct; defines the abstraction.
  2. **Data structure member** — next to each field declaration.
  3. **Implementation** — inside method bodies; describes what (not how) blocks do.
  4. **Cross-module** — describes dependencies that span modules; place where developers will discover it.
- **Two augmentation directions**:
  - **Lower-level → precision**: units, boundary conditions inclusivity, null semantics, ownership/lifetime, invariants.
  - **Higher-level → intuition**: overall purpose, "how we got here", strategy across multiple statements.
- **Pick conventions**: follow Javadoc/Doxygen/godoc; consistency matters more than which.

## Key Concepts
- **Interface vs implementation comments**: keep separate. If the interface comment must describe implementation, the module is shallow.
- **What/why, not how** for implementation comments.
- **Variables: think nouns, not verbs** — describe what the variable represents, not how it's manipulated.
- **Designated central place for cross-module info** (e.g., `designNotes` file referenced from each affected site).

## Mental Models
- After writing, ask: "Could someone who has never seen the code write this comment by looking at the code?" If yes, the comment is worthless.
- "Obvious" is from the reader's perspective, not yours.
- Higher-level comments are easier to maintain because minor code changes don't affect them.
- A method's interface comment lets a reader use it without reading the body — that's the whole point of an abstraction.

## Anti-patterns
- **Red Flag — Comment Repeats Code**: information already obvious from the code; e.g., `// Add a horizontal scroll bar` above `new JScrollBar(HORIZONTAL)`.
- **Red Flag — Implementation Documentation Contaminates Interface**: interface doc describes how, not what. Common in interface comments that mention internal RPCs, private parameters, or class internals.
- **Comments using the same words as the entity name** (`getNormalizedResourceNames` → "Obtain a normalized resource name"). Use different words that add information.
- **Vague variable comments** ("Current offset in resp Buffer") — what does "current" mean? Whose buffer?
- **Implementation comments describing how the code does it** instead of what it's doing.

## Code Examples

Bad — vague, doesn't explain meaning:

```java
// Current offset in resp Buffer
uint32_t offset;
```

Good — precise:

```java
// Position in this buffer of the first object that hasn't
// been returned to the client.
uint32_t offset;
```

Bad — describes how the variable is set, not what it represents:

```java
/* FOLLOWER VARIABLE: indicator variable that allows the Receiver
 * and the PeriodicTasks thread to communicate ...
 * Toggled to TRUE when a valid heartbeat is received.
 * Toggled to FALSE when the election timeout window is reset. */
private boolean receivedValidHeartbeat;
```

Good — describes what it represents (and the rest is inferable):

```java
/* True means that a heartbeat has been received since the last time
 * the election timer was reset. Used for communication between the
 * Receiver and PeriodicTasks threads. */
private boolean receivedValidHeartbeat;
```

- **What it demonstrates**: Variables get noun-style comments; precise, complete, and short.

A method interface comment that defines an abstraction (Doxygen style):

```c
/**
 * Copy a range of bytes from a buffer to an external location.
 *
 * \param offset  Index within the buffer of the first byte to copy.
 * \param length  Number of bytes to copy.
 * \param dest    Where to copy the bytes; must have room for at
 *                least length bytes.
 *
 * \return The actual number of bytes copied (may be less than
 *         length if the requested range extends past the end of
 *         the buffer; 0 if there is no overlap).
 */
uint32_t Buffer::copy(uint32_t offset, uint32_t length, void* dest);
```

- **What it demonstrates**: Defines the abstraction, describes each argument and return precisely, and notes how special cases are handled (errors defined out of existence).

## Reference Tables

| Comment kind | Adds | Best for |
|---|---|---|
| Lower-level | Precision (units, bounds, ownership, invariants) | Variable declarations, return values |
| Same-level | Nothing — just repeats code | Avoid |
| Higher-level | Intuition (purpose, strategy, "how we got here") | Method bodies, class headers |

| Variable comment must answer | Examples |
|---|---|
| Units | bytes? pixels? milliseconds? |
| Bounds | start ≤ x < end?  inclusive? |
| Null semantics | When is it null? What does null mean? |
| Ownership | Who frees? When? |
| Invariants | Always non-empty? Always sorted? |

## Key Takeaways
1. Comments should describe things that aren't obvious from the code.
2. Lower-level comments add precision; higher-level comments add intuition; same-level comments are noise.
3. Interface comments define abstractions — never describe internal mechanism in them.
4. For variables, comment the noun (what it is), not the verb (how it's mutated).
5. After writing, test it: could a reader produce this comment from the code alone? If yes, rewrite or delete.
6. Use a `designNotes` file for cross-module concerns that don't fit in any single file.

## Connects To
- **Ch 4 / Ch 12**: Interface comments are how abstractions exist.
- **Ch 14**: Good names reduce — but don't eliminate — the need for comments.
- **Ch 15**: Writing comments first turns the rules above into a design tool.
- **Ch 16**: Maintaining comments as code evolves.
