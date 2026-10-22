# Atlassian: running the Agentic Delivery Model

> Checked against vendor documentation on 22 October 2026. Products named here change often, so check availability, names and settings against the vendor's current documentation before relying on them. This note describes one way to run the model on this toolchain and is not an endorsement.

This note covers the Atlassian Cloud products that can carry the flow in [MODEL.md](../MODEL.md): Jira, Confluence, Rovo, Bitbucket Cloud, Jira Service Management and Atlassian Guard. Jira holds the request and the intent's working copy and assigns work items to coding agents, including its own Jira Coding Agent, formerly Rovo Dev in Jira. Bitbucket Cloud holds the repository, Bitbucket Pipelines and the deployment environments, and Jira Service Management holds the change record. Rovo Studio builds the Rovo agents that draft and advise, and the Atlassian Rovo MCP Server, a hosted Model Context Protocol (MCP) server generally available since February 2026, opens Jira, Confluence and Bitbucket to other agents. Rovo Dev is reaching end of life as a standalone product, and Code Reviewer, its pull request review agent, is moving into Jira subscriptions.

The toolchain covers steps 1, 2, 6 and 9 well, because the work item, the pull request and the change record are linked by the work item key. An adopter adds attestation at step 7, Identify. Observability at step 11, Confirm and learn, comes from another product. Agent work outside Bitbucket Pipelines also needs a per-run identity. Many Atlassian adopters keep code in GitHub or GitLab. The Jira Coding Agent and Code Reviewer support Bitbucket Cloud and GitHub, so a GitHub adopter runs steps 3 to 10 as [GitHub](GITHUB-ADM.md) describes. Neither currently supports GitLab, where a pull request is called a merge request, and [GitLab](GITLAB-ADM.md) covers it. The third-party agents Jira assigns are covered in [Anthropic](ANTHROPIC-ADM.md), [OpenAI](OPENAI-ADM.md) and [GitHub](GITHUB-ADM.md).

## 1. Where the model's elements live

| Model element | Where it lives on Atlassian |
|---|---|
| The request (2.2) | A work item in an intake space, collected by its form, with the product owner's decision recorded as a transition |
| The intent (2.1 and appendix A) and its published version | Work item fields hold the working copy, and the published version is a repository file merged by pull request |
| The work item | A Jira work item, linked by its key to branches, pull requests and deployments |
| Repository instructions | `AGENTS.md`, read by the Rovo Dev command-line interface (CLI), and `.rovodev/.review-agent.md` for Code Reviewer |
| Path-specific instructions | Nested `AGENTS.md` files, the nearest to the working directory taking priority, and Agent Skills in `.rovodev/skills/` |
| Standards as retrievable context | Published standards in version control, explained by Confluence pages that link to them |
| The agent profile (appendix C) | A versioned record in the platform repository. No Atlassian product holds its evaluation and approval |
| The context profile (appendix B) | A versioned record in the platform repository, implemented by `bitbucket-pipelines.yml` and the deployment environments |
| The tool allow-list | Rovo MCP Server permission groups and Bitbucket workspace switches at organisation level, with Rovo Dev `toolPermissions` and step scopes at run level |
| The run identity | The short-lived OAuth token issued to each Agentic Pipelines step. Other surfaces act as a person or under an admin-issued API key, and the Jira Coding Agent's identity is not documented |
| The workspace (step 3) | A branch per unit, worked in an Agentic Pipelines step, a Jira Coding Agent cloud sandbox or a Rovo Dev CLI worktree |
| The pipeline and deterministic checks (step 5) | Bitbucket Pipelines, with merge checks enforced on Premium and required custom merge checks built on Forge |
| The release candidate and its attestation (step 7) | An image in Bitbucket Packages, a package in an external registry or a release commit, attested by a signing tool in the pipeline |
| Environments and promotion (step 8) | Bitbucket deployment environments with their own variables and permissions |
| The authorisation record (step 9) | A Jira Service Management change request carrying the change authorisation and the release go or no-go as separate approvals |
| The release manifest | A file the pipeline generates and attaches to the change request |
| The decision trail (8.1) | The work item, the pull request, the Pipelines and deployment history, the change request and the Atlassian Guard audit log |
| Telemetry for step 11 | An observability product, with incidents in Jira Service Management. Compass is being phased out in favour of DX |

## 2. Readiness against the eleven steps

"Supports up to" is a judgement from the documented capabilities: the highest level an agent can hold at the step on this toolchain with the nine controls of 5.3 in place. It is not a recommended level. The model's starting levels are in 3.1, and a level above L1, Advise, takes effect only after the evaluation and approval under 5.4.

| Step | What Atlassian provides | Supports up to | Gap or complement |
|---|---|---|---|
| 1 Intent | Work item fields, forms and workflows, a Rovo Studio agent run by automation and Jira Planner in early access | L2 | Publishing to the repository needs a platform pipeline |
| 2 Gate (where triggered) | A workflow status per gate and a Rovo agent assembling the material | L1 | The decision index sits in the standards repository |
| 3 Workspace | Branches, Agentic Pipelines steps, the Jira Coding Agent's cloud sandbox and Rovo Dev CLI worktrees | L0 by design | Environment definitions and masked refresh are adopter pipeline jobs |
| 4 Build | The Jira Coding Agent, Agentic Pipelines in beta running Rovo Dev, Claude or Codex, the Rovo Dev CLI and Claude Code, Cursor or GitHub Copilot assigned from Jira | L2 | Only Agentic Pipelines issues a per-run identity |
| 5 Verify | Pipelines, enforced merge checks, required custom merge checks and Rovo Dev's failed-step analysis in beta | L3 | A branch can edit its pipeline file, so mandatory checks sit in required custom merge checks |
| 6 Review | Pull requests, default reviewers, approval counts and Code Reviewer comments against the linked work item | L1 | No path-based code owners were found, and Code Reviewer's model is undisclosed |
| 7 Identify | Pipelines builds the merged commit, and Bitbucket Packages stores images | L0 by design | No native attestation or software bill of materials (SBOM) was found, and artefacts expire after 14 days |
| 8 Promote | Deployment environments and variables, Premium deployment permissions and OpenID Connect (OIDC) | L3 | Deployment permissions name users and groups, so the agent needs a nameable identity |
| 9 Authorise | Change requests, deployment gating on Premium and Enterprise and advisory artificial intelligence (AI) risk assessment | L3 | Gating for Bitbucket Pipelines needs confirming, and the adopter generates the manifest |
| 10 Deploy | A production step that runs once gating allows it, under deployment permissions | L0 by design | The attested candidate deploys without a rebuild |
| 11 Confirm and learn | Jira Service Management incidents, Rovo agents drafting reviews and acceptance on the work item | L2 | Telemetry correlation needs an observability product |

Atlassian is strongest at the linked records people work in: the work item, the pull request and the change request. It is weakest at step 7 and at the run identity, where only Agentic Pipelines, in beta, issues an identity per run. Several agent features are in beta, in early access or in transition.

## 3. Controls and trust boundaries

| Control | How Atlassian supports it | Gap |
|---|---|---|
| 1. Never the sole authoriser | Enforced merge checks require approvals, and Code Reviewer only comments | Approval counts cannot tell an agent acting as a person from that person, so agents hold no approve or merge tool |
| 2. Cannot attest to its own work | Pipelines records build results, and enforced merge checks block a merge on failed builds | The pipeline file sits in the branch, so required custom merge checks carry the checks |
| 3. Published promotion path only | Deployment permissions and gating define the route, with OIDC credentials per step | Production variables reach any step naming the environment, so no agent step names production |
| 4. Logged under a run identity | Agentic step logs show tool calls, the Rovo Dev CLI logs locally and Guard exports audit logs | The Bitbucket audit log omits approvals, merges and pipeline runs, so records go to the adopter's store |
| 5. Manual route | Agent surfaces are optional, with workspace switches and an organisation switch to deactivate AI | The context profile records each manual route |
| 6. Hosting meets the data position | Rovo routes to OpenAI, Anthropic, Google and open models under zero data retention terms, and eligible Enterprise customers can request Atlassian-hosted models | Atlassian's guidance states that the Jira Coding Agent does not yet meet data residency |
| 7. Implementer does not review | `modelId` and the Agentic Pipelines provider pin the implementing model | Code Reviewer and the Jira Coding Agent do not disclose their model, so a pinned reviewer on another provider evidences the pairing |
| 8. Approved sources only | Instruction files and agent definitions sit in the repository, and Agentic Pipelines denies new tools until they are allow-listed | The Jira Coding Agent starts from the work item, so it is pointed at the published intent |
| 9. No data or secrets out | Secured variables, a third-party secrets provider and step tokens that expire with the step | The Rovo Dev provider needs an Atlassian application programming interface (API) token, and the adopter scans output for secrets |

| Boundary (5.5) | What holds it on Atlassian |
|---|---|
| 1. The organisation and its supplier services | Atlassian Cloud terms, Guard and data residency for Jira, Confluence, Jira Service Management and Rovo data, but not yet Bitbucket |
| 2. The agent supplier and the model providers | The permitted model list records Rovo's routing and any Atlassian-hosted option, and third-party providers work under their own terms |
| 3. The run and the systems it reaches | Step scopes, Rovo MCP Server permission groups, allowed domains, internet protocol (IP) allowlists and Rovo Dev tool permissions |
| 4. Retrieved content and the agent's instructions | Pinned instruction files and the published intent as the only build input, since Atlassian warns that runtime variables can override a prompt |
| 5. A controlled environment and the repository | Agentic steps run on Atlassian's cloud runners, so the adopter admits only masked data and scans their output for secrets |
| 6. Non-production and production | Deployment permissions, gating and the release authority's authorisation, with no agent identity permitted on production |

## 4. Configuration pattern

### 4.1 Intents, standards and context

**Request and intent.** An intake space's form collects the request record, and the product owner records the decision as a transition. The intent's work item type carries a field per element of 2.1 and the statuses of appendix A. An automation rule runs the drafting agent for each formation pass and writes the draft to the work item, so the agent only reads. Jira Planner, in early access, drafts a specification in Confluence that feeds the shape pass and is not a second intent.

**Publication.** When the intent architect moves the work item to published, a custom pipeline writes the intent file to the leading context's repository and opens a pull request for the code owners. No agent takes part, and the published status closes the working copy to edits.

**Standards and context.** Each standard has one source in version control under principle 7, read there by the pipeline and retrieved by agents. Confluence pages explain each standard and link to it. Agents read Jira and Confluence through the Rovo MCP Server's read and search groups.

### 4.2 Agents, permissions and run identities

Each row is one agent profile, at the model's starting level for its step and class of change.

| Step and class | Agent and surface | Level | How the toolchain enforces it |
|---|---|---|---|
| 1, every class | Rovo Studio agent run by automation | L2 | The rule writes the draft, and the agent only reads |
| 2, gate triggers | Rovo Studio agent | L1 | The rule posts its findings as a comment |
| 4, approved classes | Agentic step on a pinned provider, or the Jira Coding Agent | L2 | Repository and pull request write scopes, branch restrictions and draft pull requests |
| 5, failed checks | Rovo Dev failed-step analysis | L2 | It proposes fixes, and only the pipeline runs checks |
| 6, every class | Code Reviewer, or an agentic step on another provider | L1 | Read scope, with no approve or merge tool |
| 8, non-production promotion | Agentic step invoking the approved job | L3 | Deployment permissions admit it below production only |
| 9, readiness | Agentic step assembling the manifest | L3 | Gating holds production until the change request allows it |
| 11, telemetry and reviews | Rovo agent | L2 | It drafts the post-incident review, and a person publishes it |

The three permissions of 5.2 are separate grants. Retrieval is a step's implicit read access and the read and search groups of the Rovo MCP Server. Execution is the repository and pull request write scopes at L2, Draft, with a pipeline write scope added at L3, Execute under gate. Bitbucket documents the pull request read scope as sufficient to comment, so tool permissions keep a retrieval-only step from commenting.

Organisation-level settings hold the ceiling under principle 8: the Rovo MCP Server settings in Atlassian Administration and the Bitbucket workspace switches for Rovo Dev, Agentic Pipelines and each provider. No agent identity receives the `write_bitbucket` group, because it includes tools that approve and merge pull requests as the connected person. Agent configuration changes only by pull request. Rovo credit spending limits act as the cost circuit breakers of 5.4, and the halt control combines the organisation's AI switch with the workspace switches.

### 4.3 From verification to deployment

**Verify and identify.** Every push runs the Pipelines definition, and enforced merge checks block the merge until builds pass, approvals are present and tasks are resolved. An agent at L2 analyses failed steps, and Atlassian advises against using an agent's result as a release gate. After the merge the pipeline builds the candidate once, generates its SBOM, signs it and stores it in a registry, because Pipelines artefacts expire after 14 days.

**Promote.** Each deployment step names its environment with the `deployment` key and receives that environment's variables at deployment. OIDC replaces stored cloud keys, and Bitbucket runs one deployment at a time per environment.

**Authorise and deploy.** Deployment tracking in Jira Service Management raises a change request when a deployment starts, and gating holds the deployment until the request allows it. The change authority and the release authority record separate approvals against the attached manifest. Production deployment permissions name only the release authority's group. Atlassian's gating guides name GitHub, GitLab and Jenkins, so a Bitbucket Pipelines adopter confirms the integration first.

**Deployment approaches.** An artefact-based context redeploys the previous registry image, tested below production, as its proven rollback. A state-based context keeps its plan as an artefact and applies it through one adapter. A migration-based context records its dry run and reconciliation in the manifest.

### 4.4 One change through the flow

A standard-tier change adds a filter to an existing report on Bitbucket Cloud.

1. **Intent.** The product owner approves the request, the agent drafts the passes and the test engineer validates the acceptance examples before publication.
2. **Gate.** The advising agent finds no trigger, and the intent architect records the screening.
3. **Workspace.** The pipeline allocates the unit's branch and workspace.
4. **Build.** An agentic step on a pinned provider implements the published intent and opens a draft pull request with the work item key.
5. **Verify.** Pipelines runs the checks, and the required merge checks report on the pull request.
6. **Review.** A review agent on another provider comments, and a code owner who did not produce the change approves and merges.
7. **Identify.** The pipeline builds, signs and stores the candidate with its SBOM.
8. **Promote.** The promotion agent invokes the approved job for Test and Staging, Bitbucket's default non-production environments.
9. **Authorise.** The readiness step attaches the manifest, and the change authority and the release authority approve.
10. **Deploy.** Gating allows the production step, and the authorised candidate deploys.
11. **Confirm and learn.** Health checks pass, and the product owner accepts the work on the work item.

## 5. Gaps and complementary tooling

| Gap | What an adopter adds |
|---|---|
| Attestation and SBOM | A signing and SBOM tool in the pipeline and a registry for the signed candidate |
| Per-run identity outside Agentic Pipelines | Agentic Pipelines for headless runs, or the Rovo Dev CLI's non-interactive mode under a short-lived identity elsewhere |
| Path-based code owners | A required custom merge check built on Forge |
| Evidence of model separation | Pinned models, with the reviewer on a different provider |
| The release manifest | A pipeline step that assembles it and attaches it to the change request |
| Telemetry at step 11 | An observability product, and DX for delivery measures where used |
| Retention | Export of audit logs, step logs and session records to the adopter's store |
| The Rovo Dev transition | A plan for the Rovo CLI and for Code Reviewer's metering in Rovo credits from 3 December 2026 |

## 6. Decisions for adopters

1. **Repository host.** Each context's host, Bitbucket Cloud, GitHub or GitLab, sets the Atlassian agents available to it.
2. **Agent surfaces.** The adopter permits some or all of the Jira Coding Agent, Agentic Pipelines providers, the Rovo Dev CLI and third-party agents.
3. **Permitted models.** The permitted model list records Rovo's providers, any Atlassian-hosted option and each third-party provider allowed.
4. **Agent-initiated jobs.** The adopter decides whether agents may start pipeline or deployment jobs, and where.
5. **Intents.** The adopter fixes the work item type, the published version's path and the publication pipeline.
6. **Standards.** Each standard has one home in version control, and the adopter decides what Confluence holds.
7. **Rovo MCP Server access.** The adopter grants permission groups to agent identities and decides on API tokens.
8. **Plans.** Enforced checks and deployment permissions need Bitbucket Premium, and gating needs Jira Service Management Premium or Enterprise.
9. **The release candidate.** The adopter chooses the signing tool, the SBOM format and the registry.
10. **Logs.** The adopter routes audit logs, step logs and session records to its own store.

## Sources

- [Rovo Dev licensing and transition FAQ](https://www.atlassian.com/licensing/rovo-dev)
- [How we're evolving Jira for AI-native software development](https://www.atlassian.com/blog/company-news/ai-sdlc)
- [Atlassian Announces System for AI-native Software Development in Jira](https://www.businesswire.com/news/home/20260715176442/en/Atlassian-Announces-System-for-AI-native-Software-Development-in-Jira)
- [AI Agents in Jira: Your top questions answered](https://community.atlassian.com/forums/Jira-articles/AI-Agents-in-Jira-Your-top-questions-answered/ba-p/3269021)
- [Collect information for your team with forms](https://support.atlassian.com/jira-software-cloud/docs/collect-information-for-your-team-with-a-form/)
- [Manage Rovo Dev CLI settings](https://support.atlassian.com/rovo/docs/manage-rovo-dev-cli-settings)
- [Use memory in Rovo Dev CLI](https://support.atlassian.com/rovo/docs/use-memory-in-rovo-dev-cli/)
- [Extend Rovo Dev CLI with Agent Skills](https://support.atlassian.com/rovo/docs/extend-rovo-dev-cli-with-agent-skills/)
- [Set custom instructions for code reviews](https://support.atlassian.com/rovo/docs/set-custom-instructions-for-code-reviews/)
- [Rovo Dev Code Review](https://www.atlassian.com/software/rovo-dev/code-review)
- [Agentic Pipelines: Step-by-step guide](https://support.atlassian.com/bitbucket-cloud/docs/agentic-pipelines/)
- [Authentication and security for Agentic Pipelines](https://support.atlassian.com/bitbucket-cloud/docs/authentication-and-security-for-agentic-pipelines/)
- [Work with Rovo Dev in Bitbucket Pipelines](https://support.atlassian.com/rovo/docs/work-with-rovo-dev-in-bitbucket-pipelines/)
- [Suggest or require checks before a merge](https://support.atlassian.com/bitbucket-cloud/docs/suggest-or-require-checks-before-a-merge/)
- [Expanding Custom Merge Checks to the Workspace and Project level](https://www.atlassian.com/blog/bitbucket/expanding-custom-merge-checks-to-the-workspace-and-project-level)
- [Set up and monitor deployments](https://support.atlassian.com/bitbucket-cloud/docs/view-your-deployments-dashboard/)
- [Set custom deployment permissions for your environments](https://support.atlassian.com/bitbucket-cloud/docs/set-custom-deployment-permissions-for-your-environments/)
- [OIDC authentication issues troubleshooting](https://support.atlassian.com/bitbucket-cloud/kb/oidc-authentication-issues-troubleshooting/)
- [Bitbucket Cloud audit log events](https://support.atlassian.com/bitbucket-cloud/kb/bitbucket-cloud-audit-log-events/)
- [The 2025 Year in Review (and what's coming soon)](https://www.atlassian.com/blog/bitbucket/the-2025-year-in-review-and-whats-coming-soon)
- [Supported tools for the Atlassian Rovo MCP Server](https://support.atlassian.com/atlassian-ai-gateway/docs/supported-tools/)
- [Control Atlassian Rovo MCP server settings](https://support.atlassian.com/ja/security-and-access-policies/docs/control-atlassian-rovo-mcp-server-settings/)
- [What is Rovo Studio?](https://support.atlassian.com/studio/docs/what-is-rovo-studio)
- [Rovo agents](https://support.atlassian.com/rovo/docs/agents/)
- [Rovo licensing](https://www.atlassian.com/licensing/rovo)
- [Atlassian trust: AI](https://www.atlassian.com/trust/ai)
- [Understand data residency](https://support.atlassian.com/security-and-access-policies/docs/understand-data-residency/)
- [Deployment gating](https://developer.atlassian.com/cloud/jira/service-desk/deployment-gating)
- [What is deployment gating?](https://support.atlassian.com/jira-service-management-cloud/docs/what-is-deployment-gating/)
- [AI risk assessment is now generally available in Jira Service Management](https://community.atlassian.com/forums/discussion/3214684/ai-risk-assessment-is-now-generally-available-in-jira-service-management)
- [The next chapter for Compass](https://www.atlassian.com/blog/announcements/the-next-chapter-for-compass)
- [Atlassian Guard](https://www.atlassian.com/software/access)
