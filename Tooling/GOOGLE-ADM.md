# Google: running the Agentic Delivery Model

> Checked against vendor documentation on 22 October 2026. Products named here change often, so check availability, names and settings against the vendor's current documentation before relying on them. This note describes one way to run the model on this toolchain and is not an endorsement.

This note covers Google's agentic development tools and the Google Cloud services that carry a change to production. Google stopped selling new Gemini Code Assist Standard and Enterprise subscriptions on 9 October 2026 and directs customers to Google Antigravity, generally available in Gemini Enterprise since 18 August 2026, so Antigravity is the coding agent here. Gemini Enterprise Agent Platform, the evolution of Vertex AI, now includes Agent Builder and runs agents built with the Agent Development Kit (ADK) on Agent Runtime, formerly Agent Engine. Cloud Build, Artifact Registry, Binary Authorization and Cloud Deploy carry the release. The model is in [MODEL.md](../MODEL.md).

Google covers steps 7 to 10 well for container workloads on Google Kubernetes Engine (GKE) and Cloud Run. Its agents serve steps 1, 4 and 6, while unattended coding runs and the release manifest need adopter work. None of these products is a work item system, and Google's coding agents usually work against GitHub repositories. Pull requests and work items therefore follow [GitHub](GITHUB-ADM.md) or [Atlassian](ATLASSIAN-ADM.md), and an adopter that enables Anthropic models in Antigravity also reads [Anthropic](ANTHROPIC-ADM.md).

## 1. Where the model's elements live

| Model element | Where it lives on Google |
|---|---|
| The request (2.2) | A work item system outside Google, searchable by Gemini Enterprise agents through the Jira Data Center and GitHub connectors. |
| The intent (2.1 and appendix A) and its published version | The working copy on the work item, and each published version as a Markdown file in the leading context's repository. |
| The work item | GitHub Issues or Jira, written by the pipeline on the run's behalf. |
| Repository instructions | `AGENTS.md` at the repository root, read by Antigravity and Jules. |
| Path-specific instructions | `AGENTS.md` in subdirectories and `.agents/rules/*.md` files with a `glob` trigger. |
| Standards as retrievable context | Skills in `.agents/skills/` or a read-only Model Context Protocol (MCP) server over the standards repository. |
| The agent profile (appendix C) | A versioned record in the platform repository, run as an Antigravity custom agent in `.agents/agents/`. |
| The context profile (appendix B) | A document per delivery context, naming its Cloud Build and Cloud Deploy definitions. |
| The tool allow-list | Gemini Enterprise settings for MCP servers, browser URLs and models, the job's `permissions` rules and Agent Registry on Agent Platform. |
| The run identity | A credential issued to each pipeline job through Workload Identity Federation, or a Cloud Build trigger's user-managed service account. |
| The workspace (step 3) | A branch and a Cloud Build container per unit, allocated by the pipeline from a versioned definition. |
| The pipeline and deterministic checks (step 5) | Cloud Build pull request triggers, whose results GitHub shows as checks. |
| The release candidate and its attestation (step 7) | An image digest in Artifact Registry with Cloud Build provenance, the pipeline's signed record of checks and a Binary Authorization attestation. |
| Environments and promotion (step 8) | Cloud Deploy targets, with one release promoted through them and environment values supplied as deploy parameters. |
| The authorisation record (step 9) | The Cloud Deploy approval of the production rollout, with the change authorisation in the service management system. |
| The release manifest | No product holds one, so a pipeline job assembles it. |
| The decision trail (8.1) | The work item, the pull request, Cloud Audit Logs and Gemini Enterprise prompt logging. |
| Telemetry for step 11 | Google Cloud Observability alerting policies, Cloud Deploy analysis and Agent Observability for agents on Agent Platform. |

## 2. Readiness against the eleven steps

"Supports up to" is a judgement from the documented capabilities: the highest autonomy level an agent can hold at the step on this toolchain with the nine controls of 5.3 in place. It is a ceiling for the evaluation under 5.4 rather than a level to assign. The model's starting levels in 3.1 apply only after that evaluation.

| Step | What Google provides | Supports up to | Gap or complement |
|---|---|---|---|
| 1 Intent | A headless Antigravity run or an ADK agent drafts from the work item through an allow-listed MCP server. | L2 | A pipeline job posts each draft. |
| 2 Gate | The same runs identify the triggers met and assemble material read-only. | L1 | Decisions go to the decision index. |
| 3 Workspace | A Cloud Build job allocates the branch and container from a versioned definition. | L0 by design | Masked data refresh is the adopter's own job. |
| 4 Build | The Antigravity CLI runs headless with the sandbox and `permissions` rules. | L2 | Workspace writes are allowed by default, and a service account route is validated in the runner. |
| 5 Verify | Cloud Build runs the shared checks, and a triage run comments a cause for a failure. | L3 | Retries stay with the pipeline. |
| 6 Review | A headless reviewer on the evaluated reviewing model comments. | L1 | Gemini Code Assist on GitHub stays off, as 4.3 explains. |
| 7 Identify | Cloud Build rechecks the merged revision and builds once with provenance, and a trusted job signs the attestation. | L0 by design | An attestation names only the image, so the pipeline signs its own record. |
| 8 Promote | Cloud Deploy promotes one release, and Identity and Access Management (IAM) conditions can limit a run identity to non-production targets. | L3 | Production waits for the release authority. |
| 9 Authorise | Cloud Deploy requires approval on the production target and notifies a service management system. | L3 | No product computes readiness or the manifest. |
| 10 Deploy | Cloud Deploy rolls out the approved release, and Binary Authorization admits only attested images on GKE and Cloud Run. | L0 by design | Other runtimes rely on the deploy identity. |
| 11 Confirm and learn | Cloud Deploy runs verification containers and analysis against alerting policies, and an agent drafts the review. | L2 | The product owner accepts on the work item. |

Google is strongest at steps 7 to 10, with provenance at Supply-chain Levels for Software Artifacts (SLSA) level 3, admission control on attestations, approvals scoped to a target and Admin Activity audit logs that cannot be disabled. It is weakest at the edges of the flow, with no work item system, no release manifest and an unattended service account route each adopter validates.

## 3. Controls and trust boundaries

| Control | How Google supports it | Gap |
|---|---|---|
| 1. Never authorises | People alone hold `roles/clouddeploy.approver`, limited by IAM conditions to the production target. | Self-approval by a promoter is not documented as blocked, so separate groups hold the two roles. |
| 2. Cannot attest to its own work | Google signs the provenance, and only the attestation job can use the attestor's Cloud Key Management Service (Cloud KMS) key. | Headless runs accept `--dangerously-skip-permissions`, so the shared job omits it. |
| 3. Published promotion path only | Binary Authorization blocks unattested images at admission, and deploy policies block rollouts in set windows. | Admission control covers GKE and Cloud Run only. |
| 4. Logged under a run identity | Admin Activity audit logs are always written and immutable, prompt logging keeps sessions in the organisation's project and `stream-json` output records each tool step. | Prompt logging and Data Access audit logs are off by default. |
| 5. A manual route remains | A named person can promote, approve and roll back with `gcloud deploy`, and administrators can turn the AI developer tools off. | A halt also blocks new pipeline runs, cancels active jobs, ends their processes and revokes their credentials, with logs kept. |
| 6. Hosting meets the data position | Gemini Enterprise locations, a model allow-list and Google's statement that Gemini does not train on prompts or responses. | In-country regions carry no residency commitment for the AI developer tools. |
| 7. Implementing model does not review | The job's `--model` setting pins builder and reviewer to different models the agent profile records. | Custom agents set only a tier, so they use `model: inherit` and the job pins the evaluated model with `--model`. |
| 8. Approved sources, retrieved text as data | Rules load from the checked-out commit, `PreToolUse` hooks can deny a tool call and Model Armor screens Agent Gateway traffic. | Agent Gateway serves agents on Agent Platform, not Antigravity sessions. |
| 9. No data or secrets leave | The sandbox blocks `~/.ssh` and `.env` and has no network by default, and VPC Service Controls can enclose the Antigravity APIs. | Administrator defaults leave the sandbox disabled and terminal execution on Always proceed. |

| Boundary | What holds it on Google |
|---|---|
| 1. The organisation and its supplier services | The Google Cloud terms, the Gemini Enterprise location and any Enterprise Gateway the organisation operates for model traffic. Google lists ISO 27001 and SOC 2 among certifications Antigravity in Gemini Enterprise does not support. |
| 2. The agent supplier and the model providers | The model allow-list, with third-party models off until an administrator accepts the provider's terms and their inference limited to `global`, `us` or `eu`. |
| 3. The run and the systems it reaches | IAM on the job's credential, `permissions` rules, the MCP allow-list and Agent Gateway's default deny on Agent Platform. |
| 4. Retrieved content and the agent's instructions | Rules pinned in the repository, the published intent only, `PreToolUse` hooks and Model Armor. |
| 5. A controlled environment and the repository | The terminal sandbox, a VPC Service Controls perimeter, Secret Manager and secret scanning on the repository platform. |
| 6. Non-production and production | Approval on the production target, Binary Authorization at admission and a deploy identity that no agent session holds. |

## 4. Configuration pattern

### 4.1 Intents, standards and context

The work item holds the intent's working copy. On publication a pipeline job commits the published version to a folder the adopter names in the leading context's repository, through a pull request the code owners approve. A build reads only that file. Path-specific rules sit in `.agents/rules/` with frontmatter setting `trigger: glob` and `globs`, and Antigravity discards a rule file without valid frontmatter. Rule files are truncated at 24 KB, so standards sit better in skills or behind a read-only MCP server, retrieved at the point of use. The job checks out the platform repository at the tag the agent profile names.

### 4.2 Agents, permissions and run identities

Each delegated step and class has an Antigravity custom agent in `.agents/agents/` with an explicit `tools` list, a `commandExecutionPolicy` and its MCP servers. A pipeline job invokes it with `agy -p`, `--agent` and `--model`, so the job definition fixes the profile and the model. Headless mode soft-denies any tool call that needs approval and that the job's `permissions` rules do not grant.

| Profile | Step and class | Level | Enforcement on Google |
|---|---|---|---|
| Intent drafter | 1, every class | L2 | Read-only MCP access to the work item, with the pipeline posting the draft. |
| Gate screener | 2, every class | L1 | Read tools only, with output posted as a comment. |
| Builder | 4, one class such as unit tests | L2 | Read-only instructions and checks, confined writable paths, named `command` rules and the sandbox on. |
| Check triager | 5, every class | L2 | Reads check logs and comments a cause, holding no Cloud Build role. |
| Reviewer | 6, every class | L1 | A headless run on the evaluated reviewing model, commenting only. |
| Promoter | 8, one class | L3 | `roles/clouddeploy.releaser` with an IAM condition naming the non-production targets. |
| Readiness assembler | 9, every class | L3 | Read-only roles, with the pipeline writing the manifest. |
| Telemetry correlator | 11, every class | L2 | Read-only monitoring access, drafting the post-incident review. |

The model's three permissions stay separate. Retrieval comes from read-only IAM roles, MCP servers and `read_file` rules, commenting from a pipeline permission that approves nothing and execution from workspace writes, `command` rules or a Cloud Deploy role. Antigravity allows writes anywhere in the workspace by default, so the job mounts the approved instructions and check definitions read-only and confines writable paths through operating system controls. Permission rules add restrictions, as in this illustration of the builder's `~/.gemini/antigravity-cli/settings.json` in forms from Google's documentation:

```json
{
  "permissions": {
    "allow": ["command(npm test)"],
    "deny": ["command(sudo *)", "write_file(.git/)", "write_file(.agents/)"]
  }
}
```

The pipeline issues a run identity per job. GitHub Actions jobs authenticate through Workload Identity Federation, an option the Run Gemini CLI action documents. Cloud Build jobs run under the trigger's service account, with the build ID tying actions to one run. Antigravity documents Application Default Credentials for headless use, and the platform owner validates a service account route in the chosen runner before unattended builds start. Desktop sessions run under the developer's identity and are not run identities. Agent Identity, with its API in preview, gives each agent on Agent Runtime its own principal.

Gemini Enterprise settings apply whatever model a developer selects, but Google does not say whether users can override them, so the ceiling for headless runs is the job definition and its IAM bindings. Jules works on GitHub, but its paid plans are open only to individual Google Accounts, so this pattern leaves it outside the permitted surfaces.

### 4.3 From verification to deployment

**Verify (step 5).** Cloud Build triggers call the shared check definition under a user-managed service account, logging to Cloud Logging. The organisation policy `constraints/cloudbuild.allowedIntegrations` limits the hosts that can invoke triggers, and GitHub requires the checks before merge.

**Review (step 6).** The reviewer profile comments through the pipeline. Code Assist on GitHub does not identify its model or say whether it approves, so `code_review.disable: true` keeps it off. A code owner who neither formed the intent nor produced the change approves and merges.

**Identify (step 7).** On merge one trigger reruns the required checks on the exact merged revision and builds the candidate once. It publishes the image to Artifact Registry with immutable tags through the `images` field, and `requestedVerifyOption: VERIFIED` fails the build without provenance. After the build completes, a trusted job under its own service account verifies the digest and provenance. It signs a record binding the digest to the check results, approvals, standards versions, published intent, agent profile version and run identity. It then signs the Binary Authorization attestation with its Cloud KMS key and exports a software bill of materials.

**Promote (step 8).** A Cloud Deploy release created from the image digest snapshots the delivery pipeline. Each promotion creates a rollout, deploy parameters supply environment values and a verification container runs tests in each environment.

**Authorise (step 9).** A job confirms the change authorisation is current before assembling the manifest. The production target sets `requireApproval: true`, and only the release authority's group holds `roles/clouddeploy.approver` on it. Cloud Deploy notifies the `clouddeploy-approvals` Pub/Sub topic, so a service management system can record the release authority's go or no-go on the manifest and approve through the API. A release check confirms that the approver did not request, build or implement the change. The audit log then names the integration's identity, and the service management record evidences the person's decision.

**Deploy (step 10).** The rollout uses a standard or canary strategy, and Binary Authorization requires both the `built-by-cloud-build` attestor and the pipeline's attestor. Recovery uses `gcloud deploy targets rollback` to create a new rollout from the last successful release.

**Deployment approaches.** The artefact-based approach is native. A state-based change deploys a declared-state commit through a Cloud Deploy custom target type, bound by the pipeline's signed record because Binary Authorization covers images. A migration-based change runs its ordered set as a deploy hook, with the dry run and reconciliation as the adopter's own tasks.

### 4.4 One change through the flow

A standard-tier change adds a date filter to an existing order history page served from Cloud Run.

1. **Intent.** The product owner approves the request, the intent drafter drafts the intent and the intent architect publishes it once the validators have recorded their checks.
2. **Gate.** The intent architect records that no trigger is met.
3. **Workspace.** A Cloud Build job creates the unit's branch and container.
4. **Build.** The builder profile runs headless with the sandbox on, and the pipeline raises the pull request with the work item key.
5. **Verify.** One check fails, the triage run comments a cause and the corrected commit passes.
6. **Review.** The reviewer profile comments, and a code owner who neither formed the intent nor produced the change approves and merges.
7. **Identify.** The merge trigger rechecks the merged revision and builds the image once, and the trusted job signs the record and attestation.
8. **Promote.** The promoter profile promotes the release to test and pre-production, where verification runs.
9. **Authorise.** The pipeline confirms the change authorisation is current and assembles the manifest, and the release authority, who did not request, build or implement the change, approves the production rollout.
10. **Deploy.** Cloud Deploy rolls out a canary, Binary Authorization admits the attested image and analysis watches the alerting policies.
11. **Confirm and learn.** The telemetry correlator summarises the health signals, and the product owner accepts the change on the work item.

## 5. Gaps and complementary tooling

| Gap | Complement |
|---|---|
| Work items and the request record | GitHub Issues or Jira, with the pipeline writing to the work item. |
| Pull request governance and secret scanning | The repository platform, usually GitHub. |
| The change record | A service management system connected through Cloud Deploy notifications. |
| The release manifest | A pipeline job reading Cloud Deploy, Artifact Registry and Binary Authorization records. |
| A run identity for unattended coding runs | Federated credentials per job, with Antigravity authentication proved in the pilot. |
| Assurance evidence for Antigravity | A supplier assessment covering the certifications Google lists as unsupported. |

## 6. Decisions for adopters

1. **Surfaces.** The platform owner decides whether agents run only headless in pipelines or also on desktops, where JetBrains, Zed and Xcode support is in preview.
2. **Permitted models.** The security lead and the data owner set the model allow-list from the permitted model list and decide on Anthropic models.
3. **Location and model traffic.** The data owner chooses `global`, `us`, `eu` or an in-country region, and the platform owner decides whether model traffic passes through an Enterprise Gateway.
4. **Supplier assurance.** The security lead decides whether Antigravity is acceptable without the certifications Google lists as unsupported.
5. **Administrator baseline.** The platform owner turns the sandbox and prompt logging on and sets terminal execution to Require review or Proceed in sandbox.
6. **Intent location.** The intent architect names the repository folder for published intents.
7. **Run identities.** The platform owner chooses the credential each job uses and proves headless Antigravity authentication.
8. **Agent-initiated jobs.** The platform owner decides whether any agent may invoke Cloud Build or Cloud Deploy jobs.
9. **Custom agents.** The platform owner decides whether agents for steps 1, 2, 9 and 11 run on Agent Runtime behind Agent Gateway, the control point for their MCP and Agent2Agent (A2A) traffic.
10. **Jules and Code Assist.** The security lead decides whether Jules stays excluded and existing Code Assist use continues.
11. **Log retention.** The security lead decides the Data Access audit logs to enable and the sinks that retain them.

## Sources

- [Gemini Code Assist Standard and Enterprise subscriptions](https://docs.cloud.google.com/gemini/docs/codeassist/sunset)
- [Review GitHub code using Gemini Code Assist](https://docs.cloud.google.com/gemini/docs/code-review/review-repo-code)
- [Customize Gemini Code Assist behavior in GitHub](https://docs.cloud.google.com/gemini/docs/code-review/customize-repo-review)
- [How Gemini products in Google Cloud use your data](https://docs.cloud.google.com/gemini/docs/discover/data-governance)
- [Run Gemini CLI](https://github.com/marketplace/actions/run-gemini-cli)
- [AI developer tools overview](https://docs.cloud.google.com/gemini/enterprise/docs/ai-developer-tools-overview)
- [Configure AI developer tools settings](https://docs.cloud.google.com/gemini/enterprise/docs/ai-developer-tools-settings)
- [Deploy Google Antigravity Enterprise Gateway](https://docs.cloud.google.com/gemini/enterprise/docs/ai-developer-tools/deploy-enterprise-gateway)
- [Data residency for Gemini Enterprise Standard and Plus Editions and Gemini Notebook Enterprise](https://docs.cloud.google.com/gemini/enterprise/docs/locations)
- [Gemini Enterprise release notes](https://docs.cloud.google.com/gemini/enterprise/docs/release-notes)
- [Antigravity in Gemini Enterprise](https://antigravity.google/docs/enterprise)
- [Agent permissions](https://antigravity.google/docs/permissions)
- [Terminal sandbox](https://antigravity.google/docs/sandbox)
- [Rules](https://antigravity.google/docs/rules)
- [Headless mode](https://antigravity.google/docs/cli/headless)
- [Hooks](https://antigravity.google/docs/hooks/)
- [Custom subagents](https://antigravity.google/docs/subagents/)
- [Limits and Plans (Jules)](https://jules.google/docs/usage-limits/)
- [Introducing Gemini Enterprise Agent Platform, powering the next wave of agents](https://cloud.google.com/blog/products/ai-machine-learning/introducing-gemini-enterprise-agent-platform)
- [Gemini Enterprise Agent Platform release notes](https://docs.cloud.google.com/gemini-enterprise-agent-platform/release-notes)
- [Agent Development Kit](https://docs.cloud.google.com/gemini-enterprise-agent-platform/build/adk)
- [Use Agent Identity with Agent Runtime](https://docs.cloud.google.com/gemini-enterprise-agent-platform/scale/runtime/agent-identity)
- [Agent Gateway overview](https://docs.cloud.google.com/gemini-enterprise-agent-platform/govern/gateways/agent-gateway-overview)
- [Building repositories from GitHub](https://docs.cloud.google.com/build/docs/automating-builds/github/build-repos-from-github)
- [Generate and validate build provenance](https://docs.cloud.google.com/build/docs/securing-builds/view-build-provenance?authuser=0)
- [Generate and store SBOMs](https://docs.cloud.google.com/artifact-analysis/docs/generate-store-sboms)
- [Run builds with a user-managed service account](https://docs.cloud.google.com/build/docs/securing-builds/configure-user-specified-service-accounts)
- [Update repository settings](https://docs.cloud.google.com/artifact-registry/docs/repositories/update-repo-settings)
- [Deploy only images built by Cloud Build](https://docs.cloud.google.com/binary-authorization/docs/deploy-cloud-build)
- [Create a Binary Authorization attestation in a Cloud Build pipeline](https://docs.cloud.google.com/binary-authorization/docs/cloud-build)
- [Create attestations](https://docs.cloud.google.com/binary-authorization/docs/making-attestations)
- [Cloud Deploy Terminology](https://docs.cloud.google.com/deploy/docs/terminology?hl=en)
- [Promote your release and manage approvals](https://docs.cloud.google.com/deploy/docs/promote-release)
- [Use a deployment strategy](https://docs.cloud.google.com/deploy/docs/deployment-strategies?authuser=5)
- [IAM roles and permissions](https://docs.cloud.google.com/deploy/docs/iam-roles-permissions)
- [Restrict deploy behavior using policies](https://docs.cloud.google.com/deploy/docs/deploy-policy)
- [Roll back a target](https://docs.cloud.google.com/deploy/docs/roll-back)
- [Run analysis jobs in Cloud Deploy](https://docs.cloud.google.com/deploy/docs/analysis)
- [Cloud Audit Logs overview](https://docs.cloud.google.com/logging/docs/audit#types)
