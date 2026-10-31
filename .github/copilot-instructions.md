# Project Agent Instructions

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

## Required: User Input

**Trigger:** Apply this rule whenever you are about to request input from me.

**Required format:** Ask in the conversation using an ordinary chat message—not a form or structured query.

**Prohibited formats:** Do not use the ask_user tool or present fields, menus, or preset answer choices.

**Conversation requirements:** Briefly explain what you're trying to understand and why, then ask naturally, leaving room for me to clarify, expand, or redefine the subject. Don't assume my intent or motivation in the question.

**Pause:** Stop task work and wait for my reply before continuing.

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

**Rejection:** For a `REJECTED` test-plan or test-creation review, follow the specific failure and user-direction rules in the Test-First Process below. For other rejected pre-change operations, do not proceed; revise and resubmit using a new report path.

Reading, searching, and other operations that do not modify repository files do not require review.

### Review Debugging Reports

**Purpose:** Make the reviewer's decisions inspectable while developing and debugging this system. The report is debugging instrumentation, not the review decision itself.

**Reviewer requirement:** For each review, create one new report under `.agent-reviews/` before returning a verdict. Provide the exact report path. Do not waive this requirement.

**Evidence:** Record the exact proposal or actual result reviewed, every applicable rule used in the decision, and all other evidence relied upon. For each evidence item, include its source and the content used—not merely a reference to where it can be found. Explain how that evidence supports the verdict. Identify any missing evidence, assumptions, or unresolved questions that affected the decision.

**Repository context:** Record the commit hash of the repository state used for the review. If unavailable, record the review date and time with an explicit timezone.

**Executing AI requirement:** Before acting, verify that the report exists, is readable, and records the reviewed action and verdict. If it is missing, unreadable, or mismatched, do not proceed. If the reviewer cannot save or verify a report, do not make the change.

**Exception:** The reviewer-authored report is exempt from review; this exception permits no other file changes.

## Required: Approved Plan Lock

**Trigger:** After a plan receives review approval.

**Requirement:** Execute only the approved plan. Any change to that plan requires a new review and a matching report verified as `APPROVED` before execution of the changed plan. Do not change an implementation that has received review approval without the required reapproval.

**Pause:** If a change to the approved plan is needed, stop execution, explain the proposed change, and wait for user direction. User direction does not substitute for review approval; do not resume execution under the changed plan without verified reapproval.

## Required: A Good Test

**Trigger:** Whenever a test is written or modified in code.

**Requirement:** Every such test must satisfy all these criteria:

- **Representation:** The test verifies one intended behavior that no other test already verifies. A behavior is an action the system performs. Examples include responding to input, creating information, creating a function or class that can contain logic, creating a message, sending a message, receiving a message, and creating a holder for information. These examples are illustrative, not exhaustive.
- **Setup:** Creates only necessities required by every test that uses that setup. Test-specific preparation belongs in Arrange.
- **Arrange:** Inputs/preconditions only; no assertions.
- **Act:** Invokes behavior.
- **Assert:** Checks results only; it must not create or arrange anything being tested, and assertions must not be placed in Setup.
- **Single behavior:** Verifies one independently observable behavior; do not combine separate behaviors that could pass or fail independently. Multiple assertions are allowed when they check meaningful properties of that same outcome.
- **Path coverage:** The test must exercise every new execution pathway introduced by the proposed implementation.
- No `if` statements.
- One assertion, or a few checking meaningful properties of one returned object.
- First test for a behavior: regular test. Theory only if the base behavior exists and the test checks a change to it.
- Setup and Arrange may be omitted only when Act itself performs the behavior that brings the subject under test into the program's in-memory state—for example, by constructing, parsing, deserializing, loading, or inserting it. Otherwise, both Setup and Arrange are required.

**Failure action:** If a test cannot meet every criterion, do not create it. If such a test has already been created, obtain approval for its exact deletion through the required pre-change review, then delete it. Stop, identify the failed criterion, and wait for the user to change the goal or rules.

## Required: Test-First Process for Code Changes

This process applies only to code changes. Documentation and other non-code changes do not use the test gate, but still require pre-change review.

Complete phases in order; pass each before the next. Test creation requires two distinct adversarial approvals: one for the plan and proposed test before creating it, and one for the actual test before showing it to the user. Each approval requires its own verified report.

### 1. Test Expression Gate and Plan Approval

**Trigger:** Before any source-code change.

**Expression:** Define the intended behavior and provide the entire proposed test, in its current state.

**Pass criteria:** The plan must be expressible in one good test:
- One test represents the intended behavior.
- That test meets every criterion under **A Good Test**.
- That test exercises every new execution pathway introduced by the proposed implementation.

**Independent review:** Before creating the test, submit the behavior plan, proposed test, and exact test-file creation operation to the adversarial reviewer. The reviewer must explicitly approve the plan, the expression against every criterion, and the exact operation.

**Pass action:** This review also fulfills the pre-change review for creating that test. Create it only after verifying a matching, readable `APPROVED` report.

**Failure action:** If any criterion fails, do not create a test. If a criterion failure is found after a draft has been created, submit the exact draft deletion operation for pre-change review and delete it only after verifying an `APPROVED` report. Stop; tell the user which criterion failed. Resume only after the user changes the goal or rules. A reviewer rejection for a criterion failure follows this same stop. For any other rejection, do not create, show, or implement the test; stop and wait for user direction.

### 2. Create and review the actual test

After the approved test is created, submit the actual test file to the adversarial reviewer before showing it to the user. The reviewer must inspect the file, compare it with the approved plan, and verify every test-gate criterion. This is a separate review with its own report; the plan approval does not substitute for it. Show the test only after verifying a matching, readable `APPROVED` report. If the reviewer rejects it for a criterion failure, follow the failure path in phase 1. For any other rejection, do not show or implement the test; stop and wait for user direction.

### 3. User acceptance

After showing the approved test, pause. Answer questions and discuss without implementing. On request to revise, submit the exact test-file edit for pre-change review, then rerun phase 1 and phase 2; show the revision only after both approvals. Implement only after explicit acceptance of the exact current test; questions, discussion, silence, and revision requests are not acceptance. If rejected, stop and wait.

### 4. Implement and verify

Before each implementation change, obtain the required pre-change review. After acceptance, implement only accepted behavior and run the accepted test plus relevant existing tests. Do not change expected behavior to make implementation pass. If behavior or test must change, stop; rerun the gates and obtain acceptance of the revised test before resuming.

## Required: Test Naming

Name tests as concise behavioral requirements in domain language. Put concrete test data and code-level identifiers (such as type names and qualified enum-member expressions) in the test body rather than the test name.
