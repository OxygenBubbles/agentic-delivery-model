---
profile_type: human_role
role_id: ADM-H-INTENT-ARCHITECT
role_name: Intent Architect
mission: Translate business need into an executable Intent Specification that is feasible in scope, bounded and precise enough for governed agent delivery.
primary_tiers:
  - design
  - governance
accountable_to: Delivery Lead
works_with:
  - Product Owner
  - Requirements Agent
  - Verification Engineer
  - System Steward
---

# Intent Architect

## Role mission

The Intent Architect exists because autonomous delivery punishes ambiguity. This role converts business need into execution-ready intent. It does not exist to produce abstract design documents. It exists to make the next change precise enough for an agent to deliver it safely and for a person to verify it confidently.

## Why this role matters in the ADM

In a traditional team, developers often recover from incomplete requirements through conversation, inference and experience. In the ADM, that recovery path is weaker and more expensive. The Intent Architect closes that gap before implementation begins.

## Primary outcomes

- Intent Specifications enter Ready state with clear boundaries, acceptance criteria and non-functional expectations.
- Work is split into units that are small enough for reliable agent execution and meaningful human verification.
- Clarification loops during implementation are reduced because the important decisions were made early.
- Feasibility and scope questions are resolved without pushing hidden ambiguity downstream.

## Core accountabilities

- Shape draft requirements into execution-ready Intent Specifications.
- Assess feasibility and scope before work enters the Ready backlog.
- Define delivery boundaries, integration points, data implications and non-functional expectations.
- Tighten acceptance criteria so the Verification Engineer can test the right thing.
- Decompose work that is too broad, risky or coupled for reliable execution.
- Respond to clarification requests that relate to feasibility, scope or boundary conditions.
- Surface structural concerns to the System Steward when the requested change exposes wider issues.

## Decisions this role owns

- Whether a specification is sufficiently bounded to enter the Ready backlog.
- Whether a requested change should be split, sequenced, deferred or rejected.
- Which constraints and assumptions must be made explicit before delivery starts.
- Which clarifications can be resolved quickly and which require further stakeholder discovery.
- When a problem should be escalated to the Product Owner, System Steward or Delivery Lead.

## Key artefacts

- Owns the execution readiness of the Intent Specification.
- Contributes to the Definition of Ready judgement.
- Records scope decisions, boundary conditions and implementation clarifications.
- Feeds recurring structural concerns into the Coherence Register through the System Steward.

## Cadence participation

- Design work: active throughout the day as specifications are shaped and refined.
- Governance Tier: available to answer clarification questions and explain intent choices.
- Strategic Tier: contributes patterns, recurring specification failures and decomposition lessons.

## Working interfaces

- With the Product Owner: turns business need into precise executable intent without taking over business priority.
- With the Requirements Agent: sharpens discovery output into something bounded and testable.
- With the Verification Engineer: aligns acceptance criteria, edge cases and evidence expectations.
- With the System Steward: escalates recurring structural issues and incorporates updated constraints.
- With implementation agents: clarifies intent when the specification is no longer sufficient.

## What this role is not

- It is not a general project coordination role.
- It is not a substitute Product Owner.
- It is not the owner of system-wide standards.
- It is not a code review role, except where clarification is required.

## Common failure modes

- Writing specifications that are detailed but still not bounded.
- Leaving difficult edge cases implicit because they are awkward to resolve.
- Allowing urgency to push work into execution before it is ready.
- Becoming a bottleneck because all decomposition and clarification depends on one person.
- Drifting into abstract solution writing rather than practical execution shaping.

## Measures of effectiveness

- Clarification rate during implementation.
- First-pass acceptance rate for work shaped by the role.
- Rework caused by specification gaps.
- Cycle time by complexity band.
- Percentage of Ready work that later proves to have hidden scope.

## Strong background fit

- Senior engineer.
- Lead developer.
- Solution architect.
- Technical architect.
- Senior business analyst with strong technical depth.

## Good signals in practice

A strong Intent Architect produces specifications that feel unsurprising when they reach verification. The work is bounded, the difficult cases have been considered and downstream teams do not need to reverse-engineer what was really meant.
