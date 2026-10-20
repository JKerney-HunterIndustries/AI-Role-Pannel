<!-- (dl (section-meta How To Use The Panel)) -->

Use the `Multi-Agent Governance` agent for code-changing requests or planning-only deliverables that other agents will execute later.

Expected lifecycle:
1. requirements
2. design
3. implementation_plan
4. code_change

At each stage, roles vote using `.github/vote-schema.md`, and gates decide pass or blocked.

After implementation_plan review, a separate `plan_approval_required` checkpoint must pass before planned implementation begins. Planning-only requests stop after delivering the reviewed plan and its approval status; they do not enter code_change.

### Planning Across Agents and Sessions

An overarching plan may coordinate multiple independently releasable work units. Each unit needs an identifier, scope, dependencies, acceptance criteria, and test-first verification path. The Action role checks decomposition and per-unit value rather than rejecting a plan merely for containing several units.

Persist the plan with its repository baseline, revision, assumptions, decision rationale, role evidence, and explicit approval status. Later agents check the approved revision, current code, and completed dependencies before executing a unit. Each implementation unit still receives normal code-change governance; material plan changes require refreshed affected reviews and plan approval.

Use ordinary human-readable revision labels for plans and other panel-generated artifacts. Do not add cryptographic hashes, fingerprints, digests, signatures, or equivalent protection unless explicitly requested. Treat unrequested cryptographic metadata already present as non-authoritative; never validate it or use a mismatch to block a gate, and remove it when editing that artifact. Approval follows the reviewed substantive plan content, not its exact bytes: document typo fixes, clarity edits, and other non-material corrections and carry approval forward. Re-review is required when a correction changes approved scope, behavior, constraints, design decisions, dependencies, acceptance criteria, verification obligations, or risk disposition.

### Artifact and Note Retention ###

Create or persist only artifacts strictly needed by the request or an applicable review gate. Keep role reviews and intermediate notes in the active conversation or a consolidated review record; do not create a separate file for each role or activity by default. Before finishing, remove temporary artifacts created by the panel for the current task that are no longer needed. Do not remove pre-existing or user-owned files during routine cleanup.

Example planning request:

> Create and save a migration plan for the requested redesign. Apply Multi-Agent Governance through the separate plan-approval gate, review all proposed changes, and divide the work into identified implementation units with dependencies and verification paths. Include the handoff context and approval evidence. Do not implement the migration.

Example later execution request:

> Execute unit U2 from the approved migration plan. First verify the plan revision, current repository assumptions, and U1 completion evidence. Apply normal code-change governance to U2, refresh plan approval if material changes are needed, and record completion and verification evidence for the next agent.
