# Document 18 — Infrastructure, Environments, and Deployment Architecture

## Document Control

| Field | Value |
|---|---|
| Product | Habitacores |
| Document | Infrastructure, Environments, and Deployment Architecture |
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
| Infrastructure Model | Managed-cloud-first, provider-portable where practical |
| Deployment Model | Automated, reviewed, observable, and recoverable |
| Last Updated | 30 July 2026 |

## 1. Purpose

This document defines the infrastructure, environment, deployment, availability, backup, and recovery architecture for Habitacores.

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

into an implementation-ready infrastructure and release model.

This document defines:

- Infrastructure objectives
- Infrastructure principles
- Cloud and hosting boundaries
- Application runtime
- Environment architecture
- Environment isolation
- Network architecture
- Domain and DNS architecture
- Edge services
- Database infrastructure
- Object storage
- Caching
- Background jobs
- Queues
- Scheduling
- Search infrastructure
- Integration infrastructure
- AI-provider connectivity
- Secret management
- Configuration management
- Infrastructure as code
- Continuous integration
- Continuous deployment
- Branch and release flow
- Preview environments
- Database migrations
- Feature flags
- Deployment strategies
- Rollback and forward recovery
- Backup and restoration
- Disaster recovery
- Availability
- Scalability
- Capacity planning
- Performance
- Infrastructure security
- Observability integration
- Cost management
- Data residency
- Operational ownership
- Production-readiness requirements
- Acceptance criteria
- MVP infrastructure scope
- Deferred capabilities
- Risks
- Unresolved decisions
- Governance

This document is provider neutral unless a provider is explicitly identified as provisional or proposed.

Final vendor choices will be documented through Architecture Decision Records.

## 2. Infrastructure Objectives

Habitacores infrastructure must:

1. Support secure multi-tenant SaaS operation.
2. Preserve organization-level data isolation.
3. Support annual-rent leasing workflows.
4. Protect financial, agreement, occupancy, and document data.
5. Support controlled growth from pilot to multi-organization scale.
6. Support responsive web access.
7. Support future mobile applications.
8. Support users on constrained networks.
9. Provide reliable application availability.
10. Support automated deployment.
11. Support safe database migration.
12. Support rollback or forward recovery.
13. Separate development, test, staging, and production.
14. Protect secrets and provider credentials.
15. Support secure file storage.
16. Support background jobs and provider callbacks.
17. Support monitoring and incident response.
18. Support tested backups and restoration.
19. Minimize unnecessary operational complexity.
20. Avoid premature microservice infrastructure.
21. Avoid pilot-specific hardcoding.
22. Support predictable cost management.
23. Support future provider portability where economically reasonable.
24. Produce traceable release evidence.
25. Prevent a failed optional dependency from falsifying core business state.

## 3. Infrastructure Principles

### 3.1 Managed Services First

Habitacores should prefer managed infrastructure for:

- Application hosting
- PostgreSQL
- Object storage
- Secret management
- Monitoring
- Email delivery
- Background-job execution
- Content delivery
- Security controls

Managed services reduce operational burden during early product development.

### 3.2 Modular Monolith First

The initial production architecture should use a modular monolith rather than premature microservices.

This supports:

- Simpler deployment
- Easier transactions
- Lower infrastructure cost
- Faster development
- Easier debugging
- Consistent authorization
- Simpler observability

Modules must still maintain explicit domain boundaries.

### 3.3 Stateless Application Runtime

Application instances should be stateless wherever practical.

Persistent state must reside in approved systems such as:

- PostgreSQL
- Object storage
- Queue infrastructure
- Cache
- Provider systems
- Audit storage

### 3.4 Infrastructure as Code

Production infrastructure and material environment configuration should be declared, reviewed, and versioned where practical.

### 3.5 Environment Isolation

Development and test systems must not share production credentials, databases, storage, queues, or secrets.

### 3.6 Least Privilege

Applications, users, workers, pipelines, and providers should receive the minimum required infrastructure access.

### 3.7 Automated Repeatability

Deployments, migrations, tests, and environment provisioning should be repeatable.

### 3.8 Safe Failure

Infrastructure failure must not incorrectly:

- Verify a payment
- Allocate funds
- Activate an agreement
- Activate an occupancy
- Complete a refund
- Mark a signature complete
- Approve an application

### 3.9 Observable by Default

Infrastructure components must expose sufficient logs, metrics, traces, health checks, and alerts.

### 3.10 Cost-Aware Architecture

Infrastructure decisions must consider:

- Cost
- operational burden
- reliability
- scalability
- security
- vendor dependence
- migration complexity

## 4. Initial Logical Infrastructure

```text
User Browser or Future Mobile Client
        ↓
DNS and Edge Layer
        ↓
Web Application and API Runtime
        ↓
Authentication and Authorization
        ↓
Domain Application Modules
        ↓
PostgreSQL System of Record
        ↓
Object Storage / Queue / Cache / Providers
        ↓
Background Workers
        ↓
Audit, Logs, Metrics, Alerts, and Backups
````

## 5. Proposed Initial Technology Direction

The proposed initial direction is:

* Next.js
* TypeScript
* PostgreSQL
* Managed application hosting
* Managed PostgreSQL
* Private object storage
* Managed authentication or approved identity service
* Managed queue or durable job provider
* Managed email service
* Approved payment and signature providers
* GitHub source control
* GitHub-based CI
* Automated preview and production deployments

These choices remain subject to ADR approval.

## 6. Hosting Strategy

The initial application should use managed platform hosting suitable for:

* Next.js
* Server-side rendering
* API routes
* background-compatible functions
* environment variables
* preview deployments
* TLS
* custom domains
* deployment rollback
* application logs

A provider such as Vercel may be evaluated, but is not yet approved by this document.

## 7. Infrastructure Portability

Habitacores should avoid unnecessary provider lock-in in:

* Domain logic
* authorization logic
* database schema
* storage metadata
* payment workflow state
* agreement state
* provider event records
* AI orchestration

Provider-specific adapters should remain replaceable.

Portability does not require avoiding all managed services.

## 8. Environment Architecture

Habitacores should maintain:

```text
Local
Development
Automated Test
Integration
Staging
Production
```

Optional temporary environments may include:

* Pull-request preview
* security test
* performance test
* disaster-recovery rehearsal
* provider certification

## 9. Local Environment

Purpose:

* Developer implementation
* isolated testing
* schema development
* migration preparation
* component development
* documentation validation

Local environments may use:

* Local application runtime
* local or isolated database
* provider simulators
* test storage
* synthetic users
* non-production credentials

## 10. Development Environment

Purpose:

* Shared development integration
* early demonstrations
* developer collaboration
* cross-module testing

Development must not contain unrestricted production data.

## 11. Automated Test Environment

Purpose:

* Deterministic CI execution
* isolated databases
* isolated storage fixtures
* mocked or simulated providers
* repeatable test state

Each test run should avoid dependence on prior runs.

## 12. Integration Environment

Purpose:

* Real provider sandbox testing
* webhook validation
* queue processing
* storage integration
* authentication integration
* cross-module verification

## 13. Staging Environment

Staging should be production-like in:

* Application configuration
* database engine and major version
* storage behavior
* queue behavior
* authentication behavior
* migration process
* monitoring
* provider integration pattern
* release process

Staging must remain isolated from production.

## 14. Production Environment

Production contains:

* Live user accounts
* live organization data
* financial records
* agreements
* occupancies
* sensitive documents
* provider integrations
* audit history

Production access must be tightly controlled and audited.

## 15. Environment Naming

Suggested environment identifiers:

```text
local
development
test
integration
staging
production
```

Environment identifiers must not rely on ambiguous abbreviations.

## 16. Environment Isolation Requirements

Each environment should use separate:

* Database
* storage bucket or storage namespace
* authentication project or tenant
* queue
* cache
* provider credentials
* webhook secrets
* encryption keys
* application secrets
* domain names
* monitoring context
* email sender configuration
* payment-provider account or sandbox mode

## 17. Environment Data Policy

Production data must not be copied into lower environments without:

* Documented purpose
* authorization
* anonymization
* redaction
* restricted access
* deletion schedule
* security review where required

Synthetic data is preferred.

## 18. Preview Environments

Pull requests may create temporary preview deployments.

Preview environments should:

* Use non-production data
* use non-production credentials
* prevent production webhooks
* prevent real payments
* prevent live communication by default
* expire automatically
* remain access controlled where necessary
* identify their environment visibly

## 19. Preview Environment Limitations

Preview deployments must not:

* Connect to production databases
* use production storage
* use production payment credentials
* send real tenant communications
* process live provider callbacks
* expose restricted test records publicly

## 20. Domain Architecture

Potential domain structure:

```text
habitacores.com
www.habitacores.com
app.habitacores.com
api.habitacores.com
status.habitacores.com
docs.habitacores.com
```

Final domains remain subject to approval.

## 21. Environment Domains

Potential environment domains:

```text
development.app.habitacores.com
integration.app.habitacores.com
staging.app.habitacores.com
app.habitacores.com
```

Preview environments may use provider-generated temporary domains.

## 22. DNS Management

DNS records should be:

* Centrally managed
* access controlled
* documented
* protected by MFA
* changed through review where possible
* monitored for unauthorized modification

## 23. TLS

All externally accessible environments containing authenticated or sensitive capability must use TLS.

Production HTTP traffic should redirect to HTTPS.

## 24. Edge Architecture

The edge layer may provide:

* TLS termination
* content delivery
* caching
* request routing
* rate limiting
* web application firewall
* bot controls
* geographic routing later
* security headers

## 25. Content Delivery

Public static assets may use a content-delivery network.

Examples:

* JavaScript
* CSS
* approved listing images
* public fonts
* public icons
* public documentation assets

Restricted documents must not be distributed through uncontrolled public caching.

## 26. Application Runtime

The application runtime should support:

* Web rendering
* API endpoints
* authenticated sessions
* server-side authorization
* database access
* storage integration
* queue submission
* provider adapters
* audit-event creation

## 27. Runtime Separation

Where practical, the following workloads may be separated logically or physically:

* Web requests
* API commands
* webhook receivers
* background jobs
* scheduled jobs
* AI processing
* reports
* imports
* exports

Separation should be introduced according to workload and risk, not prematurely.

## 28. Stateless Runtime Requirement

Application instances must not rely on local disk or memory for durable business state.

Local temporary storage may be used only for bounded processing.

## 29. Runtime Health Checks

The runtime should expose:

* Liveness
* readiness
* deployment version
* environment
* dependency status where appropriate

Health endpoints must not expose secrets or internal topology unnecessarily.

## 30. Database Infrastructure

PostgreSQL is the proposed authoritative database.

The database should support:

* Transactions
* foreign keys
* constraints
* indexes
* row-level security where approved
* backups
* point-in-time recovery where available
* read replicas later if needed
* connection pooling
* monitoring

## 31. Database Environment Isolation

Each environment must use a separate database or strongly isolated database project.

Production and non-production must not share ordinary application credentials.

## 32. Database Roles

Potential database roles:

* Migration role
* application runtime role
* read-only reporting role
* background-worker role
* emergency administrative role
* backup role
* monitoring role

Roles must follow least privilege.

## 33. Database Connection Security

Database connections should use:

* TLS
* secret-managed credentials
* limited network access
* connection pooling
* timeout configuration
* credential rotation
* monitoring

## 34. Database Connection Pooling

Connection pooling should prevent serverless or horizontally scaled application runtimes from exhausting PostgreSQL connections.

Final pooling architecture remains pending.

## 35. Database Transactions

Transactions are required for operations such as:

* Payment verification and related audit
* Payment allocation
* Reversal
* Refund state transition
* Agreement issue
* Agreement activation
* Occupancy activation
* Role assignment
* Support-session activation
* Webhook domain processing

## 36. Database Constraints

Critical invariants should be protected through database constraints where practical.

Examples:

* Unique provider event identifiers
* unique business identifiers
* organization ownership
* valid foreign-key relationships
* allocation integrity
* incompatible occupancy prevention where practical
* immutable version references

## 37. Database Indexing

Indexes should support:

* Organization-scoped queries
* business identifiers
* statuses
* assignments
* dates
* provider references
* payment verification queues
* agreement expiry
* occupancy activity
* maintenance queues
* audit retrieval

Indexes should be reviewed for write and storage cost.

## 38. Database Scaling

Initial scaling should prioritize:

* Query optimization
* indexes
* connection pooling
* pagination
* background processing
* caching
* archival strategy

Read replicas and partitioning may be introduced later based on evidence.

## 39. Database High Availability

Production database infrastructure should support:

* Automated failover where available
* durable storage
* backups
* monitoring
* tested restoration
* defined maintenance process

## 40. Object Storage

Object storage should contain:

* Listing media
* application documents
* payment evidence
* agreement documents
* signatures
* maintenance evidence
* vendor documents
* export files
* generated receipts

## 41. Storage Privacy

Restricted files must be private by default.

Access should use:

* Server authorization
* short-lived signed URLs
* access logging
* content-type validation
* expiration
* optional step-up authentication

## 42. Storage Path Strategy

Object paths should include stable context without exposing unnecessary personal information.

Illustrative structure:

```text
environment/
organization-id/
resource-type/
resource-id/
document-id/
version-id
```

Avoid filenames containing unrestricted:

* Full government identification
* personal addresses
* bank references
* authentication data

## 43. File Upload Architecture

Preferred flow:

```text
Client requests upload intent
→ Server authorizes request
→ Server creates constrained upload authorization
→ Client uploads directly
→ Storage event or client finalization occurs
→ Server validates metadata
→ Malware scan occurs
→ File becomes available according to status
```

## 44. File Scanning

Files should not be treated as safe before scanning and validation.

Scanning states may include:

* Pending
* passed
* failed
* timed out
* rejected
* manual review required

## 45. Storage Retention

Retention should support:

* Active records
* version history
* legal hold
* archive
* expiry
* deletion
* backup implications

Final retention periods remain pending legal and privacy review.

## 46. Cache Infrastructure

Caching may support:

* Public listing data
* reference data
* configuration
* permission metadata with safe invalidation
* rate limiting
* idempotency records
* short-lived query results

## 47. Cache Restrictions

Caching must not:

* Bypass authorization
* leak tenant data
* preserve revoked permissions
* falsify financial balance
* expose signed URLs
* preserve expired support access
* return stale agreement or occupancy authorization state

## 48. Cache Key Architecture

Cache keys should include:

* Environment
* organization
* user or permission context where required
* resource
* version
* query
* locale where relevant

## 49. Cache Invalidation

Invalidation is required after:

* Membership changes
* role changes
* assignment changes
* organization relationship changes
* payment state changes
* agreement issue or activation
* occupancy changes
* support-session termination
* configuration changes

## 50. Background Job Architecture

Background jobs may handle:

* Notifications
* file scanning
* AI assistance
* report generation
* import processing
* export generation
* provider reconciliation
* scheduled reminders
* document expiry
* agreement expiry
* renewal generation
* retry processing

## 51. Queue Requirements

The queue system should support:

* Durable jobs
* retries
* delayed execution
* dead-letter handling
* idempotency
* visibility
* monitoring
* organization context
* correlation identifiers

## 52. Job Payload Security

Job payloads should contain the minimum required data.

Prefer references such as:

```text
organizationId
resourceId
jobType
correlationId
```

over complete restricted records.

## 53. Job Idempotency

Jobs must tolerate retry without duplicating:

* Payments
* allocations
* refunds
* receipts
* agreements
* notifications
* provider events
* exports

## 54. Job Retry Policy

Retry policy should distinguish:

* Temporary network failure
* provider rate limit
* provider outage
* permanent validation failure
* authorization failure
* expired action
* duplicate event

Permanent failures should not retry indefinitely.

## 55. Dead-Letter Queue

Jobs that exceed retry policy should enter a visible failed state or dead-letter queue.

Operations must be able to:

* Inspect failure
* understand cause
* retry safely
* cancel
* resolve manually
* preserve audit evidence

## 56. Scheduled Jobs

Scheduled jobs may support:

* Offer expiry
* assignment expiry
* session expiry
* agreement reminders
* renewal preparation
* document expiry
* reconciliation
* notification retries
* cleanup of temporary files
* preview-environment expiry

## 57. Scheduled Job Time Zone

Authoritative schedules should use UTC internally.

User-facing operational deadlines should respect:

* West Africa Time
* original business date
* external party time zone where relevant

## 58. Search Infrastructure

Initial search may use PostgreSQL capabilities for:

* Business identifiers
* properties
* units
* applications
* agreements
* payments
* maintenance records

A dedicated search engine should be introduced only when required by scale or functionality.

## 59. Search Index Security

Search indexes must preserve:

* Organization isolation
* field restrictions
* deletion
* assignment scope where applicable
* document sensitivity
* environment isolation

## 60. Search Reindexing

Search infrastructure must support:

* Reindex
* incremental update
* failure recovery
* reconciliation
* schema evolution
* tenant-safe validation

## 61. Provider Integration Infrastructure

Provider integrations may include:

* Payment
* electronic signature
* email
* SMS
* WhatsApp
* identity verification
* file scanning
* AI
* analytics
* monitoring

Each integration should use an adapter boundary.

## 62. Provider Environment Separation

Providers should use:

* Sandbox credentials in non-production
* production credentials only in production
* environment-specific webhook URLs
* environment validation
* separate secrets
* separate provider identifiers where available

## 63. Provider Webhook Infrastructure

Webhook endpoints should support:

* Raw-payload verification
* signature verification
* timestamp validation
* event persistence
* duplicate detection
* asynchronous processing
* monitoring
* retry compatibility

## 64. Provider Outage Handling

During provider outage:

* Core records remain accessible.
* provider-dependent actions remain pending.
* retries follow policy.
* operators receive status.
* manual alternatives remain available where approved.
* no false success is recorded.

## 65. AI Infrastructure

AI functionality should use a controlled application service that:

* Minimizes input data
* redacts restricted fields
* scopes organization context
* applies approved prompts
* validates structured output
* records safe metadata
* enforces human review
* handles provider failure independently

## 66. AI Provider Isolation

AI-provider credentials must remain server side.

Client applications must not call privileged AI services using exposed secrets.

## 67. AI Job Separation

Long-running AI tasks should use background jobs where appropriate.

AI failure must not block:

* Application review
* payment review
* agreement management
* occupancy
* maintenance
* manual communication drafting

## 68. Secret Management

Secrets include:

* Database credentials
* authentication credentials
* encryption keys
* provider API keys
* webhook secrets
* signing keys
* storage credentials
* email credentials
* monitoring credentials

Secrets must be stored in approved secret-management systems.

## 69. Secret Handling Rules

Secrets must not appear in:

* Source control
* Markdown documents
* issue comments
* screenshots
* client bundles
* browser storage
* logs
* analytics
* test fixtures
* pull-request descriptions

## 70. Secret Rotation

Secrets should support rotation for:

* Regular lifecycle
* personnel change
* suspected compromise
* provider requirement
* environment rebuild
* incident response

## 71. Configuration Management

Configuration should be classified as:

* Public configuration
* environment configuration
* organization configuration
* secret configuration
* feature configuration
* provider configuration

## 72. Configuration Validation

The application should fail safely during startup when required configuration is:

* Missing
* malformed
* inconsistent
* assigned to the wrong environment
* using sandbox values in production
* using production values in non-production

## 73. Environment Variable Standards

Environment variables should use:

* Consistent names
* schema validation
* documentation
* environment scope
* secret classification
* no uncontrolled defaults for sensitive settings

## 74. Infrastructure as Code

Infrastructure as code may define:

* Projects
* environments
* domains
* databases
* storage
* queues
* monitoring
* secrets references
* network rules
* policies
* alerts
* backups

Final tooling remains pending.

## 75. Infrastructure Code Review

Infrastructure changes should use:

* Pull requests
* plan or preview
* peer review
* security review where required
* environment-specific approval
* deployment evidence
* rollback or recovery plan

## 76. Infrastructure Drift

Infrastructure should be monitored for differences between:

* Approved code
* provider configuration
* live resources
* security policies
* backup settings
* access controls

Unauthorized drift must be investigated.

## 77. Source-Control Architecture

The application repository should use:

* Protected `main`
* feature branches
* pull requests
* required checks
* reviewed merges
* tagged releases
* environment protections
* limited administrative bypass

## 78. Branch Strategy

A lightweight trunk-based approach is preferred.

Illustrative flow:

```text
main
↑
feature or fix branches
↑
reviewed pull request
```

Long-lived environment branches should generally be avoided.

## 79. Pull-Request Environments

A pull request may trigger:

* Static checks
* tests
* build
* security scans
* preview deployment
* migration validation
* accessibility tests
* visual tests

## 80. Continuous Integration

Continuous integration should run:

* Dependency installation
* formatting validation
* linting
* type checking
* unit tests
* component tests
* integration tests
* schema validation
* security scanning
* build verification
* artifact generation

## 81. Continuous Deployment

Continuous deployment should support:

* Approved branch source
* environment-specific deployment
* immutable deployment artifact where practical
* environment validation
* migration coordination
* health checks
* smoke tests
* rollback or forward recovery
* release traceability

## 82. Deployment Promotion

Preferred promotion model:

```text
Pull Request Preview
→ Main Branch
→ Staging Validation
→ Production Approval
→ Production Deployment
→ Post-Deployment Validation
```

## 83. Production Deployment Approval

Production deployment may require:

* Passing required checks
* approved pull request
* successful staging validation
* migration review
* release notes
* security approval for high-risk changes
* Product Visionary approval for material release scope
* deployment owner
* recovery plan

## 84. Deployment Artifact

A deployment should identify:

* Source commit
* build identifier
* environment
* release version
* migration version
* deployment time
* actor or automation
* feature flags
* provider configuration version where relevant

## 85. Release Versioning

Application releases may use semantic or date-based versioning.

Example:

```text
0.1.0
0.1.1
0.2.0
1.0.0
```

Final policy remains pending.

## 86. Deployment Strategies

Potential strategies include:

* Rolling deployment
* immutable replacement
* blue-green deployment
* canary deployment
* feature-flagged release

The initial provider may determine available options.

## 87. Rolling Deployment

A rolling deployment replaces instances gradually.

It requires compatibility between:

* Old application version
* new application version
* database schema
* queue jobs
* provider callbacks

## 88. Blue-Green Deployment

Blue-green deployment may support:

* Full environment switch
* rapid rollback
* pre-release validation
* controlled cutover

It may be deferred due to cost and complexity.

## 89. Canary Deployment

Canary deployment may release to:

* Internal users
* pilot organization
* selected percentage
* selected feature flag

Canary release must not weaken tenant isolation.

## 90. Feature Flags

Feature flags may control:

* New modules
* provider integrations
* AI capabilities
* reporting features
* pilot functionality
* organization-specific rollout
* emergency shutdown

## 91. Feature-Flag Security

Feature flags are not authorization controls.

A disabled navigation item must not make a protected API accessible.

## 92. Feature-Flag Lifecycle

Each flag should have:

* Owner
* purpose
* environment
* eligible organizations
* default state
* expiry or review date
* removal plan
* tests

## 93. Kill Switches

High-risk external capabilities may have emergency kill switches.

Examples:

* Payment provider initiation
* refund execution
* signature requests
* outbound messaging
* AI requests
* sensitive exports

A kill switch should preserve safe read access where possible.

## 94. Database Migration Strategy

Database migrations must be:

* Versioned
* reviewed
* tested
* ordered
* repeatable
* observable
* compatible with deployment strategy
* recoverable

## 95. Migration Categories

Migrations may include:

* Additive schema
* data backfill
* index
* constraint
* column change
* table change
* data transformation
* destructive cleanup

## 96. Expand-and-Contract Migration

Material schema changes should use an expand-and-contract pattern where practical:

```text
Add new structure
→ Deploy compatible application
→ Backfill data
→ Switch reads and writes
→ Validate
→ Remove old structure later
```

## 97. Migration Safety

A production migration must not:

* Fabricate unknown data
* convert null to zero without approval
* delete financial history
* overwrite issued agreements
* remove audit records
* create cross-tenant ownership errors
* block the database unexpectedly without review

## 98. Migration Lock Management

Large migrations should assess:

* Table size
* lock duration
* index strategy
* deployment timing
* application compatibility
* rollback or forward recovery
* monitoring

## 99. Migration Failure

When migration fails:

* Deployment should stop safely where possible.
* partial state must be identified.
* rollback or forward recovery must follow plan.
* operators must be alerted.
* production state must be validated.

## 100. Rollback Strategy

Application rollback should support returning to a previous compatible deployment.

Rollback is safe only when:

* Database schema remains compatible.
* queue payloads remain compatible.
* provider callbacks remain compatible.
* feature flags remain compatible.

## 101. Forward Recovery

Some database or business changes cannot safely be rolled back.

Forward recovery may require:

* Corrective migration
* feature disablement
* data repair
* provider reconciliation
* manual review
* audit event

## 102. Business-State Recovery

Recovery must not silently rewrite:

* Payments
* allocations
* refunds
* receipts
* agreements
* signatures
* occupancies
* audit events

Corrections should use explicit compensating records or approved repair procedures.

## 103. Backup Architecture

Backups should cover:

* PostgreSQL
* object storage where required
* configuration
* infrastructure definitions
* audit data
* provider mapping records
* critical operational documentation

## 104. Database Backups

Production database backups should support:

* Automated schedule
* encryption
* retention
* integrity
* monitoring
* restoration
* point-in-time recovery where available

## 105. Object-Storage Protection

Object-storage protection may use:

* Versioning
* soft deletion
* retention
* backup replication
* object lock for selected records
* checksum validation

## 106. Backup Isolation

Backup credentials and access must be separate from ordinary application access.

An application compromise should not automatically permit backup deletion.

## 107. Backup Testing

Backups are not considered reliable until restoration is tested.

Restore tests should verify:

* Data integrity
* organization ownership
* financial records
* agreement versions
* object links
* audit continuity
* application compatibility

## 108. Recovery Point Objective

Recovery Point Objective defines acceptable data loss measured in time.

A provisional target for authoritative production data may be:

```text
RPO: 15 minutes or better
```

The final value remains pending provider and cost analysis.

## 109. Recovery Time Objective

Recovery Time Objective defines acceptable restoration time.

A provisional target for core production service may be:

```text
RTO: 4 hours
```

The final value remains pending business and infrastructure approval.

## 110. Disaster-Recovery Scope

Disaster recovery should consider:

* Application-hosting failure
* database failure
* storage failure
* provider failure
* region failure
* credential compromise
* accidental deletion
* malicious change
* deployment failure
* data corruption

## 111. Disaster-Recovery Plan

A disaster-recovery plan should define:

* Scenario
* detection
* incident commander
* recovery owner
* communication
* system priority
* RPO
* RTO
* recovery steps
* validation
* customer notification
* evidence
* post-event review

## 112. Disaster-Recovery Testing

Recovery exercises should occur:

* Before production launch
* after material infrastructure change
* periodically after launch
* after failed recovery
* after major provider migration

## 113. Availability Architecture

Availability should be supported through:

* Managed hosting
* health checks
* redundant runtime capacity
* managed database availability
* durable storage
* queue retry
* provider isolation
* monitoring
* tested recovery

## 114. Availability Targets

A provisional MVP service objective may be:

```text
Monthly service availability target:
99.5%
```

This target excludes approved maintenance and may be revised before contractual commitments.

## 115. Critical Capability Availability

Critical capabilities include:

* Authentication
* organization access
* payment records
* agreement access
* occupancy records
* maintenance submission
* audit

Optional AI and analytics features may have lower availability without blocking core operations.

## 116. Graceful Degradation

Examples:

### AI Outage

* Manual review remains available.
* AI controls show unavailable.
* no workflow state changes.

### Notification Outage

* Business action completes where valid.
* notification is queued or marked failed.
* authoritative status remains visible in-app.

### Payment Provider Outage

* Provider-dependent action remains pending.
* manual verification may remain available where approved.
* no false payment confirmation occurs.

### Search Outage

* Direct navigation and scoped list views remain available where practical.

## 117. Maintenance Mode

The application may support:

* Read-only mode
* feature-specific maintenance
* full maintenance page
* provider-specific outage banner
* admin-only access

Maintenance mode must not expose internal details.

## 118. Capacity Planning

Capacity planning should consider:

* Users
* organizations
* properties
* listings
* applications
* documents
* payment events
* audit events
* notifications
* background jobs
* search volume
* concurrent sessions

## 119. Initial Capacity Assumptions

The MVP should support:

* The founding pilot organization
* multiple properties and communities
* controlled onboarding of additional organizations
* hundreds to low thousands of active users
* growth without architectural replacement

Exact capacity targets remain pending.

## 120. Horizontal Scaling

Stateless application runtimes should scale horizontally according to:

* Request volume
* concurrent sessions
* webhook bursts
* report load
* background jobs
* public marketplace traffic

## 121. Background-Worker Scaling

Background workers may scale independently for:

* Notifications
* file processing
* AI jobs
* imports
* exports
* reconciliation
* scheduled workflows

## 122. Database Capacity

Database capacity planning should monitor:

* CPU
* memory
* connections
* storage
* I/O
* slow queries
* locks
* replication
* backup duration
* table growth
* index growth

## 123. Object-Storage Capacity

Storage planning should consider:

* Listing images
* government identification
* payment evidence
* agreements
* maintenance media
* exports
* retention
* versions
* backups

## 124. Performance Infrastructure

Performance should be supported through:

* Edge delivery
* optimized application rendering
* database indexing
* connection pooling
* pagination
* caching
* queue processing
* image optimization
* compressed responses
* controlled file sizes

## 125. Low-Bandwidth Infrastructure

Low-bandwidth support should include:

* Optimized media
* resumable or recoverable uploads where practical
* compressed responses
* limited payload sizes
* server-side pagination
* stable retry semantics
* text-first rendering
* cache-aware static assets

## 126. Infrastructure Security

Infrastructure security must include:

* MFA for privileged accounts
* least privilege
* secret management
* environment isolation
* TLS
* private storage
* network restrictions
* dependency scanning
* audit
* monitoring
* backup protection
* protected deployments

## 127. Production Access

Production access should be:

* Limited
* role based
* approved
* time bound where practical
* logged
* reviewed
* removed promptly when no longer needed

## 128. Direct Database Access

Direct production database access should be exceptional.

It should require:

* Approved reason
* privileged identity
* strong authentication
* time-limited access
* audit
* safe query controls
* post-access review where required

## 129. Break-Glass Access

Emergency infrastructure access should require:

* Defined incident
* strong authentication
* limited duration
* recorded reason
* heightened monitoring
* post-event review
* credential rotation where appropriate

## 130. Network Security

Network controls may include:

* Private database access
* restricted inbound rules
* restricted administrative access
* provider allowlisting where useful
* outbound controls later
* web application firewall
* denial-of-service protection

## 131. Web Application Firewall

A WAF may protect against:

* Common application attacks
* suspicious request patterns
* malicious automation
* exploit scanning
* abusive payloads

A WAF does not replace application validation.

## 132. Security Headers

Production responses should use appropriate headers such as:

* Content Security Policy
* Strict Transport Security
* frame restrictions
* content-type protection
* referrer policy
* permissions policy
* cache controls

## 133. Infrastructure Logging

Infrastructure logs may include:

* Deployment activity
* runtime failures
* database events
* queue failures
* storage events
* provider failures
* authentication events
* network events
* backup activity
* privileged access

## 134. Infrastructure Metrics

Metrics should include:

* Request volume
* response latency
* error rate
* runtime utilization
* database connections
* query latency
* queue depth
* job failures
* storage errors
* provider latency
* webhook failures
* deployment success
* backup status

## 135. Distributed Tracing

Tracing should connect:

```text
Client request
→ Edge
→ Application
→ Domain service
→ Database
→ Queue
→ Worker
→ External provider
```

Trace data must not expose restricted content.

## 136. Alerting

Alerts should cover:

* Application outage
* high error rate
* database saturation
* queue backlog
* failed jobs
* storage failure
* webhook signature failures
* provider outage
* backup failure
* deployment failure
* security event
* certificate expiry

## 137. Alert Ownership

Each alert must have:

* Severity
* owner
* escalation path
* runbook
* expected response
* recovery condition
* review date

## 138. Status Communication

A public or customer-facing status page may later communicate:

* Application availability
* login
* payments
* signatures
* communications
* scheduled maintenance

Internal security details must not be exposed.

## 139. Infrastructure Cost Management

Cost management should include:

* Environment budgets
* usage monitoring
* cost alerts
* storage lifecycle
* preview cleanup
* query optimization
* provider review
* right-sizing
* reserved capacity later where justified

## 140. Cost Allocation

Future cost allocation may support:

* Environment
* service
* provider
* organization
* feature
* storage category
* AI use

Customer billing must not be inferred directly from raw infrastructure cost without approved commercial rules.

## 141. Cost Anomaly Detection

The platform should detect unexpected increases in:

* Compute
* database
* storage
* network transfer
* AI usage
* email
* SMS
* WhatsApp
* file processing
* logging

## 142. Data Residency

Initial data residency remains pending provider and legal review.

The architecture should record:

* Primary hosting region
* database region
* storage region
* backup region
* provider data location
* AI processing location

## 143. Regional Latency

Infrastructure selection should consider latency for users in Nigeria and other expected locations.

## 144. Cross-Border Processing

Cross-border data processing should be documented for:

* Hosting
* backups
* payment providers
* signature providers
* communication providers
* AI providers
* monitoring

## 145. Infrastructure Documentation

Infrastructure documentation should include:

* Architecture diagrams
* environment inventory
* domain inventory
* credential ownership
* dependency inventory
* deployment process
* backup process
* restore process
* incident process
* provider configuration
* runbooks

## 146. Runbooks

Runbooks should exist for:

* Failed deployment
* failed migration
* application outage
* database outage
* storage outage
* queue backlog
* provider outage
* webhook failure
* backup failure
* secret compromise
* certificate issue
* data-repair request

## 147. Infrastructure Ownership

### Product Visionary

Owns:

* Product priorities
* release scope
* business availability expectations
* cost tolerance
* material provider approval

### Chief Product Architect

Owns:

* Infrastructure architecture
* environment model
* deployment integrity
* recovery architecture
* architectural review

### Engineering

Owns:

* Application deployment
* migrations
* infrastructure code
* operational fixes
* automated validation

### Security

Owns:

* Privileged access
* secret policy
* infrastructure security review
* incident security controls

### Operations

Owns:

* Monitoring
* alerts
* incident coordination
* backups
* recovery
* production readiness

## 148. Production Readiness Review

A production-readiness review should verify:

1. Architecture approved.
2. Environment isolated.
3. domains configured.
4. TLS valid.
5. secrets stored correctly.
6. database configured.
7. backups enabled.
8. restore tested.
9. storage private.
10. file scanning operational.
11. queues monitored.
12. provider webhooks verified.
13. monitoring operational.
14. alerts assigned.
15. deployment automated.
16. migration tested.
17. rollback or forward recovery documented.
18. security checks complete.
19. performance acceptable.
20. accessibility validation complete.
21. support process ready.
22. incident process ready.

## 149. Infrastructure Acceptance Criterion — Environment Isolation

```text
Given:
A preview deployment is created for a pull request.

When:
The preview application starts.

Then:
It uses non-production database, storage, secrets, queues, and providers.

And:
It cannot process production webhooks.

And:
It cannot send live customer communications by default.
```

## 150. Infrastructure Acceptance Criterion — Database Migration

```text
Given:
A migration introduces a new nullable unit-count field.

When:
The migration runs against records where Block B inventory is unknown.

Then:
The field remains null or pending.

And:
The migration does not convert the value to zero.

And:
No placeholder units are generated.
```

## 151. Infrastructure Acceptance Criterion — Failed Payment Provider

```text
Given:
The payment provider is unavailable.

When:
A user submits a provider-dependent payment operation.

Then:
The operation remains pending or fails clearly.

And:
No payment is verified automatically.

And:
No charge is settled.

And:
No agreement or occupancy activates from the failed provider request.
```

## 152. Infrastructure Acceptance Criterion — Duplicate Job

```text
Given:
A notification or provider-processing job has completed.

When:
The queue delivers the same job again.

Then:
The job is handled idempotently.

And:
No duplicate financial, agreement, occupancy, or communication event is created.
```

## 153. Infrastructure Acceptance Criterion — Application Rollback

```text
Given:
A new application deployment fails post-deployment smoke testing.

When:
The previous application version remains compatible with the database.

Then:
The platform returns to the previous approved version.

And:
The failed deployment remains traceable.

And:
No authoritative business history is deleted.
```

## 154. Infrastructure Acceptance Criterion — Restore

```text
Given:
A production-like backup exists.

When:
A restore exercise is performed.

Then:
The database and required object references are restored.

And:
Organization ownership remains correct.

And:
Financial, agreement, occupancy, and audit records remain consistent.

And:
The restored application passes validation.
```

## 155. Infrastructure Acceptance Criterion — Secret Isolation

```text
Given:
A developer opens a pull-request preview environment.

When:
The preview is inspected through the browser, logs, and application bundle.

Then:
No production database credential, provider secret, webhook secret, or signing key is exposed.
```

## 156. Infrastructure Acceptance Criterion — Support Access

```text
Given:
A support user has no approved support session.

When:
The user attempts to access production customer records.

Then:
Access is denied.

And:
Infrastructure-level privileges do not bypass the application support-session model except through approved emergency procedures.
```

## 157. Infrastructure Acceptance Criterion — Background Job Failure

```text
Given:
An AI summary job fails repeatedly.

When:
The retry limit is reached.

Then:
The job enters a visible failed state.

And:
The source application remains usable.

And:
No application decision or status changes.

And:
Operators can inspect and retry the job safely.
```

## 158. Infrastructure Acceptance Criterion — Cross-Tenant Cache

```text
Given:
A user views Organization A records.

When:
The user switches to Organization B.

Then:
Organization A cached results are not displayed.

And:
Cache keys and authorization checks prevent cross-tenant reuse.
```

## 159. MVP Infrastructure Scope

The MVP must include:

### Application

* Managed Next.js-compatible hosting
* production and non-production separation
* TLS
* custom domain
* deployment history
* health checks

### Database

* Managed PostgreSQL
* connection pooling
* encrypted connections
* backups
* restoration testing
* migration pipeline
* monitoring

### Storage

* Private object storage
* upload authorization
* signed access
* scan workflow
* version support
* retention foundation

### Jobs

* Durable background jobs
* retry
* idempotency
* failure visibility
* scheduled work
* monitoring

### Security

* Secret management
* environment isolation
* least privilege
* protected production access
* audit
* security headers
* rate limiting

### Delivery

* GitHub repository
* protected main branch
* pull-request checks
* preview deployments
* staging validation
* production approval
* smoke testing
* rollback or forward recovery

### Operations

* Logs
* metrics
* alerts
* provider health
* backup monitoring
* deployment monitoring
* incident runbooks

## 160. Deferred Infrastructure Capabilities

The following may be deferred:

* Multi-region active-active deployment
* dedicated Kubernetes infrastructure
* service mesh
* extensive microservices
* customer-managed encryption keys
* regional database shards
* dedicated tenant databases
* global traffic routing
* advanced chaos engineering
* full infrastructure self-service
* customer-specific environments
* private network connectivity for customers
* enterprise VPN access
* dedicated data warehouse
* large-scale event streaming
* dedicated search cluster
* advanced edge computing
* active-active provider failover
* full blue-green infrastructure
* continuous disaster-recovery simulation
* multi-cloud production deployment

Deferred capabilities must not weaken MVP isolation, security, backup, or recoverability.

## 161. Infrastructure Risks

### 161.1 Provider Lock-In

Risk:

Managed services create migration difficulty.

Controls:

* Adapter boundaries
* PostgreSQL standards
* infrastructure documentation
* portable domain logic
* export capability
* ADR review

### 161.2 Environment Leakage

Risk:

Non-production connects to production resources.

Controls:

* Separate credentials
* separate projects
* startup validation
* provider environment checks
* tests
* monitoring

### 161.3 Migration Failure

Risk:

A schema migration blocks or corrupts production.

Controls:

* Staging test
* expand-and-contract
* backups
* review
* monitoring
* forward-recovery plan

### 161.4 Secret Exposure

Risk:

Credentials appear in code, logs, or client bundles.

Controls:

* Secret management
* scanning
* server-side access
* rotation
* review
* incident process

### 161.5 Queue Duplication

Risk:

Retried jobs duplicate protected actions.

Controls:

* Idempotency
* unique constraints
* domain validation
* event records
* testing

### 161.6 Backup Failure

Risk:

Backups exist but cannot be restored.

Controls:

* Automated monitoring
* regular restore tests
* protected backup access
* documented recovery

### 161.7 Excessive Complexity

Risk:

Infrastructure becomes too difficult for the early team to operate.

Controls:

* Managed services
* modular monolith
* limited providers
* automation
* staged growth

### 161.8 Cost Escalation

Risk:

Uncontrolled storage, AI, logs, or provider use increases cost.

Controls:

* Budgets
* alerts
* lifecycle policies
* usage metrics
* provider review

### 161.9 Single-Provider Dependency

Risk:

A major provider outage affects core operations.

Controls:

* Graceful degradation
* backups
* export capability
* provider abstraction
* manual alternatives
* recovery planning

### 161.10 Pilot Hardcoding

Risk:

Infrastructure assumes only one property or organization.

Controls:

* Organization-scoped architecture
* configurable hierarchy
* multiple-tenant test data
* no pilot-specific credentials or paths

## 162. Unresolved Infrastructure Decisions

The following remain pending:

* Final application-hosting provider
* Final database provider
* Final authentication provider
* Final object-storage provider
* Final queue and job provider
* Final cache provider
* Final search approach
* Final email provider
* Final SMS provider
* Final WhatsApp provider
* Final payment provider
* Final signature provider
* Final file-scanning provider
* Final AI provider
* Final monitoring provider
* Final error-tracking provider
* Final infrastructure-as-code tool
* Final secret-management platform
* Final DNS provider
* Final WAF provider
* Final bot-protection provider
* Final domain topology
* Final environment topology
* Final region
* Final data-residency position
* Final application runtime model
* Final background-worker model
* Final connection-pooling strategy
* Final row-level-security deployment model
* Final backup frequency
* Final backup retention
* Final point-in-time recovery capability
* Final production RPO
* Final production RTO
* Final service-availability objective
* Final deployment versioning
* Final deployment strategy
* Final feature-flag provider
* Final kill-switch ownership
* Final migration tooling
* Final rollback policy
* Final infrastructure access model
* Final emergency-access process
* Final status-page provider
* Final log retention
* Final metric retention
* Final tracing approach
* Final capacity assumptions
* Final cost budget
* Final cost-allocation model
* Final production launch date
* Final pilot public names
* Final Block A unit labels
* Final Block B inventory
* Final second-building structure
* Final water-feature classification

These decisions must be resolved before the affected infrastructure capability becomes production dependent.

## 163. Infrastructure Governance

Changes to approved infrastructure architecture require:

1. Change description
2. Business rationale
3. Affected environment
4. Affected provider
5. Application impact
6. database impact
7. storage impact
8. queue impact
9. network impact
10. security impact
11. tenant-isolation impact
12. financial impact
13. agreement impact
14. occupancy impact
15. privacy impact
16. data-residency impact
17. availability impact
18. performance impact
19. cost impact
20. backup impact
21. recovery impact
22. deployment impact
23. migration impact
24. observability impact
25. support impact
26. testing impact
27. rollback or forward-recovery plan
28. Product Visionary approval
29. Chief Product Architect approval
30. Security review where required
31. Effective version

## 164. Infrastructure Review Triggers

A dedicated infrastructure review is required when introducing:

* New hosting provider
* new database provider
* new region
* new environment
* new storage system
* new queue system
* new cache system
* new search engine
* new external provider
* new AI provider
* new deployment model
* new migration model
* new tenant-isolation mechanism
* new backup model
* new disaster-recovery model
* new mobile backend requirement
* new data-residency obligation
* new high-volume workload
* new customer integration boundary

## 165. Current Infrastructure Decision

Habitacores will initially use a managed-cloud-first, modular-monolith, environment-isolated infrastructure architecture organized as:

```text
Source Control
→ Pull Request
→ Automated Quality and Security Checks
→ Preview Deployment
→ Main Branch
→ Staging Validation
→ Production Approval
→ Production Deployment
→ Migration
→ Health and Smoke Validation
→ Monitoring
→ Recovery Readiness
```

The runtime architecture will follow:

```text
User
→ DNS and Edge
→ Responsive Web Application and API
→ Authentication and Authorization
→ Domain Modules
→ PostgreSQL
→ Private Object Storage
→ Queue and Background Workers
→ External Provider Adapters
→ Audit, Logs, Metrics, Alerts, and Backups
```

The infrastructure must preserve:

* Organization isolation
* environment isolation
* least privilege
* secure secrets
* private storage
* transaction integrity
* payment-verification distinction
* idempotent background processing
* agreement immutability
* occupancy integrity
* provider isolation
* AI isolation
* deployment traceability
* safe migrations
* rollback or forward recovery
* tested backups
* disaster recovery
* graceful degradation
* operational visibility
* low-bandwidth delivery
* controlled scaling
* cost awareness
* future SaaS extensibility

No preview deployment, environment variable, hosting provider, database credential, queue message, object-storage URL, provider callback, cached result, feature flag, deployment success message, backup record, AI output, payment screenshot, pilot-team relationship, or infrastructure-level privilege may independently authorize, verify, approve, allocate, sign, activate, settle, refund, reverse, terminate, reveal, or otherwise finalize a protected Habitacores business action.