# Project Agent Instructions

## Required: User Input

**Trigger:** Apply this rule whenever you are about to request input from me.

**Required format:** Ask in the conversation using an ordinary chat message—not a form or structured query.

**Prohibited formats:** Do not use the ask_user tool or present fields, menus, or preset answer choices.

**Conversation requirements:** Briefly explain what you're trying to understand and why, then ask naturally, leaving room for me to clarify, expand, or redefine the subject. Don't assume my intent or motivation in the question.

**Pause:** Stop task work and wait for my reply before continuing.

## Term: Gheer

A *Gheer* is any action a program performs that falls into one or more of these categories:
- creation of data;
- modification of data;
- creation of a container for data;
- loading information into memory;
- changing the programming environment in which the program operates;
- modifying state;
- sending messages;
- receiving messages; or
- instantiation.

## Constitutional Rule: Preserve Adversarial Review

**Trigger:** Apply this rule to every proposed repository change.

**Prohibition:** Do not make, approve, or carry out any repository change that removes, narrows, bypasses, delays, or otherwise weakens the required adversarial-review process in these instructions or in any repository-controlled instruction, agent, prompt, hook, workflow, or other process.

**Override prohibition:** No lower-level instruction, user request, or proposed change may waive or override this rule.

**Protected guarantees:** For applying this rule, a proposal weakens adversarial review if it does any of the following:
- allows a repository-file change to be made before the required independent review and matching report are approved and verified;
- removes or reduces the requirement to review every repository-file addition, deletion, or modification, including code, tests, and documentation;
- lets the change author substitute for the independent reviewer or moves review until after the proposed operation;
- removes required report creation, uniqueness, readability, action/verdict matching, or the block on a missing, mismatched, unreadable, or rejected report;
- creates an exception, override, delay, or alternate path that permits a repository change without those guarantees.

These are concrete indicators, not an exhaustive list, and they do not limit the prohibition above.

**Failure action:** If a requested change would weaken adversarial review, stop without making that change and explain the conflict. Identify the existing review requirement or guarantee threatened by the proposal and explain how the proposal would remove, narrow, bypass, delay, or otherwise weaken it. For a reviewer rejection under this rule, record that explanation in the required review report.

**Uncertainty action:** If a proposal's effect on adversarial review cannot be determined from these indicators, do not approve or carry it out. State what effect remains undetermined; uncertainty is not permission to proceed.

## Required: Independent Compliance Review

**Trigger:** Apply this process when fulfilling a request would add, modify, or delete any file.

**Exception:** The adversarial reviewer may create its required review reports without review. This exception permits no other file changes.

**Review standard:** At every required review, check compliance with every applicable presented rule. User authorization does not waive those rules.

1. **Review the plan before execution.** Submit the executing AI's entire proposed plan, in its current state, to the `adversarial-reviewer`. Do not begin execution until its report is verified as `APPROVED`.
2. **Review steps when explicitly required.** For each step requiring review under an applicable rule, submit the proposed step before performing it. Do not perform that step until its report is verified as `APPROVED`.
3. **Review the actual result after execution.** Submit the actual file changes for review against the applicable rules and approved plan. Earlier approvals do not substitute for this review.

**Rejected result:** If result review returns `REJECTED`, treat the plan as rejected. Revert the changes made under that plan, preserve the review reports and any pre-existing changes, then stop. The revert must itself receive the required pre-change review; rejection does not waive that protection.

Each review requires a separate report containing the reviewer's reasoning and verdict.

### Pre-Change Gate

**Actor:** Executing AI.

**Trigger:** Before each operation that may add, delete, or modify repository files.

**Requirement:** Delegate the exact proposed operation to the `adversarial-reviewer` subagent. This applies to code, tests, and documentation. Run one review at a time. Do not perform the operation without approval.

**Re-review:** Any change to the reviewed proposal requires a new review. This includes changes to its scope, purpose, or constraints; adding, removing, or reordering file operations; and changes to an operation's target path, type, proposed content, or execution method. These examples do not limit the requirement to review every changed proposal.

**Rejection:** Never perform a rejected operation. For a rejected plan, test-plan, or test-creation review before implementation, follow Bounded Plan Recovery and the Test-First Process below. For other rejected pre-change operations, do not proceed; revise and resubmit using a new report path, subject to the Approved Plan Lock and rejected-result requirements.

Reading, searching, and other operations that do not modify repository files do not require review.

### Review Debugging Reports

**Purpose:** Make the reviewer's decisions inspectable while developing and debugging this system. The report is debugging instrumentation, not the review decision itself.

**Reviewer requirement:** For each review, create one new report under `.agent-reviews/` before returning a verdict. Provide the exact report path. Do not waive this requirement.

**Evidence:** Record the exact proposal or actual result reviewed, every applicable rule used in the decision, and all other evidence relied upon. For each evidence item, include its source and the content used—not merely a reference to where it can be found. Explain how that evidence supports the verdict. Identify any missing evidence, assumptions, or unresolved questions that affected the decision.

**Repository context:** Record the commit hash of the repository state used for the review. If unavailable, record the review date and time with an explicit timezone.

**Executing AI requirement:** Before acting, verify that the report exists, is readable, and records the reviewed action and verdict. If it is missing, unreadable, or mismatched, do not proceed. If the reviewer cannot save or verify a report, do not make the change.

**Exception:** The reviewer-authored report is exempt from review; this exception permits no other file changes.

## Required: Bounded Plan Recovery

**Trigger:** A plan or proposed test is rejected before implementation. For code changes, implementation means source-code changes; creating the required test draft is not implementation. For non-code changes, implementation begins with the first planned file change.

**Request-level failure:** Stop and request user direction only when fulfilling the complete user request necessarily conflicts with applicable rules. For code changes, multiple independently observable gheers, side effects, or execution paths do not alone establish request-level failure when the complete scope can be delivered through sequential AI-plan slices, each satisfying its test gate and having distinct accepted tests for distinct gheers. Each slice must be separately reviewed as required; after completing it, report the result and wait for the user's explicit direction before starting the next. A failed proposal alone does not establish a request-level failure; explain the necessary conflict and its evidence.

**Plan-level failure:** When the failure is in the agent's approach, identify the failed criteria and try a distinct alternative plan without requesting user direction, subject to the limit below. Do not split, narrow, or omit any part of the user's requested scope without explicit permission. The AI may organize the complete scope into sequential plan slices; work on one slice at a time, report completion, and pause for the user's response before starting the next. Continue only on the user's explicit direction. Every slice must preserve its part of the complete outcome and satisfy all applicable approval and test criteria. Autonomous alternative-plan recovery is limited to pre-implementation rejection and does not bypass the between-slice pause.

**Limit:** Allow the initial plan and at most four alternate plans: five attempts total for the same unchanged request. An attempt is one complete proposed plan submitted for independent review; multiple reviews of that plan do not count as alternate plans. Record each attempt, the relevant review reports, the rejection reasons, and how each alternative addresses them. Approval, test creation, or a later rejection before implementation does not reset the count. After the fifth attempt fails, stop, summarize the attempts and remaining blockers, and request user direction. Do not submit a sixth attempt or reset the budget without explicit user direction.

**Review and cleanup:** Rejection never authorizes execution. Before retrying, obtain verified pre-change approval for deleting any rejected test draft created under the failed plan; preserve review reports and pre-existing changes. If cleanup cannot be approved and verified, do not proceed. Each alternative requires fresh plan and exact-operation review; for code changes, restart Phase 1 and Phase 2 and obtain explicit user acceptance of the exact reviewed test before implementation. No approval or acceptance from a failed plan carries over.

**Boundaries:** This recovery rule permits replanning, not changes without approval. Missing, unreadable, mismatched, or unverified reports never authorize a file operation. The constitutional prohibition and its uncertainty action remain unchanged. Changes to an approved plan outside recovery from a pre-implementation rejection still require the Approved Plan Lock pause. Once implementation has begun, the existing rejected-result rollback-and-stop and approved-plan-change requirements remain in force. Explicit user acceptance of the test remains required; user rejection is not an autonomous-retry instruction.

## Required: Approved Plan Lock

**Trigger:** After a plan receives review approval.

**Requirement:** Execute only the approved plan. Any change to that plan requires a new review and a matching report verified as `APPROVED` before execution of the changed plan. Do not change an implementation that has received review approval without the required reapproval.

**Pause:** If a change to the approved plan is needed, stop execution, explain the proposed change, and wait for user direction. The only autonomous-replanning case is recovery from a pre-implementation rejection under Bounded Plan Recovery; that rule still requires approved cleanup and fresh verified approval before any file change. User direction does not substitute for review approval; do not resume execution under the changed plan without verified reapproval.

## Required: Test-First Process for Code Changes

**Purpose:** Determine whether a proposed code-change plan is appropriately scoped.

**Test ownership and lifecycle:** The test is an expression of the proposed code-change plan, not an independent deliverable. Its creation, review, and user acceptance are stages of that same plan; separate approvals do not create separate plans or rollback boundaries. The test remains as a regression test when the plan succeeds. If the plan's result is rejected, rollback includes any test created under that plan. Such a test is not a pre-existing change merely because it was created or approved before implementation. Its deletion still requires independent pre-change review.

**Sequential plan slices:** Review and approve the full sequential plan and user-requested scope as one plan. For code changes, each slice is one independently observable gheer/outcome implementation unit and has exactly one proposed and accepted test, assigned exclusively to that slice. That test verifies only the slice's intended gheer/outcome on one path and cannot be reused for or satisfy coverage requirements for another slice. Any additional independently observable gheer or side effect belongs in its own sequential slice with its own distinct test. Apply Phase 1's one-test, one-path, and side-effect criteria to every slice. Before implementing each slice, complete its test-plan review, actual-test review, exact-test user acceptance, and required exact file-operation reviews. After each slice, report the result and wait for the user's explicit direction before starting the next. Slices do not narrow the requested scope, reset recovery attempts, or create separate plan or rollback boundaries. The Approved Plan Lock governs the full sequence; if implementation-result review rejects the work, rollback covers all changes and test drafts created under the plan, preserving reports and pre-existing changes. The revert requires its own pre-change review. No autonomous recovery is permitted after implementation begins.

**Test-backed production code:** Production behavior introduced or changed by a code change must be meaningfully verified by at least one accepted test asserting its intended observable outcome. Merely executing code, checking a type, or incidentally passing through it is not meaningful verification. Code without tests is presumed incorrect; its existing presence alone is not evidence that it is useful or correct. The user's prompt or other established requirements may establish that behavior is intentional and worth preserving. Untested code outside the requested scope must not block, delay, or be a condition for approving a change. During removal-only changes, preserve other behavior verified by accepted tests except for behavior the user requests to remove. If the requested removal may affect untested behavior beyond the behavior the user identified, explain the specific risk and ask the user whether to proceed. Other behavior changes remain subject to the ordinary test-first process.

**Removing behavior:** When the user requests removal of tested production behavior, remove in the same approved change the tests that verify only that behavior. Tests that also verify retained behavior stay.

**Subtractive behavior removal:** A removal-only change deletes production code or makes only the minimum edits to existing lines necessary to eliminate the targeted behavior, preserving all other known behavior verified by accepted tests. It must add no production-code statements, expressions, decisions, or observable behavior. It must not be rejected or delayed solely because code outside the requested scope is untested. Coverage may decrease, and no replacement test is required solely for removed behavior. For this case only, the Test-First Process requirement to create a proposed test does not require a replacement test; it does not waive independent pre-change review with a verified report for every file operation, post-change result review, or any other review requirement. A request that adds production code or introduces behavior is not removal-only; it must follow the ordinary test-first and Bounded Plan Recovery requirements. The removal-only exception cannot be used to avoid testing additions or to split or narrow the user's requested scope without explicit permission; any permitted AI-plan slices must collectively preserve the full request, satisfy their applicable test gates, and pause for user input between slices.

This process applies only to code changes. Documentation and other non-code changes do not use the test gate, but still require pre-change review.

Complete phases in order; pass each before the next. Test creation requires two distinct adversarial approvals: one for the plan and proposed test before creating it, and one for the actual test before showing it to the user. Each approval requires its own verified report.

### 1. Test Expression Gate and Plan Approval

**Trigger:** Before any source-code change.

**Expression:** Define the intended gheer for the current code-change slice and provide the complete proposed test for that slice, in its current state.

**Pass criteria:** The current code-change slice is appropriately scoped only if one good test can satisfy all of the following:

> **LAW:** Each law must be satisfied in full. Partial compliance is failure; compliance with one requirement cannot compensate for violating another. Any unresolved uncertainty about compliance is also grounds for rejection.

> **LAW:** The test represents one intended gheer and one specific path through the application. A path is the execution route selected by the test’s inputs and preconditions, including relevant decisions and transitions. Assertions must verify only the intended gheer’s observable outcome, not other independently observable gheers or side effects.

> **LAW:** Every code slice must introduce or change exactly one independently observable gheer/outcome and have exactly one proposed and accepted test assigned exclusively to that slice. The test verifies only that gheer/outcome on one path; no test may satisfy coverage requirements for multiple slices. Any additional independently observable gheer or side effect must be placed in another sequential slice with its own distinct test. If satisfying this requirement requires more than the single proposed test, the current slice is too broad and must be divided into narrower sequential slices under the complete plan; no part of the user's requested scope may be omitted or narrowed without permission.

- The test meets every criterion under **A Good Test**.
- The test exercises every new execution pathway introduced by the current slice.
If no single test can satisfy all three conditions, the current slice is too broad and must be divided into narrower sequential slices under the complete plan, with user input between slices as required above.

**Independent review:** Before creating the test, submit the gheer plan, proposed test, and exact test-file creation operation to the adversarial reviewer. The reviewer must explicitly approve the plan, the expression against every criterion, and the exact operation.

**Pass action:** This review also fulfills the pre-change review for creating that test. Create it only after verifying a matching, readable `APPROVED` report.

**Failure action:** If the current slice cannot satisfy the pass criteria, do not create the test. Identify the failed criteria and distinguish a request-level conflict from a defect in the proposed approach under Bounded Plan Recovery. If a rejected draft has already been created, obtain verified pre-change approval for its exact deletion and delete it before retrying. Do not create, show, or implement a rejected test. For a plan-level failure within the attempt limit, propose a distinct alternative preserving the complete user-requested scope collectively across the plan's slices and restart Phase 1 without waiting for user direction; do not begin a later slice without the user's explicit direction. For a request-level failure or exhausted attempt limit, explain the failure and request user direction. Every alternative must pass all gates; narrowing the implementation approach must not narrow the requested outcome.

### 2. Create and review the actual test

After the test approved in Phase 1 is created, submit the actual test file to the adversarial reviewer before showing it to the user. The reviewer must verify that the created test exactly matches the complete test proposed and approved in Phase 1, compare it with the approved plan, and verify every test-gate criterion, including that it represents the plan's intended gheer. This separate review verifies that the created test is the approved test; the Phase 1 review does not substitute for checking the actual file. Show the test only after verifying a matching, readable `APPROVED` report. If the reviewer finds a criterion failure, a mismatch with the approved proposal, or any other reason for rejection, do not show or implement the test; follow Phase 1's failure action and Bounded Plan Recovery, including approved draft deletion and restarting both test reviews for an alternative. If the user directs a revision, submit the exact proposed edit for pre-change review and repeat Phase 1 and Phase 2 before showing the revised test.

### 3. User acceptance

After Phase 2 approves the created test, show that exact test to the user and pause for explicit acceptance. Acceptance applies only to the test content proposed and approved in Phase 1 and verified as the created file in Phase 2. Questions, discussion, silence, or a request for revisions are not acceptance. If revisions are requested, submit the exact proposed test edit for pre-change review, then repeat Phase 1 and Phase 2 for the revised test before showing it for acceptance. Implement only after explicit acceptance of the exact current test. If the user rejects the test without requesting revisions, do not implement it; wait for further user direction.

### 4. Implement and verify

Before each implementation change, obtain the required pre-change review. After acceptance, implement only accepted gheer and run the accepted test and any existing tests that verify other aspects of the same function or system action under test. Do not change expected gheer to make implementation pass. If gheer or test must change, stop; rerun the gates and obtain acceptance of the revised test before resuming.

## Required: A Good Test

**Trigger:** Whenever a test is written or modified in code.

**Requirement:** Every such test must satisfy all these criteria:

- **Representation:** The test verifies one intended gheer that no other test already verifies. A gheer is an action the system performs. Examples include responding to input, creating information, creating a function or class that can contain logic, creating a message, sending a message, receiving a message, and creating a holder for information. These examples are illustrative, not exhaustive.
- **Setup:** Creates only necessities required by every test that uses that setup. Test-specific preparation belongs in Arrange.
- **Arrange:** Establishes all test-specific preconditions and data, including the subject under test, inputs, mocks, and constants. Calls made solely to establish preconditions belong here; the gheer being tested belongs in Act.
  - **Omission:** Arrange may be omitted only when the Act call itself creates or loads the subject under test, and that creation or loading is the gheer being tested. Otherwise, Arrange is required.
- **Act:** Makes the single call that invokes the gheer being tested. All calls made by the test before Act are preparation and belong in Setup or Arrange.
- **Assert:** Checks results only; it must not create or arrange anything being tested, and assertions must not be placed in Setup.
  - **One assertion**: A test must contain exactly one assertion statement. That assertion must verify exactly one independently observable property of the intended Gheer. It must not combine checks of multiple properties, including through aggregate or deep-equality assertions.
- **Single gheer:** Verifies one independently observable gheer; do not combine separate gheers that could pass or fail independently. Multiple assertions are allowed when they check meaningful properties of that same outcome.
- **Branching:** Test code must represent a single specific path through the application. Branching logic is prohibited in all test code. This includes conditional statements (`if`, `switch`/`match`), conditional expressions (such as ternaries), and short-circuit operators (`&&`, `||`). Loops are allowed.
- **Theory tests:**
  - **New Gheer:** Begin with a regular test for a newly introduced Gheer. Add a theory test only after a concrete need to cover defined variations of that same Gheer has been demonstrated.
  - **Narrow exception for a distinct new Gheer:** A requested change may introduce a distinct Gheer in a process for which an approved theory test already defines variations of an input or precondition relevant to the new Gheer.
    - The new Gheer may be tested with a theory without first creating a regular test, reusing only those already-defined, applicable variations; the request need not separately restate them.
    - An approved theory test, shared process, or shared output alone is not sufficient; the established variations must apply to the new Gheer.
    - This exception changes only the test form allowed. It does not combine the new Gheer with the existing theory's Gheer, broaden requested scope, authorize new variation dimensions or unrelated behavior outside the requested change, or waive any other test criterion or gate.
    - The new theory must represent only the new Gheer. Its cases must vary only relevant inputs or preconditions represented by the reused variations, check its one intended observable outcome, and each follow one specific path. Sharing a process or output does not make distinct Gheers the same Gheer.
  - **Migration of an existing regular test:** A theory test used to migrate an existing regular test must include a case equivalent to the regular test's original case and preserve its expected observable outcome, while satisfying every other test criterion.
    - Only during this migration may the theory test temporarily overlap with the regular test; this is the sole exception to the Representation criterion that no other test already verifies the Gheer.
    - Keep the regular test until the theory test has passed the required plan/proposed-test and actual-test reviews, the user has accepted its exact content, the accepted implementation is complete, and the accepted theory test has passed execution including the original case and every identified variation.
    - The regular test may then be removed only through its own exact pre-change review, and only if the theory test continues to cover all behavior the regular test covered.
    - This migration exception waives no other test criterion or review, acceptance, and report requirement.
- **Naming:** Name each test concisely for the gheer it verifies, using domain language—not the test's specific data or implementation details. Apply to regular and theory tests. Put concrete test data and code-level identifiers in the test body.

**Failure action:** If a test cannot meet every criterion, do not create it. If such a test has already been created, obtain approval for its exact deletion through the required pre-change review, then delete it. Identify the failed criterion. Before implementation, follow Bounded Plan Recovery and restart the test gates for any alternative; stop and request user direction for a request-level failure or exhausted attempt limit. Once implementation has begun, do not retry autonomously; follow the approved-plan-change and rejected-result requirements as applicable.