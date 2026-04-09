# How GitLab Duo Could Support the ADM

> **Alpha.** This tooling guidance reflects the current state of the GitLab ecosystem and will be updated as tooling matures and practical experience grows.

This note sets out how GitLab Duo could support the Agentic Delivery Model in an enterprise software delivery context.

It is a practical implementation note, not a product endorsement. It focuses on enterprise software development and DevSecOps workflow.

## 1. Scope and caveat

GitLab is a credible enterprise implementation path for the ADM because it combines source control, merge requests, CI/CD, security workflow and agentic capability in one platform.

GitLab describes the Duo Agent Platform as an AI-native solution that embeds multiple agents across the software development lifecycle. It supports foundational agents, custom agents, external agents, flows, agentic chat and self-managed options.

This makes GitLab especially relevant for enterprises that want a more unified development and governance surface.

## 2. Where GitLab fits in the ADM

GitLab is strongest in:

1. integrated software lifecycle context
2. agentic execution and workflow orchestration
3. CI/CD and security integration
4. self-managed or controlled enterprise deployment options

That maps well to both the Execution Tier and the Governance Tier of the ADM.

## 3. A practical role for the Duo Agent Platform

GitLab Duo Agent Platform supports multiple agents across the software development lifecycle, including foundational agents, custom agents and external agents. GitLab also supports flows, which are designed to make multiple agents work together on complex tasks.

In ADM terms, that makes GitLab well suited to:

- issue triage and backlog-linked delivery flow
- implementation, testing and documentation work
- vulnerability and CI/CD remediation
- multi-step software delivery workflows with shared context

GitLab Duo CLI also brings agentic behaviour into the terminal, which can support engineering teams that want terminal-based interaction without leaving the GitLab ecosystem.

## 4. Enterprise strengths for the ADM

For the ADM, GitLab is especially strong where the organisation wants:

- backlog, code, CI/CD and security context in one platform
- merge request-centred governance
- agentic workflows connected to DevSecOps controls
- self-managed, dedicated or tightly controlled deployment options

This is particularly relevant in regulated environments and in organisations that prefer a more consolidated lifecycle platform.

## 5. Where GitLab needs complements

GitLab is strong inside the software delivery lifecycle, but it does not remove the need for wider enterprise operating choices around:

- stakeholder discovery and business workflow outside the engineering platform
- model governance across multiple providers
- enterprise accountability above the delivery team

Even in a GitLab-centred estate, the ADM still depends on Product Owners, Intent Architects, Verification Engineers and System Stewards doing their human work well.

## 6. Reference operating pattern

A practical GitLab-centred ADM pattern could look like this:

1. Intent Specifications are approved and linked to GitLab work items.
2. GitLab Duo Agent Platform agents work against approved delivery items.
3. Duo Chat, agents and flows support implementation, testing, documentation and vulnerability work.
4. Merge requests provide the main human review and approval surface.
5. CI/CD and security controls act as enterprise guardrails inside the same platform.
6. Self-managed or dedicated deployment options support enterprise control requirements where needed.

This makes GitLab one of the strongest integrated non-Microsoft paths for the ADM.

## 7. Recommended position in the ADM repository

The ADM should present GitLab Duo as a strong enterprise scenario for organisations that want integrated lifecycle context, strong DevSecOps alignment and the option of self-managed deployment.

It is one of the clearest non-Microsoft examples of an enterprise development stack that can support a large part of the ADM in one platform.

## 8. Source notes

This note is based on public GitLab documentation, including:

- [GitLab Duo Agent Platform](https://docs.gitlab.com/user/duo_agent_platform/)
- [Agents](https://docs.gitlab.com/user/duo_agent_platform/agents/)
- [Get started with the GitLab Duo Agent Platform](https://docs.gitlab.com/user/get_started/get_started_agent_platform/)
- [GitLab Duo CLI](https://docs.gitlab.com/user/gitlab_duo_cli/)

GitLab product scope and terminology are evolving quickly. This note should be updated as the platform matures.