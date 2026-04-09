# How Atlassian Rovo Dev Could Support the ADM

> **Alpha.** This tooling guidance reflects the current state of the Atlassian ecosystem and will be updated as tooling matures and practical experience grows.

This note sets out how Atlassian Rovo Dev could support the Agentic Delivery Model in an enterprise software delivery context.

It is a practical implementation note, not a product endorsement. It focuses on enterprise software development in Jira-centred and Atlassian-centred estates.

## 1. Scope and caveat

Atlassian is relevant to the ADM because many enterprise software teams already use Jira as the main system of work and Bitbucket or GitHub as the repository layer around it.

Rovo Dev gives Atlassian a more direct role in the software delivery layer through code reviews, CLI support, MCP connectivity and Jira-linked workflow.

This makes Rovo Dev a credible enterprise scenario for the ADM, especially where Jira is already the main operating surface for delivery flow and clarification.

## 2. Where Rovo Dev fits in the ADM

Rovo Dev is strongest in:

1. Jira-centred work management and clarification flow
2. developer assistance through the CLI
3. code review support in Bitbucket and GitHub
4. context integration through Atlassian apps and MCP

For the ADM, this makes Atlassian especially relevant around requirements flow, work management and review augmentation.

## 3. Enterprise strengths for the ADM

Rovo Dev is useful for the ADM where:

- Jira is the main work management system
- development flow is closely tied to Jira issues and enterprise coordination
- teams want code review augmentation in Bitbucket or GitHub
- developers want agent support in the CLI with configurable tool permissions
- organisations want richer context through Atlassian data and MCP connections

These are common patterns in enterprise software delivery.

## 4. Where Rovo Dev needs complements

Rovo Dev does not yet look like a complete replacement for repo-native coding agents.

In many organisations it will work best as:

- a workflow and clarification layer around Jira
- a review augmentation layer in Bitbucket or GitHub
- a CLI assistant for development tasks
- a context layer connected to Atlassian data and approved MCP sources

That means Rovo Dev is likely to complement, rather than replace, the deeper implementation layer in many ADM setups.

## 5. Reference operating pattern

A practical Atlassian-centred ADM pattern could look like this:

1. Jira remains the main work management surface for Intent Specifications and clarification flow.
2. Rovo Dev supports CLI-based development work and review flow.
3. Bitbucket or GitHub remain the repository and pull request surfaces.
4. Rovo Dev code reviews provide an additional review layer on pull requests.
5. MCP connections enrich agent context where approved.
6. People still provide the governance, sign-off and system coherence decisions the ADM requires.

This is a strong scenario for enterprises where Jira already sits at the centre of delivery management.

## 6. Recommended position in the ADM repository

The ADM should present Atlassian Rovo Dev as a credible enterprise scenario for Jira-centred delivery organisations.

It is particularly relevant where the enterprise wants agent support tied closely to backlog management, clarification flow and review workflow rather than only to the repository.

## 7. Source notes

This note is based on public Atlassian support documentation, including:

- [Rovo Dev rollout](https://support.atlassian.com/rovo/docs/rovo-dev-rollout/)
- [Use Rovo Dev CLI](https://support.atlassian.com/rovo/docs/activate-or-deactivate-rovo-dev-cli-on-your-site/)
- [Install and run Rovo Dev CLI on your device](https://support.atlassian.com/rovo/docs/install-and-run-rovo-dev-cli-on-your-device/)
- [Use tools in Rovo Dev CLI](https://support.atlassian.com/rovo/docs/use-tools-in-rovo-dev-cli/)
- [Trigger a code review](https://support.atlassian.com/rovo/docs/trigger-a-code-review/)
- [Rovo Dev and Model Context Protocol (MCP)](https://support.atlassian.com/rovo/docs/rovo-dev-and-model-context-protocol-mcp/)

Atlassian product scope and terminology are evolving quickly. This note should be updated as the platform matures.