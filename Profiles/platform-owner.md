---
profile_type: human_role
role_id: ADM-H-PLATFORM-OWNER
role_name: Platform Owner
mission: Own the engineering platform as an internal product (its environments, pipelines, run identities and allow-lists) and configure the agents that run on it, revoking their identities on suspension.
accountability: platform owner
steps:
  - 3 Workspace, through the environments the platform allocates
  - 5 Verify, 7 Identify and 8 Promote, through the shared pipeline
accountable_to: set by the adopter
works_with:
  - Release Manager
  - Security Lead
  - Data Owner
  - System Steward
  - Product Owner
---

# Platform Owner

## Role mission

The platform owner holds the platform owner accountability (section 7 of the model). The post is the product owner of the engineering platform: the environments, pipelines, run identities, allow-lists and shared definitions that carry every change through the flow. The delivery teams are its users and adoption is its measure, and the post configures the agents that run on the platform.

## Why this role matters in the ADM

The flow runs on the platform at most of its steps: the pipeline allocates workspaces, runs the checks, attests release candidates, promotes them and computes readiness into the manifest. Agents reach other systems only through run identities and the allow-list, so the platform is where agent access is controlled and where a reduction in autonomy takes effect.

## Primary outcomes

- Environments arrive within a published lead time, with an overdue request recorded as a platform defect (section 6.3 of the model).
- Each agent run has a scoped, short-lived run identity, reaches only allow-listed systems and is logged in full.
- A manual route remains available for every automated step and is exercised in recovery testing.

## Core accountabilities

- Own the platform backlog, with its priorities set with the product owners (section 6.4 of the model).
- Change shared pipelines and checks under the platform's rules: versioned, tested before rollout, rolled out in stages and pinnable.
- Own the tool allow-list and hold boundary 3 in section 5.5 of the model, with the security lead approving each addition.
- Approve every increase in autonomy, including the first delegation above L1, Advise, with the system steward, the security lead or the data owner for a matter in their remit.
- When a reduction is ordered, block the new runs it affects, cancel their active jobs, terminate the processes and revoke the execution credentials with the logs preserved, then correct the agent profile.
- Set the cost circuit breakers: alerts on spend per intent and hard limits that pause execution.

## Decisions this role owns

- Whether to approve an increase, on an evaluation the practice owner accepted against criteria set before it ran.
- When to order an immediate reduction in autonomy (section 5.4 of the model).
- Whether a change to a shared definition is breaking and so needs a new major version.

## Key artefacts

- Owns the shared definitions and templates, the environment definitions and the agent profiles and evaluations.
- Owns the execution logs and session records, the latter with the security lead (section 8.1 of the model).
- Keeps the current-state architecture with the system steward, regenerated after each release.

## Part in the flow and the rhythm

- Step 3, Workspace: the platform allocates each workspace from a versioned definition and refreshes it with masked data.
- Steps 5, 7 and 8: the shared pipeline runs the checks, attests the release candidate and promotes it.
- Between reviews: allocates or refreshes environments for waiting units and works the platform backlog.

## Working interfaces

- With the release authority: holds the standard for release candidates, environments and the manifest together. The platform owner reissues identities at the lower scope when the release authority declares an error-budget pause.
- With the security lead: submits allow-list additions for approval and holds boundary 1 together.
- With the data owner: holds boundary 5 together.
- With the system steward and the practice owners: enforces their standards in the pipeline from the published source.

## What this role is not

- It does not authorise the platform's own releases. An increase takes effect only through the attested and authorised release of the platform.
- It does not own the content of the standards or approve its own allow-list additions.
- It does not accept the evaluation behind an increase. The practice owner for the step's work, who did not run it, does.

## Common failure modes

- Letting teams fork the shared pipeline so the same check runs differently in different places.
- Treating a merged agent profile as a grant of authority.
- Issuing identities that outlive the run, so a reduction cannot take effect at once.
- Rolling a pipeline change across the estate without testing it on a representative repository.

## Measures of effectiveness

- Environment wait time.
- Adoption of the platform's services by the delivery teams.
- Stop-condition rate where a run found its context missing or out of date.
- Spend per intent against the circuit-breaker threshold.

## How this post is likely to change

This is a forecast rather than a requirement of the model, and it is paced by the autonomy an organisation raises on evidence under section 5.4, not by dates. Platform work is likely to grow as more steps and classes are delegated, particularly where evaluations, identity controls and recovery arrangements need building or maintaining. Its scale depends on existing platform capability and the number of contexts supported. Run identities, allow-lists, evaluations, cost and the manual fallback sit with the post and the platform engineers, because running agents becomes part of running the platform. The approvals stay where the model places them.

## Strong background fit

- Lead engineer of a platform team.
- Product owner for an internal platform.
- Platform engineering lead.
- Site reliability or infrastructure lead.

## Good signals in practice

A strong platform owner keeps the flow faster than the workaround, so teams call the shared pipeline because it serves them. A reduction in autonomy takes effect as soon as it is ordered.
