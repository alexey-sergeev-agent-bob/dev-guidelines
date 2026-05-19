# Chapter 6: General-Purpose Modules are Deeper

## Core Idea
Specialization may be the single greatest cause of complexity. Make new modules *somewhat general-purpose*: implement only what you need today, but design the interface to support multiple uses. Generality leads to better information hiding and deeper modules.

## Frameworks Introduced
- **"Somewhat general-purpose"**: functionality reflects today's needs, but the interface does not. The interface is general enough for multiple uses without being so abstract it's hard to use.
- **Push specialization upward** (most common): keep the lower-level module general-purpose; let upper layers handle specific features (e.g., text class is general; UI layer maps "backspace" to a generic delete).
- **Push specialization downward** (less common): when many specific implementations must conform to one interface — e.g., device drivers implementing a generic block-device API.
- **Three diagnostic questions** (6.5):
  1. What is the simplest interface that covers all my current needs?
  2. In how many situations will this method be used?
  3. Is this API easy to use for my current needs?
- **Eliminate special cases** in code by designing the normal case to subsume edge cases (e.g., empty selection has start == end; no separate "no selection" branch needed).

## Key Concepts
- **Specialization** (in interfaces): methods tailored to specific UI features (`backspace`, `deleteSelection`) rather than primitive operations.
- **False abstraction**: a method like `backspace()` that pretends to hide details readers actually need to know.
- **General-purpose interface**: small set of primitives composable for many use cases.
- **Special-General Mixture**: code combining a general-purpose mechanism with code specialized for one of its uses.

## Mental Models
- "Even if you use a class in a special-purpose way, it's less work to build it in a general-purpose way."
- General-purpose interfaces tend to be smaller (fewer methods) AND deeper (each method does more).
- A method designed for a single use case is a red flag — see if a general-purpose method could replace several.
- If you find yourself writing lots of code in higher layers to use the abstraction, the abstraction is too general (too primitive).

## Anti-patterns
- **One method per UI feature**: e.g., `backspace`, `delete`, `deleteSelection` all in the text class — leaks UI knowledge into storage.
- **State variables for special cases**: a `hasSelection` boolean leads to checks everywhere; better to make the empty selection a normal case.
- **Reflecting external concepts in lower-level modules** (Cursor, Selection types in a text class).

## Code Examples

Bad — UI-specialized text API leaks UI knowledge into storage:

```java
void backspace(Cursor cursor);
void delete(Cursor cursor);
void deleteSelection(Selection selection);
```

Good — general-purpose primitives, UI behavior implemented in the UI layer:

```java
void insert(Position position, String newText);
void delete(Position start, Position end);
Position changePosition(Position position, int numChars);

// UI: delete key
text.delete(cursor, text.changePosition(cursor, 1));
// UI: backspace key
text.delete(text.changePosition(cursor, -1), cursor);
```

- **What it demonstrates**: One general delete + position arithmetic replaces three specialized methods, hides nothing the UI actually needs to know, and is reusable.

The History/Action undo design separates a general-purpose history from special-purpose actions:

```java
public class History {
    public interface Action {
        void redo();
        void undo();
    }
    void addAction(Action action);
    void addFence();
    void undo();
    void redo();
}
```

- **What it demonstrates**: Three orthogonal layers — general history mechanism, specific action implementations, grouping policy — each independent.

## Reference Tables

| Decision | Specialized | General-purpose | Winner |
|---|---|---|---|
| Method count | Many (one per feature) | Few primitives | General |
| Information leakage | Common | Rare | General |
| Code in upper layers | Less per call site | Slightly more | General overall |
| Reuse for new uses | None | Often | General |
| Risk | Tight coupling, leakage | Over-generalization | Use "somewhat" general |

## Key Takeaways
1. Build modules **somewhat general-purpose** — interface generic, implementation only what you need.
2. Push specialization **up** to upper layers (or down into drivers when many implementations must conform).
3. Generality reduces method count AND deepens each method — better information hiding for free.
4. A method used in only one place is a red flag for over-specialization.
5. Eliminate special cases by designing the normal case to handle them (empty selection, zero-length copy).
6. Ask: "What's the simplest interface covering my current needs?"

## Connects To
- **Ch 4**: General-purpose interfaces produce deeper modules.
- **Ch 5**: Generality leads to better information hiding (UI details stay in UI).
- **Ch 9**: Separate general-purpose code from special-purpose code.
- **Ch 10**: Eliminating special cases reduces exception handling complexity.
- **Ch 19.2/19.4**: Agile/TDD tendency to "build minimum, refactor later" — author argues for designing the abstraction once it's needed.
