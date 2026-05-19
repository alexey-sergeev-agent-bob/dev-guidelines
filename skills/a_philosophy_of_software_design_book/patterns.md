# Patterns & Techniques — Ousterhout, *A Philosophy of Software Design*

## Deep Module
**When to use**: designing any class, method, subsystem, or service.
**How**: keep the interface small relative to the functionality. Hide internal data structures, algorithms, formats. Prefer one general-purpose method to several specialized ones. Examples: Unix I/O (5 calls, hundreds of thousands of LoC behind), garbage collector (zero interface).
**Trade-offs**: requires more thought up front; "depth" trades off with inheritance hierarchies that proliferate shallow leaves.

## Information Hiding
**When to use**: every module — but the technique itself: identify which design decisions live exclusively inside the module.
**How**: encapsulate format/algorithm/data-structure knowledge so it's invisible to callers. Don't expose internal representations via getters. Provide focused, typed access methods (`getIntParameter` not `getParams().get(name)`).
**Trade-offs**: sometimes a class must grow to absorb the knowledge; sometimes the right move is to combine two classes that shared a fact.

## Define Errors Out Of Existence
**When to use**: an exception case the caller usually doesn't care about (deleting a non-existent variable, slicing past a string's end, deleting an open file in Windows).
**How**: change the API's semantics so the case becomes a no-op or a normal return. `unset` becomes "ensure variable is gone." `substring` clamps. Unix marks for deletion. Python list slices return empty.
**Trade-offs**: lose some explicit error reporting; may catch fewer bugs at the boundary, but reduce overall complexity of handler code.

## Exception Masking
**When to use**: a recoverable lower-level failure shouldn't propagate (e.g., TCP packet loss, NFS server unavailable).
**How**: handle the exception inside the library; retry, fall back, or block.
**Trade-offs**: callers lose visibility — only mask when the caller genuinely doesn't need to know.

## Exception Aggregation
**When to use**: many call sites produce the same conceptual exception, handled the same way (parameter parsing, request validation).
**How**: let the exception propagate up to a single handler near a request boundary; that handler emits the response.
**Trade-offs**: handler must be generic enough to cover all sources; can be combined with attaching descriptive messages to exceptions.

## Just Crash (Fail-Fast)
**When to use**: rare, unrecoverable, indicates a bug (`malloc` fails, internal invariant violated, network socket unopenable in non-server apps).
**How**: print diagnostics and abort; provide a wrapper like `ckalloc` so call sites stay simple.
**Trade-offs**: not appropriate when recovery is essential (replicated storage); simplicity vs. resilience trade.

## Pull Complexity Downward
**When to use**: an unavoidable complexity exists; choose between handling it once inside vs. exposing it to all callers.
**How**: handle inside if (a) closely related to module's existing job, (b) it simplifies upper layers, (c) it simplifies the interface. Prefer auto-tuning over configuration parameters.
**Trade-offs**: can be overdone — pulling unrelated complexity into a class makes it large and incoherent.

## Eliminate Special Cases
**When to use**: code is dotted with `if` checks for boundary conditions.
**How**: design the normal case to subsume edge cases. Empty selection: `start == end`. Empty range yields empty result.
**Trade-offs**: must verify the normal case actually works correctly for the trivial inputs.

## Push Specialization Up (or Down)
**When to use**: code splits cleanly into a general mechanism and special-purpose users of it.
**How**:
- *Up*: keep lower module general (text storage); put specialization in higher layers (UI key-handling).
- *Down*: define a general interface and let many specialized implementations conform (device drivers, History.Action).
**Trade-offs**: upper-layer code grows slightly but is cleaner; the general module stays reusable.

## History/Action (general-purpose undo)
**When to use**: undo/redo across many kinds of state changes.
**How**: a `History` class manages a list of `Action` objects (each with `undo()`/`redo()`), plus fences to group related actions. Each domain (text, selection, cursor) implements its own Action subclass.
**Trade-offs**: more interfaces to define, but eliminates leakage; new undoable types plug in without modifying History.

## Context Object
**When to use**: replacing pass-through variables and globals.
**How**: one object per system instance holds global state. Most major objects keep a reference; new objects receive it via constructor.
**Trade-offs**: groups globals — disciplined naming and ideally immutable contents required to avoid the standard global-variable problems.

## Design It Twice
**When to use**: any major design decision.
**How**: sketch ≥2 radically different alternatives. List pros/cons. Combine or derive a third. Apply at interface, implementation, decomposition.
**Trade-offs**: hours up front; payback in clean implementation for the life of the system.

## Comments-First Workflow
**When to use**: every new class.
**How**: class interface comment → method interface comments + signatures → iterate → instance variable declarations + comments → method bodies + impl comments. Use long/awkward comments as a complexity signal.
**Trade-offs**: requires discipline; pays back in better designs and never having a comment backlog.

## Higher-Level vs Lower-Level Comments
**When to use**: every comment.
**How**:
- *Lower-level → precision*: variable units, bounds, ownership, invariants.
- *Higher-level → intuition*: purpose, strategy, "how we got here".
- Avoid same-level comments — they repeat the code.
**Trade-offs**: requires identifying what each comment is for; harder to write higher-level comments well.

## Cross-Module Documentation (designNotes)
**When to use**: a design decision affects several modules and there's no obvious central place.
**How**: maintain a `designNotes` file with named sections (e.g., "Zombies"). Each affected code site has a one-liner: `// See "Zombies" in designNotes`.
**Trade-offs**: one central source; risk of getting out of sync since it's not adjacent to any one module.

## Single-Branch Critical Path (Performance)
**When to use**: a hot operation must be optimized.
**How**: identify the smallest amount of code in the common case. Introduce a small piece of state (e.g., `availableAppendBytes`) that captures multiple special cases as one zero-test. One `if` at the top dispatches all special cases off the critical path.
**Trade-offs**: extra state to maintain; usually pays for itself many times over (RAMCloud Buffer: 2× faster, 20% smaller).

## Strategic Modification (Boy-Scout Refactoring)
**When to use**: every time you change existing code.
**How**: don't make the smallest possible diff. Look for design improvements you can make while you're here. Aim to leave the system slightly better.
**Trade-offs**: occasional big-refactor friction with deadlines; usually small-and-frequent investments pay back fast.

## Convention Enforcement via Pre-Commit Hooks
**When to use**: low-level rules (line endings, formatting, file headers).
**How**: write a check; reject commits that violate it. Use code review for higher-level conventions.
**Trade-offs**: one-time tooling cost; eliminates mass rework as new developers join.
