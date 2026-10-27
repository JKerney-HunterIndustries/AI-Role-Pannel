# Law-Driven Governance Ideas

> Exploratory notes only. Not current policy; existing governance remains unchanged.

## Discussion Record

Recorded: 2026-10-27
Baseline branch: `constitutional`
Baseline commit: `c1e532b4f580da5aa47c9ec45c5249a5b6157608`

This record captures discussion against that baseline. The worktree was clean when the baseline was recorded. These notes are exploratory and do not change the current governance behavior or voting policy.

### Current Scope

- Current goal: formalize the existing system's implicit responsibilities and behavior from observed practice.
- Use observed behavior from the existing panel to clarify responsibilities that are currently implicit.
- Keep the current panel behavior unchanged while those responsibilities are defined and evaluated.
- Do not convert roles to law-based contracts, define future panel modes, or adopt a broader law system in this phase. Evaluate the clarified current panel before deciding whether to pursue those directions.
- A well-defined responsibility may later be assigned to a separate agent, separating implementation work from decisions. Defining it does not itself require that separation now.

### Candidate Mediator

See [Communication Boundary](./mediator-orchestrator-laws.md).

## Candidate Voting-System Changes (Not Implemented)

The detailed mediator and vote-handling ideas are maintained in [Mediator/Orchestrator Role Ideas](./mediator-orchestrator-laws.md). If adopted, likely policy consumers include [.github/vote-schema.md](../.github/vote-schema.md), [.github/agent-governance.md](../.github/agent-governance.md), and role-specific contracts under [.github/roles/](../.github/roles/). None of those policy files has been changed.

## Longer-Term Direction (Exploratory)

Explore whether governance could later become law-driven through a set of mandatory laws. This is not the current goal; whether to pursue it depends on evaluating the clarified current system first.

## Possible taxonomy

Categories describe a law's intent, not who it affects or when it applies. Categories may overlap and may need refinement.

- **General laws:** Govern the system as a whole.
- **Role-based laws:** Define a role's duties or constraints.
- **Collaboration laws:** Define how a task is carried out collectively.
- **Interaction laws:** Potentially govern particular exchanges; their distinction from collaboration laws remains open.

Applicability can be specified separately from intent.

## Collaboration

The current task model is a **jury of judges**. Possible future task models:

- AI-human pairing
- AI committee generating ideas

Current collaboration rules appear to cover role-to-role coordination; separate interaction laws may not be needed for the jury model.

## Open role question

For the mediator/worker boundary and open questions, see [Relationship to the worker/implementer](./mediator-orchestrator-laws.md#relationship-to-the-workerimplementer).
