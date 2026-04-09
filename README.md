# The Agentic Delivery Model (ADM)

**An open methodology for software delivery in the age of autonomous AI agents.**

Every mainstream framework for enterprise software delivery was designed around a single assumption: that people execute the work and other people coordinate that execution. Agile, Scrum, SAFe, Kanban and their variants all encode human constraints into their structure.

That assumption is now wrong.

AI agents can complete a user story in 90 minutes. They work around the clock. They don't need standups to stay aligned. But they hallucinate, struggle with ambiguity and accumulate technical debt at unprecedented rates.

The Agentic Delivery Model is a practical, implementable operating model for organisations where AI agents execute the majority of implementation work and people focus on intent, governance and validation.

## Who is this for?

- CTOs and CIOs navigating the transition to agentic development
- Engineering leaders redesigning delivery operating models
- Technology leaders who need to understand the cost and headcount implications
- Teams already using Claude Code, GitHub Copilot, Cursor or similar tools at scale

## Read the model

📄 **[The Agentic Delivery Model v1.0](MODEL.md)** The core methodology: principles, roles, artefacts, governance, cadence and metrics.

📘 **[Adopting the ADM](ADOPTION.md)** Practical guidance on adoption strategy, team structure, working patterns, transition planning, workforce change and what the model means in practice.

🔍 **[Worked Example: Project Kestrel](KESTREL.md)** A fictional but realistic walkthrough of the ADM in action, with example artefacts and a typical week.

Supporting documents:

- [ADM Profiles](Profiles/README.md). Detailed role profiles for people and structured agent role profiles.
- [Enterprise Tooling Scenarios](Tooling/README.md). Companion notes on enterprise toolchains that could support the ADM.

## Core concepts

**Three-tier cadence** replacing the traditional sprint:

| Tier | Cadence | Purpose |
|------|---------|---------|
| Execution | Continuous (24/7) | Agents work against the prioritised backlog in isolated branches |
| Governance | Daily | People review agent output, approve PRs, provide clarification |
| Strategic | Weekly | Architecture review, coherence assessment, business outcome validation |

**Redefined roles:**

- **Intent Architect.** Defines what needs to be built with enough precision for autonomous execution.
- **Verification Engineer.** Verifies agent output against the Intent Specification and structural consistency.
- **System Steward.** Maintains architectural coherence and owns technical debt management.
- **Product Owner (evolved).** Accountable for specification precision, participates in agent-led requirements discovery.

## Principles

1. **Intent over instruction.** People specify intent. Agents determine implementation.
2. **Continuous execution, governed checkpoints.** The cadence of review is decoupled from the cadence of execution.
3. **Verification and validation serve different purposes.** Verification confirms output matches the specification. Validation confirms the specification serves the business intent.
4. **Architectural coherence is a first-class concern.** Agents optimise locally. People maintain system-level coherence.
5. **Technical debt is measured, not assumed.** Volume is not value.
6. **Precision of intent determines quality of output.** Definition of Ready is the most critical quality gate. Precision is iterative, not absolute.
7. **Human accountability is non-negotiable.** Agents execute. People are accountable for what agents produce.
8. **Transparency of provenance.** All agent-generated work must be traceable.

## Status

Version 1.0. The model will evolve as tooling matures and organisations accumulate experience.

## Contributing

Contributions are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) for guidance.

## Author

**Dan Simms**, CIO / CTO and founder of [Oxygen Bubbles](https://oxygenbubbles.com).

[LinkedIn](https://www.linkedin.com/in/dansimms)

## Licence

[Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE).