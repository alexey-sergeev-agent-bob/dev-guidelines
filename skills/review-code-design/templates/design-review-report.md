# Design Review Report Template

Use this template for the full `review-code-design` artifact. This report may contain nuance, lower-confidence observations, alternatives, and reasoning that should not necessarily appear in the final GitHub review.

```markdown
# Code Design Review Report

Reviewer skill: review-code-design v<version>
Active lenses:
- Ousterhout/APoSD lens v<version>

## Verdict

Design verdict: Accept | Accept with design concerns | Request design changes | Insufficient context

One-sentence summary:
- <summary of design impact>

## Scope Inspected

PR/diff:
- <URL or identifier>

Files inspected:
- <changed file>
- <nearby file/call site inspected for context>

Design surface area:
- Public API / module boundary / persistence / error semantics / naming / comments / other

Review limitations:
- <missing context, large diff caveat, unavailable source, etc.>

## Design Impact

Complexity reduced:
- <where the PR simplifies, hides information, removes duplication, clarifies API, etc.>

Complexity introduced or amplified:
- <where the PR adds coupling, interface burden, cognitive load, etc.>

Main design risk:
- <single most important design risk, or "none identified">

## Blocking Design Issues

### 1. <finding title>

Severity: blocking design issue
Lens: <lens and principle>
Introduced by this PR: yes | amplified | pre-existing | unclear
Evidence:
- <specific files/classes/functions/lines>

Consequence:
- <concrete maintainability or complexity cost>

Recommendation:
- <smallest safe PR-sized change>

Alternative design:
- <strategic alternative if different from smallest safe change>

Confidence: high | medium | low

## Design Concerns

### 1. <finding title>

Severity: design concern
Lens: <lens and principle>
Introduced by this PR: yes | amplified | pre-existing | unclear
Evidence:
- <specific evidence>

Consequence:
- <risk/tradeoff>

Recommendation or author question:
- <actionable suggestion or question>

Confidence: high | medium | low

## Observations

- <non-blocking design insight, positive or negative>
- <pre-existing issue worth noting but not for final review>

## Questions for Author

- <question whose answer could change the design verdict>
- <question about alternatives considered, constraints, or ownership>

## Alternatives Considered by Reviewer

### Current design

Pros:
- <pros>

Cons:
- <cons>

### Alternative A: <name>

Pros:
- <pros>

Cons:
- <cons>

### Alternative B: <name>

Pros:
- <pros>

Cons:
- <cons>

Recommended path:
- <current design accepted / smallest safe change / strategic follow-up>

## Lens Notes

Ousterhout red flags checked:
- Information leakage: found | not found | not applicable
- Shallow module: found | not found | not applicable
- Pass-through method/variable: found | not found | not applicable
- Special-general mixture: found | not found | not applicable
- Temporal decomposition: found | not found | not applicable
- Overexposure: found | not found | not applicable
- Vague/hard-to-pick names: found | not found | not applicable
- Comment/design mismatch: found | not found | not applicable
- Error complexity: found | not found | not applicable

## Not for Final Review

These points are intentionally excluded from the final synthesizer summary:

- <too speculative / pre-existing / not actionable / useful only as design note>

## Final Synthesizer Handoff

See `templates/final-synthesizer-summary.md` for the compact version.
```
