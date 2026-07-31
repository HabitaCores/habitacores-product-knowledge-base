# Document 12 — User Journeys and Workflow Specifications

## Document Control

| Field | Value |
|---|---|
| Product | Habitacores |
| Document | User Journeys and Workflow Specifications |
| Version | 0.1 |
| Status | Draft for approval |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Market | Nigeria |
| Primary Currency | Nigerian naira |
| Primary Operational Time Zone | West Africa Time |
| Primary Tenant Boundary | Organization |
| Last Updated | 30 July 2026 |

## 1. Purpose

This document defines the end-to-end user journeys and operational workflows for Habitacores.

It translates the approved:

- Product vision
- Product principles
- Business architecture
- Domain model
- Users, personas, and roles
- Business rules
- Functional requirements
- Non-functional requirements
- System architecture
- Data architecture
- Permission matrix and authorization model

into structured, testable workflow specifications.

This document defines:

- User goals
- Workflow actors
- Preconditions
- Triggering events
- Standard paths
- Alternative paths
- Exception paths
- Role handoffs
- Required permissions
- Business state transitions
- Notifications
- Audit events
- Data requirements
- Recovery behavior
- Workflow acceptance criteria

This document does not define final interface layouts, visual wireframes, API payloads, executable database migrations, or production runbooks.

Those areas will be defined in subsequent Product Knowledge Base documents.

## 2. Workflow Objectives

Habitacores workflows must:

1. Provide clear end-to-end journeys for every principal user type.
2. Preserve organization isolation.
3. Enforce approved permissions and assignments.
4. Preserve financial, legal, agreement, and occupancy integrity.
5. Support the Nigerian annual-rent lifecycle.
6. Prevent silent or uncontrolled state changes.
7. Preserve human accountability for high-impact decisions.
8. Distinguish preparation, review, approval, execution, and verification.
9. Support low-bandwidth and mobile-browser use.
10. Provide clear status visibility.
11. Support interruption and safe resumption.
12. Generate reliable notifications.
13. Preserve audit trails.
14. Support the founding pilot without hardcoding it.
15. Remain extensible for future organizations, properties, regions, and mobile applications.

## 3. Workflow Design Principles

### 3.1 Explicit State Transitions

Critical records must move through approved states.

Users must not freely overwrite lifecycle status fields.

### 3.2 Human-Readable Status

Users should see understandable status descriptions.

Examples:

- Application submitted
- Additional information required
- Payment pending verification
- Agreement awaiting signature
- Move-in awaiting approval

### 3.3 Role-Aware Workspaces

Each user should see tasks, records, and actions relevant to their active role and scope.

### 3.4 Safe Resumption

Long workflows must preserve progress and permit authorized users to resume.

### 3.5 No False Completion

The interface must not show a workflow as complete while required actions remain pending.

### 3.6 Controlled Handoffs

Handoffs between users or teams must be recorded through assignments, queues, approvals, and status transitions.

### 3.7 Fail Safely

External-provider failures must preserve the authoritative Habitacores state.

### 3.8 Separate Evidence from Verification

Submitting evidence does not constitute verification.

### 3.9 Separate Recommendation from Decision

AI summaries, agent notes, reviewer recommendations, and legal advice do not independently constitute final decisions.

### 3.10 Unknown Remains Unknown

Workflows must not invent missing property, payment, identity, or inventory facts.

## 4. Workflow Specification Format

Each workflow may include:

```text
Workflow Identifier
Workflow Name
Primary Actor
Supporting Actors
Trigger
Preconditions
Required Permissions
Input Data
Standard Flow
Alternative Flows
Exception Flows
State Transitions
Notifications
Audit Events
Postconditions
Acceptance Criteria
````

## 5. Workflow Actor Groups

### Public and Residential Actors

* Public Prospect
* Registered Prospect
* Applicant
* Co-Applicant
* Guarantor
* Primary Resident
* Co-Resident
* Household Member
* Former Resident

### Property and Organization Actors

* Organization Owner
* Organization Administrator
* Property Owner Administrator
* Property Operator
* Property Manager
* Community Administrator
* Finance Reviewer
* Maintenance Coordinator
* Document Reviewer
* Development Manager

### Professional Actors

* Estate Agency Administrator
* Estate Agent
* Legal Firm Administrator
* Estate Lawyer
* Vendor Administrator
* Vendor Worker
* Contractor

### Platform Actors

* Platform Owner
* Platform Administrator
* Platform Support Agent
* Platform Security Administrator
* Platform Auditor

## 6. Journey Map Overview

The principal residential journey is:

```text
Discover Property
→ Submit Inquiry
→ Schedule Inspection
→ Attend Inspection
→ Create Application
→ Submit Application
→ Application Review
→ Decision
→ Offer
→ Charge
→ Payment
→ Payment Verification
→ Agreement
→ Signature
→ Activation
→ Move-In
→ Occupancy
→ Renewal or Move-Out
```

The principal maintenance journey is:

```text
Report Issue
→ Triage
→ Create Work Order
→ Assign Vendor
→ Vendor Accepts
→ Work Performed
→ Completion Evidence
→ Verification
→ Closure
```

The principal financial journey is:

```text
Create Charge
→ Approve Charge
→ Record Payment
→ Submit Evidence
→ Verify Payment
→ Allocate Payment
→ Issue Receipt
→ Reconcile
```

## 7. Shared Workflow States

Habitacores workflows may use the following general state classes:

```text
Draft
Submitted
Pending Review
Information Required
Pending Approval
Approved
Rejected
Scheduled
In Progress
Pending Verification
Verified
Completed
Closed
Cancelled
Expired
Superseded
Archived
```

Each domain must use its approved domain-specific status values.

## 8. Shared Workflow Controls

Every material workflow must support:

* Organization context
* Role and scope validation
* Assignment validation
* Record-state validation
* Required-field validation
* Duplicate-action protection
* Audit event
* Correlation identifier
* User-visible status
* Notification handling
* Error recovery
* Authorization-safe response behavior

# Part I — Public Discovery and Prospect Journeys

## 9. Journey JRN-PUB-001 — Browse Public Listings

### Primary Actor

Public Prospect

### Goal

Discover available rental properties without requiring authentication.

### Preconditions

* Listing is approved.
* Listing is published.
* Unit is eligible for public marketing.
* Organization permits public publication.

### Required Permissions

Public access through:

```text
listing.read_public
listing.search_public
community.read_public
```

### Standard Flow

1. Prospect opens the Habitacores public marketplace.
2. System loads approved public listings.
3. Prospect filters by available supported criteria.
4. System returns paginated matching listings.
5. Prospect opens a listing.
6. System displays approved public information.
7. Prospect may continue to inquiry or inspection request.

### Public Information May Include

* Public property or community name
* Listing title
* Unit type
* Bedroom count
* Bathroom count
* Annual rent
* Approved additional charges
* Availability date
* General location
* Approved amenities
* Approved media
* Inspection availability
* Listing status

### Restricted Information

The public listing must not expose:

* Resident information
* Applicant information
* Internal property notes
* Ownership documents
* Exact security controls
* Private unit access instructions
* Internal agent notes
* Payment records
* Legal records

### Alternative Flow — No Results

1. System displays that no listings match the selected criteria.
2. Prospect may modify filters.
3. System may display approved related listings.

### Exception Flow — Listing Unpublished

If a listing becomes paused, closed, expired, or unavailable:

1. System stops presenting it as available.
2. Existing direct links show a safe unavailable status.
3. Private listing data remains hidden.

### Audit Events

Public listing views may be aggregated for analytics.

Personally identifying audit records should not be created unnecessarily for anonymous browsing.

### Acceptance Criteria

```text
Given:
A listing is in draft status.

When:
A public user requests the listing URL.

Then:
The system does not expose the draft listing.
```

## 10. Journey JRN-PUB-002 — Create Prospect Account

### Primary Actor

Public Prospect

### Goal

Create a Habitacores identity to continue an inquiry, inspection, or application.

### Standard Flow

1. Prospect selects Create Account.
2. System requests minimum required registration information.
3. Prospect provides email or approved phone-based identity.
4. System validates format.
5. System sends verification challenge.
6. Prospect completes verification.
7. System creates the user identity.
8. System creates or links the prospect profile.
9. System returns the prospect to the intended workflow.

### Required Data

* Name
* Email or phone
* Authentication credential or provider
* Consent acknowledgements
* Preferred contact method where supported

### Exception Flow — Existing Identity

1. System identifies an existing account.
2. System directs the person to authenticate or recover access.
3. System does not create a duplicate identity.

### Exception Flow — Verification Failure

1. Account remains unverified.
2. Prospect may request a new challenge within rate limits.
3. Protected actions remain unavailable.

### Audit Events

* UserRegistered
* EmailVerificationRequested
* EmailVerified
* PhoneVerified
* RegistrationFailed

## 11. Journey JRN-PUB-003 — Submit Listing Inquiry

### Primary Actor

Public Prospect or Registered Prospect

### Supporting Actors

Estate Agent
Property Operator

### Trigger

Prospect selects Contact, Ask a Question, or Request Information.

### Preconditions

* Listing accepts inquiries.
* Required contact information is available.
* Anti-abuse controls permit submission.

### Standard Flow

1. Prospect opens the inquiry form.
2. System prepopulates known information for authenticated users.
3. Prospect enters a message and contact preference.
4. System validates submission.
5. System creates or updates the prospect record.
6. System creates the inquiry.
7. System associates the inquiry with the listing and unit.
8. System assigns the inquiry according to organization rules.
9. Assigned user receives a notification.
10. Prospect receives confirmation and inquiry reference.

### State Transition

```text
new
→ assigned
→ contacted
→ qualified
→ closed
```

### Alternative Flow — No Agent Assignment

1. Inquiry enters an unassigned queue.
2. Organization operations users receive queue visibility.
3. Authorized user assigns the inquiry later.

### Exception Flow — Duplicate Inquiry

The system may:

* Link the inquiry to an existing active inquiry
* Warn the prospect
* Create a new inquiry when the message is materially different

The system must not silently discard the submission.

### Notifications

* Inquiry confirmation to prospect
* New inquiry notification to assigned agent
* Escalation for overdue unassigned inquiry

### Audit Events

* InquiryCreated
* InquiryAssigned
* InquiryReassigned
* InquiryClosed

## 12. Journey JRN-PUB-004 — Agent Responds to Inquiry

### Primary Actor

Estate Agent

### Preconditions

* Agent is assigned to the listing or inquiry.
* Assignment is active.
* Inquiry is open.

### Required Permissions

```text
inquiry.read
inquiry.update
communication.create
communication.send
```

### Standard Flow

1. Agent opens assigned inquiry queue.
2. Agent selects an inquiry.
3. System displays authorized prospect and listing context.
4. Agent records contact activity.
5. Agent sends or logs approved communication.
6. Agent updates inquiry status.
7. Agent may propose an inspection.
8. System records communication history.

### Restricted Actions

Agent may not:

* Approve applications
* Verify payments
* Alter approved rent without authorization
* Access unrelated inquiries
* Read resident records

### Exception Flow — Assignment Expired

1. System denies operational access.
2. Historical attribution remains visible to authorized administrators.
3. Inquiry returns to assignment queue where configured.

# Part II — Inspection Journeys

## 13. Journey JRN-INSP-001 — Request Inspection

### Primary Actor

Prospect or Applicant

### Supporting Actor

Estate Agent or Property Operator

### Preconditions

* Listing supports inspections.
* Unit is available for inspection.
* Approved inspection times exist or manual scheduling is enabled.

### Standard Flow

1. User selects Request Inspection.
2. System displays available dates or request form.
3. User selects preferred time.
4. User confirms contact details.
5. System creates inspection request.
6. System assigns the responsible agent or operator.
7. System sets status to pending confirmation.
8. Assigned user receives notification.
9. User receives request acknowledgement.

### State Transition

```text
requested
→ pending_confirmation
→ confirmed
→ completed
```

Alternative endings:

```text
cancelled
no_show
rescheduled
```

### Exception Flow — Time No Longer Available

1. System rejects the selected slot.
2. System returns current availability.
3. No duplicate inspection is created.

## 14. Journey JRN-INSP-002 — Confirm Inspection

### Primary Actor

Estate Agent or Property Operator

### Required Permissions

```text
inspection.read
inspection.confirm
inspection.reschedule
```

### Standard Flow

1. Assigned user opens pending inspection request.
2. User verifies unit and schedule availability.
3. User confirms or proposes an alternative.
4. System records assigned host.
5. System updates inspection status.
6. Prospect receives confirmation and approved instructions.
7. Reminder jobs are scheduled.

### Notification Content May Include

* Date
* Time
* General meeting location
* Contact person
* Rescheduling link
* Approved preparation instructions

Sensitive access instructions must not be exposed prematurely.

## 15. Journey JRN-INSP-003 — Reschedule Inspection

### Actors

Prospect
Estate Agent
Property Operator

### Standard Flow

1. Authorized actor requests rescheduling.
2. System displays available alternatives.
3. Actor selects new schedule.
4. System validates availability.
5. System records prior and new times.
6. System updates reminders.
7. All relevant participants receive notice.

### Audit Events

* InspectionRescheduled
* InspectionCancellationRequested
* InspectionCancelled

## 16. Journey JRN-INSP-004 — Complete Inspection

### Primary Actor

Estate Agent or Property Operator

### Standard Flow

1. Host opens the inspection record.
2. Host records attendance.
3. Host records approved outcome.
4. Host records prospect interest.
5. System updates inspection status.
6. System may offer an application invitation.
7. Prospect receives approved follow-up.

### Outcome Examples

* Interested
* Requires follow-up
* Not interested
* Unit unsuitable
* Prospect no-show
* Reschedule requested

### Restricted Data

The agent must not record unlawful discriminatory conclusions.

# Part III — Application Journeys

## 17. Journey JRN-APP-001 — Start Application

### Primary Actor

Applicant

### Preconditions

* Applicant is authenticated.
* Listing accepts applications.
* Unit is eligible for application.
* Applicant does not have a conflicting duplicate application unless permitted.

### Required Permissions

```text
application.create
application.read_own
application.update_draft
```

### Standard Flow

1. Applicant selects Apply.
2. System confirms listing and unit.
3. System creates a draft application.
4. System associates the applicant as primary applicant.
5. System displays required sections.
6. Applicant begins entering information.
7. System autosaves or preserves drafts.
8. System displays progress status.

### Alternative Flow — Application by Invitation

1. Agent or operator sends an application invitation.
2. Applicant authenticates.
3. System links the invitation to the user.
4. Applicant starts the application.

### Exception Flow — Listing Closed

1. System prevents new application creation.
2. Existing draft handling follows organization policy.
3. Applicant receives a clear explanation.

## 18. Journey JRN-APP-002 — Complete Applicant Profile

### Primary Actor

Applicant

### Standard Flow

1. Applicant enters required identity and contact information.
2. Applicant enters household information.
3. Applicant adds co-applicants or guarantors where required.
4. System validates required fields.
5. System records completion state.
6. Applicant proceeds to documents and declarations.

### Data-Minimization Rule

Only information approved as necessary for the application process should be collected.

### Sensitive Data Controls

Sensitive fields must:

* Be protected server side
* Be excluded from ordinary logs
* Be limited by role
* Follow retention policy

## 19. Journey JRN-APP-003 — Invite Co-Applicant or Guarantor

### Primary Actor

Applicant

### Supporting Actors

Co-Applicant
Guarantor

### Standard Flow

1. Applicant adds the party’s name and contact information.
2. System creates a pending application-party record.
3. System sends an invitation.
4. Invited party authenticates or creates an account.
5. System validates invitation.
6. Party completes their assigned sections.
7. Applicant sees completion status but not restricted private details.
8. Application may proceed once requirements are satisfied.

### Exception Flow — Invitation Declined

1. Invited party declines.
2. System records the decline.
3. Primary applicant receives notice.
4. Applicant may replace the party where permitted.

### Exception Flow — Invitation Expired

1. System marks invitation expired.
2. Applicant may resend or replace it.

## 20. Journey JRN-APP-004 — Upload Application Documents

### Primary Actor

Applicant, Co-Applicant, or Guarantor

### Standard Flow

1. User opens document requirements.
2. System displays required and optional document categories.
3. User selects a document.
4. System validates type and size.
5. File uploads to protected storage.
6. System performs validation and malware scanning where configured.
7. Document metadata is created.
8. Requirement status becomes submitted or pending review.
9. User receives upload confirmation.

### Exception Flow — Invalid File

1. System rejects the file.
2. System explains the acceptable format.
3. Existing valid documents remain unchanged.

### Exception Flow — Scan Failure

1. Document remains unavailable or pending validation.
2. Review cannot proceed using the file.
3. User receives a safe status message.
4. Operations receive failure visibility.

## 21. Journey JRN-APP-005 — Submit Application

### Primary Actor

Applicant

### Preconditions

* Required sections are complete.
* Required parties have completed their obligations.
* Required documents are submitted.
* Applicant has accepted required declarations.
* Application remains eligible for submission.

### Standard Flow

1. Applicant selects Review Application.
2. System displays a submission summary.
3. Applicant confirms accuracy.
4. System validates completeness.
5. Applicant submits.
6. System locks controlled submitted fields.
7. Application status changes to submitted.
8. Submission timestamp is recorded.
9. Review queue receives the application.
10. Applicant receives confirmation.

### State Transition

```text
draft
→ submitted
```

### Exception Flow — Validation Failure

1. Application remains draft.
2. System identifies incomplete sections.
3. No submission audit event is created as successful.
4. Applicant can correct and retry.

### Audit Events

* ApplicationCreated
* ApplicationSubmitted
* ApplicationSubmissionFailed

## 22. Journey JRN-APP-006 — Withdraw Application

### Primary Actor

Applicant

### Preconditions

* Application is withdrawable under current state.
* No irreversible agreement or occupancy state exists.

### Standard Flow

1. Applicant selects Withdraw.
2. System displays consequences.
3. Applicant provides optional or required reason.
4. Applicant confirms.
5. Application status changes to withdrawn.
6. Review assignments are closed.
7. Related pending workflows are updated.
8. Relevant organization users receive notification.

### Restricted Behavior

Withdrawal must not:

* Delete application history
* Delete payments
* Delete issued legal records
* Falsely imply automatic refund entitlement

# Part IV — Application Review and Decision Journeys

## 23. Journey JRN-REV-001 — Assign Application Reviewer

### Primary Actor

Property Owner Administrator or Authorized Operations User

### Standard Flow

1. User opens submitted application queue.
2. User selects an application.
3. System displays eligible reviewers.
4. User assigns reviewer.
5. System records assignment scope and effective date.
6. Reviewer receives notification.
7. Application status may change to under review.

### Required Permissions

```text
application.assign_reviewer
```

### Exception Flow — Conflict of Interest

1. System identifies known conflicting role or participation.
2. Assignment is blocked or requires disclosure and approval.
3. Attempt is recorded.

## 24. Journey JRN-REV-002 — Review Application

### Primary Actor

Assigned Application Reviewer

### Preconditions

* Reviewer has active assignment.
* Application is submitted or under review.
* Reviewer has required permissions.

### Standard Flow

1. Reviewer opens assigned queue.
2. Reviewer selects application.
3. System displays authorized sections and documents.
4. Reviewer verifies completeness.
5. Reviewer records review findings.
6. Reviewer may request more information.
7. Reviewer records recommendation.
8. Reviewer completes review.
9. Final decision queue is updated.

### Reviewer May

* Review submitted information
* Review authorized documents
* Record internal notes
* Identify missing or inconsistent information
* Request clarification
* Recommend approval, conditional approval, or rejection

### Reviewer May Not

* Invent information
* Alter applicant-submitted content silently
* Make discriminatory decisions
* Finalize a decision without decision authority
* Expose internal notes to the applicant

## 25. Journey JRN-REV-003 — Request Additional Information

### Primary Actor

Assigned Reviewer

### Supporting Actor

Applicant

### Standard Flow

1. Reviewer identifies missing or unclear information.
2. Reviewer creates an information request.
3. Reviewer selects required response items.
4. Reviewer sets due date where appropriate.
5. Application status changes to information requested.
6. Applicant receives notification.
7. Applicant opens the request.
8. Applicant submits response and supporting documents.
9. System records response.
10. Reviewer receives notification.
11. Application returns to under review.

### State Transition

```text
under_review
→ information_requested
→ under_review
```

### Exception Flow — Applicant Does Not Respond

Depending on policy:

* Reminder is sent.
* Deadline may be extended.
* Application may expire.
* Reviewer may recommend closure.

The system must not automatically reject unless an approved policy explicitly allows it.

## 26. Journey JRN-REV-004 — Record Review Recommendation

### Primary Actor

Application Reviewer

### Standard Flow

1. Reviewer completes required checks.
2. Reviewer selects recommendation.
3. Reviewer records rationale.
4. Reviewer identifies conditions where relevant.
5. System validates completeness.
6. Recommendation becomes final for that review.
7. Decision-maker receives notification.

### Possible Recommendations

* Recommend approval
* Recommend conditional approval
* Recommend rejection
* Return for more information
* Escalate for legal or owner review

### AI Boundary

AI may summarize the application but may not make the final recommendation without human review.

## 27. Journey JRN-DEC-001 — Make Application Decision

### Primary Actor

Property Owner Administrator or Authorized Decision-Maker

### Preconditions

* Review is complete or decision policy permits direct action.
* Decision-maker has authority.
* Conflict checks pass.
* Application is in a decision-eligible state.

### Standard Flow

1. Decision-maker opens decision queue.
2. System displays application summary, review recommendation, and authorized supporting information.
3. Decision-maker selects decision.
4. Decision-maker records rationale.
5. Conditions are entered where applicable.
6. System validates decision authority and state.
7. Decision record is created.
8. Application state changes.
9. Applicant receives approved decision communication.
10. Related offer or closure workflow begins.

### Decision Outcomes

```text
approved
conditionally_approved
rejected
information_required
expired
```

### Restricted Behavior

Decision-maker may not:

* Backdate decision without authorized correction process
* Delete earlier decision history
* Use protected discriminatory grounds
* Alter payment state
* Issue an agreement before required steps

## 28. Journey JRN-DEC-002 — Reject Application

### Primary Actor

Authorized Decision-Maker

### Standard Flow

1. Decision-maker selects Reject.
2. System requires internal decision rationale.
3. System requires applicant-facing reason where policy permits.
4. System validates reason and authority.
5. Rejection decision is recorded.
6. Application status becomes rejected.
7. Applicant receives approved communication.
8. Related pending offer workflow is blocked.

### Appeal or Reconsideration

A future reconsideration must:

* Create a new review or decision event
* Preserve the original decision
* Record who authorized reconsideration
* Avoid silent status reversal

## 29. Journey JRN-DEC-003 — Conditionally Approve Application

### Primary Actor

Authorized Decision-Maker

### Standard Flow

1. Decision-maker selects Conditional Approval.
2. Conditions are defined.
3. Completion deadline is set where relevant.
4. Applicant receives condition notice.
5. Applicant submits required items.
6. Reviewer verifies completion.
7. Decision-maker confirms final approval.
8. Application status changes to approved.

### Example Conditions

* Additional guarantor documentation
* Corrected identity information
* Required payment schedule confirmation
* Additional signed declaration

Conditions must be lawful, relevant, and documented.

# Part V — Offer Journeys

## 30. Journey JRN-OFF-001 — Create Offer

### Primary Actor

Property Owner Administrator or Authorized User

### Preconditions

* Application is approved or conditionally approved as permitted.
* Unit remains eligible.
* Rent and charge data are approved.
* Offer creator has authority.

### Standard Flow

1. User opens approved application.
2. User selects Create Offer.
3. System creates offer draft.
4. System prepopulates approved rent, term, unit, and charges.
5. User reviews dates and conditions.
6. User submits offer for approval where required.
7. Approver reviews offer.
8. Offer becomes approved.
9. Authorized user issues offer.
10. Applicant receives offer notification.

### Offer Must Include

* Unit
* Annual rent
* Currency
* Proposed term
* Payment deadline
* Approved mandatory charges
* Conditions
* Expiry
* Acceptance instructions

## 31. Journey JRN-OFF-002 — Applicant Reviews Offer

### Primary Actor

Applicant

### Standard Flow

1. Applicant receives offer notification.
2. Applicant authenticates.
3. System displays the issued offer.
4. Applicant reviews rent, charges, dates, conditions, and expiry.
5. Applicant may accept, decline, or request clarification.
6. System records response.

### Restricted Behavior

An issued offer must not be edited silently.

A material change requires a replacement offer version.

## 32. Journey JRN-OFF-003 — Accept Offer

### Primary Actor

Applicant

### Preconditions

* Offer is issued.
* Offer has not expired.
* Offer has not been replaced or withdrawn.
* Applicant is authorized to respond.

### Standard Flow

1. Applicant selects Accept.
2. System displays acceptance declaration.
3. Applicant confirms.
4. System records acceptance.
5. Offer status changes to accepted.
6. Charge workflow begins.
7. Organization receives notification.
8. Applicant sees next required actions.

### Exception Flow — Offer Expired

1. System prevents acceptance.
2. Offer remains historically visible.
3. Applicant may request a new offer.
4. Authorized user may issue replacement offer.

## 33. Journey JRN-OFF-004 — Decline Offer

### Primary Actor

Applicant

### Standard Flow

1. Applicant selects Decline.
2. System requests optional or required reason.
3. Applicant confirms.
4. Offer status becomes declined.
5. Organization receives notification.
6. Unit and application workflows update according to policy.

Declining an offer must not delete the application history.

## 34. Journey JRN-OFF-005 — Replace Offer

### Primary Actor

Authorized Offer Manager

### Standard Flow

1. User opens current offer.
2. User identifies required material change.
3. User creates replacement version.
4. System links replacement to prior offer.
5. Replacement follows approval process.
6. Prior offer becomes superseded.
7. Applicant receives replacement notice.

# Part VI — Charge, Payment, and Financial Journeys

## 35. Journey JRN-FIN-001 — Create Charge

### Primary Actor

Authorized Finance or Property User

### Preconditions

* Charge basis exists.
* Charge type is valid.
* Recipient organization is known.
* Amount and currency are valid.
* User has authority.

### Standard Flow

1. User selects Create Charge.
2. System prepopulates linked offer, agreement, unit, payer, and charge type where available.
3. User enters amount, due date, and description.
4. System validates charge policy.
5. Charge is saved as draft.
6. Charge follows approval where required.
7. Approved charge is issued to payer.
8. Payer receives notification.

### Restricted Behavior

The system must not combine annual rent, agency fee, legal fee, deposit, or service charge into one ambiguous amount when separate disclosure is required.

## 36. Journey JRN-FIN-002 — Approve Charge

### Primary Actor

Authorized Approver

### Preconditions

* Charge is in approval-ready state.
* User is not prohibited from self-approval.
* Amount and charge type are valid.

### Standard Flow

1. Approver reviews charge details.
2. Approver confirms payer, recipient, amount, and due date.
3. Approver approves or rejects.
4. System records approval decision.
5. Approved charge becomes payable.
6. Payer receives notice.

## 37. Journey JRN-FIN-003 — Submit Payment Evidence

### Primary Actor

Applicant or Resident

### Preconditions

* Payable charge exists.
* User is authorized for the charge.
* Payment method permits evidence submission.

### Standard Flow

1. User opens outstanding charge.
2. User selects Record Payment or Submit Evidence.
3. User enters payment date, amount, method, and reference.
4. User uploads evidence.
5. System validates fields and file.
6. Payment record is created in pending-verification state.
7. Finance review queue receives the payment.
8. User receives confirmation.

### State Transition

```text
recorded
→ pending_verification
```

### Critical Rule

A submitted screenshot, receipt, or transfer record does not automatically verify payment.

## 38. Journey JRN-FIN-004 — Review Payment Evidence

### Primary Actor

Finance Reviewer

### Preconditions

* Finance Reviewer has permission and assignment.
* Payment is pending verification.
* Evidence is available.

### Standard Flow

1. Reviewer opens pending payment queue.
2. Reviewer selects payment.
3. System displays charge, payer, amount, evidence, and available provider or bank references.
4. Reviewer checks for consistency.
5. Reviewer may request more information.
6. Reviewer verifies or rejects payment.
7. System records decision and rationale.
8. Payer receives notification.
9. Verified payment becomes eligible for allocation.

### Verification Checks May Include

* Amount
* Date
* Recipient
* Reference
* Currency
* Duplicate payment
* Bank or provider confirmation where available
* Evidence authenticity indicators

### Restricted Behavior

Finance Reviewer must not verify based solely on an unsupported screenshot when the policy requires corroboration.

## 39. Journey JRN-FIN-005 — Verify Payment

### Primary Actor

Finance Reviewer

### Preconditions

* Payment is pending verification.
* Evidence is sufficient.
* Reviewer has authority.
* Self-verification restrictions pass.

### Standard Flow

1. Reviewer selects Verify.
2. System displays verification confirmation.
3. Reviewer records verification source or rationale.
4. System validates amount and state.
5. Payment status changes to verified.
6. Verification timestamp and actor are recorded.
7. Allocation workflow becomes available.
8. Audit event is created.

### Exception Flow — Duplicate Provider Reference

1. System blocks verification.
2. Reviewer receives duplicate warning.
3. Payment remains pending.
4. Finance investigation is required.

## 40. Journey JRN-FIN-006 — Reject Payment Evidence

### Primary Actor

Finance Reviewer

### Standard Flow

1. Reviewer selects Reject.
2. Reviewer records reason.
3. Payment status changes to rejected.
4. No allocation is permitted.
5. Payer receives actionable notice.
6. Payer may submit corrected evidence or a new payment where permitted.

## 41. Journey JRN-FIN-007 — Allocate Payment

### Primary Actor

Finance Reviewer

### Preconditions

* Payment is verified.
* Charge is payable.
* Currency matches.
* Unallocated amount is sufficient.
* Charge balance is sufficient.
* Reviewer has permission.

### Standard Flow

1. Reviewer opens verified payment.
2. System displays eligible charges.
3. Reviewer selects charge and allocation amount.
4. System validates financial constraints.
5. System locks relevant records.
6. Allocation is created.
7. Payment and charge balances are recalculated.
8. Statuses are updated.
9. Receipt workflow may begin.
10. Transaction commits.
11. Audit event is created.

### Failure Rule

If any transactional step fails:

* No partial allocation remains.
* Payment balance remains unchanged.
* Charge balance remains unchanged.

## 42. Journey JRN-FIN-008 — Issue Receipt

### Primary Actor

Finance Reviewer or Authorized System Process

### Preconditions

* Payment is verified.
* Required allocation conditions are satisfied.

### Standard Flow

1. System generates receipt data.
2. Receipt number is assigned.
3. Receipt document is generated.
4. Receipt is linked to payment.
5. Payer receives notice.
6. Receipt becomes available through protected access.

### Receipt Must Show

* Receipt number
* Payment reference
* Amount
* Currency
* Date
* Payer
* Recipient
* Allocated charge information
* Issuing organization

## 43. Journey JRN-FIN-009 — Reverse Payment

### Primary Actor

Authorized Finance User

### Supporting Actor

Secondary Approver where required

### Preconditions

* Payment is eligible for reversal.
* User has reversal authority.
* Separation-of-duties requirements pass.

### Standard Flow

1. User requests reversal.
2. User provides reason and evidence.
3. System identifies affected allocations and charges.
4. Approval is requested where required.
5. Approver reviews reversal.
6. System executes reversal transactionally.
7. Allocations are reversed.
8. Charge balances are restored.
9. Payment status changes.
10. Audit and notifications are generated.

### Restricted Behavior

Verified payment history must not be deleted or rewritten.

## 44. Journey JRN-FIN-010 — Request and Approve Refund

### Actors

Applicant or Resident
Finance Reviewer
Authorized Approver

### Standard Flow

1. Refund request is created.
2. System links request to payment, deposit, or charge.
3. Finance Reviewer checks eligibility.
4. Refund amount is proposed.
5. Required approval is requested.
6. Approver approves or rejects.
7. Approved refund moves to execution.
8. Execution reference is recorded.
9. Financial balances update transactionally.
10. Recipient receives notice.

### Exception Flow — Refund Provider Failure

1. Refund remains approved but not executed.
2. System records provider failure.
3. No false completed status is shown.
4. Retry or manual resolution is initiated.

## 45. Journey JRN-FIN-011 — Reconcile Financial Records

### Primary Actor

Finance Reviewer

### Standard Flow

1. Reviewer opens reconciliation workspace.
2. System displays unmatched, unallocated, duplicated, or inconsistent records.
3. Reviewer investigates each discrepancy.
4. Reviewer records resolution.
5. Corrective workflow is initiated where needed.
6. Reconciliation report is produced.

### Reconciliation Categories

* Unallocated verified payments
* Duplicate provider references
* Overallocated payments
* Outstanding charges marked paid
* Refund mismatch
* Deposit mismatch
* Receipt mismatch

# Part VII — Agreement Journeys

## 46. Journey JRN-AGR-001 — Create Agreement Draft

### Primary Actor

Property Owner Administrator, Property Operator, or Estate Lawyer

### Preconditions

* Offer is accepted.
* Approved agreement template exists.
* Unit and parties are known.
* Required financial conditions are available.

### Standard Flow

1. Authorized user selects Create Agreement.
2. System creates agreement identity.
3. System creates draft agreement version.
4. Approved offer and party data are inserted.
5. User reviews terms.
6. User saves draft.
7. Agreement enters review workflow.

### Restricted Behavior

The draft must not become legally active merely because it exists.

## 47. Journey JRN-AGR-002 — Legal Review Agreement

### Primary Actor

Assigned Estate Lawyer

### Preconditions

* Lawyer assignment is active.
* Agreement is in legal-review state.
* Lawyer has access to required supporting records.

### Standard Flow

1. Lawyer opens assigned agreement.
2. System displays approved agreement data and authorized supporting context.
3. Lawyer reviews terms.
4. Lawyer records comments or proposed changes.
5. Lawyer marks review complete or returns draft for revision.
6. Property organization receives notification.

### Legal Advice Boundary

Habitacores records the lawyer’s review but does not independently generate a legal conclusion.

## 48. Journey JRN-AGR-003 — Approve Agreement

### Primary Actor

Property Owner Administrator

### Preconditions

* Draft is complete.
* Required legal review is complete.
* Financial terms match approved offer.
* User has authority.

### Standard Flow

1. Approver opens agreement.
2. Approver reviews parties, unit, rent, term, charges, and legal review.
3. Approver approves or returns for revision.
4. Approved version becomes issue-ready.
5. Approval audit event is created.

## 49. Journey JRN-AGR-004 — Issue Agreement

### Primary Actor

Authorized Agreement User

### Standard Flow

1. User selects Issue Agreement.
2. System validates approved version.
3. System makes issued version immutable.
4. System records issue timestamp.
5. Agreement parties receive notification.
6. Signature workflow begins.

### Critical Rule

Material changes after issuance require a new version.

## 50. Journey JRN-AGR-005 — Sign Agreement

### Primary Actors

Applicant or Resident
Property Signatory
Guarantor where required
Other Agreement Parties

### Standard Flow

1. Party receives signature request.
2. Party authenticates or uses approved secure signing method.
3. System presents the exact issued version.
4. Party reviews agreement.
5. Party provides signature or acknowledgement.
6. System records signature method, timestamp, and evidence.
7. Agreement signature status updates.
8. Remaining parties are notified as needed.

### Exception Flow — Signature Declined

1. Party records decline.
2. Agreement remains unsigned.
3. Organization receives notification.
4. Agreement may be revised, replaced, or cancelled.

## 51. Journey JRN-AGR-006 — Activate Agreement

### Primary Actor

Authorized Property User

### Preconditions

* Required parties have signed.
* Required payment conditions are satisfied.
* Unit remains eligible.
* Agreement dates are valid.
* Activation authority exists.

### Standard Flow

1. User selects Activate Agreement.
2. System validates signatures.
3. System validates payment requirements.
4. System validates unit and occupancy conflicts.
5. System activates agreement.
6. System creates planned occupancy.
7. Move-in workflow becomes available.
8. Audit event is created.
9. Parties receive confirmation.

### Transaction Rule

Agreement activation and planned occupancy creation must complete together where required.

## 52. Journey JRN-AGR-007 — Amend Agreement

### Primary Actor

Authorized Agreement User

### Standard Flow

1. User opens active agreement.
2. User selects Create Amendment.
3. System creates new draft version.
4. User records reason for amendment.
5. New version follows review, approval, issue, and signature.
6. Prior version remains immutable.
7. Replacement version becomes active according to approved effective date.

## 53. Journey JRN-AGR-008 — Terminate Agreement

### Primary Actor

Authorized Property Decision-Maker

### Supporting Actor

Estate Lawyer where required

### Preconditions

* Termination basis is documented.
* Required legal review is complete.
* User has authority.
* Notice requirements are satisfied.

### Standard Flow

1. User initiates termination case.
2. System requires reason and evidence.
3. Legal review occurs where required.
4. Approval is recorded.
5. Notice is issued.
6. Agreement status changes according to effective date.
7. Occupancy and move-out workflows are updated.
8. Audit events are generated.

### Restricted Behavior

Agreement termination must never be performed autonomously by AI.

# Part VIII — Move-In and Occupancy Journeys

## 54. Journey JRN-MOVE-001 — Prepare Unit for Move-In

### Primary Actor

Property Operator or Maintenance Coordinator

### Preconditions

* Agreement is active or activation-ready.
* Unit is assigned.
* Unit readiness checklist exists.

### Standard Flow

1. User opens move-in preparation.
2. System displays readiness requirements.
3. User verifies maintenance status.
4. User verifies utilities and approved access items.
5. User records condition evidence.
6. User marks preparation complete.
7. System sets unit readiness status.
8. Move-in scheduling becomes available.

## 55. Journey JRN-MOVE-002 — Schedule Move-In

### Primary Actors

Property Operator
Primary Resident

### Standard Flow

1. Authorized user proposes move-in schedule.
2. Resident confirms or requests alternative.
3. System validates schedule.
4. Move-in appointment is confirmed.
5. Relevant users receive notice.
6. Reminder jobs are scheduled.

## 56. Journey JRN-MOVE-003 — Complete Move-In Checklist

### Primary Actor

Property Operator

### Supporting Actor

Primary Resident

### Standard Flow

1. Parties open move-in record.
2. Unit condition is reviewed.
3. Checklist items are completed.
4. Access items are recorded.
5. Resident acknowledges condition.
6. Outstanding items are recorded.
7. Checklist is submitted for completion.

### Evidence May Include

* Unit condition photos
* Meter readings
* Access-item records
* Outstanding repairs
* Resident acknowledgement

## 57. Journey JRN-MOVE-004 — Activate Occupancy

### Primary Actor

Authorized Property User

### Preconditions

* Agreement is active.
* Move-in checklist is complete.
* Required payments are satisfied.
* Unit is ready.
* No conflicting occupancy exists.

### Standard Flow

1. User selects Complete Move-In.
2. System validates all preconditions.
3. Occupancy status becomes active.
4. Unit occupancy status becomes occupied.
5. Resident access becomes active.
6. Household roles become effective.
7. Audit event is created.
8. Resident receives confirmation.

### Failure Rule

If occupancy activation fails:

* Unit must not be marked occupied.
* Resident access must not activate partially.
* Agreement remains unchanged unless transaction design requires rollback.

## 58. Journey JRN-OCC-001 — Resident Views Occupancy

### Primary Actor

Primary Resident or Authorized Co-Resident

### Standard Flow

1. Resident opens dashboard.
2. System displays active occupancy.
3. Resident can access approved agreement, charges, payments, notices, and maintenance.
4. Restricted household or financial details follow role policy.

## 59. Journey JRN-OCC-002 — Request Household Change

### Primary Actor

Primary Resident

### Standard Flow

1. Resident selects Request Household Change.
2. Resident enters proposed change.
3. System creates request.
4. Property operator reviews.
5. Required documents or approvals are collected.
6. Request is approved or rejected.
7. Occupancy parties update with effective dates.
8. Resident receives decision notice.

### Restricted Behavior

Resident may not directly add unauthorized occupants to the official occupancy record.

## 60. Journey JRN-OCC-003 — Remove Household Member

### Actors

Primary Resident
Property Operator

### Standard Flow

1. Authorized request is initiated.
2. System validates agreement and occupancy policy.
3. Effective date is recorded.
4. Household party becomes inactive.
5. Access is revoked where applicable.
6. Historical occupancy association remains.

# Part IX — Resident Service and Notice Journeys

## 61. Journey JRN-RES-001 — Resident Views Charges and Payments

### Primary Actor

Primary Resident or Authorized Co-Resident

### Standard Flow

1. Resident opens Financials.
2. System displays authorized charges.
3. System displays payment and allocation status.
4. Resident may open receipts.
5. Resident may submit payment evidence where permitted.

### Restricted Information

Resident must not see:

* Other residents’ financial records
* Internal reconciliation notes
* Provider raw payloads
* Restricted bank information

## 62. Journey JRN-RES-002 — Publish Resident Notice

### Primary Actor

Property Operator or Community Administrator

### Standard Flow

1. User selects Create Notice.
2. User selects approved audience scope.
3. User drafts notice.
4. Approval occurs where required.
5. Notice is published.
6. Notifications are generated.
7. Delivery status is tracked.

### Audience Scopes

* Property
* Community
* Building
* Block
* Unit
* Active residents
* Selected residents

## 63. Journey JRN-RES-003 — Resident Reads Notice

### Primary Actor

Resident

### Standard Flow

1. Resident receives notification.
2. Resident opens notice.
3. System validates occupancy scope.
4. Notice is displayed.
5. Read status may be recorded.

# Part X — Maintenance and Work-Order Journeys

## 64. Journey JRN-MNT-001 — Submit Maintenance Request

### Primary Actor

Resident

### Preconditions

* Resident has active occupancy.
* Request concerns an authorized unit or community facility.
* Resident has maintenance submission permission.

### Standard Flow

1. Resident selects Report Maintenance Issue.
2. System prepopulates unit and occupancy.
3. Resident selects category.
4. Resident enters description.
5. Resident sets access notes.
6. Resident uploads approved evidence.
7. Resident submits request.
8. System creates maintenance request.
9. Request enters triage queue.
10. Resident receives reference and status.

### Exception Flow — Emergency Issue

1. System displays approved emergency instructions.
2. Request is marked urgent according to policy.
3. Operations receive priority notification.
4. System does not falsely guarantee emergency-service response.

## 65. Journey JRN-MNT-002 — Triage Maintenance Request

### Primary Actor

Maintenance Coordinator or Property Operator

### Standard Flow

1. User opens triage queue.
2. User reviews description and evidence.
3. User confirms location.
4. User sets category and priority.
5. User determines whether immediate action, internal work, or vendor work is required.
6. User records triage decision.
7. Request status updates.
8. Resident receives appropriate update.

### AI Boundary

AI may suggest category or priority but human review remains required for material action.

## 66. Journey JRN-MNT-003 — Create Work Order

### Primary Actor

Maintenance Coordinator

### Preconditions

* Maintenance request requires actionable work.
* Work scope is known.
* User has permission.

### Standard Flow

1. User selects Create Work Order.
2. System links maintenance request.
3. User defines title, scope, priority, location, and cost controls.
4. User selects vendor-assignment method.
5. Work order is created.
6. Assignment workflow begins.

## 67. Journey JRN-MNT-004 — Assign Vendor

### Primary Actor

Maintenance Coordinator

### Standard Flow

1. Coordinator opens work order.
2. System displays eligible approved vendors.
3. Coordinator selects vendor or requests quotations.
4. Assignment is created.
5. Vendor administrator receives notification.
6. Vendor accepts, declines, or assigns worker.

### Exception Flow — Vendor Declines

1. Assignment becomes declined.
2. Reason is recorded.
3. Work order returns to assignment queue.
4. Resident sees an appropriate status without unnecessary vendor details.

## 68. Journey JRN-MNT-005 — Submit Vendor Quote

### Primary Actor

Vendor Administrator

### Standard Flow

1. Vendor opens assigned work order.
2. Vendor reviews scope.
3. Vendor submits quotation.
4. System validates amount and currency.
5. Quote enters approval queue.
6. Coordinator or approver receives notice.

## 69. Journey JRN-MNT-006 — Approve Work-Order Cost

### Primary Actor

Authorized Property User

### Preconditions

* Quote exists.
* Cost approval threshold is known.
* Approver has authority.
* Self-approval rules pass.

### Standard Flow

1. Approver reviews quote and scope.
2. Approver approves, rejects, or requests revision.
3. System records decision.
4. Approved work order becomes executable.
5. Vendor receives notification.

## 70. Journey JRN-MNT-007 — Vendor Accepts Work Order

### Primary Actor

Vendor Administrator or Vendor Worker

### Standard Flow

1. Vendor opens assignment.
2. Vendor reviews scope, schedule, and approved cost.
3. Vendor accepts or declines.
4. Acceptance timestamp is recorded.
5. Work order status updates.
6. Coordinator receives notice.

## 71. Journey JRN-MNT-008 — Vendor Performs Work

### Primary Actor

Vendor Worker

### Standard Flow

1. Worker opens assigned work order.
2. System validates active assignment.
3. Worker starts work.
4. Status changes to in progress.
5. Worker records progress.
6. Worker requests clarification or cost change where needed.
7. Worker completes work.
8. Worker submits completion evidence.

### Restricted Access

Vendor Worker receives only the minimum location, access, and contact information required.

## 72. Journey JRN-MNT-009 — Request Cost or Scope Change

### Primary Actor

Vendor Administrator or Vendor Worker

### Standard Flow

1. Vendor identifies additional work.
2. Vendor submits change request.
3. System records reason, amount, and evidence.
4. Work may pause according to policy.
5. Authorized approver reviews.
6. Change is approved or rejected.
7. Vendor receives decision.

No unapproved cost increase should become payable automatically.

## 73. Journey JRN-MNT-010 — Submit Work Completion

### Primary Actor

Vendor Worker

### Standard Flow

1. Worker marks work complete.
2. Worker enters completion notes.
3. Worker uploads evidence.
4. Worker records parts or approved cost information.
5. Work order status becomes pending verification.
6. Coordinator receives notification.

## 74. Journey JRN-MNT-011 — Verify Work Completion

### Primary Actor

Maintenance Coordinator or Property Operator

### Standard Flow

1. Verifier opens pending completion.
2. Verifier reviews work and evidence.
3. Verifier may inspect physically.
4. Verifier approves completion or reopens work.
5. Work order status updates.
6. Maintenance request updates.
7. Resident receives status notice.

### Separation Rule

Vendor may not verify its own completion on behalf of the property organization.

## 75. Journey JRN-MNT-012 — Close Maintenance Request

### Primary Actor

Maintenance Coordinator

### Preconditions

* Work is verified.
* Outstanding actions are resolved.
* Resident communication is complete where required.

### Standard Flow

1. Coordinator reviews request.
2. Coordinator records closure summary.
3. Request status becomes closed.
4. Resident receives closure notice.
5. Audit event is created.

## 76. Journey JRN-MNT-013 — Reopen Maintenance Request

### Primary Actors

Resident
Maintenance Coordinator

### Standard Flow

1. Resident or coordinator identifies unresolved issue.
2. Reopen request is submitted.
3. Authorized user reviews.
4. Maintenance request returns to active state.
5. Prior completion history remains.
6. New work order may be created.

# Part XI — Renewal Journeys

## 77. Journey JRN-REN-001 — Create Renewal Case

### Primary Actor

System or Authorized Property User

### Trigger

Agreement approaches approved renewal review period.

### Standard Flow

1. System identifies agreement nearing expiry.
2. Renewal case is created.
3. Property decision deadline is calculated.
4. Authorized users receive notification.
5. Renewal review begins.

## 78. Journey JRN-REN-002 — Review Renewal Eligibility

### Primary Actor

Property Owner Administrator

### Supporting Actors

Property Operator
Finance Reviewer
Estate Lawyer where required

### Standard Flow

1. Decision-maker opens renewal case.
2. System displays agreement, occupancy, payment position, maintenance history, and authorized context.
3. Decision-maker reviews renewal options.
4. Decision-maker records decision to renew, not renew, or request further review.
5. System records rationale.
6. Renewal-offer or move-out workflow begins.

### Restricted Behavior

AI may summarize relevant records but may not independently decide renewal.

## 79. Journey JRN-REN-003 — Create Renewal Offer

### Primary Actor

Authorized Property User

### Standard Flow

1. User creates renewal offer.
2. System links existing agreement and occupancy.
3. User enters new annual rent, term, and approved charges.
4. Offer follows approval.
5. Renewal offer is issued.
6. Resident receives notification.

## 80. Journey JRN-REN-004 — Resident Responds to Renewal Offer

### Primary Actor

Primary Resident

### Standard Flow

1. Resident opens renewal offer.
2. Resident reviews proposed terms.
3. Resident accepts, declines, or requests clarification.
4. System records response.
5. Accepted offer begins new agreement workflow.
6. Declined offer begins planned move-out workflow.

## 81. Journey JRN-REN-005 — Complete Renewal

### Primary Actors

Property User
Resident
Estate Lawyer where required

### Standard Flow

1. Accepted renewal offer creates agreement version or replacement agreement.
2. Agreement follows review, approval, issue, and signature.
3. Required payment workflow completes.
4. New agreement activates.
5. Occupancy continues without conflicting duplicate occupancy.
6. Renewal case closes.

# Part XII — Move-Out and Deposit Journeys

## 82. Journey JRN-OUT-001 — Resident Initiates Move-Out

### Primary Actor

Primary Resident

### Preconditions

* Resident has active occupancy.
* Move-out initiation is permitted.
* Required notice period is disclosed.

### Standard Flow

1. Resident selects Initiate Move-Out.
2. System displays notice requirements.
3. Resident enters requested date and reason.
4. Resident confirms submission.
5. Move-out case is created.
6. Property operations receive notice.
7. Inspection and settlement workflow begins.

## 83. Journey JRN-OUT-002 — Organization Initiates Move-Out

### Primary Actor

Authorized Property User

### Preconditions

* Approved legal or operational basis exists.
* Notice requirements are satisfied.
* Required legal review is complete.

### Standard Flow

1. User initiates move-out case.
2. User records basis and supporting documents.
3. Approval occurs.
4. Required notice is issued.
5. Resident receives communication.
6. Move-out workflow proceeds.

## 84. Journey JRN-OUT-003 — Schedule Final Inspection

### Actors

Property Operator
Primary Resident

### Standard Flow

1. Authorized user proposes inspection schedule.
2. Resident confirms or requests alternative.
3. System records confirmed time.
4. Reminders are scheduled.

## 85. Journey JRN-OUT-004 — Record Final Condition

### Primary Actor

Property Operator

### Supporting Actor

Resident

### Standard Flow

1. Parties inspect unit.
2. Condition items are recorded.
3. Photos and evidence are attached.
4. Access items are reconciled.
5. Resident acknowledges or disputes observations.
6. Condition record is finalized.

## 86. Journey JRN-OUT-005 — Propose Deposit Deductions

### Primary Actor

Property Operator or Authorized Finance User

### Preconditions

* Deposit exists.
* Final condition record exists.
* Deduction basis is documented.
* Amounts are supported.

### Standard Flow

1. User opens deposit decision.
2. User lists proposed deductions.
3. Each deduction includes amount, reason, and evidence.
4. System calculates proposed refund.
5. Proposal enters approval workflow.
6. Resident receives approved disclosure.

### Restricted Behavior

Deductions must not exceed the deposit balance without a separate charge process.

## 87. Journey JRN-OUT-006 — Approve Deposit Decision

### Primary Actor

Authorized Approver

### Standard Flow

1. Approver reviews deposit, condition record, and deductions.
2. Approver approves, rejects, or returns for revision.
3. Approved decision becomes final subject to dispute workflow.
4. Refund workflow begins where applicable.

## 88. Journey JRN-OUT-007 — Resident Disputes Deposit Decision

### Primary Actor

Former or Current Resident

### Standard Flow

1. Resident opens deposit decision.
2. Resident selects Dispute.
3. Resident records reason and uploads evidence.
4. System creates dispute record.
5. Authorized reviewer receives notice.
6. Original decision remains visible.
7. Revised decision, settlement, or escalation is recorded.

## 89. Journey JRN-OUT-008 — Complete Move-Out

### Primary Actor

Property Operator

### Preconditions

* Final condition is recorded.
* Access items are returned or accounted for.
* Occupancy end date is confirmed.
* Deposit decision is recorded or pending under approved policy.

### Standard Flow

1. User selects Complete Move-Out.
2. System validates required steps.
3. Occupancy becomes ended.
4. Unit status changes to turnaround or inspection required.
5. Resident role becomes former resident.
6. Historical access is limited according to policy.
7. Agreement status updates where required.
8. Audit event is created.

## 90. Journey JRN-OUT-009 — Unit Turnaround

### Primary Actor

Property Operator or Maintenance Coordinator

### Standard Flow

1. Unit enters turnaround state.
2. Required repairs and cleaning are assessed.
3. Work orders are created.
4. Completion is verified.
5. Unit readiness is updated.
6. Unit becomes available for listing when approved.

# Part XIII — Organization and Access Administration Journeys

## 91. Journey JRN-ORG-001 — Create Organization

### Primary Actor

Platform Administrator or Authorized Onboarding User

### Standard Flow

1. User starts organization onboarding.
2. System collects organization type and legal profile.
3. Organization record is created in pending-verification state.
4. Primary contact is invited.
5. Required verification documents are collected.
6. Platform review occurs.
7. Organization becomes active or is rejected.

## 92. Journey JRN-ORG-002 — Invite Organization Member

### Primary Actor

Organization Administrator

### Standard Flow

1. Administrator selects Invite Member.
2. Administrator enters contact information.
3. Administrator selects permitted role and scope.
4. Privileged role approval is requested where required.
5. Invitation is sent.
6. Recipient accepts.
7. Membership becomes active.
8. Role assignment becomes effective.

### Exception Flow — Existing User

The invitation links the existing global user identity to the organization.

## 93. Journey JRN-ORG-003 — Assign Role

### Primary Actor

Organization Administrator or Organization Owner

### Standard Flow

1. User selects active membership.
2. User selects role and scope.
3. System checks role conflicts and approval requirements.
4. Assignment is created as pending or active.
5. Required approval occurs.
6. User receives notice.
7. Audit event is created.

## 94. Journey JRN-ORG-004 — Revoke Role

### Primary Actor

Authorized Organization Administrator

### Standard Flow

1. Administrator selects role assignment.
2. Administrator provides revocation reason.
3. System checks active workflow dependencies.
4. Role becomes revoked.
5. Cached authorization state is invalidated.
6. User receives notice where appropriate.
7. Historical attribution remains.

## 95. Journey JRN-ORG-005 — Suspend Membership

### Primary Actor

Organization Administrator or Security Administrator

### Standard Flow

1. User selects membership.
2. User records suspension reason.
3. System suspends organization access.
4. Active sessions are revalidated or revoked.
5. Assignments become inaccessible.
6. Audit event is created.

## 96. Journey JRN-ORG-006 — Approve Organization Relationship

### Primary Actors

Authorized Users from Participating Organizations

### Standard Flow

1. Source organization proposes relationship.
2. Relationship type and scope are defined.
3. Target organization reviews.
4. Required approvals occur.
5. Relationship becomes active.
6. Resource assignments may then be created.

### Critical Rule

Relationship activation alone does not grant unrestricted record access.

## 97. Journey JRN-ORG-007 — Transfer Organization Ownership

### Primary Actors

Current Organization Owner
New Organization Owner
Platform or Secondary Approver where required

### Standard Flow

1. Current owner initiates transfer.
2. System validates identity and step-up authentication.
3. New owner accepts.
4. Required approval occurs.
5. Ownership role changes transactionally.
6. Prior owner role updates according to policy.
7. Audit and notifications are generated.

# Part XIV — Property Administration Journeys

## 98. Journey JRN-PROP-001 — Create Property Hierarchy

### Primary Actor

Property Owner Administrator or Property Operator

### Standard Flow

1. User creates property.
2. User creates community.
3. User creates building.
4. User creates block.
5. User optionally creates floors.
6. User creates units.
7. System validates hierarchy and organization consistency.
8. Records remain operationally labeled until public names are approved.

### Canonical Hierarchy

```text
Organization
→ Property
→ Community
→ Building
→ Block
→ Floor
→ Unit
```

## 99. Journey JRN-PROP-002 — Record Pilot Property

### Primary Actor

Property Owner Administrator

### Confirmed Pilot Structure

```text
Property
└── Community 001
    ├── Existing Building
    │   ├── Block A
    │   │   └── Six confirmed residential units
    │   └── Block B
    ├── Functional Masjid
    └── Building Under Construction
        └── Waterside context
```

### Pending Facts

The workflow must permit the following to remain unknown or provisional:

* Public community name
* Public building names
* Block B unit count
* Second-building inventory
* Floor structure
* Unit numbering
* Exact water-feature classification

### Standard Flow

1. User creates records using neutral operational labels.
2. Confirmed Block A unit count is recorded.
3. Unknown values remain null or pending.
4. No placeholder units are created for Block B.
5. Public names may be added later without changing permanent identifiers.

## 100. Journey JRN-PROP-003 — Change Unit Readiness

### Primary Actor

Property Operator

### Standard Flow

1. User opens unit.
2. User selects readiness action.
3. System validates outstanding maintenance, occupancy, and agreement conditions.
4. Unit readiness status changes.
5. Audit event is created.
6. Listing eligibility updates.

## 101. Journey JRN-PROP-004 — Retire Unit

### Primary Actor

Property Owner Administrator

### Preconditions

* No incompatible active occupancy exists.
* No active listing remains.
* Historical records can be preserved.

### Standard Flow

1. User requests retirement.
2. System displays dependencies.
3. User confirms reason.
4. Unit status becomes retired.
5. Historical agreements, occupancy, payments, and maintenance remain accessible.

# Part XV — Legal Journeys

## 102. Journey JRN-LEG-001 — Create Legal Matter

### Primary Actor

Authorized Property or Legal User

### Standard Flow

1. User creates legal matter.
2. User selects matter type.
3. User links relevant resource.
4. User sets confidentiality level.
5. Matter is assigned to legal organization or lawyer.
6. Assigned lawyer receives notification.

## 103. Journey JRN-LEG-002 — Lawyer Reviews Matter

### Primary Actor

Assigned Estate Lawyer

### Standard Flow

1. Lawyer opens assigned matter.
2. System presents authorized supporting records.
3. Lawyer records notes and recommendations.
4. Lawyer may draft notice.
5. Lawyer updates matter status.
6. Property organization receives approved update.

## 104. Journey JRN-LEG-003 — Approve and Issue Legal Notice

### Actors

Estate Lawyer
Authorized Property Approver

### Standard Flow

1. Lawyer creates notice draft.
2. Property approver reviews.
3. Required corrections are made.
4. Approver approves notice.
5. Notice is issued.
6. Delivery status is tracked.
7. Audit event is created.

# Part XVI — Document Journeys

## 105. Journey JRN-DOC-001 — Upload Controlled Document

### Primary Actor

Authorized User

### Standard Flow

1. User selects document type.
2. System validates authorization.
3. User uploads file.
4. File validation and malware scanning occur.
5. Document metadata is created.
6. Document is linked to resource.
7. Review occurs where required.
8. Access follows sensitivity rules.

## 106. Journey JRN-DOC-002 — Review Document

### Primary Actor

Document Reviewer or Domain Reviewer

### Standard Flow

1. Reviewer opens pending document.
2. System displays approved metadata and file.
3. Reviewer verifies type, readability, validity, and context.
4. Reviewer approves, rejects, or requests replacement.
5. Status updates.
6. Uploader receives notice.

## 107. Journey JRN-DOC-003 — Replace Document Version

### Primary Actor

Authorized User

### Standard Flow

1. User selects Replace.
2. System uploads new version.
3. Prior version remains historically linked.
4. Review repeats where required.
5. Current version changes only after approval where required.

## 108. Journey JRN-DOC-004 — Access Sensitive Document

### Primary Actor

Authorized User

### Preconditions

* User has sensitive-document permission.
* User is within scope or assignment.
* Related record access is valid.

### Standard Flow

1. User requests file.
2. Server authorizes access.
3. Short-lived access is generated.
4. Access event is recorded where required.
5. User views or downloads file.

# Part XVII — Communication and Notification Journeys

## 109. Journey JRN-COM-001 — Send Workflow Notification

### Primary Actor

System

### Trigger

Approved domain event.

### Standard Flow

1. Domain event is recorded.
2. Recipient is resolved.
3. Template is selected.
4. Channel is selected.
5. Notification is queued.
6. Provider adapter sends.
7. Delivery status is recorded.
8. Retry occurs where appropriate.

### Failure Rule

Notification failure must not reverse the completed business action.

## 110. Journey JRN-COM-002 — Send Manual Communication

### Primary Actor

Authorized Organization User

### Standard Flow

1. User opens related record.
2. User selects communication action.
3. Approved recipient and context are resolved.
4. User drafts message.
5. Approval occurs where required.
6. Message is sent.
7. Communication history is recorded.

## 111. Journey JRN-COM-003 — Retry Failed Notification

### Primary Actor

Authorized Operations User or Background Process

### Standard Flow

1. Failed notification is identified.
2. Retry eligibility is evaluated.
3. Retry is queued.
4. Attempt count updates.
5. Success or final failure is recorded.

## 112. Journey JRN-COM-004 — Manage Notification Preferences

### Primary Actor

User

### Standard Flow

1. User opens preferences.
2. User selects optional communication channels.
3. System preserves mandatory legal, security, and operational notices.
4. Preferences are saved.
5. Future eligible notifications follow preferences.

# Part XVIII — AI-Assisted Journeys

## 113. Journey JRN-AI-001 — Generate Application Summary

### Primary Actor

Application Reviewer

### Preconditions

* Reviewer has access to the application.
* AI feature is enabled.
* Approved provider and policy are active.

### Standard Flow

1. Reviewer requests summary.
2. System retrieves authorized application data.
3. Sensitive data is minimized.
4. AI provider receives approved context.
5. Output is returned.
6. Output is labeled AI-generated.
7. Reviewer validates output.
8. AI activity is recorded.

### Failure Rule

Application review remains available without AI.

## 114. Journey JRN-AI-002 — Classify Maintenance Request

### Primary Actor

Maintenance Coordinator

### Standard Flow

1. User requests or receives suggested classification.
2. AI proposes category and priority.
3. User reviews suggestion.
4. User confirms or changes classification.
5. Human-selected result becomes authoritative.

## 115. Journey JRN-AI-003 — Draft Communication

### Primary Actor

Authorized User

### Standard Flow

1. User selects Generate Draft.
2. System provides approved context.
3. AI produces draft.
4. Draft is labeled.
5. User edits and approves.
6. Only the human-approved version is sent.

## 116. Journey JRN-AI-004 — AI Provider Failure

### Standard Flow

1. Provider call fails or times out.
2. System records failure.
3. User receives clear message.
4. Manual workflow remains available.
5. No business state changes automatically.

# Part XIX — Platform Support Journeys

## 117. Journey JRN-SUP-001 — Create Support Case

### Primary Actor

Platform Support Agent

### Standard Flow

1. Support Agent records customer issue.
2. Case is linked to user or organization.
3. Support-safe metadata is reviewed.
4. Agent determines whether temporary access is required.
5. Case status and priority are recorded.

## 118. Journey JRN-SUP-002 — Request Temporary Support Access

### Primary Actor

Platform Support Agent

### Supporting Actor

Authorized Approver

### Standard Flow

1. Agent requests access.
2. Request includes organization, scope, purpose, and duration.
3. Approver reviews.
4. Approved support session is created.
5. Session becomes active for defined scope.
6. Customer visibility is provided where policy requires.
7. All support actions are audited.

## 119. Journey JRN-SUP-003 — End Support Session

### Primary Actor

Support Agent, Approver, or System

### Standard Flow

1. Session reaches expiry or is manually ended.
2. Access is revoked.
3. Cached authorization is invalidated.
4. Session summary is recorded.
5. Post-session review occurs where required.

## 120. Journey JRN-SUP-004 — Emergency Access

### Primary Actor

Platform Security Administrator

### Preconditions

* Approved emergency condition exists.
* Strong authentication is completed.
* Scope and duration are defined.

### Standard Flow

1. Emergency access is requested.
2. Named authority approves where feasible.
3. Session becomes active.
4. All actions are logged.
5. Session expires automatically.
6. Post-event review is mandatory.

# Part XX — Failure, Recovery, and Exception Journeys

## 121. Journey JRN-ERR-001 — User Loses Network During Draft

### Standard Flow

1. Network interruption occurs.
2. Interface preserves unsent draft locally or through last confirmed server save.
3. User receives offline or retry status.
4. System avoids duplicate submission.
5. User resumes when connection returns.

## 122. Journey JRN-ERR-002 — Duplicate Submission

### Standard Flow

1. User submits an action.
2. Retry or duplicate request occurs.
3. System evaluates idempotency key or current state.
4. Existing result is returned or duplicate is rejected.
5. No duplicate business event is created.

## 123. Journey JRN-ERR-003 — External Payment Webhook Arrives Twice

### Standard Flow

1. Provider event is received.
2. System checks provider event identifier.
3. First event is processed.
4. Duplicate event is marked duplicate.
5. Payment is not verified or created twice.

## 124. Journey JRN-ERR-004 — Notification Provider Unavailable

### Standard Flow

1. Notification send fails.
2. Business state remains authoritative.
3. Failure is recorded.
4. Retry occurs according to policy.
5. User may still access the in-app record.

## 125. Journey JRN-ERR-005 — File Upload Interrupted

### Standard Flow

1. Upload fails.
2. Incomplete file is not treated as valid.
3. Requirement remains incomplete or pending.
4. User receives retry instruction.
5. Valid prior version remains unchanged.

## 126. Journey JRN-ERR-006 — Concurrent Unit Reservation

### Standard Flow

1. Two operations attempt incompatible unit reservation.
2. Transaction or constraint permits one.
3. Other operation receives conflict response.
4. No double reservation occurs.
5. User is shown current unit availability.

## 127. Journey JRN-ERR-007 — Unauthorized Direct API Request

### Standard Flow

1. User calls protected endpoint directly.
2. Server authenticates and authorizes.
3. Access is denied.
4. No sensitive record is returned.
5. Denial is logged where appropriate.

## 128. Journey JRN-ERR-008 — Privileged User Attempts Self-Approval

### Standard Flow

1. User prepares high-risk action.
2. Same user attempts approval.
3. System evaluates separation policy.
4. Action is denied or routed to secondary approver.
5. Attempt is recorded.

## 129. Journey JRN-ERR-009 — Assignment Expires Mid-Workflow

### Standard Flow

1. Assignment expires or is revoked.
2. User loses operational access.
3. Unsaved protected actions are rejected.
4. Historical contributions remain attributed.
5. Record returns to reassignment queue.

## 130. Journey JRN-ERR-010 — Organization Suspended

### Standard Flow

1. Organization status becomes suspended.
2. New operational actions are blocked according to policy.
3. Public listings may be paused.
4. Customer access is limited safely.
5. Platform support and security workflows remain available.
6. Historical data is preserved.

# Part XXI — Workflow Notifications

## 131. Notification Trigger Catalogue

Notifications may be triggered by:

* Inquiry created
* Inquiry assigned
* Inspection requested
* Inspection confirmed
* Inspection rescheduled
* Application submitted
* Information requested
* Application decision recorded
* Offer issued
* Offer expiring
* Payment evidence submitted
* Payment verified
* Payment rejected
* Payment allocated
* Receipt issued
* Agreement issued
* Signature requested
* Agreement activated
* Move-in scheduled
* Occupancy activated
* Maintenance request submitted
* Work order assigned
* Work completed
* Maintenance closed
* Renewal case created
* Renewal offer issued
* Agreement expiring
* Move-out initiated
* Final inspection scheduled
* Deposit decision issued
* Refund executed
* Role assigned
* Role revoked
* Support session started
* Support session ended

## 132. Notification Requirements

Notifications must:

* Identify the relevant action
* Avoid unnecessary sensitive information
* Include a secure direct link where appropriate
* Use approved templates
* Record delivery status
* Support retry
* Respect channel preferences where optional
* Preserve mandatory legal and security notices

# Part XXII — Workflow Audit Events

## 133. Workflow Audit Catalogue

Material audit events include:

```text
InquiryCreated
InquiryAssigned
InspectionConfirmed
ApplicationCreated
ApplicationSubmitted
ApplicationInformationRequested
ApplicationReviewed
ApplicationApproved
ApplicationRejected
OfferCreated
OfferApproved
OfferIssued
OfferAccepted
OfferDeclined
ChargeCreated
ChargeApproved
PaymentRecorded
PaymentVerified
PaymentRejected
PaymentAllocated
PaymentReversed
RefundApproved
RefundExecuted
ReceiptIssued
AgreementCreated
AgreementApproved
AgreementIssued
AgreementSigned
AgreementActivated
AgreementAmended
AgreementTerminated
MoveInCompleted
OccupancyActivated
HouseholdChanged
MaintenanceRequestCreated
MaintenanceTriaged
WorkOrderCreated
VendorAssigned
WorkCompleted
WorkVerified
MaintenanceClosed
RenewalOfferIssued
MoveOutInitiated
DepositDecisionApproved
OccupancyEnded
RoleAssigned
RoleRevoked
SupportSessionStarted
SupportSessionEnded
AuthorizationExceptionUsed
```

## 134. Audit Event Requirements

Each material audit event should include:

* Actor
* Organization
* Action
* Resource
* Resource identifier
* Previous status
* New status
* Reason where required
* Timestamp
* Correlation identifier
* Policy or workflow version where practical

# Part XXIII — Workflow Data Requirements

## 135. Workflow Record Requirements

Each workflow record should support:

* Permanent identifier
* Business identifier where required
* Organization scope
* Current status
* Created timestamp
* Created actor
* Updated timestamp where mutable
* Assignment
* Effective dates
* State history
* Audit references
* Related resources

## 136. Workflow History Requirements

Critical workflows must preserve:

* Status history
* Decision history
* Assignment history
* Approval history
* Version history
* Notification history
* Failure and retry history
* Supporting-document links

## 137. Workflow Correlation

A correlation identifier should connect related operations.

Example:

```text
Application approval
→ Offer creation
→ Charge creation
→ Notification
→ Audit event
```

# Part XXIV — Workflow Security Requirements

## 138. Workflow Authorization

Every step must verify:

1. Authentication
2. Active account
3. Active organization
4. Active membership
5. Permission
6. Scope
7. Assignment
8. Record state
9. Approval
10. Separation of duties
11. Sensitive-field access
12. Exception status

## 139. Cross-Tenant Workflow Protection

A user must not access another organization’s workflow through:

* Direct URL
* API identifier
* Search
* Notification link
* Export
* File URL
* Background job
* Cache key
* Related-resource traversal

## 140. Workflow Session Protection

Privileged workflows may require:

* Multi-factor authentication
* Step-up authentication
* Recent authentication
* Session revalidation
* Reason entry
* Confirmation screen

# Part XXV — Workflow Accessibility and Usability

## 141. Accessibility Requirements

Workflow interfaces should support:

* Keyboard operation
* Semantic forms
* Clear labels
* Associated error messages
* Focus management
* Status announcements
* Accessible confirmation dialogs
* Sufficient contrast
* Reduced motion
* Screen-reader-compatible progress indicators

## 142. Low-Bandwidth Requirements

Workflow design should use:

* Small payloads
* Draft preservation
* Retryable uploads
* Paginated lists
* Optimized media
* Minimal unnecessary JavaScript
* Clear progress indicators
* Direct notification links
* Server-rendered core content

## 143. Mobile Requirements

Critical workflows must remain usable on mobile browsers.

These include:

* Inquiry
* Inspection request
* Application
* Document upload
* Offer response
* Payment evidence submission
* Agreement review
* Signature
* Maintenance request
* Renewal response
* Move-out initiation

# Part XXVI — Workflow Performance and Reliability

## 144. Performance Expectations

Ordinary workflow actions should target:

```text
Median API response:
Less than 500 milliseconds

95th-percentile ordinary synchronous response:
Less than 1.5 seconds
```

These targets exclude operations dependent on external providers, AI generation, large uploads, and document processing.

## 145. Long-Running Operations

Long-running operations should:

* Run asynchronously
* Show pending status
* Provide progress where practical
* Avoid duplicate execution
* Permit safe retry
* Record failure
* Notify user on completion where appropriate

Examples:

* Report generation
* Large import
* File scanning
* AI processing
* Payment reconciliation
* Bulk notification

# Part XXVII — Workflow Metrics

## 146. Prospect and Leasing Metrics

Potential metrics include:

* Listing views
* Inquiry conversion
* Inspection request rate
* Inspection attendance
* Application-start rate
* Application-submission rate
* Review duration
* Decision duration
* Offer acceptance rate
* Time from application to occupancy

## 147. Finance Metrics

Potential metrics include:

* Payment verification duration
* Rejected payment evidence rate
* Unallocated verified payments
* Outstanding charge value
* Refund-processing duration
* Reconciliation discrepancies

## 148. Maintenance Metrics

Potential metrics include:

* Time to triage
* Time to assignment
* Vendor acceptance time
* Time to completion
* Reopen rate
* Resident-reported unresolved rate

## 149. Renewal and Move-Out Metrics

Potential metrics include:

* Renewal-case lead time
* Renewal-offer response rate
* Renewal completion rate
* Move-out completion duration
* Deposit-decision duration
* Deposit dispute rate

Metrics must not replace authoritative workflow records.

# Part XXVIII — Workflow Testing Strategy

## 150. Workflow Test Levels

Testing should include:

* Unit tests
* Domain-service tests
* Authorization tests
* Database transaction tests
* Integration tests
* External-provider contract tests
* End-to-end tests
* Accessibility tests
* Performance tests
* Failure-recovery tests
* Tenant-isolation tests

## 151. Required End-to-End Scenarios

The MVP must test:

1. Public prospect to inquiry
2. Inquiry to inspection
3. Inspection to application
4. Application submission
5. Information request and response
6. Application approval
7. Offer acceptance
8. Charge creation
9. Payment evidence submission
10. Payment verification
11. Payment allocation
12. Agreement issue and signature
13. Agreement activation
14. Move-in and occupancy activation
15. Resident maintenance request
16. Vendor assignment and completion
17. Renewal acceptance
18. Move-out completion
19. Deposit decision and refund
20. Role assignment and revocation
21. Cross-organization professional assignment
22. Support-session access
23. Unauthorized cross-tenant denial
24. External-provider failure
25. Duplicate request handling

# Part XXIX — Workflow Acceptance Criteria

## 152. Acceptance Criterion — Full Leasing Journey

```text
Given:
A published listing exists for an available unit.

When:
A prospect submits an inquiry, attends an inspection, submits a complete application, receives approval, accepts an offer, satisfies charges, signs an agreement, and completes move-in.

Then:
The system creates and links the required inquiry, inspection, application, decision, offer, charge, payment, agreement, move-in, and occupancy records.

And:
Each state transition is authorized, auditable, and visible to the appropriate user.

And:
The unit becomes occupied only after required activation conditions are satisfied.
```

## 153. Acceptance Criterion — Cross-Tenant Denial

```text
Given:
User A belongs only to Organization A.
Application B belongs to Organization B.

When:
User A requests Application B through a page, API, search result, notification link, export, or document relationship.

Then:
Access is denied.
No private Application B information is returned.
```

## 154. Acceptance Criterion — Payment Evidence Is Not Verification

```text
Given:
An applicant uploads payment evidence.

When:
The upload completes.

Then:
The payment status becomes pending verification.

And:
No charge is marked paid.

And:
No agreement or occupancy activates until authorized verification and allocation occur.
```

## 155. Acceptance Criterion — Agreement Immutability

```text
Given:
An agreement version has been issued.

When:
An authorized user needs to change a material term.

Then:
The system creates a new version.

And:
The issued version remains unchanged and historically retrievable.
```

## 156. Acceptance Criterion — Occupancy Conflict

```text
Given:
Unit 001 has an active incompatible occupancy.

When:
A user attempts to activate another incompatible occupancy for Unit 001.

Then:
The system rejects the activation.

And:
No partial occupancy or unit-state change occurs.
```

## 157. Acceptance Criterion — Vendor Assignment Scope

```text
Given:
Vendor Worker A is assigned to Work Order 001 only.

When:
Vendor Worker A requests Work Order 002.

Then:
Access is denied.
No Work Order 002 data is disclosed.
```

## 158. Acceptance Criterion — Pilot Unknown Facts

```text
Given:
The Block B unit count and second-building inventory are not confirmed.

When:
The pilot property hierarchy is entered.

Then:
The unknown values remain null, pending, or provisional.

And:
The system does not create invented units or confirmed counts.
```

## 159. Acceptance Criterion — AI Failure

```text
Given:
A reviewer requests an AI-generated application summary.

When:
The AI provider is unavailable.

Then:
The reviewer can continue manual review.

And:
No application state changes automatically.

And:
The failure is recorded.
```

# Part XXX — Workflow Risks

## 160. Risk — Hidden State Changes

Risk:

A record changes without an explicit workflow action.

Controls:

* Command-based operations
* State-transition validation
* Audit events
* Restricted direct updates
* Automated tests

## 161. Risk — Workflow Bypass

Risk:

Users call APIs directly to bypass interface controls.

Controls:

* Server-side authorization
* Record-state validation
* Approval enforcement
* API tests

## 162. Risk — Cross-Tenant Handoff Error

Risk:

A professional user receives access to the wrong organization’s record.

Controls:

* Explicit relationships
* Resource assignment
* Organization validation
* Assignment-expiry controls
* Negative authorization tests

## 163. Risk — Financial False Completion

Risk:

A payment or refund is shown complete before verification or execution.

Controls:

* Separate statuses
* Provider confirmation
* Human verification
* Reconciliation
* Audit

## 164. Risk — Agreement and Occupancy Misalignment

Risk:

Occupancy becomes active without an eligible agreement.

Controls:

* Activation transaction
* Agreement validation
* Payment validation
* Unit conflict checks
* Automated end-to-end tests

## 165. Risk — Notification Dependence

Risk:

A user misses a critical action because an external message fails.

Controls:

* In-app status
* Retry
* Delivery tracking
* Escalation
* Direct dashboard visibility

## 166. Risk — Pilot Exception Becomes Permanent

Risk:

Founding-team role combinations become unrestricted production policy.

Controls:

* Canonical separation of duties
* Explicit pilot exceptions
* Audit
* Expiry
* Later access review

# Part XXXI — Unresolved Workflow Decisions

## 167. Unresolved Decisions

The following remain pending:

* Final listing-application concurrency policy
* Final unit-reservation mechanism
* Final inspection scheduling rules
* Final applicant document requirements
* Final identity-verification process
* Final guarantor requirements
* Final application-review checklist
* Final application-decision service levels
* Final offer validity period
* Final payment deadlines
* Final partial-payment policy
* Final payment-evidence requirements
* Final payment-verification sources
* Final refund approval thresholds
* Final receipt format
* Final electronic-signature provider
* Final agreement-signature sequence
* Final move-in readiness checklist
* Final household-change approval rules
* Final resident financial-access policy
* Final Co-Resident permissions
* Final maintenance priority definitions
* Final emergency-maintenance escalation
* Final vendor quote thresholds
* Final renewal review period
* Final renewal notice schedule
* Final move-out notice rules
* Final deposit-deduction rules
* Final deposit-dispute process
* Final former-resident access period
* Final notification providers
* Final reminder cadence
* Final support-session duration
* Final AI feature activation policy
* Final workflow service-level objectives
* Final offline-draft behavior
* Final archival timing
* Final pilot self-approval exceptions

These decisions must be resolved before the affected workflow becomes production dependent.

# Part XXXII — Workflow Traceability

## 168. Traceability Model

Each workflow should map to:

* User role
* User goal
* Business rule
* Functional requirement
* Permission
* Domain entity
* Database table
* API operation
* Interface screen
* Notification
* Audit event
* Automated test
* Retention policy

### Example

```text
Workflow:
JRN-FIN-005 — Verify Payment

Role:
Finance Reviewer

Permissions:
payment.read
payment.review_evidence
payment.verify

Domain Records:
Payment
Payment Evidence

Data Tables:
payments
payment_evidence
audit_events

Audit Event:
PaymentVerified

Tests:
Authorized verification
Unauthorized verification
Cross-tenant denial
Duplicate-reference prevention
Self-verification restriction
```

# Part XXXIII — MVP Workflow Scope

## 169. MVP Public and Prospect Scope

The MVP must support:

* Public listing search
* Listing details
* Prospect registration
* Inquiry
* Agent assignment
* Inspection request
* Inspection confirmation
* Inspection outcome

## 170. MVP Application Scope

The MVP must support:

* Draft application
* Applicant parties
* Household members
* Document requirements
* Submission
* Review assignment
* Information request
* Review recommendation
* Decision
* Applicant status visibility

## 171. MVP Offer and Finance Scope

The MVP must support:

* Offer creation
* Offer approval
* Offer issue
* Offer acceptance or decline
* Charge creation
* Payment evidence
* Payment verification
* Payment allocation
* Receipt
* Reversal and refund controls

## 172. MVP Agreement and Occupancy Scope

The MVP must support:

* Agreement template
* Agreement draft
* Review
* Approval
* Issue
* Signature
* Activation
* Move-in
* Occupancy activation
* Household records
* Renewal
* Move-out

## 173. MVP Maintenance Scope

The MVP must support:

* Resident maintenance request
* Triage
* Work order
* Vendor assignment
* Quote
* Cost approval
* Work status
* Completion evidence
* Verification
* Closure
* Reopen request

## 174. MVP Administration Scope

The MVP must support:

* Organizations
* Memberships
* Roles
* Assignments
* Property hierarchy
* Configuration
* Documents
* Notifications
* Audit events
* Support access

# Part XXXIV — Deferred Workflow Capabilities

## 175. Deferred Workflows

The following are deferred unless separately approved:

* Property sales
* Mortgage workflows
* Rent financing
* Insurance claims
* Investment syndication
* Public vendor marketplace
* Smart-building automation
* Biometric access
* Government-record automation
* Multi-currency settlement
* Automated applicant scoring
* Autonomous approval agents
* Fully automated legal decisions
* Enterprise identity federation
* Advanced dispute-resolution portal
* Public property-owner marketplace
* Dynamic rent-auction workflows

Deferred workflows must not distort MVP design.

# Part XXXV — Workflow Governance

## 176. Workflow Change Governance

Changes to an approved workflow require:

1. Change description
2. Business rationale
3. Affected actors
4. Affected states
5. Affected permissions
6. Tenant-isolation impact
7. Financial impact
8. Agreement impact
9. Occupancy impact
10. Privacy impact
11. Security impact
12. Legal impact
13. Notification impact
14. Audit impact
15. Data impact
16. API impact
17. Interface impact
18. Migration impact
19. Test impact
20. Rollback or recovery strategy
21. Product Visionary approval
22. Chief Product Architect approval
23. Effective version

## 177. Current User Journey and Workflow Decision

Habitacores will implement explicit, role-aware, organization-scoped workflows covering the complete Nigerian residential rental lifecycle.

The principal journey is:

```text
Public Discovery
→ Inquiry
→ Inspection
→ Application
→ Review
→ Decision
→ Offer
→ Charge
→ Payment
→ Verification
→ Allocation
→ Agreement
→ Signature
→ Activation
→ Move-In
→ Occupancy
→ Renewal or Move-Out
```

The operating workflow is:

```text
Role
→ Permission
→ Scope
→ Assignment
→ Valid Record State
→ Approval
→ Authorized Action
→ State Transition
→ Notification
→ Audit Event
```

The workflow architecture must preserve:

* Organization isolation
* Server-side authorization
* Clear role handoffs
* Assignment-specific access
* Explicit state transitions
* Financial correctness
* Payment verification
* Immutable agreement history
* Occupancy integrity
* Human accountability
* Resident privacy
* Secure document handling
* Reliable notification
* Auditability
* Failure recovery
* Low-bandwidth usability
* Mobile accessibility
* Future SaaS extensibility

No user-interface shortcut, provider callback, AI output, client-supplied status, uploaded document, payment screenshot, role title, pilot-team relationship, or operational urgency may independently bypass an approved Habitacores workflow control.