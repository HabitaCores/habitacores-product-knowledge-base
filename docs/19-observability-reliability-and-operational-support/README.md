# Document 19 — Observability, Reliability, and Operational Support

## Document Control

| Field | Value |
|---|---|
| Product | Habitacores |
| Document | Observability, Reliability, and Operational Support |
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
| Reliability Model | Measured, observable, incident-ready, and continuously improved |
| Operational Model | Shared ownership with controlled support and escalation |
| Last Updated | 30 July 2026 |

## 1. Purpose

This document defines the observability, reliability, incident-management, operational-support, and production-service-management architecture for Habitacores.

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

into a measurable and operationally supportable production model.

This document defines:

- Observability objectives
- Reliability principles
- Logs
- Metrics
- Traces
- Correlation identifiers
- Health checks
- Service-level indicators
- Service-level objectives
- Error budgets
- Operational dashboards
- Alerting
- Alert severity
- Alert ownership
- On-call and escalation models
- Incident management
- Incident communication
- Support-case intake
- Support-case routing
- Support sessions
- Emergency access
- Production troubleshooting
- Runbooks
- Queue monitoring
- Database monitoring
- Storage monitoring
- Provider monitoring
- Webhook monitoring
- AI monitoring
- Financial-integrity monitoring
- Agreement-integrity monitoring
- Occupancy-integrity monitoring
- Document-security monitoring
- Deployment monitoring
- Migration monitoring
- Backup and recovery monitoring
- Security-event monitoring
- Business-process monitoring
- Post-incident review
- Reliability review
- Operational metrics
- Operational acceptance criteria
- MVP operational scope
- Deferred operational capabilities
- Risks
- Unresolved decisions
- Governance

This document does not replace detailed production runbooks, incident-specific playbooks, provider procedures, customer-support scripts, legal-notification procedures, or service contracts.

Those artifacts will be created during implementation and production-readiness preparation.

## 2. Observability Objectives

Habitacores observability must:

1. Detect failures quickly.
2. Explain what failed.
3. Identify the affected organization, workflow, or dependency without exposing restricted data.
4. Connect user actions to application, database, background-job, and provider activity.
5. Support tenant-safe troubleshooting.
6. Support financial-integrity monitoring.
7. Support agreement and occupancy integrity.
8. Detect provider and webhook failures.
9. Detect unusual security behavior.
10. Support production deployment validation.
11. Support database and migration monitoring.
12. Support queue and job monitoring.
13. Support file-processing monitoring.
14. Support AI-feature monitoring.
15. Support customer-support investigation.
16. Support incident response.
17. Support performance analysis.
18. Support reliability improvement.
19. Support auditability.
20. Minimize unnecessary sensitive-data collection.
21. Support future SaaS growth.
22. Provide evidence for operational and release decisions.
23. Preserve manual operations when optional dependencies fail.
24. Avoid pilot-specific observability assumptions.
25. Support clear ownership and escalation.

## 3. Reliability Objectives

Habitacores reliability must:

1. Preserve authoritative business state.
2. Avoid false success.
3. Avoid duplicate protected actions.
4. Recover safely from transient failures.
5. Degrade optional capabilities independently.
6. Maintain core read access where practical.
7. Preserve payment, agreement, occupancy, and audit history.
8. Provide clear pending and failed states.
9. Support tested restoration.
10. Provide predictable incident handling.
11. Support measurable service objectives.
12. Prevent repeated incidents through learning and corrective action.

## 4. Observability Principles

### 4.1 Instrument Critical Paths

Critical workflows must produce sufficient telemetry to understand:

- Start
- progress
- dependencies
- outcome
- failure
- retry
- authorization decision
- business-state transition

### 4.2 Correlate Across Boundaries

A single business operation should be traceable across:

```text
Client
→ Edge
→ Application
→ Domain service
→ Database
→ Queue
→ Worker
→ External provider
→ Audit event
````

### 4.3 Minimize Sensitive Data

Observability must not become an uncontrolled copy of production data.

### 4.4 Actionable Alerts

An alert should indicate:

* What is wrong
* Why it matters
* Who owns it
* What to do next
* How to confirm recovery

### 4.5 Measure User Impact

Infrastructure health alone is insufficient.

The system must monitor whether users can complete critical workflows.

### 4.6 Safe Failure

Optional dependency failure must not produce incorrect protected business state.

### 4.7 Shared Ownership

Engineering, security, operations, and product share responsibility for service reliability.

### 4.8 Continuous Improvement

Incidents, support cases, and operational trends must influence future architecture and priorities.

## 5. Observability Pillars

Habitacores will use:

```text
Logs
Metrics
Traces
Events
Health checks
Synthetic checks
Audit records
Business-process monitoring
```

Each serves a different purpose.

## 6. Logs

Logs record discrete technical and operational events.

Logs may include:

* Request start and completion
* application error
* authorization denial
* database failure
* queue failure
* provider failure
* deployment event
* migration event
* security signal
* support-session activity

Logs must not be treated as the authoritative business ledger.

## 7. Metrics

Metrics measure system behavior over time.

Examples:

* Request count
* error rate
* latency
* queue depth
* job age
* database connections
* file-scan failures
* webhook verification failures
* AI-provider latency
* payment-verification backlog

## 8. Traces

Traces connect distributed activity across components.

A trace may show:

```text
Request received
→ Authentication
→ Authorization
→ Application service
→ Database query
→ Queue submission
→ Provider request
→ Response
```

## 9. Operational Events

Operational events record significant state or system changes.

Examples:

* Deployment started
* deployment completed
* migration failed
* provider disabled
* queue paused
* backup completed
* support session activated
* incident declared

## 10. Audit Records

Audit records provide authoritative evidence of protected actions.

Examples:

* Payment verified
* refund approved
* agreement issued
* occupancy activated
* restricted document revealed
* privileged role assigned

Audit records must remain distinct from ordinary logs.

## 11. Business-Process Monitoring

Business-process monitoring evaluates whether workflows are progressing correctly.

Examples:

* Applications stuck under review
* payments pending verification too long
* agreements awaiting signature
* occupancies blocked before move-in
* work orders overdue
* refunds approved but not executed
* webhooks received but not processed

## 12. Telemetry Context

Observability records should include appropriate context such as:

* Timestamp
* environment
* application version
* correlation ID
* trace ID
* span ID
* route
* operation
* organization ID where appropriate
* actor ID where lawful
* resource type
* resource ID
* provider
* job type
* outcome
* error code
* duration

## 13. Sensitive Telemetry Restrictions

Telemetry must not include unrestricted:

* Passwords
* session tokens
* authentication cookies
* private keys
* provider secrets
* full government identification
* raw signed agreements
* payment screenshots
* bank-account evidence
* complete legal notes
* full AI prompts containing restricted data
* unrestricted webhook payloads

## 14. Correlation Identifier

Every material request should have a correlation identifier.

Preferred header:

```http
X-Correlation-Id: 01HXYZ...
```

The identifier should be available in:

* Response headers
* logs
* traces
* background jobs
* provider calls
* audit metadata
* support cases
* error screens where appropriate

## 15. Correlation-ID Generation

The server should generate a correlation identifier when:

* The client does not provide one.
* The provided identifier is invalid.
* Security policy requires replacement.

A client-provided identifier must not be trusted as proof of identity or ownership.

## 16. Trace Context

Distributed tracing should propagate approved trace context across:

* Server requests
* internal service calls
* queue messages
* workers
* provider adapters

External providers may use separate identifiers that are mapped to the internal trace.

## 17. Log Levels

Preferred levels:

* Debug
* Info
* Warning
* Error
* Critical

Production debug logging should be restricted and time limited.

## 18. Debug Logs

Debug logs may be used in:

* Local
* development
* automated test
* temporary approved production troubleshooting

They must not expose restricted data.

## 19. Information Logs

Information logs may include:

* Application startup
* successful deployment
* job completion
* provider response category
* cache invalidation
* configuration load

Information logs should not record every sensitive field.

## 20. Warning Logs

Warning logs may include:

* Recoverable provider failure
* repeated retry
* stale-record conflict
* delayed job
* cache inconsistency
* approaching capacity
* non-critical configuration issue

## 21. Error Logs

Error logs should include:

* Stable error code
* correlation ID
* affected component
* environment
* failure category
* recoverability
* safe context

## 22. Critical Logs

Critical logs should indicate:

* Service outage
* cross-tenant exposure
* payment-integrity threat
* agreement corruption
* audit failure
* backup failure with no recent valid backup
* privileged-access compromise
* unrecoverable data corruption

## 23. Structured Logging

Production logs should use structured fields rather than only free text.

Illustrative log:

```json
{
  "level": "error",
  "event": "payment.allocation.failed",
  "environment": "production",
  "organizationId": "org-id",
  "paymentId": "payment-id",
  "correlationId": "01HXYZ",
  "errorCode": "PAYMENT_BALANCE_CONFLICT",
  "durationMs": 242
}
```

## 24. Log Redaction

Redaction should occur before log storage where practical.

Redaction rules should cover:

* Authentication headers
* cookies
* tokens
* government IDs
* bank references
* email content
* document text
* provider secrets
* restricted query parameters

## 25. Log Retention

Log retention should reflect:

* Operational needs
* security investigation
* legal requirements
* privacy
* cost
* sensitivity

Final retention periods remain pending.

## 26. Metrics Architecture

Metrics should be classified as:

* Application
* Infrastructure
* Security
* Provider
* Workflow
* Business integrity
* Customer support
* Deployment
* Recovery

## 27. Application Metrics

Application metrics may include:

* Requests per second
* request count
* error rate
* latency
* route performance
* authorization denials
* cache hit rate
* active sessions
* server-rendering failures

## 28. Database Metrics

Database metrics should include:

* CPU
* memory
* storage
* active connections
* waiting connections
* query latency
* slow queries
* lock waits
* deadlocks
* transaction failures
* replication health where applicable
* backup status

## 29. Queue Metrics

Queue metrics should include:

* Queue depth
* oldest job age
* enqueue rate
* completion rate
* retry rate
* failure rate
* dead-letter count
* worker availability
* job duration

## 30. Storage Metrics

Storage metrics should include:

* Upload count
* upload failure
* scan backlog
* scan failure
* download failure
* signed-URL generation
* storage usage
* version growth
* deletion failure
* retention failure

## 31. Cache Metrics

Cache metrics may include:

* Hit rate
* miss rate
* eviction
* memory usage
* connection failures
* latency
* invalidation failure
* stale-data incidents

## 32. Provider Metrics

Provider metrics should include:

* Request count
* success rate
* failure rate
* latency
* timeout rate
* rate-limit rate
* webhook count
* invalid webhook signature
* duplicate webhook event
* reconciliation difference

## 33. Authentication Metrics

Authentication metrics may include:

* Successful login
* failed login
* password-reset request
* MFA challenge
* MFA failure
* session revocation
* suspicious-login detection
* account suspension

## 34. Authorization Metrics

Authorization metrics may include:

* Denials by permission
* cross-tenant denial
* expired assignment denial
* suspended-membership denial
* field-level denial
* self-approval denial
* support-session denial

Sudden increases require investigation.

## 35. Financial Metrics

Operational financial metrics may include:

* Payments pending verification
* payment-verification age
* rejected evidence
* duplicate-reference warnings
* verified unallocated payments
* allocation conflicts
* refund backlog
* reversal count
* reconciliation exceptions

These metrics must not be treated as financial statements without approved business logic.

## 36. Agreement Metrics

Agreement metrics may include:

* Drafts awaiting review
* agreements awaiting approval
* issued agreements awaiting signature
* signature-provider failures
* activation blockers
* agreements approaching expiry
* amendment backlog

## 37. Occupancy Metrics

Occupancy metrics may include:

* Planned occupancies
* move-ins blocked
* activation conflicts
* active occupancies
* move-outs overdue
* household-change backlog
* unit-readiness blockers

## 38. Maintenance Metrics

Maintenance metrics may include:

* New requests
* untriaged requests
* urgent requests
* overdue work orders
* unassigned work orders
* completion awaiting verification
* reopen rate
* vendor response time

## 39. Support Metrics

Support metrics may include:

* New support cases
* unresolved cases
* response time
* resolution time
* escalation count
* support sessions
* reopened cases
* repeated issue categories
* customer-impact severity

## 40. AI Metrics

AI metrics may include:

* Request count
* success rate
* timeout
* provider error
* blocked output
* malformed output
* human acceptance
* human edit rate
* human rejection
* cost
* latency
* redaction failure alerts

AI metrics must not store unnecessary prompt or output content.

## 41. Service Health Model

Health should be evaluated at multiple levels:

```text
Process health
Dependency health
Application health
Workflow health
Customer-impact health
```

## 42. Liveness Check

Liveness indicates whether an application process is running.

It should not perform expensive dependency checks.

## 43. Readiness Check

Readiness indicates whether a process can safely serve traffic.

It may evaluate:

* Required configuration
* database access
* migration compatibility
* essential dependency readiness

## 44. Dependency Health

Dependency health may include:

* Database
* object storage
* queue
* cache
* authentication provider
* payment provider
* signature provider
* email provider
* file-scanning provider
* AI provider

## 45. Partial Health

The system should represent partial degradation.

Example:

```text
Core application: Operational
AI assistance: Degraded
Email delivery: Delayed
Payment provider: Operational
```

## 46. Synthetic Monitoring

Synthetic checks should test critical user-facing paths.

Potential synthetic checks:

* Public home page
* login page
* authenticated health session
* organization dashboard
* public listing search
* application draft access
* maintenance submission endpoint
* payment record read

Synthetic checks must use safe test accounts and data.

## 47. Business Transaction Monitoring

Business transaction monitoring should verify completion of critical technical sequences without creating uncontrolled real transactions.

Examples:

* Test notification delivery
* test webhook verification
* test file upload and scan
* test queue execution
* test signed-URL expiry
* test provider sandbox callback

## 48. Service-Level Indicator

A Service-Level Indicator is a measured expression of service behavior.

Potential SLIs include:

* Availability
* latency
* error rate
* successful login rate
* successful payment-record retrieval
* successful application submission
* successful maintenance submission
* job completion time
* provider webhook processing time

## 49. Service-Level Objective

A Service-Level Objective defines a target for an SLI.

Provisional MVP examples:

```text
Core monthly availability:
99.5%

Critical synchronous request success:
99.0% or better

95th-percentile ordinary API latency:
Less than 1.5 seconds

Critical queue job completion:
95% within 5 minutes
```

Final targets remain pending production and commercial approval.

## 50. Service-Level Agreement

Habitacores must not publish contractual service-level agreements until:

* Measurement is reliable.
* exclusions are defined.
* support capability exists.
* recovery processes are tested.
* commercial and legal terms are approved.

## 51. Error Budget

An error budget represents the acceptable unreliability within an SLO.

Error-budget consumption should influence:

* Release pace
* reliability work
* provider review
* incident escalation
* technical-debt prioritization

## 52. Error-Budget Exhaustion

When a critical error budget is exhausted or likely to be exhausted:

* High-risk releases may pause.
* Reliability work may receive priority.
* unresolved incidents must be reviewed.
* ownership and recovery plan must be explicit.

## 53. Operational Dashboards

Dashboards should be role specific.

Potential dashboards:

* Executive reliability
* application health
* infrastructure health
* database health
* queue and jobs
* provider health
* security operations
* customer support
* financial workflow
* leasing workflow
* maintenance workflow
* deployment health

## 54. Executive Reliability Dashboard

May include:

* Overall availability
* major incidents
* critical customer impact
* unresolved critical defects
* provider outages
* error-budget status
* support trends
* recovery readiness

## 55. Engineering Dashboard

May include:

* Error rate
* latency
* request volume
* slow routes
* deployment version
* database health
* queue health
* job failures
* recent errors
* trace links

## 56. Financial Operations Dashboard

May include:

* Payments pending verification
* oldest pending verification
* duplicate warnings
* unallocated verified payments
* allocation failures
* refund backlog
* reconciliation exceptions

## 57. Leasing Operations Dashboard

May include:

* Applications pending review
* information-request age
* offers expiring
* agreements awaiting signature
* move-ins blocked
* renewals overdue

## 58. Maintenance Operations Dashboard

May include:

* Untriaged requests
* urgent requests
* unassigned work orders
* overdue work
* completion pending verification
* vendor failure rate

## 59. Security Operations Dashboard

May include:

* Failed logins
* suspicious sessions
* cross-tenant denials
* privileged-role changes
* restricted-document access
* support sessions
* emergency access
* invalid webhook signatures
* sensitive exports

## 60. Alerting Objectives

Alerts must:

* Detect material failure
* avoid unnecessary noise
* identify owner
* identify severity
* provide context
* provide a runbook
* define recovery
* support escalation
* avoid leaking restricted data

## 61. Alert Severity Model

### Severity 1 — Critical

Examples:

* Confirmed cross-tenant exposure
* unauthorized financial action
* production-wide outage
* agreement corruption
* active secret compromise
* audit-system failure
* unrecoverable data corruption

### Severity 2 — High

Examples:

* Major workflow unavailable
* payment allocation failing broadly
* database degradation
* provider outage affecting core operations
* backup failure without recent valid backup
* critical queue backlog

### Severity 3 — Medium

Examples:

* Partial degradation
* delayed notifications
* elevated latency
* individual provider failure with fallback
* increasing job retries
* limited user impact

### Severity 4 — Low

Examples:

* Non-critical warning
* capacity approaching threshold
* minor dashboard failure
* low-impact configuration drift

## 62. Incident Priority Versus Support Priority

Incident severity measures service or security impact.

Support-case priority measures an individual customer’s urgency.

A high-priority support case does not automatically mean a Severity 1 incident.

A repeated support pattern may reveal a broader incident.

## 63. Alert Anatomy

Each alert should contain:

* Alert name
* severity
* environment
* affected service
* detected time
* current value
* threshold
* correlation or trace context
* likely impact
* owner
* runbook
* escalation path

## 64. Alert Deduplication

Repeated alerts from the same underlying problem should be grouped where practical.

Deduplication must not conceal:

* Growing scope
* increasing severity
* new organizations affected
* financial or security escalation

## 65. Alert Suppression

Alert suppression may occur during:

* Approved maintenance
* known controlled test
* planned provider outage

Suppression must be:

* Time limited
* documented
* scoped
* visible
* automatically removed

## 66. Alert Ownership

Every production alert must have:

* Primary owner
* secondary owner
* escalation owner
* expected acknowledgment time
* runbook
* review date

## 67. On-Call Model

The initial team may use a lightweight on-call model.

The model should define:

* Primary responder
* backup responder
* escalation contact
* security contact
* provider contact
* business contact

The model should grow with production scale.

## 68. On-Call Handoff

Handoff should include:

* Active incidents
* degraded services
* risky deployments
* unresolved alerts
* provider issues
* scheduled maintenance
* pending migrations
* operational risks

## 69. Incident Declaration

An incident should be declared when:

* Significant customer impact exists.
* Financial integrity may be affected.
* Tenant isolation may be affected.
* Restricted data may be exposed.
* Core operations are unavailable.
* Recovery requires coordinated action.

## 70. Incident Lifecycle

```text
Detect
→ Acknowledge
→ Declare
→ Triage
→ Contain
→ Diagnose
→ Mitigate
→ Recover
→ Validate
→ Communicate
→ Review
→ Improve
```

## 71. Incident Roles

Potential incident roles:

* Incident Commander
* Technical Lead
* Security Lead
* Communications Lead
* Operations Lead
* Business Representative
* Scribe

One person may initially hold multiple roles, but responsibilities should remain explicit.

## 72. Incident Commander

The Incident Commander coordinates:

* Severity
* ownership
* decisions
* escalation
* status updates
* recovery validation
* closure

The Incident Commander should not become overloaded with all technical debugging.

## 73. Incident Triage

Triage should determine:

* What happened
* affected environments
* affected organizations
* affected workflows
* start time
* current severity
* security impact
* financial impact
* data-integrity impact
* immediate containment

## 74. Incident Containment

Containment actions may include:

* Disable affected feature
* activate kill switch
* revoke credentials
* suspend provider integration
* pause queue
* block malicious traffic
* enter read-only mode
* restrict support access
* isolate affected organization
* preserve evidence

## 75. Incident Mitigation

Mitigation may include:

* Roll back application
* apply feature flag
* restart worker
* fail over provider
* increase capacity
* correct configuration
* execute approved repair
* route work to manual process

## 76. Incident Recovery

Recovery requires:

* Service restored
* core workflows validated
* data integrity checked
* financial state checked where relevant
* agreement state checked where relevant
* occupancy state checked where relevant
* monitoring stable
* affected jobs reconciled
* customer impact assessed

## 77. Incident Closure

An incident should not close solely because an alert stops.

Closure requires:

* Recovery confirmed
* no active material impact
* follow-up items recorded
* communication completed
* evidence preserved
* review scheduled where required

## 78. Incident Communication

Incident communication should be:

* Timely
* factual
* scoped
* non-speculative
* privacy preserving
* updated regularly
* clear about current impact
* clear about next update

## 79. Internal Incident Update

An internal update may include:

* Incident title
* severity
* start time
* impact
* affected systems
* current action
* owner
* next update
* known risks

## 80. Customer Incident Communication

Customer communication should include:

* What capability is affected
* current user impact
* temporary workaround where available
* action required from customer
* next update
* resolution confirmation

It should not expose internal security-sensitive detail unnecessarily.

## 81. Status Page

A future public status page may report:

* Web application
* authentication
* payments
* signatures
* communications
* file processing
* scheduled maintenance

Security incidents may require controlled communication rather than immediate detailed public disclosure.

## 82. Post-Incident Review

A post-incident review should examine:

* Timeline
* impact
* detection
* response
* root causes
* contributing factors
* what worked
* what failed
* corrective actions
* owners
* deadlines

## 83. Blameless Review Principle

Reviews should focus on system improvement rather than personal blame.

Accountability remains necessary for deliberate misconduct or ignored controls.

## 84. Corrective Action Types

Corrective actions may include:

* Code fix
* architecture change
* provider change
* alert improvement
* runbook improvement
* access-control change
* training
* test addition
* process change
* documentation update

## 85. Corrective Action Tracking

Each action should include:

* Description
* owner
* priority
* due date
* status
* verification method
* linked incident

## 86. Reliability Review

A periodic reliability review should examine:

* SLO performance
* error budgets
* incidents
* support trends
* provider reliability
* capacity
* recovery readiness
* recurring manual work
* unresolved operational risks

## 87. Operational Support Model

Operational support should provide:

* User assistance
* technical investigation
* account and access support
* workflow guidance
* provider issue coordination
* escalation
* incident linkage
* knowledge-base improvement

## 88. Support Channels

Potential support channels include:

* In-app support form
* support email
* approved telephone support later
* organization administrator channel
* incident status page
* internal escalation tools

Final channels remain pending.

## 89. Support Case Categories

Support cases may include:

* Authentication
* account access
* organization membership
* property setup
* listing
* application
* payment
* agreement
* occupancy
* maintenance
* vendor
* document
* notification
* provider
* technical error
* security concern
* privacy concern

## 90. Support Case Record

A support case should include:

* Case reference
* requester
* organization
* category
* severity or priority
* summary
* affected workflow
* affected record references
* correlation IDs
* safe attachments
* status
* assigned agent
* timeline
* resolution

## 91. Support Case Priority

Potential priorities:

* Urgent
* High
* Normal
* Low

Priority should consider:

* User impact
* business deadline
* financial impact
* access impact
* number of users affected
* workaround
* security or privacy concern

## 92. Support Case Status

Suggested states:

* New
* triaged
* assigned
* awaiting customer
* awaiting internal team
* escalated
* resolved
* closed
* reopened

## 93. Support Triage

Support triage should determine:

* Is this user guidance?
* Is this a product defect?
* Is this an incident?
* Is this a security concern?
* Is this a provider issue?
* Is restricted access required?
* Is customer action required?

## 94. Support Escalation

Escalation routes may include:

* Product
* engineering
* security
* finance operations
* legal operations
* infrastructure
* provider
* organization administrator

## 95. Support Access Principle

A support case does not automatically authorize access to customer records.

Support agents should use:

* Safe metadata
* user-provided context
* correlation IDs
* logs
* approved support sessions

## 96. Support Session

A support session should include:

* Support case
* organization
* purpose
* scope
* requested duration
* approval
* actor
* start
* expiry
* access activity
* termination

## 97. Support Session Scope

Potential scopes include:

* Configuration read
* workflow metadata read
* notification status read
* provider status read
* selected document metadata read
* selected record read

Restricted document content and financial action should require stronger approval.

## 98. Support Session Approval

Approval requirements should reflect:

* Scope
* sensitivity
* duration
* organization policy
* support-agent role
* active incident

## 99. Support Session Expiry

Support sessions must:

* Expire automatically
* invalidate cached access
* revoke temporary tokens
* record termination
* prevent reuse

## 100. Support Session Monitoring

Monitoring should detect:

* Session without valid case
* excessive duration
* broad scope
* unusual data access
* restricted document access
* repeated session requests
* access after expiry

## 101. Emergency Access

Emergency access should be limited to urgent operational or security conditions.

It should require:

* Strong authentication
* explicit reason
* limited scope
* short duration
* heightened monitoring
* immutable audit
* post-access review

## 102. Production Troubleshooting

Troubleshooting should follow this order where practical:

```text
User report
→ Correlation ID
→ Application logs
→ Trace
→ Metrics
→ Audit
→ Database or job state
→ Provider state
→ Safe reproduction
→ Corrective action
```

## 103. Direct Production Changes

Direct production changes should be exceptional.

Examples requiring controlled procedure:

* Data repair
* configuration correction
* queue repair
* feature disablement
* provider mapping correction

## 104. Data-Repair Procedure

A data repair should require:

* Documented problem
* affected records
* business impact
* proposed correction
* authorization
* test or dry run
* backup or recovery consideration
* audit record
* validation
* post-repair review

## 105. Runbook Standard

Each runbook should contain:

* Purpose
* trigger
* severity
* prerequisites
* owner
* diagnosis
* containment
* recovery
* validation
* rollback or alternative
* escalation
* communication
* evidence
* review date

## 106. Required MVP Runbooks

Required runbooks include:

* Application outage
* authentication outage
* database outage
* slow database
* queue backlog
* failed background jobs
* storage outage
* malware-scan outage
* payment-provider outage
* signature-provider outage
* notification-provider outage
* webhook failure
* AI-provider outage
* failed deployment
* failed migration
* backup failure
* restore procedure
* secret compromise
* cross-tenant incident
* financial-integrity incident

## 107. Application Monitoring

Application monitoring should detect:

* Elevated error rate
* route failures
* latency increase
* rendering failures
* memory pressure
* runtime restart
* build-version mismatch
* configuration failure

## 108. Database Monitoring

Database monitoring should detect:

* Connection exhaustion
* high CPU
* high memory
* low storage
* slow queries
* lock contention
* deadlocks
* replication issue
* backup failure
* migration failure

## 109. Slow-Query Monitoring

Slow-query review should identify:

* Query
* duration
* frequency
* organization scope
* execution plan
* index need
* lock behavior
* affected workflow

Restricted values must not be copied unnecessarily into monitoring tools.

## 110. Queue Monitoring

Queue monitoring should detect:

* Increasing depth
* oldest-job age
* worker unavailable
* repeated retry
* dead-letter growth
* job-type concentration
* organization-specific backlog
* duplicate job pattern

## 111. Job Failure Classification

Job failures should be classified as:

* Transient
* permanent validation
* authorization
* expired
* duplicate
* provider failure
* malformed payload
* application defect
* unknown

## 112. Dead-Letter Operations

Dead-letter handling should support:

* Inspection
* classification
* safe retry
* cancellation
* resolution
* business reconciliation
* audit

## 113. File-Processing Monitoring

Monitoring should detect:

* Upload failure
* incomplete finalization
* scan backlog
* malware detection
* scan-provider failure
* checksum mismatch
* signed-URL failure
* unauthorized access attempt

## 114. Webhook Monitoring

Webhook monitoring should detect:

* Missing events
* invalid signature
* duplicate delivery
* replay attempt
* delayed event
* out-of-order event
* unsupported event
* processing failure
* reconciliation difference

## 115. Payment-Provider Monitoring

Monitoring should detect:

* Provider latency
* failed initiation
* missing callback
* duplicate reference
* amount mismatch
* currency mismatch
* reconciliation difference
* refund failure
* provider-account configuration issue

## 116. Signature-Provider Monitoring

Monitoring should detect:

* Request creation failure
* callback failure
* invalid signature
* duplicate callback
* signer mismatch
* document mismatch
* completion delay
* evidence-download failure

## 117. Notification Monitoring

Monitoring should detect:

* Send failure
* delivery failure
* bounce
* invalid recipient
* rate limit
* template error
* queue delay
* provider outage
* duplicate send

## 118. AI Monitoring

Monitoring should detect:

* Provider outage
* timeout
* invalid output
* schema failure
* redaction failure
* restricted-data warning
* rate limit
* excessive cost
* tenant-context mismatch
* prompt-injection signal

## 119. AI Reliability Boundary

AI monitoring must confirm that:

* AI failure does not change authoritative workflow state.
* manual operation remains available.
* output is labeled.
* protected actions remain human controlled.

## 120. Deployment Monitoring

Deployment monitoring should record:

* Commit
* build
* environment
* start
* completion
* actor or automation
* health-check result
* smoke-test result
* rollback
* feature flags
* incident linkage

## 121. Migration Monitoring

Migration monitoring should record:

* Migration identifier
* environment
* start
* duration
* rows affected where safe
* lock impact
* completion
* failure
* recovery action
* application compatibility

## 122. Backup Monitoring

Backup monitoring should detect:

* Missed backup
* incomplete backup
* backup corruption
* retention failure
* unauthorized deletion
* restore-test failure
* point-in-time recovery gap

## 123. Recovery Monitoring

Recovery exercises should record:

* Backup used
* recovery start
* recovery completion
* RPO achieved
* RTO achieved
* validation result
* integrity exceptions
* follow-up actions

## 124. Financial-Integrity Alerts

Potential alerts include:

* Allocation exceeds verified payment
* allocation exceeds charge
* currency mismatch
* duplicate payment reference
* payment verified by unauthorized actor
* self-verification conflict
* refund exceeds eligible amount
* duplicate refund execution
* reconciliation mismatch
* negative unexpected balance

## 125. Agreement-Integrity Alerts

Potential alerts include:

* Issued version changed
* signed document hash mismatch
* signature linked to wrong version
* activation without signature
* activation without approval
* amendment without source agreement
* unauthorized termination
* provider callback mismatch

## 126. Occupancy-Integrity Alerts

Potential alerts include:

* Overlapping incompatible occupancy
* activation without active agreement
* activation with unresolved payment blocker
* unit not ready
* duplicate move-in completion
* household change without approval
* move-out completed without required checks

## 127. Document-Security Alerts

Potential alerts include:

* Malware detected
* repeated restricted-document reveal
* unusual bulk download
* expired signed-URL use
* cross-tenant document request
* sensitive export
* scan bypass attempt
* government-ID exposure in search or logs

## 128. Authorization Alerts

Potential alerts include:

* Repeated cross-tenant requests
* privileged role escalation
* self-assignment attempt
* support access without session
* expired assignment use
* session access after membership revocation
* unusual platform-user activity

## 129. Workflow-Aging Monitoring

The platform should track records that remain in a state beyond expected time.

Examples:

* Application under review beyond threshold
* payment pending verification
* agreement awaiting signature
* work order overdue
* refund awaiting execution
* support case awaiting response
* export stuck processing

## 130. Workflow Thresholds

Thresholds should be configurable by:

* Workflow
* organization policy
* priority
* provider
* business hours
* environment

Thresholds should not silently change authoritative status.

## 131. Customer-Impact Analysis

Operational tooling should help determine:

* Number of users affected
* number of organizations affected
* workflows affected
* geographic concentration
* financial exposure
* data-integrity exposure
* workaround availability

## 132. Operational Business Hours

Initial operational support hours remain pending.

Time-sensitive monitoring must use:

* UTC internally
* WAT for Nigerian operations
* explicit provider time zones
* business-calendar configuration where needed

## 133. Scheduled Maintenance

Scheduled maintenance should define:

* Scope
* date
* start
* expected duration
* affected services
* customer communication
* rollback plan
* monitoring
* completion validation

## 134. Maintenance Communication

Customers should receive appropriate notice for material planned downtime.

Emergency maintenance may require shorter notice but must remain documented.

## 135. Operational Knowledge Base

The internal knowledge base should include:

* Product workflows
* role definitions
* support procedures
* error codes
* provider behavior
* runbooks
* known issues
* troubleshooting
* escalation contacts
* release notes

## 136. Known-Issue Management

A known issue should include:

* Description
* affected version
* impact
* workaround
* owner
* fix status
* expected resolution
* customer communication guidance

## 137. Operational Readiness Review

Before production launch, the review should verify:

1. Logs are structured.
2. metrics exist.
3. tracing works.
4. correlation IDs propagate.
5. dashboards exist.
6. critical alerts exist.
7. alert ownership exists.
8. runbooks exist.
9. support intake exists.
10. support escalation exists.
11. support sessions are controlled.
12. emergency access is governed.
13. backup monitoring works.
14. restore testing is complete.
15. provider monitoring works.
16. deployment monitoring works.
17. migration monitoring works.
18. security-event monitoring works.
19. business-integrity alerts exist.
20. incident communication is defined.
21. post-incident review is defined.

## 138. Operational Acceptance Criterion — Correlation

```text
Given:
A user receives an application error with a correlation identifier.

When:
Support investigates the identifier.

Then:
The identifier connects the request, application logs, trace, related job, provider call, and error record where applicable.

And:
No unrestricted sensitive content is required for the investigation.
```

## 139. Operational Acceptance Criterion — Payment Provider Outage

```text
Given:
The payment provider becomes unavailable.

When:
Provider-dependent operations fail.

Then:
An alert is generated.

And:
The provider dashboard shows degraded state.

And:
Affected payments remain pending or failed clearly.

And:
No charge is settled automatically.

And:
No agreement or occupancy activates from the failed operation.
```

## 140. Operational Acceptance Criterion — Queue Backlog

```text
Given:
The notification queue exceeds its approved age threshold.

When:
The oldest pending job crosses the threshold.

Then:
An alert is generated.

And:
The dashboard displays queue depth and oldest-job age.

And:
Operators can identify the failing job type.

And:
Financial or agreement state remains unchanged by the delay.
```

## 141. Operational Acceptance Criterion — Cross-Tenant Attempt

```text
Given:
A user in Organization A repeatedly requests Organization B resources.

When:
The authorization threshold is exceeded.

Then:
Access remains denied.

And:
No Organization B data is returned.

And:
A security signal is recorded.

And:
The event can be investigated using safe metadata.
```

## 142. Operational Acceptance Criterion — Support Session Expiry

```text
Given:
A support agent has an approved session for Organization A.

When:
The session expires.

Then:
Temporary access is revoked.

And:
Cached permissions no longer authorize access.

And:
Further customer-record access is denied.

And:
Session termination is audited.
```

## 143. Operational Acceptance Criterion — Agreement Integrity

```text
Given:
An issued agreement version is stored with a recorded checksum.

When:
A checksum mismatch is detected.

Then:
A high-severity alert is generated.

And:
The affected document is restricted from ordinary use.

And:
No activation proceeds from the mismatched version.

And:
An integrity investigation begins.
```

## 144. Operational Acceptance Criterion — Backup Failure

```text
Given:
A scheduled production backup fails.

When:
No current successful backup meets the approved recovery policy.

Then:
A high-severity alert is generated.

And:
The backup owner is notified.

And:
Recovery readiness is marked degraded.

And:
The incident remains open until a valid backup and validation exist.
```

## 145. Operational Acceptance Criterion — AI Outage

```text
Given:
The AI provider is unavailable.

When:
A reviewer requests an AI summary.

Then:
The system records the provider failure.

And:
The AI dashboard shows degraded status.

And:
The reviewer receives a clear unavailable message.

And:
Manual review remains available.

And:
No application status changes.
```

## 146. Operational Acceptance Criterion — Unknown Pilot Data

```text
Given:
Block B unit count remains unconfirmed.

When:
Operational dashboards and reports process the property hierarchy.

Then:
The unit count remains null or pending.

And:
No alert interprets the value as zero inventory.

And:
No placeholder units are generated.
```

## 147. MVP Observability Scope

The MVP must include:

### Logging

* Structured application logs
* error logs
* authorization-denial logs
* provider logs
* job logs
* deployment logs
* safe redaction

### Metrics

* Application
* database
* queue
* storage
* provider
* authentication
* financial workflow
* agreement workflow
* occupancy workflow
* maintenance workflow

### Tracing

* Request tracing
* database spans
* queue propagation
* provider spans
* correlation IDs

### Health

* Liveness
* readiness
* dependency health
* degraded-state representation
* synthetic critical checks

### Alerts

* Application outage
* high error rate
* database saturation
* queue backlog
* failed jobs
* provider outage
* webhook failures
* backup failure
* security events
* financial-integrity signals

### Support

* Support-case intake
* triage
* assignment
* escalation
* support-session controls
* knowledge base
* runbooks

### Incidents

* Severity model
* declaration
* roles
* communication
* containment
* recovery
* closure
* post-incident review

## 148. Deferred Observability Capabilities

The following may be deferred:

* Full multi-region monitoring
* advanced anomaly detection
* machine-learning-based incident prediction
* automated root-cause analysis
* customer-specific observability portals
* full 24-hour operations center
* advanced synthetic transaction network
* continuous chaos monitoring
* real-time cost attribution by tenant
* full business-intelligence observability
* automated remediation for financial workflows
* autonomous incident response
* public historical uptime reporting
* enterprise support SLAs
* dedicated customer success platform
* advanced user-experience telemetry
* session replay containing sensitive screens
* unrestricted production debugging tools

Deferred capabilities must not weaken MVP incident detection, support, backup monitoring, or integrity monitoring.

## 149. Operational Risks

### 149.1 Alert Fatigue

Risk:

Too many alerts cause responders to ignore important signals.

Controls:

* Actionable thresholds
* deduplication
* severity review
* ownership
* regular tuning

### 149.2 Insufficient Context

Risk:

Alerts do not provide enough information to respond.

Controls:

* Correlation IDs
* trace links
* runbooks
* affected component
* safe organization context

### 149.3 Sensitive Data in Telemetry

Risk:

Logs or traces expose restricted data.

Controls:

* Redaction
* schemas
* restricted access
* retention
* testing
* review

### 149.4 Monitoring Blind Spot

Risk:

Infrastructure appears healthy while workflows fail.

Controls:

* Business-process monitoring
* synthetic checks
* user-impact metrics
* support trend analysis

### 149.5 Support Overreach

Risk:

Support personnel access customer data beyond need.

Controls:

* Safe metadata
* scoped support sessions
* expiry
* audit
* monitoring

### 149.6 Provider Blindness

Risk:

External-provider degradation is detected only through customer reports.

Controls:

* Provider metrics
* synthetic checks
* webhook monitoring
* reconciliation
* alerts

### 149.7 Repair-Induced Damage

Risk:

Emergency data repair creates further inconsistency.

Controls:

* Approval
* dry run
* backup
* audit
* validation
* post-repair review

### 149.8 Business Metric Misinterpretation

Risk:

Operational metrics are treated as authoritative financial or legal reports.

Controls:

* Clear definitions
* data source labeling
* approved report logic
* reconciliation

### 149.9 Pilot Hardcoding

Risk:

Dashboards and alerts assume one organization or property.

Controls:

* Tenant-scoped telemetry
* configurable thresholds
* multi-organization test data
* neutral hierarchy

## 150. Unresolved Observability Decisions

The following remain pending:

* Final logging provider
* Final metrics provider
* Final tracing provider
* Final error-tracking provider
* Final alerting provider
* Final incident-management platform
* Final support-case platform
* Final status-page provider
* Final log-retention periods
* Final trace-retention periods
* Final metric-retention periods
* Final audit-retention periods
* Final production debug policy
* Final correlation-ID format
* Final trace-sampling policy
* Final SLI definitions
* Final SLO targets
* Final error-budget policy
* Final on-call schedule
* Final alert acknowledgment targets
* Final incident-severity definitions
* Final incident-communication cadence
* Final customer support hours
* Final support channels
* Final support priorities
* Final support-session approval model
* Final emergency-access model
* Final runbook ownership
* Final post-incident review timeline
* Final reliability-review cadence
* Final provider-health thresholds
* Final queue-age thresholds
* Final workflow-aging thresholds
* Final financial-integrity alert thresholds
* Final performance thresholds
* Final dashboard access model
* Final public status components
* Final synthetic-monitoring accounts
* Final production data-repair authority
* Final telemetry data-residency position
* Final observability cost budget
* Final pilot public names
* Final Block A unit labels
* Final Block B inventory
* Final second-building structure
* Final water-feature classification

These decisions must be resolved before the affected operational capability becomes production dependent.

## 151. Observability and Operations Governance

Changes to approved observability or operational-support architecture require:

1. Change description
2. Operational rationale
3. Affected service
4. affected environment
5. affected workflow
6. telemetry impact
7. log impact
8. metric impact
9. trace impact
10. alert impact
11. support impact
12. incident impact
13. security impact
14. privacy impact
15. tenant-isolation impact
16. financial impact
17. agreement impact
18. occupancy impact
19. provider impact
20. AI impact
21. retention impact
22. cost impact
23. runbook impact
24. testing impact
25. migration impact
26. rollback or recovery plan
27. Product Visionary approval
28. Chief Product Architect approval
29. Security review where required
30. Effective version

## 152. Operational Review Triggers

A dedicated operational review is required when introducing:

* New production service
* new provider
* new queue
* new database
* new storage system
* new payment capability
* new signature capability
* new support-access model
* new AI capability
* new security-sensitive workflow
* new customer-facing SLA
* new region
* new deployment model
* new backup model
* new incident-management platform
* new production data-repair process
* new mobile application
* new public API

## 153. Current Observability and Operational Decision

Habitacores will use a measured, correlated, tenant-safe, incident-ready operational model organized as:

```text
User or System Action
→ Correlation Identifier
→ Application Logs
→ Metrics
→ Distributed Trace
→ Domain Event
→ Audit Record
→ Alert or Dashboard
→ Support or Incident Response
→ Recovery Validation
→ Post-Incident Improvement
```

Operational support will follow:

```text
User Report
→ Support Case
→ Triage
→ Safe Metadata and Correlation Review
→ Escalation
→ Scoped Support Session Where Required
→ Resolution
→ Validation
→ Knowledge-Base Update
```

The model must preserve:

* Organization isolation
* sensitive-data minimization
* correlation
* actionable alerting
* financial integrity
* payment-verification distinction
* agreement integrity
* occupancy integrity
* provider visibility
* webhook visibility
* document security
* AI transparency
* human authority
* support-session control
* incident readiness
* backup visibility
* recovery evidence
* reliability measurement
* continuous improvement
* future SaaS extensibility

No dashboard, metric, log line, trace, health check, provider status, support case, cached result, payment screenshot, AI output, alert closure, deployment success message, pilot-team relationship, or infrastructure-level observation may independently authorize, verify, approve, allocate, sign, activate, settle, refund, reverse, terminate, reveal, or otherwise finalize a protected Habitacores business action.