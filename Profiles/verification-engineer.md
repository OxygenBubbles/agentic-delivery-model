---
profile_type: human_role
role_id: ADM-H-VERIFICATION-ENGINEER
role_name: Verification Engineer
mission: Confirm, as verifier, that a change produced by someone else conforms to the intent and the standards, satisfies the acceptance examples and preserves structural consistency before it merges.
accountability: verifier
steps:
  - 5 Verify, reading the evidence
  - 6 Review
accountable_to: set by the adopter
works_with:
  - Intent Architect
  - System Steward
  - Product Owner
  - Review agents at L1 for code, security and design
---

# Verification Engineer

## Role mission

The Verification Engineer holds the verifier accountability at step 6, Review, of the flow. This role does not exist to repeat what the deterministic checks at step 5 have already done. It exists to decide whether the delivered change conforms to the intent, whether the evidence is strong enough and whether the result is safe to merge. Whether the intent was the right change was decided at validation, before implementation.

## Why this role matters in the ADM

Agentic delivery can generate a large volume of technically plausible output. The Verification Engineer is the role that turns automated output into trusted delivery by applying judgement that is still hard to automate reliably.

## Primary outcomes

- Only changes that satisfy the intent and its acceptance examples merge.
- Weak tests, hidden regressions and misleading evidence are challenged before merge.
- Structural consistency concerns are surfaced early rather than normalised in the codebase.
- Human approval remains meaningful rather than ceremonial.

## Core accountabilities

- Review delivered changes against the intent and the standards rather than against personal preference.
- Check that acceptance examples are actually met in behaviour, not only implied in code.
- Assess test evidence for depth, relevance and realism.
- Perform exploratory or scenario-based checking where automation is not enough.
- Identify hidden debt, brittle implementation choices and unexamined edge cases.
- Approve and merge only as a code owner for the affected path, on a change they did not produce and whose intent they did not form.
- Reject the change or request rework with clear reasoning.
- Escalate structural concerns to the System Steward and business ambiguity to the Product Owner or Intent Architect.

## Decisions this role owns

- Whether the change should merge, be reworked or be escalated.
- Whether the test evidence is sufficient for the stated risk and scope.
- Whether the change is behaviourally correct even when the code looks plausible.
- Whether a concern is local rework or a wider coherence issue.

## Key artefacts

- Owns the review decision at step 6 and the review record in the decision trail (section 8.1 of the model).
- Produces acceptance evidence, rework requests and defect feedback.
- Contributes recurring failure patterns to standards, test strategy and specification guidance.

## Part in the flow and the rhythm

- Step 6, Review: reviews output as it arrives, with the daily sweep of the review queue as the minimum.
- Between reviews: monitors queue age, urgent rework and stop conditions.
- Periodic review: feeds recurring weaknesses in tests, intents and agent behaviour into the measures and the autonomy levels.

## Working interfaces

- With the Intent Architect: checks whether the delivered behaviour matches the specified intent.
- With the Product Owner: escalates where business acceptance is unclear or the requested behaviour appears wrong.
- With the System Steward: escalates structural consistency concerns and recurring drift.
- With review agents at L1: uses their findings as inputs, not as substitutes for judgement.

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

- Change failure rate and time to restore.
- Human effort per intent, including review and rework.
- Age of the review queue.
- Accuracy of merge decisions over time.

## How this post is likely to change

This is a forecast rather than a requirement of the model, and it is paced by the autonomy an organisation raises on evidence under section 5.4, not by dates. Review is likely to constrain throughput where agent output grows faster than review capacity. The constraint may instead sit in intent formation, validation, clarification or environments, and it can differ between classes of change. The post is likely to concentrate senior judgement as demand for review skill grows. Review agents support the post by commenting at L1, Advise, without approving, and approval of someone else's work stays with a person at every level of autonomy. A growing validation or review queue signals a capacity constraint. It supports the forecast only where comparable work needs less human effort without a worse change failure rate or time to restore. The stop-condition rate is read against changes in scope and the reasons runs stop.

## Strong background fit

- Senior software engineer.
- Senior QA engineer.
- Tech lead.
- Engineer with broad system understanding and good judgement.

## Good signals in practice

A strong Verification Engineer can explain why a change is safe or unsafe in a few precise sentences. They neither default to trust nor default to distrust. They use evidence, scenario thinking and system judgement.