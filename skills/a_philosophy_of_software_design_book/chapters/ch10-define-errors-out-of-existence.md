# Chapter 10: Define Errors Out Of Existence

## Core Idea
Exception handling is one of the worst sources of complexity. The best way to fight it is to reduce the number of places where exceptions must be handled — ideally, by changing semantics so the error case becomes a normal case.

## Frameworks Introduced
Four techniques for reducing exception handling, in priority order:
1. **Define errors out of existence**: redesign the API so the condition isn't an error. Example: `unset` "remove a variable" → "ensure variable doesn't exist". Calling on a missing variable becomes a no-op.
2. **Mask exceptions**: detect and handle low in the stack so higher layers never see them. Example: TCP retransmits lost packets; NFS hangs while servers are down rather than aborting apps.
3. **Aggregate exceptions**: handle many exceptions in one place. Example: a Web server's top-level dispatcher catches all parameter errors and emits one error response, instead of try/catch around every `getParameter` call.
4. **Just crash**: for rare, unrecoverable conditions, abort with diagnostics (`malloc` failure, internal-invariant violation).

## Key Concepts
- **Exception**: any uncommon condition that alters normal control flow — formal exceptions, error returns, special values.
- **Error promotion**: handle a smaller error by escalating to a larger, already-supported recovery (RAMCloud crashes a server when an object is found corrupt — reuses crash recovery instead of building a new mechanism).
- **Studies cited**: 90%+ of catastrophic failures in distributed systems come from incorrect error handling.

## Mental Models
- "Code that hasn't been executed doesn't work." Rarely-run handlers are routinely buggy.
- The complexity of exceptions comes from the **handlers**, not the throwing. Reduce handlers; don't proliferate them.
- Throwing exceptions to "let the caller decide" usually just multiplies the problem.
- Mask low (handlers near the cause); aggregate high (one handler for many causes). Pick based on which placement catches more.

## Anti-patterns
- **Defensive over-throwing**: rejecting anything suspicious produces a forest of unnecessary exceptions.
- **`String.substring` throwing IndexOutOfBoundsException**: a one-line call becomes 5–10 lines of pre-checks. Python's slice approach (clamp/return empty) is better.
- **Windows file deletion failing on open files**: Unix marks for deletion and lets it succeed → no error to handle.
- **Catching+ignoring errors at the source repeatedly**: indicates the API has too many error cases.
- **Per-call try/catch wrappers** around the same conceptual error (parameter missing, parse failure, etc.) — should aggregate at the dispatcher.
- **Masking that loses essential information**: don't mask network failures inside a comm library; callers need to know about message loss to build robust apps.

## Code Examples

Cluttered handler-per-exception (a clear sign aggregation/masking is missing):

```java
try (
    FileInputStream fileStream = new FileInputStream(fileName);
    BufferedInputStream bufferedStream = new BufferedInputStream(fileStream);
    ObjectInputStream objectStream = new ObjectInputStream(bufferedStream);
) {
    for (int i = 0; i < tweetsPerFile; i++) {
        tweets.add((Tweet) objectStream.readObject());
    }
} catch (FileNotFoundException e) { ... }
catch (ClassNotFoundException e) { ... }
catch (EOFException e) {
    // Not a problem: not all tweet files have full set of tweets.
}
catch (IOException e) { ... }
catch (ClassCastException e) { ... }
```

- **What it demonstrates**: try-catch boilerplate exceeds the actual normal-case code. Many of these should be defined away or aggregated.

## Reference Tables

| Technique | When best | Where the handler lives |
|---|---|---|
| Define out of existence | Caller doesn't actually care; no-op or natural fallback exists | Nowhere — there's no error |
| Mask | Library/lower-level method has many callers; the error is recoverable internally | Low in the stack |
| Aggregate | Many call sites produce conceptually-similar errors handled the same way | High, near a request boundary |
| Crash | Rare, unrecoverable, indicates a bug or impossible-to-handle condition | Top-level abort |

## Key Takeaways
1. Exceptions thrown by a class are part of its interface; more exceptions → shallower class.
2. Throwing is easy; handling is hard. Minimize handler count.
3. Define errors out of existence whenever the API can absorb the case naturally.
4. Mask low (cause) for many-callers/recoverable; aggregate high for many-causes/same-handling.
5. Use `ckalloc`-style crash for `malloc` failure; don't pretend to handle the unrecoverable.
6. Don't take this too far: when the caller genuinely needs to know (lost network packets, peer failure), expose the exception.

## Connects To
- **Ch 6**: Eliminating special cases reduces exceptions.
- **Ch 8**: Exception masking is a textbook example of pulling complexity downward.
- **Ch 21**: Decide what matters — expose what callers need; hide what they don't.
- **Ding Yuan et al., USENIX OSDI 2014**: "Simple Testing Can Prevent Most Critical Failures" — the 90% statistic.
