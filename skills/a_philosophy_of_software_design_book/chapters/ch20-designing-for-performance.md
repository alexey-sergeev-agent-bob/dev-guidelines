# Chapter 20: Designing for Performance

## Core Idea
Clean design and high performance are compatible — usually mutually reinforcing. Use basic awareness of what's expensive to choose naturally fast designs. Optimize only after measuring. When you must, design around the critical path.

## Frameworks Introduced
- **Three regimes**:
  - **Don't ignore performance** — accumulated 1000-cuts inefficiency is hard to fix later.
  - **Don't optimize everything** — wastes effort, often doesn't help, adds complexity.
  - **The middle path**: develop awareness of expensive operations; pick naturally efficient designs that are also clean.
- **Measure before and after**: programmer intuition about performance is unreliable; identify hotspots, then verify the fix actually helped (and back it out if it didn't).
- **Design around the critical path**:
  1. Identify the smallest amount of code needed for the common case.
  2. Imagine that "ideal" code unconstrained by current structure.
  3. Re-design the existing structure as close to the ideal as possible while staying clean.
  4. Special cases handled OFF the critical path with one early branch.

## Key Concepts
- **Critical path**: the minimum code that must execute in the most common case.
- **Single special-case test**: ideally, one if-statement at the start handles every special case; the common path runs untested afterward.
- **Hash table vs ordered map**: same simplicity, 5–10× faster — pick hash table unless you need ordering.
- **Array of structs vs array of pointers to structs**: one allocation vs many; significant.
- **Network kernel bypass (RAMCloud)**: complexity worth paying when measurements prove it's required.

## Mental Models
- "Simpler code tends to run faster than complex code." Deep classes mean fewer layer crossings; eliminating special cases removes runtime checks.
- Don't trust intuition about hot spots — measure.
- Performance redesigns work best when they also simplify (the RAMCloud Buffer rewrite was 20% smaller AND 2× faster).

## Anti-patterns
- **Optimize-by-vibe**: tweaking based on intuition; usually wastes time and adds complexity.
- **Premature pessimization**: choosing slow primitives by default (e.g., ordered map when hash table is fine).
- **Layered shallow critical path**: multiple shallow methods chained together for a hot operation — simplify and inline.
- **Repeated checks for the same special case** in a hot path.

## Reference Tables — Today's expensive operations

| Operation | Cost | Cost in instructions |
|---|---|---|
| Network round-trip (datacenter) | 10–50 µs | tens of thousands of instr |
| Network round-trip (WAN) | 10–100 ms | many millions |
| Disk I/O | 5–10 ms | millions |
| Flash storage I/O | 10–100 µs | tens of thousands |
| New non-volatile memory | ~1 µs | ~2000 |
| Dynamic allocation (malloc/new) | Significant | varies |
| Cache miss to DRAM | hundreds of instr | dominates many programs |

## Code Examples

### Original (slower, layered, multiple special-case checks)
The original `Buffer::alloc` flowed through `allocateAppend` → `Allocation::allocateAppend` — three methods with similar signatures. It checked the same special cases twice, allocated regardless of whether the last chunk could be extended, then merged afterward. Six distinct conditions on the critical path.

### Refactored (single method, single guard)
The new design introduced an `availableAppendBytes` instance variable that is zero whenever any of three special cases hold (no chunks, last chunk not internal, no space). One test handles all three; the common path is a few instructions:

```c
// Pseudocode of the critical path
if (numBytes <= availableAppendBytes) {
    availableAppendBytes -= numBytes;
    totalLength       += numBytes;
    return lastChunk->dataEnd;
}
// fall through to the slow path off the critical path
```

- **What it demonstrates**: Result — 8.8 ns → 4.75 ns (~2× faster), code is 20% smaller, and the design is also cleaner. Eliminating shallow layers and consolidating the special-case test produces a deep, fast method.

## Key Takeaways
1. Awareness of expensive operations lets you choose naturally fast and clean designs.
2. Measure before optimizing; then re-measure to confirm.
3. If a "performance fix" doesn't measurably help, back it out.
4. Designing around the critical path means: imagine the ideal common-case code and rebuild the design around it.
5. Use a single early test to dispatch all special cases off the critical path.
6. Performance tuning often simplifies the design (deeper classes, fewer layers, fewer checks).

## Connects To
- **Ch 4**: Deep classes mean fewer layer crossings — natural performance benefit.
- **Ch 6**: Eliminating special cases helps both clarity and performance.
- **Ch 7**: Pass-through layers add measurable overhead.
- **Ch 21**: "Decide What Matters" — performance is one of the things that matters in some systems; structure around it.
