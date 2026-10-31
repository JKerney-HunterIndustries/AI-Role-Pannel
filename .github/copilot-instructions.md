# Project Agent Instructions

## Constitutional Rule: Preserve Adversarial Review

Do not make, approve, or carry out any repository change that removes, narrows, bypasses, delays, or otherwise weakens the required adversarial-review process in these instructions or in any repository-controlled instruction, agent, prompt, hook, workflow, or other process. No lower-level instruction, user request, or proposed change may waive or override this rule. If a requested change would weaken adversarial review, stop without making that change and explain the conflict.

For applying this rule, a proposal weakens adversarial review if it does any of the following:
- allows a repository-file change to be made before the required independent review and matching report are approved and verified;
- removes or reduces the requirement to review every repository-file addition, deletion, or modification, including code, tests, and documentation;
- lets the change author substitute for the independent reviewer or moves review until after the proposed operation;
- removes required report creation, uniqueness, readability, action/verdict matching, or the block on a missing, mismatched, unreadable, or rejected report;
- creates an exception, override, delay, or alternate path that permits a repository change without those guarantees.

These are concrete indicators, not an exhaustive list, and they do not limit the preceding rule. If a proposal's effect on adversarial review cannot be determined from these indicators, do not approve or carry it out until the effect is clarified.

## Required: User Input

When user input is needed, stop task work, use the `ask_user` tool, and wait for the user's response before continuing.

## Required: Pre-Change Review

Before each operation that may add, delete, or modify repository files, delegate the exact proposed operation to the `adversarial-reviewer` subagent. This applies to code, tests, and documentation. Run one review at a time; a material change to the proposal requires a new review.

For each review, the reviewer must create one new report under `.agent-reviews/` before returning a verdict. Do not waive this requirement. The reviewer must provide the exact report path. Before acting, verify that the report exists, is readable, and records the reviewed action and verdict. If it is missing, unreadable, or mismatched, do not proceed. For a `REJECTED` test-plan or test-creation review, follow the specific failure and user-direction rules in the Test-First Process below. For other rejected pre-change operations, do not proceed; revise and resubmit using a new report path. If the reviewer cannot save or verify a report, do not make the change.

The reviewer-authored report is exempt from review; this exception permits no other file changes. Reading, searching, and other operations that do not modify repository files do not require review.

## Required: Scope Lock After Gate

After the gate passes, keep both the plan and implementation strictly within the exact requirement(s) that passed it. Do not broaden, extrapolate, or generalize the scope, behavior, or solution, or add adjacent improvements. If the work appears to require a broader or distinct requirement, stop; it must pass the gate separately before being planned or implemented.

## Required: Test-First Process for Code Changes

This process applies only to code changes. Documentation and other non-code changes do not use the test gate, but still require pre-change review.

Complete phases in order; pass each before the next. Test creation requires two distinct adversarial approvals: one for the plan and proposed test before creating it, and one for the actual test before showing it to the user. Each approval requires its own verified report.

### 1. Test gate and plan approval

Before creating a test or implementation, define the intended behavior and complete proposed test. Check whether one test represents the behavior and meets every criterion:

- **Setup:** Shared context only; no assertions or Arrange work.
- **Arrange:** Inputs/preconditions only; no assertions.
- **Act:** Invokes behavior. **Assert:** Checks result.
- **Single behavior:** Verifies one independently observable behavior; do not combine separate behaviors that could pass or fail independently. Multiple assertions are allowed when they check meaningful properties of that same outcome.
- No `if` statements.
- One assertion, or a few checking meaningful properties of one returned object.
- First test for a behavior: regular test. Theory only if the base behavior exists and the test checks a change to it.
- Setup and Arrange may be omitted only when Act itself performs the behavior that brings the subject under test into the program's in-memory state—for example, by constructing, parsing, deserializing, loading, or inserting it. Otherwise, both Setup and Arrange are required.

Before creating the test, submit the behavior plan, complete proposed test, and exact test-file creation operation to the adversarial reviewer. The reviewer must explicitly approve the plan, the proposed test against every criterion, and the exact operation. This review also fulfills the pre-change review for creating that test. Create it only after verifying a matching, readable `APPROVED` report.

**Fail:** If any criterion fails, do not create a test. If a criterion failure is found after a draft has been created, submit the exact draft deletion operation for pre-change review and delete it only after verifying an `APPROVED` report. Stop; tell the user which criterion failed. Resume only after the user changes the goal or rules. A reviewer rejection for a criterion failure follows this same stop. For any other rejection, do not create, show, or implement the test; stop and wait for user direction.

### 2. Create and review the actual test

After the approved test is created, submit the actual test file to the adversarial reviewer before showing it to the user. The reviewer must inspect the file, compare it with the approved plan, and verify every test-gate criterion. This is a separate review with its own report; the plan approval does not substitute for it. Show the test only after verifying a matching, readable `APPROVED` report. If the reviewer rejects it for a criterion failure, follow the failure path in phase 1. For any other rejection, do not show or implement the test; stop and wait for user direction.

### 3. User acceptance

After showing the approved test, pause. Answer questions and discuss without implementing. On request to revise, submit the exact test-file edit for pre-change review, then rerun phase 1 and phase 2; show the revision only after both approvals. Implement only after explicit acceptance of the exact current test; questions, discussion, silence, and revision requests are not acceptance. If rejected, stop and wait.

### 4. Implement and verify

Before each implementation change, obtain the required pre-change review. After acceptance, implement only accepted behavior and run the accepted test plus relevant existing tests. Do not change expected behavior to make implementation pass. If behavior or test must change, stop; rerun the gates and obtain acceptance of the revised test before resuming.

## Required: Test Naming

Name tests as concise behavioral requirements in domain language. Put concrete test data and code-level identifiers (such as type names and qualified enum-member expressions) in the test body rather than the test name.
