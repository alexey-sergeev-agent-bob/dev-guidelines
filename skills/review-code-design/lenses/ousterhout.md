# Ousterhout Lens — A Philosophy of Software Design

Version: 0.1.0
Source skill: `a_philosophy_of_software_design_book`
Primary source: John Ousterhout, *A Philosophy of Software Design*, 2nd edition.

This lens adapts Ousterhout's design principles to code and PR review. Use it to reason about complexity, not to enforce book terminology mechanically.

## Core Question

Did this change make the system easier or harder to understand and modify?

Evaluate through Ousterhout's complexity model:

- **Dependencies** — when one part cannot be understood or changed without another.
- **Obscurity** — when important information is not obvious.
- **Change amplification** — small conceptual change requires many code changes.
- **Cognitive load** — reader must keep too many facts in mind.
- **Unknown unknowns** — the worst symptom: readers do not know what they need to know.

## High-Signal Review Checks

### 1. Information Hiding and Leakage

Look for design facts duplicated across modules:

- format, protocol, schema, status, enum, workflow, validation rule, retry policy, or persistence detail known in multiple places;
- callers forced to know internal representation;
- getters exposing structures that should be behind behavior;
- tests that must duplicate internal state transitions to use the API.

Valid finding shape:

```text
Lens: Ousterhout — Information Leakage
Evidence: <same design fact appears in A and B>
Consequence: <future change requires touching both / risk of drift>
Alternative: <centralize fact / hide behind interface / combine modules>
```

Do not flag harmless duplication of stable public constants or intentional protocol definitions without concrete drift/change cost.

### 2. Deep vs Shallow Modules

A deep module has a small/simple interface that hides significant functionality. A shallow module has interface cost roughly equal to implementation benefit.

Look for:

- wrapper methods/classes that mostly pass through calls;
- APIs with many knobs for uncommon cases;
- several specialized methods where one general method would be clearer;
- new abstractions that callers must understand but that hide little.

Do not object to small wrappers used for clear dependency inversion, test seams, compatibility boundaries, or framework integration unless the wrapper adds avoidable cognitive load.

### 3. Different Layer, Different Abstraction

Adjacent layers should not simply restate the same abstraction.

Look for:

- service → manager → helper chains with the same method names and signatures;
- DTOs copied across layers without adding meaning;
- layer boundaries that mirror execution order rather than knowledge ownership.

Potential alternatives:

- remove pass-through layer;
- move behavior to the layer that owns the knowledge;
- collapse conjoined methods/classes;
- introduce a deeper interface that hides the lower-level concept.

### 4. Pass-Through Variables and Context

Pass-through variables are threaded through functions/classes that do not use them. They couple unrelated code to downstream needs.

Look for:

- parameters added to many signatures only for one leaf call;
- configuration objects passed through modules that do not own configuration;
- request/user/context values exposed to low-level code without a clear abstraction.

Alternatives:

- introduce a disciplined context object near the system boundary;
- move responsibility to the module that owns the data;
- aggregate related parameters into a cohesive concept;
- invert control so the leaf dependency is configured once.

Do not recommend global state as the default fix.

### 5. Pull Complexity Downward

Module implementers should absorb complexity when doing so simplifies many callers and fits the module's responsibility.

Look for APIs that force every caller to:

- handle the same exception;
- supply rarely changed tuning parameters;
- remember call ordering;
- perform validation that the callee could own;
- know transport/persistence/retry details.

Alternative: hide the complexity inside the module, provide safe defaults, auto-detect/auto-tune, or aggregate handling at a boundary.

Guardrail: do not pull unrelated responsibilities into a module just to hide them. A deeper module is not a god object.

### 6. Define Errors Out of Existence

Exception handling creates complexity mostly through handlers. Prefer reducing distinct exceptional cases when the caller does not need to distinguish them.

Review priorities:

1. **Define out of existence** — make harmless edge cases normal API behavior.
2. **Mask** — handle recoverable low-level failure inside the module.
3. **Aggregate** — use one boundary handler for many similar cases.
4. **Crash/fail fast** — for rare unrecoverable invariant violations.

Flag when a PR introduces broad caller-side exception handling for cases that could have simpler semantics.

Do not recommend masking failures that callers genuinely need for business decisions, audit, data integrity, or user-visible recovery.

### 7. Special-General Mixture

Specialized behavior embedded inside a general mechanism makes both harder to understand.

Look for:

- generic services with named checks for one product/customer/provider;
- framework-level code importing domain-specific types;
- plugin/driver architecture where the central mechanism knows details of individual plugins;
- feature-specific flags leaking into reusable utilities.

Alternatives:

- push specialization upward to callers;
- push specialization downward behind a generic interface;
- use strategy/driver/action objects;
- split the general mechanism from policy decisions.

### 8. Names as Design Signals

Names are part of the design. Hard-to-pick or vague names often reveal unclear concepts.

Look for:

- generic names like `data`, `info`, `handler`, `manager`, `processor`, `context` without precise role;
- same word used for different concepts;
- boolean names that are not predicates;
- names that describe implementation rather than abstraction;
- long awkward names compensating for muddled boundaries.

A naming finding is strong when renaming alone is insufficient because the underlying concept or boundary is unclear.

### 9. Comments as Design

Good comments capture information code cannot express: purpose, invariants, strategy, units, ownership, and non-obvious constraints.

Look for:

- interface comments exposing implementation details;
- comments repeating adjacent code;
- missing invariant/unit/bounds/ownership notes;
- long awkward comments that suggest the abstraction itself is hard to describe;
- comments that would have revealed a better interface if written first.

Do not demand comments for obvious code. Prefer comments that clarify abstraction over comments that narrate implementation.

### 10. Design It Twice

For major decisions, look for evidence that alternatives were considered. This matters when the PR introduces:

- a new public API;
- a new service/module boundary;
- persistence model or migration strategy;
- error-handling policy;
- cross-cutting mechanism;
- irreversible contract.

If only one design appears and the tradeoff is non-obvious, ask for alternatives or propose one. Do not require extensive redesign for small, easily reversible changes.

## Blocking Threshold

A lens finding may be blocking only when all are true:

- The issue is introduced or materially amplified by this PR.
- Evidence is concrete, not just taste.
- The consequence is likely: change amplification, coupling, hidden behavior, brittle API, or repeated future bugs.
- A feasible PR-sized alternative exists, or the current design prevents safe review.

Otherwise classify as design concern, observation, or question.

## Preferred Finding Format

```markdown
### <short finding title>

Severity: blocking design issue | design concern | observation | question
Lens: Ousterhout — <principle/red flag>
Introduced by this PR: yes | amplified | pre-existing | unclear
Evidence: <specific files/classes/functions/behavior>
Consequence: <why this increases complexity>
Recommendation: <smallest safe change or author question>
Alternative design: <optional strategic shape>
Confidence: high | medium | low
```

## Anti-Patterns in Using This Lens

- Quoting the book instead of explaining codebase-specific consequences.
- Treating every shallow wrapper as bad without checking its architectural role.
- Treating every generalization as good; premature generality can also increase complexity.
- Demanding a large refactor when a small interface change would remove the actual cost.
- Using design language to disguise a style preference.
