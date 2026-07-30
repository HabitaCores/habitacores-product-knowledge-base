# Document 05 — Users, Personas, and Roles

## Document Control

| Field | Value |
|---|---|
| Product | Habitacores |
| Document | Users, Personas, and Roles |
| Version | 0.1 |
| Status | Draft for approval |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Market | Nigeria |
| Last Updated | 29 July 2026 |

## 1. Purpose

This document defines the canonical user, persona, membership, and role model for Habitacores.

It establishes:

- User categories
- Primary personas
- Internal and external stakeholders
- Organization memberships
- Role definitions
- Role scope
- Multi-role users
- Assignment-based access
- Resident household roles
- Separation of duties
- Role conflicts
- MVP roles
- Deferred roles
- Governance requirements

This document defines business responsibilities and access intent.

The detailed permission matrix will be defined separately in Document 11.

## 2. Role-Model Principles

The Habitacores role model must follow these principles:

1. A user identity is distinct from an organization membership.
2. A user may belong to multiple organizations.
3. A user may hold multiple roles.
4. Roles may be scoped to a specific organization, property, community, building, block, unit, legal matter, or work order.
5. Access must be granted through explicit membership, assignment, or approved platform authority.
6. Roles must reflect real operational responsibilities.
7. High-impact decisions must remain attributable to a responsible human.
8. Role names must not imply permissions that have not been explicitly granted.
9. Server-side authorization must enforce all role restrictions.
10. No user should receive platform-wide access merely because they are an early team member.
11. Resident roles must remain distinct from SaaS organization roles.
12. Critical financial, legal, and access-control actions should support separation of duties.

## 3. Core Identity Concepts

### 3.1 User

A user is a person with a Habitacores identity.

A user may authenticate once and participate in multiple organizations.

A user record may include:

- User identifier
- Full name
- Preferred name
- Email
- Phone number
- Authentication status
- Identity-verification status
- Preferred language
- Time zone
- Created date
- Last active date

### 3.2 Organization Membership

A membership connects a user to an organization.

A membership may define:

- Organization
- User
- Role or roles
- Membership status
- Invitation status
- Effective dates
- Property or community scope
- Assignment restrictions
- Invited by
- Approved by
- Suspended or revoked by
- Reason for status change

### 3.3 Role

A role is a named bundle of business responsibilities.

A role does not independently guarantee access unless:

- The membership is active.
- The role is valid.
- The resource is within scope.
- The requested action is permitted.
- Required approvals are present.

### 3.4 Assignment

An assignment connects a user or organization to a specific responsibility.

Examples include:

- Agent assigned to a listing
- Lawyer assigned to an agreement
- Vendor assigned to a work order
- Operator assigned to a community
- Finance reviewer assigned to a payment
- Maintenance coordinator assigned to a building

Assignments may provide narrower access than organization-wide roles.

### 3.5 Persona

A persona is a representative user profile used to understand goals, behaviours, needs, constraints, and product experience.

A persona is not a permission role.

One persona may hold multiple roles, and one role may be held by several personas.

## 4. User Categories

Habitacores users fall into five broad categories.

### 4.1 Platform Users

Users who operate, secure, support, or govern the Habitacores SaaS platform.

### 4.2 Property-Organization Users

Users acting for a property-owning, property-operating, development, or management organization.

### 4.3 Residential Users

Prospects, applicants, residents, co-residents, household members, dependants, and guarantors.

### 4.4 Professional and Service Users

Estate agents, lawyers, vendors, contractors, maintenance personnel, and related service providers.

### 4.5 Partner and Institutional Users

Future financial, insurance, regulatory, enterprise, and integration partners.

## 5. Initial Pilot User Context

The initial Habitacores pilot supports the founding-team-owned residential property context.

The pilot currently includes:

- Community 001
- Existing Building
- Block A
- Block B
- Six confirmed residential units in Block A
- Functional masjid
- Building Under Construction
- Waterside context

The initial active users may include:

- Product Visionary
- Platform administrator
- Property-owner administrator
- Property operator
- Estate agent
- Estate lawyer
- Prospective resident
- Applicant
- Approved resident
- Maintenance coordinator
- Vendor
- Finance reviewer

A single founding-team member may initially perform more than one responsibility.

The platform must still model those responsibilities as separate roles.

## 6. Primary Personas

## 6.1 Product Visionary

### Profile

The Product Visionary defines what Habitacores should become as a product and business ecosystem.

### Primary Goals

- Define product direction
- Confirm market priorities
- Validate business requirements
- Approve major product decisions
- Protect the long-term vision
- Confirm whether specifications reflect operational reality

### Needs

- Clear product documentation
- Decision traceability
- Roadmap visibility
- Business-impact analysis
- Architecture explanations
- Risk visibility

### Constraints

- May not be a full-time software engineer
- Requires explanations that connect technical decisions to product outcomes
- Must not become a technical bottleneck for routine implementation decisions

### Typical Role

- Product Visionary
- Organization Owner where legally and operationally appropriate

## 6.2 Platform Administrator

### Profile

The Platform Administrator operates the Habitacores SaaS environment.

### Primary Goals

- Manage platform configuration
- Support organizations
- Review platform incidents
- Administer approved access
- Monitor system health
- Protect platform integrity

### Needs

- Administrative dashboards
- Organization lookup
- User-support tools
- Audit access
- Security controls
- Incident workflows

### Constraints

- Must not casually access organization data
- Must use approved support-access workflows
- Privileged actions must be audited
- Platform administration must not imply ownership of customer records

### Typical Role

- Platform Administrator

## 6.3 Property Owner Administrator

### Profile

The Property Owner Administrator represents the organization that owns or controls a property.

### Primary Goals

- Maintain ownership and property records
- Approve listings
- Approve rental policies
- Review applicants
- Approve tenancy terms
- Monitor occupancy and financial performance
- Approve major property decisions

### Needs

- Portfolio dashboard
- Unit visibility
- Applicant pipeline
- Agreement status
- Payment status
- Maintenance cost visibility
- Renewal forecasts
- Audit history

### Constraints

- Must act within the organization’s legal authority
- Should not perform every financial or legal action without review where separation of duties is required

### Typical Role

- Property Owner Administrator

## 6.4 Property Operator

### Profile

The Property Operator manages day-to-day community and resident operations.

### Primary Goals

- Maintain accurate unit status
- Coordinate inspections
- Manage move-in and move-out
- Respond to residents
- Coordinate maintenance
- Publish notices
- Track operational tasks

### Needs

- Community dashboard
- Occupancy view
- Inspection calendar
- Resident records
- Maintenance queue
- Vendor assignments
- Communication tools

### Constraints

- May not approve ownership changes
- May not approve high-value financial changes unless separately authorized
- May not make final legal decisions

### Typical Role

- Property Operator

## 6.5 Development Manager

### Profile

The Development Manager coordinates property-development or construction activity.

### Primary Goals

- Track buildings and phases
- Update construction status
- Coordinate contractors
- Record completion milestones
- Prepare units for operational handover

### Needs

- Development dashboard
- Building status
- Milestone tracking
- Contractor assignments
- Document management
- Handover workflow

### Constraints

- Development status changes must be supported by approved evidence
- Completion does not automatically make a unit leasable

### Typical Role

- Development Manager

## 6.6 Estate Agent

### Profile

The Estate Agent markets approved listings and coordinates prospects.

### Primary Goals

- Access assigned listings
- Respond to inquiries
- Schedule inspections
- Record prospect activity
- Support application handoff
- Track commission-related activity

### Needs

- Assigned listing dashboard
- Lead pipeline
- Inspection calendar
- Prospect contact history
- Application handoff
- Performance reporting

### Constraints

- May access only assigned listings, prospects, and inspections
- Must not alter approved rent or fees without authority
- Must not approve applicants
- Must not verify payments unless separately authorized
- Must not access unrelated resident records

### Typical Role

- Estate Agent

## 6.7 Estate Lawyer

### Profile

The Estate Lawyer supports agreement preparation, review, notices, and other assigned legal matters.

### Primary Goals

- Review relevant property and applicant records
- Prepare or review tenancy agreements
- Track document versions
- Generate approved notices
- Record legal outcomes

### Needs

- Assigned legal-matter dashboard
- Agreement templates
- Document access
- Version history
- Approval workflow
- Legal audit trail

### Constraints

- May access only assigned matters and supporting records
- Must not make property-management decisions
- Must not change payment records
- Must not access unrelated applicants or residents
- Legal advice remains subject to professional responsibility

### Typical Role

- Estate Lawyer

## 6.8 Finance Reviewer

### Profile

The Finance Reviewer records, reviews, verifies, allocates, and reconciles approved financial transactions.

### Primary Goals

- Review payment evidence
- Verify receipt of funds
- Allocate payments to charges
- Track outstanding balances
- Record refunds
- Produce financial reports

### Needs

- Charges dashboard
- Payment queue
- Bank-reference fields
- Allocation tools
- Reconciliation reports
- Audit history

### Constraints

- Must not approve their own unauthorized adjustments where separation of duties applies
- Must not alter tenancy terms
- Must not treat deposits as revenue
- Must not rely only on uploaded payment screenshots
- High-value refunds may require secondary approval

### Typical Role

- Finance Reviewer

## 6.9 Maintenance Coordinator

### Profile

The Maintenance Coordinator manages maintenance requests and work assignments.

### Primary Goals

- Review reported issues
- Assign priority
- Create work orders
- Assign staff or vendors
- Track completion
- Confirm closure

### Needs

- Maintenance queue
- Priority filters
- Vendor availability
- Work-order scheduling
- Cost approval workflow
- Completion evidence

### Constraints

- May not approve costs above configured thresholds
- May not access unrelated financial or legal records
- Emergency access must be documented

### Typical Role

- Maintenance Coordinator

## 6.10 Vendor Administrator

### Profile

The Vendor Administrator manages an external service-provider organization.

### Primary Goals

- Maintain vendor profile
- Manage team members
- Receive assignments
- Submit quotations
- Track work status
- Submit completion evidence and invoices

### Needs

- Vendor organization dashboard
- Assigned work orders
- Scheduling
- Quote submission
- Invoice visibility
- Performance records

### Constraints

- Access is limited to assigned work and relevant property information
- Must not access resident documents beyond operational necessity
- Must not view unrelated units or communities

### Typical Role

- Vendor Administrator

## 6.11 Vendor Worker

### Profile

The Vendor Worker performs assigned maintenance or property-service tasks.

### Primary Goals

- View assigned work
- Access necessary location details
- Record arrival and progress
- Upload completion evidence
- Report inability to complete work

### Needs

- Mobile-first task view
- Work instructions
- Contact protocol
- Scheduling
- Photo or document upload

### Constraints

- Access must be work-order-specific
- Personal resident information must be minimized
- Access should expire when the assignment ends

### Typical Role

- Vendor Worker

## 6.12 Prospective Resident

### Profile

A Prospective Resident is exploring available housing but has not yet submitted a formal application.

### Primary Goals

- View trusted listings
- Understand annual rent and charges
- Review amenities
- Ask questions
- Schedule an inspection
- Track inquiry status

### Needs

- Mobile-friendly listings
- Clear fee disclosure
- Location and property information
- Inspection booking
- Communication updates

### Constraints

- Should not access private property records
- Must not see information about other prospects, applicants, or residents

### Typical Role

- Prospect or unauthenticated visitor

## 6.13 Applicant

### Profile

An Applicant is formally applying to occupy a specific unit.

### Primary Goals

- Complete an application
- Upload required documents
- Track application status
- Respond to information requests
- Review approved offer terms

### Needs

- Save-and-resume application
- Document checklist
- Consent notices
- Status tracking
- Secure communications

### Constraints

- Can access only their own application or authorized household application
- Cannot view internal review notes unless specifically approved for disclosure
- Cannot change submitted records after defined review stages without controlled amendment

### Typical Role

- Applicant

## 6.14 Primary Resident

### Profile

The Primary Resident is the principal residential party associated with an active occupancy.

### Primary Goals

- Access tenancy documents
- View payment and charge history
- Submit maintenance requests
- Receive notices
- Manage renewal or move-out actions
- View household occupancy information

### Needs

- Resident dashboard
- Agreement access
- Maintenance tools
- Renewal reminders
- Payment history
- Community announcements

### Constraints

- Can access only their own household and occupancy records
- Cannot alter official payment or agreement records
- Household-management rights must remain within approved policy

### Typical Role

- Primary Resident

## 6.15 Co-Resident

### Profile

A Co-Resident is an approved adult resident associated with the occupancy.

### Primary Goals

- Access relevant agreement information
- Receive notices
- Submit maintenance requests
- View approved household information

### Needs

- Resident portal
- Occupancy details
- Maintenance access
- Communication preferences

### Constraints

- May have fewer permissions than the Primary Resident
- Financial or renewal actions may depend on agreement terms
- Cannot manage unrelated household members

### Typical Role

- Co-Resident

## 6.16 Household Member

### Profile

A Household Member is an authorized occupant associated with the resident household.

### Primary Goals

- Receive appropriate community communication
- Be represented accurately in occupancy records
- Access limited resident services where approved

### Constraints

- May not have an independent login
- May not receive agreement, financial, or application-management permissions
- Minor dependants must not be treated as independent platform users by default

### Typical Role

- Household Member

## 6.17 Guarantor

### Profile

A Guarantor is a person or organization providing an approved guarantee for an applicant or agreement.

### Primary Goals

- Review guarantee obligations
- Provide required information
- Sign relevant documents
- Receive notices relating to the guarantee

### Constraints

- Access must be limited to the relevant application, guarantee, or agreement
- A guarantor is not automatically a resident or organization member

### Typical Role

- Guarantor

## 6.18 Community Administrator

### Profile

The Community Administrator manages community-level information, announcements, amenities, and operating rules.

### Primary Goals

- Maintain community information
- Publish notices
- Manage amenity status
- Coordinate community activities
- Support resident communication

### Needs

- Community dashboard
- Announcement tools
- Amenity records
- Resident communication controls
- Facility-status visibility

### Constraints

- Must not access unnecessary applicant or financial data
- Must not infer religious identity from use of the masjid
- Community announcements must follow approved policy

### Typical Role

- Community Administrator

## 7. Canonical Role Catalogue

The initial canonical roles are:

### Platform Roles

- Platform Owner
- Platform Administrator
- Platform Support Agent
- Platform Security Administrator
- Platform Auditor

### Property and Organization Roles

- Organization Owner
- Organization Administrator
- Property Owner Administrator
- Property Operator
- Property Manager
- Community Administrator
- Development Manager
- Finance Reviewer
- Maintenance Coordinator
- Document Reviewer

### Professional Roles

- Estate Agent
- Estate Agency Administrator
- Estate Lawyer
- Legal Firm Administrator
- Vendor Administrator
- Vendor Worker
- Contractor

### Residential Roles

- Prospect
- Applicant
- Primary Resident
- Co-Resident
- Household Member
- Guarantor
- Former Resident

Some roles are deferred beyond the MVP.

## 8. Platform Roles

## 8.1 Platform Owner

The Platform Owner holds the highest approved business authority over the Habitacores platform organization.

Responsibilities may include:

- Platform governance
- Organization ownership
- Strategic administrative approval
- Emergency authority
- Appointment of privileged platform roles

The role must not be used for routine daily administration.

## 8.2 Platform Administrator

Responsibilities may include:

- Platform configuration
- Organization administration
- User-support escalation
- Integration administration
- Feature configuration
- Incident coordination

Access to organization data must follow controlled support-access procedures.

## 8.3 Platform Support Agent

Responsibilities may include:

- User-support cases
- Account-recovery support
- Troubleshooting
- Escalation
- Approved impersonation or support-session workflows where implemented

The role must not receive unrestricted access to sensitive records.

## 8.4 Platform Security Administrator

Responsibilities may include:

- Security policy administration
- Privileged-access review
- Incident response
- Audit review
- Authentication-policy management
- Threat monitoring

Security administrators should not perform ordinary financial or property operations.

## 8.5 Platform Auditor

Responsibilities may include:

- Read-only access to approved audit records
- Control testing
- Compliance review
- Incident review
- Reporting

The role should be read-only except for audit comments or case records.

## 9. Organization Roles

## 9.1 Organization Owner

The Organization Owner represents the highest approved authority within an organization.

Responsibilities may include:

- Organization governance
- Appointment of organization administrators
- Approval of major organization changes
- Subscription authority
- Organization closure or transfer requests

Ownership must not automatically grant operational access to every record if legal or business separation requires restrictions.

## 9.2 Organization Administrator

Responsibilities may include:

- Invite members
- Assign approved roles
- Manage organization profile
- Configure organization preferences
- Review access
- Suspend memberships
- Manage organization-level integrations

The role must not automatically include property, finance, or legal approval authority.

## 9.3 Property Owner Administrator

Responsibilities may include:

- Register properties
- Approve property records
- Approve listings
- Approve rental terms
- Approve applicant decisions
- Approve agreements
- Review property performance
- Approve renewals

## 9.4 Property Operator

Responsibilities may include:

- Manage communities, buildings, blocks, and units
- Maintain operational statuses
- Coordinate inspections
- Coordinate move-ins and move-outs
- Manage resident records
- Manage maintenance activity
- Publish operational notices

## 9.5 Property Manager

The Property Manager may combine broader operational responsibilities across one or more communities.

Responsibilities may include:

- Portfolio operations
- Staff coordination
- Resident services
- Vendor oversight
- Occupancy management
- Reporting
- Policy implementation

The distinction between Property Manager and Property Operator may be configurable by organization size.

## 9.6 Community Administrator

Responsibilities may include:

- Manage community details
- Manage amenities and facilities
- Publish community notices
- Coordinate community events
- Maintain operating information

## 9.7 Development Manager

Responsibilities may include:

- Manage developments and phases
- Update building and block status
- Coordinate development records
- Record milestones
- Manage operational handover

## 9.8 Finance Reviewer

Responsibilities may include:

- Review charges
- Record payments
- Verify payments
- Allocate payments
- Reconcile transactions
- Process approved refunds
- Produce reports

## 9.9 Maintenance Coordinator

Responsibilities may include:

- Review maintenance requests
- Set priority
- Create work orders
- Assign vendors
- Review completion
- Coordinate closure

## 9.10 Document Reviewer

Responsibilities may include:

- Review submitted documents
- Approve or reject document status
- Request corrections
- Manage expiration review
- Record document-review history

This role may be combined with other roles during the MVP.

## 10. Professional Roles

## 10.1 Estate Agency Administrator

Responsibilities may include:

- Manage estate-agency organization
- Invite agency members
- Assign estate agents
- Review agency performance
- Manage approved organization information

## 10.2 Estate Agent

Responsibilities may include:

- Manage assigned leads
- Coordinate assigned inspections
- Record prospect communication
- Support application handoff
- View assigned listing information
- Track approved commission records

## 10.3 Legal Firm Administrator

Responsibilities may include:

- Manage legal-firm organization
- Invite lawyers
- Assign legal matters
- Review workload
- Maintain firm profile

## 10.4 Estate Lawyer

Responsibilities may include:

- Prepare agreements
- Review agreements
- Manage assigned legal matters
- Draft notices
- Record legal recommendations
- Maintain matter documentation

## 10.5 Vendor Administrator

Responsibilities may include:

- Manage vendor organization
- Invite vendor workers
- Review work assignments
- Submit quotations
- Submit invoices
- Monitor performance

## 10.6 Vendor Worker

Responsibilities may include:

- View assigned work orders
- Update work status
- Record attendance
- Upload completion evidence
- Report operational blockers

## 10.7 Contractor

A Contractor is associated with development or construction work rather than routine property maintenance.

Responsibilities may include:

- View assigned development scope
- Update progress
- Upload evidence
- Submit milestone information
- Submit approved financial documentation

This role may be deferred beyond the initial MVP.

## 11. Residential Roles

## 11.1 Prospect

A Prospect may:

- Browse public listings
- Submit inquiries
- Request inspections
- Manage communication preferences

Authentication may be optional for initial browsing.

## 11.2 Applicant

An Applicant may:

- Create an application
- Save progress
- Submit information
- Upload documents
- Respond to requests
- Track status
- Review offers

## 11.3 Primary Resident

A Primary Resident may:

- View active occupancy
- Access approved agreements
- View charges and payments
- Submit maintenance requests
- Receive notices
- Initiate approved renewal or move-out actions
- Manage limited household information

## 11.4 Co-Resident

A Co-Resident may:

- View relevant occupancy information
- Access approved documents
- Submit maintenance requests
- Receive notices

## 11.5 Household Member

A Household Member may:

- Be recorded as an authorized occupant
- Receive limited access where approved

A household member may not require a separate user account.

## 11.6 Guarantor

A Guarantor may:

- Review guarantee obligations
- Submit required information
- Sign relevant documents
- Receive guarantee-related notices

## 11.7 Former Resident

A Former Resident may retain limited access to:

- Historical agreements
- Approved payment records
- Move-out records
- Deposit decisions
- Final notices

Access duration must follow retention and legal policy.

## 12. Role Scope

Roles may be scoped at different levels.

### 12.1 Platform Scope

Applies across the Habitacores platform.

Example:

- Platform Security Administrator

### 12.2 Organization Scope

Applies to one organization.

Example:

- Organization Administrator

### 12.3 Property Scope

Applies to one property.

Example:

- Property Owner Administrator for Property 001

### 12.4 Community Scope

Applies to one community.

Example:

- Community Administrator for Community 001

### 12.5 Building Scope

Applies to one building.

Example:

- Maintenance Coordinator for Existing Building

### 12.6 Block Scope

Applies to one block.

Example:

- Property Operator for Block A

### 12.7 Unit Scope

Applies to one or more units.

Example:

- Agent assigned to a listed unit

### 12.8 Matter Scope

Applies to one legal matter.

Example:

- Lawyer assigned to Agreement Matter 001

### 12.9 Work-Order Scope

Applies to one work order.

Example:

- Vendor Worker assigned to a repair task

## 13. Multi-Role Users

A user may hold multiple roles.

Examples:

```text
User: Founding Team Member

Organization: Property-Owning Organization
Roles:
- Organization Owner
- Property Owner Administrator
- Finance Reviewer

User: Estate Professional

Organization: Estate Agency
Role:
- Estate Agent

Organization: Property Organization
Assignment:
- Assigned Agent for Listing 001

Role evaluation must consider the current organization and resource context.

The platform must not combine unrelated roles into uncontrolled global access.

14. Context Switching

Users with multiple memberships must be able to identify:

Current organization
Current role
Current property or community
Current assignment scope

The interface should display the active context clearly.

Example:

Organization: Founding Property Organization
Role: Property Operator
Community: Community 001

Switching context must not preserve access to data from the previous organization.

15. Resident Household Roles

The resident household model must distinguish among:

Primary Applicant
Co-Applicant
Primary Resident
Co-Resident
Adult Household Member
Minor Dependant
Authorized Occupant
Guarantor
Emergency Contact

These labels must not be treated as interchangeable.

Primary Applicant

Leads the application.

Co-Applicant

Participates jointly in the application.

Primary Resident

Holds the primary resident responsibility after occupancy begins.

Co-Resident

Shares approved occupancy responsibility.

Adult Household Member

An approved adult occupant who may not be a party to every financial obligation.

Minor Dependant

A minor associated with the household.

A minor should not receive an independent login by default.

Authorized Occupant

An approved occupant who may not be a formal agreement party.

Guarantor

Provides an approved guarantee but does not automatically occupy the unit.

Emergency Contact

May be contacted under approved circumstances but receives no ordinary platform access.

16. Role Lifecycle

A role assignment may follow this lifecycle:

Proposed
→ Approved
→ Active
→ Suspended
→ Revoked
→ Expired

Role changes must record:

User
Organization
Previous role
New role
Scope
Effective date
Approver
Reason
Audit event
17. Membership Lifecycle

Membership status may follow:

Invited
→ Invitation Accepted
→ Active
→ Suspended
→ Revoked
→ Expired

A revoked membership must not retain active role access.

A user account may remain active globally even when one organization membership is revoked.

18. Invitation Model

Organization invitations should include:

Inviting organization
Invited email or user
Proposed role
Proposed scope
Expiration date
Invited by
Approval requirement
Acceptance status

Invitations must not grant access before acceptance and required approval.

19. Separation of Duties

Habitacores should support separation of duties for high-risk processes.

19.1 Applicant Decision

Recommended separation:

Agent coordinates prospect
Reviewer evaluates application
Property Owner Administrator makes final decision
19.2 Payment Verification

Recommended separation:

One user records payment evidence
Authorized Finance Reviewer verifies payment
High-risk adjustments may require secondary approval
19.3 Agreement Approval

Recommended separation:

Lawyer or authorized operator prepares agreement
Property Owner Administrator approves terms
Parties sign agreement
19.4 Refund Approval

Recommended separation:

Finance Reviewer prepares refund
Authorized owner or senior finance role approves
System records execution and reconciliation
19.5 Role Assignment

Recommended separation:

Organization Administrator proposes privileged role
Organization Owner or authorized approver confirms

The MVP may allow controlled role combinations while preserving audit records.

20. Role Conflicts

The system should identify potentially conflicting roles.

Examples include:

Applicant and application reviewer for the same application
Vendor and maintenance approver for the same work order
Payment recorder and high-value refund approver for the same transaction
Estate agent and final applicant decision-maker for the same application
Lawyer and final financial approver for the same agreement
Auditor and operational editor for the same records

Conflicts may be:

Prohibited
Allowed with disclosure
Allowed with secondary approval
Allowed only in the founding-team pilot

Conflict policy will be defined in the business rules and permission matrix.

21. Privileged Roles

The following roles are privileged:

Platform Owner
Platform Administrator
Platform Security Administrator
Organization Owner
Organization Administrator
Property Owner Administrator
Finance Reviewer with verification authority
Role Administrator
Auditor with sensitive-data access

Privileged roles should require:

Strong authentication
Multi-factor authentication
Access review
Audit logging
Session protection
Explicit assignment
Regular revalidation
22. Support Access

Platform support personnel must not receive automatic unrestricted access to organization data.

Support access should require:

Support case
Approved purpose
Limited scope
Time limitation
Audit record
User or organization notification where appropriate
Revocation after completion

Impersonation, if implemented, must be visibly identified and fully audited.

23. Public and Unauthenticated Users

Some Habitacores functionality may be available without authentication.

Public users may:

View approved public listings
View approved community information
Submit a basic inquiry
Begin an inspection request
Access public support information

Public users must not access:

Applicant documents
Resident records
Payment records
Private unit information
Internal notes
Legal records
Organization dashboards
24. Role Naming Standards

Role names must be:

Clear
Business-oriented
Singular
Consistent
Distinct from job titles where necessary
Distinct from permission names

Preferred:

Property Operator
Finance Reviewer
Estate Agent
Primary Resident

Avoid:

Super User
Power User
Manager 2
General Admin
Property Person

Technical permission names may be more granular than role names.

25. Role Versus Permission

A role is a business responsibility bundle.

A permission is a specific allowed action.

Example:

Role:
Property Operator

Possible Permissions:
- community.read
- building.read
- unit.update_operational_status
- inspection.manage
- resident.read_assigned
- maintenance.manage

The permission matrix will define exact permissions.

Roles must not be encoded only as hardcoded conditionals throughout the application.

26. Role Versus Ownership

Record ownership and organization ownership are distinct.

Examples:

An applicant may own the data-entry process for an application but cannot approve it.
A property organization may legally own a property.
A user may create a listing without owning the property.
A platform administrator may administer the platform without owning organization records.
A vendor may upload completion evidence without owning the work order.

Authorization must not rely only on a created_by field.

27. Role Versus Assignment

A role establishes general responsibility.

An assignment establishes specific operational scope.

Example:

Role:
Estate Agent

Assignment:
Listing HAB-LIST-000001

An Estate Agent should not automatically access every listing merely because they hold the role.

28. Initial MVP Roles

The minimum MVP role set should include:

Platform
Platform Administrator
Founding Property Organization
Organization Owner
Organization Administrator
Property Owner Administrator
Property Operator
Finance Reviewer
Maintenance Coordinator
Professional
Estate Agent
Estate Lawyer
Vendor Administrator
Vendor Worker
Residential
Prospect
Applicant
Primary Resident
Co-Resident
Household Member
Guarantor

Some roles may initially be held by the same person.

29. Deferred Roles

The following roles may be deferred beyond the initial MVP:

Platform Support Agent
Platform Security Administrator as a separate staffed role
Platform Auditor
Property Manager as distinct from Property Operator
Development Manager
Estate Agency Administrator
Legal Firm Administrator
Contractor
Insurance Partner
Financial Partner
Government Reviewer
Enterprise Auditor
API Integration Administrator

Deferred roles should remain possible without restructuring the entire authorization model.

30. Persona-to-Role Mapping
Persona	Typical Role
Product Visionary	Product Visionary, Organization Owner
Platform Operator	Platform Administrator
Founding Property Owner	Property Owner Administrator
Daily Property Operator	Property Operator
Community Coordinator	Community Administrator
Finance Personnel	Finance Reviewer
Maintenance Lead	Maintenance Coordinator
Estate Professional	Estate Agent
Legal Professional	Estate Lawyer
Vendor Business Owner	Vendor Administrator
Technician	Vendor Worker
Housing Seeker	Prospect
Rental Applicant	Applicant
Approved Occupant	Primary Resident or Co-Resident
Household Dependant	Household Member
Guarantee Provider	Guarantor

This mapping is representative, not mandatory.

31. Role Data Requirements

Each role assignment should support:

Role assignment identifier
User
Organization
Role
Scope type
Scope identifier
Status
Effective start
Effective end
Assigned by
Approved by
Assignment reason
Revocation reason
Created date
Updated date
32. Role Audit Events

The following actions must generate audit events:

Membership invitation
Invitation acceptance
Role assignment
Role elevation
Scope expansion
Role suspension
Role revocation
Privileged access grant
Support-access activation
Support-access termination
Organization ownership transfer
Failed unauthorized-access attempt where appropriate
33. Role Review

Organizations should periodically review:

Active members
Privileged roles
Expired assignments
Inactive users
Cross-organization access
Vendor access
Former employee access
Support access
Unused roles

The review cadence will be defined in the security and business-rules documents.

34. User Experience Requirements

The application should communicate role and scope clearly.

Users should be able to understand:

Which organization they are using
Which role is active
Which property or community is selected
Why an action is unavailable
Whether approval is required
Whether they are acting on behalf of an organization
Whether a view contains private or sensitive information

The interface must not expose inaccessible actions merely to return generic authorization errors after submission.

35. Privacy Requirements by Role

Roles should receive only the minimum personal information required.

Examples:

Estate agents may see prospect contact information for assigned leads.
Vendors may see access and contact details necessary for assigned work.
Community administrators should not see applicant financial documents.
Finance reviewers should not see unrelated maintenance notes.
Lawyers should see only assigned legal records.
Residents should not see other households’ records.
Platform support should access sensitive information only through approved support workflows.
36. Masjid and Community-Facility Access

The functional masjid is represented as a community facility and amenity.

Access to masjid-related notices or facility information must not be used to infer:

Religion
Religious affiliation
Religious practice
Belief
Personal identity attributes

Community-facility access and communication should remain operational rather than identity-classifying.

37. Current Role Decisions

The following decisions are approved for Version 0.1:

Users are globally identified.
Organization access is granted through memberships.
Roles are scoped rather than globally applied by default.
Assignments may narrow access to specific resources.
Users may hold multiple roles.
Resident household roles remain distinct.
Platform administration remains distinct from property ownership.
Estate agents cannot make final applicant decisions by default.
Vendors receive assignment-specific access.
Lawyers receive matter-specific access.
Financial verification remains distinct from charge creation where practical.
Critical role changes must be audited.
Privileged roles require stronger security controls.
The MVP may combine roles operationally without collapsing their conceptual boundaries.
38. Unresolved Role Decisions

The following decisions remain pending:

Exact founding-team member assignments
Whether Property Manager and Property Operator will be separate MVP roles
Whether Document Reviewer will be an independent role
Exact financial approval thresholds
Exact refund-approval workflow
Exact support-access model
Whether residents may invite household members
Whether co-residents receive full agreement access
Whether agents may create draft listings
Whether lawyers may generate agreements directly
Whether vendors may submit quotations before assignment
Whether platform administrators may approve organizations
Required periodic access-review cadence

These decisions will be resolved through the business rules and permission matrix.

39. Role Governance

Changes to canonical roles require:

A documented change proposal
Business justification
Permission-impact review
Security review
Privacy review
Separation-of-duties review
Product Visionary approval
Chief Product Architect approval
Update to the permission matrix
Audit and migration considerations where applicable
40. Current Users, Personas, and Roles Decision

Habitacores will use a membership-based, role-aware, and assignment-scoped access model.

The foundational relationship is:

User
→ Organization Membership
→ Role
→ Resource Scope
→ Assignment
→ Permission

The initial MVP will support platform, property, professional, and residential users while preserving clear distinctions among:

Platform authority
Organization authority
Property authority
Professional assignments
Resident household access
Financial approval
Legal responsibility
Vendor work access

This structure provides the foundation for the detailed permission matrix, authentication architecture, UX navigation, audit model, and server-side authorization controls.