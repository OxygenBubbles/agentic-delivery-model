# Tooling notes

These notes show how the Agentic Delivery Model can run on nine toolchains. Each note sets out where the model's elements live on the toolchain, how far it supports each of the eleven steps and how it holds the nine controls and the six trust boundaries. It then gives a configuration pattern for one change through the flow, the gaps an adopter fills with other tooling and the decisions an adopter takes. The model is vendor-neutral and depends on none of these products, so each note describes one way to run it rather than an endorsement.

Every note was checked against the vendor's documentation on 22 October 2026 and lists the pages it relied on. Products in this field change month by month, so an adopter checks availability, names and settings against the vendor's current documentation before relying on a note.

## The notes

| Toolchain | Note | What it covers |
|---|---|---|
| GitHub | [GITHUB-ADM.md](GITHUB-ADM.md) | Issues and Projects, rulesets, Actions, environments and attestations, Advanced Security, Copilot cloud agent, custom agents, code review and the third-party coding agents |
| Microsoft | [MICROSOFT-ADM.md](MICROSOFT-ADM.md) | Azure Boards, Repos, Pipelines and Artifacts, Microsoft Foundry, Copilot Studio, Agent 365, Entra Agent ID and Purview |
| GitLab | [GITLAB-ADM.md](GITLAB-ADM.md) | Work items, merge requests, pipelines, protected environments, compliance frameworks and the GitLab Duo Agent Platform |
| AWS | [AWS-ADM.md](AWS-ADM.md) | Kiro, Amazon Bedrock and AgentCore, the AWS code services, AWS Signer and CloudTrail |
| Atlassian | [ATLASSIAN-ADM.md](ATLASSIAN-ADM.md) | Jira, Confluence, Rovo, Bitbucket Cloud, Jira Service Management and Atlassian Guard |
| Anthropic | [ANTHROPIC-ADM.md](ANTHROPIC-ADM.md) | Claude Code in the terminal, the editor extensions, the desktop app and cloud sessions, the Claude Agent SDK and Claude Code in GitHub Actions and GitLab pipelines |
| OpenAI | [OPENAI-ADM.md](OPENAI-ADM.md) | Codex in its command-line interface, the editor extension, the ChatGPT desktop app and Codex Cloud, with Codex code review and the enterprise controls |
| Google | [GOOGLE-ADM.md](GOOGLE-ADM.md) | Google Antigravity in Gemini Enterprise, the Gemini Enterprise Agent Platform, Cloud Build, Artifact Registry, Binary Authorization and Cloud Deploy |
| Factory | [FACTORY-ADM.md](FACTORY-ADM.md) | Droid in its command-line interface, its headless mode and the Factory App, with custom droids, Missions, Automations and the enterprise controls |

## How the notes fit together

The platform notes (GitHub, Microsoft, GitLab, Atlassian, AWS and Google) cover the pipeline, environments and the authorisation record, and most also cover version control. The agent notes (Anthropic, OpenAI and Factory) cover agents that run on top of a platform, so an adopter usually reads one platform note with the note for each agent supplier it uses. Several platforms also host other suppliers' agents, and the notes link to each other where that happens.

## Reading the readiness tables

Section 2 of each note judges, from the vendor's documented capabilities, the highest autonomy level an agent can hold at each step with the nine controls of 5.3 of [the model](../MODEL.md) in place. The judgement describes what the toolchain can support. The level an adopter actually grants is set under 5.4 of the model, on an evaluation and a recorded period at the level below, and starts at the levels in 3.1.

## Keeping the notes current

Each note carries the date it was checked and a list of its sources. A correction against a vendor's current documentation, or a note for another toolchain in the same structure, is a welcome contribution under [CONTRIBUTING.md](../CONTRIBUTING.md). The notes written against version 1.1 remain in the history of this repository.
