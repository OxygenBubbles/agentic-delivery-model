# AWS: running the Agentic Delivery Model

> Checked against vendor documentation on 22 October 2026. Products named here change often, so check availability, names and settings against the vendor's current documentation before relying on them. This note describes one way to run the model on this toolchain and is not an endorsement.

This note covers Kiro (the Kiro IDE, the Kiro CLI and Kiro Web), Amazon Bedrock and Amazon Bedrock AgentCore. It also covers AWS CodePipeline, AWS CodeBuild, AWS CodeDeploy, AWS CodeCommit, AWS Signer, AWS CloudTrail, Amazon CloudWatch, IAM, AWS DevOps Agent, AWS Continuum and AWS Transform. Steps 5 and 7 to 10 run well, because accounts, IAM and service control policies hold the controls outside any agent's reach and CodePipeline stops for a person before production. Kiro drafts at step 1 and builds at step 4. AWS offers new customers no work management product and has no release manifest, so the adopter supplies both.

Amazon Q Developer's IDE plugins and paid subscriptions reach end of support on 30 April 2027, and AWS directs IDE users to Kiro. Many AWS adopters keep code in GitHub or GitLab, so review follows [GitHub](GITHUB-ADM.md) or [GitLab](GITLAB-ADM.md), and GitLab calls a pull request a merge request. Work items usually sit in the tools those notes and [Atlassian](ATLASSIAN-ADM.md) cover. Claude models are a hosting option on Bedrock, and [Anthropic](ANTHROPIC-ADM.md) covers Claude Code. Read the note beside [MODEL.md](../MODEL.md) for the flow and the controls.

## 1. Where the model's elements live

| Model element | Where it lives on AWS |
|---|---|
| The request (2.2) | No AWS product. The adopter's work management system holds it |
| The intent (2.1 and appendix A) and its published version | The working copy on the work item, published as a Markdown file merged by pull request into the leading context's repository, with a Kiro spec beside it in `.kiro/` |
| The work item | The work management system's record, linked to branch and pull request by key |
| Repository instructions | A steering file with `inclusion: always` in `.kiro/steering/`, or `AGENTS.md` at the root |
| Path-specific instructions | `AGENTS.md` files in subdirectories and steering files with `inclusion: fileMatch` and a `fileMatchPattern` |
| Standards as retrievable context | File references in steering, `inclusion: auto` steering or a read-only MCP server in the Kiro MCP registry |
| The agent profile (appendix C) | A versioned record in the platform repository, implemented by a Kiro custom agent file in `.kiro/agents/` naming its model and tools |
| The context profile (appendix B) | A document in the platform repository, implemented by the CodePipeline definition held as code |
| The tool allow-list | The Kiro MCP registry and `managed-settings.json` deny rules, AgentCore Gateway with Policy and IAM policies for AWS APIs |
| The run identity | An IAM role session per run: the CodeBuild role for a pipeline job, the role Kiro Web assumes per task or an AgentCore Identity workload identity |
| The workspace (step 3) | A branch and a fresh CodeBuild environment or Kiro cloud sandbox, with accounts and stacks created from versioned CloudFormation definitions |
| The pipeline and deterministic checks (step 5) | CodePipeline with CodeBuild actions and stage conditions |
| The release candidate and its attestation (step 7) | An Amazon ECR image digest signed by ECR managed signing, or a Lambda package signed by AWS Signer, with the pipeline's signed record of the checks |
| Environments and promotion (step 8) | An AWS account per environment class, with CodePipeline stages promoting through CodeDeploy or CloudFormation change sets |
| The authorisation record (step 9) | A CodePipeline manual approval action before the production stage, with the change record kept outside AWS |
| The release manifest | No product object. A CodeBuild job assembles it from the execution history, the approvals and the change record |
| The decision trail (8.1) | Pull request records, CodePipeline execution history, CloudTrail trails, Kiro prompt logs in Amazon S3, Bedrock invocation logs and AgentCore traces |
| Telemetry for step 11 | CloudWatch alarms, Application Signals, CloudWatch investigations and AWS DevOps Agent |

## 2. Readiness against the eleven steps

The column headed Supports up to is a judgement from the documented capabilities: the highest level under 5.1 that an agent can hold at that step with the nine controls of 5.3 in place. It is not a recommended starting point. The model's starting levels are in 3.1, and each takes effect only after the evaluation and approval under 5.4.

| Step | What AWS provides | Supports up to | Gap or complement |
|---|---|---|---|
| 1 Intent | Headless Kiro CLI in CodeBuild drafts a spec of requirements, design and tasks from the request | L2 | A pipeline job writes each draft to the work item, and a Kiro spec lacks several elements of 2.1 |
| 2 Gate | A read-only headless Kiro run identifies the triggers met. AWS Continuum threat modelling is in preview | L1 | Gate decisions need a home, such as a decision index |
| 3 Workspace | Fresh CodeBuild environments, a sandbox per cloud session and accounts created from versioned definitions | L0 by design | Masked data refresh is the adopter's pipeline job |
| 4 Build | Headless Kiro CLI in CodeBuild, the Kiro IDE or Kiro Web autonomous mode, opening pull requests. AWS Transform custom runs upgrades | L2 | Kiro Web runs only in US East (N. Virginia) and does not apply the organisation's MCP, model and web tool settings |
| 5 Verify | CodeBuild runs the checks under the pipeline's role, and stage conditions fail a stage when a rule fails | L3 | No run identity holds stage retry or condition override |
| 6 Review | Pull request review on the repository platform, with a read-only Kiro reviewer on another model commenting | L1 | Continuum code review and the DevOps Agent release readiness review are in preview |
| 7 Identify | CodeBuild builds the merged revision once, and ECR managed signing or AWS Signer signs the artefact | L0 by design | A signature records no check results, so the pipeline signs its own record |
| 8 Promote | CodePipeline deploys the same candidate across non-production accounts through CodeDeploy or CloudFormation | L3 | Environment values come from AWS AppConfig and AWS Secrets Manager |
| 9 Authorise | A manual approval action that only principals granted `codepipeline:PutApprovalResult` on it can approve | L3 | The action does not bar the requester, and no manifest object exists |
| 10 Deploy | The authorised execution deploys under the production deployment role, and CodeDeploy rolls back on failure or an alarm | L0 by design | Changes to the pipeline definition follow the flow |
| 11 Confirm and learn | CloudWatch and AWS DevOps Agent, investigating with a read-only role | L2 | DevOps Agent actions stay disabled until an L4 scope is approved |

The toolchain is strongest from step 5 to step 10, where accounts, IAM-scoped approvals, signing on push and automatic rollback sit outside any run's write scope. It is weakest at steps 1 and 9, where the work item and the manifest sit outside AWS. At step 4, Kiro Web's hosting and governance differ from those of the IDE and CLI.

## 3. Controls and trust boundaries

| Control | How AWS supports it | Gap |
|---|---|---|
| 1. Never the sole authoriser | IAM grants approval per action, and CodeCommit refuses approval by a pull request's author | No agent role holds approval rights in CodePipeline or the repository platform |
| 2. No self-attestation | Checks and signing run under the pipeline's role, apart from every run identity | The Kiro Agent GitHub App requests write access to Actions, Checks and Workflows, so repository rules protect workflow files and required checks |
| 3. Published promotion path only | Production accounts accept only the deployment role, enforced by IAM and service control policies | Kiro Web's IAM role and every agent role carry no production access |
| 4. Logged under a run identity | CloudTrail records each role session and any source identity, and AgentCore sends traces to CloudWatch | Kiro documents prompt logging for the IDE and CLI only, and headless Kiro authenticates with a user's API key |
| 5. Manual route | A person can start an execution, approve or redeploy an earlier CodeDeploy revision | The fallback credential and its monitoring are the adopter's design |
| 6. Hosting meets the data position | Bedrock geographic cross-Region inference, retention modes fixed by service control policy, Kiro profiles in Frankfurt and Kiro's approved model list | Kiro's Auto setting can route outside the approved list unless blocked, and Kiro's OpenAI models use Global routing |
| 7. Implementer does not review | A Kiro custom agent pins its `model`, so the reviewer runs on another | Autonomous mode chooses its own model |
| 8. Approved sources only | Kiro loads a workspace's configuration only once the workspace is trusted, and Bedrock Guardrails detects prompt attacks | A local administrator can circumvent Kiro's client-side policies |
| 9. No data or secrets out | Kiro sandbox network levels, the local command sandbox and short-lived role credentials | Kiro warns that the agent may exfiltrate secrets given to its sandbox |

| Boundary (5.5) | What holds it on AWS |
|---|---|
| 1. The organisation and its supplier services | The Kiro profile Region and the AWS and repository platform terms. Kiro does not use Identity Center users' content for service improvement |
| 2. The agent supplier and the model providers | Bedrock does not share prompts or outputs with model providers or use them to train models, and the retention mode is set per account and Region |
| 3. The run and the systems it reaches | IAM role sessions with session tags, AgentCore Policy, the AWS MCP Server's IAM context keys and Kiro permission rules |
| 4. Retrieved content and the agent's instructions | Workspace trust, steering merged by pull request, the MCP registry and builds that read only the published intent |
| 5. A controlled environment and the repository | Sandbox network levels, builds confined to a VPC, masking jobs and secret scanning in the pipeline |
| 6. Non-production and production | Separate accounts, the approval before the production stage and a deployment role only that stage assumes |

## 4. Configuration pattern

### 4.1 Intents, standards and context

The request, the work item and the intent's working copy live in the adopter's work management system. A pipeline job writes drafts and halt questions to the work item, and the run identity only reads it.

A Kiro spec in the repository's `.kiro/` directory has requirements, design and tasks files, with a bugfix file in place of requirements for a defect. The requirements carry acceptance criteria, the design carries the shape and the tasks carry the execution units. Kiro's specs documentation does not cover the data and security position, the recovery position, the stop conditions or the dependencies. The adopter therefore publishes an intent file from the appendix A template with the Kiro spec in one pull request.

Steering files carry the instructions and standards as section 1 maps them, referencing a published standard, as in `#[[file:standards/api.md]]`, rather than copying it. Custom agents load steering only where their `resources` field lists it. Kiro Memory is off by default and stays off for agents in the flow, because a memory is not an approved source under control 8 in 5.3.

### 4.2 Agents, permissions and run identities

| Step and class of change | Agent and level | How AWS enforces it |
|---|---|---|
| 1, every class | Headless Kiro CLI with a drafting custom agent, L2, Draft | A CodeBuild job trusting only read tools, with the pipeline writing to the work item |
| 2, triggered gates | The same agent, L1, Advise | Read-only tools, with the pipeline posting the material |
| 4, standard-tier application change | Headless Kiro CLI with a build custom agent, L2 | `--sandbox`, a pinned `model` and a Git token that pushes only to a branch |
| 4, dependency upgrade | AWS Transform custom through the `atx` CLI, L2 | A CodeBuild job, with its lessons reviewed before reuse |
| 5, failing check | A headless Kiro triage agent, L2 | Read access to build logs, with the pipeline posting its comment |
| 6, every class | A reviewing custom agent on a different model, L1 | Read-only tools and no approval right |
| 8, non-production promotion | An agent role, L3, Execute under gate | `codepipeline:StartPipelineExecution` on non-production pipelines only |
| 9, readiness | A job computing readiness and the manifest, L3 | No `codepipeline:PutApprovalResult` on any approval action |
| 11, telemetry | AWS DevOps Agent, L2 | The read-only Agent Space role, with agent actions disabled |

Retrieval is a read-only IAM policy and read-only tools, commenting is the pipeline posting an agent's output and execution is a branch push at L2 or a non-production pipeline start at L3. The run identity is the CodeBuild role session for one build, with the Git token the job issues. Headless Kiro also needs `KIRO_API_KEY`, generated by a user once an administrator allows it, so the adopter records the key's owner against each build. Kiro Web assumes a customer IAM role per task, with the user's Kiro ID as source identity and a `KiroSessionId` session tag. Its GitHub App authors pull requests by default, and on GitLab it acts through the user's personal access token.

Kiro enforces its managed permission policy, MCP registry and approved model list in the client. Its documentation says a local administrator can circumvent them, so the ceiling sits in IAM, service control policies, the repository platform and the pipeline. An agent on AgentCore Runtime runs each session in its own microVM, authenticates through AgentCore Identity and reaches tools through AgentCore Gateway, where Policy checks each call against Cedar rules. AWS Agent Registry catalogues approved agents, MCP servers and skills.

### 4.3 From verification to deployment

Each deployment approach has one shared CodePipeline pipeline, defined as code and started by a merge to the protected branch. At step 5, Verify, CodeBuild runs the checks under the pipeline's role and an On Success stage condition fails the stage when a rule fails. No run identity has permission for the RetryStageExecution or OverrideStageCondition operations, so no agent can re-run a check to a pass.

At step 7, Identify, CodeBuild builds the merged revision once. ECR managed signing signs a pushed image through AWS Signer with the pushing identity, so only the pipeline role holds `signer:SignPayload` on the profile. AWS Signer also signs Lambda packages, and a code signing configuration in Enforce mode blocks unsigned ones. The pipeline writes a record binding the digest to the check results, approvals and standards versions, signed with a key only the pipeline role may use.

At step 8, Promote, the same digest moves through the test and pre-production accounts. At step 9, Authorise, a job assembles the manifest and the approval action links to it as the URL for review. The release authority approves through `codepipeline:PutApprovalResult`, granted on that action alone, and an unanswered approval fails after seven days. CodePipeline does not stop a requester approving, so a pipeline check confirms the release authority's independence. At step 10, Deploy, the production stage alone assumes the deployment role.

An artefact-based change promotes a signed image or package by digest, with CodeDeploy rolling back to the last known good revision. A state-based change is a CloudFormation revision whose change set is the plan, with drift-aware change sets and recovery by a change set towards the previous revision. A migration-based change is an ordered migration set that a CodeBuild job runs after a dry run. AWS AppConfig feature flags separate deployment from release and roll back when an alarm fires during their deployment.

### 4.4 One change through the flow

A standard-tier change adds a validation rule to an existing API, with the repository on GitHub.

1. Intent: the product owner approves the request, the drafting agent drafts in CodeBuild and the intent architect publishes the validated intent with its Kiro spec.
2. Gate: the drafting agent finds no trigger met, and the intent architect records the screening.
3. Workspace: the merge starts a job that creates a branch and a fresh CodeBuild environment.
4. Build: the build agent implements the tasks, and the pipeline opens the pull request with the work item key.
5. Verify: one check fails, the triage agent comments a cause and a new run corrects it.
6. Review: a reviewing agent on another model comments, and a code owner who did not produce the change approves and merges.
7. Identify: CodeBuild builds once, ECR managed signing signs the image and the pipeline signs its record of checks.
8. Promote: CodeDeploy deploys the digest to the test and pre-production accounts.
9. Authorise: the regression and end-to-end tests run in pre-production and a job assembles the manifest. With the change authorisation recorded, the release authority approves.
10. Deploy: the production stage deploys the same digest, with CodeDeploy set to roll back on the alarm the manifest names.
11. Confirm and learn: the health checks pass and the product owner accepts against the acceptance examples.

## 5. Gaps and complementary tooling

**Work management and change records.** Amazon CodeCatalyst and AWS Systems Manager Change Manager closed to new customers on 7 November 2025, so the work item and the change record sit in the adopter's own systems. The manifest and the record of checks are the adopter's pipeline jobs.

**Repository.** CodeCommit is open to new customers again with approval rule templates, but Kiro Web works only with GitHub and GitLab, where most adopters keep code.

**Kiro Web.** Kiro Web, generally available since 3 September 2026, runs in US East (N. Virginia) only and does not apply the organisation's MCP, model, web tool or customer managed key settings. Each user sets the sandbox network level, secrets and MCP servers. Automations start runs on a schedule rather than from a published intent. An adopter keeps Cloud Sessions off until these gaps are closed or accepted for a named class of change.

**Logging.** CloudTrail Lake closed to new customers on 31 May 2026, so a new adopter queries trail files in S3 or CloudWatch.

**Kiro Crew.** Kiro Crew, a persistent agent runtime with its own memory, sits outside this pattern.

## 6. Decisions for adopters

1. Which Kiro surfaces the flow uses, and whether Cloud Sessions are on.
2. The permitted model list, including whether Kiro's Auto setting and Global-routed models are allowed.
3. The Bedrock retention mode per account and Region, and whether a service control policy fixes it.
4. Where intents live, and whether a Kiro spec accompanies each one.
5. The repository platform: GitHub, GitLab or CodeCommit.
6. The run identity design, including the owner of each Kiro API key.
7. Whether agents may start non-production pipelines at L3.
8. The allow-list mechanism: the Kiro MCP registry, AgentCore Gateway with Policy or AWS Agent Registry.
9. How the pipeline signs and keeps its record of checks.
10. The logs kept and their retention.
11. Where AWS DevOps Agent, AWS Continuum and AWS Transform sit in the flow.
12. Whether Kiro Memory and learned lessons are permitted.

## Sources

- [Agentic engineering in the cloud](https://kiro.dev/blog/agentic-engineering-in-the-cloud/)
- [Autonomous mode](https://kiro.dev/docs/web/autonomous-mode/)
- [Automations](https://kiro.dev/docs/web/automations/)
- [Cloud sessions](https://kiro.dev/docs/cloud-sessions/)
- [Kiro Web with AWS Identity Center](https://kiro.dev/docs/web/identity-center/)
- [Kiro Web: GitHub](https://kiro.dev/docs/web/github/)
- [Kiro Web: GitLab](https://kiro.dev/docs/web/gitlab/)
- [Kiro Web: Environment configuration](https://kiro.dev/docs/web/sandbox/environment-configuration/)
- [Kiro Web: Environment variables](https://kiro.dev/docs/web/sandbox/environment-variables/)
- [Specs](https://kiro.dev/docs/specs/)
- [Steering](https://kiro.dev/docs/steering/)
- [Permissions](https://kiro.dev/docs/permissions/)
- [Permission policies](https://kiro.dev/docs/enterprise/governance/permissions/)
- [MCP governance](https://kiro.dev/docs/enterprise/governance/mcp/)
- [Custom agents](https://kiro.dev/docs/custom-agents/)
- [Headless mode](https://kiro.dev/docs/cli/headless/)
- [Available models](https://kiro.dev/docs/models/available-models/)
- [Data protection](https://kiro.dev/docs/privacy-and-security/data-protection/)
- [Log user prompts](https://kiro.dev/docs/enterprise/monitor-and-track/prompt-logging/)
- [Memory](https://kiro.dev/docs/memory/)
- [Amazon Q Developer end-of-support announcement](https://aws.amazon.com/blogs/devops/amazon-q-developer-end-of-support-announcement/)
- [What is Amazon Bedrock AgentCore?](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/what-is-bedrock-agentcore.html)
- [How AgentCore Runtime works](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/runtime-how-it-works.html)
- [Release notes for Amazon Bedrock AgentCore](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/release-notes.html)
- [AWS Agent Registry is now generally available](https://aws.amazon.com/about-aws/whats-new/2026/08/aws-agent-registry-generally-available)
- [Amazon Bedrock data retention](https://docs.aws.amazon.com/bedrock/latest/userguide/data-retention.html)
- [Route model inference requests across AWS Regions with cross-Region inference](https://docs.aws.amazon.com/bedrock/latest/userguide/cross-region-inference.html)
- [Detect prompt attacks with Amazon Bedrock Guardrails](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails-prompt-attack.html)
- [Understanding IAM for Managed AWS MCP Servers](https://aws.amazon.com/blogs/security/understanding-iam-for-managed-aws-mcp-servers/)
- [Grant approval permissions to an IAM user in CodePipeline](https://docs.aws.amazon.com/codepipeline/latest/userguide/approvals-iam-permissions.html)
- [Configure conditions for a stage](https://docs.aws.amazon.com/codepipeline/latest/userguide/stage-conditions.html)
- [Redeploy and roll back a deployment with CodeDeploy](https://docs.aws.amazon.com/codedeploy/latest/userguide/deployments-rollback-and-redeploy.html)
- [Amazon ECR managed signing](https://docs.aws.amazon.com/AmazonECR/latest/userguide/managed-signing.html)
- [The Future of AWS CodeCommit](https://aws.amazon.com/blogs/devops/aws-codecommit-returns-to-general-availability/)
- [Learn more about CloudTrail Lake](https://docs.aws.amazon.com/awscloudtrail/latest/partner-onboarding/lake-partner-onboarding-learning.html)- [AWS Service Availability Updates (October 2025)](https://aws.amazon.com/about-aws/whats-new/2025/10/aws-service-availability/)- [Audit trails for autonomous agents with AWS DevOps Agent](https://aws.amazon.com/blogs/devops/audit-trails-for-autonomous-agents-with-aws-devops-agent/)
- [Introducing AWS Continuum for security at machine speed](https://aws.amazon.com/about-aws/whats-new/2026/06/aws-continuum/)
- [AWS Transform custom](https://docs.aws.amazon.com/transform/latest/userguide/custom.html)