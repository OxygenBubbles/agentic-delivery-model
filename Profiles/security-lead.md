---
profile_type: human_role
role_id: ADM-H-SECURITY-LEAD
role_name: Security Lead
mission: Hold the security standard, its thresholds and the policy exceptions for security, together with the threat model for the agent platform and approval of each allow-list addition.
accountability: security lead
steps:
  - 1 Intent, through a delegate in the data and security position pass where a trigger is met
  - 2 Gate where triggered, the security and privacy gate
  - 5 Verify, policy exceptions for a security finding at or above blocking severity
accountable_to: set by the adopter
works_with:
  - Platform Owner
  - Data Owner
  - System Steward
  - Security agent (step 6, L1)
---

# Security Lead

## Role mission

The security lead holds the security lead accountability (section 7 of the model): the security standard and its thresholds, policy exceptions for security, the threat model for the agent platform and approval of each allow-list addition. The post is usually the head of information security or a named delegate. The pipeline enforces the standard on every change, so the post does not review every change.

## Why this role matters in the ADM

Agents add threats of their own, from actions under a run identity to retrieved text that carries a planted instruction. The model answers them with the controls in section 5.3 and the six trust boundaries in section 5.5. Principle 8 holds autonomy as a ceiling no project or user can raise, so the security lead and the accountable executive can answer for what agents do.

## Primary outcomes

- The security standard is published in a form the pipeline enforces and an agent retrieves.
- Every threat in the agent platform's threat model has a control before the affected step rises above L1, Advise.
- Each allow-list addition is approved before an agent can reach it.

## Core accountabilities

- Own the security policy and thresholds standard (section 6.2 of the model).
- Grant or refuse policy exceptions for a security finding at or above blocking severity (section 3.2 of the model).
- Decide the security and privacy gate where an intent triggers it, naming the reviewer who confirms conformance at step 6, Review.
- Hold the threat model for the agent platform, reviewing it when a model, a surface or a context source is added.
- Approve each addition to the tool allow-list (section 5.2 of the model).
- Hold boundary 4 in section 5.5 of the model and boundary 2 with the data owner.
- Complete the threat model and a privacy assessment of the agent platform before the pilot, and commission penetration testing.

## Decisions this role owns

- The blocking finding severity and who may suppress a finding.
- Whether to grant a policy exception for a security finding, with its owner and expiry.
- The security and privacy gate, including any disputed applicability.
- When to order an immediate reduction in autonomy, without a meeting (section 5.4 of the model).

## Key artefacts

- Keeps the register of the policy exceptions it grants, each also recorded in the release manifest (section 8.1 of the model).
- Records each gate decision in the decision index against the intent it governs.

## Part in the flow and the rhythm

- Step 1, Intent: a delegate joins the data and security position pass where a trigger is met.
- Step 2, Gate: decides the security and privacy gate within its response period.
- Step 5, Verify: a security severity that misses its threshold proceeds only with a policy exception.
- Step 6, Review: the security agent advises at L1, Advise, before the named reviewer confirms conformance.

## Working interfaces

- With the platform owner: approves allow-list additions and holds boundary 1 together.
- With the data owner: holds boundary 2, covering the permitted model list and the approved hosting.
- With the system steward: aligns the threat model with structural governance and control design.
- With the security agent at L1, Advise: uses its findings as inputs and grants any exception itself.

## What this role is not

- It cannot set aside a never-waived control, including the secret and credential rules its own standard sets.
- It does not close every gap itself. The component's owner closes a threat that has no control.
- It does not grant exceptions for data protection. The data owner does.

## Common failure modes

- Publishing security policy as guidance the pipeline cannot enforce.
- Renewing an exception at expiry without evidence that its dependency is being removed.
- Leaving the threat model unchanged when a model, a surface or a context source is added.
- Treating prompt injection as solved because a control exists.

## Measures of effectiveness

- Exception rate per context profile, for security exceptions.
- Open and expired security exceptions.
- Time to decide the gate against its response period.
- Threats in the threat model without a control.

## How this post is likely to change

This is a forecast rather than a requirement of the model, and it is paced by the autonomy an organisation raises on evidence under section 5.4, not by dates. The security lead is likely to express more policy as code that the pipeline enforces and an agent retrieves, and to keep threat models that cover agents and the six trust boundaries. Approval of allow-list additions is likely to take more of the post's time as agents reach more systems. Policy exceptions for security stay with a person at every level of autonomy.

## Strong background fit

- Head of information security.
- Security architect.
- Application security lead.
- Named delegate of the head of information security.

## Good signals in practice

A strong security lead publishes rules the pipeline can enforce and an engineer can read, so most security questions are settled by a check rather than a meeting. Their exceptions are few and current.
