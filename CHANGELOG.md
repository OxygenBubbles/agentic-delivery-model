# Changelog

All notable changes to the Agentic Delivery Model will be documented in this file.

## [2.0.0-pre.1] - 2026-10-22

A pre-release of version 2.0, published on the pre-release/v2.0 branch for review. Version 1.1 remains the current release. The list below sets out every change since version 1.

### The model

- MODEL.md is rewritten around one flow of eleven steps that carries every change from an approved request to a confirmed outcome: Intent, Gate (where triggered), Workspace, Build, Verify, Review, Identify, Promote, Authorise, Deploy and Confirm and learn (section 3). Gates apply where the intent triggers them, and nine conditions that suspend or divert work each have a defined re-entry route. Threshold and policy exceptions have named granting authorities, owners and expiries, and six never-waived controls bind every authority (3.2). An expired exception blocks the release of any candidate that still depends on it. The rhythm has no timed agendas (3.3).
- Ten principles replace eight (section 1). One flow for every change, no one approves their own work, deploy what passed the checks, autonomy set centrally and earned by evidence and an agent stops rather than guesses are added, and the principles on architectural coherence and measured technical debt are merged.
- The request replaces the Intent Brief and the intent replaces the Intent Specification, with ten elements that are always answered (2.1 and appendix A).
- The request decision has four outcomes, with a person deciding on business merit, an agent supplying evidence and approved work capped against validation and review capacity (2.2).
- Validation before implementation replaces the Definition of Ready. It is proportionate to the risk tier, uses three forms of assessment (ratify, decide and break) and has acceptance examples drafted in a run separate from implementation (2.3). Acceptance at step 11 replaces the Definition of Done.
- Decomposition uses versioned contracts and invalidates dependent work when a contract changes (2.4).
- Risk tiers are defined by the effect of a change, with the distribution of tiers as a control (2.5).
- Three deployment approaches (artefact-based, state-based and migration-based), release candidates, attestation, release commits and three recovery positions are added (section 4). The scope now covers configuration and supplier-produced changes that the organisation versions and promotes, replacing the boundary set by whether code is written (section 11).
- The autonomy ladder runs from L0 to L4 and is assigned per step and class of change, with an organisation-level ceiling (5.1). Seven agent configuration elements and three permissions (5.2), nine controls at every level mapped to the threats they answer (5.3), the evaluation and approval that raise a level and the immediate reduction (5.4) and six trust boundaries (5.5) are added.
- The engineering platform is run as an internal product, with standards in enforceable form, the platform contract and a manual fallback for every automated step (section 6).
- Accountabilities replace the four new posts of version 1 (section 7). The titles Intent Architect, Verification Engineer, System Steward and Design Steward remain available as post names.
- The decision trail is set out as a table of records, systems of record and owners (8.1), and thirteen measures, each with what it should prompt, replace the seventeen metrics (8.2).
- A section on working with existing frameworks keeps the organisation's change procedure and produces the change record from the flow (section 9).
- Templates for the intent, the context profile and the agent profile, the coherence register and a glossary of defined terms form appendices A to E.

### Removed from the model

- The three cadence tiers as timed meetings, the Scheduling Agent and its overlap handling, the ten-role agent roster with agent identities, the seventeen-metric table, the sector overlay templates and the services-firm section. The illustrative organisation comparison remains in appendix A of ADOPTION.md, with its figures identified as the model's own illustrations.

### Companion documents

- JOURNEY.md replaces KESTREL.md. It follows one standard-tier change through the eleven steps and walks the decision trail backwards.
- ADOPTION.md is rewritten in the register of the model, with restatements of the model replaced by references to it. Section 3 now forecasts how delivery roles are likely to change as agents take on more of the work, paced by the autonomy levels rather than by dates, and the sections on career paths and workforce transition follow it.
- The Profiles folder gains profiles for the business analyst and the test engineer (holding the validators accountability), the release manager (holding the release authority), the platform owner, the security lead and the data owner. Each profile for a post includes a forecast of how the post is likely to change. The agent profiles carry every field of the agent profile template in appendix C, with approvals as set out in 5.4, and each serves one step at its starting level.
- The Tooling folder holds one note per toolchain, each checked against the vendor's documentation on 22 October 2026 and mapped onto the eleven steps, the autonomy levels, the nine controls and the six trust boundaries. The notes cover GitHub, Microsoft, GitLab, AWS, Atlassian, Anthropic, OpenAI, Google and Factory, and the OpenAI and Google notes are new.
- The readiness assessments and implementation guides of version 1 are merged into the notes for their toolchains, retiring GITHUB-COPILOT-CLAUDE-ADM.md, MICROSOFT-COPILOT-ADM.md, GITLAB-DUO-ADM.md, AWS-KIRO-ADM.md, ATLASSIAN-ROVO-ADM.md, CLAUDE-CODE-ADM.md and CLAUDE-AGENTIC-LOOPS-ADM.md.
- README.md and CONTRIBUTING.md are updated for version 2.0.

### Known gaps

- No starter agent profile yet serves step 2, Gate, step 5, Verify, step 8, Promote, step 9, Authorise, or step 11, Confirm and learn.
- No profile is provided for the post that holds the accountable executive accountability.

## [1.1.0] - 2026-06-21

### Implementation guides

- Added hands-on implementation guides to Tooling for seven toolchains: GitHub Copilot with Claude, Claude agentic loops and dynamic workflow, Factory.ai droids, AWS Kiro, the GitLab Duo Agent Platform, the Microsoft stack and Atlassian Rovo. Each maps every step of the model onto the tool: where intent and standards are stored, how each agent role is configured and the end-to-end delivery cycle.

### Currency refresh

- Updated AWS readiness for Kiro replacing Amazon Q Developer as an autonomous, spec-first agent. Moved Agent Execution from Partial to Strong.
- Updated GitHub readiness for Agent HQ multi-agent mission control and the Control Plane.
- Refreshed Microsoft readiness for GitHub Agent HQ and Agent 365 general availability.
- Refreshed Claude Code readiness for Dynamic Workflows (Claude Opus 4.8) and GitLab for Duo Agent Platform general availability.
- Positioned the model relative to spec-driven development (Section 1).
- Added AGENTS.md as the emergent cross-tool standard (Appendix C.4) and EARS as an acceptance-criteria alternative (Appendix B.2).
- Strengthened agent security guidance with the OWASP Top 10 for Agentic Applications and recent MCP supply-chain incidents (Appendix C.9).

### Model refinements

- Added risk-tiered verification to the Verification Engineer role (Section 6.2).
- Added guidance on raising specification throughput: pattern libraries, specification debt and agent assistance for the Intent Architect (Section 6.7.2).
- Added three metrics: Specification Defect Rate, Change Failure Rate and Time to Restore (Section 14.1).
- Noted that agent output is non-deterministic and that provenance, not reproducibility, is the control (Section 13.1).
- Added a fleet halt control alongside the cost circuit breaker (Appendix C.8).
- Noted that orchestration is the least uniformly supported part of the model (Section 9).
- Added brownfield legibility and a measured-pilot protocol to the transition guidance (Section 16).
- Added talent scarcity as a limitation (Section 19).

## [1.0.1] - 2026-04-13

### Tooling ecosystem readiness assessments

- Rewrote all six ecosystem readiness notes (Microsoft, GitHub, GitLab, AWS, Atlassian, Claude Code) to a consistent assessment format: lifecycle area ratings, gap analysis, summary table, reference operating pattern and key decision points.
- Added Claude Code and Agent SDK as a new tooling scenario.
- Assessed Agent Orchestration as a distinct capability across all platforms.
- Downgraded AWS Agent Execution from Strong to Partial (IDE-based, not autonomous).
- Tightened Atlassian Requirements assessment (Rovo described accurately as search and summarisation, not a discovery platform).
- Added cost and model governance as a decision point across all platforms.
- Added cross-references between companion notes.

## [1.0] - 2026-04-08

### Initial release

- Complete methodology covering principles, roles, artefacts, governance, cadence and metrics.
- Three-tier cadence model (Execution, Governance, Strategic).
- Four redefined human roles: Intent Architect, Verification Engineer, System Steward, Product Owner.
- Ten agent roles with governance framework.
- Intent Brief and paired discovery pattern for requirements.
- Companion adoption guide covering team structure, working patterns, transition planning and workforce change.
- Detailed human and agent role profiles.
- Enterprise tooling implementation notes.
- Worked example (Project Kestrel).
- Governance framework for regulated environments.
