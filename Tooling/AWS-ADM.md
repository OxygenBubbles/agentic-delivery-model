# AWS Ecosystem Readiness for the ADM

> **Alpha.** This tooling guidance reflects the current state of the AWS ecosystem and will be updated as tooling matures and practical experience grows.

This document assesses how the AWS enterprise ecosystem can support the Agentic Delivery Model across the full delivery lifecycle: requirements, execution, governance, release and operations. It identifies where the ecosystem is ready, where gaps exist and where complementary tooling may be needed.

It is a readiness assessment, not a product endorsement. Organisations building on the AWS stack should use it to understand what is available today and where to plan for investment or integration.

AWS has historically been an infrastructure and operational platform rather than a development lifecycle platform. With Kiro it now has an agentic development tool of its own, but it is still typically paired with one of the repository-centred ecosystems assessed in the [GitHub](GITHUB-ADM.md), [GitLab](GITLAB-ADM.md) or [Atlassian](ATLASSIAN-ADM.md) companion notes for source control and pull request governance.

## 1. The ADM Delivery Lifecycle

The ADM needs tooling support across five areas:

1. **Requirements and intent design.** Capturing business intent, running structured discovery, producing Intent Specifications.
2. **Agent execution.** Autonomous agents implementing, testing and documenting code changes against approved specifications.
3. **Governance and human review.** Pull request review, verification, validation and approval by people at the Governance Tier.
4. **Identity, compliance and audit.** Named agent identities, scoped permissions, provenance trails and regulatory evidence.
5. **Release, operations and feedback.** CI/CD pipelines, environment management, monitoring and operational feedback loops.

The assessment below maps AWS capabilities against each area.

## 2. Requirements and Intent Design

**Readiness: Limited.**

AWS provides no native business collaboration or requirements tooling. There is no AWS equivalent of Teams, Copilot Studio or Jira. Requirements management in an AWS-centric delivery workflow must rely entirely on external platforms. Most organisations will use Atlassian Jira, GitHub Issues or third-party collaboration platforms like Confluence.

Intent Specifications cannot be captured or managed within AWS itself.

**Gaps.** AWS has no answer for requirements collaboration. Organisations building on AWS must integrate external requirements tooling entirely separately from their infrastructure and delivery platforms. This creates a fragmented tooling experience where business requirements live outside the AWS ecosystem.

## 3. Agent Execution

**Readiness: Strong.**

AWS's primary coding agent is now Kiro, a spec-first agentic development tool that AWS positions as the successor to Amazon Q Developer. Q Developer is being retired: the newest coding models were removed from Q Developer Pro in May 2026 and end-of-support is set for April 2027. Organisations on Q Developer should plan a migration to Kiro.

Kiro is built around specifications. It generates a structured specification from a request, a design from the specification and a task breakdown from the design, then implements against them. Its autonomous agent can:

- take a work item from a backlog tool and implement against the approved specification
- work on an isolated branch and open a pull request
- monitor CI results and fix failures without a person re-prompting
- pick up reviewer feedback and push updates
- run headless in CI/CD against GitHub or GitLab

This is a closer fit to the ADM execution model than Q Developer was. Kiro's specification, design and task structure maps almost one to one onto the Intent Specification, and its autonomous agent produces merge-ready pull requests rather than IDE-level assistance. For AWS-native work, Kiro retains strong context on services such as Lambda, DynamoDB and CloudFormation.

**Gaps.** Kiro is newer and its autonomous capability should be evaluated against the organisation's specific delivery patterns before it becomes the primary execution layer. It also depends on an external repository platform, because AWS has no native repository or pull request surface (CodeCommit is deprecated). The governance surface therefore still sits in GitHub or GitLab, not in AWS.

## 4. Agent Orchestration and Coordination

**Readiness: Limited.**

Kiro's autonomous agent works on one specification at a time. There is no native AWS capability for coordinating a fleet of agents across a delivery backlog.

Amazon Bedrock provides model hosting and the Bedrock Agents capability allows organisations to build custom agents with tool access. This is a lower-level building block rather than a delivery orchestration surface. An organisation could theoretically build a Scheduling Agent on Bedrock, but this requires significant custom development and is not a pre-built capability.

**Gaps.** AWS provides building blocks (Bedrock, Lambda, Step Functions) that could support custom agent orchestration, but no pre-built delivery orchestration exists. There is no Scheduling Agent, no fleet management and no agent-to-agent communication pattern out of the box. Organisations must build orchestration from primitives.

## 5. Governance and Human Review

**Readiness: Partial.**

The ADM's governance surface depends critically on the repository platform the organisation chooses. AWS does not own the repository layer. CodeCommit (AWS's native Git service) is deprecated. Most organisations will pair AWS infrastructure with GitHub, GitLab or Bitbucket for source control and pull request management.

Kiro can open and update pull requests, but the pull request governance surface itself sits entirely outside AWS. Once a repository platform is chosen, pull request review, approval rules, branch protections and status checks follow that platform's capabilities, not AWS.

**Gaps.** AWS has no repository platform. Governance depends completely on whichever external repository platform the organisation uses. There is no native AWS pull request or branch protection surface. Kiro's pull requests are reviewed and governed on the external repository platform, not in AWS. Organisations must integrate governance across two separate platforms: the external repository platform and AWS infrastructure.

## 6. Identity, Compliance and Audit

**Readiness: Strong.**

AWS IAM (Identity and Access Management) is mature and granular. Agent service principals can be created with fine-grained permissions, resource-based policies and conditional access controls. This maps well to the ADM requirement for named service identities with constrained access.

AWS CloudTrail provides comprehensive audit logging across all AWS services, including agent actions, API calls, console access and policy changes. AWS Config tracks resource configuration changes and compliance posture. AWS Security Hub aggregates security findings and compliance signals. Service Control Policies allow organisations to constrain what agents can do at the account level.

**Gaps.** Provenance across the full lifecycle requires joining audit records from multiple systems. Code committed in an external repository produces no AWS audit entry. A pull request review and approval in GitHub creates no CloudTrail entry. A specification in Jira and a deployment in AWS produce separate audit records. Building a joined-up provenance chain from intent specification to deployed code requires deliberate integration across external platforms and AWS, and may require custom tooling or Athena queries.

## 7. Release, Operations and Feedback

**Readiness: Strong.**

AWS CodePipeline provides CI/CD with approval gates, environment management, orchestration of build and deploy stages and release tracking. AWS CodeBuild supports build automation and testing. These services integrate with external repositories (GitHub, GitLab, Bitbucket, CodeCommit).

AWS provides the infrastructure layer: EC2, Lambda, RDS, networking, monitoring (CloudWatch, X-Ray), alerting and diagnostics. CloudWatch Application Signals provides automated application observability and performance monitoring. Environment management is supported through AWS accounts, VPCs, security groups and deployment pipelines.

AWS AppConfig provides feature flag management with targeting rules, rollout schedules and integration with application code. This supports the ADM's pattern of deploying behind feature flags and activating at the Product Owner's discretion.

**Gaps.** CodePipeline approval gates are suitable for release workflows but are not optimised for the ADM's daily human governance sessions. The ADM requires a consolidated governance view of all pending work (pull requests awaiting review, specifications in progress, blocked items), which requires integration across the external repository platform, AWS CodePipeline and any separate requirements tooling. This consolidated view does not exist out of the box.

## 8. Ecosystem Readiness Summary

| ADM Area | AWS Capability | Readiness | Key Gap |
|---|---|---|---|
| Requirements and intent design | None | Not available | Requires external platform (Jira, Confluence, GitHub Issues) |
| Agent execution (coding) | Kiro (successor to Amazon Q Developer) | Strong | Depends on an external repository platform for pull request governance |
| Agent orchestration | Bedrock Agents (building blocks only) | Limited | No pre-built delivery orchestration. Must build from primitives |
| Governance and human review | External repository platform (GitHub, GitLab, Bitbucket) | Partial | Repository platform sits outside AWS. Amazon Q code reviews are separate from governance controls |
| Identity and compliance | IAM, CloudTrail, Config, Security Hub | Strong | Cross-system provenance chain requires configuration and integration |
| Release and operations | CodePipeline, CodeBuild, CloudWatch, AppConfig | Strong | No consolidated governance view across repository and pipeline |

## 9. Reference Operating Pattern

A practical AWS-centred ADM pattern:

1. Business stakeholders and teams work in external requirements and collaboration tooling (Jira, Confluence, Teams, Slack).
2. Intent Specifications are created in the external requirements platform and approved by the Product Owner and Intent Architect.
3. The approved specification is assigned to Kiro.
4. Kiro implements against the specification on an isolated branch and opens a pull request in the external repository (GitHub or GitLab).
5. Kiro runs builds and tests, monitors CI and fixes failures without re-prompting.
6. Reviewer feedback on the pull request is picked up by Kiro, which pushes updates.
7. Pull requests are raised in the external repository platform for human review at the daily governance session.
8. Repository branch protections, required reviewers and status checks enforce ADM governance controls.
9. On approval, CodePipeline orchestrates CI/CD with approval gates per environment.
10. CloudWatch and X-Ray provide operational feedback and monitoring.
11. AWS IAM and CloudTrail provide audit trails and compliance evidence.

This pattern accepts that requirements, source control and pull request governance live outside AWS, while Amazon Q provides agent execution and AWS provides infrastructure and operational capabilities.

## 10. Key Decision Points for Adopters

Organisations assessing the AWS ecosystem for ADM adoption should consider:

**Requirements and collaboration platform.** AWS provides no native requirements tooling. The organisation must select and integrate an external platform (Atlassian Jira, GitHub Issues, Confluence, Monday.com, Asana or similar). This choice is orthogonal to AWS and should be made on its own merits based on existing team workflows and scale requirements.

**Repository platform.** AWS CodeCommit is deprecated. The organisation must choose GitHub, GitLab, Bitbucket or another Git hosting platform. This choice affects the ADM's governance surface directly. GitHub supports deeper integration with AWS through IAM, though all three platforms work with CodePipeline and CodeBuild.

**Agent execution strategy.** Kiro provides an autonomous, spec-first coding agent that opens and maintains pull requests against GitHub or GitLab, a closer fit to the ADM than the IDE-based Amazon Q Developer it replaces. Organisations on Amazon Q Developer should plan a migration to Kiro, and should still pair it with a repository platform for pull request governance because AWS has no native repository surface.

**Governance surface integration.** The ADM's daily governance sessions require visibility across requirements, pull requests and infrastructure. This consolidated view does not exist out of the box in AWS. Plan for integration work using CloudWatch dashboards, repository APIs or custom tooling to join requirements status, pull request queues and deployment status into a single governance interface.

**Provenance and audit.** AWS provides strong audit foundations (CloudTrail, IAM, Security Hub), but organisations must design the integration with external requirements and repository platforms to build a complete provenance chain. This is essential for regulated industries and should be planned early in the adoption cycle.

**Cost and model governance.** AWS Cost Explorer and Bedrock usage tracking provide granular cost data for infrastructure and model API usage. However, attributing cost to individual Intent Specifications requires custom tagging and reporting. AWS tagging strategy should include specification identifiers so that agent compute, model API and infrastructure costs can be traced to individual delivery items.

## 11. Source Notes

This assessment is based on public product documentation from:

- [Kiro](https://kiro.dev/)
- [Kiro documentation (AWS)](https://aws.amazon.com/documentation-overview/kiro/)
- [Amazon Q Developer](https://aws.amazon.com/q/developer/) (being retired in favour of Kiro)
- [AWS IAM documentation](https://docs.aws.amazon.com/iam/)
- [AWS CloudTrail](https://docs.aws.amazon.com/cloudtrail/)
- [AWS CodePipeline](https://docs.aws.amazon.com/codepipeline/)
- [AWS CodeBuild](https://docs.aws.amazon.com/codebuild/)
- [AWS AppConfig feature flags](https://docs.aws.amazon.com/appconfig/)
- [Amazon Bedrock](https://aws.amazon.com/bedrock/)
- [AWS CloudWatch and monitoring](https://docs.aws.amazon.com/cloudwatch/)

AWS product capabilities are evolving. This assessment should be revisited as the ecosystem matures.