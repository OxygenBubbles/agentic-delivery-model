# ADM Profiles

This folder contains example profiles for the posts that hold the accountabilities named in section 7 of the Agentic Delivery Model and a starter set of agent profiles in the form set out in Appendix C of the model.

These profiles are examples, not a mandated standard. They are written to be adapted. Organisations should tailor them to their domain, control model, regulatory context, tooling estate and cost constraints.

## How to read these profiles

The profiles for people are operating definitions of a post that holds an accountability, not HR job descriptions. They describe why the accountability exists, what outcomes it covers, what decisions it owns, at what steps of the flow it acts, how it works with adjacent accountabilities and how it fails when the operating model is weak.

The agent profiles are structured Markdown files with YAML front matter followed by operating instructions. They are written to be readable by people and adaptable into agent configuration (section 5.2 of the model). The front matter records the step and class of change served, the model and version, the region, the autonomy level, the tools and scopes, the stop conditions, the evaluation reference and the approval record. The body gives the agent's purpose, required context, decision rules and handoff rules.

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

This is a starter set. The model has no fixed roster of agents. An adopter writes one profile for each step and class of change it delegates, and the examples here cover the steps most often delegated first. The names are kept from version 1.1 for continuity.

- [Requirements Agent](requirements-agent.md), drafting the intent at step 1, Intent, at L2
- [Implementation Agent](implementation-agent.md), building in a workspace at step 4, Build, at L2, with pipeline jobs at L3 under a gate
- [Review Agent](review-agent.md), advising the verifier at step 6, Review, at L1
- [Design Agent](design-agent.md), advising on user-facing changes at step 6 at L1
- [Security Agent](security-agent.md), advising on security findings at step 6 at L1, with the scans themselves run as deterministic checks at step 5
- [Documentation Agent](documentation-agent.md), updating documentation within the change at step 4 at L2

No profile yet serves the readiness run at step 9, Authorise (L3, with the decision a person's) or the telemetry correlation at step 11, Confirm and learn (L2, with acceptance at L0).
