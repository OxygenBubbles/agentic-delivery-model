# Microsoft (Copilot Studio, Azure DevOps and Agent 365): ADM Implementation Guide

> **Alpha.** This guide reflects the current state of the Microsoft enterprise stack for agentic delivery. It will be updated as tooling matures and practical experience grows. Product names, settings locations and schemas will change.

This is a hands-on implementation guide. For the higher-level view of where the Microsoft ecosystem is ready and where the gaps are, read [MICROSOFT-ADM.md](MICROSOFT-ADM.md) first. This guide shows how to run the Agentic Delivery Model across the Microsoft stack: Copilot Studio for requirements and business agents, Azure DevOps Boards for the backlog, GitHub Copilot for execution, Agent 365 for governance and Entra ID for identity.

One point shapes everything below. The coding execution layer in the Microsoft world is GitHub, which Microsoft owns. The repo-native detail of configuring and running the coding fleet lives in [GITHUB-COPILOT-CLAUDE-ADM.md](GITHUB-COPILOT-CLAUDE-ADM.md). This guide focuses on the Microsoft-specific layer around it: the requirements surface, the enterprise backlog, the agent control plane and identity. Read [MODEL.md](../MODEL.md) for the roles, artefacts and cadence referenced throughout.

## 1. The build at a glance

| ADM concept | Microsoft mechanism |
|---|---|
| Intent Brief and Intent Specification | An Azure DevOps work item, or a GitHub Issue where execution is on GitHub |
| Requirements discovery | A Copilot Studio agent over Microsoft 365 content, run through Teams |
| Ready backlog | Azure Boards state and board, gated by the Definition of Ready |
| Company and project standards | `AGENTS.md` and `.github` configuration on the execution repository |
| Coherence Register | `COHERENCE-REGISTER.md` in the repository, reviewed weekly |
| Requirements and business agents | Copilot Studio agents |
| Coding agent roles | GitHub Copilot coding agent and custom agents in `.github/agents` |
| Agent control plane | Agent 365, the inventory and governance layer for all agents |
| Agent identity | Entra ID service identities with scoped permissions |
| Agent sandbox | The ephemeral GitHub Actions environment per task |
| Provenance | The pull request, Entra ID logs, Azure DevOps audit and Agent 365 |
| Governance Tier | Pull request review on GitHub, or Azure DevOps pull requests |
| Release | Azure Pipelines and Azure App Configuration feature flags |

## 2. Where everything lives

### 2.1 Intent Briefs and Intent Specifications

Intent Briefs and Intent Specifications are Azure DevOps work items, with custom fields and a work item type that mirror the Intent Specification template: business intent, acceptance criteria, technical constraints, test expectations and complexity. Azure Boards holds priority and the Ready state. Organisations that run execution on GitHub can use GitHub Issues instead and keep Azure Boards in step through the Boards and GitHub integration. Azure Boards can now assign work to a GitHub Copilot coding agent and select a custom agent for the job, which connects the backlog to execution directly.

Discovery is a strength of the Microsoft stack. Microsoft 365 is where many requirements conversations already happen, and a Copilot Studio agent can run structured discovery through Teams, draw on documents, emails and meeting transcripts and draft Intent Specifications for review. The work item is where the approved specification is recorded.

### 2.2 Architectural and standards information

Because execution is repo-native on GitHub, standards live where the coding agent reads them: `AGENTS.md` at the repository root for company-level standards, path-scoped instructions for project-level rules and organisation-wide standards in the organisation `.github` repository. This is covered in detail in [GITHUB-COPILOT-CLAUDE-ADM.md](GITHUB-COPILOT-CLAUDE-ADM.md). The Coherence Register is held as `COHERENCE-REGISTER.md` in the repository and reviewed at the weekly Strategic Tier. Knowledge that business agents use, such as domain glossaries and policy documents, lives in SharePoint and is reached by Copilot Studio agents.

### 2.3 Agent configuration

There are two planes. Business and requirements agents are built in Copilot Studio with low-code tools and generative AI, and are managed alongside Microsoft 365 and partner agents under shared policies. Coding agent roles are GitHub custom agents in `.github/agents`, selected per task. Agent 365 sits above both as the control plane: an inventory of every agent, their permissions, their behaviour and their activity, discoverable and auditable from the admin centre, an API or Azure Resource Graph. This is what lets a regulated organisation see and govern its whole agent estate in one place.

### 2.4 Provenance

The pull request and its session log record the change on the execution side. Entra ID provides sign-in and access logs. Azure DevOps provides audit streams for work item, pipeline and repository events. Agent 365 provides the agent inventory and activity record. Joining these into one chain from specification to deployed code is deliberate work, but the building blocks are strong.

## 3. Configuring the agent fleet

The ADM agent roles split across the two planes. The Requirements Agent is a Copilot Studio agent. The coding roles are GitHub custom agents and automated checks, configured exactly as in the GitHub guide. Agent 365 governs the whole set.

| ADM agent role | Microsoft mechanism |
|---|---|
| Requirements Agent | A Copilot Studio agent over Microsoft 365, run through Teams |
| Implementation Agent | GitHub Copilot coding agent on a Claude or other model |
| Testing Agent | A GitHub custom agent plus an Actions test workflow |
| Review Agent | The GitHub code review agent plus a read-only custom agent |
| Security Agent | A GitHub custom agent plus code scanning, governed in Agent 365 |
| Documentation Agent | A GitHub custom agent scoped to docs |
| Design Agent | A GitHub custom agent plus path instructions for user-facing work |
| Platform Agent | A GitHub custom agent, human approval on every change |
| Refactoring Agent | The coding agent on a remediation work item |
| Scheduling Agent | Partial. Azure Boards assignment plus Agent HQ. See Section 7 |

The coding agent definitions, the `.github/agents` files and the MCP and environment configuration are all as set out in [GITHUB-COPILOT-CLAUDE-ADM.md](GITHUB-COPILOT-CLAUDE-ADM.md). The Microsoft-specific addition is governance: register every agent in Agent 365, set its permissions through Entra ID and review the inventory as part of the Strategic Tier.

## 4. The delivery cycle, step by step

### Design phase

1. The Product Owner records an Intent Brief as a work item. A Copilot Studio requirements agent runs discovery through Teams, drawing on Microsoft 365 content, and drafts the Intent Specification.
2. The Product Owner refines the draft. The Intent Architect reviews for feasibility and scope and tightens the acceptance criteria.
3. When the Definition of Ready passes, the work item moves to Ready in Azure Boards.

### Build phase

4. The work item is assigned to a GitHub Copilot coding agent, with the custom agent selected from Azure Boards or GitHub. The agent works on an isolated branch in an ephemeral environment and opens a pull request.
5. Actions workflows and the review, security and testing custom agents run against the pull request. The agent iterates on technical feedback within the session.
6. At the daily Governance Tier, the Verification Engineer reviews the pull request against the acceptance criteria and for structural consistency. Branch protection enforces the gate, and the specification author does not approve.
7. On merge, Azure Pipelines or GitHub Actions promotes the change. Where it is part of a larger feature, it deploys behind an Azure App Configuration feature flag that the Product Owner activates when the Strategic Intent is complete.

## 5. Governance, identity and guardrails

**Identity.** Entra ID provides agent service identities with scoped permissions and conditional access, distinct from any person. GitHub Enterprise integrates with Entra ID for sign-on and policy.

**Control plane.** Agent 365 is the governance surface that the other ecosystems lack. It holds the inventory of every agent across the estate, business and coding, with permissions, behaviour and activity in one place. The agent inventory schema makes the estate discoverable and auditable. In a regulated environment this is the difference between governing agents as a fleet and governing them one tool at a time.

**Access scope and sandbox.** Coding agents are constrained by the GitHub controls in the GitHub guide: tool scoping, MCP allowlists, the firewall and the ephemeral Actions environment per task. Microsoft Purview and Defender add data governance and security monitoring around them.

**Human accountability.** Branch protection and required reviewers make a named person the approver of every change into a protected branch, recorded in the GitHub and Azure DevOps audit logs and the Agent 365 activity record.

## 6. Cadence mapping

**Execution Tier, continuous.** Coding agents work Ready work items, watched through GitHub Agent HQ mission control. Copilot Studio agents run the requirements pipeline.

**Governance Tier, daily.** Pull requests queue on GitHub or Azure DevOps. The Verification Engineer reviews against acceptance criteria, the Product Owner answers clarifications and the backlog is reprioritised.

**Strategic Tier, weekly.** The System Steward reviews `COHERENCE-REGISTER.md`. The team reviews the Agent 365 inventory, model selection, cost and access changes.

## 7. Gaps and what to add

**Two-plane integration.** Business and requirements agents live in Copilot Studio, coding agents live in GitHub. Agent 365 governs both for inventory and policy, but it does not orchestrate a coding fleet. Coordinating the requirements plane with the execution plane is integration work.

**The Scheduling Agent.** Azure Boards assigns work and Agent HQ provides multi-agent mission control, but neither is the model's backlog-aware Scheduling Agent that detects specification overlap and routes work proactively. Pair them with an external orchestration layer for that pattern.

**Azure DevOps Repos.** Organisations on Azure DevOps Repos rather than GitHub lack a native coding agent and should plan to host execution repositories on GitHub or introduce a third-party agent.

**Cross-system provenance and feature flags.** A complete chain from specification to deployment spans Azure Boards, GitHub, Azure Pipelines and Entra ID. Joining these into one record is deliberate work. App Configuration feature flags are functional but basic compared with specialist platforms.

## 8. Reference operating pattern

1. Standards are codified in `AGENTS.md` and `.github` configuration on the execution repository. Coding agent roles are GitHub custom agents. Requirements agents are built in Copilot Studio. Every agent is registered in Agent 365.
2. The Product Owner records an Intent Brief as a work item. A Copilot Studio agent runs discovery through Teams and drafts the Intent Specification.
3. The Intent Architect reviews for feasibility and scope, the Definition of Ready passes and the work item moves to Ready in Azure Boards.
4. The work item is assigned to a GitHub Copilot coding agent with the custom agent selected. The agent works on an isolated branch and opens a pull request.
5. Actions workflows and the review, security and testing custom agents run. The agent iterates on technical feedback.
6. The Verification Engineer reviews the pull request at the daily Governance Tier. The specification author does not approve.
7. On merge, Azure Pipelines or GitHub Actions promotes the change behind an App Configuration flag the Product Owner activates when the Strategic Intent is complete.
8. The System Steward maintains the Coherence Register and the team reviews the Agent 365 inventory at the weekly Strategic Tier.

## 9. Source notes

This guide is based on public documentation:

- [Microsoft Copilot Studio](https://www.microsoft.com/en-us/microsoft-365-copilot/microsoft-copilot-studio)
- [What's new in Copilot Studio](https://learn.microsoft.com/en-us/microsoft-copilot-studio/whats-new)
- [Azure Boards support for GitHub Copilot custom agents](https://learn.microsoft.com/en-us/azure/devops/release-notes/2026/sprint-269-update)
- [Azure DevOps Work Items connector](https://learn.microsoft.com/en-us/microsoft-365/copilot/connectors/azure-devops-work-items-overview)
- [Microsoft Entra ID](https://learn.microsoft.com/en-us/entra/identity/)
- [Azure App Configuration feature flags](https://learn.microsoft.com/en-us/azure/azure-app-configuration/concept-feature-management)
- [GitHub Copilot with Claude implementation guide](GITHUB-COPILOT-CLAUDE-ADM.md)

Microsoft product naming and capabilities are evolving quickly. Revisit this guide as the ecosystem matures.
