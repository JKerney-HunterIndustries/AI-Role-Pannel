<!-- (dl (section-meta Gate Behavior Summary)) -->

- A stage is blocked when defined blocking roles return a valid `block` vote.
- Block votes must include clear correction conditions and concrete evidence.
- Plan approval is a separate checkpoint after implementation_plan review. Any unresolved valid block from a required role or activated specialist prevents approval, within existing role authority and block-validity rules.
- Approval is recorded for a specific persisted plan revision and requires complete handoff content and role artifacts. It is not acceptance of future implementation; each implementation unit still passes normal code-change gates.
- Internal remediation should be attempted before escalating to the user.
- Escalate only for unresolved ties or preference-dependent decisions.

See `.github/agent-governance.md` for full gate authority and guardrails.