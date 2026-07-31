# Document 16 — Security Architecture and Threat Model

## Document Control

| Field | Value |
|---|---|
| Product | Habitacores |
| Document | Security Architecture and Threat Model |
| Version | 0.1 |
| Status | Draft for approval |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Market | Nigeria |
| Primary Currency | Nigerian naira |
| Primary Operational Time Zone | West Africa Time |
| Primary Tenant Boundary | Organization |
| Primary Experience | Responsive web application |
| Accessibility Target | WCAG 2.2 Level AA |
| Security Model | Zero-trust-informed, defense in depth |
| Last Updated | 30 July 2026 |

## 1. Purpose

This document defines the Security Architecture and Threat Model for Habitacores.

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
- Permission matrix and authorization model
- User journeys and workflows
- API architecture
- UX architecture
- UI design system and interaction standards

into a structured security model.

This document defines:

- Security objectives
- Security principles
- Protected assets
- Data classifications
- Trust boundaries
- Threat actors
- Attack surfaces
- Authentication security
- Session security
- Tenant isolation
- Authorization security
- Privilege-escalation controls
- Financial security
- Agreement integrity
- Occupancy and resident privacy
- Document and file security
- Webhook and integration security
- AI-specific security risks
- Administrative-access controls
- Logging and monitoring
- Incident response
- Abuse cases
- STRIDE-based threat analysis
- Security controls
- Residual risks
- Security testing
- Security acceptance criteria
- MVP security scope
- Deferred security capabilities
- Security governance

This document is a product-security and architecture specification.

It does not replace:

- Secure coding standards
- Infrastructure hardening guides
- Incident-response runbooks
- Privacy policies
- Legal compliance advice
- Penetration-test reports
- Provider-specific security assessments
- Production configuration reviews

Those artifacts will be developed separately.

## 2. Security Objectives

Habitacores security must:

1. Protect user identities.
2. Protect organization boundaries.
3. Prevent cross-tenant data exposure.
4. Enforce server-side authorization.
5. Prevent privilege escalation.
6. Protect financial records and transactions.
7. Preserve agreement integrity.
8. Protect resident and applicant privacy.
9. Protect sensitive documents.
10. Protect against fraudulent payment evidence.
11. Protect provider integrations.
12. Prevent malicious webhook processing.
13. Protect administrative and support access.
14. Preserve auditability.
15. Detect suspicious activity.
16. Support incident response.
17. Reduce abuse of public interfaces.
18. Protect low-bandwidth and mobile users.
19. Contain failures.
20. Support recovery without falsifying system state.
21. Protect AI-assisted workflows from prompt and data attacks.
22. Preserve human authority over high-impact decisions.
23. Support secure SaaS scale.
24. Avoid pilot-specific security assumptions.
25. Maintain security through implementation, testing, and operations.

## 3. Security Principles

### 3.1 Deny by Default

Access is denied unless explicitly permitted.

### 3.2 Server-Side Enforcement

Client-side navigation, hidden buttons, role labels, and cached state are not security controls.

### 3.3 Least Privilege

Users, services, integrations, and support personnel receive only the access required.

### 3.4 Defense in Depth

Habitacores should use overlapping controls across:

- Identity
- Authorization
- Application logic
- Database
- Storage
- Network
- Monitoring
- Audit
- Operations

### 3.5 Tenant Isolation

Organization-owned data must remain isolated at every layer.

### 3.6 Explicit Trust Boundaries

No external system, provider callback, user device, uploaded file, or AI output is trusted automatically.

### 3.7 Separation of Duties

High-risk operations should separate preparation, review, approval, and execution where appropriate.

### 3.8 Immutable Evidence

Issued agreements, signed versions, audit events, and authoritative transaction history must not be silently rewritten.

### 3.9 Safe Failure

Security failures should deny or defer actions rather than approve, verify, activate, or settle them incorrectly.

### 3.10 Traceability

Material security and business actions must be attributable to an actor, service, provider, or system process.

## 4. Security Scope

Security applies to:

- Public marketplace
- Authentication
- Personal portal
- Applicant workflows
- Resident workflows
- Organization operations
- Professional workspaces
- Vendor workspaces
- Platform administration
- APIs
- Databases
- Object storage
- Background jobs
- Notifications
- Webhooks
- Provider integrations
- AI services
- Logging
- Monitoring
- Support access
- Development and deployment pipelines

## 5. Protected Asset Categories

### 5.1 Identity Assets

- User accounts
- Authentication credentials
- Session tokens
- Multi-factor credentials
- Password-reset tokens
- Email-verification tokens
- Identity-provider links
- Device and session metadata

### 5.2 Organization Assets

- Organization profiles
- Memberships
- Roles
- Permissions
- Assignments
- Organization relationships
- Configuration
- Subscription information later

### 5.3 Property Assets

- Properties
- Communities
- Buildings
- Blocks
- Floors
- Units
- Amenities
- Development information
- Operational readiness

### 5.4 Leasing Assets

- Listings
- Inquiries
- Inspections
- Applications
- Applicant declarations
- Reviews
- Recommendations
- Decisions
- Offers

### 5.5 Financial Assets

- Charges
- Payments
- Payment evidence
- Verification results
- Provider references
- Allocations
- Adjustments
- Waivers
- Deposits
- Refunds
- Reversals
- Receipts
- Reconciliation records

### 5.6 Agreement Assets

- Agreement templates
- Drafts
- Versions
- Approval records
- Issued agreements
- Signed agreements
- Signature evidence
- Amendments
- Terminations

### 5.7 Occupancy Assets

- Occupancies
- Household members
- Move-in records
- Move-out records
- Condition reports
- Access items
- Renewal cases
- Deposit decisions

### 5.8 Operational Assets

- Maintenance requests
- Work orders
- Vendor assignments
- Quotes
- Completion evidence
- Legal matters
- Communications
- Notifications
- Tasks

### 5.9 Sensitive Documents

- Government identification
- Income evidence
- Bank evidence
- Payment screenshots
- Signed agreements
- Legal documents
- Property-ownership documents
- Vendor compliance documents
- Resident condition evidence

### 5.10 Platform Assets

- Platform configuration
- Feature flags
- Provider credentials
- Encryption keys
- Deployment secrets
- Audit logs
- Support sessions
- Security events
- Incident records

## 6. Data Classification

Habitacores should classify data into:

### 6.1 Public

Examples:

- Published listing title
- Approved listing description
- Approved public images
- General location
- Published annual rent
- Approved amenities

### 6.2 Internal

Examples:

- Internal workflow metadata
- Non-sensitive operational notes
- Organization configuration
- Task assignments

### 6.3 Confidential

Examples:

- Applications
- Contact information
- Payment records
- Agreement drafts
- Maintenance details
- Vendor records
- Internal communications

### 6.4 Restricted

Examples:

- Government identification
- Bank references
- Income evidence
- Signed agreements
- Legal notes
- Security events
- Support-session records
- Authentication secrets
- Encryption keys

## 7. Data Handling by Classification

| Classification | Access | Logging | Storage | Sharing |
|---|---|---|---|---|
| Public | Public or authorized | Ordinary | Standard protected storage | Approved public use |
| Internal | Authorized personnel | Limited metadata | Encrypted | Internal only |
| Confidential | Need-to-know | Redacted | Encrypted | Controlled |
| Restricted | Explicit privileged access | Strongly redacted and audited | Strong encryption and strict policy | Exceptional and approved |

## 8. Trust Boundaries

Primary trust boundaries include:

```text
Public Internet
→ Edge and Application Boundary
→ Authentication Boundary
→ Organization Authorization Boundary
→ Domain-Service Boundary
→ Database Boundary
→ Object-Storage Boundary
→ Background-Job Boundary
→ External-Provider Boundary
→ AI-Provider Boundary
→ Platform-Administration Boundary
````

Each boundary requires independent validation.

## 9. Public Internet Boundary

Requests from public users must be treated as untrusted.

Controls include:

* TLS
* Input validation
* Rate limiting
* Bot and abuse controls
* Request-size limits
* Secure headers
* Safe errors
* Monitoring

## 10. Client Boundary

Web and future mobile clients are untrusted execution environments.

The client must not be trusted to determine:

* Organization authority
* Role authority
* Final status
* Financial balance
* Payment verification
* Agreement activation
* Occupancy activation
* Document sensitivity
* Support access

## 11. Authentication Boundary

Authentication proves an identity claim.

It does not independently prove:

* Active organization membership
* Resource access
* Role scope
* Assignment
* Approval authority
* Record-state eligibility

## 12. Organization Boundary

Each organization is a tenant boundary.

Every organization-owned resource must be associated with an organization directly or through an authorized ownership chain.

## 13. Database Boundary

The application must not rely solely on application-layer filtering for tenant isolation.

Database-level controls should be used where practical, including:

* Organization keys
* Foreign-key integrity
* Constraints
* Row-level security where adopted
* Restricted service roles
* Migration review
* Least-privileged connections

## 14. Object-Storage Boundary

Files in object storage must not be publicly accessible by default.

Controls should include:

* Private buckets
* Short-lived signed URLs
* Organization-scoped object paths
* Metadata validation
* Malware scanning
* Access auditing
* Retention controls

## 15. Provider Boundary

External providers are not authoritative over Habitacores domain state.

Provider responses must be:

* Authenticated
* Validated
* Mapped
* Deduplicated
* Recorded
* Processed through domain rules

## 16. AI Boundary

AI systems must be treated as probabilistic external processors.

AI output must not independently:

* Approve applications
* Verify payments
* Activate agreements
* Activate occupancies
* Approve refunds
* Terminate agreements
* Assign legal liability
* Expose restricted data

## 17. Threat Actors

Potential threat actors include:

### 17.1 Anonymous External Attacker

May attempt:

* Account enumeration
* Credential attacks
* Public-form abuse
* Vulnerability exploitation
* File-upload attacks
* API scraping
* denial of service

### 17.2 Malicious Authenticated User

May attempt:

* Accessing another user’s records
* Manipulating URLs
* Changing client state
* Uploading fraudulent evidence
* Replaying requests
* Exploiting workflow gaps

### 17.3 Malicious Organization Member

May attempt:

* Privilege escalation
* Financial fraud
* Unauthorized approval
* Data export
* Cross-property access
* Manipulating assignments
* Concealing activity

### 17.4 Compromised Professional Account

Examples:

* Estate Agent
* Estate Lawyer
* Finance Reviewer
* Vendor Worker

A compromised account may attempt to access assigned or related records.

### 17.5 Malicious Platform Insider

May attempt:

* Broad customer-data access
* Secret extraction
* privilege manipulation
* support-session abuse
* audit manipulation

### 17.6 Compromised Provider

A provider may send:

* False callbacks
* Duplicate events
* malformed payloads
* incorrect status
* leaked credentials
* malicious content

### 17.7 Automated Abuse Actor

May perform:

* Credential stuffing
* scraping
* spam inquiries
* fake applications
* brute force
* high-volume uploads
* AI resource abuse

### 17.8 Accidental Insider

May:

* Approve the wrong record
* expose sensitive data
* upload incorrect documents
* operate in the wrong organization
* send confidential communications
* misconfigure permissions

## 18. Attack Surfaces

Primary attack surfaces include:

* Registration
* Login
* Password reset
* Session handling
* Public listing search
* Inquiry forms
* Inspection requests
* Application forms
* File uploads
* Document downloads
* Search
* APIs
* Webhooks
* Provider integrations
* AI prompts
* Admin interfaces
* Support sessions
* Exports
* Background jobs
* Deployment pipelines
* Secrets
* Logs

## 19. Authentication Architecture

Authentication should support:

* Unique user identity
* Verified contact method
* Secure password storage where passwords are used
* Session revocation
* Password reset
* Multi-factor authentication for privileged users
* Device and session visibility
* Brute-force protection
* Suspicious-login monitoring

## 20. Password Security

Where passwords are used:

* Passwords must be hashed using an approved adaptive algorithm.
* Plaintext passwords must never be stored.
* Passwords must not be logged.
* Password-reset links must be single use.
* Compromised-password screening should be evaluated.
* Arbitrary frequent rotation should not be required without reason.
* Strong password guidance should be provided.

## 21. Account Enumeration Protection

Authentication and recovery responses should avoid confirming whether an account exists unnecessarily.

Example:

```text
If an account is associated with this address, recovery instructions will be sent.
```

## 22. Credential-Stuffing Controls

Controls may include:

* Rate limiting
* IP and device signals
* Progressive delay
* suspicious-login alerts
* MFA
* breached-password detection
* session revocation

## 23. Multi-Factor Authentication

MFA should be required or strongly encouraged for:

* Platform administrators
* Security administrators
* Organization owners
* Finance approvers
* Support users with temporary access
* Users performing high-risk actions

## 24. Step-Up Authentication

Step-up authentication may be required for:

* Viewing restricted identification
* Exporting sensitive data
* Transferring organization ownership
* Approving large refunds
* Initiating emergency access
* Changing authentication settings
* Revealing provider credentials
* Revoking all sessions

## 25. Session Security

Session controls should include:

* Secure cookies where cookies are used
* HTTP-only cookies
* SameSite policy
* short-lived access tokens
* session rotation
* logout
* revocation
* inactivity timeout
* absolute timeout
* suspicious-session termination
* device visibility

## 26. Session Fixation Protection

Sessions must be rotated after:

* Login
* Privilege change
* MFA completion
* Password reset
* Support-access activation

## 27. Session Revocation

Users and administrators should be able to revoke:

* Individual sessions
* All other sessions
* Sessions after password compromise
* Sessions after role revocation
* Sessions after membership suspension

## 28. Cross-Site Request Forgery

Where cookie authentication is used, state-changing requests must use approved CSRF controls.

Controls may include:

* SameSite cookies
* CSRF tokens
* origin validation
* referer validation
* framework protections

## 29. Cross-Site Scripting

Controls should include:

* Output encoding
* framework-safe rendering
* content security policy
* sanitization
* restricted rich text
* avoiding unsafe HTML injection
* file-content isolation

## 30. Content Security Policy

A Content Security Policy should restrict:

* Script sources
* style sources
* image sources
* frame sources
* object sources
* connection sources
* form destinations

Production policy should avoid unrestricted wildcard sources where possible.

## 31. Organization Isolation

Every protected request must validate:

1. User identity
2. Active account
3. Organization context
4. Active membership
5. Permission
6. Resource organization
7. Scope
8. Assignment
9. Record state
10. Separation of duties

## 32. Cross-Tenant Threat

Threat:

A user changes an identifier to access another organization’s record.

Controls:

* Server-side organization checks
* organization foreign keys
* row-level controls
* scoped repository methods
* safe not-found responses
* tenant-isolation tests

## 33. Indirect Tenant Leakage

Tenant data may leak through:

* Search suggestions
* analytics
* logs
* notifications
* exports
* error messages
* cache keys
* document URLs
* provider metadata

Each channel must be tenant scoped.

## 34. Cache Isolation

Cache keys must include sufficient context, including:

* Organization
* User or permission scope where relevant
* Resource
* Query
* version

Sensitive responses should avoid shared caching.

## 35. Authorization Architecture

Authorization must combine:

* Role-based access control
* permission-based access control
* scope
* assignment
* relationship
* record state
* approval rules
* separation of duties
* field-level restrictions

## 36. Insecure Direct Object Reference

Threat:

A user accesses a resource through a known identifier.

Controls:

* Authorization on every resource request
* opaque identifiers
* no reliance on hidden URLs
* safe response behavior
* automated tests

## 37. Privilege Escalation

Potential paths include:

* Editing client-side role state
* manipulating membership requests
* self-assigning roles
* abusing organization relationships
* exploiting support sessions
* reusing expired assignments
* stale permission caches

## 38. Role Assignment Security

Role assignment must require:

* Appropriate permission
* valid target user
* valid organization
* approved role
* approval where required
* audit event
* optional expiry
* separation-of-duties checks

## 39. Assignment Security

Professional assignments must include:

* Resource scope
* organization scope
* start time
* expiry
* status
* assigning authority
* revocation
* audit history

## 40. Authorization Cache Security

Permission caches must be invalidated when:

* Membership is revoked
* Role changes
* Assignment expires
* Organization relationship ends
* Support session ends
* User is suspended

Sensitive commands should perform live authorization checks.

## 41. Field-Level Authorization

The system must separately control access to fields such as:

* Government identification
* Bank references
* Internal review notes
* Legal notes
* provider payloads
* security events
* authentication metadata

Read access to a record does not imply access to all fields.

## 42. Separation of Duties

Examples of prohibited or controlled self-action:

* Preparing and approving a high-value refund
* Creating and approving a privileged role
* Submitting and verifying one’s own payment evidence
* Drafting and finally approving sensitive legal content
* Initiating and approving emergency access

## 43. Financial Security Objectives

Financial workflows must prevent:

* False payment confirmation
* Duplicate payment records
* duplicate allocation
* over-allocation
* unauthorized reversal
* unauthorized refund
* currency mismatch
* charge manipulation
* receipt falsification
* reconciliation concealment

## 44. Payment Evidence Security

Payment screenshots and uploaded evidence are untrusted.

They may be:

* Forged
* edited
* duplicated
* reused
* associated with another payment
* malware-bearing
* misleading

Evidence submission must remain distinct from verification.

## 45. Payment Verification Controls

Verification should require:

* Authorized reviewer
* Payment state validation
* payer and recipient review
* amount review
* date review
* reference review
* duplicate detection
* organization validation
* audit
* optional separation of duties

## 46. Duplicate Payment Controls

Potential controls include:

* Unique provider references
* normalized bank references
* amount and date similarity checks
* payer similarity checks
* duplicate-evidence hashes
* manual-review alerts
* provider reconciliation

## 47. Payment Allocation Security

Allocation must occur transactionally.

The system must validate:

* Payment verified
* available payment balance
* charge outstanding balance
* currency match
* organization match
* current record versions
* authorized actor

## 48. Financial Concurrency Threat

Threat:

Two reviewers allocate the same payment simultaneously.

Controls:

* Database transaction
* row locking or equivalent
* version checks
* unique constraints
* conflict response
* audit

## 49. Refund Security

Refund workflows should require:

* Source payment
* eligible refundable amount
* reason
* recipient verification
* approval
* idempotency
* provider reference
* execution state
* reconciliation
* audit

## 50. Reversal Security

A reversal must:

* Preserve original payment history
* Create explicit reversing records
* update allocation state safely
* avoid deletion
* require authorization
* preserve receipts and audit history

## 51. Receipt Integrity

Receipts must derive from authoritative financial records.

Users must not manually enter:

* settled amount
* payment verification
* allocation
* receipt number

without approved system logic.

## 52. Agreement Security Objectives

Agreement security must preserve:

* Correct parties
* Correct property and unit
* Correct financial terms
* version integrity
* approval history
* signature evidence
* activation requirements
* amendment history
* termination history

## 53. Agreement Version Integrity

Issued and signed versions must be immutable.

Material changes require:

* New version
* Amendment
* Replacement
* fresh approval
* fresh signature where required

## 54. Agreement Tampering Threat

Potential attacks include:

* Editing terms after signature
* substituting a document
* changing party details
* changing rent
* changing dates
* reusing signature evidence
* linking the wrong version

Controls include:

* Version identifiers
* checksums
* immutable storage
* signature-provider references
* signed-document preservation
* audit events

## 55. Signature Security

Signature workflows should validate:

* Signer identity
* agreement version
* signature request
* expiry
* provider callback
* signature evidence
* duplicate callback
* decline or cancellation
* final document hash

## 56. Occupancy Security Objectives

Occupancy security must prevent:

* Unauthorized resident access
* overlapping incompatible occupancy
* activation without agreement
* activation without required payment
* unauthorized household changes
* premature move-out completion
* improper deposit decisions

## 57. Resident Privacy

Resident data should be visible only to users with a legitimate operational need.

Vendors should receive only the minimum required information.

Examples:

* Work location
* access instructions
* approved contact channel
* work scope

They should not receive unrelated application or financial data.

## 58. Household Privacy

Household records may include minors and dependants.

Controls should include:

* minimum necessary fields
* restricted display
* no public search
* no unnecessary notifications
* limited export
* retention controls

## 59. Maintenance Privacy

Maintenance requests may reveal:

* Occupancy patterns
* resident schedules
* access instructions
* living conditions
* security-sensitive property information

Access must be limited.

## 60. Document Security Objectives

Document controls must protect:

* Confidentiality
* integrity
* authenticity
* availability
* retention
* lawful access
* version history

## 61. File Upload Threats

Threats include:

* Malware
* executable content
* oversized files
* decompression bombs
* spoofed MIME types
* malicious filenames
* embedded scripts
* path traversal
* duplicate malicious content
* prohibited file types

## 62. File Upload Controls

Controls should include:

* Allowlisted file types
* file-size limits
* MIME validation
* extension validation
* checksum
* malware scan
* safe filenames
* isolated storage
* no direct execution
* scan-before-access
* upload intent
* finalization validation

## 63. File Download Security

Downloads should use:

* Authorization check
* short-lived signed URL
* correct content type
* safe content disposition
* access audit
* optional step-up authentication
* no predictable public URL

## 64. Document Version Security

Replacement should create a new version rather than overwrite history where auditability is required.

## 65. Restricted Document Access

Access to restricted documents should require:

* Explicit permission
* resource relationship
* active scope
* assignment where relevant
* access logging
* optional reason
* optional step-up authentication

## 66. Search Security

Search must not reveal unauthorized:

* Names
* identifiers
* payment references
* agreement titles
* document metadata
* organizations
* units
* legal matters

Search suggestions must use the same authorization rules as full results.

## 67. Export Security

Sensitive exports should require:

* Explicit permission
* filters
* field selection
* approval where required
* step-up authentication
* secure storage
* expiry
* access logging
* download limits

## 68. Notification Security

Notifications should minimize sensitive data.

External channels should generally contain:

* Event summary
* secure link
* minimal context

They should not contain unrestricted:

* Government IDs
* bank references
* signed agreements
* legal notes
* full application content

## 69. Communication Security

Communications should protect against:

* Sending to wrong recipient
* unauthorized template use
* data leakage
* HTML injection
* malicious attachments
* spoofing
* provider delivery failure

## 70. Webhook Threats

Threats include:

* Forged signatures
* replay
* duplicate events
* delayed events
* out-of-order events
* unsupported event types
* cross-environment events
* oversized payloads
* malicious content

## 71. Webhook Controls

Webhook receivers must validate:

* Provider
* signature
* timestamp
* replay window
* event identifier
* account identifier
* environment
* payload size
* event type
* raw payload where required

## 72. Webhook Processing Pattern

```text
Receive
→ Authenticate provider
→ Validate event
→ Persist event
→ Acknowledge
→ Process asynchronously
→ Apply domain rules
→ Record result
→ Audit
```

## 73. Provider Credential Security

Provider credentials must be:

* Stored in secret management
* inaccessible to clients
* scoped
* rotated
* monitored
* environment specific
* excluded from logs
* excluded from source control

## 74. Provider Failure Security

Provider failure must not create false business state.

Examples:

* Failed payment callback must not verify a payment.
* Failed signature callback must not mark an agreement signed.
* Failed notification must not reverse a business action.
* Failed AI request must not alter decision state.

## 75. AI Threat Model

AI-specific threats include:

* Prompt injection
* malicious document content
* data exfiltration
* hallucinated facts
* biased output
* excessive data sharing
* unauthorized decisions
* model manipulation
* unsafe generated communication
* sensitive-data retention by provider
* cross-tenant prompt contamination

## 76. Prompt Injection

User-controlled content may attempt to instruct AI systems to:

* Ignore policies
* expose confidential data
* alter decisions
* reveal hidden prompts
* generate unauthorized actions

AI input must be treated as untrusted data.

## 77. AI Data-Minimization Controls

Before sending data to an AI provider:

* Remove unnecessary personal data.
* Redact restricted identifiers.
* Limit organization context.
* Send only relevant fields.
* Avoid secrets.
* avoid raw authentication or provider data.
* apply provider-retention settings where available.

## 78. AI Output Controls

AI output must:

* Be labeled
* remain advisory
* be reviewable
* be editable where appropriate
* not alter authoritative state automatically
* be logged at an appropriate level
* preserve source access
* fail safely

## 79. AI Cross-Tenant Isolation

AI prompts, retrieval contexts, caches, vector stores, and logs must remain organization scoped.

## 80. AI-Generated Communication Risk

AI-generated communication may contain:

* incorrect facts
* discriminatory language
* legal overstatement
* unsafe commitments
* privacy leakage

Authorized human review is required before external sending.

## 81. Platform Administration Security

Platform privileges must be narrowly assigned.

Platform roles should be separated across:

* Administration
* Support
* Security
* Audit
* Infrastructure

## 82. Support Access Security

Support access must not grant unrestricted tenant access by default.

Support sessions should include:

* Support case
* purpose
* scope
* organization
* approval
* start time
* expiry
* actor
* audit
* termination

## 83. Support Session Visibility

Where required, the customer organization should be able to view:

* Active support access
* purpose
* scope
* start
* expiry
* actor

## 84. Emergency Access

Emergency access should require:

* Defined emergency
* strong authentication
* reason
* restricted scope
* short duration
* heightened logging
* post-event review
* notification where appropriate

## 85. Administrative Action Security

High-risk administrative actions include:

* Organization suspension
* ownership transfer
* role escalation
* emergency access
* provider credential rotation
* audit export
* security-event closure

These actions may require:

* MFA
* confirmation
* reason
* second approval
* immutable audit

## 86. Logging Security

Logs should include:

* Correlation ID
* actor
* organization
* action
* route
* status
* error code
* duration
* provider
* security signal

Logs must not include:

* Passwords
* tokens
* session cookies
* full government IDs
* raw restricted documents
* encryption keys
* full bank evidence
* secret provider payloads

## 87. Audit Security

Audit events should be append-oriented and protected from ordinary modification.

Material events include:

* Login
* failed login
* MFA change
* password reset
* membership change
* role assignment
* assignment change
* support access
* payment verification
* allocation
* reversal
* refund
* agreement issue
* signature
* occupancy activation
* document reveal
* sensitive export

## 88. Audit Tampering Threat

Controls should include:

* Restricted write path
* append-only model where practical
* integrity monitoring
* separate audit permissions
* retention
* export controls
* incident alerts for deletion attempts

## 89. Security Monitoring

Security monitoring should detect:

* Repeated login failure
* credential stuffing
* unusual role changes
* cross-tenant access attempts
* repeated authorization denial
* unusual document downloads
* bulk exports
* repeated payment verification failure
* webhook signature failures
* provider anomalies
* emergency access
* suspicious support access

## 90. Alert Severity

Security alerts may be classified as:

* Informational
* Low
* Medium
* High
* Critical

Severity should reflect:

* Data sensitivity
* scale
* actor privilege
* exploitability
* business impact
* persistence

## 91. Incident Categories

Potential incident categories include:

* Account compromise
* Data exposure
* Cross-tenant access
* Financial fraud
* Malicious file
* Provider compromise
* webhook attack
* privilege escalation
* insider misuse
* AI data exposure
* denial of service
* secret leakage

## 92. Incident Response Lifecycle

```text
Detect
→ Triage
→ Contain
→ Investigate
→ Eradicate
→ Recover
→ Notify
→ Review
→ Improve
```

## 93. Incident Containment

Containment actions may include:

* Revoke sessions
* suspend accounts
* disable provider integration
* rotate credentials
* block malicious IPs
* pause affected commands
* disable file downloads
* isolate organization
* activate read-only mode
* preserve evidence

## 94. Recovery Principles

Recovery must:

* Preserve audit evidence
* avoid silently rewriting history
* revalidate financial state
* revalidate agreement state
* revalidate occupancy state
* notify affected operators
* document corrective actions

## 95. Vulnerability Management

The security program should include:

* Dependency scanning
* code scanning
* secret scanning
* infrastructure scanning
* patch management
* vulnerability triage
* remediation deadlines
* verification
* risk acceptance

## 96. Dependency Security

Third-party dependencies should be evaluated for:

* Maintenance
* vulnerabilities
* licensing
* popularity
* supply-chain risk
* transitive dependencies
* update path

## 97. Software Supply-Chain Security

Controls may include:

* Lockfiles
* dependency pinning
* trusted registries
* signed releases where available
* build provenance
* protected branches
* CI permissions
* secret scanning
* reviewed updates

## 98. Source-Control Security

Repository controls should include:

* Private repositories where appropriate
* branch protection
* pull-request review
* required checks
* protected main branch
* no direct production secrets
* signed commits later where approved
* least-privileged repository access

## 99. CI/CD Security

CI/CD should protect:

* Build secrets
* deployment credentials
* environments
* approval gates
* artifact integrity
* dependency installation
* pull-request permissions
* fork execution
* logs

## 100. Environment Separation

Habitacores should separate:

* Local
* Development
* Test
* Staging
* Production

Production data and credentials must not be used casually in lower environments.

## 101. Test-Data Security

Test data should use:

* Synthetic data
* redacted samples
* approved fixtures
* no real restricted documents
* no production secrets
* no real payment credentials

## 102. Secret Management

Secrets must not be stored in:

* Source files
* README files
* screenshots
* issue comments
* client bundles
* logs
* analytics
* public CI output

## 103. Encryption in Transit

All production network communication must use approved TLS.

## 104. Encryption at Rest

Confidential and restricted data should be encrypted at rest through:

* Database encryption
* object-storage encryption
* backup encryption
* secret encryption
* provider-managed encryption where approved

## 105. Key Management

Encryption keys should be:

* Environment specific
* access controlled
* rotated
* audited
* recoverable
* not embedded in source code

## 106. Backup Security

Backups should be:

* Encrypted
* access controlled
* tested
* retained according to policy
* protected against deletion
* monitored
* restorable

## 107. Backup Data Isolation

Backup access must not become a bypass around tenant authorization.

## 108. Availability Threats

Availability threats include:

* Denial of service
* provider outage
* database failure
* queue failure
* object-storage outage
* malicious traffic
* runaway jobs
* resource exhaustion

## 109. Availability Controls

Controls may include:

* Rate limiting
* autoscaling
* queue isolation
* timeouts
* circuit breakers
* caching
* health checks
* backups
* failover
* graceful degradation

## 110. Graceful Degradation

When a dependency fails:

* Manual review should remain possible where appropriate.
* AI features should fail independently.
* Notifications may queue.
* provider-dependent actions may remain pending.
* authoritative state must remain correct.

## 111. Public-Form Abuse

Public inquiry, inspection, registration, and application endpoints may be abused for:

* Spam
* harassment
* resource exhaustion
* fraudulent applications
* automated scraping

Controls may include:

* Rate limiting
* bot detection
* verification
* abuse scoring
* moderation
* throttling
* temporary blocking

## 112. Enumeration Threats

Attackers may attempt to enumerate:

* Users
* organizations
* applications
* payments
* units
* agreements
* documents

Controls include:

* Opaque identifiers
* safe errors
* authorization
* rate limiting
* monitoring

## 113. Business Logic Abuse

Examples:

* Submitting after expiry
* reusing an offer
* activating occupancy twice
* allocating a reversed payment
* signing a superseded agreement
* using an expired assignment
* bypassing approval

State-machine validation is required.

## 114. Race Conditions

Security-relevant race conditions include:

* Double payment allocation
* Double refund
* Concurrent approval
* Duplicate occupancy activation
* Role revocation during action
* Agreement version replacement

Controls include:

* Transactions
* locks
* version checks
* idempotency
* unique constraints
* conflict handling

## 115. STRIDE Framework

Habitacores will use STRIDE categories:

* Spoofing
* Tampering
* Repudiation
* Information Disclosure
* Denial of Service
* Elevation of Privilege

## 116. Spoofing Threats

Examples:

* Stolen credentials
* forged session
* fake provider webhook
* impersonated applicant
* fraudulent signer
* fake support user

Controls:

* Strong authentication
* MFA
* secure sessions
* webhook signatures
* identity verification
* step-up authentication
* audit

## 117. Tampering Threats

Examples:

* Altering payment evidence
* modifying agreement terms
* changing audit records
* editing financial allocations
* substituting files
* changing role assignments

Controls:

* Immutability
* hashes
* transactions
* authorization
* signed callbacks
* versioning
* audit

## 118. Repudiation Threats

Examples:

* Denying approval
* denying payment verification
* denying document access
* denying agreement issue
* denying support access

Controls:

* Actor identity
* timestamps
* audit events
* correlation IDs
* signature evidence
* reason capture

## 119. Information Disclosure Threats

Examples:

* Cross-tenant access
* sensitive search results
* leaked signed URLs
* verbose errors
* exposed logs
* AI data leakage
* notification leakage

Controls:

* Authorization
* redaction
* short-lived URLs
* safe errors
* logging rules
* data minimization
* encryption

## 120. Denial-of-Service Threats

Examples:

* Login flooding
* search abuse
* file-upload abuse
* webhook flooding
* AI request flooding
* export abuse
* expensive reports

Controls:

* Rate limiting
* quotas
* request-size limits
* queues
* timeouts
* circuit breakers
* monitoring

## 121. Elevation-of-Privilege Threats

Examples:

* Self-assigning administrator
* exploiting support access
* reusing expired role
* stale permission cache
* manipulating organization context
* accessing platform routes

Controls:

* Server-side checks
* approval
* role constraints
* cache invalidation
* session revalidation
* privileged monitoring

## 122. Abuse Case — Cross-Tenant Payment Access

```text
Actor:
Malicious organization user

Goal:
View or alter another organization’s payment.

Method:
Change payment identifier in the URL or API request.

Controls:
Organization validation
Permission validation
Resource ownership check
Database isolation
Safe response
Audit and monitoring
```

## 123. Abuse Case — Forged Payment Screenshot

```text
Actor:
Applicant or Resident

Goal:
Cause a charge to appear settled without valid payment.

Method:
Upload edited or reused payment evidence.

Controls:
Evidence remains unverified
Manual or provider verification
Duplicate detection
Reference validation
Allocation only after verification
Audit
```

## 124. Abuse Case — Double Allocation

```text
Actor:
Concurrent users or automated retry

Goal:
Allocate the same payment value more than once.

Method:
Submit simultaneous allocation requests.

Controls:
Database transaction
Locking or versioning
Idempotency
Balance validation
Unique constraints
Conflict response
```

## 125. Abuse Case — Agreement Replacement

```text
Actor:
Malicious organization member

Goal:
Change agreement terms after signature.

Method:
Replace the signed document or edit the issued version.

Controls:
Immutable versions
Checksums
Restricted storage
New amendment requirement
Audit
Signature evidence
```

## 126. Abuse Case — Vendor Overreach

```text
Actor:
Vendor Worker

Goal:
Access unrelated resident or financial data.

Method:
Navigate directly to other records.

Controls:
Assignment scope
Minimal response schemas
Field restrictions
Direct-access denial
Assignment-expiry enforcement
```

## 127. Abuse Case — Support Access Misuse

```text
Actor:
Platform Support Agent

Goal:
Browse customer data without a valid support reason.

Method:
Use platform privileges outside an approved support case.

Controls:
Temporary scoped support sessions
Approval
Purpose
Expiry
Audit
Customer visibility
Security monitoring
```

## 128. Abuse Case — Malicious AI Prompt

```text
Actor:
Applicant, resident, vendor, or external content source

Goal:
Cause AI to reveal data or influence a decision.

Method:
Embed instructions in text or uploaded documents.

Controls:
Treat content as untrusted
Prompt isolation
Data minimization
Output review
No autonomous decisions
Tenant-scoped retrieval
Monitoring
```

## 129. Security Testing Strategy

Security testing should include:

* Authentication tests
* session tests
* authorization tests
* tenant-isolation tests
* field-level tests
* privilege-escalation tests
* financial-integrity tests
* agreement-integrity tests
* occupancy-integrity tests
* file-upload tests
* webhook tests
* AI tests
* rate-limit tests
* dependency scanning
* static analysis
* dynamic testing
* penetration testing

## 130. Authentication Tests

Tests should cover:

* Valid login
* invalid login
* enumeration resistance
* lockout or throttling
* password reset
* expired reset token
* session revocation
* MFA
* session rotation
* suspended account

## 131. Tenant-Isolation Tests

Every organization-owned endpoint must test:

* Same-organization access
* cross-organization denial
* missing membership
* suspended membership
* expired assignment
* invalid organization relationship
* stale role
* support-session limits
* platform-role limits

## 132. Authorization Tests

Tests should cover:

* Correct permission
* missing permission
* read-only role
* field-level restriction
* self-approval prohibition
* state-dependent denial
* revoked assignment
* inherited scope
* direct URL access

## 133. Financial Security Tests

Tests should cover:

* Forged evidence
* duplicate reference
* duplicate webhook
* over-allocation
* concurrent allocation
* currency mismatch
* unauthorized reversal
* duplicate refund
* stale balance
* receipt integrity

## 134. Agreement Security Tests

Tests should cover:

* Issued-version mutation
* signed-document replacement
* wrong signer
* expired signature request
* duplicate signature callback
* activation without signature
* version mismatch
* unauthorized amendment
* unauthorized termination

## 135. Occupancy Security Tests

Tests should cover:

* Conflicting occupancy
* activation without agreement
* activation without payment
* unauthorized household change
* vendor access
* cross-tenant resident data
* move-out completion without review

## 136. File Security Tests

Tests should cover:

* Disallowed file type
* spoofed MIME type
* oversized file
* malicious filename
* malware
* scan failure
* direct object access
* expired signed URL
* unauthorized document
* replacement history

## 137. Webhook Security Tests

Tests should cover:

* Valid signature
* invalid signature
* expired timestamp
* replay
* duplicate event
* wrong provider account
* wrong environment
* unknown event
* oversized payload
* processing failure

## 138. AI Security Tests

Tests should cover:

* Prompt injection
* cross-tenant retrieval
* restricted-data redaction
* hallucinated output
* unsafe generated communication
* provider failure
* no autonomous status change
* source-record availability
* output labeling

## 139. Security Acceptance Criterion — Cross-Tenant Access

```text
Given:
User A belongs only to Organization A.
Agreement B belongs to Organization B.

When:
User A requests Agreement B directly.

Then:
The request is denied.
No Agreement B metadata is returned.
No document URL is generated.
The attempt is logged where appropriate.
```

## 140. Security Acceptance Criterion — Revoked Membership

```text
Given:
A user has an active session.
The user’s organization membership is revoked.

When:
The user performs a protected organization action.

Then:
The request is denied.
Cached permissions do not preserve access.
The session is revalidated or restricted.
```

## 141. Security Acceptance Criterion — Forged Payment Evidence

```text
Given:
An applicant uploads payment evidence.

When:
The upload succeeds.

Then:
The payment remains Pending verification.
The charge remains outstanding.
No agreement or occupancy activates solely from the upload.
```

## 142. Security Acceptance Criterion — Concurrent Allocation

```text
Given:
A verified payment has ₦400,000 available.

When:
Two reviewers simultaneously attempt to allocate ₦300,000 each.

Then:
At most ₦400,000 is allocated.
One request succeeds fully or partially according to approved rules.
The conflicting request receives a safe conflict response.
```

## 143. Security Acceptance Criterion — Agreement Integrity

```text
Given:
Agreement Version 2 has been signed.

When:
A user attempts to modify the signed version.

Then:
The change is rejected.
Version 2 remains unchanged.
A new amendment or version is required.
```

## 144. Security Acceptance Criterion — Vendor Privacy

```text
Given:
Vendor Worker A is assigned to Work Order 001.

When:
Vendor Worker A requests Application 001 or Payment 001.

Then:
Access is denied.
No applicant or financial information is returned.
```

## 145. Security Acceptance Criterion — Webhook Replay

```text
Given:
A valid provider event was processed previously.

When:
The identical event is received again.

Then:
The event is recognized as a duplicate.
No duplicate business action is created.
The endpoint responds safely.
```

## 146. Security Acceptance Criterion — AI Prompt Injection

```text
Given:
An uploaded document contains instructions asking the AI to reveal confidential data.

When:
An AI summary is generated.

Then:
The embedded instructions are treated as document content.
No unrelated tenant data is retrieved.
No protected action is performed.
The output remains subject to human review.
```

## 147. Security Acceptance Criterion — Support Session

```text
Given:
A support agent has no active support session for Organization A.

When:
The support agent attempts to access Organization A customer records.

Then:
The request is denied.

When:
A valid approved scoped support session is activated.

Then:
Only the approved scope is available until expiry.
All access is audited.
```

## 148. Security Metrics

Potential metrics include:

* Failed logins
* MFA adoption
* session revocations
* cross-tenant denials
* authorization failures
* privileged-role changes
* sensitive-document access
* webhook signature failures
* duplicate webhook rate
* malicious-file detections
* security-alert response time
* vulnerability-remediation time
* support-session frequency
* emergency-access frequency

Metrics must avoid exposing restricted data.

## 149. Security Logging Retention

Security-log retention should reflect:

* Investigation needs
* legal requirements
* privacy
* storage cost
* sensitivity
* audit obligations

Final periods remain pending policy and legal review.

## 150. Security Incident Evidence

Incident evidence may include:

* Audit events
* access logs
* provider events
* request metadata
* session history
* role changes
* file hashes
* deployment history
* configuration changes

Evidence must be protected from tampering.

## 151. MVP Security Scope

The MVP must include:

### Identity

* Secure authentication
* password reset
* session revocation
* MFA for privileged users
* account status
* audit

### Tenant Isolation

* Organization-scoped data
* membership validation
* permission validation
* assignment validation
* cross-tenant tests

### Application Security

* Input validation
* output encoding
* CSRF controls
* CORS controls
* rate limiting
* request-size limits
* safe errors

### Data Security

* Encryption in transit
* encryption at rest
* private storage
* signed URLs
* restricted-field handling
* backups

### Financial Security

* Payment-verification distinction
* duplicate protection
* transactional allocation
* reversal and refund controls
* immutable history

### Agreement Security

* Version integrity
* signed-document preservation
* signature verification
* controlled amendments
* activation checks

### Operational Security

* Vendor assignment scope
* support-session controls
* audit logs
* security monitoring
* incident procedures

### Integration Security

* Webhook signatures
* event idempotency
* provider-secret management
* provider failure handling

### AI Security

* Data minimization
* prompt isolation
* tenant isolation
* human review
* no autonomous high-impact action

## 152. Deferred Security Capabilities

The following may be deferred:

* Hardware security keys for all users
* full customer-managed encryption keys
* advanced user-behavior analytics
* dedicated security operations center
* continuous external penetration testing
* full zero-trust network segmentation
* enterprise SSO
* SCIM provisioning
* device attestation
* biometric authentication
* advanced fraud scoring
* automated applicant identity verification
* advanced data-loss prevention
* customer-managed retention policies
* immutable external audit ledger
* bug-bounty program
* full regional data residency
* multi-region active-active architecture

Deferred controls must not weaken MVP fundamentals.

## 153. Security Risks

### 153.1 Cross-Tenant Exposure

Risk:

A user accesses another organization’s data.

Controls:

* Organization validation
* database controls
* authorization
* tests
* monitoring

### 153.2 Privileged Insider Misuse

Risk:

An administrator or support user abuses access.

Controls:

* Least privilege
* separation of duties
* support sessions
* MFA
* audit
* monitoring

### 153.3 Financial Fraud

Risk:

Fraudulent evidence or duplicate transactions create false payment status.

Controls:

* Verification
* duplicate detection
* idempotency
* transactional allocation
* audit

### 153.4 Document Malware

Risk:

Uploaded files compromise users or infrastructure.

Controls:

* Type restrictions
* malware scanning
* isolated storage
* safe delivery
* scan-before-access

### 153.5 Provider Compromise

Risk:

A provider credential or webhook is compromised.

Controls:

* Secret rotation
* signature validation
* account validation
* domain-state checks
* monitoring
* circuit breakers

### 153.6 AI Data Leakage

Risk:

Sensitive data is exposed through AI prompts, context, logs, or outputs.

Controls:

* Data minimization
* redaction
* tenant isolation
* provider controls
* human review

### 153.7 Authorization Drift

Risk:

Permissions change but active sessions or caches retain access.

Controls:

* Cache invalidation
* session revalidation
* short-lived claims
* live sensitive-command checks

### 153.8 Security Complexity

Risk:

Controls become too complex to operate correctly.

Controls:

* Clear architecture
* shared authorization services
* automated tests
* documentation
* phased implementation

## 154. Residual Risks

Even after controls, residual risks may include:

* Compromised user devices
* sophisticated social engineering
* provider-side breach
* unknown software vulnerabilities
* insider collusion
* fraudulent but convincing documents
* service outages
* incorrect configuration
* human approval error

Residual risks must be documented, monitored, and reviewed.

## 155. Unresolved Security Decisions

The following remain pending:

* Final authentication provider
* Final passwordless strategy
* Final MFA methods
* Final session duration
* Final inactivity timeout
* Final step-up authentication rules
* Final row-level-security implementation
* Final encryption-key provider
* Final object-storage provider
* Final malware-scanning provider
* Final webhook secret-rotation policy
* Final rate-limit provider
* Final bot-protection provider
* Final content-security policy
* Final WAF decision
* Final audit retention
* Final security-log retention
* Final backup retention
* Final data-deletion process
* Final incident-notification process
* Final vulnerability-remediation SLAs
* Final dependency-scanning tools
* Final SAST tools
* Final DAST tools
* Final penetration-test schedule
* Final support-session approval model
* Final emergency-access approval model
* Final financial approval thresholds
* Final sensitive-export approval rules
* Final government-ID retention rules
* Final AI provider
* Final AI retention configuration
* Final AI redaction approach
* Final provider data-processing agreements
* Final security contact
* Final responsible-disclosure program
* Final production-access model
* Final CI/CD identity model
* Final secret-management platform
* Final multi-region strategy
* Final compliance roadmap
* Final pilot public names
* Final Block B inventory
* Final second-building structure
* Final water-feature classification

These decisions must be resolved before the affected capability becomes production dependent.

## 156. Security Governance

Changes to approved security architecture require:

1. Change description
2. Business rationale
3. Affected asset
4. Threat addressed
5. Trust-boundary impact
6. Authentication impact
7. Authorization impact
8. Tenant-isolation impact
9. Data-classification impact
10. Financial impact
11. Agreement impact
12. Occupancy impact
13. Privacy impact
14. Document impact
15. Provider impact
16. AI impact
17. Monitoring impact
18. Incident-response impact
19. Testing impact
20. Migration impact
21. Residual risk
22. Rollback or recovery plan
23. Product Visionary approval
24. Chief Product Architect approval
25. Security review where required
26. Effective version

## 157. Security Review Triggers

A dedicated security review is required when introducing:

* New authentication method
* New privileged role
* New external provider
* New payment capability
* New signature provider
* New restricted data type
* New AI use case
* New public API
* New file type
* New support-access capability
* New export capability
* New mobile application
* New tenant-sharing model
* New data region
* New financial approval flow

## 158. Current Security Architecture Decision

Habitacores will use a zero-trust-informed, defense-in-depth security architecture organized around:

```text
Untrusted Request
→ Authentication
→ Session Validation
→ Organization Context
→ Membership
→ Permission
→ Scope
→ Assignment
→ Record State
→ Separation of Duties
→ Domain Validation
→ Transaction
→ Audit
→ Monitoring
```

The architecture must preserve:

* Identity security
* organization isolation
* least privilege
* server-side authorization
* assignment scope
* field-level access
* financial integrity
* payment-verification distinction
* transactional allocation
* agreement immutability
* signature integrity
* occupancy integrity
* resident privacy
* secure documents
* verified webhooks
* provider isolation
* AI data minimization
* human authority
* support-access control
* auditability
* incident detection
* recovery
* future SaaS extensibility

No authenticated session, organization identifier, role label, hidden control, direct URL, uploaded file, payment screenshot, provider callback, AI output, cached permission, client-generated status, support relationship, pilot-team relationship, or external integration may independently authorize, verify, approve, allocate, sign, activate, settle, refund, reverse, terminate, reveal, or otherwise finalize a protected Habitacores business action.