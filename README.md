# Constitutional System

Use this project to add a test-first, pre-change adversarial review workflow to
your own VS Code Copilot project.

## Adopt It

Copy these files into the same paths in your project:

- `.github/copilot-instructions.md`
- `.github/agents/adversarial-reviewer.agent.md`

Open your project in VS Code and use Copilot Chat there. The project
instructions guide the main agent through a test-first gate for code changes
and require an adversarial review before every repository-file addition,
deletion, or modification.

## Workflow

For code changes:

1. The main agent checks whether the intended behavior can be represented by
   one test that meets every gate criterion. If not, it stops, explains the
   failed criterion, and does not proceed with the code change.
2. Before creating the test, the main agent submits the behavioral plan, the
   complete proposed test, and the exact file operation to the
   `adversarial-reviewer` subagent. The reviewer must approve the plan and
   proposed test against every criterion and create a report. The main agent
   verifies the report before creating the test.
3. After creating the approved test, the main agent submits the actual test
   file for a separate adversarial review. The reviewer checks the actual
   contents against the approved plan and every criterion, and creates a
   separate report. The test is not shown to the user unless that report is
   verified as approved.
4. The main agent shows the approved test to the user and pauses for questions,
   discussion, revisions, or explicit acceptance. It does not begin
   implementation until the user accepts the exact current test. Each
   requested test revision receives pre-change review and repeats both test
   approvals before it is shown.
5. Before each implementation file change, the main agent submits that exact
   operation for review. The reviewer verifies both test approvals and user
   acceptance, then checks that the proposed change stays within the accepted
   behavior.
6. After approval, the main agent implements the accepted behavior and runs
   the accepted test and relevant existing tests.

If a test-gate criterion fails, no test is created or kept; the main agent
stops, identifies the failed criterion, and resumes only after the user changes
the goal or rules. Any missing, mismatched, unreadable, or rejected review
report blocks progression.

For non-code changes, there is no code-only test gate, but every
repository-file change still receives a pre-change adversarial review. The
reviewer creates a report in `.agent-reviews/` for every verdict, including
both approvals and rejections. The main agent proceeds only after verifying a
matching `APPROVED` report. If rejected, it revises the operation and requests
a new review.

Conversation and actions that do not change repository files are not reviewed.

## Scope

This is instruction-based guidance for VS Code Copilot, not deterministic
enforcement. It depends on project instructions being available to the active
Copilot session. File-changing requests require independent review of the plan
before execution and of the actual changes afterward. Step reviews are required
where the instructions explicitly specify them, including the existing review
before every repository-file change. Every required review checks compliance
with all applicable presented rules and records its reasoning and verdict in a
separate report. See [VISION.md](VISION.md) for the longer-term goal.