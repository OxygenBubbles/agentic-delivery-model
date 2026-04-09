# Project Kestrel: A Worked Example of the ADM in Practice

This document is a companion to [MODEL.md](MODEL.md). It expands the Kestrel worked example with concrete artefacts that bring the Agentic Delivery Model to life.

The example is fictional but grounded in realistic constraints. It shows how teams structure work, how agents and people interact, how artefacts flow through the governance tiers and what the daily rhythm of agentic delivery looks like.

**Tooling Note.** This example uses Azure DevOps as the backlog tool and describes how agents connect to it. The ADM is not tied to any specific tool. Any backlog platform with a usable API (Jira, Linear, GitHub Projects, etc.) will work the same way. Organisations should be cautious about deep investment in sprint-based tool customisation that will need rebuilding as delivery tooling matures to support ADM patterns natively. See Section 8 for guidance on future-proofing tool choices.

---

## 1. The Project

Project Kestrel is a customer self-service portal for a mid-market UK insurance company. The portal allows policyholders to view their policies, submit amendment requests, track claims and manage their account details. The company has an existing monolithic policy administration system and a modern API layer built in the last 18 months.

The project scope is deliberately modern. No legacy constraints that would obscure the model. No greenfield fantasy either. A working API layer already exists. Integration points are defined. Performance budgets are known. Regulatory compliance is non-trivial but not extreme (Financial Conduct Authority oversight of insurance product terms, data protection under GDPR). This is the kind of project that hundreds of mid-market companies run every year.

---

## 2. The Team

```
Sarah Chen, Delivery Lead
├── James Wright, Product Owner
├── Priya Patel, Intent Architect (ex-Senior Developer, 8 years)
├── Tom Ellis, Intent Architect (ex-Tech Lead, 12 years)
├── Aisha Khan, Verification Engineer (ex-Senior Developer, 6 years)
├── Marcus Brown, Verification Engineer (ex-Mid Developer, 4 years, being mentored)
├── Rachel Lee, System Steward (ex-Solutions Architect, 15 years)
├── Dev Kapoor, Platform Engineer (manages agent infrastructure)
├── Agent Fleet
│   ├── REQ-1: Requirements Agent (interviews stakeholders, drafts specifications)
│   ├── IMP-1 to IMP-6: Implementation Agents (concurrent execution, also handle refactoring specs)
│   ├── TST-1: Testing Agent (generates and maintains test suites)
│   ├── RVW-1: Review Agent (automated code review and standards checking)
│   ├── DOC-1: Documentation Agent (maintains API docs and user guides)
│   ├── PLT-1: Platform Agent (CI/CD and environment management)
│   └── SEC-1: Security Agent (dependency and code security scanning)
```

Total: 8 people and 12 agents.

Rachel Lee (System Steward) also acts as Design Steward for this project, maintaining design coherence across the portal's user-facing surface. This is common in smaller teams where one person has both the architectural and design judgement to hold both roles (see MODEL.md Section 6.4). Security governance is provided by the organisation's Security Lead, who sits outside the project team but attends the weekly Strategic Review and is consulted on security-sensitive specifications. The Security Agent (SEC-1) provides continuous automated scanning, but human security accountability is not delegated to the project team.

This example is illustrative. Under a traditional model, this project scope might require 15 to 20 people and a 6-month timeline. Under the ADM, the same scope is estimated to be delivered by 8 people in 10 to 14 weeks. These are early experience figures, not benchmarks. Organisations should track their own delivery throughput as evidence accumulates.

---

## 3. A Typical Week

### The Rhythm

**Monday to Friday, 24/7: Implementation agents** work through the Ready backlog. Each agent picks up one Intent Specification, creates a feature branch, implements the change, runs automated tests and creates a PR linked to the ADO work item. A well-specified item of moderate complexity (for example, "Build the policy amendment request form with validation and submission to the amendments API") is typically completed in 2-4 hours.

RVW-1 (Review Agent) reviews each PR automatically before it reaches the governance checkpoint. It checks for standards compliance, security patterns and structural consistency. Technical clarifications (such as which error response schema to use for a new endpoint) are resolved between RVW-1 and the implementing agent without people involved. Only intent clarifications reach the morning governance session.

Not all PRs require detailed discussion. RVW-1's automated pre-checks (standards compliance, security scanning, test execution) filter the queue before the governance session. PRs that pass all automated checks and are routine implementations of well-specified items are reviewed briefly: the Verification Engineer confirms the diff matches the intent, checks the test results and approves. Only PRs that RVW-1 flagged for attention, that touch shared services or that require deeper structural judgement need extended discussion. In a typical session, 60-70% of PRs are routine approvals (2-3 minutes each) and 30-40% require deeper review (5-8 minutes each). On a day with 12 PRs, that means roughly 8 routine reviews and 4 detailed reviews, fitting comfortably within the 30-minute governance window.

### Monday to Friday, 09:00-10:00: Governance Session (Daily)

James facilitates. As Product Owner, James chairs the governance session and keeps it focused on business outcomes. The team reviews all PRs created overnight. Aisha and Marcus (Verification Engineers) verify each PR against its Intent Specification, checking that the output matches the acceptance criteria, that tests are meaningful and that no hidden debt or inconsistency has slipped past automated review. Tom and Priya (Intent Architects) answer clarification requests, refine specifications that agents flagged as ambiguous and review whether any upcoming specifications need decomposition based on what the overnight execution revealed. James confirms business intent is met. Rachel reviews PRs that touch shared services or raise coherence concerns. PRs are approved, rejected with feedback or escalated to Rachel for System Steward review. Sarah attends when people or capacity issues need addressing but this is a delivery session, not a management meeting.

The team also clears the clarification queue. Two agents flagged ambiguity overnight: one could not determine the correct error message format for amendment validation failures (specification gap), and one found a conflict between the stated API schema and the existing claims API (structural issue). James answers the first. Rachel answers the second. Both specifications are updated and the items return to the Ready backlog.

### Monday to Friday, 10:00-10:30: Requirements Sessions (as needed)

James runs a requirements interview with the customer service team lead, facilitated by REQ-1 (Requirements Agent). The agent asks structured questions about the claims tracking workflow, generating a draft Intent Specification for the next feature set. James reviews the draft for business accuracy and passes it to Priya, who adds system constraints and marks it as Ready.

### Wednesday, 14:00-15:30: Strategic Review (Weekly)

James facilitates. Rachel, the organisation's Security Lead and Tom and Priya attend. The team reviews the weekly metrics. First-pass acceptance rate is 72% (target: 80%), suggesting specifications need more detail on edge cases. Clarification rate is 15% (target: under 10%), with most requests relating to API error handling patterns. Rachel adds a new entry to the Coherence Register: standardise error response format across all portal APIs. She also commissions a refactoring item to standardise three existing endpoints with the new standard. The Security Lead reviews SEC-1's weekly scan summary and confirms no new findings require action. Cost per Intent Specification is reviewed. The downward trend from week one confirms that more precise specifications reduce agent rework and clarification cycles, which are the main cost drivers.

---

## 4. Example Artefact: Intent Brief for Policy Amendment Request Form

This is the entry point for the design phase. James Wright (Product Owner) writes this to initiate discovery and scope the work.

```
Intent Brief: Policy Amendment Request Form
Ref: IB-0042

Business Objective:
Allow policyholders to submit requests to amend key policy details (named insured, 
coverage limits, payment method) through the self-service portal without contacting 
the call centre. This reduces call volume by an estimated 30% for routine amendments, 
improves customer satisfaction through reduced wait times and creates an audit trail 
for compliance.

Success Criteria:
- Policyholders can submit amendment requests for named insured, coverage limits and 
  payment method without human intervention
- Amendment requests reach the amendment processing team within 2 seconds of submission
- Customer receives email confirmation with amendment reference number and expected 
  processing time
- Amendment audit trail is maintained for FCA oversight (12-month retention)

Known Constraints:
- Policy amendment processing is handled by a legacy system (PEAK), integrated via 
  REST API
- The integration accepts only customer ID, amendment type, amendment value and 
  reference ID
- Certain amendment types (e.g. coverage changes for lives under 18) require 
  authorisation from the original underwriter and cannot be processed through 
  self-service
- Payment method amendments cannot change the payment schedule, only the card or bank 
  account number
- The legacy system response time can exceed 3 seconds under load (peak hours 09:00-11:00 
  UK time)

Stakeholders:
- Joanne Butler (Customer Service Manager) - owns amendment processing workflow
- Richard Khan (Compliance Officer) - owns audit trail requirements
- Dr. Amelia Foster (UX Researcher) - owns customer journey validation

Complexity: High
Discovery Mode: Paired
[High complexity due to legacy integration constraints, regulatory audit requirements, and 
need to design customer experience for potentially rejected amendments. Stakeholder interviews 
are essential to understand edge cases.]

Notes:
REQ-1 should interview Joanne to understand:
- Current amendment volumes by type (monthly data)
- Amendment rejection reasons and rates
- Time-to-process currently
- Failure modes in PEAK integration

This feature is part of Strategic Intent SI-012 (self-service portal launch, April 2026).
```

---

## 5. Example Artefact: Intent Specification for Policy Amendment Request Form

This is the specification that Aisha or Marcus will verify against when the implementation is complete. It is drafted by REQ-1 from stakeholder interviews, refined by Priya (Intent Architect) for feasibility and scope, and approved by James for business accuracy.

```
INTENT SPECIFICATION
====================
ID: ADO-4127
Title: Build policy amendment request form with validation and async submission
Status: Ready
Created: 2026-03-20
Last Updated: 2026-03-22

BUSINESS INTENT
What: Enable policyholders to submit amendment requests for three specific amendment types 
(named insured update, coverage limit change, payment method change) through a web form 
in the self-service portal. The form validates input, submits to the amendment API and 
provides user confirmation with a reference number.

Why: Reduce call centre volume for routine amendments by an estimated 30%, improve customer 
experience through instant submission confirmation instead of queuing for a person, 
and create an auditable digital record for FCA compliance. This is a staged delivery: phase 1 
covers the form and submission workflow. Phase 2 (future spec) will add real-time amendment 
status tracking.

Who: Policyholders aged 18 and over with active policies. Excludes minors and policies with 
underwriter-level restrictions (flagged in the policy record).

ACCEPTANCE CRITERIA
1. GIVEN a policyholder on the policy details page WHEN they click "Request amendment" 
   THEN the amendment request form loads with three amendment type options (named insured, 
   coverage limits, payment method)

2. GIVEN the amendment form is displayed WHEN the customer selects "Named Insured" 
   THEN the form shows a text input for new insured name with character limit 100, 
   tooltip explaining full legal name required, and a "Verify Name" button

3. GIVEN the form shows a coverage limit amendment option WHEN the customer selects this option 
   THEN the form displays current coverage limits (read-only) and numeric inputs for new limits, 
   with per-field validation min/max values matching insurance underwriting rules

4. GIVEN the form shows a payment method amendment option WHEN the customer selects this option 
   THEN the form displays current payment method (masked card number or bank details) and 
   input fields for new payment details (card number, expiry, CVC or bank account, sort code)

5. GIVEN all required fields are populated with valid data WHEN the customer clicks "Submit Request" 
   THEN the form disables the submit button (prevent double-click), shows a loading spinner, 
   and calls POST /amendments endpoint (see Integration Points below)

6. GIVEN the amendment API returns HTTP 200 with amendment reference ID THEN the form is cleared, 
   a success message displays showing the amendment reference number, customer name, amendment 
   type and estimated processing time ("within 1-2 business days"), and an email confirmation 
   is sent to the registered email address (via existing email service)

7. GIVEN the amendment API returns HTTP 400 with validation error THEN the form displays 
   field-level error messages matching the API error response and the submit button is re-enabled

8. GIVEN the amendment API returns HTTP 403 (amendment type not allowed for this policy, e.g. 
   minor or restricted underwriter case) THEN the form displays a customer-facing message: 
   "This amendment cannot be processed online. Please contact our customer service team on 
   [phone number]" and a "Call us" link to the contact page

9. GIVEN the amendment API returns HTTP 500 or times out after 10 seconds THEN the form displays 
   a general error message: "We couldn't process your request. Please try again in a few minutes 
   or contact customer service." and enables the submit button for retry

10. GIVEN an amendment form submission has been attempted WHEN the customer navigates away from the 
    page before completion THEN a browser warning dialog appears: "Your amendment request is 
    unsaved. Leave anyway?" (standard HTML5 beforeunload behaviour)

TECHNICAL CONSTRAINTS
- Must use the existing .NET React component library (PolicyForm, FormInput, ValidationMessage, 
  LoadingSpinner components)
- Must follow REST API contract for POST /amendments endpoint at /api/policy-service/v1/amendments
- Amendment API response schema: { amendmentId: string, customerName: string, amendmentType: 
  enum[NAMED_INSURED|COVERAGE_LIMITS|PAYMENT_METHOD], estimatedProcessingDays: number, 
  submissionTimestamp: ISO8601 }
- Error responses follow standard error schema: { code: string, message: string, fieldErrors: 
  { [fieldName]: string } }
- All numeric inputs must use Intl.NumberFormat for locale-aware formatting (GBP for amounts)
- Must not touch the policy service core, claims service, or customer authentication layer
- Password/CVC fields must not be logged by error handlers

DESIGN CONSTRAINTS
- Use the existing PolicyDetailsPage layout (one-column, 960px max-width)
- Component: use FormContainer wrapper and PolicyForm component for consistency with 
  existing portal forms
- Amendment type selection: radio button group (not dropdown, to show all options at once)
- Input fields: use the standard FormInput component with integrated validation error display
- Loading state: use the LoadingSpinner component during submission (2-3 second expected latency 
  from API, may exceed on peak load)
- Error handling: use ValidationMessage component for field errors, AlertBox component for 
  general errors (type="error", dismissible=true)
- Accessibility: WCAG 2.1 AA minimum (form labels linked to inputs via htmlFor, error messages 
  linked via aria-describedby, loading state announced to screen readers)
- Responsive: must work on mobile (viewport 375px width) without horizontal scroll

INTEGRATION POINTS
- POST /api/policy-service/v1/amendments (legacy PEAK integration, 3-5 second response time 
  typical, can exceed 10 seconds under peak load 09:00-11:00)
- GET /api/customer-service/v1/policies/{policyId} (read current policy details for display 
  in amendment form, may be cached from page load)
- POST /api/email-service/v1/send-confirmation (async call, fire-and-forget after successful 
  amendment submission, failure of email send does not fail the amendment request)

TEST EXPECTATIONS

Unit tests (React component level):
- Render the form with all three amendment type options visible
- Verify that selecting each amendment type shows the correct form fields
- Verify that empty required fields prevent form submission
- Verify that numeric validation works for coverage limits (min/max rules enforced)
- Verify that card number field accepts only digits
- Verify that form clears after successful submission
- Verify that error messages display correctly for each API error code
- Verify that loading spinner shows during submission and hides after response

Integration tests:
- Test form submission with valid named insured amendment data to mock amendments API, verify 
  success response is handled correctly
- Test form submission with validation error response (HTTP 400), verify field errors display
- Test form submission with forbidden response (HTTP 403, amendment type not allowed), verify 
  customer-facing message displays
- Test form submission with server error (HTTP 500), verify error message and retry enabled
- Test form submission with timeout (API does not respond within 10 seconds), verify timeout 
  error handling
- Test that email confirmation call succeeds asynchronously (do not block submission response on email result)
- Test accessibility: form is navigable by keyboard, error messages are announced to screen readers

Edge cases:
- Amendment form submitted with extra whitespace in text fields (should trim before submission)
- Customer submits amendment, waits 30 seconds, submits again (second submission should be 
  rejected as duplicate within 60-second window, or allowed if policy amendment was already processed)
- Customer on slow network connection (3G, latency 1+ second) submits form, verify submit button 
  remains disabled until response received
- Amendment API returns 200 but missing amendmentId in response (handle gracefully, log error, 
  show general error message)

COMPLEXITY ASSESSMENT
- Agent autonomy: Full (well-specified API contract, clear UI patterns, defined error cases)
- Estimated scope: M (3-4 hours for a competent Implementation Agent)
- Dependencies: None blocking. Amendment API is already live and tested. Email service already operational.

DESIGN NOTES
This specification covers only the client-side form and submission workflow. Amendment processing, 
status tracking and underwriter review are out of scope for this spec. The decision to restrict 
amendments to three types (not all amendment types) is a business decision to start with high-volume, 
low-risk amendments. Additional amendment types can be added in future specs following the same pattern.

The 10-second timeout on amendment API calls is deliberate: the legacy PEAK system can be slow under load. 
A longer timeout risks poor customer experience (form appears frozen). A shorter timeout risks rejecting 
valid requests under peak load. This 10-second threshold was chosen by Rachel (System Steward) in 
consultation with the infrastructure team based on observed PEAK latency distributions.

PROVENANCE
- Requirements source: Interview with Joanne Butler (Customer Service Manager) 2026-03-20, 
  interview with Richard Khan (Compliance Officer) 2026-03-21
- Drafted by: REQ-1 (Requirements Agent) on 2026-03-20
- Refined by: Priya Patel (Intent Architect) on 2026-03-21 (feasibility and scope)
- Approved by: James Wright (Product Owner) on 2026-03-22 (business accuracy)
```

---

## 6. Example Artefact: Completed PR Description with Provenance

When the implementation agent (IMP-3) completes this specification, it creates a PR. The PR description follows a standard template that includes provenance metadata, allowing the Verification Engineer to trace the change back through intent, specification and agent execution.

```
# [MERGED] Build policy amendment request form with validation and async submission

## Provenance
- **Specification**: ADO-4127
- **Intent Brief**: IB-0042 (Policy Amendment Request Form)
- **Implementing Agent**: IMP-3 (Implementation Agent)
- **Specification Status at Execution**: Ready
- **Execution Start**: 2026-03-23 09:15 UTC
- **PR Created**: 2026-03-23 11:47 UTC
- **Execution Time**: 2 hours 32 minutes

## Change Summary
Implementation of the policy amendment request form component, including:
- React form component with three amendment type options (named insured, coverage limits, payment method)
- Form validation at field level using existing PolicyForm component library
- Async submission to /api/policy-service/v1/amendments with error handling
- Success/error state rendering with customer-facing messages
- Email confirmation call (fire-and-forget) on successful submission
- Full WCAG 2.1 AA keyboard navigation and screen reader support

## Acceptance Criteria Status
All 10 acceptance criteria from ADO-4127 verified against this implementation:

✓ AC-1: Amendment form renders with three amendment type options on policy details page
✓ AC-2: Named Insured option shows text input with 100-char limit and verification button
✓ AC-3: Coverage limits option shows numeric inputs with underwriting min/max validation
✓ AC-4: Payment method option shows card/bank detail inputs
✓ AC-5: Valid form disables submit button, shows spinner, calls POST /amendments
✓ AC-6: HTTP 200 response clears form, displays success message with amendment reference and ETA
✓ AC-7: HTTP 400 displays field-level errors and re-enables submit
✓ AC-8: HTTP 403 displays customer-facing message and contact link
✓ AC-9: HTTP 500/timeout displays error message and enables retry
✓ AC-10: Navigation away from form triggers beforeunload warning dialog

## Files Modified
- src/components/PolicyAmendmentForm/PolicyAmendmentForm.tsx (new, 280 lines)
- src/components/PolicyAmendmentForm/PolicyAmendmentForm.test.tsx (new, 420 lines)
- src/api/amendmentService.ts (new, 60 lines)
- src/pages/PolicyDetailsPage.tsx (modified, +45 lines for form rendering and state)
- src/styles/PolicyAmendmentForm.module.css (new, 85 lines)

## Test Results
- Unit tests: 24 tests, 24 passed, 0 failed (coverage: 94% statements, 89% branches)
- Integration tests: 8 tests, 8 passed, 0 failed
- Accessibility audit (axe-core): 0 violations, 0 warnings (WCAG 2.1 AA verified)
- Security scan (dependency check): 0 high/critical vulnerabilities
- Performance: Form loads in 280ms (Lighthouse). Amendment submission includes 2-3 second API latency but UX perceived as responsive (spinner visible immediately).

## Review Agent Notes (RVW-1)
- Standards compliance: PASS. Component follows PolicyForm library patterns, error handling follows team conventions, naming consistent with codebase.
- Code quality: PASS. Cyclomatic complexity within limits, no obvious performance issues, error boundaries properly implemented.
- Security: PASS. No hardcoded secrets, card/CVC fields not logged, submission over HTTPS only. Dependency versions up to date.
- Technical clarification needed: NONE

## Specification Drift Check
Implementation matches specification exactly. No deviations, additions or subtractions. Ready for verification.

## Pre-Checks by RVW-1 (Review Agent)
- Automated code review: PASS
- Unit test execution: PASS (24/24)
- Integration test execution: PASS (8/8)
- Security scan: PASS (0 high/critical)
- Accessibility audit: PASS (WCAG 2.1 AA)
- Regression suite: PASS (220 existing tests, 0 new failures)
- Linting: PASS (ESLint, Prettier)
- Build: PASS

Ready for human verification in governance session.

## Changes to Specification or Dependent Items
None. All assumptions in ADO-4127 were accurate. Integration API contract matched specification. No dependencies unblocked or changed during implementation.

---

**Verified by**: Aisha Khan (Verification Engineer) on 2026-03-23 10:15 UTC. Output matches Intent Specification. Tests pass. No hidden debt identified.
**Business intent validated by**: James Wright (Product Owner) on 2026-03-23 10:15 UTC. Confirmed the implementation serves the business objective stated in IB-0042.
**System coherence confirmed by**: Rachel Lee (System Steward) on 2026-03-23 10:20 UTC. No cross-cutting concerns. Amendment form follows existing PolicyForm patterns. No new Coherence Register entries required.
**Design coherence confirmed by**: Rachel Lee (acting Design Steward) on 2026-03-23 10:20 UTC. Component usage follows existing portal design system. WCAG 2.1 AA confirmed by automated audit.
**Merged by**: Aisha Khan (Verification Engineer) on 2026-03-23 10:25 UTC
**Merged to**: main
**Deployment approval**: Ready for next release cycle (scheduled 2026-03-27)
```

---

## 7. Backlog Tooling

The Kestrel team uses Azure DevOps with agent connections to query and update work items. Work items are tagged with `adm:ready` when they pass the Definition of Ready. Implementation agents query for items with this tag, ordered by priority.

Work item fields include:

- **Title**: Concise, action-oriented (e.g. "Build policy amendment request form with validation and submission")
- **Description**: The full Intent Specification template (see Section 5)
- **State**: Draft → In Review → Ready → In Progress → Done
- **ADM Tags**: `adm:ready`, `adm:intent-brief`, `adm:in-governance`, `adm:blocked-clarification`
- **Complexity**: S / M / L (used for agent workload planning)
- **Estimated Effort**: Hours (agent estimates only, not story points)
- **Cost Tracker**: Currency cost per specification (tracked weekly)
- **Related Items**: Links to parent Strategic Intent, related specifications, dependent work
- **Provenance Metadata** (populated when spec moves to Done):
  - Implementing agent ID
  - Execution start/end timestamps
  - PR reference
  - Verification Engineer who approved
  - First-pass acceptance (Y/N)
  - Clarifications raised during execution
  - Technical debt identified

Clarification requests are posted as comments on the work item with an `adm:clarification` tag. The Governance Tier reviews these each morning and updates the specification. Agents monitor for specification updates and pull latest versions before starting work.

---

## 8. The Future of Delivery Tooling

The ADM uses Azure DevOps in this worked example because it is common in enterprise environments. Jira, Linear, GitHub Projects and similar tools all work the same way: they hold the backlog, track state and provide the API surface that agents connect to.

But these tools were designed for a world where people manage work items. Jira's sprint boards, story points, burndown charts and velocity tracking all assume that people work in time-boxed iterations. Azure DevOps' sprint planning, capacity allocation and team velocity features assume the same. In the ADM, most of this is irrelevant. There are no sprints. There are no story points. Velocity is measured in Intent Specifications completed, not points delivered.

These platforms will evolve. The commercial incentive is clear: if their customers are moving to agentic delivery, the tooling must follow or be replaced. Organisations should look for backlog tools that natively support:

- Intent Specifications rather than user stories
- Agent fleet dashboards showing which specifications are in progress across which agents
- Governance workflows that surface PRs for human review with the right context
- Cost tracking per specification
- Provenance metadata as first-class fields
- Clarification workflows that keep agent execution and human review synchronised

The organisations that build these tools fastest will capture the market. Those that bolt agent features onto sprint-based architectures will struggle. The structural assumption in the data model matters. A tool built around sprints and story points cannot simply relabel them as governance cycles and Intent Specifications. The underlying workflows are different.

For now, any backlog tool with a reasonable API works. The ADM does not prescribe tooling. But organisations should avoid deep investment in sprint-based tool customisation that will need rebuilding when the tooling catches up.

This guidance is alpha and will evolve as tools mature to support ADM patterns natively.

---

## 9. Appendix: The Coherence Register (Strategic Tier Example)

Rachel Lee (System Steward) maintains the Coherence Register for Kestrel. This is reviewed weekly at the Strategic Tier (Wednesdays 14:00-15:30). It serves as the architecture decision log, technical debt register and coherence risk tracker.

```
COHERENCE REGISTER
==================
Last updated: 2026-03-22
System Steward: Rachel Lee
Next review: 2026-03-29

ARCHITECTURAL DECISIONS
| ID | Decision | Date | Rationale | Review Date |
|----|----------|------|-----------|-------------|
| AD-001 | Use event-driven pattern for policy amendments (async queue to legacy PEAK system) | 2026-03-01 | Decouples self-service submission from legacy system latency. Allows 10-second timeout on client while maintaining reliable amendment processing. | 2026-06-01 |
| AD-002 | Single REST endpoint for all amendment types, not separate endpoints per type | 2026-03-08 | Simplifies legacy system integration. PEAK accepts single schema. Easier to extend to new amendment types without schema versioning. | 2026-06-08 |
| AD-003 | Client-side form validation before submission, plus server-side validation on API | 2026-03-15 | Improves customer experience (instant feedback), reduces invalid submissions to backend. Server validation is defence-in-depth. | 2026-06-15 |

KNOWN TECHNICAL DEBT
| ID | Description | Severity | Owner | Decision | Target Date |
|----|-------------|----------|-------|----------|-------------|
| TD-001 | Duplicate validation logic: amendment type restrictions hardcoded in three places (form, API gateway, backend service) | Medium | Rachel Lee | Remediate: centralise to API service layer, expose as specification config. Create ADO-4201 to refactor. | 2026-04-30 |
| TD-002 | Legacy PEAK integration error responses are inconsistent (HTTP 400 sometimes used for 403 cases, message text varies) | Medium | Rachel Lee | Accept until PEAK v3 upgrade scheduled Q3 2026. Document error code mapping in agent configuration. | 2026-09-30 |
| TD-003 | Email confirmation service has no retry logic; failed emails are logged but not re-sent | Low | Rachel Lee | Accept for v1 launch. Create future spec for async email retry queue. Low business impact (customer can check amendment reference in portal). | 2026-06-30 |

COHERENCE RISKS
| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|------------|
| Agent-generated amendment form and existing policy details form diverge on input validation patterns | High | Medium | Enforce validation pattern standard in PolicyForm component library. All forms must use FormInput component and shared validation rules. Add automated coherence check in code review to flag direct field validation. |
| Legacy PEAK integration latency causes customer experience degradation if volumes exceed 500 amendments/hour | Medium | High | Implement circuit breaker on amendment submission (if API response time exceeds 10s, fail fast rather than timeout). Monitor amendment submission latency weekly at Strategic Tier. |
| Payment method amendment specification does not account for account holder name mismatch (customer enters different name than registered bank account holder) | Medium | Medium | Add acceptance criterion to future payment amendment spec: "Validate that account holder name matches registered customer name before submitting to bank verification API". Document in agent prompt. |
| Accessibility regression: future form additions may not follow WCAG 2.1 AA standard if not enforced in code review | Medium | Medium | Add accessibility audit to RVW-1 checklist (already done for ADO-4127). Make WCAG 2.1 AA validation mandatory pre-check before PR reaches governance. |
```

---

*The Agentic Delivery Model is licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE).*
