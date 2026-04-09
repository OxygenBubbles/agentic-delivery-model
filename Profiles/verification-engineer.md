---
profile_type: human_role
role_id: ADM-H-VERIFICATION-ENGINEER
role_name: Verification Engineer
mission: Confirm that delivered change meets the stated intent, satisfies the acceptance criteria and preserves structural consistency before it is accepted.
primary_tiers:
  - governance
  - execution_support
accountable_to: Delivery Lead
works_with:
  - Intent Architect
  - System Steward
  - Product Owner
  - Review Agent
  - Security Agent
---

# Verification Engineer

## Role mission

The Verification Engineer is the quality gate in the ADM. This role does not exist to repeat what automated checks have already done. It exists to decide whether the delivered change is the right change, whether the evidence is strong enough and whether the result is safe to accept.

## Why this role matters in the ADM

Agentic delivery can generate a large volume of technically plausible output. The Verification Engineer is the role that turns automated output into trusted delivery by applying judgement that is still hard to automate reliably.

## Primary outcomes

- Only changes that satisfy the intent and acceptance criteria are accepted.
- Weak tests, hidden regressions and misleading evidence are challenged before merge.
- Structural consistency concerns are surfaced early rather than normalised in the codebase.
- Human approval remains meaningful rather than ceremonial.

## Core accountabilities

- Review delivered changes against the Intent Specification rather than against personal preference.
- Check that acceptance criteria are actually met in behaviour, not only implied in code.
- Assess test evidence for depth, relevance and realism.
- Perform exploratory or scenario-based checking where automation is not enough.
- Identify hidden debt, brittle implementation choices and unexamined edge cases.
- Approve, reject or request rework with clear reasoning.
- Escalate structural concerns to the System Steward and business ambiguity to the Product Owner or Intent Architect.

## Decisions this role owns

- Whether the change should be accepted, reworked or escalated.
- Whether the test evidence is sufficient for the stated risk and scope.
- Whether the change is behaviourally correct even when the code looks plausible.
- Whether a concern is local rework or a wider coherence issue.

## Key artefacts

- Owns the verification decision and associated review notes.
- Produces acceptance evidence, rework requests and defect feedback.
- Contributes recurring failure patterns to standards, test strategy and specification guidance.

## Cadence participation

- Governance Tier: primary working forum for review, approval and escalation.
- Execution support: monitors queue health, urgent rework and clarification loops.
- Strategic Tier: feeds recurring weaknesses in tests, specs and agent behaviour back into the model.

## Working interfaces

- With the Intent Architect: checks whether the delivered behaviour matches the specified intent.
- With the Product Owner: escalates where business acceptance is unclear or the requested behaviour appears wrong.
- With the System Steward: escalates structural consistency concerns and recurring drift.
- With review, testing and security agents: uses their findings as inputs, not as substitutes for judgement.

## What this role is not

- It is not a manual regression factory.
- It is not a style enforcer for issues already covered by automated review.
- It is not a second Intent Architect rewriting the scope after implementation.
- It is not a rubber stamp at the end of a pipeline.

## Common failure modes

- Approving changes because the automated checks were green rather than because the evidence was convincing.
- Over-indexing on code style while missing behaviour risk.
- Turning the role into manual QA execution rather than verification judgement.
- Rejecting work without giving actionable reasons.
- Becoming the only person who understands where quality is actually failing.

## Measures of effectiveness

- Escaped defect rate.
- Rework rate after verification.
- First-pass approval quality.
- Review queue health.
- Accuracy of acceptance decisions over time.

## Strong background fit

- Senior software engineer.
- Senior QA engineer.
- Tech lead.
- Engineer with broad system understanding and good judgement.

## Good signals in practice

A strong Verification Engineer can explain why a change is safe or unsafe in a few precise sentences. They neither default to trust nor default to distrust. They use evidence, scenario thinking and system judgement.