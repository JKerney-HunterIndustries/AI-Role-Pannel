<!-- (dl (section-meta How To Use The Panel)) -->

Use the `Multi-Agent Governance` agent for code-changing requests.

Expected lifecycle:
1. requirements
2. design
3. implementation_plan
4. code_change

At each stage, roles vote using `.github/vote-schema.md`, and gates decide pass or blocked.

<!-- (dl (# Seek Better Ways Within Scope)) -->

Treat the current architecture and coding style as context and evidence, not presumptively correct. Compare concrete, feasible alternatives for the same requested outcome when they offer a material role-relevant benefit. Prefer better supported task-specific tradeoffs, not conformity or novelty.

In requirements, locate the affected area and constraints. In design, record the current approach, meaningful alternatives (or evidence that none qualifies), benefits/costs, and the chosen approach. Retaining the baseline when a meaningful alternative exists requires task-specific justification; "that's our style" is insufficient. In the plan, schedule selected improvements and proof paths; at acceptance, check the observed change and explain deviations.

Consider behavior, ownership, maintainability, surface area, newcomer comprehension, compatibility, verification, effort, and migration risk as relevant. Record unrelated broad redesign as outside scope, not a prerequisite. Reuse repository code/tests and concise rationale rather than requiring new abstractions or exhaustive onboarding docs.

<!-- (dl (# Example: Improve A Touched Behavior Without A Broad Redesign)) -->

Task: add a retry limit to an existing importer. Assume tests can cover the importer and its configuration, and compatibility requires keeping the current entry point.

1. **Requirements:** Locate the importer entry point and owner. Specify retry-limit behavior and failure cases. Junior exposes the undocumented assumption that `attempts` means retries rather than total calls; SDET defines boundary tests.
2. **Design:** Compare extending the existing loop with extracting a local retry helper. The helper could clarify sequencing, so it is a meaningful alternative. Retain the loop only with task-specific evidence, such as its single caller, already explicit flow, and lower verification/migration cost after clarifying the name. Existing style alone cannot decide. If the helper has better supported tradeoffs instead, choose it.
3. **Implementation plan:** Name the touched loop/config/tests; clarify the ambiguous term and its rationale. Record that replacing the entire import pipeline is unrelated and deferred. If moving ownership is selected, Architecture records revised rules, affected consumers, boundary/dependency consequences, and verification.
4. **Acceptance:** Verify retry-limit and failure tests and the selected design. Junior follows the entry point through configuration to the retry outcome using repository-visible names/tests/rationale. A concrete missing connection can block with a correction; "I prefer helpers" cannot. Roles record evidence, resolve valid blocks, and retain justified risks.
