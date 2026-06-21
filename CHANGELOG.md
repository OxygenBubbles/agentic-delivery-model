# Changelog

All notable changes to the Agentic Delivery Model will be documented in this file.

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
