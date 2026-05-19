# Chapter 4: Modules Should Be Deep

## Core Idea
The most important property of a module is depth: a simple interface that hides a large, powerful implementation. Deep modules minimize what callers must know while doing maximum work for them.

## Frameworks Introduced
- **Deep module**: small interface area + large hidden functionality. Cost = interface complexity; benefit = functionality. Maximize benefit/cost.
  - **Use when**: designing any class, method, subsystem, or service.
- **Shallow module**: interface complexity ≈ implementation complexity. Adds cost (a new interface to learn) without compensating benefit.
- **Modular design**: decompose into modules with two parts each — **interface** (what callers must know) and **implementation** (how the promise is kept).
- **Classitis**: the cult of "classes should be small, not deep." Results in armies of small shallow classes whose accumulated interfaces dwarf the system's real complexity.

## Key Concepts
- **Interface (formal)**: signatures the language enforces — parameter types, return type, declared exceptions.
- **Interface (informal)**: high-level behavior, ordering constraints, invariants. Captured only in comments.
- **Abstraction**: a simplified view that omits **unimportant** details. The word "unimportant" is critical. False abstraction = omits details that ARE important.
- **Common-case principle**: design interfaces so the most common usage is the simplest.

## Mental Models
- Picture each module as a rectangle: width = interface complexity, area = total functionality. Deep = tall + narrow.
- Cost-benefit: a class earns its keep by hiding more than it exposes.
- Two ways an abstraction goes wrong: includes unimportant detail (extra cognitive load) OR omits important detail (false abstraction → obscurity).

## Anti-patterns
- **Red Flag — Shallow Module**: interface is nearly as complex as implementation. Small modules tend to be shallow.
- **Classitis**: "if classes are good, more classes must be better." Wrong — interfaces accumulate and overwhelm.
- **Boilerplate methods like `addNullValueForAttribute(attr) { data.put(attr, null); }`** — pure cost, zero hiding.
- **Java I/O classes that require buffering to be requested explicitly** — common case (buffered I/O) made hard.

## Code Examples

The Unix I/O interface — five calls; behind them sit hundreds of thousands of lines of file system, caching, and device-driver implementation:

```c
int     open(const char* path, int flags, mode_t permissions);
ssize_t read(int fd, void* buffer, size_t count);
ssize_t write(int fd, const void* buffer, size_t count);
off_t   lseek(int fd, off_t offset, int referencePosition);
int     close(int fd);
```

Java's classic shallow stack — three objects to read serialized data from a file:

```java
FileInputStream fileStream =
    new FileInputStream(fileName);
BufferedInputStream bufferedStream =
    new BufferedInputStream(fileStream);
ObjectInputStream objectStream =
    new ObjectInputStream(bufferedStream);
```

- **What they demonstrate**: deep (Unix) vs shallow + classitis (Java). Buffering should have been default.

## Reference Tables

| Property | Deep module | Shallow module |
|---|---|---|
| Interface size | Small | Comparable to implementation |
| Hidden complexity | Large | Little |
| Cost / benefit | High benefit per unit cost | Cost ≈ benefit |
| Example | Unix I/O, GC | LinkedList wrapper, getter-only methods |

## Key Takeaways
1. Interfaces should be much simpler than their implementations.
2. It is more important for a module to have a simple interface than a simple implementation.
3. Don't break large classes into many small ones reflexively — interfaces compound.
4. Garbage collection is the ultimate deep module: zero interface, huge functionality, eliminates the "free" interface entirely.
5. Make the common case simple; require explicit work only for unusual cases.

## Connects To
- **Ch 5**: Information hiding is the primary technique for making modules deep.
- **Ch 6**: General-purpose modules tend to be deeper than special-purpose ones.
- **Ch 8**: Pull complexity downward — module developers should suffer so users don't.
- **Ch 9**: When to combine vs split, with depth as the deciding criterion.
- **Ch 19.6 (getters/setters)**: Shallow methods that violate information hiding.
