<!-- GENERATED DOCUMENT DO NOT EDIT! -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->

<!-- Compiled with doculisp https://www.npmjs.com/package/doculisp -->
<!-- Written By: jason-kerney -->

# AI Role Panel Governance Pack #

1. [Overview](#overview)
2. [Why This Exists](#why-this-exists)
3. [What Is Included](#what-is-included)
4. [Intentional Design Choice: No Default Instructions File](#intentional-design-choice-no-default-instructions-file)
5. [How To Use The Panel](#how-to-use-the-panel)
6. [Required Role Set](#required-role-set)
7. [What Each Role Cares About](#what-each-role-cares-about)
8. [Gate Behavior Summary](#gate-behavior-summary)
9. [TDD and Evidence Expectations](#tdd-and-evidence-expectations)
10. [Adoption Pattern](#adoption-pattern)
11. [Quick Start Prompt Template](#quick-start-prompt-template)
12. [Contributors ✨](#contributors-)

## Overview ##

This repository provides a drop-in governance pack for running an AI role panel on code-changing work.

It gives teams a structured, evidence-based process to improve code design quality through:
- explicit role votes
- gate checks per activity
- tie-resolution rules
- test-first expectations
- required invocation evidence

## Why This Exists ##

Most AI coding workflows are fast but inconsistent. This pack adds a lightweight governance layer so quality decisions are:
- repeatable
- auditable
- scoped by role authority
- hard to bypass accidentally

## What Is Included ##

- `.github/agent-governance.md`
  - Core operating policy.
  - Defines activation triggers, role intent, gate rules, escalation, stop conditions, TDD rules, and required output structure.

- `.github/vote-schema.md`
  - Shared vote contract for every role.
  - Standardizes vote shape: reason, condition, evidence, and scope.

- `.github/agents/multi-agent-governance.agent.md`
  - Agent profile for running the governance loop.
  - Declares required phases and role artifact requirements.

- `.github/roles/*.md`
  - Role-specific contracts with explicit checklists and block-validity rules.
  - Required roles: Action, Refactoring, Deletion, Architecture, SDET, Junior.
  - Conditional specialists: UX, Technical Writer.

## Intentional Design Choice: No Default Instructions File ##

This pack intentionally does not include a default `copilot-instructions.md`.

Reason:
- avoids collisions with existing instruction stacks in target repositories
- avoids precedence confusion across global, workspace, and project instruction files
- lets adopters opt in to this governance model explicitly

## How To Use The Panel ##

Use the `Multi-Agent Governance` agent for code-changing requests.

Expected lifecycle:
1. requirements
2. design
3. implementation_plan
4. code_change

At each stage, roles vote using `.github/vote-schema.md`, and gates decide pass or blocked.

## Required Role Set ##

Always include these roles for code-changing governance:
1. Action
2. Refactoring
3. Deletion
4. Architecture
5. SDET
6. Junior

Activate these specialists only when trigger conditions are present:
1. UX
2. Technical Writer

## What Each Role Cares About ##

This section helps users quickly understand each role's review lens and likely block conditions.

### Required Roles ###

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

### Conditional Specialists ###

1. UX (activated by workflow, interaction, navigation, copy, or accessibility changes)
- Cares most about discoverability, usability, accessibility, placement, and transition clarity.
- Looks for incoherent flows, hidden entry points, unclear feedback, and inconsistent equivalent flows.
- Blocks when severe usability/accessibility defects exist in activated scope, or when context-mismatched placement is unresolved.

2. Technical Writer (activated by user-visible behavior or documentation-impacting changes)
- Cares most about user comprehension and docs/release-note accuracy.
- Looks for missing same-change docs updates, stale instructions, weak examples, and poor discoverability.
- Blocks when material user-facing docs are missing/inaccurate for changed behavior, or release-note expectations are unmet when applicable.

### How To Use This During Review ###

When a role blocks, use its lens first to resolve the issue:
1. identify the failed check in that role's contract
2. provide concrete evidence in requirements/design/plan/diff/tests/docs
3. apply the minimum correction needed to clear the role's condition
4. rerun gate evaluation and record remaining risks explicitly

## Gate Behavior Summary ##

- A stage is blocked when defined blocking roles return a valid `block` vote.
- Block votes must include clear correction conditions and concrete evidence.
- Internal remediation should be attempted before escalating to the user.
- Escalate only for unresolved ties or preference-dependent decisions.

See `.github/agent-governance.md` for full gate authority and guardrails.

## TDD and Evidence Expectations ##

For code changes:
1. define tests first
2. implement minimal code to pass
3. refactor after passing tests
4. rerun tests and report outcomes

Final recommendations should include role invocation evidence and explicit risk tracking.

## Adoption Pattern ##

Recommended rollout:
1. Start with one pilot PR using full governance.
2. Capture friction and tune role prompts, not gate strictness.
3. Keep role artifacts in PR notes for auditability.
4. Expand to broader repo usage after 2-3 successful iterations.

## Quick Start Prompt Template ##

Use this template when invoking the panel agent:

"Apply Multi-Agent Governance to this code change. Run requirements, design, implementation_plan, and code_change activities. Use required role votes and activate specialists only when triggers apply. Enforce gate rules and provide invocation evidence, TDD plan, and risk register in the final output."

## Contributors ✨ ##

Thanks goes to these wonderful people ([emoji key](https://allcontributors.org/docs/en/emoji-key)):

<table>
  <tbody>
    <tr>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/Hunter-Industries"><img src="https://avatars.githubusercontent.com/u/30634197?v=4?s=100" width="100px;" alt="Hunter Industries Software Development"/><br /><sub><b>Hunter Industries Software Development</b></sub></a><br /><a href="#financial-Hunter-Industries" title="Financial">💵</a></td>
      <td align="center" valign="top" width="14.28%"><a href="http://www.chrisstead.net/"><img src="https://avatars.githubusercontent.com/u/4184510?v=4?s=100" width="100px;" alt="Chris Stead"/><br /><sub><b>Chris Stead</b></sub></a><br /><a href="#design-cmstead" title="Design">🎨</a> <a href="#ideas-cmstead" title="Ideas, Planning, & Feedback">🤔</a> <a href="#mentoring-cmstead" title="Mentoring">🧑‍🏫</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/jason-kerney"><img src="https://avatars.githubusercontent.com/u/5097968?v=4?s=100" width="100px;" alt="Jason Kerney"/><br /><sub><b>Jason Kerney</b></sub></a><br /><a href="#content-jason-kerney" title="Content">🖋</a> <a href="#design-jason-kerney" title="Design">🎨</a> <a href="https://github.com/JKerney-HunterIndustries/AI-Role-Panel/commits?author=jason-kerney" title="Documentation">📖</a> <a href="#ideas-jason-kerney" title="Ideas, Planning, & Feedback">🤔</a> <a href="#maintenance-jason-kerney" title="Maintenance">🚧</a> <a href="#research-jason-kerney" title="Research">🔬</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/eddie-craftsmanshipcounts"><img src="https://avatars.githubusercontent.com/u/6170865?v=4?s=100" width="100px;" alt="eddie-craftsmanshipcounts"/><br /><sub><b>eddie-craftsmanshipcounts</b></sub></a><br /><a href="#ideas-eddie-craftsmanshipcounts" title="Ideas, Planning, & Feedback">🤔</a></td>
      <td align="center" valign="top" width="14.28%"><a href="https://github.com/jasonlin858"><img src="https://avatars.githubusercontent.com/u/310755558?v=4?s=100" width="100px;" alt="jasonlin858"/><br /><sub><b>jasonlin858</b></sub></a><br /><a href="#ideas-jasonlin858" title="Ideas, Planning, & Feedback">🤔</a></td>
    </tr>
  </tbody>
</table>

This project follows the [all-contributors](https://github.com/all-contributors/all-contributors) specification. Contributions of any kind welcome!

<!-- Written By: jason-kerney -->
<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->
<!-- GENERATED DOCUMENT DO NOT EDIT! -->