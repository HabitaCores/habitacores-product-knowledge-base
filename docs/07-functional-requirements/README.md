# Document 07 — Functional Requirements

## Document Control

| Field | Value |
|---|---|
| Product | Habitacores |
| Document | Functional Requirements |
| Version | 0.1 |
| Status | Draft for approval |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Market | Nigeria |
| Primary Currency | Nigerian naira |
| Last Updated | 30 July 2026 |

## 1. Purpose

This document defines the functional requirements for Habitacores.

It translates the approved:

- Product vision
- Product strategy
- Product principles
- Business architecture
- Domain model
- User and role model
- Business rules

into specific system capabilities.

The requirements in this document describe what Habitacores must enable users, organizations, administrators, professionals, residents, vendors, and system components to do.

This document does not define the complete technical architecture, database schema, API contract, or user-interface design.

Those details will be defined in later documents.

## 2. Functional Requirement Objectives

The functional requirements must ensure that Habitacores can:

1. Support the complete residential-property operating lifecycle.
2. Support Nigerian annual-rent practices.
3. Maintain accurate property, community, building, block, floor, and unit records.
4. Support prospects, applicants, residents, property operators, agents, lawyers, finance personnel, and vendors.
5. Preserve organization-level data isolation.
6. Enforce role-based and assignment-based access.
7. Separate charges, payments, allocations, deposits, refunds, and revenue.
8. Maintain immutable and traceable historical records.
9. Support human approval for high-impact actions.
10. Enable future multi-tenant SaaS expansion.
11. Support the initial founding-team pilot without hardcoding the pilot structure.
12. Provide a foundation for APIs, database entities, workflows, tests, and user-interface design.

## 3. Requirement Classification

Each requirement is classified by priority.

### 3.1 Must Have

Required for the initial MVP or for core architectural integrity.

### 3.2 Should Have

Important for usability or operational maturity but may follow the first MVP release.

### 3.3 Could Have

Useful enhancement that may be implemented later.

### 3.4 Deferred

Explicitly outside the current MVP scope.

## 4. Functional Requirement Identifier Convention

Functional requirements use the following format:

```text
FR-[DOMAIN]-[NUMBER]
Examples:

FR-AUTH-001
FR-ORG-001
FR-PROP-001
FR-LIST-001
FR-APP-001
FR-PAY-001
FR-AGR-001
FR-MNT-001

Requirement identifiers must remain stable after approval.

If a requirement is retired, its identifier must not be reused.

5. Functional Scope Overview

Habitacores must support the following major functional areas:

Authentication and identity
Organization management
Membership and role management
Property hierarchy management
Development and construction tracking
Amenities and facilities
Unit readiness and availability
Listing management
Prospect and inquiry management
Inspection management
Application management
Applicant review and decisions
Offer management
Charge management
Payment and allocation management
Agreement management
Occupancy management
Move-in management
Resident portal
Renewal management
Move-out management
Maintenance management
Work-order management
Vendor management
Legal-matter management
Document management
Communication and notification management
Reporting and dashboards
Audit and compliance
Platform administration
Search and filtering
AI-assisted functions
Data import and export
Configuration management
6. Authentication and Identity Management
FR-AUTH-001 — User Registration

Priority: Must Have

The system must allow an eligible person to create a Habitacores user account.

Registration must support:

Full name
Email address
Phone number where required
Password or approved identity-provider authentication
Acceptance of applicable terms
Verification of required contact methods
FR-AUTH-002 — Email Verification

Priority: Must Have

The system must support email-address verification before allowing access to protected functionality where required.

FR-AUTH-003 — Phone Verification

Priority: Should Have

The system should support phone-number verification using an approved verification method.

FR-AUTH-004 — User Login

Priority: Must Have

The system must allow registered users to authenticate using approved credentials.

FR-AUTH-005 — Logout

Priority: Must Have

The system must allow users to end their authenticated session.

FR-AUTH-006 — Password Reset

Priority: Must Have

The system must provide a secure password-reset process.

FR-AUTH-007 — Multi-Factor Authentication

Priority: Must Have for privileged roles

The system must support multi-factor authentication for privileged users.

FR-AUTH-008 — Session Management

Priority: Must Have

Users must be able to view and terminate active sessions where supported.

FR-AUTH-009 — Account Status

Priority: Must Have

The system must support account statuses including:

Pending verification
Active
Suspended
Locked
Disabled
Deleted or anonymized where legally permitted
FR-AUTH-010 — Identity Profile

Priority: Must Have

Users must be able to view and update approved profile fields.

Changes to sensitive identity fields may require re-verification.

FR-AUTH-011 — Authentication Audit

Priority: Must Have

The system must record material authentication events, including:

Successful login
Failed login
Password reset
Multi-factor authentication change
Account lock
Session revocation
FR-AUTH-012 — Step-Up Authentication

Priority: Should Have

The system should require recent or stronger authentication for high-impact actions.

Examples include:

Privileged role assignment
Payment reversal
High-value refund approval
Organization ownership transfer
Sensitive data export
7. Organization Management
FR-ORG-001 — Create Organization

Priority: Must Have

An authorized user must be able to create an organization.

Organization types may include:

Property owner
Property operator
Estate agency
Legal firm
Vendor
Development company
Platform organization
FR-ORG-002 — Organization Profile

Priority: Must Have

Authorized users must be able to maintain:

Organization name
Organization type
Contact information
Country
Currency
Time zone
Registration information where applicable
Operational status
FR-ORG-003 — Organization Status Management

Priority: Must Have

Authorized users must be able to activate, suspend, archive, or close an organization according to business rules.

FR-ORG-004 — Organization Verification

Priority: Should Have

The system should support organization verification workflows.

FR-ORG-005 — Organization Relationships

Priority: Must Have

The system must allow approved relationships between organizations.

Examples:

Property owner and estate agency
Property owner and legal firm
Property operator and vendor
Development company and contractor
FR-ORG-006 — Organization Context Switching

Priority: Must Have

Users who belong to multiple organizations must be able to switch active organization context.

The active context must be clearly displayed.

FR-ORG-007 — Organization Data Isolation

Priority: Must Have

The system must prevent an organization from accessing another organization’s records without an approved relationship, assignment, or platform authority.

FR-ORG-008 — Organization Ownership Transfer

Priority: Should Have

The system should support a controlled organization ownership-transfer workflow.

FR-ORG-009 — Organization Archival

Priority: Must Have

Archiving an organization must preserve historical records and prevent unauthorized new activity.

8. Membership and Role Management
FR-ROLE-001 — Invite Organization Member

Priority: Must Have

Authorized users must be able to invite users to join an organization.

FR-ROLE-002 — Accept Invitation

Priority: Must Have

Invited users must be able to accept or decline an invitation.

FR-ROLE-003 — Assign Role

Priority: Must Have

Authorized users must be able to assign one or more approved roles to an organization member.

FR-ROLE-004 — Assign Resource Scope

Priority: Must Have

Role assignments must support scope at one or more of the following levels:

Organization
Property
Community
Building
Block
Unit
Listing
Application
Legal matter
Work order
FR-ROLE-005 — Suspend Membership

Priority: Must Have

Authorized users must be able to suspend a membership without deleting historical attribution.

FR-ROLE-006 — Revoke Membership

Priority: Must Have

Authorized users must be able to revoke membership access.

FR-ROLE-007 — Change Role

Priority: Must Have

Authorized users must be able to change a member’s roles and scopes.

FR-ROLE-008 — Privileged Role Approval

Priority: Must Have

Privileged role assignments must support approval requirements.

FR-ROLE-009 — Role History

Priority: Must Have

The system must retain role-assignment history.

FR-ROLE-010 — Assignment-Based Access

Priority: Must Have

The system must support assignment-specific access for:

Estate agents
Lawyers
Vendors
Vendor workers
Maintenance coordinators
Finance reviewers
Property operators
FR-ROLE-011 — Role Conflict Warning

Priority: Should Have

The system should detect and warn about conflicting role assignments.

FR-ROLE-012 — Access Review

Priority: Should Have

Authorized administrators should be able to review active roles, scopes, and memberships.

9. Property Management
FR-PROP-001 — Create Property

Priority: Must Have

Authorized users must be able to create a property record.

FR-PROP-002 — Maintain Property Profile

Priority: Must Have

The property profile must support:

Property name or provisional label
Address
Location description
Ownership or control status
Operating organization
Property status
Verification status
Notes
Supporting documents
FR-PROP-003 — Property Verification Workflow

Priority: Should Have

The system should support the submission, review, approval, rejection, and expiration of property-verification evidence.

FR-PROP-004 — Property Status

Priority: Must Have

The system must support statuses such as:

Draft
Pending verification
Active
Suspended
Archived
FR-PROP-005 — Property Archive

Priority: Must Have

Authorized users must be able to archive a property when business-rule conditions are satisfied.

FR-PROP-006 — Property Ownership History

Priority: Should Have

The system should retain property ownership or control history.

FR-PROP-007 — Property Documents

Priority: Must Have

Authorized users must be able to attach controlled documents to a property.

FR-PROP-008 — Property Dashboard

Priority: Must Have

Authorized users must be able to view a property summary including:

Communities
Buildings
Blocks
Units
Occupancy
Listings
Applications
Agreements
Payments
Maintenance activity
10. Community Management
FR-COM-001 — Create Community

Priority: Must Have

Authorized users must be able to create a community within a property context.

FR-COM-002 — Provisional Community Label

Priority: Must Have

The system must allow provisional labels such as:

Community 001
FR-COM-003 — Community Profile

Priority: Must Have

The community profile must support:

Operational label
Public name
Description
Status
Address or location context
Amenities
Facilities
Community rules
Contact information
FR-COM-004 — Community Status

Priority: Must Have

The system must support statuses including:

Planned
Under development
Partially operational
Operational
Suspended
Archived
FR-COM-005 — Community Rules

Priority: Should Have

Authorized users should be able to create and publish versioned community rules.

FR-COM-006 — Community Announcements

Priority: Must Have

Authorized users must be able to publish announcements to eligible residents and users.

FR-COM-007 — Community Dashboard

Priority: Must Have

The system must provide a community-level operational dashboard.

11. Building Management
FR-BLD-001 — Create Building

Priority: Must Have

Authorized users must be able to create a building within a community.

FR-BLD-002 — Building Profile

Priority: Must Have

A building profile must support:

Permanent identifier
Operational label
Optional public name
Building type
Status
Description
Construction stage
Operational readiness
Related documents
FR-BLD-003 — Provisional Building Names

Priority: Must Have

The system must support provisional names such as:

Existing Building
Building Under Construction
FR-BLD-004 — Building Status Management

Priority: Must Have

Authorized users must be able to update building status subject to approval and evidence requirements.

FR-BLD-005 — Building Readiness

Priority: Must Have

The system must distinguish:

Construction completion
Operational readiness
Listing readiness
Occupancy readiness
FR-BLD-006 — Building Dashboard

Priority: Must Have

Users with authority must be able to view:

Blocks
Floors
Units
Occupancy
Maintenance
Development status
Documents
12. Block Management
FR-BLK-001 — Create Block

Priority: Must Have

Authorized users must be able to create a block within a building.

FR-BLK-002 — Block Code

Priority: Must Have

Each block must have an operational code unique within the building.

FR-BLK-003 — Block Profile

Priority: Must Have

A block profile must include:

Code
Name or label
Status
Unit count where confirmed
Construction status
Operational status
FR-BLK-004 — Pending Unit Count

Priority: Must Have

The system must allow a block’s unit count to remain unknown or pending.

FR-BLK-005 — Pilot Block Facts

Priority: Must Have

The pilot configuration must support:

Block A
Six confirmed residential units in Block A
Block B
Block B unit count pending
FR-BLK-006 — Block Dashboard

Priority: Must Have

Authorized users must be able to view unit, occupancy, maintenance, and listing status by block.

13. Floor Management
FR-FLR-001 — Optional Floor Structure

Priority: Must Have

The system must allow units to exist without a floor record where the floor structure has not been confirmed.

FR-FLR-002 — Create Floor

Priority: Should Have

Authorized users should be able to create floor records within a building or block.

FR-FLR-003 — Floor Naming Convention

Priority: Should Have

The system should support configurable floor naming and numbering conventions.

FR-FLR-004 — Floor Reordering

Priority: Could Have

Authorized users may be able to reorder or rename floor display labels without changing permanent identifiers.

14. Unit Management
FR-UNIT-001 — Create Unit

Priority: Must Have

Authorized users must be able to create a unit within a valid physical hierarchy.

FR-UNIT-002 — Unit Profile

Priority: Must Have

The unit profile must support:

Permanent identifier
Operational unit code
Optional public display name
Building
Block
Floor where applicable
Unit type
Bedrooms
Bathrooms
Description
Operational status
Occupancy status
Readiness status
Media
Amenities
Charges
Documents
FR-UNIT-003 — Unit Code Uniqueness

Priority: Must Have

The system must prevent duplicate active unit codes within the configured scope.

FR-UNIT-004 — Unit Status Management

Priority: Must Have

The system must support separate status dimensions for:

Construction
Operational readiness
Listing availability
Occupancy
Maintenance
FR-UNIT-005 — Unit Readiness Checklist

Priority: Must Have

Authorized users must be able to complete a readiness checklist before making a unit available.

FR-UNIT-006 — Unit Availability

Priority: Must Have

Authorized users must be able to mark a unit:

Available
Reserved
Occupied
Under maintenance
Unavailable
Retired

subject to business rules.

FR-UNIT-007 — Unit Status History

Priority: Must Have

The system must preserve unit-status history.

FR-UNIT-008 — Unit Media

Priority: Must Have

Authorized users must be able to upload and manage approved unit images and media.

FR-UNIT-009 — Unit Document Association

Priority: Must Have

The system must support controlled documents associated with a unit.

FR-UNIT-010 — Unit Retirement

Priority: Must Have

Authorized users must be able to retire a unit while preserving all historical relationships.

15. Development and Construction Management
FR-DEV-001 — Create Development Record

Priority: Should Have

Authorized users should be able to create a development or construction record.

FR-DEV-002 — Construction Milestones

Priority: Should Have

The system should support milestones such as:

Planned
Approved
Groundwork
Structural work
Finishing
Inspection
Completed
Operational handover
FR-DEV-003 — Progress Updates

Priority: Should Have

Authorized users should be able to record progress updates with:

Date
Percentage or stage
Notes
Photos
Documents
Responsible party
FR-DEV-004 — Contractor Assignment

Priority: Could Have

The system may allow contractors to be assigned to approved development work.

FR-DEV-005 — Operational Handover

Priority: Should Have

The system should support a controlled handover from development status to operational management.

FR-DEV-006 — Planned Inventory

Priority: Should Have

The system should allow planned buildings, blocks, floors, and units to be recorded separately from operational inventory.

16. Amenity and Facility Management
FR-AMN-001 — Create Amenity

Priority: Must Have

Authorized users must be able to create amenities or facilities associated with:

Property
Community
Building
Block
Unit
FR-AMN-002 — Amenity Profile

Priority: Must Have

The amenity profile must support:

Name
Category
Description
Location
Availability status
Operating hours
Restrictions
Responsible operator
Media
FR-AMN-003 — Masjid Facility

Priority: Must Have

The pilot must support the functional masjid as a community facility and amenity.

FR-AMN-004 — Facility Status

Priority: Must Have

Authorized users must be able to mark a facility:

Available
Closed
Under maintenance
Restricted
Retired
FR-AMN-005 — Amenity Notices

Priority: Should Have

Authorized users should be able to publish facility-related notices.

FR-AMN-006 — Religious Privacy

Priority: Must Have

The system must not infer or assign religious identity based on masjid-related activity.

FR-AMN-007 — Waterside Context

Priority: Must Have

The system must support a provisional waterside-context attribute without forcing an unconfirmed water-feature classification.

17. Listing Management
FR-LIST-001 — Create Draft Listing

Priority: Must Have

Authorized users must be able to create a draft listing for an eligible unit.

FR-LIST-002 — Listing Profile

Priority: Must Have

A listing must support:

Listing identifier
Unit
Title
Description
Annual rent
Additional charges
Refundable amounts
Non-refundable amounts
Availability date
Media
Amenities
Contact method
Assigned agent
Status
Publication dates
FR-LIST-003 — Listing Validation

Priority: Must Have

The system must validate required commercial and unit information before publication.

FR-LIST-004 — Listing Approval

Priority: Must Have

Authorized approvers must be able to approve or reject a draft listing.

FR-LIST-005 — Publish Listing

Priority: Must Have

An approved listing must be publishable to the Habitacores public marketplace or approved channel.

FR-LIST-006 — Pause Listing

Priority: Must Have

Authorized users must be able to pause a listing.

FR-LIST-007 — Close Listing

Priority: Must Have

Authorized users must be able to close a listing with a closure reason.

FR-LIST-008 — Listing Version History

Priority: Must Have

Material listing changes must retain history.

FR-LIST-009 — Listing Media Management

Priority: Must Have

Authorized users must be able to add, remove, reorder, and caption listing media.

FR-LIST-010 — Assigned Agent

Priority: Must Have

A listing may be assigned to one or more authorized estate agents.

FR-LIST-011 — Listing Search

Priority: Must Have

Public users must be able to search approved listings.

FR-LIST-012 — Listing Filters

Priority: Must Have

Search filters should include:

Location
Annual rent range
Unit type
Bedrooms
Availability
Amenities
Community
Building
FR-LIST-013 — Listing Detail Page

Priority: Must Have

The system must provide a public detail view for an approved listing.

FR-LIST-014 — Duplicate Listing Detection

Priority: Should Have

The system should identify probable duplicate active listings for the same unit.

18. Prospect and Inquiry Management
FR-INQ-001 — Submit Inquiry

Priority: Must Have

A public or authenticated prospect must be able to submit an inquiry.

FR-INQ-002 — Inquiry Profile

Priority: Must Have

An inquiry must support:

Prospect
Listing or unit
Contact details
Message
Preferred contact method
Source
Status
Assigned user
Communication history
FR-INQ-003 — Inquiry Assignment

Priority: Must Have

Authorized users must be able to assign an inquiry to an agent or operator.

FR-INQ-004 — Inquiry Status

Priority: Must Have

The system must support statuses such as:

New
Contacted
Qualified
Inspection requested
Application started
Closed
Disqualified
FR-INQ-005 — Inquiry Notes

Priority: Must Have

Authorized users must be able to record internal notes.

FR-INQ-006 — Prospect Communication

Priority: Must Have

Authorized users must be able to record communications with the prospect.

FR-INQ-007 — Duplicate Prospect Detection

Priority: Should Have

The system should flag probable duplicate prospect or inquiry records.

FR-INQ-008 — Convert Inquiry

Priority: Must Have

An authorized user must be able to convert an inquiry into:

Inspection request
Application invitation
Closed outcome
19. Inspection Management
FR-INSP-001 — Request Inspection

Priority: Must Have

A prospect must be able to request an inspection for an eligible listing or unit.

FR-INSP-002 — Create Inspection

Priority: Must Have

Authorized users must be able to create an inspection appointment.

FR-INSP-003 — Inspection Details

Priority: Must Have

An inspection must include:

Prospect
Listing or unit
Date
Time
Assigned representative
Meeting instructions
Status
Notes
FR-INSP-004 — Confirm Inspection

Priority: Must Have

Authorized users or an approved automated workflow must be able to confirm an inspection.

FR-INSP-005 — Reschedule Inspection

Priority: Must Have

A prospect or authorized representative must be able to request or perform rescheduling according to policy.

FR-INSP-006 — Cancel Inspection

Priority: Must Have

Authorized users and prospects must be able to cancel an inspection with a reason.

FR-INSP-007 — Inspection Reminder

Priority: Should Have

The system should send reminders before a confirmed inspection.

FR-INSP-008 — Record Attendance

Priority: Must Have

Authorized users must be able to record:

Attended
No-show
Cancelled
Rescheduled
FR-INSP-009 — Inspection Outcome

Priority: Must Have

Authorized users must be able to record inspection feedback and next steps.

FR-INSP-010 — Inspection History

Priority: Must Have

The system must preserve inspection history.

20. Application Management
FR-APP-001 — Start Application

Priority: Must Have

An eligible prospect must be able to start an application for an available unit.

FR-APP-002 — Save Draft Application

Priority: Must Have

Applicants must be able to save and resume a draft application.

FR-APP-003 — Application Sections

Priority: Must Have

The application may include:

Applicant identity
Contact details
Household information
Employment or income information where approved
Guarantor information
Required documents
Consents
Declarations
FR-APP-004 — Co-Applicant

Priority: Must Have

The system must support co-applicants.

FR-APP-005 — Household Members

Priority: Must Have

Applicants must be able to provide household-member information.

FR-APP-006 — Guarantor

Priority: Must Have

The application must support an optional or required guarantor.

FR-APP-007 — Upload Application Documents

Priority: Must Have

Applicants must be able to upload required documents securely.

FR-APP-008 — Application Checklist

Priority: Must Have

Applicants must be able to see outstanding requirements before submission.

FR-APP-009 — Submit Application

Priority: Must Have

Applicants must be able to submit a complete application.

FR-APP-010 — Submission Confirmation

Priority: Must Have

The system must confirm successful submission and provide an application reference.

FR-APP-011 — Application Status Tracking

Priority: Must Have

Applicants must be able to view approved status information.

FR-APP-012 — Information Request

Priority: Must Have

Authorized reviewers must be able to request additional information.

FR-APP-013 — Applicant Response

Priority: Must Have

Applicants must be able to respond to information requests.

FR-APP-014 — Controlled Amendment

Priority: Must Have

The system must support controlled amendments after submission.

FR-APP-015 — Withdraw Application

Priority: Must Have

Applicants must be able to withdraw an application before final processing where policy permits.

FR-APP-016 — Application Expiration

Priority: Should Have

The system should support automatic or manual expiration of inactive applications.

FR-APP-017 — Application History

Priority: Must Have

The system must preserve application status, review, and amendment history.

21. Applicant Review and Decision Management
FR-REV-001 — Review Queue

Priority: Must Have

Authorized reviewers must be able to view applications awaiting review.

FR-REV-002 — Assign Reviewer

Priority: Must Have

Applications must be assignable to an authorized reviewer.

FR-REV-003 — Review Checklist

Priority: Must Have

The system must support a configurable review checklist.

FR-REV-004 — Document Review

Priority: Must Have

Authorized reviewers must be able to:

Approve document
Reject document
Request replacement
Record review notes
FR-REV-005 — Internal Review Notes

Priority: Must Have

Reviewers must be able to record protected internal notes.

FR-REV-006 — Conflict Detection

Priority: Should Have

The system should warn when a reviewer has a conflict with the applicant or application.

FR-REV-007 — Approve Application

Priority: Must Have

An authorized decision-maker must be able to approve an application.

FR-REV-008 — Conditional Approval

Priority: Must Have

An authorized decision-maker must be able to approve an application subject to documented conditions.

FR-REV-009 — Reject Application

Priority: Must Have

An authorized decision-maker must be able to reject an application with a recorded reason.

FR-REV-010 — Decision Notification

Priority: Must Have

The system must notify the applicant of the approved applicant-facing decision.

FR-REV-011 — Decision Audit

Priority: Must Have

The system must record the decision-maker, decision date, reason, and related evidence.

FR-REV-012 — Human Final Decision

Priority: Must Have

The system must prevent AI from independently issuing a final application decision.

22. Offer Management
FR-OFR-001 — Create Offer

Priority: Must Have

Authorized users must be able to create an offer for an approved or conditionally approved application.

FR-OFR-002 — Offer Terms

Priority: Must Have

An offer must include:

Applicant
Unit
Annual rent
Additional charges
Refundable amounts
Non-refundable amounts
Proposed term
Payment deadline
Conditions
Expiration date
FR-OFR-003 — Offer Approval

Priority: Must Have

Offers must support approval before issuance.

FR-OFR-004 — Issue Offer

Priority: Must Have

Authorized users must be able to issue an approved offer to the applicant.

FR-OFR-005 — Accept Offer

Priority: Must Have

Applicants must be able to accept an active offer.

FR-OFR-006 — Decline Offer

Priority: Must Have

Applicants must be able to decline an offer.

FR-OFR-007 — Extend Offer

Priority: Should Have

Authorized users should be able to extend an offer expiration date with an audit record.

FR-OFR-008 — Replace Offer

Priority: Must Have

Material offer changes must create a replacement or new version.

FR-OFR-009 — Offer Expiration

Priority: Must Have

The system must prevent acceptance of an expired offer unless it is reissued or extended.

23. Charge Management
FR-CHG-001 — Create Charge

Priority: Must Have

Authorized users must be able to create a charge.

FR-CHG-002 — Charge Types

Priority: Must Have

The system must support configurable charge types, including:

Annual rent
Service charge
Agency fee
Legal fee
Deposit
Utility charge
Maintenance charge
Platform fee
Other approved charge
FR-CHG-003 — Charge Attributes

Priority: Must Have

Each charge must support:

Charge type
Recipient
Amount
Currency
Due date
Refundability
Description
Status
Related offer, agreement, occupancy, or unit
FR-CHG-004 — Charge Approval

Priority: Should Have

Organizations should be able to require approval for selected charge types.

FR-CHG-005 — Charge Adjustment

Priority: Must Have

Authorized users must be able to create controlled charge adjustments.

FR-CHG-006 — Charge Waiver

Priority: Should Have

Authorized users should be able to request and approve charge waivers.

FR-CHG-007 — Charge Balance

Priority: Must Have

The system must calculate:

Original amount
Adjustments
Allocated amount
Outstanding balance
Settlement status
FR-CHG-008 — Charge History

Priority: Must Have

The system must preserve charge history.

FR-CHG-009 — Annual Rent Separation

Priority: Must Have

Annual rent must remain distinguishable from all other charges.

24. Payment Management
FR-PAY-001 — Record Payment

Priority: Must Have

Authorized users must be able to record a payment.

FR-PAY-002 — Applicant Payment Submission

Priority: Must Have

Applicants or residents must be able to submit payment details or evidence where manual verification is used.

FR-PAY-003 — Payment Attributes

Priority: Must Have

A payment must support:

Payer
Amount
Currency
Date
Method
Provider reference
Bank reference
Evidence
Verification status
Verification actor
Reversal status
FR-PAY-004 — Payment Verification Queue

Priority: Must Have

Finance Reviewers must be able to view payments awaiting verification.

FR-PAY-005 — Verify Payment

Priority: Must Have

Authorized Finance Reviewers must be able to verify a payment.

FR-PAY-006 — Reject Payment Evidence

Priority: Must Have

Authorized reviewers must be able to reject unsupported or invalid payment evidence.

FR-PAY-007 — Payment Allocation

Priority: Must Have

Authorized users must be able to allocate a verified payment to one or more charges.

FR-PAY-008 — Partial Allocation

Priority: Must Have

The system must support partial allocation.

FR-PAY-009 — Allocation Validation

Priority: Must Have

The system must prevent allocations exceeding:

Payment amount
Unallocated payment balance
Outstanding charge amount, unless approved overpayment handling applies
FR-PAY-010 — Unallocated Balance

Priority: Must Have

The system must display any unallocated payment amount.

FR-PAY-011 — Payment Receipt

Priority: Must Have

The system must generate or record a payment receipt after approved verification.

FR-PAY-012 — Payment Reversal

Priority: Must Have

Authorized users must be able to reverse a payment through a controlled workflow.

FR-PAY-013 — Allocation Reversal

Priority: Must Have

Payment reversal must reverse or invalidate related allocations.

FR-PAY-014 — Refund Request

Priority: Must Have

Authorized users must be able to create a refund request.

FR-PAY-015 — Refund Approval

Priority: Must Have

Refunds must support approval requirements.

FR-PAY-016 — Refund Execution

Priority: Must Have

Authorized users must be able to record refund execution and reference details.

FR-PAY-017 — Deposit Tracking

Priority: Must Have

Refundable deposits must remain separately identifiable.

FR-PAY-018 — Financial Audit History

Priority: Must Have

The system must preserve payment, allocation, reversal, refund, and approval history.

FR-PAY-019 — Payment Integration

Priority: Should Have

The system should support approved Nigerian payment-provider or banking integrations.

FR-PAY-020 — No Automatic Verification from Screenshot

Priority: Must Have

An uploaded payment screenshot must not independently verify a payment.

25. Agreement Management
FR-AGR-001 — Create Agreement Draft

Priority: Must Have

Authorized users must be able to create an agreement draft from approved application, offer, unit, and charge data.

FR-AGR-002 — Agreement Template

Priority: Must Have

The system must support approved agreement templates.

FR-AGR-003 — Agreement Parties

Priority: Must Have

The agreement must identify:

Property-owning or controlling party
Primary resident
Co-resident where applicable
Guarantor where applicable
Authorized signatories
FR-AGR-004 — Agreement Terms

Priority: Must Have

The agreement must include:

Unit
Start date
End date
Annual rent
Charges
Deposit terms
Renewal terms
Applicable rules
Signatures
FR-AGR-005 — Legal Review

Priority: Must Have where configured

The system must support assignment of an agreement to an Estate Lawyer for review.

FR-AGR-006 — Agreement Approval

Priority: Must Have

An authorized role must be able to approve an agreement for issuance.

FR-AGR-007 — Agreement Versioning

Priority: Must Have

The system must create a new version for material agreement changes.

FR-AGR-008 — Issue Agreement

Priority: Must Have

Authorized users must be able to issue an approved agreement to the parties.

FR-AGR-009 — Agreement Signature

Priority: Must Have

The system must support recording or collecting required signatures.

FR-AGR-010 — Signed Agreement Immutability

Priority: Must Have

Signed agreement versions must not be overwritten.

FR-AGR-011 — Agreement Activation

Priority: Must Have

The system must activate an agreement only after configured conditions are satisfied.

FR-AGR-012 — Agreement Amendment

Priority: Must Have

Authorized users must be able to create controlled amendments.

FR-AGR-013 — Agreement Termination

Priority: Must Have

The system must support controlled agreement termination.

FR-AGR-014 — Agreement Expiry

Priority: Must Have

The system must track agreement expiry.

FR-AGR-015 — Agreement Access

Priority: Must Have

Authorized parties must be able to access approved agreement versions.

FR-AGR-016 — Agreement Audit

Priority: Must Have

The system must preserve all approval, issue, signature, activation, amendment, and termination events.

26. Occupancy Management
FR-OCC-001 — Create Occupancy

Priority: Must Have

Authorized users must be able to create an occupancy record linked to:

Unit
Agreement
Primary resident
Household
Start date
FR-OCC-002 — Occupancy Status

Priority: Must Have

The system must support statuses such as:

Planned
Pending move-in
Active
Pending move-out
Ended
Cancelled
FR-OCC-003 — Occupancy Conflict Validation

Priority: Must Have

The system must prevent incompatible overlapping occupancies.

FR-OCC-004 — Household Association

Priority: Must Have

The system must associate approved household members with an occupancy.

FR-OCC-005 — Occupancy History

Priority: Must Have

The system must preserve occupancy history.

FR-OCC-006 — Resident Activation

Priority: Must Have

Approved users must receive resident access only when required occupancy conditions are satisfied.

FR-OCC-007 — Occupancy End

Priority: Must Have

Authorized users must be able to end an occupancy through an approved move-out or termination workflow.

27. Move-In Management
FR-MOVEIN-001 — Create Move-In Workflow

Priority: Must Have

The system must create a move-in workflow for an eligible occupancy.

FR-MOVEIN-002 — Move-In Checklist

Priority: Must Have

The move-in checklist may include:

Agreement signed
Required payments verified
Unit ready
Resident identity confirmed
Condition inspection complete
Keys prepared
Access instructions prepared
Required documents issued
FR-MOVEIN-003 — Move-In Inspection

Priority: Must Have

Authorized users must be able to record unit condition before occupancy.

FR-MOVEIN-004 — Condition Evidence

Priority: Must Have

The system must support:

Photos
Notes
Existing defects
Meter readings
Resident acknowledgement
FR-MOVEIN-005 — Key and Access Handover

Priority: Must Have

The system must record issued keys, codes, cards, or access items.

FR-MOVEIN-006 — Approve Move-In

Priority: Must Have

An authorized operator must be able to approve move-in.

FR-MOVEIN-007 — Activate Occupancy

Priority: Must Have

Successful move-in approval must activate the occupancy and update the unit’s occupancy status.

FR-MOVEIN-008 — Move-In Record

Priority: Must Have

The system must preserve the final move-in record.

28. Resident Portal
FR-RES-001 — Resident Dashboard

Priority: Must Have

Residents must have access to a dashboard showing relevant:

Agreement
Occupancy
Charges
Payments
Maintenance requests
Notices
Renewal status
Household information
FR-RES-002 — View Agreement

Priority: Must Have

Authorized residents must be able to view approved agreement documents.

FR-RES-003 — View Charges

Priority: Must Have

Authorized residents must be able to view charges and outstanding balances.

FR-RES-004 — View Payments

Priority: Must Have

Authorized residents must be able to view verified payments and allocations.

FR-RES-005 — Submit Maintenance Request

Priority: Must Have

Residents must be able to submit maintenance requests.

FR-RES-006 — Track Maintenance

Priority: Must Have

Residents must be able to view approved maintenance status updates.

FR-RES-007 — Receive Notices

Priority: Must Have

Residents must be able to access material property and community notices.

FR-RES-008 — Household Information

Priority: Must Have

Primary Residents must be able to view approved household-member information.

FR-RES-009 — Request Household Change

Priority: Should Have

Residents should be able to request addition or removal of household members.

FR-RES-010 — Communication Preferences

Priority: Should Have

Residents should be able to manage approved communication preferences.

FR-RES-011 — Former Resident Access

Priority: Should Have

Former residents should retain limited access to approved historical records for the configured retention period.

29. Renewal Management
FR-REN-001 — Expiry Monitoring

Priority: Must Have

The system must identify agreements approaching expiry.

FR-REN-002 — Renewal Dashboard

Priority: Must Have

Authorized users must be able to view upcoming expirations and renewal status.

FR-REN-003 — Renewal Reminder

Priority: Must Have

The system must support configurable renewal reminders.

FR-REN-004 — Owner Renewal Decision

Priority: Must Have

Authorized owner roles must be able to decide whether to offer renewal.

FR-REN-005 — Create Renewal Offer

Priority: Must Have

Authorized users must be able to create a renewal offer.

FR-REN-006 — Renewal Terms

Priority: Must Have

The renewal offer must support:

New rent
Charges
New term
Conditions
Acceptance deadline
FR-REN-007 — Resident Renewal Response

Priority: Must Have

Residents must be able to accept or decline a renewal offer.

FR-REN-008 — Renewal Agreement

Priority: Must Have

An accepted renewal must support creation of a new or renewed agreement version.

FR-REN-009 — Renewal Payment Tracking

Priority: Must Have

The system must support charges and payments associated with renewal.

FR-REN-010 — Renewal Decline Workflow

Priority: Must Have

A declined renewal must support transition to move-out planning.

FR-REN-011 — Expired Agreement Alert

Priority: Must Have

The system must alert authorized users when an agreement expires without completed renewal or move-out resolution.

30. Move-Out Management
FR-MOVEOUT-001 — Initiate Move-Out

Priority: Must Have

An authorized user or resident must be able to initiate a move-out request.

FR-MOVEOUT-002 — Record Notice

Priority: Must Have

The system must record notice date, proposed move-out date, and related agreement.

FR-MOVEOUT-003 — Move-Out Checklist

Priority: Must Have

The move-out checklist may include:

Notice accepted
Final inspection scheduled
Outstanding charges reviewed
Keys returned
Access revoked
Condition recorded
Deposit decision completed
Final documents issued
FR-MOVEOUT-004 — Final Inspection

Priority: Must Have

Authorized users must be able to perform and document a final inspection.

FR-MOVEOUT-005 — Damage Assessment

Priority: Must Have

The system must support documented classification of:

Normal wear
Existing defect
Resident-caused damage
Maintenance issue
Disputed condition
FR-MOVEOUT-006 — Deposit Deduction Proposal

Priority: Must Have

Authorized users must be able to propose documented deposit deductions.

FR-MOVEOUT-007 — Deposit Approval

Priority: Must Have

Deposit deductions and refunds must support approval.

FR-MOVEOUT-008 — Final Statement

Priority: Must Have

The system must generate or store a final financial statement.

FR-MOVEOUT-009 — Close Occupancy

Priority: Must Have

Completion of the move-out workflow must end the occupancy.

FR-MOVEOUT-010 — Unit Turnaround Status

Priority: Must Have

After move-out, the system must place the unit into an approved operational status such as:

Turnaround
Maintenance
Inspection required
Available
Unavailable
FR-MOVEOUT-011 — Former Resident Status

Priority: Must Have

The system must update the resident relationship appropriately without deleting historical records.

31. Maintenance Management
FR-MNT-001 — Create Maintenance Request

Priority: Must Have

Residents and authorized users must be able to create a maintenance request.

FR-MNT-002 — Maintenance Request Details

Priority: Must Have

A request must support:

Property context
Unit or facility
Reporter
Description
Category
Priority
Photos
Access notes
Status
FR-MNT-003 — Maintenance Triage

Priority: Must Have

Authorized users must be able to review and assign priority.

FR-MNT-004 — Emergency Classification

Priority: Must Have

Authorized users must be able to classify a request as an emergency.

FR-MNT-005 — Assign Coordinator

Priority: Must Have

A maintenance request must be assignable to a Maintenance Coordinator.

FR-MNT-006 — Create Work Order

Priority: Must Have

Authorized users must be able to create one or more work orders from a request.

FR-MNT-007 — Request Status

Priority: Must Have

Statuses may include:

Submitted
Triaged
Assigned
In progress
Awaiting parts
Awaiting approval
Completed
Verified
Closed
Reopened
Cancelled
FR-MNT-008 — Resident Updates

Priority: Must Have

Residents must receive approved status updates.

FR-MNT-009 — Completion Verification

Priority: Must Have

Authorized users must be able to verify completion.

FR-MNT-010 — Reopen Request

Priority: Must Have

Residents or authorized users must be able to request reopening.

FR-MNT-011 — Maintenance History

Priority: Must Have

The system must preserve all maintenance activity and status history.

FR-MNT-012 — Preventive Maintenance

Priority: Should Have

The system should support scheduled preventive maintenance.

FR-MNT-013 — Facility Maintenance

Priority: Must Have

Maintenance requests must support community facilities, including the masjid.

32. Work-Order Management
FR-WO-001 — Create Work Order

Priority: Must Have

Authorized users must be able to create a work order.

FR-WO-002 — Work Scope

Priority: Must Have

A work order must include:

Source request
Work description
Location
Priority
Assigned provider
Schedule
Cost estimate
Approval status
Work status
FR-WO-003 — Assign Vendor

Priority: Must Have

Authorized users must be able to assign an approved vendor or worker.

FR-WO-004 — Quote Submission

Priority: Should Have

Vendors should be able to submit a quotation.

FR-WO-005 — Quote Approval

Priority: Should Have

Authorized users should be able to approve or reject a quotation.

FR-WO-006 — Work Acceptance

Priority: Must Have

Assigned vendors must be able to accept or decline work.

FR-WO-007 — Work Status Update

Priority: Must Have

Assigned workers must be able to update work status.

FR-WO-008 — Completion Evidence

Priority: Must Have

Workers must be able to submit:

Work notes
Photos
Parts used
Completion date
Invoice or cost details
FR-WO-009 — Cost Change Request

Priority: Must Have

Vendors must be able to request approval for material cost changes.

FR-WO-010 — Verify Work

Priority: Must Have

Authorized users must be able to verify completed work.

FR-WO-011 — Close Work Order

Priority: Must Have

Verified work orders must be closable.

FR-WO-012 — Work-Order History

Priority: Must Have

The system must preserve assignment, status, cost, and completion history.

33. Vendor Management
FR-VEN-001 — Create Vendor Organization

Priority: Must Have

Authorized users must be able to register a vendor organization.

FR-VEN-002 — Vendor Profile

Priority: Must Have

The profile must support:

Business name
Contact information
Service categories
Coverage area
Verification status
Compliance documents
Status
FR-VEN-003 — Vendor Member Management

Priority: Must Have

Vendor Administrators must be able to invite and manage Vendor Workers.

FR-VEN-004 — Vendor Verification

Priority: Should Have

The system should support vendor verification.

FR-VEN-005 — Compliance Documents

Priority: Should Have

Vendors should be able to upload licences, insurance, certifications, and safety records where required.

FR-VEN-006 — Compliance Expiration

Priority: Should Have

The system should track vendor-document expiration.

FR-VEN-007 — Vendor Assignment View

Priority: Must Have

Vendors must be able to view only assigned work.

FR-VEN-008 — Vendor Performance Record

Priority: Should Have

The system should record documented performance indicators.

FR-VEN-009 — Suspend Vendor

Priority: Must Have

Authorized users must be able to suspend a vendor from receiving new assignments.

34. Legal-Matter Management
FR-LEGAL-001 — Create Legal Matter

Priority: Should Have

Authorized users should be able to create a legal matter.

FR-LEGAL-002 — Assign Lawyer

Priority: Must Have where legal review is enabled

A legal matter must be assignable to an authorized Estate Lawyer.

FR-LEGAL-003 — Matter Profile

Priority: Should Have

A matter should support:

Matter type
Related property or agreement
Parties
Assigned lawyer
Status
Confidential notes
Documents
Deadlines
Outcome
FR-LEGAL-004 — Legal Document Drafting

Priority: Should Have

Authorized legal users should be able to create or upload legal drafts.

FR-LEGAL-005 — Agreement Review

Priority: Must Have where enabled

Lawyers must be able to review an assigned agreement and record:

Approved
Changes requested
Rejected
Advice provided
FR-LEGAL-006 — Notice Management

Priority: Should Have

Authorized users should be able to create and issue controlled legal notices.

FR-LEGAL-007 — Matter-Scoped Access

Priority: Must Have

Lawyers must access only assigned matters and approved supporting information.

FR-LEGAL-008 — Matter Closure

Priority: Should Have

Authorized users should be able to close a legal matter with a recorded outcome.

35. Document Management
FR-DOC-001 — Upload Document

Priority: Must Have

Authorized users must be able to upload a document.

FR-DOC-002 — Document Metadata

Priority: Must Have

Documents must support:

Type
Related resource
Organization
Sensitivity
Version
Status
Expiration date
Uploader
Upload date
FR-DOC-003 — File Validation

Priority: Must Have

The system must validate uploaded files for:

File type
File size
Upload completion
Malicious content
Corruption
FR-DOC-004 — Document Preview

Priority: Should Have

Authorized users should be able to preview supported document types.

FR-DOC-005 — Document Download

Priority: Must Have

Authorized users must be able to download approved documents.

FR-DOC-006 — Document Access Control

Priority: Must Have

Document access must follow role, assignment, organization, and sensitivity rules.

FR-DOC-007 — Document Review

Priority: Must Have

Authorized users must be able to approve, reject, or request replacement of controlled documents.

FR-DOC-008 — Document Versioning

Priority: Must Have

Replacing a controlled document must create a new version.

FR-DOC-009 — Signed Document Protection

Priority: Must Have

Signed documents must be immutable.

FR-DOC-010 — Document Expiry Tracking

Priority: Should Have

The system should identify documents approaching expiration.

FR-DOC-011 — Government Identification Protection

Priority: Must Have

Government-issued identification must receive heightened access protection.

FR-DOC-012 — Public Document Publication

Priority: Must Have

A document must be explicitly approved before public publication.

FR-DOC-013 — Document Retention

Priority: Must Have

The system must support retention rules by document type.

36. Communication Management
FR-COMM-001 — Record Communication

Priority: Must Have

Authorized users must be able to record communication related to a business record.

FR-COMM-002 — Communication Channels

Priority: Must Have

The system should support records for:

Email
SMS
WhatsApp
Phone
In-app message
In-person communication
FR-COMM-003 — Send Email

Priority: Must Have

The system must support approved email notifications.

FR-COMM-004 — Send SMS

Priority: Should Have

The system should support approved SMS notifications.

FR-COMM-005 — WhatsApp Integration

Priority: Should Have

The system should support approved WhatsApp messaging integration.

FR-COMM-006 — Communication Templates

Priority: Must Have

Authorized users must be able to manage approved communication templates.

FR-COMM-007 — Delivery Status

Priority: Should Have

The system should record delivery status where supported.

FR-COMM-008 — Communication History

Priority: Must Have

Authorized users must be able to view communication history within their scope.

FR-COMM-009 — Internal Notes

Priority: Must Have

Authorized users must be able to create internal notes not visible to external users.

FR-COMM-010 — Sensitive Information Protection

Priority: Must Have

Notifications must not expose unnecessary sensitive information.

37. Notification Management
FR-NOT-001 — Event-Triggered Notifications

Priority: Must Have

The system must support notifications triggered by approved business events.

FR-NOT-002 — Notification Preferences

Priority: Should Have

Users should be able to manage configurable notification preferences.

FR-NOT-003 — Notification Queue

Priority: Must Have

The system must queue and process notifications reliably.

FR-NOT-004 — Delivery Retry

Priority: Should Have

The system should retry failed notifications according to configured policy.

FR-NOT-005 — Notification History

Priority: Must Have

The system must retain notification history.

FR-NOT-006 — Reminder Scheduling

Priority: Must Have

The system must support scheduled reminders for:

Inspections
Application actions
Offer expiry
Payment deadlines
Agreement expiry
Renewal actions
Maintenance appointments
Document expiry
FR-NOT-007 — Escalation

Priority: Should Have

The system should support escalation when critical notifications remain unresolved.

FR-NOT-008 — Duplicate Suppression

Priority: Should Have

The system should prevent unnecessary duplicate notifications.

38. Search and Navigation
FR-SEARCH-001 — Global Search

Priority: Must Have

Authorized users must be able to search records within their permitted scope.

FR-SEARCH-002 — Searchable Record Types

Priority: Must Have

Search should support:

Organizations
Properties
Communities
Buildings
Blocks
Units
Listings
Prospects
Applications
Residents
Agreements
Payments
Maintenance requests
Vendors
Documents
FR-SEARCH-003 — Filtered Search

Priority: Must Have

Users must be able to filter by status, date, assignment, location, and relevant domain attributes.

FR-SEARCH-004 — Saved Filters

Priority: Could Have

Users may be able to save frequently used filters.

FR-SEARCH-005 — Recent Records

Priority: Should Have

Users should be able to access recently viewed records.

FR-SEARCH-006 — Contextual Navigation

Priority: Must Have

Users must be able to navigate from parent records to related child records.

Example:

Property
→ Community
→ Building
→ Block
→ Unit
FR-SEARCH-007 — Search Access Enforcement

Priority: Must Have

Search results must not reveal inaccessible records.

39. Dashboards and Reporting
FR-RPT-001 — Platform Dashboard

Priority: Must Have

Platform administrators must have access to approved platform-level metrics.

FR-RPT-002 — Organization Dashboard

Priority: Must Have

Organization users must have access to organization-scoped metrics.

FR-RPT-003 — Property Dashboard

Priority: Must Have

The property dashboard must show relevant operational and financial summaries.

FR-RPT-004 — Occupancy Dashboard

Priority: Must Have

Authorized users must be able to view:

Total units
Available units
Reserved units
Occupied units
Units under maintenance
Expiring occupancies
FR-RPT-005 — Listing Funnel

Priority: Should Have

The system should report:

Listing views
Inquiries
Inspections
Applications
Approvals
Agreements
Occupancies
FR-RPT-006 — Application Dashboard

Priority: Must Have

Authorized users must be able to view application volume and status.

FR-RPT-007 — Financial Dashboard

Priority: Must Have

Authorized finance users must be able to view:

Charges
Payments
Allocations
Outstanding balances
Refunds
Deposits
Unallocated payments
FR-RPT-008 — Maintenance Dashboard

Priority: Must Have

Authorized users must be able to view maintenance workload and status.

FR-RPT-009 — Renewal Dashboard

Priority: Must Have

Authorized users must be able to view upcoming agreement expirations and renewal progress.

FR-RPT-010 — Vendor Dashboard

Priority: Should Have

Authorized users should be able to view vendor assignments and performance records.

FR-RPT-011 — Export Report

Priority: Must Have

Authorized users must be able to export approved reports.

FR-RPT-012 — Scheduled Reports

Priority: Could Have

Users may be able to schedule recurring reports.

FR-RPT-013 — Report Data Isolation

Priority: Must Have

Reports must respect organization and role scope.

40. Audit and Compliance Functions
FR-AUD-001 — Audit Event Creation

Priority: Must Have

The system must create audit events for material actions.

FR-AUD-002 — Audit Event Details

Priority: Must Have

Audit records must include:

Actor
Organization
Action
Resource
Date and time
Previous value where applicable
New value where applicable
Reason where required
Correlation identifier where available
FR-AUD-003 — Audit Search

Priority: Must Have

Authorized users must be able to search audit records.

FR-AUD-004 — Audit Filters

Priority: Must Have

Audit records must be filterable by:

User
Organization
Resource type
Action
Date range
Correlation identifier
FR-AUD-005 — Audit Export

Priority: Should Have

Authorized auditors should be able to export approved audit records.

FR-AUD-006 — Audit Protection

Priority: Must Have

Ordinary users must not edit or delete audit events.

FR-AUD-007 — Access Attempt Logging

Priority: Should Have

The system should record significant unauthorized or denied access attempts.

FR-AUD-008 — Approval History

Priority: Must Have

The system must preserve approval and rejection history for controlled workflows.

FR-AUD-009 — Exception Register

Priority: Should Have

The system should support a register of approved business-rule exceptions.

41. Platform Administration
FR-ADMIN-001 — Platform Organization Management

Priority: Must Have

Platform administrators must be able to view and manage approved organization lifecycle actions.

FR-ADMIN-002 — User Support Search

Priority: Must Have

Authorized support users must be able to locate users and organizations within approved support scope.

FR-ADMIN-003 — Controlled Support Access

Priority: Must Have

The system must support controlled, time-limited, audited support access.

FR-ADMIN-004 — Feature Configuration

Priority: Should Have

Platform administrators should be able to configure approved feature settings.

FR-ADMIN-005 — Reference Data Management

Priority: Must Have

Authorized administrators must be able to manage approved controlled vocabularies.

Examples:

Organization types
Unit types
Charge types
Maintenance categories
Document types
Status reasons
FR-ADMIN-006 — Integration Management

Priority: Should Have

Authorized administrators should be able to configure approved integrations.

FR-ADMIN-007 — Notification Template Management

Priority: Must Have

Authorized administrators must be able to manage notification templates.

FR-ADMIN-008 — System Health Visibility

Priority: Should Have

Platform administrators should be able to view system and integration health information.

FR-ADMIN-009 — Incident Record

Priority: Could Have

The system may support incident-management records.

FR-ADMIN-010 — No Unrestricted Default Access

Priority: Must Have

Platform administrators must not receive unrestricted customer-data access by default.

42. Configuration Management
FR-CONF-001 — Organization Configuration

Priority: Must Have

Organizations must be able to configure approved operational settings.

FR-CONF-002 — Configurable Settings

Priority: Must Have

Configurable settings may include:

Time zone
Currency
Inspection hours
Application requirements
Legal-review requirement
Partial-payment policy
Payment-verification workflow
Renewal reminders
Maintenance thresholds
Refund thresholds
Communication channels
Document requirements
FR-CONF-003 — Configuration Versioning

Priority: Must Have

Material configuration changes must retain history.

FR-CONF-004 — Configuration Effective Date

Priority: Should Have

Configuration changes should support future effective dates.

FR-CONF-005 — Configuration Scope

Priority: Must Have

Settings may apply at:

Platform
Organization
Property
Community
FR-CONF-006 — Configuration Inheritance

Priority: Should Have

Lower-level records should inherit higher-level configuration unless an approved override exists.

FR-CONF-007 — Configuration Audit

Priority: Must Have

Material configuration changes must be audited.

43. AI-Assisted Functional Requirements
FR-AI-001 — AI Assistance Identification

Priority: Must Have

AI-generated or AI-assisted output must be identifiable.

FR-AI-002 — Application Summary

Priority: Should Have

AI may generate an application summary for an authorized human reviewer.

FR-AI-003 — Document Summary

Priority: Should Have

AI may summarize approved documents for authorized users.

FR-AI-004 — Maintenance Classification

Priority: Should Have

AI may suggest maintenance category and priority.

FR-AI-005 — Communication Drafting

Priority: Should Have

AI may draft communications for human review.

FR-AI-006 — Listing Description Drafting

Priority: Should Have

AI may draft listing descriptions using approved property data.

FR-AI-007 — Agreement Summary

Priority: Should Have

AI may summarize agreement terms for authorized users.

FR-AI-008 — Anomaly Flagging

Priority: Could Have

AI may flag potential anomalies in:

Duplicate records
Payment information
Application information
Maintenance patterns
Listing data
FR-AI-009 — Human Approval

Priority: Must Have

AI-assisted output must require human approval before becoming an authoritative record where the action is high impact.

FR-AI-010 — Organization-Scoped AI Context

Priority: Must Have

AI requests must remain scoped to authorized organization data.

FR-AI-011 — Sensitive Data Minimization

Priority: Must Have

AI workflows must use the minimum necessary sensitive information.

FR-AI-012 — AI Activity Audit

Priority: Should Have

The system should record material AI-assisted activity.

FR-AI-013 — Prohibited AI Final Decisions

Priority: Must Have

AI must not independently finalize:

Applicant approval
Applicant rejection
Agreement termination
Eviction
Payment verification
Refund denial
Ownership verification
Vendor suspension
Legal conclusions
44. Data Import and Export
FR-DATA-001 — Import Property Data

Priority: Should Have

Authorized users should be able to import approved property and unit data using a controlled template.

FR-DATA-002 — Import Validation

Priority: Must Have for supported imports

Imports must be validated before records are created or updated.

FR-DATA-003 — Import Preview

Priority: Should Have

Users should be able to preview import results and errors.

FR-DATA-004 — Import Audit

Priority: Must Have

Imports must create an audit record.

FR-DATA-005 — Export Records

Priority: Must Have

Authorized users must be able to export approved data within their scope.

FR-DATA-006 — Sensitive Export Approval

Priority: Must Have

Sensitive exports must require appropriate authorization.

FR-DATA-007 — Export History

Priority: Must Have

The system must record sensitive or material export activity.

FR-DATA-008 — Data Portability

Priority: Should Have

Organizations should be able to obtain an approved export of their data.

45. Public Marketplace Requirements
FR-PUB-001 — Public Listing Access

Priority: Must Have

Unauthenticated users must be able to view approved public listings.

FR-PUB-002 — Public Community Information

Priority: Should Have

Unauthenticated users should be able to view approved public community information.

FR-PUB-003 — Public Search

Priority: Must Have

Unauthenticated users must be able to search approved listings.

FR-PUB-004 — Inquiry Submission

Priority: Must Have

Unauthenticated users must be able to submit a listing inquiry.

FR-PUB-005 — Inspection Request

Priority: Must Have

Public users must be able to initiate an inspection request.

FR-PUB-006 — Account Conversion

Priority: Must Have

A public prospect must be able to create or activate an account when beginning a protected workflow.

FR-PUB-007 — Public Data Limitation

Priority: Must Have

Public pages must not expose:

Resident information
Applicant information
Private documents
Internal notes
Private unit-access details
Financial records
Legal records
FR-PUB-008 — Listing Share Link

Priority: Should Have

Users should be able to share an approved listing link.

46. Mobile and Responsive Functional Requirements
FR-MOB-001 — Responsive User Experience

Priority: Must Have

All core MVP workflows must function on supported mobile and desktop browsers.

FR-MOB-002 — Mobile Prospect Experience

Priority: Must Have

Listing search, inquiry, inspection request, and application initiation must be mobile friendly.

FR-MOB-003 — Mobile Resident Experience

Priority: Must Have

Residents must be able to view records and submit maintenance requests from mobile devices.

FR-MOB-004 — Mobile Vendor Experience

Priority: Must Have

Vendor workers must be able to view assignments and upload completion evidence from mobile devices.

FR-MOB-005 — Mobile Media Capture

Priority: Should Have

Users should be able to capture and upload photos from supported mobile devices.

FR-MOB-006 — Low-Bandwidth Consideration

Priority: Should Have

Core workflows should remain usable under constrained network conditions.

47. Functional Data Validation
FR-VAL-001 — Required Fields

Priority: Must Have

The system must enforce required fields before allowing controlled status changes.

FR-VAL-002 — Valid Status Transition

Priority: Must Have

The system must prevent invalid lifecycle transitions.

FR-VAL-003 — Date Validation

Priority: Must Have

The system must prevent contradictory date sequences.

FR-VAL-004 — Amount Validation

Priority: Must Have

The system must validate financial amounts and currency.

FR-VAL-005 — Duplicate Detection

Priority: Should Have

The system should identify probable duplicate records without automatically merging them.

FR-VAL-006 — Referential Integrity

Priority: Must Have

The system must prevent records from referencing invalid or unauthorized parent records.

FR-VAL-007 — Cross-Organization Validation

Priority: Must Have

The system must prevent relationships between records from unrelated organizations unless an approved cross-organization relationship exists.

FR-VAL-008 — File Validation

Priority: Must Have

The system must validate uploaded documents and media.

48. Functional Error Handling
FR-ERR-001 — Clear Validation Errors

Priority: Must Have

Users must receive clear and actionable validation messages.

FR-ERR-002 — Authorization Error

Priority: Must Have

The system must clearly indicate when an action is not permitted without exposing inaccessible data.

FR-ERR-003 — Conflict Error

Priority: Must Have

The system must report conflicts such as:

Unit already occupied
Listing already active
Offer expired
Payment already reversed
Agreement version superseded
FR-ERR-004 — Partial Failure

Priority: Must Have

The system must not report full success when only part of a workflow completed.

FR-ERR-005 — Integration Failure

Priority: Must Have

The system must identify when an external integration failed or remains pending.

FR-ERR-006 — Retry Support

Priority: Should Have

Users or system processes should be able to retry appropriate failed operations safely.

FR-ERR-007 — Error Correlation

Priority: Should Have

Material failures should include a support reference or correlation identifier.

49. Functional Access Requirements
FR-ACCESS-001 — Role-Based Access

Priority: Must Have

Every protected function must evaluate the user’s active role.

FR-ACCESS-002 — Scope-Based Access

Priority: Must Have

Every protected function must evaluate resource scope.

FR-ACCESS-003 — Assignment-Based Access

Priority: Must Have

The system must evaluate assignment-specific access where applicable.

FR-ACCESS-004 — Organization Context

Priority: Must Have

Every organization-owned operation must execute within an explicit organization context.

FR-ACCESS-005 — Server-Side Enforcement

Priority: Must Have

Access decisions must be enforced server side.

FR-ACCESS-006 — User Interface Enforcement

Priority: Must Have

The interface must hide or disable actions that the user cannot perform.

FR-ACCESS-007 — Sensitive Field Restrictions

Priority: Must Have

The system must support field-level restrictions for highly sensitive data.

FR-ACCESS-008 — Time-Limited Access

Priority: Must Have

Assignments and support sessions must support expiration.

FR-ACCESS-009 — Revocation

Priority: Must Have

Revoked access must become ineffective promptly.

FR-ACCESS-010 — Access Decision Audit

Priority: Should Have

Material privileged access decisions should be auditable.

50. MVP Functional Requirements

The MVP must include the following minimum capabilities.

50.1 Identity and Organizations
User authentication
Organization creation
Organization membership
Role assignment
Organization context switching
Multi-organization isolation
50.2 Property Structure
Property
Community
Building
Block
Optional floor
Unit
Facility and amenity
Functional masjid representation
Waterside-context representation
50.3 Leasing Lifecycle
Listing
Inquiry
Inspection
Application
Application review
Decision
Offer
Charges
Payment verification
Payment allocation
Agreement
Occupancy
Move-in
Resident portal
Renewal tracking
Move-out
50.4 Operations
Maintenance requests
Work orders
Vendor assignments
Documents
Notifications
Audit events
Dashboards
Search
50.5 Governance
Approval workflows
Role scope
Historical traceability
Human review for high-impact decisions
Financial separation
Server-side authorization
51. Pilot Functional Representation

The MVP must support the initial pilot without hardcoding it as the only possible property structure.

The pilot may be represented as:

Property
└── Community 001
    ├── Existing Building
    │   ├── Block A
    │   │   └── Six confirmed residential units
    │   └── Block B
    ├── Functional Masjid
    └── Building Under Construction
        └── Waterside context

The system must allow the following information to remain pending:

Public community name
Public building names
Block B unit count
Second-building unit inventory
Floor structure
Unit numbering convention
Exact water-feature classification
52. Deferred Functional Requirements

The following capabilities are deferred beyond the initial MVP:

Property sales
Mortgage origination
Rent financing
Insurance underwriting
Investment syndication
Public vendor marketplace
Full construction procurement
Multi-currency settlement
Advanced general ledger
Automated applicant scoring
Autonomous applicant decisions
Government system integrations
Native mobile applications
Smart-building hardware integration
Biometric access control
Full facilities-booking marketplace
Advanced predictive maintenance
Large-enterprise portfolio consolidation
Public property-investment marketplace

Deferred capabilities must not be implied as available in the MVP.

53. Functional Acceptance Criteria Standard

Each implemented functional requirement must have acceptance criteria.

Acceptance criteria should describe:

User role
Preconditions
Trigger
Expected result
Validation
Authorization
Audit result
Failure behaviour

Example:

Requirement:
FR-PAY-007 — Payment Allocation

Given:
A Finance Reviewer has an active organization membership.
A verified payment of ₦1,000,000 exists.
A charge has an outstanding balance of ₦600,000.

When:
The Finance Reviewer allocates ₦600,000 to the charge.

Then:
The charge balance becomes ₦0.
The payment retains an unallocated balance of ₦400,000.
The allocation is recorded.
An audit event is created.

Example:

Requirement:
FR-REV-007 — Approve Application

Given:
An application is under review.
The user is an Estate Agent without applicant-decision authority.

When:
The user attempts to approve the application.

Then:
The system denies the action.
The application remains unchanged.
The user receives an authorization message.
The attempt may be audited.
54. Functional Test Coverage

Functional testing must include:

Successful workflows
Validation failures
Unauthorized access
Cross-organization access attempts
Invalid status transitions
Duplicate records
Partial workflow completion
Integration failure
Audit-event creation
Approval requirements
Exception handling
Mobile-browser workflows
File upload validation
Financial calculation validation
AI human-review requirements
55. Traceability Requirements

Every functional requirement should eventually map to:

Product objective
Business rule
Domain entity
User role
User story
API capability
Database entity
User-interface flow
Automated test
Audit requirement

Example:

Functional Requirement:
FR-PAY-009 — Allocation Validation

Related Business Rules:
BR-PAY-006
BR-PAY-007

Related Roles:
Finance Reviewer

Related Domain Entities:
Payment
Payment Allocation
Charge

Related Tests:
Payment allocation limit test
Charge over-allocation test
Cross-organization allocation test
56. Unresolved Functional Decisions

The following functional decisions remain pending:

Final authentication provider
Exact phone-verification method
Exact organization-verification workflow
Exact property-verification workflow
Final unit-numbering convention
Final Block B inventory
Final second-building inventory
Final floor structure
Exact listing approval sequence
Whether agents may create listing drafts
Exact inspection availability rules
Exact application fields
Exact required applicant documents
Exact applicant review checklist
Exact guarantor requirements
Exact partial-payment policy
Exact payment-provider integration
Exact payment-verification evidence
Exact deposit structure
Exact fee structure
Exact agreement-signature method
Exact legal-review requirements
Exact move-in checklist
Exact resident household-management permissions
Exact renewal schedule
Exact maintenance priority definitions
Exact maintenance approval thresholds
Exact vendor-verification requirements
Exact communication channels enabled at launch
Exact notification templates
Exact document-retention periods
Exact support-access process
Exact reporting exports
Exact AI capabilities enabled in the MVP

These decisions must be resolved before the affected functionality is activated in production.

57. Functional Requirements Governance

Changes to approved functional requirements require:

Requirement identifier
Proposed change
Business rationale
Related business rules
Affected users and roles
Workflow impact
Data impact
Security impact
Privacy impact
Financial impact where applicable
Legal impact where applicable
API impact
User-interface impact
Test impact
Migration impact
Product Visionary approval
Chief Product Architect approval
Effective version
58. Current Functional Requirements Decision

Habitacores will provide a multi-organization, role-aware, assignment-scoped residential-property operating system.

The primary functional lifecycle is:

Organization Setup
→ Property Setup
→ Community and Building Setup
→ Block and Unit Setup
→ Unit Readiness
→ Listing
→ Inquiry
→ Inspection
→ Application
→ Review and Decision
→ Offer
→ Charges
→ Payment and Allocation
→ Agreement
→ Occupancy
→ Move-In
→ Resident Operations
→ Renewal or Move-Out

The functional design must preserve:

Accurate property hierarchy
Nigerian annual-rent operations
Transparent charges
Financial integrity
Human accountability
Role-scoped access
Assignment-scoped professional access
Resident household privacy
Historical traceability
Document control
Auditability
Multi-organization isolation
Mobile usability
Future SaaS extensibility

No implemented function may contradict an approved mandatory business rule.