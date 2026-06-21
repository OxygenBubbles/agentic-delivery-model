# GitHub Copilot with Claude: ADM Implementation Guide

> **Alpha.** This guide reflects the current state of GitHub Copilot, Agent HQ and Claude model availability on GitHub. It will be updated as tooling matures and practical experience grows. Specific file paths, settings locations and model names will change.

This is a hands-on implementation guide, not a readiness assessment. For the higher-level view of where the GitHub ecosystem is ready and where the gaps are, read [GITHUB-ADM.md](GITHUB-ADM.md) first. This guide shows, step by step, how to run the Agentic Delivery Model on GitHub using Copilot's coding agent and custom agents powered by Claude models. It covers where Intent Specifications are stored, where architectural standards live, how each ADM agent role is configured, which agent does what and how a specification moves from intent to merged code.

It assumes familiarity with the model. Read [MODEL.md](../MODEL.md) for the roles, artefacts and cadence referenced throughout.

## 1. Two ways Claude appears on GitHub

It is worth being clear which integration this guide covers, because there are two and they are easy to confuse.

**Claude as the model behind Copilot.** GitHub Copilot's coding agent and custom agents can be powered by Claude models, selected through Agent HQ. GitHub owns the orchestration, the environment and the pull request workflow. You choose Claude as the engine. This guide covers this path.

**Claude Code running inside GitHub.** Anthropic's own agent can run as a GitHub Action (`anthropics/claude-code-action`), triggered by a mention or an assignment. This gives you the Claude agent loop directly, with your own orchestration. That path is covered in [CLAUDE-AGENTIC-LOOPS-ADM.md](CLAUDE-AGENTIC-LOOPS-ADM.md), which also shows how to combine the two.

Many organisations will use both. Copilot's coding agent for repo-native execution and governance, and Claude Code in Actions for orchestration and tasks that need the full agent loop.

## 2. The build at a glance

This table maps each ADM concept to the GitHub mechanism that carries it. The rest of the guide expands each row.

| ADM concept | GitHub mechanism |
|---|---|
| Intent Brief | GitHub Issue using an Issue Form template |
| Intent Specification | GitHub Issue using the Intent Specification Issue Form, held in a Project |
| Strategic Intent | Project, milestone or parent issue grouping related specifications |
| Ready backlog | Project board column or `ready` label, gated by the Definition of Ready checklist |
| Company standards | `AGENTS.md` at repo root and the organisation `.github` repository, plus `.github/copilot-instructions.md` |
| Project standards | Path-scoped `.github/instructions/*.instructions.md` and per-directory `AGENTS.md` |
| Coherence Register | `COHERENCE-REGISTER.md` in the repo plus a Project view, reviewed weekly |
| Agent roles (skills) | Custom agents in `.github/agents/*.agent.md`, plus organisation-level custom agents |
| Implementation Agent | Copilot coding agent on a Claude model, assigned to an Issue |
| Review, Security, Testing agents | Custom agents plus GitHub Actions checks and the Copilot code review agent |
| Scheduling Agent | Partial. Assignment plus Actions sequencing. See the gaps in Section 8 |
| Agent sandbox | Ephemeral GitHub Actions environment provisioned per task |
| Provenance | Pull request body, commit history, Issue links, Actions logs, organisation audit log |
| Governance Tier | Pull request review, branch protection, required reviewers, status checks |
| Release | GitHub Actions CI/CD, environments and an external feature flag service |

## 3. Where everything lives

The ADM depends on knowing where intent, standards, agent configuration and provenance are stored. On GitHub these are deliberate, version-controlled locations.

### 3.1 Intent Briefs and Intent Specifications

Intent Briefs and Intent Specifications are stored as GitHub Issues. Use Issue Forms (`.github/ISSUE_TEMPLATE/*.yml`) so every specification captures the same fields as the model's Intent Specification template. A specification Issue Form maps the template directly: business intent, acceptance criteria in Gherkin, technical constraints, design constraints, test expectations, complexity assessment and provenance.

A minimal `intent-specification.yml` Issue Form:

```yaml
name: Intent Specification
description: A unit of governed work ready for agent execution
labels: ["intent-spec", "draft"]
body:
  - type: textarea
    id: business-intent
    attributes:
      label: Business intent
      description: What capability or change is needed, why and for whom
    validations:
      required: true
  - type: textarea
    id: acceptance-criteria
    attributes:
      label: Acceptance criteria (Gherkin)
      description: GIVEN / WHEN / THEN, one per scenario
    validations:
      required: true
  - type: textarea
    id: technical-constraints
    attributes:
      label: Technical constraints
      description: Must follow, must not touch, integration points
  - type: textarea
    id: test-expectations
    attributes:
      label: Test expectations
  - type: dropdown
    id: complexity
    attributes:
      label: Complexity
      options: ["S", "M", "L"]
```

The backlog is a GitHub Project. The Definition of Ready is a checklist on the Issue. A specification becomes Ready when the checklist passes and the Intent Architect moves it to the Ready column. Strategic Intents group related specifications through a parent issue, a milestone or a Project field. This gives the release-management grouping the model needs for feature flags without recreating a heavy epic hierarchy.

GitHub does not run requirements discovery. The discovery work that produces a draft specification happens outside GitHub or through a requirements custom agent in Copilot Chat. The Issue is where the approved specification is recorded, not where intent is discovered. This is the main reason Section 8 lists requirements tooling as a gap.

### 3.2 Architectural and standards information

Standards are not documents people read and agents ignore. On GitHub they are configuration files the agent consumes on every task.

`AGENTS.md` at the repository root holds the company-level standards the agent must follow: technology stack, coding conventions, API design rules, testing expectations, security standards and domain rules. Copilot reads the nearest `AGENTS.md` in the directory tree, so a service directory can hold its own `AGENTS.md` with project-level standards that extend the root. This is how the model's split between company-level and project-level standards is expressed.

`.github/copilot-instructions.md` holds repository-wide instructions that apply to every Copilot interaction. Path-specific rules live in `.github/instructions/*.instructions.md`, each with an `applyTo` glob so the rules attach only to matching files. For example, a `frontend.instructions.md` with `applyTo: "src/ui/**"` carries the design system constraints that the Design Steward owns.

Organisation-wide standards that apply across every repository live in the organisation `.github` (or `.github-private`) repository. Standards defined there are inherited by all repositories, which is where estate-wide company standards belong.

The Coherence Register is the System Steward's architectural decision log, technical debt register and coherence risk tracker. Hold it as `COHERENCE-REGISTER.md` in the repository so it is versioned and visible to agents, and mirror open debt items as Issues in a Project view so remediation can be prioritised alongside feature work. The weekly Strategic Tier session reviews this file.

### 3.3 Agent configuration and skills

Custom agents are the ADM agent roles expressed as configuration. Each is a Markdown file with YAML frontmatter in `.github/agents/<name>.agent.md`. The frontmatter sets the display name, description, allowed tools and MCP servers. The body is the agent's system prompt. Organisation-level custom agents live in the `/agents` directory of the organisation `.github` repository and are shared across repositories.

Model Context Protocol servers extend what agents can reach. Configure them in the repository under Settings, then Copilot, then the coding agent section. Allowlist read-only tools where possible, because the agent uses allowlisted tools without asking for approval.

The agent's environment is provisioned by `.github/workflows/copilot-setup-steps.yml`. Use it to install the exact toolchain, dependencies and linters the agent needs before it starts work, so execution is deterministic. The agent firewall allowlist, also under the coding agent settings, controls which network endpoints the agent's shell can reach.

### 3.4 Provenance

Every change carries a trail. The pull request body links to the originating Issue and records the session. Commit history shows what changed. The Actions run holds the build, test and scan logs. The organisation audit log records who triggered the agent and who approved the merge. Together these give the model's provenance requirement: which specification, which agent, which model, which codebase version and who approved it.

## 4. Configuring the agent fleet

In the model, agent roles are configurations of a general-purpose model, not separate products. On GitHub this maps cleanly. The Implementation Agent is the Copilot coding agent itself, steered by `AGENTS.md` and the assigned Issue. The specialist roles are custom agents and automated checks that run against the pull request.

| ADM agent role | GitHub construct | Configuration location | Typical model |
|---|---|---|---|
| Requirements Agent | Custom agent used in Copilot Chat | `.github/agents/requirements.agent.md` | Claude Sonnet |
| Implementation Agent | Copilot coding agent on an Issue | `AGENTS.md` plus the assigned Issue | Claude Opus or Sonnet |
| Testing Agent | Custom agent plus Actions test workflow | `.github/agents/testing.agent.md` and a workflow | Claude Sonnet |
| Review Agent | Copilot code review plus custom agent | `.github/agents/review.agent.md` | Claude Sonnet |
| Security Agent | Custom agent plus code scanning | `.github/agents/security.agent.md` and CodeQL | Claude Sonnet |
| Documentation Agent | Custom agent | `.github/agents/documentation.agent.md` | Claude Sonnet |
| Design Agent | Custom agent plus path instructions | `.github/agents/design.agent.md` | Claude Sonnet |
| Platform Agent | Custom agent, human approval on every change | `.github/agents/platform.agent.md` | Claude Opus |
| Refactoring Agent | Coding agent on a remediation Issue | `AGENTS.md` plus the Coherence Register | Claude Opus or Sonnet |
| Scheduling Agent | Assignment plus Actions, with gaps | See Section 8 | n/a |

Model selection is per task. When you start a coding agent task in Agent HQ you pick the Claude model that fits the work. Heavier implementation and review tasks justify a more capable model. Lighter documentation or summary tasks use a faster one. This is the model's principle that different roles justify different models.

A custom agent for the Review Agent role, scoped to read-only tools so it cannot change code:

```markdown
---
name: review-agent
description: Reviews a changeset against acceptance criteria and structural consistency. Flags duplication, complexity and naming violations. Does not modify code.
tools: ["read", "search"]
---

You are the Review Agent in an Agentic Delivery Model team.

Review the changeset against the linked Intent Specification. Check that:
- every acceptance criterion has corresponding code and tests
- the change follows the standards in AGENTS.md and the path instructions
- there is no obvious duplication, dead code or unjustified complexity
- naming and structure are consistent with the surrounding codebase

Report findings as a checklist. Resolve technical clarifications by citing the
standard or pattern that applies. Escalate any question about business intent to
a person rather than guessing.
```

A custom agent for the Security Agent role:

```markdown
---
name: security-agent
description: Scans a changeset for dependency vulnerabilities, secret exposure, injection risks and licence issues before human review.
tools: ["read", "search"]
---

You are the Security Agent. For the changeset under review:
- check new or updated dependencies against known vulnerabilities and the
  approved licence list in AGENTS.md
- look for secrets, injection risks and insecure transport
- confirm security-relevant acceptance criteria are met

Report severity-rated findings. Block on anything high or critical. Defence in
depth assumes you will miss some issues, so report uncertainty plainly.
```

The Implementation Agent does not need its own `.agent.md`. It is the coding agent, and its behaviour is set by `AGENTS.md`, the path instructions and the Issue it is assigned. Keep `AGENTS.md` precise. It is the single biggest lever on implementation quality.

## 5. The delivery cycle, step by step

This is the full path from intent to merged code on GitHub.

### Design phase

1. The Product Owner writes an Intent Brief as an Issue using the Intent Brief form. It states the business objective, success criteria, constraints, stakeholders and a complexity and discovery-mode signal.
2. Discovery produces a draft specification. GitHub does not facilitate stakeholder discovery, so this happens through a requirements custom agent in Copilot Chat working from notes and existing context, or through discovery run outside GitHub. The output is a draft Intent Specification recorded as an Issue.
3. The Product Owner refines the draft for business accuracy. The Intent Architect reviews for feasibility and scope, tightens the acceptance criteria and confirms boundaries.
4. When the Definition of Ready checklist passes, the Intent Architect moves the Issue to the Ready column. Only Ready specifications are assigned to an agent.

### Build phase

5. Assign the Ready Issue to Copilot and select the Claude model for the task. Copilot acknowledges the Issue, opens a draft pull request and provisions an ephemeral Actions environment using `copilot-setup-steps.yml`. It works on an isolated branch and never commits to a protected branch.
6. As the agent pushes commits, GitHub Actions runs the build, the test workflow, code scanning and any custom check workflows. The Copilot code review agent and your review and security custom agents examine the changeset.
7. Technical clarifications resolve without a person. The agent iterates on the automated feedback and on `@copilot` review comments within the same session, rebasing against main where needed. Only a question about business intent stops the work and returns to a person.
8. At the daily Governance Tier, the Verification Engineer reviews the pull request against the acceptance criteria and for structural consistency, drawing on the automated checks and the agent's session log. Branch protection enforces that the merge cannot happen without an approving review and passing status checks. The person who authored or refined the specification must not be the person who approves the pull request.
9. On approval the pull request merges. CI/CD workflows promote the change through environments with approval gates. Where the change is part of a larger feature, it deploys behind a feature flag and the Product Owner activates the flag once every contributing specification is complete.

Provenance accumulates at each step. The Issue, the pull request, the commits, the Actions logs and the audit log together form the trail from business requirement to deployed code.

## 6. Governance, identity and guardrails

**Identity.** The coding agent acts as a distinct GitHub actor, separate from the person who triggered it, so its actions are attributable in the audit log. Only users with write access can assign work to the agent, which is the control boundary on who can put work into the fleet. For tighter identity control, Enterprise Managed Users give scoped accounts governed by your identity provider.

**Access scope.** Custom agents are restricted to the tools listed in their frontmatter. A review agent with read and search tools cannot modify code. The MCP allowlist limits which external tools the coding agent can call, and read-only tools are preferred because the agent uses them without prompting. The firewall allowlist restricts network egress from the agent's shell. Branch protection, `CODEOWNERS` and required reviewers control who must approve what.

**Sandbox.** Each coding agent task runs in an ephemeral Actions environment created for that task and destroyed on completion. No state carries between tasks, which limits the blast radius of any single agent run. This is the model's ephemeral sandbox requirement.

**Human accountability.** Branch protection and required reviewers make a person the named approver of every change into a protected branch. The audit log records that approval. This satisfies the model's non-negotiable principle that a named person is accountable for what agents produce.

## 7. Cadence mapping

**Execution Tier, continuous.** Coding agents work against Ready Issues. Agent HQ provides a mission-control view to start, watch and steer multiple agent tasks across repositories from github.com, the IDE, the CLI or mobile.

**Governance Tier, daily.** The pull request queue is the agenda. The Verification Engineer works through overnight pull requests, the Product Owner answers clarifications recorded on Issues and the backlog is reprioritised in the Project. Branch protection holds the approval gate.

**Strategic Tier, weekly.** The System Steward presents `COHERENCE-REGISTER.md`, reviews architectural decisions, assesses technical debt and commissions refactoring as remediation Issues. Metrics are reviewed here. Copilot usage metrics give some of the input, though cost per specification needs the instrumentation noted below.

## 8. Gaps and what to add

GitHub is strong on execution and governance. The ADM needs more than GitHub provides natively in four areas.

**Requirements discovery.** There is no native discovery surface. Run discovery through a requirements custom agent in Copilot Chat, through Claude or outside GitHub, then record the result as an Issue. Plan a specification-to-Issue workflow that bridges this gap.

**Multi-agent orchestration.** Copilot's coding agent works one Issue at a time. There is no native Scheduling Agent that reads the backlog, detects specification overlap and coordinates a fleet. Actions can sequence steps but not orchestrate agents. For the model's Scheduling Agent and agent-to-agent coordination, introduce an orchestration layer. The Claude Agent SDK loop in [CLAUDE-AGENTIC-LOOPS-ADM.md](CLAUDE-AGENTIC-LOOPS-ADM.md) is the natural fit, calling GitHub through MCP while Copilot handles repo-native execution.

**Governance dashboard.** There is no single view of specifications in flight, the review queue, blocked items and escalations. Build it from a GitHub Project plus the API, or use an external tracker that reads from GitHub.

**Feature flags and cost per specification.** GitHub has no native feature flag management, so integrate a flag service from the start. Copilot meters usage in premium requests at the account level, not per specification, so tag agent runs and Actions cost to the Issue to get the model's cost-per-specification metric in currency.

## 9. Reference operating pattern

A practical GitHub and Claude pattern that an organisation can adopt:

1. Standards are codified in `AGENTS.md` at the repo root, in path instructions and in the organisation `.github` repository.
2. The Product Owner records an Intent Brief as an Issue. A requirements custom agent drafts the Intent Specification, recorded as an Issue using the Issue Form.
3. The Intent Architect reviews for feasibility and scope. The Definition of Ready checklist passes and the Issue moves to Ready in the Project.
4. The Issue is assigned to the Copilot coding agent on a selected Claude model. The agent opens a draft pull request and works in an ephemeral environment on an isolated branch.
5. Actions workflows and the review, security and testing custom agents run against the pull request. The agent iterates on technical feedback within the session.
6. The Verification Engineer reviews the pull request at the daily Governance Tier. Branch protection and required reviewers enforce the gate, and the specification author does not approve.
7. On merge, CI/CD promotes the change through environments behind a feature flag. The Product Owner activates the flag when the Strategic Intent is complete.
8. The System Steward maintains the Coherence Register and commissions refactoring as remediation Issues at the weekly Strategic Tier.
9. Provenance is assembled from the Issue, pull request, commits, Actions logs and audit log. Cost and usage are tagged back to the specification.

## 10. Source notes

This guide is based on public product documentation:

- [GitHub Copilot: Anthropic Claude](https://docs.github.com/en/copilot/concepts/agents/anthropic-claude)
- [GitHub Copilot: About custom agents](https://docs.github.com/en/copilot/concepts/agents/cloud-agent/about-custom-agents)
- [GitHub Copilot: Custom agents configuration](https://docs.github.com/en/copilot/reference/custom-agents-configuration)
- [GitHub Copilot: Creating custom agents for the cloud agent](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/create-custom-agents)
- [GitHub Copilot: Adding repository custom instructions](https://docs.github.com/copilot/customizing-copilot/adding-custom-instructions-for-github-copilot)
- [GitHub Copilot: Configure the development environment](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/coding-agent/customize-the-agent-environment)
- [GitHub Copilot: Customising the agent firewall](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)
- [GitHub Copilot: Extending the coding agent with MCP](https://docs.github.com/copilot/how-tos/agents/copilot-coding-agent/extending-copilot-coding-agent-with-mcp)
- [Copilot coding agent now supports AGENTS.md](https://github.blog/changelog/2025-08-28-copilot-coding-agent-now-supports-agents-md-custom-instructions/)
- [Model selection for Claude and Codex agents on github.com](https://github.blog/changelog/2026-04-14-model-selection-for-claude-and-codex-agents-on-github-com/)
- [Pick your agent: use Claude and Codex on Agent HQ](https://github.blog/news-insights/company-news/pick-your-agent-use-claude-and-codex-on-agent-hq/)

GitHub product capabilities are evolving. Revisit this guide as the ecosystem matures.
