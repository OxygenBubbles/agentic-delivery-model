---
profile_type: human_role
role_id: ADM-H-SYSTEM-STEWARD
role_name: System Steward
mission: Preserve structural consistency, coherence and long-term operability as agents optimise locally for speed and task completion.
primary_tiers:
  - strategic
  - governance
accountable_to: Delivery Lead
works_with:
  - Intent Architect
  - Verification Engineer
  - Platform Engineer
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

- Own structural consistency across the codebase and service landscape.
- Maintain the standards, constraints and preferred patterns that agents should work within.
- Own the Coherence Register as the live record of drift, debt and remediation needs.
- Decide whether identified debt is accepted, deferred or remediated.
- Commission refactoring work where the long-term shape of the system is being damaged.
- Review recurring implementation and verification findings for wider patterns.
- Partner with security leadership on the agent threat model and control design.

## Decisions this role owns

- Which patterns are standard and which are exceptions.
- Which debt items can be tolerated and which must be remediated.
- When a local issue signals wider structural drift.
- When standards need to change because the system or delivery model has changed.
- When refactoring work should displace feature work.

## Key artefacts

- Owns the Coherence Register.
- Owns system-level standards, guardrails and remediation priorities.
- Produces refactoring Intent Specifications and structural guidance.
- Records exceptions and rationale where temporary divergence is accepted.

## Cadence participation

- Governance Tier: advises on escalated structural concerns and difficult trade-offs.
- Strategic Tier: leads coherence review, standards updates and debt prioritisation.
- Between cadences: maintains standards and commissions remediation work.

## Working interfaces

- With the Intent Architect: provides constraints and receives early warnings about system tension.
- With the Verification Engineer: turns recurring review concerns into structural action.
- With the Platform Engineer: aligns standards with tooling, environments and control enforcement.
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

- Technical debt trend.
- Structural consistency across services, modules and interfaces.
- Reuse of approved patterns.
- Volume and age of open coherence issues.
- Closure rate of remediation items.

## Strong background fit

- Solution architect.
- Technical architect.
- Principal engineer.
- Enterprise architect with delivery depth.

## Good signals in practice

A strong System Steward can explain the system's current shape, where it is drifting and which interventions matter now. Their standards are specific enough to guide work and flexible enough to remain usable.