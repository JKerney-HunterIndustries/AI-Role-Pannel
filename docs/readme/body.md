<!-- (dl (section-meta TBD)) -->

<!-- (dl (# How To Use The Panel)) -->

Use the `Multi-Agent Governance` agent for code-changing requests.

Expected lifecycle:
1. requirements
2. design
3. implementation_plan
4. code_change

At each stage, roles vote using `.github/vote-schema.md`, and gates decide pass or blocked.

<!-- (dl (# Required Role Set)) -->

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

<!-- (dl (# Gate Behavior Summary)) -->

- A stage is blocked when defined blocking roles return a valid `block` vote.
- Block votes must include clear correction conditions and concrete evidence.
- Internal remediation should be attempted before escalating to the user.
- Escalate only for unresolved ties or preference-dependent decisions.

See `.github/agent-governance.md` for full gate authority and guardrails.

<!-- (dl (# TDD and Evidence Expectations)) -->

For code changes:
1. define tests first
2. implement minimal code to pass
3. refactor after passing tests
4. rerun tests and report outcomes

Final recommendations should include role invocation evidence and explicit risk tracking.

<!-- (dl (# Adoption Pattern)) -->

Recommended rollout:
1. Start with one pilot PR using full governance.
2. Capture friction and tune role prompts, not gate strictness.
3. Keep role artifacts in PR notes for auditability.
4. Expand to broader repo usage after 2-3 successful iterations.

<!-- (dl (# Quick Start Prompt Template)) -->

Use this template when invoking the panel agent:

"Apply Multi-Agent Governance to this code change. Run requirements, design, implementation_plan, and code_change activities. Use required role votes and activate specialists only when triggers apply. Enforce gate rules and provide invocation evidence, TDD plan, and risk register in the final output."
