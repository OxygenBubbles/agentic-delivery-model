# How Claude Code and the Anthropic Agent SDK Could Support the ADM

> **Alpha.** This tooling guidance reflects the current state of the Claude Code and Agent SDK ecosystem and will be updated as tooling matures and practical experience grows.

This note sets out how Claude Code and the Anthropic Agent SDK could support the Agentic Delivery Model in an enterprise software delivery context.

It is a practical implementation note, not a product endorsement. It focuses on enterprise software development rather than general collaboration tooling.

## 1. Scope and caveat

Claude Code is Anthropic's agentic coding tool. It runs in the terminal, operates directly against a codebase and can plan, implement, test and iterate autonomously within a scoped environment.

The Anthropic Agent SDK extends this further. It provides a framework for building managed agent systems where a parent agent orchestrates multiple sub-agents, each configured with specific tools, context and permissions. This maps closely to how the ADM defines agent roles as configurations of a general-purpose capability rather than distinct products.

MCP (Model Context Protocol) is the open standard that underpins tool access. It allows agents to connect to external systems (repositories, CI/CD, databases, documentation, backlog tools) through a consistent interface. MCP servers define what an agent can see and do. This is directly relevant to how the ADM scopes agent access.

## 2. Where Claude Code fits in the ADM

Claude Code is strongest in three ADM areas:

1. implementation against approved Intent Specifications
2. testing, review and documentation as distinct agent configurations
3. orchestrated multi-agent delivery through managed sub-agents

This makes it a strong candidate for the Execution Tier and a contributor to Governance Tier workflows through its review and documentation capabilities.

## 3. Agent roles as configurations

The ADM defines agent roles (Requirements, Implementation, Testing, Review, Documentation, Platform, Refactoring, Security, Scheduling) not as separate products but as configurations of general-purpose agents. Each role is differentiated by its configured context, access scope and assigned tools rather than by inherent capability.

Claude Code and the Agent SDK implement this pattern directly. A single Claude model can be configured as any ADM agent role by varying:

- **The system prompt and project context.** A CLAUDE.md file (the agent configuration file referenced in the ADM) defines what the agent knows, what conventions it follows and what boundaries it observes.
- **The tool set.** MCP servers control which external systems the agent can access. An Implementation Agent might have access to the repository, CI/CD pipeline and documentation. A Security Agent might have access to vulnerability scanners, dependency audit tools and compliance databases. The tools define the role.
- **The access scope.** File system permissions, branch protections and API credentials are scoped per agent session. Agents operate in isolated environments with only the access they need.

This is the ADM's "configured context" model in concrete form.

## 4. Managed agents and the Scheduling Agent

The Agent SDK's managed agent pattern is particularly relevant to the ADM's Scheduling Agent (SCH-1) and its coordination model.

In the Agent SDK, a parent agent can launch sub-agents to handle specific tasks. Each sub-agent runs with its own context, tools and permissions. The parent agent decides what to delegate, how to sequence the work and how to handle the results.

This maps to three ADM patterns:

**Single-agent delivery.** For most Intent Specifications, a single Claude Code agent works in an isolated branch, implements the change, runs tests and raises a pull request. This is the default ADM execution pattern.

**Coordinated multi-agent delivery.** For complex specifications that require multiple skills (implementation, security analysis, performance testing), a parent agent orchestrates multiple sub-agents against the same feature branch. Each sub-agent is configured for its specific role. The parent sequences their contributions and maintains context across the work. The output remains a single PR for governance review.

**Agent-to-agent resolution.** When a sub-agent encounters technical ambiguity that another agent can resolve (a testing agent identifying an edge case that the implementation agent can address), the parent agent routes the issue between sub-agents without requiring human intervention. Only intent-level questions escalate to the Governance Tier.

## 5. MCP as the access control layer

The ADM requires that agents operate in isolated sandboxes with scoped identity and authentication. MCP provides the mechanism for this.

Each MCP server defines a set of capabilities: reading files, executing commands, querying databases, calling APIs. By configuring which MCP servers an agent session can access, the organisation controls what the agent can see and do without modifying the agent itself.

This matters for enterprise governance because:

- access controls are defined outside the agent, not inside it
- the same agent model can be scoped differently for different roles
- audit trails capture which tools were invoked, by which agent, against which specification
- new capabilities can be added by connecting new MCP servers without reconfiguring the agent

For regulated environments, MCP's explicit tool boundary model supports the separation of concerns the ADM requires.

## 6. The agent configuration file

The ADM references agent configuration files as the mechanism for defining how agents behave within a specific codebase or project. In Claude Code, this is the CLAUDE.md file.

CLAUDE.md sits in the repository root and defines:

- coding conventions and architectural standards
- project-specific rules and constraints
- references to key files and patterns
- boundaries on what the agent should and should not modify

This is a practical implementation of the ADM's principle that architecture and standards should be encoded where agents can read them, not locked in documents that only people consult.

## 7. Where Claude Code is strongest

For the ADM, Claude Code is especially strong in:

- autonomous implementation from specification to pull request
- multi-agent orchestration through managed sub-agents
- flexible role configuration through prompts, tools and access scoping
- MCP-based integration with enterprise toolchains
- repository-native operation with direct codebase access
- iterative rework in response to review feedback

## 8. Where Claude Code needs complements

Claude Code is not the whole ADM operating model on its own.

Most organisations will still need complementary tooling for:

- backlog management and specification workflow (Jira, Azure DevOps, Linear or similar)
- repository hosting and pull request governance (GitHub, GitLab, Azure Repos)
- CI/CD pipeline execution
- enterprise identity and access management
- cost tracking and model usage governance across a wider agent estate

Claude Code operates within the development environment. The broader ADM operating model spans requirements discovery, governance workflow, release management and operational feedback loops that sit in surrounding systems.

## 9. Reference operating pattern

A practical Claude Code and Agent SDK pattern for the ADM could look like this:

1. Intent Specifications are approved and marked Ready in the backlog tool.
2. A Scheduling Agent (built on the Agent SDK) picks up the highest-priority ready item.
3. The Scheduling Agent checks specification boundaries against work in progress and assigns the item to an implementation sub-agent.
4. The implementation sub-agent (Claude Code) works in an isolated branch against the CLAUDE.md configuration and project conventions.
5. For complex specifications, the Scheduling Agent launches additional sub-agents (testing, security, documentation) and orchestrates their contributions.
6. The agent raises a pull request linked to the originating specification.
7. People review the pull request through the Governance Tier.
8. Repository branch protections, CI checks and approval controls act as enterprise guardrails.
9. Agent session logs, MCP tool invocation records and pull request history provide provenance.

## 10. Recommended position in the ADM

The ADM should present Claude Code and the Anthropic Agent SDK as a strong route for organisations that want to implement the ADM's multi-agent orchestration model directly.

It is particularly compelling where:

- the organisation wants to configure agent roles from a single general-purpose model rather than assembling separate products
- multi-agent coordination (the Scheduling Agent pattern) is a priority
- MCP-based integration with diverse enterprise toolchains is needed
- the team values terminal-native, developer-oriented tooling

## 11. Source notes

This note is based on public Anthropic documentation, including:

- [Claude Code overview](https://docs.anthropic.com/en/docs/claude-code/overview)
- [Claude Agent SDK](https://docs.anthropic.com/en/docs/agents/agent-sdk)
- [Model Context Protocol](https://modelcontextprotocol.io/)
- [Claude Code best practices](https://docs.anthropic.com/en/docs/claude-code/best-practices)

Anthropic's agent tooling is evolving quickly. This note should be updated as the platform matures.
