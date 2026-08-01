# ADR-010 — Transactional Outbox, Background Jobs, and Asynchronous Processing

## Decision Record

| Field | Value |
|---|---|
| Product | Habitacores |
| Decision | Transactional Outbox, Background Jobs, and Asynchronous Processing |
| ADR | ADR-010 |
| Version | 0.1 |
| Status | Proposed |
| Decision Type | Foundational Reliability and Integration Architecture |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Authoritative Asynchronous Handoff | PostgreSQL transactional outbox |
| Initial Dispatch Model | Database polling and safe row claiming |
| Delivery Semantics | At least once |
| Handler Requirement | Idempotent |
| Worker Architecture | Bounded workers within the monorepo |
| Retry Model | Exponential backoff with jitter |
| Terminal Failure | Explicit dead-letter or failed state |
| Worker Tenant Access | Explicit organization-scoped context |
| External Calls Inside Business Transaction | Prohibited |
| In-Memory Queue as Authority | Prohibited |
| Last Updated | 1 August 2026 |

## 1. Context

Habitacores will perform work that should not occur synchronously inside a user-facing request or authoritative business transaction.

Examples include:

- Email delivery
- invitation delivery
- verification notifications
- password and security notifications
- receipt rendering
- agreement-document rendering
- document conversion
- image processing
- virus or malware scanning
- payment-provider reconciliation
- webhook delivery
- webhook processing
- maintenance notifications
- scheduled reminders
- occupancy expiry processing
- agreement renewal reminders
- retention jobs
- search indexing
- analytics extraction
- audit forwarding
- Responsible AI summarization
- Responsible AI classification
- AI evaluation
- provider synchronization
- export generation

A business action may commit successfully while a downstream provider or worker is temporarily unavailable.

Habitacores must not lose required work merely because:

- A process crashes
- a deployment occurs
- an external provider times out
- a network request fails
- a worker restarts
- a queue consumer retries
- a provider returns an ambiguous response
- the same event is delivered more than once

The system also must not claim exactly-once side effects where external systems and distributed execution cannot guarantee them.

## 2. Decision Drivers

The decision is driven by the need to:

1. Keep authoritative state and asynchronous intent consistent.
2. avoid lost notifications and integrations.
3. avoid external network calls inside critical transactions.
4. support process crashes and deployments.
5. support controlled retries.
6. support provider outages.
7. support at-least-once delivery safely.
8. prevent duplicate external effects.
9. preserve organization and actor context.
10. preserve audit and correlation.
11. support scheduled work.
12. support dead-letter handling.
13. support operational replay.
14. support webhook deduplication.
15. support worker observability.
16. support bounded AI jobs.
17. avoid an early dependency on a separate message broker.
18. preserve future migration to broker-based messaging.
19. keep domain modules independent of provider SDKs.
20. provide explicit Codex implementation boundaries.

## 3. Decision

Habitacores will use a PostgreSQL-backed transactional outbox as the authoritative handoff for asynchronous work.

When a protected business operation requires a downstream effect, the application transaction will write:

- The authoritative business state
- the required audit event
- the outbox record

inside the same PostgreSQL transaction.

Conceptual flow:

```text
Begin transaction
→ Validate actor and organization
→ Authorize command
→ Apply domain state change
→ Append audit event
→ Append outbox event
→ Commit
````

After commit:

```text
Dispatcher claims outbox event
→ Routes event to approved handler
→ Handler performs asynchronous work
→ Records outcome
→ Marks event completed or schedules retry
```

## 4. Why a Transactional Outbox

Without an outbox, a command may follow this unsafe sequence:

```text
Update database
→ Commit
→ Call email or payment provider
→ Process crashes
```

The authoritative state exists, but the required downstream effect is lost.

The reverse sequence is also unsafe:

```text
Call provider
→ Provider succeeds
→ Database transaction fails
```

The external effect occurs without matching authoritative state.

The transactional outbox records the durable intent before the database transaction commits.

## 5. Outbox Is Not the Domain Model

The outbox stores instructions or events for asynchronous processing.

It is not the source of truth for:

* Payments
* charges
* agreements
* occupancies
* applications
* memberships
* documents
* audit history

Handlers must read current authoritative records where required.

## 6. Outbox Record

A directional outbox record should include:

```text
id
message_type
message_version
aggregate_type
aggregate_id
organization_id
actor_id
membership_id
support_session_id
correlation_id
causation_id
payload
status
priority
available_at
attempt_count
maximum_attempts
claimed_at
claimed_by
lease_expires_at
completed_at
failed_at
last_error_code
created_at
```

Only fields meaningful to the message should be populated.

## 7. Outbox Identifier

Each outbox record will use a UUIDv7 identifier.

The outbox ID is distinct from:

* Domain-resource ID
* audit-event ID
* provider-event ID
* job-attempt ID
* correlation ID
* causation ID

## 8. Message Type

Message types must use stable machine-readable names.

Examples:

```text
InvitationEmailRequested
PaymentReceiptGenerationRequested
AgreementDocumentGenerationRequested
PaymentReconciliationRequested
WebhookDeliveryRequested
NotificationDispatchRequested
AIApplicationSummaryRequested
AuditForwardingRequested
```

Message names must describe intent or an established business fact.

They must not depend on route names, UI component names, or provider names unless the message is provider specific.

## 9. Message Versioning

Every message type must have a schema version.

Example:

```text
message_type = PaymentReceiptGenerationRequested
message_version = 1
```

Breaking payload changes require a new version.

Workers must reject unsupported versions safely.

## 10. Message Registry

Habitacores should maintain a message registry containing:

* Message type
* version
* owning module
* producer
* handler
* payload schema
* organization requirements
* actor requirements
* retry policy
* timeout policy
* idempotency policy
* dead-letter policy
* sensitivity classification
* observability fields

No new message type should be created without an owner.

## 11. Payload Design

Outbox payloads must remain bounded and versioned.

Payloads should normally contain:

* Stable resource identifiers
* safe immutable parameters
* operation identifiers
* required provider-routing information
* references to authoritative records

Payloads should not normally contain complete domain records.

## 12. Sensitive Payloads

Outbox payloads must not contain:

* Passwords
* access tokens
* refresh tokens
* MFA secrets
* private keys
* unrestricted government identification
* raw payment evidence
* complete signed documents
* unrestricted legal notes
* unrestricted AI prompts
* unrestricted AI outputs
* provider credentials

Sensitive content should remain in controlled storage and be referenced through stable IDs.

## 13. Organization Scope

Organization-owned messages must include:

```text
organization_id
```

The organization must come from trusted application context.

Workers must not infer organization scope from:

* Browser input
* stale JWT claims
* display names
* untrusted payload text
* AI output

## 14. Actor and Workload Context

Messages should preserve:

* Initiating actor
* organization
* membership where relevant
* support session where relevant
* correlation ID
* causation ID

The worker itself must also have a distinct workload identity.

## 15. Delivery Semantics

Habitacores will design for:

```text
At-least-once delivery
```

An outbox message may be handled more than once because of:

* Worker timeout
* process crash
* ambiguous provider result
* lease expiry
* retry
* manual replay
* duplicate provider response

Every handler must therefore be idempotent.

## 16. No Exactly-Once Claim

Habitacores will not claim universal exactly-once processing.

Exactly-once effects are generally not guaranteed across:

* PostgreSQL
* workers
* email providers
* payment providers
* object storage
* webhook receivers
* AI providers
* external document systems

The system will achieve safe practical outcomes through:

* Idempotency
* deduplication
* unique constraints
* persisted provider references
* reconciliation
* state-machine validation

## 17. Idempotency Key

Every material handler should define an idempotency key.

Examples:

```text
receipt:<receipt_id>:version:<version>
email:<notification_id>
webhook:<subscription_id>:<event_id>
ai:<ai_operation_id>
provider-reconciliation:<provider>:<transaction_id>
```

The key must be stable across retries.

## 18. Handler Idempotency

An idempotent handler must:

1. Determine whether the work has already completed.
2. return the existing result where appropriate.
3. avoid duplicate provider calls where possible.
4. preserve the prior external reference.
5. avoid duplicate domain state changes.
6. remain safe after a crash between external success and local acknowledgement.

## 19. External Provider Idempotency

Where a provider supports idempotency keys, the adapter should use the stable Habitacores idempotency key.

Provider support does not eliminate Habitacores-side idempotency and reconciliation.

## 20. Ambiguous Provider Outcomes

A provider call may time out after the provider has already accepted the request.

The handler must not blindly retry a non-idempotent operation.

Approved responses may include:

* Querying provider status
* retrying with the same provider idempotency key
* entering reconciliation state
* requiring manual review
* deferring until certainty improves

## 21. Status Model

Directional outbox states include:

```text
PENDING
CLAIMED
PROCESSING
RETRY_SCHEDULED
COMPLETED
FAILED_TERMINAL
CANCELLED
```

Exact state names may be refined during implementation.

## 22. Pending

A pending message is eligible when:

```text
status = PENDING
and
available_at <= current time
```

## 23. Claimed Work

A worker claims a bounded batch before processing.

Claiming must prevent ordinary concurrent workers from processing the same record simultaneously.

## 24. Safe Row Claiming

The dispatcher should use a safe database-claiming pattern such as:

```text
Select eligible rows
→ lock without waiting on rows claimed elsewhere
→ assign worker identity
→ establish lease expiry
→ commit claim
```

The exact SQL will be implementation reviewed.

## 25. Worker Lease

A claim should have a lease.

If a worker crashes, the message becomes eligible after the lease expires.

Lease duration must exceed the expected normal processing time while still allowing recovery.

## 26. Lease Renewal

Long-running jobs may renew a lease.

Renewal must verify:

* Same worker
* same attempt
* current lease ownership
* job remains active
* cancellation has not occurred

## 27. Attempt Record

Each processing attempt should preserve:

```text
attempt_id
outbox_id
attempt_number
worker_id
started_at
completed_at
outcome
error_code
retryable
provider_reference
duration
```

Detailed attempt records support investigation without mutating prior attempts.

## 28. Retry Policy

Retryable failures should use bounded exponential backoff with jitter.

Conceptual:

```text
delay = bounded exponential function of attempt number
+ random jitter
```

The policy should prevent synchronized retry storms.

## 29. Retryable Failures

Potential retryable failures include:

* Provider timeout
* temporary network failure
* rate limiting
* service unavailable
* transient database conflict
* temporary object-storage failure
* temporary AI-provider failure

Classification must be handler specific.

## 30. Non-Retryable Failures

Potential terminal failures include:

* Invalid payload
* unsupported message version
* organization mismatch
* missing required resource after approved lifecycle handling
* invalid recipient
* prohibited data classification
* authorization context invalid
* permanent provider rejection
* workflow cancellation

Non-retryable failures must not be retried indefinitely.

## 31. Maximum Attempts

Every message type must define a maximum-attempt policy.

The number must reflect:

* Business urgency
* provider behavior
* duplicate risk
* cost
* user impact
* manual recovery process

A global maximum is insufficient for every message type.

## 32. Dead-Letter or Terminal Failure

After retry exhaustion or a terminal error, the record should enter:

```text
FAILED_TERMINAL
```

or an equivalent dead-letter state.

The record must preserve:

* Final error classification
* attempt history
* next operational action
* notification requirement
* organization
* resource
* correlation ID

## 33. Operational Review

Terminal failures should appear in an authorized operations interface.

Operators may:

* Inspect safe error details
* view related resource
* view attempt history
* initiate approved replay
* cancel obsolete work
* escalate provider or domain issues

Operators must not edit payloads arbitrarily.

## 34. Replay

Replay is a privileged operation.

Replay must:

* Use the original or explicitly superseding message
* preserve prior attempts
* create a new attempt
* preserve correlation
* record the operator
* record the reason
* enforce current authorization
* remain idempotent
* create an audit event

## 35. Payload Correction

An invalid message payload should not normally be edited in place.

Correction options include:

* Fix the authoritative source record and enqueue a new message
* create a superseding message
* cancel the invalid message
* record a correction audit event

## 36. Cancellation

A message may be cancelled only when the business effect is no longer required or is unsafe.

Cancellation must preserve:

* Cancelling actor
* reason
* time
* current state
* whether provider execution had begun
* audit event

Cancellation cannot reverse an external effect that already occurred.

## 37. Scheduled Work

The same job infrastructure may support scheduled jobs.

Examples:

* Renewal reminders
* overdue-payment reminders
* invitation expiry
* occupancy review
* retention enforcement
* reconciliation
* AI evaluation
* security-review jobs

Scheduled work must produce a durable job or outbox record before execution.

## 38. Scheduler Authority

The scheduler decides when work becomes eligible.

It does not decide the protected business outcome.

The handler must still validate current authoritative state.

## 39. Stale Scheduled Work

A scheduled message may become obsolete.

Example:

```text
Rent reminder scheduled
→ Payment settles before execution
```

The handler should inspect current state and complete as a safe no-op where appropriate.

## 40. No-Op Completion

A handler may complete successfully without an external side effect when:

* The desired state already exists
* the work is obsolete
* the recipient is no longer eligible
* the event was superseded
* another process already completed the effect

The no-op reason should be observable.

## 41. Transaction Boundary

External network calls must not occur while holding the authoritative business transaction open.

Prohibited:

```text
Begin PostgreSQL transaction
→ lock payment
→ call email, AI, object-storage, or payment provider
→ wait
→ commit
```

Approved:

```text
Begin transaction
→ change state
→ append audit
→ append outbox
→ commit

Worker
→ perform external call
```

## 42. Worker Database Access

Workers will use explicit database roles and application-service boundaries.

Ordinary workers must:

* Not own protected tables
* not have `BYPASSRLS`
* not use unrestricted service credentials
* use explicit organization or system-operation context
* invoke approved repositories and services

## 43. Worker Authorization

Workers may act under:

* Original actor context
* explicit organization-scoped workload authority
* approved platform operation
* verified provider-event authority

A worker identity alone does not grant permission to every resource.

## 44. Current-State Revalidation

Before a material side effect, the handler should revalidate:

* Resource exists
* organization matches
* message remains applicable
* operation is not cancelled
* current state permits the action
* recipient remains valid
* required data remains accessible
* support or temporary authority remains appropriate where applicable

## 45. Worker Isolation

Worker categories should remain bounded.

Examples:

```text
Notification Worker
Document Worker
Integration Worker
Finance Reconciliation Worker
Responsible AI Worker
Retention Worker
```

One universal worker with unrestricted privileges is not approved.

## 46. Monorepo Placement

Worker code should live in dedicated applications or packages inside the approved monorepo.

Directional structure:

```text
apps/
  web/
  worker-notifications/
  worker-documents/
  worker-integrations/
  worker-ai/

packages/
  database/
  outbox/
  jobs/
  identity/
  authorization/
  audit/
  observability/
```

Exact names may vary.

## 47. Shared Domain Logic

Workers should invoke shared application and domain services.

They must not duplicate:

* Authorization rules
* money arithmetic
* state-transition rules
* agreement prerequisites
* occupancy prerequisites
* audit behavior
* tenant isolation

## 48. Provider Adapters

External providers must be accessed through adapters.

Examples:

```typescript
interface EmailProvider {
  send(message: EmailMessage, idempotencyKey: string): Promise<EmailResult>;
}
```

Domain modules must not depend directly on provider SDKs.

## 49. Provider References

Successful or ambiguous calls should preserve provider references.

Examples:

* Provider message ID
* document-generation job ID
* payment-provider event ID
* AI request ID
* webhook delivery ID

References support idempotency and reconciliation.

## 50. Incoming Webhooks

Incoming webhooks require:

* Signature verification
* provider identification
* environment validation
* event-ID deduplication
* bounded payload storage or reference
* receipt timestamp
* asynchronous processing where appropriate
* audit and observability

## 51. Webhook Receipt

Webhook receipt and webhook processing should be separable.

Directional flow:

```text
Receive webhook
→ Verify signature
→ Persist provider event
→ Acknowledge safely
→ Enqueue processing
→ Worker applies domain rules
```

## 52. Provider Event Deduplication

A unique constraint should prevent processing the same provider event repeatedly.

Potential key:

```text
provider
+ provider_account
+ environment
+ provider_event_id
```

## 53. Webhook Authority

A valid signature proves the message came through the configured provider mechanism.

It does not automatically authorize every domain transition.

The handler must validate:

* Expected provider account
* environment
* resource mapping
* event state
* amount and currency where financial
* prior processing
* permitted transition

## 54. Outgoing Webhooks

Outgoing webhooks should use:

* Stable event identity
* delivery ID
* signed payload
* destination allowlist
* retry policy
* idempotency
* delivery-attempt history
* endpoint suspension policy

## 55. Webhook Security

Webhook destinations must be protected against server-side request forgery.

Controls should include:

* Approved URL schemes
* DNS and IP validation
* restricted ports
* private-network protection
* redirect controls
* timeout
* response-size limit
* destination verification

The precise network policy is deferred.

## 56. Notification Jobs

Notification messages should reference:

* Notification record
* recipient identity
* template
* locale
* organization
* purpose
* related resource
* delivery channel

The outbox should not contain unrestricted rendered personal content where a reference is sufficient.

## 57. Notification Preferences

Before delivery, handlers should evaluate current notification policy where applicable.

Security, legal, and transactional notices may have different preference rules from optional communication.

## 58. Email Idempotency

Email retries should not create uncontrolled duplicate messages.

Controls may include:

* Notification record uniqueness
* provider idempotency where supported
* sent-state persistence
* provider-reference persistence
* bounded retries
* content versioning

## 59. Document Generation

Document jobs may include:

* Receipt generation
* agreement rendering
* notice rendering
* export creation
* thumbnail generation

The job should reference an immutable or versioned source record.

## 60. Document Immutability

A document generated for an issued receipt or agreement must be tied to the authoritative version used at generation time.

A retry must not silently render newer mutable data.

## 61. Financial Jobs

Finance-related asynchronous work may include:

* Provider-status polling
* settlement import
* reconciliation
* receipt delivery
* refund-status synchronization

Workers must use exact Money representation and deterministic finance services.

## 62. Financial Authority

A background worker may apply a deterministic financial transition only when:

* The workflow explicitly permits system execution
* provider evidence is verified
* idempotency is enforced
* current state is validated
* transaction rules are applied
* audit is recorded

AI cannot make the financial decision.

## 63. AI Jobs

Responsible AI work should execute through registered asynchronous operations where latency or reliability requires it.

Directional flow:

```text
Authorized AI request
→ AI operation record
→ Outbox message
→ AI worker
→ Permission-aware retrieval
→ minimization and redaction
→ AI Gateway
→ schema and safety validation
→ result record
→ human review where required
```

## 64. AI Job Context

An AI message should identify:

* AI operation ID
* approved use-case ID
* organization
* requesting actor
* target resource
* prompt-template version
* model-configuration reference
* retrieval-policy version
* output-schema version
* correlation ID

It must not carry unrestricted database content.

## 65. AI Retry Policy

AI retries must account for:

* Cost
* non-deterministic output
* provider rate limits
* safety failures
* schema failures
* model unavailability
* duplicate result generation

A retry must remain attached to the same AI operation unless a new operation is explicitly created.

## 66. AI Idempotency

An AI retry may produce a new candidate output, but it must not:

* Create duplicate authoritative decisions
* overwrite an accepted human-reviewed result
* trigger duplicate external actions
* bypass human review
* change the use case silently

## 67. Search Indexing

Search-index jobs must treat PostgreSQL as authoritative.

Index failure must not invalidate the committed domain record.

Search results must preserve tenant and permission boundaries.

## 68. Analytics Jobs

Analytics extraction must use:

* Approved workload identity
* purpose limitation
* minimized fields
* tenant or aggregate scope
* checkpointing
* idempotency
* retention

Analytics pipelines must not become a bypass path around RLS.

## 69. Retention Jobs

Retention workers may archive, anonymize, or delete records only under approved policy.

Each retention operation must:

* Identify policy version
* identify target scope
* respect legal holds
* preserve required audit
* remain idempotent
* support dry-run where appropriate

## 70. Job Ordering

Some messages require ordering.

Examples:

* Agreement issued before delivery
* payment verified before receipt generation
* document generated before notification
* occupancy activated before welcome communication

Ordering should be modeled through:

* Explicit dependencies
* current-state validation
* aggregate sequence where needed
* causation
* message eligibility

Global total ordering is not required.

## 71. Aggregate Sequence

Where strict per-resource ordering is necessary, messages may preserve an aggregate version or sequence number.

Handlers must reject or defer messages that conflict with current authoritative state.

## 72. Concurrency

The worker system must protect against:

* Two workers processing one message concurrently
* duplicate provider callbacks
* repeated replay
* lease expiry during long processing
* stale job state
* simultaneous cancellation and execution
* out-of-order aggregate updates

## 73. Timeouts

Every handler and provider adapter must define bounded timeouts.

An unbounded external request is prohibited.

Timeout configuration should reflect the operation and provider.

## 74. Resource Limits

Workers must apply limits for:

* Batch size
* concurrent jobs
* provider concurrency
* payload size
* document size
* AI token budget
* execution duration
* memory use
* retries

## 75. Backpressure

When downstream providers slow down, Habitacores should reduce dispatch pressure rather than create uncontrolled failures.

Controls may include:

* Concurrency limits
* provider-specific queues
* retry scheduling
* circuit breakers
* rate-limit awareness
* priority
* operational alerts

## 76. Priority

Messages may have priorities such as:

```text
CRITICAL
HIGH
NORMAL
LOW
```

Priority must not permit one organization to starve all others indefinitely.

## 77. Fairness

Dispatch should consider fairness across:

* Organizations
* message types
* providers
* priorities

A single high-volume tenant should not monopolize all worker capacity.

## 78. Circuit Breakers

A provider adapter may suspend ordinary calls after repeated failures.

During an open circuit:

* New work remains durable
* retries are deferred
* operations are alerted
* critical exceptions follow explicit policy

Circuit state must not delete or silently complete pending work.

## 79. Health Monitoring

Worker health should expose:

* Process availability
* polling success
* oldest pending message age
* pending count
* retry count
* terminal-failure count
* claim recovery count
* processing latency
* provider success rate
* provider rate-limit events

## 80. Queue-Lag Indicators

Useful indicators include:

```text
oldest_pending_age
pending_messages
retry_scheduled_messages
terminal_failures
processing_duration
completion_rate
```

Alerts should be message-type aware.

## 81. Structured Logging

Worker logs should include:

* Worker identity
* outbox ID
* message type
* attempt number
* organization ID where safe
* resource ID
* correlation ID
* result code
* duration

Logs must not include restricted payload contents or secrets.

## 82. Audit Events

Material asynchronous actions should generate audit events where required.

Examples:

* Receipt generated
* agreement delivered
* provider reconciliation completed
* terminal failure acknowledged
* message replayed
* message cancelled
* export generated
* AI output accepted or rejected

Technical retries do not necessarily require durable business audit events for every attempt.

## 83. Outbox Attempt History

Technical attempt history remains distinct from the application audit architecture.

Attempt records answer:

```text
How did processing execute technically?
```

Audit events answer:

```text
What accountable business or administrative action occurred?
```

## 84. Error Classification

Errors should use stable categories such as:

```text
TRANSIENT_NETWORK_ERROR
PROVIDER_RATE_LIMITED
PROVIDER_UNAVAILABLE
PROVIDER_REJECTED
INVALID_MESSAGE_PAYLOAD
UNSUPPORTED_MESSAGE_VERSION
RESOURCE_NOT_FOUND
ORGANIZATION_MISMATCH
JOB_OBSOLETE
IDEMPOTENCY_CONFLICT
LEASE_LOST
RETRY_EXHAUSTED
SECURITY_POLICY_VIOLATION
```

Raw provider errors may be stored only in restricted diagnostic form.

## 85. User-Facing Status

User interfaces should expose business-meaningful status rather than internal queue terminology.

Example:

```text
Receipt generation pending
Receipt ready
Delivery delayed
Delivery failed — support notified
```

Users do not need to see lease, polling, or attempt implementation details.

## 86. Security

Workers must follow:

* Least privilege
* environment isolation
* secret management
* network allowlists where applicable
* dependency governance
* audit
* tenant isolation
* no browser credentials
* no unrestricted service-role use

## 87. Secrets

Provider credentials must be loaded from approved secret storage.

They must not be placed in:

* Outbox payloads
* job-attempt records
* logs
* audit metadata
* Git
* screenshots
* coding-agent prompts
* AI requests

## 88. Environment Isolation

Development, test, staging, and production jobs must remain isolated.

Production messages must not be processed by non-production workers.

Provider endpoints and credentials must be environment specific.

## 89. Deployment

Workers must tolerate rolling deployment.

A deployment may interrupt processing, but:

* Durable messages remain
* leases expire safely
* retries remain idempotent
* no message is lost
* completed effects are reconciled

## 90. Graceful Shutdown

A worker receiving a shutdown signal should:

1. Stop claiming new work.
2. allow bounded active work to finish.
3. release or allow leases to expire safely.
4. flush telemetry.
5. terminate.

## 91. Schema Migration

Outbox schema changes must preserve compatibility with messages created by the prior application version.

Rolling deployment may temporarily involve multiple application and worker versions.

## 92. Backward Compatibility

Workers should support approved earlier message versions during the migration window.

Removing support requires proving no applicable messages remain.

## 93. Broker Migration

A dedicated broker may be introduced later.

The transactional outbox can then publish to:

* A message broker
* an event stream
* a managed queue

PostgreSQL remains the durable source of unpublished intent until publication succeeds.

## 94. Broker Introduction Criteria

A separate broker should be considered when:

* Throughput exceeds practical database polling
* independent services require decoupled consumption
* fan-out becomes extensive
* ordering or streaming requirements increase
* retention requirements differ materially
* operational scaling justifies another platform

## 95. No Premature Broker

Habitacores will not introduce Kafka, RabbitMQ, or another dedicated broker merely for architectural appearance.

The operational cost must be justified by measured requirements.

## 96. Data Retention

Completed outbox records and attempts require retention and archival policy.

They should not remain indefinitely in the hot dispatch table without operational need.

Potential approach:

* Active table for pending and recent messages
* archival or partitioning for completed history
* audit events retained according to audit policy

The exact retention period is deferred.

## 97. Cleanup

Cleanup jobs must:

* Exclude pending and retryable records
* preserve terminal-failure review requirements
* preserve required audit linkage
* respect legal or incident holds
* remain idempotent
* record cleanup metrics

## 98. Testing

Tests must cover:

* State and outbox commit together
* rollback removes both
* message schema validation
* safe concurrent claiming
* lease expiry
* worker crash recovery
* duplicate delivery
* idempotent completion
* retry scheduling
* retry exhaustion
* terminal failure
* replay
* cancellation
* provider ambiguity
* organization isolation
* worker privileges
* webhook deduplication
* AI job boundaries

## 99. Transaction Test

```text
Given:
A payment is verified and a receipt-generation request is required.

When:
The transaction succeeds.

Then:
The payment state, audit event, and outbox message commit together.
```

## 100. Rollback Test

```text
Given:
The payment state change is valid.

And:
The outbox append fails.

When:
The transaction executes.

Then:
The payment change does not commit.

And:
No unauditable or undispatchable state remains.
```

## 101. Duplicate Delivery Test

```text
Given:
A receipt-generation message is delivered twice.

When:
Both attempts execute.

Then:
Only one authoritative receipt version exists.

And:
Both attempts resolve safely to the same result.
```

## 102. Crash Recovery Test

```text
Given:
A worker claims a message.

And:
The worker crashes before marking completion.

When:
The lease expires.

Then:
Another worker may reclaim the message.

And:
The idempotent handler prevents duplicate authoritative effects.
```

## 103. Tenant Isolation Test

```text
Given:
A worker processes an Organization A message.

When:
It requests the related resource.

Then:
The transaction uses Organization A context.

And:
Organization B resources remain inaccessible.
```

## 104. Ambiguous Provider Test

```text
Given:
A provider accepts a request.

And:
The network response is lost.

When:
The handler retries.

Then:
The same idempotency key is used or provider status is reconciled.

And:
A duplicate external effect is not created blindly.
```

## 105. AI Boundary Test

```text
Given:
An AI summary message is processed.

When:
The AI worker executes.

Then:
It uses the approved use case and organization-scoped retrieval.

And:
It does not approve an application, verify payment, activate an agreement, or activate occupancy.
```

## 106. Migration Requirements

The initial migration should define:

* Outbox table
* message type and version
* organization and actor context
* correlation and causation
* bounded payload
* statuses
* eligibility time
* claim and lease fields
* attempt count
* indexes
* RLS where applicable
* runtime grants
* attempt-history table where selected

## 107. Likely Indexes

Directional indexes include:

```text
status, available_at, priority
organization_id, created_at
message_type, status, available_at
claimed_by, lease_expires_at
correlation_id
aggregate_type, aggregate_id
```

Indexes must follow actual dispatch and investigation queries.

## 108. Codex Boundaries

Every Codex work package introducing asynchronous behavior must specify:

* Message type
* message version
* producer
* handler
* payload schema
* organization context
* actor context
* transaction boundary
* idempotency key
* provider behavior
* retryable errors
* terminal errors
* maximum attempts
* timeout
* lease behavior
* audit events
* tests
* prohibited data

Codex must not:

* Perform external calls inside authoritative transactions
* use in-memory queues as durable authority
* omit idempotency
* claim exactly-once delivery
* place secrets in payloads
* create unrestricted worker roles
* use browser service credentials
* bypass RLS
* mutate completed attempt history
* retry terminal errors indefinitely
* edit failed payloads silently
* attribute AI as authoritative decision-maker
* process unsupported message versions silently

## 109. Prohibited Patterns

The following are not approved:

* Database commit followed by non-durable fire-and-forget work
* external provider call before authoritative commit
* external call inside a long database transaction
* in-memory job queue as the only record
* universal unrestricted worker
* runtime worker with `BYPASSRLS`
* non-idempotent retryable handlers
* unlimited retries
* retry without backoff
* payloads containing secrets
* duplicate provider processing without deduplication
* silent dead-letter failure
* manual payload editing
* AI jobs with unrestricted database access
* ordinary business code coupled directly to provider SDKs

## 110. Options Considered

### Option A — Synchronous External Calls

Advantages:

* Straightforward request flow
* immediate response where provider is fast
* fewer worker components

Disadvantages:

* Long user-facing latency
* provider outage blocks business transactions
* inconsistent database and provider outcomes
* difficult retries
* process-crash risk

Decision:

Rejected for effects requiring durable reliability.

### Option B — In-Memory Background Jobs

Advantages:

* Simple
* fast
* minimal infrastructure

Disadvantages:

* Work is lost on crash or deployment.
* no durable retry.
* no reliable replay.
* unsuitable for financial, legal, or document workflows.

Decision:

Rejected as authoritative infrastructure.

### Option C — Dedicated Message Broker Immediately

Advantages:

* Mature queue semantics
* high throughput
* independent consumers
* advanced routing

Disadvantages:

* Additional infrastructure
* operational overhead
* still requires transactional publication strategy
* premature for the initial architecture

Decision:

Deferred until justified.

### Option D — PostgreSQL Transactional Outbox

Advantages:

* Atomic with authoritative state
* durable
* compatible with existing infrastructure
* supports retries and replay
* low initial operational complexity
* can later publish to a broker

Disadvantages:

* Polling load
* cleanup and indexing required
* worker claiming requires care
* not intended as an unlimited event-stream platform

Decision:

Approved.

## 111. Positive Consequences

* Required asynchronous work is not lost after commit.
* external provider outages do not require holding business transactions open.
* workers survive deployments and crashes.
* duplicate execution becomes explicitly controlled.
* operations gain retry and replay visibility.
* organization and actor context remain traceable.
* AI work remains bounded and auditable.
* a future broker can be introduced without redesigning every producer.

## 112. Negative Consequences

* Worker infrastructure is required.
* handlers must be idempotent.
* outbox records require cleanup.
* message schemas require governance.
* retries and terminal failures require operations support.
* PostgreSQL receives polling and queue-management load.
* ambiguous provider outcomes require reconciliation logic.

## 113. Risks

### 113.1 Duplicate Side Effects

Controls:

* Stable idempotency keys
* provider idempotency
* persisted provider references
* unique constraints
* reconciliation
* tests

### 113.2 Lost Work

Controls:

* Same-transaction outbox append
* durable PostgreSQL records
* lease recovery
* monitoring
* rollback tests

### 113.3 Retry Storm

Controls:

* Bounded exponential backoff
* jitter
* concurrency limits
* circuit breakers
* provider-specific throttling

### 113.4 Tenant Leakage

Controls:

* Organization context
* RLS
* bounded workers
* no bypass credentials
* cross-tenant tests

### 113.5 Poison Messages

Controls:

* Schema validation
* attempt limits
* terminal failure
* operational review
* superseding messages

### 113.6 Worker Privilege Escalation

Controls:

* Dedicated roles
* least privilege
* no table ownership
* no `BYPASSRLS`
* explicit operation contexts

## 114. Acceptance Criterion — Atomic Handoff

```text
Given:
An application is submitted successfully.

And:
A confirmation notification is required.

When:
The submission transaction commits.

Then:
The application state, audit event, and notification outbox message all exist.

And:
It is impossible for the application to commit while the required outbox append silently disappears.
```

## 115. Acceptance Criterion — At-Least-Once Safety

```text
Given:
The same outbox message is handled more than once.

When:
The handler executes repeatedly.

Then:
The authoritative external or internal effect is not duplicated improperly.

And:
The prior successful result may be returned or reconciled.
```

## 116. Acceptance Criterion — Terminal Failure

```text
Given:
A message reaches its approved maximum attempts.

When:
The final retry fails.

Then:
The message enters a terminal-failure state.

And:
Its attempt history and final error remain inspectable.

And:
The failure is not silently discarded.
```

## 117. Acceptance Criterion — Replay

```text
Given:
An authorized operator determines that a terminally failed message may be retried safely.

When:
Replay is initiated.

Then:
The operator, reason, original message, new attempt, and outcome are recorded.

And:
Current authorization and idempotency rules still apply.
```

## 118. Acceptance Criterion — External Transaction Boundary

```text
Given:
A command requires an email, AI request, webhook, or document-provider call.

When:
The authoritative PostgreSQL transaction executes.

Then:
No external network call occurs inside that transaction.

And:
The external work is represented by a durable outbox message.
```

## 119. Implementation Sequence

Recommended implementation order:

```text
1. Establish message registry and versioning
2. create PostgreSQL outbox table
3. create bounded payload schemas
4. integrate outbox append with authorized transactions
5. implement dispatcher and safe row claiming
6. implement worker lease and crash recovery
7. implement attempt history
8. establish idempotency framework
9. establish retry and terminal-failure policies
10. implement operations inspection and replay
11. implement notification worker
12. implement document worker
13. implement incoming webhook persistence and deduplication
14. implement integration and reconciliation workers
15. implement Responsible AI worker
16. add queue-lag and failure monitoring
17. add concurrency, crash, duplicate, and tenant-isolation tests
18. defer dedicated broker until measured requirements justify it
```

## 120. Deferred Decisions

This ADR does not decide:

* Exact worker hosting provider
* exact polling interval
* exact batch size
* exact lease duration
* exact retry counts per message
* exact jitter algorithm
* exact circuit-breaker library
* exact scheduler technology
* exact email provider
* exact webhook-signing standard
* exact document-rendering provider
* exact queue-retention period
* exact archive strategy
* exact operations dashboard
* exact future broker
* exact worker autoscaling policy
* exact provider-specific timeout values
* exact AI provider retry policy

These require bounded implementation decisions or later ADRs.

## 121. Supersession Conditions

This ADR should be reviewed when:

* PostgreSQL polling cannot meet measured throughput.
* independently deployed services require broker-native messaging.
* message fan-out becomes extensive.
* ordered event streams become a core requirement.
* queue storage materially impacts primary database performance.
* regional asynchronous processing becomes necessary.
* a managed queue becomes operationally preferable.
* strict customer-isolated processing is introduced.

## 122. Final Decision Statement

Habitacores will use a PostgreSQL-backed transactional outbox as the authoritative handoff between committed business transactions and asynchronous processing.

When a protected state change requires downstream work, the authoritative state change, required audit event, and outbox message will be persisted inside the same PostgreSQL transaction.

External provider calls, AI requests, email delivery, webhook delivery, document generation, search indexing, reconciliation, analytics extraction, and other asynchronous effects will not occur inside authoritative business transactions.

Outbox processing will use at-least-once delivery semantics. Every handler must be idempotent and must tolerate retries, worker crashes, lease expiry, duplicate delivery, provider ambiguity, and operational replay.

Workers will use explicit workload identities, organization scope, approved application services, tenant-isolated database access, bounded provider adapters, controlled secrets, structured observability, and durable attempt history.

Retries will be bounded and scheduled using operation-specific backoff policies. Terminal failures will remain visible, inspectable, auditable, and eligible for controlled replay or cancellation.

Incoming provider events will be signature verified, environment validated, deduplicated, persisted, and processed through deterministic domain rules. A valid provider signature will not bypass Habitacores authorization, financial validation, state transitions, or audit controls.

Responsible AI jobs will operate through registered use cases, permission-aware retrieval, data minimization, the AI Gateway, schema and safety validation, and human review where required. AI workers will receive no unrestricted tenant-bypass access and will not become authoritative business decision-makers.

PostgreSQL polling is approved for the initial architecture. A dedicated message broker may be introduced only when measured throughput, fan-out, service independence, ordering, regional processing, or operational requirements justify the additional infrastructure.

No browser, application server, worker, scheduler, provider adapter, webhook, database role, support operator, coding agent, or AI model may silently discard required work, create unbounded retries, bypass idempotency, expose secrets, cross organization boundaries, misattribute actors, or perform protected external side effects without a durable Habitacores record.