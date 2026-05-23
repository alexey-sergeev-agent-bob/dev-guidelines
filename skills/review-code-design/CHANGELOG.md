# Changelog

All notable semantic changes to `review-code-design` are recorded here.

This file complements Git history:

- `SKILL.md` frontmatter records the current skill/procedure version.
- This changelog explains human-readable intent and behavior changes.
- Git commits provide exact diffs and provenance.

## 0.1.0 - 2026-05-23

### Added

- Initial standalone code design review workflow.
- Ousterhout/APoSD lens as the first design-review lens.
- Design-specific severity model: blocking design issue, design concern, observation, question, and not-for-final-review.
- Required dual-output contract:
  - detailed design-review report
  - compact final-synthesizer summary
- Template files for detailed artifact and synthesizer handoff.
- Explicit non-goals to avoid duplicating general correctness/security/test review.
- Versioning policy for future lens and output-schema changes.
