# Document 14 — UX Architecture and Information Architecture

## Document Control

| Field | Value |
|---|---|
| Product | Habitacores |
| Document | UX Architecture and Information Architecture |
| Version | 0.1 |
| Status | Draft for approval |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Market | Nigeria |
| Primary Currency | Nigerian naira |
| Primary Operational Time Zone | West Africa Time |
| Primary Tenant Boundary | Organization |
| Primary Experience | Responsive web application |
| Future Experience Channels | Native mobile applications and approved partner interfaces |
| Last Updated | 30 July 2026 |

## 1. Purpose

This document defines the User Experience Architecture and Information Architecture for Habitacores.

It translates the approved:

- Product vision
- Product strategy
- Product principles
- Business architecture
- Domain model
- Users, personas, and roles
- Business rules
- Functional requirements
- Non-functional requirements
- System architecture
- Data architecture
- Permission matrix
- User journeys
- API architecture

into a coherent, role-aware, responsive, accessible, and implementation-ready user experience.

This document defines:

- Experience principles
- Portal boundaries
- Application shells
- Navigation structures
- Page hierarchy
- Route architecture
- Dashboard composition
- Role-aware workspaces
- Organization and property context
- Search and filtering
- Forms and validation
- Task queues
- Status presentation
- Approval interfaces
- Sensitive-data presentation
- Notifications
- Activity history
- Empty, loading, error, conflict, and unauthorized states
- Responsive behavior
- Low-bandwidth behavior
- Accessibility requirements
- UX acceptance criteria
- UX traceability
- MVP experience scope
- Deferred experience capabilities
- UX governance

This document does not define final visual branding, exact colors, production typography, finalized component implementation, high-fidelity mockups, or native mobile application screens.

Those areas will be addressed through the UI Design System, prototypes, implementation, and later approved design decisions.

## 2. UX Architecture Objectives

The Habitacores UX architecture must:

1. Make complex property workflows understandable.
2. Present only actions the user is authorized to perform.
3. Preserve organization and tenant boundaries visibly and technically.
4. Support multiple roles without creating context confusion.
5. Support end-to-end annual-rent operations.
6. Support public prospects, applicants, residents, operators, professionals, vendors, and platform users.
7. Preserve financial, legal, agreement, and occupancy integrity.
8. Clearly distinguish drafts, submissions, approvals, verifications, and final states.
9. Provide reliable task visibility.
10. Support low-bandwidth environments.
11. Work effectively on mobile browsers.
12. Meet accessibility requirements.
13. Preserve progress during long workflows.
14. Provide safe recovery from errors.
15. Avoid exposing sensitive information.
16. Support the founding-team pilot without hardcoding the pilot.
17. Scale to multiple organizations, properties, communities, and users.
18. Reduce training burden through consistent patterns.
19. Support human review of AI-generated assistance.
20. Provide implementation teams with clear screen and navigation boundaries.

## 3. Experience Principles

### 3.1 Clarity Before Density

Habitacores may contain extensive operational information, but screens must prioritize the user’s immediate goal.

### 3.2 Context Must Be Visible

Users must always understand:

- Which organization they are acting within
- Which role is active
- Which property or community is selected
- Which record they are changing
- Whether an action is final or provisional

### 3.3 Actions Reflect Authority

The interface must not present unauthorized actions as though they are available.

Client-side visibility does not replace server-side authorization.

### 3.4 State Must Be Explicit

Users must be able to distinguish:

- Draft
- Submitted
- Pending review
- Information required
- Pending approval
- Approved
- Rejected
- Pending verification
- Verified
- Active
- Completed
- Cancelled
- Expired
- Archived

### 3.5 Evidence Is Not Verification

Uploading a document or payment screenshot must not be visually represented as approval or verification.

### 3.6 Recommendation Is Not Decision

AI output, reviewer commentary, agent notes, and legal recommendations must be visually distinct from authoritative decisions.

### 3.7 Safe Defaults

The interface should default to the least risky action.

### 3.8 Progressive Disclosure

Advanced or sensitive controls should appear only when relevant.

### 3.9 Preserve Work

Drafts and long-form workflows should preserve entered information wherever practical.

### 3.10 Explain Consequences

High-impact actions must clearly explain their effect before confirmation.

## 4. Experience Channels

Habitacores will initially support:

### 4.1 Public Web Experience

For:

- Browsing listings
- Viewing listing details
- Submitting inquiries
- Requesting inspections
- Registering an account
- Authentication

### 4.2 Authenticated Personal Portal

For:

- Applicants
- Guarantors
- Residents
- Co-Residents
- Household members
- Former Residents

### 4.3 Organization Operations Portal

For:

- Organization Owners
- Organization Administrators
- Property Owner Administrators
- Property Operators
- Property Managers
- Community Administrators
- Finance Reviewers
- Maintenance Coordinators
- Document Reviewers
- Development Managers

### 4.4 Professional Workspaces

For:

- Estate Agents
- Estate Lawyers
- Vendor Administrators
- Vendor Workers
- Contractors

### 4.5 Platform Administration Portal

For:

- Platform Owners
- Platform Administrators
- Platform Support Agents
- Platform Security Administrators
- Platform Auditors

### 4.6 Future Native Mobile Experiences

Future mobile applications may use the same approved information architecture and APIs while applying mobile-native navigation patterns.

## 5. Portal Boundary Model

The user-facing architecture is divided into the following primary portals:

```text
Public Marketplace
Applicant Portal
Resident Portal
Organization Operations Portal
Professional Workspace
Vendor Workspace
Platform Administration Portal
````

A single authenticated person may have access to more than one portal.

The interface must provide controlled context switching rather than combining unrelated roles into one ambiguous dashboard.

## 6. Global Experience Architecture

```text
Habitacores
├── Public Marketplace
├── Authentication
├── Personal Portal
│   ├── Applicant Workspace
│   ├── Guarantor Workspace
│   ├── Resident Workspace
│   └── Former Resident Records
├── Organization Portal
│   ├── Portfolio
│   ├── Marketplace Operations
│   ├── Applications
│   ├── Finance
│   ├── Agreements
│   ├── Occupancy
│   ├── Maintenance
│   ├── Vendors
│   ├── Documents
│   ├── Communications
│   ├── Reports
│   └── Administration
├── Professional Portal
│   ├── Agent Workspace
│   ├── Lawyer Workspace
│   └── Contractor Workspace
└── Platform Portal
    ├── Customer Organizations
    ├── Support
    ├── Security
    ├── Integrations
    ├── Configuration
    └── Audit
```

## 7. Application Shells

Habitacores should use separate but visually related application shells.

### 7.1 Public Shell

Contains:

* Habitacores identity
* Marketplace navigation
* Search
* Sign in
* Create account
* Help
* Approved public footer links

### 7.2 Personal Portal Shell

Contains:

* Personal dashboard
* Applications
* Offers
* Payments
* Agreements
* Occupancy
* Maintenance
* Notices
* Documents
* Account settings

Navigation changes according to the user’s active personal relationship.

### 7.3 Organization Portal Shell

Contains:

* Organization switcher
* Active role
* Portfolio context
* Primary navigation
* Task indicator
* Notifications
* Search
* User menu

### 7.4 Professional Workspace Shell

Contains:

* Professional organization context
* Assignment queue
* Assigned resources
* Messages
* Documents
* Activity
* Profile

### 7.5 Platform Shell

Contains:

* Platform scope
* Organization search
* Support cases
* Security events
* Integration status
* Platform audit
* Configuration

## 8. Global Header

The authenticated global header should contain:

* Habitacores logo or wordmark
* Organization context where applicable
* Property or community context where applicable
* Global search where permitted
* Task or work indicator
* Notification center
* Help or support
* User profile menu

The header must not expose navigation links solely because a user has another unrelated role in another organization.

## 9. Organization Context Switcher

The organization context switcher must display:

* Organization name
* Organization type
* Active membership
* Active role or role set
* Status
* Optional property context

When switching organizations, the system must:

1. Validate membership.
2. Clear organization-specific navigation state.
3. Clear tenant-specific search results.
4. Clear unauthorized cached data.
5. Reload available permissions.
6. Return the user to an appropriate dashboard.

## 10. Property Context Switcher

Property, community, or operational context may be selected beneath the organization context.

Example:

```text
Organization
→ Property
→ Community
```

The selector may display:

* All authorized properties
* Recently used properties
* Search
* Favorite or pinned properties
* Current property
* Current community

Users must not be able to select a property outside their scope.

## 11. Active Role Presentation

Where a user has multiple roles, the interface should display the active operational role.

Example:

```text
Acting as:
Finance Reviewer
```

Role switching must not create permissions not already granted.

The role label helps users understand why particular actions are available.

## 12. Primary Navigation Architecture

The Organization Operations Portal may use the following primary navigation:

```text
Overview
Portfolio
Marketplace
Applications
Finance
Agreements
Occupancy
Maintenance
Vendors
Documents
Communications
Reports
Administration
```

Navigation items must be permission aware.

## 13. Secondary Navigation

Secondary navigation should organize related capabilities.

Example:

```text
Finance
├── Overview
├── Charges
├── Payments
├── Verification Queue
├── Allocations
├── Deposits
├── Refunds
├── Receipts
└── Reconciliation
```

## 14. Breadcrumb Architecture

Deep operational pages should provide breadcrumbs.

Example:

```text
Portfolio
> Community 001
> Existing Building
> Block A
> Unit 01
```

Example:

```text
Applications
> HAB-APP-000001
> Review
```

Breadcrumbs must not expose unauthorized ancestor resources.

## 15. URL and Page Hierarchy

Illustrative organization routes:

```text
/app/organizations/{organizationId}
/app/organizations/{organizationId}/overview
/app/organizations/{organizationId}/properties
/app/organizations/{organizationId}/applications
/app/organizations/{organizationId}/finance
/app/organizations/{organizationId}/agreements
/app/organizations/{organizationId}/occupancies
/app/organizations/{organizationId}/maintenance
```

Illustrative record routes:

```text
/app/organizations/{organizationId}/applications/{applicationId}
/app/organizations/{organizationId}/payments/{paymentId}
/app/organizations/{organizationId}/agreements/{agreementId}
/app/organizations/{organizationId}/work-orders/{workOrderId}
```

Personal routes may use:

```text
/portal
/portal/applications
/portal/payments
/portal/agreements
/portal/occupancies
/portal/maintenance
```

## 16. Route Security

A route must not reveal record existence merely because the URL is valid.

Unauthorized routes may return:

* Safe not-found experience
* Unauthorized experience
* Access-expired experience
* Assignment-expired experience

The exact response depends on security policy.

## 17. Dashboard Architecture

Dashboards should answer:

1. What requires my attention?
2. What changed?
3. What is at risk?
4. What should I do next?
5. What is the current operational position?

Dashboards should not become undifferentiated collections of charts.

## 18. Dashboard Content Priority

Preferred order:

```text
Urgent tasks
→ Pending approvals
→ Exceptions and failures
→ Time-sensitive deadlines
→ Operational summaries
→ Recent activity
→ Analytics
```

## 19. Personal Dashboard

The personal dashboard may display:

* Active application status
* Outstanding information requests
* Current offers
* Outstanding charges
* Pending payment verification
* Signature requests
* Move-in tasks
* Active occupancy
* Maintenance updates
* Renewal deadlines
* Move-out tasks
* Notices

The dashboard must adapt to the user’s active lifecycle stage.

## 20. Organization Overview Dashboard

The organization overview may display:

* Properties and units
* Available units
* Published listings
* New inquiries
* Pending inspections
* Applications under review
* Payments pending verification
* Agreements awaiting action
* Upcoming move-ins
* Open maintenance requests
* Renewal cases
* Operational alerts

## 21. Role-Specific Dashboard Composition

### Finance Reviewer

* Payments pending verification
* Unallocated verified payments
* Refunds pending review
* Reconciliation exceptions
* Charges due
* Recent financial actions

### Estate Agent

* Assigned inquiries
* Upcoming inspections
* Uncontacted prospects
* Follow-up tasks
* Assigned listings
* Expiring assignments

### Estate Lawyer

* Agreements pending legal review
* Assigned legal matters
* Notices awaiting preparation
* Deadlines
* Recently updated matters

### Maintenance Coordinator

* Emergency or urgent requests
* Untriaged requests
* Unassigned work orders
* Quotes awaiting approval
* Work pending verification
* Overdue work orders

### Vendor Worker

* Assigned work orders
* Work due today
* Work awaiting acceptance
* Cost-change responses
* Completion requests

## 22. Task Center

Habitacores should provide a consolidated task center.

Task categories may include:

* Assigned to me
* Awaiting my approval
* Awaiting information
* Due soon
* Overdue
* Escalated
* Failed
* Completed recently

A task should include:

* Title
* Resource
* Organization
* Property context
* Due date
* Priority
* Status
* Required action
* Assignment

## 23. Task Generation

Tasks should be generated from authoritative workflow state.

Examples:

* Application requires review
* Payment requires verification
* Agreement requires approval
* Work order requires acceptance
* Renewal case requires decision
* Document requires review

Users must not manually mark authoritative workflow tasks complete without completing the underlying action.

## 24. Task Detail

Task detail should show:

* Why the task exists
* Required action
* Relevant record summary
* Due date
* Current status
* Who assigned it
* Related documents
* History
* Available actions

## 25. Record Summary Pattern

Major records should use a consistent summary header.

Example:

```text
HAB-APP-000001
Application

Status: Under Review
Applicant: [Authorized Name]
Unit: Block A — Unit 01
Submitted: 30 July 2026
Assigned Reviewer: [Name]
```

The summary header should contain:

* Record type
* Business identifier
* Primary label
* Status
* Key relationships
* Primary actions
* Context actions
* Overflow menu

## 26. Record Detail Tabs

A complex record may use tabs such as:

```text
Overview
Details
Documents
Activity
Communications
Decisions
Audit
```

Tabs must be permission aware.

Users without audit permission must not see an Audit tab.

## 27. Public Marketplace Architecture

The Public Marketplace should contain:

```text
Home
Search Listings
Listing Details
Request Inspection
Submit Inquiry
Sign In
Create Account
Help
```

## 28. Public Marketplace Home

The public home page may include:

* Primary value proposition
* Listing search
* Featured approved listings
* Approved community information
* How the process works
* Trust and safety information
* Registration prompt
* Support information

The home page must not present unverified claims about properties or communities.

## 29. Listing Search Experience

Listing search should support:

* Location
* Annual rent range
* Unit type
* Bedroom count
* Bathroom count
* Availability
* Approved amenities

Search results should display:

* Primary image
* Listing title
* General location
* Annual rent
* Unit type
* Availability
* Approved key features
* Inspection action

## 30. Listing Detail Experience

Listing detail may contain:

* Image gallery
* Public name
* Location summary
* Annual rent
* Mandatory disclosed charges
* Availability
* Unit information
* Approved amenities
* Community information
* Inspection request
* Inquiry action
* Application eligibility

The interface must clearly distinguish:

* Annual rent
* Deposit
* Agency fee
* Legal fee
* Service charge
* Other approved charges

## 31. Applicant Portal Architecture

The Applicant Portal may use:

```text
Dashboard
Applications
Offers
Payments
Agreements
Documents
Messages
Notifications
Profile
```

## 32. Application Progress Architecture

An application should display a progress structure such as:

```text
1. Personal Information
2. Household
3. Co-Applicants or Guarantor
4. Supporting Documents
5. Declarations
6. Review
7. Submit
```

Each step should show:

* Not started
* In progress
* Complete
* Requires correction
* Locked after submission

## 33. Application Autosave

Application forms should:

* Save after meaningful changes
* Display last saved time
* Show saving status
* Prevent duplicate submissions
* Preserve unsaved input where practical
* Warn before leaving with unsaved changes

Example:

```text
Saved 30 seconds ago
```

## 34. Submitted Application Experience

After submission, the Applicant Portal should show:

* Submission confirmation
* Application reference
* Current status
* Expected next step
* Outstanding requests
* Document status
* Offer status where applicable
* Communication history

Submitted controlled fields should be read-only unless an amendment workflow is opened.

## 35. Information Request Experience

An information request should show:

* What is required
* Why it is required
* Due date
* Requested documents or answers
* Requesting organization
* Response status
* Submit-response action

Internal review commentary must remain hidden.

## 36. Application Decision Presentation

Applicant-facing decision screens must:

* Use clear language
* Distinguish approval, conditional approval, rejection, and expiration
* Show actionable next steps
* Avoid exposing internal notes
* Preserve decision history
* Show offer or closure action where applicable

## 37. Offer Experience

An issued offer should clearly show:

* Unit
* Annual rent
* Currency
* Proposed start and end dates
* Charges
* Conditions
* Payment deadline
* Offer expiry
* Accept
* Decline
* Request clarification

Material terms should be visually prominent.

## 38. Payment Experience

The personal payment experience should separate:

```text
Charge
Payment submission
Verification
Allocation
Receipt
```

A payment submitted by the user should display:

```text
Pending verification
```

not:

```text
Paid
```

until the approved verification and allocation workflow is complete.

## 39. Payment Status Language

Preferred user-facing states:

* Payment not submitted
* Evidence submitted
* Pending verification
* Additional information required
* Verified
* Rejected
* Partially allocated
* Fully allocated
* Reversed
* Refunded

## 40. Receipt Experience

Receipts should be available through:

* Payment detail
* Charge detail
* Receipt list
* Secure document access

Receipt information should include:

* Receipt number
* Amount
* Currency
* Payment date
* Recipient
* Charge allocation
* Download or view action

## 41. Agreement Experience

Agreement screens should contain:

* Agreement reference
* Current version
* Parties
* Unit
* Term
* Annual rent
* Charges
* Signature status
* Activation status
* Related documents
* Version history

## 42. Agreement Signature Experience

Before signing, the user must be shown:

* Exact agreement version
* Material terms
* Signer identity
* Signature declaration
* Consent
* Download or view capability
* Decline option where permitted

Signed or issued versions must appear immutable.

## 43. Resident Portal Architecture

The Resident Portal may contain:

```text
Home
Occupancy
Agreement
Charges and Payments
Maintenance
Notices
Household
Renewal
Move-Out
Documents
Messages
Profile
```

## 44. Resident Home

The Resident Home should display:

* Active occupancy
* Outstanding charges
* Payment status
* Open maintenance requests
* Recent notices
* Upcoming renewal
* Move-out status
* Important agreement dates

## 45. Household Experience

The household screen should display:

* Primary Resident
* Co-Residents
* Authorized occupants
* Dependants
* Pending household-change requests
* Effective dates
* Role descriptions

The interface must not imply that all household members have equal authority.

## 46. Maintenance Submission Experience

The resident maintenance form should include:

* Unit or facility
* Category
* Description
* Priority indicators
* Access notes
* Media upload
* Emergency guidance
* Submit action

The system should not promise emergency response unless such service is operationally supported.

## 47. Maintenance Tracking Experience

Residents should see:

* Request reference
* Category
* Submitted date
* Current status
* Scheduled visit
* Assigned organization where appropriate
* Recent updates
* Completion status
* Reopen request

Internal vendor pricing and confidential operational notes should remain hidden.

## 48. Renewal Experience

The renewal screen should show:

* Current agreement expiry
* Renewal status
* Offered annual rent
* New term
* Charges
* Response deadline
* Accept
* Decline
* Request clarification

## 49. Move-Out Experience

The move-out experience should show:

* Notice requirements
* Requested date
* Confirmed date
* Final inspection
* Access-item return
* Condition review
* Deposit decision
* Refund status
* Final documents

## 50. Organization Portfolio Architecture

The Portfolio area should support:

```text
Properties
Communities
Buildings
Blocks
Floors
Units
Amenities
Developments
```

## 51. Portfolio Tree View

A hierarchical view may display:

```text
Property
└── Community
    └── Building
        └── Block
            └── Floor
                └── Unit
```

The tree must support:

* Expand and collapse
* Search
* Status indicators
* Record counts
* Permission-aware actions
* Responsive alternative for small screens

## 52. Portfolio List View

List views should support:

* Search
* Filter
* Sort
* Pagination
* Status
* Context
* Bulk selection where safe
* Saved views later

## 53. Pilot Property Presentation

The pilot may be represented as:

```text
Community 001
├── Existing Building
│   ├── Block A
│   │   └── Six confirmed residential units
│   └── Block B
├── Functional Masjid
└── Building Under Construction
    └── Waterside context
```

The interface must not invent:

* Block B unit count
* Second-building inventory
* Floor structure
* Public names
* Unit numbering
* Exact water-feature classification

Unknown values should display:

```text
Not yet confirmed
```

rather than:

```text
0
```

## 54. Unit Detail Experience

Unit detail should contain:

* Operational identifier
* Display label
* Hierarchy
* Unit type
* Readiness
* Listing status
* Occupancy status
* Maintenance status
* Current listing
* Current occupancy
* Agreement references
* Documents
* Activity

## 55. Marketplace Operations Architecture

Marketplace Operations may contain:

```text
Listings
Inquiries
Inspections
Agents
Marketplace Activity
```

## 56. Listing Management Experience

Listing management should support:

* Draft
* Submit for approval
* Approve
* Reject
* Publish
* Pause
* Close
* Media
* Charge disclosure
* Assigned agent
* Version history

Primary actions must reflect status.

## 57. Inquiry Queue

Inquiry queue columns may include:

* Inquiry reference
* Prospect
* Listing
* Source
* Assigned agent
* Status
* Created time
* Last contact
* Next action

## 58. Inspection Calendar and List

Inspection management may provide:

* Calendar view
* List view
* Pending confirmation
* Upcoming
* Completed
* Rescheduled
* Cancelled
* No-show

Calendar entries must respect property and assignment scope.

## 59. Application Operations Architecture

The Applications area may contain:

```text
Overview
Submitted
Under Review
Information Required
Decision Queue
Approved
Rejected
Expired
All Applications
```

## 60. Application Review Workspace

The reviewer workspace should include:

* Applicant summary
* Unit and listing
* Application sections
* Parties
* Documents
* Information requests
* Review checklist
* Internal notes
* Recommendation
* Activity history

The reviewer must not be able to silently edit applicant-submitted content.

## 61. Decision Workspace

The decision workspace should contain:

* Application summary
* Reviewer recommendation
* Supporting evidence
* Conditions
* Decision options
* Internal rationale
* Applicant-facing communication
* Conflict warning
* Confirmation

## 62. Finance Architecture

Finance navigation may contain:

```text
Overview
Charges
Payments
Verification Queue
Allocations
Deposits
Refunds
Receipts
Reconciliation
Reports
```

## 63. Payment Verification Workspace

The workspace should show:

* Payment reference
* Payer
* Amount
* Currency
* Date
* Method
* Evidence
* Charge context
* Duplicate indicators
* Provider or bank reference
* Verification history
* Verify action
* Reject action
* Request-information action

The Verify action must use a high-confidence confirmation pattern.

## 64. Financial Warning Patterns

Warnings should appear for:

* Duplicate provider reference
* Amount mismatch
* Currency mismatch
* Over-allocation
* Payment already reversed
* Charge already settled
* Self-verification conflict
* Organization mismatch

## 65. Payment Allocation Workspace

The allocation interface should display:

* Verified payment amount
* Already allocated amount
* Unallocated balance
* Eligible charges
* Charge outstanding balances
* Proposed allocations
* Remaining balance
* Transaction confirmation

The user must not enter an allocation greater than either available payment balance or charge balance.

## 66. Refund Workspace

Refund detail should show:

* Refund reference
* Source payment
* Related charge or deposit
* Requested amount
* Approved amount
* Recipient
* Reason
* Review status
* Approval status
* Execution status
* Provider reference
* Activity

## 67. Agreement Operations Architecture

Agreement navigation may contain:

```text
Templates
Drafts
Legal Review
Pending Approval
Issued
Awaiting Signature
Active
Expiring
Terminated
All Agreements
```

## 68. Agreement Review Workspace

The agreement review screen should show:

* Version
* Template
* Parties
* Unit
* Financial terms
* Dates
* Clauses
* Legal comments
* Approval history
* Version comparison
* Required actions

## 69. Agreement Version Comparison

A comparison experience may display:

* Added content
* Removed content
* Changed terms
* Changed amounts
* Changed dates
* Changed parties

The original issued version must remain accessible.

## 70. Occupancy Operations Architecture

Occupancy navigation may contain:

```text
Planned
Pending Move-In
Active
Pending Move-Out
Ended
Household Changes
Move-In Calendar
Move-Out Calendar
```

## 71. Move-In Workspace

The move-in workspace should show:

* Resident
* Unit
* Agreement
* Payment conditions
* Unit readiness
* Checklist
* Condition report
* Access items
* Schedule
* Outstanding blockers
* Complete move-in action

The final action must be disabled or denied when blockers remain.

## 72. Maintenance Operations Architecture

Maintenance navigation may contain:

```text
Overview
Triage Queue
Requests
Work Orders
Quotes
Pending Verification
Vendors
Categories
Reports
```

## 73. Maintenance Triage Workspace

The triage workspace should include:

* Request description
* Resident-submitted evidence
* Unit or facility
* Category
* Suggested category where AI is used
* Priority
* Access notes
* Existing related requests
* Create work order
* Resolve internally
* Request information

AI suggestions must be labeled and editable.

## 74. Work-Order Workspace

Work-order detail should show:

* Work-order reference
* Source request
* Location
* Scope
* Priority
* Assigned vendor
* Assigned worker
* Quote
* Approved cost
* Status
* Schedule
* Completion evidence
* Verification
* Activity

## 75. Vendor Workspace Architecture

Vendor navigation may contain:

```text
Dashboard
Assignments
Work Orders
Quotes
Completion Requests
Compliance Documents
Performance
Team
```

## 76. Vendor Assignment Experience

A vendor should see only:

* Assigned work
* Required location details
* Approved access instructions
* Work scope
* Schedule
* Approved cost information
* Relevant communication

The vendor must not see unrelated resident, applicant, agreement, or financial records.

## 77. Legal Workspace Architecture

Legal navigation may contain:

```text
Dashboard
Assigned Matters
Agreement Reviews
Legal Notices
Documents
Deadlines
Activity
```

## 78. Legal Matter Workspace

A legal matter should show:

* Matter reference
* Type
* Related resource
* Parties
* Confidentiality
* Assignment
* Deadlines
* Notes
* Documents
* Notices
* Status
* Activity

Sensitive legal information requires enhanced presentation controls.

## 79. Document Center Architecture

The Document Center may support:

```text
All Documents
Pending Review
Approved
Rejected
Expiring
Sensitive
Archived
```

## 80. Document List

Document-list fields may include:

* Document title
* Type
* Related resource
* Owner or uploader
* Status
* Sensitivity
* Uploaded date
* Expiry
* Current version
* Review status

## 81. Document Viewer

The document viewer should provide:

* File preview where supported
* Download
* Version
* Metadata
* Related resource
* Sensitivity label
* Review status
* Expiration
* Access history where permitted

Highly sensitive files should not load automatically when unnecessary.

## 82. Communication Center

Communication areas may contain:

```text
Inbox
Sent
Drafts
Threads
Templates
Delivery Failures
```

Communication history should be linked to the relevant business resource.

## 83. Notification Center

The Notification Center should support:

* Unread
* All
* Action required
* Informational
* Failed or unresolved where operational
* Mark as read
* Open related record

Notifications must not serve as the only source of authoritative status.

## 84. Global Search

Global search may support:

* Business identifiers
* Properties
* Units
* Listings
* Applications
* Payments
* Agreements
* Occupancies
* Maintenance requests
* Work orders
* Vendors
* Documents

Search results must be:

* Tenant scoped
* Permission aware
* Field restricted
* Categorized
* Paginated

## 85. Search Results Architecture

Search results may be grouped:

```text
Applications
Payments
Agreements
Units
Maintenance
```

Each result should display:

* Resource type
* Business identifier
* Primary label
* Status
* Context
* Last updated

## 86. Filters

Common filter patterns should include:

* Status
* Date range
* Property
* Community
* Assignment
* Priority
* User
* Amount range
* Expiry
* Document type
* Vendor

Filters should show active values visibly.

## 87. Saved Views

Future saved views may allow users to preserve:

* Filters
* Sorting
* Visible columns
* Scope
* Page size

Saved views must not preserve access to records after permissions change.

## 88. Table Architecture

Operational tables should support:

* Column headings
* Sort
* Filters
* Pagination
* Selection where safe
* Row actions
* Status
* Empty state
* Loading state
* Responsive alternative

Tables must not rely exclusively on horizontal scrolling on small screens.

## 89. Responsive Table Alternatives

On smaller screens, tables may transform into:

* Cards
* Stacked rows
* Priority fields
* Expandable details
* Limited primary actions

Critical information must remain visible.

## 90. Bulk Actions

Bulk actions should be limited to low-risk or strongly governed operations.

Potential bulk actions:

* Assign reviewer
* Send approved reminder
* Archive low-risk records
* Export approved records
* Update category

Bulk actions should generally not include:

* Verify payments
* Approve applications
* Activate agreements
* Activate occupancies
* Approve refunds
* Terminate agreements

## 91. Form Architecture

Forms should use:

* Clear sections
* Descriptive labels
* Help text
* Required-field indicators
* Inline validation
* Summary validation
* Save draft
* Cancel
* Submit
* Confirmation
* Recovery

## 92. Validation Presentation

Validation should appear:

* Near the affected field
* In an error summary
* With clear correction guidance
* Without clearing valid input

Example:

```text
Enter an amount greater than zero.
```

Avoid:

```text
Invalid input.
```

## 93. Required and Optional Fields

Required fields must be visibly identified.

Optional fields should not be presented as required merely because they exist in the data model.

## 94. Unknown and Pending Fields

When a fact is unknown, the interface should permit:

* Not yet confirmed
* Pending verification
* Not applicable
* Not provided
* Unknown

The system must not force inaccurate placeholder data.

## 95. Multi-Step Forms

Multi-step forms should include:

* Step title
* Progress
* Completion status
* Back
* Continue
* Save and exit
* Error summary
* Review before submission

## 96. Destructive Action Pattern

Destructive or irreversible actions should require:

* Clear action name
* Consequence summary
* Affected record
* Reason
* Confirmation
* Additional authentication where required

Examples:

* Revoke membership
* Reverse payment
* Terminate agreement
* Retire unit
* Suspend organization

## 97. Confirmation Dialog Pattern

A confirmation dialog should state:

* What will happen
* What will not happen
* Whether the action can be reversed
* Which records will be affected
* Required approval
* Primary and secondary actions

Avoid generic:

```text
Are you sure?
```

Preferred:

```text
Verify this payment?

This will make the payment eligible for allocation. It will not automatically settle a charge until an allocation is completed.
```

## 98. Approval Interface Pattern

Approval screens should contain:

* Requester
* Requested action
* Resource
* Reason
* Supporting evidence
* Risk or conflict warnings
* Approve
* Reject
* Return for revision
* Approval history

## 99. Separation-of-Duties Presentation

Where self-approval is prohibited, the interface should show:

```text
Another authorized reviewer must approve this action because you prepared the request.
```

The interface must not hide the action and leave the user uncertain why it is unavailable.

## 100. Status Badge Architecture

Status badges should:

* Use text, not color alone
* Remain consistent across modules
* Avoid ambiguous labels
* Distinguish pending, approved, failed, and archived states
* Include accessible names

## 101. Status Timeline

Critical records should show a chronological status timeline.

Example:

```text
Application created
Application submitted
Reviewer assigned
Information requested
Applicant responded
Review completed
Application approved
```

## 102. Activity Timeline

Activity may include:

* Status changes
* Assignments
* Comments
* Document uploads
* Approvals
* Notifications
* Provider updates
* System events

Activity visibility must follow permissions.

## 103. Empty States

An empty state should explain:

* What the page contains
* Why no records are present
* What the user can do next

Example:

```text
No payments are awaiting verification.

New submissions will appear here after applicants or residents submit payment evidence.
```

## 104. Loading States

Loading states should:

* Preserve page structure where practical
* Avoid layout movement
* Show progress for long operations
* Provide cancellation when supported
* Avoid falsely appearing complete

## 105. Error States

Error states should contain:

* Plain-language summary
* Retry action
* Safe navigation
* Correlation identifier for material failures
* Support pathway where appropriate

## 106. Conflict States

A conflict occurs when the user had authority but the record changed.

Example:

```text
This application was updated by another reviewer.

Refresh the page to review the current version before continuing.
```

The interface must not silently overwrite the newer state.

## 107. Unauthorized State

Unauthorized pages should state:

```text
You do not have permission to access this record.
```

Where disclosure itself is sensitive, the interface may use a safe not-found state.

## 108. Assignment-Expired State

Professional users should see:

```text
Your assignment to this record has ended.
```

The interface may offer:

* Return to assignments
* Contact administrator
* View permitted historical activity where allowed

## 109. Session-Expired State

When authentication expires:

* Preserve unsent draft where practical
* Prompt reauthentication
* Return the user to the prior safe location
* Revalidate permissions after authentication

## 110. Offline and Network-Loss State

The interface should distinguish:

* Offline
* Slow network
* Save failed
* Upload failed
* Action pending
* Retry available

Users must not receive a success confirmation until the server confirms the action.

## 111. File Upload Experience

File upload should provide:

* Accepted types
* Maximum size
* Upload progress
* Validation status
* Scan status
* Success
* Failure
* Retry
* Replace

## 112. Document Status Language

Preferred document states:

* Uploading
* Processing
* Pending scan
* Pending review
* Approved
* Rejected
* Replacement required
* Expired
* Archived

## 113. Sensitive Data Presentation

Sensitive data should use:

* Redaction
* Masking
* Reveal controls
* Permission checks
* Step-up authentication where required
* Access logging
* Clear sensitivity labels

Example:

```text
Bank reference:
••••••••4821
```

## 114. Government Identification Presentation

Government-identification information should:

* Be hidden by default
* Show only necessary fields
* Require appropriate permission
* Avoid appearing in lists
* Avoid appearing in global search snippets
* Avoid browser caching where practical

## 115. Financial Data Presentation

Financial screens should display:

* Amount
* Currency
* Financial state
* Source record
* Allocation
* Outstanding balance
* Verification status
* Reversal status
* Refund status

Financial totals must not mix currencies without clear separation.

## 116. AI-Assisted UX

AI-assisted features must display:

* AI-generated label
* Source context where appropriate
* Review requirement
* Regenerate action
* Edit capability
* Feedback where approved
* Failure state
* Human-authoritative action

## 117. AI Summary Presentation

An application summary should be labeled:

```text
AI-generated summary — Review before use
```

The summary must not visually replace the source application.

## 118. AI Draft Communication

AI-generated communication must remain in draft state until a user:

* Reviews it
* Edits where necessary
* Approves it
* Sends it

## 119. AI Failure Presentation

Preferred message:

```text
The AI summary is temporarily unavailable. You can continue reviewing the application manually.
```

## 120. Notification Design

Notifications should contain:

* Action or event
* Resource
* Time
* Priority
* Required next step
* Secure link

Sensitive data should not be included unnecessarily in email, SMS, or WhatsApp messages.

## 121. In-App Versus External Notification

The in-app record remains authoritative.

External notifications should direct the user back to the secure application.

## 122. Responsive Breakpoints

The final breakpoint system will be defined in the Design System.

The experience should support:

* Small mobile
* Large mobile
* Tablet
* Laptop
* Desktop
* Wide desktop

## 123. Mobile Navigation

Personal portals may use:

* Bottom navigation
* Compact top navigation
* More menu

Operational portals may use:

* Collapsible side navigation
* Context drawer
* Mobile task list
* Simplified tables

## 124. Mobile Action Placement

Critical actions should remain reachable without excessive scrolling.

High-impact actions should not be placed so close to ordinary actions that accidental activation is likely.

## 125. Low-Bandwidth UX

The experience should:

* Load critical text before large media
* Use optimized images
* Avoid unnecessary animations
* Use progressive loading
* Preserve drafts
* Support retryable uploads
* Avoid repeated downloads
* Use concise payloads
* Provide direct task links

## 126. Accessibility Standard

Habitacores should target:

```text
WCAG 2.2 Level AA
```

## 127. Keyboard Accessibility

All interactive features must support keyboard operation.

This includes:

* Navigation
* Menus
* Tabs
* Dialogs
* Tables
* Filters
* File uploads
* Date selection
* Status actions
* Approvals

## 128. Focus Management

After:

* Opening a dialog
* Submitting a form
* Encountering validation errors
* Changing steps
* Completing a command

focus should move to the appropriate location.

## 129. Screen Reader Support

Interfaces should provide:

* Semantic headings
* Form labels
* Landmark regions
* Status announcements
* Error summaries
* Accessible names
* Table headers
* Progress descriptions

## 130. Color Independence

Status and meaning must not rely on color alone.

Use:

* Text
* Icons
* Labels
* Patterns
* Accessible contrast

## 131. Reduced Motion

Users who request reduced motion should not receive unnecessary animation.

## 132. Date and Time Presentation

Dates should display in the relevant context.

Example:

```text
30 July 2026, 6:00 PM WAT
```

Where useful, the interface may additionally show relative time:

```text
Due in 2 days
```

## 133. Currency Presentation

Nigerian naira should be displayed clearly.

Example:

```text
₦500,000.00
```

Currency code may be shown where ambiguity exists:

```text
NGN 500,000.00
```

## 134. Localization Architecture

Stable codes should remain independent from user-facing labels.

The MVP may operate primarily in English.

The UX architecture should support future:

* Localized labels
* Localized notifications
* Localized date formatting
* Localized currency formatting
* Additional Nigerian and regional languages

## 135. Help Architecture

Help should be contextual.

Potential support elements:

* Field help
* Process explanation
* Status explanation
* Frequently asked questions
* Contact support
* Report a problem
* Correlation identifier

## 136. User Onboarding

Role-specific onboarding may include:

* Welcome
* Organization context
* Role explanation
* Primary navigation
* First task
* Security setup
* Notification preferences
* Help resources

## 137. Organization Onboarding

Organization onboarding may include:

1. Organization profile
2. Verification
3. Primary users
4. Property setup
5. Role assignment
6. Configuration
7. Payment setup
8. Notification setup
9. Pilot readiness
10. Go-live review

## 138. Product Tour

Product tours should be:

* Optional
* Role specific
* Dismissible
* Restartable
* Short
* Contextual

## 139. Support Session UX

When platform support has approved temporary access, the interface should visibly display:

```text
Support session active
```

The session indicator should include:

* Purpose
* Scope
* Expiry
* End-session action
* Customer visibility where required

## 140. Emergency Access UX

Emergency access should use a visually distinct environment with:

* Emergency banner
* Purpose
* Actor
* Time remaining
* Restricted actions
* Audit reminder
* End session

## 141. Platform Administration Information Architecture

Platform navigation may include:

```text
Overview
Organizations
Users
Support Cases
Support Sessions
Security
Integrations
Feature Configuration
Audit
System Health
```

## 142. Platform Organization Summary

The platform organization summary may show:

* Organization identity
* Type
* Status
* Verification
* Membership count
* Property count
* Integration health
* Support cases
* Security signals
* Subscription status later

Platform users should not see unrestricted customer records by default.

## 143. Platform Support Workspace

Support workspace should show:

* Case
* Organization
* User
* Issue category
* Safe metadata
* Troubleshooting actions
* Support-access request
* Communication
* Status
* Resolution

## 144. Security Workspace

Security workspace may show:

* Suspicious authentication
* Privileged-role changes
* Session revocations
* Emergency access
* Sensitive exports
* Support sessions
* Security incidents

## 145. Reports Information Architecture

Reports may be organized into:

```text
Portfolio
Marketplace
Applications
Finance
Occupancy
Maintenance
Vendors
Renewals
Audit
```

## 146. Report Filter Architecture

Reports should provide:

* Date range
* Organization
* Property
* Community
* Status
* Assigned user
* Currency where applicable
* Export action

## 147. Analytics Presentation

Analytics should:

* Show definitions
* Show data period
* Show filters
* Show update time
* Avoid misleading precision
* Link to operational records where authorized

## 148. Export UX

Export requests should show:

* Export type
* Filters
* Included fields
* Sensitivity
* Approval requirement
* Estimated processing
* Status
* Expiry
* Secure download

## 149. Audit UX

Audit views should support:

* Actor
* Action
* Resource
* Date
* Previous value
* New value
* Reason
* Correlation identifier
* Filters
* Export where permitted

Audit views should be read-only.

## 150. Navigation Permission Behavior

When a user lacks access:

* The navigation item may be hidden.
* Direct access must still be denied.
* The absence of a navigation item must not be the only control.

When a user has read access but not action access:

* The page remains visible.
* Unauthorized actions are hidden or disabled with explanation.

## 151. Disabled Versus Hidden Actions

Hide an action when:

* The user never has authority in the current context.
* Revealing the action would create confusion.
* The resource should not be disclosed.

Disable with explanation when:

* The user may become eligible after a state change.
* A prerequisite is incomplete.
* Another approval is required.
* The action is temporarily unavailable.

## 152. Action Priority

Each screen should have:

* One clear primary action
* Limited secondary actions
* Overflow actions for low-frequency tasks
* Destructive actions separated visually

## 153. Page Title Pattern

Page titles should include:

* Resource or workspace name
* Business identifier where relevant
* Status
* Context
* Primary action

## 154. Content Density

Operational users may require dense views.

Density should be managed through:

* Clear hierarchy
* Tabs
* Collapsible sections
* Summaries
* Filters
* Adjustable table columns later
* Progressive disclosure

## 155. Cross-Module Links

Related records should be linked where authorized.

Examples:

```text
Application
→ Offer
→ Charge
→ Payment
→ Agreement
→ Occupancy
```

Cross-module links must not bypass permissions.

## 156. Record Relationship Panel

Complex records may include a relationship panel showing:

* Parent record
* Child records
* Related documents
* Related communications
* Related tasks
* Related financial records

## 157. Draft Versus Final Visual Treatment

Drafts should be clearly labeled.

Final, approved, issued, signed, verified, or active records should use stronger authoritative presentation.

## 158. Version History UX

Versioned records should show:

* Current version
* Prior versions
* Created date
* Created by
* Approval
* Effective date
* Superseded status
* View comparison

## 159. Time-Sensitive UX

Records approaching deadlines should show:

* Due date
* Time remaining
* Urgency
* Required action
* Escalation status

Examples:

* Offer expiry
* Payment deadline
* Agreement expiry
* Document expiry
* Inspection time
* Renewal response deadline

## 160. UX Metrics

Potential UX metrics include:

* Task completion rate
* Form abandonment
* Application completion time
* Payment-evidence failure rate
* Search success
* Navigation errors
* Upload retries
* Accessibility defects
* Mobile completion rate
* Support cases per workflow
* Time to locate assigned task

Metrics must not collect unnecessary sensitive data.

## 161. UX Testing Strategy

Testing should include:

* Navigation testing
* Role-based usability testing
* Mobile testing
* Low-bandwidth testing
* Keyboard testing
* Screen-reader testing
* Form validation testing
* Draft recovery testing
* Authorization-state testing
* Error-state testing
* Conflict-state testing
* Sensitive-data exposure testing
* End-to-end workflow testing

## 162. Persona-Based UX Testing

Tests should cover:

* Public Prospect
* Applicant
* Guarantor
* Primary Resident
* Co-Resident
* Estate Agent
* Finance Reviewer
* Estate Lawyer
* Property Operator
* Maintenance Coordinator
* Vendor Worker
* Organization Administrator
* Platform Support Agent

## 163. UX Acceptance Criterion — Organization Context

```text
Given:
A user belongs to Organization A and Organization B.

When:
The user switches from Organization A to Organization B.

Then:
Organization A navigation state, search results, selected records, and cached tenant data are cleared.

And:
The interface reloads Organization B permissions and context.
```

## 164. UX Acceptance Criterion — Payment Evidence

```text
Given:
An applicant uploads payment evidence successfully.

When:
The upload completes.

Then:
The payment is displayed as Pending verification.

And:
The related charge is not displayed as settled.

And:
The agreement and occupancy workflows remain blocked where verification is required.
```

## 165. UX Acceptance Criterion — Unauthorized Record

```text
Given:
An Estate Agent is assigned to Listing 001 only.

When:
The agent opens a direct link to Listing 002 operational records.

Then:
The interface does not display Listing 002 information.

And:
The user receives a safe unauthorized or not-found experience.
```

## 166. UX Acceptance Criterion — Unknown Pilot Data

```text
Given:
Block B unit count has not been confirmed.

When:
An authorized user views Block B.

Then:
The interface displays Not yet confirmed.

And:
The interface does not display zero confirmed units.

And:
The interface does not generate placeholder units.
```

## 167. UX Acceptance Criterion — Agreement Immutability

```text
Given:
An agreement version has been issued.

When:
An authorized user views the agreement.

Then:
The issued version is read-only.

And:
A material change requires Create Amendment or Create New Version.
```

## 168. UX Acceptance Criterion — Self-Approval

```text
Given:
A user created a high-value refund request.
The organization policy prohibits self-approval.

When:
The user opens the approval interface.

Then:
The Approve action is unavailable.

And:
The interface explains that another authorized approver is required.
```

## 169. UX Acceptance Criterion — Mobile Application Submission

```text
Given:
An applicant uses a supported mobile browser.

When:
The applicant completes and submits an application.

Then:
All required sections, document uploads, validation messages, declarations, and submission confirmation remain usable without desktop access.
```

## 170. UX Acceptance Criterion — AI Failure

```text
Given:
A reviewer requests an AI application summary.

When:
The AI provider fails.

Then:
The interface shows that AI assistance is unavailable.

And:
The reviewer can continue manual review.

And:
No application status changes.
```

## 171. UX Traceability

Each screen should map to:

* User role
* User journey
* Permission
* Business rule
* Functional requirement
* API endpoint
* Domain entity
* Data record
* Audit event
* Automated test

Example:

```text
Screen:
Payment Verification Detail

Journey:
JRN-FIN-004
JRN-FIN-005
JRN-FIN-006

Role:
Finance Reviewer

Permissions:
payment.read
payment.review_evidence
payment.verify
payment.reject

APIs:
GET /payments/{paymentId}
POST /payments/{paymentId}/verify
POST /payments/{paymentId}/reject

Audit:
PaymentVerified
PaymentRejected
```

## 172. MVP Public Experience Scope

The MVP must support:

* Public marketplace
* Listing search
* Listing detail
* Inquiry
* Inspection request
* Registration
* Authentication

## 173. MVP Applicant Experience Scope

The MVP must support:

* Applicant dashboard
* Draft application
* Application parties
* Document upload
* Submission
* Information requests
* Status
* Decisions
* Offers
* Payment evidence
* Agreement review and signature

## 174. MVP Resident Experience Scope

The MVP must support:

* Resident dashboard
* Occupancy
* Agreement
* Charges and payments
* Receipts
* Household
* Maintenance
* Notices
* Renewal
* Move-out
* Documents

## 175. MVP Organization Experience Scope

The MVP must support:

* Organization context
* Property hierarchy
* Listings
* Inquiries
* Inspections
* Application review
* Decisions
* Finance
* Agreements
* Occupancy
* Maintenance
* Vendors
* Documents
* Communications
* Reports
* Memberships
* Roles
* Assignments
* Configuration
* Audit

## 176. MVP Professional Experience Scope

The MVP must support:

* Agent assignments
* Lawyer assignments
* Vendor assignments
* Assigned-record navigation
* Restricted supporting context
* Task queues
* Communication
* Document access
* Assignment expiry

## 177. Deferred UX Capabilities

The following may be deferred:

* Native mobile applications
* Fully customizable dashboards
* Customer-created navigation
* Advanced saved views
* Drag-and-drop workflow builders
* Public developer portal
* In-app video calling
* Real-time collaborative document editing
* Interactive property maps
* Smart-building controls
* Biometric experiences
* Automated applicant scoring interfaces
* Autonomous agent interfaces
* Multi-currency account dashboards
* Property-sale interfaces
* Mortgage interfaces
* Insurance interfaces
* Investment dashboards
* Marketplace bidding
* Advanced dispute-resolution portal

Deferred capabilities must not distort the MVP information architecture.

## 178. UX Risks

### 178.1 Context Confusion

Risk:

A user performs an action in the wrong organization or role.

Controls:

* Visible organization context
* Visible role
* Context confirmation
* Tenant-specific navigation
* Safe switching

### 178.2 False Financial Status

Risk:

Uploaded evidence appears as verified payment.

Controls:

* Separate status language
* Verification workflow
* Allocation presentation
* Clear charge balance

### 178.3 Excessive Navigation

Risk:

Users cannot find critical tasks.

Controls:

* Role-aware navigation
* Task center
* Dashboard priorities
* Global search
* Contextual links

### 178.4 Sensitive Data Exposure

Risk:

Sensitive information appears in lists, search, notifications, or error messages.

Controls:

* Field masking
* Permission-aware presentation
* Redacted previews
* Secure documents
* UX security tests

### 178.5 Mobile Workflow Failure

Risk:

Users cannot complete applications, uploads, payments, or maintenance tasks on mobile.

Controls:

* Responsive testing
* Mobile-first critical flows
* Progressive forms
* Retry
* Reduced payloads

### 178.6 Overloaded Dashboards

Risk:

Dashboards become difficult to understand.

Controls:

* Task-first design
* Role-specific composition
* Progressive disclosure
* Limited primary metrics

### 178.7 Pilot Hardcoding

Risk:

Pilot labels or counts become permanent product assumptions.

Controls:

* Neutral operational labels
* Unknown-state support
* Stable identifiers
* Configurable hierarchy

## 179. Unresolved UX Decisions

The following remain pending:

* Final Habitacores visual identity
* Final logo
* Final typography
* Final color system
* Final component library
* Final navigation layout
* Final mobile-navigation pattern
* Final organization-switcher design
* Final property-context design
* Final dashboard widgets
* Final table-density options
* Final saved-view support
* Final global-search implementation
* Final form autosave interval
* Final offline-draft strategy
* Final file-upload limits
* Final document-viewer provider
* Final calendar component
* Final notification-center behavior
* Final in-app communication model
* Final status-color mapping
* Final confirmation-dialog standards
* Final step-up authentication interface
* Final support-session banner
* Final emergency-access presentation
* Final report visualization library
* Final accessibility testing tools
* Final user research plan
* Final design-prototype platform
* Final native mobile design timeline
* Final localization roadmap
* Final public marketplace content
* Final pilot public names
* Final Block A unit labels
* Final Block B inventory
* Final second-building structure
* Final water-feature classification

These decisions must be resolved before the affected interface becomes production dependent.

## 180. UX Governance

Changes to approved UX Architecture require:

1. Change description
2. User problem
3. Affected persona
4. Affected journey
5. Navigation impact
6. Permission impact
7. Tenant-isolation impact
8. Data impact
9. Financial impact
10. Agreement impact
11. Occupancy impact
12. Privacy impact
13. Security impact
14. Accessibility impact
15. Mobile impact
16. Low-bandwidth impact
17. API impact
18. Component impact
19. Analytics impact
20. Test impact
21. Migration impact
22. Product Visionary approval
23. Chief Product Architect approval
24. Effective version

## 181. Current UX Architecture Decision

Habitacores will use a role-aware, organization-scoped, responsive information architecture organized around:

```text
Public Marketplace
→ Personal Portal
→ Organization Operations Portal
→ Professional Workspaces
→ Platform Administration
```

The authenticated experience will use:

```text
User Identity
→ Active Organization
→ Active Role
→ Property or Assignment Context
→ Relevant Navigation
→ Task or Record
→ Authorized Action
→ Explicit Status
→ Confirmation
→ Activity and Audit
```

The experience must preserve:

* Organization clarity
* Role clarity
* Assignment scope
* Explicit workflow state
* Financial truth
* Payment-verification distinction
* Immutable agreement presentation
* Occupancy integrity
* Resident privacy
* Sensitive-data protection
* Human approval
* AI transparency
* Task visibility
* Mobile usability
* Low-bandwidth usability
* Accessibility
* Error recovery
* Future SaaS extensibility

No hidden client state, navigation visibility, role label, uploaded file, payment screenshot, AI output, provider callback, cached page, direct URL, pilot-team relationship, or visual confirmation may independently authorize or finalize a protected Habitacores business action.