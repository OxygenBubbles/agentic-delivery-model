# ADM Profiles

This folder contains detailed role profiles for the core people roles in the Agentic Delivery Model and a selected starter set of structured agent role profiles.

These profiles are examples, not a mandated standard. They are written to be adapted. Organisations should tailor them to their domain, control model, regulatory context, tooling estate and cost constraints.

## How to read these profiles

The people role profiles are operating role definitions, not HR job descriptions. They describe why the role exists, what outcomes it is accountable for, which decisions it owns, how it works with adjacent roles and how it fails when the operating model is weak.

The agent role profiles are structured Markdown files with YAML front matter followed by operating instructions. They are written to be readable by people and adaptable into agent configuration files. Each profile combines role intent, required context, decision rules, guardrails, escalation triggers, evidence expectations and handoff rules.

## Design principles

- Mission before task list. Each profile starts with the role's purpose in the ADM, not just a set of activities.
- Decision rights are explicit. Good operating models are clear about who decides, not just who contributes.
- Guardrails matter as much as capability. This is especially important for agents.
- Non-goals are part of the design. A role profile should say what the role must not absorb.
- Measures should reward good system behaviour, not just local throughput.
- Profiles must be usable. They should help a team assign accountability, shape onboarding and configure agents in practice.

## Human role profiles

- [Intent Architect](intent-architect.md)
- [Verification Engineer](verification-engineer.md)
- [System Steward](system-steward.md)
- [Design Steward](design-steward.md)
- [Product Owner (Evolved)](product-owner-evolved.md)

## Structured agent role profiles

This is a selected starter set, not the full ADM agent roster.

- [Requirements Agent](requirements-agent.md)
- [Implementation Agent](implementation-agent.md)
- [Review Agent](review-agent.md)
- [Design Agent](design-agent.md)
- [Security Agent](security-agent.md)
- [Documentation Agent](documentation-agent.md)
