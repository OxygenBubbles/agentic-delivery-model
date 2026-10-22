---
profile_type: human_role
role_id: ADM-H-TEST-ENGINEER
role_name: Test Engineer
mission: Confirm, as a validator, that the acceptance examples describe the correct behaviour by trying to falsify them before implementation.
accountability: validators, for the acceptance examples
steps:
  - 1 Intent, the behaviour pass, the tier check and validation of the acceptance examples
  - 4 Build, answering questions halted runs place on the work item
accountable_to: set by the adopter
works_with:
  - Business Analyst
  - Product Owner
  - Practice Owner for Test
  - Drafting agent (step 1, L2)
---

# Test Engineer

## Role mission

The test engineer holds the validators accountability for the acceptance examples (section 7 of the model). The examples are concrete, business-owned examples of correct behaviour, with edge cases and error paths, written before implementation and held as executable tests. The test engineer validates them by trying to falsify them, so that the checks at step 5, Verify, test what the business needs.

## Why this role matters in the ADM

An error in the acceptance examples has no downstream verification, because the pipeline and the reviewer confirm that the output matches them (section 2.3 of the model). The examples are drafted in a run separate from implementation, but separate runs do not establish independence on their own. The assurance comes from business-owned expected results and from checks derived from evidence the implementer did not supply.

## Primary outcomes

- Each example states a precondition, an action and an expected result the business owns, with its edge cases and error paths.
- Examples that encode a wrong assumption are corrected before publication rather than after release.
- The stated risk tier matches the effect of the change.

## Core accountabilities

- Join the behaviour pass with the requester and the business analyst (section 2.3 of the model).
- Apply the break assessment to the acceptance examples, recording the cases examined and the evidence considered.
- Validate the acceptance examples and record the evidence relied on.
- Act as the one validator for a standard-tier intent where the acceptance examples carry the risk.
- A test engineer who helped shape the intent does not declare it validated alone, including when acting as the standard-tier validator (section 2.3 of the model).
- Check the tier the engineer states, with the higher tier applying where two could (section 2.5 of the model).

## Decisions this role owns

- Whether the examples are concrete, executable and business-owned enough to validate.
- Whether a drafted example survives the break assessment.
- Whether the stated risk tier is right on the criteria in section 2.5 of the model.
- Whether an example needs a threshold over a set of cases because the output is not deterministic (section 6.2 of the model).

## Key artefacts

- Records the validation on the work item with the evidence relied on (appendix A of the model).
- Records each break assessment, including any change it caused.
- Contributes the validated examples, in Gherkin or the Easy Approach to Requirements Syntax, that run as tests at step 5.

## Part in the flow and the rhythm

- Step 1, Intent: joins the behaviour pass, checks the tier and validates the examples.
- Step 4, Build: answers halted runs' questions about the examples.
- Daily sweep: works the clarification queue with the other validators and the product owner.
- Periodic review: takes recurring weaknesses in examples to the practice owner for test.

## Working interfaces

- With the business analyst and the requester: forms the worked examples, with expected results from the business.
- With the product owner: confirms expected results with the owner of the outcome, who accepts against them at step 11.
- With the intent architect: records the validation the intent architect needs before deciding to publish.
- With the drafting agent at L2, Draft: tries to break examples drafted in a run that cannot implement against them.

## What this role is not

- It does not own the expected results. The business does.
- It does not implement a unit whose examples it validated (section 2.4 of the model).
- It does not verify the output of an intent it formed (section 7 of the model).
- It does not set the test standard or grant exceptions to its figures. The practice owner for test does.

## Common failure modes

- Checking that examples are well formed instead of trying to falsify them.
- Accepting expected results an agent inferred rather than ones the business supplied.
- Leaving edge cases and error paths implicit.
- Raising the tier because a platform handles personal or financial data rather than because of the effect of the change.

## Measures of effectiveness

- Acceptance examples changed between draft and publication, used to select records for review.
- First-time validation failure rate.
- Defects after release that a validated example should have described.
- Share of changes in the high tier, as a review trigger.

## How this post is likely to change

This is a forecast rather than a requirement of the model, and it is paced by the autonomy an organisation raises on evidence under section 5.4, not by dates. As agents draft acceptance tests and run more of the checks, the test engineer is likely to spend less time executing tests and more time designing acceptance examples. The post is also likely to build more of the evaluations that raise an agent's level, with the platform owner owning them under section 6.4. Validation of the acceptance examples stays with a person at every level of autonomy, whatever share of the testing agents carry.

## Strong background fit

- Test engineer.
- Test analyst.
- Quality engineer with test design experience.
- Senior quality assurance engineer.

## Good signals in practice

A strong test engineer finds the case the draft missed before a unit is built. Their examples read as the business's own statement of correct behaviour, specific enough that a plausible but wrong assumption fails them.
