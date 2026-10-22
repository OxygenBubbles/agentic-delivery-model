---
profile_type: human_role
role_id: ADM-H-BUSINESS-ANALYST
role_name: Business Analyst
mission: Confirm, as a validator, that the intent expresses the need and the behaviour of the request before anything is built from it.
accountability: validators, for the expression of the need and the behaviour
steps:
  - 1 Intent, formation passes 1, 2 and 4 and validation
  - 4 Build, answering questions halted runs place on the work item
accountable_to: set by the adopter
works_with:
  - Product Owner
  - Test Engineer
  - Intent Architect
  - Drafting agent (step 1, L2)
---

# Business Analyst

## Role mission

The business analyst holds the validators accountability for the expression of the need and the behaviour (section 7 of the model). The post confirms that the intent states what the request needs, in the requester's own terms and as worked examples, before anyone builds from it. The request decision and the outcome stay with the product owner.

## Why this role matters in the ADM

Verification confirms that the output matches the intent, so a change can pass every check and still be the wrong change (principle 3). An agent fills gaps in the intent with plausible assumptions and may generate tests that reproduce a wrong one. Validation before implementation is where a misstated need is found.

## Primary outcomes

- Each intent states the outcome, the measure, the accepting person, the colleagues affected and the exclusions from scope.
- The required behaviour is expressed as worked examples, with edge cases and error paths.
- An intent that does not express the request returns for refinement before any unit is created.
- Questions from halted runs about the need are answered so the runs resume.

## Core accountabilities

- Lead the need pass with the requester and the behaviour pass with the requester and the test engineer (section 2.3 of the model).
- Lead the data and security position pass with the data engineer, joined by the security lead's delegate where a trigger is met.
- Apply the break assessment to every assumption and open question bearing on the need or the behaviour.
- Validate the expression of the need and the behaviour, recording the evidence relied on.
- Act as the one validator for a standard-tier intent where the acceptance examples do not carry the risk.

## Decisions this role owns

- Whether the intent expresses the need and the behaviour of the request.
- Whether the exclusions from scope are complete enough that no omission reads as permission.
- Whether an assumption bearing on the need survives the break assessment.
- When to refer a question to the product owner as a change of scope (section 3.2 of the model).

## Key artefacts

- Records the validation on the work item with the evidence relied on (appendix A of the model).
- Records the decisions of each pass it leads on the work item, for the drafting agent's next revision.
- Records the cases examined in each break assessment.

## Part in the flow and the rhythm

- Step 1, Intent: leads its passes, then validates. Validation is proportionate to tier, and no one who shaped the intent declares it validated alone.
- Step 4, Build: answers halted runs' questions about the need or the behaviour.
- Daily sweep: works the clarification queue with the other validators and the product owner.

## Working interfaces

- With requesters and business owners: draws out the need in their own terms.
- With the product owner: records the decisions on the outcome and its measure.
- With the test engineer: forms the worked examples together, leaving their validation to the test engineer.
- With the intent architect: provides the need and the behaviour the shape builds on.
- With the drafting agent at L2, Draft: corrects drafts that infer the need rather than state it.

## What this role is not

- It is not the owner of the request decision, the outcome or business priority.
- It does not validate the acceptance examples. The test engineer does.
- It is not a writer of requirements documents separate from the intent.
- It is not a gate owner or the acceptor of the delivered work.

## Common failure modes

- Validating a draft because it reads fluently rather than because it states the need.
- Letting a drafted assumption stand as a fact.
- Answering a halted run with a change to the required behaviour without revalidation and republication.
- Holding validation with no named backup, so intents wait.

## Measures of effectiveness

- First-time validation failure rate.
- Stop-condition rate, read for questions about the need.
- Human effort per intent.
- Lead time from approved request to live, read with the wait for validation.

## How this post is likely to change

This is a forecast rather than a requirement of the model, and it is paced by the autonomy an organisation raises on evidence under section 5.4, not by dates. As agents draft more of each intent, the business analyst is likely to spend less time writing requirements and more on validating intents, running the formation passes and testing the need with the business. Senior business analysts are also one likely source of intent architects as demand for that post rises. Validation stays with a person at every level of autonomy, and a lengthening validation queue is one signal that the forecast is arriving.

## Strong background fit

- Business analyst.
- Senior business analyst with domain depth.
- Requirements engineer.
- Process analyst.

## Good signals in practice

A strong business analyst produces intents that a requester recognises as their own need and that an engineer can read without asking what was meant. Their validations record what was checked and against what evidence.
