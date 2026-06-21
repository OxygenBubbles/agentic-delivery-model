# GitLab Duo Agent Platform: ADM Implementation Guide

> **Alpha.** This guide reflects the current state of the GitLab Duo Agent Platform. It will be updated as tooling matures and practical experience grows. File paths, folder names and flow definitions will change.

This is a hands-on implementation guide. For the higher-level view of where GitLab is ready and where the gaps are, read [GITLAB-ADM.md](GITLAB-ADM.md) first. This guide shows how to run the Agentic Delivery Model on the GitLab Duo Agent Platform, which reached general availability in January 2026. It covers where Intent Specifications live, where standards live, how each ADM agent role is configured and how a specification moves from intent to merged code.

GitLab is the most end-to-end single-platform fit of the ecosystems assessed, because the backlog, the agents, multi-agent flows, CI/CD, feature flags and the audit trail are all native and in one place. Read [MODEL.md](../MODEL.md) for the roles, artefacts and cadence referenced throughout.

## 1. The build at a glance

| ADM concept | GitLab mechanism |
|---|---|
| Intent Brief and Intent Specification | A work item (issue), optionally mirrored as a spec file in the repository |
| Strategic Intent | An epic grouping related work items |
| Ready backlog | A board column or label, gated by the Definition of Ready |
| Company standards | `AGENTS.md` at the repository root |
| Project standards | Nested `AGENTS.md` files, combined with the user-level file |
| Coherence Register | `COHERENCE-REGISTER.md` plus a tracked work item, reviewed weekly |
| Agent roles (skills) | Foundational agents, custom agents and custom flows in `.gitlab/duo/` |
| Scheduling Agent and coordination | GitLab Flows, for example the Software Development Flow |
| Implementation, Testing, Security, Documentation agents | Duo foundational and custom agents |
| Agent sandbox | Duo execution on an isolated branch |
| Tool access | MCP server configuration in `.gitlab/duo/` |
| Provenance | The merge request, audit events and the linked work item |
| Governance Tier | Merge request review, approval rules, code owners |
| Release | GitLab CI/CD and native feature flags |

## 2. Where everything lives

### 2.1 Intent Briefs and Intent Specifications

Intent Briefs and Intent Specifications are GitLab work items. Issues hold individual specifications and epics group them under a Strategic Intent, which gives the release-management grouping the model needs for feature flags. Use a description template so every specification captures the Intent Specification fields: business intent, acceptance criteria, technical constraints, test expectations and complexity. The backlog is a board, and a specification becomes Ready when the Definition of Ready checklist passes and it moves to the Ready column.

Where teams want the specification to travel with the code, mirror each Ready specification as a file under `specs/`, so it is part of the agent's context and serves as the handover document between sessions.

### 2.2 Architectural and standards information

Standards live in `AGENTS.md`. The platform reads an `AGENTS.md` at the repository root for company-level standards, and combines it with a user-level file and any nested `AGENTS.md` files for project-level standards in a specific area. This is a cross-tool standard, so the same file works with other agentic tools that support it. It documents the repository structure, coding conventions, style, build and test instructions and project context, and it feeds Duo Chat and both foundational and custom flows.

The Coherence Register is held as `COHERENCE-REGISTER.md` in the repository and mirrored as a tracked work item so remediation can be prioritised alongside feature work. The weekly Strategic Tier reviews it.

### 2.3 Agent configuration and skills

The Duo Agent Platform provides three kinds of agent: foundational agents for common tasks, custom agents for organisation-specific work and external agents that bring third-party capabilities. Custom flow definitions and the MCP server configuration live together in the `.gitlab/duo/` folder, version-controlled alongside the code. A custom flow can read the `AGENTS.md` context passed from the executor, and can be written to handle the case where no `AGENTS.md` exists. GitLab Flows coordinate several agents through a multi-step process, which is how the model's orchestration is expressed.

### 2.4 Provenance

The merge request records the change and the agent session. GitLab audit events record who did what, including agent actions. The work item links the change to its intent. On self-managed deployments the organisation controls the full audit store. Together these give the model's provenance chain.

## 3. Configuring the agent fleet

The foundational agents cover the common ADM roles out of the box. Custom agents and custom flows fill the rest. Scope each agent to the minimum it needs.

| ADM agent role | GitLab mechanism |
|---|---|
| Requirements Agent | A custom agent in Duo Chat, drafting from the work item and linked docs |
| Implementation Agent | The implementation step of the Software Development Flow |
| Testing Agent | A foundational test agent, plus the native test stage in CI |
| Review Agent | The Code Review capability on the merge request |
| Security Agent | Native SAST, DAST, dependency and container scanning, enforced on the merge request |
| Documentation Agent | A foundational or custom documentation agent |
| Design Agent | A custom agent plus design rules in `AGENTS.md` for user-facing work |
| Platform Agent | A custom agent for infrastructure, with human approval on every change |
| Refactoring Agent | The Software Development Flow on a remediation work item |

A root `AGENTS.md` carrying company standards:

```markdown
# Standards for agents

## Stack and conventions
- TypeScript services. Repository pattern for data access, no direct SQL in services.
- All endpoints return the standard error schema.

## Testing
- Every change includes unit tests for the happy path, validation failure and error handling.
- The pipeline must be green before a merge request is marked ready for review.

## Security
- Dependencies come from the approved registry only. The security scan must pass.
```

Custom flow definitions and MCP configuration sit in `.gitlab/duo/`, so the orchestration and the tools an agent can reach are reviewed and versioned like application code. GitLab's built-in security scanning is enforced through merge request rules, which gives the Security Agent role real teeth without a separate product.

## 4. The delivery cycle, step by step

### Design phase

1. The Product Owner records an Intent Brief as a work item. Discovery with stakeholders happens through a requirements custom agent in Duo Chat or outside GitLab, because GitLab is a specification repository rather than a discovery surface.
2. The draft Intent Specification is refined for business accuracy. The Intent Architect reviews for feasibility and scope and tightens the acceptance criteria.
3. When the Definition of Ready checklist passes, the work item moves to Ready.

### Build phase

4. The work item is assigned to the Software Development Flow. The flow plans the work, then coordinates the implementation, testing and documentation agents through the steps, working on an isolated branch.
5. The agents raise a merge request with the change and the results. Native security scanning, the test stage and the Code Review capability run against it.
6. Technical questions resolve within the flow. A question about business intent stops the work and returns to a person.
7. At the daily Governance Tier, the Verification Engineer reviews the merge request against the acceptance criteria and for structural consistency. Approval rules, code owners and protected branches enforce the gate, and the specification author does not approve.
8. On merge, GitLab CI/CD promotes the change. Where it is part of a larger feature, it deploys behind a native feature flag that the Product Owner activates when the Strategic Intent is complete.

## 5. Governance, identity and guardrails

**Identity.** Agents act under scoped service accounts, distinct from any person, with every action logged. SAML and LDAP integration cover enterprise sign-on. On self-managed deployments the organisation controls authentication and the audit store, which matters in regulated environments.

**Access scope.** Approval rules, code owners and protected branches control who must approve what. The MCP configuration in `.gitlab/duo/` limits which external tools agents reach. Custom flows and agents are version-controlled and reviewed like code.

**Sandbox.** Agents work on isolated branches and never on protected branches, so concurrent work does not collide at the file level. Security scanning runs in the pipeline before a merge request is eligible.

**Human accountability.** Merge request approval rules make a named person the approver of every change into a protected branch, recorded in GitLab audit events.

## 6. Cadence mapping

**Execution Tier, continuous.** The Software Development Flow and the Duo CLI work Ready work items. Flows coordinate the agents within each specification.

**Governance Tier, daily.** Merge requests queue. The Verification Engineer reviews against acceptance criteria, the Product Owner answers clarifications and the backlog is reprioritised. Approval rules hold the gate.

**Strategic Tier, weekly.** The System Steward reviews `COHERENCE-REGISTER.md` and commissions refactoring. The team reviews metrics, agent and flow configuration, model selection and cost.

## 7. Gaps and what to add

**Requirements discovery.** GitLab stores specifications but does not facilitate stakeholder discovery. Run it through a requirements custom agent or outside GitLab, and record the result as a work item.

**Fleet-level scheduling.** GitLab Flows coordinate agents within a single specification, which is closer to the model than most platforms. There is still no backlog-aware Scheduling Agent that reads the whole Ready backlog, detects specification overlap across work items and routes work proactively. Build a custom agent for that role or add an external orchestration layer.

**Governance dashboard.** Merge request governance is strong, but a consolidated view of specifications in flight, the review queue and escalations needs building from project views or a custom dashboard.

**Feature flags and cost.** Native feature flags are functional but basic compared with specialist platforms. Cost attribution to a specification is not built in, so tag agent usage to the work item to get cost per specification in currency.

## 8. Reference operating pattern

1. Standards are codified in `AGENTS.md` at the root and per area. Agent roles are foundational agents plus custom agents and custom flows in `.gitlab/duo/`. MCP configuration sits there too.
2. The Product Owner records an Intent Brief as a work item. A requirements custom agent drafts the Intent Specification.
3. The Intent Architect reviews for feasibility and scope, the Definition of Ready passes and the work item moves to Ready.
4. The Software Development Flow plans and coordinates the implementation, testing and documentation agents on an isolated branch and raises a merge request.
5. Native security scanning, the test stage and the Code Review capability run. Technical questions resolve in the flow. Intent questions escalate to a person.
6. The Verification Engineer reviews the merge request at the daily Governance Tier. The specification author does not approve.
7. On merge, GitLab CI/CD promotes the change behind a feature flag the Product Owner activates when the Strategic Intent is complete.
8. The System Steward maintains the Coherence Register and commissions refactoring at the weekly Strategic Tier. Cost per specification is tracked from tagged usage.

## 9. Source notes

This guide is based on public documentation:

- [GitLab Duo Agent Platform](https://docs.gitlab.com/user/duo_agent_platform/)
- [GitLab Duo: Agents](https://docs.gitlab.com/user/duo_agent_platform/agents/)
- [GitLab Duo: Custom agents](https://docs.gitlab.com/user/duo_agent_platform/agents/custom/)
- [GitLab Duo: AGENTS.md customization files](https://docs.gitlab.com/user/duo_agent_platform/customize/agents_md/)
- [GitLab Duo: Customize](https://docs.gitlab.com/user/duo_agent_platform/customize/)
- [GitLab Duo Agent Platform general availability](https://ir.gitlab.com/news/news-details/2026/GitLab-Announces-the-General-Availability-of-GitLab-Duo-Agent-Platform/default.aspx)

GitLab product capabilities are evolving quickly. Revisit this guide as the platform matures.
