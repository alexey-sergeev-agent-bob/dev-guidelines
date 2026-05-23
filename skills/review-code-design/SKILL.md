---
name: review-code-design
description: "Use when reviewing a code diff or pull request specifically for software design quality: complexity, abstraction boundaries, information hiding, API depth, naming, comments, error semantics, and maintainability tradeoffs. Produces a detailed design-review artifact plus a compact final-synthesizer handoff."
version: 0.1.0
author: Alexey Sergeev / Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [code-review, design-review, software-design, complexity, pull-request-review, maintainability]
    related_skills: [a_philosophy_of_software_design_book, backend-code-review]
  review_code_design:
    schema_version: 1
    lenses:
      - id: ousterhout
        name: A Philosophy of Software Design
        version: 0.1.0
        file: lenses/ousterhout.md
    templates:
      detailed_report: templates/design-review-report.md
      synthesizer_summary: templates/final-synthesizer-summary.md
---

# Review Code Design

## Overview

Use this skill to review a code diff or pull request through a **software design lens**, not as a general correctness/security/test reviewer. The primary question is whether the change reduces or increases long-term complexity: module boundaries, information hiding, API depth, naming precision, comment quality, error semantics, and maintainability tradeoffs.

This skill is designed to work both standalone and as a parallel specialist thread in a multi-review workflow. When used with a final synthesizer, produce two outputs:

1. A detailed design-review report using `templates/design-review-report.md`.
2. A compact final-synthesizer summary using `templates/final-synthesizer-summary.md`.

The initial lens is Ousterhout's *A Philosophy of Software Design* via `lenses/ousterhout.md` and the source skill `a_philosophy_of_software_design_book`. Future lenses may be added without changing the core review contract.

## When to Use

Use this skill when:

- Reviewing a GitHub PR, patch, diff, branch, or design-affecting code change.
- The user explicitly asks for a design review, architecture review, maintainability review, complexity review, or Ousterhout-style review.
- The change touches public APIs, module boundaries, service boundaries, persistence contracts, domain abstractions, error handling, configuration, naming, or cross-cutting flows.
- A normal reviewer already checked correctness/tests and a separate design-focused perspective is needed.
- The review result will feed a final synthesizer or PR-review workflow.

Do **not** use this as the primary tool for:

- Security review, unless the issue is directly caused by design/interface exposure.
- Pure test review, unless the tests reveal design seams, fixture complexity, or missing abstraction.
- Formatting/style-only review.
- Line-by-line bug hunting with no design implications.
- Posting GitHub comments directly. This skill produces findings/artifacts; the surrounding workflow decides what to post.

## Inputs

Expected input is one of:

- PR URL.
- `owner/repo#number`.
- Local branch/diff path.
- Patch text.
- A design proposal plus affected files.

Optional modifiers:

- `--lens ousterhout` — currently the default and only built-in lens.
- `--strict` — include only concrete design issues worth blocking or highlighting in final review.
- `--synthesizer-only` — produce only the compact final-synthesizer summary after doing the review internally.
- `--rereview` — inspect prior comments and avoid repeating resolved design findings.

## Core Review Principle

Review the **change in design pressure**, not whether the code matches a book perfectly.

A valid design finding must include:

- **Evidence** from the PR or nearby code.
- **Design principle/lens** that explains the risk.
- **Concrete consequence** such as change amplification, cognitive load, unknown unknowns, interface burden, or future coupling.
- **Actionable alternative** or a clear author question.

Do not raise findings that are only philosophical preferences. Do not demand large refactors unless the current PR introduces concrete design harm or the refactor is the smallest safe way to remove the harm.

## Review Workflow

### Step 1 — Gather PR and diff context

Use `gh` when available:

```bash
gh pr view <PR> -R <OWNER/REPO> --json title,body,author,baseRefName,headRefName,files,additions,deletions,commits,reviews,comments
gh pr diff <PR> -R <OWNER/REPO>
```

Read:

- PR title/body and linked ticket if available.
- Changed files and diff size.
- Commit messages if they reveal intent.
- Existing review conversation on re-review.
- Nearby code needed to evaluate boundaries and call sites.

Design claims often require more than the diff. Inspect surrounding files before asserting information leakage, shallow modules, or bad abstraction boundaries.

### Step 2 — Classify design surface area

Identify which areas the PR changes:

- Public API, DTO, event, schema, config, or wire contract.
- Module/class/function boundary.
- Domain model or vocabulary.
- Error semantics and exception handling.
- Persistence or transaction boundary.
- Integration boundary.
- Cross-cutting concern such as auth, logging, retries, metrics, idempotency.
- Tests/fixtures that expose design seams.

If the PR has little design surface area, say so and keep the report short.

### Step 3 — Apply active lenses

Start with `lenses/ousterhout.md`. If the full source skill `a_philosophy_of_software_design_book` is available, use it for deeper context. Otherwise use the compact lens file.

Look especially for:

- Information leakage: the same design fact reflected in multiple modules.
- Shallow modules: interface cost roughly equals implementation benefit.
- Pass-through methods or variables.
- Special-general mixture: specialized cases embedded in general mechanisms.
- Temporal decomposition: structure mirrors execution order rather than knowledge ownership.
- Overexposure: common users forced to understand rare cases.
- Error cases that could be defined out of existence, masked, aggregated, or intentionally crashed.
- Names that are vague, inconsistent, or hard to pick because the design concept is muddled.
- Comments that repeat code, expose implementation through interfaces, or reveal awkward abstractions.
- Missed "design it twice" opportunities for major design choices.

### Step 4 — Separate introduced vs pre-existing design issues

Classify every issue as:

- **introduced by this PR** — eligible for inclusion in final review.
- **amplified by this PR** — eligible if the PR makes the cost worse.
- **pre-existing** — mention only if it blocks understanding or the PR depends on it.
- **unclear** — ask a question; do not present as fact.

Do not block a PR for unrelated legacy design debt unless the PR relies on or worsens it.

### Step 5 — Judge severity

Use this design-specific severity model:

- **blocking design issue** — concrete maintainability harm introduced/amplified by the PR; likely to cause change amplification, hidden coupling, invalid API semantics, or repeated future bugs. Must include evidence and a feasible alternative.
- **design concern** — real design risk or complexity increase, but tradeoff may be acceptable or fix may be larger than this PR.
- **observation** — useful note about design direction, not a merge blocker.
- **question** — author context needed before judging.
- **not for final review** — insight useful for the artifact but too speculative, too broad, or not introduced by this PR.

A finding should not be blocking only because it violates a named principle. It blocks only when the violation creates concrete cost for this codebase.

### Step 6 — Propose alternatives

For every blocking issue and major design concern, include at least one alternative:

- **Smallest safe change** — the minimum PR-sized adjustment.
- **Strategic design** — a better long-term shape if different from the minimum fix.
- **Accept current design** — when the complexity is intentional and justified.

When a major design decision has only one proposed solution, explicitly ask whether a second design was considered.

### Step 7 — Produce two outputs

Always produce both outputs unless the user asks otherwise:

1. **Detailed Design Review Report** using `templates/design-review-report.md`.
2. **Final Synthesizer Summary** using `templates/final-synthesizer-summary.md`.

The detailed report preserves nuance and uncertainty. The synthesizer summary is ruthless: only include findings that might belong in the final GitHub review.

## Output Rules

- Prefer concise GitHub-style Markdown.
- Use concrete file/function/class names and line references when available.
- Every finding must say whether it is blocking, concern, observation, or question.
- Every blocking finding must include evidence, consequence, and an actionable alternative.
- Keep book references compact: `Lens: Ousterhout — Information Leakage`, not long quotations.
- Do not over-post philosophy. Convert design principles into codebase-specific consequences.
- If no meaningful design issues are found, say that explicitly and provide a short rationale.

## Relationship to Other Review Skills

This skill is complementary to general code-review skills such as `backend-code-review`:

- General reviewer: correctness, safety, tests, migrations, contracts, operational risk.
- Cross-examiner: pressure-tests the review for false positives/negatives.
- `review-code-design`: design pressure, complexity, abstraction boundaries, and maintainability tradeoffs.
- Final synthesizer: decides what is worth posting and merges signals from all reviewers.

When feeding a final synthesizer, do not attempt to own the final verdict for the whole PR. Provide a design-specific verdict and compact handoff.

## Versioning

This skill uses semantic versioning for review behavior and output contract:

- Patch version: wording, examples, clarifications, non-semantic template edits.
- Minor version: new lens, new output field, changed severity interpretation, workflow step added.
- Major version: incompatible output schema or materially different review philosophy.

Maintain `CHANGELOG.md` for human-readable semantic history. Git history provides exact diffs and provenance.

## Common Pitfalls

1. **Becoming a generic reviewer.** Do not duplicate correctness/security/test review unless the issue is caused by design complexity.
2. **Book-principle purity.** A design smell is not a finding until it creates concrete cost in this PR.
3. **Diff-only overconfidence.** Design claims require nearby code, call sites, and ownership boundaries.
4. **Blocking on legacy debt.** Only block on debt introduced or materially amplified by the PR.
5. **Overloading the final review.** Keep the full artifact detailed, but make the synthesizer summary short and selective.
6. **No alternative.** A design finding without a plausible alternative is often just a complaint.
7. **Ignoring author context.** Domain constraints can justify complexity; ask questions when intent is unclear.

## Verification Checklist

Before finalizing a design review:

- [ ] PR intent and changed design surface area are understood.
- [ ] Nearby code/call sites were inspected for boundary claims.
- [ ] Each finding is classified as introduced, amplified, pre-existing, or unclear.
- [ ] Each blocking issue has evidence, consequence, and a feasible alternative.
- [ ] Findings are mapped to a lens only when the mapping clarifies the issue.
- [ ] The detailed report follows `templates/design-review-report.md`.
- [ ] The final handoff follows `templates/final-synthesizer-summary.md`.
- [ ] The synthesizer summary excludes speculative or non-actionable observations.
