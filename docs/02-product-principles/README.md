# Document 02 — Product Principles

## Document Control

| Field | Value |
|---|---|
| Product | Habitacores |
| Document | Product Principles |
| Version | 0.1 |
| Status | Draft for approval |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Market | Nigeria |
| Last Updated | 29 July 2026 |

## 1. Purpose

This document defines the permanent product principles that guide the design, prioritization, architecture, implementation, operation, and evolution of Habitacores.

These principles apply to:

- Product strategy
- Business rules
- User experience
- Data architecture
- System architecture
- API design
- Security
- Artificial intelligence
- Testing
- Deployment
- Product governance
- Future market expansion

When two possible solutions appear equally valid, the solution that aligns more closely with these principles should be preferred.

## 2. Principle Hierarchy

Habitacores product decisions should be evaluated in the following order:

1. Safety and legal integrity
2. Security and privacy
3. Correctness and data integrity
4. User trust and transparency
5. Operational usefulness
6. Accessibility and usability
7. Scalability and maintainability
8. Delivery speed
9. Technical novelty

Delivery speed must never override security, correctness, legal integrity, or user trust.

## 3. Community First

Habitacores treats the residential community as the primary operating context.

A property should not be modeled only as an isolated listing. It exists within a broader structure that may include:

- Development
- Community
- Phase
- Block
- Floor
- Unit
- Amenities
- Shared facilities
- Residents
- Management
- Vendors
- Future construction

The community-first principle ensures that the platform can support both individual units and complete residential developments.

## 4. Reality Drives Software

Habitacores must reflect validated real-world workflows.

Software must not force property owners, residents, agents, lawyers, or operators into processes that do not match legitimate operational reality.

Every major feature must be supported by at least one of the following:

- A real workflow in the founding-team property
- A validated customer requirement
- A legal or regulatory requirement
- A documented operational risk
- A clear strategic platform requirement

Features based only on assumption should be marked as experimental until validated.

## 5. Nigeria First, Globally Extensible

Habitacores will initially be designed for Nigerian residential property operations.

The initial product must support:

- Annual rent
- Upfront rental payments
- Agency fees
- Legal fees
- Caution or security deposits
- Service charges
- Property inspections
- Estate-agent workflows
- Tenancy agreements
- Bank-transfer confirmation
- Local communication practices
- Nigerian addresses and location conventions

Localization must be configurable rather than hardcoded wherever practical.

The architecture should later support:

- Monthly rent
- Different currencies
- Different lease periods
- Jurisdiction-specific documents
- Country-specific payment providers
- Localized date, address, tax, and identity formats

## 6. Transparency by Default

Habitacores must make material property information visible to the appropriate users.

Examples include:

- Annual rent
- Additional charges
- Refundable amounts
- Non-refundable amounts
- Lease duration
- Payment deadlines
- Application status
- Inspection status
- Agreement status
- Maintenance progress
- Renewal dates
- Responsible parties

The platform must avoid hidden charges, ambiguous statuses, and misleading property information.

Transparency does not mean exposing confidential information. Access must remain role-appropriate and permission-controlled.

## 7. Trust Must Be Designed

Trust is a product capability, not merely a marketing claim.

Habitacores should establish trust through:

- Verified identities
- Verified organizations
- Property ownership or authority records
- Accurate listing information
- Complete fee disclosure
- Document version history
- Payment evidence
- Audit records
- Clear responsibilities
- Human approval for critical decisions
- Consistent communication

Trust indicators must correspond to actual verification actions. The platform must not display unsupported claims such as “verified” without a recorded verification process.

## 8. Role-Aware Experiences

Every user should receive an experience tailored to their role, responsibilities, organization, and assigned property context.

Examples include:

- Property owner
- Owner administrator
- Property manager
- Estate agent
- Estate lawyer
- Prospective resident
- Approved resident
- Vendor
- Maintenance worker
- Platform administrator

A user may hold multiple roles across multiple organizations.

Role-aware design must not be implemented solely by hiding interface elements. Authorization must also be enforced on the server and data layer.

## 9. Least Privilege

Users, services, integrations, and AI agents should receive only the minimum access necessary to perform their approved responsibilities.

The system must avoid broad default access.

Examples:

- An agent should not access unrelated financial records.
- A resident should not access another resident’s application or lease.
- A vendor should see only assigned work orders and relevant access information.
- A lawyer should access only assigned legal matters and supporting records.
- An internal operator should not automatically receive platform-wide administrative privileges.

Permission elevation must be deliberate, traceable, and reversible.

## 10. Multi-Tenancy from the Beginning

Habitacores must be designed as a multi-tenant SaaS platform from the beginning, even though the founding organization will be the first operational tenant.

Every tenant-owned business record should be scoped to an organization or another approved tenancy boundary.

Tenant isolation must be enforced through:

- Data modeling
- Queries
- Authorization
- APIs
- Background jobs
- File storage
- Search
- Analytics
- Logs
- Exports
- Artificial intelligence context

Tenant isolation must not depend only on user-interface filtering.

## 11. Property and SaaS Tenant Terminology Must Remain Distinct

Habitacores operates in a domain where “tenant” can mean two different things:

1. A SaaS tenant, meaning an organization using the platform
2. A residential tenant, meaning a person occupying a rental unit

To avoid ambiguity:

- Use `organization` for the SaaS tenant
- Use `resident`, `applicant`, or `occupant` for residential users
- Use `tenancy agreement` for the legal occupancy agreement
- Avoid using the standalone term `tenant` in technical schemas unless the meaning is explicit

The glossary and codebase must preserve this distinction.

## 12. One Authoritative Source of Truth

Each critical business concept must have one authoritative source.

Examples:

- The property registry is authoritative for property identity.
- The unit record is authoritative for unit status.
- The agreement record is authoritative for lease dates and terms.
- The payment ledger is authoritative for payment status.
- The application record is authoritative for applicant status.
- The permission model is authoritative for access decisions.
- The Product Knowledge Base is authoritative for approved product and architecture specifications.

Derived views may summarize authoritative data but must not independently redefine it.

## 13. Data Integrity Over Convenience

The platform must preserve accurate relationships and states, even when doing so requires additional validation.

The system should prevent or explicitly handle:

- Duplicate units
- Overlapping active agreements
- Multiple active residents assigned incompatibly
- Invalid payment allocations
- Impossible workflow transitions
- Missing ownership context
- Conflicting fee totals
- Invalid organization relationships
- Orphaned documents
- Unauthorized status changes

Critical business records should not be silently overwritten.

## 14. Explicit Workflows

Important business processes must have defined states and permitted transitions.

Examples include:

### Listing

```text
Draft
→ Pending Review
→ Approved
→ Published
→ Reserved
→ Leased
→ Archived
Inspection
Requested
→ Confirmed
→ Completed
→ Cancelled
→ No-Show
Application
Started
→ Submitted
→ Under Review
→ More Information Required
→ Approved
→ Rejected
→ Withdrawn
Agreement
Draft
→ Legal Review
→ Issued
→ Partially Signed
→ Fully Signed
→ Active
→ Expired
→ Terminated

Workflow transitions must be validated and auditable.

15. Human Accountability for Critical Decisions

Artificial intelligence and automation may assist with:

Summarization
Classification
Reminder generation
Document extraction
Risk flagging
Recommended actions
Draft communications
Operational predictions

AI must not independently make final decisions concerning:

Applicant approval or rejection
Legal conclusions
Eviction
Agreement termination
Ownership verification
Financial eligibility
Refund denial
Security incidents
Regulatory reporting

A responsible human must approve high-impact outcomes.

16. AI Must Be Explainable and Reviewable

AI-generated outputs must be clearly identified when they influence business workflows.

Where practical, the platform should preserve:

Model or service used
Input context
Output
Timestamp
Confidence or uncertainty
Human reviewer
Final human decision
Corrections made

Users must not be led to believe that AI-generated information is verified legal, financial, or property advice unless independently reviewed by a qualified person.

17. Security by Design

Security requirements must be defined before implementation, not added only after development.

Habitacores should apply:

Strong authentication
Multi-factor authentication for privileged roles
Server-side authorization
Tenant isolation
Encryption in transit
Encryption at rest where appropriate
Secure secret management
Input validation
Rate limiting
Audit logging
Secure file handling
Dependency monitoring
Backup and recovery procedures
Incident-response processes

Security-sensitive implementation decisions must be documented.

18. Privacy by Design

Habitacores will process potentially sensitive personal and property information.

The product should follow principles of:

Data minimization
Purpose limitation
Controlled access
Retention limits
Secure deletion
Consent where required
User access to appropriate records
Correction of inaccurate information
Auditability
Safe export and sharing

The system should not collect information merely because it may become useful later.

19. Auditability

Critical actions must be attributable.

Audit records should capture, where applicable:

Actor
Organization
Role
Action
Resource
Previous value
New value
Timestamp
Source
Reason
Approval
Correlation or request identifier

Examples of auditable actions include:

Application decisions
Listing publication
Fee changes
Payment-status changes
Agreement updates
Permission changes
Document replacement
Maintenance closure
Ownership verification
User suspension

Audit records must be resistant to ordinary modification.

20. Financial Transparency and Separation

Habitacores must clearly distinguish among:

Annual rent
Agency fee
Legal fee
Service charge
Caution or security deposit
Utility deposit
Inspection fee
Other approved charges
Refundable funds
Non-refundable fees
Platform fees
Payment-processing fees

The system must not represent every inflow as revenue.

The platform should preserve the relationship between:

Amount expected
Amount received
Payment method
Payer
Recipient
Allocation
Evidence
Reconciliation status
Refund status
21. Mobile-First and Low-Friction Design

Many users will interact with Habitacores primarily through mobile devices.

Core workflows should be:

Responsive
Touch-friendly
Understandable on small screens
Efficient on limited bandwidth
Resilient to interrupted sessions
Compatible with common browsers
Clear without technical knowledge

The platform should reduce unnecessary steps while preserving required validation.

22. Familiar Channels, Structured Records

Habitacores may integrate familiar communication channels such as:

Email
SMS
WhatsApp
Push notifications

However, critical business decisions and records must remain structured inside Habitacores.

A WhatsApp message may notify a user, but the authoritative application, inspection, agreement, payment, or maintenance status must remain in the platform.

23. Accessibility

Habitacores should be usable by people with different abilities, devices, literacy levels, and technical experience.

The platform should aim to support:

Keyboard navigation
Screen-reader compatibility
Sufficient contrast
Clear labels
Understandable error messages
Logical heading structure
Responsive text
Accessible forms
Alternative text for meaningful images
Reduced reliance on color alone

Accessibility is a product-quality requirement.

24. Simplicity Before Expansion

The MVP must solve a narrow set of important problems exceptionally well.

The product must avoid premature inclusion of:

Unvalidated marketplaces
Complex financial products
Full construction ERP
Advanced accounting
Unnecessary social features
Excessive AI automation
Separate applications for every stakeholder
Premature microservices
Excessive configuration

Future scale must be supported architecturally without requiring all future features to be built now.

25. Modular Monolith Before Microservices

The initial platform should prefer a modular monolith unless verified scale, security, organizational, or operational requirements justify service separation.

Modules should have:

Clear domain responsibilities
Defined interfaces
Controlled dependencies
Independent test coverage
Explicit ownership
Migration paths

Microservices must not be adopted for prestige or theoretical scalability.

26. Configuration Before Custom Code

Differences among properties, organizations, jurisdictions, and lease policies should be represented through configuration where practical.

Examples include:

Lease period
Fee types
Currency
Required application documents
Inspection rules
Approval steps
Renewal reminders
Agreement templates
Notification preferences
Property amenities

Configuration should not be used when it would weaken validation, security, or clarity.

27. APIs as Product Contracts

APIs must be designed as stable product contracts.

They should have:

Clear versioning
Consistent naming
Authentication
Authorization
Tenant scoping
Input validation
Predictable errors
Idempotency where required
Pagination
Auditability
Documentation
Test coverage

Interfaces should not expose internal database structure unnecessarily.

28. No Silent Failure

Users and operators must receive clear outcomes when important actions fail.

The system should:

Display understandable errors
Preserve user input where safe
Avoid false success states
Retry appropriate background jobs
Escalate repeated failures
Record technical diagnostics
Notify responsible operators for critical failures

Failures involving payments, agreements, permissions, and data isolation must receive special treatment.

29. Observable Operations

The system must provide enough operational visibility to understand:

Whether services are available
Whether background jobs are succeeding
Whether notifications are delivered
Whether payments reconcile
Whether integrations are failing
Whether tenant isolation controls are functioning
Whether unusual access patterns exist
Whether performance is degrading

Logs must not expose passwords, authentication tokens, government identification, payment credentials, or unnecessary personal information.

30. Test Business Rules, Not Only Code Paths

Testing must validate real product behavior.

Tests should cover:

Role permissions
Organization isolation
Workflow transitions
Financial calculations
Duplicate prevention
Agreement dates
Renewal rules
Application decisions
Audit generation
File access
Notification triggers
Failure handling

The test suite should reflect approved business rules from the Product Knowledge Base.

31. Backward-Compatible Evolution

Habitacores must evolve without unnecessarily breaking:

Existing organizations
Property records
User access
APIs
Documents
Agreements
Payment records
Audit history
Integrations

Breaking changes must be deliberate, versioned, documented, tested, and accompanied by a migration strategy.

32. Evidence-Based Prioritization

Features should be prioritized using evidence such as:

Pilot-property operations
User feedback
Support requests
Conversion data
Maintenance trends
Error rates
Time savings
Revenue impact
Legal requirements
Security risk
Strategic alignment

Executive preference alone may initiate investigation, but major implementation should still be connected to a documented problem or strategic requirement.

33. Clear Ownership

Every product capability must have clear responsibility.

The platform should identify who is responsible for:

Listing approval
Inspection confirmation
Applicant review
Agreement preparation
Payment verification
Move-in approval
Maintenance assignment
Work-order closure
Renewal decisions
Refund decisions
Property verification

Unclear ownership creates operational failure even when the software functions correctly.

34. Decisions Must Be Recorded

Material decisions must be documented through:

Product specifications
Business rules
Architecture Decision Records
Change proposals
Pull requests
Issue discussions
Release notes

Important decisions must not exist only in chat conversations, private notes, or verbal discussions.

35. Build for Handover

The platform and Product Knowledge Base must be understandable by future:

Engineers
Product managers
Designers
Security professionals
Legal advisers
Operations personnel
Investors
Auditors
AI coding agents

Implementation must not depend on one person’s undocumented memory.

36. Product Principle Review Questions

Before approving a feature or architectural decision, reviewers should ask:

Does this solve a validated problem?
Does it respect the community-first model?
Is it appropriate for the Nigerian launch market?
Does it preserve future localization?
Is the full financial impact transparent?
Is access limited by role and organization?
Is tenant isolation enforced beyond the interface?
Is the data authoritative and auditable?
Are workflow states explicit?
Does a human remain accountable for high-impact decisions?
Is personal information minimized and protected?
Can the feature be tested against real business rules?
Is the solution simpler than the alternatives?
Does it create unnecessary coupling?
Has the decision been documented?
37. Principle Enforcement

These principles are mandatory design constraints, not optional recommendations.

A proposed implementation that conflicts with an approved principle must not be merged unless:

The conflict is explicitly identified.
The business justification is documented.
Security, legal, and operational effects are reviewed.
An Architecture Decision Record is created where applicable.
The Product Visionary approves the exception.
The Chief Product Architect approves the exception.
The affected Product Knowledge Base documents are updated.
38. Current Product Principle Decision

Habitacores will prioritize trust, transparency, security, operational usefulness, and maintainable architecture over rapid expansion or unnecessary feature volume.

The initial platform will remain focused on the founding-team-owned residential community while preserving the architectural foundations required for future multi-property, multi-organization, and international growth.