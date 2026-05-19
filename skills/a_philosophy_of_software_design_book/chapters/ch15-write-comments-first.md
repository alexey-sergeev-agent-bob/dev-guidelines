# Chapter 15: Write The Comments First (Use Comments As Part Of The Design Process)

## Core Idea
Write the interface comments before you write the code. Comments become a design tool — they force you to define abstractions early, when redesigning is cheap, and they reveal complexity before you've committed implementation effort.

## Frameworks Introduced
- **Comments-first workflow**:
  1. Write the class interface comment.
  2. Write interface comments and signatures for the most important public methods (empty bodies).
  3. Iterate on these until the structure feels right.
  4. Write declarations and comments for the most important instance variables.
  5. Fill in method bodies, adding implementation comments as needed.
  6. As new methods/variables emerge during coding, write their interface comments first too.
- **Comments as a complexity canary**: if a method or variable requires a long, complicated comment, that's a red flag the abstraction isn't right.
- **Three benefits**:
  1. Better comments — written when the design is fresh.
  2. Better designs — abstraction is forced into focus.
  3. More fun — you're solving design problems, not bookkeeping at the end.

## Key Concepts
- **Comments as a measurement**: an interface comment that's short and complete signals a deep abstraction; a long, gnarly comment signals a shallow or muddled one.
- **Compare interface comment vs. implementation**: if the interface comment must restate the major implementation features, the method is shallow.

## Mental Models
- "Comments serve as a canary in the coal mine of complexity." If you can't describe something cleanly, the design is wrong.
- Writing the comment forces you to ask: "What is the essence of this thing?"
- Cost is small (≤5% of dev time); the design feedback you get back is huge.

## Anti-patterns
- **Red Flag — Hard to Describe**: comment is unavoidable but cannot be made simple-and-complete. Probably indicates a design problem.
- **"I'll write the comments after"**: virtually guarantees they don't get written, or are written badly when memory of design decisions has faded.
- **Comments tacked on at the end** to look respectable: shallow, repetitive, miss the important rationale.

## Code Examples
*(No code in this chapter — the workflow itself is the artifact.)*

## Reference Tables

| Step (in order) | Output | Question to ask |
|---|---|---|
| 1. Class interface comment | What this class IS | What's the abstraction users see? |
| 2. Method interface comments + sigs | Empty methods + docs | Can I describe each in a few short lines? |
| 3. Iterate | Adjusted interface | Can the comments be simpler? |
| 4. Instance variables | Field decls + comments | What does each represent (noun)? |
| 5. Method bodies | Implementation + impl comments | What/why does each block do? |
| 6. New methods discovered | Interface comment first | Same as step 2 |

## Key Takeaways
1. Write interface comments first; treat them as design specifications.
2. If you can't write a short, complete comment, you don't have a clean abstraction yet.
3. The act of commenting is also the act of testing your design.
4. Implementation comments come later, while you write the body.
5. Cost is tiny (≤5% of dev time), payback is design clarity for the life of the system.
6. Once you experience this workflow, comment-writing becomes one of the fun parts of programming.

## Connects To
- **Ch 4**: Comments-first surfaces shallow modules early.
- **Ch 12 / Ch 13**: This chapter operationalizes "write good comments" as a design practice.
- **Ch 16**: Comments-first reduces maintenance burden because they're done as you go.
- **Ch 11 (design-it-twice)**: Comments-first plays well with design-it-twice — try multiple sets of interface comments.
