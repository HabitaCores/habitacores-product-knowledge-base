# Document 20 — Integration Architecture and External Systems Strategy

## Document Control

| Field | Value |
|---|---|
| Product | Habitacores |
| Document | Integration Architecture and External Systems Strategy |
| Version | 0.1 |
| Status | Draft for approval |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Market | Nigeria |
| Primary Currency | Nigerian naira |
| Primary Operational Time Zone | West Africa Time |
| Primary Tenant Boundary | Organization |
| Primary Experience | Responsive web application |
| Proposed Application Framework | Next.js and TypeScript |
| Proposed System of Record | PostgreSQL |
| Integration Model | Adapter-based, asynchronous where appropriate, domain-controlled |
| Trust Model | Zero-trust-informed and least privilege |
| Last Updated | 30 July 2026 |

## 1. Purpose

This document defines the integration architecture and external-systems strategy for Habitacores.

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

into a controlled model for connecting Habitacores with external systems.

This document defines:

- Integration objectives
- Integration principles
- Integration boundaries
- Provider categories
- Adapter architecture
- Domain ownership
- Synchronous integrations
- Asynchronous integrations
- Webhooks
- Polling
- Reconciliation
- Provider credentials
- Provider account mapping
- Payment integrations
- Bank-transfer evidence
- Electronic-signature integrations
- Email integrations
- SMS integrations
- WhatsApp integrations
- Identity-verification integrations
- File-scanning integrations
- AI integrations
- Analytics integrations
- Mapping and geolocation integrations
- Document-generation integrations
- Calendar integrations
- Accounting and finance integrations
- Government, legal, and regulatory integrations
- Partner APIs
- Import and export
- Integration events
- Retry and idempotency
- Rate limiting
- Data mapping
- Schema versioning
- Provider failure handling
- Monitoring
- Security
- Privacy
- Testing
- Operational support
- Integration acceptance criteria
- MVP integration scope
- Deferred capabilities
- Risks
- Unresolved decisions
- Governance

This document does not approve any specific provider unless explicitly stated through an approved Architecture Decision Record.

## 2. Integration Objectives

Habitacores integrations must:

1. Extend the product without surrendering domain authority.
2. Preserve organization-level tenant isolation.
3. Protect financial, agreement, occupancy, and document integrity.
4. Prevent provider callbacks from bypassing business rules.
5. Support reliable payment verification and reconciliation.
6. Support secure agreement-signature workflows.
7. Support dependable user communications.
8. Support secure file and identity processing.
9. Support AI-assisted workflows without autonomous high-impact decisions.
10. Support provider replacement.
11. Support multiple provider accounts.
12. Support environment isolation.
13. Support webhook authentication and idempotency.
14. Support retries without duplicate business actions.
15. Support reconciliation after provider failure.
16. Support monitoring and operational troubleshooting.
17. Minimize sensitive-data transfer.
18. Preserve auditability.
19. Support future partner APIs.
20. Avoid provider-specific logic inside core domain modules.
21. Avoid pilot-specific provider assumptions.
22. Support Nigerian payment and communication realities.
23. Support future geographic expansion.
24. Support low-bandwidth workflows.
25. Fail safely.

## 3. Integration Principles

### 3.1 Habitacores Owns Domain State

External providers may report facts or statuses.

Habitacores determines whether those facts satisfy approved business rules.

Examples:

- A payment provider may report a successful transaction.
- Habitacores decides whether the payment maps to the correct payer, organization, charge, amount, and currency.
- A signature provider may report completion.
- Habitacores decides whether the signer, agreement version, and activation prerequisites are valid.

### 3.2 Providers Are Untrusted Boundaries

Provider requests, callbacks, files, and status values must be authenticated, validated, mapped, and audited.

### 3.3 Adapter-Based Integration

Each provider should be accessed through an internal adapter or gateway.

Core domain logic should depend on Habitacores contracts rather than provider-specific SDKs.

### 3.4 Asynchronous Where Appropriate

Long-running, retryable, provider-dependent, or high-latency operations should use asynchronous processing where practical.

### 3.5 Idempotency

Repeated requests, retries, or duplicate callbacks must not create duplicate protected actions.

### 3.6 Reconciliation

Critical integrations must support later comparison between:

- Habitacores records
- provider records
- bank or financial records
- signed-document records
- message-delivery records

### 3.7 Least Data

Only the minimum necessary data should be sent to a provider.

### 3.8 Environment Isolation

Production integrations must use production credentials and endpoints only.

Non-production environments must use sandbox, simulator, or isolated accounts.

### 3.9 Provider Replaceability

Provider-specific differences should be contained in adapters, mapping rules, configuration, and ADRs.

### 3.10 Observable Integrations

Every critical integration must expose:

- Request status
- response category
- provider reference
- latency
- retry history
- failure reason
- correlation identifier
- reconciliation state

## 4. Integration Boundary Model

```text
Habitacores Client
→ Habitacores API
→ Domain Service
→ Integration Port
→ Provider Adapter
→ External Provider
→ Provider Callback or Polling
→ Integration Event Store
→ Domain Validation
→ Authoritative State Transition
→ Audit
````

## 5. Integration Categories

Habitacores may integrate with:

* Payment providers
* Banking and reconciliation systems
* Electronic-signature providers
* Email providers
* SMS providers
* WhatsApp providers
* Identity-verification providers
* File-scanning providers
* AI providers
* Analytics providers
* Maps and geolocation providers
* Document-generation providers
* Calendar providers
* Accounting systems
* Customer-support systems
* Government and regulatory systems
* Legal-information systems
* Property-data providers
* Partner platforms
* Future mobile-notification providers

## 6. Integration Ownership

Each integration must have:

* Business owner
* technical owner
* security owner where required
* provider relationship owner
* configuration owner
* operational owner
* escalation contact
* renewal or commercial owner
* decommissioning owner

## 7. Integration Registry

Habitacores should maintain an integration registry containing:

* Integration name
* provider
* purpose
* environment
* owner
* status
* data exchanged
* data classification
* credentials
* endpoints
* webhook URLs
* rate limits
* retention
* monitoring
* incident contact
* contract or agreement
* ADR reference
* review date

## 8. Integration Status

Suggested statuses:

* Proposed
* under evaluation
* sandbox
* approved
* active
* degraded
* suspended
* deprecated
* retired

## 9. Integration Port

An integration port defines the provider-neutral capability expected by Habitacores.

Example payment port:

```typescript
interface PaymentGateway {
  createPaymentRequest(input: CreatePaymentRequest): Promise<PaymentRequestResult>;
  verifyTransaction(input: VerifyTransactionInput): Promise<ProviderTransactionResult>;
  requestRefund(input: RefundRequest): Promise<RefundResult>;
  fetchTransaction(input: FetchTransactionInput): Promise<ProviderTransactionResult>;
}
```

The domain should not depend directly on provider-specific response objects.

## 10. Provider Adapter

A provider adapter is responsible for:

* Authentication
* request construction
* provider-specific fields
* provider API calls
* response parsing
* error mapping
* webhook validation
* status normalization
* retry classification
* safe logging

## 11. Provider-Neutral Domain Models

Provider-neutral models should include:

* Internal transaction identifier
* provider identifier
* provider account
* organization
* amount
* currency
* status
* timestamps
* payer or signer reference
* raw-event reference
* normalized result
* reconciliation status

## 12. Anti-Corruption Layer

Provider terminology must not automatically become Habitacores domain terminology.

Example:

A provider status called `successful` may map to:

```text
Provider transaction confirmed
```

It does not automatically map to:

```text
Habitacores payment verified and allocated
```

## 13. Synchronous Integration

Synchronous integration may be used when:

* Immediate response is required.
* provider latency is bounded.
* failure can be communicated safely.
* no long-running work is expected.
* retries can be controlled.

Examples:

* Creating a provider checkout request
* validating an address
* requesting a signed upload URL
* fetching a provider status for review

## 14. Asynchronous Integration

Asynchronous integration should be used for:

* Webhook processing
* file scanning
* AI generation
* bulk communication
* reconciliation
* report generation
* import
* export
* delayed provider confirmation
* signature completion processing

## 15. Integration Command Flow

```text
User or System Command
→ Authenticate
→ Authorize
→ Validate Domain Prerequisites
→ Create Internal Pending Record
→ Call Provider or Queue Work
→ Record Provider Reference
→ Await Response or Callback
→ Validate Provider Result
→ Apply Domain Rules
→ Update Internal State
→ Audit
→ Notify
```

## 16. Integration Event Store

Habitacores should persist material provider events.

An integration event record may include:

* Internal event ID
* provider
* provider account
* environment
* provider event ID
* event type
* received time
* signature status
* processing status
* organization mapping
* related resource
* attempt count
* error code
* processed time
* raw-payload location or hash
* retention classification

## 17. Raw Provider Payloads

Raw provider payloads may be retained where required for:

* Signature validation
* dispute investigation
* reconciliation
* provider support
* audit

Raw payload access must be restricted.

Sensitive payloads should not be copied into ordinary logs.

## 18. Provider Event Status

Suggested statuses:

* Received
* authenticated
* rejected
* duplicate
* mapped
* processing
* processed
* failed
* awaiting review
* ignored

## 19. Webhook Architecture

Preferred flow:

```text
Provider
→ Public webhook endpoint
→ Payload-size validation
→ Signature validation
→ Timestamp and replay validation
→ Provider-account validation
→ Event persistence
→ Immediate acknowledgment
→ Asynchronous processing
→ Domain validation
→ Authoritative state transition
→ Audit and monitoring
```

## 20. Webhook Authentication

Webhook authentication may use:

* HMAC signature
* asymmetric signature
* signed JWT
* mutual TLS
* provider-specific verification protocol
* source-network restriction as a supplementary control

IP allowlisting alone is insufficient.

## 21. Webhook Replay Protection

Replay controls should include:

* Provider event ID
* signature timestamp
* replay window
* unique constraints
* processing history
* idempotent domain commands

## 22. Webhook Acknowledgment

Webhook endpoints should acknowledge valid receipt quickly.

Long-running business processing should occur asynchronously.

## 23. Duplicate Webhook Handling

Duplicate callbacks must:

* Be recognized
* avoid duplicate domain actions
* preserve receipt history
* return a safe response
* remain visible for monitoring

## 24. Out-of-Order Events

Provider events may arrive out of order.

Processing must evaluate:

* Event time
* provider sequence where available
* current internal state
* version
* terminal-state rules
* reconciliation need

An older event must not overwrite a newer authoritative state improperly.

## 25. Unsupported Provider Events

Unsupported events should be:

* Authenticated
* recorded safely
* ignored or routed for review
* monitored
* not treated as successful business commands

## 26. Polling

Polling may be used when:

* The provider does not support webhooks.
* webhook delivery is uncertain.
* reconciliation requires status confirmation.
* manual refresh is needed.

Polling must respect provider rate limits.

## 27. Reconciliation

Reconciliation compares Habitacores and provider records.

Reconciliation may identify:

* Missing provider event
* missing internal record
* amount mismatch
* currency mismatch
* status mismatch
* duplicate transaction
* duplicate refund
* unmatched signature
* failed message delivery
* unprocessed callback

## 28. Reconciliation Status

Suggested statuses:

* Not required
* pending
* matched
* mismatch
* manually resolved
* provider investigation
* internal correction required
* closed

## 29. Reconciliation Ownership

Reconciliation may be owned by:

* Finance operations
* technical operations
* legal operations
* support
* provider integration owner

Ownership depends on integration type.

## 30. Provider Credentials

Provider credentials may include:

* API key
* secret key
* public key
* signing key
* webhook secret
* OAuth client
* refresh token
* service account
* certificate

## 31. Credential Storage

Credentials must be:

* Server side
* environment specific
* secret managed
* access controlled
* rotated
* audited
* excluded from source control
* excluded from client bundles
* excluded from logs

## 32. Credential Scope

Credentials should be scoped by:

* Environment
* provider account
* organization where supported
* capability
* read versus write
* webhook verification
* refund authority

## 33. Provider Account Mapping

Habitacores may support:

* One platform-owned provider account
* multiple platform-owned accounts
* organization-owned provider accounts
* hybrid provider accounts

The selected model must be documented per provider.

## 34. Organization-Owned Provider Accounts

Where organizations connect their own provider accounts, Habitacores must define:

* Authorization flow
* token ownership
* permissions
* revocation
* organization mapping
* settlement ownership
* support responsibility
* data access
* deconnection behavior

## 35. Platform-Owned Provider Accounts

Where Habitacores operates the provider account, it must define:

* Merchant-of-record implications
* settlement
* reconciliation
* fees
* refunds
* disputes
* tax implications
* organization allocation
* regulatory obligations

These require legal and financial review.

## 36. OAuth Integration

OAuth integrations should use:

* Authorization code flow
* PKCE where appropriate
* state validation
* exact redirect URIs
* encrypted token storage
* refresh-token rotation where supported
* token revocation
* least scopes

## 37. OAuth State

OAuth state must bind:

* User
* organization
* environment
* requested provider
* intended redirect
* expiration
* nonce

## 38. OAuth Token Handling

OAuth tokens must not be exposed to:

* Browser logs
* analytics
* unauthorized support personnel
* unrelated organizations
* source control

## 39. Rate Limits

Habitacores must track provider rate limits.

Controls may include:

* Client-side throttling where useful
* server-side throttling
* queues
* exponential backoff
* jitter
* request coalescing
* caching
* provider quota alerts

## 40. Retry Classification

Provider failures should be classified as:

* Retryable transient
* rate limited
* provider unavailable
* authentication failure
* validation failure
* permission failure
* permanent business rejection
* duplicate
* unknown

## 41. Retry Strategy

Retries should use:

* Bounded attempts
* exponential backoff
* jitter
* idempotency key
* visible failure state
* dead-letter handling
* operational alert where required

## 42. Timeout Strategy

Every provider call must have a timeout.

Timeouts should be appropriate to:

* Provider
* operation
* user experience
* retry behavior
* business impact

## 43. Circuit Breaker

A circuit breaker may temporarily stop calls to a failing provider.

States may include:

* Closed
* open
* half-open

A circuit breaker must not cause false success.

## 44. Provider Fallback

Fallback may include:

* Secondary provider
* manual process
* queued retry
* alternative communication channel
* in-app notification
* provider-independent operation

Fallback must be explicitly approved.

## 45. Payment Integration Objectives

Payment integration must support:

* Annual rent
* deposits
* service charges
* application fees where approved
* maintenance charges
* refunds
* reversals
* receipts
* reconciliation

## 46. Payment Integration Boundary

The payment provider may:

* Create checkout or transfer references
* report transaction status
* process a refund
* provide reconciliation data

Habitacores remains authoritative for:

* Charge
* payment verification
* allocation
* balance
* receipt
* agreement prerequisite
* occupancy prerequisite

## 47. Payment Initiation

Payment initiation should validate:

* User
* organization
* charge
* amount
* currency
* payer
* provider account
* idempotency
* return URL
* callback configuration

## 48. Bank Transfer

Bank-transfer support may include:

* Dedicated virtual account
* payment reference
* bank statement reconciliation
* uploaded evidence
* manual review
* provider confirmation

Uploaded evidence alone does not prove payment.

## 49. Virtual Accounts

Virtual-account integration may support:

* Permanent account per organization
* permanent account per resident
* temporary account per charge
* temporary account per transaction

The chosen model must preserve reconciliation accuracy.

## 50. Payment Status Mapping

Provider statuses should map to internal provider-transaction states such as:

* Initiated
* pending
* confirmed
* failed
* cancelled
* reversed
* refunded
* unknown

Internal payment verification remains separate.

## 51. Payment Verification

Payment verification must evaluate:

* Provider authenticity
* provider reference
* amount
* currency
* recipient account
* payer context
* date
* duplicate status
* organization
* charge or intended purpose
* reviewer authority where manual review applies

## 52. Payment Allocation

Provider confirmation must not directly allocate funds.

Allocation occurs through Habitacores financial-domain rules.

## 53. Payment Refund

Refund integration must validate:

* Source payment
* refundable balance
* recipient
* approval
* provider account
* idempotency
* provider response
* execution status
* reconciliation

## 54. Payment Reversal

Provider reversal must create explicit internal reversal records.

Original payment history must remain preserved.

## 55. Payment Disputes

Future dispute support may include:

* Chargeback notice
* evidence submission
* status tracking
* reserve impact
* provider communication
* financial correction
* audit

## 56. Payment Reconciliation

Payment reconciliation may compare:

* Provider transactions
* bank settlements
* provider fees
* internal payments
* allocations
* refunds
* reversals
* receipts

## 57. Payment Provider Selection Criteria

Criteria should include:

* Nigerian naira support
* bank-transfer capability
* settlement reliability
* reconciliation data
* webhook security
* refund support
* virtual accounts
* fees
* API quality
* sandbox quality
* support
* regulatory standing
* uptime
* dispute handling

## 58. Electronic-Signature Objectives

Signature integrations must support:

* Agreement version integrity
* correct signer
* signing order
* signature request
* reminder
* decline
* completion
* signed-document retrieval
* evidence
* audit

## 59. Signature Request

A signature request should contain:

* Agreement version
* signer identity
* signer role
* signing order
* expiration
* callback metadata
* redirect URL
* authentication method
* provider account

## 60. Signature Provider Callback

A completion callback must be validated against:

* Provider signature
* provider account
* request ID
* agreement version
* signer
* status
* final document
* checksum
* environment

## 61. Signed Document

The final signed document should be:

* Retrieved securely
* checksum validated
* stored privately
* linked to the correct agreement version
* immutable
* auditable

## 62. Signature Evidence

Evidence may include:

* Provider completion certificate
* signer timestamps
* authentication method
* IP or device metadata where lawful
* document hash
* request identifier

## 63. Signature Failure

Signature failure must not:

* Mark the agreement signed
* activate the agreement
* activate occupancy
* remove the unsigned version
* conceal provider failure

## 64. Signature Provider Selection Criteria

Criteria should include:

* Legal suitability
* evidence quality
* signer authentication
* document integrity
* Nigerian accessibility
* mobile usability
* API quality
* webhook security
* document retention
* pricing
* provider support

## 65. Email Integration

Email may support:

* Account verification
* password recovery
* application notifications
* payment notifications
* agreement notifications
* maintenance updates
* support communication
* security alerts

## 66. Email Architecture

Preferred flow:

```text
Domain event
→ Notification policy
→ Template rendering
→ Recipient validation
→ Queue
→ Email provider
→ Delivery event
→ Notification record
```

## 67. Email Security

Email controls should include:

* SPF
* DKIM
* DMARC
* approved sender domains
* template escaping
* recipient validation
* unsubscribe where applicable
* bounce handling
* suppression handling
* minimal sensitive content

## 68. Email Delivery Status

Suggested statuses:

* Queued
* sent
* delivered
* delayed
* bounced
* rejected
* failed
* suppressed

## 69. Email Content

Email should generally contain:

* Brief event summary
* secure link
* action deadline
* support reference

Restricted documents should not normally be attached directly.

## 70. SMS Integration

SMS may support:

* Verification codes
* urgent reminders
* inspection reminders
* payment reminders
* maintenance updates
* security alerts

## 71. SMS Constraints

SMS has:

* Limited length
* delivery uncertainty
* privacy risks on shared devices
* provider variations
* cost
* sender-ID limitations

Sensitive content must be minimized.

## 72. WhatsApp Integration

WhatsApp may support:

* Transactional notifications
* approved reminders
* support interaction
* inspection communication
* maintenance updates

Use must comply with provider template and consent requirements.

## 73. Communication Channel Preference

Users may have preferences for:

* In-app
* email
* SMS
* WhatsApp
* push notification later

Critical security notices may override ordinary marketing preferences where lawful.

## 74. Communication Deduplication

The notification system must avoid repeated identical messages from:

* Duplicate events
* retries
* provider callbacks
* repeated scheduled jobs

## 75. Communication Provider Selection Criteria

Criteria should include:

* Nigerian delivery quality
* sender support
* template support
* webhook status
* API reliability
* pricing
* support
* consent controls
* compliance
* rate limits

## 76. Identity Verification

Identity verification may support:

* Government identification
* identity-document validation
* facial matching where lawful
* liveness where lawful
* address verification
* phone verification
* bank-account-name verification

## 77. Identity Verification Boundary

Identity-provider output is evidence.

Habitacores determines:

* Whether verification is required
* whether the result is sufficient
* who may view it
* whether human review is needed
* how long data is retained

## 78. Identity Data Minimization

Identity integrations should minimize:

* Full document retention
* biometric data
* raw provider payload
* repeated verification
* unnecessary cross-border transfer

## 79. Identity Verification Status

Suggested statuses:

* Not started
* pending
* verified
* partially verified
* failed
* expired
* manual review
* unavailable

## 80. Identity Provider Selection Criteria

Criteria should include:

* Nigerian document support
* verification accuracy
* fraud controls
* privacy
* retention
* API quality
* audit evidence
* pricing
* human review
* regulatory position

## 81. File-Scanning Integration

File-scanning integration should support:

* Malware scan
* file-type verification
* scan result
* threat classification
* provider reference
* retry
* quarantine
* manual review

## 82. File-Scanning Boundary

A scan result of safe means:

* No known threat was detected according to the provider.

It does not prove:

* Document authenticity
* payment authenticity
* identity authenticity
* legal validity

## 83. File-Scan Failure

When scanning fails or times out:

* The file remains unavailable or restricted.
* The business workflow remains pending.
* no automatic verification occurs.
* operations receive visibility.

## 84. AI Integration Objectives

AI may assist with:

* Summaries
* classification
* drafting
* search assistance
* anomaly orientation
* operational support
* document extraction
* workflow guidance

## 85. AI Integration Boundary

AI must not independently:

* Approve or reject an application
* verify a payment
* allocate funds
* approve a refund
* activate an agreement
* activate occupancy
* terminate an agreement
* issue legal conclusions
* expose restricted data

## 86. AI Provider Gateway

AI calls should pass through a controlled internal gateway that handles:

* Provider selection
* approved model
* prompt template
* redaction
* organization scope
* token limits
* timeout
* retry
* structured output
* content safety
* cost
* audit metadata

## 87. AI Retrieval

Retrieval-augmented AI must enforce:

* Organization filtering
* user permission
* resource scope
* field restrictions
* document sensitivity
* source traceability

## 88. AI Prompt Construction

Prompts should separate:

* System instructions
* application policy
* task instruction
* trusted metadata
* untrusted user content
* untrusted document content

## 89. AI Output Validation

Structured AI output should be validated against:

* Schema
* required fields
* allowed enums
* length limits
* prohibited actions
* source references
* tenant context

## 90. AI Human Review

AI output used in material workflows must remain:

* Visible
* labeled
* editable
* reviewable
* attributable to source
* non-authoritative

## 91. AI Provider Selection Criteria

Criteria should include:

* Security
* privacy
* retention controls
* data-use policy
* regional processing
* model quality
* structured output
* rate limits
* latency
* cost
* availability
* content safety
* contractual terms

## 92. Analytics Integration

Analytics may support:

* Product usage
* workflow completion
* usability
* feature adoption
* performance
* operational trends

Analytics must not become an uncontrolled data replica.

## 93. Analytics Restrictions

Analytics should avoid collecting:

* Full application content
* government IDs
* bank evidence
* signed agreements
* legal notes
* passwords
* tokens
* unrestricted resident details
* restricted support content

## 94. Analytics Identity

Analytics may use:

* Anonymous session ID
* pseudonymous user ID
* organization ID where approved
* role category
* feature event

Direct personal identifiers should be minimized.

## 95. Maps and Geolocation

Maps may support:

* Property location
* inspection navigation
* nearby amenities
* approximate public location
* maintenance routing

## 96. Public Location Privacy

Public listings may show:

* Approximate location
* neighborhood
* estate
* approved map area

Exact property coordinates may be restricted until appropriate.

## 97. Geocoding

Geocoding output should be reviewed for:

* Accuracy
* address normalization
* privacy
* provider terms
* caching restrictions

## 98. Document Generation

Document-generation integrations may support:

* Receipts
* agreement PDFs
* notices
* inspection forms
* condition reports
* operational reports

Generated documents must derive from authoritative data.

## 99. Document Generation Integrity

Generated documents should include:

* Stable identifier
* version
* generation time
* source record
* checksum
* template version
* organization
* authorized access

## 100. Calendar Integration

Calendar integrations may support:

* Inspections
* move-ins
* move-outs
* maintenance appointments
* agreement deadlines
* support appointments

## 101. Calendar Privacy

Calendar events should minimize sensitive information.

External event descriptions should not expose:

* Government IDs
* payment evidence
* full application details
* legal notes
* unrestricted resident information

## 102. Accounting Integration

Future accounting integration may support:

* Charges
* verified payments
* allocations
* refunds
* fees
* deposits
* settlement records
* chart-of-account mapping

## 103. Accounting Boundary

The accounting platform may be a downstream financial-reporting system.

Habitacores remains authoritative for operational leasing state unless an approved architecture changes that ownership.

## 104. Accounting Synchronization

Synchronization should define:

* Direction
* source of truth
* mapping
* timing
* idempotency
* reconciliation
* error handling
* correction process

## 105. Customer-Support Integration

Support integrations may support:

* Ticket creation
* assignment
* conversation history
* SLA tracking
* escalation
* knowledge-base articles

Restricted customer access still requires Habitacores support-session controls.

## 106. Government and Regulatory Integrations

Potential future integrations may include:

* Identity authorities
* property registries
* tax authorities
* court systems
* tenancy authorities
* licensing systems

No such integration should be assumed available until verified and approved.

## 107. Legal Information Integrations

Legal integrations may provide:

* Statutes
* regulatory guidance
* standard clauses
* court references
* notice requirements

Legal content must remain subject to qualified human review.

## 108. Property Data Integrations

Future property data integrations may support:

* Address validation
* ownership evidence
* valuation context
* geographic data
* utility information
* development records

External property data must not overwrite verified internal records automatically.

## 109. Partner API Strategy

Habitacores may expose partner APIs for:

* Approved property operators
* accounting systems
* payment partners
* legal partners
* maintenance partners
* approved marketplace partners

Partner APIs must be separately governed.

## 110. Partner Authentication

Partner APIs may use:

* OAuth
* signed requests
* API keys with strict scope
* mutual TLS
* service accounts

Browser session tokens must not be repurposed automatically for partner systems.

## 111. Partner Authorization

Partner authorization must include:

* Partner identity
* organization relationship
* approved scope
* resource scope
* environment
* expiry
* rate limit
* audit

## 112. Partner API Versioning

Partner APIs should use explicit versioning.

Example:

```text
/api/partners/v1/...
```

Breaking changes require migration support.

## 113. Partner API Documentation

Documentation should include:

* Authentication
* authorization
* endpoints
* schemas
* errors
* idempotency
* rate limits
* webhooks
* examples
* version policy
* support process

## 114. Import Architecture

Imports may support:

* Properties
* units
* residents
* agreements
* charges
* payments
* vendors
* historical records

## 115. Import Flow

```text
Upload
→ Scan
→ Parse
→ Schema validation
→ Tenant validation
→ Preview
→ Error report
→ Approval
→ Staged import
→ Transactional commit
→ Reconciliation
→ Audit
```

## 116. Import Safety

Imports must not:

* Create cross-tenant records
* fabricate unknown values
* overwrite issued agreements
* duplicate verified payments
* create conflicting occupancy
* erase audit history
* bypass authorization

## 117. Export Architecture

Exports may support:

* Operational records
* financial records
* agreements
* audit records
* organization data
* migration data

## 118. Export Controls

Sensitive exports should require:

* Permission
* filters
* field selection
* approval where required
* step-up authentication
* expiry
* secure download
* audit

## 119. Data Mapping

Each integration must define mappings between:

* Habitacores field
* provider field
* type
* required status
* transformation
* null behavior
* enum mapping
* validation
* source of truth

## 120. Enum Mapping

Provider enums must map through explicit tables.

Unknown provider values should:

* Remain unknown
* be recorded
* trigger review where material
* not map silently to success

## 121. Money Mapping

Money mappings must preserve:

* Currency
* amount
* decimal precision
* minor units
* rounding
* provider fee
* net settlement
* tax where applicable

## 122. Date and Time Mapping

Integration timestamps should preserve:

* Original timestamp
* provider time zone
* UTC normalized time
* received time
* processed time

## 123. Identifier Mapping

Provider identifiers must be stored separately from internal identifiers.

A provider ID must not become the sole internal primary key.

## 124. Null and Unknown Mapping

Unknown provider values must not be converted to:

* Zero
* false
* empty string
* successful
* completed

without approved semantics.

## 125. Schema Validation

All inbound and outbound integration payloads should use schema validation.

Validation should cover:

* Required fields
* types
* lengths
* enums
* money
* dates
* identifiers
* nested objects
* additional fields policy

## 126. Schema Versioning

Integration schemas should include versions where possible.

Changes may be:

* Backward compatible
* conditionally compatible
* breaking

## 127. Breaking Provider Changes

A breaking provider change should trigger:

* Impact assessment
* adapter update
* contract tests
* staging validation
* migration plan
* release plan
* provider coordination

## 128. Provider SDKs

Provider SDKs may be used inside adapters.

SDK use should be reviewed for:

* Security
* maintenance
* versioning
* transitive dependencies
* runtime compatibility
* telemetry behavior
* retry behavior

## 129. Integration Error Model

Provider errors should map to internal categories such as:

* Authentication failed
* authorization failed
* validation failed
* rate limited
* provider unavailable
* timeout
* conflict
* duplicate
* rejected
* unknown

## 130. User-Facing Errors

User-facing errors should:

* Explain the safe next step
* avoid provider secrets
* avoid internal topology
* avoid false success
* provide correlation ID where useful
* distinguish pending from failed

## 131. Integration Audit Events

Material integration audit events may include:

* Provider connected
* provider disconnected
* credential rotated
* payment initiated
* provider transaction confirmed
* payment verified
* refund requested
* refund executed
* signature request created
* agreement signed
* document scan completed
* AI output generated
* partner access granted

## 132. Integration Monitoring

Monitoring should cover:

* Request count
* success rate
* latency
* timeout
* retry
* rate limit
* authentication failure
* webhook failure
* queue backlog
* reconciliation mismatch
* provider outage
* cost

## 133. Integration Alerting

Alerts may include:

* Sustained provider error rate
* invalid webhook signature
* missing callbacks
* reconciliation mismatch
* credential expiry
* rate-limit exhaustion
* abnormal cost
* provider schema change
* duplicate event surge
* integration queue backlog

## 134. Integration Dashboards

Dashboards may include:

* Provider health
* webhook processing
* payment provider
* signature provider
* communications
* file scanning
* AI
* reconciliation
* partner API
* import and export

## 135. Provider Status

Provider status may be represented as:

* Operational
* degraded
* partial outage
* unavailable
* suspended
* unknown

Provider status must remain distinct from internal workflow state.

## 136. Integration Security

Integration security must include:

* TLS
* signature verification
* secret management
* least privilege
* environment isolation
* input validation
* output validation
* idempotency
* rate limiting
* audit
* monitoring

## 137. Integration Privacy

Privacy review should evaluate:

* Data transferred
* purpose
* lawful basis
* provider retention
* cross-border transfer
* subprocessors
* deletion
* data-subject rights
* security controls
* contract terms

## 138. Data Processing Agreement

Providers handling personal or restricted data may require:

* Data-processing agreement
* confidentiality terms
* security obligations
* breach notification
* retention terms
* deletion terms
* subprocessor disclosure
* audit rights

## 139. Provider Due Diligence

Due diligence may include:

* Legal entity
* regulatory standing
* security documentation
* privacy policy
* uptime history
* financial stability
* support quality
* data location
* breach history
* subcontractors
* termination capability

## 140. Integration Testing

Testing should include:

* Unit tests
* adapter tests
* schema tests
* contract tests
* sandbox tests
* webhook tests
* replay tests
* idempotency tests
* failure tests
* rate-limit tests
* reconciliation tests
* security tests
* tenant-isolation tests

## 141. Provider Simulator

Provider simulators should support:

* Success
* pending
* failed
* timeout
* malformed payload
* invalid signature
* duplicate event
* out-of-order event
* rate limit
* provider outage

## 142. Contract Tests

Contract tests should verify:

* Request shape
* response shape
* enum mapping
* error mapping
* signature validation
* webhook payload
* version compatibility

## 143. Sandbox Tests

Sandbox testing should validate:

* Credentials
* endpoints
* account mapping
* callbacks
* return URLs
* expected statuses
* failure conditions
* rate limits where possible

## 144. Production Verification

Production verification should use controlled low-risk transactions or checks.

It must avoid uncontrolled live financial, legal, or customer actions.

## 145. Integration Acceptance Criterion — Payment Callback

```text
Given:
A valid payment-provider callback reports a successful transaction.

When:
Habitacores processes the event.

Then:
The event is authenticated and stored.

And:
The provider transaction is mapped to the correct environment, provider account, organization, payer, amount, and currency.

And:
The payment is not allocated until Habitacores financial-domain rules approve allocation.

And:
No agreement or occupancy activates solely from the provider status.
```

## 146. Integration Acceptance Criterion — Duplicate Callback

```text
Given:
A provider event has already been processed.

When:
The identical provider event is received again.

Then:
The event is recognized as a duplicate.

And:
No duplicate payment, refund, signature, notification, or business transition is created.

And:
The duplicate remains observable.
```

## 147. Integration Acceptance Criterion — Signature Version

```text
Given:
Agreement Version 3 was sent for signature.

When:
The signature provider reports completion for Version 2.

Then:
Version 3 is not marked signed.

And:
No agreement activation occurs.

And:
The mismatch is recorded for review.
```

## 148. Integration Acceptance Criterion — Invalid Webhook Signature

```text
Given:
A webhook request contains a valid-looking provider event but an invalid signature.

When:
The webhook endpoint receives the request.

Then:
The event is rejected.

And:
No domain command is executed.

And:
No protected status changes.

And:
A security signal is recorded.
```

## 149. Integration Acceptance Criterion — AI Prompt Injection

```text
Given:
An uploaded document contains instructions requesting unrelated tenant information.

When:
The document is summarized through the AI integration.

Then:
The document instructions are treated as untrusted content.

And:
No unrelated organization data is retrieved.

And:
No protected action is executed.

And:
The output remains advisory and subject to human review.
```

## 150. Integration Acceptance Criterion — Communication Retry

```text
Given:
An email provider times out after receiving a notification request.

When:
Habitacores cannot determine whether delivery was accepted.

Then:
The notification remains pending or uncertain.

And:
Any retry uses idempotency or duplicate protection.

And:
The underlying business action is not repeated.
```

## 151. Integration Acceptance Criterion — File Scan

```text
Given:
A restricted document upload completes.

When:
The file-scanning provider is unavailable.

Then:
The document remains pending scan or restricted.

And:
No user receives unrestricted access.

And:
No business verification is inferred.

And:
The failure is visible operationally.
```

## 152. Integration Acceptance Criterion — Environment Isolation

```text
Given:
A staging environment is configured with a provider integration.

When:
The staging application sends requests or receives callbacks.

Then:
Only sandbox or staging provider accounts and credentials are used.

And:
Production provider events cannot be processed.

And:
Production customer records cannot be affected.
```

## 153. Integration Acceptance Criterion — Unknown Provider Status

```text
Given:
A provider returns a new status that Habitacores does not recognize.

When:
The response is mapped.

Then:
The value remains unknown or awaiting review.

And:
It is not mapped silently to successful, verified, signed, delivered, or completed.

And:
The schema difference is recorded.
```

## 154. Integration Acceptance Criterion — Unknown Pilot Data

```text
Given:
Block B unit count remains unconfirmed.

When:
An import, export, partner API, accounting integration, or analytics integration processes Block B.

Then:
The value remains null or pending.

And:
No placeholder units are generated.

And:
No external system response is interpreted as authoritative inventory without approved verification.
```

## 155. MVP Integration Scope

The MVP should include:

### Payment

* One approved payment provider or controlled manual payment workflow
* payment initiation where applicable
* provider transaction reference
* webhook verification
* payment evidence
* manual verification
* allocation
* refund foundation
* reconciliation

### Signature

* One approved electronic-signature provider or controlled signature workflow
* agreement-version binding
* signer validation
* callback processing
* signed-document preservation
* evidence

### Communication

* Email
* in-app notification
* SMS or WhatsApp only where approved
* queueing
* delivery status
* retry
* redaction

### Files

* Private storage
* file-scanning integration
* signed access
* scan state
* restricted failure behavior

### AI

* Controlled AI gateway
* redaction
* tenant scope
* advisory output
* human review
* provider failure isolation

### Operations

* Integration registry
* provider credentials
* webhook event store
* idempotency
* retries
* reconciliation
* monitoring
* alerts
* runbooks

## 156. Deferred Integration Capabilities

The following may be deferred:

* Multiple simultaneous payment providers
* automated provider failover
* open-banking aggregation
* direct bank statement ingestion
* biometric identity verification
* government property-registry integration
* court-system integration
* tax-authority integration
* accounting-platform synchronization
* calendar synchronization
* advanced WhatsApp support
* mobile push notifications
* customer-owned provider accounts
* partner developer portal
* public marketplace syndication
* external valuation providers
* utility-provider integrations
* full data warehouse integration
* real-time event streaming
* enterprise integration marketplace
* no-code integration builder
* customer-configurable webhooks
* external workflow automation
* automated legal filing
* fully autonomous AI integration

Deferred capabilities must not weaken MVP financial, signature, document, communication, or AI controls.

## 157. Integration Risks

### 157.1 Provider Lock-In

Risk:

Provider-specific behavior becomes embedded in the domain.

Controls:

* Integration ports
* adapters
* normalized models
* ADRs
* export capability
* contract tests

### 157.2 Callback Spoofing

Risk:

A malicious actor sends a forged provider callback.

Controls:

* Signature validation
* timestamp validation
* provider-account validation
* event persistence
* domain validation

### 157.3 Duplicate Actions

Risk:

Retries or duplicate callbacks create repeated payments, refunds, signatures, or communications.

Controls:

* Idempotency
* unique constraints
* event store
* state-machine checks
* reconciliation

### 157.4 Provider Outage

Risk:

A provider failure blocks critical workflows.

Controls:

* Pending states
* retries
* manual alternatives
* circuit breaker
* monitoring
* runbooks

### 157.5 Incorrect Mapping

Risk:

Provider fields map incorrectly to internal records.

Controls:

* Explicit mapping
* schema validation
* test fixtures
* contract tests
* reconciliation
* human review

### 157.6 Credential Exposure

Risk:

Provider secrets are exposed.

Controls:

* Secret management
* server-side use
* scanning
* rotation
* environment isolation
* least privilege

### 157.7 Data Leakage

Risk:

Restricted data is transferred unnecessarily.

Controls:

* Data minimization
* field allowlists
* redaction
* provider due diligence
* retention controls
* privacy review

### 157.8 Reconciliation Failure

Risk:

Habitacores and provider records diverge undetected.

Controls:

* Scheduled reconciliation
* mismatch alerts
* provider references
* audit
* manual resolution

### 157.9 Provider Schema Drift

Risk:

Provider changes break integration behavior.

Controls:

* Schema validation
* contract tests
* version monitoring
* unknown-value handling
* adapter isolation

### 157.10 Pilot Hardcoding

Risk:

Provider accounts, callbacks, or mappings assume only the founding property.

Controls:

* Organization mapping
* provider account abstraction
* environment mapping
* multi-tenant tests
* neutral property hierarchy

## 158. Unresolved Integration Decisions

The following remain pending:

* Final payment provider
* final payment-account ownership model
* final virtual-account strategy
* final manual payment-verification process
* final refund provider process
* final reconciliation schedule
* final signature provider
* final signer-authentication method
* final signed-document evidence requirements
* final email provider
* final SMS provider
* final WhatsApp provider
* final sender domains
* final communication preference model
* final identity-verification provider
* final government-ID verification scope
* final biometric-data position
* final file-scanning provider
* final AI provider
* final AI model
* final AI gateway architecture
* final AI provider-retention settings
* final analytics provider
* final geolocation provider
* final document-generation provider
* final accounting integration
* final support-platform integration
* final calendar integration
* final legal-data integrations
* final government integrations
* final property-data providers
* final partner API authentication
* final partner API rate limits
* final customer webhook capability
* final import file formats
* final export formats
* final integration event retention
* final raw-provider-payload retention
* final retry policy
* final timeout values
* final circuit-breaker policy
* final provider fallback policy
* final provider due-diligence process
* final data-processing agreements
* final cross-border processing position
* final provider incident contacts
* final integration cost budget
* final provider renewal process
* final integration decommissioning process
* final pilot public names
* final Block A unit labels
* final Block B inventory
* final second-building structure
* final water-feature classification

These decisions must be resolved before the affected integration becomes production dependent.

## 159. Integration Governance

Changes to approved integration architecture require:

1. Change description
2. Business rationale
3. Integration category
4. Provider
5. Environment
6. Data exchanged
7. Data classification
8. Source of truth
9. Domain impact
10. authentication impact
11. authorization impact
12. tenant-isolation impact
13. financial impact
14. agreement impact
15. occupancy impact
16. document impact
17. privacy impact
18. security impact
19. provider credential impact
20. webhook impact
21. retry impact
22. idempotency impact
23. reconciliation impact
24. monitoring impact
25. support impact
26. cost impact
27. testing impact
28. migration impact
29. rollback or decommissioning plan
30. Product Visionary approval
31. Chief Product Architect approval
32. Security review where required
33. Legal or compliance review where required
34. Effective version

## 160. Integration Review Triggers

A dedicated integration review is required when introducing:

* New payment provider
* new bank-transfer model
* new refund capability
* new signature provider
* new communication channel
* new identity provider
* new file-processing provider
* new AI provider
* new analytics provider
* new partner API
* new customer webhook
* new accounting integration
* new government integration
* new restricted-data transfer
* new cross-border data flow
* new provider-owned authoritative state
* new organization-owned provider account
* new external write capability
* new import or bulk synchronization process

## 161. Current Integration Architecture Decision

Habitacores will use an adapter-based, provider-neutral, domain-controlled integration architecture organized as:

```text
Habitacores Domain
→ Integration Port
→ Provider Adapter
→ External Provider
→ Authenticated Response or Webhook
→ Integration Event Store
→ Mapping and Validation
→ Domain Rules
→ Authoritative State Transition
→ Audit
→ Monitoring and Reconciliation
```

Critical provider workflows will follow:

```text
Internal Pending Record
→ Provider Request
→ Provider Reference
→ Callback, Polling, or Reconciliation
→ Authentication
→ Idempotency
→ Organization Mapping
→ Amount, Currency, Party, Version, and State Validation
→ Human Review Where Required
→ Protected Domain Command
```

The architecture must preserve:

* Organization isolation
* provider neutrality
* source-of-truth clarity
* least privilege
* environment isolation
* secret protection
* webhook authenticity
* idempotency
* retry safety
* reconciliation
* financial integrity
* payment-verification distinction
* agreement-version integrity
* signature integrity
* occupancy integrity
* document security
* communication privacy
* AI data minimization
* human authority
* auditability
* observability
* provider replaceability
* future SaaS extensibility

No provider callback, provider dashboard, payment screenshot, transaction status, signature status, delivery receipt, identity score, file-scan result, geocoding result, accounting record, partner request, AI output, analytics event, imported value, pilot-team relationship, or external system response may independently authorize, verify, approve, allocate, sign, activate, settle, refund, reverse, terminate, reveal, or otherwise finalize a protected Habitacores business action.