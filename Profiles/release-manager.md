---
profile_type: human_role
role_id: ADM-H-RELEASE-MANAGER
role_name: Release Manager
mission: Own the release manifest and its ordering and give the go or no-go at step 9 on a change the post did not request, build or implement.
accountability: release authority
steps:
  - 1 Intent, the operational position where no context profile covers the recovery position
  - 9 Authorise
  - 10 Deploy, deciding any departure from the named recovery action
accountable_to: set by the adopter
works_with:
  - Operations Lead
  - Product Owner
  - Platform Owner
  - Readiness run (step 9, L3)
---

# Release Manager

## Role mission

The release manager holds the release authority accountability (section 7 of the model), within the production change authority of the operations lead. The post owns the release manifest and its ordering and gives the go or no-go at step 9, Authorise, on a change it did not request, build or implement. It authorises a release, one or more release candidates assembled together, rather than a change.

## Why this role matters in the ADM

A person authorises every production change (principle 5), and production deployment stays at L0, Not used. By step 9 the evidence is bound to each release candidate and a run has computed readiness into the manifest at L3, Execute under gate. What remains is a person's judgement on whether that evidence, the declared order and the recovery positions are sufficient.

## Primary outcomes

- Every production release carries a recorded go or no-go from a person independent of the change.
- The manifest names the exact candidates, their recovery positions, any exception and the recovery action and its trigger.
- A failed deployment meets a recovery action named before deployment rather than an improvised one.
- Each product's release rhythm follows its release frequency, change failure rate and time to restore.

## Core accountabilities

- Decide the go or no-go once the change authorisation is current, recorded separately from it (section 3.1 of the model).
- Confirm that each candidate has an established recovery position, exercised below production first for a high-tier change (section 4 of the model).
- Give the go or no-go on catalogue changes and feature flag activations as on every release.
- Declare the pause when a product spends its error budget and agree resumption with the product owner (section 3.2 of the model).
- Hold boundary 6 in section 5.5 of the model, between non-production and production.

## Decisions this role owns

- Whether a release goes, on the evidence in the manifest.
- Whether to depart from the named recovery action after a failed deployment, or fix forward by the emergency route.
- Whether an authorised recovery uses the manual fallback route (section 6.4 of the model).
- When to order an immediate reduction in an agent's autonomy (section 5.4 of the model).

## Key artefacts

- Owns the release manifest, with both decisions at step 9 recorded in it.
- Owns the change records and authorisations, the pipelines and deployment history and the software bills of materials (section 8.1 of the model).
- Owns the standard for release candidates, environments and the manifest with the platform owner.

## Part in the flow and the rhythm

- Step 1, Intent: joins the operational position pass where no context profile covers the recovery position.
- Step 9, Authorise: reads the manifest the readiness run computed and decides.
- Step 10, Deploy: if a deployment fails, the named recovery action executes on its trigger and the release manager decides any departure from it.

## Working interfaces

- With the operations lead: works within the production change authority the operations lead holds.
- With the product owner: sets the release rhythm and agrees resumption after an error-budget pause.
- With the platform owner: declares the error-budget pause, and the platform owner then applies the reduction in autonomy by reissuing identities at the lower scope.
- With the readiness run at L3, Execute under gate: reads what the run computes and takes the decision no agent may take.

## What this role is not

- It does not authorise a change it requested, built or will implement. That decision passes to the next holder of the same authority.
- It does not grant exceptions, and an expired exception blocks any candidate still depending on it.
- It is not a release meeting or a second code review.

## Common failure modes

- Authorising because the manifest assembled rather than because its evidence was read.
- Accepting a high-tier recovery position that was declared but not exercised.
- Treating a missed cut-off or a commitment date as an emergency.
- Holding the authority with no named backup, so releases stall or segregation bends.

## Measures of effectiveness

- Change failure rate.
- Time to restore.
- Release frequency per product.
- Exception rate per context profile.

## How this post is likely to change

This is a forecast rather than a requirement of the model, and it is paced by the autonomy an organisation raises on evidence under section 5.4, not by dates. As readiness is computed into the manifest at step 9, the coordination in the post is likely to fall. The post is likely to become a judgement on risk and on recovery positions, held by fewer and more senior people. The go or no-go stays with a person at every level of autonomy, and an organisation reads its own change failure rate and time to restore before reshaping the post.

## Strong background fit

- Release manager.
- Change manager.
- Service delivery manager with release and recovery experience.
- Senior engineer with deployment and recovery experience.

## Good signals in practice

A strong release manager can say in a few sentences why a release is ready or not, naming the evidence and the recovery position that decided it. When a release fails, the recovery runs as it was named at step 9.
