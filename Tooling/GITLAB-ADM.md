# GitLab Ecosystem Readiness for the ADM

> **Alpha.** This tooling guidance reflects the current state of the GitLab ecosystem and will be updated as tooling matures and practical experience grows.

This document assesses how the GitLab ecosystem can support the Agentic Delivery Model across the full delivery lifecycle: requirements, execution, governance, release and operations. It identifies where the ecosystem is ready, where gaps exist and where complementary tooling may be needed.

It is a readiness assessment, not a product endorsement. Organisations building on GitLab should use it to understand what is available today and where to plan for investment or integration.

Organisations using GitLab alongside AWS infrastructure should also refer to the [AWS ecosystem assessment](AWS-ADM.md) for the operational and infrastructure layer.

## 1. The ADM Delivery Lifecycle

The ADM needs tooling support across five areas:

1. **Requirements and intent design.** Capturing business intent, running structured discovery, producing Intent Specifications.
2. **Agent execution.** Autonomous agents implementing, testing and documenting code changes against approved specifications.
3. **Governance and human review.** Merge request review, verification, validation and approval by people at the Governance Tier.
4. **Identity, compliance and audit.** Named agent identities, scoped permissions, provenance trails and regulatory evidence.
5. **Release, operations and feedback.** CI/CD pipelines, environment management, monitoring and operational feedback loops.

The assessment below maps GitLab capabilities against each area.

## 2. Requirements and Intent Design

**Readiness: Partial.**

GitLab work items (issues and epics) provide a backlog surface where Intent Specifications can be stored and managed. Merge request-linked discussions and issue comments allow stakeholders to collaborate on specification details.

However, GitLab is not typically where business stakeholder discovery happens. Requirements gathering and stakeholder interviews usually occur outside the engineering platform in email, documentation tools or specialist discovery platforms. A Requirements Agent capability comparable to Copilot Studio does not exist within GitLab. Work items are a good specification repository but do not provide structured discovery facilitation.

**Gaps.** GitLab has no built-in Requirements Agent or structured discovery capability. Requirements must be captured elsewhere and then formalised as work items in GitLab. This separates discovery work from specification management.

## 3. Agent Execution

**Readiness: Strong.**

The GitLab Duo Agent Platform provides multiple agents across the software development lifecycle: foundational agents for common tasks, custom agents for organisation-specific workflows and external agents that integrate third-party capabilities.

These agents can:

- implement code changes against approved Intent Specifications
- run tests and provide test coverage analysis
- identify and remediate security vulnerabilities
- generate documentation from code
- work on isolated branches and raise merge requests for human review

GitLab Flows allow multiple agents to coordinate on complex multi-step delivery tasks. This is especially valuable for the ADM's pattern of orchestrating implementation, testing, security and documentation work within a single specification lifecycle.

GitLab Duo CLI brings agentic capability to the terminal, supporting teams that want agent assistance in command-line workflows.

The maturity of autonomous end-to-end delivery (taking a work item, creating a branch, implementing changes and raising a merge request without human involvement in the implementation step) varies across the Duo Agent Platform. Foundational agents handle common scenarios well. Custom agents require explicit configuration and testing. Organisations should evaluate the current autonomous capability against their specific delivery patterns before committing to GitLab as the primary execution layer.

**Gaps.** Agent capabilities are distributed across the Duo Agent Platform. While foundational agents cover common scenarios, custom agents require explicit build and configuration. There is no pre-built "Scheduling Agent" equivalent that coordinates a fleet of agents across multiple specifications simultaneously.

## 4. Agent Orchestration and Coordination

**Readiness: Partial.**

GitLab Flows provide some coordination capability, allowing multiple agents to work together on complex multi-step tasks within a single specification lifecycle. This is closer to the ADM's orchestration model than most competing platforms.

However, Flows coordinate agents within a single delivery task. There is no pre-built equivalent of the ADM's Scheduling Agent that reads the backlog, detects specification overlap across multiple work items and assigns work to agents proactively. Custom agents can be built to fill this role, but it requires explicit development and configuration.

**Gaps.** GitLab provides task-level orchestration (Flows) but not fleet-level orchestration (a Scheduling Agent managing work distribution across multiple specifications). Organisations wanting the full ADM scheduling pattern will need to build custom agents or integrate an external orchestration layer.

## 5. Governance and Human Review

**Readiness: Strong.**

Merge requests provide the main governance surface. Approval rules, code owners, status checks and protected branch policies are all configurable to enforce ADM governance requirements. Comments and review threads provide a record of human decisions.

Security scanning (SAST, DAST, dependency scanning, container scanning) is built in and can be enforced through merge request rules, creating a unified security and delivery governance interface.

**Gaps.** GitLab does not provide a consolidated governance dashboard out of the box. The ADM's Governance Tier needs visibility of all pending work: merge requests awaiting review, specifications in progress, blocked items and escalations. Most organisations will need to build or configure such a dashboard through GitLab's project-level views, custom dashboards or reporting tools.

## 6. Identity, Compliance and Audit

**Readiness: Strong for self-managed deployments.**

GitLab provides audit events and compliance frameworks. Self-managed GitLab deployments give organisations complete control over authentication, audit logs, data storage and compliance integration. This is especially valuable in regulated environments where data residency and audit requirements are strict.

Agent service principals can be created with scoped permissions. Every agent action is logged with full provenance. SAML and LDAP integration support enterprise single sign-on.

SaaS (gitlab.com) depends on GitLab's infrastructure. For strict regulated environments, self-managed or dedicated GitLab deployments are the appropriate choice.

**Gaps.** SaaS GitLab governance depends on GitLab's infrastructure choices. Organisations requiring complete data control or strict residency requirements must use self-managed deployment, which increases operational overhead. Provenance across the full lifecycle (from specification to deployed code) requires deliberate linking across systems if code is deployed to infrastructure outside GitLab.

## 7. Release, Operations and Feedback

**Readiness: Strong.**

GitLab CI/CD is built in. Pipelines support approval gates, environment management, deployment strategies and release tracking. Feature flags are integrated through the feature flag management interface, supporting the ADM's pattern of deploying behind flags and activating at the Product Owner's discretion.

Environment tracking, deployment history and rollback capability are all native to GitLab. This supports daily release cadence with safety controls at each environment stage.

**Gaps.** GitLab's feature flag capability is functional but basic compared to specialist platforms (LaunchDarkly, Split). Complex multivariate testing or advanced targeting may require a dedicated feature flag service.

## 8. Ecosystem Readiness Summary

| ADM Area | GitLab Capability | Readiness | Key Gap |
|---|---|---|---|
| Requirements and intent design | Work items, issues, epics | Partial | No structured Requirements Agent or discovery facilitation |
| Agent execution | Duo Agent Platform, foundational agents, flows | Strong | Custom agents require explicit build and configuration |
| Agent orchestration | Duo Flows, custom agents | Partial | Task-level orchestration exists. Fleet-level scheduling requires custom build |
| Governance and review | Merge requests, approval rules, security scanning | Strong | No consolidated governance dashboard out of the box |
| Identity and compliance | Audit events, self-managed deployment options, SAML/LDAP | Strong | SaaS has less control. Provenance requires system linking |
| Release and operations | CI/CD pipelines, feature flags, environment management | Strong | Feature flag capability is basic compared to specialist platforms |

## 9. Reference Operating Pattern

A practical GitLab-centred ADM pattern:

1. Intent Specifications are approved and linked to GitLab work items.
2. The work item is assigned to a GitLab Duo agent or agent flow.
3. Agents implement, test, document and scan for security vulnerabilities.
4. Agents raise a merge request with full change context and results.
5. The Verification Engineer reviews the merge request at the daily governance session.
6. Approval rules, code owner reviews and status checks enforce governance controls.
7. Merged code flows through CI/CD pipelines with approval gates per environment.
8. Deployment occurs behind feature flags that are activated by the Product Owner.
9. GitLab audit logs provide full provenance from specification to deployment.
10. Operational feedback from deployed code informs future specifications.

This pattern uses GitLab end to end for delivery execution and governance.

## 10. Key Decision Points for Adopters

Organisations assessing GitLab for ADM adoption should consider:

**Deployment model.** GitLab SaaS (gitlab.com) is simpler to operate but depends on GitLab's infrastructure. Self-managed GitLab provides complete control over data, audit logs and compliance but increases operational overhead. Regulated organisations should evaluate whether self-managed is required before committing to GitLab.

**Requirements and discovery.** GitLab is a specification repository, not a requirements discovery tool. Organisations must plan for where discovery happens (teams, documents, discovery platforms) and how specifications flow into GitLab work items. This may require a separate Requirements Agent capability or manual intake process.

**Agent strategy.** Foundational agents cover common scenarios. Organisations with custom or specialised delivery patterns will need to invest in custom agent builds or integrate external agents. Plan for this capability investment early.

**Governance visibility.** Merge request governance is strong but dashboard-level visibility across all pending work requires configuration or custom tooling. Plan for a governance dashboard early if this is important to the organisation's daily governance sessions.

**Feature flag maturity.** Feature flag capability is built in but basic. Organisations with complex rollout scenarios should evaluate whether a specialist feature flag platform is needed alongside GitLab.

**Cost and model governance.** GitLab provides usage analytics for Duo features, but cost attribution to individual specifications is not built in. The ADM tracks cost per Intent Specification in currency. Organisations will need custom reporting to link agent usage and model costs to work items.

## 11. Source Notes

This assessment is based on public product documentation from:

- [GitLab Duo Agent Platform](https://docs.gitlab.com/user/duo_agent_platform/)
- [Agents](https://docs.gitlab.com/user/duo_agent_platform/agents/)
- [Get started with the GitLab Duo Agent Platform](https://docs.gitlab.com/user/get_started/get_started_agent_platform/)
- [GitLab Duo CLI](https://docs.gitlab.com/user/gitlab_duo_cli/)

GitLab product naming, scope and capabilities are evolving quickly. This assessment should be revisited as the ecosystem matures.