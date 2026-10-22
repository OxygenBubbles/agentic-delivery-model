# The Agentic Delivery Model (ADM)

**An open methodology for software delivery where agents do much of the implementation under controls that people set and hold.**

Software delivery frameworks were designed around the assumption that people execute the work and other people coordinate it. Agents built on language models now draft, implement and check much of the work, continuously and at machine speed, and they also take specifications literally, fill gaps with plausible assumptions and optimise locally. An operating model built for the first assumption throttles agents to the pace of the people around them or lets their output flow unchecked into production.

The Agentic Delivery Model is a practical, implementable operating model for organisations where agents execute much of the implementation and people decide intent, validate it, verify the output and authorise every production change. It is generic and vendor-neutral, and it is written to be adopted by a single product team or a whole technology function.

## Who is this for?

- CTOs and CIOs deciding how agents will be governed in software delivery
- Engineering leaders redesigning a delivery operating model around one flow and an autonomy ladder
- Assurance, risk and audit functions that need to see how an agent-produced change was controlled
- Teams already using coding agents at scale who need the controls around them

## Read the model

📄 **[The Agentic Delivery Model version 2.0](MODEL.md)** The core document. It covers the principles, the intent as the unit of work, the flow of eleven steps and the deployment approaches. It then covers the autonomy ladder and the controls on agents, the engineering platform, the accountabilities, the evidence trail and the measures.

📘 **[Adopting the ADM](ADOPTION.md)** Practical guidance on adoption phases, brownfield environments, team sizing, how people work day to day, career paths, release and working patterns, business readiness and workforce change. It includes a forecast of how delivery roles are likely to change as agents take on more of the work.

🔍 **[The journey of a change](JOURNEY.md)** A worked example that follows one fictional change through the eleven steps, showing who decides what at each step, the agent's part at its level and the evidence left behind.

Supporting documents:

- [ADM Profiles](Profiles/README.md). Example profiles for the posts that hold each of the model's accountabilities, each with a forecast of how the post is likely to change, and a starter set of agent profiles.
- [Tooling notes](Tooling/README.md). How the model runs on GitHub, Microsoft, GitLab, AWS, Atlassian, Anthropic, OpenAI, Google and Factory, with one note per toolchain checked against the vendor's documentation in October 2026.

## Core concepts

**One flow for every change.** Eleven steps carry every change from an approved request to a confirmed outcome: Intent, Gate (where triggered), Workspace, Build, Verify, Review, Identify, Promote, Authorise, Deploy and Confirm and learn. Differences between platforms sit in a context profile and a deployment approach, not in a separate process.

**The intent is the unit of work.** A specification complete enough to be executed without further explanation from its author, validated before implementation and built only from its published version. Acceptance examples are owned by the business and drafted separately from the implementation.

**An autonomy ladder.** Five levels set what an agent may do at each step for each class of change. They run from L0 (not used) through L1 (advise), L2 (draft) and L3 (execute under gate) to L4 (execute in production within a scope). Levels are set centrally, rise only on an evaluation and a recorded period at the level below, and fall without a meeting. Production deployment stays at L0.

**Deploy what passed the checks.** Each execution unit becomes a release candidate that is attested once and promoted unchanged, with the release commit checked and authorised where a platform promotes source, and with a recovery position established before authorisation. Three deployment approaches, artefact-based, state-based and migration-based, give every kind of change a release candidate and a recovery position.

**Accountabilities, not posts.** The model names what has to be decided and owned, from the product owner's request decision to the release authority's go or no-go, and an adopter maps each accountability to the posts it has.

**Evidence created by the systems that do the work.** Every record in the decision trail is produced where the work happened and carries the reference to the next. A line of deployed code therefore traces back to the request, and a request traces forward to the release.

## Principles

1. **Intent over instruction.** People define what needs to exist and why, with enough precision that an agent can execute against it.
2. **One flow for every change.** Platform differences live in the context profile and the deployment approach, not in a separate process.
3. **Verification and validation serve different purposes.** Verification confirms the output matches the intent and the standards. Validation confirms the intent serves the business need.
4. **No one approves their own work.** No person, agent or model approves a change it produced or attests to the checks on it.
5. **A person authorises every production change.** Deployment to production stays at L0 and a named person holds the authority.
6. **Deploy what passed the checks.** The release candidate is attested once and promoted unchanged, with the release commit checked where source is promoted.
7. **Each standard has one source and coherence has an owner.** Pipelines and agents read the same published standard, and the system steward owns coherence.
8. **Autonomy is set centrally and earned by evidence.** A level rises on an evaluation and falls without a meeting.
9. **Evidence is created by the systems that do the work.** Records are produced where the work happened, without transcription.
10. **An agent stops rather than guesses.** A run halts at a stop condition and records its state so the work resumes.

## Status

This branch holds the version 2.0 pre-release of October 2026 for review. Version 2.0 restates the model around one flow, an autonomy ladder and accountabilities rather than posts, and version 1.1 remains the current release until version 2.0 is published. The [changelog](CHANGELOG.md) lists the changes since version 1.

## Contributing

Contributions are welcome. See [CONTRIBUTING.md](CONTRIBUTING.md) for guidance.

## Author

**Dan Simms**, CIO / CTO and founder of [Oxygen Bubbles](https://oxygenbubbles.com).

[LinkedIn](https://www.linkedin.com/in/dansimms)

## Licence

[Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE).
