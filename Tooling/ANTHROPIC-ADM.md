# Anthropic: running the Agentic Delivery Model

> Checked against vendor documentation on 22 October 2026. Products named here change often, so check availability, names and settings against the vendor's current documentation before relying on them. This note describes one way to run the model on this toolchain and is not an endorsement.

This note covers Anthropic's agent toolchain: Claude Code in the terminal, integrated development environment (IDE) extensions, the desktop app and cloud sessions, with the Claude Agent SDK running the same agent loop inside an adopter's application. It also covers Claude Code in GitHub Actions and GitLab continuous integration and continuous delivery (CI/CD) pipelines, and Team and Enterprise administration. Claude models are available through the Claude application programming interface (API), Claude Platform on AWS, Amazon Bedrock, Google Cloud's Agent Platform (formerly Vertex AI) and Microsoft Foundry. The current models are Claude Fable 5.1, Claude Opus 5.5, Claude Sonnet 5.5 and Claude Haiku 5.5.

The toolchain covers the agent's part well: drafting the intent, building, triaging checks, reviewing before a person does and correlating telemetry. It holds no work item, pipeline, attestation, environment or deployment engine. The adopter's engineering platform carries those, as the [GitHub](GITHUB-ADM.md), [GitLab](GITLAB-ADM.md), [Microsoft](MICROSOFT-ADM.md), [AWS](AWS-ADM.md) and [Atlassian](ATLASSIAN-ADM.md) notes describe. This note covers Claude Code running inside GitHub, and the [GitHub](GITHUB-ADM.md) note covers Claude as a model behind GitHub Copilot. Terms and step numbers follow [MODEL.md](../MODEL.md).

## 1. Where the model's elements live

| Model element | Where it lives on Anthropic |
|---|---|
| The request (2.2) | The work management system. An agent supplies evidence at L1 through a read-only Model Context Protocol (MCP) server |
| The intent (2.1 and appendix A) and its published version | The working copy on the work item. The published version is a Markdown file merged into the leading context's repository by pull request |
| The work item | The work management system, read through an MCP server. The pipeline posts the run's drafts and questions |
| Repository instructions | `CLAUDE.md`, or `AGENTS.md` where no `CLAUDE.md` exists, with an organisation-wide `CLAUDE.md` at the managed policy path |
| Path-specific instructions | `.claude/rules/*.md` files with a `paths` frontmatter list, and nested `CLAUDE.md` files |
| Standards as retrievable context | Skills (`.claude/skills/<name>/SKILL.md`) in a plugin from the organisation's marketplace, or a read-only MCP server |
| The agent profile (appendix C) | A subagent file in `.claude/agents/` or an Agent SDK definition, beside a versioned record of level, evaluation and approval |
| The context profile (appendix B) | A versioned document in the platform repository |
| The tool allow-list | Managed settings: locked permission rules, the sandbox's managed domains and allow-lists for MCP servers and plugin marketplaces |
| The run identity | The pipeline job's OpenID Connect (OIDC) token, exchanged through workload identity federation for a short-lived credential |
| The workspace (step 3) | A runner, container or git worktree the pipeline allocates, or a self-hosted environment runner (in public beta) |
| The pipeline and deterministic checks (step 5) | The adopter's CI/CD platform, running Claude Code as a job |
| The release candidate and its attestation (step 7) | The CI/CD platform |
| Environments and promotion (step 8) | The deployment engine |
| The authorisation record (step 9) | The service management system and the platform's environment approvals |
| The release manifest | The pipeline that generates it |
| The decision trail (8.1) | Pull requests, commits, OpenTelemetry events, each run's session identifier and transcripts kept through the Agent SDK or the Compliance API |
| Telemetry for step 11 | The observability platform, read through a read-only MCP server |

## 2. Readiness against the eleven steps

The "Supports up to" column is a judgement, from Anthropic's documented capabilities, of the highest level an agent can hold at each step with the nine controls of section 5.3 in place. The model's starting levels are in 3.1, and each takes effect only after evaluation and approval under 5.4.

| Step | What Anthropic provides | Supports up to | Gap or complement |
|---|---|---|---|
| 1 Intent | A headless Agent SDK run drafts from the work item and returns JSON for the pipeline to post | L2 | The publication pull request sits on the platform |
| 2 Gate | A read-only run screens the intent against the gate triggers | L1 | Gate decisions sit outside the toolchain |
| 3 Workspace | Anthropic adds nothing here. The pipeline allocates a runner, container or worktree from a versioned definition | L0 by design | Environment definitions sit on the platform |
| 4 Build | Claude Code in a pipeline job implements on the unit branch under permission rules, the sandbox and hooks | L2 | Branch protection sits on the platform |
| 5 Verify | Hooks run fast checks inside a run, and a run triages failing checks | L3 | L3 needs a job token that cannot re-run, cancel or approve workflow runs |
| 6 Review | Code Review (in research preview) and review workflows comment inline without approving | L1 | A code owner approves. Code Review documents no model choice |
| 7 Identify | Anthropic adds nothing here. The CI/CD platform builds and attests the candidate | L0 by design | The attestation binds the run's session identifier |
| 8 Promote | A run invokes an approved non-production job exposed as one allow-listed tool | L3 | Environment protection sits on the platform |
| 9 Authorise | A run assembles readiness evidence through read-only tools | L3 | The decision sits with the release authority |
| 10 Deploy | Anthropic adds nothing here, and no agent job holds a production credential | L0 by design | The deployment engine deploys |
| 11 Confirm and learn | A run correlates the change with telemetry and drafts the post-incident review | L2 | Acceptance stays at L0 with the product owner |

The toolchain is strongest in central control over the agent: managed settings that no project or user can override, deny-first permission rules, an operating system sandbox and an enforced permitted model list. It is weakest after the merge, where the platform does most of the work. Cloud sessions, routines and pull request auto-fix also act through the user's GitHub connection rather than a run identity.

## 3. Controls and trust boundaries

| Control | How Anthropic supports it | Gap |
|---|---|---|
| 1. Never the sole authoriser | Claude Code holds no approval authority, and Code Review's check run never blocks a merge | Approval rules live on the version control platform |
| 2. Cannot attest to its own work | Deny rules block edits to pipeline definitions, writes to `.claude` and `.git` are not auto-approved and `allowManagedHooksOnly` ignores hooks a run writes | The Claude GitHub App can re-run workflows, so agent jobs use a token without Actions write |
| 3. Only the published promotion path | Claude Code has no deployment function, and agent jobs hold no deployment credential | Environment protection on the platform enforces the path |
| 4. Logged under a run identity | OpenTelemetry events per session record tool decisions, and the Agent SDK `SessionStore` or the Compliance API keeps transcripts | Pipeline runs fall outside the Compliance API |
| 5. Manual route remains | People run the same `claude` command, and administrators can switch agent features off centrally | The manual deployment route is the platform's |
| 6. Hosting meets the data position | Commercial terms exclude training and set 30-day retention, with zero data retention (ZDR) for qualified Enterprise accounts | The Claude API offers US or global inference only. Fable models are unavailable under ZDR unless Anthropic authorises it |
| 7. Implementing model does not review | The model is pinned per subagent with `model` and per pipeline job with `--model` | Every model is a Claude model, and `fallbackModel` can switch models within a turn |
| 8. Approved instruction sources only | `--bare` runs load only what the pipeline passes, and `strictPluginOnlyCustomization` blocks user and project sources | Without `--bare`, a `-p` run loads a project's hooks and MCP servers with no trust prompt |
| 9. No data or secrets out | Managed sandbox domain allow-lists, with `Read` deny rules and credential rules for secret files | The sandbox covers shell commands only and does not inspect encrypted traffic by default. Secret scanning is the pipeline's |

| Trust boundary (5.5) | What holds it on this toolchain |
|---|---|
| 1. The organisation and its supplier services | Commercial terms, a Team or Enterprise organisation with single sign-on, `forceLoginOrgUUID` and ZDR where the organisation qualifies |
| 2. The agent supplier and the model providers | The permitted model list with its approved hosting: `inference_geo` on the Claude API, Bedrock inference profiles, Agent Platform regional endpoints or Foundry hosted on Azure |
| 3. The run and the systems it reaches | A federated identity per job, permission rules, managed MCP allow-lists and the sandbox |
| 4. Retrieved content and the agent's instructions | `--bare` runs with pinned instruction files, the published intent read at a commit and review configuration from the base branch |
| 5. A controlled environment and the repository | Sandbox read denials, credential masking, network allow-lists and the pipeline's secret scanning |
| 6. Non-production and production | Environment protection on the platform and the release authority at step 9 |

## 4. Configuration pattern

### 4.1 Intents, standards and context

A drafting run reads the work item through an MCP server with only its read tools allowed. On the intent architect's decision, the pipeline commits the intent as a Markdown file to the leading context's repository through a pull request the code owners approve. A build run receives that file at the merge commit with no work item server, so an edit on the work item cannot redirect it.

Anthropic documents `CLAUDE.md` and its rules files as context rather than enforcement, so a rule the pipeline must enforce also appears as a permission rule, a hook or a pipeline check. Standards ship as skills in a plugin from the organisation's marketplace at a fixed git ref, enabled through managed `enabledPlugins`. Managed settings also limit plugin sources with `strictKnownMarketplaces` and block user and project customisation with `strictPluginOnlyCustomization`.

### 4.2 Agents, permissions and run identities

Headless runs start with `--bare`, as Anthropic recommends for scripted and SDK calls, to skip the repository's hooks, skills, subagents, plugins, MCP servers and `CLAUDE.md`. The pipeline passes the profile's configuration by flag, and GitHub Actions runs receive it through the action's `settings` input.

| Agent profile (step and class) | Level | How the toolchain enforces it |
|---|---|---|
| Intent drafter (step 1, every class) | L2 | Agent SDK run in `dontAsk` mode with read-only work item tools, returning a JSON draft |
| Gate screener (step 2, every class) | L1 | `--tools` limited to read-only built-in tools, with findings posted by the pipeline |
| Implementer (step 4, standard-tier application change) | L2 | Pipeline job on the unit branch in `dontAsk` mode with an allow-list, the required sandbox and `Edit` deny rules on pipeline definitions |
| Check triager (step 5, every class) | L2 | Read access to check output and one comment tool |
| First reviewer (step 6, every class) | L1 | Separate job on a different pinned model, with read tools and the inline comment tool `mcp__github_inline_comment__create_inline_comment` |
| Readiness assembler (step 9, every release) | L3 | Pipeline job with read-only servers for the records, writing the manifest draft |
| Telemetry correlator (step 11, every class) | L2 | Read-only observability server, with the draft review posted by the pipeline |

Retrieval, commenting and execution are authorised separately. Retrieval is the built-in read tools plus named read tools on each MCP server, and `dontAsk` denies every tool left unlisted. Commenting is a single named comment tool. Execution is Edit, Write and Bash, for L2 and L3 profiles only and inside the sandbox. Auto mode uses a classifier rather than a deterministic check, so a profile adopting it covers the classifier in its evaluation. Dynamic workflows run subagents from a script under the same rules, and `disableWorkflows` turns them off. Interactive sessions are a person's tool, and their output enters the flow as that person's pull request.

The organisation-level ceiling is a managed settings file in the runner image and a mobile device management (MDM) profile on developer machines. It sets `allowManagedPermissionRulesOnly`, `allowManagedHooksOnly`, `permissions.disableBypassPermissionsMode`, `availableModels` with `enforceAvailableModels` and the sandbox keys. Anthropic describes server-managed settings as a client-side control rather than a security boundary.

Each pipeline job authenticates through workload identity federation, exchanging its OIDC token for a short-lived token bound to a Claude Console service account. A federation rule per agent profile attributes each run to its profile, and revoking the rule enacts a reduction under 5.4. GitHub operations use a repository-scoped token from a custom GitHub App without Actions write.

### 4.3 From verification to deployment

Steps 5 to 10 run on the adopter's CI/CD platform and deployment engine, where the pipeline runs the acceptance examples, contract tests and security checks. Hooks can run fast checks inside a run, but a timed-out `PreToolUse` hook does not block a call, so they do not replace the pipeline.

At step 6 a review job on a different pinned model comments inline, or Code Review posts findings with a neutral check run. Review runs take their configuration from the base branch, as the action's security guidance recommends for `pull_request_target` events. A code owner who did not produce the change approves and merges. At step 7 the pipeline attests the release candidate with the run's session identifier, agent profile version and federated identity. The deployment engine owns steps 8 and 10, and the release authority decides at step 9.

For the deployment approaches of section 4, the implementer drafts code and tests, declared configuration or ordered migrations at L2. The pipeline validates the plan or runs the dry run, and it exercises the intent's recovery position below production.

### 4.4 One change through the flow

The example is a standard-tier change adding a filter to an existing search screen.

1. **Intent.** The product owner approves the request, a drafting run drafts the intent and the intent architect publishes it after validation.
2. **Gate.** A screening run finds no trigger met, and the intent architect records the screening.
3. **Workspace.** The publication merge triggers the pipeline to create the unit branch and an ephemeral runner.
4. **Build.** A job runs `anthropics/claude-code-action@v1` with the implementer's settings and pinned model, and the pipeline opens the pull request with the work item key.
5. **Verify.** The push triggers the pipeline, and a failing check starts a triage run that comments with a likely cause.
6. **Review.** A review job on a different Claude model comments, and a code owner who did not produce the change approves and merges.
7. **Identify.** The pipeline builds and attests the candidate with the session identifier and profile version.
8. **Promote.** The deployment engine promotes the candidate through test and pre-production.
9. **Authorise.** A readiness run assembles the manifest draft, and the release authority authorises the release.
10. **Deploy.** The deployment engine deploys the authorised candidate with no agent involved.
11. **Confirm and learn.** Health checks pass, a correlation run reads the telemetry and the product owner accepts the change.

## 5. Gaps and complementary tooling

| Gap | Complement |
|---|---|
| No work item system, pipeline, attestation, environments or deployment engine | The engineering platform, as the [GitHub](GITHUB-ADM.md), [GitLab](GITLAB-ADM.md) and [Microsoft](MICROSOFT-ADM.md) notes describe. GitLab calls a pull request a merge request, and its Claude Code integration is in beta |
| Cloud sessions, routines (in research preview) and auto-fix act as the user who started them | Pipeline jobs with federated identities, or self-hosted environments (in public beta) whose signed session tokens internal services verify |
| The Compliance API covers only Claude Enterprise sign-ins | OpenTelemetry export and an Agent SDK `SessionStore` into the organisation's own store |
| Every model is a Claude model | A reviewing model from another provider, such as those in the [OpenAI](OPENAI-ADM.md) and [Google](GOOGLE-ADM.md) notes, where the evaluation shows a Claude pairing sharing errors |
| The Claude API offers US or global inference only | Bedrock inference profiles, Agent Platform regional endpoints or Foundry hosted on Azure, without cloud sessions, Code Review or server-managed settings |
| The sandbox covers shell commands only and does not run on native Windows | A container or virtual machine around the whole run, with a credential-injecting proxy as its only egress |

## 6. Decisions for adopters

1. The provider and plan, weighing the data position against the features each offers.
2. The permitted model list, enforced by `availableModels` and `enforceAvailableModels`, and whether Fable models are permitted given their retention requirement.
3. The reviewing model paired with each implementing model, and whether evaluated profiles may set `fallbackModel`.
4. Where published intents live and how a build run receives the published version.
5. How managed settings reach runners and developer machines, and the locking keys that set the ceiling.
6. The permission mode per profile: `dontAsk` with an allow-list, or auto mode with its classifier covered by the evaluation.
7. Whether agent jobs use the Claude GitHub App or a custom app without Actions write, and whether agents may start pipeline jobs.
8. Where headless runs execute: pipeline runners, self-hosted environments or Claude Managed Agents (in beta and not eligible for ZDR).
9. Whether cloud sessions, routines, auto-fix, Code Review, dynamic workflows and agent teams (experimental) are enabled.
10. Where session records go, for how long and whether telemetry captures prompts and tool details.
11. The sandbox policy, including `failIfUnavailable`, the managed domain list and the credential rules.
12. The cost circuit breakers under 5.4, such as `--max-budget-usd` and `--max-turns` on each run.

## Sources

- [Claude Code overview](https://code.claude.com/docs/en/overview.md)
- [Models overview](https://platform.claude.com/docs/en/models/overview)
- [Claude Platform release notes](https://platform.claude.com/docs/en/release-notes/overview)
- [How Claude remembers your project](https://code.claude.com/docs/en/memory.md)
- [Create custom subagents](https://code.claude.com/docs/en/sub-agents.md)
- [Orchestrate subagents at scale with dynamic workflows](https://code.claude.com/docs/en/workflows.md)
- [Hooks reference](https://code.claude.com/docs/en/hooks.md)
- [Extend Claude with skills](https://code.claude.com/docs/en/skills.md)
- [Manage Claude Code plugins](https://code.claude.com/docs/en/plugins/org.md)
- [Choose a permission mode](https://code.claude.com/docs/en/permission-modes.md)
- [Configure permissions](https://code.claude.com/docs/en/permissions.md)
- [Configure the sandboxed Bash tool](https://code.claude.com/docs/en/sandboxing.md)
- [Deploy managed settings](https://code.claude.com/docs/en/managed-settings.md)
- [Configure server-managed settings](https://code.claude.com/docs/en/server-managed-settings.md)
- [Model configuration](https://code.claude.com/docs/en/model-config.md)
- [Run Claude Code programmatically](https://code.claude.com/docs/en/headless.md)
- [CLI reference](https://code.claude.com/docs/en/cli-reference.md)
- [Claude Code GitHub Actions](https://code.claude.com/docs/en/github-actions.md)
- [Claude Code Action security](https://github.com/anthropics/claude-code-action/blob/main/docs/security.md)
- [Claude Code GitLab CI/CD](https://code.claude.com/docs/en/gitlab-ci-cd.md)
- [Code Review](https://code.claude.com/docs/en/code-review.md)
- [Use Claude Code in the cloud](https://code.claude.com/docs/en/claude-code-on-the-web.md)
- [Self-hosted environments](https://code.claude.com/docs/en/self-hosted-environments.md)
- [Verify session identity in self-hosted environments](https://code.claude.com/docs/en/self-hosted-environments-identity.md)
- [Automate work with routines](https://code.claude.com/docs/en/routines.md)
- [Workload identity federation](https://platform.claude.com/docs/en/manage-claude/workload-identity-federation)
- [Agent SDK overview](https://code.claude.com/docs/en/agent-sdk/overview.md)
- [Persist sessions to external storage](https://code.claude.com/docs/en/agent-sdk/session-storage.md)
- [Securely deploying AI agents](https://code.claude.com/docs/en/agent-sdk/secure-deployment.md)
- [Claude Managed Agents overview](https://platform.claude.com/docs/en/managed-agents/overview)
- [Feature availability](https://code.claude.com/docs/en/feature-availability.md)
- [Claude in Microsoft Foundry](https://platform.claude.com/docs/en/build-with-claude/claude-in-microsoft-foundry)
- [Data residency](https://platform.claude.com/docs/en/manage-claude/data-residency)
- [Data usage](https://code.claude.com/docs/en/data-usage.md)
- [Zero data retention](https://code.claude.com/docs/en/zero-data-retention.md)
- [Monitoring](https://code.claude.com/docs/en/monitoring-usage.md)
- [Retrieve session transcripts](https://platform.claude.com/docs/en/manage-claude/compliance-sessions)
