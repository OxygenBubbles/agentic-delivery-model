# Claude Agentic Loops and Dynamic Workflow: ADM Implementation Guide

> **Alpha.** This guide reflects the current state of Claude Code, the Claude Agent SDK and the Claude GitHub Action. It will be updated as tooling matures and practical experience grows. File paths, SDK function names and flags will change.

This is a hands-on implementation guide, not a readiness assessment. For the higher-level view of where Claude Code and the Agent SDK are ready and where the gaps are, read [CLAUDE-CODE-ADM.md](CLAUDE-CODE-ADM.md) first. This guide shows how to run the Agentic Delivery Model with Claude as an agentic loop: who configures what, where intent and standards are stored, how each ADM agent role is defined and how a specification moves from intent to merged code under a dynamic, orchestrated workflow.

It assumes familiarity with the model. Read [MODEL.md](../MODEL.md) for the roles, artefacts and cadence referenced throughout.

## 1. Three ways to run Claude for the ADM

Claude supports the ADM in three modes. Most organisations will use all three, each for the part of the cycle it fits.

**Interactive Claude Code.** A person runs Claude Code at the terminal or in the IDE. This fits the design phase and verification: a Product Owner drafting a specification with a requirements subagent, an Intent Architect checking feasibility and scope, a Verification Engineer reviewing a changeset against acceptance criteria. The person stays in the loop and steers each step.

**The headless orchestration loop.** The Claude Agent SDK runs the same agent loop that powers Claude Code from inside your own program, with no terminal. This is the dynamic workflow. A parent orchestrator reads the Ready backlog, dispatches role subagents into isolated sandboxes, coordinates them and escalates intent questions to people. This is where the model's Scheduling Agent and continuous Execution Tier are realised.

**Claude inside GitHub.** The Claude GitHub Action runs the agent loop in CI, triggered by a mention or an assignment. This gives repo-native automation without standing up your own orchestrator, and it sits alongside GitHub Copilot's coding agent. See Section 8.

The dividing line is who drives the loop. Interactive mode, a person drives. Headless mode, your orchestrator drives. GitHub mode, a workflow event drives.

## 2. At a glance

| ADM concept | Claude mechanism |
|---|---|
| Intent Brief and Intent Specification | Backlog item in Jira, Linear or GitHub, read and written through MCP. Optionally mirrored as a file in the repo |
| Ready backlog | A query the orchestrator runs through MCP for items in the Ready state |
| Company standards | `CLAUDE.md` (or `AGENTS.md`) at the repo root, plus user and enterprise memory files |
| Project standards | Per-directory `CLAUDE.md`, loaded by proximity |
| Coherence Register | `COHERENCE-REGISTER.md` in the repo, read by agents and reviewed weekly |
| Agent roles (skills) | Subagents in `.claude/agents/*.md`, plus reusable skills in `.claude/skills/` |
| Scheduling Agent | The parent orchestrator built on the Agent SDK `query()` loop |
| Implementation, Testing, Review, Security, Documentation, Design, Platform, Refactoring agents | Subagents, each with its own context, tools and model |
| Agent sandbox | A container or git worktree per task, with scoped credentials |
| Governance gates | Hooks (`PreToolUse`, `PostToolUse`, `Stop`) plus permission settings |
| Tool access | MCP servers declared per agent, allow and deny lists in settings |
| Provenance | Session logs, commit trailers, the backlog item and the pull request |
| Governance Tier | Pull request review on the source platform, outside the loop |
| Release | The organisation's CI/CD, triggered by the merged pull request |

## 3. Where everything lives

### 3.1 Intent Briefs and Intent Specifications

Claude Code is not the backlog. Intent Briefs and Intent Specifications live in the organisation's backlog tool: Jira, Linear, Azure Boards or GitHub Issues. Claude reaches them through an MCP server for that tool. The orchestrator reads the Ready queue through MCP, and agents post status and clarifications back as comments through the same server, scoped so they cannot delete or reassign items.

For teams that prefer specifications to travel with the code, mirror each Ready specification as a file under `specs/` in the repository. The file is then part of the context the implementation agent receives directly, and the specification itself becomes the handover document between agent sessions, which is the model's cross-session continuity mechanism.

### 3.2 Architectural and standards information

Standards live in memory files that Claude loads automatically. `CLAUDE.md` at the repository root holds company-level standards: stack, conventions, API rules, testing expectations, security standards and domain rules. Claude also reads `AGENTS.md` if you prefer the cross-tool standard, which keeps the same file working across Claude, Copilot and other agents.

Project-level standards live in per-directory `CLAUDE.md` files. Claude loads the file nearest to the code it is working on, so a service directory can extend the root standards with its own bounded-context rules. User-level standards live in `~/.claude/CLAUDE.md` and apply across every project on that machine, and enterprise-managed memory applies organisation-wide. This layering expresses the model's split between company-level and project-level standards.

The Coherence Register is held as `COHERENCE-REGISTER.md` in the repository so agents read it as context and the System Steward versions it like code. The weekly Strategic Tier session reviews it, and remediation items flow back into the backlog as refactoring specifications.

### 3.3 Agent configuration and skills

Two mechanisms carry the model's idea that agent roles are configurations, not separate products.

Subagents are the agent roles. Each is a Markdown file with YAML frontmatter in `.claude/agents/<name>.md` for the project, or `~/.claude/agents/` for personal use. The frontmatter sets the name, a description that tells the orchestrator when to delegate, the allowed tools and the model. The body is the system prompt. A subagent runs in its own context window, so its work does not crowd the main conversation, and it returns only its result.

Skills are reusable procedures. Each is a folder under `.claude/skills/<name>/` containing a `SKILL.md` with a name and description, loaded only when relevant. Skills hold the team's repeatable methods: how to write an Intent Specification, how to run the Definition of Ready check, how to format a provenance trailer. Subagents do not inherit skills automatically, so list the skills a subagent needs in its configuration.

Supporting configuration sits alongside. Slash commands in `.claude/commands/*.md` give people repeatable prompts for the design and verification phases. MCP servers are declared in `.mcp.json` for the project. Permissions and hooks live in `.claude/settings.json`, version-controlled, with `.claude/settings.local.json` for personal overrides.

### 3.4 Provenance

Each agent session produces a log of every action and decision. Commits carry trailers that record the agent, the model and the specification. The backlog item links to the pull request. The pull request carries the session summary. Joined together these give the model's provenance chain. Because the logs are not forwarded anywhere by default, a regulated environment should configure forwarding to its audit store.

## 4. Configuring the agent fleet

Each ADM agent role is a subagent. The table shows the mapping, the tool scope and a sensible model. Scope tools tightly. A review agent that cannot write code cannot damage the codebase.

| ADM agent role | Subagent file | Tool scope | Model |
|---|---|---|---|
| Requirements Agent | `.claude/agents/requirements.md` | Read, MCP to backlog and docs | Sonnet |
| Implementation Agent | `.claude/agents/implementation.md` | Read, Edit, Bash for build and test | Opus or Sonnet |
| Testing Agent | `.claude/agents/testing.md` | Read, Edit on test files, Bash for test runners | Sonnet |
| Review Agent | `.claude/agents/review.md` | Read, search only | Sonnet |
| Security Agent | `.claude/agents/security.md` | Read, search, MCP to vulnerability data | Sonnet |
| Documentation Agent | `.claude/agents/documentation.md` | Read, Edit on docs | Sonnet |
| Design Agent | `.claude/agents/design.md` | Read, search, MCP to the design system | Sonnet |
| Platform Agent | `.claude/agents/platform.md` | Read, Edit on non-production config, human approval | Opus |
| Refactoring Agent | `.claude/agents/refactoring.md` | Read, Edit, Bash for build and test | Opus or Sonnet |

The Implementation Agent subagent:

```markdown
---
name: implementation
description: Implements a Ready Intent Specification on an isolated branch, runs tests and prepares a changeset. Delegate when a specification has passed the Definition of Ready.
tools: ["Read", "Edit", "Bash", "Grep", "Glob"]
model: opus
---

You are the Implementation Agent in an Agentic Delivery Model team.

Work only from the assigned Intent Specification and the standards in CLAUDE.md
and the nearest directory CLAUDE.md. For the specification:
- implement the change on the feature branch you were given, never on a
  protected branch
- write or update tests to cover every acceptance criterion
- run the build, tests and linters before you finish

Resolve technical questions yourself by reading the codebase and the standards.
If you hit a question about business intent, which behaviour is correct or scope,
stop and report it for human clarification rather than guessing. Return a summary
of the change, the tests added and any clarification needed.
```

The Review Agent subagent, read-only:

```markdown
---
name: review
description: Reviews a changeset against acceptance criteria and structural consistency. Returns findings only. Never edits code.
tools: ["Read", "Grep", "Glob"]
model: sonnet
---

You are the Review Agent. Given a changeset and its Intent Specification, confirm
that every acceptance criterion is met, that the change follows the standards and
that it is structurally consistent with the surrounding code. Flag duplication,
unjustified complexity and naming violations. Report a checklist. Escalate any
question about business intent to a person.
```

## 5. The dynamic workflow: the orchestration loop

This is the heart of the request. The model's Scheduling Agent and continuous Execution Tier are realised as a control loop built on the Agent SDK. A parent orchestrator holds the high-level goal, breaks work out to subagents, collects their results and decides what happens next. This is the operator pattern: one agent plans and directs, specialised subagents execute.

The loop, in plain terms:

1. Poll the Ready backlog through MCP. Take the highest-priority Ready specification.
2. The orchestrator acts as the Scheduling Agent. Before dispatching, it checks the specification boundaries, the "must not touch" and integration points, against work already in progress. On overlap it combines, sequences to the same agent or holds the second item, exactly as the model prescribes. A judgement call about priority or boundaries is escalated to a person.
3. Provision an isolated sandbox for the task: a container or a git worktree, with scoped credentials that allow the feature branch and nothing in production.
4. Dispatch the Implementation Agent subagent into the sandbox with the specification and the standards. It runs its own loop: read, implement, run tests, repeat.
5. Run the Testing, Review, Security and Documentation subagents. Because each runs in its own context, the orchestrator dispatches them in parallel and collects their findings, rather than growing one long context.
6. Resolve technical clarifications agent-to-agent. The implementation subagent consults the testing or review subagent through the orchestrator. An intent clarification stops the work, is written to the backlog item as a comment and is escalated to a person.
7. Create the pull request with the provenance trail attached.
8. The human Governance Tier reviews and approves the pull request. That gate sits outside the loop, which is the point. Agents never approve their own work.

A sketch of the orchestrator with the Agent SDK. It is illustrative, not a finished program:

```typescript
import { query } from "@anthropic-ai/claude-agent-sdk";

for (const spec of await readyBacklog()) {         // step 1, via MCP
  if (await overlapsInFlight(spec)) {               // step 2, Scheduling Agent
    await routeOverlap(spec);                        // combine, sequence or hold
    continue;
  }
  const sandbox = await provisionSandbox(spec);      // step 3, container or worktree

  await query({                                      // step 4 to 6
    prompt: `Deliver Intent Specification ${spec.id}. Follow CLAUDE.md.`,
    options: {
      cwd: sandbox.path,
      mcpServers: { backlog, vcs },                  // scoped tools
      agents: ["implementation", "testing", "review", "security", "documentation"],
      permissionMode: "default",                     // hooks enforce the gates
      hooks: governanceHooks,
    },
  });

  await openPullRequest(spec, sandbox);              // step 7
}                                                     // step 8 is human, outside the loop
```

### Hooks are the governance gates

Hooks make governance deterministic rather than relying on the model to behave. They run on lifecycle events and can block an action. Configure them in `.claude/settings.json`.

- A `PreToolUse` hook denies any commit or push to a protected branch, and denies access to secret files. This enforces branch isolation as a hard rule.
- A `PreToolUse` hook checks that the work item is actually in the Ready state before implementation begins, enforcing the Definition of Ready as a gate.
- A `PostToolUse` hook runs the test suite and the linter after edits, so failing changes never reach a pull request.
- A `Stop` or `SubagentStop` hook writes the provenance trailer and posts the session summary to the backlog item.

Because hooks are code, they are versioned and reviewed like the application. That matters, because an agent's configuration is part of its attack surface.

### Running the fleet in parallel

Multiple specifications run at once, each in its own sandbox on its own branch, which prevents file-level conflicts. The Agent SDK supports background agents for running many independent sessions and monitoring them from one place, and agent teams for sessions that need to communicate. How many run in parallel depends on backlog depth, review capacity and cost appetite. Pair the parallelism with cost circuit breakers, described in Section 6.

## 6. Interactive Claude Code for design and verification

Not every part of the cycle should be autonomous. The design phase and verification keep a person in the loop, and interactive Claude Code is the tool.

The Product Owner runs a requirements subagent to turn an Intent Brief into a draft Intent Specification, working from notes, transcripts and existing context reached through MCP. The agent probes for the real business problem, not just the stated request, and the person steers. This is paired discovery: the person and the agent in the session together when the domain is messy.

The Intent Architect uses a slash command that runs the Definition of Ready check against a draft and reports what is missing, before marking the specification Ready. The Verification Engineer reviews a changeset interactively, asking Claude to walk the diff against the acceptance criteria and to surface structural inconsistency the automated checks missed. In each case the person makes the call. Claude removes the mechanical work so the person spends judgement where it counts.

## 7. Governance, identity, guardrails and cost

**Identity.** Each agent session runs under a scoped service account, distinct from any person, recorded in commits and logs. The orchestrator holds no production credentials. Sandboxes receive only the credentials for their task.

**Access scope.** Tools are scoped twice: by the subagent frontmatter and by allow and deny lists in settings. MCP servers expose only the operations an agent needs, for example read work items and post comments but not delete. Deny lists block access to secrets and production.

**Sandboxes.** Each task runs in an ephemeral container or git worktree, created for the task and destroyed on completion, so no state carries between tasks. This is the model's ephemeral sandbox requirement and it limits the blast radius of any single run.

**Cost circuit breakers.** Agent cost can spike when a specification is ambiguous or an agent loops. Set spend alerts per specification and a hard limit that pauses the fleet when a daily or weekly budget is reached. Tag each session with the specification identifier so cost per specification can be measured in currency, which is the model's metric.

## 8. Claude inside GitHub

Claude runs in GitHub without a self-hosted orchestrator, through the Claude GitHub Action.

Install it with `/install-github-app` from Claude Code, which sets up the GitHub app and the required secret, or add the workflow by hand. The workflow uses `anthropics/claude-code-action@v1`. It activates on `@claude` mentions in issues and pull request comments, on issue assignment or on an explicit prompt for automation such as a scheduled audit. For authentication, prefer Workload Identity Federation, which exchanges the workflow's OIDC token for a short-lived Anthropic token so there is no static key to store. Amazon Bedrock, Google Vertex AI and Microsoft Foundry are also supported for teams that must keep inference inside a chosen boundary.

The same `CLAUDE.md`, `.claude/agents` and `.claude/skills` that drive local and headless runs drive the Action, so the fleet behaves the same way in CI as it does on a developer machine. Claude Code on the web is a further option, giving cloud sandboxes managed by Anthropic for running tasks without local setup.

This is where the two Claude paths meet GitHub. Copilot's coding agent gives repo-native execution and the pull request governance surface, covered in [GITHUB-COPILOT-CLAUDE-ADM.md](GITHUB-COPILOT-CLAUDE-ADM.md). The Claude orchestration loop gives the Scheduling Agent and agent-to-agent coordination that Copilot does not provide natively. A common pattern is to let the orchestrator plan and coordinate while either the Claude Action or Copilot performs the repo-native implementation and raises the pull request.

## 9. Cadence mapping

**Execution Tier, continuous.** The orchestration loop runs against the Ready backlog. Background agents handle parallel work and give one place to watch the fleet.

**Governance Tier, daily.** Pull requests created by the loop queue on the source platform. The Verification Engineer reviews against acceptance criteria, the Product Owner answers escalated clarifications and the backlog is reprioritised. Branch protection on the platform holds the gate.

**Strategic Tier, weekly.** The System Steward reviews `COHERENCE-REGISTER.md`, commissions refactoring specifications and the team reviews metrics, model selection and cost trends. Model choice per agent role is tuned here.

## 10. Gaps and what to add

**Backlog and governance surface.** Claude does not own the backlog or the pull request review surface. Both belong to the source platform, reached through MCP. Plan the MCP integration and the conventions that mark a specification Ready.

**Orchestration is yours to build.** The Agent SDK gives the loop, the subagents, the hooks and the permissions. The scheduling logic, the overlap detection and the escalation rules are code you write and test. Start simple, with one agent and a tight hook set, and add coordination as confidence grows.

**Consolidated visibility.** There is no single dashboard of specifications in flight, the review queue and escalations. Build it from the backlog tool plus the loop's own status output.

**Release and operations.** Claude generates changes and pull requests. CI/CD, deployment and monitoring belong to the existing platform. Close the feedback loop by letting the orchestrator read monitoring through MCP and raise follow-up specifications.

## 11. Reference operating pattern

1. Standards are codified in `CLAUDE.md` at the repo root and per directory. Agent roles are defined as subagents in `.claude/agents`, with reusable methods as skills in `.claude/skills`. Governance gates are coded as hooks in `.claude/settings.json`.
2. The Product Owner drafts an Intent Specification interactively with the requirements subagent, recorded in the backlog through MCP.
3. The Intent Architect runs the Definition of Ready check and marks the specification Ready.
4. The orchestration loop reads the Ready backlog. The Scheduling Agent checks for overlap and routes accordingly.
5. The Implementation Agent subagent runs in an isolated sandbox on a feature branch. Testing, Review, Security and Documentation subagents run in parallel and return findings.
6. Hooks enforce the gates: no protected-branch commits, tests run after edits, provenance written on completion. Technical clarifications resolve agent-to-agent. Intent clarifications escalate to a person.
7. A pull request is created with the provenance trail. The Verification Engineer reviews it at the daily Governance Tier, and the specification author does not approve.
8. On merge, the platform's CI/CD promotes the change. The orchestrator can read monitoring and raise follow-up specifications.
9. The System Steward maintains the Coherence Register and commissions refactoring at the weekly Strategic Tier. Cost per specification is tracked from tagged sessions.

## 12. Source notes

This guide is based on public documentation:

- [Claude Code: Subagents](https://code.claude.com/docs/en/sub-agents)
- [Claude Code: Agent SDK overview](https://code.claude.com/docs/en/agent-sdk/overview)
- [Claude Agent SDK: Subagents](https://platform.claude.com/docs/en/agent-sdk/subagents)
- [Building agents with the Claude Agent SDK](https://www.anthropic.com/engineering/building-agents-with-the-claude-agent-sdk)
- [Steering Claude Code: skills, hooks, rules, subagents](https://claude.com/blog/steering-claude-code-skills-hooks-rules-subagents-and-more)
- [Claude Code: GitHub Actions](https://code.claude.com/docs/en/github-actions)
- [anthropics/claude-code-action](https://github.com/anthropics/claude-code-action)
- [Model Context Protocol](https://modelcontextprotocol.io/)

Claude Code and the Agent SDK are evolving rapidly. Revisit this guide as capabilities mature.
