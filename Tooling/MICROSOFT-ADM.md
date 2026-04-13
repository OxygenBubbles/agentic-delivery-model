# Microsoft Ecosystem Readiness for the ADM

> **Alpha.** This tooling guidance reflects the current state of the Microsoft ecosystem and will be updated as tooling matures and practical experience grows.

This document assesses how the Microsoft enterprise ecosystem can support the Agentic Delivery Model across the full delivery lifecycle: requirements, execution, governance, release and operations. It identifies where the ecosystem is ready, where gaps exist and where complementary tooling may be needed.

It is a readiness assessment, not a product endorsement. Organisations building on the Microsoft stack should use it to understand what is available today and where to plan for investment or integration.

Organisations using GitHub without the rest of the Microsoft stack should refer to the [GitHub ecosystem assessment](GITHUB-ADM.md) for a standalone GitHub perspective.

## 1. The ADM Delivery Lifecycle

The ADM needs tooling support across five areas:

1. **Requirements and intent design.** Capturing business intent, running structured discovery, producing Intent Specifications.
2. **Agent execution.** Autonomous agents implementing, testing and documenting code changes against approved specifications.
3. **Governance and human review.** Pull request review, verification, validation and approval by people at the Governance Tier.
4. **Identity, compliance and audit.** Named agent identities, scoped permissions, provenance trails and regulatory evidence.
5. **Release, operations and feedback.** CI/CD pipelines, environment management, monitoring and operational feedback loops.

The assessment below maps Microsoft capabilities against each area.

## 2. Requirements and Intent Design

**Readiness: Strong.**

Microsoft 365 is where many enterprise requirements conversations already happen. Teams, Outlook, SharePoint and Loop provide the collaboration surface for stakeholder engagement, discovery sessions and document sharing.

Copilot Studio can support a Requirements Agent that:

- runs structured discovery sessions with stakeholders through Teams or other Microsoft channels
- gathers context from Microsoft 365 content (documents, emails, meeting transcripts)
- drafts Intent Specifications for Product Owner and Intent Architect review
- keeps interview structure and template usage consistent across teams

Azure DevOps provides backlog management through work items, boards and queries. Intent Specifications can be represented as work items with custom fields for complexity band, boundaries and integration points. Azure Boards supports the Ready state workflow the ADM requires.

**Gaps.** Azure DevOps work items are not designed around the Intent Specification template. Custom field configuration and workflow customisation are needed to match the ADM artefact structure. This is achievable but requires deliberate setup.

## 3. Agent Execution

**Readiness: Strong, through GitHub.**

Microsoft's primary coding agent capability is GitHub Copilot coding agent, available through GitHub Enterprise. GitHub is wholly owned by Microsoft and integrates with the wider Microsoft ecosystem through Azure AD (Entra ID), Azure DevOps and Azure Pipelines.

GitHub Copilot coding agent can:

- implement code changes against approved Intent Specifications
- work on isolated feature branches (never committing to protected branches)
- raise pull requests for human review
- iterate on review feedback within the same session
- provide session logs and provenance for every change

This is repo-native agent execution and is the strongest part of the Microsoft ecosystem for ADM delivery. GitHub's branch protection rules, required reviewers, status checks and approval controls map directly to the ADM's governance requirements.

Azure DevOps Repos provides an alternative source control and pull request surface. It supports branch policies, required reviewers and build validation. However, Azure DevOps does not currently have an equivalent autonomous coding agent capability. Organisations using Azure DevOps Repos for source control may need to introduce a separate coding agent that works against Azure DevOps Git repositories, or migrate repository hosting to GitHub Enterprise.

**Gaps.** The coding agent capability lives in GitHub, not Azure DevOps. Organisations with Azure DevOps as their primary development platform face a choice: adopt GitHub Enterprise for repository hosting, or integrate a third-party coding agent with Azure DevOps Repos. Neither option is seamless today.

## 4. Agent Orchestration and Coordination

**Readiness: Partial.**

Copilot Studio provides multi-agent orchestration where a coordinating agent can route work to specialised agents. This can support the ADM's pattern of a Scheduling Agent coordinating multiple execution agents (implementation, testing, security analysis).

Agent 365 (Microsoft 365 Copilot's agent management layer) provides a control plane for:

- agent inventory and ownership
- policy and access controls
- deployment status
- operational visibility

This is useful for the ADM's governance requirements, particularly in regulated environments where agent inventory and policy compliance must be visible.

**Gaps.** Copilot Studio orchestration is designed primarily for business process agents, not software delivery agents working inside repositories. Coordinating a Requirements Agent in Copilot Studio with a GitHub Copilot coding agent requires integration across two different agent platforms. There is no single orchestration surface today that spans business workflow agents and repo-native coding agents within the Microsoft ecosystem.

## 5. Governance and Human Review

**Readiness: Strong.**

GitHub pull requests provide the primary governance surface for the Execution and Governance Tiers. Required reviewers, approval rules, status checks and branch protections are all configurable to match ADM governance requirements.

Azure DevOps pull requests provide equivalent controls for organisations using Azure Repos.

Microsoft Teams can support the human governance sessions (daily reviews, strategic reviews) through scheduled meetings, channels and integration with backlog tooling. Copilot in Teams can summarise meeting outcomes and track action items.

**Gaps.** The ADM's Governance Tier needs a consolidated view of all pending agent work: PRs awaiting review, specifications in progress, blocked items and escalations. Neither GitHub nor Azure DevOps provides this consolidated governance dashboard out of the box. Most organisations will need to build or configure a governance view, whether through Azure DevOps dashboards, GitHub Projects, Power BI or a custom integration.

## 6. Identity, Compliance and Audit

**Readiness: Strong.**

Microsoft Entra ID (Azure AD) provides the identity layer. Agent service principals can be created with scoped permissions, conditional access policies and audit logging. This maps well to the ADM requirement for named service identities with constrained access.

GitHub Enterprise integrates with Entra ID for SSO and can enforce organisation-level policies on agent access.

Azure Policy, Microsoft Purview and Defender for Cloud provide compliance, data governance and security monitoring capabilities that support the ADM in regulated environments.

Audit trails span the Microsoft ecosystem. Every agent action in GitHub is logged. Entra ID provides sign-in and access logs. Azure DevOps provides audit streams for work item changes, pipeline runs and repository events.

**Gaps.** Provenance across the full lifecycle (from Intent Specification to deployed code) requires deliberate linking. A specification in Azure DevOps, code in GitHub, a pipeline in Azure Pipelines and a deployment in Azure all produce separate audit records. Joining these into a single provenance chain requires configuration and may require custom tooling or Azure Data Explorer queries.

## 7. Release, Operations and Feedback

**Readiness: Strong.**

Azure Pipelines (in Azure DevOps) provides CI/CD with approval gates, environment management, deployment strategies and release tracking. GitHub Actions provides equivalent capability for GitHub-hosted workflows.

Azure provides the infrastructure layer: compute, networking, monitoring (Application Insights, Azure Monitor), alerting and diagnostics. Azure DevOps release pipelines support the ADM's daily release cadence with approval gates at each environment stage.

Feature flag management can be handled through Azure App Configuration, which supports feature flags with targeting, rollout schedules and integration with application code. This supports the ADM's pattern of deploying behind feature flags and activating at the Product Owner's discretion.

**Gaps.** Azure App Configuration's feature flag capability is functional but basic compared to specialist feature flag platforms (LaunchDarkly, Split). Organisations with complex rollout requirements may need a dedicated feature management service.

## 8. Ecosystem Readiness Summary

| ADM Area | Microsoft Capability | Readiness | Key Gap |
|---|---|---|---|
| Requirements and intent design | Microsoft 365, Copilot Studio, Azure DevOps Boards | Strong | Work item templates need customisation for Intent Specifications |
| Agent execution (coding) | GitHub Copilot coding agent | Strong | Requires GitHub Enterprise. Not available in Azure DevOps Repos |
| Agent orchestration | Copilot Studio, Agent 365 | Partial | No single orchestration surface spanning business and coding agents |
| Governance and review | GitHub PRs, Azure DevOps PRs, Teams | Strong | No consolidated governance dashboard out of the box |
| Identity and compliance | Entra ID, Purview, Defender | Strong | Cross-system provenance chain requires configuration |
| Release and operations | Azure Pipelines, GitHub Actions, Azure infrastructure | Strong | Feature flag capability is basic |

## 9. Reference Operating Pattern

A practical Microsoft-centred ADM pattern:

1. Business stakeholders work with a Requirements Agent through Teams and Copilot Studio.
2. The Requirements Agent drafts Intent Specifications into Azure DevOps work items.
3. The Product Owner and Intent Architect review and approve the specification.
4. The approved specification is assigned to GitHub Copilot coding agent.
5. The coding agent implements on an isolated branch and raises a pull request in GitHub.
6. The Verification Engineer reviews the PR at the daily governance session.
7. Branch protections, status checks and required approvals enforce governance controls.
8. Azure Pipelines (or GitHub Actions) runs CI/CD with approval gates per environment.
9. Agent 365 provides a control-plane view of agent inventory, policy and audit signals.
10. Azure Monitor and Application Insights provide operational feedback.

This pattern uses Microsoft end to end but acknowledges that the coding execution layer sits in GitHub rather than Azure DevOps.

## 10. Key Decision Points for Adopters

Organisations assessing the Microsoft ecosystem for ADM adoption should consider:

**Source control platform.** GitHub Enterprise provides the richest agent execution capability. Azure DevOps Repos is viable for source control and pull requests but lacks an equivalent autonomous coding agent. Organisations currently on Azure DevOps Repos should evaluate whether to adopt GitHub Enterprise for repository hosting or introduce a third-party coding agent.

**Backlog tooling.** Azure DevOps Boards can serve as the backlog tool with customisation. GitHub Issues and Projects are an alternative if the organisation consolidates onto GitHub. The choice depends on existing workflows and whether non-engineering stakeholders need access.

**Orchestration strategy.** If the organisation wants a single control surface for all agents (business workflow and coding), that integration does not exist today within Microsoft. Plan for separate orchestration of business-facing agents (Copilot Studio) and coding agents (GitHub), with integration at the governance layer.

**Provenance and compliance.** The building blocks exist (Entra ID, audit logs, pipeline records), but a joined-up provenance chain from specification to deployment requires deliberate design. Regulated organisations should plan for this integration work early.

**Cost and model governance.** Azure provides cost management tooling that can track agent-related compute and API spend. GitHub Copilot usage is tracked at the organisation level. However, mapping cost to individual Intent Specifications (which the ADM requires) needs custom configuration. Plan for linking agent usage metrics to specification identifiers early.

## 11. Source Notes

This assessment is based on public product documentation from:

- [Microsoft Copilot Studio](https://www.microsoft.com/en-us/microsoft-365-copilot/microsoft-copilot-studio)
- [GitHub Copilot coding agent](https://docs.github.com/en/enterprise-cloud@latest/copilot/concepts/agents/coding-agent/about-coding-agent)
- [Azure DevOps documentation](https://learn.microsoft.com/en-us/azure/devops/)
- [Microsoft Entra ID](https://learn.microsoft.com/en-us/entra/identity/)
- [Azure Pipelines](https://learn.microsoft.com/en-us/azure/devops/pipelines/)
- [Azure App Configuration feature flags](https://learn.microsoft.com/en-us/azure/azure-app-configuration/concept-feature-management)

Microsoft product naming, scope and capabilities are evolving quickly. This assessment should be revisited as the ecosystem matures.
