# Atlassian Ecosystem Readiness for the ADM

> **Alpha.** This tooling guidance reflects the current state of the Atlassian ecosystem and will be updated as tooling matures and practical experience grows.

This document assesses how the Atlassian enterprise ecosystem can support the Agentic Delivery Model across the full delivery lifecycle: requirements, execution, governance, release and operations. It identifies where the ecosystem is ready, where gaps exist and where complementary tooling may be needed.

It is a readiness assessment, not a product endorsement. Organisations building on the Atlassian stack should use it to understand what is available today and where to plan for investment or integration.

Atlassian is strongest at requirements and work management. For autonomous agent execution and infrastructure, refer to the companion assessments for [GitHub](GITHUB-ADM.md), [GitLab](GITLAB-ADM.md), [AWS](AWS-ADM.md) or [Microsoft](MICROSOFT-ADM.md).

## 1. The ADM Delivery Lifecycle

The ADM needs tooling support across five areas:

1. **Requirements and intent design.** Capturing business intent, running structured discovery, producing Intent Specifications.
2. **Agent execution.** Autonomous agents implementing, testing and documenting code changes against approved specifications.
3. **Governance and human review.** Pull request review, verification, validation and approval by people at the Governance Tier.
4. **Identity, compliance and audit.** Named agent identities, scoped permissions, provenance trails and regulatory evidence.
5. **Release, operations and feedback.** CI/CD pipelines, environment management, monitoring and operational feedback loops.

The assessment below maps Atlassian capabilities against each area.

## 2. Requirements and Intent Design

**Readiness: Strong.**

Jira is one of the most widely used enterprise work management platforms. Intent Specifications can be represented as Jira work items with custom fields for complexity band, boundaries and integration points. Jira's workflow engine supports the Ready state and clarification flows the ADM requires.

Confluence provides documentation and knowledge management across the organisation. Rovo (Atlassian's AI assistant) can surface context from Jira and Confluence, which supports requirements gathering by making existing knowledge searchable. However, Rovo is a search and summarisation tool, not a structured discovery platform. It does not run facilitated stakeholder sessions or enforce interview templates in the way that a dedicated agent-building platform like Copilot Studio can.

Jira work items support a full audit trail of specification creation, review and approval. Boards and queries provide visibility into the backlog and specification readiness.

**Gaps.** Jira work items are not structured around the Intent Specification template out of the box. Custom field configuration and workflow customisation are needed to represent the ADM artefact structure precisely. This is achievable but requires deliberate setup. Rovo's context surfacing is useful but does not replace a structured Requirements Agent that can facilitate discovery, enforce templates and draft specifications autonomously.

## 3. Agent Execution

**Readiness: Partial.**

Rovo Dev provides development assistance through the CLI and IDE integration. Rovo Dev CLI can work in the terminal alongside developer tools and MCP (Model Context Protocol) connectivity allows access to external tools and data sources.

Rovo Dev supports code review in Bitbucket and GitHub, providing code analysis and review suggestions. However, Rovo Dev is not yet a fully autonomous coding agent in the sense of GitHub Copilot coding agent. It assists with code review and development tasks but does not autonomously implement code changes against specifications.

For Atlassian-centred organisations, Rovo Dev represents the primary agent execution capability today. Its limitations are the largest gap in Atlassian's ADM readiness.

**Gaps.** Rovo Dev is a development assistant, not an autonomous implementation agent. It does not independently take a specification and produce complete, ready-to-merge code changes. This is the biggest gap in the Atlassian ecosystem for ADM adoption. Many Atlassian-centred organisations will need to introduce a separate autonomous coding agent or integrate with GitHub Copilot coding agent running on a different repository platform.

## 4. Agent Orchestration and Coordination

**Readiness: Limited.**

Atlassian does not provide multi-agent orchestration. Rovo Dev is a single assistant, not an agent fleet. MCP connectivity allows Rovo Dev to access external tools and data, but this is tool access, not agent coordination.

Orchestrating a Requirements Agent with Rovo Dev with additional specialised agents (testing, security analysis, documentation) requires integration across multiple platforms and custom orchestration logic.

**Gaps.** There is no single orchestration surface within Atlassian for managing multiple agents. Rovo Dev operates as a standalone assistant rather than as a node within a coordinated agent fleet. Organisations wanting to implement the ADM's pattern of a Scheduling Agent coordinating multiple execution agents will need to build this orchestration outside Atlassian or integrate with a dedicated orchestration platform.

## 5. Governance and Human Review

**Readiness: Partial.**

Bitbucket provides pull request review with approval rules and merge checks. Code reviews can be enforced at the branch level and audit trails are captured for compliance. However, many Atlassian-centred organisations use GitHub or GitLab for source control rather than Bitbucket, which means governance flows may span multiple platforms.

Jira provides the workflow governance layer for specifications. Status tracking, approval workflows and audit history are all available through Jira work items and can be linked to Bitbucket pull requests.

**Gaps.** The ADM's Governance Tier needs a consolidated view of all pending agent work: pull requests awaiting review, specifications in progress, blocked items and escalations. If an organisation uses Bitbucket for some repositories and GitHub for others, governance visibility fragments across platforms. Atlassian does not provide a consolidated governance dashboard that spans both Bitbucket and external source control systems.

## 6. Identity, Compliance and Audit

**Readiness: Partial.**

Atlassian Access provides SSO, SCIM provisioning and audit logging for Atlassian cloud products. Atlassian Guard provides security policies and data protection. For organisations using Atlassian Data Center (self-managed), identity management is more flexible.

However, Atlassian does not provide the depth of identity and compliance tooling that Microsoft (Entra ID, Purview) or AWS (IAM, CloudTrail) offer natively. Agent service principals with scoped permissions require more manual configuration in Atlassian than in cloud-native identity platforms.

Audit trails exist for Jira and Bitbucket actions, but joining provenance across work items, pull requests and deployments requires custom tooling.

**Gaps.** Named agent identities with scoped permissions require more manual configuration in Atlassian than in cloud-native identity systems. Organisations will need to implement agent identity patterns manually, likely using API tokens with restricted scopes. Cross-system provenance from specification to deployed code requires deliberate configuration and integration work.

## 7. Release, Operations and Feedback

**Readiness: Limited.**

Atlassian does not provide CI/CD, deployment pipelines or operational monitoring natively. Bitbucket Pipelines is available but is basic compared to GitHub Actions, GitLab CI/CD or Azure Pipelines. Most organisations pair Atlassian with Jenkins, GitHub Actions, GitLab CI/CD or another pipeline tool for real CI/CD capability.

Operational monitoring, alerting and feedback loops must be sourced from outside Atlassian, such as Datadog, New Relic, Prometheus or cloud-native monitoring services.

**Gaps.** Atlassian's pipeline capability (Bitbucket Pipelines) is limited. Most organisations will need to introduce separate CI/CD tooling. This fragmentation means that a complete ADM delivery pipeline requires Atlassian for backlog and governance plus external tools for build, test, deploy and operational monitoring.

## 8. Ecosystem Readiness Summary

| ADM Area | Atlassian Capability | Readiness | Key Gap |
|---|---|---|---|
| Requirements and intent design | Jira, Confluence, Rovo | Strong | Work item templates need customisation for Intent Specifications |
| Agent execution (coding) | Rovo Dev CLI | Partial | Assists with development but does not autonomously implement code changes |
| Agent orchestration | None | Limited | No multi-agent orchestration surface. Rovo Dev operates as a single assistant |
| Governance and review | Bitbucket pull requests, Jira workflows | Partial | No consolidated governance dashboard. Fragmented if using GitHub alongside Bitbucket |
| Identity and compliance | Atlassian Access, Atlassian Guard | Partial | Agent identity patterns require manual configuration. Cross-system provenance requires custom work |
| Release and operations | Bitbucket Pipelines | Limited | Basic pipeline capability. Most organisations need external CI/CD and monitoring tools |

## 9. Reference Operating Pattern

A practical Atlassian-centred ADM pattern, acknowledging the need for complementary tooling:

1. Business stakeholders work with a Requirements Agent through Confluence and Rovo.
2. The Requirements Agent drafts Intent Specifications into Jira work items.
3. The Product Owner and Intent Architect review and approve the specification in Jira.
4. The approved specification is assigned to a coding agent (external to Atlassian, such as GitHub Copilot coding agent or a third-party autonomous agent).
5. The coding agent implements on an isolated branch and raises a pull request in the source control platform (Bitbucket, GitHub or GitLab).
6. The Verification Engineer reviews the pull request at the daily governance session.
7. Branch protections and required approvals enforce governance controls at the source control level.
8. An external CI/CD system (GitHub Actions, GitLab CI/CD, Jenkins or similar) runs build, test and deployment pipelines.
9. Operational monitoring and feedback loops are sourced from external monitoring and observability tools.
10. Jira remains the hub for backlog visibility, governance workflows and audit trails.

This pattern uses Atlassian as the strongest requirements and work management layer, but it requires complementary tooling for autonomous agent execution, CI/CD and operations.

## 10. Key Decision Points for Adopters

Organisations assessing the Atlassian ecosystem for ADM adoption should consider:

**Agent execution strategy.** Rovo Dev assists with development tasks but does not autonomously implement code against specifications. Organisations committed to Atlassian for backlog and governance should plan to introduce a separate autonomous coding agent. This may be GitHub Copilot coding agent (which requires GitHub Enterprise for repository hosting) or a third-party agent that works with Bitbucket or other source control systems.

**Source control consolidation.** Bitbucket is Atlassian-native but has limited autonomous agent capability. Many organisations will use GitHub alongside Bitbucket for coding agent support. This means governance flows span two platforms unless all repositories are consolidated. Evaluate whether to standardise on a single source control platform or accept multi-platform governance complexity.

**CI/CD tooling.** Bitbucket Pipelines is functional for basic workflows but is not a first-class CI/CD platform. Plan to integrate with GitHub Actions, GitLab CI/CD, Jenkins or another dedicated pipeline tool. This is normal practice but adds integration complexity.

**Orchestration and integration.** If the organisation wants all agents (backlog management, requirements gathering, code execution, testing, security) coordinated through a single system, that does not exist within Atlassian today. Design integration points between Jira (backlog and governance), external coding agents and external CI/CD systems. Consider whether a dedicated agent orchestration platform is needed.

**Atlassian's inverse positioning.** Atlassian is the inverse of AWS. It is strongest at the requirements and work management layer and weakest at autonomous agent execution and operational infrastructure. Use it for those strengths. Complement it with specialised tools for execution and operations.

**Cost and model governance.** Atlassian provides no native cost tracking for agent or model usage. Agent execution costs will sit in external platforms (the coding agent provider, the CI/CD platform, the cloud infrastructure). Organisations will need to aggregate cost data from multiple sources and link it to Jira work items to achieve the ADM's cost-per-specification tracking.

## 11. Source Notes

This assessment is based on public product documentation from:

- [Rovo Dev rollout documentation](https://support.atlassian.com/rovo/docs/rovo-dev-rollout/)
- [Rovo Dev CLI installation and use](https://support.atlassian.com/rovo/docs/install-and-run-rovo-dev-cli-on-your-device/)
- [Rovo Dev code review](https://support.atlassian.com/rovo/docs/trigger-a-code-review/)
- [Rovo Dev and Model Context Protocol](https://support.atlassian.com/rovo/docs/rovo-dev-and-model-context-protocol-mcp/)
- [Jira documentation](https://support.atlassian.com/jira-cloud/)
- [Bitbucket Cloud documentation](https://support.atlassian.com/bitbucket-cloud/)
- [Bitbucket Pipelines documentation](https://support.atlassian.com/bitbucket-cloud/docs/get-started-with-bitbucket-pipelines/)

Atlassian product naming, scope and capabilities are evolving. This assessment should be revisited as Rovo and the broader Atlassian ecosystem mature.