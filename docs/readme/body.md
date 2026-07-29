<!-- (dl (section-meta TBD)) -->








<!-- (dl (# Adoption Pattern)) -->

Recommended rollout:
1. Start with one pilot PR using full governance.
2. Capture friction and tune role prompts, not gate strictness.
3. Keep role artifacts in PR notes for auditability.
4. Expand to broader repo usage after 2-3 successful iterations.

<!-- (dl (# Quick Start Prompt Template)) -->

Use this template when invoking the panel agent:

"Apply Multi-Agent Governance to this code change. Run requirements, design, implementation_plan, and code_change activities. Use required role votes and activate specialists only when triggers apply. Enforce gate rules and provide invocation evidence, TDD plan, and risk register in the final output."
