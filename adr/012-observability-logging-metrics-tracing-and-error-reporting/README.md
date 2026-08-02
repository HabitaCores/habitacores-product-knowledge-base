# ADR-012 — Observability, Logging, Metrics, Tracing, and Error Reporting

## Decision Record

| Field | Value |
|---|---|
| Product | Habitacores |
| Decision | Observability, Logging, Metrics, Tracing, and Error Reporting |
| ADR | ADR-012 |
| Version | 0.1 |
| Status | Proposed |
| Decision Type | Foundational Reliability, Security, and Operations Architecture |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Telemetry Standard | OpenTelemetry |
| Primary Signals | Traces, metrics, and structured logs |
| Context Propagation | W3C-compatible distributed context through OpenTelemetry |
| Instrumentation Boundary | Provider neutral |
| Initial Application Integration | Next.js `instrumentation.ts` and shared telemetry packages |
| Error Reporting | Structured exception capture with sanitization and fingerprinting |
| Business Audit | Separate ADR-009 architecture |
| Sensitive Data Policy | Minimize, redact, classify, and prohibit secrets |
| Tenant Data in Telemetry | Restricted and minimized |
| High-Cardinality Attributes | Controlled by registry |
| Alerting Model | Service objectives, symptoms, and actionable conditions |
| AI Observability | Registered, privacy-aware, cost- and safety-aware telemetry |
| Last Updated | 1 August 2026 |

## 1. Context

Habitacores will operate:

- A Next.js web application
- Route Handlers
- Server Actions
- Server Components
- background workers
- transactional outbox dispatchers
- notification workers
- document-processing workers
- payment and reconciliation integrations
- storage operations
- webhooks
- scheduled jobs
- Responsible AI operations
- PostgreSQL transactions
- external provider adapters

These components must be observable in production.

The operating team must be able to determine:

- Whether the platform is available
- whether users can complete critical workflows
- where latency occurs
- which dependency is failing
- whether queues are falling behind
- whether tenant isolation controls are functioning
- whether database operations are healthy
- whether external providers are degraded
- whether AI operations are failing safety or schema validation
- whether errors affect one organization or the entire platform
- whether a deployment caused regression
- whether an incident requires rollback, mitigation, or escalation

Habitacores requires more than unstructured console output.

It needs a coherent telemetry architecture that supports:

- Logs
- metrics
- distributed traces
- error reporting
- dashboards
- alerts
- service objectives
- deployment correlation
- incident investigation
- capacity planning
- privacy and security controls

## 2. Decision Drivers

The decision is driven by the need to:

1. Diagnose production failures quickly.
2. correlate requests across application and worker boundaries.
3. measure critical user journeys.
4. monitor background queues and providers.
5. identify latency and bottlenecks.
6. distinguish tenant-specific from platform-wide failures.
7. preserve provider portability.
8. avoid unrestricted personal data in telemetry.
9. prevent secrets from entering logs and traces.
10. separate audit evidence from technical telemetry.
11. support structured machine-readable logs.
12. support stable error classification.
13. support deployment and version comparison.
14. support service-level indicators and objectives.
15. control high-cardinality telemetry costs.
16. monitor Responsible AI reliability, safety, and cost.
17. support incident response.
18. support development and testing.
19. support future distributed services.
20. provide clear Codex implementation boundaries.

## 3. Decision

Habitacores will use OpenTelemetry as its provider-neutral instrumentation standard.

The platform will emit and correlate:

```text
Traces
Metrics
Structured logs
Errors
Deployment metadata
Resource metadata
````

Telemetry may be exported to one or more approved observability backends.

Application and domain code must not depend directly on a proprietary observability vendor’s data model.

## 4. Architectural Flow

The approved flow is:

```text
User, worker, scheduler, integration, or provider event
→ Entry-point instrumentation
→ Correlation and trace context
→ Application and domain operations
→ Database, storage, provider, and worker spans
→ Structured metrics and logs
→ Sanitization and classification
→ OpenTelemetry export
→ Approved observability backend
→ Dashboards, alerts, and investigations
```

## 5. OpenTelemetry Boundary

Habitacores will use OpenTelemetry concepts for:

* Trace context
* spans
* metrics
* resource attributes
* baggage where specifically approved
* log correlation
* exporter abstraction

A hosting provider or telemetry vendor may provide collection and visualization, but it will not define Habitacores application semantics.

## 6. Shared Telemetry Package

The monorepo should contain a shared telemetry package.

Directional structure:

```text
packages/
  observability/
    tracing/
    metrics/
    logging/
    errors/
    redaction/
    context/
    testing/
```

The exact structure may be refined during implementation.

## 7. Next.js Instrumentation

The Next.js application will initialize server-side instrumentation through the supported `instrumentation.ts` convention.

Instrumentation setup should include:

* Resource identity
* environment
* service version
* trace exporter
* metric exporter
* log correlation
* error handlers
* approved automatic instrumentation
* custom Habitacores instrumentation

Client-side monitoring must remain separately controlled.

## 8. Worker Instrumentation

Every worker process must initialize telemetry before processing jobs.

Worker resource attributes should identify:

* Service name
* worker category
* environment
* release version
* deployment identifier
* region where applicable
* runtime version

Examples:

```text
habitacores-web
habitacores-worker-notifications
habitacores-worker-documents
habitacores-worker-integrations
habitacores-worker-ai
```

## 9. Resource Attributes

Every telemetry signal should carry stable resource metadata where available.

Examples:

```text
service.name
service.version
deployment.environment
deployment.id
cloud.region
runtime.name
runtime.version
```

Attribute names should follow approved semantic conventions where applicable.

## 10. Correlation Identifiers

Habitacores will preserve distinct identifiers for:

* Trace ID
* span ID
* correlation ID
* request ID
* causation ID
* audit-event ID
* outbox-message ID
* job-attempt ID
* provider-event ID
* AI-operation ID

These identifiers serve different purposes and must not be collapsed into one universal field.

## 11. Trace Context

Trace context should propagate across:

* Incoming HTTP requests
* Server Components
* Server Actions
* Route Handlers
* internal application services
* database operations
* outbox production
* worker dispatch
* provider adapters
* incoming and outgoing webhooks
* Responsible AI operations

## 12. Async Context Propagation

When an application transaction creates an outbox message, the message should preserve safe trace-linking information.

The worker may create a new trace linked to the originating trace rather than pretending it is one uninterrupted synchronous request.

## 13. Baggage

OpenTelemetry baggage must be used sparingly.

Baggage must not contain:

* Personal names
* email addresses
* phone numbers
* addresses
* government identifiers
* payment references
* document names
* access tokens
* organization secrets
* raw role or permission lists

Only approved low-risk routing or correlation values may be propagated.

## 14. Trace Design

A trace should represent one coherent execution path.

Examples:

```text
Submit rental application
Verify payment
Generate receipt
Process provider webhook
Render agreement
Run AI-assisted application summary
Download restricted document
```

Traces must not become permanent business records.

## 15. Span Design

Custom spans should represent meaningful technical operations.

Examples:

```text
authorization.evaluate
database.transaction
payment.verify
outbox.append
outbox.claim
notification.dispatch
storage.authorize-upload
document.validate
malware.scan
provider.webhook.process
ai.retrieve-context
ai.generate
ai.validate-output
```

## 16. Span Naming

Span names must:

* Be stable
* describe operation classes
* avoid user-provided values
* avoid resource UUIDs
* avoid URLs containing identifiers
* avoid organization names
* avoid document filenames

Prohibited:

```text
GET /organizations/019.../payments/019...
```

Preferred:

```text
GET /organizations/:organizationId/payments/:paymentId
```

## 17. Span Attributes

Permitted attributes may include:

* Stable operation name
* resource type
* result code
* environment
* provider name
* message type
* job attempt
* HTTP method
* normalized route
* database operation category
* AI use-case identifier
* document-processing category

Sensitive values require explicit approval.

## 18. Tenant Attributes

Internal organization IDs may be useful for restricted investigation but create cardinality and privacy concerns.

Therefore:

* Organization IDs must not be attached to every metric.
* organization names must not be telemetry attributes.
* traces and restricted logs may include an internal organization ID only where operationally justified.
* exported telemetry access must remain restricted.
* public dashboards must not reveal tenant identity.

## 19. User Attributes

User IDs must not appear in ordinary metrics.

Restricted traces or logs may include an internal user ID only where:

* The investigation purpose is legitimate.
* access is restricted.
* retention is controlled.
* the value is not sent to an unapproved provider.

Names, emails, and phone numbers should not be used as telemetry dimensions.

## 20. Structured Logging

Habitacores will use structured logs.

A directional log record may contain:

```text
timestamp
severity
service
environment
release
operation
message
result_code
trace_id
span_id
correlation_id
request_id
resource_type
resource_id
organization_id where approved
provider
duration_ms
error_class
error_fingerprint
```

Fields should appear only where applicable.

## 21. No Unstructured Logging as the Primary Model

Plain string logs may still exist for local development, but production telemetry should be machine parseable.

Prohibited production pattern:

```typescript
console.log("payment failed for " + email + " with payload", requestBody);
```

Preferred:

```typescript
logger.warn("Payment verification failed", {
  operation: "payment.verify",
  resultCode: "PAYMENT_EVIDENCE_UNSUPPORTED",
  paymentId,
  correlationId
});
```

## 22. Log Severity

Approved severity levels should follow a consistent mapping:

```text
TRACE
DEBUG
INFO
WARN
ERROR
FATAL
```

### TRACE and DEBUG

Detailed diagnostic information, normally disabled or sampled in production.

### INFO

Expected significant technical lifecycle events.

### WARN

Unexpected but recoverable conditions requiring awareness.

### ERROR

Operation failure requiring investigation or controlled retry.

### FATAL

Process or critical subsystem cannot continue safely.

## 23. Logging Success

Habitacores must not log every successful low-value operation at high volume.

Successful activity should be represented through:

* Metrics
* traces under sampling
* audit events where materially accountable
* selective structured logs

## 24. Audit Separation

ADR-009 audit events remain distinct from observability logs.

Audit events are designed for:

* Accountability
* evidentiary history
* legal and financial investigation
* sensitive access history
* protected business actions

Observability logs are designed for:

* Diagnosis
* reliability
* performance
* technical operations

A required audit event must not be replaced by an ordinary log.

## 25. Sensitive Data Prohibition

The following must never enter ordinary telemetry:

* Passwords
* password hashes
* access tokens
* refresh tokens
* recovery tokens
* MFA seeds
* TOTP codes
* private keys
* database credentials
* storage service credentials
* signed download URLs
* signed upload URLs
* complete payment-provider payloads
* bank-account numbers
* complete identity documents
* unrestricted payment evidence
* full agreement contents
* raw AI prompts and outputs by default

## 26. Redaction

Telemetry must pass through centralized redaction utilities.

Redaction should address:

* Headers
* query strings
* URL parameters
* request bodies
* response bodies
* exception objects
* provider errors
* database errors
* AI-provider metadata

Redaction should occur before export where possible.

## 27. Allowlist Principle

Habitacores should prefer telemetry attribute allowlists over attempting to blacklist every sensitive field.

Only specifically approved attributes should be exported for protected workflows.

## 28. HTTP Telemetry

HTTP instrumentation should capture:

* Method
* normalized route
* status code
* duration
* request size where safe
* response size where safe
* deployment
* trace context

It should not automatically capture:

* Authorization header
* cookies
* full query strings
* request bodies
* response bodies
* signed URLs

## 29. Database Telemetry

Database instrumentation should capture:

* Operation category
* table or approved logical component
* duration
* transaction result
* retry
* lock or timeout category
* connection-pool health
* trace relationship

It must not capture full SQL with personal or financial values by default.

## 30. SQL Statements

Raw SQL statement capture should be disabled or sanitized in production unless a narrowly controlled diagnostic process approves it.

Parameterized SQL may still expose:

* Table names
* schema details
* business identifiers
* comments
* query structure

Diagnostic access must remain restricted.

## 31. Database Metrics

Priority database metrics include:

```text
connection_pool_active
connection_pool_idle
connection_pool_waiting
transaction_duration
transaction_failure_total
query_duration
query_timeout_total
deadlock_total
migration_status
```

Exact metric names will follow implementation conventions.

## 32. Outbox Metrics

ADR-010 outbox monitoring should include:

```text
outbox_pending
outbox_oldest_pending_age
outbox_processing
outbox_retry_scheduled
outbox_terminal_failures
outbox_completion_duration
outbox_claim_recovery_total
```

Metrics should be grouped by bounded dimensions such as message type and environment.

## 33. Document Metrics

ADR-011 document monitoring should include:

```text
upload_intents_created
uploads_completed
uploads_expired
validation_failures
quarantined_documents
scan_pending_age
malware_detected
scan_failures
document_generation_duration
storage_reconciliation_mismatches
```

Document IDs and filenames must not become metric labels.

## 34. Authentication Metrics

Authentication monitoring may include:

```text
login_attempts
login_failures
mfa_challenges
mfa_failures
session_refresh_failures
password_recovery_requests
rate_limit_events
```

Metrics must avoid enabling account enumeration or exposing identities.

## 35. Authorization Metrics

Authorization monitoring may include:

```text
authorization_denials
cross_tenant_denials
step_up_required
separation_of_duty_denials
support_session_denials
```

Security-significant events may also create audit records under ADR-009.

## 36. Finance Metrics

Finance telemetry may include:

```text
payment_verification_duration
allocation_conflicts
refund_processing_failures
reconciliation_mismatches
provider_callback_delay
receipt_generation_delay
```

Authoritative monetary totals must not be reconstructed from observability metrics.

## 37. Provider Metrics

Each external provider adapter should expose:

```text
provider_request_total
provider_request_duration
provider_error_total
provider_timeout_total
provider_rate_limit_total
provider_circuit_state
```

Provider account identifiers must not become uncontrolled labels.

## 38. AI Metrics

Responsible AI monitoring should include:

```text
ai_operation_total
ai_operation_duration
ai_provider_error_total
ai_schema_validation_failure_total
ai_safety_validation_failure_total
ai_grounding_failure_total
ai_human_acceptance_total
ai_human_edit_total
ai_human_rejection_total
ai_estimated_input_tokens
ai_estimated_output_tokens
ai_estimated_cost
```

Metrics should be grouped by approved use-case ID and model-configuration ID rather than raw prompts.

## 39. AI Content Privacy

Raw prompts, retrieved context, model responses, and reviewer notes are not ordinary telemetry.

Where debugging requires content retention:

* Use a dedicated restricted record.
* define purpose and retention.
* minimize personal data.
* audit access.
* reference the record from telemetry using an internal identifier.

## 40. AI Quality Monitoring

AI observability should distinguish:

* Provider availability
* technical completion
* schema validity
* policy safety
* grounding quality
* human disposition
* downstream business outcome

A technically successful provider response is not automatically a valid or useful AI result.

## 41. Error Reporting

Unhandled and selected handled exceptions should be captured through the approved error-reporting boundary.

Error records should include:

* Error class
* sanitized message
* stable fingerprint
* service
* environment
* release
* normalized route or job type
* trace and correlation context
* result code
* bounded stack information
* occurrence count
* first and last seen time

## 42. Error Fingerprinting

Errors should be grouped by stable technical characteristics.

Fingerprints must not depend on:

* User names
* UUIDs
* document filenames
* provider transaction IDs
* monetary amounts
* arbitrary request values

## 43. Expected Domain Errors

Expected domain outcomes should not automatically be reported as system exceptions.

Examples:

```text
PERMISSION_DENIED
CURRENCY_MISMATCH
PAYMENT_NOT_VERIFIED
DOCUMENT_NOT_AVAILABLE
APPLICATION_ALREADY_SUBMITTED
```

These may contribute to metrics or security telemetry without producing noisy exception reports.

## 44. Unexpected Errors

Unexpected conditions include:

* Unhandled exception
* invariant violation
* impossible state
* database outage
* provider contract mismatch
* unsupported message version in active use
* repeated RLS context failure
* corrupt document metadata
* audit append failure
* outbox append failure

These require structured error reporting and appropriate alerts.

## 45. Stack Traces

Server-side stack traces may be retained in restricted error-reporting systems.

They must not be:

* Returned to end users
* written into audit metadata
* exposed across organizations
* shown in public support messages

## 46. Client-Side Errors

Client-side error reporting may capture:

* Application route
* release
* browser class
* device class
* error fingerprint
* performance timing
* trace correlation where supported

It must not capture:

* Form contents
* passwords
* uploaded files
* private document URLs
* full page HTML
* unrestricted session replay

## 47. Session Replay

Full browser session replay is not approved by default.

It requires a separate privacy and security review because it may capture:

* Personal data
* payment information
* document content
* messages
* application answers
* legal information

## 48. Metrics Design

Metrics must have:

* A clear operational question
* stable name
* defined unit
* bounded labels
* owner
* expected cardinality
* dashboard or alert use
* retention policy

Metrics must not be created merely because a value is available.

## 49. Metric Types

Approved types may include:

* Counter
* histogram
* observable gauge
* up-down counter

The chosen type must match the meaning of the measurement.

## 50. High Cardinality

High-cardinality metric dimensions are prohibited unless explicitly reviewed.

Prohibited metric labels include:

* User ID
* organization ID
* payment ID
* application ID
* document ID
* trace ID
* request ID
* email
* property address
* error message

## 51. Cardinality Registry

The observability package should maintain a registry of approved metric attributes.

New dimensions require review for:

* Expected unique values
* operational value
* privacy
* cost
* backend limits
* retention

## 52. Service-Level Indicators

Habitacores should define service-level indicators for critical capabilities.

Examples:

```text
Successful authenticated requests
Application submission success
Payment verification processing availability
Agreement-generation availability
Document upload completion
Restricted document retrieval
Background-job completion delay
Notification dispatch delay
AI operation technical completion
```

## 53. Service-Level Objectives

Service-level objectives should be established gradually using measured baseline performance.

Each objective must define:

* Service or journey
* indicator
* success condition
* measurement window
* target
* exclusions
* ownership
* alert strategy

This ADR does not set numerical targets.

## 54. Critical User Journeys

Initial critical journeys should include:

1. Register and verify account.
2. sign in with required authentication assurance.
3. create or join an organization.
4. publish and view an approved listing.
5. submit a rental application.
6. upload a required document.
7. record and verify payment.
8. allocate payment.
9. issue and retrieve a receipt.
10. generate and activate an agreement.
11. activate occupancy.
12. submit and process maintenance requests.

## 55. Synthetic Monitoring

Synthetic checks may validate selected public and authenticated journeys.

Synthetic monitoring must use:

* Dedicated synthetic accounts
* synthetic organizations
* synthetic properties
* safe test data
* restricted permissions
* environment-specific credentials

Production customer accounts must not be used.

## 56. Health Checks

Each deployable service should provide bounded health checks.

Potential categories:

```text
liveness
readiness
dependency health
worker health
```

Health responses must not disclose:

* Secrets
* connection strings
* database versions unnecessarily
* provider credentials
* internal stack traces
* customer data

## 57. Liveness

Liveness indicates whether a process can continue running.

It should not fail merely because an optional downstream provider is temporarily unavailable.

## 58. Readiness

Readiness indicates whether the service can safely receive or process work.

A worker may become unready when it cannot:

* Reach the authoritative database
* establish required context
* claim work safely
* access a mandatory dependency

## 59. Dependency Health

Dependency monitoring should distinguish:

* PostgreSQL
* identity provider
* object storage
* email provider
* payment providers
* AI providers
* malware scanner
* external webhook destinations

A degraded optional provider should not always make the entire platform appear offline.

## 60. Dashboards

Initial dashboards should cover:

* Platform overview
* web request health
* PostgreSQL health
* authentication and authorization
* outbox and workers
* documents and storage
* financial integrations
* notifications
* Responsible AI
* deployment comparison
* security signals

## 61. Alerting Principle

Alerts should be:

* Actionable
* owned
* severity classified
* deduplicated
* linked to a runbook
* based on symptoms or meaningful risks
* tested

Alerts must not be created for every isolated error.

## 62. Alert Severity

Directional severity levels include:

```text
SEV-1 — Critical platform or security impact
SEV-2 — Major degradation or high-risk workflow impact
SEV-3 — Limited degradation requiring timely investigation
SEV-4 — Informational operational issue
```

The exact incident model will be finalized later.

## 63. Priority Alerts

Priority alert conditions include:

* Cross-tenant isolation anomaly
* audit append failures
* outbox append failures
* sustained database unavailability
* authentication outage
* high payment-provider discrepancy rate
* terminal finance-job failures
* malware-scanning backlog
* restricted document exposure signal
* uncontrolled error-rate increase
* severe latency regression
* AI safety-control bypass
* secret exposure detection

## 64. Error-Budget Alerts

Once service objectives exist, Habitacores should prefer burn-rate or error-budget alerts over arbitrary static thresholds for customer-facing reliability.

Initial thresholds may remain simpler until sufficient data exists.

## 65. Deployment Correlation

Every telemetry signal should identify the deployed version where possible.

Operations must be able to compare:

* Before deployment
* after deployment
* canary or preview
* rollback version
* worker version
* migration version

## 66. Preview Environments

Preview-environment telemetry must remain distinguishable from production.

Preview traffic must not:

* Trigger production alerts
* contaminate production service objectives
* use production customer data
* use production credentials

## 67. Sampling

Trace sampling is required to control volume and cost.

Sampling should preserve:

* Errors
* high-latency traces
* security-significant traces
* selected critical workflows
* representative normal traffic

The exact sampling policy is deferred.

## 68. Tail-Based Sampling

Tail-based sampling may be considered later where the backend or collector supports retaining traces based on completed outcomes.

This may help retain:

* Errors
* slow requests
* rare workflow failures
* important provider interactions

It is not required for the MVP.

## 69. Log Sampling

Repeated low-value logs may be sampled or rate limited.

Security-significant, audit-required, and critical failure records must not be silently sampled where the controlling policy requires durable evidence.

## 70. Retention

Telemetry retention should vary by signal and sensitivity.

Retention decisions must consider:

* Investigation needs
* cost
* privacy
* security
* service objectives
* incident-response windows
* vendor limitations
* contractual requirements

Exact periods are deferred.

## 71. Telemetry Access

Access to observability systems must use:

* Named accounts
* MFA
* least privilege
* environment separation
* role-based access
* access review
* export restrictions

Not every developer requires unrestricted production telemetry.

## 72. Telemetry Export

Exporting traces, logs, or error data requires:

* Approved purpose
* bounded time range
* field review
* secure destination
* access authorization
* deletion expectation
* audit where sensitive

## 73. Vendor Selection

The initial observability backend may be:

* A hosting-platform capability
* an OpenTelemetry-compatible managed provider
* a self-managed OpenTelemetry-compatible backend
* a combination of approved services

The provider decision is deferred.

Instrumentation must remain portable.

## 74. OpenTelemetry Collector

A collector may be introduced when Habitacores needs:

* Centralized export
* filtering
* enrichment
* redaction
* batching
* retry
* multi-backend routing
* tail sampling
* telemetry governance

The MVP may export directly where operationally appropriate.

## 75. Failure of Telemetry Export

Telemetry export failure must not normally fail customer business operations.

The application should:

* Bound export time
* use asynchronous batching
* avoid blocking authoritative transactions
* drop or buffer according to policy
* expose exporter health
* avoid recursive logging failure

Required audit writes remain governed separately by ADR-009 and may fail protected transactions where required.

## 76. Local Development

Local development should support readable telemetry without requiring production credentials.

Developers should be able to inspect:

* Structured logs
* trace relationships
* error classifications
* metrics in test form
* correlation IDs

Sensitive production data must not be copied into local environments.

## 77. Testing

Observability tests should cover:

* Instrumentation initialization
* trace propagation
* async trace linking
* structured log shape
* redaction
* prohibited fields
* metric label allowlists
* exception sanitization
* provider timeout telemetry
* outbox and worker metrics
* AI telemetry boundaries
* deployment metadata
* health checks
* telemetry-export failure

## 78. Redaction Tests

Automated tests must confirm that telemetry does not contain:

```text
authorization
cookie
set-cookie
password
access_token
refresh_token
mfa_secret
signed_url
private_key
bank_account
raw_document
```

The production prohibited-field registry will be broader.

## 79. Cardinality Tests

Metric tests should reject unapproved labels such as:

* Internal UUIDs
* email addresses
* arbitrary error text
* raw route parameters
* filenames
* payment references

## 80. Trace-Link Test

```text
Given:
A web request commits an outbox message.

When:
A worker processes the message later.

Then:
The worker trace preserves a safe link to the originating context.

And:
The worker has its own execution trace and span hierarchy.
```

## 81. Error-Sanitization Test

```text
Given:
An external provider returns an error containing credentials or personal data.

When:
The error is recorded.

Then:
The exported error contains the approved classification and safe summary.

And:
The prohibited values are removed before export.
```

## 82. Audit-Separation Test

```text
Given:
An authorized reviewer verifies a payment.

When:
The transaction succeeds.

Then:
The required PAYMENT_VERIFIED audit event is appended under ADR-009.

And:
Observability telemetry records technical duration and result.

And:
Neither record is treated as a substitute for the other.
```

## 83. Cross-Tenant Privacy Test

```text
Given:
A production error affects Organization A.

When:
An operator without Organization A-sensitive telemetry access investigates general platform health.

Then:
The dashboard reveals the technical failure pattern.

And:
It does not reveal Organization A's names, users, documents, payments, or application contents.
```

## 84. AI Telemetry Test

```text
Given:
An AI summarization operation completes.

When:
Telemetry is exported.

Then:
It includes the approved use-case ID, model-configuration ID, duration,
token estimates, validation outcome, and result code.

And:
It does not include the raw prompt, private retrieved text, or raw model response.
```

## 85. Operational Runbooks

Every high-severity alert should link to a runbook containing:

* Alert meaning
* likely causes
* immediate checks
* safe mitigation
* rollback guidance
* escalation owner
* customer-impact assessment
* evidence-preservation steps
* recovery verification

## 86. Incident Correlation

During an incident, operators should be able to pivot using:

* Deployment ID
* trace ID
* correlation ID
* service
* route
* message type
* provider
* result code
* error fingerprint
* restricted organization ID where authorized

## 87. Security Monitoring

Security telemetry should support detection of:

* Repeated login failure
* MFA bypass attempts
* cross-tenant resource attempts
* support-session misuse
* service-role credential misuse
* unexpected database role
* excessive document downloads
* webhook-signature failures
* suspicious export activity
* repeated AI prompt-injection failures

Security telemetry does not replace authoritative security audit events.

## 88. Data Residency

The selected telemetry backend must be reviewed for:

* Storage region
* subprocessors
* encryption
* access
* retention
* deletion
* export
* data-classification compatibility

Restricted data must not be sent to an unapproved region or provider.

## 89. Cost Governance

Observability cost must be controlled through:

* Sampling
* bounded log volume
* label governance
* retention tiers
* event prioritization
* dashboard ownership
* unused-metric review
* provider-cost monitoring

Cost control must not disable critical security or reliability evidence without review.

## 90. Codex Boundaries

Every Codex work package introducing a production feature must specify:

* Entry-point telemetry
* critical spans
* result codes
* required metrics
* expected errors
* redaction requirements
* correlation requirements
* high-cardinality risks
* dashboards or alert impact
* audit distinction
* tests

Codex must not:

* Log request or response bodies by default
* log secrets or tokens
* log signed storage URLs
* use emails or UUIDs as metric labels
* create uncontrolled dynamic span names
* attach organization names to telemetry
* report expected domain outcomes as exceptions automatically
* replace audit events with logs
* couple domain code directly to a telemetry vendor
* block protected transactions on external telemetry export
* send raw AI content to ordinary telemetry
* enable browser session replay without approval

## 91. Prohibited Patterns

The following are not approved:

* `console.log` as the production observability architecture
* full request-body logging
* full provider-payload logging
* unrestricted SQL statement capture
* passwords or tokens in telemetry
* signed URLs in logs
* document contents in traces
* organization or user IDs as metric labels
* arbitrary error messages as metric dimensions
* vendor-specific telemetry throughout domain code
* audit records replaced by logs
* synchronous remote telemetry calls inside business transactions
* unrestricted browser session replay
* raw AI prompts and outputs in ordinary telemetry
* alerts without owners or runbooks

## 92. Options Considered

### Option A — Hosting-Provider Logs Only

Advantages:

* Minimal implementation
* immediate visibility
* integrated with deployment platform

Disadvantages:

* Weak portability
* incomplete application semantics
* limited cross-worker correlation
* provider lock-in
* insufficient metric and trace governance

Decision:

Rejected as the complete architecture.

### Option B — Proprietary SDK Throughout the Codebase

Advantages:

* Fast access to vendor-specific features
* mature dashboards and error grouping

Disadvantages:

* Deep vendor coupling
* difficult migration
* inconsistent instrumentation
* domain contamination
* duplicated semantics across tools

Decision:

Rejected.

### Option C — Custom Logging and Metrics Framework

Advantages:

* Maximum control
* tailored semantics

Disadvantages:

* Significant maintenance
* reinvents standards
* difficult ecosystem integration
* weaker portability

Decision:

Rejected.

### Option D — OpenTelemetry With Provider-Neutral Habitacores Semantics

Advantages:

* Industry-standard signal model
* provider portability
* distributed context propagation
* Next.js compatibility
* worker compatibility
* multi-backend support
* centralized governance
* future collector support

Disadvantages:

* Requires instrumentation discipline
* sampling and cardinality governance
* backend selection remains necessary
* some vendor-specific features may require adapters

Decision:

Approved.

## 93. Positive Consequences

* Production behavior becomes diagnosable.
* requests and workers can be correlated.
* deployment regressions become easier to identify.
* provider failures become measurable.
* queue and document-processing health become visible.
* AI reliability, cost, and safety failures become observable.
* telemetry remains portable across vendors.
* logs, metrics, traces, errors, and audit remain conceptually distinct.
* privacy and cardinality controls become explicit.

## 94. Negative Consequences

* Instrumentation requires ongoing engineering effort.
* telemetry volume creates cost.
* sampling may omit some successful traces.
* dashboards and alerts require ownership.
* redaction requires continuous maintenance.
* OpenTelemetry versions and exporters require governance.
* operators require training.

## 95. Risks

### 95.1 Sensitive Data Leakage

Controls:

* Allowlists
* centralized redaction
* prohibited-field tests
* restricted access
* provider review
* no body capture by default

### 95.2 Excessive Cardinality

Controls:

* Metric registry
* bounded dimensions
* tests
* cost monitoring
* no user or resource IDs in metrics

### 95.3 Alert Fatigue

Controls:

* Actionable conditions
* ownership
* runbooks
* deduplication
* symptom-based alerts
* periodic review

### 95.4 Vendor Lock-In

Controls:

* OpenTelemetry instrumentation
* provider adapters
* portable semantics
* no vendor SDK in domain code

### 95.5 Telemetry Failure Affecting Product

Controls:

* Asynchronous export
* bounded timeouts
* batching
* fail-open for ordinary telemetry
* separate required audit architecture

### 95.6 AI Content Exposure

Controls:

* Metadata-only ordinary telemetry
* dedicated restricted content records
* minimization
* access audit
* explicit retention

## 96. Acceptance Criterion — Request Correlation

```text
Given:
A user submits an application.

When:
The request executes through authorization, PostgreSQL, audit, and outbox creation.

Then:
The trace correlates the normalized HTTP route, authorization evaluation,
database transaction, audit append, and outbox append.

And:
No application answers, identity documents, or personal form values appear
in the exported trace.
```

## 97. Acceptance Criterion — Worker Correlation

```text
Given:
A receipt-generation worker processes an outbox message.

When:
The worker executes.

Then:
The telemetry identifies the worker service, message type, attempt,
correlation ID, result code, and duration.

And:
It is safely linked to the originating workflow.
```

## 98. Acceptance Criterion — Metric Cardinality

```text
Given:
A metric records document-scan outcomes.

When:
The metric is emitted.

Then:
It may include bounded labels such as environment and scan result.

And:
It does not include document ID, filename, user ID, or organization ID.
```

## 99. Acceptance Criterion — Telemetry Failure

```text
Given:
The observability exporter is temporarily unavailable.

When:
A valid rental application transaction executes.

Then:
The authoritative transaction may complete safely.

And:
Telemetry export failure is bounded and observable.

And:
Required audit and outbox records remain governed by their own
transactional requirements.
```

## 100. Implementation Sequence

Recommended implementation order:

```text
1. Establish shared observability package
2. define telemetry attribute and redaction registries
3. initialize Next.js instrumentation
4. initialize worker instrumentation
5. establish resource and deployment attributes
6. establish trace and correlation context
7. implement structured logger
8. implement centralized error sanitization and fingerprinting
9. instrument authorized database transactions
10. instrument outbox dispatch and worker attempts
11. implement initial platform, database, and worker metrics
12. instrument document, finance, authentication, and provider boundaries
13. instrument Responsible AI operations
14. create initial dashboards
15. define health and readiness checks
16. establish priority alerts and runbooks
17. add redaction and cardinality tests
18. establish service-level indicators
19. select or confirm the initial observability backend
20. consider an OpenTelemetry Collector when operationally justified
```

## 101. Deferred Decisions

This ADR does not decide:

* Exact observability backend
* exact OpenTelemetry exporter packages
* exact collector deployment
* exact trace-sampling rates
* exact log-retention periods
* exact metric-retention periods
* exact error-reporting provider
* exact alerting and on-call provider
* exact numerical service-level objectives
* exact incident-management platform
* exact browser-performance provider
* exact real-user-monitoring implementation
* exact synthetic-monitoring provider
* exact tail-sampling policy
* exact data-residency region
* exact session-replay policy
* exact telemetry cost budget

These require bounded implementation decisions or later ADRs.

## 102. Supersession Conditions

This ADR should be reviewed when:

* OpenTelemetry no longer meets platform requirements.
* telemetry volume materially exceeds the selected architecture.
* regulatory requirements change telemetry handling.
* regional telemetry isolation becomes mandatory.
* independently deployed services require a dedicated collector topology.
* customer-managed observability becomes required.
* security operations require a dedicated SIEM architecture.
* the hosting architecture moves away from Next.js and Node.js.
* the product requires advanced real-user or session analytics.

## 103. Final Decision Statement

Habitacores will use OpenTelemetry as its provider-neutral standard for distributed tracing, metrics, structured-log correlation, resource metadata, and telemetry export across the Next.js application, background workers, scheduled jobs, provider adapters, document processing, financial integrations, and Responsible AI operations.

Habitacores will maintain a shared observability package defining initialization, context propagation, structured logging, metric instruments, error sanitization, redaction, resource attributes, deployment metadata, and test utilities.

The Next.js application will initialize server-side observability through the supported instrumentation boundary. Every worker will initialize its own service identity and telemetry before processing durable work.

Trace context and correlation identifiers will propagate across synchronous application operations and be safely linked across asynchronous outbox and worker boundaries.

Observability telemetry will remain distinct from ADR-009 audit evidence, domain events, outbox messages, provider records, and authoritative business data. Logs and traces will not substitute for required audit events.

Production logs will be structured and machine readable. Metrics will use stable names, documented units, explicit ownership, and bounded low-cardinality attributes. Dynamic user, organization, resource, document, payment, request, trace, and error-message values will not become ordinary metric labels.

Passwords, tokens, cookies, MFA secrets, private keys, signed storage URLs, bank details, complete provider payloads, document contents, payment evidence, agreement contents, raw AI prompts, and raw AI outputs will not enter ordinary observability telemetry.

Habitacores will prefer attribute allowlists, centralized redaction, normalized routes, stable result codes, sanitized exceptions, and approved telemetry schemas.

Expected business outcomes will be represented through stable result codes and metrics rather than automatically being treated as system exceptions. Unexpected failures, invariant violations, dependency outages, unsupported contracts, and security-control failures will receive structured error reporting.

Critical user journeys, provider dependencies, PostgreSQL, outbox processing, document processing, authentication, authorization, finance, notifications, storage, and Responsible AI will receive measurable service indicators and operational dashboards.

Alerts will be actionable, severity classified, owner assigned, deduplicated, linked to runbooks, and based on customer-impact symptoms, service objectives, or material security and integrity risks.

Telemetry export will use bounded asynchronous behavior and will not normally block authoritative business transactions. Required audit and outbox writes remain subject to their separate transactional guarantees.

No browser, application process, worker, provider adapter, database library, support operator, coding agent, AI model, observability vendor, or hosting platform may silently expose protected data, invent authorization context, create uncontrolled telemetry cardinality, substitute logs for audit evidence, block critical transactions indefinitely, or redefine authoritative Habitacores business state through observability data.