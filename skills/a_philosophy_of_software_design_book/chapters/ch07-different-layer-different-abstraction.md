# Chapter 7: Different Layer, Different Abstraction

## Core Idea
In a well-designed system, every layer presents a different abstraction from the layers above and below it. Adjacent layers with similar abstractions are a red flag that responsibilities are tangled.

## Frameworks Introduced
- **Layered abstraction rule**: as you trace a call up or down the stack, the abstraction must change at every step. Otherwise, that layer isn't earning its keep.
- **Pass-through method**: a method whose only job is to call another method with similar signature. Adds interface, contributes no functionality.
- **Pass-through variable**: an argument that travels through a chain of methods that don't use it, just to reach a deep callee.
- **Decorator pattern (skeptical view)**: encourages API duplication. Tends to produce shallow classes filled with pass-throughs.
- **Context object**: one per system instance, holds all globally-needed state. Replaces both pass-through variables and global variables.

## Key Concepts
- **Dispatcher**: legitimate same-signature method that selects which target to invoke. Provides real functionality (the routing decision).
- **Interface vs implementation rule**: a class's interface representation should differ from its internal representation. Same → shallow.
- **Pass-through variable**: forces every intermediate method to know about a variable they don't use; if you add another such variable, every signature changes again.

## Mental Models
- "Each piece of design infrastructure must eliminate more complexity than it introduces" — pass-through methods, decorators, and pass-through variables fail this test.
- Decorators are tempting because they look modular; in practice they're often a thin layer adding little.
- The context object is "far from ideal" — it has global-variable problems — but better than the alternatives.

## Anti-patterns
- **Red Flag — Pass-Through Method**: method does nothing but call another with the same API. Indicates muddled responsibility between classes.
- **Pass-through variables**: visible in many signatures even though most methods don't use them.
- **Decorator-per-feature** (cf. Java I/O `BufferedInputStream`/`ObjectInputStream`): combines easily with classitis to produce shallow towers.
- **Line-oriented text class API where storage is also line-oriented**: same abstraction at both layers → shallow class.

## Code Examples

Pass-through methods — 13 of 15 public methods just delegated:

```java
public class TextDocument {
    private TextArea textArea;

    public Character getLastTypedCharacter() {
        return textArea.getLastTypedCharacter();
    }
    public int getCursorOffset() {
        return textArea.getCursorOffset();
    }
    public void insertString(String t, int o) {
        textArea.insertString(t, o);
    }
}
```

- **What it demonstrates**: TextDocument and TextArea share the same abstraction; one of them is unnecessary. Fix by collapsing or redistributing.

## Reference Tables

| Pass-through variable solution | Pros | Cons |
|---|---|---|
| Shared object both ends already access | Eliminates threading the variable | The shared object may itself be a pass-through |
| Global variable | No threading needed | Breaks multi-instance, complicates testing |
| **Context object** (preferred) | One place for global state; testable | Behaves like grouped globals; needs discipline |

| Question for a decorator | Action |
|---|---|
| Could it be merged into the underlying class? | Do that (if generally useful) |
| Specialized to one use case? | Merge into use case |
| Could it merge with an existing decorator? | Yes — fewer, deeper decorators |
| Does it really need to wrap? | Often a stand-alone class is better |

## Key Takeaways
1. Different layers should expose different abstractions — sameness signals a tangled responsibility.
2. Pass-through methods betray confused class boundaries; refactor to merge or redistribute.
3. Treat decorators with suspicion; prefer adding the feature to the base class when it's nearly universal.
4. A class's interface should differ from its implementation; line-storage with line-API is shallow.
5. Use a context object instead of pass-through variables or globals — and keep its contents disciplined and ideally immutable.
6. Same-signature methods are fine for dispatchers and multi-implementation interfaces.

## Connects To
- **Ch 4**: Pass-throughs and decorators yield shallow modules.
- **Ch 5**: Pass-through variables are a form of information leakage across layers.
- **Ch 8**: Pull complexity downward instead of pushing it through layers as parameters.
- **Ch 9**: When pass-throughs appear, the "join classes" rule often applies.
