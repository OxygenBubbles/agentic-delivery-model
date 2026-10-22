---
profile_type: human_role
role_id: ADM-H-SYSTEM-STEWARD
role_name: System Steward
mission: Preserve structural consistency, coherence and long-term operability as agents optimise locally for speed and task completion.
accountability: system steward, plus design coherence unless a Design Steward is appointed
steps:
  - 2 Gate where triggered, the architecture gate
  - 6 Review, for cross-cutting changes
  - 11 Confirm and learn
accountable_to: set by the adopter
works_with:
  - Intent Architect
  - Verification Engineer
  - Platform Owner
  - Security Lead
---

# System Steward

## Role mission

The System Steward protects the shape of the system over time. In the ADM, agents are very good at solving the task in front of them. They are much less reliable at protecting long-term coherence unless that coherence is made explicit and actively governed.

## Why this role matters in the ADM

Without stewardship, agentic delivery can create fast local optimisation and slow system decay. The System Steward provides the judgement, standards and remediation discipline that stop the codebase becoming harder to change with every successful delivery.

## Primary outcomes

- The system remains understandable, operable and consistent as delivery volume increases.
- Technical debt is governed explicitly rather than accumulated by default.
- Teams have usable standards and guardrails rather than vague aspirations.
- Refactoring and remediation work are commissioned before drift becomes normal.

## Core accountabilities

- Own structural consistency across the codebase and the service estate.
- Own the architecture decisions and contracts standard, and keep each standard to one source in enforceable form (section 6.2 of the model).
- Own the coherence register (Appendix D of the model) as the live record of decisions, debt and coherence risk.
- Decide whether identified debt is accepted, deferred or remediated.
- Commission refactoring work where the long-term shape of the system is being damaged.
- Review recurring implementation and verification findings for wider patterns.
- Partner with security leadership on the agent threat model and control design.
- Hold design coherence unless a Design Steward is appointed.

## Decisions this role owns

- Which patterns are standard and which are exceptions.
- Which debt items can be tolerated and which must be remediated.
- When a local issue signals wider structural drift.
- When standards need to change because the system or delivery model has changed.
- When refactoring is needed to restore coherence, raised as a request under section 2.2 of the model.

## Key artefacts

- Owns the coherence register.
- Owns system-level standards, guardrails and remediation priorities.
- Raises requests for remediation that enter the flow as intents and produces structural guidance.
- Records exceptions with an owner and an expiry where temporary divergence is accepted (section 3.2 of the model).

## Part in the flow and the rhythm

- Step 2: decides the architecture gate where an intent triggers it.
- Step 6: advises on escalated structural concerns and difficult trade-offs.
- Periodic review: leads the review of the coherence register, standards, autonomy levels and open exceptions.
- Between reviews: maintains standards and raises remediation requests.

## Working interfaces

- With the Intent Architect: provides constraints and receives early warnings about system tension.
- With the Verification Engineer: turns recurring review concerns into structural action.
- With the platform owner: aligns standards with the engineering platform so that each standard is enforced in the pipeline (section 6.3 of the model).
- With the Security Lead: aligns structural governance with the threat model and control posture.

## What this role is not

- It is not a detached architecture review board.
- It is not the owner of every design decision in every specification.
- It is not a blocker whose default answer is no.
- It is not a substitute for delivery leadership or product priority.

## Common failure modes

- Becoming abstract and distant from actual delivery pressure.
- Treating standards as static doctrine rather than living guardrails.
- Accepting too much debt in the name of speed.
- Overreacting to every inconsistency and paralysing delivery.
- Failing to convert observed drift into concrete remediation work.

## Measures of effectiveness

- Code health or debt ratio.
- Exception rate per context profile.
- Share of changes in the high tier.
- Structural consistency across services, modules and interfaces.
- Volume and age of open coherence register entries.

## How this post is likely to change

This is a forecast rather than a requirement of the model, and it is paced by the autonomy an organisation raises on evidence under section 5.4, not by dates. Standards in enforceable form are likely to become the context agents retrieve at the point of use, so the steward's work is likely to move from writing guidance to maintaining that context. The coherence register is likely to take more of the post's time as change volume rises. Coherence keeps a named owner at every level of autonomy, because agents optimise locally and hold no accountability.

## Strong background fit

- Solution architect.
- Technical architect.
- Principal engineer.
- Enterprise architect with delivery depth.

## Good signals in practice

A strong System Steward can explain the system's current shape, where it is drifting and which interventions matter now. Their standards are specific enough to guide work and flexible enough to remain usable.