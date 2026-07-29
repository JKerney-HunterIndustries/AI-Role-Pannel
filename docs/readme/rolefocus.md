<!-- (dl (section-meta What Each Role Cares About)) -->

This section helps users quickly understand each role's review lens and likely block conditions.

<!-- (dl (## Required Roles)) -->

1. Action
- Cares most about scope discipline: one independently releasable, testable slice per governed change.
- Looks for bundled work that should be split into separate slices.
- Blocks when a change mixes multiple shippable outcomes or adds preparatory scaffolding not needed now.

2. Refactoring
- Cares most about maintainability trajectory in the touched area.
- Looks for avoidable complexity, duplication, coupling, mixed responsibilities, and unmitigated temporal coupling.
- Blocks when the change makes local maintainability worse versus baseline or skips in-scope cleanup that should happen now.

3. Deletion
- Cares most about minimizing avoidable implementation surface.
- Looks for unnecessary new abstractions, duplicate paths, future-only indirection, and obsolete retained code.
- Blocks when existing code could be reused/collapsed or when obsolete/duplicate surface remains without strong in-scope justification.

4. Architecture
- Cares most about boundaries, ownership, dependency direction, and coupling control.
- Looks for layer drift, cross-boundary reach-through, dependency reversals, and cycle risk.
- Blocks when the change violates established ownership or architectural dependency rules.

5. SDET
- Cares most about proving behavior with automated tests.
- Looks for testable claims, observable outcomes, failure-path coverage, and edge-case coverage.
- Blocks when behavior changes are not backed by sufficient automated tests or when testability gaps lack a valid exception path.

6. Junior
- Cares most about reasoning quality and cross-stage coherence.
- Looks for hidden assumptions, weak rationale, overconfidence, and drift between requirements, design, plan, and implementation.
- Blocks when key assumptions are unstated or when stage-to-stage logic is inconsistent.

<!-- (dl (## Conditional Specialists)) -->

1. UX (activated by workflow, interaction, navigation, copy, or accessibility changes)
- Cares most about discoverability, usability, accessibility, placement, and transition clarity.
- Looks for incoherent flows, hidden entry points, unclear feedback, and inconsistent equivalent flows.
- Blocks when severe usability/accessibility defects exist in activated scope, or when context-mismatched placement is unresolved.

2. Technical Writer (activated by user-visible behavior or documentation-impacting changes)
- Cares most about user comprehension and docs/release-note accuracy.
- Looks for missing same-change docs updates, stale instructions, weak examples, and poor discoverability.
- Blocks when material user-facing docs are missing/inaccurate for changed behavior, or release-note expectations are unmet when applicable.

<!-- (dl (## How To Use This During Review)) -->

When a role blocks, use its lens first to resolve the issue:
1. identify the failed check in that role's contract
2. provide concrete evidence in requirements/design/plan/diff/tests/docs
3. apply the minimum correction needed to clear the role's condition
4. rerun gate evaluation and record remaining risks explicitly
