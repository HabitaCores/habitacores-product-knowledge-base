# Document 09 — System Architecture

## Document Control

| Field | Value |
|---|---|
| Product | Habitacores |
| Document | System Architecture |
| Version | 0.1 |
| Status | Draft for approval |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Market | Nigeria |
| Primary Currency | Nigerian naira |
| Primary Operational Time Zone | West Africa Time |
| Proposed Application Stack | Next.js and TypeScript |
| Proposed Data Platform | Supabase and PostgreSQL |
| Proposed Deployment Platform | Vercel |
| Last Updated | 30 July 2026 |

## 1. Purpose

This document defines the proposed system architecture for Habitacores.

It translates the approved:

- Product vision
- Product principles
- Business architecture
- Domain model
- User and role model
- Business rules
- Functional requirements
- Non-functional requirements

into an implementable technical structure.

This document defines:

- Architectural principles
- System boundaries
- Application composition
- Multi-tenant architecture
- Frontend architecture
- Backend architecture
- Data architecture
- Authentication and authorization
- File and document handling
- Background processing
- Notification delivery
- Payment integration
- AI integration
- Audit and observability
- Deployment topology
- Environment separation
- Security trust boundaries
- Scalability and resilience
- MVP architecture
- Future architectural evolution

This document does not define the complete database schema, API specification, permission matrix, user-interface wireframes, or deployment runbooks.

Those areas will be defined in later Product Knowledge Base documents.

## 2. Architecture Objectives

The Habitacores architecture must:

1. Support a secure multi-tenant SaaS platform.
2. Preserve strict organization-level data isolation.
3. Support the complete Nigerian annual-rent lifecycle.
4. Support the founding-team pilot without hardcoding the pilot structure.
5. Enforce server-side authorization.
6. Preserve financial, agreement, occupancy, and audit integrity.
7. Support responsive web experiences across desktop and mobile devices.
8. Remain usable under constrained network conditions.
9. Support reliable background processing.
10. Separate high-impact human decisions from AI assistance.
11. Support controlled integrations with payment, communication, identity, and AI providers.
12. Allow incremental growth without premature microservice complexity.
13. Remain testable, observable, maintainable, and recoverable.
14. Support future Android and iOS applications through stable APIs.
15. Preserve flexibility to replace external providers where practical.

## 3. Architecture Style

Habitacores Version 0.1 will use a:

> Modular multi-tenant SaaS architecture implemented initially as a modular monolith with controlled service boundaries.

The initial platform should not begin as a distributed microservice system.

The modular-monolith approach provides:

- Faster MVP delivery
- Lower infrastructure complexity
- Easier local development
- Simpler transactional consistency
- Lower operational cost
- Easier debugging
- Clearer domain evolution

The internal architecture must still preserve domain boundaries so that selected modules may later become independent services when justified.

## 4. Architecture Principles

### 4.1 Secure by Default

All protected capabilities must deny access unless permission is explicitly established.

### 4.2 Tenant Isolation by Design

Organization isolation must be enforced across:

- Application logic
- Database access
- File storage
- Search
- Caching
- Background jobs
- Reports
- Notifications
- AI context
- Integrations

### 4.3 Server-Side Authority

The server is the authoritative enforcement point for:

- Authentication
- Authorization
- Business rules
- Financial validation
- State transitions
- Data isolation
- Audit generation

### 4.4 Modular Domain Boundaries

The codebase should be divided into modules aligned with the Habitacores domain.

### 4.5 Explicit Business Workflows

Critical lifecycle transitions must use explicit commands or services rather than unrestricted record editing.

### 4.6 Immutable Historical Records

Signed agreements, verified financial events, decisions, occupancy history, and audit records must not be silently overwritten.

### 4.7 Human Accountability

AI may assist but must not independently finalize high-impact decisions.

### 4.8 Provider Abstraction

External providers should be accessed through internal interfaces where practical.

### 4.9 Configuration over Hardcoding

Organization-specific policies must be represented through configuration rather than tenant-specific source-code branches.

### 4.10 Operational Simplicity

The architecture should remain as simple as possible while satisfying security, integrity, and scalability requirements.

## 5. System Context

Habitacores operates between several user and external-system groups.

```text
Public Prospects
Applicants
Residents
Property Organizations
Estate Agents
Estate Lawyers
Finance Reviewers
Maintenance Coordinators
Vendors
Platform Administrators
        |
        v
Habitacores Web Platform
        |
        +--> Authentication Provider
        +--> PostgreSQL Database
        +--> Object Storage
        +--> Email Provider
        +--> SMS Provider
        +--> WhatsApp Provider
        +--> Payment Provider
        +--> AI Provider
        +--> Monitoring and Error Tracking
Habitacores remains the authoritative application record for:

Organization membership
Property structure
Listing status
Application status
Applicant decision
Charge status
Payment verification
Payment allocation
Agreement status
Occupancy status
Maintenance status
Role assignment
Audit history

External providers must not silently replace Habitacores business state.

6. High-Level Architecture

The initial platform will contain the following principal layers:

User Interface Layer
        |
Application and API Layer
        |
Domain and Business-Service Layer
        |
Authorization and Policy Layer
        |
Data Access Layer
        |
PostgreSQL, Storage, Queues, and External Providers
User Interface Layer

Responsible for:

Public marketplace
Authentication screens
Organization dashboards
Applicant portal
Resident portal
Vendor portal
Administrative interfaces
Responsive mobile-browser experiences
Application and API Layer

Responsible for:

Request handling
Input validation
Authentication context
Authorization invocation
Workflow orchestration
Response formatting
Error handling
Audit correlation
Domain and Business-Service Layer

Responsible for:

Business rules
Lifecycle transitions
Financial calculations
Agreement controls
Occupancy validation
Application decisions
Maintenance workflows
Notification triggers
Domain events
Authorization and Policy Layer

Responsible for evaluating:

User
Organization membership
Role
Permission
Scope
Assignment
Record state
Approval requirements
Data Access Layer

Responsible for:

Tenant-scoped queries
Transactions
Persistence
Concurrency control
Repository or query abstractions
Database mapping
Audit persistence
7. Proposed Technology Stack

The proposed initial stack is:

Application
Next.js
TypeScript
React
Server Components where appropriate
Server Actions or API route handlers where appropriate
Database
PostgreSQL
Supabase-managed PostgreSQL, subject to final approval
Authentication
Supabase Auth or another approved identity provider
File Storage
Supabase Storage or another private object-storage provider
Deployment
Vercel for the web application, subject to final infrastructure approval
Source Control
GitHub
Continuous Integration
GitHub Actions or approved equivalent
Testing
Vitest
React Testing Library
Playwright
Database integration tests
Authorization and tenant-isolation tests
Validation
TypeScript
Schema-validation library such as Zod
Styling and Components
An approved accessible component system
Responsive CSS architecture

These choices remain proposed until formally approved through architecture decision records.

8. Application Repository Strategy

The initial implementation may use a single primary application repository.

Suggested structure:

habitacores/
├── apps/
│   └── web/
├── packages/
│   ├── domain/
│   ├── database/
│   ├── authorization/
│   ├── validation/
│   ├── notifications/
│   ├── payments/
│   ├── ai/
│   ├── audit/
│   ├── ui/
│   └── configuration/
├── tests/
├── scripts/
├── docs/
└── infrastructure/

A simplified initial repository may use:

src/
├── app/
├── modules/
├── components/
├── lib/
├── database/
├── authorization/
├── integrations/
├── jobs/
└── tests/

The exact repository structure will be defined in the coding-standards document.

9. Domain Modules

The modular monolith should contain the following logical modules.

9.1 Identity Module

Responsible for:

User identity
Authentication profile
Verification status
Session-related application context
9.2 Organization Module

Responsible for:

Organizations
Memberships
Roles
Assignments
Organization relationships
9.3 Property Module

Responsible for:

Properties
Communities
Buildings
Blocks
Floors
Units
Amenities
Facilities
Development records
9.4 Marketplace Module

Responsible for:

Listings
Public listing search
Inquiries
Inspections
Lead assignment
9.5 Application Module

Responsible for:

Applications
Applicants
Co-applicants
Household members
Guarantors
Document requirements
Review
Decisions
Offers
9.6 Finance Module

Responsible for:

Charges
Payments
Allocations
Deposits
Credits
Adjustments
Reversals
Refunds
Receipts
9.7 Agreement Module

Responsible for:

Agreement templates
Agreement drafts
Legal review
Approval
Versioning
Signatures
Activation
Amendment
Termination
9.8 Occupancy Module

Responsible for:

Occupancy
Move-in
Residents
Household occupancy
Renewal
Move-out
Unit turnaround
9.9 Maintenance Module

Responsible for:

Maintenance requests
Triage
Work orders
Vendor assignment
Completion evidence
Verification
Closure
9.10 Document Module

Responsible for:

File metadata
Uploads
Access
Classification
Versioning
Review
Expiration
Retention
9.11 Communication Module

Responsible for:

Email
SMS
WhatsApp
In-app notifications
Templates
Delivery status
Communication history
9.12 Audit Module

Responsible for:

Audit-event generation
Audit persistence
Search
Export
Correlation identifiers
9.13 Platform Administration Module

Responsible for:

Platform organizations
Support access
Reference data
Feature configuration
Integration management
Platform health visibility
9.14 AI Assistance Module

Responsible for:

Application summaries
Document summaries
Maintenance classification
Communication drafting
Listing-description drafting
AI activity records
Human-review controls
10. Module Dependency Principles

Module dependencies must be controlled.

Preferred dependency direction:

User Interface
→ Application Services
→ Domain Services
→ Ports and Interfaces
→ Infrastructure Adapters

Domain logic must not depend directly on:

React components
Vercel APIs
Supabase client details
Email-provider SDKs
Payment-provider SDKs
AI-provider SDKs

External provider details should remain in infrastructure adapters.

11. Multi-Tenant Architecture

The primary tenant boundary is the Organization.

Most operational records must contain or inherit:

organization_id

Examples include:

Property
Community
Building
Block
Unit
Listing
Application
Charge
Payment
Agreement
Occupancy
Maintenance request
Work order
Document
Notification
Audit event
11.1 Tenant Resolution

Every protected request must resolve:

Authenticated user
Active organization
Active membership
Role
Scope
Assignment where applicable
Requested resource
Requested action
11.2 Tenant Validation

The server must verify that:

The user has authority in the organization.
The requested record belongs to the organization or an approved related organization.
The requested action is permitted.
The requested resource is within scope.
11.3 Database Enforcement

Tenant isolation should use multiple controls:

Organization-scoped queries
Server-side authorization
Database constraints
PostgreSQL Row Level Security where appropriate
Tenant-isolation tests
Restricted service credentials
Secure storage policies

No single layer should be treated as the only tenant-isolation defense.

11.4 Cross-Organization Relationships

Cross-organization access may occur only through explicit records such as:

Agency engagement
Legal assignment
Vendor assignment
Property-management agreement
Platform support session
12. Authentication Architecture

Authentication establishes the user’s identity.

The authentication layer should support:

Registration
Email verification
Login
Logout
Password reset
Multi-factor authentication
Session management
Account suspension
Session revocation

Authentication does not independently grant organization access.

After authentication, the application must resolve organization membership and authorization separately.

13. Authorization Architecture

Habitacores will use a combination of:

Role-based access control
Scope-based access control
Assignment-based access control
Record-state checks
Approval-policy checks

The access decision may be represented as:

Can user U perform action A
on resource R
within organization O
under role and scope S
given assignment X
and record state T?
13.1 Authorization Service

Authorization logic should be centralized through a dedicated policy layer.

Example conceptual interface:

authorize({
  userId,
  organizationId,
  action,
  resourceType,
  resourceId,
  context,
});
13.2 Permission Naming

Permissions should use explicit names such as:

property.read
property.update
listing.create
listing.approve
application.review
application.decide
payment.verify
payment.allocate
agreement.approve
occupancy.activate
maintenance.assign
role.grant
13.3 Field-Level Protection

Highly sensitive fields may require additional authorization.

Examples:

Government-identification number
Applicant financial information
Internal legal notes
Bank references
Support-access records
14. Frontend Architecture

The frontend will use a role-aware and context-aware application shell.

Primary interface areas may include:

Public Marketplace
Applicant Portal
Resident Portal
Organization Operations Portal
Professional Portal
Vendor Portal
Platform Administration
14.1 Public Marketplace

Responsible for:

Public listing search
Listing detail
Inquiry
Inspection request
Account conversion
14.2 Applicant Portal

Responsible for:

Application
Documents
Information requests
Application status
Offer response
Payment evidence
Agreement access
14.3 Resident Portal

Responsible for:

Occupancy
Agreements
Charges
Payments
Maintenance
Notices
Renewal
Move-out
14.4 Operations Portal

Responsible for:

Property hierarchy
Listings
Prospect pipeline
Applications
Finance
Agreements
Occupancy
Maintenance
Vendors
Reports
14.5 Active Context

The application shell must clearly display:

Active organization
Active role
Selected property or community where applicable
15. Backend and API Architecture

The backend may use:

Next.js route handlers
Server Actions
Internal application services
Background workers
Webhooks

The architectural boundary is more important than the framework mechanism.

15.1 Command Operations

State-changing operations should use explicit commands.

Examples:

ApproveApplication
VerifyPayment
AllocatePayment
ApproveAgreement
ActivateOccupancy
CompleteMoveIn
AssignWorkOrder
ApproveRefund
15.2 Query Operations

Read operations should return only authorized, scoped data.

15.3 API Response Model

APIs should provide consistent:

Success responses
Validation errors
Authorization errors
Conflict errors
Pending states
Correlation identifiers
15.4 API Versioning

Internal APIs may evolve with the application.

External partner or mobile APIs should use explicit versioning once exposed.

16. Database Architecture

PostgreSQL is the proposed system-of-record database.

16.1 Database Responsibilities

The database will store:

Identity references
Organizations
Memberships
Roles
Property hierarchy
Listings
Applications
Financial records
Agreements
Occupancy
Maintenance
Document metadata
Notifications
Audit records
Configuration
16.2 Relational Integrity

The database should use:

Primary keys
Foreign keys
Unique constraints
Check constraints
Transactional updates
Appropriate indexes
Concurrency controls
16.3 Identifier Strategy

Each principal record should use:

Internal permanent identifier
Optional business-facing identifier
Optional display label

Example:

Internal ID:
uuid

Business ID:
HAB-UNIT-000001

Display Label:
Block A — Unit 01
16.4 Financial Data

Financial amounts must use precise decimal or minor-unit representation.

Floating-point representation must not be used for authoritative monetary calculations.

16.5 Record History

Critical records may use:

Version tables
Event tables
Audit records
Immutable snapshots
Status-history tables

The final approach may differ by domain.

17. Transaction Boundaries

Transactions should protect operations that must complete together.

Examples include:

Payment Allocation
Validate payment
→ Validate charge
→ Create allocation
→ Update payment balance
→ Update charge balance
→ Create audit event
Move-In Completion
Validate agreement
→ Validate payment conditions
→ Validate unit readiness
→ Complete move-in checklist
→ Activate occupancy
→ Update unit occupancy status
→ Activate resident access
→ Create audit event
Application Decision
Validate reviewer authority
→ Validate application state
→ Record decision
→ Update application status
→ Create notification event
→ Create audit event
18. Concurrency Control

The architecture must prevent conflicting concurrent operations.

Potential controls include:

Database transactions
Row locking
Optimistic concurrency
Version numbers
Unique constraints
Idempotency keys

Concurrency protection is especially important for:

Application decisions
Payment allocations
Refunds
Agreement activation
Occupancy activation
Unit reservation
Role changes
19. File and Document Architecture

Files should be stored in private object storage.

The database should store file metadata rather than file contents.

19.1 Upload Flow
User requests upload
→ Server validates authorization
→ Upload location is issued
→ File is uploaded
→ File validation and malware scan occur
→ Metadata is finalized
→ Document becomes available according to status
19.2 File Metadata

File metadata should include:

Organization
Uploader
Related resource
File type
Document type
Sensitivity
Version
Validation status
Storage key
Size
Checksum
Created date
Expiration date where applicable
19.3 File Access

Private files must use:

Authorized requests
Short-lived signed URLs or equivalent access
Organization and assignment validation
Audit where required
19.4 Signed Agreements

Signed agreement files must be immutable.

A change must create a new agreement version.

20. Background-Job Architecture

Background processing is required for:

Email delivery
SMS delivery
WhatsApp delivery
File scanning
Image processing
Report generation
Import processing
Renewal reminders
Document-expiration reminders
Offer-expiration checks
AI processing
Payment-provider reconciliation
Audit export
20.1 Job Requirements

Jobs should support:

Durable processing
Retry
Idempotency
Organization context
Correlation identifier
Failure status
Dead-letter handling
Operational visibility
20.2 Job Isolation

Every tenant-owned job must include the relevant organization context.

A background job must not infer organization scope from untrusted input alone.

21. Domain Events

Business operations may publish internal domain events.

Examples:

ListingPublished
InspectionConfirmed
ApplicationSubmitted
ApplicationApproved
OfferIssued
PaymentVerified
AgreementActivated
OccupancyActivated
MaintenanceRequestCreated
WorkOrderCompleted
AgreementExpiring

Domain events may trigger:

Notifications
Audit events
Reporting updates
Background tasks
Integration calls

The original transaction must remain authoritative.

22. Notification Architecture

Notifications should be generated from approved business events.

The notification architecture should contain:

Trigger
Template
Recipient resolution
Channel selection
Queue
Provider adapter
Delivery status
Retry
Failure escalation
22.1 Channel Adapters

Internal interfaces should isolate:

Email provider
SMS provider
WhatsApp provider
Future push-notification provider
22.2 Authoritative State

Notification delivery must not change the business state unless the related workflow explicitly requires a confirmed response.

23. Payment Architecture

Habitacores must separate:

Charge
Payment
Payment Allocation
Deposit
Adjustment
Reversal
Refund
23.1 Payment Flow
Charge created
→ Payment initiated or recorded
→ Provider confirmation or manual evidence
→ Payment verification
→ Payment allocation
→ Charge settlement
→ Receipt
23.2 Manual Verification

Where manual payment verification is used:

Applicant or resident submits evidence.
Finance Reviewer reviews the evidence.
External bank or provider information is checked where available.
Payment is verified or rejected.
Verification is audited.

A screenshot alone cannot automatically verify payment.

23.3 Provider Integration

Payment providers should be accessed through an internal payment interface.

The integration must support:

Provider reference
Webhook verification
Idempotency
Pending status
Failure handling
Reconciliation
Refund reference
23.4 Financial Transactions

Allocation, reversal, and refund operations must use transactional controls.

24. Agreement and Signature Architecture

Agreement creation should use approved templates and versioned data.

Conceptual flow:

Approved application
→ Approved offer
→ Agreement draft
→ Legal review where required
→ Owner approval
→ Issue
→ Signature
→ Payment and readiness validation
→ Activation

The final electronic-signature provider remains pending.

The architecture should permit:

Internal acknowledgement
Uploaded signed document
Third-party electronic signature
Hybrid signature process
25. Search Architecture

Initial search may use PostgreSQL-supported search capabilities.

Search must be:

Tenant scoped
Permission aware
Paginated
Indexed
Protected from sensitive-field exposure

Future search may use a specialized service.

The domain model and API should not depend on a specific search provider.

26. Reporting Architecture

Operational dashboards may use:

Direct optimized queries
Database views
Materialized views
Precomputed summaries
Background aggregation

Reports must remain:

Organization scoped
Role restricted
Date aware
Currency aware
Auditable when exported

Analytics should not replace authoritative transactional records.

27. Audit Architecture

Audit generation should be centralized.

Audit records should capture:

Actor
Organization
Action
Resource
Resource identifier
Previous values where appropriate
New values where appropriate
Reason
Correlation identifier
Timestamp
Source service
IP or device context where lawful and useful

Ordinary users must not modify audit records.

Audit records may be stored in the primary database initially.

Future scale may justify separate archival storage.

28. Observability Architecture

The platform should provide:

Structured application logs
Error tracking
Request correlation
Performance metrics
Queue monitoring
Database monitoring
Provider health
Availability monitoring
Security alerts

Observability systems must minimize personal and sensitive data.

29. Error-Handling Architecture

Errors should be classified as:

Validation error
Authentication error
Authorization error
Not-found result
Conflict
Rate-limit response
External-provider failure
Pending external confirmation
Internal system failure

User-facing errors must be clear without exposing sensitive implementation details.

Material errors should include a correlation identifier.

30. AI Integration Architecture

AI functionality must remain optional and bounded.

30.1 Permitted AI Assistance

AI may assist with:

Application summarization
Document summarization
Maintenance categorization
Communication drafting
Listing-description drafting
Anomaly flagging
30.2 Prohibited Autonomous Actions

AI must not independently finalize:

Application approval
Application rejection
Agreement termination
Eviction
Payment verification
Refund denial
Ownership verification
Vendor suspension
Legal conclusions
30.3 AI Service Boundary

AI providers should be accessed through an internal AI interface.

The interface should support:

Provider abstraction
Organization context
Prompt templates
Data minimization
Output classification
Human-review status
Usage limits
Audit references
30.4 Retrieval and Grounding

Material AI summaries should use authorized Habitacores records as grounding context.

AI output must not replace those records.

30.5 Prompt Injection Protection

Uploaded files and user-supplied text must be treated as untrusted content.

Instructions contained inside documents must not override system authorization or application policy.

31. Security Trust Boundaries

The principal trust boundaries are:

Public User
→ Browser
→ Application Edge
→ Authenticated Application
→ Authorization Layer
→ Database and Storage
→ External Providers

Special trust boundaries exist around:

Authentication provider
Payment provider
AI provider
Communication providers
File-upload pipeline
Administrative interfaces
Support-access sessions
Webhooks

Every trust-boundary crossing requires:

Authentication or verification
Input validation
Authorization
Scope validation
Error handling
Logging where appropriate
32. Caching Architecture

Caching may be used for:

Public listing data
Static reference data
Approved public community information
Non-sensitive configuration
Derived dashboard summaries

Caching must not compromise:

Tenant isolation
Authorization
Data freshness
Financial correctness
Agreement state
Occupancy state

Cache keys must include organization or scope where tenant-specific data is cached.

33. Deployment Architecture

The initial deployment may use:

GitHub
  |
  v
Continuous Integration
  |
  v
Vercel Web Application
  |
  +--> Supabase PostgreSQL
  +--> Supabase Storage
  +--> Authentication Service
  +--> Background Job Service
  +--> Email/SMS/WhatsApp Providers
  +--> Payment Provider
  +--> AI Provider
  +--> Monitoring Services

The exact background-job and queue provider remains pending.

34. Environment Architecture

The project should maintain separated environments.

Local Development

Used for:

Developer implementation
Unit tests
Local validation
Local database where appropriate
Test or Integration

Used for:

Automated integration tests
Database testing
Authorization tests
Migration validation
Preview or Staging

Used for:

Pull-request previews
Product review
End-to-end testing
Release validation
Production

Used for live customer and pilot operations.

Production secrets and live personal data must not be copied casually into lower environments.

35. Continuous Integration Architecture

Every pull request should run:

Dependency installation
Formatting check
Linting
Type checking
Unit tests
Integration tests
Authorization tests
Tenant-isolation tests
Build
Migration validation
Secret scanning
Dependency security checks where configured

Production deployment must require successful mandatory checks.

36. Database Migration Architecture

Database migrations must be:

Version controlled
Reviewed
Tested
Repeatable
Backward compatible where practical
Recoverable

A migration should not:

Delete required historical records without approval
Collapse unknown values into assumed values
Remove organization scope
Break active agreements
Alter financial history silently
37. Backup and Recovery Architecture

The production database must use automated backups.

Recovery planning must include:

PostgreSQL data
File storage
Configuration
Secrets
Deployment definitions
Integration settings
Operational documentation

Initial targets:

Recovery Point Objective: 24 hours or less
Recovery Time Objective: 24 hours or less

The architecture should support future tighter targets for financial and agreement records.

38. Availability and Resilience Architecture

The MVP should target:

99.5% monthly availability

The architecture must support graceful degradation.

Examples:

AI failure does not block manual review.
WhatsApp failure does not block in-app access.
Analytics failure does not block financial operations.
Email failure does not erase an issued offer.
Payment-provider delay keeps payment pending rather than verified.

External calls must use:

Timeouts
Retry where appropriate
Idempotency
Failure status
Operational monitoring
39. Scalability Architecture

The architecture must support growth in:

Organizations
Properties
Communities
Buildings
Units
Listings
Applications
Payments
Agreements
Occupancies
Maintenance records
Documents
Audit events

Initial scaling should prioritize:

Efficient queries
Correct indexes
Pagination
Background jobs
Media optimization
Stateless web execution
Connection management
Query monitoring

Microservices should be introduced only when supported by measurable operational need.

40. Low-Bandwidth Architecture

The application should support constrained Nigerian network conditions through:

Server-rendered useful content
Optimized images
Lazy loading
Pagination
Small response payloads
Draft preservation
Retryable uploads
Clear loading states
Minimal unnecessary client JavaScript
Direct links from notifications

Critical text and actions must not depend on loading large media assets.

41. Accessibility Architecture

The component architecture should support WCAG 2.2 Level AA objectives.

Shared components should provide:

Semantic HTML
Keyboard navigation
Accessible labels
Focus management
Error association
Status announcements
Sufficient contrast
Reduced-motion support

Accessibility should be implemented through reusable components rather than corrected independently on every page.

42. Configuration Architecture

Configuration should support the following scopes:

Platform
→ Organization
→ Property
→ Community

Examples include:

Currency
Time zone
Inspection hours
Partial-payment policy
Legal-review policy
Renewal reminder schedule
Maintenance thresholds
Refund thresholds
Document requirements
Notification channels

Configuration should support:

Safe defaults
Inheritance
Approved overrides
Version history
Effective dates
Audit events
43. Data-Retention Architecture

Retention rules should be applied by record category.

Categories include:

Applications
Government identification
Agreements
Financial records
Occupancy records
Maintenance records
Communications
Audit records
Logs
Backups

Retention execution must account for:

Legal hold
Active disputes
Financial obligations
Agreement history
Privacy requests
Backup lifecycle
44. Pilot Architecture

The initial pilot must be represented through normal platform records.

Property
└── Community 001
    ├── Existing Building
    │   ├── Block A
    │   │   └── Six confirmed residential units
    │   └── Block B
    ├── Functional Masjid
    └── Building Under Construction
        └── Waterside context

The implementation must not hardcode:

Community 001 as the only community
Existing Building as a permanent public name
Two buildings as a platform limit
Two blocks as a platform limit
Six units as an organization limit
Block B unit count
Second-building inventory
Floor structure
Exact water-feature classification
45. MVP Architecture

The MVP architecture should include:

Application
One primary Next.js application
Modular domain structure
Responsive public and authenticated experiences
Server-side application services
Data
PostgreSQL
Organization-scoped records
Controlled migrations
Transactional financial operations
Immutable agreement versions
Audit records
Security
Authentication
Server-side authorization
Role, scope, and assignment evaluation
Multi-factor authentication for privileged roles
Private file storage
Secret management
Operations
Background processing
Email notifications
Payment recording and verification
Monitoring
Error tracking
Automated backups
CI/CD
Governance
Human approval for high-impact actions
AI bounded by explicit policy
Audit generation
Versioned configuration
Production-readiness checks
46. Deferred Architectural Capabilities

The following are deferred unless separately approved:

Full microservice decomposition
Multi-region active-active deployment
Native mobile applications
Dedicated search cluster
Distributed event-streaming platform
Advanced data warehouse
General-ledger accounting engine
Mortgage services
Insurance services
Public investment marketplace
Smart-building hardware
Biometric access control
Government-system integration
Fully autonomous AI agents
Automated applicant scoring
Cross-currency settlement
Dedicated enterprise identity federation
47. Future Architecture Evolution

Potential future evolution may include:

Phase 1
Modular monolith
Single PostgreSQL cluster
Private object storage
Managed authentication
Basic background jobs
Email and payment integration
Phase 2
Specialized search
Expanded queueing
Dedicated notification service
Advanced reporting
Native mobile API
Provider redundancy
Phase 3
Selective service extraction
Regional infrastructure
Data warehouse
Enterprise integrations
Advanced security monitoring
Formal compliance automation

Service extraction should be driven by:

Independent scaling need
Security isolation
Operational ownership
Release independence
Provider complexity
Reliability requirements
48. Architecture Decision Records

Material technical decisions should use Architecture Decision Records.

An ADR should contain:

Decision identifier
Title
Status
Context
Decision
Alternatives
Consequences
Security impact
Privacy impact
Cost impact
Reversal strategy
Approval date

Initial ADR candidates include:

ADR-001 — Select Next.js and TypeScript
ADR-002 — Select PostgreSQL
ADR-003 — Select Supabase or Alternative Data Platform
ADR-004 — Select Vercel or Alternative Hosting
ADR-005 — Adopt Modular Monolith
ADR-006 — Define Multi-Tenant Isolation Strategy
ADR-007 — Define Authentication Provider
ADR-008 — Define Background-Job Provider
ADR-009 — Define Payment Provider
ADR-010 — Define File Storage Strategy
49. Architecture Validation Requirements

The architecture must be validated through:

Domain review
Business-rule traceability
Functional-requirement traceability
Non-functional-requirement traceability
Threat modeling
Tenant-isolation testing
Authorization testing
Financial-integrity testing
Performance testing
Backup and recovery testing
Accessibility testing
Deployment testing
Migration testing
External-provider failure testing
AI safety testing
50. Architecture Acceptance Criteria

Example:

Requirement:
Organization Isolation

Given:
User A is a member of Organization A only.
Property B belongs to Organization B.

When:
User A requests Property B through a page, API, search, export, file link, or background task.

Then:
Access is denied.
No private Property B information is returned.
The response does not disclose unnecessary record details.
The denial is recorded where appropriate.

Example:

Requirement:
Payment Allocation Transaction

Given:
A verified payment has ₦400,000 available.
A charge has an outstanding balance of ₦500,000.

When:
A Finance Reviewer attempts to allocate ₦450,000.

Then:
The allocation is rejected.
The payment remains unchanged.
The charge remains unchanged.
No partial financial mutation occurs.
An actionable validation message is returned.

Example:

Requirement:
AI Provider Failure

Given:
An authorized reviewer requests an AI application summary.

When:
The AI provider is unavailable.

Then:
The application remains accessible.
The reviewer can continue manual review.
No application status changes automatically.
The failure is shown clearly.
The incident is logged.
51. Architecture Risks
51.1 Tenant Data Leakage

Risk:

A missing organization filter exposes another tenant’s data.

Controls:

Central authorization
Tenant-aware repositories
Row Level Security
Automated isolation tests
Tenant-safe caching
Secure storage policies
51.2 Financial Race Conditions

Risk:

Concurrent allocations or refunds create incorrect balances.

Controls:

Transactions
Concurrency checks
Unique constraints
Idempotency
Financial tests
51.3 Provider Lock-In

Risk:

Heavy dependence on one provider makes migration difficult.

Controls:

Internal interfaces
Stable domain models
Export capability
Provider adapters
ADR review
51.4 Overgrown Modular Monolith

Risk:

Modules become tightly coupled and difficult to maintain.

Controls:

Dependency boundaries
Module ownership
Architecture tests
ADR governance
Clear application services
51.5 AI Data Exposure

Risk:

Sensitive or cross-tenant information is sent to an AI provider.

Controls:

Tenant-scoped retrieval
Data minimization
Approved prompts
Provider controls
Audit
Human review
51.6 Operational Complexity

Risk:

Premature infrastructure complexity slows delivery.

Controls:

Managed services
Modular monolith
Minimal initial provider set
Cost monitoring
Incremental evolution
52. Unresolved Architecture Decisions

The following remain pending:

Final hosting provider
Final database provider
Final authentication provider
Final object-storage provider
Final background-job and queue provider
Final email provider
Final SMS provider
Final WhatsApp provider
Final payment provider
Final electronic-signature provider
Final AI provider and model
Final error-tracking provider
Final monitoring provider
Final analytics provider
Exact Row Level Security strategy
Exact data-access library or ORM
Exact API style
Exact server-action usage
Exact repository structure
Exact UI component system
Exact cache provider
Exact search implementation
Exact malware-scanning service
Exact backup retention
Exact deployment regions
Exact production support-access implementation
Exact audit-storage strategy
Exact identifier-generation strategy
Exact event and queue implementation
Exact financial reconciliation integration

These decisions must be resolved through Architecture Decision Records before the affected implementation becomes production dependent.

53. Architecture Governance

Changes to the approved system architecture require:

Change description
Business rationale
Affected modules
Domain impact
Functional impact
Security impact
Privacy impact
Tenant-isolation impact
Financial-integrity impact
Reliability impact
Performance impact
Cost impact
Deployment impact
Migration impact
Test impact
Rollback or reversal strategy
Architecture Decision Record where material
Product Visionary approval
Chief Product Architect approval
Effective version
54. Current System Architecture Decision

Habitacores will initially use a secure modular-monolith architecture built around a multi-tenant PostgreSQL system of record.

The primary architecture is:

Responsive Web Experiences
→ Application and API Layer
→ Central Authorization
→ Domain and Workflow Services
→ Tenant-Scoped Data Access
→ PostgreSQL and Private Object Storage
→ Background Jobs and External Provider Adapters
→ Audit, Monitoring, and Recovery Controls

The initial proposed implementation stack is:

Next.js
TypeScript
PostgreSQL
Supabase
Vercel
GitHub

These provider choices remain subject to formal Architecture Decision Records.

The architecture must preserve:

Organization isolation
Server-side authorization
Role and assignment scope
Financial correctness
Immutable agreement history
Occupancy integrity
Secure file access
Reliable background processing
Human accountability
AI safety
Mobile usability
Low-bandwidth performance
Auditability
Recoverability
Future SaaS extensibility

No technical convenience, provider limitation, performance optimization, or delivery deadline may override an approved security, privacy, tenant-isolation, financial-integrity, or legal-integrity requirement.