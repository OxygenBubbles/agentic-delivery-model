# OpenAI (Codex): running the Agentic Delivery Model

> Checked against vendor documentation on 22 October 2026. Products named here change often, so check availability, names and settings against the vendor's current documentation before relying on them. This note describes one way to run the model on this toolchain and is not an endorsement.

This note covers OpenAI's Codex coding agent and the controls around it. Codex runs in its command-line interface (CLI), an integrated development environment (IDE) extension, the ChatGPT desktop app and Codex Cloud. It reviews pull requests in GitHub, takes repository work started in Slack through the ChatGPT app and runs headless through `codex exec`, the Codex GitHub Action and the Codex SDK. The note also covers the ChatGPT Enterprise controls that govern Codex and the Agents SDK and Agents API for orchestration.

Codex serves steps 4 and 6, Build and Review, directly. A pipeline can also invoke it to draft at steps 1, 5, 9 and 11. It holds no work item, pipeline, release candidate, environment or release record, so the other steps run on the version control and continuous integration and continuous delivery (CI/CD) platform beside it. That is usually GitHub, covered in [GitHub](GITHUB-ADM.md), with [GitLab](GITLAB-ADM.md), [Microsoft](MICROSOFT-ADM.md) and [Atlassian](ATLASSIAN-ADM.md) covering other platforms and work management. Section 3 of this note relates Codex's review of another vendor's agent, for example from [Anthropic](ANTHROPIC-ADM.md), [Google](GOOGLE-ADM.md) or [Factory](FACTORY-ADM.md), to control 7 in 5.3 of [MODEL.md](../MODEL.md).

## 1. Where the model's elements live

| Model element | Where it lives on OpenAI (Codex) |
|---|---|
| The request (2.2) | Outside Codex, in the work management system. A read-only run can gather the evidence for 2.2 through an allow-listed Model Context Protocol (MCP) server. |
| The intent (2.1 and appendix A) and its published version | The working copy on the work item, and a Markdown file published by pull request into the leading context's repository. |
| The work item | A GitHub issue, a GitLab issue (in beta), a Linear issue or a record reached through MCP, written by the pipeline and only read by the run. |
| Repository instructions | The root `AGENTS.md`, with a `## Code Review Rules` section for code review. |
| Path-specific instructions | Nested `AGENTS.md` or `AGENTS.override.md` files, the nearest taking precedence. |
| Standards as retrievable context | A standards repository checked out beside the change, or an approved MCP server. |
| The agent profile (appendix C) | A versioned record in the platform repository, mapped to the model, sandbox or permission profile (in beta), rules, hooks and run inputs. |
| The context profile (appendix B) | The platform repository. A Codex Cloud environment covers only workspace setup. |
| The tool allow-list | Managed `requirements.toml` in Agent Security: MCP servers, command rules, features and network domains. |
| The run identity | Codex issues none. A pipeline run uses the job's token and an OpenAI application programming interface (API) key. |
| The workspace (step 3) | A pipeline runner with Codex in the `workspace-write` sandbox, or an isolated Codex Cloud task. |
| The pipeline and deterministic checks (step 5) | The CI/CD platform. Codex runs inside a job and owns no check. |
| The release candidate and its attestation (step 7) | The CI/CD platform. |
| Environments and promotion (step 8) | The CI/CD platform and the deployment engine. |
| The authorisation record (step 9) | The service management system and the CI/CD platform's environment approvals. |
| The release manifest | A pipeline job, with Codex able to draft it. |
| The decision trail (8.1) | Pull request records, Compliance API records, OpenTelemetry events and the `codex exec --json` stream kept as a pipeline artefact. |
| Telemetry for step 11 | The observability platform, read through an allow-listed MCP server. |

## 2. Readiness against the eleven steps

The "Supports up to" column is a judgement from OpenAI's documented capabilities: the highest level an agent can hold at the step on this toolchain with the nine controls of 5.3 in place. The model's starting levels are in 3.1, and a level takes effect only after the evaluation and approval under 5.4.

| Step | What OpenAI (Codex) provides | Supports up to | Gap or complement |
|---|---|---|---|
| 1 Intent | `codex exec` in a pipeline job drafts the intent, with `--output-schema` fixing its shape. | L2 | The work item system and the job that writes drafts back. |
| 2 Gate | A read-only run identifies the triggers met and assembles the material. | L1 | The decision index. |
| 3 Workspace | A pipeline runner per job, or a Codex Cloud task from a published environment. | L0 by design | The install script kept in the repository, and masked data from pipeline jobs. |
| 4 Build | The CLI, IDE extension, Codex Cloud, `codex exec` and Codex SDK, under a sandbox, approval policy, command rules and hooks. | L2 | A per-run identity from the CI/CD platform. |
| 5 Verify | A run in a pipeline job reads a failing check's log and proposes the cause. | L2 for triage, L3 where the CI/CD platform holds the gate | Required checks, retries and protected workflow files. |
| 6 Review | Codex code review flags only P0 and P1 issues. A pipeline job can review on a pinned model. | L1 | Approval and merge by a code owner under branch protection. |
| 7 Identify | Codex has no part. The CI/CD platform builds and attests the candidate deterministically. | L0 by design | The CI/CD platform's attestation. |
| 8 Promote | A run invokes one approved non-production job that a command rule allows. | L3, with the gate on the CI/CD platform | Environment protection and deployment history. |
| 9 Authorise | A pipeline job has Codex draft the manifest. | L3 for assembly, the decision a person's | The change record and the release authority. |
| 10 Deploy | Codex has no part. The deployment engine applies the authorised candidates in declared order. | L0 by design | The deployment engine under the authorisation. |
| 11 Confirm and learn | A read-only run correlates telemetry and drafts the post-incident review. | L2 | Health checks, observability and acceptance at L0. |

The toolchain is strongest where the agent runs: the sandbox, approval policy, command rules, hooks and managed requirements give the platform owner a ceiling that users cannot override. It is weakest at steps 7 to 10, where it provides nothing. Step 3 is also weak, because a person rather than the pipeline publishes a cloud environment. The run identity is the other weakness: interactive runs and cloud tasks act as the person who started them.

## 3. Controls and trust boundaries

| Control | How OpenAI (Codex) supports it | Gap |
|---|---|---|
| 1. Never the sole authoriser | Codex has no approval authority of its own, and its review does not replace a code owner's approval. | The review state Codex posts is undocumented, so branch protection counts only code owners' approvals. |
| 2. Cannot attest to its own work | Under `workspace-write`, the `.git`, `.codex` and `.agents` paths stay read-only. | Checks, retries and attestation sit on the CI/CD platform, with code owners on workflow files. |
| 3. Published promotion path only | No deployment credential in a run, `forbidden` rules on deployment commands and proxy-held network secrets in Codex Cloud. | The promotion path belongs to the CI/CD platform. |
| 4. Logged under a run identity | `codex exec --json` events, OpenTelemetry tool decisions and Compliance API records. | Records name a person or an API key, compliance logs are kept for 30 days and OpenTelemetry export is off by default. |
| 5. Manual route | Codex runs inside the repository and pipeline, so a person can perform the same step there. | The context profile records the route. |
| 6. Hosting meets the data position | No training on business data by default, admin enablement of the newest models and data residency for the Codex app and CLI. | Residency excludes Codex Web. The Agents API has residency only in the United States and no zero data retention. |
| 7. Implementer does not review | Each run pins its model through configuration, `--model` or the action's `model` input. Codex reviewing another vendor's agent brings a different model. | The hosted review model is not named, so an evaluated pairing uses a review job on a pinned model. |
| 8. Approved sources, retrieved text as data | The `AGENTS.md` chain, project rules and hooks loaded only from a trusted project layer, managed-only hooks and cached web search by default. | `AGENTS.md` is read from the working tree, so code owners approve changes to it. |
| 9. No data or secrets out | No local network by default, cloud internet access per environment with domain allow-lists and `sudo` removed by the GitHub Action. | Secret scanning of agent output and data masking belong to the pipeline. |

| Boundary | What holds it on OpenAI (Codex) |
|---|---|
| 1. The organisation and its supplier services | The ChatGPT Enterprise terms, retention settings, data residency where eligible, single sign-on with directory sync and an address allow-list for the workspace and the Compliance API. |
| 2. The agent supplier and the model providers | OpenAI is both. The permitted model list draws on the Codex models page and the workspace model settings, and a custom provider needs a compatible Responses API endpoint. |
| 3. The run and the systems it reaches | The sandbox or permission profile, command rules, approved MCP servers, proxy domain rules and the job's token permissions. |
| 4. Retrieved content and the agent's instructions | Control 8, with a build that reads only the published intent. |
| 5. A controlled environment and the repository | Control 9, with masked data from pipeline jobs and saved cloud task state recoverable for up to seven days. |
| 6. Non-production and production | Codex holds nothing that crosses it. The CI/CD platform's protected environments and the authorisation at step 9 hold it. |

## 4. Configuration pattern

### 4.1 Intents, standards and context

The intent's working copy lives on the work item, and its published version is a Markdown file merged by pull request into the leading context's repository. A build run reads only that file from the checkout. Codex has no intent object, so the appendix A template is held as a file.

Repository instructions go in the root `AGENTS.md`. Codex reads the file in its home directory, then one file per directory from the project root to the working directory, with nearer files taking precedence. A nested `AGENTS.md` therefore carries path-specific instructions, and the chain stops at `project_doc_max_bytes`, 32 KiB by default.

Standards come from a standards repository checked out beside the change or from an MCP server on the approved `mcp_servers` list. Configuration, rules and skills sit in `.codex` or `.agents`, and OpenAI states that repository configuration cannot grant workspace, model or connected-system access. Code owners hold these folders, `AGENTS.md` and the standards.

### 4.2 Agents, permissions and run identities

Each agent profile in appendix C names a step and class, a level, a pinned model and the Codex settings that enforce the level. The platform owner holds the ceiling in managed `requirements.toml` that users cannot override, delivered through Agent Security, a system file or macOS device management. The profiles below suit a context on GitHub.

| Profile | Step and class | Level | How Codex enforces it |
|---|---|---|---|
| Intent drafter | 1, every class | L2 | `codex exec` read-only, the pipeline writing the draft to the work item |
| Gate adviser | 2, intents meeting a trigger | L1 | Read-only, the pipeline posting the comment |
| Builder | 4, the classes the evaluation covers | L2 | `workspace-write` in a job holding `contents: read`, producing a patch |
| Triage | 5, a failing check | L2 | Read-only over the job log |
| Reviewer | 6, every class | L1 | Codex code review, or a review job on a different model |
| Promoter | 8, classes approved at L3 | L3 | One `allow` rule for the approved job, `forbidden` rules for other deployment commands |
| Readiness | 9, every release | L3 | A pipeline job drafting the manifest |
| Confirmer | 11, every class | L2 | Read-only, with a read-only telemetry MCP server |

The three permissions of 5.2 map to separate settings. Retrieval is the `read-only` sandbox with read-only MCP servers. Commenting belongs to a pipeline job holding `pull-requests: write`, while the Codex job keeps `contents: read` as OpenAI's continuous integration guidance recommends. Execution is `workspace-write` or a permission profile at L2 and one `allow` rule at L3.

The requirements also limit approval policies, the approvals reviewer, permission profiles, web search and MCP servers. A client that cannot load the cloud requirements returns an error rather than starting without them. Because `auto_review` lets a reviewer agent approve an action beyond the sandbox, the security lead decides its use through `allowed_approvals_reviewers`. Workspace settings and custom roles decide who may use Codex locally or in the cloud.

A cloud task uses the signed-in person's GitHub connection, and an interactive run or cloud task is that person's own work, so the person cannot approve its pull request. Headless runs therefore go through the CI/CD platform, where the job's short-lived token serves as the run identity and the API key reaches only the step that runs Codex.

A reduction under 5.4 uses the same settings: the platform owner tightens the requirements, withdraws a role or a model, revokes the API key or disables the workflow. Role changes can take up to five minutes to apply. Disabling the Codex workflows and revoking the key give the halt control for headless runs.

### 4.3 From verification to deployment

Steps 5 to 10 run on the CI/CD platform, and [GitHub](GITHUB-ADM.md) describes required workflows, artefact attestation, protected environments and deployment history. At step 5 a read-only triage run proposes the cause of a failing check, and the pipeline posts it and decides whether the change proceeds. At step 6 Codex code review checks the pull request against the `## Code Review Rules` and posts a standard GitHub review that flags only P0 and P1 issues. OpenAI positions it as another review signal before a person's merge approval. GitLab calls a pull request a merge request, and Codex reviews merge requests there in beta.

At step 8 a promoter profile invokes one approved non-production job, and the CI/CD platform's environment rules decide whether it runs. At step 9 Codex drafts the manifest in a pipeline job from the check results, approvals and recovery evidence. The release authority decides, and nothing Codex produces counts as the change authorisation or the go or no-go. At steps 7 and 10 the pipeline attests the candidate from the exact merged revision and the deployment engine applies it, with no part for Codex.

Codex works the same way for each deployment approach. It drafts code, declared state or ordered migrations. The pipeline then builds the package, validates the plan or runs the dry run and reconciliation. No Codex run holds a production credential, and the context profile limits its commands with rules like the illustration below, written in OpenAI's documented syntax.

```
# Illustration only: a builder profile may not start pipeline workflows
prefix_rule(
    pattern = ["gh", "workflow", "run"],
    decision = "forbidden",
    justification = "Only the pipeline or an approved promoter profile starts workflows",
)
```

OpenAI labels command rules experimental, so the boundary remains the credentials a run lacks.

### 4.4 One change through the flow

The example is a standard-tier change that adds a validation rule to an existing field in a web application on GitHub.

1. Intent: a pipeline job runs `codex exec` read-only to draft the intent and writes it to the work item. The validators validate it and the intent architect publishes it by pull request.
2. Gate: the drafting run records that no trigger is met, and the intent architect confirms the screening.
3. Workspace: the pipeline allocates a branch and a runner from a versioned definition.
4. Build: a job runs `codex exec` in `workspace-write` on the pinned model against the published intent and uploads a patch. A separate job applies it and opens a pull request carrying the work item key.
5. Verify: the required checks run, and on a failure the pipeline posts the cause a read-only triage run proposes.
6. Review: Codex code review comments on any P0 or P1 issue, and a code owner who did not produce the change approves and merges.
7. Identify: the pipeline builds and attests the candidate, recording the agent profile version and the job reference.
8. Promote: the pipeline promotes the candidate through test and pre-production, because no Codex profile holds L3 for this class.
9. Authorise: Codex drafts the manifest in a pipeline job, and the release authority authorises the release.
10. Deploy: the deployment job applies the authorised candidate with no further approval.
11. Confirm and learn: health checks run, a read-only run reads telemetry through an allow-listed MCP server and the product owner accepts the change.

## 5. Gaps and complementary tooling

| Gap | What an adopter uses |
|---|---|
| The request decision and the work item | A work item system, covered in [Atlassian](ATLASSIAN-ADM.md), [Microsoft](MICROSOFT-ADM.md) and [GitHub](GITHUB-ADM.md). |
| A run identity per run | The CI/CD platform's per-job token, with the API key held in one API project and rotated. |
| Pipeline, attestation, environments and deployment | The CI/CD platform, covered in [GitHub](GITHUB-ADM.md), [GitLab](GITLAB-ADM.md) and [Microsoft](MICROSOFT-ADM.md). |
| Manifest, change record and telemetry | A manifest generator in the pipeline, the service management system and the observability platform. |
| Evaluation under 5.4 | A harness built from `codex exec`, `--output-schema` and historical changes. OpenAI's Evals product is no longer available from 30 November 2026. |
| Record retention | A security information and event management system that collects Compliance API records within 30 days. |
| Orchestration across steps | The pipeline, with the Agents SDK for agents in code or the Agents API, in public beta, for a managed Codex harness. |

Three transitions bear on an adoption plan. Code review, Security Review and the GitHub and Linear integrations still run on Codex Cloud (Legacy), and OpenAI plans to deprecate it without a published date. The `codex mcp-server` command was removed on 5 September 2026, and its replacement, the Codex app server, is experimental and not supported for production workloads. Agent Builder is no longer available on the OpenAI platform from 30 November 2026.

## 6. Decisions for adopters

1. The platform owner and the security lead decide the Codex surfaces that carry delivery work, including Slack delegation, where anyone with access to the channel can read a reply.
2. The security lead and the data owner decide the permitted model list from the models page, currently GPT-6 Astra, GPT-6.1 Sol, GPT-6 Sol and GPT-6 Luna. A retirement, such as GPT-5.5 under ChatGPT sign-in on 14 October 2026, suspends the affected profiles' execution authority under 5.4.
3. The platform owner decides whether sign-in is pinned to the ChatGPT workspace through login requirements in a system file or device management. The same owner names the API project that holds the pipeline's key.
4. The intent architect and the platform owner decide where intents live and how the pipeline publishes them.
5. The security lead decides whether any agent profile may use `auto_review`.
6. The platform owner and the release authority decide whether agent-initiated pipeline jobs are allowed at step 8 and under what rules.
7. The security lead sets the network position: cloud internet access, the domain allow-list and local proxy rules, whose managed requirements section is experimental.
8. The data owner decides what surfaces meet the data position, on the residency and retention terms in control 6.
9. The platform owner decides whether Codex reviews another vendor's agent output under control 7 or the reverse. The evaluation under 5.4 covers the chosen pairing.
10. The platform owner plans the move of code review and the integrations off Codex Cloud (Legacy).
11. The security lead decides whether only managed hooks run and what events feed the decision trail.
12. The platform owner chooses the orchestration route: the pipeline with `codex exec`, the Codex SDK, the Agents SDK or the Agents API.

## Sources

- [Changelog](https://learn.chatgpt.com/docs/changelog)
- [Codex IDE extension](https://learn.chatgpt.com/docs/codex/ide.md)
- [Agent approvals & security](https://learn.chatgpt.com/docs/agent-approvals-security.md)
- [Sandboxing](https://learn.chatgpt.com/docs/sandboxing)
- [Permissions](https://learn.chatgpt.com/docs/permissions)
- [Rules](https://learn.chatgpt.com/docs/agent-configuration/rules.md)
- [Hooks](https://learn.chatgpt.com/docs/hooks.md)
- [Custom instructions with AGENTS.md](https://learn.chatgpt.com/docs/agent-configuration/agents-md.md)
- [Codex Models](https://learn.chatgpt.com/docs/models.md)
- [Cloud environments](https://learn.chatgpt.com/docs/environments/cloud-environments)
- [Using Codex cloud](https://help.openai.com/en/articles/20001545-using-codex-cloud)
- [Codex code review in GitHub](https://learn.chatgpt.com/docs/third-party/github)
- [GitHub code reviews](https://learn.chatgpt.com/use-cases/github-code-reviews)
- [Use Codex in Slack](https://learn.chatgpt.com/docs/third-party/slack.md)
- [Non-interactive mode](https://learn.chatgpt.com/docs/non-interactive-mode.md)
- [Codex GitHub Action](https://learn.chatgpt.com/docs/github-action.md)
- [Codex SDK](https://learn.chatgpt.com/docs/codex-sdk.md)
- [Use Codex with the Agents SDK](https://learn.chatgpt.com/docs/mcp-server.md)
- [Admin setup](https://learn.chatgpt.com/docs/enterprise/admin-setup)
- [Managed configuration](https://learn.chatgpt.com/docs/enterprise/managed-configuration.md)
- [Agent security](https://learn.chatgpt.com/docs/enterprise/agent-security)
- [Governance](https://learn.chatgpt.com/docs/enterprise/governance.md)
- [OpenAI Compliance Platform for Enterprise and Edu customers](https://help.openai.com/articles/9261474)
- [ChatGPT Enterprise admin quickstart](https://help.openai.com/en/articles/20001264-chatgpt-enterprise-admin-quickstart)
- [Managing feature access with role-based access control in ChatGPT](https://help.openai.com/sr-rs/articles/11750701-managing-feature-access-with-role-based-access-control-in-chatgpt)
- [Data residency and inference residency for ChatGPT](https://help.openai.com/en/articles/9903489-data-residency-and-inference-residency-for-chatgpt)
- [Enterprise privacy at OpenAI](https://openai.com/enterprise-privacy/)
- [Agents SDK](https://developers.openai.com/api/docs/guides/agents/sdk.md)
- [Introducing the Agents API](https://openai.com/index/introducing-the-agents-api/)
- [Agents API overview](https://developers.openai.com/api/docs/guides/agents-api/overview)
- [Introducing AgentKit](https://openai.com/index/introducing-agentkit/)
