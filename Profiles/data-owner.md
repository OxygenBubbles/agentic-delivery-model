---
profile_type: human_role
role_id: ADM-H-DATA-OWNER
role_name: Data Owner
mission: Own the data definitions, masking and the personal data position, deciding the data gate where an intent triggers it and the policy exceptions for data protection.
accountability: data owner
steps:
  - 2 Gate where triggered, the data gate
  - 3 Workspace, through the masking rule the refresh jobs apply
  - 5 Verify, the data checks and policy exceptions for data protection
accountable_to: set by the adopter
works_with:
  - Security Lead
  - Platform Owner
  - Practice Owner for Test
  - Privacy Adviser
---

# Data Owner

## Role mission

The data owner holds the data owner accountability (section 7 of the model): data definitions, masking, the personal data position and policy exceptions for data protection, advised by a privacy adviser. The post is usually the head of data or a data governance lead. The standards it owns or shares set the masking and classification checks that production-derived data passes before it enters a workspace, a prompt or an output.

## Why this role matters in the ADM

Data an agent reads as context could leave a controlled environment through a prompt, an output or the repository. A model is used only where its hosting meets the organisation's data position (control 6), and an agent does not move data or secrets out of a controlled environment (control 9). The categories of data that may never leave production are one of the six never-waived controls.

## Primary outcomes

- Workspaces hold masked data refreshed to the rule for their environment class.
- The categories of data that may never leave production are stated, enforced and never excepted.
- An intent that meets a data trigger, such as personal data, a new source or retention, is decided at the data gate.
- Each data protection exception is time-limited, owned and recorded in the manifest.

## Core accountabilities

- Own the data definitions and quality standard, including the restatement rule for a published metric (section 6.2 of the model).
- Own the test data and masking standard with the practice owner for test.
- Decide the data gate where an intent triggers it, naming the reviewer who confirms conformance at step 6, Review.
- Grant or refuse policy exceptions for data protection (section 3.2 of the model).
- Hold boundary 5 in section 5.5 of the model with the platform owner and boundary 2 with the security lead.
- Approve, with the platform owner, an increase in autonomy on a matter in its remit (section 5.4 of the model).

## Decisions this role owns

- The data gate decision, including any disputed applicability.
- Whether to grant a policy exception for data protection, with its owner and expiry.
- The masking rule for each environment class, with the practice owner for test.
- Whether a standing decision under the data definitions standard covers a familiar case, as in the worked example of a change.

## Key artefacts

- Records each data gate decision in the decision index against the intent it governs (section 8.1 of the model).
- Keeps the register of the policy exceptions it grants, each also recorded in the release manifest.

## Part in the flow and the rhythm

- Step 1, Intent: the business analyst and the data engineer form the data and security position the data gate works from.
- Step 2, Gate: decides the data gate within its response period.
- Step 3, Workspace: pipeline jobs no agent runs refresh each workspace with masked data.
- Step 5, Verify: the data checks run, and an exception for data protection comes to the data owner.

## Working interfaces

- With the security lead: holds boundary 2, covering the permitted model list and the approved hosting.
- With the platform owner: holds boundary 5, relying on masking that runs as logged pipeline jobs.
- With the practice owner for test: owns the test data and masking standard together.
- With the privacy adviser: takes advice on the personal data position and on exceptions.

## What this role is not

- It cannot except the categories of data that may never leave production.
- It does not decide the security and privacy gate. The security lead does.
- It is not among the accountabilities that may order an immediate reduction in autonomy (section 5.4 of the model).

## Common failure modes

- Writing a masking rule that no pipeline job can check.
- Letting the list of data that may never leave production fall behind new sources.
- Treating every data trigger as a new decision where a standing decision already covers it.
- Leaving connected contexts with inconsistent masked data, so end-to-end tests read different states.

## Measures of effectiveness

- Exception rate per context profile, for data protection exceptions.
- Open and expired data protection exceptions.
- Time to decide the data gate against its response period.
- Failures of the masking and classification checks.

## How this post is likely to change

This is a forecast rather than a requirement of the model, and it is paced by the autonomy an organisation raises on evidence under section 5.4, not by dates. As agents read more context, the data position for models, masking for agent context and the categories of data that never leave production are likely to become recurring decisions rather than occasional ones. Policy exceptions for data protection and the data gate stay with a person at every level of autonomy, because the agent holds no accountability.

## Strong background fit

- Head of data.
- Data governance lead.
- Data architect with governance responsibility.
- Data steward for a business domain.

## Good signals in practice

A strong data owner can state what data each environment class holds and how it was masked. Their standing decisions settle familiar cases quickly, so the gate spends its time on new ones.
