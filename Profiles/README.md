# ADM Profiles

This folder contains example profiles for the posts that hold the accountabilities in section 7 of the Agentic Delivery Model and a starter set of agent profiles. The profiles for posts cover every accountability except the accountable executive. Each agent profile carries every field of the agent profile template in appendix C of the model and follows the permissions in section 5.2 and the approvals in section 5.4.

These profiles are examples, not a mandated standard. They are written to be adapted. Organisations should tailor them to their domain, control model, regulatory context, tooling estate and cost constraints.

## How to read these profiles

The profiles for people are operating definitions of a post that holds an accountability, not HR job descriptions. They describe why the accountability exists, what outcomes it covers, what decisions it owns, at what steps of the flow it acts, how it works with adjacent accountabilities and how it fails when the operating model is weak.

The agent profiles are structured Markdown files with YAML front matter followed by operating instructions. They are written to be readable by people and adaptable into agent configuration (section 5.2 of the model). The front matter carries each field of appendix C under a key named after the field, from the step and class of change served to the suspension conditions. The platform owner completes each value marked as set or recorded by the adopter, and a completed profile grants nothing until it is approved and released under section 5.4 of the model. The body gives the agent's purpose, required context, decision rules and handoff rules.

## Design principles

- Mission before task list. Each profile starts with the role's purpose in the ADM, not just a set of activities.
- Decision rights are explicit. Good operating models are clear about who decides, not just who contributes.
- Guardrails matter as much as capability. This is especially important for agents.
- Non-goals are part of the design. A role profile should say what the role must not absorb.
- Measures should reward good system behaviour, not just local throughput.
- Profiles must be usable. They should help a team assign accountability, shape onboarding and configure agents in practice.

## Profiles for the posts that hold accountabilities

The model keeps the version 1.1 titles as available names for the posts that hold its accountabilities. Each profile names the accountability it holds and the steps of the flow where it acts. Each of these profiles also includes a section on how the post is likely to change as agents take on more of the work, summarising for that post the forecast in [the adoption guide](../ADOPTION.md).

- [Intent Architect](intent-architect.md), holding the intent architect accountability
- [Verification Engineer](verification-engineer.md), holding the verifier accountability
- [System Steward](system-steward.md), holding the system steward accountability, with design coherence unless a Design Steward is appointed
- [Design Steward](design-steward.md), an optional appointment for products with a user-facing surface
- [Product Owner (Evolved)](product-owner-evolved.md), holding the product owner accountability
- [Business Analyst](business-analyst.md), holding the validators accountability for the expression of the need and the behaviour
- [Test Engineer](test-engineer.md), holding the validators accountability for the acceptance examples
- [Release Manager](release-manager.md), holding the release authority
- [Platform Owner](platform-owner.md), holding the platform owner accountability
- [Security Lead](security-lead.md), holding the security lead accountability
- [Data Owner](data-owner.md), holding the data owner accountability

## Agent profiles

This is a starter set, as the model has no fixed roster of agents. An adopter writes one profile for each step and class of change it delegates, and the examples here cover the steps most often delegated first. The names are kept from version 1.1 for continuity.

- [Requirements Agent](requirements-agent.md), drafting the intent at step 1, Intent, at L2
- [Implementation Agent](implementation-agent.md), building in a workspace at step 4, Build, at L2
- [Review Agent](review-agent.md), advising the verifier at step 6, Review, at L1
- [Design Agent](design-agent.md), advising on user-facing changes at step 6 at L1
- [Security Agent](security-agent.md), advising on security findings at step 6 at L1, with the scans themselves run as deterministic checks at step 5
- [Documentation Agent](documentation-agent.md), updating documentation within the change at step 4 at L2

No starter profile yet serves the steps below, where section 3.1 of the model gives an agent a starting level above L0.

| Step | Starting level under section 3.1 of the model |
|---|---|
| 2, Gate | L1, identifying the triggers met and assembling the material |
| 5, Verify | L2 to triage a failing check and L3 to run the pipeline and assemble the evidence |
| 8, Promote | L3, preparing and invoking the approved job in non-production |
| 9, Authorise | L3, computing readiness and assembling the manifest, with the decision a person's |
| 11, Confirm and learn | L2, correlating change with telemetry and drafting the post-incident review, with acceptance at L0 |
