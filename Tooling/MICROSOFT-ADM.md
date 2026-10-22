# Microsoft: running the Agentic Delivery Model

> Checked against vendor documentation on 22 October 2026. Products named here change often, so check availability, names and settings against the vendor's current documentation before relying on them. This note describes one way to run the model on this toolchain and is not an endorsement.

This note covers Azure Boards, Azure Repos, Azure Pipelines and Azure Artifacts in Azure DevOps, with GitHub Advanced Security for Azure DevOps and the GitHub Copilot features that reach it. It also covers Microsoft Foundry, Microsoft Copilot Studio, Microsoft Agent 365, Microsoft Entra Agent ID, Microsoft Purview, Microsoft Defender for Cloud, Azure SRE Agent and agent mode in Visual Studio and Visual Studio Code (VS Code). Azure Pipelines carries steps 5 and 8 to 10 well, because approvals and checks sit on environments outside the pipeline file. Entra Agent ID, Agent 365 and Purview can give each agent a governed identity, an inventory entry and an audit record. The gaps are at steps 4 and 7. The GitHub Copilot cloud agent works only with GitHub repositories, and Azure Pipelines has no native attestation feature.

Microsoft recommends GitHub repositories with Azure Boards and Azure Pipelines for the GitHub-only Copilot capabilities. Where you run Microsoft with GitHub, [GitHub](GITHUB-ADM.md) covers the repository, the pull request and the coding agent. This note covers the layer around them. Both platforms say pull request, where some others say merge request. [Anthropic](ANTHROPIC-ADM.md) and [OpenAI](OPENAI-ADM.md) cover the model providers, and [MODEL.md](../MODEL.md) holds the flow.

## 1. Where the model's elements live

| Model element | Where it lives on Microsoft |
|---|---|
| The request (2.2) | A custom Request work item type in Azure Boards, whose outcome states only the product owner group can set |
| The intent (2.1 and appendix A) and its published version | A custom Intent work item type holds the working copy, and a pull request merges the published version into the leading context's repository |
| The work item | The Azure Boards work item, linked to branches, pull requests and builds |
| Repository instructions | `AGENTS.md` and `.github/copilot-instructions.md`, with Copilot code review in Azure Repos reading the latter or `.azuredevops/copilot-instructions.md` |
| Path-specific instructions | `*.instructions.md` files with an `applyTo` pattern in `.github/instructions/` or `.azuredevops/instructions/` |
| Standards as retrievable context | A standards repository read in read-only mode through the Azure DevOps Remote MCP Server, a hosted Model Context Protocol (MCP) server |
| The agent profile (appendix C) | A versioned file in the platform repository, partly implemented by a Foundry hosted agent version or a GitHub custom agent file |
| The context profile (appendix B) | A document in the platform repository, implemented by required YAML templates and environment checks |
| The tool allow-list | `allowed_tools` on Foundry MCP tools, Remote MCP Server toolsets, VS Code enterprise policies and the GitHub Copilot MCP allow-list for Visual Studio |
| The run identity | A job access token that lasts one job, and an Entra agent identity for each agent |
| The workspace (step 3) | A branch and a fresh virtual machine (VM) per job, with environments created from Bicep or Terraform definitions |
| The pipeline and deterministic checks (step 5) | YAML pipelines extending required templates, with build validation and status check branch policies |
| The release candidate and its attestation (step 7) | A package version in Azure Artifacts or an image digest in Azure Container Registry signed with Notation, with a provenance record the pipeline writes and signs |
| Environments and promotion (step 8) | Azure Pipelines environments with approvals and checks, and feed views for packages |
| The authorisation record (step 9) | The production environment's approval, with self-approval barred, and a change record check |
| The release manifest | A pipeline artefact assembled by a pipeline job, as no product object exists |
| The decision trail (8.1) | Work item links, pull requests, pipeline runs, deployment history, the audit stream, Entra sign-in logs and Purview audit |
| Telemetry for step 11 | Azure Monitor and Application Insights, read by Azure SRE Agent |

## 2. Readiness against the eleven steps

The Supports up to column is a judgement from the documented capabilities: the highest level under 5.1 an agent can hold at the step with the nine controls of 5.3 in place. The model's starting levels are in 3.1, subject to the evaluation under 5.4.

| Step | What Microsoft provides | Supports up to | Gap or complement |
|---|---|---|---|
| 1 Intent | A Foundry hosted agent drafts with read-only retrieval, and a pipeline job writes the draft to the work item | L2 | The Intent type, rules and publishing pipeline are the adopter's build |
| 2 Gate | The drafting agent screens triggers and assembles material, posted as a comment by the pipeline | L1 | Gate decisions go to a decision index in the standards repository |
| 3 Workspace | Pipeline jobs create branches and environments from versioned definitions on fresh VMs | L0 by design | Masked data refresh is an adopter job. Azure Deployment Environments retires on 22 February 2027 |
| 4 Build | Azure Boards starts the Copilot cloud agent with a chosen custom agent and model, for GitHub repositories only | L2 | The Boards integration sends the work item's text and comments rather than the published intent |
| 5 Verify | Build validation, CodeQL, dependency and secret scanning and the Advanced Security status checks | L3 | Copilot Autofix is in limited public preview |
| 6 Review | Copilot code review for Azure Repos, in public preview, leaves a Comment review, not an approval | L1 | Its model is undocumented, so a named pairing needs a separate reviewing agent |
| 7 Identify | The pipeline builds once, publishes, signs with Notation and attaches a software bill of materials | L0 by design | With no native attestation, the pipeline signs its own provenance record |
| 8 Promote | Environments with approvals and checks, required templates, exclusive locks and feed view promotion | L3 | The Evaluate artifact check covers container images only |
| 9 Authorise | The production approval and a change record check, with an agent computing readiness | L3 | The manifest is the adopter's own job |
| 10 Deploy | The authorised run deploys through a guarded production service connection | L0 by design | A resource administrator can bypass approvals, and the bypass is recorded |
| 11 Confirm and learn | Azure Monitor, Application Insights and Azure SRE Agent investigation | L2 | New SRE Agent response plans default to Autonomous |

The toolchain is strongest at steps 5, 8, 9 and 10, where resource owners hold the checks and a job token lasts one job. It is weakest at step 4 on Azure Repos and at step 7, where the adopter supplies the coding agent and the provenance record.

## 3. Controls and trust boundaries

| Control | How Microsoft supports it | Gap |
|---|---|---|
| 1. Never the sole authoriser | Branch policies can bar requestors and the most recent pusher from approving, and environment approvals can bar self-approval | A group approval needs one member, so approver groups contain no agent identity |
| 2. No self-attestation | Resource owners hold the checks, outside the YAML file | Bypass permissions and resource administration stay off agent identities |
| 3. Published promotion path only | Required template and branch control checks on production service connections and environments | Resource administrators can bypass approvals and some checks |
| 4. Logged under a run identity | Entra sign-in and audit logs for agents, Azure DevOps auditing and Purview audit of agent interactions | Azure DevOps auditing is in preview, off by default and kept 90 days unless streamed |
| 5. Manual route | Manually queued runs through the same environments, checks and approvals | The fallback credential is the adopter's design |
| 6. Hosting meets the data position | Foundry deployment types, the approved-models Azure Policy definitions and Copilot Studio external model controls | Copilot code review processing does not follow the Azure DevOps organisation's geography |
| 7. Implementer does not review | Azure Boards and Foundry let the adopter name the implementing model | Copilot code review names no model and SRE Agent routes models itself, so a pinned model needs a Foundry deployment |
| 8. Approved sources only | Code review reads instruction files from the target branch only, and Foundry guidance treats tool output as untrusted | The Boards integration passes work item comments to the agent |
| 9. No data or secrets out | Secret push protection and Purview data loss prevention for agent interactions | Masking checks on production-derived data are the adopter's jobs |

| Boundary (5.5) | What holds it on Microsoft |
|---|---|
| 1. The organisation and its supplier services | The Azure DevOps organisation's region and the supplier terms, with Copilot code review following GitHub Copilot data residency |
| 2. The agent supplier and the model providers | Foundry deployment types, the approved-models policy and the hosting option for Claude models |
| 3. The run and the systems it reaches | Job authorisation scope, read-only Remote MCP Server access and Conditional Access for agent identities |
| 4. Retrieved content and the agent's instructions | Instruction files from the protected branch and the published intent, with Foundry agent guardrails and Defender threat protection in preview |
| 5. A controlled environment and the repository | Secret push protection, Purview data loss prevention and the platform's masking jobs |
| 6. Non-production and production | A separate production environment and service connection, approved by a person and used only by the authorised run |

## 4. Configuration pattern

### 4.1 Intents, standards and context

The platform owner adds Request and Intent work item types to an inherited process, the Intent type carrying the elements of appendix A as fields. Rules restrict each state transition to the group holding that decision and make the fields read-only once published.

Publication is a pipeline job, triggered when a service hook on the Work item updated event, filtered on State, calls an incoming webhook. The job writes the intent to a path such as `intents/<key>/intent.md` in the leading context's repository and raises a pull request. An Automatically included reviewers policy on that path requires the context's code owners.

Standards live in a repository with its own code owners, and instruction files point to them rather than copying them. Agent runs read standards and work items through the Azure DevOps Remote MCP Server, with the `X-MCP-Toolsets` header limiting the tools and `X-MCP-Readonly` set to true.

### 4.2 Agents, permissions and run identities

| Agent profile | Step and class | Level | How Microsoft enforces it |
|---|---|---|---|
| Intent drafter | 1, every class | L2 | A Foundry hosted agent holding its own Entra agent identity and read-only retrieval, with a pipeline job writing the draft |
| Gate screener | 2, every class | L1 | The drafter's read-only identity, with the pipeline posting the screening |
| Builder | 4, evaluated low and standard tier classes | L2 | On GitHub repositories, Azure Boards starts the Copilot cloud agent. On Azure Repos, a pipeline job builds, its identity holding Contribute and Create branch on one repository |
| Check triage | 5, every class | L2, and L3 to queue the pipeline | Queue builds permission only, without rights to edit or bypass policies |
| Reviewer | 6, every class | L1 | Copilot code review, or a Foundry agent on a different model whose comments the pipeline posts |
| Promoter | 8, classes with an approved job | L3 | An identity that queues the approved non-production job, with environment checks deciding |
| Readiness assembler | 9, every class | L3 | Read access to runs and the change record |
| Operations investigator | 11, every class | L2 | Azure SRE Agent with a Reader role on production and Review mode on every response plan |

Retrieval, commenting and execution are separate grants. Retrieval is repository Read and the Azure Reader role. Commenting is the Contribute to pull requests permission alone. With Strict Vote Mode on, voting needs Contribute, so an advising identity can comment but cannot vote. Execution goes only to the run identity that needs it.

Job authorisation scope limits each pipeline identity to one project, and service connections can use Entra workload identities instead of stored secrets. Entra Agent ID gives each Foundry hosted agent and Copilot Studio agent its own identity, and it documents an ephemeral identity that lasts one task. On a reduction under 5.4, the platform owner disables the affected identities and agent pipelines at once.

### 4.3 From verification to deployment

At step 5, Verify, a build validation policy runs the shared pipeline on every pull request. The pipeline extends a required template holding the tests, the scans and the thresholds of the tiering standard. At step 6, Review, Copilot code review comments first. An Automatically included reviewers policy then requires a code owner group containing no agent identity.

At step 7, Identify, the pipeline checks the exact merged revision and builds once. A package goes to the @local view of an Azure Artifacts feed. An image goes to Azure Container Registry by digest, signed by the Notation task with an Azure Key Vault key. The job writes a signed provenance record binding the commit, the check results, the approvals and any agent profile version and run identity.

At step 8, Promote, the same candidate moves through the non-production environments, with packages promoted to the @prerelease and @release views. At step 9, Authorise, a readiness job assembles the release manifest and the change record check confirms the change authorisation. The release authority then approves the production environment, under a check barring self-approval. At step 10, Deploy, the approved stage runs with no further approval.

An artefact-based context promotes a package version or image digest unchanged. A state-based context keeps Bicep or Terraform definitions in version control, and its pipeline previews the change before applying it through one service connection. A migration-based context runs an ordered migration set with a dry run in pre-production, where the recovery position is exercised before authorisation.

### 4.4 One change through the flow

1. **Intent.** The product owner approves a request to add a delivery date to the order service's response. The intent drafter drafts the intent and the engineer states the standard tier. The business analyst validates, and the intent architect publishes through a pull request the code owners approve.
2. **Gate.** The gate screener finds no trigger, so the merged intent passes to step 3.
3. **Workspace.** The pipeline creates a branch and a disposable test environment from versioned definitions.
4. **Build.** A pipeline job runs the builder, pushes the branch and raises a pull request carrying the work item key.
5. **Verify.** Build validation runs the shared template, and the Advanced Security status check blocks new high or critical alerts.
6. **Review.** Copilot code review comments, then a code owner who did not produce the change approves and completes the pull request.
7. **Identify.** The pipeline builds the merged revision once, publishes the package and signs the provenance record.
8. **Promote.** The release candidate moves through test and pre-production, with checks deciding each stage.
9. **Authorise.** The readiness job assembles the manifest, the change record check passes and the release authority approves.
10. **Deploy.** The approved stage deploys the same package version.
11. **Confirm and learn.** Health checks run, Azure SRE Agent watches in Review mode and the product owner accepts against the acceptance examples.

## 5. Gaps and complementary tooling

**A coding agent for Azure Repos.** The cloud agent works only with GitHub repositories, and an Azure DevOps product manager stated in October 2026 that there are no plans to bring it to Azure Repos. An adopter on Azure Repos runs a coding agent as a Foundry hosted agent or as a pipeline job, a pattern Microsoft documents for the Modernize CLI.

**Attestation and the release manifest.** Azure Pipelines records runs, approvals and deployments but produces neither a signed attestation nor a release manifest. Shared templates generate both, and an Invoke REST API check can verify the attestation before production.

## 6. Decisions for adopters

1. Whether source stays in Azure Repos, moves to GitHub or follows the hybrid keeping Azure Boards and Azure Pipelines.
2. The permitted model list, enforced by the approved-models Azure Policy definitions, and each model's Foundry deployment type.
3. The hosting option for Claude models in Foundry, and the providers allowed in Copilot Studio and Azure SRE Agent.
4. The Intent work item fields and the published file path in each leading context.
5. One Entra agent identity per agent profile, or an ephemeral identity per run.
6. Whether agent-initiated pipeline jobs are allowed, and at what steps.
7. Whether Copilot code review is enabled, given its processing location.
8. The stream target and retention period for audit events.
9. The run mode for each Azure SRE Agent response plan.

## Sources

- [Use GitHub Copilot with Azure Boards](https://learn.microsoft.com/azure/devops/boards/github/work-item-integration-github-copilot?view=azure-devops)
- [Rules and rule evaluation](https://learn.microsoft.com/azure/devops/organizations/settings/work/rule-reference?view=azure-devops)
- [Service hook events](https://learn.microsoft.com/azure/devops/service-hooks/events?view=azure-devops)
- [resources.webhooks.webhook definition](https://learn.microsoft.com/azure/devops/pipelines/yaml-schema/resources-webhooks-webhook?view=azure-pipelines)
- [Azure DevOps released features](https://learn.microsoft.com/azure/devops/release-notes/features-timeline-released)
- [Get started with Copilot code review for pull requests](https://learn.microsoft.com/en-us/azure/devops/repos/git/copilot-code-reviews)
- [Configure Copilot code review instructions](https://learn.microsoft.com/en-us/azure/devops/repos/git/configure-copilot-code-review-instructions?view=azure-devops)
- [Troubleshoot Copilot code review](https://learn.microsoft.com/en-us/azure/devops/repos/git/copilot-code-reviews-faq?view=azure-devops)
- [Updates to Copilot Code Reviews for Azure Repos](https://devblogs.microsoft.com/devops/updates-to-copilot-code-reviews-for-azure-repos)
- [Set up the remote Azure DevOps MCP Server](https://learn.microsoft.com/en-us/azure/devops/mcp-server/remote-mcp-server)
- [Define approvals and checks](https://learn.microsoft.com/azure/devops/pipelines/process/approvals?view=azure-devops)
- [Set and manage branch policies](https://learn.microsoft.com/azure/devops/repos/git/branch-policies?view=azure-devops)
- [Set Git repository settings and policies](https://learn.microsoft.com/azure/devops/repos/git/repository-settings?view=azure-devops)
- [Security groups, service accounts, and permissions reference](https://learn.microsoft.com/azure/devops/organizations/security/permissions?view=azure-devops)
- [Configure GitHub Advanced Security for Azure DevOps](https://learn.microsoft.com/azure/devops/repos/security/configure-github-advanced-security-features?view=azure-devops)
- [Access, export and filter audit log](https://learn.microsoft.com/azure/devops/organizations/audit/azure-devops-auditing?view=azure-devops)
- [Microsoft-hosted agents](https://learn.microsoft.com/azure/devops/pipelines/agents/hosted?view=azure-devops)
- [Azure DevOps Server 2020 Update 1 release notes](https://learn.microsoft.com/azure/devops/server/release-notes/azuredevops2020u1?view=azure-devops)
- [Azure DevOps Sprint 275 update](https://learn.microsoft.com/azure/devops/release-notes/2026/sprint-275-update)
- [What are feed views?](https://learn.microsoft.com/azure/devops/artifacts/concepts/views?view=azure-devops)
- [Azure Deployment Environments retirement guide](https://learn.microsoft.com/azure/deployment-environments/deployment-environments-retirement-guide)
- [Sign and verify a container image with Notation in Azure Pipeline](https://learn.microsoft.com/azure/security/container-secure-supply-chain/articles/notation-ado-task-sign)
- [Implement artifact signing and build provenance](https://microsoft.github.io/zerotrustassessment/docs/workshop-guidance/devsecops/DS_023)
- [CI/CD integration with Modernize CLI](https://learn.microsoft.com/azure/developer/github-copilot-app-modernization/modernization-agent/cicd-integration)
- [Getting the most out of Azure DevOps and GitHub](https://devblogs.microsoft.com/devops/getting-the-most-out-of-azure-devops-and-github/)
- [Overview of Copilot cloud agent](https://docs.github.com/en/copilot/concepts/agents/cloud-agent/about-cloud-agent)
- [What's new in Copilot Studio](https://learn.microsoft.com/microsoft-copilot-studio/whats-new)
- [Overview of Microsoft Agent 365](https://learn.microsoft.com/microsoft-agent-365/overview)
- [Microsoft Entra Agent ID design patterns](https://learn.microsoft.com/entra/agent-id/concept-agent-id-design-patterns)
- [Use Microsoft Purview to manage data security and compliance for Microsoft Agent 365](https://learn.microsoft.com/en-us/purview/ai-agent-365)
- [What's new in Defender for Cloud features](https://learn.microsoft.com/azure/defender-for-cloud/release-notes)
- [Run modes in Azure SRE Agent](https://learn.microsoft.com/azure/sre-agent/run-modes)
- [Manage approvals and permissions in VS Code](https://code.visualstudio.com/docs/agents/run/approvals)
- [Use custom instructions in VS Code](https://code.visualstudio.com/docs/agent-customization/custom-instructions)
- [Admin controls for GitHub Copilot in Visual Studio](https://learn.microsoft.com/visualstudio/ide/visual-studio-github-copilot-admin?view=visualstudio)
- [What are hosted agents?](https://learn.microsoft.com/azure/foundry/agents/concepts/hosted-agents)
- [Connect agents to Model Context Protocol servers](https://learn.microsoft.com/azure/foundry/agents/how-to/tools/model-context-protocol)
- [Guardrails and controls overview in Microsoft Foundry](https://learn.microsoft.com/azure/foundry/guardrails/guardrails-overview)
- [Built-in policies for model deployment in Microsoft Foundry portal](https://learn.microsoft.com/azure/foundry/how-to/model-deployment-policy)
- [Deployment types for Microsoft Foundry Models](https://learn.microsoft.com/azure/foundry/foundry-models/concepts/deployment-types)
- [Compare hosting options for Claude models in Microsoft Foundry](https://learn.microsoft.com/azure/foundry/foundry-models/concepts/claude-models-hosting-comparison)
