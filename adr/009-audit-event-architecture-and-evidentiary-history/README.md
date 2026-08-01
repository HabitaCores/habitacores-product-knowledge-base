# ADR-009 — Audit Event Architecture and Evidentiary History

## Decision Record

| Field | Value |
|---|---|
| Product | Habitacores |
| Decision | Audit Event Architecture and Evidentiary History |
| ADR | ADR-009 |
| Version | 0.1 |
| Status | Proposed |
| Decision Type | Foundational Security, Compliance, and Operational Architecture |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Authoritative Audit Store | PostgreSQL |
| Audit Model | Append-only application audit events |
| Transaction Model | Same transaction as protected state change where possible |
| Audit Ownership | Habitacores application services |
| Ordinary Logs | Separate from authoritative audit records |
| Sensitive Data Policy | Minimize, redact, hash, or reference |
| Tenant Isolation | Organization scoped with PostgreSQL RLS |
| AI Audit Model | Use-case, model, prompt, source, validation, and human disposition metadata |
| Normal Mutation | Prohibited |
| Last Updated | 1 August 2026 |

## 1. Context

Habitacores will execute sensitive and legally meaningful workflows involving:

- User registration
- identity linking
- email verification
- MFA enrollment and reset
- session revocation
- organization invitations
- membership creation
- role assignment
- permission changes
- property administration
- rental applications
- application decisions
- payment evidence
- payment verification
- payment allocation
- refunds
- adjustments
- receipts
- agreements
- signatures
- occupancy activation
- maintenance
- document access
- legal assignments
- support sessions
- platform administration
- integration events
- Responsible AI operations

The system must preserve reliable evidence of material actions.

That evidence must support:

- Operational investigation
- customer support
- financial reconciliation
- dispute resolution
- security incident response
- access review
- compliance review
- legal inquiry
- Responsible AI governance
- internal accountability

Ordinary application logging is insufficient because logs may be:

- Sampled
- rotated
- reformatted
- dropped
- sent to external systems
- optimized for debugging rather than evidence
- missing authoritative business context

Database change history alone is also insufficient because it may not explain:

- Actor intent
- authorization context
- command purpose
- support-session authority
- human review
- rejected attempts
- external evidence
- AI assistance
- business outcome

Habitacores therefore requires a dedicated application audit architecture.

## 2. Decision Drivers

The decision is driven by the need to:

1. Preserve evidence of material business actions.
2. identify the responsible actor.
3. preserve organization scope.
4. distinguish human, workload, provider, and support actors.
5. record successful and selected failed actions.
6. preserve resource identity.
7. preserve command and outcome.
8. support financial and legal investigations.
9. support session and authorization investigations.
10. support Responsible AI accountability.
11. preserve audit history when display values change.
12. prevent destructive audit modification.
13. avoid storing secrets or unrestricted personal data.
14. support tenant isolation.
15. support operational search and exports.
16. link activity across services and workers.
17. remain transactionally consistent with authoritative changes.
18. support future archival.
19. provide clear Codex boundaries.
20. avoid turning logs or event streams into the sole audit source.

## 3. Decision

Habitacores will implement a first-class append-only audit-event system stored in PostgreSQL.

The approved model is:

```text
Protected operation
→ Authentication and authorization
→ Application service
→ Domain validation
→ Authoritative state change
→ Audit event appended
→ Outbox event appended where required
→ Transaction committed
````

Audit records will be:

* Append only
* structured
* typed
* versioned
* organization scoped where applicable
* actor attributable
* resource attributable
* correlation linked
* minimally sensitive
* queryable
* protected through authorization and RLS

## 4. Audit Versus Logs

Audit records and application logs serve different purposes.

### Audit records answer

* Who acted?
* under which organization?
* under which membership or support session?
* what operation occurred?
* which resource was affected?
* what was the outcome?
* when did it occur?
* why was it performed where a reason is required?
* which approved workflow produced it?
* was AI involved?
* which related events belong to the same workflow?

### Logs answer

* What happened technically?
* how long did it take?
* which dependency failed?
* what stack or route was involved?
* which service or worker processed the request?

Logs must not substitute for audit records.

Audit records must not become verbose technical logs.

## 5. Audit Versus Domain Events

Audit events and domain events are related but distinct.

### Domain event

Represents a business fact that other components may react to.

Examples:

```text
ApplicationSubmitted
PaymentVerified
AgreementActivated
OccupancyActivated
```

### Audit event

Represents accountable evidence of an action, attempt, decision, access, or administrative operation.

Examples:

```text
APPLICATION_SUBMISSION_RECORDED
PAYMENT_VERIFICATION_APPROVED
RESTRICTED_DOCUMENT_ACCESSED
SUPPORT_SESSION_OPENED
```

One protected operation may create:

* A domain event
* an audit event
* an outbox event

These records must remain semantically distinct.

## 6. Audit Authority

Application services are the primary authority for business audit creation.

They understand:

* Actor
* command
* authorization
* resource
* state transition
* outcome
* reason
* support context
* correlation
* human review
* AI involvement

Database triggers may supplement selected low-level integrity controls but are not the primary business audit architecture.

## 7. Audit Event Schema

A directional audit record should contain:

```text
id
event_type
event_version
occurred_at
recorded_at
environment
actor_type
actor_id
organization_id
membership_id
support_session_id
resource_type
resource_id
operation
outcome
reason_code
reason_text_reference
correlation_id
causation_id
request_id
source
authentication_assurance
ip_metadata_reference
user_agent_metadata_reference
change_summary
sensitivity_classification
retention_class
created_at
```

Fields should be included only where meaningful.

## 8. Audit Identifier

Each audit event will use a dedicated UUIDv7 identifier.

The audit event ID must remain distinct from:

* Resource ID
* domain-event ID
* outbox-event ID
* correlation ID
* causation ID
* provider-event ID
* support-session ID

## 9. Event Type

Audit event types must use stable machine-readable names.

Examples:

```text
IDENTITY_SESSION_REVOKED
ORGANIZATION_MEMBERSHIP_GRANTED
ORGANIZATION_ROLE_REMOVED
APPLICATION_SUBMITTED
APPLICATION_DECISION_RECORDED
PAYMENT_EVIDENCE_ACCESSED
PAYMENT_VERIFIED
PAYMENT_ALLOCATION_CREATED
REFUND_APPROVED
AGREEMENT_ISSUED
AGREEMENT_ACTIVATED
OCCUPANCY_ACTIVATED
RESTRICTED_DOCUMENT_DOWNLOADED
SUPPORT_SESSION_OPENED
AI_OUTPUT_ACCEPTED
```

Event types must not depend on mutable UI labels.

## 10. Event Versioning

Each event type must have a schema version.

Example:

```text
event_type = PAYMENT_VERIFIED
event_version = 1
```

A breaking metadata change requires a new version.

Historical records must remain interpretable.

## 11. Actor Types

Audit actors may include:

```text
USER
SYSTEM
WORKER
INTEGRATION
SUPPORT_OPERATOR
PLATFORM_OPERATOR
MIGRATION
SECURITY_OPERATOR
```

AI is not an authoritative actor for protected business decisions.

An AI operation may be recorded as assistance attached to:

* The requesting human actor
* the executing workload
* the approved AI use case

## 12. Human Actor

A human-initiated event should normally preserve:

* Internal user ID
* organization membership where relevant
* active organization
* authentication assurance
* support session where applicable
* correlation ID

Provider subject identifiers should not replace the internal user ID.

## 13. Workload Actor

A worker or scheduled operation should preserve:

* Workload identity
* operation name
* originating human actor where relevant
* organization scope
* causation event
* correlation ID

The phrase `system` alone is insufficient when a more precise workload identity is available.

## 14. Provider Actor

Provider events should preserve:

* Provider
* environment
* provider account
* provider event ID
* signature-verification result
* internal integration identity
* related resource
* correlation ID

A provider callback is evidence, not unrestricted authority.

## 15. Support Actor

Support actions must preserve:

* Support operator ID
* support-session ID
* target organization
* target user where applicable
* authorized scope
* reason
* approval reference where required
* session expiry
* operation result

Support activity must never be recorded as though the customer performed it.

## 16. Organization Scope

Organization-owned audit records must contain:

```text
organization_id
```

The organization must come from trusted server-side context.

It must not be inferred solely from:

* Client request
* URL
* cookie
* stale JWT claim
* resource display label
* AI output

## 17. Platform Events

Platform-wide events may have no tenant organization.

Examples:

* New organization created
* platform security policy changed
* global provider configuration changed
* emergency access activated

Such events require platform-level authorization and restricted audit access.

## 18. Resource Identity

Audit records should preserve:

* Resource type
* stable internal resource ID

Examples:

```text
resource_type = PAYMENT
resource_id = <payment UUID>
```

Mutable names and labels may be included only as optional safe snapshots.

They must not replace stable identity.

## 19. Parent and Related Resources

An event may include related resource references where necessary.

Example:

```text
Payment allocation
- payment_id
- charge_id
- allocation_id
```

Related identifiers should use structured metadata rather than unstructured prose.

## 20. Operation

The operation field should describe the protected action.

Examples:

```text
CREATE
VIEW
DOWNLOAD
SUBMIT
APPROVE
REJECT
VERIFY
ALLOCATE
REVERSE
ACTIVATE
DEACTIVATE
ASSIGN
REVOKE
EXPORT
IMPERSONATE
```

Event type remains more specific than the general operation.

## 21. Outcome

Possible outcomes include:

```text
SUCCEEDED
DENIED
FAILED
PENDING
CANCELLED
PARTIALLY_SUCCEEDED
```

Only selected failed or denied actions should become durable audit events.

Routine validation errors should not overwhelm the audit store.

## 22. Reason Codes

Material decisions should preserve stable reason codes.

Examples:

```text
APPLICATION_REQUIREMENTS_SATISFIED
PAYMENT_EVIDENCE_UNSUPPORTED
REFUND_DUPLICATE_REQUEST
MEMBERSHIP_REVOKED_BY_ADMINISTRATOR
SUPPORT_ACCESS_APPROVED
AI_OUTPUT_REJECTED_UNGROUNDED
```

Free text may supplement a reason code but must be controlled.

## 23. Free-Text Reasons

Free-text audit content should be minimized.

Where a reason requires detailed notes:

* Store the note in an authorized domain record.
* Reference that record from the audit event.
* Avoid duplicating sensitive content in audit metadata.

Free text must not contain secrets, passwords, tokens, or unnecessary personal data.

## 24. Correlation

Every material workflow should preserve a correlation identifier.

Correlation connects:

* Web request
* application service
* database transaction
* audit event
* outbox event
* worker execution
* provider request
* provider callback
* reconciliation
* AI operation

Correlation does not replace individual event identity.

## 25. Causation

Where one event causes another, preserve a causation identifier.

Example:

```text
ApplicationSubmitted
→ NotificationRequested
→ EmailDispatched
```

Causation supports reconstruction of workflow lineage.

## 26. Request Identity

Where appropriate, the audit record may preserve a request identifier distinct from correlation ID.

This supports linking multiple commands issued within one broader workflow.

## 27. Time Fields

Audit records should distinguish:

```text
occurred_at
recorded_at
created_at
```

### `occurred_at`

When the business action or provider event occurred.

### `recorded_at`

When Habitacores accepted the audit event.

### `created_at`

Database record creation time.

These values may differ.

UUID timestamps are not authoritative audit timestamps.

## 28. Transactional Consistency

When an audit event describes a committed protected state change, the audit record should be written in the same PostgreSQL transaction.

Examples:

* Payment verified
* allocation created
* agreement activated
* occupancy activated
* membership granted
* role revoked

If the transaction rolls back:

* The state change must not persist.
* The success audit event must not persist.

## 29. Failed Attempts

Failed attempts may require audit outside the failed business transaction.

Examples:

* Repeated cross-tenant access attempt
* MFA reset denial
* privileged operation denial
* support-session misuse
* restricted export denial
* security configuration change failure

Such audit events require a controlled separate write path.

They must not falsely indicate successful business state.

## 30. Audit Writer

The audit package should expose an append-only interface.

Conceptual example:

```typescript
interface AuditWriter {
  append(
    transaction: DatabaseTransaction,
    event: AuditEvent
  ): Promise<void>;
}
```

A separate security-audit writer may support selected denied attempts outside normal transactions.

## 31. Append-Only Enforcement

Normal application roles may:

* Insert authorized audit events
* read authorized audit events

They must not normally:

* Update audit records
* delete audit records
* rewrite event types
* rewrite actors
* rewrite timestamps
* rewrite outcomes

Database grants and application APIs should enforce this rule.

## 32. Corrections

An incorrect audit event must not be silently edited.

A correction should create an additional event such as:

```text
AUDIT_EVENT_CORRECTION_RECORDED
```

The correction should reference:

* Original audit event
* correction reason
* correcting actor
* corrected interpretation
* approval where required

## 33. Database Storage

The primary audit store will be PostgreSQL.

Directional table:

```text
audit_events
```

Potential supporting tables include:

```text
audit_event_metadata
audit_exports
audit_retention_actions
audit_integrity_checks
```

Additional tables should be added only when required.

## 34. PostgreSQL Representation

The audit table should use:

* UUID primary key
* explicit organization ownership
* typed event and actor fields
* timestamptz for event timestamps
* JSONB only for bounded metadata
* indexes supporting investigation
* RLS
* append-only privileges

## 35. Structured Metadata

Structured metadata may include:

* Changed field names
* prior state classification
* new state classification
* related resource IDs
* decision code
* provider reference
* AI use-case reference
* export scope
* document access mode

Metadata must use a defined schema per event type and version.

## 36. JSONB Restrictions

JSONB must not become an unrestricted dumping ground.

Prohibited metadata includes:

* Full request bodies
* full database rows
* passwords
* access tokens
* refresh tokens
* MFA secrets
* bank-account data
* complete identity documents
* payment evidence files
* signed agreements
* unrestricted support notes
* raw AI prompts and outputs by default

## 37. Before-and-After Values

Audit records should not automatically store complete before-and-after row snapshots.

Instead, use:

* Changed field names
* state transition
* safe selected values
* references to immutable versions
* cryptographic digest where justified

Complete snapshots may create unnecessary privacy and retention risk.

## 38. Sensitive Data Classification

Every audit event type should define a sensitivity classification.

Examples:

```text
INTERNAL
CONFIDENTIAL
RESTRICTED
HIGHLY_RESTRICTED
```

Classification influences:

* Access
* export
* retention
* masking
* monitoring
* archival

## 39. Sensitive Identifiers

Where exact sensitive identifiers are unnecessary, audit metadata should use:

* Internal resource ID
* last four characters
* deterministic digest
* tokenized reference
* secure domain record reference

Hashing must not be presented as anonymization where re-identification remains possible.

## 40. IP and User-Agent Metadata

IP address and user-agent details may support security investigation.

They should be:

* Collected only where justified
* classified appropriately
* stored in minimized or normalized form
* retained according to policy
* restricted from broad operational access

The exact privacy treatment is deferred.

## 41. Document Access Auditing

Restricted document operations should record:

* Actor
* organization
* document ID
* document version
* operation
* access purpose where required
* support session where applicable
* result
* correlation ID

The audit event must not contain the document contents or permanent storage URL.

## 42. Financial Auditing

Material finance events include:

* Charge created
* charge adjusted
* payment evidence submitted
* evidence reviewed
* payment verified
* payment rejected
* allocation created
* allocation reversed
* refund requested
* refund approved
* refund executed
* receipt issued
* reconciliation exception resolved

Amounts may be included as structured Money metadata where the event definition approves it.

## 43. Money Metadata

Money audit metadata must preserve:

```text
minor_amount
currency
```

Minor amounts should use exact string serialization where represented in JSON.

Floating-point values are prohibited.

## 44. Agreement Auditing

Material agreement events include:

* Draft created
* version issued
* sent for signature
* signature completed
* signature rejected
* version superseded
* agreement activated
* agreement terminated
* legal notice issued

Audit events should reference the immutable agreement version.

## 45. Occupancy Auditing

Material occupancy events include:

* Occupancy proposed
* prerequisites evaluated
* occupancy activated
* unit assignment changed
* occupancy ended
* conflicting activation denied

The audit history must preserve the responsible actor and prerequisite result.

## 46. Membership and Permission Auditing

Material access events include:

* Invitation created
* invitation accepted
* membership activated
* membership suspended
* membership revoked
* role assigned
* role removed
* permission override created
* assignment added
* assignment removed

The event should preserve:

* Acting administrator
* affected user
* organization
* role or permission
* effective time
* reason
* assurance level

## 47. Support Auditing

Support-session events include:

* Session requested
* session approved
* session opened
* session used
* session extended
* session expired
* session terminated
* scope change attempted
* restricted action denied

Each action performed during a support session must remain attributable to the support operator.

## 48. Authentication Auditing

Selected identity events include:

* Email verified
* session revoked
* all sessions revoked
* MFA enrolled
* MFA removed
* MFA reset
* identity linked
* identity unlinked
* account suspended
* account reactivated

Passwords, tokens, recovery secrets, and factor secrets must never be recorded.

## 49. AI Operation Auditing

Approved AI operations should record:

* AI operation ID
* use-case ID
* organization ID
* requesting actor
* target resource
* prompt-template ID
* prompt version
* model-configuration ID
* retrieval-policy version
* source resource references
* input classification
* redaction result
* output-schema version
* safety-validation result
* grounding result
* generated time
* review status
* human reviewer
* accepted, edited, rejected, or expired result
* correlation ID
* estimated cost metadata where approved

## 50. Raw AI Content

Raw AI prompts and outputs should not automatically enter the general audit table.

Where retention is required:

* Store content in a dedicated restricted record.
* Reference that record from the audit event.
* Apply purpose, access, encryption, and retention controls.
* Minimize personal data.
* Preserve prompt and output versioning.

## 51. Human Disposition

Material AI assistance should record a human disposition such as:

```text
ACCEPTED
ACCEPTED_WITH_EDITS
REJECTED
EXPIRED
SUPERSEDED
```

AI generation does not equal human approval.

## 52. AI Decision Boundary

The audit architecture must never represent AI as the approving actor for:

* Application decisions
* payment verification
* allocations
* refunds
* agreement activation
* occupancy activation
* legal decisions
* membership or permission changes

The accountable human or deterministic workload remains the authoritative actor.

## 53. Audit Authorization

Audit access is itself protected.

Potential permissions include:

```text
audit.view-own-activity
audit.view-organization
audit.view-financial
audit.view-security
audit.view-support
audit.export
audit.manage-retention
audit.verify-integrity
```

A general organization administrator must not automatically receive every sensitive audit category.

## 54. Tenant Isolation

Organization-scoped audit records will use PostgreSQL RLS.

The normal runtime role:

* Must not own the audit table
* must not have `BYPASSRLS`
* must not delete or update audit records
* must receive only approved insert and read capabilities

## 55. Personal Audit Access

Users may be allowed to see selected security and account activity concerning themselves.

Personal views must:

* Exclude internal security details
* exclude other users’ activity
* exclude support investigation notes
* avoid exposing detection methods
* use controlled projections

## 56. Platform Audit Access

Platform-wide audit access requires:

* Explicit platform permission
* MFA or step-up authentication
* stated purpose
* restricted interface
* access auditing
* export controls

Platform role labels alone are insufficient.

## 57. Audit-of-Audit

Sensitive audit operations should themselves be audited.

Examples:

* Viewing highly restricted audit history
* exporting audit records
* creating an audit retention exception
* performing an integrity check
* accessing platform-wide audit data

## 58. Search

Audit search should support:

* Time range
* organization
* actor
* resource
* event type
* operation
* outcome
* correlation ID
* support-session ID
* provider-event ID
* AI use-case ID
* sensitivity class

Search must preserve tenant and permission restrictions.

## 59. Pagination

Audit records should use stable cursor-based pagination for large result sets.

Directional ordering:

```text
ORDER BY occurred_at DESC, id DESC
```

Offset pagination may become inefficient for large histories.

## 60. Indexing

Likely indexes include:

```text
organization_id, occurred_at
organization_id, event_type, occurred_at
actor_id, occurred_at
resource_type, resource_id, occurred_at
correlation_id
support_session_id
outcome, occurred_at
```

Partial or specialized indexes should follow measured requirements.

## 61. Partitioning

Table partitioning is not required initially.

It may be introduced when:

* Audit volume justifies it
* retention and archival require it
* query performance requires it
* operational maintenance requires it

Partitioning must not weaken tenant isolation or evidence integrity.

## 62. Retention

Audit retention must be category specific.

Different events may require different periods based on:

* Financial obligations
* agreement lifecycle
* tenancy history
* identity security
* support operations
* legal requirements
* Responsible AI governance
* incident investigation

The exact schedule requires a later retention decision.

## 63. Retention Versus Immutability

Append-only does not necessarily mean permanent retention.

Deletion or archival under an approved retention process may be required.

Such processes must be:

* Authorized
* automated where appropriate
* policy driven
* audited
* legally reviewed where required
* tenant aware
* secure

Ordinary users and administrators must not manually delete audit evidence.

## 64. Legal Hold

Future legal-hold functionality may suspend normal retention for selected:

* Organizations
* users
* resources
* event categories
* time ranges

Legal holds require explicit authority and audit.

## 65. Archival

Long-term audit archival may use immutable object storage or another controlled system.

Archival must preserve:

* Event identity
* schema version
* ordering
* integrity
* access restrictions
* encryption
* retrieval process
* legal-hold status

The exact archival provider is deferred.

## 66. Integrity Protection

Habitacores should support progressive audit-integrity controls.

Initial controls include:

* Append-only grants
* transaction consistency
* immutable migrations
* restricted administration
* backups
* audit-of-audit

Future controls may include:

* Hash chaining
* signed batches
* immutable archival
* external timestamping
* periodic integrity manifests

## 67. Hash Chaining

Hash chaining is not required for the MVP but remains directionally supported.

If adopted, the design must address:

* Organization-specific versus global chains
* concurrency
* partitioning
* correction events
* archival
* verification
* key management
* performance

A superficial hash column is not sufficient tamper evidence.

## 68. Database Administrators

Database administrators may technically possess elevated access.

Administrative access must be governed by:

* Named identities
* least privilege
* MFA
* emergency-access process
* access logging
* change control
* post-access review
* credential rotation

The application must not falsely claim that PostgreSQL alone makes data impossible for all administrators to alter.

## 69. Exports

Audit exports require:

* Explicit permission
* organization or platform scope
* event-category scope
* time range
* field allowlist
* sensitivity review
* stated purpose
* secure generation
* secure delivery
* expiry
* export audit event

## 70. Export Format

Exports may include:

* CSV
* JSON
* PDF report
* signed evidence package

Machine-readable exports should preserve:

* Event ID
* event type
* version
* actor
* organization
* resource
* outcome
* timestamps
* correlation
* safe metadata

## 71. Export Integrity

High-assurance exports may include:

* File digest
* generation time
* generating actor
* scope
* export ID
* record count
* application version
* signature where approved

## 72. Notifications

Selected high-risk audit events may trigger notifications.

Examples:

* Privileged role assigned
* MFA reset
* all sessions revoked
* refund approved
* support session opened
* restricted export generated
* emergency access used
* repeated cross-tenant denial

Notification failure must not erase the audit event.

## 73. Outbox Integration

Where an audit-triggered notification or downstream process is required:

```text
Authoritative transaction
→ Audit event
→ Outbox event
→ Commit
→ Worker processes notification
```

Audit records should not depend on notification success.

## 74. Observability

Audit infrastructure telemetry may include:

* Append latency
* append failures
* event-type counts
* export duration
* query latency
* RLS denials
* retention-job status
* integrity-check status
* storage growth

Telemetry must not duplicate sensitive event metadata unnecessarily.

## 75. Audit Write Failure

For protected state changes requiring audit:

* Audit write failure should normally fail the transaction.
* The protected change must not commit without required evidence.

Exceptions require explicit classification.

## 76. Best-Effort Audit

Best-effort audit may be acceptable only for narrowly defined low-risk technical events.

It is not approved for:

* Financial actions
* membership changes
* permission changes
* agreement actions
* occupancy actions
* support access
* restricted document access
* security-factor changes
* material AI dispositions

## 77. Availability

The audit store is part of the primary PostgreSQL transaction architecture.

Its design must avoid:

* External network dependency during critical transactions
* synchronous calls to external SIEM systems
* separate remote audit service required for every state change

External forwarding should occur asynchronously.

## 78. SIEM and Security Analytics

Selected security events may be forwarded to an external security platform.

PostgreSQL remains the initial authoritative application audit source.

Forwarding must be:

* Asynchronous
* filtered
* redacted
* resilient
* monitored
* idempotent where appropriate

## 79. Audit Event Registry

Habitacores should maintain a registry defining each audit event.

Each registration should include:

* Event type
* version
* owner
* trigger
* actor type
* organization requirements
* resource requirements
* metadata schema
* sensitivity
* retention class
* access category
* notification behavior
* tests

## 80. Event Ownership

Every event type must have an owning module.

Examples:

```text
Identity
Organizations
Finance
Agreements
Occupancy
Maintenance
Documents
Support
Platform
Responsible AI
```

An event must not be introduced without an owner.

## 81. Schema Validation

Audit metadata must be validated before insertion.

A malformed audit event must not:

* Enter the store silently
* drop required organization context
* omit required actor identity
* contain prohibited fields
* use an unregistered event type

## 82. Event Registry Compatibility

A new event version must preserve historical interpretation.

Consumers and export tools must remain able to process earlier versions.

## 83. Data Minimization

Audit design must follow:

```text
Record enough to prove and investigate the event.
Do not record everything available.
```

The audit system must not become a shadow copy of the entire product database.

## 84. Unknown Values

Unknown audit facts must remain unknown.

Examples:

* Unknown provider occurrence time must not become callback time.
* unknown IP must not become `0.0.0.0`.
* unknown reason must not become an invented reason.
* unknown support approval must not become approved.

Null, absent, or pending values must retain their proper meaning.

## 85. Error Codes

Audit infrastructure may return stable errors such as:

```text
AUDIT_EVENT_INVALID
AUDIT_EVENT_TYPE_UNKNOWN
AUDIT_CONTEXT_MISSING
AUDIT_ORGANIZATION_MISMATCH
AUDIT_APPEND_FAILED
AUDIT_ACCESS_DENIED
AUDIT_EXPORT_SCOPE_INVALID
AUDIT_INTEGRITY_CHECK_FAILED
```

Raw database details must not be exposed to users.

## 86. Testing

Audit tests must cover:

* Event validation
* required actor context
* required organization context
* resource identity
* event version
* transaction rollback
* append-only grants
* RLS
* cross-tenant reads
* prohibited update
* prohibited delete
* correction events
* sensitive metadata rejection
* export authorization
* AI event metadata
* support-session attribution
* worker attribution

## 87. Transaction Test

Tests must prove:

```text
State change succeeds
+ audit append succeeds
→ both commit

State change succeeds
+ required audit append fails
→ both roll back
```

## 88. Append-Only Test

The ordinary runtime role must fail when attempting to:

* Update an audit row
* delete an audit row
* rewrite an actor
* rewrite an outcome
* rewrite an event time

## 89. Cross-Tenant Test

Given Organization A context, the runtime must not retrieve Organization B audit records.

This remains true even when:

* Event ID is known
* actor ID is known
* correlation ID is known
* resource ID is known

## 90. Sensitive Metadata Test

Audit validation must reject metadata containing prohibited keys such as:

```text
password
access_token
refresh_token
mfa_secret
private_key
raw_payment_evidence
```

The final prohibited-key and data-classification policy will be implementation controlled.

## 91. AI Audit Test

An AI output-acceptance event must prove:

* Approved use case
* requesting actor
* organization
* resource
* model configuration
* prompt version
* safety result
* human reviewer
* disposition

It must not represent the model as the approving business actor.

## 92. Migration Requirements

The baseline audit migration must define:

* Audit table
* UUID primary key
* organization ownership
* event type and version
* actor fields
* resource fields
* outcome
* timestamps
* correlation
* bounded metadata
* indexes
* RLS
* runtime grants
* append-only restrictions

## 93. Schema Evolution

Adding audit metadata must preserve:

* Historical event interpretation
* event-version compatibility
* export compatibility
* retention classification
* RLS
* index strategy

Applied migrations must remain immutable.

## 94. Performance

Audit writes should remain efficient enough to participate in protected transactions.

Controls include:

* Bounded metadata
* appropriate indexes
* no external synchronous call
* batch forwarding
* partitioning only when justified
* representative load tests

## 95. Audit Volume

Habitacores must not audit every low-value read automatically.

High-volume read auditing should be reserved for:

* Restricted documents
* financial evidence
* security information
* support access
* protected exports
* highly sensitive identity data

Routine page views may remain operational logs rather than durable audit events.

## 96. Codex Boundaries

Every Codex work package introducing a material action must specify:

* Audit event type
* event version
* owning module
* actor
* organization
* resource
* operation
* outcome
* reason code
* metadata schema
* sensitivity
* retention class
* transaction behavior
* tests

Codex must not:

* Use ordinary logs instead of required audit
* write success audit after the transaction without consistency
* place secrets in metadata
* record full request bodies
* make audit rows mutable
* omit organization scope
* attribute support actions to customers
* attribute protected decisions to AI
* invent event types without registry updates
* delete audit history to fix tests
* create unrestricted audit exports

## 97. Prohibited Patterns

The following are not approved:

* Ordinary logs as the sole audit source
* database triggers as the only business audit source
* mutable audit records
* normal user deletion of audit history
* full row snapshots by default
* raw request-body storage
* password or token storage
* unrestricted JSONB metadata
* support impersonation hidden from audit
* AI represented as protected business approver
* cross-tenant audit queries
* audit export without permission and audit
* protected state changes committed without required audit
* audit data publicly exposed
* service credentials stored in audit metadata

## 98. Options Considered

### Option A — Application Logs Only

Advantages:

* Simple
* already required for operations
* easy external aggregation

Disadvantages:

* Not transactionally consistent
* may be sampled or dropped
* weak business semantics
* weak tenant ownership
* poor evidentiary reliability
* retention may be unsuitable

Decision:

Rejected.

### Option B — Database Triggers Only

Advantages:

* Captures low-level row changes
* difficult for application code to forget
* database-centered

Disadvantages:

* Limited actor and business intent
* difficult authorization context
* weak rejected-attempt coverage
* difficult AI and support context
* row changes do not equal business actions

Decision:

Rejected as the primary model.

### Option C — External Audit Service for Every Action

Advantages:

* Independent store
* separate scaling
* centralized security capability

Disadvantages:

* Network dependency in protected transactions
* distributed consistency
* higher operational complexity
* slower MVP delivery
* failure handling complexity

Decision:

Rejected for the MVP.

### Option D — Append-Only PostgreSQL Application Audit

Advantages:

* Transactional consistency
* business context
* tenant isolation
* queryability
* integration with current architecture
* low operational complexity
* future archival and forwarding support

Disadvantages:

* Primary database growth
* event governance required
* restricted access design required
* retention and archival needed later

Decision:

Approved.

## 99. Positive Consequences

* Material actions become attributable.
* financial and legal workflows gain reliable history.
* support access becomes transparent.
* authorization changes are reviewable.
* AI assistance becomes governable.
* state changes and audit can commit consistently.
* tenant isolation applies to audit history.
* future exports and investigations become possible.
* external security forwarding remains possible.

## 100. Negative Consequences

* Audit-event governance adds implementation work.
* PostgreSQL storage will grow.
* event schemas require versioning.
* access control becomes more granular.
* retention and archival require later decisions.
* high-risk denied actions may need a separate write path.
* developers must distinguish audit, logs, domain events, and outbox events.

## 101. Risks

### 101. Audit Data Leakage

Controls:

* RLS
* granular permissions
* field minimization
* sensitivity classification
* export controls
* audit-of-audit

### 101.2 Missing Audit Events

Controls:

* Application-service templates
* work-package requirements
* transaction tests
* architecture review
* event registry

### 101.3 Sensitive Data Duplication

Controls:

* Metadata schemas
* prohibited fields
* references instead of copies
* automated tests
* redaction

### 101.4 Audit Mutation

Controls:

* Append-only API
* database grants
* correction events
* administrative review
* integrity checks

### 101.5 Audit Store Growth

Controls:

* Selective event policy
* indexes
* retention classes
* archival
* partitioning when justified

### 101.6 AI Misattribution

Controls:

* Human disposition
* requester and reviewer identity
* AI operation metadata
* no AI business authority
* explicit event types

## 102. Acceptance Criterion — Transactional Audit

```text
Given:
An authorized finance reviewer verifies a payment.

When:
The transaction succeeds.

Then:
The payment state and PAYMENT_VERIFIED audit event commit together.

And:
The event identifies the reviewer, organization, payment, outcome, assurance level, and correlation ID.
```

## 103. Acceptance Criterion — Transaction Rollback

```text
Given:
A payment verification state change is valid.

And:
The required audit event cannot be appended.

When:
The transaction executes.

Then:
The payment verification does not commit.

And:
No unaudited protected state remains.
```

## 104. Acceptance Criterion — Append Only

```text
Given:
A finalized audit event exists.

When:
The ordinary application runtime attempts to update or delete it.

Then:
PostgreSQL rejects the operation.

And:
A correction requires a new linked event.
```

## 105. Acceptance Criterion — Support Attribution

```text
Given:
A support operator accesses a restricted customer document through an approved support session.

When:
The document is accessed.

Then:
The audit event identifies the support operator and support session.

And:
It does not represent the customer as the actor.
```

## 106. Acceptance Criterion — AI Assistance

```text
Given:
An AI-generated application summary is accepted with edits by an authorized reviewer.

When:
The disposition is recorded.

Then:
The audit history identifies the AI use case, model configuration, prompt version, source references, safety result, reviewer, and ACCEPTED_WITH_EDITS disposition.

And:
The reviewer remains the accountable human actor.
```

## 107. Acceptance Criterion — Sensitive Metadata

```text
Given:
An audit event is prepared.

When:
Its metadata contains an access token or MFA secret.

Then:
Validation rejects the event.

And:
The secret is not persisted.
```

## 108. Acceptance Criterion — Cross-Tenant Isolation

```text
Given:
An actor belongs only to Organization A.

And:
An audit event belongs to Organization B.

When:
The actor searches by event ID, actor ID, resource ID, or correlation ID.

Then:
The Organization B event remains inaccessible.
```

## 109. Acceptance Criterion — Unknown Fact

```text
Given:
The provider occurrence time is unavailable.

When:
The provider callback is audited.

Then:
The occurrence time remains unknown.

And:
The callback receipt time is stored separately.

And:
The system does not falsely treat receipt time as provider occurrence time.
```

## 110. Implementation Sequence

Recommended implementation order:

```text
1. Establish audit-event registry
2. establish audit-event contract and versioning
3. create append-only audit table
4. create RLS and grants
5. create audit writer
6. integrate audit writer with authorized transactions
7. establish actor and organization context
8. establish resource-reference conventions
9. establish safe metadata schemas
10. establish correction-event pattern
11. implement initial identity and membership events
12. implement finance audit events
13. implement agreement and occupancy events
14. implement restricted document access events
15. implement support-session events
16. implement Responsible AI events
17. implement audit query services
18. implement restricted exports
19. add append-only, transaction, RLS, and metadata tests
20. defer archival and hash chaining until justified
```

## 111. Deferred Decisions

This ADR does not decide:

* Exact retention periods
* legal-hold implementation
* archive-storage provider
* hash-chaining design
* external timestamping
* external SIEM provider
* exact IP-address retention
* exact user-agent normalization
* final event catalogue
* exact audit-export formats
* exact cryptographic signing
* exact database partitioning
* exact integrity-manifest schedule
* exact privacy-request treatment
* jurisdiction-specific legal evidentiary requirements
* exact incident-response integration

These require bounded implementation decisions or later ADRs.

## 112. Supersession Conditions

This ADR should be reviewed when:

* Audit volume makes PostgreSQL unsuitable as the active store.
* legal requirements require tamper-evident external storage.
* regulated customers require immutable evidence systems.
* multiple services require a shared independent audit platform.
* data residency requires regional audit storage.
* retention obligations materially change.
* an external ledger becomes authoritative.
* database-per-tenant deployment is introduced.

## 113. Final Decision Statement

Habitacores will maintain a first-class, append-only application audit-event architecture stored initially in PostgreSQL.

Application services will create authoritative audit events for material identity, membership, authorization, finance, agreement, occupancy, document, support, platform, integration, and Responsible AI operations.

Where an audit event describes a protected committed state change, the state change and required audit event will be written in the same PostgreSQL transaction.

Audit events will preserve stable event type and version, actor, organization, membership or support context, resource identity, operation, outcome, reason code, timestamps, correlation, causation, authentication assurance, sensitivity, retention class, and bounded structured metadata where applicable.

Audit records will remain distinct from ordinary application logs, domain events, outbox events, provider events, and business-resource records.

Normal application workflows will not update or delete finalized audit events. Corrections will be represented through additional linked events.

Audit metadata will be minimized. Passwords, access tokens, refresh tokens, recovery tokens, MFA secrets, private keys, unrestricted payment evidence, complete identity documents, signed-document contents, raw request bodies, and unrestricted AI prompts or outputs will not be stored in the general audit table.

Organization-scoped audit history will be protected through application authorization and PostgreSQL RLS. Sensitive audit access, export, support use, emergency access, and platform-wide investigation will themselves be auditable.

AI operations will preserve use case, model configuration, prompt version, retrieval and source references, safety validation, output validation, human reviewer, and human disposition. AI will not be represented as the accountable approving actor for protected Habitacores business decisions.

No user, administrator, provider, worker, support operator, database role, migration, logger, export tool, coding agent, or AI model may silently erase, rewrite, misattribute, disclose, fabricate, or bypass required Habitacores audit evidence.