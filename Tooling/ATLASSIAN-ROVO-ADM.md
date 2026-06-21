# Atlassian (Rovo, Jira and Bitbucket): ADM Implementation Guide

> **Alpha.** This guide reflects the current state of Rovo and the Atlassian stack. It will be updated as tooling matures and practical experience grows. File paths, settings and module names will change.

This is a hands-on implementation guide. For the higher-level view of where the Atlassian ecosystem is ready and where the gaps are, read [ATLASSIAN-ADM.md](ATLASSIAN-ADM.md) first. This guide shows how to run the Agentic Delivery Model across the Atlassian stack: Rovo agents over Jira and Confluence for requirements, Rovo Dev for execution, Bitbucket for the governance surface and Atlassian Access for identity.

One point shapes everything below. Atlassian is strongest at requirements and work management, and its execution agent, Rovo Dev, is less autonomous end to end than GitHub Copilot's coding agent or Kiro. Many Atlassian-centred teams run Jira and Confluence for intent and governance and pair them with a stronger execution agent. This guide shows the Atlassian-native path and marks clearly where pairing helps. Read [MODEL.md](../MODEL.md) for the roles, artefacts and cadence referenced throughout.

## 1. The build at a glance

| ADM concept | Atlassian mechanism |
|---|---|
| Intent Brief and Intent Specification | A Jira work item, drafted with help from a Rovo agent over Jira and Confluence |
| Strategic Intent | A Jira epic grouping related work items |
| Ready backlog | A Jira board and workflow state, gated by the Definition of Ready |
| Company and project standards | Standards in the repository, plus reference material in Confluence |
| Coherence Register | `COHERENCE-REGISTER.md` in the repository, mirrored as a Jira item, reviewed weekly |
| Requirements and business agents | Rovo agents, built through Rovo and Forge |
| Coding agent roles | Rovo Dev subagents in `.rovodev/subagents/`, plus Agent Skills |
| Implementation Agent | Rovo Dev, interactive or run in CI through the Rovo Dev action |
| Agent sandbox | An isolated branch, locally or in a CI runner |
| Tool access | MCP servers connected to Rovo Dev |
| Provenance | The pull request, the Jira item and Atlassian audit logs |
| Governance Tier | Bitbucket pull request review, or GitHub where execution is there |
| Release | Bitbucket Pipelines, or an external CI/CD and feature flag platform |

## 2. Where everything lives

### 2.1 Intent Briefs and Intent Specifications

Intent Briefs and Intent Specifications are Jira work items. Jira is one of the most widely used enterprise backlogs, and its workflow engine supports the Ready state and clarification flows the model needs. Use a work item type and custom fields that mirror the Intent Specification template: business intent, acceptance criteria, technical constraints, test expectations and complexity. Epics group specifications under a Strategic Intent for release coordination.

Discovery is the Atlassian strength. A Rovo agent surfaces context from Jira and Confluence, and Confluence holds the domain knowledge that requirements draw on. The work item is where the approved specification is recorded.

### 2.2 Architectural and standards information

Standards that the coding agent enforces live in the repository, where Rovo Dev reads them, the same pattern as the other tools. Confluence holds the wider reference material: domain glossaries, policy documents and decision records that requirements agents and people consult. The Coherence Register is held as `COHERENCE-REGISTER.md` in the repository and mirrored as a Jira item so remediation can be prioritised alongside feature work. The weekly Strategic Tier reviews it.

### 2.3 Agent configuration and skills

There are two kinds of agent. Rovo agents are built through Rovo and the Forge rovo-agent module, and work across Jira and Confluence as requirements and business agents. Rovo Dev is the developer agent, and its roles are expressed as subagents: Markdown files with YAML frontmatter for name, description and tools, followed by a system prompt. Project subagents sit in `.rovodev/subagents/` and user subagents in `~/.rovodev/subagents/`, and the main agent calls them through an invoke tool. A subagent runs only with the context Rovo Dev supplies, which keeps its work focused. Agent Skills extend Rovo Dev with reusable capabilities, and MCP connects it to external systems. The Rovo Dev CLI configuration file sets behaviour such as the model, system prompt additions and output format.

### 2.4 Provenance

The pull request records the change. The Jira item links it to its intent. Atlassian Access and Guard provide audit logging across Jira and Bitbucket. Where execution and pull requests are on GitHub, that platform's logs join the chain. Together these give the model's provenance, though joining them across Atlassian and an external repository platform is deliberate work.

## 3. Configuring the agent fleet

Requirements and business roles are Rovo agents. Coding roles are Rovo Dev subagents and Agent Skills. Scope each to the minimum it needs.

| ADM agent role | Atlassian mechanism |
|---|---|
| Requirements Agent | A Rovo agent over Jira and Confluence |
| Implementation Agent | Rovo Dev, interactive or in CI through the Rovo Dev action |
| Testing Agent | A Rovo Dev subagent plus the test stage in the pipeline |
| Review Agent | Rovo Dev code review on the pull request |
| Security Agent | A Rovo Dev subagent plus dependency and secret scanning in CI |
| Documentation Agent | A Rovo Dev subagent scoped to docs |
| Design Agent | A Rovo Dev subagent plus design reference in Confluence |
| Platform Agent | A subagent for infrastructure, with human approval on every change |
| Refactoring Agent | Rovo Dev on a remediation work item |

A Rovo Dev subagent for the Review Agent role, scoped read-only:

```markdown
---
name: review-agent
description: Reviews a diff against acceptance criteria and structural consistency. Returns findings only, never edits code.
tools: ["read", "search"]
---

You are the Review Agent in an Agentic Delivery Model team. Given the diff and
the linked Jira specification, confirm that every acceptance criterion is met,
that the change follows the repository standards and that it is structurally
consistent with the surrounding code. Report a checklist of findings. Escalate
any question about business intent to a person.
```

Because Rovo Dev is less autonomous end to end than the strongest coding agents, many teams use it for review, security and documentation roles and pair it with GitHub Copilot or Kiro for the heavy implementation role. That pairing is a legitimate pattern, not a failure.

## 4. The delivery cycle, step by step

### Design phase

1. The Product Owner records an Intent Brief as a Jira work item. A Rovo agent runs discovery over Jira and Confluence and helps draft the Intent Specification.
2. The Product Owner refines the draft. The Intent Architect reviews for feasibility and scope and tightens the acceptance criteria.
3. When the Definition of Ready passes, the work item moves to Ready on the Jira board.

### Build phase

4. The work item is assigned for execution. Rovo Dev implements on an isolated branch, run interactively or in CI through the Rovo Dev action. Alternatively a paired coding agent does the implementation.
5. The agent opens a pull request in Bitbucket or GitHub. Rovo Dev code review, the test stage and security scanning run against it.
6. Technical questions resolve within the agent's loop. A question about business intent stops the work and returns to a person.
7. At the daily Governance Tier, the Verification Engineer reviews the pull request against the acceptance criteria and for structural consistency. Branch policies and required reviewers enforce the gate, and the specification author does not approve.
8. On merge, the pipeline promotes the change. Where it is part of a larger feature, it deploys behind a feature flag from the chosen flag platform, which the Product Owner activates when the Strategic Intent is complete.

## 5. Governance, identity and guardrails

**Identity.** Atlassian Access provides SSO, SCIM provisioning and audit logging, and Atlassian Guard adds security policies. Agent identities are created as scoped API tokens or service accounts. This is workable, though less deep than the cloud-native identity of Entra ID or AWS IAM, so plan the agent identity pattern deliberately.

**Access scope.** Rovo Dev subagents are limited to their declared tools, and MCP connections expose only what an agent needs. Bitbucket branch policies, merge checks and required reviewers control who approves what.

**Sandbox.** Agents work on isolated branches, never on protected branches, whether locally or in a CI runner. Scanning runs in the pipeline before a pull request is eligible to merge.

**Human accountability.** Branch policies and required reviewers make a named person the approver of every change into a protected branch, recorded in Atlassian audit logs.

## 6. Cadence mapping

**Execution Tier, continuous.** Rovo Dev works Ready items, interactively or in CI through the Rovo Dev action, alongside any paired coding agent.

**Governance Tier, daily.** Pull requests queue in Bitbucket or GitHub. The Verification Engineer reviews against acceptance criteria, the Product Owner answers clarifications and the Jira backlog is reprioritised.

**Strategic Tier, weekly.** The System Steward reviews `COHERENCE-REGISTER.md`. The team reviews subagent and Rovo agent configuration, model selection and cost. Jira remains the hub for backlog visibility and audit.

## 7. Gaps and what to add

**Autonomous execution maturity.** Rovo Dev assists strongly with review, code analysis and development tasks, but it is less mature at taking a specification and producing a merge-ready pull request unattended than GitHub Copilot's coding agent or Kiro. Plan to pair Atlassian for backlog and governance with a stronger execution agent where full autonomy matters.

**Orchestration.** Rovo Dev subagents coordinate within a session. There is no backlog-aware Scheduling Agent that reads the whole Ready backlog, detects specification overlap and routes work proactively. Add an external orchestration layer for that pattern.

**CI/CD and feature flags.** Bitbucket Pipelines is functional but basic, and there is no native feature flag management. Most teams pair Atlassian with a dedicated CI/CD platform and a feature flag service.

**Cross-platform governance and cost.** If repositories span Bitbucket and GitHub, governance visibility fragments. Atlassian provides no native cost tracking for agent usage, so aggregate cost from the execution and CI platforms and link it to Jira items for cost per specification.

## 8. Reference operating pattern

1. Standards live in the repository and in Confluence. Requirements roles are Rovo agents. Coding roles are Rovo Dev subagents in `.rovodev/subagents/`, with Agent Skills and MCP for tools.
2. The Product Owner records an Intent Brief as a Jira work item. A Rovo agent runs discovery over Jira and Confluence and drafts the Intent Specification.
3. The Intent Architect reviews for feasibility and scope, the Definition of Ready passes and the work item moves to Ready.
4. Rovo Dev, or a paired coding agent, implements on an isolated branch and opens a pull request in Bitbucket or GitHub.
5. Rovo Dev code review, the test stage and security scanning run. Technical questions resolve in the agent loop. Intent questions escalate to a person.
6. The Verification Engineer reviews the pull request at the daily Governance Tier. The specification author does not approve.
7. On merge, the pipeline promotes the change behind a feature flag the Product Owner activates when the Strategic Intent is complete.
8. The System Steward maintains the Coherence Register and the team reviews agent configuration at the weekly Strategic Tier. Cost per specification is aggregated from the execution and CI platforms.

## 9. Source notes

This guide is based on public documentation:

- [Rovo: Agents](https://support.atlassian.com/rovo/docs/agents/)
- [Use Rovo Dev CLI](https://support.atlassian.com/rovo/docs/use-rovo-dev-cli/)
- [Use subagents in Rovo Dev CLI](https://support.atlassian.com/rovo/docs/use-subagents-in-rovo-dev-cli/)
- [Extend Rovo Dev CLI with Agent Skills](https://support.atlassian.com/rovo/docs/extend-rovo-dev-cli-with-agent-skills/)
- [Rovo Dev advanced agentic configuration (Bitbucket)](https://support.atlassian.com/bitbucket-cloud/docs/rovo-dev-advanced-agentic-configuration/)
- [Rovo agent (Forge module)](https://developer.atlassian.com/platform/forge/manifest-reference/modules/rovo-agent/)
- [Rovo Dev action for CI/CD](https://github.com/atlassian-labs/rovo-dev-action)

Atlassian product naming and capabilities are evolving. Revisit this guide as Rovo matures.
