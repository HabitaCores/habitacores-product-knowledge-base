# Document 03 — Business Architecture

## Document Control

| Field | Value |
|---|---|
| Product | Habitacores |
| Document | Business Architecture |
| Version | 0.1 |
| Status | Draft for approval |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Market | Nigeria |
| Last Updated | 29 July 2026 |

## 1. Purpose

This document defines the business architecture of Habitacores.

It describes:

- The operating model
- Business capability structure
- Stakeholder relationships
- Value streams
- Organizational boundaries
- Product-module boundaries
- Core business responsibilities
- Pilot-community operations
- Revenue-generating capabilities
- Expansion paths
- Business governance

This document does not define the final technical implementation. It establishes the business structure that the technical architecture must support.

## 2. Business Architecture Objective

Habitacores must operate as one connected real-estate ecosystem rather than a collection of disconnected applications.

The business architecture must support:

- Residential property ownership
- Property development
- Unit listing and leasing
- Applicant management
- Resident onboarding
- Annual rent administration
- Agreement management
- Maintenance and vendor operations
- Community communication
- Legal workflows
- Financial administration
- Portfolio reporting
- Future third-party SaaS customers

The architecture must support the founding team’s property first while remaining capable of serving multiple external organizations later.

## 3. Business Operating Model

Habitacores will initially operate using a controlled owner-operator model.

The founding team will act as:

- Property owner
- Property operator
- Platform owner
- Initial product customer
- Initial business-process validator

The initial operating model is:

```text
Founding Team
        ↓
Owns and controls the property
        ↓
Defines leasing and operating policies
        ↓
Uses Habitacores to manage the property
        ↓
Validates workflows and business rules
        ↓
Offers the validated platform to external organizations

The long-term operating model is a multi-tenant SaaS ecosystem supporting multiple independent organizations.

4. Founding Business Entities

Habitacores should distinguish among the following business entities even if they are initially controlled by the same founding team.

4.1 Habitacores Platform Organization

Responsible for:

Product strategy
Platform ownership
Software operations
Security
Product development
SaaS subscriptions
Platform support
Integrations
Data governance
Technical infrastructure
4.2 Property-Owning Organization

Responsible for:

Property ownership
Property-related legal obligations
Rental-income entitlement
Capital improvements
Ownership documentation
Property-level financial decisions
Lease approval
4.3 Property-Operating Organization

Responsible for:

Daily property operations
Resident communication
Inspections
Maintenance coordination
Vendor assignment
Move-in and move-out processes
Community notices
Operational reporting

Initially, one legal entity may perform several of these responsibilities. The platform data model must still preserve the distinctions.

5. Initial Community Structure

The first live operating environment is the founding-team-owned residential development.

The initial structure is:

Founding Organization
└── Residential Community 001
    ├── Block A
    │   ├── Unit A01
    │   ├── Unit A02
    │   ├── Unit A03
    │   ├── Unit A04
    │   ├── Unit A05
    │   └── Unit A06
    ├── Functional Masjid
    └── Planned Future Block

Final unit identifiers may be refined after the building layout and floor structure are confirmed.

6. Business Stakeholders

Habitacores supports multiple stakeholder groups.

6.1 Internal Stakeholders
Product Visionary
Platform administrators
Property-owner administrators
Property operators
Finance personnel
Community administrators
Maintenance coordinators
Development personnel
6.2 External Stakeholders
Prospective residents
Approved residents
Estate agents
Estate lawyers
Vendors
Contractors
Payment providers
Financial institutions
Insurance partners
Government or regulatory bodies
6.3 Future SaaS Customers
Independent landlords
Property-development companies
Property-management firms
Estate agencies
Housing cooperatives
Student-housing operators
Build-to-rent operators
Institutional property owners
7. Business Relationship Model

The property and community form the center of the business relationship model.

Property Owner
        ↓ owns
Residential Community
        ↓ contains
Blocks and Units
        ↓ occupied by
Residents
        ↑ supported by
Property Operators
        ↑ coordinated with
Agents, Lawyers, Vendors and Partners

Habitacores connects these relationships through shared workflows, records, documents, communications, and permissions.

8. Core Value Streams

Habitacores will support several end-to-end business value streams.

8.1 Property Development to Operational Readiness
Development Planned
→ Block Construction
→ Unit Definition
→ Construction Progress
→ Inspection
→ Completion
→ Handover
→ Listing Readiness
→ Operational Property

Business outcome:

A completed unit becomes a managed and leasable asset inside Habitacores.

8.2 Property Registration to Listing Publication
Property Registered
→ Ownership or Authority Recorded
→ Community Created
→ Block Created
→ Units Created
→ Amenities Recorded
→ Listing Prepared
→ Internal Review
→ Listing Published

Business outcome:

A verified and operationally ready unit becomes visible to prospective residents.

8.3 Inquiry to Inspection
Prospect Views Listing
→ Inquiry Submitted
→ Agent or Operator Responds
→ Inspection Requested
→ Inspection Scheduled
→ Reminder Sent
→ Inspection Completed
→ Feedback Recorded

Business outcome:

A qualified prospect completes a traceable property inspection.

8.4 Application to Approval
Application Started
→ Applicant Information Entered
→ Documents Uploaded
→ Application Submitted
→ Review Conducted
→ Additional Information Requested if needed
→ Applicant Approved or Rejected

Business outcome:

The property owner receives a documented and reviewable applicant decision.

8.5 Approval to Move-In
Applicant Approved
→ Rent and Fees Confirmed
→ Payment Instructions Issued
→ Payment Recorded
→ Tenancy Agreement Prepared
→ Agreement Reviewed
→ Agreement Signed
→ Move-In Approved
→ Handover Completed
→ Resident Activated

Business outcome:

An approved applicant becomes an active resident with complete records.

8.6 Resident Service and Maintenance
Resident Submits Request
→ Request Categorized
→ Operator Reviews
→ Vendor or Staff Assigned
→ Work Scheduled
→ Work Completed
→ Evidence Uploaded
→ Resident Confirms or Disputes
→ Request Closed

Business outcome:

Property issues are resolved transparently and with accountability.

8.7 Renewal or Move-Out
Lease Expiry Approaches
→ Renewal Reminder
→ Owner Reviews Terms
→ Renewal Offer Issued
→ Resident Accepts or Declines
→ Payment and Agreement Updated

or:

Move-Out Notice
→ Inspection Scheduled
→ Property Condition Recorded
→ Outstanding Obligations Reviewed
→ Deposit Decision Recorded
→ Keys Returned
→ Resident Deactivated
→ Unit Prepared for Relisting

Business outcome:

The unit transitions cleanly into a renewed tenancy or new leasing cycle.

9. Business Capability Map

Habitacores requires the following business capabilities.

9.1 Strategy and Governance
Product strategy
Business-policy definition
Decision governance
Architecture governance
Compliance oversight
Risk management
Performance management
9.2 Organization and Identity Management
Organization registration
User onboarding
Membership management
Role assignment
Invitation management
Access review
Organization suspension
Organization verification
9.3 Property and Community Management
Community registration
Development registration
Phase management
Block management
Floor management
Unit management
Amenity management
Ownership records
Property status management
Occupancy status management
9.4 Listing Management
Listing creation
Media management
Fee disclosure
Availability management
Listing approval
Publication
Listing suspension
Listing expiration
Listing analytics
9.5 Prospect and Lead Management
Inquiry intake
Prospect profiles
Communication history
Lead assignment
Qualification
Follow-up tracking
Conversion tracking
9.6 Inspection Management
Inspection requests
Scheduling
Rescheduling
Agent assignment
Reminders
Attendance tracking
No-show tracking
Inspection feedback
Access coordination
9.7 Application Management
Application forms
Applicant profiles
Household information
Document collection
Review workflow
Information requests
Approval
Rejection
Withdrawal
Decision history
9.8 Leasing and Agreement Management
Offer preparation
Fee confirmation
Agreement generation
Legal review
Agreement versioning
Signature tracking
Activation
Renewal
Termination
Expiration
9.9 Payment and Financial Administration
Annual rent schedules
Fee definitions
Payment instructions
Payment evidence
Payment confirmation
Allocation
Reconciliation
Deposit tracking
Refund tracking
Outstanding balances
Financial reporting
9.10 Resident Management
Resident profiles
Household members
Occupancy records
Agreement access
Payment history
Community communication
Resident status
Renewal status
Move-out records
9.11 Maintenance and Work Management
Request intake
Categorization
Priority assignment
Staff assignment
Vendor assignment
Work-order management
Scheduling
Completion evidence
Cost tracking
Resident confirmation
Closure
Recurring maintenance
9.12 Vendor Management
Vendor registration
Verification
Service categories
Property assignments
Quotations
Work orders
Service history
Invoices
Performance records
Suspension
9.13 Legal and Compliance Management
Ownership-document records
Legal assignments
Agreement templates
Legal review
Notices
Disputes
Compliance evidence
Document retention
Legal audit history
9.14 Communication and Notification
Email
SMS
WhatsApp integration
In-app notifications
Reminders
Community announcements
Message templates
Delivery tracking
Escalation alerts
9.15 Document Management
Document upload
Classification
Versioning
Access control
Review
Approval
Expiration tracking
Retention
Secure download
Audit history
9.16 Reporting and Analytics
Occupancy reports
Vacancy reports
Lead conversion
Inspection performance
Application conversion
Payment status
Maintenance performance
Vendor performance
Renewal forecasts
Portfolio reporting
9.17 Platform Administration
Tenant administration
User support
Feature configuration
Security administration
Audit review
Integration management
Subscription management
Usage monitoring
Incident management
10. Product Module Map

The business capabilities will eventually be grouped into product modules.

10.1 Habitacores Core

Shared capabilities:

Identity
Organizations
Roles
Permissions
Notifications
Documents
Audit
Configuration
10.2 Habitacores Property

Supports:

Communities
Developments
Blocks
Units
Amenities
Ownership
Operations
Occupancy
10.3 Habitacores Listings

Supports:

Listings
Media
Search
Inquiries
Inspections
Prospects
10.4 Habitacores Resident

Supports:

Applications
Resident onboarding
Agreements
Payment history
Maintenance
Notices
Renewals
10.5 Habitacores Agent

Supports:

Listing assignments
Leads
Inspections
Follow-ups
Applicant handoff
Commission tracking
10.6 Habitacores Legal

Supports:

Agreement preparation
Review
Legal assignments
Notices
Disputes
Legal records
10.7 Habitacores Vendor

Supports:

Vendor profiles
Work orders
Quotations
Scheduling
Completion records
Invoices
Performance
10.8 Habitacores Finance

Supports:

Annual rent
Fees
Deposits
Payment records
Reconciliation
Refunds
Reporting
10.9 Habitacores Development

Supports:

Construction phases
Blocks under development
Contractors
Progress reporting
Unit readiness
Handover
10.10 Habitacores Intelligence

Supports:

Automation
Forecasting
Document intelligence
Pricing support
Operational recommendations
Risk flagging
Analytics

These modules are business boundaries. They do not require separate applications or microservices in the MVP.

11. MVP Business Scope

The MVP will support the founding-team organization and initial community.

The MVP business scope includes:

One founding organization
One residential community
Block A
Six units
One functional masjid as a community amenity
Planned future block representation
Internal owner administrators
Internal property operators
Prospective residents
Approved residents
Assigned estate agents
Assigned estate lawyers
Maintenance vendors

The MVP must support the complete leasing lifecycle and basic resident operations.

12. MVP In-Scope Capabilities

The MVP includes:

Organization setup
Community setup
Block and unit registration
Amenity registration
Unit availability
Listing publication
Inquiry intake
Inspection scheduling
Application submission
Application review
Annual rent and fee disclosure
Applicant approval
Payment-status recording
Agreement workflow
Move-in administration
Resident records
Maintenance requests
Renewal reminders
Audit history
Basic reporting
13. MVP Out-of-Scope Capabilities

The MVP will not initially include:

Open property marketplace onboarding
Complex property accounting
Mortgage origination
Rent financing
Insurance underwriting
Automated legal decisions
Full construction ERP
Public vendor marketplace
Advanced AI applicant scoring
Multi-country tax logic
White-label deployments
Public developer APIs
Microservice decomposition
Native mobile applications

These may be added after validation.

14. Business Responsibility Model
14.1 Habitacores Platform Responsibilities

Habitacores Platform is responsible for:

Platform availability
Access control
Tenant isolation
Product configuration
Platform security
Audit capability
Data processing
User support
Technical integrations
Product updates
14.2 Property Organization Responsibilities

The property organization is responsible for:

Property ownership or authority
Listing accuracy
Rental policy
Fee policy
Applicant decisions
Agreement approval
Property condition
Resident treatment
Maintenance delivery
Legal compliance
14.3 Agent Responsibilities

Agents are responsible for:

Accurate prospect communication
Inspection coordination
Lead updates
Applicant handoff
Compliance with assigned authority
Commission-record accuracy
14.4 Lawyer Responsibilities

Lawyers are responsible for:

Legal review
Agreement correctness
Legal notices
Jurisdiction-specific guidance
Professional accountability
14.5 Vendor Responsibilities

Vendors are responsible for:

Accurate quotations
Timely service
Safe property access
Completion evidence
Invoice accuracy
Compliance with assignment terms
14.6 Resident Responsibilities

Residents are responsible for:

Accurate information
Timely document submission
Payment obligations
Property-care obligations
Maintenance reporting
Agreement compliance
Community conduct
15. Approval Authorities

Habitacores must distinguish between operational actions and approval authority.

Examples:

Business Decision	Recommended Approval Authority
Publish listing	Property owner administrator
Confirm inspection	Assigned agent or operator
Approve applicant	Property owner administrator
Reject applicant	Property owner administrator
Prepare agreement	Assigned lawyer or authorized operator
Approve final agreement	Property owner administrator
Confirm payment	Authorized finance or owner role
Approve move-in	Property operator or owner administrator
Assign maintenance vendor	Property operator
Approve maintenance cost	Authorized owner or operator
Offer renewal	Property owner administrator
Approve refund	Authorized finance or owner role
Verify ownership	Authorized compliance or owner role

The permission matrix will define these authorities in greater detail.

16. Business Information Ownership

Each critical business record must have a defined owner.

Record	Business Owner
Organization profile	Organization administrator
Property ownership record	Property owner administrator
Community record	Property owner or operator
Unit record	Property operator
Listing	Property owner or assigned agent
Inquiry	Assigned agent or operator
Inspection	Assigned agent or operator
Application	Applicant, with review controlled by owner
Agreement	Property owner and assigned lawyer
Payment record	Authorized finance role
Resident record	Property operator
Maintenance request	Property operator
Vendor record	Property operator
Legal matter	Assigned lawyer or owner administrator
Audit record	Platform-controlled

Business ownership does not automatically imply unrestricted technical access.

17. Annual Rent Business Model

The initial product will treat annual rent as the default rental model.

A rental offer may include:

Annual rent
Agency fee
Legal fee
Caution or security deposit
Service charge
Utility deposit
Inspection fee
Other approved charges

The platform must identify:

Amount
Currency
Charge type
Responsible payer
Recipient
Due date
Refundability
Payment status
Allocation
Supporting evidence

The platform must clearly separate rental income from deposits, service charges, legal fees, agency fees, and platform revenue.

18. Community Amenity Model

The functional masjid must be represented as a community amenity and shared facility.

Amenity records may include:

Name
Type
Description
Location
Availability
Operating status
Access rules
Responsible operator
Maintenance schedule
Community notices

Future amenities may include:

Parking
Security gate
Borehole
Water treatment
Generator
Solar power
Children’s play area
Garden
Waste collection
CCTV
Community hall
19. Development Expansion Model

The planned future block must be represented before construction begins.

A future block may progress through:

Concept
→ Planned
→ Approved
→ Under Construction
→ Finishing
→ Inspection
→ Completed
→ Operational

The business architecture must support the transition from development inventory into operational property inventory.

20. Revenue Capability Map

Potential Habitacores revenue streams depend on the following business capabilities.

20.1 Subscription Revenue

Requires:

Organization plans
Property or unit limits
Feature entitlements
Billing cycles
Usage tracking
Subscription status
20.2 Transaction Revenue

Requires:

Payment events
Service fees
Transaction records
Provider reconciliation
Refund handling
Revenue allocation
20.3 Managed-Service Revenue

Requires:

Service assignments
Operational work records
Property onboarding
Listing support
Application coordination
Renewal administration
Service reporting
20.4 Referral and Partnership Revenue

Requires:

Partner records
Referral attribution
Consent
Conversion tracking
Revenue-share records
Compliance controls

No revenue model is final until validated.

21. Business Performance Indicators

The business architecture should support measurement of:

Property Performance
Occupancy rate
Vacancy duration
Annual rent collected
Outstanding balances
Renewal rate
Maintenance cost
Unit turnaround time
Leasing Performance
Inquiry volume
Inspection conversion
Application conversion
Approval rate
Time to occupancy
Listing performance
Agent performance
Resident Experience
Maintenance-response time
Maintenance-resolution time
Communication response time
Renewal acceptance
Resident satisfaction
Complaint volume
Platform Performance
Active organizations
Active properties
Active units
Monthly active users
Workflow completion
Support requests
Subscription revenue
Transaction revenue
22. Business Risk Model

Habitacores must account for:

False ownership claims
Fraudulent listings
Inaccurate fees
Applicant-data misuse
Unauthorized access
Payment misallocation
Agreement disputes
Agent misrepresentation
Vendor non-performance
Maintenance delays
Regulatory non-compliance
Data loss
Platform downtime
Cross-tenant data exposure

Risk controls will be defined across the security, permissions, audit, and testing documents.

23. Business Governance

Material business decisions must be documented.

Governance records may include:

Product specifications
Business rules
Approval policies
Architecture Decision Records
Legal guidance
Pricing decisions
Risk acceptance
Process exceptions
Change proposals

No critical business policy should exist only in private conversations or informal messages.

24. Business Architecture Decision Principles

Business architecture decisions should:

Support the founding-team pilot.
Preserve future multi-tenant expansion.
Distinguish platform responsibilities from property responsibilities.
Preserve role accountability.
Maintain transparent financial records.
Keep business records auditable.
Avoid unnecessary operational complexity.
Allow configuration for different organizations.
Preserve human approval for critical decisions.
Map clearly to product and technical capabilities.
25. Current Business Architecture Decision

Habitacores will begin as an owner-operated platform validated through the founding-team-owned residential community.

The business architecture will separate platform, ownership, and property-operation responsibilities even where the same founding team initially performs all three.

The initial MVP will support the complete leasing and basic resident-operation lifecycle for Block A and its six units while preserving the business foundations required for future external organizations, additional blocks, multiple properties, and broader market expansion.