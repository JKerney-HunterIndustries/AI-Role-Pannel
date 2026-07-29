# AI Role Panel Governance Pack

This repository provides a drop-in governance pack for running an AI role panel on code-changing work.

It gives teams a deterministic, evidence-based process to improve code design quality through:
- explicit role votes
- gate checks per activity
- tie-resolution rules
- test-first expectations
- required invocation evidence

## Why This Exists

Most AI coding workflows are fast but inconsistent. This pack adds a lightweight governance layer so quality decisions are:
- repeatable
- auditable
- scoped by role authority
- hard to bypass accidentally

## What Is Included

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
  - Role-specific contracts with deterministic checklists and block-validity rules.
  - Required roles: Action, Refactoring, Deletion, Architecture, SDET, Junior.
  - Conditional specialists: UX, Technical Writer.

## Intentional Design Choice: No Default Instructions File

This pack intentionally does not include a default `copilot-instructions.md`.

Reason:
- avoids collisions with existing instruction stacks in target repositories
- avoids precedence confusion across global, workspace, and project instruction files
- lets adopters opt in to this governance model explicitly

## How To Use The Panel

Use the `Multi-Agent Governance` agent for code-changing requests.

Expected lifecycle:
1. requirements
2. design
3. implementation_plan
4. code_change

At each stage, roles vote using `.github/vote-schema.md`, and gates decide pass or blocked.

## Required Role Set

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

## Gate Behavior Summary

- A stage is blocked when defined blocking roles return a valid `block` vote.
- Block votes must include clear correction conditions and concrete evidence.
- Internal remediation should be attempted before escalating to the user.
- Escalate only for unresolved ties or preference-dependent decisions.

See `.github/agent-governance.md` for full gate authority and guardrails.

## TDD and Evidence Expectations

For code changes:
1. define tests first
2. implement minimal code to pass
3. refactor after passing tests
4. rerun tests and report outcomes

Final recommendations should include role invocation evidence and explicit risk tracking.

## Adoption Pattern

Recommended rollout:
1. Start with one pilot PR using full governance.
2. Capture friction and tune role prompts, not gate strictness.
3. Keep role artifacts in PR notes for auditability.
4. Expand to broader repo usage after 2-3 successful iterations.

## Quick Start Prompt Template

Use this template when invoking the panel agent:

"Apply Multi-Agent Governance to this code change. Run requirements, design, implementation_plan, and code_change activities. Use required role votes and activate specialists only when triggers apply. Enforce gate rules and provide invocation evidence, TDD plan, and risk register in the final output."
