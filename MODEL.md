# The Agentic Delivery Model

**Version 2.0 pre-release, October 2026**

An open, vendor-neutral model for software delivery where agents built on language models do much of the implementation under controls that people set and hold.

The model will evolve as tooling matures and organisations contribute experience.

---

## Introduction

The Agentic Delivery Model (ADM) is an open methodology for software delivery where agents built on language models carry out much of the implementation under controls that people set and hold. It is freely available under an open licence, designed to be adapted and intended to evolve as the technology matures. An agent here is a configured run of a language model that works in an allocated workspace under its own identity and produces output that pipelines and people check. People define intent, maintain coherence, verify output and govern the process.

The model is written for the people who run software delivery in an organisation, from the technology leader who answers for it to the engineers, analysts and testers who do the work. It also serves the assurance functions that need to see how a change was controlled. It is sector-agnostic, applies to greenfield and brownfield environments and suits a small team or a large technology function.

It covers the process from a request to a confirmed outcome and the engineering platform available to every team, the shared environments, pipelines and tooling of section 6. It also covers the controls that create evidence while the work is done and the use of agents at each step. One flow carries every change, agents work within limits set centrally and a person authorises every production change.

Spec-driven development holds that a written specification, not the code, is the primary artefact and that code is a regenerable output produced from it by agents. This model shares that principle and describes how an organisation runs it: the accountabilities, controls and flow needed to deliver from intent at scale. From Agile the model keeps iteration, collaboration and working software as the measure of progress. In place of ceremonies built around human coordination it has one flow, in place of posts defined by manual execution it has accountabilities and in place of estimation it reads observed lead time.

The model is read in order: the principles, then the unit of work, the flow, the deployment approaches, the agents, the platform, the accountabilities, the evidence and the measures, then adoption. Sections 9 and 11 place the model beside existing frameworks and state what it is not, and the appendices carry the templates and the defined terms. Four companions sit beside it: the adoption guide, the journey of a change (a worked example that follows one change through the flow), the profiles and the tooling notes. If you are reading in order to adopt, section 10 and the adoption guide give the order of work.

Version 2.0 restates the model around one flow and an autonomy ladder, five levels that set what an agent may do at each step (5.1). The changelog records the detail of what changed.

---

## 1. Principles

The model rests on ten principles. They decide any question the rest of the model does not answer. An adopter whose design cannot meet one records the departure and its reason in the coherence register, the single record of architecture decisions, technical debt and coherence risks (appendix D).

| Principle | What it means | Why it matters |
|---|---|---|
| 1. Intent over instruction | People define what needs to exist and why, with enough precision that an agent can execute against it. Precision is iterative: an intent improves through the passes in 2.3, not through upfront perfection. | An agent takes the intent literally and fills gaps with assumptions that may be plausible but wrong. The quality of what it delivers rises with the precision of the intent it works against. |
| 2. One flow for every change | Every change in scope follows the eleven steps in 3.1 and leaves the flow only by the exception routes in 3.2. Platform differences sit in the context profile (the platform's implementation of each step, appendix B) and the deployment approach (section 4), not in a separate process. | One flow gives one process to learn and one basis for assessing the controls and the evidence. |
| 3. Verification and validation serve different purposes | Verification confirms that the output matches the intent and the standards. Validation confirms that the intent serves the business need. Validation happens before implementation and at acceptance, verification through the checks at step 5, Verify, and the review at step 6, Review. | A change can match its intent exactly and still be the wrong change, so neither replaces the other. |
| 4. No one approves their own work | No person, agent or model approves a change it produced or attests to the checks on it. An implementing agent cannot override the checks on its own output, and a model reviewing another model's output comments without approving. | The person who formed the intent is a poor judge of the output, and the independence required of people extends to agents and models so that none approves its own output at scale. |
| 5. A person authorises every production change | Deployment to production and data change in production stay at autonomy level L0, Not used, under 5.1. A person holding the authority authorises them, and no one is the sole authoriser of a change they requested, built or will implement. | Production is where a mistake reaches customers, and accountability frameworks require a person in the chain of responsibility whatever the autonomy below production. |
| 6. Deploy what passed the checks | Each execution unit (a separately controlled part of an intent, 2.4) becomes a release candidate, an identified object promoted unchanged between environments with its checks and approvals recorded against it as its attestation. Where a platform promotes source, the candidate is linked to the release commit that deploys and the checks run again on that commit. | A rebuild between environments can make the object in production differ from the object that passed the checks. |
| 7. Each standard has one source and coherence has an owner | Every rule the pipeline enforces is read from the published standard, not a copy, and an agent retrieves the same standard at the point of use (6.2). The system steward owns coherence and the coherence register (section 7). | One published version prevents drift between the rule as written and the rule as enforced. Agents optimise locally, and without an owner for coherence an agent-built codebase drifts towards inconsistency. |
| 8. Autonomy is set centrally and earned by evidence | What an agent may do is set per step and per class of change (a kind of change the organisation treats alike), in organisation-level settings no project or user can raise. It rises only on an evaluation, a measured test of the agent's output at that step and class, and falls without a meeting (5.4). | A ceiling no project or user can raise lets the security lead and the accountable executive answer for what agents do. |
| 9. Evidence is created by the systems that do the work | Each record is created by the system that performs the step, without transcription, and carries the reference that links it to the next. Agent-produced work is traceable to the agent, the model, the run identity (the identity issued to one run), the intent and the codebase version. | Transcription breaks the links an assessor follows, and because agent output is not reproducible the control is the trail and a named person's authorisation. |
| 10. An agent stops rather than guesses | An agent halts at a stop condition, or where its context is missing or out of date, rather than inferring what to do, and records its state so the work resumes. A question inside the delegation boundary in the intent is resolved there, and a question about intent stops the run. | A recorded halt lets an unresolved assumption be checked before work continues. A guess carries it forward and the cost compounds downstream. |

---

## 2. The unit of work

The intent is the unit of work: a specification of one change, complete enough to be executed without further explanation from its author, formed and validated on its work item and built only from its published version.

### 2.1 What an intent carries

The work item is the record in the organisation's work management system that carries the request reference and the state of the work, and appendix A carries the template.

| Element | What it states | Why it is required |
|---|---|---|
| 1. Outcome and measure | The outcome, the measure that will show it, the person who accepts it and the colleagues whose work it changes. | Acceptance at step 11, Confirm and learn, is decided against it. |
| 2. Scope boundary | What is in scope and, separately, what is out. | An unstated exclusion reads as permission. |
| 3. Acceptance examples | Concrete examples of correct behaviour, with edge cases and error paths, owned by the business and written before implementation. | An agent may implement a wrong assumption and generate tests that reproduce it. |
| 4. Contexts and execution units | The delivery contexts affected (each one platform with its own deployment mechanism), the execution units and their order, with a leading context named to hold the published intent where more than one is affected. | It sets the execution environment, the deployment approach and the order of work. |
| 5. Contracts to honour | The interface, data and architectural contracts each unit honours, by version. | Compatibility across contexts is established before release. |
| 6. Data and security position | Personal data, permission changes, classification, retention, any protection assessment and the threats the change introduces. | These are gate triggers at step 2, Gate, identified before implementation. |
| 7. Recovery position | The planned route back if the change fails, per context (section 4), and the evidence required before authorisation. | It is established before authorisation at step 9, Authorise, not discovered at an incident. |
| 8. Delegated judgement and stop conditions | What the agent may decide for itself, what it must stop and ask about and the conditions that halt a run. | They bound delegated authority and stop execution after an assumption has failed. |
| 9. Assumptions and open questions | Unverified assumptions, each stated so it can be tested, and open questions with their closing action and owner. | A false assumption surfaces at a checkpoint, not at release, and a unit waits while a question bearing on it is open. |
| 10. Dependencies | What the change waits on and what waits on it, by execution unit, bound to the release candidates at step 7, Identify. | They set the assembly and release order and feed dependency invalidation under 2.4. |

Every element is answered. Where an element has no content the intent records that it was considered and does not apply, because a blank cannot be distinguished from an omission and an agent reads an unstated constraint as an absent one.

### 2.2 The request decision

A request is a proposal for a change recorded before any specification begins, and it is not the intent. The request decision establishes whether it merits implementation, and the product owner decides on business merit, with the business owner where the outcome is theirs.

| Outcome | What it means |
|---|---|
| Approve | The request enters the flow at step 1, Intent. |
| Redirect | An existing capability or planned change meets the need, or the real problem differs from the one described, and the requester is referred. |
| Defer | The case is valid but a dependency, such as an undecided business process, is outstanding and is named with its owner and a date. |
| Reject | The case is not justified, or no technology change is needed, and the rationale is retained for any resubmission. |

The agent supplies evidence at L1, Advise. The decision stays with people.

| What the agent supplies | Why it can |
|---|---|
| Whether this was requested before and with what outcome. | The intake channel and the queue of approved intents are searchable records. |
| Whether an existing capability already does this. | The current-state architecture describes the estate as it is. |
| Previous failures in the area and other work in progress on the same contexts. | Incident records and the work item system are indexed by system and context. |

The review is proportionate: a short conversation for a familiar class of request and a fuller assessment for an unfamiliar class of change, a cross-product change, a business commitment or a previously rejected request. A catalogue change (a low-risk class pre-authorised in the standard change catalogue, section 9) is exempt from the review but still counts against the cap on approved work and receives the go or no-go at step 9. The request record states the problem, the expected benefit or obligation, the business owner, the indicative effort, the alternatives considered and the work displaced, in a short form rather than a business case. Approval requires a completed record.

Approved work, whether a person or an agent will implement it, is capped against validation and review capacity, because capping human implementation alone moves demand downstream. The team confirms capacity before approval takes effect, an intent that would exceed it waits and the cap is reviewed at the periodic review in 3.3. The share of requests rejected, redirected or deferred is a measure under 8.2, named request decision outcomes. A rate near zero prompts a sample of the decisions, because indiscriminate approval and effective upstream prioritisation both produce few rejections.

### 2.3 Forming and validating the intent

An approved request becomes an intent through six passes. The agent drafts at L2, Draft, in headless runs (runs the pipeline invokes without a person at the keyboard), incorporating the decisions people record on the work item at each pass and identifying missing information. The pipeline writes each draft back to the work item.

| Pass | What is added | Who leads it, with the agent drafting |
|---|---|---|
| 1. The need | The outcome in the requester's own terms, the measure, the accepting person, the colleagues affected and the exclusions from scope. | The requester with the business analyst. |
| 2. The behaviour | The required behaviour as concrete worked examples, with edge cases and error paths. | The requester and the business analyst with the test engineer. |
| 3. The shape | The contexts affected, the execution units, the contracts each unit honours by version and any change to an existing design. | The intent architect: a solution architect or lead engineer according to tier. |
| 4. The data and security position | Personal data, permissions, classification, retention, any protection assessment and the threats the change introduces with their requirements. | The business analyst with the data engineer, joined by the security lead's delegate where a trigger is met. |
| 5. The operational position | The recovery position per context, the non-functional expectations and the procedure for reversing a partial deployment. | Whoever led the shape, with the release manager where no context profile covers the recovery position. |
| 6. The delegation boundary | What the agent may decide for itself, what it must stop and ask about and the conditions that halt a run. | The intent architect with the lead engineer. |

Validation precedes implementation and is iterative: the agent drafts, the team supplies missing information and the agent revises until the validators are satisfied. The business analyst validates the expression of the request, the test engineer the acceptance examples and the intent architect confirms the contracts and standards before deciding to publish. No one who shaped the intent declares it validated alone, and an intent that fails validation returns for refinement with the failure counted under 8.2. Validation is proportionate to tier: every validator for a high-tier intent, one person for a standard-tier intent and one team member for a low-tier intent, and a catalogue change is checked against its catalogue entry. The one person is the test engineer where the acceptance examples carry the risk and otherwise the business analyst. Most intents complete the six passes in one working session or in exchanges on the work item. A pattern library of intents for recurring shapes of change, such as a new field, an integration or a report, lets a common change be formed by adaptation rather than from a blank page. Each drafted element receives one of three forms of assessment.

| Form | Elements | What the person does |
|---|---|---|
| Ratify | Contexts and execution units, contracts, the data and security position, the recovery position and dependencies. | Checks the draft against stated evidence and records on the work item what it was checked against. |
| Decide | Outcome and measure, the accepting person, the scope boundary, delegated judgement and stop conditions. | Owns the problem, the alternatives and the criteria, including options the agent did not propose, recording the chosen and rejected options. |
| Break | Acceptance examples, every assumption and every open question before the unit it bears on is worked. | Tries to falsify the draft and records the cases examined, the evidence considered and any resulting change. |

Ratification is substantive because an error in a ratified element can prevent the right checks being selected. A consequential assumption is assessed by break instead, as are the acceptance examples, because an error in them has no downstream verification.

The acceptance examples are drafted in a run separate from implementation: the agent that drafts them cannot implement against them or share the implementation run. Separate runs do not establish independence on their own, since two runs may share missing sources or the same misunderstanding of the business and reproduce one assumption. The assurance comes from business-owned expected results and from checks derived from evidence the implementer did not supply.

Only a published version of the intent is built. When the validators have recorded their validations and the intent architect has decided to publish, the work item system publishes that version to the leading context's repository. It goes through a pull request the context's code owners approve (version control's required approvers for a path), and that pull request records the validations rather than commissioning a further review. The merge hands the intent to step 2, Gate, where a trigger is met, and otherwise to step 3, Workspace. The work item then holds the working copy read-only until the intent is reopened, and an agent at step 4, Build, reads only the published version, so an edit on the work item cannot redirect a run.

### 2.4 Decomposition and dependencies

An execution unit is a separately controlled part of an intent, one or more per context, each becoming its own release candidate with its own activation conditions and recovery position. The intent architect decides the decomposition the agent proposes at step 1, recording it in the intent. Each unit is classified on three questions.

| Classification | What is assessed | Outcome |
|---|---|---|
| Implementation | A version-controlled representation, enough in the intent to act within the judgement limits and evaluated tooling for the context. | An agent where all three hold, otherwise a person. |
| Validation | Independence from the implementer, with business acceptance from the examples and integration and security checks from another source. | Never the implementer. |
| Release authority | Who gives the go or no-go at step 9. | A person in every case, without determining who implements. |

Independently completed units are not automatically compatible and a discovery in one platform can invalidate work completed in another, so the intent declares five requirements before release.

| Requirement | What the intent declares | Why |
|---|---|---|
| 1. Versioned contracts | Each unit states the contract version it produces and the versions it consumes. | Incompatibility is detectable before deployment. |
| 2. Dependency invalidation | A change to a contract marks every dependent completed unit for revalidation, and the release manifest (the record of a release's candidates, order and recovery positions, assembled at step 9) cannot include it until then. | Work is not released on evidence that no longer applies. |
| 3. Coordinated rollout and the partial position | The safe partial-deployment states and the units that must be recovered together where a partial deployment is unsafe. | Recovery is declared, not improvised. |
| 4. Resumable state | A unit that halts records where it stopped and what it had established. | Work resumes rather than starting again. |
| 5. An end-to-end test | The path across the contexts is demonstrated working before release. | Component tests passing does not show that the path works, and the product owner's acceptance completes the intent. |

A context may contribute several units, each a separately controlled rollout stage. Replacing a field is the general case: one unit expands the schema to carry the new field beside the old, one keeps the two compatible while consumers move and one backfills the data. A final unit removes the old field, and the intent states the safe resting state at every intermediate point.

Dependency invalidation applies to work in progress: a contract change halts a running unit at its next checkpoint and marks it for revalidation before it resumes. Where dependencies are cyclic the units iterate across contexts, an intent whose units cannot be ordered requires further review of its decomposition and a unit that cannot become one release candidate returns to step 1.

### 2.5 Risk tiers

Every intent carries a risk tier.

| Tier | Defined by effect |
|---|---|
| High | A change that alters a monetary calculation or an input to one, changes how personal data is collected, stored, shared or retained, changes a permission model or a security control, alters a published interface contract, changes the outcome of a customer-facing journey or is a migration-based change against production data. |
| Standard | A change to behaviour a user or an interface can observe, within an existing journey, calculation or contract, where none of the high-tier conditions applies. |
| Low | A change that leaves the behaviour a user or an interface can observe unchanged and meets no high-tier condition, such as a refactor, a code comment, a test or internal documentation. |

Where two tiers could apply the higher applies. The engineer states the tier at step 1, Intent, with the test engineer checking it. Tiering follows the effect of the change, not the platform: a platform that processes financial or personal data does not by itself make a change high tier. On the same platform, adding a field that changes observable behaviour is standard tier where none of the high-tier conditions applies. The tier sets the validators under 2.3, the test expectations the tiering standard under 6.2 sets and the reviewers at step 6, Review. The distribution of tiers is itself a control. A high-tier share that rises materially over a quarter against the product's baseline, or a sample showing incorrect classification, prompts a review of the tier criteria. The system steward and the practice owner for test, the person accountable for the test standard, carry out that review. The share of changes in the high tier is a measure under 8.2 and is a review trigger rather than a target.

---

## 3. The flow

### 3.1 The eleven steps

Eleven steps apply to every delivery context. The context profile (appendix B) holds the platform-specific implementation of each step for one context, from its workspace and checks to its release candidate form and recovery position. Agent work runs by default as a headless run under a run identity issued for that run. People shape and decide it through the work item and the pull request, not by supervising a run. In this model a deterministic check enforces a rule that is never waived. A threshold check compares a measured result with a numeric limit set by a practice owner, the person accountable for a discipline's standard, and it may carry an exception under 3.2. The table gives each step's starting autonomy level under 5.1.

| Step | What happens | The agent's part | Who decides and the evidence | If it fails |
|---|---|---|---|---|
| 1 Intent | The approved request becomes an intent carrying the elements in 2.1, decomposed into execution units with contracts and dependencies declared, and the engineer states the tier. | L2, Draft: drafts the intent and proposes the decomposition, the pipeline writing each draft to the work item. | The validators and the intent architect, who decides decomposition and publication. Evidence: the validated intent, published to the repository. | Refinement, with the first-time failure counted. |
| 2 Gate (where triggered) | A gate applies only where the intent meets one of its triggers, and the team raises it with the material the trigger needs. | L1, Advise: identifies the triggers met and assembles the material. | The gate's owner: the system steward (design), the data owner (data), the security lead (security and privacy), a decider the teams concerned agree (cross-team) or the accountable executive or a holder of recorded delegation (legal, finance and customer outcome). Evidence: the recorded decision (8.1). | The intent waits. An exception comes only from the owner of the decision, with an expiry. |
| 3 Workspace | An isolated workspace (a branch, sandbox, schema or tenant) is allocated per unit from a versioned definition and refreshed with masked data by pipeline jobs no agent runs. | L0, Not used: the pipeline allocates it, and an agent changes a definition only by pull request. | The platform. Evidence: the environment record and its refresh position. | The unit waits rather than sharing another team's workspace, and scarcity is measured as a platform defect. |
| 4 Build | The unit is implemented from the published intent, by an agent or a person according to its classification, under the intent's stop conditions and checks the agent cannot disable. | L2: implements in the isolated workspace under a run identity and raises a pull request carrying the work item key. | The accountable engineer. Evidence: the branch, the change and its tests. | The run halts at a stop condition and records its state so the work resumes rather than repeats. |
| 5 Verify | The acceptance examples, the contract-derived integration tests and the security and data checks run locally and in the pipeline from the same rule versions, the pipeline being authoritative. | L2 to triage a failing check and L3, Execute under gate, to run the pipeline and assemble the evidence, never disabling, skipping, re-running to a pass or attesting against any check. | The pipeline. Evidence: the check results and the findings on the pull request. | The change does not proceed. The failing change is corrected and checked again, a defective check is corrected through the standard's own route and a threshold may carry an exception under 3.2. The pipeline owns bounded retries, keeps every attempt and never lets an agent choose the passing attempt. |
| 6 Review | The pull request, carrying the intent reference, the tests and the recovery position, is reviewed by the reviewers the gate decisions and the tier require. | L1: an agent on a model different from the one that produced the change reviews first and comments, never approving. | A code owner who neither formed the intent nor produced the change approves and merges to the protected destination branch. Evidence: the approvals, bound to the candidate at step 7. | The change is revised, and a disputed review goes to the practice owner rather than being settled by seniority. |
| 7 Identify | The merged unit becomes one release candidate (a built artefact, a declared-state revision or an ordered migration set), carrying the agent profile version and run identity where an agent built it. | L0: the pipeline checks the exact merged revision, then produces and attests the candidate deterministically. | The platform. Evidence: the attested candidate with every check result and approval recorded against it. | A unit that cannot become one candidate returns to step 1 for review of its decomposition. |
| 8 Promote | The deployment engine for the context applies the candidate environment by environment below production, with environment values supplied at deployment rather than carried in the change. | L3: prepares and invokes the approved job in non-production under a scoped, short-lived identity. | The pipeline, invoking the deployment engine. Evidence: the deployment history against the same candidate. | Promotion stops at the failed environment and recovery follows the declared position. The unit re-enters at the earliest step the failure affects, and an unchanged candidate keeps its evidence. |
| 9 Authorise | The release's units are assembled in declared order, contract compatibility is checked, the regression and end-to-end tests run in pre-production and readiness is computed into the release manifest. | L3: computes readiness and assembles the manifest. The decision is a person's. | The release authority, on a change they did not request, build or implement, once the change authorisation is current. Evidence: the manifest naming the exact candidates and recovery positions, with both decisions recorded. | Authorisation is withheld, a unit whose contract assumptions changed is revalidated and a break-fix goes by the emergency route. |
| 10 Deploy | The authorised candidates are deployed to production in declared order under the authorisation at step 9, with no further approval created. | L0. | No new decision. Evidence: the deployment record against the authorised candidates. | The recovery action and trigger named at step 9 execute at L0, the release authority decides any departure from them or a fix forward by the emergency route and the failure counts against the change failure rate. |
| 11 Confirm and learn | Health checks run, the end-to-end path is confirmed, the product owner accepts or rejects the work against the acceptance examples and outcome telemetry is read against the intent's measure. | L2: correlates change with telemetry and drafts the post-incident review. Acceptance stays at L0. | The product owner, with the business owner where the outcome is theirs. Evidence: the health result, the acceptance decision and the outcome reading. | A failed health check invokes the recovery decision at step 10. Rejected work and a missed outcome return as a new intent, not reopened in place. |

A gate at step 2, Gate, is triggered by the intent rather than applied to every change. The gates are architecture (a new integration or anything outside the agreed standards), data (a model change, a new source, personal data or retention) and security and privacy (a permission change, a new external flow or a vulnerability). The others are cross-team (a shared environment or a release collision), legal (published terms or a customer's rights), finance (a monetary calculation or payment handling) and customer outcome (the content of a customer communication or journey). Each gate has a named decider and a response period, and the accountable person decides on the evidence available once the period passes without advice. The intent architect records the trigger screening at step 1, Intent, and a gate's owner decides any disputed applicability. Each gate decision names the reviewer who confirms at step 6, Review, that the change conforms to the position approved for it.

Two decisions at step 9, Authorise, are distinct and recorded separately. The change authorisation is given under the organisation's change procedure by whoever holds the production change authority, or by the catalogue's pre-authorisation for a catalogue change. Without it the manifest does not assemble. The release go or no-go is the release authority's, taken on the assembled evidence. It authorises the release rather than the change, a release being one or more candidates assembled and authorised together. The manifest names the exact candidates and their recovery positions, any exception with its owner and expiry, the recovery action and its trigger and the health signals, recovery threshold and observer after deployment.

The item is complete on the acceptance decision at step 11, Confirm and learn, not on the deployment finishing. Acceptance confirms the delivered behaviour against the acceptance examples validated under 2.3 rather than reopening the specification. The outcome reading against the intent's measure is recorded separately when the measure can be read, with its owner and date recorded before the work item closes.

### 3.2 Exceptions and re-entry

An exception is a time-limited departure from a control or from the flow, granted by the authority responsible for the rule and never by a weaker route. A threshold exception is granted by the practice owner who set the figure. A policy exception is granted by the policy's owner: the security lead for a blocking security finding, the data owner for data protection, the system steward for an architecture standard and the accountable executive for business risk. Every exception has a named owner and an expiry and is recorded in the release manifest. An expired exception blocks the release of any candidate that still depends on it until the owner remediates or obtains reauthorisation from the granting authority, and a candidate that removes the dependency may proceed.

Six never-waived controls bind every authority. The first three are anything that would breach a legal requirement, the rule that only an attested release candidate deploys and the rule that the recovery position is established before authorisation. The other three are approval by someone who did not build the change, the secret and credential rules and the categories of data that may never leave production. Nine conditions suspend or divert work, each with a defined re-entry route.

| Condition | What happens | Re-entry |
|---|---|---|
| An intent fails validation | It returns for refinement at step 1, Intent, and no unit is created. | Revalidation. A pattern of first-time failures goes to the practice owners rather than to the team. |
| A run reaches a stop condition | The run halts and records its state, and its question goes on the work item. | The engineer answers, with the relevant specialist where it touches architecture, data or security, the intent is republished where it had a gap and the run resumes from the recorded state. |
| A deterministic check fails | The unit stops at step 5, Verify. The check is never waived. | The corrected change re-enters at step 5, or the practice owner corrects a defective check through the standard's own route and versions the correction. |
| A threshold is not met | The unit stops at step 5. | The granting owner records a threshold exception, or a policy exception from the security lead for a security severity, against the unit, the pipeline carries it into the manifest before authorisation and the unit proceeds. |
| Contract assumptions change | Every dependent completed unit is marked for revalidation and cannot enter the manifest until revalidated. A running unit halts at its next checkpoint. | Revalidation against the new contract version, or the intent is reopened where the change is material. |
| The scope changes | The run halts at its next checkpoint and the product owner decides on the new scope as on a request. | The intent is revised, the triggers and the tier are reassessed and the work re-enters at the earliest step the revision affects. |
| An environment is unavailable or the cap on approved work is reached | The unit waits rather than moving to a shared workspace or joining an overloaded queue. | The platform owner allocates or refreshes an environment and capped work enters as capacity frees. |
| An emergency arises | A break-fix change restoring a failed or degraded service leaves the flow at whatever step it has reached and goes to the emergency change authority with the minimum evidence it requires. The six never-waived controls hold before it deploys. A missed cut-off is not an emergency. | The record is completed by the end of the next working day, recording the checks that ran and the evidence that was omitted rather than implying it existed at authorisation, and the permanent fix follows the flow as an intent. During the incident an agent may advise at L1, Advise, on telemetry and draft the fix at L2, Draft, in a workspace, and it acts on nothing in production. |
| A product spends its error budget | Ordinary releases to that product pause and agent autonomy on it drops a level under 5.4. The error budget is the unreliability a product's service level objective allows over a period. Fixes and security patches continue by their normal route. | The release authority and the product owner agree resumption on the reliability evidence. |

### 3.3 The rhythm

Execution is continuous and event-driven. A headless run is invoked by the pipeline when an event on the work item or in the repository calls for it, and review happens as output arrives rather than at a fixed hour. The minimum rhythm for people is a daily sweep of the review queue and the clarification queue, run as a working review rather than a status meeting. The reviewers work the pull requests, the engineers, the validators and the product owner answer the questions halted runs have placed on the work item and the runs resume. No meeting approves a change: review happens on the pull request at step 6, Review, with authorisation at step 9, Authorise.

A periodic review, weekly or monthly as the organisation chooses, looks at the measures under 8.2, the coherence register, the autonomy levels in force and the open exceptions. It reviews the cap on approved work, reads the tier distribution against the criteria and retires or schedules the items on the coherence register. It reads intent quality from the stop-condition rate and from the acceptance examples changed between draft and publication, commissions remediation as requests under 2.2 and raises or lowers autonomy on the evidence under 5.4.

The release rhythm is set per product on evidence, by trust in the product's verification and by what its pipeline supports rather than by agent speed. The product owner and the release authority set it, record it against the product and change it on the measures: release frequency, change failure rate and time to restore.

Where the organisation is regulated, the assurance view in section 9 is built from the same evidence that governs release pace and autonomy.

## 4. Deployment approaches and recovery

Not every change is code that is compiled and packaged. Declarative configuration, infrastructure declared as code and data that cannot simply be replaced are built, identified, promoted and recovered differently, so the model names that difference. A **deployment approach** is the way a change to code, configuration or data is built, identified, promoted and recovered according to what it alters. The model recognises three, each with its own release candidate and recovery position.

| Approach | Release candidate and promotion | Recovery |
|---|---|---|
| Artefact-based | Built once into a package, or attested as a commit where source is promoted, then promoted unchanged with environment values supplied at promotion | A proven rollback to the previous candidate, tested below production before authorisation |
| State-based | A versioned revision declaring the desired state. The pipeline validates the plan, one adapter applies it, the result is verified and drift is detected | Restoration of the previous desired state through a new plan towards the previous revision, or a tested compensating change where restoration is unavailable |
| Migration-based | An ordered set of migrations altering data in place, with a dry run, a snapshot where possible, resumable execution and reconciliation against a stated result | A proven rollback where reversible. Where irreversible, a tested restore and a forward remediation route, with the residual risk accepted in writing |

A change follows the approach of what it alters, not an approach assigned to its platform as a whole. A **delivery context** is one platform or system with its own workspace, pipeline route and deployment engine (the tool that applies a candidate to an environment), and it may carry more than one approach. A platform that is usually artefact-based carries state-based change for its surrounding configuration, and a data platform that is usually state-based carries migration-based change where a structural change moves data.

A **release candidate** is one execution unit in the form attested at step 7, Identify: a package version, a declared-state revision recorded as a commit or an ordered migration set with its dry-run result and reconciliation expectation. The pipeline produces it once and binds to it the check results from step 5, Verify, the review from step 6, Review, as well as the work item and change record references. An **attestation** is the pipeline's signed record that the named commit or package passed the required checks and reviews, and neither a person nor an agent produces one. The attested object is the one that deploys. The attestation is evidence for its named object and cannot authorise a changed one, so a new revision requires its own checks, review and attestation. It also binds the versions of the standards the checks applied, and the required workflows and accepted check identities are enforced at organisation level outside any run's write scope. The authorisation at step 9, Authorise, names one set of candidates, so a changed set is a new release. Where a platform promotes source, the merge into the destination branch produces a **release commit** distinct from the unit commit. The checks run again on it at the last environment below production, and the authorisation names the release commit with no further merge before deployment.

A **recovery position** is the tested route back to a known good state if a change fails. The model recognises three: a proven rollback, restoration of the previous desired state (or a tested compensating change where restoration is unavailable) and, for a technically irreversible change, a tested restore with a forward remediation route. For a supplier's own release that the organisation cannot reverse, the accountable executive accepts in writing the residual risk of the continuity and remediation measures available, and the supplier's release process stays outside the flow. The intent carries the position as element 7 in 2.1 and the **release manifest**, the record that assembles readiness, exceptions and recovery for a release, records the position for each candidate. No release reaches step 9 without an established recovery position, one of the never-waived controls under 3.2, and for a high-tier change the position is exercised below production first.

A **context profile** holds what is specific to a platform so that the flow stays the same across contexts, from the workspace to the recovery position and the manual fallback under 6.4, and appendix B gives the template. A profile does not determine classification: a change is not a standard change because its route is routine.

A deployment approach is distinct from a **deployment strategy** such as blue-green or canary release, and a strategy, governing how a released change is introduced into live service and withdrawn, can be used with any approach. A feature flag, a switch that exposes a deployed change to users when turned on, decouples deployment from release where a change spans several intents. Each intent's candidate deploys behind the flag. The product owner requests activation once every contributing intent is deployed, the activation is itself a production change authorised at step 9, Authorise, as a catalogue change where one is designated, and acceptance at step 11 follows it. A flag that remains after release is recorded in the coherence register until it is retired.

## 5. Agents: autonomy and governance

The model is tool-agnostic but not control-agnostic: agents require explicit boundaries on access, identity, execution environment, model usage and knowledge sources. An **agent** is a software agent built on a language model that reads context and proposes, drafts or executes within the autonomy level assigned to its step and class of change. It runs on the engineering platform under a **run identity**, a scoped short-lived identity issued for each run and recorded in every commit, comment, work item update and log entry, distinct from the person who triggered it.

### 5.1 The autonomy ladder

An **autonomy level** states what an agent may do at a step of the flow for a **class of change**, a named kind of change such as a unit test, a permission set or a data migration. The model defines five levels, set out below.

| Level | What the agent may do | Applicable controls |
|---|---|---|
| L0, Not used | A person or deterministic automation performs the step | The controls of the step itself |
| L1, Advise | Read context and propose through a comment, with no write access to code, configuration, the work item or any environment | Scoped read-only retrieval and commenting permission, no execution permission |
| L2, Draft | Write into an isolated workspace or draft an intent, under the same review as a person's work | Branch protection, code ownership and a named person's merge authority |
| L3, Execute under gate | Run an approved job in non-production, where a deterministic check it cannot override decides whether the work proceeds | The required checks for the step and class, with no agent able to disable, skip, re-run to a pass or self-attest |
| L4, Execute in production within a scope | Perform a catalogue action in production, inside a named scope, logged in full, with a manual route available | The catalogue entry, the scoped identity, full logging and the suspension route |

A level is assigned to a step and a class, so one agent may execute at L3 for a unit test and only advise at L1 on a permission set. A level at one class authorises nothing at another, and it is held as a ceiling in organisation-level settings that no project or user can raise. The starting levels in 3.1 take effect only after the evaluation and approval under 5.4, and production deployment and production data change stay at L0.


L4 is assigned to nothing by default. Its first candidate is production verification at step 11, Confirm and learn: an agent executing the approved health checks, or a scoped catalogue action after a failed check, with deployment and production data change excluded from the scope. A **catalogue change** is an action pre-authorised in the standard change catalogue of section 9.

### 5.2 Agent configuration

An **agent profile** is a configuration, not a capability: the context the agent receives, what it can read, what it can write and what approval its output needs before it takes effect. Seven elements, held beside the code or in organisation-level settings every repository inherits, govern how an agent is instructed and what it may reach.

| Element | What it is | Who owns it |
|---|---|---|
| Repository instructions | One instruction file per repository: conventions, test expectations, branching rules and what the agent must never do | Lead engineer |
| Path-specific instructions | Narrower files for matched paths, so the rules fit each technology | Lead engineer with the practice owner |
| Standards as retrievable context | The standards of 6.2, retrieved at the point of use | Each standard's owner |
| The tool allow-list | The systems and tools an agent may reach and the operations permitted at each level, with credentials held as secrets | Platform owner, each addition approved by the security lead |
| The agent profile | A versioned record of step and class, model, hosting, level, tools, scopes, stop conditions, evaluation and approval (appendix C) | Platform owner, approved under 5.4 |
| Business and process context | The process the change codifies, with its entities and decision rules, in a form a pipeline check and an agent both read | Process owner with the system steward |
| Current-state architecture | The estate as built (inventory, designs, interfaces, data model, infrastructure), versioned beside the code and regenerated after each release | System steward with the platform owner |

Three permissions are authorised separately, so an advising agent reviews without write access.

| Permission | What it allows | Where it applies |
|---|---|---|
| Retrieval | Reading the repository, the standards and any allow-listed system, always read-only | Every level from L1 |
| Commenting | A review comment on a pull request, or a comment the pipeline posts on the work item, approving nothing | L1 and above |
| Execution | Writing to a workspace or drafting an intent at L2. Invoking an approved job in non-production at L3. A catalogue action in production at L4 | Only at the level that names it, through a run identity scoped to the run |

The organisation level is authoritative for every element: a project or a user may add a restriction and cannot remove one, and retrieval beyond the repository is read-only, enforced by the permissions of the authenticated identity. A context source is adopted only where its support status is approved and its access can be revoked, and the pipeline rule prevails over an inconsistent instruction file.

The pipeline, rather than the agent, writes to the work item. The pipeline job binds each run to one work item before it starts and writes to that item alone. It updates the draft only while the item is in refinement and posts a halt question to the item at any step. The run identity only reads the work item, every write is logged against the run and a run at step 4, Build, reads only the published version of the intent.

Instruction files and standards state development requirements, not the configuration of a particular environment. An agent therefore reads the approved process definitions and decision rules the intent names and the current-state architecture for the systems the change touches, rather than inferring them from the code. A run that finds its context missing or out of date stops under principle 10 and the intent records an open question.

An agent proposes a change to shared context and never approves one: it raises a pull request from its run identity, the content owner approves it and a code owner merges it. An unmerged proposal is not retrievable as context, the same control applies to an agent profile and a misleading entry in a shared source would otherwise be inherited by every later run.

### 5.3 Controls at every level

Nine controls apply at every level above L0. The table below maps each to its threat.

1. **An agent never authorises a change and never publishes a workaround or article it drafted.** The rule that bars a person from authorising a change they requested, built or will implement extends to an agent, and an agent holds no approval authority at all.
2. **An agent cannot attest to its own work: it may not disable, skip, re-run to a pass or mark as satisfied any check on its own output.** The pipeline, not the agent, writes the attestation.
3. **Production deployment uses only the published promotion path, and an L4 action uses only its approved catalogue route and scope.** The path is the context profile's promotion sequence under the authorisation at step 9, Authorise. A credential that bypasses either route is a control failure.
4. **Everything an agent does is logged and retained, under a run identity.** Each run's session record, with barred content removed, reaches the organisation's own store and security monitoring.
5. **A manual route remains available for every automated step.** A named authority may suspend an automation and fall back to the route, as 6.4 sets out.
6. **A model is used only where its hosting meets the organisation's data position.** Inference takes place within an approved hosting location or boundary, prompts and outputs are not used for training and the **permitted model list** records each model's provider, hosting, retention and evidence.
7. **The model that implements a change does not review it.** The drafting, implementing and reviewing models are named separately in the agent profile and the evaluation under 5.4 establishes the reviewing pairing.
8. **An agent takes instructions only from approved sources, and treats retrieved text as data.** Approved sources are the published intent, the instruction files, the standards and the agent profile with its pinned skills. During refinement a drafting run uses the decisions people record on its bound work item as input to the next draft, and a build uses a republished intent where an answer changes it. A build may also use an authorised clarification answer within the published delegation boundary, and an answer that changes the intent requires republication. Any other instruction found in a work item, a comment or a dependency is prompt injection and cannot override the published intent, the approved instructions or the run's permissions. The run identity's scope, not the agent's judgement, limits what it could reach.
9. **An agent does not move data or secrets out of a controlled environment.** A run reads no credential beyond its own identity. Production-derived data enters a prompt or an output only after the masking and classification checks the data standard sets, the categories barred from leaving production never do and secret scanning applies to agent output.

| Threat | Control |
|---|---|
| An agent authorises, attests to or publishes its own work | Controls 1 and 2 |
| A production deployment or an L4 action bypasses its authorised route | Control 3, with deployment at L0 |
| An agent's actions cannot be attributed or reconstructed | Control 4, with the identity issued per run |
| An automated step fails with no fallback | Control 5 and the manual fallback |
| Data or prompts leave the data position through a model | Control 6 and the permitted model list |
| The producing and reviewing models share the same error | Control 7 and the pairing evaluation under 5.4 |
| An instruction planted in retrieved content redirects a run | Control 8 and the published intent |
| Data leaves a controlled environment through agent output | Control 9 |
| A context source or server is compromised or changes its behaviour | The allow-list, the approved support status and revocable access under 5.2 and suspension under 5.4 |

The security lead holds the threat model for the agent platform and reviews it when a model, a surface or a context source is added. A threat without a control is a gap closed by the component's owner before the affected step rises above L1.

An assistant inside a product is a change like any other: its prompts, retrieval configuration and model version are declarative configuration in version control, released under the flow. Because its output is not deterministic, its acceptance examples carry a threshold over a set of cases rather than one expected answer each.

### 5.4 Raising and lowering autonomy

Autonomy is set centrally and earned by evidence under principle 8, and three conditions govern the first delegation of a step and class above L1 as well as every later increase. The first is an **evaluation**: a representative run against the organisation's own historical changes for that step and class, measuring output acceptability and the review and rework required. The second is a recorded period at the level below, set as a number of changes rather than a duration, with every output reviewed and the failure modes named. The third is a versioned agent profile stating the model, tools, scopes and stop conditions. An evaluation covers one class, one model and one reviewing pairing, because a review result is not the same in both directions.

The platform owner approves the first delegation and every increase, with the system steward, the security lead or the data owner for a matter in their remit. The practice owner for the step's work (the owner of that discipline's standard), who did not run the evaluation, accepts it against criteria set before it ran. A merged profile grants nothing by itself: the approval identifies the evaluation supporting it and an increase takes effect only through the attested and authorised release of the platform. A material change to the evaluated model, instructions, skills, context sources or tools suspends execution authority at that step and class until reassessment.

A reduction is immediate. The platform owner, the security lead, the release authority or the accountable executive may each order one at any time, without a meeting. Execution identities are issued per run and scoped from the released profile and the organisation-level settings, so a reduction revokes the active identities it affects and binds every later run at once. The platform owner revokes them on the decision and corrects the profile afterwards.

The **error budget**, the unreliability a service level objective permits over its window, also governs autonomy. When a product spends its budget, the recorded level for each step on that product above L1 drops by one, with L1 as the floor. The reduction applies to the steps implicated in the failures, or to every step with the reason recorded. When the release authority declares the pause, the platform owner applies the reduction by reissuing execution identities at the lower scope. Ending the pause does not restore a level by itself. The authority that approved the level restores it on the existing evaluation and the reliability evidence where agent-produced change did not contribute to the failures, and otherwise the increase follows this section in full.

The platform operates in one of three states: normal, degraded and manual. In the degraded state reduced agent capacity takes only the highest-priority intents, and in the manual state people execute them under the same controls. The platform owner sets cost circuit breakers: alerts when spend per intent exceeds a threshold and hard limits that pause execution at a budget. Where a compromise is suspected, one fast way to pause all agent execution is needed: this halt control has a named owner, is tested before it is needed and pauses execution without losing the provenance of work in flight.

### 5.5 Trust boundaries

A **trust boundary** is a point where control passes from one party or environment to another. The model names six, each with its two sides, what crosses it, the control and the owner, so that a reviewer can check the control and an auditor can find its evidence.

| Boundary | Between | What crosses it | Control and owner |
|---|---|---|---|
| 1 | The organisation and its supplier services | Source code, pipeline logs and the agent supplier's session content | The residency position decided as a supplier assessment, organisation-level session settings and the supplier terms. Platform owner with the security lead |
| 2 | The agent supplier and the model providers | Prompts and outputs | The permitted model list, the approved hosting location or boundary and the provider terms on training and retention (control 6). Security lead with the data owner |
| 3 | The run and the systems it reaches | The actions an agent takes | The run identity scoped to each run, the allow-list and read-only access to the work item. Platform owner |
| 4 | Retrieved content and the agent's instructions | An instruction carried in retrieved text | Control 8: retrieved text is data, instruction files are pinned and a build reads only the published version of the intent, using an authorised clarification answer only within its delegation boundary. Security lead |
| 5 | A controlled environment and the repository | Data read in a workspace | Control 9: production-derived content leaves a workspace only after the masking and classification checks the data standard sets, the categories barred from leaving production never do and agent output is scanned for secrets. Data owner with the platform owner |
| 6 | Non-production and production | A deployment | A person authorises at step 9, Authorise, deployment at step 10, Deploy, is held at L0 and only the attested candidate or release commit deploys. Release authority |

An agent's execution sandbox is destroyed when its run ends, and the approved artefacts and recorded checkpoints carry the state a later run needs. An agent session holds no production deployment credential, deterministic deployment uses a separate identity, an L4 action receives only its approved scope and a privacy assessment of the agent platform precedes the pilot.

## 6. The engineering platform

### 6.1 The platform as a product

The **engineering platform** is the set of environments, pipelines, run identities, allow-lists and shared definitions that carries every change through the flow. It is an internal product: the platform owner named in section 7 is its product owner, it has a backlog, the delivery teams are its users and adoption is its measure, read with the delivery measures in 8.2. A team may decline an optional service, but the shared pipeline and the mandatory checks apply to every repository. The platform provides seven services.

| What it provides | Why |
|---|---|
| Environments on request, from versioned definitions, refreshed with masked data by pipeline jobs | Every other service depends on them, and isolated workspaces remove conflicting change |
| One shared pipeline per deployment approach, called rather than copied and required by policy, or an approved adapter under platform ownership with the same checks and evidence | Identical checks apply to every team and agent, and no agent bypasses the required workflow |
| Release candidates with the evidence bound to them | Each change stays traceable and readiness can be computed |
| Publication of approved intents to the repository, with drafts, questions and status written back to the work item by the pipeline | People follow a change in one place and agents build only from a published version |
| The current-state architecture, regenerated after each release | Agents read the estate as it is built, and a run that finds it missing stops |
| The release manifest, computed from the records the flow creates | Release preparation is read from the record rather than assembled by hand |
| Post-deployment health checks and tested recovery | Recovery is exercised rather than assumed |

The platform reports code, checks, candidates and deployments back to the work item, so a person follows a change there from intake to acceptance, and 8.1 sets out the references that link each record to the next.

### 6.2 Standards in enforceable form

The practice owners keep ownership of their standards and publish them in a form a pipeline can enforce and an agent can retrieve, with enforceable rules and explicit values rather than aspirational guidance. The first two standards below are published before the first pipeline is built, because that pipeline enforces them.

| Standard | Owner | Minimum content |
|---|---|---|
| Risk tiering and test expectations | Practice owner for test (a lead test engineer) | The test types, coverage figure and performance requirement per tier, the blocking defect severity and the threshold rule for non-deterministic output |
| The pull request contract | Practice owner for engineering (a lead engineer) | What a pull request carries, its maximum size, who may approve, the required reviewers and the branch and merge rules |
| Architecture decisions and contracts | System steward | The decision index, the contract format, the versioning and deprecation rules with the notice before retirement and the exception regime |
| Security policy and thresholds | Security lead | The blocking finding severity, who may suppress, secret and credential rules, approved dependency sources and licences, version pinning with the route for dependency updates and testing beyond static analysis |
| Data definitions and quality | Data owner | The data dictionary standard, the restatement rule for a published metric and the quality, freshness and lineage checks |
| The design system | System steward or design steward | The component library every team builds to and the accessibility and usability checks for an interface change |
| Test data and masking | Practice owner for test with the data owner | The masking rule per environment class, consistency across connected contexts, the refresh cadence and the data that may never leave production |
| Release candidates, environments and the manifest | Release authority with the platform owner | The candidate per approach, the environment classes and refresh rules and the manifest fields required before authorisation |
| Non-functional thresholds | System steward | Numeric thresholds per service for response, throughput, run time and capacity, reviewed annually |
| Service level objectives | Product owner with the operations lead | The objective, its measurement window, the indicator and the exception and resumption decisions, in a versioned approval record |

The tiering standard sets what each tier requires before authorisation.

| Tier | Required before authorisation |
|---|---|
| High | Unit and contract tests, regression on the affected path, a performance test where triggered, a security review, exploratory testing, business acceptance in pre-production and an exercised recovery position, recorded in the manifest |
| Standard | Unit and contract tests to the coverage figure, automated regression on the affected component and a recovery position recorded |
| Low | The existing tests passing, with no new test required |

The practice owner for test records each required test, its owner, its step and any agent level in the tiering standard, and the context profile carries the platform-specific checks. An agent drafts acceptance tests at L2, Draft, from the business-owned examples and contract tests from the published contracts, never from its own implementation. It may use implementation detail for a unit test, and that test does not count as acceptance evidence. Manual accessibility checks for interaction patterns that automated checks miss, exploratory testing and penetration testing remain person-led, the last commissioned by the security lead.

### 6.3 The platform contract

The **platform contract** states the reciprocal commitments between the platform and each team.

| The platform commits to | The team commits to |
|---|---|
| An environment within a published lead time, refreshed on a published cadence, with an overdue request recorded as a platform defect | Releasing an environment it no longer needs |
| A shared pipeline that runs the same way for every team, with failures resolved by the platform team | Calling the shared definition rather than forking it and reporting a gap instead of a workaround |
| A delivery flow faster than the workaround, with a published route to change it | Departing from the flow only with an exception granted by the authority responsible for the rule under 3.2, never against a never-waived control |
| Standards the pipeline enforces, versioned, with time-limited exceptions visible | Running the checks locally before raising a pull request |
| Readiness computed into the manifest rather than requested from the team | Keeping the work item, the change and the change record linked |

Environment definitions are held centrally in the platform repository: the classes, how each is provisioned, its refresh cadence and its data set, a masked copy or a synthetic set. Provisioning, refresh and masking run as pipeline jobs that no agent runs, so production-derived data passes only through deterministic and logged steps, and an agent changes a definition through a pull request like any code. A shared pre-production class (the class nearest production) connects the contexts as production does with consistent masked data, and the regression and end-to-end tests at step 9, Authorise, run there with the pre-production acceptance of a high-tier change. Each context's definition sets the isolation, reservation and reset rules for that class, so that the tested composition and data state of each release are identifiable and the class does not become a queue.

### 6.4 Operating the platform

Shared pipelines, templates, checks, standards and agent configurations are software and carry the same engineering controls as other delivered software. A change to one follows the deployment approach of what it alters and carries a change record, and six rules apply.

- Versioned and released: a team calls a tagged version, and a breaking change is a new major version with a published window to move.
- Tested before rollout: a change to a shared pipeline is verified against a representative repository for each deployment approach before it reaches the estate.
- Rolled out in stages: a change goes to one team, then across a deployment approach, then across the estate.
- Pinnable: a team can pin a shared definition to its previous version while a regression is resolved, and the pin is recorded.
- Owned: the platform owner owns the definitions, templates, environment definitions, agent profiles and evaluations. The practice owners own the content of the standards.
- On the backlog: platform work sits on the platform backlog, with its capacity agreed and its priorities set by the platform owner with the product owners.

The platform is on the path to changing and recovering every service, so its recovery objectives are set from the most demanding tolerance among the services that depend on it and recorded with its service level objectives. It enters live support before the first release under the model.

Each context profile records a manual fallback for its automated steps that keeps the controls and evidence of the step it replaces. For promotion and deployment it is the manual route recorded before the context's first release (control 3 in 5.3). It is used while the pipeline or the deployment engine is unavailable, for an emergency change or for an authorised recovery on the release authority's decision. A named person uses it through a time-limited credential issued on the operations lead's authority, and an agent never does. It deploys only a release candidate already attested at step 7, Identify, and a tested route restores the minimum verification and attestation needed before a new candidate deploys while the platform is unavailable. Afterwards the deployment is reconciled into the manifest and the change record, use of the route is reviewed as an incident, each route is exercised in recovery testing and its credential's issue and use reach security monitoring.

## 7. Accountabilities

The model names accountabilities rather than posts. An adopter maps each accountability to a post it already has and records any gap with an owner, rather than assuming the post exists. Organisations staff them differently depending on product complexity, regulation, legacy burden and how much broader business change sits around software delivery. In a smaller team one person may hold more than one accountability. What matters is that each is explicit and its holder is named. The model also names a few holders outside the table: the practice owner for a discipline, the operations lead, the process owner and the accountable engineer for a unit. The adopter maps each in the same way, with its authority and its backup. The version 1.1 titles, Intent Architect, Verification Engineer, System Steward and Design Steward, remain available as post names.

| Accountability | What it decides and owns | Usually held by |
|---|---|---|
| Product owner | The request decision under 2.2, the outcome and measure the intent carries and acceptance at step 11, Confirm and learn. Each intent has one accountable product owner. | A product owner or product manager for the product area |
| Intent architect | The shape of the intent (its contexts, contracts, decomposition and delegation boundary) and the decision to publish it. | A solution architect or a lead engineer |
| Validators | The expression of the need and the behaviour (the business analyst) and the acceptance examples (the test engineer). | A business analyst and a test engineer, with no one who shaped the intent declaring it validated alone |
| Verifier | Verification at step 6, Review, by a code owner who neither formed the intent nor produced the change, on agent-produced work as on a person's. Version 1.1 called this post the Verification Engineer. | A code owner named per repository path, an experienced engineer |
| System steward | Coherence across the estate, the standards, the coherence register and technical debt. Design coherence sits here unless a design steward is appointed. | A chief architect or a lead architect |
| Release authority | The go or no-go at step 9, Authorise, on a change they did not request, build or implement, together with the release manifest and its ordering. | A release manager, within the production change authority of the operations lead |
| Platform owner | The engineering platform as a product (environments, pipelines, run identities and allow-lists) and the revocation of identities on suspension. | The product owner or lead engineer of the platform team |
| Security lead | The security standard and its thresholds, policy exceptions for security, the threat model for the agent platform and approval of each allow-list addition. | The head of information security or a named delegate |
| Data owner | Data definitions, masking, the personal data position and policy exceptions for data protection, advised by a privacy adviser. | The head of data or a data governance lead |
| Accountable executive | Business risk acceptance, the assurance view under section 9 and the decisions the organisation reserves to the business, taken before a change is built. | The executive accountable for technology or for the business outcome |
| The agent | No accountability. A configured participant that advises, drafts or executes under an agent profile and a run identity at the level assigned under 5.1, holding no approval or merge authority. | No one holds it. The platform owner configures it and the evaluation under 5.4 governs its level |

**Segregation.** Principles 4 and 5 bar anyone from approving their own work or being the sole authoriser of a change they requested, built or will implement, and two rules follow from them here. A person who formed an intent does not verify its output, because the assumptions built into the intent make its author a poor judge of whether the output matches it. Where the release authority requested, built or will implement a change, the go or no-go passes to the next holder of the same authority. Independent review at step 6, Review, supports that separation rather than discharging it.

**Continuity.** Each accountability has a named backup who is cross-trained to cover it and who can keep segregation for the work in question. The backup covers the accountability rather than taking it over, so the team does not stall when one person is absent. A stand-in does not verify work they formed. Where no backup without a conflict is available, delivery on the affected items pauses until the holder returns or a replacement is in place. Delivery pauses rather than compromising segregation, since maintaining throughput at its expense defeats the purpose of governed delivery. The adopter names who keeps the continuity plan that maps each accountability to its backup, with the product owner as the default for a team.

**Verification is risk-tiered.** Review by a person does not scale in step with agent output, so the reviewer's attention is allocated by risk rather than spread evenly. A low-tier change that passes every deterministic check at step 5, Verify, receives a confirmation that the output matches the intent. A high-tier change receives a deep review, as does a change of another tier where a shared service, a security boundary or a wide blast radius warrants one. The risk tier under 2.5 and the checks at step 5 set the depth. The reviewer assesses whether the code is maintainable, proportionate to the problem and faithful to the spirit of the standards as well as the letter. Over time the reviewer also assesses whether the codebase grows in proportion to the value delivered or accumulates boilerplate. Allocating attention by risk keeps review a control as throughput rises.

**Design coherence.** Agents can generate user interfaces, but without codified design constraints they tend to produce generic, inconsistent interfaces. The design system standard under 6.2 holds those constraints: the component library, interaction patterns, layout conventions, accessibility standards and brand rules. Design coherence sits with the system steward unless a design steward is appointed. A design steward is appointed where a product has a user-facing surface that warrants a dedicated holder. In a smaller team one person may hold both accountabilities where they have both skill sets.

**Senior judgement.** The model concentrates judgement in senior people and is not a reason to remove them. The intent architect accountability requires domain understanding, system-level awareness and the ability to anticipate where an agent will struggle or go wrong. The verifier accountability requires experienced engineers who understand systems at a level above individual functions and files. Junior engineers assist with evidence gathering while senior judgement takes the harder calls. The system steward owns the architectural direction, maintains the standards and instructions that constrain agent behaviour and identifies when refactoring is needed to restore coherence. Technical debt has a concrete impact and a named owner. It enters the flow as a request decided under 2.2.

## 8. Evidence and measures

### 8.1 The decision trail

The records below form one decision trail from the request to the release. Each record is created without transcription by the system where the decision or the work took place and carries the reference that links it to the next. The request reference links to the work item. The work item key and the intent version link to the branch and the pull request. The release candidate links to the change record, the deployment and the recovery position. A line of deployed code can therefore be traced back to the request and the decisions behind it. A request can be traced forward to the code and the release that carry it. For an agent-implemented unit the release candidate records the agent profile version and the run identity that produced the work, so a misconfigured profile can be traced to the changes it touched.

Provenance records what an agent did. It does not make the output reproducible, because the same intent run again may produce different code. The control is therefore the trail and a named person's approval of the artefact that shipped, not the ability to regenerate it identically. A reviewer or an auditor assesses the artefact that was approved rather than a hypothetical re-run.

| Record | System of record | Owner |
|---|---|---|
| The validated intent with its acceptance examples | The work item, with each published version in the repository | Product owner |
| Versioned code, configuration and migrations | The repository, under branch protection and code ownership | The lead engineer of the owning team |
| Reviews and merge decisions, including the agent's comments | The pull request record in the repository | The code owners |
| Findings and suppressions | The scanning the pipeline raises on the pull request | Platform owner, with the security lead for policy |
| Software bills of materials | The pipeline, with the attestation of each release candidate | Release authority, with the security lead for the format |
| Test evidence and the test completion record | The test estate, with completion recorded in the release manifest | Practice owner for test |
| Pipelines and deployment history | The pipeline and each context's deployment engine | Release authority |
| Defects | The work item, linked to the intent, the release candidate and the change record | Product owner |
| The release manifest | The pipeline that generates it, linked from the change record | Release authority |
| Change records and authorisations | The service management system, linked to the manifest and the authorised candidates | Release authority |
| Exceptions | The release manifest, with a policy exception also on the policy owner's register | The owner who granted it |
| Architecture decisions and gate decisions | The decision index in the standards repository, each carrying the work item key of the intent it governs | System steward, with each deciding authority |
| Objectives and error budgets | The telemetry, with a versioned approval record per objective | The operations lead |
| Agent profiles, evaluations, execution logs and session records | The repository for profiles and evaluations, the organisation's own log store for execution logs and the filtered session record of each run | Platform owner, with the security lead for the session records |

Each record is retained to the organisation's retention schedule and is available to internal audit, external audit and any regulator on request. Custody of a record grants no approval authority.

### 8.2 Measures

Instrumentation comes first. The flow and failure measures cannot be reported until commit, deployment and incident records are linked, so linking them is the first measurement task. Each measure is baselined before a target is set. No target is set where it would distort the decision the measure observes, so request decision outcomes and the share of changes in the high tier carry none. The table gives what each measure should prompt rather than a target.

| Measure | What it should prompt |
|---|---|
| First-time validation failure rate | A high rate prompts a review of intent preparation: the template, the intake, the retrieval sources and model performance, investigated before a conclusion is drawn. |
| Acceptance examples changed between draft and publication | It selects validation records for review. A textual difference does not itself establish challenge and correct examples may survive an effective review unchanged. |
| Request decision outcomes | A rate of redirect, defer or reject near zero prompts a sample to establish whether every request is approved or prioritisation upstream is effective. |
| Share of changes in the high tier | A rising share prompts a review of the tier criteria under 2.5, as a trigger rather than a target. |
| Lead time from approved request to live | Reported by risk tier, its observed distribution informs the forecasts the business needs. A rise prompts a look at the review, clarification and environment queues. |
| Release frequency per product | It shows whether intervals are shortening and is the evidence for shortening a product's release rhythm under 3.3. |
| Change failure rate | It shows whether more frequent release brings more failure, on a measure leadership already trusts. A rise holds the release rhythm and prompts a review of autonomy levels. |
| Time to restore | It measures the time from an incident caused by a change being raised to service being restored. It tests whether provenance and fast fixes shorten recovery. |
| Human effort per intent | It measures the net change across generation, review and rework, showing whether delegation reduces effort or transfers it. A rise without a quality improvement prompts investigation at the periodic review. |
| Stop-condition rate | A high rate usually indicates intent quality and can also reflect domain unfamiliarity or real ambiguity. The trend matters more than the absolute number. |
| Exception rate per context profile | It shows how far teams use the standard route, captured from the manifest. A rise prompts a review of the profile or the standard rather than of the people granting exceptions. |
| Environment wait time | It shows whether the platform meets its environment commitment. Environment scarcity is a platform defect, so a rising wait time becomes a platform backlog item. |
| Code health | The ratio of code removed or refactored to code added, weighted by complexity, kept as a sampling signal with its unit and its treatment of a period with no additions defined by the practice owner. A falling ratio prompts a sample of the affected changes for unnecessary growth or duplication, and a confirmed structural issue enters the coherence register. |

The practice owner defines each measure's denominator, window and attribution, so that a stop-condition rate per intent is not confused with one per run and first-time validation failure counts from the first formal submission rather than from iterative drafting. Measures feed back into the flow. Where one type of intent repeatedly fails validation or stops, the template, the intake questions or the retrieval sources are updated to address the pattern. Release rhythm and autonomy follow 3.3 and 5.4, and a confirmed structural issue enters the coherence register in appendix D.

## 9. Working with existing frameworks

**Service management.** The flow governs how a software change is produced. The surrounding service obligations remain: change management, incident management, problem management, service level management and configuration management continue to apply. Service management adds the operational coordination: who communicates with affected users, who decides between a workaround and a fix and who escalates to a third party.

The flow produces the change record from its own evidence, as a by-product of the work from step 7, Identify, to step 11, Confirm and learn, rather than as a separate manual gate. The information a change advisory board (CAB) would assess, what changed, what was tested and the recovery position, is already in the decision trail under 8.1. The release manifest is created at step 9, Authorise, from the change record and cannot assemble without it. The service management change type, standard, normal or emergency, is recorded when the intent is published.

A standard change catalogue pre-authorises classes of repeatable low-risk change. The change authority designates each class on the sponsorship of the function that owns it. The context profile supplies the known procedure and the tested rollback, so a profile alone does not make a change standard. A catalogue change skips the request decision under 2.2, and the catalogue entry supplies its change authorisation at step 9. It still needs a business purpose and an authorised requester, it follows the remaining steps of the flow and it receives the release go or no-go as every release does.

For a normal change the change authorisation and the release go or no-go are recorded separately at step 9. Step 10, Deploy, creates no further approval and production deployment stays at L0, Not used.

Emergency change is its own route, for a break-fix only. The emergency change authority authorises it on the minimum evidence that authority requires, the record is reconciled with the full evidence by the end of the next working day and the permanent fix follows the flow as an intent. Emergency releases are recorded separately in the manifest so that the pattern is visible. A missed cut-off or a commitment date is not an emergency.

Incidents and problems enter the flow as requests. Step 11 initiates the incident procedure when a release causes an incident. Known issues pass to problem management with owners and workarounds before the change record closes. A known error is not published until first-line support can execute the workaround. Under control 1 in 5.3 an agent does not publish a workaround it drafted.

Operational service level management continues. Production systems still need service level agreements (SLAs), availability targets and service reviews, owned by whoever manages live services. The flow feeds that discipline with the provenance and test evidence every release carries and does not absorb it. The release manifest and the decision trail feed whatever configuration management database (CMDB) the organisation keeps rather than a separate update process.

**Regulated environments.** The controls in the flow apply in every environment. In a regulated environment additional controls, accountability frameworks and assurance obligations apply. The model meets them as follows.

The never-waived controls under 3.2, the segregation rules in section 7 and the decision trail under 8.1 are the provisions a regulator or an auditor reads first. A named person is accountable for the governance of agent-produced change, and the decisions the organisation reserves to the business are named in advance and taken before a change is built. The accountable executive does not review every pull request but needs confidence that the controls work and that exceptions surface.

A periodic assurance view gives the accountable executive, a risk forum or an equivalent body a decision-ready view of whether the controls are working, whether exceptions are growing and whether the pace of change remains within risk appetite. Its cadence is proportionate to the pace and risk of change.

| The assurance view covers | Content |
|---|---|
| Volume | Agent-produced change, releases and material changes since the previous view, by risk tier, with emergency releases shown separately |
| Incidents | Open incidents, near misses and post-incident actions relating to agent-delivered change |
| Exceptions | Open and expired exceptions and the high-risk decisions taken since the previous view |
| Trends | First-time validation failure rate, stop-condition rate, human effort per intent, change failure rate and code health |
| Agent configuration | Material changes to the permitted model list, hosting, access controls or autonomy levels |
| Open actions | Regulatory, compliance or control actions awaiting attention |

A sector overlay adds fields to the intent under 2.1 and gate triggers at step 2, Gate. It adds constraints on the permitted model list and on data access. It does not change the core. An adopter maps each of its policy requirements to the step or control that discharges it.

## 10. Adopting the model

Adoption is an operating model transition rather than a tooling rollout. It proceeds in the order below.

**Make the codebase legible first.** The model treats the codebase as a knowledge store that agents read. Legacy systems are often not legible enough for that to work, so brownfield adoption usually needs an upfront investment: characterisation tests, clear module boundaries and current documentation. This work goes through the flow, specified and governed as any other change. It is a precondition for agent productivity rather than a side task. A unit can be agent-implemented only where the change has a version-controlled representation, so a context's first work is whatever makes its changes representable and promotable unchanged.

**Run a measured pilot.** The pilot is where an organisation produces its own evidence. A useful pilot fixes a small scope, takes a baseline from the team before adoption and measures the leading measures from the first day: lead time, first-time validation failure rate, stop-condition rate and human effort per intent. The security lead completes the threat model and a privacy assessment of the agent platform before the pilot. Security monitoring of the platform is in place before any step rises above L1, Advise. Consumption measured in the pilot sets a spend cap before wider rollout. Decision gates at the end of the pilot decide whether to scale, adjust or pause.

**Start agent advice at L1 and delegate execution only on evidence.** The levels in 3.1 are the levels a step reaches once the evidence under 5.4 supports them. Until then agents advise at L1, Advise, while Workspace, Identify, production deployment and acceptance stay at L0, Not used, and the first delegation at L2, Draft, needs the first-delegation evidence under 5.4 as every later increase does.

**Publish the first two standards before the first pipeline.** Risk tiering and test expectations and the pull request contract are published before the first pipeline is built, because the first pipeline enforces them.

**Prove the platform on one delivery context before the next.** The first release of the platform covers one context. Further contexts and self-service follow once the first is in use, with each required standard enforceable before a context uses it. Each context records its manual fallback before its first release and each team submits its low-tier changes as catalogue candidates. Until automatic evidence capture is operating for a context, a manually maintained index records where each release's evidence sits.

**Keep the implementation plan separate.** The adopter's lifecycle design and its implementation plan are separate documents. The plan is governed by the model and carries the construction work, its sequence and dates, its dependencies and its open items with their owners.

ADOPTION.md carries the phases, team sizing, brownfield guidance, career paths and workforce change.

## 11. What this model is not, and its limits

The model is not a product discovery method. User experience research, user testing and product discovery sit outside it and their output feeds in as context, although design system governance and user interface quality are inside it once a product has a user-facing surface. Agents execute implementation and people still decide what to build and why. It is not a reason to remove senior engineers: the accountabilities in section 7 carry more senior judgement than a conventional team, so the team may be smaller but it is not less skilled. It is not tool-specific. The model is about operating structure rather than vendor choice: what depends on a product sits in the context profile and the agent profile. It is not finished. It will evolve as tooling matures, as organisations accumulate experience and as the community contributes. It does not govern a vendor's own delivery process, infrastructure operations or service desk work, and those sit alongside it under their own operating models. A supplier-produced change that the organisation versions and promotes follows the flow, so the boundary is whether a change is versioned and promoted through the flow, not whether code is written.

The model is based on practical experience and informed judgement rather than on controlled studies. The limits below define the boundaries of what it can claim with confidence.

| Limit | What it means for an adopter |
|---|---|
| Specification throughput | The model assumes that intents can be formed and validated fast enough to keep the agents occupied, with no published data on the rate one person can sustain across domains. It is the most important empirical question the model raises, so the pilot measures the rate and validation and review capacity caps approved work. |
| Talent scarcity | The model concentrates judgement in people who can hold the intent architect, system steward and verifier accountabilities, for whom the market is tight. Most organisations will grow them internally rather than hire them. |
| Verification at scale | Review by a person does not scale in step with agent output and the ratio of agent output to review capacity where review stops being a control is not known. Risk-tiered review and human effort per intent are watched from the first day. |
| Agent quality plateau | If agent quality plateaus at a level that requires heavy rework by people, the review overhead could exceed the implementation saving. Human effort per intent is tracked and the cost assumptions are revisited if it does not fall. |
| Workforce transition evidence | No published case study of a full transition with before-and-after cost and quality data is known to the author. The model makes no headcount claim, and the pilot establishes the staffing implications. |
| Security maturity | Prompt injection defence is immature and the model identifies the threats under 5.3 and 5.5 without claiming to solve them. Agent security is an evolving risk that needs continuous investment rather than a one-time configuration. |
| Tooling dependency | The model is tool-neutral but depends on agent capabilities that advance unevenly, so assumptions built on one provider's current capability may fail when models change. The permitted model list holds more than one model and the agent profile pins the version. |
| Code quality trajectory | Agent-generated codebases are new and whether they become more consistent as standards mature or accumulate structural issues that compound is not yet known. The code health ratio is tracked over time and early adopters publish their findings. |

The model relocates the conceptual work from code to the intent and does not remove it. Writing a precise intent for a complex domain is at least as hard as writing the code, the essential difficulty Brooks described in 1987. An adopter monitors these limits and contributes findings back to the community as evidence accumulates.

## An invitation

The Agentic Delivery Model, version 2.0, is published as an open methodology. It is designed to be used, adapted, challenged and improved by practitioners working in real enterprise environments.

If you are a chief technology officer, a chief information officer or an engineering leader working through this transition, get in touch with the author. If there is sufficient interest, a working group will be established to evolve the model collaboratively. Frameworks improve when a community of practice refines them. Findings from measured pilots are the evidence the next version will be built on.

---

**Dan Simms**
CIO / CTO. Founder of [Oxygen Bubbles](https://oxygenbubbles.com), an AI strategy consultancy helping organisations navigate the shift to agentic delivery.
[LinkedIn](https://www.linkedin.com/in/dansimms)

*The Agentic Delivery Model is licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE).*

---

## Appendix A. The intent template

The template carries the request record and the ten elements of the intent in the order given under 2.1. Every element is answered, including "considered, does not apply". The work item holds the working copy while the intent is formed and validated. The repository holds each published version, the only version a build uses.

```
INTENT
Work item: [key]
Title: [short title naming the change]
Status: draft | in validation | published | in build | released | accepted
Risk tier: high | standard | low
Published version: [identifier, blank until published]

REQUEST RECORD
Request reference: [reference]
Problem: [what is wrong or missing, in the requester's own terms]
Expected benefit or obligation: [the measurable benefit, or the obligation that requires the change]
Business owner: [name]
Indicative effort: [order of magnitude]
Alternatives considered: [including doing nothing]
Work displaced: [what waits if this is approved]
Decision and rationale: [approve | redirect | defer | reject, with the reason and the date]

1 OUTCOME AND MEASURE
[the outcome in the business owner's terms, the observable measure that shows it is achieved, the person who accepts it and the colleagues whose work it changes]

2 SCOPE BOUNDARY
In scope: [the change to make]
Must follow: [patterns, services and schemas]
Must not touch: [boundaries and services out of scope]

3 ACCEPTANCE EXAMPLES
Given [precondition] when [action] then [expected result]
Given [precondition] when [action] then [expected result]
[edge cases and error paths follow the main path]

4 CONTEXTS AND EXECUTION UNITS
Leading context: [the context whose repository holds the published version]
Unit [n]: [context, what it alters, deployment approach, rollout order and the conditions for its activation]

5 CONTRACTS TO HONOUR
Unit [n]: [interfaces, services and schemas, with each contract version produced and each version consumed]

6 DATA AND SECURITY POSITION
[data touched and its classification, masking, permissions changed, retention, any protection assessment, the threats introduced with their requirements and the gates triggered]

7 RECOVERY POSITION
[the planned recovery position for each unit among the three under section 4, the safe partial states, the units recovered together and the proof required before authorisation]

8 DELEGATED JUDGEMENT AND STOP CONDITIONS
Inside the boundary: [decisions the agent may take and record]
Stop conditions: [what halts the run for a person]

9 ASSUMPTIONS AND OPEN QUESTIONS
[each assumption, stated so it can be tested, with its owner. Each open question with its closing action and owner]

10 DEPENDENCIES
Unit [n]: [what it waits on, what waits on it and their state, bound to the release candidate identifiers at step 7]

AUTONOMY CLASSIFICATION
Unit [n]: [agent-implemented at the approved level for its step and class, or person-implemented]. Validation and release authority stay with people, and production deployment, production data change and acceptance stay at L0

PROVENANCE
Drafted by: [agent profile and version, run identity]
Shaped by: [intent architect]
Validated by: [business analyst, test engineer, each with the evidence relied on]
Published version: [identifier and repository]
Approved by: [product owner, request decision and date]
```

Acceptance examples may be written in Gherkin, as the template shows, or in EARS (Easy Approach to Requirements Syntax). Either notation is acceptable, since the notation matters less than the examples being concrete, executable and business-owned. Where the output is not deterministic the examples carry a threshold over a set of cases rather than a single expected result.

## Appendix B. The context profile template

A context profile is the approved route from a request to production for one delivery context. It holds what is specific to a platform, so that the flow in section 3 holds nothing specific to any. A platform enters the flow only through a context profile. Until it has one its changes follow their existing route.

```
CONTEXT PROFILE
Delivery context: [the platform and its build and deployment mechanism]
Profile owner and version: [name, version, date approved]

Deployment approaches carried: [artefact-based | state-based | migration-based, the usual one first]
Workspace and allocation: [the isolated place a change is made, how the pipeline allocates it from a versioned definition and how it is refreshed]
Local check: [what an engineer runs before raising a pull request, from the same rule versions the pipeline uses]
Pull request contract reference: [the published contract and its version]
Verification checks at step 5: [the deterministic checks and thresholds, by risk tier]
Release candidate form: [what step 7 produces and attests and the identifier it carries]
Promotion sequence and environment values: [the deployment engine, the environment order and what is supplied as an environment value rather than carried]
Recovery position and its proof: [the position that applies and the evidence the manifest carries]
Post-deployment verification: [the health checks that provide the evidence for acceptance at step 11]
Manual fallback route: [the route used when the pipeline or engine is unavailable, keeping the controls and deploying only an attested candidate]
Catalogue changes designated: [the classes pre-authorised for this context and the entry for each]
```

Any exception to a step is recorded in the profile with its owner and expiry. An operation that needs a named person's approval in addition to the authorisation at step 9, Authorise, is listed in the profile and resolves to an accountability in section 7. A destructive change and a permission change that widens access are examples.

## Appendix C. The agent profile template

An agent profile is the versioned record for one agent. It is held in version control and changed only through a pull request. An increase in level takes effect only through the attested release of the platform and a reduction takes effect at once. The organisation-level settings, meaning the autonomy ceiling, the permitted model list and the allow-list, are held outside the profile and are authoritative over it. The run identity is issued per run and scoped from the profile at the point of issue.

```
AGENT PROFILE
Profile identifier and version: [identifier, version]
Step and class of change served: [step, class]
Autonomy level: [L0 | L1 | L2 | L3 | L4]
Model and version: [model, version, pinned]
Inference hosting and provider: [hosting location or boundary, provider, from the permitted model list]
Tools it may call: [each tool, from the allow-list]
Scopes it holds: [the read, comment, write and execute permissions for the step and class, bounded by the approved autonomy level]
Context sources it reads: [each approved source and its permitted scope]
Instruction files at pinned versions: [repository and path instructions and skills, each with its version]
Stop conditions: [what halts the run]
Reviewing model pairing: [the model that reviews this agent's output, as the evaluation established]
Evaluation reference and result: [evaluation identifier, changes evaluated, acceptability, review and rework measured]
Period at the level below: [number of changes, completed on]
Approval record: [approving authority, practice owner who accepted the evaluation, date]
Suspension conditions: [what suspends execution authority, including a material configuration change]
```

## Appendix D. The coherence register

The coherence register serves three purposes. It is a decision log, a record of significant technical decisions, their rationale and their review dates. It is a technical debt register, where every identified item of debt is logged, severity-rated and given an explicit accept or remediate decision with a target date where applicable. It is a coherence risk tracker, a living record of where the codebase is at risk of drifting towards inconsistency. The system steward owns the register and presents it at the periodic review under 3.3.

The decision log is the decision index named under 8.1. It holds architecture decisions, design departures and the gate decisions at step 2, Gate, each carrying the work item key of the intent it governs. Technical debt is a managed register with an accountable owner rather than an emergent property. When debt is marked for remediation, the system steward raises a request. Once approved under 2.2 the remediation enters the flow as an intent and is delivered like feature work, so debt is identified, governed, remediated and verified in one loop. A stale feature flag, a retained local pipeline definition and a bespoke component built where configuration would have served are each register entries. Design decisions sit in the same register, with the design steward owning their entries where one is appointed.

```
COHERENCE REGISTER
Last updated: [date]
Owner: [the system steward]

DECISION LOG
| ID | Decision | Work item | Date | Rationale | Review date |
| AD-[n] | [the decision taken] | [key] | [date] | [why] | [date] |

TECHNICAL DEBT
| ID | Description | Severity | Owner | Decision | Target date |
| TD-[n] | [the item] | [high, medium or low] | [name] | [accept or remediate] | [date, or none] |

COHERENCE RISKS
| ID | Risk | Likelihood | Impact | Control |
| CR-[n] | [where the codebase is drifting] | [high, medium or low] | [high, medium or low] | [the standard or check that answers it] |
```

## Appendix E. Defined terms

| Term | Meaning |
|---|---|
| Acceptance examples | Concrete, business-owned examples of correct behaviour, including edge cases and error paths, held as executable tests. |
| Agent | A software agent built on a language model that advises, drafts or executes within an assigned autonomy level under a run identity. An assistant inside a product is not one. |
| Agent profile | The versioned record for one agent: what it serves, its level, model, tools, scopes, stop conditions and approval (appendix C). |
| Allow-list | The organisation-level list of the systems, servers and tools an agent may reach and the operations permitted. |
| Attestation | The pipeline's signed record that a named commit or release candidate passed the required checks and reviews. It applies only to that object and cannot authorise a changed one. |
| Autonomy level | One of five levels under 5.1, from L0, Not used, to L4, Execute in production within a scope, assigned to a step and class of change. |
| Catalogue change | A change in a pre-authorised class of repeatable low-risk change, with the context profile supplying the procedure and the tested rollback. |
| Class of change | The category, by tier and effect, that carries an autonomy level, so a level at one class does not authorise another. |
| Coherence register | The system steward's register of architecture decisions, technical debt and coherence risks (appendix D). |
| Context profile | The approved route from a request to production for one delivery context (appendix B). |
| Delivery context | One technology platform with its own build and deployment mechanism, determined by the estate rather than chosen. |
| Deployment approach | How a change is built, identified, promoted and recovered according to what it alters: artefact-based, state-based or migration-based. It is distinct from a deployment strategy such as canary. |
| Deterministic check | A pipeline check that enforces a rule that is never waived, distinct from a threshold check that may carry an exception. No agent may disable, skip, re-run to a pass or mark any check as satisfied. |
| Engineering platform | The environments, pipelines, run identities, allow-lists and shared definitions provided to every team as an internal product (section 6). |
| Evaluation | A test of an agent against the organisation's own historical changes for a step and class, measuring acceptability and the review and rework required. |
| Exception | A time-limited departure from a control or from the flow, granted by the authority responsible for the rule, with a named owner and an expiry, recorded in the manifest and never setting aside a never-waived control. |
| Execution unit | A separately controlled part of an intent, each its own release candidate with its own recovery position and autonomy classification. |
| Flow, the | The single sequence of eleven steps under 3.1 that applies to every change in every context. |
| Intent | The unit of work: a specification complete enough to execute without further explanation, carrying the ten elements under 2.1 and built only from its published version. |
| Never-waived control | One of the six controls under 3.2 that no exception can set aside. |
| Permitted model list | The organisation-level record of each model an agent may use, with its provider, hosting location, version and retention terms. |
| Recovery position | One of the three tested positions under section 4: proven rollback, restoration of the previous desired state (or a compensating change where restoration is unavailable) or, for an irreversible change, a tested restore with a forward remediation route. |
| Release candidate | One execution unit in the form attested at step 7, Identify, with a unique identifier binding its checks, review and references, promoted unchanged. |
| Release commit | On a platform that promotes source, the destination-branch commit used for deployment, distinct from the unit commit and named by the authorisation. |
| Release manifest | The record assembling readiness, ordering, exceptions and the recovery position for a release, created from the change record at step 9, Authorise. |
| Request | The recorded statement of a need that enters the request decision under 2.2, with its problem, expected benefit or obligation and business owner. |
| Risk tier | High, standard or low, defined by the effect of the change under 2.5, with the higher tier applying where two could. |
| Run identity | The scoped, short-lived identity issued for each agent run, carrying its permissions and the log of its actions. |
| Step | One stage of the flow, with a defined input, one operation to one standard and a defined output, implemented per platform by the context profile. |
| Threshold | A numeric figure a standard sets, such as a coverage level or a performance limit, that the pipeline checks and that may carry a threshold exception from the practice owner who set it. |
| Validation | The assessment that the intent is the right change, made before implementation under 2.3 and confirmed at step 11, Confirm and learn. |
| Verification | The assessment that the change conforms to the intent and the standards, by deterministic checks at step 5, Verify, then a reviewer at step 6, Review. |
| Work item | The record where a request becomes an intent, holding the working copy and each validator's record and linking to the branch, pull request and release candidate. |
