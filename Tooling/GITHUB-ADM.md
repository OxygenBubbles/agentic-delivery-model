# How GitHub Enterprise and GitHub Copilot Could Support the ADM

> **Alpha.** This tooling guidance reflects the current state of the GitHub ecosystem and will be updated as tooling matures and practical experience grows.

This note sets out how GitHub Enterprise and GitHub Copilot could support the Agentic Delivery Model in an enterprise software delivery context.

It is a practical implementation note, not a product endorsement. It focuses on enterprise software development rather than general collaboration tooling.

## 1. Scope and caveat

GitHub is one of the clearest non-Microsoft examples of an enterprise development stack that can support the ADM directly.

GitHub Copilot coding agent is an autonomous and asynchronous software development agent integrated into GitHub. GitHub Enterprise also provides the repository, pull request, policy and audit surface that many ADM controls need.

This makes GitHub particularly strong in the engineering execution layer of the ADM. It is less naturally the place where business requirements discovery, wider stakeholder workflow and enterprise operating governance begin. Most organisations would still pair it with other systems for those parts of the model.

## 2. Where GitHub fits in the ADM

GitHub is strongest in four ADM areas:

1. implementation agents
2. review and pull request workflow
3. repository-level governance and provenance
4. integration with enterprise engineering controls

This makes it a strong anchor for the Execution Tier and an important part of the Governance Tier.

## 3. A practical role for GitHub Copilot coding agent

GitHub Copilot coding agent can be asked to work from issues, GitHub Copilot Chat, the GitHub CLI and agentic coding tools or IDEs with MCP support.

In ADM terms, that makes it a natural candidate for:

- implementation against approved Intent Specifications
- changes on isolated branches
- pull request creation for human review
- iterative rework in response to review comments

GitHub also provides session tracking and pull request-centred review flow, which fit well with ADM provenance and human verification.

## 4. Enterprise controls that matter for the ADM

GitHub's enterprise controls are especially relevant because the ADM depends on constrained execution rather than unrestricted autonomy.

GitHub documents several built-in mitigations for Copilot coding agent, including:

- only users with write access can trigger the agent
- pushes are limited to agent branches rather than protected branches
- workflows do not run until a human approves them

These are exactly the kinds of control points the ADM expects in enterprise software delivery.

## 5. Where GitHub is strongest

For the ADM, GitHub is especially strong in:

- repo-native agent execution
- pull request and review workflow
- branch protection and approval controls
- auditability through repository history and session tracking
- engineering teams that already work primarily in GitHub Enterprise

This makes GitHub one of the most natural non-Microsoft implementation paths for the ADM.

## 6. Where GitHub needs complements

GitHub is not the whole ADM operating model on its own.

Most organisations will still need complementary tooling for:

- requirements discovery with business stakeholders
- wider backlog and portfolio workflow where that sits outside GitHub
- enterprise governance that spans more than the repository
- cost, model and vendor management across a wider agent estate

In some organisations GitHub Issues will be enough for backlog flow. In others, GitHub will sit alongside Jira, Azure DevOps or another enterprise work management layer.

## 7. Reference operating pattern

A practical GitHub-centred ADM pattern could look like this:

1. Intent Specifications are approved outside or alongside GitHub.
2. GitHub Issues or linked backlog items represent ready work.
3. Copilot coding agent is assigned to implement against the approved intent.
4. The agent works on an isolated branch and raises a pull request.
5. People review the pull request through the Governance Tier.
6. Branch protections, checks and approval controls act as enterprise guardrails.
7. Session logs, repository history and pull request discussion provide provenance.

This is a strong pattern for the engineering execution part of the ADM.

## 8. Recommended position in the ADM repository

The ADM should present GitHub Enterprise and GitHub Copilot as a strong non-Microsoft route for repo-native agent execution in enterprise software development.

It is particularly compelling where:

- the repository is already the centre of delivery flow
- pull request review is the main governance mechanism
- engineering controls are already mature in GitHub

## 9. Source notes

This note is based on public GitHub documentation, including:

- [About GitHub Copilot coding agent](https://docs.github.com/en/enterprise-cloud%40latest/copilot/concepts/agents/coding-agent/about-coding-agent)
- [GitHub Copilot coding agent](https://docs.github.com/en/enterprise-cloud%40latest/copilot/how-tos/use-copilot-agents/coding-agent)
- [Responsible use of GitHub Copilot coding agent on GitHub.com](https://docs.github.com/en/enterprise-cloud%40latest/copilot/responsible-use/copilot-coding-agent)
- [Managing GitHub Copilot coding agent in your enterprise](https://docs.github.com/copilot/how-tos/administer/enterprises/managing-copilot-coding-agent-in-your-enterprise)

GitHub product scope and controls are evolving quickly. This note should be updated as the platform matures.