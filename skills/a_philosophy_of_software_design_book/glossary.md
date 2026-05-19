# Glossary — Ousterhout, *A Philosophy of Software Design*

**Abstraction** — A simplified view of an entity that omits unimportant details. The word "unimportant" is critical; if a detail matters, omitting it makes the abstraction false. (Ch 4)

**Aggregation, exception** — Handling many exceptions in one place rather than at each throw site. (Ch 10)

**Back-door leakage** — Information leakage when two modules share knowledge of, e.g., a file format, even if neither exposes it via interface. More dangerous than interface leakage. (Ch 5)

**Change amplification** — Symptom of complexity: a single conceptual change requires modifications in many places. (Ch 2)

**Classitis** — The mistaken view that "if classes are good, more classes are better." Produces shallow classes whose accumulated interfaces overwhelm the system. (Ch 4)

**Cognitive load** — Symptom of complexity: how much information a developer must absorb to complete a task. Not the same as line count. (Ch 2)

**Comments-first** — Writing the interface comment for each class/method before the body, using the comment as a design tool and a complexity canary. (Ch 15)

**Complexity** — Anything about a system's structure that makes it hard to understand or modify. Practically defined and incremental. (Ch 2)

**Conjoined methods** — Methods you can't understand independently — a red flag indicating a bad split. (Ch 9)

**Consistency** — Doing similar things in similar ways and dissimilar things differently. Source of cognitive leverage. (Ch 17)

**Context object** — A per-instance object holding system-global state, used to avoid pass-through variables and global variables. (Ch 7)

**Critical path** — The smallest amount of code executed for the most common case in a hot operation. Designs should be structured around it. (Ch 20)

**Cross-module comment** — Documentation of a design decision affecting multiple modules; placed where developers will encounter it (or in a `designNotes` file). (Ch 13)

**Decorator** — Wrapper class extending an underlying class. Often shallow and pass-through-heavy; treat with skepticism. (Ch 7)

**Deep module** — A module with a small interface and large hidden functionality. The most important property of a good module. (Ch 4)

**Define errors out of existence** — Redesign the API so an error condition is no longer an error. The best technique for reducing exception complexity. (Ch 10)

**Dependency** — A relationship where code can't be understood or modified in isolation; one of the two root causes of complexity. (Ch 2)

**Design it twice** — Sketch at least two radically different designs for any major decision before choosing. (Ch 11)

**Design pattern** — Generally-accepted solution to a common design problem. Useful when applicable; harmful when forced. (Ch 19.5)

**Dispatcher** — Method that uses arguments to select another method to invoke. Same-signature delegation is fine here because the dispatcher provides real functionality. (Ch 7)

**Exception masking** — Detecting and handling an exception at a low level so higher levels never see it. (Ch 10)

**False abstraction** — An abstraction that omits details users actually need. Looks simple, isn't. (Ch 4 / Ch 6)

**Fence** (History) — A marker in a history list that groups undo actions; used in the History/Action design. (Ch 6)

**General-purpose module** — A module whose interface supports many uses, even if its implementation only handles current needs. Generally deeper than special-purpose modules. (Ch 6)

**Getters/setters** — Public accessor methods. Usually a smell — better to not expose instance variables. (Ch 19.6)

**Hard to Describe** — Red flag: a method/variable cannot be given a simple-and-complete comment. Suggests design problem. (Ch 15)

**Hard to Pick Name** — Red flag: cannot find a precise short name for a variable or method. (Ch 14)

**Implementation comment** — A comment inside method bodies describing what (or why) a block of code does, not how. (Ch 13)

**Implementation inheritance** — Inherits both signatures and default implementations. Creates dependencies between parent and subclasses; use cautiously. (Ch 19.1)

**Information hiding** — Encapsulating design decisions inside a module so they don't appear in the interface. Most important technique for deep modules. (Ch 5)

**Information leakage** — A design decision reflected in multiple modules. The most important red flag in software design. (Ch 5)

**Interface (formal vs informal)** — Formal: signatures the language enforces. Informal: behavior, ordering, side effects, preconditions; only describable in comments. (Ch 4)

**Interface inheritance** — Inheriting signatures only; multiple impls. Provides depth via reuse. (Ch 19.1)

**Investment mindset** — Trade short-term effort for long-term productivity; spend 10–20% of dev time on design. (Ch 3)

**Invariant** — A property of a variable or structure always true. Reduces special cases. (Ch 17)

**Modular design** — Decomposing a system into relatively independent modules, each with an interface and an implementation. (Ch 4)

**Nonobvious Code** — Red flag: meaning/behavior cannot be understood at a glance; reader is missing important info. (Ch 18)

**Obscurity** — One of the two causes of complexity: important information is not obvious (vague names, missing units, hidden conventions). (Ch 2)

**Obvious code** — Code where a reader's first guess is correct without much thought. Reader-defined. (Ch 18)

**Overexposure** — Red flag: API for a common feature forces users to learn rarely-used features. (Ch 5)

**Pass-through method** — A method that does little except call another with the same signature; usually a red flag of muddled responsibility. (Ch 7)

**Pass-through variable** — A parameter passed through a chain of methods that don't use it, just to reach a deep callee. (Ch 7)

**Promote (errors)** — Handle a smaller error by escalating to a larger, already-supported recovery (RAMCloud crashes a server on object corruption). (Ch 10)

**Pull complexity downwards** — Handle unavoidable complexity inside the module rather than passing it up to all callers. (Ch 8)

**Red flag** — A recognizable symptom that code is more complicated than necessary. (Ch 1; recurring)

**Repetition** — Red flag: same code appears multiple times — the right abstraction is missing. (Ch 9)

**Shallow module** — A module whose interface is nearly as complex as its implementation. Doesn't earn its keep. (Ch 4)

**Somewhat general-purpose** — Functionality reflects today's needs; interface is general enough for several uses. The recommended sweet spot. (Ch 6)

**Special-General Mixture** — Red flag: a general-purpose mechanism contains code specialized for a particular use of it. (Ch 9)

**Strategic programming** — Mindset where the primary goal is great long-term system design, not just shipping the current feature. (Ch 3)

**System tests / Integration tests** — Tests that exercise the whole application, often run by QA. (Ch 19.3)

**Tactical programming** — Mindset focused on getting the next feature done quickly; accumulates technical debt. (Ch 3)

**Tactical tornado** — Prolific tactical developer who pumps out features fast and leaves messes behind. (Ch 3)

**TDD (Test-Driven Development)** — Writing failing tests before code. Author critical of it as a default workflow; recommends it for bug fixes. (Ch 19.4)

**Temporal decomposition** — Red flag: structuring code by execution order ("first read, then parse, then write") rather than by knowledge. Causes leakage. (Ch 5)

**Unit test** — Small test focused on a single method or class. Strongly endorsed because it enables refactoring. (Ch 19.3)

**Unknown unknowns** — Worst symptom of complexity: you can't even tell what code/info you need to consider for a change. (Ch 2)

**Vague Name** — Red flag: a name so generic it doesn't convey the entity's meaning. (Ch 14)

**"When in Rome"** — When working in a new file or area, follow whatever conventions are already in place. (Ch 17)
