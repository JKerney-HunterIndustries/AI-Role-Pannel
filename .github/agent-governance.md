# Agent Governance Details

This file contains the detailed governance mechanics for the AI Role Panel governance pack.

## Request Classification
- `code_change`: implement or modify software behavior. Run the applicable governance activities through `code_change`.
- `planning_only`: create or materially revise a persisted plan for future code changes. Run `requirements`, `design`, and `implementation_plan`, then the separate plan-approval gate; do not implement the planned changes.
- `analysis_only`: analyze, explain, or review without implementing software changes or creating/materially revising a plan. Run only the relevant activities and stop at the highest relevant activity.

## Artifact Revision and Protection
- Use ordinary human-readable revision labels to identify plans and other panel-generated artifacts. Revision labels support traceability; they are not integrity controls.
- Do not add cryptographic hashes, fingerprints, digests, signatures, or equivalent protection to plans, role artifacts, reports, or other panel-generated content unless the user explicitly requests it.
- If unrequested cryptographic metadata is already present, treat it as non-authoritative: never validate it or use a mismatch to block a gate, and remove it when editing that artifact.
- Plan approval applies to the reviewed substantive content, not to an exact byte-for-byte file representation. A typo fix, clarity edit, or correction that does not change approved scope, behavior, constraints, design decisions, dependencies, acceptance criteria, verification obligations, or risk disposition is non-material. Record the correction and carry approval forward; do not block execution or require re-review solely because the file or revision label changed.
- A change to any of those approved substantive elements is material: refresh affected role reviews and obtain plan approval for the revised content before execution.

## Artifact and Note Retention
- Create or persist an artifact only when it is strictly required by the user's request or an applicable panel review gate. Do not create documents merely to hold routine role notes, votes, or intermediate reasoning.
- Required role reviews remain independent outputs, but may be kept in the active conversation or consolidated review record; they do not require one file per role or activity. Record invocation evidence in the required response or review record rather than creating separate files by default.
- Keep only deliverables and evidence needed to satisfy the request or pass the applicable review, such as a requested implementation plan, changed source/tests, or explicitly required verification evidence.
- Before finishing, remove temporary artifacts created by the panel for the current task that are no longer needed. Never remove pre-existing files or user-owned artifacts as part of routine cleanup.

## Specialist Activation Triggers
- UX Expert: user flow or interaction changes, key journey behavior changes, copy, IA, navigation, form behavior, feedback behavior, and accessibility implications.
- Technical Writer: user-visible behavior changes, CLI command changes, UI navigation or workflow changes, setup or run changes, error or status semantics changes, or explicit doc update requests.

## Role Intent Statements
- Action: enforce that each implementation change is one executable, testable, independently releasable unit of value; overarching plans must decompose work into such units.
- Refactoring: prevent local maintainability regressions and avoidable structural debt in the touched area.
- Deletion: prevent avoidable added or retained implementation surface in the touched area.
- Architecture: preserve boundaries, low coupling, and clear ownership.
- SDET: prove behavior via observable evidence and failure coverage.
- Junior: expose hidden assumptions and weak rationale.
- UX: protect clarity, usability, accessibility, and page-purpose coherence.
- Technical Writer: protect user comprehension, task-oriented docs quality, discoverability, and release-note accuracy.

## Role Authority By Activity
Roles may comment broadly, but must vote from their authority lens.

Full role definitions are in the `roles/` directory. When invoking a role sub-agent, include the corresponding file and [vote-schema.md](./vote-schema.md) in the prompt.
All required roles review code-changing and planning-only work. For planning-only work, assess the proposed behavior at `requirements`, `design`, and `implementation_plan`; do not assess unimplemented behavior as `code_change`. Conditional specialists are activated by the triggers below, based on the proposed behavior.

| Role | File | Type |
|------|------|------|
| Action | [roles/action.md](./roles/action.md) | Required |
| Refactoring | [roles/refactoring.md](./roles/refactoring.md) | Required |
| Deletion | [roles/deletion.md](./roles/deletion.md) | Required |
| Architecture | [roles/architecture.md](./roles/architecture.md) | Required |
| Security | [roles/security-role.md](./roles/security-role.md) | Required |
| SDET | [roles/sdet.md](./roles/sdet.md) | Required |
| Junior | [roles/junior.md](./roles/junior.md) | Required |
| UX | [roles/ux.md](./roles/ux.md) | Conditional specialist |
| Technical Writer | [roles/technical-writer.md](./roles/technical-writer.md) | Conditional specialist |

## Speculative Design and Plan Review Policy

The design and implementation plan are treated as provisional evidence, not immutable truth.

- existing design is a baseline for comparison, not a sacred or unchangeable constraint unless current evidence clearly requires it
- the plan artifact is a reviewable governance object and may be revised before implementation begins
- A plan must not be considered finished unless it passes the required role gates.

Creation or material revision of a plan should be reviewed as if it represents the code change it describes.

#### Plan Review Parity
- Plans receive the same substantive scrutiny and gate weight as the code changes they propose. For every proposed work unit, reviewers must reason through the projected implementation and evaluate its expected code shape, behavior, boundaries, risks, tests, and documentation against the same role criteria, quality bar, and applicable gates used for the resulting code change.
- Requirements, design, and implementation-plan checks are additional planning-stage checks; they do not replace review of the proposed implementation outcome. A plan too vague to assess against the applicable code-change criteria is incomplete and cannot pass plan approval.
- The evidence standard differs by stage, not the scrutiny. Label conclusions about unimplemented behavior as predictions, keep executed checks distinct from planned verification, and do not present the plan approval as proof that future code is correct. The later implementation must still pass review of its actual diff and verification evidence.

### Planning-Only Deliverables and Handoffs
- Classify a request to create or materially revise a plan for future code changes as `planning_only`, not `analysis_only`. Run `requirements`, `design`, and `implementation_plan` with all required roles, actual role invocation evidence, and specialists activated by the proposed behavior.
- A plan may coordinate multiple changes performed by different agents in later sessions. Review the full proposed scope, not just the act of writing the plan.
- Persist the plan at an explicit repository or user-designated artifact path. Include enough context for another agent to proceed without the original conversation:
  - plan identifier, human-readable revision, status (draft | blocked | approved), and repository baseline reference
  - objective, scope, non-goals, constraints, assumptions, and relevant requirements/design references
  - material decisions, alternatives, tradeoffs, risks, and evidence that would require revisiting decisions
  - work units with stable identifiers, target modules/interfaces, one outcome per unit, dependencies, execution order, and which units may run in parallel
  - per-unit acceptance criteria, test-first verification paths including failure/edge cases, and applicable security, UX, documentation, rollout, and compatibility obligations
  - role votes, invocation/evidence references, accepted risks, unresolved blocks, and the separate plan-approval decision tied to this revision
- Finish a planning-only request with the persisted plan and approval status. Do not implement the planned changes merely to complete the planning workflow.
- Plan approval approves the proposed work, not future code. Before executing a unit, a later agent must check the approved substantive content, current repository state, dependency completion evidence, and applicable assumptions; then apply the normal code-change governance and acceptance gates to that unit.
- Record completed units and verification evidence for later agents. Material changes to scope, design, dependencies, or verification invalidate approval of the changed work until affected role reviews are refreshed and the plan-approval gate passes for the revised content. Do not silently inherit approval across material revisions.

## Gate Rules

### Activity Gates
- requirements: blocked if Junior, SDET, or Security votes block.
- design: blocked if Architecture, Junior, SDET, or Security votes block.
- implementation_plan: blocked if Action, Junior, SDET, or Security votes block.
- code_change acceptance: blocked if Junior, SDET, or Security votes block.

Required roles and activated specialists must still provide a valid review output for every applicable review activity, even when their vote is not itself a blocking gate at that activity. The output may be transient or included in a consolidated review record; apply the role's stated authority and block-validity rules before counting a block.

### Plan Approval Gate
- `plan_approval_required` is a separate approval checkpoint after implementation_plan review and before execution of planned changes. Passing the implementation_plan activity alone does not satisfy it.
- Blocked while any required role or activated specialist has an unresolved valid block on the plan. Existing role authority, block-validity rules, and evidence requirements apply.
- This gate applies to single-change and overarching multi-change plans, including planning-only deliverables. It uses the collected plan-review votes; duplicate invocations are not required solely because it is a separate checkpoint.
- All required and activated specialist plan-review artifacts and the handoff content above must be present. Record pass | blocked against the human-readable plan revision; missing artifacts cannot produce approval. Non-material corrections may carry that approval forward as described in Artifact Revision and Protection.
- The activity-specific specialist thresholds below do not narrow this all-role plan-approval gate. Other activity and code-change acceptance gates remain unchanged.

### Specialist Gates (When UX Is Activated)
- requirements and design: blocked if UX blocks within activated scope.
- implementation_plan: specialist block is task-wide only if proof path is missing for activated scope.
- code_change acceptance: blocked if UX blocks for severe activated-scope defects.

### Technical Writer Gates (When Activated)
- requirements and design: blocked if required user-doc scope is missing or ambiguous for changed behavior.
- implementation_plan: blocked if no same-change doc update proof path exists.
- code_change acceptance: blocked if user docs are materially stale or incorrect for activated scope.

### Additional Guardrails
- Action block votes for bundled scope must name the primary slice and additional separately shippable slices. For overarching plans, blocks must identify a concrete decomposition, dependency, standalone-value, or verification failure under the Action role contract; the mere presence of multiple work units is not a failure.
- Architecture may block code_change only for true boundary or coupling violations.
- Refactoring and Deletion may block only for concrete maintainability or redundancy failures.
- Deletion may block code_change when commented-out executable code remains in the touched area, unless it is clearly temporary, narrowly scoped, and paired with a TODO that states the removal trigger and cleanup rationale.
- Refactoring block votes at requirements and design must cite a specific structural risk in the current codebase that makes the regression predictable.
- Refactoring block votes at implementation_plan must name the missing or deferred cleanup step.
- Refactoring block votes at code_change must compare the diff to a concrete pre-change baseline in the touched area.
- Refactoring must not block only because the surrounding subsystem is messy; it must show that this change made the touched area worse or preserved avoidable debt that was in scope to fix.
- Refactoring must hard-block new temporal coupling in the touched area unless it is required for the current behavior and explicitly mitigated.
- Deletion block votes at requirements and design must cite the specific existing code that makes the new surface avoidable.
- Deletion block votes at implementation_plan must name the missing reuse or removal step.
- Deletion block votes at code_change must compare the diff to a concrete pre-change baseline and identify specific added or retained surface that could have been removed, reused, or collapsed.
- Deletion block votes for commented-out code must treat the retained comments as avoidable surface unless the code is temporary, under 100 lines, and preceded by a TODO that explains why the code must remain and what condition removes it.
- Deletion must not block only because more cleanup would be nice; it must show that this change introduced or preserved unnecessary surface within scope.
- UX may block code_change only for severe usability or accessibility defects in activated scope, or context-mismatched placement without approved remodel.
- Technical Writer may block code_change only for material user-doc defects in activated scope.
- UX must block design for context-mismatched placement and provide:
  1. misplaced element
  2. current page
  3. correct page
  4. proposed flow remodel across affected pages
- Security fallback votes are provisional until the minimum security artifact contract is satisfied.
- Security must not produce a final approve/block outcome without minimum security artifacts that include executed evidence for material security claims.
- Any block vote is invalid without a clear condition and evidence requirement.
- For planning reviews, distinguish observed facts from proposed behavior. Require named future verification steps for unimplemented behavior, not fabricated passing results. Material claims about existing controls or executed checks still require executed evidence; plan approval does not certify future implementation.

### Security Fallback Minimum Artifact Contract
When distinct role agents are unavailable and Security runs in fallback mode, final Security outcomes require all of the following:
- threat-surface evidence: assets, trust boundaries, privileged actions, and external inputs for touched scope.
- proof-path evidence: named tests/checks/scans for each material security claim.
- executed evidence: at least one executed artifact per material claim about existing security behavior or completed verification with pass/fail outcome. Explicitly proposed, unimplemented behavior instead requires planned execution and provenance capture; it must remain labeled unverified.
- provenance evidence: command or tool identity plus run id, timestamp, or artifact path for each executed artifact.

If any required security fallback artifact is missing:
- Security output is provisional only.
- activity gate status remains blocked until artifacts are collected.

## Overlap Policy
Each role must:
- anchor comments to its authority lens.
- state at least one agreement or disagreement with another role when applicable.
- identify one concern likely underweighted by others.

## Tie Resolution
1. Attempt specialist reconciliation using conditions and evidence.
2. If still tied and no unresolved block remains, allow non-specialized arbiter choice between viable options.
3. Arbiter cannot override specialist block votes.

## User Escalation Policy
Escalate only for unresolved ties or explicit preference-dependent choices.

Do not escalate solely because:
- a gate failed and can be remediated internally.
- role artifacts are missing and can be collected.

Escalation format:
- Decision (1 sentence)
- Why unresolved (1 sentence)
- Option A: supporting roles, benefits, costs and risks
- Option B: supporting roles, benefits, costs and risks
- What would resolve internally (missing constraint or evidence)
- Default if no user preference

## Final Recommendation Completeness
A recommendation is complete only when all are true:
- all required role votes are present for relevant activities.
- all activated specialist votes are present.
- block votes are resolved or escalated.
- approve_with_risk items are documented as accepted risks.
- disagreement resolution is explicit.
- each major decision has at least one proof path.
- security decisions over material claims about existing behavior or completed verification include executed evidence artifacts with provenance; proposed behavior has explicit future verification obligations and is not represented as proven.
- code-changing and planning-only requests include invocation evidence for required and activated specialist roles.
- planning deliverables include a persisted handoff artifact and explicit plan-approval status for its revision; an unresolved block must not be labeled approved.

### Documentation Completeness Requirement (When Technical Writer Is Activated)
- user-facing docs updated in the same change.
- docs include current UI paths and CLI commands for changed behavior.
- docs include at least one end-to-end example task.

## TDD Rules (Detailed)
1. Define tests first.
2. State expected behavior.
3. Implement minimum code to pass.
4. Refactor after passing tests.
5. Re-run tests and report outcomes.

## Response Format (Detailed)
For coding and planning-only tasks, include Role Engagement Record before section 1. For planning-only tasks, report the persisted plan path/revision and separate plan-approval decision; do not report planned tests as executed.

1. Activity and scope
2. Requirements summary (objective, constraints, assumptions, risks)
3. Role votes by activity
4. Gate decision (pass or blocked)
5. Agreement and disagreement summary
6. Tie resolution or user escalation (if needed)
7. Final recommendation
8. TDD plan and implementation steps (if coding is requested)
9. Risk register
10. Agent invocation evidence (required for code-changing and planning-only requests)

### Agent Invocation Evidence Format
- Required and activated specialist roles: role, agent name, role-specific prompt summary, output summary, vote by activity, evidence artifacts, and limitations if fallback.
- Non-activated specialists: role, activation decision (not activated), and one-sentence rationale.

## Stop Conditions
For planning-only requests, stop after delivering the reviewed plan and its approval status; implementation requires an implementation request.

Stop and request user decision only when:
- tie escalation conditions are met.
- tradeoff is user-preference dependent.

Do not stop for internal remediation of:
- gates that can be cleared with technical iteration.
- missing required role artifacts.

If tooling cannot execute required role agents, use best-available fallback per role and escalate only if no deterministic valid vote set can be produced.
