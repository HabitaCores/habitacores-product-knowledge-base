# Document 22 — Implementation Backlog and Delivery Plan

## Document Control

| Field | Value |
|---|---|
| Product | Habitacores |
| Document | Implementation Backlog and Delivery Plan |
| Version | 0.1 |
| Status | Draft for approval |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Market | Nigeria |
| Initial Geographic Focus | Lagos and Southwest Nigeria |
| Primary Currency | Nigerian naira |
| Primary Operational Time Zone | West Africa Time |
| Primary Tenant Boundary | Organization |
| Primary Experience | Responsive web application |
| Proposed Application Framework | Next.js and TypeScript |
| Proposed System of Record | PostgreSQL |
| Initial Architecture | Modular monolith |
| Delivery Model | Vertical-slice, dependency-aware, risk-first, and evidence-based |
| Implementation Unit Model | Epic → Capability → Feature → Story → Task → Acceptance Evidence |
| Last Updated | 30 July 2026 |

## 1. Purpose

This document defines the canonical implementation backlog and delivery plan for Habitacores.

It translates the approved Product Knowledge Base into executable implementation units.

The document converts the approved:

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
- Data architecture and database design
- Permission matrix and authorization model
- User journeys and workflow specifications
- API architecture and specification
- UX architecture and information architecture
- UI design system and interaction standards
- Security architecture and threat model
- Testing and quality strategy
- Infrastructure, environments, and deployment architecture
- Observability, reliability, and operational support
- Integration architecture and external systems strategy
- MVP scope, release plan, and product roadmap

into:

- Delivery epics
- capabilities
- features
- user stories
- technical enablers
- implementation tasks
- dependencies
- release mappings
- vertical slices
- risk priorities
- acceptance criteria
- quality gates
- pilot-readiness activities
- operational-readiness activities
- Codex-compatible work packages

This document is the bridge between product architecture and software implementation.

It does not replace:

- Architecture Decision Records
- source-code documentation
- sprint plans
- engineering estimates
- issue-tracker records
- pull-request descriptions
- test reports
- provider-selection records
- release notes
- runbooks
- production incident procedures

Those artifacts will derive from this document.

## 2. Delivery Objectives

The implementation plan must:

1. Preserve approved product intent.
2. prevent developers or AI coding agents from inventing material product decisions.
3. build the riskiest foundations before dependent workflows.
4. preserve organization-level tenant isolation.
5. preserve server-side authorization.
6. preserve financial correctness.
7. preserve agreement immutability.
8. preserve occupancy integrity.
9. preserve resident and applicant privacy.
10. support mobile and low-bandwidth use.
11. maintain accessibility.
12. support automated testing.
13. support observable and recoverable deployment.
14. support the founding pilot without hardcoding it.
15. support future organizations.
16. use vertical slices that produce working product evidence.
17. allow controlled manual fallbacks.
18. ensure every backlog item has acceptance evidence.
19. ensure critical requirements are traceable.
20. allow Codex to work within bounded implementation units.
21. make unresolved decisions explicit.
22. avoid premature microservices.
23. avoid speculative post-MVP work.
24. support phased production readiness.
25. preserve clean architecture and future extensibility.

## 3. Delivery Principles

### 3.1 Foundation Before Dependency

A dependent capability must not be implemented before its authoritative foundation.

Examples:

- Payment allocation must not precede payment verification.
- Occupancy activation must not precede agreement activation.
- Vendor access must not precede assignment authorization.
- AI retrieval must not precede tenant-safe access controls.
- public APIs must not precede partner authorization.

### 3.2 Vertical Slices Over Horizontal Completion

The team should prefer working vertical slices that include:

- Data
- domain rule
- API
- authorization
- UI
- audit
- tests
- telemetry

over isolated layers that cannot be validated by a user.

### 3.3 Risk-First Sequencing

Critical risk areas should be implemented and tested early.

These include:

- Tenant isolation
- authentication
- authorization
- payment integrity
- agreement immutability
- occupancy conflict prevention
- restricted-document access
- provider webhook security

### 3.4 No Hidden Business Logic

Material business rules must not exist only in:

- UI code
- comments
- spreadsheets
- prompts
- provider configuration
- undocumented database procedures
- developer memory

### 3.5 Server Authority

The server and domain layer remain authoritative for protected actions.

### 3.6 Deterministic Core

Core authorization, finance, agreement, occupancy, and audit behavior must be deterministic.

### 3.7 AI Is Bounded

AI-assisted coding and product features must operate within approved boundaries.

### 3.8 Test With Implementation

Tests are part of the implementation unit, not deferred cleanup.

### 3.9 Observability With Implementation

Material workflows must include logging, metrics, audit, and operational visibility before completion.

### 3.10 Documentation With Change

Architecture, API, schema, and operational documentation must remain synchronized with implementation.

## 4. Backlog Hierarchy

Habitacores will use:

```text
Product Objective
→ Release
→ Epic
→ Capability
→ Feature
→ User Story
→ Engineering Task
→ Acceptance Evidence
````

## 5. Epic Definition

An epic is a major product or platform outcome.

An epic:

* Has a business objective.
* spans multiple capabilities.
* may span multiple releases.
* has measurable exit criteria.
* has named dependencies.
* has architectural significance.

## 6. Capability Definition

A capability is a coherent product or platform ability.

Examples:

* Organization membership
* property hierarchy
* application review
* payment verification
* agreement versioning
* occupancy activation

## 7. Feature Definition

A feature is a user-visible or system-visible function within a capability.

Example:

```text
Capability:
Payment Verification

Features:
Verification queue
Evidence review
Approve
Reject
Request information
Duplicate warning
Audit history
```

## 8. User Story Definition

A user story should express:

```text
As a [role],
I need [capability],
so that [outcome].
```

Stories must not omit security, tenant, state, or authorization context.

## 9. Technical Enabler Definition

A technical enabler supports multiple product capabilities.

Examples:

* Authorization policy engine
* audit-event service
* upload-intent service
* background-job framework
* provider-adapter framework
* correlation-ID middleware

## 10. Task Definition

A task is an implementation action small enough to review and verify.

Tasks may include:

* Schema migration
* domain service
* API route
* page
* component
* test
* telemetry
* documentation
* deployment configuration

## 11. Acceptance Evidence

Acceptance evidence may include:

* Passing automated test
* test report
* screenshot
* trace
* audit record
* API response
* accessibility result
* performance result
* approved manual validation
* staging demonstration
* production smoke result

## 12. Backlog Identifiers

Suggested identifier format:

```text
HC-EPIC-001
HC-CAP-001
HC-FEAT-001
HC-STORY-001
HC-TASK-001
HC-ADR-001
HC-DEFECT-001
```

Identifiers must remain stable after creation.

## 13. Priority Classes

### P0 — Mandatory Foundation

Required before protected workflows can operate safely.

### P1 — Core MVP

Required to complete the controlled pilot lifecycle.

### P2 — Pilot Enhancement

Improves efficiency but has an approved fallback.

### P3 — Post-MVP

Deferred until the MVP is proven.

## 14. Risk Classes

### Critical

Failure may cause:

* Cross-tenant exposure
* financial corruption
* agreement corruption
* unauthorized occupancy
* restricted-data exposure
* severe outage

### High

Failure blocks or materially damages a core workflow.

### Medium

Failure causes recoverable operational disruption.

### Low

Failure causes cosmetic or low-impact inconvenience.

## 15. Estimation Model

Initial planning may use relative sizing:

* XS
* S
* M
* L
* XL

An XL item must normally be decomposed before implementation.

Time estimates remain separate from relative complexity.

## 16. Dependency Types

Dependencies may be:

* Architectural
* data
* authorization
* workflow
* provider
* legal
* security
* design
* infrastructure
* operational
* business-policy
* pilot-data

## 17. Work Item Readiness

A work item is ready only when:

* Objective is clear.
* actor is clear.
* organization context is clear.
* permissions are clear.
* business rules are clear.
* state transitions are clear.
* data impact is clear.
* UI or API impact is clear.
* security impact is clear.
* acceptance criteria exist.
* unresolved dependencies are identified.
* test approach is defined.
* rollout impact is understood.

## 18. Work Item Completion

A work item is complete only when:

* Code is implemented.
* tests pass.
* authorization is tested.
* tenant isolation is tested where applicable.
* audit is implemented where required.
* telemetry is implemented where required.
* accessibility is reviewed where applicable.
* documentation is updated.
* migration is validated.
* staging validation passes.
* acceptance criteria are satisfied.
* pull request is approved.

## 19. Release Mapping

The implementation backlog maps to:

```text
R0 — Architecture and Product Knowledge Base
R1 — Engineering Foundation
R2 — Internal Alpha
R3 — Founding Organization Alpha
R4 — Controlled Pilot Beta
R5 — Production MVP
R6 — Multi-Organization Readiness
R7 — Commercial Expansion
```

## 20. Epic Inventory

The MVP implementation backlog includes:

1. EPIC 01 — Engineering Foundation
2. EPIC 02 — Identity and Authentication
3. EPIC 03 — Organizations, Memberships, and Authorization
4. EPIC 04 — Property Hierarchy and Inventory
5. EPIC 05 — Design System and Application Shell
6. EPIC 06 — Public Marketplace
7. EPIC 07 — Inquiries and Inspections
8. EPIC 08 — Applications and Screening
9. EPIC 09 — Decisions and Offers
10. EPIC 10 — Charges and Financial Ledger
11. EPIC 11 — Payments, Verification, and Allocation
12. EPIC 12 — Agreements and Signatures
13. EPIC 13 — Occupancy and Move-In
14. EPIC 14 — Resident Portal
15. EPIC 15 — Maintenance and Vendor Operations
16. EPIC 16 — Renewals and Move-Out
17. EPIC 17 — Documents and File Security
18. EPIC 18 — Communications and Notifications
19. EPIC 19 — Search, Reporting, and Dashboards
20. EPIC 20 — Audit, Support, and Platform Administration
21. EPIC 21 — Integrations and Provider Framework
22. EPIC 22 — Security Hardening
23. EPIC 23 — Quality Engineering
24. EPIC 24 — Infrastructure and Deployment
25. EPIC 25 — Observability and Operations
26. EPIC 26 — Pilot Data and Onboarding
27. EPIC 27 — Production Readiness
28. EPIC 28 — Multi-Organization Readiness
29. EPIC 29 — AI-Assisted Capabilities
30. EPIC 30 — Commercialization Foundation

# EPIC 01 — Engineering Foundation

## 21. Epic Objective

Create a secure, testable, deployable application foundation for all Habitacores capabilities.

## 22. Epic Priority

```text
Priority: P0
Risk: Critical
Primary Release: R1
```

## 23. Engineering Foundation Capabilities

* Application repository
* workspace structure
* Next.js application
* TypeScript
* environment configuration
* database access
* migration framework
* API conventions
* validation framework
* error model
* domain-module structure
* test framework
* CI foundation
* coding standards

## 24. HC-CAP-001 — Application Repository

### Features

* Repository initialization
* protected main branch
* pull-request workflow
* branch naming
* commit conventions
* issue templates
* pull-request template
* CODEOWNERS later if needed

### Acceptance Criteria

* Repository is private or appropriately controlled.
* `main` is protected.
* required checks are configured.
* direct production secrets are absent.
* pull requests are required.
* repository documentation identifies the product and architecture source of truth.

## 25. HC-CAP-002 — Application Workspace

Recommended initial structure:

```text
apps/
  web/

packages/
  config/
  database/
  domain/
  authorization/
  audit/
  integrations/
  jobs/
  observability/
  testing/
  ui/

docs/
```

The final structure must be approved through an ADR before implementation.

## 26. HC-CAP-003 — TypeScript Foundation

Implementation tasks:

* Strict TypeScript configuration
* shared aliases
* no uncontrolled `any`
* type-safe environment schema
* type-safe API contracts
* type-safe database models
* type-safe domain identifiers

## 27. HC-CAP-004 — Environment Configuration

Tasks:

* Environment schema
* startup validation
* public-versus-secret separation
* environment identifier
* sandbox-versus-production checks
* safe defaults
* configuration documentation

Acceptance:

* Missing required production configuration causes safe startup failure.
* Production secrets are not exposed to the browser.
* Production credentials cannot be used silently in preview.

## 28. HC-CAP-005 — Error Architecture

Tasks:

* Stable internal error categories
* safe API error envelope
* correlation identifiers
* field errors
* conflict errors
* authorization errors
* provider errors
* operational error logging

Illustrative envelope:

```json
{
  "error": {
    "code": "PAYMENT_BALANCE_CONFLICT",
    "message": "The payment balance changed. Refresh and try again.",
    "correlationId": "01HXYZ"
  }
}
```

## 29. HC-CAP-006 — Validation Framework

Tasks:

* Request schemas
* command schemas
* response schemas
* environment schemas
* provider schemas
* form schemas
* unknown-enum handling
* money validation
* date validation

## 30. HC-CAP-007 — Domain Module Structure

Initial modules:

* Identity
* Organizations
* Properties
* Listings
* Leasing
* Finance
* Agreements
* Occupancy
* Maintenance
* Documents
* Communications
* Support
* Platform
* Integrations

## 31. HC-CAP-008 — Test Foundation

Tasks:

* Unit-test framework
* component-test framework
* integration-test environment
* database-test setup
* API-test setup
* browser-test setup
* fixtures
* factories
* synthetic tenants
* CI reports

## 32. Engineering Foundation Exit Criteria

* Application builds.
* strict type checking passes.
* test command passes.
* database migration can run.
* environment validation works.
* API error model works.
* correlation ID is returned.
* deployment to non-production succeeds.
* no critical dependency vulnerability remains unresolved.

# EPIC 02 — Identity and Authentication

## 33. Epic Objective

Provide secure user identity, account lifecycle, authentication, session, recovery, and privileged MFA.

## 34. Epic Classification

```text
Priority: P0
Risk: Critical
Primary Release: R1
```

## 35. HC-CAP-009 — User Account

Data requirements:

* User ID
* email
* normalized email
* phone where approved
* display name
* account status
* verification status
* created time
* last authentication time
* suspension metadata

## 36. HC-CAP-010 — Registration

User story:

```text
As a prospective Habitacores user,
I need to create an account,
so that I can begin an authorized workflow.
```

Tasks:

* Registration form
* email normalization
* duplicate handling
* verification initiation
* rate limiting
* bot protection foundation
* audit event
* safe errors

## 37. HC-CAP-011 — Email Verification

Tasks:

* Verification token
* expiry
* single use
* resend
* rate limit
* safe response
* audit

## 38. HC-CAP-012 — Login

Tasks:

* Credential validation
* account-state check
* session creation
* session rotation
* failed-login tracking
* rate limiting
* secure cookies
* audit
* suspicious-login signal

## 39. HC-CAP-013 — Password Recovery

Tasks:

* Recovery request
* enumeration-resistant response
* token expiry
* token single use
* password update
* session revocation
* audit

## 40. HC-CAP-014 — Session Management

Features:

* Active sessions
* current session
* revoke session
* revoke all other sessions
* inactivity timeout
* absolute timeout
* session metadata
* account suspension invalidation

## 41. HC-CAP-015 — Privileged MFA

Required for:

* Platform Administrator
* Platform Support Agent with privileged scope
* Organization Owner
* high-risk finance approver
* emergency-access user

Tasks:

* Enrollment
* verification
* recovery method
* disable control
* step-up flow
* audit

## 42. Identity Acceptance Criteria

* Invalid login does not reveal account existence unnecessarily.
* suspended accounts cannot use protected functions.
* password reset revokes applicable sessions.
* privileged user cannot perform protected high-risk actions without required MFA.
* session identifiers rotate after authentication.
* authentication events are audited safely.

# EPIC 03 — Organizations, Memberships, and Authorization

## 43. Epic Objective

Implement the canonical multi-tenant boundary and authorization model.

## 44. Epic Classification

```text
Priority: P0
Risk: Critical
Primary Release: R1
```

## 45. HC-CAP-016 — Organization

Data:

* Organization ID
* legal or display name
* slug
* status
* type
* primary contact
* configuration
* created time
* suspension metadata

## 46. HC-CAP-017 — Membership

Data:

* Membership ID
* user
* organization
* status
* invited by
* invited time
* activated time
* suspended time
* ended time

## 47. HC-CAP-018 — Membership Invitation

Tasks:

* Invite user
* resolve existing user
* invitation token
* expiry
* accept
* reject
* cancel
* resend
* audit

## 48. HC-CAP-019 — Role Assignment

Initial roles:

* Organization Owner
* Organization Administrator
* Property Operator
* Estate Agent
* Finance Reviewer
* Maintenance Coordinator
* Estate Lawyer
* Vendor Worker
* Platform Administrator
* Platform Support Agent

## 49. HC-CAP-020 — Permission Model

Tasks:

* Permission catalogue
* role-permission mapping
* custom role decision deferred
* permission checks
* sensitive permissions
* test matrix
* audit for role changes

## 50. HC-CAP-021 — Authorization Service

Authorization evaluation:

```text
Identity
→ Account status
→ Organization context
→ Membership status
→ Permission
→ Resource organization
→ Scope
→ Assignment
→ Record state
→ Separation of duties
```

## 51. HC-CAP-022 — Organization Context Switch

Tasks:

* Eligible organization list
* active context
* context switch
* cache invalidation
* route update
* audit where appropriate
* no cross-tenant state residue

## 52. HC-CAP-023 — Professional Assignment

Data:

* Assignment ID
* professional user
* organization
* resource type
* resource ID
* status
* scope
* start
* expiry
* assigned by
* revoked by
* reason

## 53. HC-CAP-024 — Support Session Authorization

Tasks:

* Support case requirement
* requested organization
* requested scope
* approval
* start
* expiry
* revoke
* session-bound access
* audit
* customer visibility later

## 54. Tenant Isolation Test Matrix

Every organization-owned endpoint must test:

* Authorized same-organization access
* unauthorized same-organization access
* cross-organization read
* cross-organization write
* suspended membership
* expired assignment
* revoked assignment
* organization switch
* stale cache
* platform-role boundary

## 55. Authorization Exit Criteria

* All protected resources require server-side authorization.
* cross-tenant access returns no protected metadata.
* role labels in the client are not sufficient authorization.
* expired assignments stop authorizing access.
* membership revocation invalidates protected access.
* authorization test matrix passes.

# EPIC 04 — Property Hierarchy and Inventory

## 56. Epic Objective

Represent configurable property structures without pilot-specific hardcoding.

## 57. Epic Classification

```text
Priority: P0
Risk: High
Primary Release: R2
```

## 58. HC-CAP-025 — Property Hierarchy

Canonical hierarchy:

```text
Organization
→ Property
→ Community
→ Building
→ Block
→ Floor
→ Unit
```

## 59. HC-CAP-026 — Property

Features:

* Create
* edit
* view
* status
* address
* general location
* owner organization
* operational team
* internal notes
* documents
* audit

## 60. HC-CAP-027 — Community

Features:

* Create
* edit
* parent property
* display name
* description
* status
* location context
* amenities
* audit

## 61. HC-CAP-028 — Building

Features:

* Create
* edit
* parent community
* development status
* operational status
* display name
* construction notes
* audit

## 62. HC-CAP-029 — Block

Features:

* Create
* edit
* parent building
* display label
* status
* confirmed unit count where known
* unknown unit count
* audit

## 63. HC-CAP-030 — Floor

Features:

* Optional creation
* floor label
* sequence
* parent block
* status
* no forced floor creation

## 64. HC-CAP-031 — Unit

Features:

* Unit identifier
* display label
* parent structure
* type
* bedrooms
* bathrooms
* status
* readiness
* annual rent
* deposit
* service charge
* availability
* amenities
* media
* audit

## 65. HC-CAP-032 — Amenities

Amenity types may include:

* Masjid
* parking
* water
* security
* common area
* power provision
* accessibility feature
* approved community feature

## 66. HC-CAP-033 — Unit Readiness

Readiness may include:

* Unknown
* inspection required
* repairs required
* cleaning required
* ready
* hold

Readiness must remain separate from availability where needed.

## 67. Pilot Configuration Acceptance

The system must support:

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

Without inventing:

* Block B units
* second-building units
* floors
* public names
* exact water-feature type

## 68. Property Epic Exit Criteria

* Founding property can be represented accurately.
* Block A contains six confirmed units.
* Block B count remains null or pending.
* hierarchy supports a synthetic second organization.
* display names can change without changing IDs.
* no fixed number of hierarchy levels is assumed beyond approved parent rules.

# EPIC 05 — Design System and Application Shell

## 69. Epic Objective

Create a consistent, accessible, responsive interface foundation.

## 70. Epic Classification

```text
Priority: P0
Risk: High
Primary Release: R1–R2
```

## 71. HC-CAP-034 — Design Tokens

Tokens:

* Typography
* spacing
* sizing
* radius
* elevation
* borders
* motion
* breakpoints
* semantic color roles

Final brand values require approved design decisions.

## 72. HC-CAP-035 — Core Components

Components:

* Button
* link
* input
* select
* checkbox
* radio
* textarea
* date input
* money input
* status badge
* alert
* toast
* dialog
* drawer
* table
* card
* tabs
* breadcrumb
* pagination
* skeleton
* empty state

## 73. HC-CAP-036 — Application Shell

Features:

* Public shell
* personal portal shell
* organization shell
* platform shell
* responsive navigation
* organization switcher
* role-aware navigation
* account menu
* notification access
* support access

## 74. HC-CAP-037 — Form Architecture

Features:

* Field labels
* help
* validation
* error summary
* focus management
* autosave status
* unsaved-change warning
* submit protection
* accessible confirmation

## 75. HC-CAP-038 — Table Architecture

Features:

* Sorting
* filtering
* pagination
* loading
* empty state
* error state
* mobile transformation
* row actions
* authorization-aware actions

## 76. HC-CAP-039 — Accessibility Foundation

Tasks:

* Keyboard support
* focus visibility
* semantic structure
* accessible names
* error announcements
* dialog focus
* contrast
* zoom
* reflow
* reduced motion

# EPIC 06 — Public Marketplace

## 77. Epic Objective

Allow public discovery of approved available units without exposing private operational data.

## 78. Epic Classification

```text
Priority: P1
Risk: Medium
Primary Release: R3
```

## 79. HC-CAP-040 — Listing

Data:

* Listing ID
* organization
* unit
* title
* summary
* description
* public rent
* public service charge
* deposit
* availability
* location summary
* approved amenities
* media
* publication status
* publish and expiry dates

## 80. HC-CAP-041 — Listing Workflow

States:

* Draft
* under review
* approved
* published
* paused
* expired
* archived

## 81. HC-CAP-042 — Public Search

Features:

* Search
* location filter
* rent filter
* bedroom filter where known
* availability
* pagination
* mobile display
* no restricted data leakage

## 82. HC-CAP-043 — Listing Details

Include:

* Approved images
* annual rent
* service charge
* deposit
* unit features
* approved amenities
* general location
* availability
* inquiry
* inspection request

## 83. Marketplace Acceptance Criteria

* Unpublished listings are not public.
* restricted coordinates are not exposed.
* occupied units cannot remain publicly available without an approved exceptional state.
* search returns only approved public fields.
* unknown values display as unconfirmed rather than zero.

# EPIC 07 — Inquiries and Inspections

## 84. Epic Objective

Capture prospect interest and support controlled inspection workflows.

## 85. HC-CAP-044 — Inquiry

Features:

* Public submission
* listing relationship
* contact details
* message
* category
* acknowledgment
* assigned user
* status
* internal notes
* communication history

## 86. HC-CAP-045 — Inquiry Assignment

Features:

* Assign agent
* reassign
* unassign
* assignment audit
* scoped visibility
* queue

## 87. HC-CAP-046 — Inspection Request

Features:

* Requested dates
* proposed time
* assigned agent
* confirm
* reschedule
* cancel
* complete
* outcome
* notes
* notifications

## 88. HC-CAP-047 — Inquiry Conversion

Possible conversions:

* Inquiry to inspection
* inquiry to application
* inspection to application
* closed inquiry

# EPIC 08 — Applications and Screening

## 89. Epic Objective

Support secure, mobile-friendly application submission and controlled review.

## 90. Epic Classification

```text
Priority: P1
Risk: High
Primary Release: R3
```

## 91. HC-CAP-048 — Application Draft

Features:

* Create from listing or unit
* save draft
* resume
* progress
* autosave
* expiry
* withdrawal
* draft ownership

## 92. HC-CAP-049 — Applicant Profile

Fields may include:

* Name
* contact
* current address
* employment declaration
* income declaration
* household
* intended occupancy
* guarantor requirement
* declarations

Final fields require privacy and legal review.

## 93. HC-CAP-050 — Application Documents

Features:

* Required document list
* upload
* scan
* replace
* view status
* remove before submission where permitted
* restricted access
* versioning

## 94. HC-CAP-051 — Application Submission

Submission must validate:

* Required sections
* required documents
* declarations
* applicant authority
* unit eligibility
* application version
* duplicate submission
* organization
* audit

## 95. HC-CAP-052 — Review Queue

Features:

* Submitted applications
* age
* completeness
* assigned reviewer
* intended unit
* risk or attention indicators without opaque scoring
* filters
* priority

## 96. HC-CAP-053 — Information Request

Features:

* Request details
* requested fields or documents
* deadline
* applicant response
* repeated request
* history
* notification

## 97. HC-CAP-054 — Review Notes

Controls:

* Internal only
* role restricted
* field-level access
* no vendor access
* no public access
* audit where sensitive

## 98. Application Acceptance Criteria

* Draft remains applicant scoped.
* submitted application is versioned.
* vendor cannot access application data.
* cross-tenant access is denied.
* submission is idempotent.
* applicant can complete the critical workflow on mobile.

# EPIC 09 — Decisions and Offers

## 99. Epic Objective

Support controlled, auditable application decisions and offers.

## 100. HC-CAP-055 — Recommendation

Features:

* Reviewer recommendation
* rationale
* conditions
* supporting references
* status
* no automatic final decision

## 101. HC-CAP-056 — Decision

States:

* Approved
* approved with conditions
* rejected
* withdrawn
* expired

Controls:

* Decision permission
* separation of duties where approved
* reason
* audit
* applicant notification

## 102. HC-CAP-057 — Offer

Features:

* Unit
* term
* annual rent
* deposit
* service charge
* other approved charges
* conditions
* issue
* accept
* decline
* expire
* replace

## 103. Offer Acceptance

Acceptance must validate:

* Offer current
* applicant identity
* unit still eligible
* offer not expired
* offer not superseded
* idempotency
* audit

# EPIC 10 — Charges and Financial Ledger

## 104. Epic Objective

Create authoritative charges and auditable financial records.

## 105. Epic Classification

```text
Priority: P1
Risk: Critical
Primary Release: R4
```

## 106. HC-CAP-058 — Charge

Charge types:

* Annual rent
* security deposit
* service charge
* approved fee
* maintenance charge
* adjustment
* waiver

## 107. Charge Data

* Charge ID
* organization
* payer
* occupancy or application relationship
* charge type
* amount
* currency
* due date
* status
* original amount
* adjusted amount
* outstanding balance
* created by
* audit

## 108. HC-CAP-059 — Money Model

Requirements:

* Decimal-safe representation
* currency code
* no binary floating-point
* approved rounding
* exact balance calculations
* display formatting separate from storage

## 109. HC-CAP-060 — Adjustment

Features:

* Adjustment amount
* direction
* reason
* approval
* source charge
* audit
* no history deletion

## 110. HC-CAP-061 — Waiver

Features:

* Requested amount
* reason
* authority
* approval
* effective balance
* audit
* optional expiry

## 111. Financial Ledger Principles

* Append-oriented history
* explicit corrections
* no silent deletion
* no untraceable balance edits
* transaction-based updates
* audit

# EPIC 11 — Payments, Verification, and Allocation

## 112. Epic Objective

Support payment evidence, verification, provider confirmation, allocation, receipts, refunds, and reversals without false settlement.

## 113. Epic Classification

```text
Priority: P1
Risk: Critical
Primary Release: R4
```

## 114. HC-CAP-062 — Payment Record

Data:

* Payment ID
* organization
* payer
* amount
* currency
* method
* payment date
* provider
* provider reference
* bank reference
* verification status
* available balance
* audit

## 115. HC-CAP-063 — Payment Evidence

Features:

* Upload
* scan
* document relationship
* submitted time
* evidence type
* replacement
* reviewer access
* duplicate hash warning

Boundary:

```text
Evidence submitted
≠ Payment verified
```

## 116. HC-CAP-064 — Payment Verification Queue

Features:

* Pending items
* age
* amount
* payer
* intended charge
* evidence
* provider status
* duplicate warning
* reviewer assignment

## 117. HC-CAP-065 — Payment Verification Decision

Actions:

* Verify
* reject
* request information
* mark duplicate
* escalate

Validation:

* Amount
* currency
* recipient
* payer
* date
* provider or bank reference
* organization
* duplicate state
* reviewer authority

## 118. HC-CAP-066 — Payment Allocation

Features:

* Allocate to charge
* partial allocation
* multi-charge allocation
* multi-payment settlement
* available balance
* outstanding balance
* transactional command
* concurrency protection

## 119. Allocation Acceptance

Given a verified payment with an available amount:

* Total allocations cannot exceed the payment.
* allocations cannot exceed charge balances.
* currencies must match.
* unverified or reversed payment cannot allocate.
* stale concurrent requests fail safely.

## 120. HC-CAP-067 — Receipt

Features:

* Unique identifier
* source payment
* allocations
* amount
* currency
* issue date
* payer
* purpose
* secure download
* immutable representation
* reversal state

## 121. HC-CAP-068 — Refund

Features:

* Request
* source payment
* eligible amount
* recipient
* reason
* approval
* execution
* provider reference
* reconciliation
* audit

## 122. HC-CAP-069 — Reversal

Features:

* Source payment
* explicit reversing record
* allocation correction
* balance correction
* reason
* authorization
* audit
* no deletion

# EPIC 12 — Agreements and Signatures

## 123. Epic Objective

Create, approve, issue, sign, activate, amend, and preserve agreements.

## 124. Epic Classification

```text
Priority: P1
Risk: Critical
Primary Release: R4
```

## 125. HC-CAP-070 — Agreement Template

Features:

* Template metadata
* organization
* version
* clause sections
* active status
* review
* approval
* jurisdiction context
* no autonomous legal authority

## 126. HC-CAP-071 — Agreement Draft

Data:

* Parties
* unit
* term
* rent
* deposit
* service charge
* start
* end
* conditions
* source offer
* draft version

## 127. HC-CAP-072 — Agreement Review

Features:

* Reviewer
* comments
* requested change
* approval
* rejection
* version comparison
* legal assignment

## 128. HC-CAP-073 — Agreement Issue

Issuance must:

* Freeze version
* assign issue identifier
* generate document
* calculate checksum
* identify signers
* create audit
* prevent ordinary edit

## 129. HC-CAP-074 — Signature Request

Features:

* Signer
* role
* order
* request
* expiry
* provider or fallback
* reminder
* decline
* cancel

## 130. HC-CAP-075 — Signature Completion

Validation:

* Provider authenticity
* agreement version
* signer
* request
* environment
* final document
* checksum
* evidence

## 131. HC-CAP-076 — Agreement Activation

Prerequisites:

* Approved agreement
* required signatures
* correct version
* required payment condition
* unit eligibility
* authorized actor
* no conflict

## 132. HC-CAP-077 — Amendment Foundation

Features:

* Source agreement
* proposed change
* new version
* approval
* signature where required
* effective date
* history

## 133. HC-CAP-078 — Termination Foundation

Features:

* Reason
* notice
* authority
* effective date
* related occupancy impact
* audit
* no silent deletion

# EPIC 13 — Occupancy and Move-In

## 134. Epic Objective

Activate and manage lawful, non-conflicting occupancy.

## 135. Epic Classification

```text
Priority: P1
Risk: Critical
Primary Release: R4
```

## 136. HC-CAP-079 — Planned Occupancy

Data:

* Occupancy ID
* organization
* unit
* agreement
* primary resident
* planned start
* planned end
* status
* blockers

## 137. HC-CAP-080 — Occupancy Conflict Detection

Checks:

* Existing active occupancy
* overlapping planned occupancy
* compatible state
* unit status
* agreement relationship
* concurrent activation

## 138. HC-CAP-081 — Occupancy Activation

Prerequisites:

* Active agreement
* required payment
* unit readiness
* no conflict
* valid resident
* authorized operator
* transaction
* audit

## 139. HC-CAP-082 — Move-In Checklist

Items may include:

* Unit readiness
* condition report
* keys
* access cards
* utility notes
* resident acknowledgment
* move-in time
* operator completion

## 140. HC-CAP-083 — Household

Features:

* Add household member
* relationship
* authority
* effective date
* minor or dependant minimization
* approval
* remove
* history

# EPIC 14 — Resident Portal

## 141. Epic Objective

Provide residents with transparent self-service access.

## 142. HC-CAP-084 — Resident Home

Display:

* Current occupancy
* unit
* agreement
* charges
* verified payments
* receipts
* maintenance
* notices
* support

## 143. HC-CAP-085 — Resident Financial View

Include:

* Charges
* outstanding balance
* payments
* verification status
* allocations
* receipts
* refunds
* no misleading settlement status

## 144. HC-CAP-086 — Resident Agreement View

Include:

* Current agreement
* signed version
* amendments
* dates
* secure download
* access audit where required

## 145. HC-CAP-087 — Resident Profile and Security

Features:

* Contact details
* password
* MFA where used
* sessions
* notification preferences
* household view
* support

# EPIC 15 — Maintenance and Vendor Operations

## 146. Epic Objective

Support resident maintenance requests and scoped vendor work.

## 147. Epic Classification

```text
Priority: P1
Risk: High
Primary Release: R4
```

## 148. HC-CAP-088 — Maintenance Request

Features:

* Category
* description
* priority
* media
* access notes
* occupancy relationship
* submission
* acknowledgment
* status

## 149. HC-CAP-089 — Triage

Features:

* Review
* priority adjustment
* emergency handling
* duplicate detection
* assignment
* internal notes

## 150. HC-CAP-090 — Work Order

Data:

* Work order ID
* maintenance request
* scope
* assigned coordinator
* vendor
* schedule
* status
* quote
* approval
* completion evidence

## 151. HC-CAP-091 — Vendor Assignment

Controls:

* Assigned work only
* minimal resident data
* approved access instructions
* start
* expiry
* revoke
* audit

## 152. HC-CAP-092 — Completion and Verification

Features:

* Vendor completion
* evidence
* resident or operator verification
* reopen
* closure
* cost record
* audit

# EPIC 16 — Renewals and Move-Out

## 153. Epic Objective

Support end-of-term renewal and controlled move-out.

## 154. HC-CAP-093 — Renewal Case

Features:

* Current agreement
* proposed term
* proposed annual rent
* proposed charges
* review
* offer
* accept
* decline
* expire
* new agreement

## 155. HC-CAP-094 — Move-Out Request

Features:

* Notice
* requested date
* reason
* acknowledgment
* approval where required
* schedule
* status

## 156. HC-CAP-095 — Move-Out Checklist

Items:

* Condition report
* keys
* access items
* outstanding charges
* maintenance findings
* deposit assessment
* final date

## 157. HC-CAP-096 — Deposit Decision

Features:

* Original deposit
* deductions
* evidence
* approval
* refund amount
* dispute
* audit

## 158. HC-CAP-097 — Occupancy End

Effects:

* Occupancy ends
* unit transitions
* agreement status updates appropriately
* resident access changes
* history preserved
* unit readiness begins

# EPIC 17 — Documents and File Security

## 159. Epic Objective

Provide secure upload, scanning, storage, access, versioning, and retention foundations.

## 160. Epic Classification

```text
Priority: P0–P1
Risk: Critical
Primary Release: R1–R4
```

## 161. HC-CAP-098 — Upload Intent

Tasks:

* Authorize upload
* document purpose
* resource relationship
* file restrictions
* storage path
* expiry
* upload authorization

## 162. HC-CAP-099 — Upload Finalization

Validation:

* Upload exists
* metadata valid
* checksum
* content type
* size
* organization
* resource
* authorization

## 163. HC-CAP-100 — Malware Scan

States:

* Pending
* passed
* failed
* timed out
* rejected
* manual review

## 164. HC-CAP-101 — Document Metadata

Data:

* Document ID
* organization
* resource
* type
* classification
* version
* scan status
* review status
* storage key
* checksum
* uploader
* created time

## 165. HC-CAP-102 — Secure Document Access

Requirements:

* Authorization
* resource relationship
* short-lived signed URL
* safe disposition
* access audit
* optional step-up authentication
* no public default

## 166. HC-CAP-103 — Document Versioning

Features:

* Replacement
* previous version
* current version
* immutable signed documents
* version history
* archive foundation

# EPIC 18 — Communications and Notifications

## 167. Epic Objective

Deliver reliable, minimal, secure transactional communications.

## 168. HC-CAP-104 — Notification Record

Data:

* Notification ID
* recipient
* organization
* channel
* template
* related event
* status
* attempt
* delivery result
* correlation ID

## 169. HC-CAP-105 — In-App Notifications

Features:

* Inbox
* unread state
* secure destination
* event summary
* mark read
* archive
* organization context

## 170. HC-CAP-106 — Email Delivery

Features:

* Queue
* template
* recipient validation
* provider adapter
* status
* retry
* bounce
* suppression
* safe links

## 171. HC-CAP-107 — Notification Preferences

Features:

* Channel preference
* event category
* mandatory security notices
* transactional versus marketing distinction
* organization policy

## 172. HC-CAP-108 — Template Management Foundation

Features:

* Template identifier
* version
* subject
* body
* variables
* preview
* approval
* safe rendering
* no unrestricted restricted-data insertion

# EPIC 19 — Search, Reporting, and Dashboards

## 173. Epic Objective

Provide permission-aware operational visibility without creating a parallel source of truth.

## 174. HC-CAP-109 — Organization Search

Searchable resources:

* Property
* unit
* application
* payment
* agreement
* occupancy
* maintenance
* user
* business identifier

## 175. Search Controls

* Organization scoped
* permission scoped
* field scoped
* assignment scoped
* safe suggestions
* safe no-result behavior
* no restricted metadata leakage

## 176. HC-CAP-110 — Role Dashboards

Initial dashboards:

* Organization Owner
* Property Operator
* Estate Agent
* Finance Reviewer
* Maintenance Coordinator
* Resident
* Platform Support

## 177. HC-CAP-111 — Operational Reports

Reports:

* Unit availability
* application pipeline
* payment verification
* outstanding charges
* unallocated verified payments
* agreements awaiting signature
* active occupancies
* maintenance backlog
* expiring agreements

## 178. HC-CAP-112 — Secure Export Foundation

Requirements:

* Permission
* field selection
* organization scope
* step-up authentication where required
* background generation
* expiry
* secure download
* audit

# EPIC 20 — Audit, Support, and Platform Administration

## 179. Epic Objective

Provide protected platform governance, traceability, and support operations.

## 180. HC-CAP-113 — Audit Event Service

Audit fields:

* Event ID
* actor
* actor type
* organization
* action
* resource
* timestamp
* outcome
* reason
* correlation ID
* source
* metadata

## 181. Audit Event Requirements

* Append oriented
* restricted write path
* protected read
* no secret content
* material event coverage
* retention foundation

## 182. HC-CAP-114 — Support Case

Features:

* Case reference
* requester
* organization
* category
* priority
* status
* assignment
* timeline
* correlation IDs
* resolution

## 183. HC-CAP-115 — Support Triage

Classifications:

* Guidance
* defect
* incident
* security
* privacy
* provider
* finance
* legal
* operations

## 184. HC-CAP-116 — Support Session

Features:

* Case relationship
* scope
* approval
* start
* expiry
* revoke
* access logging
* no unrestricted default

## 185. HC-CAP-117 — Platform Organization Administration

Features:

* Create organization
* suspend organization
* restore organization
* view safe metadata
* onboarding status
* no unrestricted customer-content access

## 186. HC-CAP-118 — Platform Security Administration

Features:

* User suspension
* session revocation
* privileged-role review
* security-event review
* provider credential metadata
* emergency access controls

# EPIC 21 — Integrations and Provider Framework

## 187. Epic Objective

Create secure, provider-neutral integration foundations.

## 188. HC-CAP-119 — Integration Registry

Data:

* Provider
* capability
* environment
* owner
* status
* credentials reference
* endpoints
* webhook
* monitoring
* ADR

## 189. HC-CAP-120 — Provider Adapter Framework

Requirements:

* Provider-neutral port
* adapter implementation
* normalized errors
* timeout
* retry classification
* safe logs
* metrics
* test simulator

## 190. HC-CAP-121 — Webhook Receiver

Features:

* Raw body
* payload limit
* signature validation
* timestamp validation
* account validation
* event persistence
* immediate acknowledgment
* asynchronous processing

## 191. HC-CAP-122 — Integration Event Store

Features:

* Provider event ID
* uniqueness
* environment
* provider account
* status
* payload reference
* mapping
* processing attempt
* result
* audit

## 192. HC-CAP-123 — Reconciliation Foundation

Features:

* Provider records
* internal records
* mismatch
* review
* resolution
* closure
* monitoring

# EPIC 22 — Security Hardening

## 193. Epic Objective

Implement and verify the approved security architecture.

## 194. Security Work Packages

* Authentication hardening
* authorization hardening
* tenant-isolation review
* CSRF
* XSS
* CSP
* secure headers
* rate limiting
* secret scanning
* dependency scanning
* file security
* webhook security
* audit protection
* production access
* support-session security
* incident preparation

## 195. HC-CAP-124 — Security Headers

Implement:

* Strict Transport Security
* Content Security Policy
* content-type protection
* frame restriction
* referrer policy
* permissions policy
* cache policy

## 196. HC-CAP-125 — Rate Limiting

Apply to:

* Registration
* login
* recovery
* verification resend
* public inquiry
* inspection request
* application submission
* upload intent
* webhook endpoint
* expensive search
* AI requests

## 197. HC-CAP-126 — Secret Scanning

Requirements:

* Local or pre-commit support
* CI scanning
* repository-history awareness
* incident response
* rotation process
* documented exceptions

## 198. HC-CAP-127 — Security Review

Before production:

* Threat model traceability
* dependency review
* configuration review
* authorization review
* tenant-isolation review
* file review
* provider review
* penetration test or approved alternative
* remediation record

# EPIC 23 — Quality Engineering

## 199. Epic Objective

Create a reliable automated and manual quality system.

## 200. HC-CAP-128 — Unit and Domain Tests

Mandatory areas:

* Authorization
* hierarchy rules
* applications
* charges
* payments
* allocation
* agreements
* occupancy
* maintenance
* unknown values

## 201. HC-CAP-129 — Integration Tests

Mandatory areas:

* Database
* API
* authorization
* audit
* storage
* queue
* provider adapters
* cache invalidation

## 202. HC-CAP-130 — End-to-End Tests

Critical journeys:

* Prospect to inquiry
* applicant to submission
* application decision
* payment verification
* agreement signature
* occupancy activation
* maintenance request
* support session

## 203. HC-CAP-131 — Accessibility Tests

Tasks:

* Automated scans
* keyboard review
* screen-reader review
* focus
* forms
* dialogs
* mobile
* critical journey audit

## 204. HC-CAP-132 — Performance Tests

Targets:

* Ordinary API latency
* page load
* search
* uploads
* queue
* concurrent allocation
* report generation

## 205. HC-CAP-133 — Security Tests

Required:

* Cross-tenant
* privilege escalation
* IDOR
* file upload
* webhook replay
* session revocation
* support-session expiry
* AI prompt injection where applicable

# EPIC 24 — Infrastructure and Deployment

## 206. Epic Objective

Provide repeatable, isolated, secure environments and deployments.

## 207. HC-CAP-134 — Environment Provisioning

Environments:

* Local
* development
* test
* integration
* staging
* production
* preview

## 208. HC-CAP-135 — Database Infrastructure

Tasks:

* Managed PostgreSQL
* TLS
* connection pooling
* roles
* backups
* monitoring
* migration access
* least privilege

## 209. HC-CAP-136 — Object Storage

Tasks:

* Private bucket
* environment separation
* upload policy
* signed URLs
* lifecycle
* versioning decision
* monitoring

## 210. HC-CAP-137 — Queue and Worker Infrastructure

Tasks:

* Queue provider
* worker runtime
* retries
* dead letter
* scheduling
* metrics
* correlation propagation

## 211. HC-CAP-138 — CI Pipeline

Stages:

* Install
* format
* lint
* type check
* unit tests
* integration tests
* security scans
* build
* preview

## 212. HC-CAP-139 — CD Pipeline

Stages:

* Main build
* staging deployment
* migrations
* staging smoke
* production approval
* production deployment
* health checks
* production smoke
* release record

## 213. HC-CAP-140 — Backup and Restore

Tasks:

* Automated backup
* retention
* encryption
* monitoring
* restoration procedure
* restore exercise
* evidence

# EPIC 25 — Observability and Operations

## 214. Epic Objective

Ensure production behavior can be detected, understood, supported, and recovered.

## 215. HC-CAP-141 — Structured Logging

Fields:

* Level
* event
* environment
* version
* correlation
* organization where safe
* resource
* outcome
* error code
* duration

## 216. HC-CAP-142 — Metrics

Required:

* Request count
* errors
* latency
* database
* queue
* storage
* provider
* authentication
* authorization
* payment workflow
* agreement workflow
* occupancy workflow

## 217. HC-CAP-143 — Tracing

Propagation:

* Request
* database
* queue
* worker
* provider
* audit metadata

## 218. HC-CAP-144 — Alerts

Required alerts:

* Application outage
* high error rate
* database saturation
* queue backlog
* job failure
* provider outage
* webhook rejection surge
* backup failure
* cross-tenant signal
* financial-integrity signal

## 219. HC-CAP-145 — Runbooks

Initial runbooks:

* Application outage
* database outage
* queue backlog
* provider outage
* failed deployment
* failed migration
* backup failure
* restore
* secret compromise
* cross-tenant incident
* financial-integrity incident

## 220. HC-CAP-146 — Incident Workflow

Features:

* Declare
* severity
* assign roles
* timeline
* updates
* containment
* recovery
* closure
* post-incident review

# EPIC 26 — Pilot Data and Onboarding

## 221. Epic Objective

Prepare accurate pilot data, users, training, and controlled operations.

## 222. HC-CAP-147 — Pilot Data Inventory

Collect and classify:

* Confirmed property data
* pending property data
* unit records
* annual-rent values
* service charges
* deposits
* images
* documents
* organization users
* operational roles

## 223. HC-CAP-148 — Pilot Data Validation

Validation must confirm:

* Block A has six units.
* Block B count remains pending.
* second-building structure remains pending.
* water feature remains unclassified until confirmed.
* public names remain pending until approved.
* no fabricated floors or units.

## 224. HC-CAP-149 — Pilot User Provisioning

Tasks:

* User list
* role mapping
* least privilege
* invitations
* MFA
* training
* support channel
* acknowledgment

## 225. HC-CAP-150 — Pilot Training

Training modules:

* Authentication
* organization context
* property setup
* listings
* applications
* payment verification
* agreements
* occupancy
* maintenance
* documents
* support
* incident reporting

## 226. HC-CAP-151 — Pilot Manual Fallbacks

Document fallbacks for:

* Payment verification
* signature
* communication
* maintenance assignment
* provider outage
* reconciliation

Fallback data must later reconcile into Habitacores.

# EPIC 27 — Production Readiness

## 227. Epic Objective

Validate that Habitacores can safely become the authoritative system for the approved pilot scope.

## 228. HC-CAP-152 — Production Readiness Review

Checklist:

* Architecture
* security
* quality
* accessibility
* performance
* infrastructure
* backup
* restore
* providers
* support
* incidents
* training
* data
* go-live

## 229. HC-CAP-153 — Launch Data Migration

Tasks:

* Source inventory
* mapping
* validation
* dry run
* reconciliation
* approval
* production import
* post-import validation
* audit

## 230. HC-CAP-154 — Release Candidate Validation

Evidence:

* Source commit
* test reports
* security report
* accessibility report
* performance report
* migration report
* smoke test
* known issues
* rollback or recovery plan

## 231. HC-CAP-155 — Go-Live

Tasks:

* Production deployment
* production migration
* health check
* smoke test
* provider check
* user confirmation
* monitoring watch
* support activation
* launch communication

## 232. HC-CAP-156 — Hypercare

Initial post-launch focus:

* Daily operational review
* support triage
* error review
* payment review
* agreement review
* occupancy review
* provider review
* defect prioritization
* pilot feedback

# EPIC 28 — Multi-Organization Readiness

## 233. Epic Objective

Remove founding-organization dependencies and support repeatable customer onboarding.

## 234. HC-CAP-157 — Organization Onboarding

Features:

* Organization profile
* owner setup
* policy configuration
* property import
* role templates
* agreement templates
* payment configuration
* support orientation

## 235. HC-CAP-158 — Organization Configuration

Potential configuration:

* Currency
* operational time zone
* rent policy
* application requirements
* approval thresholds
* document requirements
* communication templates
* maintenance priorities

## 236. HC-CAP-159 — Second Organization Validation

Acceptance:

* New organization onboarded without schema change.
* no founding data is visible.
* configuration differs safely.
* support sessions remain scoped.
* reports remain isolated.
* provider mappings remain correct.

# EPIC 29 — AI-Assisted Capabilities

## 237. Epic Objective

Introduce controlled AI assistance only after foundational security and workflow maturity.

## 238. Epic Classification

```text
Priority: P2
Risk: High
Primary Release: R4–R6
```

## 239. HC-CAP-160 — AI Gateway

Requirements:

* Approved provider
* server-side credential
* model configuration
* organization scope
* data minimization
* redaction
* prompt templates
* timeout
* cost tracking
* structured output
* audit metadata

## 240. HC-CAP-161 — Application Summary

Requirements:

* Source application
* permitted fields
* advisory label
* source links
* human review
* no automatic decision
* prompt-injection tests

## 241. HC-CAP-162 — Communication Draft

Requirements:

* Approved context
* human edit
* human approval
* no automatic send
* privacy validation
* safe tone
* audit metadata

## 242. HC-CAP-163 — Maintenance Classification Suggestion

Requirements:

* Suggested category
* suggested priority
* explanation
* operator review
* no autonomous emergency decision
* no vendor assignment

## 243. AI Exit Criteria

* No cross-tenant retrieval.
* no protected action from AI output.
* provider failure leaves manual workflow available.
* output is labeled.
* human rejection and edit are supported.
* prompt injection tests pass.

# EPIC 30 — Commercialization Foundation

## 244. Epic Objective

Prepare the product for repeatable commercial adoption after MVP validation.

## 245. HC-CAP-164 — Subscription Model Foundation

Potential data:

* Plan
* organization
* status
* start
* renewal
* unit allowance
* feature entitlement
* billing reference

No pricing is approved by this document.

## 246. HC-CAP-165 — Feature Entitlements

Entitlements may control:

* Module access
* usage limits
* provider access
* AI access
* advanced reporting
* support tier

Entitlements must not replace authorization.

## 247. HC-CAP-166 — Usage Measurement

Potential usage:

* Active units
* active occupancies
* organization users
* storage
* communications
* AI use
* transactions

## 248. Delivery Sequence

Recommended sequence:

```text
Stage 1
Engineering Foundation

Stage 2
Identity, Organizations, and Authorization

Stage 3
Property Hierarchy and Design System

Stage 4
Documents, Audit, Infrastructure, and Observability Foundations

Stage 5
Marketplace, Inquiries, and Inspections

Stage 6
Applications, Decisions, and Offers

Stage 7
Charges, Payments, Verification, and Allocation

Stage 8
Agreements and Signatures

Stage 9
Occupancy, Resident Portal, and Move-In

Stage 10
Maintenance and Vendors

Stage 11
Renewal and Move-Out

Stage 12
Pilot Data, Training, Production Readiness, and Launch
```

## 249. Vertical Slice 1 — Secure Organization Shell

Includes:

* User login
* organization
* membership
* role
* organization switch
* protected dashboard
* authorization
* audit
* logs
* tests

Exit:

* A user can access only an authorized organization.

## 250. Vertical Slice 2 — Property Hierarchy

Includes:

* Property
* community
* building
* block
* floor
* unit
* create and view UI
* API
* authorization
* audit
* tests

Exit:

* Pilot hierarchy can be represented accurately.

## 251. Vertical Slice 3 — Public Listing to Inquiry

Includes:

* Unit availability
* listing
* publication
* public page
* inquiry
* internal queue
* notification
* tests

Exit:

* A public prospect can discover an approved listing and create an inquiry.

## 252. Vertical Slice 4 — Application Submission

Includes:

* Applicant account
* draft
* documents
* submission
* review queue
* notification
* audit
* mobile tests

Exit:

* A controlled applicant can submit a complete application.

## 253. Vertical Slice 5 — Application Decision and Offer

Includes:

* Review
* information request
* recommendation
* decision
* offer
* acceptance
* audit
* notifications

Exit:

* An approved applicant can accept a valid offer.

## 254. Vertical Slice 6 — Charge and Payment Evidence

Includes:

* Charge
* payment record
* evidence upload
* scan
* verification queue
* audit
* resident or applicant status view

Exit:

* Evidence is submitted but charge remains outstanding until verification.

## 255. Vertical Slice 7 — Payment Verification and Allocation

Includes:

* Verification decision
* allocation
* concurrency controls
* balance
* receipt
* audit
* financial tests

Exit:

* Verified payment can settle approved charges accurately.

## 256. Vertical Slice 8 — Agreement and Signature

Includes:

* Template
* draft
* issue
* immutable version
* signature
* evidence
* activation prerequisites
* audit

Exit:

* A correct agreement version can be signed and activated safely.

## 257. Vertical Slice 9 — Occupancy and Resident Portal

Includes:

* Planned occupancy
* conflict check
* activation
* move-in
* resident portal
* agreement
* charges
* receipts
* audit

Exit:

* Approved resident can access an active occupancy.

## 258. Vertical Slice 10 — Maintenance

Includes:

* Resident request
* triage
* work order
* vendor assignment
* completion
* verification
* closure

Exit:

* A scoped vendor can complete assigned work without unrelated access.

## 259. Critical Dependency Map

```text
Authentication
→ Organization membership
→ Authorization
→ Resource access

Property hierarchy
→ Unit
→ Listing
→ Application
→ Offer
→ Charge
→ Agreement
→ Occupancy

Payment record
→ Verification
→ Allocation
→ Receipt
→ Agreement prerequisite
→ Occupancy prerequisite

Agreement issue
→ Signature
→ Agreement activation
→ Occupancy activation

Occupancy
→ Resident portal
→ Maintenance
→ Renewal
→ Move-out
```

## 260. ADR Dependency Gate

Implementation should not begin on provider-dependent or architecture-dependent items until the relevant ADR is approved.

Initial ADRs should include:

1. Application repository and workspace structure
2. Next.js application architecture
3. PostgreSQL access and ORM or query strategy
4. Authentication provider
5. authorization implementation model
6. row-level security decision
7. identifier strategy
8. money representation
9. audit-event architecture
10. object-storage provider
11. queue and background-job provider
12. application-hosting provider
13. email provider
14. payment workflow and provider
15. signature workflow and provider
16. observability stack
17. infrastructure-as-code approach
18. feature-flag approach
19. AI provider and gateway, when needed

## 261. Codex Work Package Standard

Every Codex implementation request should contain:

* Work-package identifier
* objective
* approved source documents
* exact scope
* explicit exclusions
* repository path
* allowed files
* data model
* API contract
* authorization rules
* business rules
* UI requirements
* test requirements
* audit requirements
* observability requirements
* migration requirements
* acceptance criteria
* validation commands

## 262. Codex Work Package Example

```markdown
# HC-WP-003 — Create Organization Membership Model

## Objective

Implement the organization membership persistence model and domain rules.

## Sources

- Document 04 — Domain Model
- Document 06 — Business Rules
- Document 10 — Data Architecture
- Document 11 — Permission Matrix
- Document 16 — Security Architecture
- Document 22 — Implementation Backlog

## Scope

- Membership schema
- membership statuses
- foreign keys
- unique rules
- domain validation
- repository methods
- unit tests
- database integration tests

## Exclusions

- Invitation email
- UI
- custom roles
- organization billing

## Rules

- Membership belongs to one user and one organization.
- Suspended membership does not authorize access.
- Unknown values must not be invented.
- Cross-tenant access must be denied.

## Validation

- Type check
- unit tests
- integration tests
- migration test
```

## 263. Codex Guardrails

Codex must not:

* Invent product policy.
* create new roles without approval.
* weaken authorization.
* hardcode the founding organization.
* invent Block B inventory.
* invent second-building inventory.
* map unknown values to zero.
* treat payment evidence as payment verification.
* edit issued or signed agreements.
* activate occupancy without prerequisites.
* expose restricted documents publicly.
* add provider dependencies without ADR approval.
* send external communication without approved workflow.
* introduce microservices without approval.
* bypass tests.
* modify unrelated files unnecessarily.

## 264. Pull Request Scope

A pull request should normally:

* Address one coherent work package.
* remain reviewable.
* include tests.
* include migrations where needed.
* include documentation.
* identify risk.
* identify rollback or recovery.
* avoid unrelated refactoring.

## 265. Commit Strategy

Commits should be:

* Focused
* descriptive
* logically ordered
* free of secrets
* free of unrelated generated artifacts

Suggested prefixes:

* `feat:`
* `fix:`
* `docs:`
* `test:`
* `refactor:`
* `chore:`
* `security:`
* `infra:`

## 266. Pull Request Template Requirements

Each implementation PR should include:

* Purpose
* linked work item
* source documents
* included scope
* excluded scope
* architecture impact
* data impact
* authorization impact
* security impact
* tests
* migrations
* screenshots where applicable
* operational impact
* rollback or recovery
* checklist

## 267. Backlog Refinement

Backlog refinement should:

* Clarify scope
* split large items
* confirm dependencies
* resolve contradictions
* identify ADR need
* identify design need
* identify security review
* identify provider dependency
* confirm acceptance criteria
* update priority

## 268. Sprint or Iteration Planning

The implementation plan does not mandate a fixed sprint duration.

Each iteration should select work that:

* Produces a coherent vertical increment.
* fits available capacity.
* has resolved prerequisites.
* includes quality work.
* does not overload review.
* advances the release objective.

## 269. Work in Progress Limits

The team should limit simultaneous work to reduce:

* Integration conflicts
* review delay
* unfinished migration chains
* architecture drift
* testing backlog
* operational debt

## 270. Defect Policy

Defects should be linked to:

* Affected epic
* affected capability
* severity
* release
* test gap
* root cause
* correction
* regression test

## 271. Critical Defect Policy

A critical defect blocks release when it affects:

* Tenant isolation
* unauthorized protected action
* financial integrity
* agreement integrity
* occupancy integrity
* restricted documents
* production data
* critical availability
* secret exposure

## 272. Technical Debt Backlog

Technical debt records should include:

* Description
* reason accepted
* risk
* affected module
* owner
* target release
* remediation
* verification

## 273. Documentation Backlog

Implementation documentation should include:

* README
* module overview
* local setup
* environment variables
* migrations
* provider setup
* test commands
* deployment
* runbooks
* troubleshooting
* ADR links

## 274. Security Backlog

Security backlog categories:

* Identity
* authorization
* tenancy
* storage
* provider
* secrets
* logging
* monitoring
* vulnerability
* incident response
* penetration findings
* privacy

## 275. Operational Backlog

Operational backlog categories:

* Alerts
* dashboards
* runbooks
* support procedures
* recovery exercises
* provider contacts
* status communication
* capacity
* cost
* reliability

## 276. Pilot Backlog

Pilot backlog categories:

* Data confirmation
* configuration
* users
* roles
* training
* support
* communication
* manual fallbacks
* feedback
* measurement
* migration

## 277. Backlog Traceability

Every P0 and P1 capability should trace to:

```text
Approved Requirement
→ Business Rule
→ Domain Entity
→ Permission
→ API
→ UI
→ Test
→ Release
→ Acceptance Evidence
```

## 278. Traceability Example — Payment Allocation

```text
Requirement:
Allocate verified payments to charges.

Business Rules:
Payment must be verified.
Currency must match.
Allocation cannot exceed payment or charge balance.

Domain:
Payment
Charge
Allocation

Permission:
finance.payment.allocate

API:
POST /payments/{paymentId}/allocations

UI:
Payment Allocation Workspace

Tests:
Authorization
Tenant isolation
Currency mismatch
Over-allocation
Concurrency
Idempotency

Release:
R4

Evidence:
Passing financial integration suite
```

## 279. Delivery Metrics

Potential delivery metrics:

* Work items completed
* lead time
* cycle time
* pull-request age
* review time
* deployment frequency
* failed deployment rate
* escaped defects
* reopened defects
* flaky tests
* critical-rule coverage
* technical-debt age

## 280. Outcome Metrics

Delivery output must be balanced with outcomes such as:

* Application completion
* payment verification time
* agreement completion
* occupancy activation
* maintenance resolution
* support demand
* manual fallback frequency
* operator adoption

## 281. MVP Implementation Exit Criteria

Implementation readiness is complete when:

* Documents 00 through 22 are approved.
* initial ADR backlog is approved.
* application repository exists.
* work-package template exists.
* first vertical slice is defined.
* unresolved decisions are assigned.
* implementation responsibilities are identified.
* development environment is ready.
* quality gates are ready.
* Codex guardrails are documented.

## 282. R1 Exit Criteria

Engineering Foundation is complete when:

* Application builds.
* database works.
* migrations work.
* authentication works.
* organization tenancy works.
* authorization framework works.
* audit framework works.
* storage foundation works.
* jobs foundation works.
* CI works.
* staging deployment works.
* logs and correlation work.
* critical security tests pass.

## 283. R2 Exit Criteria

Internal Alpha is complete when:

* Founding organization exists.
* roles and memberships work.
* pilot hierarchy is represented accurately.
* Block A has six units.
* unknown facts remain pending.
* internal users can manage inventory.
* documents are private.
* dashboards provide basic visibility.
* synthetic second organization remains isolated.

## 284. R3 Exit Criteria

Founding Organization Alpha is complete when:

* Public listings work.
* inquiries work.
* inspections work.
* applications work.
* review works.
* decisions work.
* offers work.
* mobile and low-bandwidth acceptance pass.
* application privacy passes.

## 285. R4 Exit Criteria

Controlled Pilot Beta is complete when:

* Charges work.
* payment evidence works.
* verification works.
* allocation works.
* receipts work.
* agreements work.
* signatures work.
* occupancy works.
* resident portal works.
* maintenance works.
* support works.
* end-to-end controlled lifecycle passes.

## 286. R5 Exit Criteria

Production MVP is complete when:

* Production readiness review passes.
* no critical defect remains.
* backup and restore pass.
* monitoring and alerts are active.
* pilot users are trained.
* support ownership exists.
* provider integrations are approved.
* security review is complete.
* founding organization accepts the system.

## 287. Deferred Implementation Scope

Deferred items include:

* Native mobile applications
* advanced autonomous AI
* automated tenant scoring
* government registry integration
* accounting synchronization
* open public partner API
* customer webhooks
* multi-region architecture
* microservices
* Kubernetes
* advanced event streaming
* customer-managed encryption keys
* biometric verification
* predictive maintenance
* advanced fraud engine
* national rental index
* full vendor marketplace
* mortgage processing
* property sales

## 288. Implementation Risks

### 288.1 Oversized Work Packages

Risk:

Tasks become difficult to review and test.

Controls:

* Vertical decomposition
* XS-to-L target
* work-package template
* dependency mapping

### 288.2 Architecture Drift

Risk:

Implementation contradicts approved documents.

Controls:

* Source-document references
* ADR review
* architecture review
* traceability
* pull-request checklist

### 288.3 AI Coding Overreach

Risk:

Codex invents missing decisions or edits unrelated code.

Controls:

* Explicit scope
* exclusions
* allowed files
* validation commands
* small work packages
* human review

### 288.4 Pilot Hardcoding

Risk:

Founding property details become permanent assumptions.

Controls:

* Synthetic second tenant
* configurable hierarchy
* neutral IDs
* pending-value tests
* review

### 288.5 Financial Defects

Risk:

Incorrect balances or allocation damage trust.

Controls:

* Exact money
* transactions
* locks
* tests
* audit
* reconciliation

### 288.6 Provider Delay

Risk:

Provider selection blocks delivery.

Controls:

* Adapter interfaces
* simulators
* manual fallback
* provider ADRs
* delayed activation

### 288.7 Testing Debt

Risk:

Features accumulate without adequate verification.

Controls:

* Tests in Definition of Done
* required CI
* release gates
* no deferred critical testing

### 288.8 Operational Debt

Risk:

Features ship without alerts, logs, or support readiness.

Controls:

* Observability requirements
* runbooks
* production-readiness checklist
* operational acceptance

### 288.9 Scope Expansion

Risk:

Post-MVP ideas delay the pilot.

Controls:

* Priority classes
* exclusions
* change governance
* roadmap parking lot

### 288.10 Unresolved Policy

Risk:

Engineering implements assumptions before business decisions.

Controls:

* Pending-decision register
* ADR or policy gate
* nullable values
* blocked work status
* Product Visionary review

## 289. Unresolved Implementation Decisions

The following remain pending:

* Final application repository name
* final monorepo or single-application structure
* final package manager
* final Node.js version
* final Next.js version
* final UI component approach
* final CSS approach
* final database provider
* final ORM or query builder
* final migration framework
* final identifier format
* final authentication provider
* final MFA method
* final session strategy
* final row-level-security strategy
* final authorization library or internal approach
* final object-storage provider
* final file-scanning provider
* final queue provider
* final job framework
* final application-hosting provider
* final email provider
* final payment provider
* final signature provider
* final observability stack
* final error-tracking provider
* final infrastructure-as-code tool
* final secret-management platform
* final feature-flag provider
* final test frameworks
* final browser support matrix
* final mobile device matrix
* final release cadence
* final iteration cadence
* final issue-tracking platform
* final estimation method
* final staffing model
* final Codex operating model
* final initial ADR order
* final first vertical slice
* final pilot launch date
* final production launch date
* final Block A unit labels
* final Block B inventory
* final second-building structure
* final water-feature classification
* final pilot public names
* final annual-rent charge values
* final service-charge values
* final deposit policy
* final application requirements
* final guarantor policy
* final approval thresholds
* final support hours
* final pilot training schedule

These decisions must be resolved before the affected work becomes implementation dependent.

## 290. Backlog Change Governance

A material backlog change requires:

1. Change description
2. reason
3. affected release
4. affected epic
5. user impact
6. business impact
7. architecture impact
8. data impact
9. authorization impact
10. tenant-isolation impact
11. security impact
12. financial impact
13. agreement impact
14. occupancy impact
15. accessibility impact
16. infrastructure impact
17. integration impact
18. operational impact
19. testing impact
20. dependency impact
21. delivery impact
22. scope displaced
23. migration impact
24. rollback or fallback
25. Product Visionary approval
26. Chief Product Architect approval
27. security review where required
28. effective release

## 291. Backlog Review Triggers

A dedicated backlog review is required when:

* MVP scope changes
* release order changes
* a critical provider changes
* a major architecture decision changes
* pilot data changes materially
* a new regulated capability is introduced
* financial policy changes
* agreement policy changes
* occupancy policy changes
* security incident exposes a gap
* staffing or funding changes
* a major dependency is delayed
* a work item expands beyond its original boundary

## 292. Current Implementation Decision

Habitacores will use a dependency-aware, risk-first, vertical-slice delivery model organized as:

```text
Approved Product Knowledge Base
→ Architecture Decision Records
→ Prioritized Epics
→ Capabilities
→ Bounded Work Packages
→ Implementation
→ Automated Verification
→ Human Review
→ Staging Validation
→ Release Gate
→ Controlled Pilot
→ Production Evidence
```

The initial implementation sequence is:

```text
Engineering Foundation
→ Identity and Authentication
→ Organizations and Authorization
→ Property Hierarchy
→ Design System and Application Shell
→ Documents and Audit
→ Public Marketplace
→ Inquiries and Inspections
→ Applications and Decisions
→ Charges and Payments
→ Agreements and Signatures
→ Occupancy and Resident Portal
→ Maintenance and Vendors
→ Renewals and Move-Out
→ Pilot Preparation
→ Production Readiness
```

The delivery model must preserve:

* Product traceability
* organization isolation
* server-side authorization
* permanent identifiers
* accurate property hierarchy
* unknown-value integrity
* financial correctness
* payment-verification distinction
* transactional allocation
* agreement immutability
* signature integrity
* occupancy prerequisites
* resident privacy
* vendor scope
* secure documents
* provider isolation
* AI boundaries
* accessibility
* mobile usability
* low-bandwidth resilience
* testing
* observability
* recoverability
* auditability
* operational support
* future SaaS extensibility

No work item, sprint objective, coding-agent suggestion, provider feature, developer convenience, hidden spreadsheet, pilot urgency, verbal request, AI-generated implementation, successful compilation, passing happy-path test, or founding-team relationship may independently justify bypassing an approved Habitacores business rule, authorization control, tenant boundary, financial control, agreement control, occupancy prerequisite, security requirement, test gate, architecture decision, or production-readiness requirement.