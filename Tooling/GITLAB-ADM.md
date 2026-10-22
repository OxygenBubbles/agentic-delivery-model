# GitLab: running the Agentic Delivery Model

> Checked against vendor documentation on 22 October 2026. Products named here change often, so check availability, names and settings against the vendor's current documentation before relying on them. This note describes one way to run the model on this toolchain and is not an endorsement.

This note covers GitLab.com, GitLab Self-Managed and GitLab Dedicated with the GitLab Duo Agent Platform. It covers the platform's agents, flows and AI Catalog, the Code Review Flow, model selection, GitLab Duo Self-Hosted and the Model Context Protocol (MCP) client and server. It also covers the work items, continuous integration and continuous delivery (CI/CD) pipelines, environments, policies and audit events the flow in [MODEL.md](../MODEL.md) relies on. GitLab calls a pull request a merge request, and this note uses the model's term except in GitLab feature names.

GitLab covers step 4, Build, to step 10, Deploy, in one permission model and one audit trail. An adopter builds three of the model's records: the published intent, an attestation that binds approvals to a release candidate and the release manifest. The [Atlassian](ATLASSIAN-ADM.md) note applies where Jira holds the work item, and the [AWS](AWS-ADM.md) note covers infrastructure on AWS. The [Anthropic](ANTHROPIC-ADM.md) and [OpenAI](OPENAI-ADM.md) notes cover Claude Code and Codex, both available in GitLab as external agents.

## 1. Where the model's elements live

| Model element | Where it lives on GitLab |
|---|---|
| The request (2.2) | A work item in an intake project, with custom fields for the request record |
| The intent (2.1 and appendix A) and its published version | A work item of a configured intent type holds the working copy. The published version is a Markdown file in the leading context's repository |
| The work item | A GitLab work item (an issue, a task or a configured type), grouped under epics |
| Repository instructions | `AGENTS.md` and `.gitlab/duo/chat-rules.md`, with `.gitlab/duo/mr-review-instructions.yaml` for the Code Review Flow |
| Path-specific instructions | `AGENTS.md` in a subdirectory, honoured only in editor extensions and the GitLab Duo command-line interface (CLI) |
| Standards as retrievable context | Project skills in `skills/<skill-name>/SKILL.md` and a standards repository the run identity can read |
| The agent profile (appendix C) | No native record. A versioned file in the platform repository, naming the AI Catalog item version |
| The context profile (appendix B) | No native record. A versioned file in the platform repository |
| The tool allow-list | Agent tool governance (in beta), MCP server blocking and the remote sandbox's domain allowlist |
| The run identity | Composite identity: the flow's service account combined with the person who started the run |
| The workspace (step 3) | A branch per execution unit, worked in an ephemeral workload pipeline set up from `.gitlab/duo/agent-config.yml` |
| The pipeline and deterministic checks (step 5) | GitLab CI/CD, with jobs from pipeline execution policies and scans from scan execution policies |
| The release candidate and its attestation (step 7) | A package or container image built once from the merged commit, with runner provenance in Supply-chain Levels for Software Artifacts (SLSA) format signed by a pipeline job |
| Environments and promotion (step 8) | Environments with deployment tiers, and deployment jobs that deploy the same artefact |
| The authorisation record (step 9) | Deployment approvals on the protected production environment, beside the change record in the service management tool |
| The release manifest | No native object. A pipeline job assembles it as a job artefact |
| The decision trail (8.1) | Linked work item, pull request, pipeline and deployment records, audit events and AI audit events (in beta) |
| Telemetry for step 11 | GitLab Observability (in beta on GitLab.com and GitLab Self-Managed), incident management and alerts |

## 2. Readiness against the eleven steps

The "Supports up to" column is a judgement, from GitLab's documented capabilities, of the highest level an agent can hold at that step with the nine controls of 5.3 in place, and it grants no level. The model's starting levels, from L0, Not used, to L3, Execute under gate, are in 3.1 and take effect only after the evaluation under 5.4.

| Step | What GitLab provides | Supports up to | Gap or complement |
|---|---|---|---|
| 1 Intent | The Planner agent edits work items, and custom fields, statuses and description templates structure the intent | L2 | No publication step, so a pipeline job publishes the validated intent |
| 2 Gate | Linked work items and compliance framework labels, with an agent assembling material in a comment | L1 | Gate owners record decisions on the work item |
| 3 Workspace | Branches, workload pipelines and dynamic environments created by the pipeline | L0 by design | Masked data refresh is the adopter's pipeline job |
| 4 Build | The Developer Flow, custom flows and external agents on runners, started from an issue, a mention, an event or the Flows API | L2 | Flows read work item comments, so instructions confine the build to the published intent, and Flows API runs need composite identity enforced |
| 5 Verify | Pipeline execution, scan execution and merge request approval policies, and the Fix CI/CD Pipeline Flow proposing fixes | L3 | Flows can call the pipeline and job application programming interfaces (APIs), so profiles are denied tools they do not need |
| 6 Review | The Code Review Flow comments under its own model setting, with its approval option, in beta, left off | L1 | It reads neither `AGENTS.md` nor skills |
| 7 Identify | A pipeline on the merged commit builds once, and the runner writes provenance for a job to sign | L0 by design | Binding approvals into the attestation is the adopter's job |
| 8 Promote | Deployment jobs and protected environments, with the pipeline and job APIs available to flows | L3 | Protected environments keep runs out of the higher tiers |
| 9 Authorise | Deployment approvals with named approvers, excluding only the pipeline's triggerer by default | L3 | No manifest or change record, so a flow assembles readiness read-only |
| 10 Deploy | The approved deployment job runs, and a rollback redeploys an earlier deployment | L0 by design | Ordering across products needs a multi-project release pipeline |
| 11 Confirm and learn | GitLab Observability (in beta), incidents and alerts | L2 | The product owner records acceptance on the work item |

GitLab is strongest where the run, the pull request and the deployment meet, because composite identity, approval rules, policies and protected environments are records in one system. It is weakest where the model needs records GitLab does not hold, and in the maturity of its agent governance.

## 3. Controls and trust boundaries

| Control | How GitLab supports it | Gap |
|---|---|---|
| 1. Never authorises | Approval rules, Code Owner approval and deployment approvals need named people, and a flow's pull request is attributed to the person who started it | The Code Review Flow approval option stays off |
| 2. No self-attestation | Pipeline execution policies inject jobs a project cannot override, and approvals reset on new commits | Flows can call pipeline and job APIs, so attestation runs in a policy job no profile can reach, with variable overrides denied |
| 3. Live only through the promotion path | Protected environments limit who deploys, and flow jobs receive no custom CI/CD variables | Agent service accounts stay off production's "Allowed to deploy" list and all bypass lists, and only approved promotion identities deploy below production |
| 4. Logged under a run identity | Actions are attributed to the flow's service account with the person recorded, and audit events are kept indefinitely | AI audit events are in beta with storage off by default, and sessions are deleted after 30 days of inactivity |
| 5. A manual route remains | Flows add to pipelines people can run, and flow execution can be turned off at each level | The manual deployment route is the adopter's own |
| 6. Hosting meets the data position | Model selection per feature, GitLab Duo Self-Hosted and the AI Gateway for GitLab Dedicated | GitLab states that its hosted AI Gateway is not a data residency solution |
| 7. The implementing model does not review | Separate model settings for the Developer Flow and the Code Review Flow | GitLab allows the same model in both, so the platform owner pins different models and the agent profile records the pairing |
| 8. Instructions only from approved sources | Prompt injection protection, tool output sanitisation and code owners on instruction files | Flows read work items and comments, and external agents lack GitLab's prompt scanning |
| 9. No data or secrets leave | Gitleaks redaction, context exclusions, the sandbox allowlist and secret push protection | The sandbox falls back to unsandboxed execution where it cannot start, so the platform owner stops such a run and runner egress controls also enforce the allow-list |

| Boundary (5.5) | What holds it on GitLab |
|---|---|
| 1. The organisation and its supplier services | The offering: GitLab Dedicated runs single-tenant in an AWS region fixed at provisioning, GitLab Self-Managed in the organisation's estate and GitLab.com as a shared service in a US region |
| 2. The agent supplier and the model providers | Model settings per feature, with GitLab's model sub-processors restricted from training on inputs and outputs, or self-hosted models |
| 3. The run and the systems it reaches | Composite identity, tool governance, MCP server blocking and a sandbox network policy that projects cannot extend |
| 4. Retrieved content and the agent's instructions | Prompt injection protection set to interrupt and code owners on instruction files |
| 5. A controlled environment and the repository | No custom CI/CD variables in flow jobs, ID tokens instead of stored credentials and context exclusions |
| 6. Non-production and production | Protected environments, deployment approvals by people and jobs that deploy only the attested artefact |

## 4. Configuration pattern

### 4.1 Intents, standards and context

The product owner decides each request on a work item in an intake project. Configurable work item types let an adopter add an intent type. Its custom fields carry the risk tier, a status lifecycle carries the appendix A statuses and a description template lays out the ten elements of 2.1. An agent drafts the intent at L2, Draft. The validators record their validations as comments. When the intent architect decides to publish, a pipeline job writes the description to a Markdown file in the leading context's repository. Code owners approve the pull request that publishes it.

Practice owners publish standards in a standards repository, and project skills package them for agents. Group-level review instructions give the Code Review Flow the pull request contract. Code owners approve every change to `AGENTS.md`, `.gitlab/duo/` and the skills folders.

### 4.2 Agents, permissions and run identities

Each profile below serves one step and one class of change. Profiles at L1, Advise, comment only.

| Step and class of change | Agent on GitLab | Level | Enforcement |
|---|---|---|---|
| 1, any intent | Custom flow | L2 | A deterministic tool step, not the agent, writes the draft to the bound work item |
| 2, gate material | Custom agent | L1 | Read and comment tools only |
| 4, standard-tier code | Developer Flow | L2 | Protected default branch and pull request review |
| 5, failing check triage | Fix CI/CD Pipeline Flow | L2 | Fixes arrive as code suggestions or a new pull request |
| 6, any change | Code Review Flow on a different model | L1 | Comments only |
| 8, test environments | Custom flow on a pipeline event | L3 | Higher tiers are protected for a deploy group |
| 9, readiness | Custom flow | L3 | Read and comment tools, with the decision left to the release authority |
| 11, post-incident review | Custom agent | L2 | A draft comment that a person publishes |

The three permissions of 5.2 map to agent tool governance: retrieval to the read category set to Always Allow, commenting to the note tools and execution to every other write or delete tool. The rules apply per tool and access type, not per profile, so execution stays at Always Deny where no profile needs it. A profile's narrower grant comes from its approved flow definition and separately enforced credentials or an execution broker. A top-level group Owner sets the rules and a project can only tighten them, as the ceiling in 5.1 requires. Enforcement for background flows sits behind a feature flag that is off by default, so the platform owner confirms it before any step rises above L1.

Each flow enabled in a group has a service account, `ai-<flow>-<group>`, with the Developer role in enabling projects. A run combines it with the person who started the flow in a one-time composite identity, with an OAuth token scoped to `ai_workflows` and `mcp`, so it reaches only what both can reach. Flows API runs require this identity only once `enforce_composite_identity_for_api_started_workflows`, off by default, is enabled, so the platform owner enables and tests the flag or blocks that entry point. A reduction under 5.4 cancels the affected flow's running sessions and their workload pipelines and disables the flow, removing its service accounts and triggers, with job logs and audit events preserved. The halt control does this for every flow and turns off the Agent Platform at the top-level group or instance.

A top-level group Owner on GitLab.com, or an administrator on GitLab Self-Managed and GitLab Dedicated, sets the model per feature. A withdrawn model makes a feature unavailable rather than falling back. AI Catalog items carry immutable versions that each group pins, and the AI Catalog CI/CD component publishes them from the platform repository on a tag pipeline. A profile change is therefore released like other platform code. Agents in an editor or the GitLab Duo CLI act as the person, so their output is that person's work, and delegated work at L2 or above runs remotely under composite identity.

### 4.3 From verification to deployment

At step 5, Verify, pipeline execution policies inject the deterministic checks with the `inject_policy` strategy, so project configuration cannot override them. Each pipeline execution policy sets `variables_override` to `allowed: false` with reviewed exceptions only and keeps `dotenv` at `respect_policy`, so no agent-controlled variable alters a policy job. Scan execution policies add static and dynamic application security testing, secret detection, dependency scanning and container scanning. A policy takes effect only when its own pull request merges in the security policy project. Merge request approval policies block a merge on findings above a threshold and route it, through `require_approval`, to the owner who may grant an exception. The pipeline carries the exception, its owner and its expiry into the release manifest. All three policy types need GitLab Ultimate, so the platform owner confirms that entitlement first.

At step 6, Review, a code owner who neither formed the intent nor produced the change approves and merges. Approval settings locked at the top-level group prevent approval by the author or by committers and remove approvals when commits are added. At step 7, Identify, a pipeline on the merged commit builds the release candidate once. With `RUNNER_GENERATE_ARTIFACTS_METADATA` set, the runner writes an in-toto statement with an SLSA v1.0 provenance predicate. A policy job then signs a statement naming the artefact digest, check results, approvals, work item key, agent profile version and run identity. It uses Sigstore keyless signing on GitLab.com or a self-hosted Sigstore elsewhere.

At steps 8 to 10, deployment jobs deploy the same artefact to each environment, and group-level protected environments restrict staging and production to a deploy group. The manifest job assembles the manifest only once the change authorisation is current. The release authority's approval of the production deployment is the separate go or no-go, and a release check confirms that this person did not request, build or implement the change. The check binds the decision to the manifest and candidate digest. Approval does not start the job, so the release manager runs it without a further approval, and the job deploys only after verifying the signed statement against the trusted signer and the manifest's candidate.

An artefact-based change rolls back through "Rollback environment" to an earlier deployment whose job pulls the earlier artefact. A state-based change applies a reviewed plan against a protected environment and restores through a new plan. A migration-based change runs a dry run in pre-production and serialises production runs with a `resource_group`. GitLab feature flags decouple deployment from release, and each activation is authorised at step 9.

### 4.4 One change through the flow

1. The product owner approves the request, a custom flow drafts a standard-tier intent and a pipeline job publishes it after validation.
2. No gate trigger is met, and the intent architect records the screening.
3. The engineer assigns the Developer Flow's service account to the unit's issue, and a workload pipeline starts in the sandbox.
4. The flow implements the unit from the published intent and opens a draft pull request carrying the work item key.
5. The policy checks run, and the engineer can ask the Fix CI/CD Pipeline Flow to propose a fix for a failure.
6. The Code Review Flow comments, and a code owner who neither formed the intent nor produced the change approves and merges it.
7. A pipeline on the merged commit builds the candidate, and a policy job signs the attestation.
8. Deployment jobs promote the candidate through test and staging.
9. The manifest job confirms the change authorisation and assembles readiness, and an independent release authority approves the production deployment.
10. The release manager runs the approved job against the attested candidate.
11. Health checks run, the product owner records acceptance and an agent drafts any post-incident review.

## 5. Gaps and complementary tooling

| Gap | Complement |
|---|---|
| No intent publication or validation record | A configured work item type and a publication job |
| No attestation that binds approvals to a candidate | A signing job in a pipeline execution policy |
| No release manifest or change record | A manifest job and the service management tool |
| Agent governance in beta | Testing by the security lead before a control depends on it |
| Sessions deleted after 30 days of inactivity | AI audit events stored, and streamed to the organisation's log store where the offering supports it |
| No evaluation harness for 5.4 | Custom flows run against historical changes in an isolated project |
| AI processing outside a residency boundary on GitLab.com, and by default on GitLab Dedicated | GitLab Dedicated with every permitted feature set to an approved regional self-hosted model, or GitLab Duo Self-Hosted |
| External agents run with limited isolation and without GitLab's prompt scanning | A supplier assessment of each provider, or GitLab's own flows for the same work |

## 6. Decisions for adopters

1. The platform owner and the security lead choose the offering against the residency position.
2. The security lead and the data owner decide the permitted models for each feature, GitLab-managed or self-hosted.
3. The platform owner pairs each implementing model with a different reviewing model, as the evaluation under 5.4 established.
4. The system steward decides the intent's work item type, fields and published location.
5. The platform owner decides the flows, external agents and AI Catalog items each group may enable.
6. The platform owner decides whether editors are permitted for person-led assistance, while delegated work at L2 or above uses a per-run identity.
7. The platform owner and the security lead decide whether agents may start pipeline jobs and the tools each profile holds.
8. The platform owner decides the tested privileged runner and image combinations that flows may use.
9. The security lead decides whether prompt injection protection logs or interrupts, and the beta features a control may rely on.
10. The security lead decides whether AI audit event storage is on and locked, and where events are retained.
11. The release authority and the platform owner decide how the attestation is produced and whether experimental SLSA attestations are used.
12. The platform owner sets usage caps and names the holder of the halt control.

## Sources

- [GitLab Duo Agent Platform](https://docs.gitlab.com/user/duo_agent_platform/)
- [Control GitLab Duo Agent Platform availability](https://docs.gitlab.com/user/duo_agent_platform/turn_on_off/)
- [GitLab 19.0 release notes](https://docs.gitlab.com/releases/19/gitlab-19-0-released/)
- [GitLab 19.2 release notes](https://docs.gitlab.com/releases/19/gitlab-19-2-released/)
- [GitLab 19.3 release notes](https://docs.gitlab.com/releases/19/gitlab-19-3-released/)
- [AI Catalog](https://docs.gitlab.com/user/duo_agent_platform/ai_catalog/)
- [Planner Agent](https://docs.gitlab.com/user/duo_agent_platform/agents/foundational_agents/planner/)
- [External agents](https://docs.gitlab.com/user/duo_agent_platform/agents/external/)
- [Foundational flows](https://docs.gitlab.com/user/duo_agent_platform/flows/foundational_flows/)
- [Custom flows](https://docs.gitlab.com/user/duo_agent_platform/flows/custom/)
- [Triggers](https://docs.gitlab.com/user/duo_agent_platform/triggers/)
- [Flows API](https://docs.gitlab.com/api/duo_agent_platform_flows/)
- [Software Development Flow](https://docs.gitlab.com/editor_extensions/software_development/)
- [Configure flow execution](https://docs.gitlab.com/user/duo_agent_platform/flows/execution/)
- [Flow execution variables](https://docs.gitlab.com/user/duo_agent_platform/flows/execution/execution-variables/)
- [Security considerations for flow execution](https://docs.gitlab.com/user/duo_agent_platform/flows/execution/security-considerations/)
- [Remote execution environment sandbox](https://docs.gitlab.com/user/duo_agent_platform/environment_sandbox/)
- [Composite identity](https://docs.gitlab.com/user/duo_agent_platform/composite_identity/)
- [Security threats in agentic systems](https://docs.gitlab.com/user/duo_agent_platform/security_threats/)
- [Sessions](https://docs.gitlab.com/user/duo_agent_platform/sessions/)
- [Agent tool governance](https://docs.gitlab.com/user/ai-governance/tool-governance/)
- [Audit AI events](https://docs.gitlab.com/user/ai-governance/ai-audit-events/)
- [Agent Platform AI models](https://docs.gitlab.com/user/duo_agent_platform/model_selection/)
- [GitLab Duo data usage](https://docs.gitlab.com/user/gitlab_duo/data_usage/)
- [Self-hosted models](https://docs.gitlab.com/administration/gitlab_duo_self_hosted/)
- [AI Gateway](https://docs.gitlab.com/administration/gitlab_duo/gateway/)
- [Configure self-hosted models for GitLab Dedicated](https://docs.gitlab.com/administration/dedicated/configure_instance/dedicated_self_hosted_models/)
- [Data residency and high availability](https://docs.gitlab.com/administration/dedicated/create_instance/data_residency_high_availability/)
- [GitLab MCP clients](https://docs.gitlab.com/user/gitlab_duo/model_context_protocol/mcp_clients/)
- [Code Review Flow](https://docs.gitlab.com/user/duo_agent_platform/flows/foundational_flows/code_review/)
- [Developer Flow](https://docs.gitlab.com/user/project/merge_requests/developer/)
- [Fix CI/CD Pipeline Flow](https://docs.gitlab.com/ci/pipelines/fix_pipeline/)
- [AGENTS.md customization files](https://docs.gitlab.com/user/duo_agent_platform/customize/agents_md/)
- [Custom rules](https://docs.gitlab.com/user/duo_agent_platform/customize/custom_rules/)
- [Agent Skills](https://docs.gitlab.com/user/duo_agent_platform/customize/agent_skills/)
- [GitLab Duo Agent Platform contextual awareness](https://docs.gitlab.com/user/duo_agent_platform/context/)
- [Configurable work item types](https://docs.gitlab.com/user/work_items/configurable_work_item_types/)
- [Workplan](https://docs.gitlab.com/user/work_items/workplan/)
- [Code Owners](https://docs.gitlab.com/user/project/codeowners/)
- [Merge request approval settings](https://docs.gitlab.com/user/project/merge_requests/approvals/settings/)
- [Policies](https://docs.gitlab.com/user/application_security/policies)
- [Pipeline execution policies](https://docs.gitlab.com/user/application_security/policies/pipeline_execution_policies/)
- [Scan execution policies](https://docs.gitlab.com/user/application_security/policies/scan_execution_policies/)
- [Merge request approval policies](https://docs.gitlab.com/user/application_security/policies/merge_request_approval_policies/)
- [Compliance frameworks](https://docs.gitlab.com/user/compliance/compliance_frameworks/)
- [Environments](https://docs.gitlab.com/ci/environments/)
- [Protected environments](https://docs.gitlab.com/ci/environments/protected_environments/)
- [Deployment approvals](https://docs.gitlab.com/ci/environments/deployment_approvals/)
- [Deployments](https://docs.gitlab.com/ci/environments/deployments/)
- [Deployment safety](https://docs.gitlab.com/ci/environments/deployment_safety/)
- [Configuring runners](https://docs.gitlab.com/ci/runners/configure_runners/)
- [SLSA level 3 provenance attestations](https://docs.gitlab.com/ci/pipeline_security/slsa/level_3/)
- [Use Sigstore for keyless signing and verification](https://docs.gitlab.com/ci/yaml/signing_examples/)
- [Audit events](https://docs.gitlab.com/user/compliance/audit_events/)
- [Observability](https://docs.gitlab.com/operations/observability/observability/)
- [Feature flags](https://docs.gitlab.com/operations/feature_flags/)
