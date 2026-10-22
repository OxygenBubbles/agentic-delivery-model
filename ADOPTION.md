# Adopting the Agentic Delivery Model

**Companion to [the Agentic Delivery Model version 2.0](MODEL.md)**

This guide covers the practice of adopting the Agentic Delivery Model: the phases of adoption, the working day, how roles are likely to change, team structure, career paths, release patterns and workforce change. The model ([MODEL.md](MODEL.md)) defines the flow, the controls and the accountabilities, and section 10 of the model sets the order of work that this guide expands. Where the guide depends on part of the model it points to the section that defines it, so you read the two side by side.

The model names accountabilities rather than posts (section 7 of the model). This guide uses Intent Architect, Verification Engineer, System Steward and Design Steward as example post titles, beside generic posts such as product owner, business analyst, test engineer and release manager. You map each accountability to a post you already have and record any gap with an owner.

Products change faster than the model, so the guide names none and [the tooling notes](Tooling/README.md) describe how particular toolchains run it.

---

## 1. Adopting the model

Adoption involves roles, governance, tooling, delivery confidence and the workforce together, so it runs in four phases rather than as one move. The phases describe what happens rather than a calendar, and their duration depends on the organisation's appetite for change, its risk tolerance and its existing workload. Phase 1 and the pilot often take longer than expected, because team capacity, existing infrastructure, the standards still to write and the learning needed all constrain them, so the adopter plans them with margin.

### 1.1 Phase 1, foundations and enablement

Phase 1 puts the operating conditions in place before agents begin delivery work.

**Leadership alignment.** The organisation names the leader accountable for the transition, defines its scope and sets the success criteria. It states whether it is testing tooling, adopting the full operating model or preparing a workforce redesign, because confusion on that point creates false expectations later.

**Role design and training.** The adopter identifies who will hold each accountability in section 7 of the model ([MODEL.md](MODEL.md)) and records any gap with an owner. Training happens in this phase, ahead of the move into new posts that section 9 of this guide sets out, since a change of title alone does not change how someone works. Intent Architects practise forming intents with every element answered and validators practise the break assessment of acceptance examples. Verification Engineers practise review against the intent and its evidence rather than code style, System Stewards practise running the coherence register and release managers practise reading the evidence bound to a release candidate.

**Tooling setup.** The platform owner configures the minimum pilot environment: the link between the work management system and the repositories, repository permissions and branch protection, isolated workspaces, the tool allow-list, test execution, logging and capture of the decision trail. The security lead and the data owner agree the first entries on the permitted model list, with each model's hosting and fallback. The platform owner confirms how each run identity appears in commits, pipeline logs and the work item, and each toolchain provides these controls differently, as [the tooling notes](Tooling/README.md) describe.

**Risk assessment.** Agents change the risk profile of delivery, so the adopter assesses that risk before any agent produces code.

| Area | What the assessment covers |
|---|---|
| Data handling | What agents can read, where inference takes place and what crosses each trust boundary under 5.5 of the model |
| Intellectual property | The provenance of code and licence compliance |
| Security | Workspace isolation, credential management and dependency governance |
| Operational resilience | Failure modes and the recovery position for each deployment approach (section 4 of the model) |

Under the UK General Data Protection Regulation, for example, the assessment determines whether a data protection impact assessment is required. Other jurisdictions and sectors carry equivalent obligations, such as the EU Artificial Intelligence Act, the Health Insurance Portability and Accountability Act in US healthcare, the Payment Card Industry Data Security Standard and the Financial Conduct Authority's resilience rules. The legal, security and compliance functions therefore take part from phase 1. The adopter maps each policy requirement to the step or control that discharges it (section 9 of the model). The security lead completes the threat model and the privacy assessment of the agent platform before the pilot (section 10 of the model), since building controls in from the start costs less than retrofitting them.

**Baseline standards.** The practice owners publish the first two standards, risk tiering with its test expectations and the pull request contract, before the first pipeline is built (section 10 of the model). The adopter also prepares first versions of the intent template, a context profile for the pilot platform and an agent profile for each step an agent will serve (appendices A, B and C of the model). Each has to be good enough to support repeatable delivery and review rather than perfect.

**Pilot selection.** The adopter chooses a bounded pilot area where the team can learn without unacceptable operational or regulatory risk. A well-understood product area with a tolerable blast radius suits a pilot better than the most visible or least stable part of the estate, and a brownfield pilot area needs baseline test coverage first (1.5 of this guide).

### 1.2 Phase 2, a confidence-building pilot

The pilot shows the model working end to end in the organisation's own environment and aims at confidence rather than volume. Section 10 of the model sets its shape, from the baseline taken before adoption to the decision gates at the end. The pilot establishes:

- whether the people holding the accountabilities can work in the rhythm under 3.3 of the model
- whether the agent configurations and controls are strong enough for governed delivery
- whether the test strategy catches regressions
- whether the decision trail satisfies audit, security and regulated stakeholders
- the real human effort per intent, including review and rework

A suitable pilot runs on a contained internal product, a non-critical slice of a product or a test project that mirrors real conditions, and people review every output closely. Beside the leading measures in section 10 of the model, the team watches the change failure rate and the age of the oldest pull request in the review queue.

**Greenfield and simulation pilots.** Many teams find it easier to start on greenfield code or a fictional project, because it removes legacy constraints and lets people concentrate on the accountabilities, the rhythm and the controls. The journey of a change ([JOURNEY.md](JOURNEY.md)) can serve as the basis for such an exercise. The team assigns the accountabilities, configures an agent profile for each step it will delegate and runs several changes through the flow in a fresh repository. The exercise builds skill but is not an evaluation under 5.4 of the model, because an evaluation uses the organisation's own historical changes. For a team new to agentic development it is often the fastest route to confidence.

### 1.3 Phase 3, controlled scale-up

Once the pilot has built confidence, every change in the scaled area runs through the flow under the rhythm in 3.3 of the model. Autonomy rises one level at a time on the evidence under 5.4 of the model, and in a regulated environment the same evidence feeds the assurance view in section 9 of the model. The people holding the intent architect, verifier, product owner and system steward accountabilities work routinely in their posts with explicit support and coaching. The adopter tightens standards, permissions, tests and escalation routes on the pilot's findings, aiming at controlled repeatability rather than maximum speed.

### 1.4 Phase 4, scaled operation

Phase 4 extends the model to further product areas, adding cross-product System Steward capacity where coherence across products needs it. Leadership receives the measures in 8.2 of the model, with human effort per intent and lead time from approved request to live as the headline pair. The adopter refines model selection, standards, training, governance and the balance between review capacity and agent runs on the accumulated evidence rather than to a target.

### 1.5 Brownfield environments

The phases assume reasonable test coverage, clear boundaries and documented interfaces, and many enterprise codebases have none of these. Section 10 of the model makes the codebase legible first, and the practice below adds what an existing estate needs.

**Test coverage first.** A thin or unreliable test suite moves the burden of verification onto people. Before the pilot begins, the adopter builds enough automated tests around the boundaries agents will touch to catch regressions, rather than chasing a blanket coverage target. Once test generation has its own evidence under 5.4 of the model, an agent at L2 can draft further characterisation tests for a person to review. Those tests describe current behaviour and do not count as acceptance evidence (6.2 of the model).

**Start at the edges.** In a tightly coupled monolith, agent work starts at the boundaries: new endpoints on an existing interface, services that integrate with the core without modifying it and user interface components backed by existing services. Agents work deeper in the codebase as coverage improves and boundaries become clearer.

**The System Steward early.** The existing architecture is the constraint, so the System Steward maps it, defines the boundaries agents must respect and writes down the rules experienced engineers carry in their heads. That transfer of knowledge is the hardest part of brownfield adoption, and without it agents break unwritten rules and produce code that is architecturally wrong.

**Debt and rework.** Agents surface debt that people used to work around, such as inconsistent patterns, duplicated logic and missing error handling, and the coherence register fills quickly. The System Steward plans for a substantial volume of refactoring requests in the early months. Human effort per intent and the stop-condition rate run higher than in a well-structured codebase and improve as the System Steward codifies conventions into standards that agents retrieve, so the adopter plans for a slower ramp.

### 1.6 Adapting to team size

A smaller organisation combines posts, runs fewer agents at once and keeps a lighter rhythm, such as asynchronous review and a monthly periodic review. A larger one separates posts more clearly, adds specialist capacity and formalises assurance further. At every size the accountabilities and the control points stay intact, under the segregation and continuity rules in section 7 of the model.

## 2. How people work under the model

### 2.1 The working day

Section 7 of the model ([MODEL.md](MODEL.md)) sets what each accountability decides and 3.1 of the model sets where it acts. The table shows how three of the posts spend the working day once the flow is running, and section 3 of this guide covers the others.

| Post | What fills the working day |
|---|---|
| Product owner | Request decisions, priorities, quick answers to the business questions halted runs raise and acceptance at step 11, Confirm and learn |
| Intent Architect | Shaping drafted intents, deciding their decomposition, contracts and delegation boundary and answering clarification questions on feasibility and scope |
| Verification Engineer | Reviewing pull requests as they arrive at step 6, Review, and between reviews working the queue, exploratory testing and checking that automated coverage is meaningful |

Each pull request that reaches step 6 has passed the deterministic checks at step 5, Verify, and meets its thresholds or carries an authorised exception. The Verification Engineer judges whether the change conforms to the intent and the standards and is structurally consistent, since validation has already settled whether the intent was the right change. How many pull requests one person can review properly in a day varies with the codebase, the risk tier and the maturity of the checks. The team sets the cap on approved work under 2.2 of the model from that observed capacity rather than from a target.

### 2.2 Forecasting without estimation

The model has no sprints or fixed periods, because agents execute continuously against published intents. The question for a new request is whether approving it keeps the team within its validation and review capacity, and validation under 2.3 of the model takes the place of estimation.

The business still needs expected delivery windows for training, user acceptance testing, release coordination and planning, and the team forecasts them by measuring lead time from approved request to live by risk tier and context profile. The observed distribution shows a constraint as it forms: a rising first-time validation failure rate points to the forming of intents, and rising lead time with steady validation points to build, verification or review.

### 2.3 Where bottlenecks form

The model moves the bottlenecks of delivery from implementation to forming and validating intents and to review. The flow and its rhythm (3.1 and 3.3 of the model) manage them without removing them.

| Bottleneck | How it forms | What relieves it |
|---|---|---|
| Intents | Agents build faster than people form and validate intents, so runs wait for work. It is likely to be the binding constraint at maturity | Drafting by the agent at L2 from the request and the history it can retrieve, a pattern library for recurring shapes of change and a person beside the agent in a complex or ambiguous domain (2.3 of the model) |
| Review | Agent output arrives faster than people review it and the queue grows | The cap on approved work (2.2 of the model), checks and thresholds at step 5 and agents commenting at L1 first, so the reviewer concentrates on conformance and structure |
| Clarification | Runs halt at stop conditions while Intent Architects are busy, and poorly formed intents raise more halts | Validation with every element answered, including the delegation boundary and the stop conditions |
| Standards | Agents decide without enough constraint when the System Steward falls behind, and code that passes its tests drifts structurally | Standards in enforceable form that agents retrieve (6.2 of the model) and the coherence register (appendix D of the model) |
| Design | Uncodified design constraints produce inconsistent interfaces on a user-facing product, or interface review falls behind | The design system standard (6.2 of the model) and a Design Steward where the surface warrants one |

### 2.4 Leading indicators

The measures in 8.2 of the model state what each should prompt, and the team adds two queue indicators beside them. The number of validated intents waiting for a workspace shows whether forming intents is the constraint, and the age of the oldest pull request in the review queue shows whether review capacity needs attention. With the stop-condition rate and human effort per intent, they give four early indicators of the bottlenecks above.

## 3. How roles are likely to change

### 3.1 The status of the forecast

This section is a forecast and not a requirement of the model ([MODEL.md](MODEL.md)). The model's normative content on people is the accountabilities in section 7 of the model, and posts are the adopter's choice. The forecast describes how posts are likely to change as agents take on more of the work, and an organisation tests it against its own evidence before acting on it. It carries no dates and no headcount figures, and each human profile in [the profiles](Profiles/README.md) carries the same forecast for its post.

### 3.2 Three horizons paced by autonomy

The forecast is paced by autonomy rather than by the calendar, because the model raises autonomy only on evidence (principle 8 and 5.4 of the model). Levels are assigned per step and class of change, so an organisation can stand at different horizons for different classes at once.

| Horizon | What it looks like |
|---|---|
| 1. Drafting | Agents draft at L2 at most delegated steps and run approved pipeline jobs at L3 under a gate. Most adopters reach this horizon first, once the pilot has produced the evidence for the first delegation above L1 |
| 2. Executing under gates | L3 is common for standard-tier and low-tier changes, with L4 in narrow, well-evidenced scopes such as dependency updates, documentation and configuration within set limits |
| 3. Executing within wider scopes | L4 covers a wider set of scopes where the evidence supports it, and people spend most of their time on intent, judgement and the platform |

An L4 scope at any horizon is a catalogue action within a named scope under 5.1 of the model, and production deployment and production data change stay at L0 throughout (principle 5).

### 3.3 The direction for each post

The table gives the likely direction for each post where an organisation has it, and appendix A.3 of this guide maps the posts of a traditional delivery function onto these.

| Post | Likely direction |
|---|---|
| Product owner | More time on outcome measures and acceptance at step 11, Confirm and learn. The request decision becomes the main lever as building gets cheaper, and the cap on approved work makes prioritisation sharper |
| Intent Architect, drawn from solution architects, lead engineers and senior business analysts | Demand for the post rises. The intent becomes the main engineering artefact, and skill in decomposition, contracts and the delegation boundary becomes scarce |
| Business analyst, as a validator | Less time writing requirements and more time validating intents, running the formation passes and testing the need with the business |
| Test engineer, as a validator | Less time executing tests and more time designing acceptance examples and building the evaluations that raise an agent's level under 5.4 of the model |
| Verification Engineer, holding the verifier accountability | Review becomes the constraint on throughput, supported by agents at L1. The post concentrates senior judgement, and demand for review skill grows |
| System Steward, with the Design Steward where one is appointed | Standards in enforceable form become the context agents retrieve, so the work moves from guidance to maintaining that context and the coherence register as the volume of change rises |
| Release manager, holding the release authority | Coordination falls as readiness is computed at step 9, Authorise. The post becomes a judgement on risk and on recovery positions, held by fewer and more senior people |
| Platform owner and platform engineers | The largest new demand is likely to fall here, because running agents as part of the platform sits with these posts: run identities, allow-lists, evaluations, cost and the manual fallback |
| Security lead | More policy expressed as code, approval of allow-list additions and threat models that cover agents and the six trust boundaries in 5.5 of the model |
| Data owner | The data position for models, masking for agent context and the categories of data that never leave production become recurring decisions rather than occasional ones |
| Engineers | Less time writing code by hand and more on decomposition, acceptance examples, review, stop conditions and platform work. The routine work that trained new engineers moves to agents, so the entry route has to be designed deliberately (5.1 of this guide) |
| Accountable executive | The post is unchanged in substance. The assurance view and risk acceptance remain, with richer evidence from the decision trail |

### 3.4 What stays with people at every horizon

At every horizon the accountabilities stay with people and the agent holds none (section 7 of the model). The request decision, validation, approval of someone else's work, authorisation of every production change, acceptance, exceptions and risk acceptance remain decisions for a person, under principles 4 and 5 and control 1 in 5.3 of the model. Rising autonomy changes who drafts and executes the work and leaves unchanged who decides and who authorises it.

### 3.5 Signals that the forecast is arriving

Four signals show a horizon arriving. Human effort per intent and the stop-condition rate fall (8.2 of the model), the validation and review queues become the binding constraint and more steps are raised a level under 5.4 of the model. The periodic review under 3.3 of the model reads them together, beside the autonomy levels in force, because one signal alone can mislead.

### 3.6 What an organisation does about it

An organisation prepares before the signals arrive in full, because the people who hold the scarcest accountabilities are mostly grown internally rather than hired (section 11 of the model). It redesigns career paths around the accountabilities and the directions in 3.3 above, as section 5 of this guide sets out, and designs the entry route for new engineers deliberately (5.1 of this guide). It reskills people whose work moves to agents through the workforce transition in section 9 of this guide.

## 4. Team structure

### 4.1 Caveats on comparison

The comparisons in this section and in appendix A of this guide, including the scaling model in A.4, are thought models rather than measured comparisons. Organisations differ in product complexity, regulation, legacy burden, sourcing and platform mix, and some functions, such as project management, a programme management office (PMO), security and architecture, serve more than software delivery. Adoption changes the delivery effort of those functions rather than their whole remit, and it is not a one-for-one conversion of the workforce.

### 4.2 A minimum organisation

The minimum organisation under the model is defined by named accountabilities held by people and by agent profiles governed under section 5 of the model ([MODEL.md](MODEL.md)), not by headcount. One person may hold more than one accountability.

```
Technology leader
├── Delivery lead
│   ├── Intent Architect
│   ├── Verification Engineer (verifier)
│   ├── System Steward
│   ├── Design Steward (where the product has a user-facing surface)
│   ├── Release manager (release authority)
│   └── Platform owner
├── Product lead
│   ├── Product owner
│   └── Business analyst and test engineer (validators)
├── Security lead
├── Data owner
└── Agent profiles, one per step and class of change served
    ├── Drafting the intent, step 1 Intent, L2
    ├── Screening gate triggers, step 2 Gate, L1
    ├── Building in a workspace, step 4 Build, L2
    ├── Documentation within the change, step 4 Build, L2
    ├── Running the pipeline and assembling evidence, step 5 Verify, L3
    ├── Reviewing code, security and design, step 6 Review, L1
    ├── Promotion below production, step 8 Promote, L3
    ├── Readiness and the manifest, step 9 Authorise, L3, the decision a person's
    └── Telemetry and post-incident drafting, step 11 Confirm and learn, L2, acceptance at L0
```

The levels are the starting levels in 3.1 of the model and apply only once the evidence under 5.4 of the model supports them, with agents advising at L1 until then. The chart is a minimum operating shape rather than a staffing prescription. The delivery lead and the product lead are management posts shown for completeness, and the model leaves the structure above the accountabilities to the organisation.

### 4.3 The platform team

Section 6 of the model defines the engineering platform and the rules for operating it, and platform changes follow the flow like any other change. One platform owner with a named backup may be enough for the minimum organisation at the first horizon, because provisioning, refresh and masking run as pipeline jobs from versioned definitions. Platform work is likely to be the largest new demand as autonomy rises (3.3 of this guide), and a larger estate is likely to need a dedicated platform team. The number of runs in flight follows the supply of validated intents, review capacity and cost appetite, and review capacity is usually the binding constraint.

## 5. Onboarding and career paths

### 5.1 The entry route for new engineers

The organisation designs the entry route around paired review and rotation, as the engineer direction in 3.3 of this guide requires. Each rotation follows section 7 of the model ([MODEL.md](MODEL.md)), where junior engineers assist with evidence gathering while senior judgement takes the harder calls.

| Rotation | What the new engineer does |
|---|---|
| Verification | Reviews pull requests alongside a Verification Engineer, who keeps the approval, and learns the codebase through structured review |
| Testing | Checks agent-drafted test suites and writes further edge-case tests |
| Documentation | Checks agent-drafted documentation for accuracy and clarity |
| Platform | Works on environment definitions through pull requests and tracks agent use and cost |

The strongest move on to training as Intent Architects or Verification Engineers, at a pace set by the evidence of their work rather than a fixed period.

### 5.2 Progression

| Post | Next step | For those drawn to |
|---|---|---|
| Verification Engineer | Intent Architect | Specification and design |
| Verification Engineer | System Steward | Coherence and governance |
| Business analyst | Intent Architect | Shaping intents in a domain they know well |
| Test engineer | Evaluation design or the practice owner for test | Evidence and the measurement of agent output |
| Intent Architect | System Steward, product owner or delivery leadership | Structural, commercial or operational work |
| System Steward | Technology leadership, head of engineering or enterprise architecture | Direction across the estate |

The System Steward is the most senior technical post in the delivery team.

### 5.3 Cross-training and cover

Section 7 of the model requires a named, cross-trained backup for each accountability who can keep segregation for the work in question. In practice each person covers an adjacent post: Intent Architects verify changes they did not shape, Verification Engineers draft simpler intents and product owners answer clarification questions quickly. Cover never relaxes segregation, and in a smaller team it keeps one absence from stopping delivery.

## 6. Delivery, release and working patterns

### 6.1 Release rhythm and the autonomy progression

Each product's release rhythm is set under 3.3 of the model ([MODEL.md](MODEL.md)) and can be continuous, daily, weekly or batched, as a deliberate decision recorded against the product rather than a consequence of agent speed. Autonomy rises in three stages, and in each a person authorises every production change at step 9, Authorise, while deployment at step 10, Deploy, stays at L0.

| Stage | Autonomy | Release practice |
|---|---|---|
| The pilot, phase 2 | Agents advise at L1, drafting and building move to L2 on the first-delegation evidence under 5.4 of the model and every other step runs at L0 | The release authority authorises each release and the pipeline promotes the attested release candidate with a person watching each promotion |
| Scale-up, phases 3 and 4 | Steps reach the starting levels in 3.1 of the model for the classes the evaluations support: steps 1 and 4 at L2 and steps 5, 8 and 9 at L3. This is the first horizon in 3.2 of this guide | Deployment runs on the shared pipeline once authorised, and a change authorised in the morning can be live the same afternoon where the product's rhythm allows |
| The later horizons | L3 widens across classes and L4 is assigned only where 5.4 of the model is satisfied, never to deployment | Authorisation at step 9 stays with a person for every production change |

The pace of each stage depends on the evidence the organisation produces rather than a calendar, and the constraint is rarely agent speed: it is confidence in the controls and the maturity of the pipeline.

### 6.2 Environments

**Ownership.** The platform owner owns environment provisioning, lifecycle and decommissioning from versioned definitions held in the platform repository (6.1 and 6.3 of the model). Manual configuration creates drift, delay and inconsistency that compound as the volume of change rises, and the products that define environments as code are in [the tooling notes](Tooling/README.md). Environment activity in mature operation is substantial, so a larger organisation is likely to need a platform team, particularly where the platform owner also carries agent configuration, tooling governance and cost control.

**Integration and pre-production environments.** These follow the isolation, reservation and reset rules in each context's versioned environment definition (6.3 of the model), so that the tested composition and data state of each release are identifiable. Stale environment state is a common source of false failures and false confidence, and the reset rule for a context says when a rebuild from its definition is right and when a reservation and a data reset suffice.

**Test data.** Production-derived data reaches a non-production environment only under the test data and masking standard (6.2 of the model). The masked, anonymised or synthetic data used reflects production volumes and edge cases without exposing personal or commercially sensitive information. An agent at L2 can draft synthetic data sets from schema definitions, business rules and the acceptance examples, and they reach an environment through a pull request like any other definition. The platform team regenerates test data as fields, validation rules and business logic change, and the data owner reviews the data position at the periodic review.

**Deployment strategies and feature flags.** The model is compatible with any deployment strategy, such as blue-green, canary or progressive rollout, and keeps the strategy distinct from the deployment approach (section 4 of the model). Section 4 of the model also sets how a feature flag decouples deployment from release, with activation requested by the product owner and authorised at step 9 as a production change. The periodic review checks the active flags and those due for retirement, and each flag that remains after release stays in the coherence register until it is retired.

### 6.3 User acceptance testing

User acceptance testing remains a business validation activity. Where it runs, it runs in the pre-production class after the review at step 6, Review, and before authorisation at step 9, and a high-tier change requires business acceptance there (6.2 of the model). Where several intents serve one outcome, testing at the level of the outcome can be more meaningful, and the product owner coordinates its timing with the business.

### 6.4 Working patterns

People keep the rhythm under 3.3 of the model, and the daily sweep and the periodic review are its fixed points while everything else can flex. Verification Engineers may review throughout the day with a short daily exchange on escalations, and Intent Architects may work in concentrated sessions.

**Follow-the-sun.** A global organisation can run the flow across time zones, each zone holding its own daily sweep at the start of its working day. Intent Architects form intents in their working hours, runs continue and the reviewers in the next zone clear the overnight pull requests before forming intents of their own. Each zone hands on validated intents and a cleared review queue. The pattern needs at least two zones with review capability and works best with two or three. No zone needs every accountability, but each has at least one person who can act as verifier and one who can act as release authority.

## 7. Business readiness for accelerated change

Adoption is likely to raise the volume of change ready for release, and the rest of the business has to absorb it. Training, operations, customer service, communications and acceptance teams outside delivery all have to keep pace with new features and fixes. Where they do not, delivery either throttles itself back and loses much of the benefit, or change reaches production faster than the business can support it, causing confusion, complaints and risk.

The periodic review under 3.3 of the model ([MODEL.md](MODEL.md)) keeps a standing item on business readiness. It asks whether the rate of change is being absorbed, whether any function is falling behind and whether a product's release rhythm needs adjusting. The product owner coordinates release timing and communication with business stakeholders as well as the content of intents, because absorbing change is as much a matter of organisational change management as of delivery.

## 8. Service management and operations

Section 9 of the model ([MODEL.md](MODEL.md)) sets out how the flow works with change, incident, problem, service level and configuration management, and this section adds the practice around it.

### 8.1 Data residency and model governance

Control 6 in 5.3 of the model, the permitted model list and the trust boundaries in 5.5 of the model govern where inference takes place and who decides it. The adopter adds one practice: the periodic review notes every change to the permitted model list or to hosting, so that the assurance view under section 9 of the model reports it.

### 8.2 Incident response

An incident caused by agent-produced code follows the emergency route in 3.2 and section 9 of the model rather than a separate process.

**Diagnosis and the fix.** The decision trail under 8.1 of the model links the failing behaviour to the release candidate, the pull request, the intent, the model and the run that produced it. While the engineer traces the cause, an agent may advise on telemetry at L1 and draft the break-fix at L2 in a workspace, acting on nothing in production (3.2 of the model). The fix goes to the emergency change authority with the minimum evidence it requires, and the time to restore measure shows how quickly fixes move.

**Controls under pressure.** The six never-waived controls in 3.2 of the model hold on the emergency route before anything deploys, and so do the controls on agents in 5.3 of the model. No one approves or authorises their own fix. Review happens at once rather than at the next daily sweep, and the record is completed by the end of the next working day. The team keeps an out-of-hours rota naming a reviewer, a holder of the emergency change authority and a release authority for each period.

**Post-incident review.** An agent drafts the post-incident review at L2 at step 11, Confirm and learn, and the review asks where forming, validation, verification or review failed as well as what went wrong technically.

| Finding | What it points to |
|---|---|
| The intent was right and faithfully built, but the outcome was wrong | Validation |
| The agent departed from the intent | Verification and review |
| The intent was incomplete | The acceptance examples and edge cases for that class of change, strengthened by the validators |

Remediation enters the flow as a request under 2.2 of the model, and a structural finding enters the coherence register.

### 8.3 Ethical governance of artificial intelligence

The model governs how agents deliver software, and an organisation also needs a wider framework for its responsible use of artificial intelligence.

**Transparency.** The decision trail identifies every agent-produced artefact (8.1 of the model), and an organisation extends the principle so that customers, clients and regulators know when output they receive was produced by an agent.

**Bias and fairness.** Agents inherit bias from their training data and from the intents they receive, so Intent Architects consider whether an intent could disadvantage a group of users, particularly in pricing, eligibility, credit, fraud detection or clinical triage. The periodic review includes a bias review of high-tier intents, and the customer outcome gate at step 2, Gate, examines an individual intent before it is built.

**Accountability and environmental impact.** Every decision in section 7 of the model has a named holder and the agent holds none, so a governance failure is answered by the person who held the decision. Agent runs also consume compute, and an adopter can report compute consumed per intent beside the measures in 8.2 of the model as industry carbon reporting standards mature.

## 9. Workforce transition

Most adoptions are likely to involve workforce change, and the pilot and the signals in 3.5 of this guide establish how much. Section 11 of the model ([MODEL.md](MODEL.md)) makes no headcount claim and neither does this guide. Organisations that plan openly, invest in retraining and treat people with respect tend to manage the transition better than those that deny the implications until abrupt cuts follow.

**Phasing.** Workforce change follows the evidence rather than the adoption calendar. No post changes in phase 1, the first people move into Intent Architect and Verification Engineer posts in phase 2 and from phase 3 posts change as each horizon arrives in a product area.

**Reskilling.** Not everyone can or wants to move into the posts that grow. The Intent Architect post needs deep domain knowledge and system-level thinking and the Verification Engineer post needs the judgement to assess agent output critically, so some people will thrive in them and others will not. The organisation runs structured reskilling towards the directions in 3.3 of this guide, gives each person a fair chance and is direct when it is not working.

**Redeployment and redundancy.** In a large organisation people whose work moves to agents may move to service management, data, programme governance or commercial work, where their analytical and stakeholder skills transfer well. Redeployment is a real option rather than a delay tactic. Some posts may be made redundant where the evidence shows the need. The organisation handles this with generous terms, outplacement support and clear timelines, because handling it badly damages trust and makes the senior people the model depends on harder to recruit.

**Communication.** Launching a pilot without telling the team what it means for their posts is likely to do the most harm, because people can see agents doing implementation work and fill any silence with anxiety and rumour. Leadership tells the team at the outset that the operating model is changing, what it means for each post, the timeline and the support available.

## Contributing findings

If you adopt the model, the evidence you record is what tests the forecast in section 3 of this guide. [CONTRIBUTING.md](CONTRIBUTING.md) explains how to share findings from a pilot, and measured results from real teams are particularly useful to the next version.

---

## Appendix A. Illustrative organisation comparison

This appendix compares a fictional software development function before and after adoption, under the caveats in 4.1 of this guide. Every figure in it is the model's own illustration and not a prediction for any organisation, and the picture after adoption is not a target staffing model.

### A.1 The function before adoption

The illustration starts from a software development function of 100 people in a mid-sized organisation. Technology operations, the service desk and infrastructure support are excluded because the model does not address them.

```
Technology leader
├── Head of development
│   ├── 6 Scrum Masters
│   ├── 31 developers (9 senior, 15 mid-level, 7 junior)
│   ├── 8 quality assurance engineers
│   └── 3 automation testers
├── Head of business analysis and projects
│   ├── 6 project managers
│   ├── 10 business analysts
│   └── 3 product owners
├── Head of platform and DevOps
│   ├── 5 DevOps engineers
│   ├── 4 infrastructure engineers
│   └── 2 cloud and platform engineers
├── Head of architecture
│   ├── 2 solution architects
│   ├── 1 technical architect
│   └── 1 enterprise architect
├── Security lead
│   ├── 1 security architect
│   ├── 1 security engineer
│   └── 1 security and compliance lead
├── PMO and governance
│   └── 2 PMO analysts
└── Data
    ├── 4 data engineers
    └── 2 data analysts
```

Of the 100 people, 86 in development, business analysis and projects, platform and architecture are dedicated to software delivery. The other 14, in security, the PMO and data, have responsibilities beyond it, so the comparison is most meaningful for the 86.

The function runs two-week sprints across four development teams, with daily stand-ups, planning, reviews and retrospectives, and project managers coordinate across the teams. Most developer time goes on writing code, with significant time lost to ceremonies, context switching and waiting on dependencies.

### A.2 The organisation after adoption

The matching illustration after adoption keeps the same focus on software delivery posts.

```
Technology leader
├── Delivery lead
│   ├── 6 Intent Architects
│   ├── 4 Verification Engineers
│   ├── 2 System Stewards
│   └── 1 platform owner
├── Product lead
│   ├── 2 product owners
│   └── 2 programme managers
├── Security lead
│   ├── 1 security architect
│   ├── 1 security engineer
│   └── 1 security and compliance lead
├── PMO and governance
│   └── 2 PMO analysts
└── Data
    ├── 4 data engineers
    └── 2 data analysts
```

The illustration totals 33 people working with agents under the agent profiles in 4.2 of this guide. The 19 under the delivery lead and the product lead form the delivery organisation, and the other 14 are carried across at their earlier numbers because their work extends beyond software development. Keeping those 14 unchanged avoids overstating the change, although agent assistance may affect their work over time. The illustrative comparison is therefore 86 people dedicated to software delivery before adoption and 19 after it, as an illustration rather than a claim.

Before the shape is used as an operating design, the adopter maps the validators, the release authority and a backup for each accountability to its posts. The illustration shows no business analyst, test engineer or release manager post, so those accountabilities go to posts it does show. No one who shaped an intent declares it validated alone, as 2.3 of the model ([MODEL.md](MODEL.md)) requires.

The chart shows no Design Steward. Design coherence sits with the system steward unless a design steward is appointed (section 7 of the model), so a product with a user-facing surface either appoints one or has a System Steward with both skill sets hold both. The illustration also holds the platform at one platform owner, while the forecast in 3.3 of this guide expects platform work to be the largest new demand. An organisation sizes its own platform team from the forecast and its own measures.

### A.3 Indicative role transitions

This table maps the posts in A.1 to the posts in 3.3 of this guide that their holders are likely to move towards, and it shares the status of that forecast.

| Post before adoption | Likely destination |
|---|---|
| Developer, junior to senior | Senior developers towards the Intent Architect, Verification Engineer, System Steward and platform posts, others towards the engineer direction in 3.3 or adjacent engineering paths |
| Business analyst | Business analyst as a validator, with senior analysts towards Intent Architect |
| Product owner | Retained with a wider remit, because faster execution is less forgiving of ambiguity |
| Project manager | Fewer inside software delivery, with some moving to programme or change roles that coordinate rollout, readiness, training and dependencies outside the codebase |
| Programme manager | Usually retained where work spans teams or wider business change |
| Scrum Master or delivery manager | Ceremony facilitation falls away, and some move to release manager, holding the release authority |
| Solution or technical architect | Intent Architect or System Steward, expressing design and boundaries as intents and structural constraints |
| Enterprise architect | Outside the delivery team, or aligned with a senior System Steward |
| Quality assurance or test engineer | Test engineer as a validator, with exploratory testing and acceptance judgement |
| Security architect | Retained, or mapped into the security lead and System Steward accountabilities |
| Security engineer | Control design, triage, threat modelling and exceptions, while scans run as pipeline checks at step 5 and an agent comments at L1 at step 6 |
| Security and compliance lead | Retained, because regulated environments need named accountability for assurance |
| Technical author | Retained where audience, regulation or training content needs a specialist, with an agent drafting documentation at L2 |
| DevOps, infrastructure or platform engineer | Platform engineer, with one holding the platform owner accountability |
| PMO or governance analyst | Fewer inside software delivery, with some remaining at portfolio level |
| Data engineer or analyst | Often retained, with agent support |
| Delivery lead | Retained or created as a people management post outside the flow that manages capacity and provides the escalation route, holding neither the release authority nor the verifier accountability in this illustration |

### A.4 Illustrative scaling model

The figures below are the model's own illustration of direction, not observed norms and not a prediction for any organisation. The actual mix depends on system complexity, the controls, shared services and how much of the wider change effort sits inside the technology function.

| Organisation shape | Delivery team before adoption | Team after adoption | Agent runs in flight | Caveat |
|---|---|---|---|---|
| Single product, moderate complexity | 15 to 20 people | 5 to 8 people | 4 to 8 | Wide variance with legacy burden and regulatory overhead |
| Mid-sized, 3 to 5 products | 60 to 100 people | 20 to 35 people | 10 to 20 per product | Shared services and governance posts may sit outside the counted team |
| Large enterprise, 10 or more products | 200 to 400 people | 50 to 100 people | Scaled to review capacity | Enterprise architecture, security and portfolio functions may remain outside delivery teams |
