# Mediator/Orchestrator Role Ideas

> Exploratory notes only. Not current policy; existing governance remains unchanged.

## Discussion Anchor

Recorded: 2026-10-27
Baseline branch: `constitutional`
Baseline commit: `c1e532b4f580da5aa47c9ec45c5249a5b6157608`

This note records exploratory role boundaries discussed against that baseline; it does not change current governance behavior. The related candidate voting-system changes are listed in [law-driven-governance.md](./law-driven-governance.md).

## Purpose

The mediator/orchestrator is the process authority for the jury-of-judges task. It frames the matter, coordinates the panel, applies the collaboration rules, and determines whether votes are valid under the applicable laws and role contracts.

## Panel context

### Shared context packet (Candidate)

Give every role the same shared context packet for the review. Identify the matter and scope without duplicating all reviewed materials; roles must be able to access the actual in-scope materials. Give each role its role contract and role-specific assignment separately from the shared factual context.

Candidate packet fields:

- **User request and outcome sought:** the request and intended outcome. Whether to include the original request verbatim in every case or provide a faithful concise rendering remains open; preserve the exact wording where it may affect scope.
- **Decision requested:** what the panel is being asked to approve or block in this loop.
- **Scope under review:** the included code, plan, artifacts, or other materials; the state or revision being reviewed; and explicit exclusions. This may cover multiple files, the full codebase, or another defined set.
- **User-stated requirements and constraints:** attributed to the user.
- **Relevant facts and evidence:** identified with sources or references so roles can inspect the underlying materials.
- **Assumptions:** labeled explicitly and not presented as established facts.
- **Known unknowns:** information not established in the current context.
- **Mediator interpretation:** any mediator summary or inference, labeled as interpretation and kept distinct from the user's request and observed evidence.

Frame the matter neutrally; do not steer roles toward a predetermined answer. The packet is transient working context for the active review, not a durable session record. Which information carries forward into a fresh revision loop remains open; votes from earlier versions do not approve a revised artifact.

## Communication Boundary (Candidate)

The mediator provides each role with the context and work for its review, then receives the role's vote. During that session, the mediator does not ask questions, provide clarifications, or solicit a revised vote. A material change or addition to the context starts a new session.

This boundary is intended to keep each role's judgment independent and make the reviewed context stable. It is exploratory and has not been adopted as current process.

## Handoff and Session Retention (Candidate)

The mediator's handoff to the worker/plan author is limited to `PASS`, or `BLOCK` with all blocking reasons. No additional report sections are included. The mediator uses the context, votes, evidence, and validation details needed to reach its decision during the active session. It reports the result to the user, but no separate durable session record is required afterward.

## Missing-Context Blocks (Candidate)

A role may block when a decision-critical fact is absent from the fixed session context and the role cannot responsibly assess the matter within its authority. The reason may be expressed as a question, but the vote must also identify the context gap, why it is material to the task, a plausible task-relevant adverse case and consequence, and the information or evidence needed to resolve it in a new session. The adverse case is not represented as an established fact.

The human may override an AI role's generic materiality assessment by supplying task-specific context and rationale showing that the concern does not apply to the actual scope. Record the override; if it changes the fixed session context, evaluate it in a new session.

### Mediator Validation of Missing-Context Blocks (Candidate)

For this block type, the mediator validates procedural completeness and scope, not whether the adverse case is substantively true. Check that:

- The uncertainty falls within the role's authority.
- The role identifies a specific missing fact, and the fixed session context does not establish it.
- The role explains why the fact is decision-critical to this task, rather than merely useful to know.
- The role describes a plausible, task-relevant adverse case and consequence without asserting that case as fact.
- The role states what information or evidence would resolve the uncertainty in a new session.

When all criteria are met, treat the block as procedurally valid and apply its applicable gate effect without requiring the mediator to agree that the adverse case will occur. If a criterion is not met, preserve the original vote and record the unmet criterion and supporting evidence; do not rewrite the vote. If validity cannot be confidently determined, do not silently invalidate the block: record the uncertainty and leave the gate unresolved until the applicable process resolves it.

These checks apply to missing-context blocks. They do not yet define how the mediator validates a block whose subject falls within the role's purview but whose reason may be irrelevant or out of scope; that is a separate rule to develop.

### Mediator Validation of In-Purview but Irrelevant Blocks (Candidate)

A role can have authority over a subject while the specific reason for its block is unrelated or inapplicable to the task. The mediator therefore checks the reason's task relevance, not just the role's general purview. Check that:

- The vote identifies the specific criterion or concern it relies on.
- The role states a concrete connection between that concern and the task, proposal, or changed behavior in the fixed session context. The mediator must not invent this connection for the role.
- The fixed context supports the connection. If context affirmatively rules out the concern, or the stated reason has no task-relevant connection, the block may be procedurally invalid. A concern being unlikely or unpersuasive does not by itself make it irrelevant.
- If the connection depends on a fact absent from the context, assess the vote under the missing-context-block criteria rather than deeming it irrelevant. The role must satisfy that block type's requirements.

The mediator may find this block invalid only when its stated reason is demonstrably inapplicable to the task or lacks the required task connection. Preserve the original vote and record the criterion, context evidence, and gate effect. If applicability cannot be confidently determined, do not silently invalidate the block; record the uncertainty and leave the gate unresolved until the applicable process resolves it.

## Context-Building and Re-review (Candidate)

A missing-context block is a vote on the current session, not an invitation for the mediator to answer the role, clarify the context, or request a revised vote. A valid block does not automatically trigger another panel review. The mediator determines whether the identified gap meets defined criteria for pursuit, while remaining responsible for routing the issue rather than answering it substantively.

When the missing fact is discoverable from available evidence, the mediator may delegate context gathering to a separate context-building agent. That agent returns sourced facts, uncertainties, and a proposed context update; it does not decide the issue or recommend how a role should vote. When resolving the gap requires human intent or scope information, the mediator presents that need to the human instead.

Material new information or a changed proposal starts a new session and fresh full-panel loop. Every revised plan or code change submitted for panel review receives a new vote from every role on the full applicable panel; the mediator does not decide which roles can be skipped. The task's established requirements and relevant context carry forward together with the current artifact. Prior votes do not count as approval of the revised version. What prior-session information, if any, belongs in the new panel context remains open; a durable vote history is not required after the result is reported to the user.

Implementation may begin only after the plan version passes its approval gate. If code review findings lead to a code revision, that revised code is submitted to a new full-panel loop and is not accepted until that review passes. The worker may make a batch of edits before submitting a revision; each submitted revision is reviewed as a whole.

The human may provide scope-specific context that overrides an AI role's generic materiality assessment. Record the override and rationale; if it changes the fixed context, include it in a new session.

## Process and authority

Coordinate the panel's stages and role reviews, apply collaboration rules and gates, and facilitate reconciliation or escalation where the rules call for it. Under the candidate communication boundary, reconciliation does not occur through in-session questions, clarifications, or revised votes; material new context is handled in a new session. Preserve each role's authority: do not invent role votes, silently replace a role's substantive judgment, or override a valid block merely because of disagreement.

## Vote validity

Assess whether each vote—especially a block—meets the applicable law and role-contract requirements, has the required evidence and correction condition, and stays within the role's authority. Do not treat an unsupported or out-of-scope concern as a valid block.

When ruling that a vote is invalid or excluding it from affecting the decision, retain the original vote in the record and explain:

- the rule or validity requirement applied
- the evidence for the validity decision
- why the vote does or does not affect the gate

Distinguish procedural invalidity from disagreement with a role's judgment. Never silently discard, rewrite, or misrepresent a vote.

## Relationship to the worker/implementer

The base governance agent currently appears to combine mediator/orchestrator and worker/implementer responsibilities. Treat the worker—who makes the requested change and responds to panel findings—as a distinct responsibility for now; whether it is a separate role and how the two responsibilities interact remain open.

The mediator's purpose is process framing, coordination, gate application, and transparent vote-validity decisions. It is not to implement the requested change or silently substitute its own substantive judgment for the panel's.

## Status and open questions

- The mediator/orchestrator is the first of the two responsibilities to define; this is a purpose summary, not a complete role contract or sample role.
- Future work should define duties, limits, and interaction rules at a level of detail comparable to the existing role contracts.
- The final session-context packet and which information carries forward between full-panel revision loops remain to be decided; the packet above is a candidate.
- The worker/implementer boundary, and any conflict between implementation and judging the adequacy of remediation, still needs discussion.