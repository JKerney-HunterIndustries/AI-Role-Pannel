# Vote Schema

This file defines the vote output contract for all role agents in the governance system. Include this file alongside a role file when invoking a role subagent.

## Review Output Rules

- Review the assigned activity only: `requirements`, `design`, `implementation_plan`, or `code_change`. Treat `plan_approval_required` as a separate gate over the reviewed plan revision, not as another activity.
- Emit one vote for the assigned role and activity. Use exactly one vote type and include all four required fields below.
- For every checklist item required by the role, emit exactly one status: `pass`, `fail`, or `not_applicable`, followed by concise evidence. Use `not_applicable` only when the relevant surface is absent from scope, and state what was inspected to confirm that.
- Distinguish observed facts and executed checks from assumptions, proposed behavior, and future verification. Never report a planned check as executed.
- Follow the role's required output sections and ordering. Do not omit required fields; use an explicit empty list or `none` when a section has no findings.
- A role review output is an artifact for gate purposes, not a requirement to create a standalone file. Keep it in the active review context or consolidated review record unless persistence is strictly required by the request or a review gate. Follow Artifact and Note Retention in [agent-governance.md](./agent-governance.md).

## Plan Review Parity

Required roles are active for planning-only requests as well as code-changing requests. Activate specialists from the proposed changes, not merely the plan document's format.

Plans receive the same substantive scrutiny and gate weight as the code changes they propose. For each proposed work unit, reason through the implementation it would produce and evaluate that projected result against the same role criteria, quality bar, and applicable gates that would apply to the resulting code change. Assess the expected code shape, behavior, boundaries, risks, tests, and documentation—not only whether the plan document is complete or internally consistent. Apply the assigned `requirements`, `design`, and `implementation_plan` checks as additional planning-stage checks, not as substitutes for evaluating the proposed implementation outcome. A plan that is too vague to evaluate against those criteria is incomplete and must not pass plan approval.

The evidence standard differs by stage, not the scrutiny: distinguish observed baseline facts and executed checks from assumptions, predicted outcomes, and future verification obligations. Evaluate proposed behavior as a projection; never claim unimplemented behavior or planned checks are already implemented or executed. Cite the human-readable plan revision and work-unit identifiers when they exist; do not use cryptographic fingerprints as revision identifiers unless explicitly requested. Approval tracks substantive plan content, so documented non-material corrections do not require a new vote, while material changes require refreshed review and approval. Plan approval authorizes proposed work but does not certify future code: review the actual implementation against its code-change criteria before accepting it. Follow the plan-approval and handoff rules in [agent-governance.md](./agent-governance.md).

## Vote Types

- `approve` — the change satisfies this role's criteria with no conditions.
- `approve_with_risk` — the change is acceptable but carries identified risk; state the risk and the mitigation or acceptance condition.
- `block` — the change fails this role's criteria; the block must name a concrete condition that would clear it.

## Required Fields

Every vote must include all four fields. Field names and scope values are literal; do not substitute synonyms.

| Field | Purpose |
|-------|---------|
| **Reason** | Why you are voting this way, anchored to your role's authority lens. |
| **Condition** | The specific criterion that determined the vote. For blocks, the condition that would clear it. |
| **Evidence** | Concrete facts, file references, or observations that support the vote. Must be specific enough for independent verification. |
| **Scope** | Where the vote applies: `local` (the immediate change), `task-wide` (multiple areas in this change), or `architectural` (system-level boundaries or ownership). |

## Output Format

```
Vote: <approve | approve_with_risk | block>
- Reason: [concise statement anchored to role lens]
- Condition: [specific criterion; for blocks, what clears it]
- Evidence: [concrete facts or references]
- Scope: local | task-wide | architectural
```
