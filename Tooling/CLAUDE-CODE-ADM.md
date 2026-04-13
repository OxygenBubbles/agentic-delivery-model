# Claude Code and Agent SDK Ecosystem Readiness for the ADM

> **Alpha.** This tooling guidance reflects the current state of Claude Code and the Agent SDK, and will be updated as tooling matures and practical experience grows.

This document assesses how Claude Code and the Anthropic Agent SDK can support the Agentic Delivery Model across the full delivery lifecycle: requirements, execution, governance, compliance and release. It identifies where these tools are ready, where gaps exist and where complementary tooling is needed.

It is a readiness assessment, not a product endorsement. Organisations adopting Claude Code and the Agent SDK should use it to understand what is available today and where to plan for integration or complementary tooling.

Claude Code and the Agent SDK plug into whatever development platform the organisation already uses. For platform-specific guidance, refer to the companion assessments for [Microsoft](MICROSOFT-ADM.md), [GitHub](GITHUB-ADM.md), [GitLab](GITLAB-ADM.md), [AWS](AWS-ADM.md) or [Atlassian](ATLASSIAN-ADM.md).

## 1. The ADM Delivery Lifecycle

The ADM needs tooling support across five areas:

1. **Requirements and intent design.** Capturing business intent, running structured discovery, producing Intent Specifications.
2. **Agent execution.** Autonomous agents implementing, testing and documenting code changes against approved specifications.
3. **Governance and human review.** Pull request review, verification, validation and approval by people at the Governance Tier.
4. **Identity, compliance and audit.** Named agent identities, scoped permissions, provenance trails and regulatory evidence.
5. **Release, operations and feedback.** CI/CD pipelines, environment management, monitoring and operational feedback loops.

The assessment below maps Claude Code and Agent SDK capabilities against each area.

## 2. Requirements and Intent Design

**Readiness: Partial.**

Claude Code can assist in drafting Intent Specifications by analysing existing codebase context, architecture documents, design decisions and conversation history. It can extract requirements from unstructured sources and structure them into the ADM's Intent Specification template.

The Model Context Protocol (MCP) extends Claude Code's reach to external systems. Through MCP connections, Claude Code can read from Jira, Confluence, Slack, GitHub Discussions and other business systems. This enables a Requirements Agent configured within Claude Code to gather context across tools without copying data manually.

However, Claude Code is not a requirements discovery platform. It assists in structuring and documenting requirements but does not own the discovery workflow. The organisation's existing requirements tools (Jira, Azure DevOps, Linear, etc.) remain the authoritative backlog platform.

**Gaps.** Claude Code cannot run structured stakeholder discovery sessions on its own. It requires stakeholders to provide context (documents, conversation transcripts, design docs) as input. There is no native "discovery session" mode. Integration with an organisation's backlog tool (through MCP or direct API) is needed to write Intent Specifications as work items, and custom conventions must be established to indicate the specification is ready for agent execution.

## 3. Agent Execution

**Readiness: Strong.**

Claude Code is a fully autonomous coding agent. It operates directly against a Git repository, understands the codebase, plans changes, implements them, runs tests and creates pull requests. It works on isolated feature branches, never committing directly to protected branches.

Each Claude Code session includes:

- full read and write access to the codebase (within configured scope)
- ability to run test suites, linters and build tools
- integration with Git and GitHub/GitLab/Azure DevOps APIs
- session logs and full provenance of every action taken

The Anthropic Agent SDK extends this capability further. A parent agent can spawn managed sub-agents, each configured with:

- a specific role and context (implementation, testing, security analysis, documentation)
- scoped tools and permissions via MCP
- isolated execution environment
- ability to read and write code, run commands and create artefacts

This maps directly to the ADM's concept of agent roles as configurations of a general-purpose model, not capability limitations.

**Gaps.** Claude Code and the Agent SDK have no native CI/CD integration. They generate code changes and create pull requests. All downstream activity (build verification, test execution, deployment, monitoring) depends on the organisation's existing CI/CD platform (GitHub Actions, Azure Pipelines, GitLab CI/CD, Jenkins, etc.) and must be triggered through existing pull request workflows.

## 4. Agent Orchestration and Coordination

**Readiness: Strong.**

The Agent SDK's managed agent pattern is the closest implementation of the ADM's Scheduling Agent concept. A parent agent orchestrates multiple sub-agents, each with specialised context and scoped tools:

- a Requirements Agent gathers context from business systems via MCP
- an Implementation Agent translates specifications into code
- a Testing Agent runs test suites and validates behaviour
- a Security Agent performs static analysis and scans
- a Documentation Agent updates relevant docs
- a Review Agent formats code and prepares pull requests

The parent agent sequences their work (serial or parallel), routes technical ambiguities between agents (agent-to-agent resolution) and escalates intent-level questions to humans. Each sub-agent runs in an isolated sandbox with specific tools and permissions configured through MCP.

MCP (Model Context Protocol) is the open standard for tool access. An MCP server wraps external systems (GitHub API, Jira, Slack, databases, internal services) and exposes only the tools a specific agent needs. This provides both capability and governance: each agent role can be configured with precise tool access.

**Gaps.** The Agent SDK is new. Practical patterns for coordinating multiple agents through complex delivery workflows are still being established. Documentation and examples for agent-to-agent communication patterns and escalation handling are evolving. Organisations will need to develop and test their own orchestration logic.

## 5. Governance and Human Review

**Readiness: Partial.**

Claude Code creates pull requests that route to whatever review surface the organisation uses (GitHub, GitLab, Azure DevOps). The governance controls themselves (required reviewers, branch protections, approval workflows, status checks) belong to the source control platform, not to Claude Code.

Claude Code provides full provenance: session logs show every action, decision and reasoning. This supports the Verification Engineer's review process but does not replace the need for a pull request review surface and approval workflow.

For teams using GitHub, the experience is seamless: Claude Code creates PRs, humans review them with all standard GitHub controls (comments, suggestions, dismissals, required approvals). For organisations using other platforms, Claude Code's PR creation integrates with their native workflows.

**Gaps.** Claude Code does not own the governance layer. It is the execution tool, not the review infrastructure. The organisation must have a pull request workflow and approval process already in place. Additionally, there is no consolidated governance dashboard showing all pending agent work (specifications in flight, PRs awaiting review, blocked items, escalations) across the tools Claude Code uses. Most organisations will need to build or configure this visibility layer.

## 6. Identity, Compliance and Audit

**Readiness: Partial.**

Claude Code sessions are scoped to a specific Git repository and branch. Access is controlled through the organisation's existing Git credentials (GitHub tokens, Azure DevOps PATs, GitLab tokens, etc.). A Claude Code session runs with the identity and permissions of the authenticated user or service account.

MCP provides explicit tool boundaries. An MCP server defines precisely which operations an agent can perform on an external system. For example, an MCP server for Jira might expose "read work items" and "post comments" but not "delete work items" or "modify permissions". This scoping is explicit and auditable.

However, Claude Code does not provide enterprise identity management. It does not create or manage named service accounts, define role-based access controls, enforce conditional access policies or aggregate audit logs across systems. These capabilities depend on the organisation's surrounding infrastructure (GitHub Enterprise with Entra ID integration, native platform SSO, log aggregation tools, etc.).

Session audit logs are available but are not automatically forwarded to a centralised audit system. Compliance teams must configure forwarding if required.

**Gaps.** For regulated environments, audit trail completeness is a gap. Claude Code provides full session logs. The organisation's Git platform (GitHub, GitLab, Azure DevOps) provides additional commit and PR logs. External systems accessed through MCP provide their own audit logs. Joining these into a single provenance chain from specification to deployed code requires deliberate integration and is not automatic.

## 7. Release, Operations and Feedback

**Readiness: Not available.**

Claude Code has no CI/CD capability, no deployment machinery, no monitoring, no alerting and no operational feedback loops. It generates code changes. Everything downstream is owned by the organisation's existing infrastructure.

When a Claude Code session creates a pull request, the subsequent workflow is entirely outside Claude Code:

- CI/CD platform (GitHub Actions, Azure Pipelines, etc.) detects the PR and runs build, test and quality checks
- Human or automated approval gates determine if the change can merge
- Deployment platform executes the merge and releases the change
- Monitoring tools (Datadog, New Relic, Azure Monitor, etc.) track the change's effect in production
- Feedback (metrics, errors, logs) flows back to humans and agents

Claude Code does not participate in this workflow. It is a code generation and change submission tool, not a deployment or operations tool.

**Gaps.** Complete. Release, operations and feedback are entirely outside Claude Code's scope.

## 8. Ecosystem Readiness Summary

| ADM Area | Capability | Readiness | Key Gap |
|---|---|---|---|
| Requirements and intent design | Claude Code + MCP for context gathering | Partial | Not a discovery platform. Requires integration with backlog tooling |
| Agent execution (coding) | Claude Code, Agent SDK with sub-agents | Strong | No native CI/CD. Depends on external platform |
| Agent orchestration | Agent SDK managed agents, MCP tool scoping | Strong | Patterns for complex orchestration still evolving |
| Governance and review | Pull requests via native platform APIs | Partial | No consolidated governance dashboard |
| Identity and compliance | Git identity, MCP tool scoping | Partial | No enterprise identity management. Cross-system provenance requires integration |
| Release and operations | Not available | Not available | Entirely dependent on external CI/CD and ops platforms |

## 9. Reference Operating Pattern

A practical Claude Code and Agent SDK centred ADM pattern:

1. Product Owner and Intent Architect define business intent through existing channels (Confluence, Slack, Jira, etc.).
2. A Requirements Agent (Agent SDK) reads context from these systems via MCP and drafts an Intent Specification.
3. Product Owner and Intent Architect review and approve the specification in the backlog tool.
4. The specification is marked Ready for agent execution.
5. A Scheduling Agent (parent Agent SDK) reads the specification and spawns task-specific sub-agents.
6. An Implementation Agent reads the specification and codebase, plans changes and begins implementation on an isolated branch.
7. A Testing Agent runs the test suite against each change and provides feedback.
8. A Security Agent performs static analysis and scanning.
9. A Documentation Agent updates relevant docs and architecture diagrams.
10. A Review Agent formats the code, prepares commit messages and creates a pull request.
11. The Scheduling Agent orchestrates agent-to-agent communication for any technical issues (implementation asks testing to validate a specific scenario).
12. The pull request appears in GitHub/GitLab/Azure DevOps with full session provenance available.
13. The Verification Engineer reviews the PR at the daily governance session with branch protections, required approvals and status checks as controls.
14. On approval and merge, the organisation's CI/CD platform (GitHub Actions, Azure Pipelines, etc.) runs builds, tests and deployments.
15. Monitoring tools provide feedback on the deployment.
16. The Scheduling Agent can be configured to monitor feedback and raise new specifications for follow-up work if needed.

## 10. Key Decision Points for Adopters

Organisations assessing Claude Code and the Agent SDK for ADM adoption should consider:

**Agent configuration and MCP scope.** Each agent role needs precise configuration: context (documents, codebase excerpts, role description), tools (via MCP servers) and constraints (branch naming, code review standards, testing requirements). Plan for defining these configurations per agent role and per project. MCP servers need to be written for each external system the agents access.

**Backlog and specification tooling.** Claude Code does not own requirements discovery. The organisation's existing backlog tool (Jira, Linear, Azure DevOps, etc.) remains authoritative. Plan for integrating Claude Code with that tool via MCP so agents can read specifications and update status.

**Orchestration complexity.** Multi-agent coordination is powerful but requires careful design. Plan for testing your orchestration logic: how agents sequence work, how they escalate technical issues, when and how the Scheduling Agent escalates to humans. Start simple and iterate.

**Governance dashboard.** There is no native dashboard showing all pending agent work. Organisations will need to build visibility into specification status, PR queue, blocked items and escalations. This might be a custom integration, a GitHub Projects board, a Jira dashboard or a purpose-built tool.

**CI/CD integration.** Claude Code creates pull requests that trigger your existing CI/CD workflows. Ensure your pipelines are configured for frequent merges (the ADM assumes daily releases). Ensure approval gates at each environment are clear and fast.

**Provenance and compliance.** Audit trail requirements vary. Claude Code provides session logs. Your Git platform provides commit logs. External systems accessed via MCP provide their own logs. If regulations require a complete end-to-end audit trail from specification to deployed code, plan for integrating these logs into a centralised system early.

**Operational feedback loops.** Claude Code itself does not monitor deployments or ingest operational feedback. However, you can configure the Scheduling Agent to read from monitoring tools via MCP (Datadog, Prometheus, CloudWatch, etc.) and raise new specifications for follow-up work based on production metrics or errors. This closes the feedback loop from operations back to agents.

**Cost and model governance.** Anthropic provides API usage tracking that shows token consumption and cost per session. However, mapping cost to individual Intent Specifications requires custom instrumentation. Organisations should tag agent sessions with specification identifiers and aggregate cost data from Claude API usage, MCP tool invocations and external platform costs to achieve the ADM's cost-per-specification tracking.

## 11. Source Notes

This assessment is based on public documentation from:

- [Claude Code Overview](https://docs.anthropic.com/en/docs/claude-code/overview)
- [Anthropic Agent SDK](https://docs.anthropic.com/en/docs/agents/agent-sdk)
- [Model Context Protocol](https://modelcontextprotocol.io/)
- [Claude Code Best Practices](https://docs.anthropic.com/en/docs/claude-code/best-practices)

Claude Code and the Agent SDK are evolving rapidly. This assessment should be revisited as capabilities mature and practical delivery patterns emerge.
