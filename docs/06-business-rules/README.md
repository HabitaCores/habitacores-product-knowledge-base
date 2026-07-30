# Document 06 — Business Rules

## Document Control

| Field | Value |
|---|---|
| Product | Habitacores |
| Document | Business Rules |
| Version | 0.1 |
| Status | Draft for approval |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Market | Nigeria |
| Last Updated | 30 July 2026 |

## 1. Purpose

This document defines the canonical business rules for Habitacores.

These rules govern:

- Organizations and memberships
- Property and community registration
- Buildings, blocks, floors, and units
- Development and construction status
- Amenities and facilities
- Listing publication
- Prospect inquiries
- Inspections
- Applications
- Applicant review and decisions
- Offers
- Annual rent and other charges
- Payments and allocations
- Tenancy agreements
- Occupancy
- Move-in and move-out
- Resident operations
- Maintenance
- Work orders
- Vendors
- Legal matters
- Documents
- Notifications
- Renewals
- Audit events
- Exceptions
- Approvals
- Data retention
- Multi-organization isolation

These rules define expected business behaviour.

They do not prescribe the final technical implementation.

## 2. Rule Classification

Every business rule should be classified using one of the following categories.

### 2.1 Mandatory Rule

A rule that must always be enforced unless an approved exception exists.

Example:

> A payment allocation must not exceed the unallocated amount of the payment.

### 2.2 Configurable Rule

A rule whose value may differ by organization, property, community, jurisdiction, or operating policy.

Example:

> The number of days before lease expiry when renewal reminders begin.

### 2.3 Conditional Rule

A rule that applies only when specified conditions are true.

Example:

> Legal review is required before an agreement is issued when the organization has enabled mandatory legal review.

### 2.4 Advisory Rule

A rule that produces a warning, recommendation, or operational alert without automatically blocking an action.

Example:

> Warn an operator when an inspection is scheduled outside normal property-access hours.

### 2.5 Prohibited Action

An action that the platform must prevent.

Example:

> An estate agent must not make the final applicant decision unless separately assigned an authorized approval role.

## 3. Rule Governance

Each material business rule should eventually have:

- Rule identifier
- Rule name
- Rule category
- Business rationale
- Scope
- Trigger
- Conditions
- Expected action
- Exception policy
- Approval authority
- Audit requirement
- Test requirement
- Effective date
- Version

Business-rule changes must follow the Product Knowledge Base governance process.

## 4. Rule Identifier Convention

Business rules should use stable identifiers.

Suggested format:

```text
BR-[DOMAIN]-[NUMBER]
Examples:

BR-ORG-001
BR-PROP-001
BR-LIST-001
BR-INSP-001
BR-APP-001
BR-PAY-001
BR-AGR-001
BR-OCC-001
BR-MNT-001
BR-AUD-001

Identifiers must not be reused after a rule is retired.

5. General Platform Rules
BR-GEN-001 — Organization Scope Required

Every organization-owned business record must belong to or inherit a valid organization scope.

Applies to:

Properties
Communities
Buildings
Blocks
Units
Listings
Applications
Charges
Payments
Agreements
Occupancies
Maintenance requests
Documents
Notifications
Audit events

A record must not exist without an approved organization context unless it is explicitly platform-global.

BR-GEN-002 — Cross-Organization Access Requires Authority

A user must not access records belonging to another organization unless one of the following applies:

The user has an active membership in that organization.
The user has an approved cross-organization assignment.
The organizations have a valid relationship supporting the access.
The user has authorized platform support access.
The user has an approved platform administrative role.

All privileged cross-organization access must be audited.

BR-GEN-003 — Permanent Identifiers Must Not Depend on Public Names

Permanent system identifiers must not change when:

Community names change
Building names change
Unit display names change
Marketing labels change
Ownership display names change
Public branding changes
BR-GEN-004 — Unconfirmed Facts Must Remain Unconfirmed

The system must not convert missing or provisional information into confirmed data.

Examples include:

Block B unit count
Building Under Construction unit inventory
Floor structure
Final public building names
Exact classification of the waterside feature

Pending information must be represented explicitly as pending, unknown, or not yet recorded.

BR-GEN-005 — Historical Records Must Remain Traceable

Critical historical records must not be silently overwritten.

This includes:

Signed agreements
Application decisions
Payment verification
Charge changes
Role assignments
Listing publication history
Occupancy history
Maintenance closure
Legal outcomes
BR-GEN-006 — Critical Status Changes Require Audit Events

A status change must generate an audit event when it affects:

Access
Legal rights
Financial obligations
Occupancy
Property availability
Applicant decisions
Agreement validity
Payment verification
Vendor authorization
Document approval
BR-GEN-007 — No Silent Success

The system must not report success when the requested action:

Failed
Was only partially completed
Is pending external confirmation
Requires approval
Was queued but not processed
Was rejected by an integration
BR-GEN-008 — Business Time Zone Required

Each organization must have an approved operational time zone.

All deadlines, reminders, inspections, and agreement dates must use the applicable organization or property time zone.

The initial market time zone will normally be West Africa Time where applicable.

6. Organization Rules
BR-ORG-001 — Organization Creation

An organization must include:

Name
Organization type
Country
Primary contact
Primary currency
Time zone
Status
Creating user
Creation timestamp
BR-ORG-002 — Organization Activation

An organization may become active only after:

Required profile information is complete.
The creating user has accepted applicable terms.
Required ownership or authority checks are completed where configured.
At least one active Organization Owner or Organization Administrator exists.
BR-ORG-003 — Organization Type Does Not Automatically Grant Permissions

An organization classified as a legal firm, estate agency, vendor, or property owner must not automatically receive access to property records.

Access requires:

Membership
Relationship
Assignment
Scope
Permission
BR-ORG-004 — Organization Suspension

Suspension must prevent new operational activity while preserving historical records.

Suspension must not delete:

Agreements
Payments
Audit events
Documents
Occupancy history
Legal matters
BR-ORG-005 — Organization Closure

An organization must not be closed while unresolved obligations remain unless an approved closure process handles them.

Potential unresolved obligations include:

Active agreements
Open payments
Active residents
Open maintenance requests
Pending legal matters
Active subscriptions
Unresolved data-export requests
7. Membership and Role Rules
BR-ROLE-001 — Active Membership Required

An organization role is valid only when the associated membership is active.

BR-ROLE-002 — Invitation Does Not Grant Access

An invitation must not grant access before:

The intended user accepts it.
Required approval is completed.
The role becomes active.
The assignment scope is established.
BR-ROLE-003 — Role Scope Must Be Enforced

A scoped role must not access resources outside its scope.

Examples:

A Building-scoped operator must not access unrelated buildings.
A Unit-scoped agent must not access unrelated listings.
A Matter-scoped lawyer must not access unrelated legal matters.
A Work-Order-scoped vendor worker must not access unrelated work orders.
BR-ROLE-004 — Privileged Role Assignment Requires Approval

Privileged roles require assignment by an authorized approver.

Privileged roles include:

Platform Administrator
Organization Owner
Organization Administrator
Property Owner Administrator
Finance Reviewer with verification authority
Security Administrator
Role Administrator
BR-ROLE-005 — Role Elevation Must Be Audited

Any increase in access scope or privilege must record:

Previous role
New role
Previous scope
New scope
Approver
Reason
Effective date
Audit event
BR-ROLE-006 — Former Members Lose Operational Access

A revoked, suspended, or expired membership must not retain operational access.

Historical attribution must remain intact.

BR-ROLE-007 — Separation of Duties May Be Required

Organizations may configure high-risk actions to require different users for:

Preparation
Review
Approval
Execution

The founding-team pilot may allow controlled combinations, but the system must record who performed each action.

8. Property Rules
BR-PROP-001 — Property Registration

A property must include:

Owning or controlling organization
Operational organization where applicable
Property name or provisional label
Address or location information
Status
Authority or ownership status
Creating user
BR-PROP-002 — Property Authority Required

A property must not be represented as fully verified unless the organization has completed the approved property-authority verification process.

BR-PROP-003 — Property Verification Labels Must Be Accurate

Verification labels must correspond to completed checks.

The platform must not display:

Verified ownership
Verified property
Verified listing
Verified organization

without a recorded verification action.

BR-PROP-004 — Property Archival

A property may be archived only when:

No active agreement requires ordinary access.
No active occupancy depends on the property.
Required records have been retained.
Archival authority is confirmed.

Archival must not delete historical records.

9. Community Rules
BR-COM-001 — Community Must Belong to a Property Context

A community must reference a valid property or approved operating context.

BR-COM-002 — Provisional Community Names Permitted

A community may use a provisional operational label before public branding is approved.

Example:

Community 001
BR-COM-003 — Public Name Approval

A public community name should be approved before:

Public listing publication
Permanent signage
Tenancy agreement issuance
Marketing campaigns
Public map registration
Resident onboarding at scale
BR-COM-004 — Partially Operational Communities Supported

A community may be partially operational while additional buildings remain under construction.

Operational buildings must remain distinguishable from construction areas.

BR-COM-005 — Community Rules Must Be Versioned

Community rules affecting residents must include:

Version
Effective date
Approving authority
Publication status
Change history
10. Building Rules
BR-BLD-001 — Building Must Belong to a Community

A building must reference a valid community.

BR-BLD-002 — Building Status Must Reflect Reality

A building status must not be changed to completed or operational without authorized evidence.

BR-BLD-003 — Construction and Occupancy Must Be Separated

A building under construction must not be available for residential occupancy unless the relevant portion has completed the approved readiness process.

BR-BLD-004 — Building Public Name Optional During Development

A building may use a provisional label such as:

Existing Building
Building Under Construction

before final branding approval.

BR-BLD-005 — Waterside Description Must Remain Accurate

Marketing and operational records must not classify the water feature as a lake, river, lagoon, creek, canal, or other category until confirmed.

The provisional term is:

Waterside context
11. Block Rules
BR-BLK-001 — Block Must Belong to a Building

Every block must reference a valid building.

BR-BLK-002 — Block Code Must Be Unique Within the Building

A building must not contain two active blocks with the same operational code.

BR-BLK-003 — Confirmed Unit Counts Only

A block’s confirmed unit count must reflect actual approved inventory.

For the pilot:

Block A has six confirmed residential units.
Block B unit count remains pending.
BR-BLK-004 — Block Status Controls Operational Use

A block that is under construction, closed, or unsafe must not expose its units as available unless an approved exception exists.

12. Floor Rules
BR-FLR-001 — Floor Records Are Optional Until Confirmed

A unit may be recorded without a floor reference when floor structure has not yet been confirmed.

BR-FLR-002 — Floor Numbering Must Be Consistent

Once a floor-numbering convention is approved for a building, all units in that building must follow it.

BR-FLR-003 — Floor Renumbering Requires Migration Review

Changing floor numbering after operational use requires review of:

Unit codes
Listings
Agreements
Signage
Resident records
Payment references
Audit history
13. Unit Rules
BR-UNIT-001 — Unit Must Belong to a Valid Physical Structure

A unit must reference:

Community
Building
Block
Floor where applicable
BR-UNIT-002 — Unit Code Uniqueness

A unit operational code must be unique within the approved property or community scope.

BR-UNIT-003 — Unit Identifier Layers

Each unit should support:

Permanent system identifier
Operational unit code
Optional public display name
BR-UNIT-004 — Unit Availability Requires Readiness

A unit may become available only when:

Construction or finishing is complete.
The unit has passed required inspection.
Required utilities or services are available.
The owner or operator approves availability.
No conflicting active occupancy exists.
No blocking maintenance issue exists.
BR-UNIT-005 — Occupancy and Operational Status Must Remain Separate

A unit’s operational status must not be inferred solely from occupancy status.

Examples:

A vacant unit may be unavailable because of maintenance.
An occupied unit may have an active maintenance condition.
A reserved unit may not yet have active occupancy.
BR-UNIT-006 — No Conflicting Active Occupancies

A unit must not have multiple incompatible active occupancies unless the occupancy model explicitly permits shared or subdivided occupancy.

BR-UNIT-007 — Unit Retirement Preserves History

A retired unit must remain associated with:

Historical agreements
Payments
Occupancies
Maintenance records
Documents
Audit events
14. Amenity and Facility Rules
BR-AMN-001 — Amenity Status Must Be Accurate

An amenity must not be advertised as available when it is unavailable, unsafe, closed, or under maintenance.

BR-AMN-002 — Masjid Representation

The functional masjid must be represented as:

A facility
A community amenity
Operational when available
Subject to maintenance and access rules
BR-AMN-003 — No Religious Inference

The system must not infer or classify a user’s religion from:

Use of the masjid
Interest in masjid announcements
Location near the masjid
Participation in community activities
BR-AMN-004 — Amenity Access Rules

Where access limitations exist, the amenity record must define:

Eligible users
Operating hours
Restrictions
Responsible operator
Temporary closure process
15. Listing Rules
BR-LIST-001 — Listing Must Reference an Authoritative Unit

A unit listing must reference a valid unit record.

BR-LIST-002 — Listing Publication Requires Approval

A listing must not be publicly published until an authorized user approves it.

BR-LIST-003 — Listing Requires Confirmed Commercial Information

Before publication, the listing must contain or explicitly state:

Annual rent
Applicable charges
Refundable amounts
Non-refundable amounts
Availability
Unit description
Location context
Contact or inquiry method
BR-LIST-004 — No Hidden Mandatory Charges

Any mandatory charge known at publication time must be disclosed.

BR-LIST-005 — Listing Accuracy

Listings must not include:

Unapproved amenities
Unconfirmed water-feature classifications
Incorrect unit details
False verification claims
Misleading availability
Unsupported photographs
Unapproved pricing
BR-LIST-006 — Listing Media Authority

Media must be:

Owned by the publishing organization
Authorized for use
Accurate to the listed property
Free from prohibited personal information
BR-LIST-007 — Primary Active Listing

Only one primary active listing should normally exist for the same unit and leasing cycle.

Additional marketing channels may reference the primary listing.

BR-LIST-008 — Listing Reservation

A reserved listing must not automatically imply:

Payment received
Application approved
Agreement signed
Occupancy active
BR-LIST-009 — Listing Closure

A listing should close or pause when:

The unit becomes leased.
The unit becomes unavailable.
The owner withdraws it.
Information becomes materially inaccurate.
The listing expires.
A blocking maintenance issue exists.
16. Prospect and Inquiry Rules
BR-INQ-001 — Inquiry Requires Contact Path

An inquiry must contain an approved communication method.

BR-INQ-002 — Duplicate Inquiry Handling

The system should identify probable duplicate inquiries without automatically deleting them.

BR-INQ-003 — Assignment Required for Managed Follow-Up

A managed inquiry must be assigned to an authorized agent or operator.

BR-INQ-004 — Prospect Privacy

Prospect contact information must be visible only to authorized users with a legitimate assignment.

BR-INQ-005 — Inquiry Does Not Guarantee Availability

An inquiry must not reserve a unit unless the approved reservation workflow is completed.

BR-INQ-006 — Inquiry Closure Reason

A closed or disqualified inquiry should include a reason where appropriate.

17. Inspection Rules
BR-INSP-001 — Inspection Must Reference a Valid Context

An inspection must reference:

Prospect
Unit or listing
Date and time
Assigned agent or operator
Status
BR-INSP-002 — Inspection Confirmation

An inspection request becomes confirmed only after an authorized user or approved automated process confirms:

Property availability
Time
Access
Assigned representative
BR-INSP-003 — Inspection Rescheduling Preserves History

Rescheduling must preserve:

Original appointment
New appointment
Actor
Reason
Timestamp
BR-INSP-004 — Inspection Access Minimization

A prospect must receive only the access information necessary for the inspection.

BR-INSP-005 — Inspection Completion

An inspection may be marked completed only after the scheduled time and when attendance or completion evidence exists.

BR-INSP-006 — Inspection Does Not Equal Approval

Inspection completion must not automatically:

Approve an application
Reserve a unit
Create a payment obligation
Create an agreement
BR-INSP-007 — No-Show Status

A no-show status should require:

Confirmed appointment
Scheduled time passed
Attendance check
Authorized recorder
18. Application Rules
BR-APP-001 — Application Must Reference a Unit

A formal application must reference a valid unit or approved housing opportunity.

BR-APP-002 — Application Ownership

An applicant may create and manage their application within the allowed workflow but may not approve it.

BR-APP-003 — Minimum Required Information

The required application fields must be configurable and lawfully justified.

BR-APP-004 — Data Minimization

The application process must collect only information necessary for:

Identity
Contact
Household
Eligibility
Agreement
Payment
Regulatory obligations
BR-APP-005 — Consent Required

Where required, applicants must provide informed consent before:

Document processing
Identity verification
Background checks
Financial checks
Sharing with assigned professionals
Automated analysis
BR-APP-006 — Submission Locks Controlled Fields

After submission, material fields should not be freely editable.

Corrections must occur through:

Amendment
Information request
Resubmission
Authorized review action
BR-APP-007 — Application Review Assignment

Application review must be performed by an authorized reviewer.

BR-APP-008 — Agent Cannot Make Final Decision by Default

An estate agent must not make the final applicant decision unless separately assigned an approved decision-making role.

BR-APP-009 — Decision Requires Reason

Approval, conditional approval, rejection, or expiration should include a recorded reason.

Applicant-facing reasons may differ from internal review notes where legally appropriate.

BR-APP-010 — Decision Audit

Final decisions must record:

Decision
Decision maker
Date
Reason
Conditions where applicable
Supporting review record
BR-APP-011 — No Discriminatory Inference

The system must not infer or use protected personal characteristics from:

Names
Photos
Masjid use
Community participation
Location
Language
Household composition

unless legally required and explicitly governed.

BR-APP-012 — AI Cannot Make Final Applicant Decision

AI may summarize, classify, or flag information but must not independently issue the final applicant decision.

19. Offer Rules
BR-OFR-001 — Offer Requires Approved Application Context

An offer should reference:

Approved or conditionally approved application
Unit
Proposed tenancy period
Charge schedule
Expiration date
Approver
BR-OFR-002 — Offer Terms Must Be Complete

The offer must disclose:

Annual rent
Other charges
Refundable amounts
Non-refundable amounts
Payment deadline
Proposed agreement dates
Conditions
Expiration date
BR-OFR-003 — Offer Acceptance Does Not Equal Occupancy

Acceptance must not automatically create:

Active agreement
Verified payment
Active occupancy
Resident status
BR-OFR-004 — Offer Expiration

An expired offer must not be accepted without reissue or authorized extension.

BR-OFR-005 — Offer Changes Require New Version

Material changes after issue must create a new version or replacement offer.

20. Charge Rules
BR-CHG-001 — Charge Type Required

Every charge must have an approved charge type.

BR-CHG-002 — Charge Amount Must Be Positive or Explicit Adjustment

A standard charge amount must be greater than zero.

Negative amounts must be represented through approved adjustment, credit, refund, or reversal processes.

BR-CHG-003 — Currency Required

Every charge must include a currency.

The initial pilot will normally use Nigerian naira.

BR-CHG-004 — Charge Recipient Required

Every charge must identify the intended recipient or holding context.

BR-CHG-005 — Refundability Required

Every charge must state whether it is:

Refundable
Conditionally refundable
Non-refundable
Not applicable
BR-CHG-006 — Annual Rent Is Distinct from Other Charges

Annual rent must remain separate from:

Agency fee
Legal fee
Service charge
Deposit
Utility charge
Inspection fee
Platform fee
BR-CHG-007 — Charge Changes Require Audit

Material charge changes must record:

Previous amount
New amount
Reason
Actor
Approver where required
Timestamp
BR-CHG-008 — Paid Charge Cannot Be Silently Altered

A paid or partially paid charge must not be silently edited.

Corrections require:

Adjustment
Reversal
Credit
Replacement charge
Approved exception
21. Annual Rent Rules
BR-RENT-001 — Annual Rent Is Default Initial Model

The initial Nigerian market configuration will treat annual rent as the default rental period.

BR-RENT-002 — Lease Period Must Be Explicit

The agreement must state:

Start date
End date
Rent period
Annual rent amount
Renewal policy
BR-RENT-003 — Annual Rent Does Not Include Other Charges by Default

Other charges must be listed separately unless the approved agreement explicitly combines them.

BR-RENT-004 — Rent Changes Require Approval

A rent amount change requires authorized approval.

BR-RENT-005 — Public Listing Rent Must Match Approved Offer Data

The listing amount, offer amount, charge amount, and agreement amount must be reconcilable.

Any difference must be explained and authorized.

BR-RENT-006 — Renewal Rent May Differ

A renewal rent amount may differ from the previous term only through an approved renewal process.

22. Payment Rules
BR-PAY-001 — Payment Is Distinct from Charge

A payment records received or initiated funds.

A charge records an obligation.

BR-PAY-002 — Payment Evidence Is Not Verification

An uploaded screenshot, receipt, or transfer notification must not automatically mark a payment as verified.

BR-PAY-003 — Payment Verification Requires Authority

Only an authorized Finance Reviewer or approved payment integration may verify payment.

BR-PAY-004 — Payment Reference Required Where Available

A payment should record:

Provider reference
Bank reference
Transaction reference
Payer
Amount
Currency
Date
BR-PAY-005 — Allocation Required

A verified payment should be allocated to one or more charges before those charges are considered settled.

BR-PAY-006 — Allocation Limit

Total active allocations must not exceed the valid payment amount.

BR-PAY-007 — Charge Settlement Limit

A charge must not receive allocations beyond its outstanding amount unless an approved overpayment process exists.

BR-PAY-008 — Partial Payments

Partial payments may be supported only where the organization’s policy allows them.

BR-PAY-009 — Payment Reversal

A reversed payment must:

Reverse affected allocations
Recalculate charge balances
Create audit events
Preserve original payment history
BR-PAY-010 — Refund Approval

A refund must require:

Refund reason
Refund amount
Original payment reference
Recipient
Approver
Execution status
Audit record
BR-PAY-011 — Deposit Funds Remain Separate

Refundable deposits must remain distinguishable from rent and revenue.

BR-PAY-012 — Overpayment

Overpayments must be handled through:

Unallocated balance
Credit
Refund
Approved reallocation

They must not disappear into an unspecified account balance.

23. Agreement Rules
BR-AGR-001 — Agreement Requires Valid Parties

An agreement must identify approved parties.

BR-AGR-002 — Agreement Requires Unit

A residential tenancy agreement must reference a valid unit.

BR-AGR-003 — Agreement Requires Dates

An agreement must include:

Start date
End date
Execution date where applicable
BR-AGR-004 — Signed Versions Are Immutable

A signed agreement version must not be overwritten.

BR-AGR-005 — Material Change Requires New Version

Material changes require:

New version
Reapproval
Reissue
Resignature where applicable
BR-AGR-006 — Legal Review Configurable

An organization may require legal review before agreement issuance.

BR-AGR-007 — Agreement Activation Conditions

Agreement activation may require:

Approval
Required signatures
Required payment verification
Move-in readiness
Final authorization
BR-AGR-008 — No Conflicting Active Agreement

A unit must not have incompatible overlapping active tenancy agreements without approved exception handling.

BR-AGR-009 — Agreement Termination

Termination must record:

Reason
Authority
Effective date
Notice
Legal context
Financial consequences
Audit event
BR-AGR-010 — AI Legal Limitation

AI may assist with drafting or summarization but must not provide final legal approval.

24. Occupancy Rules
BR-OCC-001 — Occupancy Is Distinct from Agreement

An agreement may be signed before occupancy begins.

Occupancy begins only through the approved move-in process.

BR-OCC-002 — Active Occupancy Requires Valid Unit

An occupancy must reference a valid unit.

BR-OCC-003 — Active Occupancy Requires Approved Resident Context

An active occupancy must reference:

Approved resident or household
Agreement or approved exception
Start date
Unit
Status
BR-OCC-004 — Occupancy Dates Must Be Valid

The occupancy start date must precede the end date where an end date exists.

BR-OCC-005 — Occupancy Conflict Prevention

The system must prevent incompatible overlapping occupancies.

BR-OCC-006 — Resident Status Follows Occupancy Workflow

An applicant should become an active resident only after required occupancy conditions are met.

BR-OCC-007 — Former Resident Access

Former residents may retain limited access to approved historical records according to retention policy.

25. Move-In Rules
BR-MOVEIN-001 — Move-In Approval Required

Move-in requires approval from an authorized operator or owner role.

BR-MOVEIN-002 — Move-In Preconditions

Configured move-in preconditions may include:

Approved application
Accepted offer
Verified payments
Signed agreement
Unit readiness
Handover checklist
Identity confirmation
BR-MOVEIN-003 — Move-In Condition Record

The move-in process should record:

Unit condition
Meter readings where applicable
Keys or access items issued
Existing defects
Resident acknowledgement
Operator
BR-MOVEIN-004 — Access Issuance

Keys, codes, credentials, or access rights must not be issued before authorized move-in approval except for controlled inspection or maintenance access.

26. Move-Out Rules
BR-MOVEOUT-001 — Move-Out Notice

A move-out workflow should record:

Notice date
Requested move-out date
Agreement context
Reason where appropriate
Acknowledgement
BR-MOVEOUT-002 — Final Inspection

Move-out should include an approved final inspection.

BR-MOVEOUT-003 — Property Condition

Final condition findings must distinguish:

Normal wear
Existing damage
Resident-caused damage
Maintenance responsibility
Unresolved dispute
BR-MOVEOUT-004 — Deposit Decision

A deposit decision must record:

Original deposit
Deductions
Evidence
Refund amount
Approver
Dispute status
BR-MOVEOUT-005 — Unit Turnaround

After move-out, a unit should enter an appropriate status such as:

Turnaround
Maintenance
Available
Unavailable

It must not automatically become available without readiness confirmation.

27. Resident Rules
BR-RES-001 — Resident Access Is Household-Scoped

A resident must not access another household’s records.

BR-RES-002 — Primary Resident Responsibilities

The Primary Resident may receive authority to:

View agreement
View charges and payment records
Submit maintenance requests
Receive notices
Initiate renewal or move-out requests
Manage limited household information
BR-RES-003 — Co-Resident Access Configurable

Co-Resident access must follow approved organization and agreement policy.

BR-RES-004 — Minor Dependants

Minor dependants should not receive independent platform accounts by default.

BR-RES-005 — Household Changes Require Approval

Adding or removing an occupant after agreement activation may require:

Resident request
Identity information
Owner approval
Agreement amendment
Updated occupancy record
BR-RES-006 — Resident Suspension

Suspending a resident account must not silently terminate the legal agreement or occupancy.

BR-RES-007 — Resident Communications

Material notices should record:

Recipient
Channel
Delivery status
Date
Related agreement or occupancy
28. Renewal Rules
BR-REN-001 — Renewal Monitoring

The system must track agreement expiry dates.

BR-REN-002 — Renewal Reminder Schedule

Reminder timing must be configurable.

Possible reminder points include:

120 days before expiry
90 days before expiry
60 days before expiry
30 days before expiry
14 days before expiry

No final reminder schedule is approved in Version 0.1.

BR-REN-003 — Renewal Requires Owner Decision

A renewal offer must be approved by an authorized owner role.

BR-REN-004 — Renewal Terms Must Be Explicit

A renewal offer must state:

New rent
Other charges
Start date
End date
Conditions
Acceptance deadline
BR-REN-005 — Renewal Acceptance Does Not Automatically Renew Agreement

A renewed agreement becomes active only after required:

Approval
Payment
Signature
Document update
BR-REN-006 — Renewal Decline

A declined renewal should trigger the appropriate move-out or expiry workflow.

BR-REN-007 — Expired Agreement

An expired agreement must not remain marked active without an approved extension or renewal.

29. Maintenance Rules
BR-MNT-001 — Maintenance Request Requires Context

A maintenance request must reference:

Organization
Community
Building
Unit or facility
Reporter
Description
Priority
Status
BR-MNT-002 — Maintenance Priority Must Follow Policy

Priority categories must be defined and applied consistently.

BR-MNT-003 — Emergency Maintenance

Emergency maintenance may permit accelerated access and approval but must still be documented.

BR-MNT-004 — Assignment Required

A maintenance request must be assigned before work begins unless an emergency exception applies.

BR-MNT-005 — Cost Approval Thresholds

Organizations may configure thresholds requiring approval before work proceeds.

BR-MNT-006 — Completion Evidence

Completion may require:

Work notes
Photos
Parts used
Cost
Completion date
Vendor or staff confirmation
BR-MNT-007 — Closure Requires Verification

A maintenance request should not be closed before:

Work is completed.
Required evidence is present.
An authorized user verifies completion.
Resident confirmation is obtained where configured.
BR-MNT-008 — Reopening

A closed request may be reopened when:

The issue remains unresolved.
The issue recurs.
Completion evidence was inaccurate.
The resident disputes closure.
BR-MNT-009 — Resident-Caused Damage

A maintenance finding must not automatically create a resident charge without review and approval.

30. Work-Order Rules
BR-WO-001 — Work Order Requires Source

A work order must reference:

Maintenance request
Preventive maintenance plan
Inspection finding
Authorized operational task
BR-WO-002 — Work Scope Required

A work order must define the required work.

BR-WO-003 — Assigned Worker Access

Vendor or worker access must be limited to:

Assigned work order
Necessary location information
Required contact protocol
Approved schedule
BR-WO-004 — Work Order Status History

Status changes must preserve history.

BR-WO-005 — Cost Change Approval

A vendor must not materially exceed an approved cost without authorization.

BR-WO-006 — Verification Before Closure

A work order may be marked completed by the worker but should be verified before closure.

31. Vendor Rules
BR-VEN-001 — Vendor Registration

A vendor must provide required profile and contact information.

BR-VEN-002 — Vendor Verification

A vendor must not be labelled verified without the approved verification process.

BR-VEN-003 — Vendor Access Is Assignment-Specific

A vendor must not access unrelated properties, residents, or work orders.

BR-VEN-004 — Vendor Compliance

Where required, vendors must maintain current:

Licences
Insurance
Certifications
Identity records
Safety documents
BR-VEN-005 — Expired Compliance Documents

Expired required documents may suspend new assignments.

BR-VEN-006 — Vendor Suspension

Vendor suspension must prevent new assignments while preserving historical records.

BR-VEN-007 — Vendor Performance

Performance records must be based on documented events rather than unsupported subjective ratings.

32. Legal-Matter Rules
BR-LEGAL-001 — Legal Matter Assignment

A legal matter must be assigned to an authorized lawyer or legal role where professional review is required.

BR-LEGAL-002 — Matter-Scoped Access

Legal professionals must access only assigned matters and supporting records.

BR-LEGAL-003 — Legal Record Confidentiality

Legal notes, advice, and privileged documents must receive appropriate access protection.

BR-LEGAL-004 — Legal Outcome Authority

The platform must not represent AI output as a final legal conclusion.

BR-LEGAL-005 — Notice Generation

A generated notice must identify:

Matter
Parties
Effective date
Issuing authority
Delivery method
Document version
BR-LEGAL-006 — Legal-Matter Closure

A matter may close only when:

Outcome is recorded.
Required documents are stored.
Outstanding actions are resolved or transferred.
Closing authority is confirmed.
33. Document Rules
BR-DOC-001 — Document Organization Scope

Every non-public document must belong to an organization or approved platform context.

BR-DOC-002 — Document Classification

Documents should be classified by:

Type
Sensitivity
Related resource
Version
Status
Retention requirement
BR-DOC-003 — Sensitive Document Access

Sensitive documents must be accessible only to authorized roles and assignments.

BR-DOC-004 — Document Versioning

Replacing a controlled document must create a new version rather than silently overwriting the previous version.

BR-DOC-005 — Signed Document Preservation

Signed documents must remain immutable.

BR-DOC-006 — Document Expiration

Documents with expiration dates should trigger reminders or status changes.

BR-DOC-007 — Applicant Documents

Applicant documents must not remain broadly accessible after the application workflow ends.

BR-DOC-008 — Public Documents

A document must be explicitly approved before public publication.

BR-DOC-009 — Malware and File Validation

Uploaded files must be validated for:

Allowed file type
File size
Malicious content
Corruption
Upload completion
BR-DOC-010 — Government Identification

Government-issued identity documents must receive heightened access protection and must not be exposed in logs, notifications, or public URLs.

34. Communication Rules
BR-COMM-001 — Communication Context

Material communications should reference the related:

Listing
Inspection
Application
Agreement
Payment
Occupancy
Maintenance request
Legal matter
BR-COMM-002 — Authorized Sender

Organization communications must be sent by an authorized user or approved automated process.

BR-COMM-003 — Delivery Status

Where available, delivery status should be recorded.

BR-COMM-004 — WhatsApp Is Not the Authoritative Record

WhatsApp may be used for notifications and communication, but authoritative business status must remain in Habitacores.

BR-COMM-005 — Consent and Preferences

Promotional communications must respect:

Consent
Communication preferences
Applicable law
Unsubscribe requirements

Operational communications may follow separate lawful rules.

BR-COMM-006 — Sensitive Information

Notifications must not expose unnecessary:

Identity documents
Payment credentials
Legal notes
Authentication secrets
Private household information
35. Notification Rules
BR-NOT-001 — Notification Trigger Must Be Defined

Automated notifications require a documented trigger.

BR-NOT-002 — Duplicate Notification Control

The system should prevent excessive duplicate notifications.

BR-NOT-003 — Critical Notification Escalation

Critical notifications may require escalation when:

Delivery fails
Action remains overdue
Safety is involved
Payment or agreement deadlines are missed
BR-NOT-004 — Notification Does Not Change Status

Sending a notification must not independently change the underlying business status unless the workflow explicitly defines it.

36. Audit Rules
BR-AUD-001 — Audit Actor Required

Every user-initiated audit event must identify the actor.

BR-AUD-002 — System Events Identify Source

Automated actions must identify:

Service
Integration
Job
AI agent
System component
BR-AUD-003 — Audit Events Must Be Protected

Ordinary users must not edit or delete audit events.

BR-AUD-004 — Audit Before-and-After Values

Material updates should capture previous and new values where appropriate.

BR-AUD-005 — Audit Reason

High-impact actions should require a reason.

Examples:

Applicant rejection
Payment reversal
Charge waiver
Agreement termination
Role revocation
Vendor suspension
Document rejection
BR-AUD-006 — Correlation Identifier

Related technical and business events should share a correlation or request identifier where practical.

BR-AUD-007 — No Secrets in Audit Logs

Audit logs must not contain:

Passwords
Authentication tokens
Private keys
Full payment credentials
Unnecessary identity-document contents
37. Development and Construction Rules
BR-DEV-001 — Development Status Evidence

Construction status changes must be supported by authorized updates or evidence.

BR-DEV-002 — Completion Does Not Equal Operational Readiness

A completed building or block must still pass operational readiness requirements before listings or occupancy.

BR-DEV-003 — Handover Required

A development asset should transition to operations through an approved handover workflow.

BR-DEV-004 — Planned Inventory May Be Recorded

Planned buildings, blocks, and units may be represented before completion, but their status must remain distinct from operational inventory.

BR-DEV-005 — No Public Availability Before Approval

A planned or under-construction unit must not be advertised as immediately available unless the listing accurately states its future availability and has approval.

38. Data-Quality Rules
BR-DATA-001 — Required Fields

Critical records must enforce required fields before entering states that depend on them.

BR-DATA-002 — Duplicate Detection

The system should detect probable duplicates for:

Users
Organizations
Properties
Units
Listings
Applicants
Payments
Vendors

Detection must not automatically merge records without authorization.

BR-DATA-003 — Canonical Values

Common values should use approved controlled vocabularies where practical.

Examples:

Status
Charge type
Unit type
Organization type
Maintenance priority
Document type
Relationship type
BR-DATA-004 — Date Validation

Date rules must prevent impossible or contradictory sequences.

Examples:

Agreement end before start
Inspection completion before scheduling
Payment receipt before record creation without documented import context
Move-out before move-in
Renewal starting before prior agreement end without approved overlap
BR-DATA-005 — Currency Consistency

Charges, payments, allocations, and refunds must preserve currency.

Cross-currency allocation is not supported in the MVP.

BR-DATA-006 — Address Flexibility

The address model must support Nigerian addressing practices without assuming that every property has a standardized street-number format.

39. Privacy Rules
BR-PRIV-001 — Minimum Necessary Access

Users must access only the personal data needed for their role and assignment.

BR-PRIV-002 — Purpose Limitation

Personal data must be used only for approved purposes.

BR-PRIV-003 — Sensitive Data Protection

Sensitive data requires stricter protection.

Examples:

Government identification
Financial eligibility information
Legal records
Household information
Authentication information
Payment evidence
BR-PRIV-004 — Export Control

Sensitive data exports require authorization and must be audited.

BR-PRIV-005 — Deletion and Retention

Deletion requests must be balanced against:

Legal retention
Agreement records
Financial records
Fraud prevention
Audit requirements
Active disputes
BR-PRIV-006 — Religious Data

Habitacores must not infer religious identity from the presence or use of the community masjid.

40. Security Rules
BR-SEC-001 — Server-Side Authorization

Authorization must be enforced on the server, not only in the user interface.

BR-SEC-002 — Multi-Factor Authentication

Privileged roles should require multi-factor authentication.

BR-SEC-003 — Session Protection

Sensitive actions may require:

Recent authentication
Step-up verification
Reconfirmation
Secondary approval
BR-SEC-004 — Secret Management

Secrets must not be stored in:

Source code
Product documentation
Public issue descriptions
Audit logs
Client applications
BR-SEC-005 — File Access

Private files must use controlled access rather than publicly guessable URLs.

BR-SEC-006 — Rate Limiting

Public and sensitive endpoints should apply appropriate rate limits.

BR-SEC-007 — Access Revocation

Revoked roles, sessions, and assignments should lose access promptly.

41. AI Rules
BR-AI-001 — AI Output Must Be Identified

AI-generated or AI-assisted content should be identifiable where it influences decisions or records.

BR-AI-002 — Human Review for High-Impact Actions

AI must not independently finalize:

Applicant approval
Applicant rejection
Eviction
Agreement termination
Legal conclusions
Payment verification
Refund denial
Ownership verification
Vendor suspension
BR-AI-003 — AI Context Must Be Organization-Scoped

AI systems must not receive data from unrelated organizations.

BR-AI-004 — Sensitive Data Minimization

AI requests should include only the minimum required sensitive data.

BR-AI-005 — AI Auditability

Where appropriate, the system should record:

AI service or model
Time
Input context reference
Output
Human reviewer
Final action
BR-AI-006 — AI Suggestions Are Not Authoritative Records

AI output must not replace the authoritative application, payment, agreement, or property record without human approval.

42. Exception Rules
BR-EXC-001 — Exceptions Must Be Explicit

An exception must not be implemented through undocumented manual changes.

BR-EXC-002 — Exception Record

An exception should record:

Rule
Resource
Reason
Requester
Approver
Effective period
Risk
Compensating controls
Audit event
BR-EXC-003 — Temporary Exception Expiry

Temporary exceptions must have an expiration date where practical.

BR-EXC-004 — Prohibited Exceptions

No exception may authorize:

Cross-organization data leakage
Secret exposure
Silent alteration of signed agreements
Deletion of required audit history
False property-verification claims
Unlawful discrimination
Unauthorized payment diversion
43. Approval Rules
BR-APR-001 — Approval Authority Must Be Defined

Each approval process must define the required role.

BR-APR-002 — Self-Approval Restrictions

Where configured, users must not approve their own:

Privileged role elevation
High-value refund
Material financial adjustment
Vendor invoice
Application conflict
Security exception
BR-APR-003 — Approval History

Approval records must include:

Approver
Decision
Date
Reason
Conditions
Related record
BR-APR-004 — Approval Revocation

Revoking approval must not erase the original approval history.

44. MVP Configuration Decisions

The following apply to the initial MVP unless superseded by approved rules.

44.1 Market
Initial country: Nigeria
Initial currency: Nigerian naira
Initial rental model: Annual rent
44.2 Pilot Physical Context
Community 001
Existing Building
Block A
Six confirmed Block A units
Block B
Functional masjid
Building Under Construction
Waterside context
44.3 Pending Property Details

The following remain pending:

Public community name
Public building names
Block B unit count
Second-building inventory
Floor structure
Unit numbering
Exact water-feature classification
44.4 MVP User Roles

Minimum operational roles include:

Platform Administrator
Organization Owner
Organization Administrator
Property Owner Administrator
Property Operator
Finance Reviewer
Maintenance Coordinator
Estate Agent
Estate Lawyer
Vendor Administrator
Vendor Worker
Prospect
Applicant
Primary Resident
Co-Resident
Household Member
Guarantor
44.5 MVP Workflow Coverage

The MVP must support:

Property and community setup
Building, block, and unit records
Listing
Inquiry
Inspection
Application
Review
Offer
Charge
Payment
Agreement
Occupancy
Move-in
Resident access
Maintenance
Renewal tracking
Move-out
Documents
Notifications
Audit events
45. Deferred Rules

Detailed rules are deferred for:

Mortgage origination
Rent financing
Insurance underwriting
Public vendor marketplace
Government integrations
Multi-currency settlement
Advanced accounting
Automated applicant scoring
Native mobile applications
Property sales
Investment syndication
Full construction procurement

Deferred rules must not be implied by the MVP.

46. Business-Rule Test Requirements

Every mandatory or prohibited rule should have automated or documented validation.

Testing should include:

Positive case
Negative case
Unauthorized case
Cross-organization case
Invalid-state case
Duplicate case
Audit-event case
Exception case where applicable

Examples:

Rule:
BR-PAY-006 — Allocation Limit

Positive:
Allocate ₦500,000 from a ₦500,000 payment.

Negative:
Attempt to allocate ₦550,000 from a ₦500,000 payment.

Expected:
The second allocation is blocked and audited.
Rule:
BR-APP-008 — Agent Cannot Make Final Decision by Default

Positive:
Property Owner Administrator approves the application.

Negative:
Assigned Estate Agent attempts final approval without an approval role.

Expected:
The action is denied.
47. Business-Rule Conflict Resolution

When two rules conflict, use this priority:

Applicable law
Safety
Security
Privacy
Financial integrity
Legal and contractual integrity
Approved Product Knowledge Base rule
Organization configuration
User preference
Delivery convenience

A lower-priority rule must not override a higher-priority rule without formal review.

48. Unresolved Business-Rule Decisions

The following remain pending:

Exact applicant document requirements
Exact applicant review criteria
Exact inspection operating hours
Exact reservation policy
Whether partial annual-rent payments are permitted
Exact payment-verification process
Exact charge types and amounts
Exact deposit policy
Exact legal-review requirement
Exact agreement-signature method
Exact move-in checklist
Exact renewal reminder schedule
Exact rent-increase approval process
Exact maintenance approval thresholds
Exact refund approval thresholds
Exact resident household invitation policy
Exact former-resident access duration
Exact document-retention periods
Exact vendor-verification requirements
Exact public listing publication workflow
Exact support-access policy

These decisions must be resolved before the affected production functionality is activated.

49. Business-Rule Governance

Changes to business rules require:

Rule identifier
Proposed change
Business rationale
Affected workflows
Affected roles
Security review
Privacy review
Legal review where applicable
Database and API impact
Test impact
Product Visionary approval
Chief Product Architect approval
Effective date
Migration or transition plan where required
50. Current Business-Rules Decision

Habitacores will use explicit, auditable, organization-scoped business rules to govern the complete residential-property lifecycle.

The primary operational sequence is:

Property and Community Setup
→ Unit Readiness
→ Listing
→ Inquiry
→ Inspection
→ Application
→ Review
→ Offer
→ Charges
→ Payment
→ Agreement
→ Occupancy
→ Move-In
→ Resident Operations
→ Renewal or Move-Out

The business-rule framework will preserve:

Nigerian annual-rent requirements
Transparent charges
Human accountability
Multi-organization isolation
Financial separation
Historical traceability
Role-scoped authority
Physical-property accuracy
Security
Privacy
Auditability

No implementation may contradict an approved mandatory business rule without a documented and approved exception.