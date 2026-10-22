# The journey of a change: a worked example of the Agentic Delivery Model

This example is a companion to the model. It follows one change through the eleven steps of the flow and shows at each step who does what, the agent's part at its level, the decision taken and the evidence left behind. The setting is a fictional mid-sized insurance company with conduct and data protection obligations that runs a customer self-service portal: a web application, with an integration service connecting it to the policy administration system. The example illustrates the model and does not extend it, and where it is silent the model governs.

## The change

A customer who submits an amendment request receives a reference number and hears nothing further until the amendment completes, so the contact centre takes a steady volume of calls asking about progress. Request 2031 asks for the progress of an amendment request to be shown on the policy page, records the expected benefit as a reduction in those calls and names the contact centre manager as business owner.

The change touches two delivery contexts. The integration service gains an operation that reads the status of a customer's amendment requests from the policy administration system and returns one of four customer-facing states: received, under review, complete and not completed. The web application gains a progress panel on the policy page that calls the operation and shows the state. The intent carries two execution units with a versioned contract between them: the service produces version 2.3 of its amendment contract by adding the operation without altering any existing one, and the web application consumes it. That contract is internal to the portal, consumed only by the web application, so it is not a published interface in the sense of the high-tier criteria, and the published interface of the policy administration system is untouched. The engineer states the standard tier and the test engineer confirms it. The change alters no monetary calculation, collects no new personal data, changes no permission and changes no outcome of the customer's journey, only what the customer can see of its progress. The panel does add content to a customer journey, so the customer outcome gate applies at step 2.

## The eleven steps

### Step 1, Intent

The product owner takes the request decision on business merit. A headless run supplies the evidence first, at L1, Advise: no earlier request asked for this, no existing capability shows progress and the last changes to the journey produced no incident. The product owner approves, capacity is confirmed against the cap on approved work and the decision is recorded on work item PORTAL-412 against request 2031. The six formation passes then run in one refinement session and in exchanges on the work item, with a run redrafting after each pass. The business analyst validates that the need and the behaviour express the request. The test engineer validates the acceptance examples under the break assessment and changes one: the draft showed a declined amendment as complete, and the signed version shows it as not completed. The intent architect decides the shape: two units with the web application leading, the contract version each produces or consumes and the deployment order. The intent architect also records the trigger screening, naming the data and customer outcome gates as the triggers met. The operational position records proven rollback for both units and the delegation boundary lists the stop conditions. With both validations recorded the pipeline publishes version 1 of the intent to the web repository through a pull request the code owners approve, and the workflow holds the working copy read-only.

### Step 2, Gate where triggered

A gate applies only when its trigger is met, and this change meets two triggers. At L1, Advise, a run reads the published intent against the trigger list and assembles the material. The operation follows the integration pattern the service already uses, so no architecture trigger is met. The new state on the policy page shows the customer personal data about their own amendment, so the data gate applies. It carries no personal data beyond what the customer already sees and changes neither classification nor retention. The data owner therefore records that the standing decision under the data definitions standard covers it and names the reviewer who confirms conformance at step 6. The change touches no permission, external flow, shared environment, monetary calculation or published term. The four state labels are new content in a customer journey, so the customer outcome gate applies. The holder of the accountable executive's recorded delegation for customer communications approves the wording within two working days and names the reviewer who confirms it on the pull request at step 6. The decision is recorded against the intent version, with the screening of the other triggers, and the intent proceeds.

### Step 3, Workspace

The pipeline allocates an isolated workspace per execution unit from the versioned environment definition in each context profile, with the step at L0, Not used. The web unit receives a short-lived branch from the protected destination branch of the web repository and a sandbox holding the service's contract stub at version 2.3. The service unit receives a branch and a sandbox connected to the test instance of the policy administration system, refreshed with masked data to the published rule by a pipeline job that no agent runs. Each run is issued a scoped run identity for that run alone, with its retrieval, commenting and execution permissions authorised separately. The platform owner's team provides the environments, and the evidence is the environment record for each workspace with its refresh position.

### Step 4, Build

Two runs start, one per unit, each reading version 1 of the intent from the repository and never the working copy. The agent implements at L2 under its run identity, within the tool allow-list and under deterministic checks it cannot disable. The service run completes without a halt. The web run reaches a stop condition the intent set: the progress panel does not fit the policy page at the smallest supported viewport without a change to the shared page layout, and the scope boundary excludes that component. The run halts, records its state and the pipeline posts the question on the work item. The intent architect records that collapsing the panel into the page's existing accordion pattern sits within the published delegation boundary and that the boundary stands, and the run resumes from its recorded state. An answer that changed the required behaviour would instead have reopened the intent for validation and republication before the run resumed. Each run opens a pull request carrying the work item key, the intent version and the provenance block. The accountable engineer decides, reading each change before it goes forward. The evidence is the branch, the change and its tests, with the recorded halt and its answer on the work item.

### Step 5, Verify

The acceptance examples run as automated tests, the contract-derived integration tests exercise version 2.3 from both sides and the security scan runs against the blocking severity threshold. One contract test fails in the service unit. The agent triages it at L2 and proposes the cause, a timestamp serialised in a format the contract does not allow. The engineer accepts the fix and the checks run again. At L3, Execute under gate, the agent executes the pipeline and assembles the evidence. It cannot skip, disable, re-run to a pass or mark as satisfied any check on its own output. The web unit passes every deterministic check and misses one threshold: the delivered script exceeds the size budget the context profile sets because the panel imports a date formatting library. The practice owner who set the budget grants a threshold exception for thirty days naming the accountable engineer as owner, and a low-tier request to replace the library enters the flow. The pipeline decides at this step. The evidence is the check results attached to each change, the findings on the pull requests and the exception recorded for the release manifest.

### Step 6, Review

A reviewing agent runs first on each pull request under a profile that uses a different model from the one that implemented the change, commenting at L1 and never approving. On the web pull request it notes that the four state labels are written into the component rather than drawn from the design system's text resources. The engineer corrects this and the pipeline runs the required checks again on the revised commit. A code owner for each context who neither formed the intent nor produced the change then reviews on a pull request carrying the intent reference, the impact note, the tests and the recovery position. The standard tier and the gate decisions set the required reviewers: a code owner for each unit, the reviewer the data gate named and, on the web pull request, the reviewer the customer outcome gate named. Each gate reviewer confirms that the change conforms to the recorded decision, the labels matching the approved wording among them. Branch protection enforces those approvals. Each code owner approves on the pull request, recording that the change conforms to the published intent, then merges the unit branch, producing the commit the next step identifies. The decision is the code owner's and the evidence is the review record and the merge.

### Step 7, Identify

Each merged unit becomes one release candidate, produced deterministically by the pipeline at L0. The service unit becomes amendments-service 2.3.0 and the web unit becomes portal-web 4.12.0, each a built package with its software bill of materials. The pipeline attests each candidate with a record that the named commit passed the required checks and reviews, a record no person or agent produces. Against each candidate it binds the step 5 check results, the step 6 approvals, the work item key and, because both units were agent-implemented, the agent profile version and the run identity that produced the work. The exception from step 5 is bound to portal-web 4.12.0 with its owner and expiry, and a changed revision of either unit would need its own attestation. The platform decides and the evidence is the two attested candidates.

### Step 8, Promote

The deployment engine for each context applies its candidate environment by environment below production, in the order the intent declared: the service first and then the web application. Environment values are supplied at promotion rather than carried in the change: the address of the policy administration system instance, the polling interval and the credential the service uses, referenced by name and never carried as a value. The agent prepares and invokes the approved job in each non-production environment at L3 within a scoped short-lived identity. The post-deployment verification in each context profile runs after each promotion, confirming that the operation answers and that the panel renders against the promoted service. The deployment engine invoked by the pipeline decides, and the evidence is the deployment history against the same release candidate in each environment.

### Step 9, Authorise

The two candidates are assembled in their declared order, contract compatibility is checked across them and the regression and end-to-end tests run on the assembled release in pre-production. The release authority takes two decisions and records them separately. The first is the change authorisation for CHG-2290 as a normal change under the organisation's change procedure, and the manifest does not assemble without it. Once that authorisation is current and the product owner has confirmed business and support readiness, a run computes readiness into the release manifest at L3. The manifest carries CHG-2290, the recovery position for each candidate (proven rollback) and the safe partial state, the service alone, whose new operation has no caller until the web application deploys. It also carries the communication to the contact centre, the confirmed knowledge article, the alert for the health check, the test completion and the open exception. The second decision is the release go, taken on the assembled manifest and naming the exact candidates, their recovery plans and the recovery trigger. The release authority did not request, build or implement the change. The evidence is the manifest and the two recorded decisions.

### Step 10, Deploy

The authorised candidates deploy to production in their declared order. The step is at L0 and no agent takes part. The deployment pipeline executes under the authorisation given at step 9 and creates no further approval. It deploys only the exact candidates the authorisation names and verifies their attestations first. After each deployment the pipeline compares the deployed content with what was attested and verifies the supplied environment values against the declared set. Had a deployment failed, the recovery named in the authorisation would have executed on its trigger, and the release authority would have decided any departure from it or a fix forward through the emergency route. The evidence is the deployment record against each authorised candidate: DEP-5531 for amendments-service 2.3.0 and DEP-5532 for portal-web 4.12.0.

### Step 11, Confirm and learn

The post-deployment health checks run from the alert registered at step 9, and the end-to-end journey is confirmed working. The product owner confirms each acceptance example through the approved production verification on designated test records and records the acceptance decision on PORTAL-412. The work item is complete on that decision, and acceptance stays at L0. Over the following month a run correlates the change with telemetry at L2 and drafts the outcome reading: the volume of progress enquiries to the contact centre against the baseline the request recorded. The product owner records the reading on the work item. The periodic review notes the open exception and the one acceptance example changed between draft and publication, and the coherence register gains an entry for the date formatting library with its owner and target date. The evidence is the health result, the acceptance decision and the outcome reading.

## What the record shows

Start from one line in the deployed web application, the function that labels a state for the customer. The package carrying it is portal-web 4.12.0, named in deployment record DEP-5532 and deployed under change record CHG-2290 with the release manifest, the release authority's two decisions and the exception. The candidate's attestation binds the merge commit, the step 5 check results and the step 6 review record. The merge commit comes from pull request 1187 on branch portal-412-progress, opened under run identity run-7f3a21 and carrying work item key PORTAL-412 with published intent version 1. The published intent sits in the web repository as a versioned document carrying request 2031, and the request record with the product owner's approve sits on the work item, where the acceptance decision closes the loop. Each record carries the reference to the next, so the trail reads in both directions. The pull request description follows, with the candidate identifier written back after step 7.

```
PORTAL-412: show amendment request progress (web application unit)

Provenance
- Request: 2031
- Intent: PORTAL-412, published version 1
- Agent profile: build-web 1.4
- Model and version: permitted model list entry 3
- Run identity: run-7f3a21
- Base commit: 9c41e7d on main
- Reviewing agent: review-web 1.2, permitted model list entry 5
- Reviewer: web application code owner, neither formed the intent nor produced the change
- Release candidate: portal-web 4.12.0, attested at step 7

Change summary
Adds the progress panel to the policy page, calling the progress
operation of amendment contract 2.3 and showing one of four states.

Acceptance examples
Examples 1 to 7 of version 1 passing in the pipeline.

Contract changes and dependency invalidation
Consumes contract 2.3. No declared contract changed during implementation, so no unit needs revalidation.

Halts
One stop condition, answered on PORTAL-412 by the intent architect.
```

## The same flow on any platform

The eleven steps are the same whatever the platform. The context profile carries the differences: the workspace and its refresh, the local check, the pull request contract, the verification checks, the release candidate, the promotion sequence and its environment values, the recovery position and the post-deployment verification. The journey of a high-tier change adds what the model describes. Its triggers determine the gates at step 2, and every validator and the intent architect take part at step 1. Further reviewers act at step 6 and the business owner accepts in pre-production before the release authority decides at step 9.
