# Adopting the Agentic Delivery Model

**Companion to [The Agentic Delivery Model v1.0](MODEL.md)**

This document covers how to adopt, implement and operate the ADM. The core model is defined in [MODEL.md](MODEL.md). This companion provides practical guidance on adoption strategy, team structure, working patterns, transition planning and workforce change.

**Tooling maturity note.** The tooling landscape for agentic software delivery is evolving rapidly. Tooling references in this document reflect the current state at the time of writing and should be treated as alpha guidance that will be updated as tooling matures.

---

## 1. Adopting the ADM

### 1.1 Phases and Progression

No organisation should attempt a wholesale shift to the ADM. Adoption involves roles, governance, tooling, delivery confidence and workforce change. The transition must be phased.

Timelines vary significantly with business appetite for change, risk tolerance and existing workload. The phases below describe what happens, not a calendar schedule. The first real phase, tooling setup and the confidence-building pilot, will take longer than most organisations expect. This is a factor of many things: team capacity, existing infrastructure, standards definition and the depth of learning needed. Plan generously.

#### Phase 1: Foundations and Enablement

Establish the basic operating conditions before agents begin meaningful delivery.

**Leadership alignment.** Name the accountable leader, define the transition scope and set success criteria. Be clear whether you are testing tooling, the full operating model or preparing for workforce redesign. Confusion here creates false expectations later.

**Role design and training.** Identify the people who will act as Product Owner, Intent Architect, Verification Engineer, System Steward and Platform Engineer. They need explicit training during this phase, though actual role transitions begin in Phase 2. Intent Architects need practice shaping precise Intent Specifications. Verification Engineers need practice in evidence-based review rather than code-style review. System Stewards need practice running the Coherence Register. Role transition does not happen by title change alone.

**Tooling setup.** Configure the minimum viable environment: backlog connector, repository permissions, branch protections, sandboxing, approved commands, test execution, logging and provenance capture. Select initial models and define fallback rules. Confirm where inference occurs, what data leaves the trust boundary and how agent identities appear in commits and logs.

**Risk assessment and regulatory compliance.** Introducing AI agents into the delivery process changes the risk profile. Before agents begin generating code, conduct a risk assessment covering data handling (what data agents can access, where inference occurs, what leaves the trust boundary), intellectual property (code provenance, licence compliance), security posture (agent sandboxing, credential management, dependency governance) and operational resilience (failure modes, rollback capability). In the UK, this will include a Data Protection Impact Assessment (DPIA) under UK GDPR. Other jurisdictions and industries carry equivalent obligations: the EU AI Act, HIPAA in US healthcare, SOC 2 for SaaS providers, PCI DSS for payment processing and sector-specific frameworks such as the FCA's operational resilience requirements or the NHS Data Security and Protection Toolkit. The specific assessments depend on where you operate and what you are regulated for. The point is universal: do not skip this step. Identify the applicable regulatory and compliance requirements early, involve your legal, security and compliance teams in Phase 1 and build the controls into the operating model from the start. Retrofitting compliance after agents are already delivering is significantly harder and more expensive.

**Baseline standards.** Prepare the first version of the Intent Specification template, Definition of Ready, testing expectations and initial agent configuration files. They need not be perfect. They must be good enough to support repeatable delivery and review.

**Pilot selection.** Choose a bounded area where the team can learn without unacceptable operational or regulatory risk. Pick a well-understood product area with tolerable blast radius, not the most politically visible or chaotic part of the estate. For brownfield environments, baseline test coverage in the pilot area is a prerequisite.

#### Phase 2: Confidence-Building Pilot

The pilot proves the ADM works end to end in your environment. The aim is not volume. It is confidence in the operating model. Use it to answer practical questions.

- Can Product Owners, Intent Architects and Verification Engineers work effectively in the new cadence?
- Are the agent configurations and controls strong enough to support governed delivery?
- Does the test strategy catch regressions?
- Does the provenance trail satisfy audit, security and regulated stakeholders?
- What is the real first-pass acceptance rate?

The right pilot is a contained internal product, a non-critical product slice or a test project that mirrors real conditions without unnecessary business risk. People stay close to the loop. Measure first-pass acceptance rate, clarification rate, cycle time and review queue health from the start.

**Greenfield and simulation pilots.** Many teams will find it easier to pilot against greenfield code or even a fictional project rather than an existing codebase. This removes the complication of legacy constraints and lets people focus on learning the roles, cadence and governance discipline. Project Kestrel ([KESTREL.md](KESTREL.md)) provides a realistic worked example that can serve as the basis for this kind of exercise. Teams assign the human roles, configure a small agent fleet against a fresh repository and run the model for a week. The learning transfers directly to real delivery. For teams new to agentic development, this is often the fastest route to confidence.

#### Phase 3: Controlled Scale-Up

Once confidence is established, begin scaling. Introduce the three-tier cadence in full. Agents pick up work autonomously from the backlog. Daily governance sessions replace standups. Weekly strategic sessions begin. In regulated environments, feed the formal assurance overlay described in [Section 13](MODEL.md#13-governance-for-regulated-environments).

Role transitions become real. People working as Intent Architects, Verification Engineers, Product Owners and System Stewards operate routinely with explicit support and coaching. Tighten tooling based on pilot findings: improve standards, correct permissions, strengthen tests, clarify escalation paths.

The focus is controlled repeatability, not maximum speed.

#### Phase 4: Scaled Operation

Expand to additional product areas. Introduce cross-product System Steward capacity where needed. Optimise the ratio of people to agent capacity. Begin reporting Human Leverage Ratio to leadership. Refine model selection, standards, role training and governance based on accumulated data.

### 1.2 Adopting the ADM in Brownfield Environments

The phased approach assumes good test coverage, clear boundaries and documented APIs. Most enterprise codebases are none of these. The ADM applies to brownfield environments, but starting conditions differ.

**Test coverage is the prerequisite.** Agents generate code and run tests to validate it. If the test suite is thin or unreliable, verification burden shifts entirely to people and breaks the model's economics. Before Phase 1, invest in baseline test coverage for the pilot area. Not 80% everywhere, but enough automated tests around the boundaries agents will touch to catch regressions. A Testing Agent can generate test suites for existing code before implementation agents begin feature work.

**Start at the edges.** In a tightly coupled monolith, introduce agent code at the boundaries: new API endpoints, services that integrate with (not modify) the core, new UI components backed by existing APIs. This limits blast radius while the team builds confidence. As test coverage improves and boundaries become clearer, agents can work deeper in the codebase.

**The System Steward is critical early.** In a brownfield environment, the existing architecture is the constraint. The Steward maps the current architecture, defines boundaries agents must respect and documents the rules experienced developers carry in their heads. This knowledge transfer is the hardest part of brownfield adoption. Without it, agents violate unwritten rules and generate architecturally wrong code.

**Technical debt becomes visible fast.** Agents surface debt that was previously tolerable because people worked around it: inconsistent patterns, duplicated logic, unclear boundaries, missing error handling. The Coherence Register fills quickly. This is a feature, not a problem. The System Steward should expect to commission significant refactoring work in the early months.

**Expect lower first-pass acceptance rates.** Well-structured codebases achieve 70-80% first-pass rates within weeks. Brownfield codebases with inconsistent patterns may start at 40-50%. This improves as the System Steward codifies conventions into agent-readable standards. The ramp is slower. Plan for it.

### 1.3 Adapting the ADM by Team Size

The ADM flexes with team size and change volume. Smaller organisations combine roles, run smaller fleets and operate with lighter governance such as asynchronous review or less frequent Strategic Tier. Larger organisations separate roles more clearly, add specialist capacity and formalise assurance more heavily. The principle is to preserve accountabilities and control points while scaling depth to fit context.

---

## 2. How the ADM Changes the Way People Work

### 2.1 What People Do in the Delivery Cycle

**Product Owners** make intent precise enough for execution and resolve business ambiguities quickly when they arise. Their day is priorities, stakeholder alignment and specification sign-off.

**Intent Architects** shape executable intent. They review draft specifications from requirements agents, tighten acceptance criteria, define constraints, decompose work that is too broad for reliable execution and answer clarification requests on feasibility and scope.

**Verification Engineers** review agent output. Each PR at the governance session has already passed automated checks: code review, tests, security scanning. The Verification Engineer assesses whether the changeset matches the Intent Specification and is structurally consistent. They approve, request rework or escalate. Validation of whether the specification itself serves the business intent is the Product Owner's responsibility. Outside governance sessions, they monitor the review queue, conduct exploratory testing and validate that automated test coverage is meaningful. A Verification Engineer may process eight to twelve PRs per day with proper scrutiny. These numbers are indicative, not targets. They vary with codebase complexity and the maturity of automated pre-checks.

**System Stewards** maintain coherence. They review cross-cutting changes, update standards and constraints, analyse where the codebase is drifting and commission refactoring work.

### 2.2 Estimation and Forecasting

The ADM has no sprints or fixed periods. Agents execute continuously against a prioritised backlog. The question is not "how big is this?" but "is this ready for agent execution?"

The Definition of Ready in [Appendix B.4](MODEL.md#b4-definition-of-ready-for-agents) replaces estimation. A specification that meets the readiness standard can be assigned to an agent. Complexity assessment is for routing and decomposition, not capacity planning. If a specification is too large for reliable single-agent execution, the Intent Architect decomposes it.

However, organisations still need to communicate expected delivery windows to stakeholders for training, user acceptance testing, release coordination and business planning. Forecasting based on observed throughput provides this basis. Teams measure specification throughput and delivery throughput by S, M and L complexity band. Over time this builds an empirical model: small specifications take X hours on average, medium Y, large Z.

This is more accurate than story points and surfaces constraints faster. Declining specification throughput signals a design bottleneck. Dropping delivery throughput signals build or verification constraints. The metrics in [Section 14.1](MODEL.md#141-core-metrics) formalise this.

### 2.3 Where Bottlenecks Form

The ADM shifts bottlenecks from implementation to specification and verification.

**Specification bottleneck.** Agents execute faster than people can specify. A well-configured implementation agent can complete a mid-complexity feature in hours. An Intent Architect shaping that same feature may take a full day. If the specification pipeline dries up, agents sit idle. This is the most likely bottleneck at maturity.

**Verification bottleneck.** Agent output accumulates faster than people can review it. Twelve implementation agents each deliver a PR overnight. The governance session the next morning must process twelve reviews. If review capacity is insufficient, the queue grows and delivery stalls despite agents working at full speed.

**Clarification bottleneck.** Agents that encounter ambiguity in a specification must escalate to a person. If Intent Architects are fully occupied shaping new specifications, clarification requests queue up and agents block. This creates a feedback loop: poor specification quality generates more clarification requests, which consume the time that should be spent improving specification quality.

**Standards bottleneck.** When the System Steward falls behind on guidance, agents make decisions without sufficient constraint. This produces working code that passes tests but introduces structural inconsistency. The cost shows up later as rework and coherence debt.

**Design bottleneck.** For products with a user-facing surface, the Design Steward can become a constraint in the same way as the System Steward. If design constraints are not codified, agents produce inconsistent interfaces. If the Design Steward cannot review UI output fast enough, either the review queue grows or design quality degrades. This bottleneck only applies where the product has a UI, but where it does, it can be as significant as the standards bottleneck.

### 2.4 How the Model Addresses This

The three-tier cadence manages these bottlenecks. It does not eliminate them.

The Intent Brief ([Appendix B.1](MODEL.md#b1-the-intent-brief)) gives the Product Owner a fast way to feed the pipeline. They write a short Brief, assign it to a requirements agent and the agent runs discovery, producing draft specifications. The Intent Architect refines and approves rather than working from a blank page. This multiplies specification throughput without adding people. For complex or ambiguous domains, paired discovery ([Section 4.2](MODEL.md#42-intent-briefs-and-requirements-discovery)) puts a person alongside the agent during the session itself.

Automated checks filter agent output before it reaches human review. The Review Agent, Testing Agent and Security Agent run before the governance session. The reviewer focuses on business intent alignment and structural judgement, not syntax and style.

Specification quality breaks the clarification feedback loop. The Definition of Ready in [Appendix B.4](MODEL.md#b4-definition-of-ready-for-agents) ensures specifications that meet the standard generate fewer clarification requests, which frees Intent Architect time for shaping more. Investing in specification quality is the single most valuable activity in the model.

The Coherence Register gives the System Steward a prioritised backlog of structural concerns. The Steward issues guidance proactively. Agents consume it as configuration.

### 2.5 Leading Indicators

Monitor these metrics to detect bottlenecks early.

**Ready backlog depth.** Intent Specifications in Ready state, waiting for agent execution. If this drops below one day's agent capacity, the specification pipeline is the constraint. If it grows beyond a week, agents may be underprovisioned or blocked.

**Review queue depth.** Completed PRs awaiting review. If this exceeds what the governance session can process in a day, verification capacity needs attention.

**Clarification request rate.** Agent escalations per specification. A rising rate signals declining specification quality. A falling rate signals the Definition of Ready is working.

**First-pass acceptance rate.** The percentage of agent PRs approved without rework. This is the single best measure of system health. Low rates mean either specifications are imprecise or agent configuration needs improvement.

The ADM requires fewer people, each doing more valuable work. This is a structural change in the ratio of people to delivered software, not a marginal efficiency gain. Plan openly. Invest in retraining. Be transparent about timelines. Treat affected staff with respect. The worst outcome is pretending nothing will change and then making abrupt cuts.

---

## 3. Roles That Change, Split or Remain

The ADM restructures the delivery function. Traditional roles disappear from day-to-day delivery, split across human and agent responsibilities or remain with tighter governance focus.

The indicative role transition table is in [Appendix A.3](#a3-indicative-role-transition-table).

---

## 4. Team Structure

### 4.1 Caveats on Comparison

Before-and-after comparisons here are thought models, not benchmarks. Organisations differ in product complexity, regulation, legacy burden, sourcing strategy and platform mix. Some roles exist for more than software delivery: project managers coordinate broader business change, PMO staff govern portfolios beyond software, security and architecture span the wider estate.

The ADM changes where people put effort and which roles matter most. It is not a one-for-one workforce conversion exercise.

### 4.2 A Minimum ADM Organisation

The smallest ADM organisation is defined by named accountabilities held by people and a governed agent fleet, not by headcount. One person may cover more than one role. What matters is explicit responsibility.

[Appendix A](#appendix-a-illustrative-organisation-comparison) shows a fuller illustrative comparison between a traditional software delivery function and an ADM organisation.

```
CTO / IT Director
├── Delivery Lead
│   ├── Intent Architect
│   ├── Verification Engineer
│   ├── System Steward
│   ├── Design Steward (where the product has a user-facing surface)
│   └── Platform Engineer
├── Product Lead
│   └── Product Owner
├── Security Lead
├── Data Lead
└── Agent Fleet
    ├── Requirements Agent
    ├── Implementation Agents
    ├── Testing Agent
    ├── Review Agent
    ├── Documentation Agent
    ├── Platform Agent
    ├── Refactoring Agent
    ├── Security Agent
    ├── Design Agent (where the product has a user-facing surface)
    └── Scheduling Agent
```

This is the minimum operating shape, not a staffing prescription. Smaller organisations combine roles within fewer people. Larger organisations add depth under each lead. The illustrative appendix shows one scaling model. Do not lift it as a target organisation chart.

The core ADM roles are Intent Architect, Verification Engineer, System Steward, Design Steward, Product Owner and Platform Engineer. These are the roles the model defines. The Delivery Lead, Product Lead, Security Lead and Data Lead are organisational management roles shown here for completeness. The ADM does not prescribe how the management structure above the core roles is organised. Different organisations will structure this differently.

### 4.3 Agentic Platform Engineering

Platform engineering is largely agent-driven under human oversight. People manage guardrails, approvals, exceptions and accountability. The function is responsible for:

- **Agent infrastructure.** Provisioning, scaling and monitoring the agent fleet. Managing compute costs. Ensuring agent sandboxes are properly isolated.
- **Pipeline governance.** Maintaining CI/CD pipelines that agents use. Ensuring automated checks such as linting, security scanning and test execution run reliably.
- **Standards enforcement.** Embedding company and project-level standards into agent configuration files. Ensuring standards updates propagate to all active agents.
- **Observability.** Monitoring agent fleet performance, utilisation, error rates and cost. Providing data for the weekly Strategic Tier review.
- **Platform Agent oversight.** The Platform Agent manages routine tasks under tighter human oversight than other agents. All Platform Agent changes require explicit human approval before application.

One Platform Engineer may be enough at this size. Most execution is agent-run. Larger estates may need a dedicated Agent Operations team. A nominated System Steward or senior Intent Architect should be cross-trained as backup.

Agent fleet size scales with backlog depth, human review capacity and cost appetite. The constraint is usually human review capacity, not agent availability. There is no value running twelve implementation agents if the governance session reviews only eight PRs per day.

### 4.4 How This Scales

An illustrative scaling model is provided in [Appendix A.4](#a4-illustrative-scaling-model).

---

## 5. Onboarding and Career Paths

The ADM creates new entry points and progression routes. Plan these deliberately. They will not emerge on their own.

**Entry points.** Graduates and junior engineers enter through verification support, testing support, documentation support or platform support. Verification support means reviewing agent-generated PRs alongside a Verification Engineer, checking test coverage, flagging inconsistencies and learning the codebase through structured review. Testing support means validating agent test suites, writing additional edge-case tests and monitoring test health dashboards. Documentation support means reviewing and improving agent-generated documentation for accuracy and clarity. Platform support means monitoring agent infrastructure, tracking utilisation and assisting with sandbox provisioning. These roles learn by seeing what good delivery looks like from the verification side. The strongest move towards Intent Architect or Verification Engineer training within 12 to 18 months.

**Verification Engineer progression.** Verification Engineers develop deep judgement about code quality, structural consistency and business intent alignment. The progression path leads to either Intent Architect, for those drawn to specification and design, or System Steward, for those drawn to coherence and governance.

**Intent Architect progression.** Intent Architects are senior practitioners. The progression path leads to System Steward, Product Owner or delivery leadership, depending on whether their strength is structural, commercial or operational.

**System Steward progression.** System Stewards are the most senior technical role in the ADM delivery team. Progression leads to CTO, Head of Engineering or enterprise architecture functions.

**Cross-training.** Every person in an ADM team should cover at least one adjacent role. Intent Architects should verify. Verification Engineers should draft basic specifications. Product Owners should answer agent clarifications quickly and precisely. A person who authored or refined an Intent Specification must not be the person who verifies the resulting PR. Cross-training enables coverage, but segregation of duties must be maintained. In regulated environments this is a firm requirement. In smaller teams, one person's absence creates a gap. Cross-training is not optional.

---

## 6. Delivery, Release and Working Patterns

### 6.1 Delivery Cadence

The ADM enables daily releases. Governance approvals flow through CI/CD to production the same day. Most organisations achieve weekly releases on adopting the model and move to daily as pipeline confidence grows. The progression is:

1. **Pilot stage.** Manual deployment after each governance session. Weekly releases at most. The team is learning the cadence and building confidence in agent output.
2. **Structured autonomy.** Semi-automated deployment. Changes approved at the morning governance session deploy the same afternoon. Two to three releases per week.
3. **Scaled operation.** Fully automated CI/CD. Every approved PR deploys automatically. Daily releases become the norm.

The duration of each stage depends on the organisation. A small team with greenfield code and high risk appetite may move through these stages in weeks. A large regulated organisation with legacy systems and formal change governance may take months. The stages describe a progression, not a calendar.

The constraint is never agent speed. It is governance confidence and pipeline maturity.

**Release strategy is a governance decision.** Deployment frequency and release strategy are separate concerns. The ADM supports any cadence: continuous, daily, weekly or batched. The choice depends on risk appetite, regulatory context and pipeline maturity. SaaS products may release continuously. Insurers with regulatory reporting may batch weekly. Healthcare systems may release fortnightly. The Strategic Tier reviews the strategy periodically. What matters is that it is a deliberate decision, not an accident of agent speed.

### 6.2 Environment Strategy

Agents fill deployment pipelines faster than traditional teams. Environments must be designed for this throughput. Environment separation remains essential, but the pattern of use changes and the management overhead increases significantly.

**Ownership.** The Platform Engineer owns environment provisioning, lifecycle and decommissioning. In larger organisations this may require a dedicated platform team rather than a single person. The volume of environment activity in a mature ADM operation (ephemeral sandboxes spinning up and down, integration environments rebuilding, staging promotions, production deployments) is a substantial operational responsibility. Organisations should not underestimate this. If the Platform Engineer is also responsible for agent fleet configuration, tooling governance and cost controls, environment management alone can become a bottleneck.

**Infrastructure as code.** All environments should be defined as infrastructure as code (Terraform, CloudFormation, Pulumi or equivalent). This is not optional at ADM throughput. Manual environment configuration creates drift, delays and inconsistency that compound as delivery volume increases. Environment definitions sit in version control alongside the application code and are subject to the same governance.

**Development environments** are ephemeral. Each agent works in an isolated sandbox ([Appendix C.5](MODEL.md#c5-agent-sandboxes)) with isolated branches, build and test environments. Agents do not step on each other's work.

**Integration and staging environments** should rebuild from infrastructure as code before each test run. This prevents state contamination from previous runs and ensures tests validate the current state of both the application and its infrastructure. At daily delivery cadence, stale environment state is a common source of false failures and false confidence. Automated teardown and rebuild is the default pattern.

**Test data.** Environments need realistic data to produce meaningful test results. Production data must not be used in non-production environments without appropriate controls. Use redacted, anonymised or synthetic model data that reflects production volumes and edge cases without exposing personal or commercially sensitive information. Synthetic test data generation is a natural fit for agents. A dedicated test data agent or the Testing Agent can generate realistic datasets from schema definitions, business rules and edge case catalogues defined in the Intent Specification. Test data should be regenerated as the system changes: new fields, changed validation rules or revised business logic all invalidate existing datasets. Stale test data produces false confidence. Test data management is a Platform Engineer responsibility. The data strategy should be reviewed at the Strategic Tier, particularly in regulated environments where data handling obligations apply.

**Production deployment patterns** depend on risk appetite and regulatory constraints. The ADM is compatible with any pattern: blue-green, canary, progressive rollout or direct deployment with automated rollback. The model does not prescribe a pattern. It requires that the chosen pattern is automated, repeatable and that rollback triggers automatically on failure.

**Feature flags** deserve specific attention. When multiple Intent Specifications contribute to a larger feature, individual specifications may deploy to production behind a flag before the full feature is ready for users. This decouples deployment from release. Code reaches production early, reducing integration risk, but users see the feature only when the Product Owner activates the flag. Feature flag management should be reviewed at the Strategic Tier: which flags are active, which should be retired, which are accumulating debt by remaining in the codebase too long.

### 6.3 User Acceptance Testing

User acceptance testing remains a business validation activity. UAT happens after verification (the Verification Engineer confirming the output matches the specification) and before the Product Owner confirms the feature is ready for users. For specifications grouped under a Strategic Intent, UAT may be more meaningful at the Strategic Intent level. The Product Owner coordinates UAT timing with business stakeholders.

### 6.4 Working Patterns

Agents work continuously regardless of when people are working. People participate in the Governance Tier (daily) and Strategic Tier (weekly), scheduled to suit the team.

For global organisations, the ADM supports follow-the-sun. Each time zone has its own governance session, typically at the start of the local working day. Intent Architects in one zone write specifications during their working hours. Agents execute continuously. The governance team in the next zone reviews completed PRs from the overnight agent work and writes their own specifications. Each zone hands off a refreshed backlog and a queue of reviewed PRs to the next. This requires at least two zones with human review capability and works best with two or three. It does not require duplicated roles in every zone, but each zone must have at least one person who can conduct governance review.

Compressed and asynchronous patterns also work. Verification Engineers may review PRs throughout the day with a brief daily sync covering escalations only. Intent Architects may work concentrated sessions rather than fill eight hours. The governance sessions are the fixed points. Everything else can flex.

The fundamental tension in a mature ADM team is whether people generate specifications fast enough to keep agents productively occupied. Section 2 covers where bottlenecks form and the leading indicators to monitor.

---

## 7. Business Readiness for Accelerated Change

The ADM dramatically increases the volume of deployable change. A well-functioning ADM team can deliver daily releases where a traditional team managed fortnightly or monthly. This is a significant operating advantage, but it creates a corresponding challenge: the rest of the business must absorb that change.

Training teams need to update documentation and materials more frequently. Operations teams need to adapt processes. Customer service teams need briefing on new features and fixes. Communications teams need to manage internal and external messaging. Testing and acceptance teams outside the delivery function need to keep pace.

If the delivery function accelerates but the surrounding business functions do not, one of two things happens: either the delivery function throttles itself back to match business absorption capacity (losing the advantage of the model), or change reaches production faster than the business can support it (creating confusion, complaints and risk).

Organisations adopting the ADM should plan for this explicitly. The weekly Strategic Tier session should include a standing item on business readiness: is the rate of change being absorbed, are any functions falling behind and does the release cadence need to be adjusted? The Product Owner should coordinate with business stakeholders on release timing and communication, not just on specification content.

This is not a delivery problem. It is an organisational change management problem. The ADM creates change. Someone has to receive it. Plan for both sides.

---

## 8. ITIL and Operational Frameworks

The ADM changes how software changes are generated and governed, but operational disciplines remain: change management, incident management, problem management, service level management and configuration management.

Detailed discussion of how the ADM aligns with ITIL is provided in [Appendix D](MODEL.md#appendix-d-itil-and-operational-frameworks).

### 8.1 Data Residency and Model Governance

Organisations must consider where agent inference occurs, particularly when using cloud-hosted models. The ADM does not prescribe specific tooling, but requires that data residency and model selection decisions are documented and reviewed at the Strategic Tier (see also [Appendix C.6](MODEL.md#c6-model-selection-and-data-residency)).

### 8.2 Incident Response in Agentic Delivery

When agent-generated code causes a production incident, the response process follows the standard ADM delivery cycle on an accelerated track. It is not a separate process.

**Diagnosis.** The provenance metadata attached to every agent-generated change (which agent, which specification, which model version) makes root cause identification faster. The team can trace from the failing behaviour back to the specific PR, the specific specification and the specific agent session that produced it.

**Fix generation.** An implementation agent can produce a fix significantly faster than a developer working manually. The process is: diagnose the root cause, write a focused Intent Specification for the fix, assign it to an agent and fast-track it through verification. In a well-functioning ADM team, a fix can move from diagnosis to verified PR in under an hour.

**Governance under pressure.** The temptation during an incident is to bypass the Governance Tier and deploy a fix without verification. This must be resisted. The three-tier cadence exists precisely for situations where speed creates risk. A fast-tracked fix still requires Verification Engineer review before deployment. The difference is that the review is immediate rather than waiting for the next morning governance session. Teams should define an escalation path for out-of-hours incidents that includes a named Verification Engineer on call.

**Post-incident review.** The ADM's post-incident review should assess not just what went wrong technically, but where the specification or verification process failed to catch the issue. If the specification was correct and the agent implemented it faithfully but the outcome was wrong, the specification process needs improvement. If the agent deviated from the specification, the verification process needs tightening. If the specification was incomplete, the Intent Architect needs to invest more in edge case coverage for that type of work.

The relationship to ITIL incident management is discussed in [Appendix D](MODEL.md#appendix-d-itil-and-operational-frameworks).

### 8.3 Ethical AI Governance

The ADM governs how AI agents deliver software. Organisations also need a broader framework for governing AI use responsibly. The two are related but distinct.

**Transparency.** The ADM's provenance requirements ([Section 13.1](MODEL.md#131-provenance-and-audit-trail)) serve transparency: every agent-generated artefact is identified as such. Organisations should extend this principle beyond the delivery function. Stakeholders, clients and regulators should know when they are interacting with agent-generated output.

**Bias and fairness.** Agents inherit biases from their training data and from the specifications they receive. Intent Architects should consider whether specifications could produce outputs that disadvantage particular user groups. This is particularly relevant in insurance (pricing models, claims decisions), financial services (credit scoring, fraud detection) and healthcare (triage, treatment recommendations). The Strategic Tier should include periodic bias review for high-impact specifications.

**Human accountability.** Agents do not bear accountability. People do. The ADM is explicit about this: the Product Owner is accountable for the intent and for validating that it serves the business need, the Verification Engineer for verifying that agent output matches the specification, the System Steward for architectural coherence. Organisations must ensure that accountability is not diffused by the presence of agents in the delivery chain. "The agent did it" is not an acceptable answer to a governance failure.

**Environmental impact.** Agent fleets consume significant compute. Organisations should track and report the carbon footprint of their agent operations alongside the efficiency gains. The Cost per Intent Specification metric ([Section 14.1](MODEL.md#141-core-metrics)) should be complemented by an environmental impact metric as industry standards for AI carbon reporting mature.

---

## 9. Workforce Transition

Every ADM adoption involves headcount change. Pretending otherwise is misleading and counterproductive. Organisations that plan openly and treat people with respect execute better than those that deny the implications until forced into abrupt cuts.

**Phasing.** The transition is phased over a period appropriate to your context and risk tolerance. During Phase 1 of adoption, no roles change. During Phase 2, transitions begin for those becoming Intent Architects and Verification Engineers. During Phase 3, the full ADM structure is in place and roles that are no longer needed are formally transitioned.

**Retraining.** Not everyone can or wants to transition to ADM roles. Intent Architect requires deep domain knowledge and system-level thinking. Verification Engineer requires the judgement to assess agent output critically. Some people will thrive. Others will not. Invest in structured retraining that gives people a fair chance, and be direct when it is not working.

**Redeployment.** In large organisations, people whose roles disappear may be redeployed to other functions: service management, data, programme governance, commercial roles. This is a genuine option, not a delay tactic. The skills developed in technology teams (analytical thinking, problem-solving, stakeholder management) transfer well.

**Redundancy.** Some roles will be made redundant. This is inevitable. Handle it well: generous terms, genuine outplacement, transparent timelines. Organisations that do this protect reputation and maintain morale. Those that do not create a toxic environment that harms recruitment of the senior people the ADM needs.

**Communication.** The worst approach is launching an ADM pilot without telling the team what it means for their roles. People can see that agents do implementation work. If leadership does not address this openly, the team fills the silence with anxiety and rumour. Be clear from the outset. The operating model is changing. Here is what it means for each role. Here is the timeline. Here is the support.

---

## Appendix A. Illustrative Organisation Comparison

This appendix contains a fictional before-and-after comparison. It is deliberately simplified and should be read as a thought model only. It focuses on the software development function. Many of the roles listed (project managers, architects, security leads) do work beyond software delivery. The ADM changes their software delivery effort, not their entire job. Organisations should not read this as a one-for-one workforce conversion exercise or overstate the benefit for roles that span multiple functions.

It shows one illustrative path from a traditional software delivery organisation to an ADM organisation. It is not a benchmark and it should not be read as a target staffing model.

### A.1 Pre-ADM Organisation

A typical 100-person software development function in a mid-market organisation looks like this. This chart covers the people involved in building and changing software. IT operations, service desk and infrastructure support functions are excluded because the ADM does not address them.

```
CTO / IT Director
├── Head of Development
│   ├── 6 Scrum Masters
│   ├── 31 Developers (9 senior, 15 mid, 7 junior)
│   ├── 8 QA Engineers
│   └── 3 Automation Testers
├── Head of BA & Projects
│   ├── 6 Project Managers
│   ├── 10 Business Analysts
│   └── 3 Product Owners
├── Head of Platform / DevOps
│   ├── 5 DevOps Engineers
│   ├── 4 Infrastructure Engineers
│   └── 2 Cloud/Platform Engineers
├── Head of Architecture
│   ├── 2 Solution Architects
│   ├── 1 Technical Architect
│   └── 1 Enterprise Architect
├── Security Lead
│   ├── 1 Security Architect
│   ├── 1 Security Engineer
│   └── 1 Security/Compliance Lead
├── PMO / Governance
│   └── 2 PMO Analysts
└── Data
    ├── 4 Data Engineers
    └── 2 Data Analysts
```

**Total: 100 people**

Of these 100 people, 86 (Development, BA and Projects, Platform/DevOps and Architecture) are dedicated to software delivery. The remaining 14 (Security, PMO/Governance and Data) have responsibilities that extend beyond software delivery into broader organisational functions. The ADM comparison is most meaningful for the 86 dedicated software delivery roles.

This organisation runs two-week sprints across 4 development squads, holds daily standups per squad, sprint planning, sprint reviews, retrospectives and cross-squad dependency meetings. Project managers coordinate across squads. A large BA team translates business needs into stories and manages requirements through refinement sessions. Scrum Masters facilitate ceremonies. Solution architects define technical approaches. Security architects and engineers handle threat modelling, code security reviews and compliance standards. QA engineers test manually and maintain automation suites. DevOps engineers manage pipelines and infrastructure. A data team supports reporting and analytics. The majority of developer time is spent writing code, with significant time lost to ceremonies, context switching and waiting for dependencies.

### A.2 Illustrative ADM Organisation

This is the matching illustrative ADM organisation used for comparison in this appendix. It keeps the same focus on software delivery roles only. It excludes wider enterprise functions outside the delivery scope.

```
CTO / IT Director
├── Delivery Lead
│   ├── 6 Intent Architects
│   ├── 4 Verification Engineers
│   ├── 2 System Stewards
│   └── 1 Platform Engineer
├── Product Lead
│   ├── 2 Product Owners
│   └── 2 Programme Managers
├── Security Lead
│   ├── 1 Security Architect
│   ├── 1 Security Engineer
│   └── 1 Security/Compliance Lead
├── PMO / Governance
│   └── 2 PMO Analysts
└── Data
    ├── 4 Data Engineers
    └── 2 Data Analysts
```

**Total: 33 people plus an agent fleet**

Of these 33 people, 19 (under the Delivery Lead and Product Lead) are the ADM delivery organisation. The remaining 14 (Security, PMO/Governance and Data) are carried across at their pre-ADM headcount because these roles serve functions outside software development. The after organisation could be smaller. Some of these supporting roles will benefit from agent assistance over time. This comparison keeps them unchanged to avoid overstating the saving. The meaningful comparison is 86 dedicated software delivery people before the ADM to 19 after it. That is the structural shift the model enables.

This chart does not show a dedicated Design Steward. Where the product has a user-facing surface, the Design Steward role (see [Section 6.4](MODEL.md#64-design-steward)) should be filled. In smaller teams, a System Steward may hold both responsibilities. In larger teams or design-intensive products, it is a dedicated role.

An illustrative agent fleet working with this team might include Requirements Agents, Implementation Agents, Testing Agents, a Review Agent, a Documentation Agent, a Platform Agent, a Refactoring Agent, a Security Agent, a Design Agent and a Scheduling Agent. The exact mix would vary with backlog depth, risk profile and cost appetite.

### A.3 Indicative Role Transition Table

This table expands the role transition view summarised in Section 3.

| Traditional Role | ADM Outcome | Rationale |
|-----------------|-------------|-----------|
| **Developer (junior, mid, senior)** | Implementation effort by people reduces sharply. Senior engineers move into Intent Architect, Verification Engineer, System Steward or Platform Engineering roles. Junior and mid-level engineers either develop into verification and specification support roles or move into adjacent engineering paths. | Agents execute most implementation work. The durable value shifts towards specification quality, verification quality, platform control and system judgement. |
| **Business Analyst** | Split between the Product Owner, Intent Architect and Requirements Agent. | Requirements elicitation, scope shaping and backlog refinement still exist, but they are no longer a stand-alone handoff function. |
| **Product Owner** | Retained and expanded. | The business accountability becomes more important because execution is faster and less forgiving of ambiguity. |
| **Project Manager** | Reduced within software delivery. Some move into programme or change roles that sit around software delivery. Some may still remain where software delivery is one part of a broader business change initiative. | The ADM removes much of the task-level coordination inside delivery. It does not remove the need to coordinate rollout, readiness, training and dependencies outside the codebase. |
| **Programme Manager** | Usually retained where multi-team or business change coordination is required. | Enterprise delivery still needs people to coordinate funding, readiness, rollout and non-technical dependencies. |
| **Scrum Master / Delivery Manager** | Ceremony facilitation reduces sharply. Governance facilitation moves to the Product Owner. | Continuous agent execution removes much of the sprint ceremony overhead that created this role in its current form. The Product Owner chairs the governance sessions. |
| **Solution Architect** | Usually transitions into Intent Architect or System Steward. | Solution design remains necessary, but the output must now be encoded as executable intent and structural constraints. |
| **Technical Architect** | Usually transitions into Intent Architect or System Steward. | Technical boundary-setting and pattern selection remain central to reliable agent execution. |
| **Enterprise Architect** | Remains outside the delivery team or aligns with a senior System Steward function. | Enterprise-wide standards, technology direction and cross-domain governance still require ownership by someone. |
| **QA / Test Engineer** | Mechanical test execution becomes heavily agent-led. Human testing remains in verification, exploratory testing and acceptance judgement. | Agents can generate and run tests quickly, but people still decide whether the right things are being tested. |
| **Security Architect** | Retained in governance or mapped into Security Lead and System Steward responsibilities. | Security design and control decisions remain key accountabilities. |
| **Security Engineer** | Continuous scanning and routine checks become agent-led. The work shifts towards control design, triage, threat modelling and exception handling. | The Security Agent handles routine code and dependency review, but security ownership remains with people. |
| **Security / Compliance Lead** | Retained. | Regulated environments need named accountability for security, compliance and assurance. |
| **Technical Author / Documentation Specialist** | Some work becomes agent-led through the Documentation Agent. Specialist documentation roles remain where audience complexity, regulatory burden or training content requires them. | Documentation is still required. The model changes how much of it is drafted automatically. |
| **Platform / DevOps Engineer** | Retained and elevated. | Agentic delivery increases the importance of sandboxing, pipeline governance, observability, cost control and access boundaries. |
| **PMO / Governance Analyst** | Reduced inside software delivery, though some responsibilities remain at portfolio or enterprise level. | The ADM reduces status reporting and sprint administration but does not eliminate governance outside the team boundary. |
| **Data Engineer / Analyst** | Often retained, with agent support. | Data work can follow the same agentic pattern, but the roles do not disappear simply because the software team changes shape. |
| **Delivery Lead** | Retained or created as a people management role. Does not facilitate delivery governance (the Product Owner does). | Someone still needs to manage people, handle capacity planning and provide the organisational escalation path. The Delivery Lead sits outside the delivery cadence and attends governance sessions only when people or capacity issues are relevant. |

### A.4 Illustrative Scaling Model

| Organisation Shape | Traditional Delivery Team | ADM Human Team | Agent Fleet | Caveat |
|-------------------|---------------------------|----------------|-------------|--------|
| **Single product, moderate complexity** | 15 to 20 people | 5 to 8 people | 4 to 8 concurrent agents | Wide variance based on legacy burden and regulatory overhead |
| **Mid-market, 3 to 5 products** | 60 to 100 people | 20 to 35 people | 10 to 20 concurrent agents per product | Shared services and governance roles may sit outside the counted team |
| **Large enterprise, 10 or more products** | 200 to 400 people | 50 to 100 people | Scaled agent fleet | Enterprise architecture, security and portfolio functions may remain outside delivery teams |

These figures are illustrative estimates, not claims of observed norm. They are intended to show direction of travel. The actual mix will depend on system complexity, controls, shared services and how much of the wider change effort sits inside the technology function.
