---
name: a_philosophy_of_software_design_book
description: Knowledge base from "A Philosophy of Software Design" (2nd ed., 2021) by John Ousterhout. Use when applying Ousterhout's frameworks for fighting complexity, designing deep modules, information hiding, eliminating exceptions, and writing comments as design.
when_to_use: complexity, deep modules, information hiding, information leakage, shallow modules, classitis, modular design, abstraction, define errors out of existence, exception aggregation, exception masking, pull complexity downward, pass-through method, pass-through variable, decorator, design it twice, comments first, write the comments first, strategic programming, tactical programming, technical debt, red flags, choosing names, vague name, temporal decomposition, general-purpose modules, Ousterhout, software design philosophy
allowed-tools: Read Grep
argument-hint: [topic, framework name, or chapter number e.g. ch07]
---

# A Philosophy of Software Design (2nd Edition)
**Author**: John Ousterhout | **Pages/Spine items**: ~32 | **Chapters**: 22 | **Generated**: 2026-05-10

## How to Use This Skill

- **Without arguments** — `/a_philosophy_of_software_design_book` loads the core frameworks below for reference.
- **With a topic** — e.g. `/a_philosophy_of_software_design_book information leakage` → look up the topic in the Topic Index, then read the relevant chapter file.
- **With chapter** — e.g. `/a_philosophy_of_software_design_book ch10` → load that chapter file from `chapters/`.
- **Browse** — ask "what chapters do you have?" to see the full index.

For detail beyond Core Frameworks, read the matching chapter file. Topic Index maps concepts → chapters. Cheatsheet has the principles and red flags lists. Patterns has actionable techniques.

---

## Core Frameworks & Mental Models

### The single subject: complexity
Software design is the fight against complexity. Complexity is anything about a system's structure that makes it hard to understand or modify. It accumulates in many small contributions; sweat the small stuff. **Two causes**: dependencies and obscurity. **Three symptoms**: change amplification, cognitive load, and unknown unknowns (the worst — you can't know what you don't know).

### The investment mindset (Ch 3)
**Strategic programming**, not tactical. Spend 10–20% of dev time on design quality. Tactical programming wins for a few weeks; strategic wins for the life of the project. Crossover: 6–18 months. Working code isn't enough — your goal is a great design that also happens to work.

### Modules should be deep (Ch 4)
**Deep module** = small interface area + large hidden functionality. Cost = interface complexity; benefit = functionality. Maximize benefit / cost. Unix I/O is the canonical deep interface (5 calls hide hundreds of thousands of lines). **Shallow module** = interface complexity ≈ implementation complexity → pure cost. Combat **classitis**: the cult of "small classes."

> **It's more important for a module to have a simple interface than a simple implementation.**

### Information hiding & leakage (Ch 5)
Each module encapsulates a few pieces of design knowledge that don't appear in its interface. **Information leakage** — a design decision reflected in multiple modules — is the most important red flag. Watch for **temporal decomposition** (structuring code by execution order rather than knowledge) — almost always leaks.

### General-purpose is deeper (Ch 6)
Specialization may be the single greatest source of complexity. Aim for **somewhat general-purpose**: implementation reflects today's needs; interface supports several uses. **Push specialization upward** to upper layers (or downward into many drivers behind a generic interface). Eliminate special cases by making the normal case absorb them.

### Different layer, different abstraction (Ch 7)
Same abstraction at adjacent layers signals tangled responsibility. **Pass-through methods** (no work, just delegate) and **pass-through variables** (threaded through methods that don't use them) waste interface and create coupling. Use a **context object** instead of pass-through variables or globals. Treat **decorators** with skepticism.

### Pull complexity downward (Ch 8)
Module developers should suffer so users don't. Hide unavoidable complexity inside the module rather than exposing it through configuration parameters or thrown exceptions. Auto-tune > config parameter when feasible.

### Define errors out of existence (Ch 10)
The complexity of exceptions comes from their handlers. Reduce handlers, in priority order:
1. **Define out of existence** — redesign the API so the case is normal (Tcl `unset`, Unix file delete, Python list slice).
2. **Mask** — handle low (TCP retransmits, NFS hangs).
3. **Aggregate** — one top-level handler for many sources (Web server's getParameter exceptions).
4. **Just crash** — for rare unrecoverable conditions (`malloc` failure, broken invariants).

### Design it twice (Ch 11)
Sketch ≥2 radically different alternatives for any major decision. Compare pros/cons; the answer is often a third design that combines the best of both. Especially hard for smart people whose first idea was always good enough as students — the problems just got harder.

### Comments are design (Ch 12 / 13 / 15)
Comments aren't bookkeeping; they capture abstractions code can't. **Write comments first** — interface comment before method body. A long, awkward comment is a complexity canary. Lower-level comments add **precision** (units, bounds, ownership, invariants); higher-level comments add **intuition** (purpose, strategy). Same-level comments just repeat the code and are noise.

### Choosing names (Ch 14)
Names are documentation. Two properties: **precise** (narrow enough that readers know what it refers to) and **consistent** (same purpose everywhere; never reused for a different purpose). The Sprite `block` bug — same name for two concepts — caused six months of debugging. Booleans should be predicates. If a name is hard to pick, the design may be muddled.

### Decide what matters (Ch 21)
Software design is separating what matters from what doesn't. Structure systems around what matters; minimize and hide the rest. **Emphasize** with prominence, repetition, and centrality. Two failure modes: too many things treated as important (cluttered, shallow design); too few (unknown unknowns). "Good taste" is the trained ability to make this call.

### Top red flags to recognize on sight
- **Information leakage** — same fact in two modules
- **Shallow module** — interface ≈ implementation
- **Pass-through method/variable** — adds interface, no functionality
- **Special-General Mixture** — general mechanism with specialized code embedded
- **Repetition** — missing abstraction
- **Conjoined methods** — can't understand one without the other
- **Vague name** / **Hard to Pick Name** / **Hard to Describe**
- **Comment Repeats Code** / **Implementation Documentation Contaminates Interface**
- **Temporal Decomposition** — structure by time order
- **Overexposure** — common feature forces awareness of rare features
- **Nonobvious Code** — first guess wrong without much thought

---

## Chapter Index

| # | Title | Key Frameworks |
|---|---|---|
| [ch01](chapters/ch01-introduction.md) | Introduction (It's All About Complexity) | Two approaches: eliminate vs encapsulate; red flags |
| [ch02](chapters/ch02-nature-of-complexity.md) | The Nature of Complexity | Dependencies, obscurity; change amplification, cognitive load, unknown unknowns |
| [ch03](chapters/ch03-working-code-isnt-enough.md) | Working Code Isn't Enough | Strategic vs tactical; investment mindset; tactical tornado |
| [ch04](chapters/ch04-modules-should-be-deep.md) | Modules Should Be Deep | Deep modules; classitis; common-case principle |
| [ch05](chapters/ch05-information-hiding.md) | Information Hiding (and Leakage) | Information hiding/leakage; temporal decomposition |
| [ch06](chapters/ch06-general-purpose-modules.md) | General-Purpose Modules are Deeper | Somewhat general-purpose; push specialization up/down; eliminate special cases |
| [ch07](chapters/ch07-different-layer-different-abstraction.md) | Different Layer, Different Abstraction | Pass-through methods/variables; decorators; context object |
| [ch08](chapters/ch08-pull-complexity-downwards.md) | Pull Complexity Downwards | Pulling-down rule; auto-tune over config |
| [ch09](chapters/ch09-better-together-or-apart.md) | Better Together Or Better Apart? | Four signs to combine; conjoined methods; method splitting |
| [ch10](chapters/ch10-define-errors-out-of-existence.md) | Define Errors Out Of Existence | Define out / mask / aggregate / crash |
| [ch11](chapters/ch11-design-it-twice.md) | Design it Twice | Two-alternative design |
| [ch12](chapters/ch12-why-write-comments.md) | Why Write Comments? Four Excuses | Refutations; comments as abstraction |
| [ch13](chapters/ch13-comments-describe-non-obvious.md) | Comments Should Describe Non-Obvious | Lower-level precision; higher-level intuition |
| [ch14](chapters/ch14-choosing-names.md) | Choosing Names | Precise + consistent; Sprite block bug |
| [ch15](chapters/ch15-write-comments-first.md) | Write The Comments First | Comments-first workflow; complexity canary |
| [ch16](chapters/ch16-modifying-existing-code.md) | Modifying Existing Code | Stay strategic; comment maintenance rules |
| [ch17](chapters/ch17-consistency.md) | Consistency | Document/enforce; "When in Rome"; invariants |
| [ch18](chapters/ch18-code-should-be-obvious.md) | Code Should be Obvious | Whitespace; reader expectations; generic containers |
| [ch19](chapters/ch19-software-trends.md) | Software Trends | OOP, agile, unit tests, TDD, design patterns, getters/setters |
| [ch20](chapters/ch20-designing-for-performance.md) | Designing for Performance | Critical path; measure; RAMCloud Buffer |
| [ch21](chapters/ch21-decide-what-matters.md) | Decide What Matters | Leverage; emphasis; minimize what matters |
| [ch22](chapters/ch22-conclusion.md) | Conclusion | Synthesis: complexity above all |

## Topic Index

- **Abstraction** → ch04, ch12, ch13
- **Agile development** → ch19
- **Aggregation, exception** → ch10
- **Anti-patterns / Red flags** → ch04, ch05, ch07, ch09, ch10, ch13, ch14, ch15, ch18, cheatsheet
- **Backspace example (text editor)** → ch06
- **Buffer optimization (RAMCloud)** → ch20
- **Change amplification** → ch02
- **Classitis** → ch04
- **Cognitive load** → ch02, ch07
- **Comments / Comment categories** → ch12, ch13, ch15, ch16
- **Complexity (definition, causes, symptoms)** → ch02
- **Configuration parameters** → ch08
- **Conjoined methods** → ch09
- **Consistency** → ch17, ch14
- **Context object** → ch07
- **Critical path** → ch20
- **Cross-module documentation / designNotes** → ch13, ch16
- **Decorators** → ch07
- **Deep modules** → ch04, ch06
- **Define errors out of existence** → ch10
- **Dependencies** → ch02
- **Design it twice** → ch11
- **Design patterns** → ch17, ch19
- **Different layer, different abstraction** → ch07
- **Dispatcher** → ch07
- **Event-driven programming** → ch18
- **Exception handling** → ch10, ch08
- **Exception masking** → ch10, ch08
- **Generic containers (Pair)** → ch18
- **General-purpose modules / interfaces** → ch06
- **Getters and setters** → ch19, ch04, ch05
- **History/Action (undo)** → ch06
- **HTTP request handling examples** → ch05, ch10
- **Hungarian Notation** → ch14
- **Implementation comments** → ch13
- **Implementation inheritance / OOP** → ch19
- **Information hiding** → ch05, ch04
- **Information leakage** → ch05
- **Interface vs implementation** → ch04, ch07
- **Invariants** → ch17, ch13
- **Investment mindset** → ch03, ch16
- **Java I/O example** → ch04, ch07, ch10, ch21
- **Layered abstractions** → ch07
- **Leverage (designing what matters)** → ch21
- **Maintaining comments** → ch16
- **Modular design** → ch04
- **Names / naming** → ch14, ch18
- **NFS hang behavior** → ch10
- **Obscurity** → ch02, ch18
- **Obvious code** → ch18, ch02
- **Overexposure** → ch05
- **Parnas (information hiding paper)** → ch05
- **Pass-through method** → ch07
- **Pass-through variable** → ch07
- **Performance** → ch20
- **Pulling complexity downward** → ch08, ch10
- **Pulling specialization up/down** → ch06
- **Red flags (full list)** → cheatsheet, ch04, ch05, ch07, ch09, ch13, ch14, ch15, ch18
- **Refactoring** → ch16, ch19
- **Repetition** → ch09
- **Selection example (text editor)** → ch06, ch09
- **Shallow module** → ch04
- **Sprite block bug** → ch14
- **Special cases (eliminating)** → ch06, ch10, ch20
- **Special-General Mixture** → ch09
- **Strategic programming** → ch03, ch16
- **Substring (Java) example** → ch10
- **Subdivision (when to combine vs split)** → ch09
- **Tactical programming / tactical tornado** → ch03
- **Tcl unset example** → ch10
- **Technical debt** → ch03
- **Temporal decomposition** → ch05
- **Test-driven development (TDD)** → ch19
- **Tests, unit / system** → ch19
- **Unix I/O** → ch04
- **Unknown unknowns** → ch02, ch12
- **Vague Name / Hard to Pick Name / Hard to Describe** → ch14, ch15, ch18
- **Whitespace** → ch18
- **"When in Rome"** → ch17
- **Write the comments first** → ch15
- **Zombies (cross-module example)** → ch13

## Supporting Files

- [glossary.md](glossary.md) — every significant term, alphabetical, with chapter reference
- [patterns.md](patterns.md) — actionable techniques (when, how, trade-offs)
- [cheatsheet.md](cheatsheet.md) — verbatim principles list, red flags table, decision tables

---

## Scope & Limits

This skill covers the book content only — Ousterhout's frameworks for designing software to minimize complexity. The book uses Java/C++/C examples for illustration; the principles apply across languages. For hands-on application in your codebase, combine these frameworks with project-specific tools and conventions. For topics beyond this book (specific frameworks, languages, performance tuning at the silicon level), consult related skills or ask Claude directly.
