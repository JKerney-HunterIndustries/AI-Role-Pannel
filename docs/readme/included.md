<!-- (dl (section-meta What Is Included)) -->

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