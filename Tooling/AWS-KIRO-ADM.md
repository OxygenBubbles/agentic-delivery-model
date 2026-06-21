# AWS Kiro: ADM Implementation Guide

> **Alpha.** This guide reflects the current state of Kiro and the AWS delivery stack. It will be updated as tooling matures and practical experience grows. File paths, directory names and flags will change.

This is a hands-on implementation guide. For the higher-level view of where the AWS ecosystem is ready and where the gaps are, read [AWS-ADM.md](AWS-ADM.md) first. This guide shows how to run the Agentic Delivery Model on AWS using Kiro, the spec-first agentic tool that replaced Amazon Q Developer. It covers where Intent Specifications live, where architectural standards live, how each ADM agent role is configured and how a specification moves from intent to merged code.

Kiro is a close fit to the ADM because it is built around specifications. Its requirements, design and task model maps almost one to one onto the Intent Specification, and its autonomous agent produces merge-ready pull requests rather than IDE-level assistance. Read [MODEL.md](../MODEL.md) for the roles, artefacts and cadence referenced throughout.

## 1. The build at a glance

| ADM concept | Kiro mechanism |
|---|---|
| Intent Brief and Intent Specification | A Kiro spec under `.kiro/specs/<feature>/`: `requirements.md`, `design.md`, `tasks.md`. Linked to a Jira, Linear or GitHub item |
| Acceptance criteria | EARS notation in `requirements.md`, the spec-driven equivalent of the model's Gherkin |
| Ready backlog | The Ready state in the connected tracker, plus an approved spec |
| Company standards | Always-on steering files in `.kiro/steering/` |
| Project standards | Scoped steering files loaded by description or on demand |
| Coherence Register | `COHERENCE-REGISTER.md`, referenced from a steering file, reviewed weekly |
| Agent roles (skills) | Kiro custom agents, Agent Skills and Kiro Powers |
| Implementation Agent | Kiro's autonomous agent |
| Agent sandbox | Kiro's execution environment on an isolated branch, or headless CLI in CI/CD |
| Governance gates | Agent hooks in `.kiro/hooks/` plus autonomy settings |
| Tool access | MCP servers in `.kiro/settings/mcp.json` |
| Provenance | The spec documents, the pull request and the session log |
| Governance Tier | Pull request review on GitHub or GitLab |
| Release | CodePipeline or GitHub Actions, plus AWS AppConfig feature flags |

## 2. Where everything lives

### 2.1 Intent Briefs and Intent Specifications

Kiro makes the specification a first-class artefact in the repository. A spec lives under `.kiro/specs/<feature>/` as three files: `requirements.md` holds the requirements in EARS notation, `design.md` holds the technical design and `tasks.md` holds the implementation breakdown. This is the model's design phase made concrete. The requirements file is the Intent Specification, the design file is the feasibility and scope work an Intent Architect owns and the task file is the decomposition the model otherwise leaves to the agent.

Because Kiro reads the spec from the repository, the spec travels with the code and becomes the handover document between sessions, which is the model's cross-session continuity mechanism. The backlog itself still lives in Jira, Linear or GitHub, reached through MCP, and the tracker item links to the spec. The tracker holds priority and the Ready state. The spec holds the detail.

### 2.2 Architectural and standards information

Standards live in steering files under `.kiro/steering/`. Each is a Markdown file with optional YAML frontmatter that sets its inclusion mode. An always-on file is loaded into every session and carries the company-level standards: stack, conventions, API rules, testing expectations, security standards and domain rules. An auto file is loaded only when the request matches its description, which suits project-level standards for a specific area. A manual file is pulled in on demand through a slash command. Workspace steering sits in `.kiro/steering/` and applies to that repository. Global steering sits under the user's home `~/.kiro` and applies everywhere. This expresses the model's split between company-level and project-level standards.

The Coherence Register is held as `COHERENCE-REGISTER.md` in the repository and referenced from an always-on steering file so agents read it as context. The System Steward versions it like code, and the weekly Strategic Tier reviews it.

### 2.3 Agent configuration and skills

Kiro offers several layers. Custom agents are defined through the Kiro CLI and carry their own prompt, tools and model, which is how the ADM agent roles are expressed. Agent Skills package reusable capabilities. Kiro Powers bundle a `POWER.md` manifest with keywords and onboarding, an optional `mcp.json` and a `steering/` folder, which is a way to ship a whole role as an installable unit. MCP servers are configured in `.kiro/settings/mcp.json` at workspace level or under `~/.kiro/settings/` globally, and a spec or steering file controls whether they are included.

Governance gates are agent hooks under `.kiro/hooks/`. A hook has a matcher on internal tool names such as `fs_write` or `execute_bash` and runs before or after a tool call, and a hook placed in a subdirectory only fires for changes in that directory. This keeps automation contained and is the mechanism for enforcing the model's hard rules.

### 2.4 Provenance

The spec documents record what was asked and agreed. The pull request records what was built. The session log records every action. The tracker item links them. Together they give the model's provenance chain. AWS CloudTrail and IAM add the infrastructure-side audit record.

## 3. Configuring the agent fleet

Kiro's autonomous agent does the implementation. The specialist ADM roles are expressed as steering files, custom agents, Agent Skills and hooks rather than as separate products, which matches the model's idea that roles are configurations.

| ADM agent role | Kiro mechanism |
|---|---|
| Requirements Agent | Kiro spec mode, drafting `requirements.md` from a brief, supervised by a person |
| Implementation Agent | The autonomous agent working `tasks.md` |
| Testing Agent | Test expectations in the spec, plus a post-write hook that runs the suite |
| Review Agent | A read-only custom agent invoked on the diff |
| Security Agent | A custom agent plus a dependency and secret scan in CI |
| Documentation Agent | A custom agent scoped to docs, or a steering rule to update docs with code |
| Design Agent | Design steering files plus a review custom agent for user-facing work |
| Platform Agent | A custom agent for infrastructure, with human approval on every change |
| Refactoring Agent | The autonomous agent on a remediation spec from the Coherence Register |

A company-standards steering file, always loaded:

```markdown
---
inclusion: always
---

# Engineering standards

- Use the repository pattern for data access. No direct SQL in service classes.
- All API endpoints return the standard ErrorResponse schema.
- Every change includes tests covering the happy path, validation failure and error handling.
- Follow the security standards in SECURITY.md and the approved licence list.
```

A governance hook that keeps the agent off protected branches and runs tests after edits:

```json
{
  "hooks": [
    { "event": "preToolUse", "matcher": "execute_bash",
      "deny": ["git push origin main", "git commit * main"] },
    { "event": "postToolUse", "matcher": "fs_write",
      "run": "npm test && npm run lint" }
  ]
}
```

Keep the always-on steering tight, because it is loaded into every session and is the biggest single lever on output quality.

## 4. The delivery cycle, step by step

### Design phase

1. The Product Owner records an Intent Brief in the tracker. Discovery with stakeholders happens through a requirements custom agent or outside Kiro, because Kiro plans from a brief but does not facilitate stakeholder sessions.
2. Kiro spec mode produces `requirements.md` in EARS, then `design.md`, then `tasks.md`. A person reviews and approves each step. The Intent Architect confirms feasibility and scope at the design stage. This is the model's design phase, with a human gate at each transition.
3. When the spec is approved and the tracker item is Ready, the work is cleared for execution.

### Build phase

4. Kiro's autonomous agent works through `tasks.md` on an isolated branch, never on a protected branch. It implements, runs builds and tests and iterates.
5. The agent opens a pull request on GitHub or GitLab, monitors CI and fixes failures without a person re-prompting. The testing, review and security configurations run against the change.
6. Technical questions resolve within the agent's loop. A question about business intent stops the work and returns to a person.
7. At the daily Governance Tier, the Verification Engineer reviews the pull request against the acceptance criteria and for structural consistency. Branch protection on the repository platform enforces the gate, and the spec author does not approve.
8. On merge, CI/CD promotes the change. Where it is part of a larger feature, it deploys behind an AppConfig feature flag that the Product Owner activates when the Strategic Intent is complete.

## 5. Governance, identity and guardrails

**Identity.** Agents run under scoped AWS IAM identities and the credentials for the repository platform, distinct from any person. No agent holds production credentials beyond what its task needs.

**Access scope.** Hooks deny protected-branch operations and run checks after edits. MCP servers expose only the operations an agent needs. Steering files constrain behaviour, and custom agents are limited to their declared tools. Autonomy is set to the lowest level that lets the work complete.

**Sandbox.** Work runs on an isolated branch in Kiro's execution environment, or headless in a CI/CD container. The repository platform's branch protection is the merge gate, because AWS has no native repository surface.

**Human accountability.** The pull request on GitHub or GitLab makes a named person the approver of every change into a protected branch, recorded in that platform's audit log and joined with AWS CloudTrail for the infrastructure side.

## 6. Cadence mapping

**Execution Tier, continuous.** Kiro's autonomous agent works approved specs, and headless `kiro` runs in CI/CD handle scheduled or batch work.

**Governance Tier, daily.** Pull requests queue on GitHub or GitLab. The Verification Engineer reviews against acceptance criteria, the Product Owner answers clarifications and the backlog is reprioritised.

**Strategic Tier, weekly.** The System Steward reviews `COHERENCE-REGISTER.md` and commissions refactoring specs. The team reviews steering quality, autonomy policy, model selection and cost.

## 7. Gaps and what to add

**Requirements discovery.** Kiro drafts and plans well from a brief but does not run stakeholder discovery. Use a requirements custom agent or run discovery outside Kiro, and record the result in the tracker.

**Fleet orchestration.** Kiro's autonomous agent works one spec at a time. There is no native Scheduling Agent that reads the backlog, detects specification overlap and coordinates a fleet. For that pattern, add an orchestration layer or sequence work through the tracker.

**Repository and governance surface.** AWS has no native repository or pull request surface, so GitHub or GitLab carries the governance gate. Plan that integration.

**Governance dashboard, feature flags and cost.** Build a consolidated view of specifications, the review queue and escalations from the tracker plus the repository platform. AppConfig covers feature flags. Tag sessions to the spec to get cost per specification in currency.

## 8. Reference operating pattern

1. Standards are codified in always-on and scoped steering files under `.kiro/steering/`. Agent roles are expressed as custom agents, Agent Skills and hooks. Governance gates are coded as hooks.
2. The Product Owner records an Intent Brief in the tracker. A requirements custom agent drafts the spec.
3. Kiro spec mode produces requirements, design and tasks. The Intent Architect approves feasibility and scope, and the spec is marked Ready.
4. Kiro's autonomous agent implements `tasks.md` on an isolated branch and opens a pull request on GitHub or GitLab.
5. Hooks enforce the gates. The agent monitors CI and fixes failures, and the review and security configurations run.
6. The Verification Engineer reviews the pull request at the daily Governance Tier. The spec author does not approve.
7. On merge, CI/CD promotes the change behind an AppConfig flag the Product Owner activates when the Strategic Intent is complete.
8. The System Steward maintains the Coherence Register and commissions refactoring at the weekly Strategic Tier. Cost per specification is tracked from tagged sessions.

## 9. Source notes

This guide is based on public documentation:

- [Kiro](https://kiro.dev/)
- [Kiro: Steering](https://kiro.dev/docs/steering/)
- [Kiro: CLI custom agent configuration reference](https://kiro.dev/docs/cli/custom-agents/configuration-reference/)
- [Kiro and MCP servers (AWS)](https://repost.aws/articles/ARuX8rkojgSx-TYCc65JyAOw/getting-started-with-kiro-and-mcp-servers-connect-your-ai-ide-to-real-world-tools)
- [Kiro documentation (AWS)](https://aws.amazon.com/documentation-overview/kiro/)
- [AWS CodePipeline](https://docs.aws.amazon.com/codepipeline/)
- [AWS AppConfig feature flags](https://docs.aws.amazon.com/appconfig/)

Kiro is evolving rapidly. Revisit this guide as the tool matures.
