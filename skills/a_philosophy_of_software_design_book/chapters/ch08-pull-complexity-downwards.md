# Chapter 8: Pull Complexity Downwards

## Core Idea
Most modules have more users than developers. As a module developer, take on extra work yourself so callers don't have to. Prefer a simple interface over a simple implementation.

## Frameworks Introduced
- **Pull complexity downward**: when unavoidable complexity exists, handle it inside the module rather than passing it up to every caller.
- **Three conditions** when pulling down is right (8.3):
  1. Complexity being pulled down is closely related to the module's existing function.
  2. Pulling it down simplifies things elsewhere.
  3. Pulling it down simplifies the module's interface.
- **Tempting opposite** ("pass the buck"): throw an exception, expose configuration parameters, demand the caller decide. Easier for the developer; multiplies pain across all callers and admins.

## Key Concepts
- **Configuration parameters**: an excuse to push policy decisions to users/admins who often can't determine the right value. Should be a last resort.
- **Auto-tuning > config**: example — TCP-style retry interval computed from observed RTTs vs. a configurable timeout.
- **Character-oriented vs line-oriented text API**: line-oriented is easier to implement, but pushes splitting/joining work to every UI caller. Character-oriented pulls that complexity down.

## Mental Models
- "Take a little extra suffering upon yourself in order to reduce the suffering of your users."
- If a class throws, every caller pays. If a class exposes config, every installation pays.
- Default to "I'll handle it"; require overrides only for genuinely meaningful options.

## Anti-patterns
- **Configuration parameters as a default solution**: "I don't know what's best, so let the user pick." Often the user doesn't know either.
- **Throwing exceptions to dodge logic**: punts the problem upward without solving it.
- **Line-oriented APIs for editor text**: forces every UI op to split/join.
- **Taking it too far**: pulling everything into one giant class. Pulling down only helps when the complexity is related to the class's existing function.

## Code Examples
*(No new code in this chapter; it builds on Chapter 6's text API example: character-oriented `insert(Position, String)` / `delete(Position, Position)` pulls splitting/joining into the storage class.)*

## Reference Tables

| Question to ask before pulling complexity down | If yes, pull down |
|---|---|
| Is this complexity related to the module's primary job? | Yes |
| Will pulling it down simplify upper-layer code? | Yes |
| Does it simplify the module's interface? | Yes |

| Configuration parameter checklist | If any "no", default it |
|---|---|
| Can the user genuinely know a better value than we can? |
| Is a sensible default impossible? |
| Is the value stable over time? (config drifts; auto-tuning adapts) |

## Key Takeaways
1. It is more important for a module to have a simple interface than a simple implementation.
2. When you find unavoidable complexity, handle it once inside; don't multiply it across callers.
3. Avoid configuration parameters by default — try to compute reasonable values internally.
4. Provide reasonable defaults so users only intervene in exceptional cases.
5. Pulling down is bounded by the three conditions; don't overload one class with unrelated complexity.

## Connects To
- **Ch 4 / Ch 5**: Pulling complexity down deepens modules and improves information hiding.
- **Ch 6**: General-purpose APIs naturally pull complexity downward.
- **Ch 10**: Exception masking is a specific form of pulling complexity downward.
- **Ch 21**: Things that don't matter to users should be hidden — implemented automatically by the module.
