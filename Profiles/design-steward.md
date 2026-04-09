---
profile_type: human_role
role_id: ADM-H-DESIGN-STEWARD
role_name: Design Steward
mission: Preserve design coherence, accessibility and user experience quality as agents generate user-facing interfaces at scale.
primary_tiers:
  - strategic
  - governance
accountable_to: Delivery Lead
works_with:
  - System Steward
  - Intent Architect
  - Verification Engineer
  - Product Owner
---

# Design Steward

## Role mission

The Design Steward protects the quality and consistency of the user experience across the product. In the ADM, agents can generate UI quickly, but without codified design constraints they produce generic, inconsistent interfaces. The Design Steward makes design decisions explicit, codifies them as constraints and ensures agent output meets the standard.

## Why this role matters in the ADM

Agents are local optimisers. Given a specification, they will produce a working interface that satisfies the acceptance criteria. But they have no inherent sense of visual consistency, interaction coherence or brand alignment across screens. Without explicit design governance, a product built by multiple agents working on separate specifications will drift towards inconsistency in the same way the codebase drifts without codified technical standards.

## Primary outcomes

- The product presents a consistent, coherent user experience across all surfaces.
- Design constraints are codified and enforceable, not aspirational.
- Accessibility standards are met as a baseline, not an afterthought.
- Agent-generated UI is reviewed against design criteria before release.
- The design system evolves in step with the product.

## Core accountabilities

- Own the design system: component library, interaction patterns, layout conventions, accessibility standards and brand rules.
- Codify design constraints in agent configuration files and specification templates.
- Maintain the Design Register as the live record of design decisions, component usage patterns and known deviations.
- Review agent-generated UI for visual consistency, interaction correctness and accessibility compliance.
- Define and update Design Constraints sections in Intent Specifications for work with a user-facing surface.
- Commission design remediation work when drift accumulates.

## Decisions this role owns

- Which components and patterns are standard and which are exceptions.
- When a local design deviation signals wider inconsistency.
- When the design system needs to change because the product or user needs have changed.
- What level of accessibility compliance is required and where exceptions are tolerated.
- When design remediation should displace feature work.

## Key artefacts

- Owns the Design Register.
- Owns the design system documentation, component library and interaction pattern references.
- Produces the Design Constraints sections for Intent Specifications.
- Records exceptions and rationale where temporary design divergence is accepted.

## Cadence participation

- Governance Tier: advises on escalated design concerns and reviews agent-generated UI.
- Strategic Tier: leads design system review, updates constraints and prioritises design debt.
- Between cadences: maintains the design system and reviews delivered work.

## Working interfaces

- With the System Steward: aligns design governance with architectural governance. Shared interest in consistency, different lenses.
- With the Intent Architect: provides design constraints during specification and receives early warnings about UX tension.
- With the Verification Engineer: defines design acceptance criteria and supports UI verification.
- With the Product Owner: translates product intent into design direction. Receives context from UX research and user testing conducted outside the ADM.

## What this role is not

- It is not a UX researcher. Research and user testing sit outside the ADM. Their output feeds in as context.
- It is not the owner of every visual decision in every specification. Agents should work within the design system without requiring approval for every screen.
- It is not a blocker whose default answer is no.
- It is not a substitute for product thinking or user research.

## Common failure modes

- Treating the design system as static doctrine rather than a living tool.
- Becoming a bottleneck by reviewing every screen manually instead of codifying constraints that agents can follow.
- Accepting too much visual debt in the name of speed.
- Overreacting to minor inconsistencies and paralysing delivery.
- Failing to update the design system as the product evolves.

## Measures of effectiveness

- Design consistency across delivered screens and components.
- Accessibility compliance rate.
- Design debt trend (tracked in the Design Register).
- First-pass design acceptance rate of agent-generated UI.
- Component reuse rate.

## Strong background fit

- UX designer or lead.
- Design system lead.
- Front-end architect with strong design sensibility.
- Product designer with governance experience.

## Good signals in practice

A strong Design Steward can explain the current state of the design system, where it is drifting and which interventions matter now. Their constraints are specific enough to guide agent output and flexible enough to remain usable. Agents working within their design system produce UI that looks intentional, not generated.

## When this role applies

This role is optional. It applies only when the product has a user-facing surface. In smaller teams, a single person may hold both System Steward and Design Steward responsibilities if they have both skill sets. In larger teams or design-intensive products, this is a dedicated role.
