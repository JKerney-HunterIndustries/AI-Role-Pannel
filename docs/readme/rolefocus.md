<!-- (dl (section-meta What Each Role Cares About)) -->

This section helps users quickly understand each role's review lens and likely block conditions.

<!-- (dl (# Required Roles)) -->

1. Action
- Cares most about scope discipline: one independently releasable, testable slice per governed change.
- Looks for bundled work that should be split into separate slices.
- Blocks when a change mixes multiple shippable outcomes or adds preparatory scaffolding not needed now.

2. Refactoring
- Seeks meaningful task-scoped improvements to clarity and maintainability in the touched area.
- Looks for avoidable complexity, duplication, coupling, mixed responsibilities, and unmitigated temporal coupling.
- Compares meaningful local alternatives; the baseline is evidence, not the default winner.
- Blocks for concrete regressions or avoidable retained debt feasible to fix for this task, not general messiness or style preference. Retention must address the alternative's benefit and costs.

3. Deletion
- Seeks simpler task-scoped ways to deliver the outcome by minimizing unnecessary implementation surface.
- Looks for unnecessary new abstractions, duplicate paths, future-only indirection, and obsolete retained code.
- Compares reuse, removal, collapse, and narrower replacement; existing mechanisms are not automatically preferable.
- Blocks when concrete avoidable surface remains without task-specific justification against a feasible in-scope alternative.

4. Architecture
- Cares most about boundaries, ownership, dependency direction, and coupling control.
- Looks for layer drift, cross-boundary reach-through, dependency reversals, and cycle risk.
- Evaluates task-appropriate structure, including justified scoped ownership/rule revisions with affected consumers and verification.
- Blocks for concrete structural failures or missing predictive comparison/revision evidence, not baseline nonconformance alone. At code acceptance, only concrete boundary, dependency-direction, cycle, or unintended-coupling violations can block.

5. Security
- Cares most about exploitability, impact, and control adequacy in the touched scope.
- Looks for concrete attack paths, missing boundary controls, unsafe defaults, and absent verification for security claims.
- Blocks when a plausible high-impact exploit path or mandatory control gap exists without adequate mitigation evidence.

6. SDET
- Cares most about proving behavior with automated tests.
- Looks for testable claims, observable outcomes, failure-path coverage, and edge-case coverage.
- Blocks when behavior changes are not backed by sufficient automated tests or when testability gaps lack a valid exception path.

7. Junior
- Cares most about reasoning quality, cross-stage coherence, and discoverability/understandability for a new developer on the team.
- Preserves checks for hidden assumptions, weak rationale, overconfidence, and drift between requirements, design, plan, and implementation.
- Traces how a newcomer finds where behavior belongs, follows entry-to-outcome flow, and understands names and rationale without unwritten team knowledge.
- Blocks for concrete assumption/coherence or discovery/flow/name/rationale gaps with task/location evidence and a correction path, not unfamiliarity or preferred naming style. Does not take over Architecture, Refactoring, or SDET authority.

<!-- (dl (# Conditional Specialists)) -->

1. UX (activated by workflow, interaction, navigation, copy, or accessibility changes)
- Cares most about discoverability, usability, accessibility, placement, and transition clarity.
- Looks for incoherent flows, hidden entry points, unclear feedback, and inconsistent equivalent flows.
- Blocks when severe usability/accessibility defects exist in activated scope, or when context-mismatched placement is unresolved.

2. Technical Writer (activated by user-visible behavior or documentation-impacting changes)
- Cares most about user comprehension and docs/release-note accuracy.
- Looks for missing same-change docs updates, stale instructions, weak examples, and poor discoverability.
- Blocks when material user-facing docs are missing/inaccurate for changed behavior, or release-note expectations are unmet when applicable.

<!-- (dl (# How To Use This During Review)) -->

When a role blocks, use its lens first to resolve the issue:
1. identify the failed check in that role's contract
2. provide concrete evidence in requirements/design/plan/diff/tests/docs
3. apply the minimum correction needed to clear the role's condition
4. rerun gate evaluation and record remaining risks explicitly
