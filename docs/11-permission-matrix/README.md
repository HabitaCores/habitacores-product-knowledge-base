# Document 11 — Permission Matrix and Authorization Model

## Document Control

| Field | Value |
|---|---|
| Product | Habitacores |
| Document | Permission Matrix and Authorization Model |
| Version | 0.1 |
| Status | Draft for approval |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Market | Nigeria |
| Primary Tenant Boundary | Organization |
| Authorization Model | Role-, scope-, assignment-, state-, and approval-aware |
| Last Updated | 30 July 2026 |

## 1. Purpose

This document defines the canonical permission matrix and authorization model for Habitacores.

It translates the approved:

- Domain model
- User personas and roles
- Business rules
- Functional requirements
- Non-functional requirements
- System architecture
- Data architecture

into an enforceable authorization framework.

This document defines:

- Canonical permission naming
- Role-to-permission mapping
- Scope evaluation
- Assignment-based access
- Organization context
- Cross-organization access
- Resident household access
- Field-level restrictions
- Privileged actions
- Approval requirements
- Separation of duties
- Role conflicts
- Support access
- Authorization decision logic
- Denial behavior
- Audit requirements
- Test requirements
- MVP authorization scope
- Deferred authorization capabilities

This document defines access intent and policy.

It does not yet define the final executable authorization code, database policies, or Row Level Security SQL.

Those controls must be implemented and tested against this specification.

## 2. Authorization Objectives

The Habitacores authorization model must:

1. Deny access by default.
2. Enforce organization isolation.
3. Separate authentication from authorization.
4. Separate user identity from organization membership.
5. Support multiple roles per user.
6. Support resource-level scope.
7. Support assignment-specific access.
8. Support record-state-dependent permissions.
9. Support approval requirements.
10. Support separation of duties.
11. Prevent role escalation.
12. Minimize access to sensitive data.
13. Preserve resident household privacy.
14. Restrict professional users to assigned work.
15. Restrict platform support access.
16. Produce attributable audit records.
17. Be enforced server side.
18. Remain testable and understandable.
19. Support the founding-team pilot without introducing unrestricted access.
20. Scale to future multi-tenant SaaS operations.

## 3. Authorization Principles

### 3.1 Deny by Default

An action must be denied unless all required authorization conditions are satisfied.

### 3.2 Server-Side Enforcement

Every protected action must be authorized on the server.

Client-side interface restrictions are supplementary only.

### 3.3 Explicit Organization Context

Every organization-owned operation must execute within an explicit organization context.

### 3.4 Least Privilege

Users must receive the minimum access necessary for their responsibilities.

### 3.5 Scope Before Convenience

A broad role must not override a narrower assignment or resource scope.

### 3.6 Assignment Does Not Equal Ownership

An assigned user may act on the assigned record without receiving ownership of the underlying property, organization, resident, or financial data.

### 3.7 Record State Matters

Permission to perform an action may depend on the record’s current state.

Example:

An approved listing may be published.

A draft listing may not be published by a user without approval authority.

### 3.8 Approval Is Distinct from Execution

The ability to prepare, approve, and execute a high-impact action may be separated.

### 3.9 Historical Attribution Must Remain

Revoking access must not remove historical attribution from prior actions.

### 3.10 No Privilege Through Indirect Reference

A user must not gain access to a restricted record merely because an accessible record references it.

## 4. Authorization Decision Model

A Habitacores authorization decision should evaluate:

```text
User
+ Authentication Status
+ Active Organization
+ Active Membership
+ Role
+ Permission
+ Scope
+ Assignment
+ Resource Ownership
+ Resource State
+ Approval Policy
+ Separation-of-Duties Policy
+ Sensitive-Field Policy
= Authorization Decision
````

Conceptually:

```text
Can User U
perform Action A
on Resource R
within Organization O
under Role P
within Scope S
through Assignment X
given Record State T
and Approval Policy Y?
```

The result must be one of:

```text
Allow
Deny
Require Approval
Require Step-Up Authentication
Require Additional Assignment
```

## 5. Permission Naming Convention

Permissions must use:

```text
<resource>.<action>
```

Examples:

```text
organization.read
organization.update
membership.invite
role.assign
property.create
listing.approve
application.review
application.decide
payment.verify
payment.allocate
agreement.activate
occupancy.activate
maintenance.assign
document.read_sensitive
audit.export
```

Permission codes must:

* Be lowercase
* Use singular resource names
* Use stable action names
* Avoid role names
* Avoid ambiguous terms such as `manage_all`
* Remain unchanged after approval unless formally deprecated

## 6. Permission Action Vocabulary

Preferred actions include:

```text
create
read
read_sensitive
update
archive
restore
delete
submit
review
approve
reject
assign
unassign
publish
pause
close
verify
allocate
reverse
refund
issue
sign
activate
terminate
export
import
configure
grant
revoke
impersonate
```

Action names should reflect real business behavior.

Avoid generic permission codes such as:

```text
admin
manage
full_access
superuser
```

unless they exist only as internal aggregates and are never used as final policy decisions.

## 7. Scope Model

Permissions may apply at the following scopes:

```text
Platform
Organization
Property
Community
Building
Block
Floor
Unit
Listing
Application
Offer
Payment
Agreement
Occupancy
Legal Matter
Maintenance Request
Work Order
Document
Household
```

### 7.1 Platform Scope

Applies across Habitacores.

Example:

```text
platform.organization.read
```

### 7.2 Organization Scope

Applies to all authorized records owned by one organization.

### 7.3 Property Scope

Applies to one property and its descendants where policy allows inheritance.

### 7.4 Community Scope

Applies to one community and its buildings, blocks, floors, units, amenities, and authorized operations.

### 7.5 Building Scope

Applies to one building.

### 7.6 Block Scope

Applies to one block.

### 7.7 Unit Scope

Applies to one or more identified units.

### 7.8 Assignment Scope

Applies only to a specifically assigned business record.

Examples:

* Listing
* Application
* Legal matter
* Payment review
* Work order

### 7.9 Household Scope

Applies only to the resident’s approved household and occupancy.

## 8. Scope Inheritance

Scope inheritance must be explicit.

Preferred physical inheritance:

```text
Property
→ Community
→ Building
→ Block
→ Floor
→ Unit
```

A Property-scoped role may access descendant records only when:

* The permission supports inherited scope.
* The descendant belongs to the same organization.
* No narrower restriction blocks the action.
* The record is not additionally protected.

Assignment-scoped professional roles should not inherit access beyond the assigned record unless the workflow requires supporting context.

## 9. Organization Context

Every authorization decision involving organization-owned data must resolve:

* Current organization
* Membership identifier
* Membership status
* Role assignment
* Scope
* Requested resource organization
* Cross-organization relationship where applicable

A user must not select an arbitrary `organization_id` and thereby gain access.

The server must validate the organization context independently.

## 10. Cross-Organization Authorization

Cross-organization access requires an explicit approved relationship or assignment.

Examples include:

* Property organization assigning a listing to an estate agency
* Property organization assigning an agreement to a legal firm
* Property organization assigning a work order to a vendor
* Platform support receiving temporary customer-data access

Cross-organization authorization must evaluate:

1. Source organization
2. Target organization
3. Relationship type
4. Relationship status
5. Assignment
6. Resource
7. Permission
8. Effective dates
9. Approval
10. Revocation status

A relationship alone must not grant unrestricted access.

## 11. Canonical Role Catalogue

### Platform Roles

* Platform Owner
* Platform Administrator
* Platform Support Agent
* Platform Security Administrator
* Platform Auditor

### Organization and Property Roles

* Organization Owner
* Organization Administrator
* Property Owner Administrator
* Property Operator
* Property Manager
* Community Administrator
* Development Manager
* Finance Reviewer
* Maintenance Coordinator
* Document Reviewer

### Professional Roles

* Estate Agency Administrator
* Estate Agent
* Legal Firm Administrator
* Estate Lawyer
* Vendor Administrator
* Vendor Worker
* Contractor

### Residential Roles

* Prospect
* Applicant
* Primary Resident
* Co-Resident
* Household Member
* Guarantor
* Former Resident

## 12. Platform Permissions

Canonical platform permissions may include:

```text
platform.dashboard.read
platform.organization.read
platform.organization.create
platform.organization.suspend
platform.organization.restore
platform.user.read
platform.user.support_lookup
platform.configuration.read
platform.configuration.update
platform.integration.read
platform.integration.configure
platform.audit.read
platform.audit.export
platform.security.read
platform.security.respond
platform.support_session.create
platform.support_session.terminate
platform.feature.configure
```

Platform permissions must not automatically grant unrestricted access to customer-owned data.

## 13. Organization Permissions

Canonical organization permissions may include:

```text
organization.read
organization.update
organization.archive
organization.transfer_ownership
organization.relationship.create
organization.relationship.approve
organization.relationship.revoke

membership.read
membership.invite
membership.suspend
membership.revoke

role.read
role.assign
role.approve
role.revoke
role.review_access

configuration.read
configuration.update
configuration.approve
```

## 14. Property Permissions

```text
property.create
property.read
property.update
property.verify
property.archive
property.restore

community.create
community.read
community.update
community.archive
community.publish_notice

building.create
building.read
building.update
building.change_status
building.archive

block.create
block.read
block.update
block.change_status
block.archive

floor.create
floor.read
floor.update
floor.archive

unit.create
unit.read
unit.update
unit.change_status
unit.mark_ready
unit.mark_available
unit.retire
unit.restore

amenity.create
amenity.read
amenity.update
amenity.change_status
amenity.publish_notice
```

## 15. Marketplace Permissions

```text
listing.create
listing.read
listing.update
listing.submit_for_approval
listing.approve
listing.reject
listing.publish
listing.pause
listing.close
listing.assign_agent
listing.manage_media

inquiry.create
inquiry.read
inquiry.update
inquiry.assign
inquiry.close
inquiry.read_internal_notes

inspection.create
inspection.read
inspection.confirm
inspection.reschedule
inspection.cancel
inspection.record_attendance
inspection.record_outcome
```

## 16. Application Permissions

```text
application.create
application.read_own
application.read
application.update_draft
application.submit
application.withdraw
application.request_information
application.respond_information
application.assign_reviewer
application.review
application.read_internal_notes
application.decide
application.approve
application.conditionally_approve
application.reject
application.expire

application_document.upload
application_document.read
application_document.review
application_document.approve
application_document.reject
```

## 17. Offer Permissions

```text
offer.create
offer.read
offer.update_draft
offer.submit_for_approval
offer.approve
offer.reject
offer.issue
offer.accept
offer.decline
offer.extend
offer.replace
offer.expire
```

## 18. Finance Permissions

```text
charge.create
charge.read
charge.update_draft
charge.approve
charge.adjust
charge.waive
charge.cancel

payment.create
payment.read
payment.submit_evidence
payment.review_evidence
payment.verify
payment.reject
payment.allocate
payment.reverse

refund.create
refund.review
refund.approve
refund.execute

deposit.read
deposit.decide
deposit.refund
deposit.forfeit

receipt.issue
financial_report.read
financial_report.export
```

Finance permissions must remain separate by default.

A user with `payment.create` must not automatically receive `payment.verify`.

## 19. Agreement Permissions

```text
agreement_template.create
agreement_template.read
agreement_template.update
agreement_template.approve
agreement_template.archive

agreement.create
agreement.read
agreement.read_sensitive
agreement.update_draft
agreement.submit_for_legal_review
agreement.legal_review
agreement.approve
agreement.issue
agreement.sign
agreement.activate
agreement.amend
agreement.terminate
agreement.archive
```

## 20. Occupancy and Resident Permissions

```text
occupancy.create
occupancy.read
occupancy.activate
occupancy.update_household
occupancy.end

move_in.create
move_in.read
move_in.complete_checklist
move_in.approve
move_in.complete

renewal.create_case
renewal.read
renewal.decide
renewal.create_offer
renewal.issue_offer
renewal.respond
renewal.complete

move_out.initiate
move_out.read
move_out.schedule_inspection
move_out.record_condition
move_out.propose_deduction
move_out.approve_deduction
move_out.complete

resident.read_own
resident.read_household
resident.update_contact
resident.request_household_change
resident.submit_maintenance
resident.view_notice
```

## 21. Maintenance Permissions

```text
maintenance.create
maintenance.read
maintenance.read_own
maintenance.triage
maintenance.assign
maintenance.change_priority
maintenance.update_status
maintenance.verify
maintenance.close
maintenance.reopen

work_order.create
work_order.read
work_order.assign_vendor
work_order.submit_quote
work_order.approve_quote
work_order.accept
work_order.update_status
work_order.request_cost_change
work_order.approve_cost_change
work_order.submit_completion
work_order.verify
work_order.close
```

## 22. Vendor Permissions

```text
vendor.create
vendor.read
vendor.update
vendor.verify
vendor.suspend
vendor.restore

vendor_member.invite
vendor_member.assign
vendor_member.revoke

vendor_compliance.upload
vendor_compliance.read
vendor_compliance.review

vendor_performance.read
vendor_performance.record
```

## 23. Legal Permissions

```text
legal_matter.create
legal_matter.read
legal_matter.read_sensitive
legal_matter.assign
legal_matter.update
legal_matter.close

legal_note.create
legal_note.read
legal_notice.create
legal_notice.approve
legal_notice.issue

agreement.legal_review
```

## 24. Document Permissions

```text
document.upload
document.read
document.read_sensitive
document.download
document.review
document.approve
document.reject
document.replace
document.archive
document.publish
document.export
document.view_access_history
```

## 25. Communication Permissions

```text
communication.create
communication.read
communication.read_internal
communication.send
communication.send_bulk
communication.manage_template
communication.view_delivery

notification.create
notification.read
notification.retry
notification.cancel
notification.manage_template
notification.configure
```

## 26. Audit Permissions

```text
audit.read
audit.read_sensitive
audit.export
audit.review_access
audit.review_financial
audit.review_security
```

Audit permissions should normally be read-only.

## 27. AI Permissions

```text
ai.application_summary.generate
ai.document_summary.generate
ai.maintenance_classification.generate
ai.communication_draft.generate
ai.listing_description.generate
ai.output.review
ai.activity.read
ai.configuration.update
```

AI permissions do not grant authority over the final business action.

## 28. Platform Owner Permission Profile

The Platform Owner may typically:

* View platform governance information
* Appoint privileged platform administrators
* Approve high-impact platform configuration
* Approve emergency platform actions
* Review major audit and security events

The Platform Owner should not use the role for routine customer operations.

The role does not automatically grant unrestricted customer-record access.

## 29. Platform Administrator Permission Profile

The Platform Administrator may typically:

* Manage platform configuration
* Manage organization lifecycle
* Manage approved integrations
* Review platform operations
* Create controlled support sessions
* View platform-level audit information

The Platform Administrator may not automatically:

* Read applicant documents
* Read resident financial records
* View legal notes
* Alter customer payments
* Approve customer applications
* Activate occupancies

Customer-data access requires a controlled support session or explicit operational authority.

## 30. Platform Support Agent Permission Profile

A Platform Support Agent may typically:

* Search for a user or organization
* Review support-safe metadata
* Create a support case
* Request temporary support access
* Troubleshoot approved workflows

A Platform Support Agent may not automatically:

* Download sensitive documents
* Verify payments
* Change agreements
* Change occupancy
* Assign privileged roles
* Export customer data

## 31. Platform Security Administrator Permission Profile

A Platform Security Administrator may typically:

* Review authentication and security events
* Revoke sessions
* Investigate suspicious activity
* Suspend compromised access
* Review privileged-role assignments
* Manage security configuration

The role should not include ordinary property or finance operations.

## 32. Organization Owner Permission Profile

The Organization Owner may typically:

* View organization profile
* Approve organization changes
* Assign Organization Administrators
* Approve privileged organization roles
* Approve organization relationships
* Review organization audit records
* Approve ownership transfer

The Organization Owner does not automatically receive all property, finance, legal, or maintenance permissions.

Those permissions require additional role assignments where applicable.

## 33. Organization Administrator Permission Profile

The Organization Administrator may typically:

* Invite members
* Assign non-privileged roles
* Request privileged role approval
* Suspend or revoke memberships
* Manage organization profile
* Configure approved organization settings
* Review access assignments

The Organization Administrator may not automatically:

* Verify payments
* Approve applicants
* Activate agreements
* Approve refunds
* Access confidential legal notes

## 34. Property Owner Administrator Permission Profile

The Property Owner Administrator may typically:

* Create and manage property records
* Approve listings
* Approve applicant decisions
* Approve offers
* Approve agreement terms
* Approve renewals
* Review occupancy
* Review financial summaries
* Approve high-impact property decisions

The role should not automatically:

* Verify a payment they personally submitted
* Approve their own privileged role elevation
* Execute unrestricted platform administration
* Access unrelated organizations

## 35. Property Operator Permission Profile

The Property Operator may typically:

* Manage communities, buildings, blocks, floors, and units
* Update operational status
* Coordinate inspections
* Manage move-in and move-out
* View resident occupancy records
* Manage maintenance
* Publish operational notices
* Coordinate vendors

The Property Operator may not automatically:

* Approve applicant decisions
* Verify payments
* Approve high-value refunds
* Change legal terms
* Assign privileged organization roles

## 36. Community Administrator Permission Profile

The Community Administrator may typically:

* Read community, building, block, and unit operational information
* Update community information
* Manage amenities
* Publish community notices
* Record facility status

The role may not automatically:

* Read applicant financial data
* Read legal notes
* Verify payments
* Approve applications
* Access other communities

## 37. Finance Reviewer Permission Profile

The Finance Reviewer may typically:

* Read charges and payments
* Review payment evidence
* Verify or reject payments
* Allocate payments
* Prepare refunds
* Reconcile financial records
* Issue receipts
* Read financial reports

The Finance Reviewer may not automatically:

* Change agreement terms
* Approve applicants
* Change property ownership
* Read unrelated legal records
* Approve their own high-value refund where separation of duties applies

## 38. Maintenance Coordinator Permission Profile

The Maintenance Coordinator may typically:

* Read maintenance requests
* Triage issues
* Set priority
* Create work orders
* Assign vendors
* Approve costs within threshold
* Verify completion
* Close maintenance requests

The role may not automatically:

* Read applicant documents
* Verify payments
* Approve agreements
* View unrelated resident financial data

## 39. Estate Agent Permission Profile

An Estate Agent may typically:

* Read assigned listings
* Read assigned inquiries
* Contact assigned prospects
* Schedule and record inspections
* Support application handoff
* Read approved listing and unit information
* Record assigned prospect activity

An Estate Agent may not automatically:

* Approve applicants
* Verify payments
* Change approved rent
* Access unrelated applications
* Read resident records
* Access confidential legal records
* Publish unapproved listings

## 40. Estate Lawyer Permission Profile

An Estate Lawyer may typically:

* Read assigned legal matters
* Read supporting records required for the matter
* Review assigned agreements
* Create legal notes
* Draft notices
* Record legal recommendations
* Close assigned matters where authorized

An Estate Lawyer may not automatically:

* Change payment status
* Approve applicants
* Manage property operations
* Access unrelated agreements
* Access unrelated residents
* Approve property refunds

## 41. Vendor Administrator Permission Profile

A Vendor Administrator may typically:

* Manage the vendor organization
* Invite vendor workers
* Read assigned work orders
* Submit quotations
* Submit invoices or cost information
* Review vendor performance data

A Vendor Administrator may not:

* Browse unrelated properties
* View unrelated resident records
* Read applicant records
* Approve their own work
* Verify completion on behalf of the property organization

## 42. Vendor Worker Permission Profile

A Vendor Worker may typically:

* Read assigned work orders
* View necessary location information
* Accept or decline assignments
* Update work status
* Submit completion evidence
* Request cost or scope clarification

A Vendor Worker may not:

* View unrelated work orders
* View resident financial data
* View applicant data
* Access legal records
* Approve completion
* Approve cost changes

Access should expire when the assignment ends.

## 43. Applicant Permission Profile

An Applicant may typically:

* Read and update their draft application
* Submit their application
* Upload required documents
* Respond to information requests
* Read approved application status
* Accept or decline an offer
* Submit payment evidence
* Read issued agreements
* Sign where authorized

An Applicant may not:

* Read internal review notes
* View other applications
* Approve their own application
* Verify their own payment
* Modify submitted controlled fields outside an amendment workflow

## 44. Primary Resident Permission Profile

A Primary Resident may typically:

* Read their occupancy
* Read approved agreements
* Read their household
* Read charges and payments
* Submit maintenance requests
* Read maintenance status
* Read notices
* Respond to renewal offers
* Initiate move-out
* Request household changes

A Primary Resident may not:

* Modify official financial records
* Alter signed agreements
* Access another household
* Close maintenance work orders
* Approve deposit deductions

## 45. Co-Resident Permission Profile

A Co-Resident may typically:

* Read approved occupancy information
* Read approved agreement information
* Submit maintenance requests
* Read notices
* Read their household role

Access to financial, renewal, and household-management actions must be configurable.

## 46. Household Member Permission Profile

A Household Member may:

* Be represented as an authorized occupant
* Receive limited access where approved
* Read selected community notices

A Household Member does not automatically receive:

* Agreement-management access
* Financial access
* Renewal authority
* Household-management authority

Minor dependants should not receive independent accounts by default.

## 47. Guarantor Permission Profile

A Guarantor may typically:

* Read the relevant guarantee obligation
* Upload required information
* Sign relevant documents
* Receive guarantee-related notices

The Guarantor may not:

* Access unrelated applicant or resident information
* Manage occupancy
* Change agreement terms
* View unrelated financial data

## 48. Former Resident Permission Profile

A Former Resident may retain limited read access to:

* Historical agreements
* Approved payment records
* Move-out records
* Deposit decisions
* Final statements
* Approved notices

Former Resident access must be time limited according to retention policy.

## 49. Assignment-Based Access

Assignment-based access is required for professional and operational workflows.

### 49.1 Listing Assignment

An assigned Estate Agent may receive:

```text
listing.read
inquiry.read
inquiry.update
inspection.create
inspection.update
```

only for the assigned listing and related prospects.

### 49.2 Application Review Assignment

An assigned reviewer may receive:

```text
application.read
application.review
application.request_information
application.read_internal_notes
```

Final decision authority remains separate.

### 49.3 Legal-Matter Assignment

An assigned Estate Lawyer may receive:

```text
legal_matter.read
legal_matter.read_sensitive
legal_note.create
agreement.legal_review
```

only for the assigned matter.

### 49.4 Payment Review Assignment

An assigned Finance Reviewer may receive:

```text
payment.read
payment.review_evidence
payment.verify
payment.reject
```

for the assigned payment queue or payment.

### 49.5 Work-Order Assignment

An assigned Vendor Worker may receive:

```text
work_order.read
work_order.accept
work_order.update_status
work_order.submit_completion
```

only for the assigned work order.

## 50. Resident Household Authorization

Resident authorization must evaluate:

* User
* Occupancy
* Household role
* Effective dates
* Agreement party status
* Household policy
* Requested action

The household model must distinguish:

* Primary Resident
* Co-Resident
* Adult Household Member
* Minor Dependant
* Authorized Occupant
* Guarantor
* Emergency Contact

These roles must not be treated as equivalent.

## 51. Field-Level Restrictions

Some records may be readable while selected fields remain restricted.

### 51.1 Applicant Record

Possible restricted fields:

* Government-identification number
* Financial eligibility details
* Internal risk notes
* Reviewer notes
* Background-check results

### 51.2 Payment Record

Possible restricted fields:

* Full bank reference
* Provider raw payload
* Payment evidence
* Reconciliation notes

### 51.3 Legal Matter

Possible restricted fields:

* Privileged legal notes
* Draft legal advice
* Confidential evidence

### 51.4 Support Record

Possible restricted fields:

* Security signals
* Internal investigation notes
* Authentication recovery details

Field-level access must be enforced server side.

## 52. Sensitive-Document Access

Sensitive document access must evaluate:

* Organization
* Role
* Scope
* Assignment
* Document sensitivity
* Related resource
* Retention status
* Legal hold
* Support-access status

Examples of sensitive documents include:

* Government identification
* Payment evidence
* Signed agreements
* Legal advice
* Applicant financial documents

Sensitive file access may require a dedicated permission such as:

```text
document.read_sensitive
```

## 53. Privileged Actions

Privileged actions include:

* Assigning privileged roles
* Transferring organization ownership
* Suspending an organization
* Exporting sensitive data
* Verifying payment
* Reversing payment
* Approving refund
* Approving applicant decision
* Approving agreement
* Activating occupancy
* Terminating agreement
* Creating support access
* Viewing sensitive audit records

Privileged actions should support:

* Multi-factor authentication
* Step-up authentication
* Reason entry
* Approval
* Audit event
* Session protection

## 54. Approval Requirements

Authorization may return `Require Approval` rather than immediate denial.

Approval-controlled actions may include:

* Privileged role assignment
* Listing publication
* Applicant decision
* Offer issue
* Charge waiver
* Payment reversal
* Refund
* Agreement issue
* Agreement termination
* Deposit deduction
* Vendor suspension
* Sensitive export
* Support session

An approval record must identify:

* Requested action
* Requester
* Approver
* Decision
* Conditions
* Date
* Related resource

## 55. Separation of Duties

Habitacores should support separation of duties for high-risk workflows.

### 55.1 Application Decision

Recommended:

```text
Agent
→ Application Reviewer
→ Property Owner Administrator
```

### 55.2 Payment Verification

Recommended:

```text
Payment Recorder
→ Finance Reviewer
→ Secondary Approver where required
```

### 55.3 Agreement Workflow

Recommended:

```text
Agreement Preparer
→ Lawyer Reviewer
→ Property Owner Approver
→ Parties Sign
```

### 55.4 Refund Workflow

Recommended:

```text
Refund Requester
→ Finance Reviewer
→ Authorized Approver
→ Executor
```

### 55.5 Vendor Work

Recommended:

```text
Vendor Performs
→ Maintenance Coordinator Verifies
→ Finance Role Processes Payment
```

The pilot may allow one person to hold multiple roles, but each action must remain separately recorded.

## 56. Self-Approval Restrictions

A user should not approve their own:

* Privileged role assignment
* Organization ownership transfer
* High-value refund
* Material financial adjustment
* Vendor invoice
* Security exception
* Sensitive export
* Applicant conflict decision

Where the founding-team pilot requires an exception, the exception must be:

* Explicit
* Audited
* Time limited where practical
* Recorded with reason
* Reviewed later

## 57. Role Conflicts

Potential conflicts include:

* Applicant and reviewer for the same application
* Agent and final decision-maker for the same application
* Vendor worker and completion verifier for the same work order
* Payment recorder and reversal approver for the same payment
* Auditor and operational editor for the same record
* Lawyer and final financial approver for the same agreement
* Support agent and customer-role administrator during an active support session

Conflict policies may be:

```text
Prohibited
Allowed with warning
Allowed with disclosure
Allowed with secondary approval
Allowed only under pilot exception
```

## 58. Role Combination Policy

A user may hold multiple roles.

However, permissions must be evaluated per:

* Active organization
* Active context
* Resource
* Scope
* Assignment
* Record state

Roles must not be flattened into uncontrolled global permission accumulation.

Example:

A user who is:

* Finance Reviewer in Organization A
* Resident in Organization B

must not receive Finance Reviewer access in Organization B.

## 59. Context Switching

Users with multiple memberships must clearly see:

* Active organization
* Active role
* Selected property or community
* Whether they are acting as a resident or organization representative

Switching context must clear or revalidate:

* Cached data
* Selected records
* Search results
* Navigation state
* Authorization state

## 60. Support Access

Support personnel must not receive unrestricted customer-data access.

A support session should include:

* Support case
* Target organization
* Authorized support user
* Approved scope
* Purpose
* Start time
* Expiry time
* Approver
* Revocation status
* Audit events

Support access may permit:

* Read-only troubleshooting
* Selected configuration review
* Controlled user-support actions

Support access must not automatically permit:

* Payment verification
* Agreement changes
* Applicant decisions
* Role elevation
* Sensitive export
* Legal-note access

## 61. Platform Emergency Access

Emergency platform access may be used only for:

* Security incident response
* Severe operational failure
* Data-protection incident
* Service restoration
* Legal obligation

Emergency access must require:

* Documented emergency
* Named actor
* Time-limited scope
* Strong authentication
* Audit record
* Post-event review

## 62. Public and Unauthenticated Access

Public users may access only explicitly public resources.

Permitted public actions may include:

```text
listing.read_public
listing.search_public
community.read_public
inquiry.create
inspection.request
account.register
```

Public users must not access:

* Applicant records
* Resident records
* Internal notes
* Payment records
* Legal matters
* Private property documents
* Audit records
* Support information

## 63. Record-State Authorization

Permissions may depend on current state.

Examples:

### Listing

```text
Draft:
listing.update

Pending Approval:
listing.approve

Approved:
listing.publish

Published:
listing.pause or listing.close
```

### Application

```text
Draft:
application.update_draft

Submitted:
application.review

Under Review:
application.request_information

Reviewed:
application.decide
```

### Payment

```text
Recorded:
payment.review_evidence

Pending Verification:
payment.verify

Verified:
payment.allocate

Reversed:
no new allocation
```

### Agreement

```text
Draft:
agreement.update_draft

Under Review:
agreement.legal_review

Approved:
agreement.issue

Signed:
agreement.activate

Active:
agreement.amend or agreement.terminate
```

## 64. Authorization Order of Evaluation

Recommended evaluation order:

1. Is the user authenticated where required?
2. Is the account active?
3. Is the organization active?
4. Is the membership active?
5. Is the requested permission assigned?
6. Is the resource within organization scope?
7. Is the resource within role scope?
8. Is an assignment required?
9. Is the assignment active?
10. Is the record in a valid state?
11. Is sensitive-field access required?
12. Is approval required?
13. Is separation of duties satisfied?
14. Is step-up authentication required?
15. Is an exception active?
16. Allow or deny

## 65. Authorization Response Behavior

A denial must not disclose unnecessary information.

Preferred outcomes:

```text
401 Unauthorized
```

when authentication is absent or invalid.

```text
403 Forbidden
```

when identity is known but permission is absent.

```text
404 Not Found
```

may be used where revealing record existence would expose sensitive information.

```text
409 Conflict
```

when the action is authorized in principle but invalid for the current record state.

```text
428 Precondition Required
```

or an equivalent application result may be used when approval or step-up authentication is required.

## 66. Denial Messages

User-facing denial messages should be understandable.

Preferred:

```text
You do not have permission to approve this application.
```

```text
This payment cannot be allocated because it has not been verified.
```

```text
Your assignment to this work order has expired.
```

Avoid exposing:

* Internal role identifiers
* Other organization names
* Sensitive record details
* Database-policy details
* Security implementation details

## 67. Authorization Audit Events

The following should generate audit events:

* Privileged role assignment
* Scope expansion
* Role revocation
* Membership suspension
* Cross-organization assignment
* Support-session activation
* Support-session termination
* Sensitive-document access
* Sensitive export
* Payment verification
* Payment reversal
* Refund approval
* Agreement activation
* Occupancy activation
* Applicant decision
* Authorization exception
* Significant denied-access attempt where appropriate

## 68. Authorization Data Model

The logical authorization model may include:

```text
users
organizations
organization_memberships
roles
permissions
role_permissions
membership_role_assignments
resource_assignments
organization_relationships
approval_requests
approval_decisions
authorization_exceptions
support_sessions
role_conflicts
audit_events
```

## 69. Permission Aggregation

A role may contain multiple permissions.

A user’s effective permissions are derived from:

```text
Active Membership
+ Active Role Assignments
+ Scope
+ Active Resource Assignments
+ Approved Relationships
- Explicit Restrictions
- Expired Access
- Suspended Access
```

Explicit restrictions should override broader grants where supported.

## 70. Permission Inheritance

Permission inheritance must remain controlled.

Example:

```text
property.read
```

may include reading:

* Communities
* Buildings
* Blocks
* Floors
* Units

only if the role has descendant-read inheritance.

It must not automatically include:

* Applicant documents
* Payment evidence
* Legal notes
* Sensitive resident information

## 71. Permission Bundles

Roles may be defined using permission bundles.

Example:

```text
Property Operator Bundle
- community.read
- building.read
- block.read
- unit.read
- unit.update
- inspection.read
- inspection.confirm
- maintenance.read
- maintenance.assign
```

Permission bundles are implementation conveniences.

The authoritative authorization decision remains permission based.

## 72. Canonical MVP Permission Matrix

Legend:

```text
A = Allowed by role
S = Allowed only within assigned scope
O = Allowed only for own records
P = Requires approval
R = Read only
— = Not permitted by default
```

### 72.1 Core Organization Matrix

| Capability              | Org Owner | Org Admin | Property Owner Admin | Property Operator | Finance Reviewer |
| ----------------------- | --------: | --------: | -------------------: | ----------------: | ---------------: |
| Read organization       |         A |         A |                    A |                 A |                A |
| Update organization     |         P |         A |                    — |                 — |                — |
| Invite members          |         P |         A |                    — |                 — |                — |
| Assign ordinary roles   |         P |         A |                    — |                 — |                — |
| Assign privileged roles |         P |         P |                    — |                 — |                — |
| Read property           |         R |         R |                    A |                 A |                R |
| Update property         |         — |         — |                    A |                 A |                — |
| Approve listing         |         — |         — |                    A |                 — |                — |
| Decide application      |         — |         — |                    A |                 — |                — |
| Verify payment          |         — |         — |                    — |                 — |                A |
| Allocate payment        |         — |         — |                    — |                 — |                A |
| Approve agreement       |         — |         — |                    A |                 — |                — |
| Activate occupancy      |         — |         — |                    P |                 A |                — |
| Manage maintenance      |         — |         — |                    R |                 A |                — |

### 72.2 Professional Matrix

| Capability                | Estate Agent |                 Estate Lawyer | Vendor Admin | Vendor Worker |
| ------------------------- | -----------: | ----------------------------: | -----------: | ------------: |
| Read assigned listing     |            S |                             — |            — |             — |
| Manage inquiry            |            S |                             — |            — |             — |
| Schedule inspection       |            S |                             — |            — |             — |
| Read assigned application |            S | S where legal matter requires |            — |             — |
| Decide application        |            — |                             — |            — |             — |
| Read assigned agreement   |            — |                             S |            — |             — |
| Legal review              |            — |                             S |            — |             — |
| Verify payment            |            — |                             — |            — |             — |
| Read assigned work order  |            — |                             — |            S |             S |
| Submit quote              |            — |                             — |            S |             — |
| Update work status        |            — |                             — |            S |             S |
| Verify work completion    |            — |                             — |            — |             — |

### 72.3 Residential Matrix

| Capability                 | Prospect | Applicant | Primary Resident |         Co-Resident | Guarantor |
| -------------------------- | -------: | --------: | ---------------: | ------------------: | --------: |
| Browse public listings     |        A |         A |                A |                   A |         A |
| Submit inquiry             |        O |         O |                O |                   O |         O |
| Request inspection         |        O |         O |                O |                   O |         O |
| Create application         |        — |         O |                — |                   — |         — |
| Update draft application   |        — |         O |                — |                   — |         — |
| Read application status    |        — |         O |                — |                   — |         S |
| Read internal review notes |        — |         — |                — |                   — |         — |
| Accept offer               |        — |         O |                — |                   — |         — |
| Read agreement             |        — |         S |                O | O subject to policy |         S |
| Read charges               |        — |         S |                O | O subject to policy |         — |
| Submit payment evidence    |        — |         O |                O | O subject to policy |         — |
| Verify payment             |        — |         — |                — |                   — |         — |
| Submit maintenance request |        — |         — |                O |                   O |         — |
| Respond to renewal         |        — |         — |                O |        Policy based |         — |
| Initiate move-out          |        — |         — |                O |        Policy based |         — |

## 73. Fine-Grained Financial Matrix

| Action                    |     Property Owner Admin | Property Operator | Finance Reviewer | Applicant/Resident |
| ------------------------- | -----------------------: | ----------------: | ---------------: | -----------------: |
| Create charge             |                        P |                 — |                A |                  — |
| Approve charge            |                        A |                 — |                P |                  — |
| Submit payment evidence   |                        — |                 — |                — |                  O |
| Record payment            |                        — |                 — |                A |                  — |
| Verify payment            |                        — |                 — |                A |                  — |
| Allocate payment          |                        — |                 — |                A |                  — |
| Request refund            |                        A |                 — |                A |  O where permitted |
| Approve refund            | A or designated approver |                 — |                P |                  — |
| Execute refund            |                        — |                 — |                A |                  — |
| Approve deposit deduction |                        A |                 P |                R |                  — |

## 74. Fine-Grained Application Matrix

| Action                         |      Estate Agent | Reviewer | Property Owner Admin | Applicant |
| ------------------------------ | ----------------: | -------: | -------------------: | --------: |
| Read assigned application      |                 S |        S |                    A |         O |
| Request information            | S where permitted |        S |                    A |         — |
| Review documents               |                 — |        S |                    A |         — |
| Record internal notes          |                 S |        S |                    A |         — |
| Approve application            |                 — |        — |                    A |         — |
| Reject application             |                 — |        — |                    A |         — |
| Respond to information request |                 — |        — |                    — |         O |
| Withdraw application           |                 — |        — |                    — |         O |

## 75. Fine-Grained Agreement Matrix

| Action                 | Property Owner Admin | Property Operator | Estate Lawyer | Primary Resident |
| ---------------------- | -------------------: | ----------------: | ------------: | ---------------: |
| Create agreement draft |                    A |                 S |             S |                — |
| Edit draft             |                    A |                 S |             S |                — |
| Legal review           |                    — |                 — |             S |                — |
| Approve agreement      |                    A |                 — |             — |                — |
| Issue agreement        |                    A |                 S |             — |                — |
| Sign agreement         |          Party based |                 — |   Party based |                O |
| Activate agreement     |                    A |                 S |             — |                — |
| Terminate agreement    |                    A |                 P | S advice only |                — |
| Read signed agreement  |                    A |                 S |             S |                O |

## 76. Fine-Grained Maintenance Matrix

| Action                     | Property Operator | Maintenance Coordinator | Vendor Admin | Vendor Worker |      Resident |
| -------------------------- | ----------------: | ----------------------: | -----------: | ------------: | ------------: |
| Create request             |                 A |                       A |            — |             — |             O |
| Triage request             |                 A |                       A |            — |             — |             — |
| Assign work order          |                 A |                       A |            — |             — |             — |
| Submit quote               |                 — |                       — |            S |             — |             — |
| Accept assignment          |                 — |                       — |            S |             S |             — |
| Update work status         |                 R |                       R |            S |             S |             R |
| Submit completion evidence |                 — |                       — |            S |             S |             — |
| Verify completion          |                 A |                       A |            — |             — | Feedback only |
| Close request              |                 A |                       A |            — |             — |             — |
| Reopen request             |                 A |                       A |            — |             — |     O request |

## 77. Authorization Exceptions

An exception record should include:

```text
rule_or_permission
user
organization
resource
scope
reason
requester
approver
effective_from
effective_to
risk
compensating_controls
status
audit_reference
```

Exceptions must not authorize:

* Cross-tenant data leakage
* Secret exposure
* False verification
* Silent agreement mutation
* Unapproved financial diversion
* Unlawful discrimination
* Deletion of required audit records

## 78. Emergency and Pilot Exceptions

The founding-team pilot may require temporary role combinations.

Examples:

* Organization Owner and Property Owner Administrator
* Property Operator and Maintenance Coordinator
* Finance Reviewer and Charge Creator

Pilot exceptions must not collapse the canonical permission model.

The implementation must still record:

* Which role authorized each action
* Which user performed it
* Whether self-approval occurred
* Why the exception was permitted

## 79. Row Level Security Alignment

Database Row Level Security should align with this permission model.

RLS may enforce:

* Organization membership
* Organization scope
* Public-record access
* Own-record access
* Assignment access
* Support-session access

RLS must not be the only authorization mechanism.

Complex decisions involving:

* Approval
* Record state
* Separation of duties
* Sensitive fields
* Cross-organization relationships

must also be enforced by application policy.

## 80. Authorization Service Architecture

A centralized authorization service should expose a conceptual interface such as:

```typescript
authorize({
  actor,
  organizationId,
  permission,
  resource,
  scope,
  assignment,
  context,
});
```

The service should return:

```typescript
{
  decision: "allow" | "deny" | "require_approval" | "require_step_up",
  reasonCode: string,
  policyVersion: string,
  auditRequired: boolean,
}
```

The final code structure will be defined during implementation.

## 81. Policy Versioning

Authorization policy changes must be versioned.

Material changes should record:

* Policy version
* Effective date
* Changed permissions
* Changed roles
* Changed scopes
* Migration impact
* Approval
* Test impact

Historic audit records should retain the policy version where practical.

## 82. Permission Seed Data

Canonical permissions and roles should be seeded through version-controlled migrations or seed scripts.

Seed operations must be:

* Idempotent
* Environment aware
* Non-destructive
* Version controlled
* Tested

Customer organizations must not be able to redefine canonical permission meaning.

Organizations may configure approved role bundles later without altering platform security invariants.

## 83. Authorization Testing

Testing must include:

1. Unauthenticated access
2. Suspended user
3. Suspended membership
4. Revoked role
5. Expired assignment
6. Same-organization access
7. Cross-organization denial
8. Property-scope inheritance
9. Unit-scoped restriction
10. Listing assignment
11. Application reviewer assignment
12. Legal-matter assignment
13. Work-order assignment
14. Resident own-record access
15. Household access
16. Field-level restriction
17. Sensitive-document access
18. Privileged action
19. Approval requirement
20. Self-approval restriction
21. Record-state restriction
22. Support session
23. Emergency access
24. Public access
25. Audit-event creation

## 84. Authorization Acceptance Criteria

Example:

```text
Requirement:
Estate Agent assignment scope

Given:
Agent A is assigned to Listing 001.
Listing 002 belongs to the same organization but is not assigned to Agent A.

When:
Agent A requests Listing 002 prospect records.

Then:
Access is denied.
Listing 002 prospect data is not returned.
The user receives an appropriate authorization response.
```

Example:

```text
Requirement:
Applicant own-record access

Given:
Applicant A owns Application 001.
Application 002 belongs to another applicant.

When:
Applicant A requests Application 002.

Then:
Access is denied.
No Application 002 information is disclosed.
```

Example:

```text
Requirement:
Payment verification separation

Given:
User A recorded Payment 001.
Organization policy prohibits self-verification.

When:
User A attempts to verify Payment 001.

Then:
The action requires another Finance Reviewer.
Payment 001 remains pending verification.
The attempt is recorded where appropriate.
```

Example:

```text
Requirement:
Vendor work-order expiry

Given:
Vendor Worker A had an assignment to Work Order 001.
The assignment has expired.

When:
Vendor Worker A requests Work Order 001.

Then:
Operational access is denied.
Historical attribution remains intact.
```

## 85. Authorization Traceability

Each permission should map to:

* Role
* Scope
* Functional requirement
* Business rule
* API operation
* Database policy
* User-interface action
* Audit event
* Automated test

Example:

```text
Permission:
payment.verify

Roles:
Finance Reviewer

Business Rules:
BR-PAY-002
BR-PAY-003

Functional Requirements:
FR-PAY-004
FR-PAY-005
FR-PAY-020

Data:
payments
payment_evidence

Audit:
PaymentVerified
PaymentRejected

Tests:
Authorized verification
Unauthorized verification
Self-verification restriction
Cross-organization verification denial
```

## 86. MVP Authorization Scope

The MVP must implement:

### Identity and Membership

* Authentication
* Active membership
* Organization context
* Role assignment
* Role revocation

### Physical Scope

* Organization
* Property
* Community
* Building
* Block
* Unit

### Assignment Scope

* Listing
* Application review
* Legal matter
* Payment review
* Work order

### Residential Scope

* Applicant own records
* Resident household records
* Guarantor relevant records
* Former Resident limited access

### Privileged Controls

* Applicant decision
* Payment verification
* Refund approval
* Agreement approval
* Occupancy activation
* Role assignment
* Sensitive export
* Support access

### Enforcement

* Server-side authorization
* Tenant-aware database access
* Audit events
* Automated authorization tests

## 87. Deferred Authorization Capabilities

The following may be deferred:

* Customer-defined custom roles
* Attribute-based policy language
* Enterprise identity federation
* Delegated regional administration
* Temporary resident guest access
* Fine-grained API-token administration
* Government reviewer access
* Insurance-partner access
* Financial-partner access
* Advanced policy simulation
* Formal privileged-access-management integration
* Just-in-time enterprise access
* Automated periodic certification campaigns

Deferred capabilities must not weaken the MVP authorization model.

## 88. Authorization Risks

### 88.1 Missing Tenant Filter

Risk:

A valid role accesses another organization’s record.

Controls:

* Explicit organization context
* Central authorization
* Tenant-aware queries
* RLS
* Cross-tenant tests

### 88.2 Role Overreach

Risk:

A role receives broader permissions than intended.

Controls:

* Canonical permission mapping
* Role review
* Least privilege
* Permission tests
* Approval for privileged roles

### 88.3 Assignment Bypass

Risk:

An agent, lawyer, or vendor accesses unrelated records.

Controls:

* Assignment checks
* Expiry
* Resource-specific policies
* Negative tests

### 88.4 Field Exposure

Risk:

A user can read a record but sees restricted fields.

Controls:

* Field-level authorization
* Response shaping
* Sensitive serializers
* Tests

### 88.5 Support Abuse

Risk:

Support access becomes permanent customer-data access.

Controls:

* Time-limited sessions
* Approval
* Visibility
* Audit
* Automatic expiry

### 88.6 Self-Approval

Risk:

One user performs incompatible preparation and approval actions.

Controls:

* Actor comparison
* Policy checks
* Secondary approval
* Pilot exception logging

### 88.7 Client-Side Trust

Risk:

UI restrictions are bypassed through direct API calls.

Controls:

* Server-side enforcement
* API tests
* Deny-by-default policy

## 89. Unresolved Authorization Decisions

The following remain pending:

* Final permission catalogue codes
* Final role-permission mappings
* Exact Property Manager role
* Exact Document Reviewer role
* Final custom-role policy
* Final support-session permissions
* Final support-session duration
* Final step-up authentication actions
* Final privileged-role approval chain
* Final refund approval thresholds
* Final maintenance cost thresholds
* Final sensitive-export approval policy
* Final resident household permissions
* Final Co-Resident financial access
* Final Guarantor document access
* Final former-resident access duration
* Final Estate Agent application visibility
* Final lawyer supporting-record access
* Final assignment inheritance rules
* Final Row Level Security implementation
* Final field-level authorization mechanism
* Final authorization caching policy
* Final policy-engine implementation
* Final denial-status strategy
* Final access-review cadence
* Final role-conflict rules
* Final emergency-access process
* Final pilot self-approval exceptions

These decisions must be resolved before the affected capability is activated in production.

## 90. Authorization Governance

Changes to the authorization model require:

1. Change description
2. Business rationale
3. Affected roles
4. Affected permissions
5. Scope impact
6. Assignment impact
7. Tenant-isolation impact
8. Security impact
9. Privacy impact
10. Financial impact
11. Legal impact
12. Resident impact
13. Support-access impact
14. Database-policy impact
15. API impact
16. User-interface impact
17. Audit impact
18. Test impact
19. Migration impact
20. Product Visionary approval
21. Chief Product Architect approval
22. Effective version

## 91. Current Permission and Authorization Decision

Habitacores will use a deny-by-default authorization model based on:

```text
Authentication
→ Organization Membership
→ Role
→ Permission
→ Scope
→ Assignment
→ Record State
→ Approval
→ Separation of Duties
→ Authorization Decision
```

The model must preserve:

* Organization isolation
* Server-side enforcement
* Least privilege
* Assignment-specific professional access
* Resident household privacy
* Sensitive-field protection
* Financial separation
* Human approval
* Role-conflict control
* Support-access restriction
* Historical attribution
* Auditable decisions

No role title, organization relationship, early-team status, record ownership, user-interface visibility, or client-supplied identifier may independently grant access to a protected Habitacores resource.

