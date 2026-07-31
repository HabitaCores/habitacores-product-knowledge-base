# Document 21 — MVP Scope, Release Plan, and Product Roadmap

## Document Control

| Field | Value |
|---|---|
| Product | Habitacores |
| Document | MVP Scope, Release Plan, and Product Roadmap |
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
| Delivery Model | Incremental, pilot-led, evidence-based, and risk-controlled |
| Initial Product Architecture | Modular monolith |
| Proposed Application Framework | Next.js and TypeScript |
| Proposed System of Record | PostgreSQL |
| Last Updated | 30 July 2026 |

## 1. Purpose

This document defines the Minimum Viable Product scope, release strategy, pilot plan, phased product roadmap, delivery boundaries, release criteria, and post-MVP expansion model for Habitacores.

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

into an executable product-delivery plan.

This document defines:

- MVP objectives
- MVP success definition
- MVP scope
- MVP exclusions
- pilot boundaries
- target users
- target organizations
- core workflows
- release phases
- release sequencing
- dependency sequencing
- risk sequencing
- feature priorities
- acceptance gates
- pilot onboarding
- production launch
- roadmap horizons
- commercialization readiness
- post-MVP expansion
- roadmap governance

This document does not replace:

- The implementation backlog
- sprint plans
- engineering estimates
- staffing plans
- detailed release runbooks
- commercial contracts
- legal launch approval
- provider-selection ADRs
- production deployment procedures

Those artifacts will be developed separately.

## 2. Product Delivery Objective

Habitacores must first prove that a controlled real-estate organization can use one secure digital system to manage the complete rental lifecycle from property setup through resident occupancy and operational support.

The first release must demonstrate that Habitacores can:

1. Represent a real Nigerian property structure accurately.
2. support multiple user roles.
3. preserve organization-level data isolation.
4. publish and manage available residential units.
5. receive inquiries and inspection requests.
6. accept structured applications.
7. support controlled application review.
8. create annual-rent charges.
9. receive and verify payment evidence.
10. allocate verified payments correctly.
11. generate and manage agreements.
12. preserve signed agreement versions.
13. activate occupancy only when prerequisites are satisfied.
14. provide a resident portal.
15. receive and manage maintenance requests.
16. preserve documents, financial records, and audit history.
17. operate reliably on mobile and constrained networks.
18. support internal operations without relying on spreadsheets as the authoritative source.
19. produce evidence that the product is ready for additional organizations.
20. avoid inventing unconfirmed pilot-property facts.

## 3. MVP Definition

The Habitacores MVP is the smallest secure, usable, operationally supportable, and commercially testable version of the product that can manage a real annual-rent residential lifecycle for the founding organization and its pilot property.

The MVP is not:

- A visual demonstration only
- a static listing website
- a landlord accounting spreadsheet
- a payment screenshot repository
- a generic property marketplace
- a complete national property registry
- a legal-advice platform
- a fully autonomous AI system
- a full enterprise resource-planning platform
- a finished multi-country product

## 4. MVP Product Thesis

The MVP will test the following thesis:

```text
A structured, transparent, role-based digital rental workflow can reduce ambiguity, fragmented records, payment disputes, agreement uncertainty, and operational confusion for Nigerian residential rentals.
````

## 5. MVP Business Thesis

The MVP will test whether:

* Property owners and operators value a unified operating system.
* tenants value transparent charges, agreements, receipts, and records.
* agents can work within controlled assignments.
* finance reviewers can distinguish payment evidence from verified payment.
* lawyers can participate without receiving unrestricted platform access.
* annual-rent workflows can be represented accurately.
* organizations can onboard without creating a custom software implementation for each property.
* the founding pilot can generate credible evidence for external customer acquisition.

## 6. MVP Primary Users

The MVP should support:

### Public Users

* Public Prospect
* Returning Prospect

### Personal Portal Users

* Applicant
* Guarantor where required
* Primary Resident
* Co-Resident with limited authority

### Organization Users

* Organization Owner
* Organization Administrator
* Property Operator
* Estate Agent
* Finance Reviewer
* Maintenance Coordinator

### Professional Users

* Estate Lawyer with scoped assignment
* Vendor Worker with scoped work-order access

### Platform Users

* Platform Administrator
* Platform Support Agent with controlled support-session access

## 7. MVP Primary Organization

The founding organization will be the first operational tenant.

The product must nevertheless be implemented as a true multi-tenant SaaS system.

The founding organization must not receive:

* Hardcoded authorization
* hardcoded database rules
* special untracked financial treatment
* unrestricted support access
* code-level property assumptions
* direct production database dependency
* permanent pilot-only schemas

## 8. MVP Pilot Property

The pilot property structure currently includes:

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

Confirmed facts:

* Block A contains six residential units.
* Block B exists.
* A functional masjid exists within the relevant community perimeter.
* Another building is under construction opposite or across the same street within the wider community or estate.
* A water feature exists beside or near the second building.
* Annual rent is the operating policy.

Unconfirmed facts must remain pending, including:

* Block B unit count
* exact floor structure
* exact unit labels
* second-building inventory
* water-feature classification
* final public names
* final building names
* final community name

## 9. MVP Geographic Boundary

Initial operational focus:

* Nigeria
* Lagos
* Southwest Nigeria

The architecture must remain extensible to other Nigerian states.

The MVP does not require:

* Multi-country tax handling
* multiple legal jurisdictions
* foreign-currency settlement
* multilingual national localization
* international property marketplace coverage

## 10. MVP Currency Boundary

The MVP primary currency is:

```text
NGN — Nigerian naira
```

The data model may remain currency aware, but the MVP does not require active multi-currency settlement.

## 11. MVP Rental Model

The MVP must support annual residential rent as the primary rent model.

It may represent:

* Annual rent
* security deposit
* service charge
* approved one-time fees
* maintenance-related charges
* adjustments
* waivers
* refunds

It does not initially require:

* Daily rentals
* hourly rentals
* hotel reservations
* short-stay booking
* mortgage origination
* property-sale closing
* fractional property ownership

## 12. MVP Capability Groups

The MVP is divided into:

1. Platform foundation
2. identity and access
3. organization administration
4. property and inventory
5. public marketplace
6. inquiries and inspections
7. applications
8. decision and offer
9. financial operations
10. agreements
11. occupancy
12. resident operations
13. maintenance
14. documents
15. communications
16. audit and reporting
17. support and operations
18. security and reliability

## 13. Platform Foundation Scope

The MVP must include:

* Responsive web application
* secure API
* PostgreSQL database
* private object storage
* organization-level tenancy
* modular domain architecture
* background jobs
* audit events
* environment separation
* automated CI
* controlled deployment
* logs, metrics, and alerts
* backup and restore foundation

## 14. Identity and Authentication Scope

The MVP must include:

* User registration
* secure login
* logout
* email verification
* password reset
* session management
* session revocation
* account suspension
* privileged-user MFA
* basic session and device visibility
* authentication audit events

Passwordless authentication may be introduced later through an ADR.

## 15. Organization Administration Scope

The MVP must include:

* Organization creation by authorized platform process
* organization profile
* organization status
* membership invitations
* membership activation
* membership suspension
* role assignment
* role revocation
* permission enforcement
* organization switching for eligible users
* scoped professional assignments
* organization audit history

## 16. MVP Organization Roles

Initial organization roles should include:

* Organization Owner
* Organization Administrator
* Property Operator
* Estate Agent
* Finance Reviewer
* Maintenance Coordinator

Initial professional roles should include:

* Estate Lawyer
* Vendor Worker

Platform roles should remain separate.

## 17. Property Hierarchy Scope

The MVP must support:

```text
Organization
→ Property
→ Community
→ Building
→ Block
→ Floor
→ Unit
```

Every level should:

* Use a permanent internal identifier
* allow editable display names
* support status
* support parent-child validation
* preserve unknown optional attributes
* remain organization scoped

## 18. Property Setup Scope

The MVP must include:

* Create and edit property
* create community
* create building
* create block
* create floor where applicable
* create unit
* record amenities
* record operational status
* record development status
* assign internal managers
* attach documents
* preserve audit history

## 19. Unit Scope

A unit should support:

* Internal identifier
* display label
* property hierarchy
* unit type
* bedroom count where known
* bathroom count where known
* status
* readiness
* annual rent
* service charge where applicable
* deposit where applicable
* amenities
* approved media
* current occupancy relationship
* future availability

## 20. Unit Statuses

The MVP should distinguish statuses such as:

* Draft
* unavailable
* available
* reserved
* occupied
* maintenance hold
* administrative hold
* under construction
* retired

Status behavior must be governed by business rules.

## 21. Public Marketplace Scope

The MVP should include:

* Public property and unit listings
* listing search
* basic filters
* listing details
* approved images
* annual rent display
* service-charge display where applicable
* amenity display
* general location
* availability status
* inquiry action
* inspection-request action
* mobile-friendly presentation

## 22. Public Listing Privacy

Public listings must not expose:

* Resident details
* exact restricted documents
* internal notes
* payment records
* agreement records
* unrestricted exact coordinates
* internal security information
* private organization configuration

## 23. Listing Administration Scope

The MVP must support:

* Listing draft
* listing review
* listing publication
* listing unpublication
* listing expiry
* listing update
* media management
* approved public fields
* audit history

## 24. Inquiry Scope

The MVP must include:

* Public inquiry submission
* contact details
* listing relationship
* inquiry category
* message
* acknowledgment
* assigned organization user
* status
* internal notes
* communication history
* conversion to inspection or application

## 25. Inspection Scope

The MVP must include:

* Inspection request
* proposed date and time
* assigned agent
* confirmation
* rescheduling
* cancellation
* completion
* outcome
* internal notes
* applicant or prospect communication

Full calendar-provider synchronization may be deferred.

## 26. Application Scope

The MVP must include:

* Application creation
* draft saving
* applicant profile
* intended unit
* household information
* employment or income declarations where required
* guarantor information where required
* supporting documents
* declarations
* submission
* information request
* applicant response
* withdrawal
* expiry
* review status
* decision history

## 27. Application Privacy Scope

Application records must be restricted by:

* Organization membership
* role
* permission
* assignment
* field-level access
* current workflow state

Vendor Workers must not access applications.

## 28. Application Review Scope

The MVP must support:

* Review queue
* assigned reviewer
* completeness check
* document review
* information request
* internal recommendation
* decision preparation
* approval or rejection
* reason capture
* audit
* applicant notification

AI may assist with summaries later in the MVP or pilot, but must remain advisory.

## 29. Application Decision Scope

The MVP must support:

* Approved
* approved with conditions
* rejected
* withdrawn
* expired

Decision authority must be explicit.

Decision reasons must be protected and professionally worded.

## 30. Offer Scope

The MVP should support:

* Offer creation
* selected unit
* annual rent
* deposit
* service charge
* other approved charges
* offer terms
* approval
* issue
* acceptance
* decline
* expiry
* replacement
* audit

## 31. Charge Scope

The MVP must support:

* Annual-rent charge
* security-deposit charge
* service-charge entry
* approved fee
* maintenance-related charge
* adjustment
* waiver
* refund relationship
* charge balance
* due date
* status
* audit

## 32. Payment Scope

The MVP must support:

* Payment record
* payer
* organization
* amount
* currency
* payment date
* payment method
* provider reference where available
* bank reference where available
* payment evidence
* verification status
* reviewer
* verification result
* rejection reason
* available balance
* audit

## 33. Payment Evidence Boundary

The MVP must preserve the distinction:

```text
Payment evidence submitted
≠ Payment verified
≠ Charge settled
≠ Agreement active
≠ Occupancy active
```

## 34. Payment Verification Scope

The MVP must include:

* Verification queue
* evidence review
* provider or bank-reference review where available
* duplicate warning
* amount validation
* currency validation
* payer and recipient validation
* approve
* reject
* request information
* reviewer identity
* audit

## 35. Payment Allocation Scope

The MVP must support:

* Full allocation
* partial allocation
* one payment to multiple charges
* multiple payments to one charge
* available payment balance
* outstanding charge balance
* transactional allocation
* concurrency protection
* audit

## 36. Receipt Scope

The MVP must support:

* Generated receipt
* unique receipt identifier
* payment reference
* allocation reference
* amount
* currency
* payer
* charge purpose
* issue date
* organization
* secure download
* immutable history
* reversal representation

## 37. Refund and Reversal Scope

The MVP should include a controlled foundation for:

* Refund request
* review
* approval
* execution status
* provider reference where applicable
* reconciliation
* payment reversal
* allocation correction
* audit

Highly automated refund processing may be deferred until provider and control maturity exist.

## 38. Agreement Scope

The MVP must support:

* Agreement template
* agreement draft
* selected parties
* selected unit
* rent terms
* deposit terms
* service-charge terms
* start and end dates
* document generation
* review
* approval
* issue
* signature
* activation
* amendment foundation
* termination foundation
* version history

## 39. Agreement Version Scope

The MVP must ensure:

* Draft versions are editable.
* issued versions are immutable.
* signed versions are immutable.
* each material change creates a new version or amendment.
* signature evidence binds to a specific version.
* old versions remain accessible to authorized users.

## 40. Signature Scope

The MVP should support either:

### Preferred

* Approved electronic-signature provider
* signer identity
* signature request
* signature callback
* signed document
* completion evidence

### Controlled Fallback

* Securely uploaded signed document
* authorized verification
* signer declaration
* document checksum
* audit

The fallback must not weaken version integrity.

## 41. Agreement Activation Scope

An agreement may activate only when:

* Required approval exists.
* required signatures exist.
* required payment conditions are satisfied.
* the correct version is used.
* the unit remains eligible.
* the actor is authorized.
* no conflicting state exists.

## 42. Occupancy Scope

The MVP must include:

* Planned occupancy
* selected unit
* agreement relationship
* primary resident
* co-residents or household members
* planned start
* actual start
* planned end
* status
* activation
* move-in
* renewal foundation
* move-out
* audit

## 43. Occupancy Activation Scope

Occupancy activation must require:

* Active eligible agreement
* required payment conditions
* unit availability
* no incompatible occupancy
* move-in readiness
* authorized operator
* transaction and audit

## 44. Move-In Scope

The MVP should support:

* Move-in date
* unit-readiness confirmation
* condition report
* access-item record
* resident acknowledgment
* completion
* audit

## 45. Household Scope

The MVP should support:

* Primary Resident
* Co-Resident
* dependant or household member record
* effective dates
* relationship
* access authority
* approval status
* privacy controls

The system must minimize information collected about minors.

## 46. Resident Portal Scope

The MVP resident portal must include:

* Current occupancy
* unit information
* agreement access
* charges
* verified payments
* allocations
* receipts
* maintenance requests
* documents
* notices
* support access
* profile and security settings

## 47. Renewal Scope

The MVP should include a controlled renewal foundation:

* Renewal case
* current agreement
* proposed annual rent
* proposed term
* review
* offer
* acceptance or decline
* new agreement version
* occupancy continuation

Advanced automated renewal campaigns may be deferred.

## 48. Move-Out Scope

The MVP should support:

* Move-out request
* notice record
* planned move-out
* condition report
* access-item return
* outstanding-charge review
* deposit decision
* refund relationship
* occupancy end
* unit-readiness transition
* audit

## 49. Maintenance Scope

The MVP must include:

* Maintenance request
* resident-submitted issue
* category
* description
* priority
* media
* access notes
* triage
* assignment
* work order
* vendor assignment
* scheduling
* status
* completion evidence
* verification
* closure
* reopen request

## 50. Maintenance Priority

The MVP should support:

* Emergency
* urgent
* normal
* low

Final response targets may be configured later.

## 51. Vendor Scope

Vendor Workers should be able to access only:

* Assigned work order
* approved location
* work scope
* authorized contact method
* approved access instructions
* scheduling information
* completion submission

They must not access:

* Applications
* unrelated resident information
* payment records
* agreements
* legal notes
* unrestricted organization data

## 52. Legal Workflow Scope

The MVP should include a controlled legal-workspace foundation:

* Matter creation
* assigned lawyer
* related applicant, resident, agreement, property, or occupancy
* restricted notes
* notices
* document attachments
* deadlines
* status
* closure
* audit

The MVP does not provide autonomous legal advice.

## 53. Document Management Scope

The MVP must include:

* Upload intent
* direct private upload
* file validation
* malware scan
* document type
* resource relationship
* version
* review status
* restricted access
* signed URL
* replacement
* archive foundation
* audit

## 54. Document Categories

Initial categories may include:

* Identification
* application evidence
* income evidence
* payment evidence
* agreement
* signed agreement
* receipt
* condition report
* maintenance evidence
* vendor document
* legal document
* property document

## 55. Communications Scope

The MVP must include:

* In-app notifications
* transactional email
* notification preferences
* queue
* delivery status
* retry
* secure links
* minimal sensitive content
* template management foundation

SMS or WhatsApp may be included only after provider approval.

## 56. Communication Events

Initial communication events may include:

* Email verification
* password reset
* inquiry acknowledgment
* inspection confirmation
* application submitted
* information requested
* application decision
* offer issued
* payment evidence received
* payment verified or rejected
* agreement issued
* signature reminder
* agreement activated
* move-in reminder
* maintenance update
* support update

## 57. Search Scope

The MVP should include organization-scoped search for:

* Properties
* units
* users
* applications
* payments
* agreements
* occupancies
* maintenance records
* business identifiers

Search must enforce the same permissions as direct access.

## 58. Reporting Scope

The MVP should include basic operational reports for:

* Unit availability
* active occupancies
* application pipeline
* payment-verification queue
* outstanding charges
* verified unallocated payments
* agreements awaiting signature
* maintenance backlog
* expiring agreements
* workflow-aging records

Reports must not replace authoritative transaction records.

## 59. Dashboard Scope

The MVP should provide role-specific dashboards.

Examples:

### Organization Owner

* Portfolio summary
* unit status
* occupancy
* financial workflow
* agreements
* maintenance
* risks

### Property Operator

* Available units
* move-ins
* move-outs
* blocked occupancies
* maintenance

### Finance Reviewer

* Payments pending verification
* duplicate warnings
* unallocated payments
* refund requests
* reconciliation issues

### Estate Agent

* Assigned inquiries
* inspections
* applications
* offers

### Resident

* Agreement
* charges
* receipts
* maintenance
* notices

## 60. Audit Scope

The MVP must record material events including:

* Authentication
* membership changes
* role changes
* assignment changes
* listing publication
* application submission
* application decision
* payment verification
* allocation
* refund approval
* agreement issue
* signature completion
* agreement activation
* occupancy activation
* restricted-document access
* support session
* data repair

## 61. Support Scope

The MVP must include:

* Support request
* case reference
* category
* priority
* status
* assignment
* communication
* escalation
* correlation identifier
* resolution
* controlled support session
* audit

## 62. Platform Administration Scope

The MVP platform administration must include:

* Organization creation
* organization suspension
* platform-user management
* support-case management
* controlled support access
* provider configuration metadata
* environment visibility
* security-event review
* audit access
* feature-flag foundation

Platform administrators must not automatically receive unrestricted customer-data access.

## 63. Security Scope

The MVP security baseline must include:

* TLS
* secure authentication
* MFA for privileged users
* session revocation
* server-side authorization
* organization isolation
* permission checks
* assignment checks
* field-level protection
* CSRF protection
* XSS protection
* secure headers
* input validation
* rate limiting
* private object storage
* signed URLs
* secret management
* audit
* vulnerability scanning
* backup protection

## 64. Reliability Scope

The MVP reliability baseline must include:

* Structured logs
* correlation IDs
* application metrics
* database metrics
* queue monitoring
* provider monitoring
* critical alerts
* health checks
* backup monitoring
* restore test
* incident process
* runbooks
* deployment monitoring

## 65. Accessibility Scope

The MVP must target WCAG 2.2 Level AA.

Critical journeys must support:

* Keyboard navigation
* visible focus
* labels
* error identification
* screen-reader compatibility
* sufficient contrast
* zoom and reflow
* accessible dialogs
* accessible forms
* mobile accessibility

## 66. Responsive Scope

The MVP must support:

* Small mobile
* large mobile
* tablet
* laptop
* desktop

Critical workflows must be fully usable on mobile.

## 67. Low-Bandwidth Scope

The MVP should include:

* Optimized images
* compressed responses
* server pagination
* draft preservation
* retry behavior
* upload recovery where practical
* clear pending states
* text-first critical screens
* no false success after interrupted requests

## 68. AI Scope

AI is optional within the MVP release boundary.

Approved initial AI use cases may include:

* Application summary
* document-orientation summary
* communication draft
* support knowledge assistance
* maintenance categorization suggestion

AI must remain:

* Advisory
* labeled
* human reviewed
* tenant scoped
* permission aware
* data minimized
* independently degradable

## 69. AI MVP Exclusions

The MVP must not allow AI to autonomously:

* Approve applications
* reject applications
* verify payments
* allocate payments
* approve refunds
* activate agreements
* activate occupancies
* issue legal decisions
* expose restricted information
* send material external communication without authorized review

## 70. MVP Integration Scope

Initial integrations should be limited to those necessary for safe operation.

Likely MVP integrations:

* Email provider
* file-scanning provider
* payment provider or controlled manual payment workflow
* electronic-signature provider or controlled fallback
* AI provider only where approved
* monitoring and error-tracking provider

## 71. Provider Selection Boundary

No provider becomes mandatory until:

* Due diligence is complete.
* sandbox testing is complete.
* security requirements are satisfied.
* privacy requirements are satisfied.
* commercial terms are understood.
* ADR is approved.
* fallback or failure behavior is defined.

## 72. MVP Exclusions

The MVP does not require:

* Native iOS application
* native Android application
* international multi-currency settlement
* property sales
* mortgage processing
* short-let reservations
* hotel booking
* insurance marketplace
* automated credit scoring
* biometric identity verification
* government property-registry integration
* court-system integration
* automated legal filing
* full accounting synchronization
* customer-configurable webhooks
* open public partner API
* advanced predictive analytics
* advanced AI agents
* multi-region active-active infrastructure
* microservices
* Kubernetes
* enterprise SSO
* SCIM
* dedicated tenant databases
* advanced data warehouse
* full vendor marketplace
* property valuation marketplace
* consumer social network

## 73. Scope-Control Principle

An idea should not enter the MVP merely because:

* It is technically possible.
* a competitor has it.
* a provider offers it.
* AI can generate it.
* it appears impressive in a demonstration.
* it may be useful later.

An MVP capability must support the core pilot objective or a mandatory safety, legal, operational, or architectural requirement.

## 74. MVP Prioritization Model

Capabilities should be prioritized using:

* User value
* business value
* operational necessity
* legal or compliance necessity
* security risk
* dependency value
* learning value
* implementation effort
* failure impact
* pilot relevance

## 75. Priority Classes

### P0 — Mandatory Foundation

Without this capability, the MVP cannot operate safely.

Examples:

* Authentication
* tenant isolation
* property hierarchy
* authorization
* audit
* private storage
* payment-verification distinction

### P1 — Core Pilot

Required to complete the rental lifecycle.

Examples:

* Listings
* applications
* charges
* payments
* agreements
* occupancy
* resident portal
* maintenance

### P2 — Pilot Enhancement

Improves efficiency but may have a manual fallback.

Examples:

* AI summaries
* advanced dashboards
* SMS
* WhatsApp
* automated reconciliation
* automated renewal reminders

### P3 — Post-MVP

Valuable after pilot proof.

Examples:

* Native mobile applications
* accounting synchronization
* public partner APIs
* government integrations

## 76. MVP Critical Path

The main delivery dependency path is:

```text
Platform Foundation
→ Identity and Organization Isolation
→ Property Hierarchy
→ Listings and Inquiries
→ Applications and Decisions
→ Charges and Payments
→ Agreements and Signatures
→ Occupancy and Resident Portal
→ Maintenance
→ Operational Readiness
→ Pilot Launch
```

## 77. Delivery Workstreams

The MVP may be delivered through the following workstreams:

1. Foundation and developer platform
2. identity, tenancy, and authorization
3. property and inventory
4. marketplace and prospect experience
5. application lifecycle
6. financial lifecycle
7. agreement lifecycle
8. occupancy and resident lifecycle
9. maintenance and vendor operations
10. documents and communications
11. security and quality
12. infrastructure and operations
13. pilot preparation and onboarding

## 78. Release Philosophy

Habitacores should release in small, reviewable increments.

Each release should:

* Preserve approved architecture
* remain deployable
* include tests
* include monitoring
* avoid hidden migration debt
* preserve backward compatibility where required
* provide rollback or forward recovery
* produce usable evidence

## 79. Release Stages

The roadmap uses the following release stages:

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

## 80. Release 0 — Architecture and Product Knowledge Base

### Objective

Create the approved source of truth required for implementation.

### Includes

* Documents 00 through 22
* initial ADRs
* glossary
* architecture boundaries
* security requirements
* testing strategy
* implementation backlog
* Codex instructions

### Exit Criteria

* Approved Product Knowledge Base
* unresolved decisions identified
* implementation dependencies mapped
* initial ADR sequence approved
* no major architectural contradiction

## 81. Release 1 — Engineering Foundation

### Objective

Create a secure, deployable application foundation.

### Includes

* Application repository
* Next.js and TypeScript foundation
* PostgreSQL connection
* schema and migration foundation
* authentication
* organization tenancy
* authorization framework
* audit framework
* object-storage foundation
* background-job foundation
* CI
* staging
* logs and error tracking
* design-system primitives

### Exit Criteria

* Automated build passes.
* environment isolation works.
* organization isolation tests pass.
* authentication works.
* privileged MFA works.
* deployment to staging works.
* migration pipeline works.
* logs and correlation IDs work.
* no critical security defect remains.

## 82. Release 2 — Internal Alpha

### Objective

Allow the founding team to configure the platform and validate core administrative workflows.

### Includes

* Organization profile
* membership
* roles
* assignments
* property hierarchy
* Block A six-unit representation
* Block B pending representation
* unit management
* listing draft
* document upload
* basic dashboards
* audit
* internal support tooling

### Exit Criteria

* Founding property can be represented accurately.
* unknown values remain null or pending.
* organization users can be assigned correctly.
* cross-tenant tests pass with at least two synthetic organizations.
* property and document workflows are usable on mobile.
* internal team accepts the hierarchy.

## 83. Release 3 — Founding Organization Alpha

### Objective

Operate the prospect-to-application lifecycle with controlled internal users.

### Includes

* Public listings
* search
* inquiries
* inspections
* applicant registration
* application draft and submission
* application documents
* review queue
* information requests
* decisions
* offers
* transactional email

### Exit Criteria

* A synthetic or controlled applicant can complete the journey.
* application privacy tests pass.
* mobile and low-bandwidth tests pass.
* all material actions are audited.
* no cross-tenant leakage exists.
* organization team can process applications without engineering intervention.

## 84. Release 4 — Controlled Pilot Beta

### Objective

Complete the financial, agreement, and occupancy lifecycle for selected real pilot cases.

### Includes

* Charges
* payment evidence
* payment verification
* payment allocation
* receipts
* agreement generation
* signature workflow
* agreement activation
* occupancy activation
* move-in
* resident portal
* maintenance
* vendor assignment
* support cases

### Exit Criteria

* At least one controlled end-to-end rental lifecycle completes.
* payment evidence never bypasses verification.
* allocations remain transactionally correct.
* signed agreement versions remain immutable.
* occupancy cannot activate with unmet prerequisites.
* resident portal works on mobile.
* support and incident procedures are ready.
* backup restore has been tested.

## 85. Release 5 — Production MVP

### Objective

Launch Habitacores as the authoritative operating system for the founding organization’s approved pilot scope.

### Includes

* Hardened R4 capabilities
* production monitoring
* production backups
* security review
* accessibility review
* performance validation
* operational reports
* production support
* release communication
* data-migration validation
* approved provider integrations

### Exit Criteria

* Product Visionary approval
* Chief Product Architect approval
* security readiness
* quality readiness
* infrastructure readiness
* operational readiness
* pilot-user training completed
* production support ownership assigned
* no unresolved critical defects
* no material high-risk defect without approved mitigation

## 86. Release 6 — Multi-Organization Readiness

### Objective

Prove that Habitacores can onboard additional organizations without custom architecture or direct engineering dependence.

### Includes

* Organization onboarding workflow
* reusable configuration
* organization-specific templates
* policy configuration
* enhanced subscription foundation
* organization usage reporting
* tenant-safe support operations
* scalable permissions
* onboarding documentation
* data-import foundation

### Exit Criteria

* A second organization can be onboarded.
* no founding-organization hardcoding remains.
* organization-specific configuration works.
* billing model is defined.
* data isolation remains verified.
* onboarding effort is measured and repeatable.

## 87. Release 7 — Commercial Expansion

### Objective

Expand Habitacores beyond the controlled pilot.

Potential capabilities:

* Broader marketplace
* additional Nigerian regions
* SMS and WhatsApp
* stronger identity verification
* advanced reconciliation
* accounting integration
* partner APIs
* native mobile applications
* advanced analytics
* organization subscription management
* configurable legal and operational templates

## 88. Pilot Cohort Strategy

The initial pilot should remain controlled.

Potential participants:

* Founding organization owners
* selected administrators
* selected property operators
* selected agents
* selected finance reviewers
* selected lawyer
* selected vendors
* selected applicants
* selected residents

Pilot access should not be opened broadly before operational readiness.

## 89. Pilot Entry Criteria

A workflow may enter real pilot use only when:

* Requirements are approved.
* implementation is complete.
* tests pass.
* authorization is verified.
* tenant isolation is verified.
* audit exists.
* monitoring exists.
* support guidance exists.
* rollback or manual fallback exists.
* pilot users are trained.

## 90. Pilot Data Strategy

Pilot data must distinguish:

* Verified real data
* unconfirmed real data
* synthetic test data
* migrated historical data
* demonstration data

The interface should not present demonstration data as real inventory or financial history.

## 91. Pilot User Training

Training should cover:

* Login and security
* organization context
* role boundaries
* property records
* listing workflow
* application workflow
* payment verification
* agreement workflow
* occupancy
* maintenance
* documents
* support
* incident reporting

## 92. Pilot Support Model

The pilot should use:

* Defined support channel
* case references
* priority classification
* operating hours
* escalation contacts
* support-session controls
* issue tracking
* weekly review

## 93. Pilot Feedback Model

Feedback should be classified as:

* Defect
* usability issue
* workflow gap
* training issue
* configuration request
* new feature
* security concern
* policy decision
* provider issue

Not every request should become immediate scope.

## 94. Pilot Measurement

The pilot should measure:

* Listing setup time
* inquiry response time
* application completion rate
* application review time
* payment-verification time
* agreement completion time
* occupancy activation time
* maintenance response time
* support-case volume
* user satisfaction
* defect rate
* manual workaround frequency

## 95. MVP Success Metrics

Potential MVP success measures include:

### Operational

* Pilot inventory represented accurately
* core workflows completed without spreadsheet authority
* no unresolved cross-tenant incident
* no material financial corruption
* no signed-agreement mutation
* successful backup and restore

### User

* Applicants can submit on mobile
* residents can retrieve agreements and receipts
* operators can understand workflow status
* finance reviewers can process payments
* vendors see only assigned work

### Business

* Founding organization adopts Habitacores for approved pilot operations
* at least one complete lifecycle is processed
* external organization interest is validated
* onboarding requirements become measurable
* commercial packaging can be defined

## 96. MVP Failure Conditions

The MVP should not be considered successful when:

* Users still depend on hidden spreadsheets as the authoritative source.
* payment evidence is treated as verified payment.
* organization isolation depends on user behavior.
* agreements can be edited after signature.
* occupancy can activate without prerequisites.
* support personnel receive unrestricted access.
* unknown property facts are invented.
* critical workflows fail on mobile.
* backups have not been restored successfully.
* production issues cannot be investigated.

## 97. Release Acceptance Layers

Every material release should pass:

1. Product acceptance
2. architecture acceptance
3. functional acceptance
4. security acceptance
5. tenant-isolation acceptance
6. financial-integrity acceptance
7. agreement-integrity acceptance
8. occupancy-integrity acceptance
9. accessibility acceptance
10. performance acceptance
11. operational acceptance
12. deployment acceptance

## 98. Release Candidate

A release candidate must identify:

* Version
* source commit
* included scope
* excluded scope
* migrations
* feature flags
* known defects
* provider dependencies
* test evidence
* security findings
* operational readiness
* rollback or forward-recovery plan

## 99. Go-Live Decision

The go-live decision should include:

* Product Visionary
* Chief Product Architect
* engineering owner
* security owner
* operations owner
* pilot organization representative

The final authority and sign-off structure remain pending.

## 100. No-Go Conditions

A release must not proceed when:

* A critical defect remains unresolved.
* cross-tenant access is possible.
* payment allocation can exceed verified funds.
* signed agreements can be changed.
* occupancy conflicts can occur.
* production secrets are exposed.
* backup restoration has not been validated.
* required monitoring is absent.
* critical accessibility blockers remain.
* provider callbacks can bypass domain rules.
* migrations are unsafe.
* production support is not assigned.

## 101. Rollout Strategy

Production rollout may use:

* Internal users first
* selected pilot users
* selected units
* selected workflows
* feature flags
* staged organization access
* controlled provider activation

## 102. Pilot Rollback Strategy

Pilot rollback may involve:

* Disable affected feature
* return workflow to approved manual process
* preserve submitted records
* suspend provider processing
* restore prior application version
* execute forward data correction
* notify affected users
* document incident

Rollback must not erase authoritative history.

## 103. Manual Fallbacks

Approved manual fallbacks may exist for:

* Payment verification
* signature collection
* communication
* maintenance assignment
* support investigation
* reconciliation

Manual fallback records must still enter Habitacores or be reconciled into it.

## 104. Product Roadmap Horizon

The roadmap is divided into:

```text
Horizon 0 — Architecture Readiness
Horizon 1 — MVP Foundation
Horizon 2 — Founding Pilot
Horizon 3 — Multi-Organization SaaS
Horizon 4 — Nigerian Market Expansion
Horizon 5 — Ecosystem and Intelligence
```

## 105. Horizon 0 — Architecture Readiness

Focus:

* Product Knowledge Base
* architecture decisions
* implementation backlog
* engineering standards
* provider evaluation
* development environment
* delivery governance

Exit:

* Codex and engineers can implement without inventing major product decisions.

## 106. Horizon 1 — MVP Foundation

Focus:

* Identity
* tenancy
* authorization
* property hierarchy
* database
* storage
* audit
* design system
* infrastructure
* testing

## 107. Horizon 2 — Founding Pilot

Focus:

* Listings
* inquiries
* applications
* payments
* agreements
* occupancy
* maintenance
* resident portal
* operations

## 108. Horizon 3 — Multi-Organization SaaS

Focus:

* Reusable onboarding
* tenant configuration
* subscriptions
* organization templates
* usage controls
* import
* customer support
* stronger analytics

## 109. Horizon 4 — Nigerian Market Expansion

Potential focus:

* Additional states
* broader agent network
* WhatsApp and SMS
* identity verification
* virtual accounts
* advanced financial reconciliation
* configurable legal templates
* partner integrations
* marketplace growth

## 110. Horizon 5 — Ecosystem and Intelligence

Potential focus:

* Native mobile applications
* advanced AI assistance
* predictive maintenance
* fraud risk signals
* tenant credibility with strong governance
* property-performance analytics
* partner marketplace
* accounting integrations
* government or registry integrations where available
* national rental-data infrastructure

## 111. Tenant Credibility Roadmap

A future tenant-credibility capability requires separate governance.

It must not become:

* An unchallengeable blacklist
* an opaque AI score
* a discriminatory screening mechanism
* a collection of unverified allegations
* a permanent record without correction rights

Future design must include:

* Verified sources
* purpose limitation
* dispute process
* correction
* retention
* consent or lawful basis
* transparency
* human review
* anti-discrimination controls

## 112. Marketplace Roadmap

The marketplace may evolve from:

```text
Founding property listings
→ selected organization listings
→ verified operator marketplace
→ broader regional marketplace
→ national property discovery
```

Expansion should preserve listing accuracy and operator accountability.

## 113. Mobile Roadmap

The responsive web application is the MVP primary experience.

Native mobile applications may follow when evidence demonstrates need for:

* Push notifications
* offline capability
* device camera workflows
* biometric login
* app-store distribution
* deeper mobile engagement

## 114. AI Roadmap

AI adoption should progress through:

```text
Drafting and summaries
→ Classification assistance
→ Search and retrieval assistance
→ Anomaly orientation
→ Operational recommendations
→ Advanced controlled agents
```

High-impact decisions must remain governed.

## 115. Financial Roadmap

Financial capability may evolve through:

* Manual evidence review
* provider transaction confirmation
* virtual accounts
* automated reconciliation
* organization settlement reporting
* accounting synchronization
* advanced fraud detection
* controlled financial analytics

## 116. Agreement Roadmap

Agreement capability may evolve through:

* Controlled templates
* versioned documents
* electronic signature
* amendments
* renewals
* configurable clauses
* jurisdiction-specific templates
* legal-system integrations where viable

## 117. Maintenance Roadmap

Maintenance capability may evolve through:

* Requests
* work orders
* vendor assignment
* quotes
* service-level tracking
* preventive maintenance
* asset maintenance
* vendor marketplace
* predictive maintenance

## 118. Reporting Roadmap

Reporting may evolve through:

* Operational dashboards
* downloadable reports
* scheduled reports
* organization analytics
* portfolio analytics
* financial reconciliation
* benchmarking
* predictive insights

## 119. Commercial Readiness

Before commercial expansion, Habitacores should define:

* Target customer segments
* pricing model
* subscription tiers
* implementation fees
* payment-provider fees
* support tiers
* contract terms
* service expectations
* data-processing terms
* onboarding process
* customer-success process

## 120. Potential Customer Segments

Potential segments include:

* Individual landlords with multiple units
* property-owning families
* estate managers
* property-management companies
* real-estate developers
* housing estates
* institutional property owners
* faith-based residential communities
* student-housing operators
* cooperative housing organizations

The MVP should not optimize prematurely for every segment.

## 121. Potential Commercial Models

Potential models include:

* Monthly organization subscription
* annual organization subscription
* per-unit subscription
* per-active-occupancy fee
* transaction fee
* setup or onboarding fee
* tiered feature plan
* enterprise contract

No pricing model is approved by this document.

## 122. Product Packaging Roadmap

Potential packages may include:

### Core

* Properties
* listings
* applications
* agreements
* occupancy
* maintenance

### Financial Operations

* Charges
* payments
* allocations
* receipts
* reconciliation

### Professional Operations

* Agent assignments
* legal assignments
* vendor workflows

### Intelligence

* AI assistance
* analytics
* anomaly detection

Packaging remains pending market validation.

## 123. Roadmap Dependency Rules

A later capability must not proceed when its foundation is incomplete.

Examples:

* Advanced AI cannot precede reliable authorization.
* Automated reconciliation cannot precede correct payment records.
* tenant credibility cannot precede dispute and correction governance.
* public APIs cannot precede partner authorization.
* native mobile cannot bypass secure API design.
* multi-region deployment cannot precede tested single-region recovery.

## 124. Technical-Debt Policy

Technical debt may be accepted only when:

* It is documented.
* business benefit is clear.
* security is not compromised.
* tenant isolation is not compromised.
* financial integrity is not compromised.
* remediation owner exists.
* target resolution exists.

## 125. Roadmap Evidence

Roadmap changes should use evidence from:

* Pilot usage
* user interviews
* support cases
* defect patterns
* workflow completion
* operational cost
* provider reliability
* security findings
* commercial demand
* implementation effort

## 126. Roadmap Review Cadence

The roadmap should be reviewed:

* After each major release
* after major pilot findings
* after material incident
* after provider change
* before commercial expansion
* when legal or market assumptions change

## 127. Release Documentation

Each release should include:

* Release version
* date
* purpose
* included features
* excluded features
* migrations
* resolved defects
* known issues
* operational changes
* security changes
* provider changes
* support guidance
* rollback or recovery guidance

## 128. MVP Acceptance Criterion — Property Accuracy

```text
Given:
The founding pilot property is configured.

When:
Authorized users inspect the hierarchy.

Then:
Block A contains exactly six confirmed residential units.

And:
Block B unit count remains null or pending.

And:
The second-building inventory remains unconfirmed.

And:
No placeholder units or floors are generated.
```

## 129. MVP Acceptance Criterion — Cross-Tenant Isolation

```text
Given:
Organization A and Organization B exist.

When:
A user from Organization A attempts to access Organization B property, application, payment, agreement, occupancy, document, or maintenance records.

Then:
Access is denied.

And:
No Organization B metadata is returned.

And:
The event is logged where appropriate.
```

## 130. MVP Acceptance Criterion — Payment Evidence

```text
Given:
An applicant uploads payment evidence.

When:
The upload and malware scan complete.

Then:
The payment remains pending verification.

And:
The related charge remains outstanding.

And:
No agreement or occupancy activates.
```

## 131. MVP Acceptance Criterion — Allocation Integrity

```text
Given:
A verified payment has ₦500,000 available.

When:
Allocation requests exceed the available balance.

Then:
The system prevents total allocation above ₦500,000.

And:
Balances remain transactionally consistent.

And:
The outcome is audited.
```

## 132. MVP Acceptance Criterion — Agreement Integrity

```text
Given:
Agreement Version 1 is issued and signed.

When:
A user attempts to edit Version 1.

Then:
The operation is rejected.

And:
Version 1 remains unchanged.

And:
A new version or amendment is required.
```

## 133. MVP Acceptance Criterion — Occupancy Activation

```text
Given:
An agreement is signed but required payment conditions remain unmet.

When:
An operator attempts to activate occupancy.

Then:
Activation is denied.

And:
The unit does not become occupied.

And:
The blocker is displayed clearly.
```

## 134. MVP Acceptance Criterion — Vendor Scope

```text
Given:
Vendor Worker A is assigned only to Work Order 001.

When:
Vendor Worker A requests another work order, application, agreement, or payment.

Then:
Access is denied.

And:
No unrelated resident or financial data is returned.
```

## 135. MVP Acceptance Criterion — Mobile Applicant

```text
Given:
An applicant uses a supported mobile device and a constrained connection.

When:
The applicant creates, saves, resumes, completes, and submits an application.

Then:
The draft is preserved.

And:
Uploads provide clear status.

And:
Submission does not duplicate.

And:
The applicant receives confirmation.
```

## 136. MVP Acceptance Criterion — Provider Failure

```text
Given:
A payment, signature, email, scanning, or AI provider is unavailable.

When:
A dependent operation is attempted.

Then:
The operation remains pending or fails clearly.

And:
No protected business status is falsely completed.

And:
Manual or retry procedures remain available where approved.
```

## 137. MVP Acceptance Criterion — Support Access

```text
Given:
A support agent has no active approved support session.

When:
The agent requests customer records.

Then:
Access is denied.

When:
A scoped support session is approved.

Then:
Only the approved scope is available until expiry.

And:
All access is audited.
```

## 138. Production MVP Exit Criteria

Production MVP approval requires:

### Product

* Approved user journeys complete
* founding organization acceptance
* pilot training complete
* known scope clearly communicated

### Architecture

* No major contradiction with approved documents
* modular boundaries preserved
* provider adapters preserved
* data ownership clear

### Security

* Tenant-isolation tests pass
* privileged MFA operational
* secrets protected
* file access protected
* webhook authentication operational
* security review complete

### Quality

* Critical tests pass
* no unresolved critical defect
* accessibility review complete
* mobile testing complete
* low-bandwidth testing complete

### Financial

* Payment verification distinction proven
* allocation concurrency proven
* receipt integrity proven
* refund and reversal controls reviewed

### Agreements

* Version immutability proven
* signature binding proven
* activation prerequisites proven

### Occupancy

* Conflict prevention proven
* activation prerequisites proven
* resident access proven

### Operations

* Logs, metrics, alerts, and runbooks exist
* backup and restore tested
* support process active
* incident process active
* production owner assigned

## 139. Deferred Roadmap Capabilities

Deferred capabilities include:

* Native mobile applications
* advanced AI agents
* automated tenant scoring
* government registry integration
* multi-country operation
* property sales
* mortgage processing
* insurance distribution
* automated legal filing
* advanced accounting synchronization
* open partner marketplace
* customer-defined webhooks
* multi-region active-active hosting
* complex microservices
* dedicated data warehouse
* predictive maintenance
* advanced fraud engines
* public developer portal
* real-time national rental index

Deferred scope must remain documented and must not quietly enter the MVP.

## 140. Roadmap Risks

### 140.1 Scope Expansion

Risk:

Too many features delay pilot completion.

Controls:

* MVP boundaries
* priority classes
* change approval
* manual fallback
* roadmap parking lot

### 140.2 Architecture Without Delivery

Risk:

Documentation continues without implementation.

Controls:

* Complete Documents 21 and 22
* approve initial ADRs
* begin engineering foundation
* tie new documentation to implementation need

### 140.3 Premature Coding

Risk:

Implementation begins before critical decisions are resolved.

Controls:

* ADR gates
* unresolved-decision tracking
* implementation readiness review
* vertical-slice planning

### 140.4 Pilot Hardcoding

Risk:

The founding property becomes embedded permanently.

Controls:

* Multi-organization tests
* configurable hierarchy
* neutral IDs
* unknown-value handling
* second synthetic organization

### 140.5 Operational Overload

Risk:

The founding team cannot operate the product consistently.

Controls:

* Role clarity
* training
* dashboards
* support
* phased rollout
* limited pilot cohort

### 140.6 Provider Dependency

Risk:

A provider delays the release.

Controls:

* Adapter boundaries
* controlled manual fallback
* provider evaluation early
* pending states
* phased activation

### 140.7 Financial Error

Risk:

Incorrect payment or allocation state damages trust.

Controls:

* Verification separation
* transactions
* concurrency tests
* manual review
* audit
* reconciliation

### 140.8 Legal Overreach

Risk:

The platform presents legal content as professional legal advice.

Controls:

* Lawyer review
* scoped legal workflows
* disclaimers
* version control
* human approval

### 140.9 Weak Adoption

Risk:

Users return to WhatsApp, paper, or spreadsheets.

Controls:

* Usability
* training
* mobile design
* workflow value
* leadership adoption
* transition plan

### 140.10 Roadmap Driven by Novelty

Risk:

AI or marketplace ideas displace core operations.

Controls:

* Evidence-based prioritization
* MVP success metrics
* dependency rules
* Product Visionary approval

## 141. Unresolved MVP and Roadmap Decisions

The following remain pending:

* Final public pilot community name
* final building names
* final Block A unit labels
* final Block B unit count
* final floor structure
* final second-building structure
* final water-feature classification
* final pilot-user cohort
* final number of real pilot applications
* final pilot launch date
* final production launch date
* final payment provider
* final signature provider
* final email provider
* final file-scanning provider
* final AI provider
* final manual payment-verification procedure
* final electronic-signature fallback
* final refund execution process
* final application eligibility rules
* final guarantor requirements
* final approval thresholds
* final annual-rent charge structure
* final service-charge policy
* final deposit policy
* final move-in checklist
* final move-out checklist
* final maintenance priorities
* final support hours
* final support channel
* final pilot training plan
* final release sign-off authority
* final pricing model
* final commercial customer segment
* final subscription tiers
* final onboarding fees
* final commercial launch geography
* final roadmap review cadence
* final engineering staffing model
* final delivery cadence
* final MVP implementation estimate
* final Codex implementation workflow
* final ADR sequence
* final application repository structure

These decisions must be resolved before the affected capability becomes production dependent.

## 142. MVP Scope Change Governance

A proposed MVP scope change must include:

1. Change description
2. user problem
3. business value
4. affected user
5. affected workflow
6. priority class
7. architectural impact
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
20. delivery impact
21. schedule impact
22. deferred work displaced
23. rollback or fallback
24. Product Visionary approval
25. Chief Product Architect approval
26. security review where required
27. effective release

## 143. Roadmap Review Triggers

A dedicated roadmap review is required when:

* Pilot objectives change
* target customer changes
* commercial model changes
* new regulated capability is proposed
* new financial role is introduced
* new geography is introduced
* new provider becomes critical
* new AI decision use case is proposed
* material pilot feedback changes the product thesis
* a major incident exposes architectural weakness
* release schedule changes materially
* funding or staffing changes materially

## 144. Current MVP and Roadmap Decision

Habitacores will use a pilot-led, risk-controlled, incremental release strategy organized as:

```text
Approved Product Knowledge Base
→ Engineering Foundation
→ Internal Alpha
→ Founding Organization Alpha
→ Controlled Pilot Beta
→ Production MVP
→ Multi-Organization Readiness
→ Commercial Expansion
```

The core MVP lifecycle is:

```text
Property and Unit
→ Listing
→ Inquiry and Inspection
→ Application
→ Review and Decision
→ Offer
→ Charges
→ Payment Evidence
→ Payment Verification
→ Payment Allocation
→ Agreement
→ Signature
→ Agreement Activation
→ Occupancy
→ Move-In
→ Resident Operations
→ Maintenance
→ Renewal or Move-Out
```

The MVP must preserve:

* Organization isolation
* permanent identifiers
* accurate property hierarchy
* annual-rent workflows
* payment-verification distinction
* transactional allocation
* agreement immutability
* signature integrity
* occupancy prerequisites
* resident privacy
* vendor scope
* document security
* human authority
* accessible mobile operation
* low-bandwidth resilience
* operational support
* auditability
* controlled provider use
* future SaaS extensibility

No roadmap label, pilot urgency, provider capability, AI output, payment screenshot, verbal approval, spreadsheet entry, hidden administrator control, implementation convenience, founding-team relationship, or demonstration success may independently justify bypassing an approved Habitacores business rule, authorization control, tenant boundary, financial control, agreement control, occupancy prerequisite, security requirement, test gate, or production-readiness requirement.