# Document 04 — Domain Model

## Document Control

| Field | Value |
|---|---|
| Product | Habitacores |
| Document | Domain Model |
| Version | 0.2 |
| Status | Draft for approval |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Market | Nigeria |
| Last Updated | 29 July 2026 |

## 1. Purpose

This document defines the canonical business-domain model for Habitacores.

It establishes:

- Core business entities
- Canonical terminology
- Entity relationships
- Logical aggregate boundaries
- Multi-organization scoping
- Lifecycle states
- Business identifiers
- Domain invariants
- Confirmed pilot-community facts
- Provisional labels
- Unresolved domain decisions
- Rules that future database schemas, APIs, interfaces, workflows, analytics, and AI coding agents must follow

This document defines the business meaning of the Habitacores domain. It does not prescribe the final physical database schema.

## 2. Confirmed Pilot Facts

The following facts are confirmed for the initial Habitacores pilot:

- Habitacores is the platform brand.
- Nigeria is the initial market.
- The founding team owns or controls the initial residential property.
- The initial rental model is based on annual rent.
- The current residential structure contains Block A and Block B.
- Block A contains six residential units.
- The exact unit inventory for Block B remains pending confirmation.
- A functional masjid exists within the relevant property or community perimeter.
- A second building is under construction.
- The second building is located on the opposite side of the same street.
- The buildings are within the same broader community or estate context.
- A water feature exists beside or close to the second building.
- The exact classification of the water feature remains pending confirmation.
- Public names for the estate, community, and buildings remain pending.
- Final unit-numbering conventions remain pending physical-layout confirmation.

The domain model must preserve these facts without inventing missing physical, legal, financial, or branding details.

## 3. Provisional Pilot Representation

Until final public names and physical inventory details are approved, the pilot will use neutral operational labels.

```text
Pilot Property Context
└── Community 001
    ├── Existing Building
    │   ├── Block A
    │   │   └── Six residential units
    │   └── Block B
    │       └── Unit inventory pending confirmation
    ├── Functional Masjid
    └── Building Under Construction
        ├── Located opposite the Existing Building
        └── Waterside context

These labels are provisional and must not be treated as final public brands.

4. Domain Modeling Principles

The Habitacores domain model must follow these principles:

Business terminology must remain consistent across documentation and implementation.
Every organization-owned record must be scoped to an organization.
Community, building, block, floor, and unit structures must reflect physical reality.
SaaS organizations must not be confused with residential occupants.
Critical records must have explicit lifecycle states.
Historical records must remain traceable.
Charges, payments, allocations, deposits, and revenue must remain distinct concepts.
Documents must retain their business context.
Authorization must consider organization, role, property context, and assignment.
Derived information must not replace authoritative records.
Provisional names must not become permanent identifiers.
Unconfirmed facts must be marked as pending rather than inferred.
5. Canonical Terminology
5.1 Platform

The platform is the complete Habitacores software ecosystem.

It includes shared identity, organization management, property operations, leasing, resident services, maintenance, legal workflows, financial administration, documents, notifications, analytics, and audit capabilities.

5.2 Organization

An organization is a business or institutional entity participating in Habitacores.

The term organization must be used for a SaaS tenant.

Examples include:

Habitacores platform organization
Property-owning organization
Property-operating organization
Property-management company
Estate agency
Legal firm
Vendor company
Development company
Financial-service partner
Insurance partner

An organization may perform more than one business function.

5.3 User

A user is a person with a Habitacores identity.

A user may:

Belong to multiple organizations
Hold multiple roles
Have different permissions in different organizations
Be assigned to specific communities, buildings, units, matters, or work orders
5.4 Membership

A membership connects a user to an organization.

A membership defines:

Organization
User
Role or roles
Membership status
Invitation status
Effective dates
Property or community scope where applicable
Assignment limitations
Approval history
5.5 Property

A property is a legally or operationally recognized real-estate asset.

A property may represent:

One titled parcel
One development
One estate
Multiple buildings under common ownership or management
Another legally valid property structure

The exact legal-property boundaries for the pilot remain pending confirmation.

5.6 Community

A community is the primary operational context for a grouped residential environment.

A community may contain:

Buildings
Blocks
Floors
Units
Amenities
Facilities
Residents
Operators
Development activity
Community rules
Shared infrastructure

The term estate may be used publicly where appropriate, but community will remain the canonical platform term unless changed by an approved decision.

5.7 Estate

An estate is a public-facing or legal description that may correspond to a Habitacores community.

The platform must avoid assuming that every community is legally an estate.

Where both terms are used:

Community is the canonical software concept.
Estate name is an optional public or legal label.
5.8 Development

A development represents planned or active construction activity associated with a property or community.

A development may include:

Buildings
Blocks
Infrastructure
Facilities
Phases
Construction milestones
Handover activities
5.9 Phase

A phase represents a defined construction or operational stage within a development.

A phase may group:

Buildings
Blocks
Infrastructure
Facilities
Handover milestones
5.10 Building

A building is a distinct physical structure within a community.

A building may contain:

One or more blocks
Floors
Units
Shared entrances
Building-specific facilities
Building-specific amenities

For the pilot, the provisional building labels are:

Existing Building
Building Under Construction

Public building names remain pending.

5.11 Block

A block is a distinct physical or operational subdivision of a building.

For the pilot:

Block A is confirmed.
Block B is confirmed.
Block A contains six residential units.
Block B unit inventory remains pending confirmation.

A block may contain floors and units.

5.12 Floor

A floor is a physical level within a block or building.

Floor records are optional until the actual physical layout is confirmed.

The domain must not assume:

Number of floors
Ground-floor numbering
U.S.-style or Nigerian-style floor numbering
Unit sequence by floor
5.13 Unit

A unit is an independently identifiable residential or operational space.

Examples may include:

Apartment
Flat
Studio
Staff unit
Shop
Office
Utility space

For the initial MVP, the primary unit type is residential.

5.14 Amenity

An amenity is a shared feature, benefit, or service available within a property or community.

Examples may include:

Masjid
Parking
Security
Water supply
Generator
Solar infrastructure
Waste collection
Children’s play area
Garden
Community hall
5.15 Facility

A facility is a physical shared structure or operational asset.

Examples may include:

Masjid building
Security post
Borehole
Generator house
Water-treatment structure
Community office
Waste-storage area

An amenity may reference a facility.

5.16 Resident

A resident is a person legally or operationally associated with occupancy of a unit.

A resident may be:

Primary resident
Co-resident
Household member
Dependent
Authorized occupant

The standalone technical term tenant should be avoided unless its meaning is explicit.

5.17 Applicant

An applicant is a person applying to occupy a unit.

An applicant does not become an active resident until the applicable approval, agreement, payment, and move-in conditions are completed.

5.18 Household

A household groups applicants or residents associated with one application or occupancy.

A household may contain:

Primary applicant
Co-applicant
Spouse
Dependent
Child
Authorized occupant
Guarantor, where modeled separately
5.19 Prospect

A prospect is a person or household that has expressed interest in a listing but has not necessarily submitted an application.

5.20 Listing

A listing is a market-facing representation of an available unit or approved property opportunity.

A listing is not the authoritative property or unit record.

5.21 Inquiry

An inquiry records a prospect’s expressed interest in a listing, unit, building, or community.

5.22 Inspection

An inspection records a scheduled or completed property-viewing event.

Within the Nigerian rental context, inspection commonly refers to a prospective resident’s viewing appointment.

5.23 Application

An application is a formal request by an applicant or household to occupy a specific unit.

5.24 Application Review

An application review records:

Reviewer
Review action
Information request
Recommendation
Final decision
Decision reason
Timestamp
Supporting evidence
5.25 Offer

An offer records approved commercial and occupancy terms presented to an applicant before agreement activation.

5.26 Charge

A charge is an amount owed for a defined purpose.

Examples include:

Annual rent
Agency fee
Legal fee
Service charge
Caution deposit
Security deposit
Utility deposit
Inspection fee
Other approved charge

A charge is not a payment.

5.27 Payment

A payment is a monetary transfer received, initiated, or recorded against one or more charges.

5.28 Payment Allocation

A payment allocation links all or part of a payment to one or more charges.

5.29 Deposit

A deposit is a separately tracked financial obligation that may be:

Refundable
Conditionally refundable
Non-refundable, where legally and contractually valid

Deposits must not be treated automatically as revenue.

5.30 Agreement

An agreement is a legally or operationally binding document between approved parties.

For residential occupancy, the user-facing term should normally be tenancy agreement.

5.31 Occupancy

Occupancy records the operational relationship between a household or resident and a unit.

Occupancy is distinct from the agreement because:

Signing may occur before move-in.
Move-in may occur after payment.
Occupancy may end before formal document expiration in approved circumstances.
5.32 Move-In

Move-in is the controlled process through which an approved applicant becomes an active resident and receives possession or access to a unit.

5.33 Move-Out

Move-out is the controlled process through which occupancy ends and the unit is inspected, reconciled, recovered, and prepared for its next lifecycle.

5.34 Maintenance Request

A maintenance request records a reported issue, repair need, service request, or operational concern.

5.35 Work Order

A work order is an authorized task assigned to staff or a vendor.

5.36 Vendor

A vendor is an approved external organization or individual providing property-related services.

5.37 Legal Matter

A legal matter represents a legal workflow, review, dispute, notice, agreement task, or compliance activity.

5.38 Document

A document is a controlled digital file or generated record associated with one or more business entities.

5.39 Notification

A notification is a message generated in response to an event, deadline, workflow change, or manual action.

5.40 Audit Event

An audit event is a protected record of a significant user, system, administrative, or integration action.

6. Top-Level Domain Hierarchy

The primary physical-property hierarchy is:

Platform
└── Organization
    └── Property
        └── Community
            ├── Developments
            ├── Phases
            ├── Buildings
            │   ├── Blocks
            │   │   ├── Floors
            │   │   └── Units
            │   └── Building-Specific Facilities
            ├── Community Amenities
            └── Shared Facilities

The broader operational domain includes:

Organization
├── Users and Memberships
├── Properties and Communities
├── Buildings, Blocks, Floors and Units
├── Listings
├── Prospects and Inquiries
├── Inspections
├── Applications
├── Offers
├── Charges
├── Payments and Allocations
├── Agreements
├── Occupancies and Residents
├── Maintenance Requests and Work Orders
├── Vendors
├── Legal Matters
├── Documents
├── Communications and Notifications
└── Audit Events
7. Organization Domain
7.1 Organization Attributes

An organization may include:

Organization identifier
Legal name
Trading name
Organization type
Status
Country
Primary currency
Time zone
Contact information
Verification status
Created date
Updated date
7.2 Organization Types

Initial organization types may include:

Platform operator
Property owner
Property operator
Property manager
Development company
Estate agency
Legal firm
Vendor organization
Financial partner
Insurance partner

An organization may perform multiple functions.

7.3 Organization Status
Pending
→ Active
→ Suspended
→ Closed
7.4 Organization Relationship

Organizations may be connected through explicit relationships.

Examples include:

Owns
Manages
Operates
Develops
Represents
Provides legal services to
Provides maintenance services to
Finances
Insures

Each relationship should include:

Source organization
Target organization
Relationship type
Effective date
Expiration date where applicable
Status
Scope
Supporting authority where required
8. Identity and Membership Domain
8.1 User Attributes

A user may include:

User identifier
Legal or preferred name
Email
Phone number
Identity status
Authentication status
Preferred language
Time zone
Created date
Last active date
8.2 Membership Attributes

A membership must include:

User
Organization
Role assignment
Status
Invitation status
Scope
Start date
End date where applicable
Invited by
Approved by
8.3 Membership Status
Invited
→ Active
→ Suspended
→ Revoked
→ Expired
8.4 Assignment Scope

A role assignment may be:

Platform-wide
Organization-wide
Property-specific
Community-specific
Building-specific
Block-specific
Unit-specific
Legal-matter-specific
Work-order-specific

The detailed permission model will be defined separately.

9. Property and Community Domain
9.1 Property Attributes

A property may include:

Property identifier
Owning organization
Operating organization
Legal or operational name
Property type
Address
Geographic location
Status
Ownership or authority status
Community references
Created date
Updated date
9.2 Property Status
Draft
→ Under Verification
→ Active
→ Suspended
→ Disposed
→ Archived
9.3 Community Attributes

A community may include:

Community identifier
Property reference
Operational name
Public name where approved
Description
Address or location
Operational status
Community rules
Owning organization
Operating organization
Opening date where known
9.4 Community Status
Planned
→ Under Development
→ Partially Operational
→ Operational
→ Suspended
→ Closed

A community may be partially operational while additional buildings remain under construction.

9.5 Naming Status

Public-facing names for the community and buildings remain pending.

During product development and testing, the platform will use neutral operational labels such as:

Community 001
Existing Building
Building Under Construction
Block A
Block B

Final public names may be approved later without changing:

Permanent system identifiers
Property relationships
Historical records
Listings
Agreements
Payments
Audit events
10. Development Domain
10.1 Development Attributes

A development may include:

Development identifier
Property
Community
Development name or operational label
Development type
Status
Planned start date
Actual start date
Planned completion date
Actual completion date
Responsible organization
Progress percentage
Supporting documents
10.2 Development Status
Concept
→ Planned
→ Approved
→ Under Construction
→ Finishing
→ Inspection
→ Completed
→ Operational
→ Cancelled
10.3 Phase Attributes

A phase may include:

Phase identifier
Development
Name or code
Scope
Planned dates
Actual dates
Status
Completion percentage
11. Building Domain
11.1 Building Attributes

A building must include:

Building identifier
Community
Development or phase where applicable
Operational label
Public name where approved
Description
Status
Address or physical position
Owning organization
Operating organization
Planned completion date where applicable
Actual completion date where applicable
11.2 Building Status
Concept
→ Planned
→ Approved
→ Under Construction
→ Finishing
→ Inspection
→ Completed
→ Partially Operational
→ Operational
→ Temporarily Closed
→ Archived
11.3 Pilot Buildings

The pilot currently includes:

Existing Building

Confirmed details:

Located within Community 001
Contains Block A
Contains Block B
Block A contains six residential units
Block B inventory remains pending confirmation
Public building name remains pending
Building Under Construction

Confirmed details:

Located within the same broader community or estate context
Positioned opposite the Existing Building
Associated with a nearby water feature
Public building name remains pending
Internal block and unit structure remains pending confirmation
12. Block Domain
12.1 Block Attributes

A block may include:

Block identifier
Building
Block code
Operational name
Public name where applicable
Description
Status
Number of floors where confirmed
Planned unit count
Actual unit count
Completion date
12.2 Block Status
Planned
→ Approved
→ Under Construction
→ Finishing
→ Inspection
→ Completed
→ Operational
→ Temporarily Closed
→ Archived
12.3 Pilot Block Inventory

Current confirmed inventory:

Building	Block	Confirmed Unit Count
Existing Building	Block A	6
Existing Building	Block B	Pending confirmation
Building Under Construction	Pending	Pending confirmation

The system must not infer missing unit counts.

13. Floor Domain
13.1 Floor Attributes

A floor may include:

Floor identifier
Building
Block where applicable
Floor number
Floor code
Display name
Status
13.2 Floor Configuration Status

The pilot floor structure remains pending confirmation.

No floor numbering convention is approved in this version.

14. Unit Domain
14.1 Unit Attributes

A unit may include:

Unit identifier
Building
Block
Floor where applicable
Operational unit code
Public display name where applicable
Unit type
Bedroom count where confirmed
Bathroom count where confirmed
Physical description
Size where available
Operational status
Occupancy status
Listing readiness
Current agreement reference where applicable
14.2 Unit Operational Status
Planned
→ Under Construction
→ Finishing
→ Inspection
→ Available
→ Reserved
→ Occupied
→ Maintenance
→ Temporarily Unavailable
→ Retired
14.3 Unit Occupancy Status
Vacant
Reserved
Occupied
Notice Given
Move-Out Pending
Turnaround
Unavailable

Operational status and occupancy status must remain separate.

14.4 Unit Identifier Layers

Each unit must eventually support three distinct identifiers:

Permanent system identifier
Human-readable operational code
Optional public display or marketing name

Example structure:

System identifier: HAB-UNIT-000001
Operational code: Pending physical-layout approval
Public display name: Optional

No final operational unit codes are approved in this version.

15. Amenity and Facility Domain
15.1 Amenity Attributes

An amenity may include:

Amenity identifier
Property or community
Building where applicable
Name
Type
Description
Status
Access policy
Availability schedule
Responsible organization
15.2 Facility Attributes

A facility may include:

Facility identifier
Community or building
Name
Facility type
Physical location
Status
Maintenance requirements
Responsible operator
15.3 Masjid Representation

The functional masjid must be represented as:

A physical facility
A community amenity
Operational
Associated with the pilot property or community
Subject to access, maintenance, and community-operation policies

The system must not infer or record a user’s religion based solely on access to, proximity to, or use of the masjid.

15.4 Water Feature Representation

The water feature associated with the second building may be recorded as a geographic or environmental feature.

Until confirmed, the platform must not classify it specifically as:

Lake
River
Creek
Lagoon
Canal
Drainage channel

The provisional term is:

Waterside context
16. Listing Domain
16.1 Listing Attributes

A listing must reference:

Organization
Property
Community
Building
Block where applicable
Unit
Listing title
Description
Annual rent
Additional charge summary
Availability date
Media
Amenities
Publication status
Assigned agent or operator
Publication channels
16.2 Listing Status
Draft
→ Pending Review
→ Approved
→ Published
→ Paused
→ Reserved
→ Leased
→ Expired
→ Archived
16.3 Listing Rules
A listing must reference an authoritative unit or approved property context.
A unit may have a history of listings.
Only one primary active listing should normally exist for the same unit and leasing cycle.
Publication requires approved rent and fee information.
Listing status must not directly determine occupancy without an approved workflow.
A public building name is not required for internal testing.
A final public name should be approved before public marketing where practical.
17. Prospect and Inquiry Domain
17.1 Prospect Attributes

A prospect may represent:

Individual
Household
Organization
Authorized representative
17.2 Inquiry Attributes

An inquiry must include:

Inquiry identifier
Listing
Prospect
Source
Message or interest type
Assigned agent or operator
Status
Received date
Response date
17.3 Inquiry Status
New
→ Assigned
→ Contacted
→ Qualified
→ Inspection Requested
→ Application Started
→ Closed
→ Disqualified
18. Inspection Domain
18.1 Inspection Attributes

An inspection must include:

Inspection identifier
Prospect
Listing
Unit
Scheduled date and time
Assigned agent or operator
Status
Attendance
Notes
Feedback
Cancellation reason
18.2 Inspection Status
Requested
→ Confirmed
→ Rescheduled
→ Completed
→ Cancelled
→ No-Show
18.3 Inspection Rules
An inspection must reference a valid unit or listing.
Rescheduling must preserve history.
Inspection completion does not automatically approve an application.
Access instructions must be limited to authorized participants.
19. Application Domain
19.1 Application Attributes

An application must include:

Application identifier
Organization
Applicant
Household
Unit
Listing
Status
Submission date
Review status
Decision
Decision reason
Decision maker
Decision date
19.2 Applicant Data

Applicant records may include:

Identity information
Contact information
Household information
Employment or income information where lawfully required
References
Supporting documents
Consent records

Only information necessary for the approved application process should be collected.

19.3 Application Status
Started
→ Submitted
→ Under Review
→ More Information Required
→ Resubmitted
→ Approved
→ Rejected
→ Withdrawn
→ Expired
19.4 Application Decision

Possible decision values include:

Pending
Approved
Conditionally approved
Rejected
Withdrawn
Expired

Status and decision may remain distinct where operationally necessary.

19.5 Review History

Application review history must preserve:

Reviewer
Review action
Information requested
Notes
Recommendation
Final decision
Timestamp
20. Offer and Charge Domain
20.1 Offer Attributes

An offer must include:

Offer identifier
Application
Unit
Proposed tenancy period
Proposed start date
Proposed end date
Charge schedule
Expiration date
Status
Approved by
20.2 Offer Status
Draft
→ Approved
→ Issued
→ Accepted
→ Declined
→ Expired
→ Withdrawn
20.3 Charge Attributes

A charge must include:

Charge identifier
Organization
Applicant, resident, agreement, or unit context
Charge type
Description
Amount
Currency
Due date
Refundability
Recipient
Status
20.4 Initial Charge Types

Initial charge types may include:

Annual rent
Agency fee
Legal fee
Caution deposit
Security deposit
Service charge
Utility deposit
Inspection fee
Other approved charge

The existence, amount, and applicability of each charge must be configured rather than assumed.

20.5 Charge Status
Draft
→ Issued
→ Partially Paid
→ Paid
→ Overdue
→ Waived
→ Cancelled
→ Refunded
21. Payment Domain
21.1 Payment Attributes

A payment must include:

Payment identifier
Organization
Payer
Amount
Currency
Payment method
Provider reference
Bank or transfer reference
Received date
Verification status
Supporting evidence
Recorded by
21.2 Payment Status
Initiated
→ Pending
→ Received
→ Verified
→ Partially Allocated
→ Fully Allocated
→ Reversed
→ Refunded
→ Failed
21.3 Payment Allocation Attributes

A payment allocation must include:

Payment
Charge
Allocated amount
Allocation date
Allocated by
Reversal status
21.4 Payment Rules
A payment must not automatically be treated as revenue.
One payment may cover multiple charges.
One charge may be covered by multiple payments.
Allocations must not exceed the valid payment amount.
Refundable deposits must remain separately identifiable.
Manual verification must preserve the verifier and evidence.
Payment confirmation must not rely only on a user-uploaded screenshot.
22. Agreement Domain
22.1 Agreement Attributes

An agreement must include:

Agreement identifier
Organization
Property
Community
Building
Unit
Approved parties
Start date
End date
Rent terms
Charge references
Version
Status
Prepared by
Reviewed by
Approved by
Signed dates
22.2 Agreement Status
Draft
→ Legal Review
→ Approved
→ Issued
→ Partially Signed
→ Fully Signed
→ Active
→ Renewal Pending
→ Expired
→ Terminated
→ Superseded
22.3 Agreement Parties

Agreement parties may include:

Property-owning organization
Property-operating organization
Primary resident
Co-resident
Guarantor
Legal representative
22.4 Agreement Rules
Signed agreement history must be preserved.
New versions must not overwrite signed versions.
Only one active primary tenancy agreement should normally govern a unit at a time.
Overlapping agreement periods require explicit validation or an approved exception.
Agreement activation may depend on approved payment and move-in conditions.
23. Occupancy and Resident Domain
23.1 Resident Attributes

A resident may include:

Resident identifier
User reference where applicable
Applicant source
Contact details
Resident status
Household role
Organization context
Community context
Unit context
23.2 Occupancy Attributes

An occupancy must include:

Occupancy identifier
Unit
Agreement
Household
Primary resident
Start date
End date
Status
Move-in record
Move-out record
23.3 Occupancy Status
Pending
→ Move-In Scheduled
→ Active
→ Notice Given
→ Move-Out Scheduled
→ Ended
→ Cancelled
23.4 Resident Status
Prospective
→ Approved
→ Pending Move-In
→ Active
→ Notice Given
→ Former
→ Suspended
24. Move-In and Move-Out Domain
24.1 Move-In Record

A move-in record may include:

Occupancy
Scheduled date
Actual date
Handover checklist
Unit condition
Meter readings
Keys issued
Access credentials
Responsible operator
Resident acknowledgement
24.2 Move-Out Record

A move-out record may include:

Occupancy
Notice date
Scheduled date
Actual date
Final inspection
Unit condition
Outstanding charges
Deposit decision
Keys returned
Responsible operator
25. Maintenance Domain
25.1 Maintenance Request Attributes

A maintenance request must include:

Request identifier
Organization
Community
Building
Block where applicable
Unit or facility
Reporter
Category
Description
Priority
Status
Assigned operator
Created date
Target resolution date
25.2 Maintenance Status
Submitted
→ Acknowledged
→ Under Review
→ Assigned
→ Scheduled
→ In Progress
→ Awaiting Parts
→ Awaiting Approval
→ Completed
→ Resident Confirmation
→ Closed
→ Reopened
→ Cancelled
25.3 Maintenance Priority
Emergency
Urgent
High
Normal
Low

Priority definitions must be documented in the business-rules specification.

26. Work Order Domain
26.1 Work Order Attributes

A work order must include:

Work-order identifier
Maintenance request or operational source
Assigned staff member or vendor
Scope
Schedule
Cost estimate
Approval status
Completion evidence
Actual cost
Status
26.2 Work-Order Status
Draft
→ Awaiting Approval
→ Approved
→ Assigned
→ Scheduled
→ In Progress
→ Completed
→ Verified
→ Closed
→ Cancelled
27. Vendor Domain
27.1 Vendor Attributes

A vendor may be an individual or organization.

A vendor record may include:

Vendor identifier
Organization
Vendor type
Service categories
Contact information
Verification status
Compliance records
Status
Performance history
27.2 Vendor Status
Pending
→ Verified
→ Active
→ Suspended
→ Inactive
→ Rejected
28. Legal Domain
28.1 Legal Matter Attributes

A legal matter must include:

Matter identifier
Organization
Property, unit, application, agreement, resident, or vendor context
Matter type
Assigned lawyer
Status
Priority
Open date
Close date
Outcome
28.2 Legal Matter Types

Examples include:

Agreement preparation
Agreement review
Ownership review
Notice preparation
Dispute
Termination
Compliance review
Recovery action
Advisory matter
28.3 Legal Matter Status
Opened
→ Assigned
→ Under Review
→ Awaiting Information
→ Action Required
→ Resolved
→ Closed
→ Reopened
29. Document Domain
29.1 Document Attributes

A document must include:

Document identifier
Organization
Document type
File reference
Record owner
Related business entity
Version
Status
Sensitivity classification
Uploaded by
Uploaded date
Expiration date where applicable
29.2 Document Status
Draft
→ Uploaded
→ Under Review
→ Approved
→ Rejected
→ Superseded
→ Expired
→ Archived
29.3 Document Types

Examples include:

Ownership document
Applicant identity document
Application support document
Tenancy agreement
Payment evidence
Inspection report
Maintenance evidence
Vendor certificate
Legal notice
Development plan
Construction-progress record
30. Communication and Notification Domain
30.1 Communication Attributes

A communication record may include:

Sender
Recipient
Organization
Related business entity
Channel
Subject
Message
Timestamp
Delivery status
30.2 Notification Attributes

A notification must include:

Notification identifier
Recipient
Notification type
Trigger
Channel
Delivery status
Read status
Related resource
Created date
30.3 Supported Channels

Potential notification channels include:

In-app
Email
SMS
WhatsApp
Push notification

Only approved and configured channels should be active.

31. Audit Domain
31.1 Audit Event Attributes

An audit event must include:

Audit identifier
Organization
Actor
Actor role
Action
Resource type
Resource identifier
Previous value where appropriate
New value where appropriate
Reason
Timestamp
Source
Request or correlation identifier
31.2 Auditable Events

Examples include:

Role changes
Property verification
Building or unit status changes
Listing approval
Application decisions
Charge changes
Payment verification
Agreement approval
Agreement status changes
Resident activation
Maintenance closure
Vendor approval
Document replacement
Organization suspension
32. Business Identifier Strategy

Internal primary keys should not be exposed as the only human-readable identifiers.

Suggested business identifiers include:

Organization: HAB-ORG-000001
Property:     HAB-PROP-000001
Community:    HAB-COM-000001
Building:     HAB-BLD-000001
Block:        HAB-BLK-000001
Unit:         HAB-UNIT-000001
Listing:      HAB-LIST-000001
Inquiry:      HAB-INQ-000001
Inspection:   HAB-INSP-000001
Application:  HAB-APP-000001
Offer:        HAB-OFR-000001
Charge:       HAB-CHG-000001
Agreement:    HAB-AGR-000001
Payment:      HAB-PAY-000001
Occupancy:    HAB-OCC-000001
Maintenance:  HAB-MNT-000001
Work Order:   HAB-WO-000001
Legal Matter: HAB-LEGAL-000001
Document:     HAB-DOC-000001

Final identifier formats will be confirmed during database and API design.

33. Multi-Organization Scoping

Every organization-owned domain entity must include or inherit organization scope.

Organization-scoped entities include:

Properties
Communities
Developments
Buildings
Blocks
Floors
Units
Amenities
Facilities
Listings
Prospects
Inquiries
Inspections
Applications
Offers
Charges
Payments
Agreements
Residents
Occupancies
Maintenance requests
Work orders
Vendors
Legal matters
Documents
Notifications
Audit events

Cross-organization access must require:

Explicit business relationship
Approved assignment
Valid role
Resource scope
Platform-authorized administrative access
34. Logical Aggregate Boundaries

The initial logical aggregate boundaries are:

Organization Aggregate
Organization
Memberships
Organization relationships
Roles
Assignments
Property Aggregate
Property
Community
Development
Phase
Building
Block
Floor
Unit
Amenity
Facility
Listing Aggregate
Listing
Listing media
Inquiry
Inspection
Application Aggregate
Application
Applicant
Household
Supporting documents
Review history
Decision
Leasing Aggregate
Offer
Charges
Agreement
Agreement parties
Occupancy
Move-in
Move-out
Payment Aggregate
Payment
Payment allocation
Refund
Reconciliation record
Maintenance Aggregate
Maintenance request
Work orders
Assignments
Completion evidence
Legal Aggregate
Legal matter
Legal assignments
Notices
Legal documents

These are logical boundaries and do not require separate microservices.

35. Prohibited Ambiguities

The following terminology must be clarified whenever used:

Ambiguous Term	Required Clarification
Tenant	Organization or resident
Property	Legal asset, community, building, block, or unit
Estate	Legal estate, public name, or community
Payment	Payment, charge, allocation, deposit, or revenue
Lease	Agreement, agreement period, or occupancy
Owner	User, organization, legal owner, record owner, or platform owner
Agent	Estate agent, software agent, or AI agent
Building	Physical structure or public development name
Block	Physical subdivision or informal building label
Status	Entity-specific lifecycle status
Verification	Identity, organization, property, payment, document, or vendor verification
Waterside	Lake, river, creek, lagoon, canal, drainage, or other feature
36. Core Domain Invariants

The platform must enforce or validate these invariants:

A building belongs to a valid community.
A block belongs to a valid building.
A floor belongs to a valid block or building structure.
A unit belongs to a valid building and block context.
An organization-scoped record cannot silently move between organizations.
A listing must reference an authoritative unit or approved property opportunity.
An application must reference a valid applicant and unit.
An offer must reference an approved application or authorized leasing process.
An active agreement must reference valid parties and a valid unit.
Overlapping active agreements require validation.
A payment cannot be allocated beyond its valid available amount.
A charge cannot be marked paid without sufficient allocation or approved adjustment.
A deposit must remain distinguishable from revenue.
Resident activation requires an approved occupancy workflow.
A work order must reference an authorized maintenance or operational need.
Critical status changes must generate audit events.
Signed agreements must not be overwritten.
Documents must retain organization and resource context.
Cross-organization access requires explicit authorization.
Provisional public names must not replace permanent identifiers.
Unconfirmed physical details must not be represented as confirmed data.
37. Initial MVP Domain Scope

The MVP must implement the minimum domain necessary to support:

One founding organization
The initial property context
Community 001
Existing Building
Block A
Six Block A units
Block B
Functional masjid
Building Under Construction
Waterside context
Internal users and memberships
Listings
Prospects
Inquiries
Inspections
Applications
Offers
Charges
Payments
Agreements
Residents
Occupancies
Move-ins
Maintenance requests
Work orders
Documents
Notifications
Audit events

Records for unconfirmed Block B and second-building inventory may exist provisionally but must not contain invented unit details.

38. Deferred Domain Areas

The following areas are deferred beyond the MVP:

Mortgage origination
Rent financing
Insurance underwriting
Advanced property accounting
Public vendor marketplace
Government property-registry integration
Automated applicant scoring
International tax logic
Multi-currency settlement
Full construction procurement
Investment syndication
Property sales transactions
Native mobile-application-specific domain models

Deferred areas must not distort the MVP domain.

39. Unresolved Domain Decisions

The following decisions remain open:

Final community or estate name
Final names of both buildings
Exact legal-property boundaries
Exact address structure
Exact classification of the water feature
Block B unit count
Second-building block structure
Second-building unit count
Number of floors in each structure
Unit numbering convention
Bedroom and bathroom inventory
Public marketing names
Final ownership and operating-entity names
Final fee types and amounts
Final tenancy-agreement terminology by jurisdiction

These decisions must be resolved through later specifications or Architecture Decision Records.

40. Domain Model Governance

Changes to canonical terminology or core relationships require:

A documented change proposal
Identification of affected specifications
Review of database implications
Review of API implications
Review of migration implications
Product Visionary approval
Chief Product Architect approval
Glossary update
Architecture Decision Record where material
41. Current Domain Model Decision

Habitacores will use an organization-scoped, community-first physical hierarchy:

Organization
→ Property
→ Community
→ Building
→ Block
→ Floor
→ Unit

The pilot will be represented provisionally as:

Community 001
├── Existing Building
│   ├── Block A
│   │   └── Six residential units
│   └── Block B
├── Functional Masjid
└── Building Under Construction
    └── Waterside context

The rental lifecycle will be modeled through distinct but connected concepts:

Listing
→ Inquiry
→ Inspection
→ Application
→ Offer
→ Charge
→ Payment
→ Agreement
→ Occupancy
→ Resident Operations
→ Renewal or Move-Out

This model preserves physical accuracy, business clarity, financial separation, auditability, and future multi-organization scalability without prematurely inventing unresolved property details.