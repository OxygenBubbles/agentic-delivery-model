# GitHub: running the Agentic Delivery Model

> Checked against vendor documentation on 22 October 2026. Products named here change often, so check availability, names and settings against the vendor's current documentation before relying on them. This note describes one way to run the model on this toolchain and is not an endorsement.

This note covers github.com and GitHub Enterprise Cloud. Issues and Projects hold the work items, and rulesets and code owners govern the repositories. GitHub Actions runs the pipeline, deploys through environments and produces artifact attestations. GitHub Advanced Security, sold as GitHub Code Security and GitHub Secret Protection, supplies the security checks. The agents are Copilot cloud agent (formerly Copilot coding agent), custom agents, Copilot code review and Copilot CLI. GitHub Agentic Workflows and the Claude and Codex coding agents run in public preview. Agent HQ, announced in October 2025, now appears as the agents page, the third-party coding agents and AI Controls.

GitHub covers steps 3 to 7 well and records much of the decision trail without transcription. An adopter adds other tools for discovery before a request, the release manifest and change authorisation at step 9, feature flags and the telemetry read at step 11, Confirm and learn. The [Anthropic](ANTHROPIC-ADM.md) note covers Claude Code in GitHub Actions, the [OpenAI](OPENAI-ADM.md) note covers Codex's own GitHub integration and the [Microsoft](MICROSOFT-ADM.md) note covers Azure. The model is set out in [MODEL.md](../MODEL.md).

## 1. Where the model's elements live

| Model element | Where it lives on GitHub |
|---|---|
| The request (2.2) | An issue from a request form (public preview), with issue fields for the business owner and the decision. |
| The intent (2.1 and appendix A) and its published version | An intent issue with one form section per element holds the working copy. The published version is a file in the leading context's repository, merged with code owner approval. |
| The work item | The issue, with an issue type, sub-issues for execution units, "blocked by" dependencies for their order and Projects views for the queues. |
| Repository instructions | `.github/copilot-instructions.md` or `AGENTS.md`, or one `CLAUDE.md` or `GEMINI.md` at the root. |
| Path-specific instructions | `.github/instructions/NAME.instructions.md` files with an `applyTo` pattern, and nested `AGENTS.md` files, the nearest taking precedence. |
| Standards as retrievable context | The standards repository, read through the GitHub Model Context Protocol (MCP) server with a read-only token that reaches it. Skills in `.github/skills/` carry procedures. |
| The agent profile (appendix C) | A custom agent profile (public preview) in `.github/agents/` or at organisation or enterprise level, the last in a designated `.github-private` repository, with the other appendix C fields in `metadata`. |
| The context profile (appendix B) | No GitHub object. A versioned file in the platform repository names the workflows, environments and setup file for each step. |
| The tool allow-list | The profile's `tools` and `mcp-servers`, the repository MCP configuration and the firewall allowlists, with enterprise managed settings for the CLI and integrated development environments (IDEs). |
| The run identity | The cloud agent acts as Copilot under a session ID, Claude and Codex act through their own GitHub Apps and Actions jobs use a short-lived `GITHUB_TOKEN`. |
| The workspace (step 3) | A `copilot/` branch and an ephemeral Actions environment built from `.github/workflows/copilot-setup-steps.yml`. |
| The pipeline and deterministic checks (step 5) | Actions workflows required by organisation rulesets, with status checks from an expected source and code scanning and code quality results. |
| The release candidate and its attestation (step 7) | An artefact built once from the merged commit and attested with `actions/attest`, with a software bill of materials (SBOM). |
| Environments and promotion (step 8) | Actions environments with deployment branch rules, environment secrets, OpenID Connect (OIDC) federation and deployment history. |
| The authorisation record (step 9) | The required reviewer's approval of the production deployment, with Prevent self-review set, and a custom deployment protection rule (public preview) confirming the change authorisation. |
| The release manifest | No GitHub object. A pipeline job assembles it from the runs, attestations, pull requests and change record. |
| The decision trail (8.1) | Issues, pull requests, commits linked to session logs, workflow runs, attestations, deployment history and the streamed audit log. |
| Telemetry for step 11 | The adopter's observability platform, with deployment events and Copilot usage metrics from GitHub. |

## 2. Readiness against the eleven steps

The "Supports up to" column judges, from GitHub's documented capabilities, the highest level an agent can hold at each step with the nine controls of 5.3 in place. The starting levels in 3.1 apply until an evaluation under 5.4 supports more.

| Step | What GitHub provides | Supports up to | Gap or complement |
|---|---|---|---|
| 1 Intent | Issue forms, types, fields and sub-issues hold the intent. An agentic workflow drafts it, and a safe output, a write performed by a separate job, posts the draft. | L2 | Validation records and publication are adopter conventions built from fields, code owners and a workflow. |
| 2 Gate | Labels, issue fields and Projects views mark the triggers, and a read-only agentic workflow can comment on those met. | L1 | GitHub documents no gate workflow, so decisions sit on the issue or in the decision index. |
| 3 Workspace | The `copilot/` branch and a fresh virtual machine per session on GitHub-hosted runners, set up by `copilot-setup-steps.yml`. | L0 by design | Masked data, schemas and tenants come from the adopter's pipeline jobs. |
| 4 Build | The cloud agent and custom agents work on one branch under hooks, the firewall and rulesets, and the Claude and Codex agents carry the same documented protections. | L2 | Repository custom agents override organisation agents of the same name, so agent paths need ruleset protection. |
| 5 Verify | Required status checks, code scanning, secret scanning, dependency review and code quality, with the agent scanning its own output first. | L3 | Runs on an agent's pull request wait for a person with write access unless an administrator allows automatic runs. |
| 6 Review | The requester cannot approve, and an unattributed Copilot pull request needs an extra approval (public preview). | L1 | Copilot approvals (public preview) stay off. Copilot code review's models cannot be chosen, so it and the cloud agent's own review pass stay off and a reviewer on the evaluated model comments. |
| 7 Identify | A workflow on the merged commit builds once and attests, reaching Supply-chain Levels for Software Artifacts (SLSA) v1.0 Build Level 3 through a reusable workflow. | L0 by design | The provenance names the workflow, commit and trigger but no checks or approvals, so a second attestation binds that evidence. |
| 8 Promote | Environments with deployment branch rules, environment secrets, OIDC federation and deployment history. | L3 | The `dispatch-workflow` safe output of Agentic Workflows starts an approved non-production workflow, and the adopter configures the permitted workflows, refs, candidate binding and environment checks. |
| 9 Authorise | Required reviewers with Prevent self-review, administrator bypass disabled and custom deployment protection rules (public preview). | L3 | The manifest and change authorisation come from the adopter's pipeline and service management system. Prevent self-review covers only whoever started the run, so a release check confirms the approver did not request, build or implement the change. |
| 10 Deploy | The approved job deploys with OIDC credentials after `gh attestation verify` checks the signer workflow, source commit and predicate, or a Kubernetes admission controller checks the attestation. | L0 by design | A deployment engine outside Actions verifies the attestation itself. |
| 11 Confirm and learn | Deployment history, issue closure as the acceptance record and Copilot usage metrics, with the cloud agent able to draft a post-incident review. | L2 | Health checks, telemetry and the outcome reading come from an observability platform. |

GitHub is strongest from step 3 to step 7, where branch isolation, segregated approval, rulesets and attestation enforce the controls with little adopter code. It is weakest at either end, where the request decision and the gates rely on adopter conventions and the manifest and step 11 sit outside it.

## 3. Controls and trust boundaries

| Control | How GitHub supports it | Gap |
|---|---|---|
| 1. Never authorises | Coding agents cannot approve or merge, the requester cannot approve and environment reviewers are people. | Copilot approvals (public preview) stay off, and no agentic workflow holds a safe output that submits a review, merges or approves a workflow run. |
| 2. No self-attestation | Status checks come from an expected source, runs on agent pull requests need a person's approval by default and the workflow writes the attestation. | Workflow files sit in the repository, so `.github/workflows/` needs code owner review. |
| 3. Only the published promotion path | Environments restrict the deploying branches, release secrets after approval and can refuse administrator bypass. | A credential held outside GitHub can bypass the environments, so cloud trust policies accept only the production environment's OIDC subject. |
| 4. Logged under a run identity | Session logs, signed commits linked to them and audit events with `actor_is_agent` and `agent_session_id`. | Streaming agent session events is in public preview, for Enterprise Managed Users and data residency enterprises only. |
| 5. A manual route remains | People implement issues, and deployments run through the same environments without agents. | The manual deployment route and its credential are the adopter's procedure. |
| 6. Hosting meets the data position | Per-provider hosting and retention terms, model enablement and bring your own key (BYOK) for the CLI and IDEs. | Code review may use models outside the Models settings. An opt-in policy on Enterprise Cloud with data residency keeps inference for generally available Copilot features in the United States or the European Union. |
| 7. The implementer does not review | Cloud agent runs, custom agents and agentic workflows each name a model or engine. | Code review's models cannot be chosen, so it stays off, and each profile pins the model its evaluated pairing names rather than Auto. |
| 8. Approved sources only | Hidden characters are filtered, comments from users without write access never reach the agent and automations ignore their events by default. | Code review reads instructions from the head branch, and Copilot Memory (public preview) entries are not reviewed files. |
| 9. No data or secrets out | The firewall, agent secrets kept apart from Actions secrets and secret scanning of agent output. | The firewall covers processes started through the agent's Bash tool, not MCP servers, and masking is the adopter's. |

| Boundary (5.5) | What holds it on GitHub |
|---|---|
| 1. The organisation and its supplier services | Enterprise Cloud, optionally with data residency, AI Controls policies and audit log streaming to the organisation's store. |
| 2. The agent supplier and the model providers | Per-provider hosting and retention terms, model enablement and separate policies for the Claude and Codex agents. |
| 3. The run and the systems it reaches | A session token for one branch, a read-only GitHub MCP server, the firewall, the profile's tools and hooks that can deny a tool call. |
| 4. Retrieved content and the agent's instructions | Hidden-character filtering, the write-access filter and code owner review of instruction, agent and hook files. |
| 5. A controlled environment and the repository | Separate agent secrets, secret scanning of agent output and the firewall, with masking from the adopter. |
| 6. Non-production and production | Required reviewers with Prevent self-review, deployment branch rules, OIDC trust bound to production and attestation verification. |

## 4. Configuration pattern

### 4.1 Intents, standards and context

A request is an issue from a request form, with the product owner's decision under 2.2 in an issue field. An approved request becomes an intent issue with one form section per element of 2.1 and sub-issues for the execution units, and the pipeline posts drafts and halt questions to it through safe outputs. Once the validators have recorded their validations, the intent architect sets the publication field and a workflow opens a pull request adding the intent file to the leading context's repository. Code owner approval and the merge hand the intent to step 2 or step 3. Because an issue stays editable, a build reads the published file at its commit and an assigned issue carries only a pointer to it.

Standards are published through pull requests in a standards repository and read at the point of use through the GitHub MCP server, with skills carrying procedures rather than copies of rules. GitHub ranks repository instructions above organisation instructions, so the binding rules sit in checks and rulesets, as 5.2 requires.

### 4.2 Agents, permissions and run identities

Each agent profile is a custom agent file whose `model`, `tools` and `mcp-servers` properties pin the model, the allow-list and the servers, with the step, class, level, evaluation and approval in `metadata`. Enterprise profiles sit in the designated `.github-private` repository, changed only by enterprise owners under the AI Controls ruleset. Because a repository profile overrides an organisation profile of the same name, an organisation ruleset requires platform team approval on agent, instruction and hook paths. Repository MCP settings are applied after a profile's own servers and can override them beyond the reach of file review, so the platform owner administers MCP settings and agent secrets. Each profile's `tools` list admits only allow-listed tools, filtering built-in and MCP tools alike.

| Step and class | Agent on GitHub | Level | How GitHub enforces it |
|---|---|---|---|
| 1 Intent, every class | Agentic workflow on issue events | L2 | Read-only job, draft posted by a safe output |
| 2 Gate, every class | Agentic workflow on issue events | L1 | Read-only job, a comment as the only safe output |
| 4 Build, unit tests and low-tier refactors | Cloud agent with a custom agent profile | L2 | One `copilot/` branch, no approval or merge, firewall and hooks |
| 4 Build, standard tier | Cloud agent, Claude or Codex | L2 | One branch, no approval or merge, code owner review on every path |
| 5 Verify, failing-check triage | Cloud agent mentioned on the pull request | L2 | Pushes only to that branch, reruns wait for approval |
| 6 Review, every class | Agentic workflow on the evaluated reviewing model | L1 | Comment safe outputs only, with no review submission or merge |
| 8 Promote, non-production | Agentic workflow with the `dispatch-workflow` safe output | L3 | Listed workflows and refs only, environment rules decide |
| 9 Authorise, readiness | Agentic workflow assembling the manifest | L3 | Read-only job, result attached by the pipeline |
| 11 Confirm and learn | Cloud agent drafting the post-incident review | L2 | A pull request a person merges |

Retrieval uses the GitHub MCP server's read-only token and the `read` and `search` tools. Commenting is a comment or review comment safe output. Execution, granted only at L2 and above, is the cloud agent's push to its own branch or a pull request or workflow dispatch made through a safe output. Copilot CLI in Actions is invoked with `-p` and named `--allow-tool` and `--deny-tool` rules.

GitHub documents no identity issued per run. The cloud agent acts as Copilot under a session token limited to one branch, and `agent_session_id` and the session link in each commit tie actions to one run. Agentic workflows and Copilot CLI run under the job's short-lived `GITHUB_TOKEN`, the nearest equivalent to a run identity. The agent tasks application programming interface, in public preview, accepts only user tokens, so pipeline-started cloud agent tasks run on a person's credential.

An increase is a reviewed profile change released with the platform under 5.4. A reduction is immediate: the platform owner blocks new starts through the agent policies, AI Controls toggles and Actions settings, stops the affected sessions, cancels their workflow runs and revokes their credentials, then corrects the profile. A stopped session ends its Actions run and keeps its pushed commits and log, while a profile edit alone leaves an existing pull request on its original version. Automations are not stored in Git, so agentic workflows, held as reviewed files, suit agent profiles better.

### 4.3 From verification to deployment

Step 5 runs the Actions workflows that organisation rulesets require, and each required status check names its expected source. At step 6 a code owner who neither formed the intent nor produced the change approves and merges, with stale approvals dismissed and approval of the most recent push required. At step 7 the merge workflow runs the required checks again on the exact merged commit, then builds the candidate once and attests its provenance. A second `actions/attest` attestation with a custom predicate binds the candidate digest to the check results, approvals, standards versions, published intent, profile version and session ID. Step 8 deploys it to each non-production environment, with environment values supplied at deployment and OIDC for cloud access. At step 9 the pipeline confirms the change authorisation is current before assembling the manifest, and a custom deployment protection rule checks it again. The release authority, who did not request, build or implement the change, then approves the production job on the exact candidates and recovery positions the manifest names, in a record separate from the change authorisation. Step 10 runs `gh attestation verify` with `--signer-workflow`, `--source-digest` and `--predicate-type`, so only the released workflow's attestations for the authorised commit and candidate pass, and it creates no further approval.

An artefact-based change follows this path unchanged, and its rollback redeploys the previous attested candidate. A state-based change packages the declared-state revision as the candidate, and the job plans, applies and checks drift against it. A migration-based change ships the ordered migration set, with its dry run in pre-production and restore evidence in the manifest. Where a context promotes source, the checks run again on the release commit and the authorisation names it.

### 4.4 One change through the flow

1. **Intent.** An agentic workflow drafts the intent, the validators record their validations and the code owners approve the published file.
2. **Gate.** The screening recorded at step 1 meets no gate trigger.
3. **Workspace.** Assigning the unit's sub-issue to Copilot opens a `copilot/` branch and its environment.
4. **Build.** The cloud agent implements from the published file and opens a draft pull request.
5. **Verify.** A person approves the workflow runs and the required checks pass.
6. **Review.** An agentic workflow on the evaluated reviewing model comments, then a code owner who neither formed the intent nor produced the change approves and merges.
7. **Identify.** The merge workflow re-checks the merged commit, then builds and attests the candidate once.
8. **Promote.** The candidate deploys to test and pre-production, where regression and end-to-end tests run.
9. **Authorise.** The pipeline confirms the change authorisation before assembling the manifest, and a release authority who did not request, build or implement the change approves.
10. **Deploy.** The job verifies the attestations against the manifest and deploys under OIDC credentials.
11. **Confirm and learn.** The product owner accepts against the acceptance examples and schedules the outcome reading.

## 5. Gaps and complementary tooling

Discovery with business people happens before a request issue is raised. The service management system supplies the change authorisation, a flag service or the application supplies feature flags and an observability platform supplies health checks, telemetry and outcome readings.

Within GitHub, the evaluation under 5.4 has no equivalent, since the custom agent test step checks a release rather than measuring output against historical changes. Repository custom agents, MCP settings and instructions can override organisation settings, so rulesets, code owners and control of repository administration carry the authority the model places there. No single documented switch halts every agent surface, so the halt control applies the reduction in 4.2 to every surface at once. Cost per intent needs tagging, because agent sessions are metered in AI credits and Actions minutes rather than per work item.

## 6. Decisions for adopters

1. The agent surfaces each organisation enables, set through the cloud agent policy states, custom properties and the Claude, Codex, agent app and automation policies.
2. The default policy for new features, in effect from 22 October 2026, so that a newly available feature reaches users only by decision.
3. The permitted model list for each surface, and the implementing and reviewing models each profile pins.
4. Copilot approvals kept off, and the automation level for changes an automation makes to issues.
5. Where intents live: the working copy's form fields, the published file's path and the repository that holds each version.
6. Whether workflow runs on agent pull requests wait for a person's approval or run automatically.
7. Whether agent-initiated pipeline jobs are allowed, and the workflows and refs the `dispatch-workflow` safe output may name.
8. The runners agents use, the firewall and allowlist settings, the MCP servers each profile may use and who administers repository MCP settings, with managed settings for the CLI and IDEs.
9. Whether Copilot Memory is enabled, given that its entries are not reviewed files.
10. The destination and retention period of the audit log and agent session events.
11. The plan, since required reviewers and attestations in private repositories need GitHub Enterprise Cloud.

## Sources

- [About GitHub Copilot cloud agent](https://docs.github.com/en/copilot/concepts/agents/cloud-agent/about-cloud-agent)
- [Risks and mitigations for GitHub Copilot cloud agent](https://docs.github.com/en/copilot/concepts/agents/cloud-agent/risks-and-mitigations)
- [Building guardrails for GitHub Copilot cloud agent](https://docs.github.com/en/copilot/tutorials/cloud-agent/build-guardrails)
- [Configure the development environment](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment)
- [Configuring settings for GitHub Copilot cloud agent](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/cloud-agent/configuring-agent-settings)
- [Managing agent sessions](https://docs.github.com/en/copilot/how-tos/copilot-on-github/use-copilot-agents/manage-and-track-agents)
- [Research, plan, and code with Copilot cloud agent](https://github.blog/changelog/2026-03-31-research-plan-and-code-with-copilot-cloud-agent)
- [Third-party coding agents](https://docs.github.com/en/copilot/concepts/agent-capabilities/about-agents/about-third-party-coding-agents)
- [Agent management for enterprises](https://docs.github.com/en/copilot/concepts/enterprise/agent-management)
- [Blocking agentic features in your enterprise](https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/block-agentic-features)
- [Default Enablement of Copilot features for Copilot Business and Enterprise](https://github.blog/changelog/2026-09-24-default-enablement-of-copilot-features-for-copilot-business-and-enterprise)
- [Audit log events for agents](https://docs.github.com/en/copilot/reference/enterprise-administrators/agentic-audit-log-events)
- [Adding repository custom instructions for GitHub Copilot](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/add-custom-instructions/add-repository-instructions)
- [Custom agents configuration](https://docs.github.com/en/copilot/reference/custom-agents-configuration)
- [Preparing to use custom agents in your enterprise](https://docs.github.com/en/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents)
- [About hooks for GitHub Copilot](https://docs.github.com/en/copilot/concepts/agents/hooks)
- [Model Context Protocol (MCP) and GitHub Copilot cloud agent](https://docs.github.com/en/copilot/concepts/agents/cloud-agent/mcp-and-cloud-agent)
- [MCP server usage in your company](https://docs.github.com/en/copilot/concepts/enterprise/mcp-management)
- [Customizing or disabling the firewall for GitHub Copilot](https://docs.github.com/en/copilot/how-tos/copilot-on-github/customize-copilot/customize-the-firewall)
- [Copilot automations](https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/about-automations)
- [GitHub Agentic Workflows](https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/about-github-agentic-workflows)
- [Security Architecture](https://github.github.com/gh-aw/introduction/architecture/), GitHub Agentic Workflows documentation
- [Safe Outputs](https://github.github.com/gh-aw/reference/safe-outputs/), GitHub Agentic Workflows documentation
- [Copilot CLI in GitHub Actions](https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/copilot-cli-in-github-actions)
- [About GitHub Copilot CLI](https://docs.github.com/en/copilot/concepts/agents/copilot-cli/about-copilot-cli)
- [Using Copilot cloud agent via the API](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/cloud-agent/use-cloud-agent-via-the-api)
- [GitHub Copilot code review](https://docs.github.com/en/copilot/concepts/agent-capabilities/agent-powered-workflows-and-operations/code-review)
- [Changing the AI model for GitHub Copilot cloud agent](https://docs.github.com/en/copilot/how-tos/use-copilot-agents/cloud-agent/changing-the-ai-model)
- [Hosting of models for GitHub Copilot](https://docs.github.com/en/copilot/reference/ai-models/model-hosting)
- [GitHub Copilot with data residency](https://docs.github.com/en/enterprise-cloud%40latest/admin/data-residency/github-copilot-with-data-residency)
- [Bring your own key for GitHub Copilot](https://docs.github.com/en/copilot/concepts/models/bring-your-own-key)
- [About GitHub Copilot Memory](https://docs.github.com/en/copilot/concepts/agents/copilot-memory)
- [Available rules for rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/available-rules-for-rulesets)
- [Deployments and environments](https://docs.github.com/en/actions/reference/workflows-and-actions/deployments-and-environments)
- [Managing environments for deployment](https://docs.github.com/en/actions/how-tos/deploy/configure-and-manage-deployments/manage-environments)
- [Artifact attestations](https://docs.github.com/en/actions/concepts/security/artifact-attestations)
- [Using artifact attestations to establish provenance for builds](https://docs.github.com/en/actions/how-tos/secure-your-work/use-artifact-attestations/use-artifact-attestations)
- [actions/attest](https://github.com/actions/attest)
- [gh attestation verify](https://cli.github.com/manual/gh_attestation_verify), GitHub CLI manual
- [Immutable subject claims for GitHub Actions OIDC tokens](https://github.blog/changelog/2026-04-23-immutable-subject-claims-for-github-actions-oidc-tokens)
- [About GitHub Advanced Security](https://docs.github.com/en/get-started/learning-about-github/about-github-advanced-security)
- [Syntax for issue forms](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/syntax-for-issue-forms)
- [Adding sub-issues](https://docs.github.com/en/issues/tracking-your-work-with-issues/using-issues/adding-sub-issues)
