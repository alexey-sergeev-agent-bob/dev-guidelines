# Final Synthesizer Summary Template

Use this template for the compact handoff from `review-code-design` to a final review synthesizer. This is not the full design report. It should be short, selective, and suitable for merging with correctness/security/test review signals.

```markdown
# Final Synthesizer Summary — Code Design

Reviewer skill: review-code-design v<version>
Design verdict: Accept | Accept with design concerns | Request design changes | Insufficient context
Confidence: high | medium | low

## Include in Final Review

Include only findings that are concrete, actionable, and likely worth surfacing in the final GitHub review.

### 1. <short finding title>

Recommended final-review severity: blocking | major | minor/question
Lens: <lens and principle>
Why include:
- <short reason this matters to merge confidence or maintainability>

Suggested GitHub wording:
> <compact, neutral, actionable wording suitable for final review>

Evidence to cite:
- <file/function/line if available>

Recommended fix:
- <smallest safe change or author question>

## Do Not Include

Exclude design observations that are speculative, pre-existing, too broad for this PR, or useful only as internal context.

- <finding/observation>: <reason to exclude>
- <finding/observation>: <reason to exclude>

## Interaction with Other Review Threads

- May reinforce correctness/test/security finding: <yes/no + note>
- May conflict with another reviewer: <describe conflict or "none">
- Needs final-synthesizer judgment on: <tradeoff/question or "none">

## Suggested Final Verdict Contribution

From design perspective only:
- Approve: <why>
- Approve with design concerns: <why>
- Request changes: <why>
- Insufficient context: <what context is missing>

Do not treat this as the whole-PR verdict unless the workflow has no other reviewers.
```
