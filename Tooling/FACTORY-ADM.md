# Factory.ai (Droids): ADM Implementation Guide

> **Alpha.** This guide reflects the current state of Factory.ai and the Droid CLI. It will be updated as tooling matures and practical experience grows. File paths, flags and command names will change.

This is a hands-on implementation guide. It shows how to run the Agentic Delivery Model on Factory.ai using Droids: where Intent Specifications are stored, where architectural standards live, how each ADM agent role is configured as a custom droid, which droid does what and how a specification moves from intent to merged code under an orchestrated, governed workflow.

Factory maps unusually well to the ADM. It ships a coordinator that decomposes work and dispatches to specialised droids, a two-phase specification mode that mirrors the model's design and build split, tiered autonomy that maps to governed checkpoints and a single standards file the droids read. Where the GitHub and Claude guides have to assemble orchestration from parts, Factory provides much of it natively.

It assumes familiarity with the model. Read [MODEL.md](../MODEL.md) for the roles, artefacts and cadence referenced throughout.

## 1. The build at a glance

| ADM concept | Factory mechanism |
|---|---|
| Intent Brief and Intent Specification | A ticket in Jira, Linear or GitHub, reached through the integration or MCP. Optionally mirrored as a spec file in the repo |
| Specification planning | Specification mode (`--use-spec`), which plans before implementing |
| Ready backlog | The Ready state in the connected tracker |
| Company standards | `AGENTS.md` at the repo root |
| Project standards | Nested `AGENTS.md` files in subdirectories |
| Coherence Register | `COHERENCE-REGISTER.md` in the repo, reviewed weekly |
| Agent roles (skills) | Custom droids in `.factory/droids/*.md`, plus reusable skills |
| Scheduling Agent and agent coordination | Mission mode, an orchestrator with worker and validator agents |
| Implementation, Testing, Review, Security, Documentation, Design droids | Custom droids, each with its own prompt, tools and model |
| Agent sandbox | A git worktree per task, local execution through Factory Bridge or remote execution |
| Governed checkpoints | Autonomy levels plus lifecycle hooks |
| Tool access | MCP servers declared per droid, with an enterprise MCP policy |
| Provenance | Session logs, the connected ticket and the pull request |
| Governance Tier | Pull request review on GitHub or GitLab |
| Release | CI/CD triggered by the merge, or `droid exec` at high autonomy |

## 2. Where everything lives

### 2.1 Intent Briefs and Intent Specifications

Factory is not the backlog. Intent Briefs and Intent Specifications live in Jira, Linear or GitHub, reached through Factory's native integrations or an MCP server. A droid can read the ticket, the linked design docs and the related logs, then work against them. Status and clarifications go back to the ticket as comments, scoped by the integration's permissions.

Specification mode adds a planning step that suits the model well. Started with `--use-spec`, a droid first produces a plan from the ticket, then implements against it. This is the model's design phase and build phase in one tool: the plan is the executable intent, reviewed before any code is written. You can run planning on a stronger model than implementation with `--spec-model`, which matches the model's guidance that different work justifies different models.

For teams that want the specification to travel with the code, mirror each Ready specification as a file under `specs/` so it is part of the droid's context and serves as the handover document between sessions.

### 2.2 Architectural and standards information

`AGENTS.md` at the repository root is the single source of truth that governs how droids work. It holds the company-level standards: stack, conventions, API rules, testing expectations, security standards and domain rules. Nested `AGENTS.md` files in subdirectories carry project-level standards for a bounded context, and the droid reads the file nearest the code. This expresses the model's split between company-level and project-level standards in one mechanism.

The Coherence Register is held as `COHERENCE-REGISTER.md` in the repository so droids read it as context and the System Steward versions it like code. The weekly Strategic Tier reviews it, and remediation flows back as refactoring specifications.

### 2.3 Droid configuration and skills

Custom droids are the ADM agent roles. Each is a Markdown file with YAML frontmatter, stored in `.factory/droids/<name>.md` for the project and shared with the team, or in `~/.factory/droids/` for personal use. Project droids override personal ones of the same name. The frontmatter sets the name, description, model, reasoning effort, the allowed tools and the MCP servers. The body is the system prompt. Each droid runs with a fresh context window, so its work does not bloat the main session.

The `tools` field takes either a category or an explicit list. The categories are `read-only` for `Read`, `LS`, `Grep` and `Glob`; `edit` for `Create`, `Edit` and `ApplyPatch`; `execute` for `Execute`; and `web` for `WebSearch` and `FetchUrl`. The `mcpServers` field limits a droid to named servers from `.factory/mcp.json`. A built-in `GenerateDroid` tool scaffolds a droid from a short description, and existing Claude Code subagents can be imported, so a team that has defined its fleet in Claude Code can reuse it here.

Skills hold reusable procedures, created with `/create-skill` and managed with `/skills`. Lifecycle hooks are managed with `/hooks`. MCP servers are configured in `.factory/mcp.json` or with `droid mcp add`, drawing on a registry of more than 40 servers including Jira, Linear, Sentry and Notion.

### 2.4 Provenance

Each session produces a log of every action. The connected ticket links the work to its intent. The pull request carries the change and the session summary, and `/share` publishes a session to the organisation. Joined together these give the model's provenance chain. A regulated environment should forward session logs to its audit store.

## 3. Configuring the droid fleet

Each ADM agent role is a custom droid. The table maps the roles, with tool scope and a sensible model and reasoning effort. Scope tools tightly so a reviewer cannot change code.

| ADM agent role | Droid file | Tools | Model and effort |
|---|---|---|---|
| Requirements Agent | `.factory/droids/requirements.md` | `read-only`, MCP to tracker and docs | Sonnet, medium |
| Implementation Agent | `.factory/droids/implementation.md` | `["Read", "Edit", "Execute", "Grep", "Glob"]` | Opus, high |
| Testing Agent | `.factory/droids/testing.md` | `["Read", "Edit", "Execute"]` | Sonnet, medium |
| Review Agent | `.factory/droids/review.md` | `read-only` | Sonnet, high |
| Security Agent | `.factory/droids/security.md` | `read-only`, MCP to vulnerability data | Sonnet, high |
| Documentation Agent | `.factory/droids/documentation.md` | `["Read", "Edit"]` | Sonnet, low |
| Design Agent | `.factory/droids/design.md` | `read-only`, MCP to the design system | Sonnet, medium |
| Platform Agent | `.factory/droids/platform.md` | `["Read", "Edit"]`, human approval | Opus, high |
| Refactoring Agent | `.factory/droids/refactoring.md` | `["Read", "Edit", "Execute"]` | Opus, high |

A Review Agent droid, read-only so it cannot modify code:

```markdown
---
name: review-agent
description: Reviews a diff against acceptance criteria and structural consistency. Returns findings only.
model: inherit
reasoningEffort: high
tools: read-only
---

You are the Review Agent in an Agentic Delivery Model team.

Given the diff and the linked Intent Specification, confirm that every acceptance
criterion is met, that the change follows AGENTS.md and the nearest directory
AGENTS.md and that it is structurally consistent with the surrounding code. Flag
duplication, unjustified complexity and naming violations.

Respond with:
Summary: <one line>
Findings:
- <issue or no blockers>

Escalate any question about business intent to a person rather than guessing.
```

A Security Agent droid scoped to specific MCP servers:

```markdown
---
name: security-agent
description: Scans a change for vulnerabilities, secrets, injection risks and licence issues before human review.
model: inherit
reasoningEffort: high
tools: ["Read", "Grep", "Glob"]
mcpServers: ["sentry"]
---

You are the Security Agent. For the change under review, check new and updated
dependencies against known vulnerabilities and the approved licence list in
AGENTS.md, look for secrets and injection risks and confirm security-relevant
acceptance criteria. Report severity-rated findings and block on anything high or
critical.
```

## 4. The dynamic workflow: spec mode and mission mode

Factory realises the model's dynamic workflow through two features.

**Specification mode** is the design-then-build split. A run started with `--use-spec` plans the work before touching code. The plan is the point at which the Intent Architect and Product Owner confirm feasibility, scope and acceptance criteria, the model's design phase, before the build phase begins. Implementation then proceeds against the agreed plan.

**Mission mode** is the Scheduling Agent and agent-to-agent coordination realised natively. Started with `--mission`, an orchestrator decomposes the work and dispatches it to worker agents, with validator agents checking the output. You set the models for each tier: `--worker-model` for the workers and `--validator-model` for the validators, each with its own reasoning effort. The orchestrator sequences the workers to avoid conflicts and the validators provide the agent-side verification that precedes human review. Mission Control, opened with Ctrl+T in the interactive session, gives the single view of the fleet that the model's Execution Tier needs.

The mapping to the model is close. The orchestrator is the Scheduling Agent. The workers are the Implementation, Testing, Documentation and other execution droids. The validators perform the automated verification that the Review and Security agents own, before the human Governance Tier. Overlap handling, the model's combine, sequence or hold logic, is the orchestrator's job.

For continuous execution, `droid exec` runs the loop headless, suited to CI pipelines, scheduled jobs and batch work. A mission can run headless too, with `droid exec --mission -f mission.md`. Parallel work uses git worktrees, one branch per task in its own directory, so concurrent droids do not collide:

```bash
# Two specifications in flight at once, each isolated on its own branch
droid exec -w spec-101 --auto medium -f specs/101.md &
droid exec -w spec-102 --auto medium -f specs/102.md &
wait
```

## 5. Governance, identity and guardrails

**Autonomy levels are the governed checkpoints.** `droid exec` runs read-only by default. `--auto low` permits safe edits and formatters. `--auto medium` permits dependency installs, builds, tests and local commits, which is the right level for implementation. `--auto high` permits push and deploy, reserved for trusted CI steps. `--skip-permissions-unsafe` removes all guardrails and belongs only in a disposable container. Mapping each ADM activity to the lowest autonomy that lets it complete is the practical expression of proportionate governance.

**Identity and access.** Each droid is restricted to the tools in its frontmatter and the MCP servers it names. An enterprise MCP policy blocks servers that are not approved, even if a droid lists them, which keeps tool access governed centrally. API keys are held in environment variables, never in source.

**Sandboxes.** Each task runs in an isolated git worktree, locally through the Factory Bridge app or in a remote environment. A clean worktree is removed automatically when a headless run finishes, and a dirty one is preserved for review. This gives the model's per-task isolation and limits the blast radius of any single run.

**Human accountability.** Validators and autonomy levels keep agents from approving their own work. The pull request on GitHub or GitLab is the human gate, and branch protection there makes a named person the approver of every change into a protected branch.

## 6. Interactive and headless

Use the interactive `droid` for the design phase and verification, where a person steers. The Product Owner drafts a specification with a requirements droid working from the ticket and design docs. The Intent Architect uses specification mode to produce and confirm a plan. The Verification Engineer uses `/review` to walk a change against acceptance criteria. Slash commands and the `/skills` library hold the repeatable methods.

Use `droid exec` for the Execution Tier, where the fleet runs autonomously against Ready specifications at a controlled autonomy level, in CI or on a schedule. The same `AGENTS.md`, droids and skills drive both, so behaviour is consistent across interactive and headless runs.

## 7. Integrations

Factory connects to the tools the team already uses, so droids see what people see. Native integrations cover GitHub and GitLab for code and pull requests, Jira and Linear for the backlog, Slack and Teams for collaboration and Sentry and PagerDuty for incidents. This matters for the model's feedback loop: an incident in PagerDuty or an error in Sentry can become an Intent Brief through the same backlog, and a droid can triage it.

Factory Bridge connects a droid to a local development environment, so it can run builds and tests against the local toolchain while the session is coordinated centrally. Work can also run against a remote repository directly. This flexibility lets a regulated team keep execution inside its own environment while still using the platform's orchestration.

## 8. Cadence mapping

**Execution Tier, continuous.** `droid exec` and missions run against Ready specifications. Mission Control gives the single view of the fleet.

**Governance Tier, daily.** Pull requests created by the droids queue on GitHub or GitLab. The Verification Engineer reviews against acceptance criteria, the Product Owner answers escalated clarifications and the backlog is reprioritised. Branch protection holds the gate.

**Strategic Tier, weekly.** The System Steward reviews `COHERENCE-REGISTER.md` and commissions refactoring. The team reviews metrics, autonomy policy, model selection per droid and cost. The readiness report (`/readiness-report`) gives a useful input to this session.

## 9. Gaps and what to add

**Requirements discovery.** Factory drafts and plans well from a ticket, but it does not facilitate stakeholder discovery. Run discovery through a requirements droid working from notes and existing context, or outside the tool, then record the result in the tracker.

**Governance dashboard.** Mission Control shows the fleet, but a consolidated view of specifications in flight, the review queue and escalations across trackers needs building from the tracker plus session output.

**Feature flags and cost per specification.** Factory has no native feature flag management, so integrate a flag service. Tag sessions to the specification so cost can be measured in currency, which is the model's metric.

**Regulated controls.** Autonomy levels, the enterprise MCP policy and Bridge-based local execution give strong building blocks, but data residency, model governance and the audit trail still need deliberate configuration. Keep inference inside the trust boundary with BYOK or local execution where the sector demands it.

## 10. Reference operating pattern

1. Standards are codified in `AGENTS.md` at the repo root and per directory. Droid roles are defined in `.factory/droids`, with reusable methods as skills and governance gates as hooks.
2. The Product Owner drafts an Intent Specification with the requirements droid, recorded in Jira or Linear through the integration.
3. The Intent Architect runs specification mode to produce and confirm a plan, then marks the specification Ready.
4. A mission runs against the Ready specification. The orchestrator decomposes the work, dispatches worker droids in isolated worktrees and runs validators.
5. The Implementation droid works at `--auto medium`. Testing, Review, Security and Documentation droids run and return findings. Technical clarifications resolve agent-to-agent. Intent clarifications escalate to a person.
6. A pull request is opened on GitHub or GitLab with the session trail. The Verification Engineer reviews it at the daily Governance Tier, and the specification author does not approve.
7. On merge, CI/CD promotes the change behind a feature flag. The Product Owner activates the flag when the Strategic Intent is complete.
8. The System Steward maintains the Coherence Register and commissions refactoring at the weekly Strategic Tier. Cost per specification is tracked from tagged sessions.

## 11. Source notes

This guide is based on public documentation:

- [Factory: Custom Droids (Subagents)](https://docs.factory.ai/cli/configuration/custom-droids)
- [Factory: Droid CLI Reference](https://docs.factory.ai/reference/cli-reference)
- [Factory: Droid Exec overview](https://docs.factory.ai/cli/droid-exec/overview)
- [Factory: MCP configuration](https://docs.factory.ai/cli/configuration/mcp)
- [Factory: product and integrations](https://factory.ai/)
- [Factory is GA: Droids for the entire SDLC](https://factory.ai/news/factory-is-ga)

Factory.ai is evolving rapidly. Revisit this guide as capabilities mature.
