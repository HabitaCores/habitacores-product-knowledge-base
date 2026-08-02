# ADR-021 — Backup, Restore, Disaster Recovery, and Business Continuity Strategy

## Decision Record

| Field | Value |
|---|---|
| Product | Habitacores |
| ADR | ADR-021 |
| Version | 0.1 |
| Status | Proposed |
| Decision Type | Foundational Resilience, Recovery, Data-Protection, and Operations Architecture |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Production Database | PostgreSQL through Supabase |
| Database Recovery | Provider backup, PITR where approved, and independent logical exports |
| Object Recovery | Provider capability plus Habitacores-owned inventory and recovery controls |
| Infrastructure-State Recovery | Governed by ADR-019 |
| Source-Code Recovery | Independent repository archives |
| Recovery Objectives | Defined by workload and data class |
| Restore Testing | Mandatory |
| Production Restore | Authorized incident workflow only |
| Restore Destination | Isolated recovery environment before production cutover where practical |
| Recovery Authority | Habitacores incident and recovery process |
| Provider Backup Authority | Recovery source only |
| Cross-Tenant Integrity Validation | Mandatory |
| Financial Reconciliation | Mandatory after applicable recovery |
| Legal and Evidentiary Integrity | Mandatory |
| Manual Continuity Procedures | Required for critical workflows |
| AI Recovery Authority | Prohibited |
| Last Updated | 2 August 2026 |

## 1. Context

Habitacores will depend on:

- PostgreSQL business records
- authentication and identity-provider records
- private object storage
- agreements and execution evidence
- payment and reconciliation records
- audit events
- outbox and background-job records
- notification records
- application source code
- infrastructure definitions
- infrastructure state
- configuration and provider references
- encryption and signing keys
- mobile-build and release configuration
- external payment, signature, email, SMS, push, and AI providers

Failure or loss may result from:

- Accidental deletion
- incorrect migration
- defective deployment
- provider outage
- provider account compromise
- administrative error
- malicious activity
- ransomware or destructive credentials
- corrupted data
- storage-object loss
- infrastructure-state loss
- repository deletion
- region or platform failure
- lost encryption keys
- failed third-party integration
- human unavailability
- operational process failure

Habitacores must recover not merely infrastructure, but trustworthy business operation.

## 2. Decision Drivers

The strategy must:

1. Minimize permanent data loss.
2. restore critical services within defined objectives.
3. protect against provider and account-level failure.
4. preserve tenant isolation after recovery.
5. preserve financial integrity.
6. preserve agreement and evidentiary records.
7. preserve audit history.
8. protect private documents.
9. recover infrastructure and source code.
10. validate backups through restoration.
11. support controlled manual operation.
12. distinguish rollback, restore, failover, and reconstruction.
13. prevent recovery from bypassing security controls.
14. support incident investigation.
15. avoid exposing production data during testing.
16. define ownership and escalation.
17. document unrecoverable dependencies.
18. provide implementation-ready operational controls.

## 3. Decision

Habitacores will implement a layered backup and recovery architecture.

The approved recovery model is:

```text
Workload inventory
→ Data classification
→ Recovery-tier assignment
→ RPO and RTO definition
→ Provider-native protection
→ Habitacores-controlled backup where required
→ Encryption and access control
→ Backup integrity verification
→ Isolated restoration test
→ Application and data validation
→ Recovery evidence
→ Production readiness review
````

A backup is not considered operationally valid until Habitacores has demonstrated that it can restore and validate it.

## 4. Core Definitions

Habitacores will distinguish:

```text
BACKUP
SNAPSHOT
POINT_IN_TIME_RECOVERY
LOGICAL_EXPORT
REPLICA
FAILOVER
APPLICATION_ROLLBACK
DATA_RESTORE
SERVICE_RECONSTRUCTION
DISASTER_RECOVERY
BUSINESS_CONTINUITY
```

These terms must not be used interchangeably.

## 5. Recovery Point Objective

Recovery Point Objective, or RPO, defines the maximum acceptable period of data loss measured backward from an incident.

Directional initial targets:

| Recovery Tier                                                    |             Directional RPO |
| ---------------------------------------------------------------- | --------------------------: |
| Tier 0 — Financial, agreement, identity, audit authority         |        15 minutes or better |
| Tier 1 — Core tenancy and operational records                    |            1 hour or better |
| Tier 2 — Derived, reproducible, or non-critical operational data |          24 hours or better |
| Tier 3 — Ephemeral caches and rebuildable projections            | Rebuild rather than restore |

These are architectural targets, not claims of implemented capability.

Final targets require cost, provider, workload, legal, and operational validation.

## 6. Recovery Time Objective

Recovery Time Objective, or RTO, defines the target time for restoring an acceptable service level.

Directional initial targets:

| Recovery Tier |                       Directional RTO |
| ------------- | ------------------------------------: |
| Tier 0        |                               4 hours |
| Tier 1        |                               8 hours |
| Tier 2        |                              24 hours |
| Tier 3        | Rebuild according to service priority |

The recovery plan may restore a safe degraded mode before complete service restoration.

## 7. Recovery Tiers

### Tier 0 — Authoritative and Evidentiary

Includes:

* Users and identity links
* organization memberships
* roles and permissions
* payment records
* allocations
* receipts
* refunds and reversals
* agreement versions
* signatures and evidence packages
* audit events
* legal holds
* document identity and integrity records

### Tier 1 — Core Operations

Includes:

* Properties
* buildings
* blocks
* floors
* units
* applications
* occupancies
* maintenance records
* notification state
* outbox and job state
* support workflow state

### Tier 2 — Recoverable Operational Data

Includes:

* Search indexes
* reporting projections
* analytics aggregates
* non-authoritative AI artifacts
* cached provider metadata

### Tier 3 — Ephemeral Data

Includes:

* Short-lived caches
* temporary files
* regenerated previews
* disposable build artifacts
* retryable transient computation

## 8. PostgreSQL Protection

The production PostgreSQL strategy will include:

* Provider-managed physical backups
* Point-in-Time Recovery where the approved production plan supports it
* periodic encrypted logical exports
* schema and migration history in Git
* restore validation
* recovery monitoring
* independent retention appropriate to the recovery tier

PITR and logical exports serve different purposes and neither replaces the other.

## 9. Point-in-Time Recovery

PITR should be enabled for production before material tenant, financial, agreement, or evidentiary data enters the system.

The implementation must verify:

* Supported retention window
* restoration procedure
* restoration destination
* expected restoration duration
* provider limitations
* compute and pricing requirements
* encryption behavior
* operational permissions
* post-restore application compatibility

Enabling PITR without testing restore behavior is insufficient.

## 10. Logical Database Exports

Habitacores will maintain periodic logical exports independently of provider dashboard access.

Logical exports should:

* use supported PostgreSQL tooling;
* include required schemas;
* preserve roles and ownership only through an approved process;
* exclude unnecessary provider-managed internals;
* be encrypted before durable retention;
* be integrity checked;
* have documented restore commands;
* be retained outside the production project’s destruction boundary.

Logical exports must not be stored in the application repository.

## 11. Backup Frequency

Exact frequency is deferred, but the initial direction is:

* Continuous provider recovery capability for Tier 0 and Tier 1 production data
* Daily independent logical exports
* Additional export before high-risk migrations
* Additional recovery point before material destructive maintenance
* Monthly retained recovery checkpoint
* Annual review of retention and recovery effectiveness

Frequency must be reconciled with the approved RPO.

## 12. Object Storage

Database backups do not automatically establish recoverability of private storage objects.

Object recovery must account for:

* Original document bytes
* immutable document versions
* agreement artifacts
* execution evidence
* receipts
* uploaded payment evidence
* malware-scan state
* object keys
* hashes
* retention state
* legal holds
* deletion markers

The PostgreSQL document record and the storage object must remain reconcilable.

## 13. Object Inventory

Habitacores will maintain an authoritative inventory containing:

* Object ID
* storage provider
* bucket
* immutable key
* version
* classification
* size
* cryptographic hash
* creation time
* retention state
* legal-hold state
* related domain record
* recovery tier

Recovery validation must detect missing, unexpected, or hash-mismatched objects.

## 14. Object Recovery Strategy

The selected storage architecture must provide an approved combination of:

* Provider replication or durability
* versioning or recoverable deletion where available
* independent export for critical artifacts where justified
* object inventory
* cryptographic integrity validation
* tested restore or rehydration procedures

Provider durability claims do not replace Habitacores recovery tests.

## 15. Authentication Recovery

Recovery must consider both:

* Habitacores-owned user and identity-link records; and
* provider-managed authentication records.

The plan must determine how to recover or reconstruct:

* User-provider mappings
* verified contact relationships
* MFA enrollment consequences
* active-session invalidation
* identity-provider configuration
* redirect configuration
* signing secrets
* recovery codes where provider governed

A recovered application database with unrecoverable authentication relationships is not a complete recovery.

## 16. Source-Code Recovery

GitHub is the primary collaborative source repository, but it will not be the only recoverable copy.

Habitacores will maintain scheduled archives or mirrors of critical repositories including:

* Application source
* Product Knowledge Base
* Infrastructure definitions
* migrations
* operational scripts
* runbooks
* mobile configuration

Backups must include Git history and any required Git LFS objects.

Repository archives must be stored outside the same account-compromise boundary where practical.

## 17. Infrastructure-State Recovery

ADR-019 remains authoritative for:

* OpenTofu state
* state encryption
* state locking
* state versioning
* state backups
* encryption-key recovery
* infrastructure reconstruction

Infrastructure state is not a substitute for database, storage, repository, or provider-data backup.

## 18. Configuration Recovery

Habitacores must preserve a recoverable inventory of:

* Required environment variables
* secret identifiers
* provider-account references
* domain mappings
* callback URLs
* webhook endpoints
* mobile bundle and package identifiers
* signing configuration
* feature-management project references
* observability integrations
* notification-provider configuration

The inventory must not expose plaintext production secrets.

## 19. Secret Recovery

A secret backup must not mean uncontrolled copying of secret values.

The recovery design must define:

* Which secrets are recoverable
* which must be rotated
* recovery custodians
* break-glass access
* encryption
* access logging
* replacement procedure
* dependent-service update order

Following a material compromise, rotation is generally preferred over restoring potentially compromised credentials.

## 20. External Providers

Habitacores must maintain a provider dependency and recovery register covering:

* Payments
* electronic signatures
* email
* SMS
* push notifications
* AI
* feature management
* observability
* domain and DNS
* storage
* authentication

For each provider, the register must identify:

* Authoritative Habitacores records
* provider-only records
* export capability
* retention
* recovery process
* credential recovery
* webhook replay capability
* reconciliation procedure
* fallback or manual procedure
* known unrecoverable data

## 21. Payment Recovery

Payment recovery must preserve:

* Payment intents
* attempts
* provider events
* verified payments
* allocations
* receipts
* refunds
* reversals
* settlements
* reconciliation records

After applicable restoration, Habitacores must reconcile recovered state with current provider and approved bank evidence before normal financial processing resumes.

Recovered data must not silently duplicate provider events or payments.

## 22. Agreement Recovery

Agreement recovery must preserve:

* Template versions
* agreement versions
* exact rendered artifacts
* cryptographic hashes
* signer assignments
* signatures
* provider events
* executed documents
* evidence packages
* activation history
* amendments and termination records

A restored agreement must not be treated as valid if its document or evidence integrity cannot be confirmed.

## 23. Audit Recovery

Audit events are Tier 0 records.

Recovery must preserve:

* Ordering information
* actor attribution
* organization context
* protected-resource references
* operation relationships
* AI-operation correlations
* correction and reversal history

Audit gaps identified after recovery must be documented and investigated rather than concealed.

## 24. Transactional Outbox Recovery

Recovery may return the database to a point where:

* Some external actions already occurred but their internal completion state was lost.
* Some outbox messages reappear as pending.
* Some jobs were processing at the recovery point.
* Some provider callbacks occurred after the restored point.

Workers must therefore remain idempotent.

Before resuming workers, Habitacores must reconcile affected message and provider state.

## 25. Recovery Isolation

A backup should ordinarily be restored first into an isolated recovery environment.

The recovery environment must:

* Use restricted access
* prevent production emails, SMS, push, payments, signatures, and webhooks
* prevent AI-provider calls unless explicitly approved
* use isolated secrets
* avoid production DNS
* prevent background workers from performing external actions
* clearly identify restored data as restricted
* record recovery access

## 26. Restore Validation

A restore is not complete until validation confirms, as applicable:

* Database starts successfully
* migrations and schema are compatible
* critical tables are present
* expected row counts are plausible
* constraints are valid
* RLS is active
* runtime roles lack `BYPASSRLS`
* organization isolation works
* financial invariants hold
* agreement hashes match
* document objects exist and match hashes
* audit events remain readable
* outbox state is reconciled
* application health checks pass
* provider callbacks remain controlled

## 27. Recovery Reconciliation

Recovery reconciliation may compare:

```text
Recovered PostgreSQL
↔ Object storage
↔ Payment providers
↔ Signature providers
↔ Notification providers
↔ Infrastructure configuration
↔ Source and migration versions
↔ Audit records
```

Discrepancies must enter controlled review queues.

## 28. Recovery Cutover

Production cutover requires:

* Incident authority
* validated restore
* selected recovery point
* documented data-loss window
* reconciled external actions
* credential review
* DNS or routing plan
* worker-resumption plan
* monitoring readiness
* stakeholder communication
* rollback or forward-repair plan
* recorded approval

## 29. Data-Loss Disclosure

Where recovery results in confirmed or suspected loss, the incident record must identify:

* Affected period
* affected systems
* affected organizations
* affected record classes
* confidence level
* reconciliation status
* remediation
* communication and legal-review requirements

Habitacores must not represent uncertain recovered data as complete.

## 30. Business Continuity

Habitacores will define deterministic manual or degraded procedures for critical operations.

Potential continuity modes include:

```text
READ_ONLY
PAYMENT_CONFIRMATION_PENDING
MANUAL_PAYMENT_REVIEW
MANUAL_AGREEMENT_PROCESSING
DOCUMENT_UPLOAD_SUSPENDED
NOTIFICATION_DEGRADED
AI_DISABLED
PROVIDER_OUTAGE
MAINTENANCE_MODE
```

A degraded mode must not fabricate successful completion.

## 31. Read-Only Mode

Read-only mode may preserve access to safe existing records while preventing unsafe writes.

It must not:

* bypass authorization;
* expose cross-tenant data;
* accept financial changes that cannot be committed safely;
* represent queued changes as completed;
* alter immutable records.

## 32. Manual Records During Outage

Where authorized operations continue outside the platform, the continuity procedure must define:

* Allowed activity
* responsible actor
* temporary record format
* unique references
* evidence
* approvals
* secure storage
* later reconciliation
* duplicate prevention
* final audit entry

Ad hoc messaging threads must not become the permanent system of record.

## 33. Recovery Roles

Directional roles include:

```text
INCIDENT_COMMANDER
RECOVERY_LEAD
DATABASE_RECOVERY_OPERATOR
INFRASTRUCTURE_RECOVERY_OPERATOR
SECURITY_LEAD
FINANCIAL_RECONCILIATION_LEAD
LEGAL_AND_EVIDENCE_REVIEWER
COMMUNICATIONS_LEAD
BUSINESS_OWNER
```

At founding scale, one person may hold several roles, but responsibilities must remain explicit.

## 34. Recovery Authorization

Production restore, state replacement, destructive recovery, DNS cutover, or provider credential replacement requires explicit authorization.

AI, coding agents, ordinary application workers, and pull-request workflows cannot authorize production recovery.

## 35. Restore Testing

Habitacores will conduct:

* Automated backup-success checks
* routine logical-restore tests
* object-integrity tests
* repository-restore tests
* infrastructure-state recovery tests
* credential-recovery exercises
* provider-outage tabletop exercises
* periodic full disaster-recovery exercises

A test that only verifies that a backup file exists is insufficient.

## 36. Initial Testing Cadence

Directional cadence:

| Test                                           | Directional Cadence                                       |
| ---------------------------------------------- | --------------------------------------------------------- |
| Backup job and integrity monitoring            | Every execution                                           |
| Logical database restore                       | Monthly                                                   |
| Critical-object sampling and hash verification | Monthly                                                   |
| Repository recovery test                       | Quarterly                                                 |
| Infrastructure-state recovery test             | Quarterly                                                 |
| Provider-outage tabletop                       | Semiannually                                              |
| Full production-like recovery exercise         | At least annually and after material architectural change |

Final cadence requires operational approval.

## 37. Recovery Evidence

Each material restore test should preserve:

* Backup identifier
* recovery point
* recovery environment
* source
* encryption and integrity result
* operator
* start and completion time
* achieved RPO
* achieved RTO
* validation results
* discrepancies
* remediation
* final disposition

## 38. Observability

Recovery metrics may include:

```text
backup_jobs
backup_failures
backup_age
backup_integrity_failures
restore_tests
restore_test_failures
restore_duration
achieved_rpo
achieved_rto
missing_storage_objects
object_hash_mismatches
reconciliation_discrepancies
recovery_runbook_failures
```

Sensitive backup locations, object keys, users, and tenant identifiers must not become ordinary metric labels.

## 39. Alerts

Priority alerts include:

* Production backup failure
* PITR unavailable
* backup older than policy
* logical export failure
* backup-integrity mismatch
* failed restore test
* lost state-encryption-key access
* missing critical object
* source archive failure
* unauthorized backup access
* unexpected backup deletion
* RPO or RTO target breach
* restored environment sending production communications
* recovery environment reaching production providers

## 40. Security

Backups must receive protection equivalent to or stronger than the source data.

Controls include:

* Encryption in transit
* encryption at rest
* least privilege
* separate recovery credentials
* bounded retention
* access auditing
* immutability where feasible
* deletion control
* geographic and contractual review
* malware and integrity consideration
* no public access

## 41. Backup Deletion

Backup deletion must be governed by:

* Retention policy
* legal holds
* incident holds
* authorization
* provider limitations
* evidence of deletion
* dependency review

A compromised application runtime must not possess authority to delete all production backups.

## 42. Responsible AI

AI may assist with:

* Summarizing recovery logs
* organizing reconciliation discrepancies
* drafting incident reports
* comparing runbook outcomes
* identifying potential missing validation steps

AI may not:

* Select the production recovery point
* authorize a restore
* rotate credentials independently
* declare data complete
* resolve financial discrepancies autonomously
* declare agreement evidence valid
* execute DNS cutover
* delete backups
* close the incident

## 43. Testing Requirements

Required tests include:

* Database restore
* PITR recovery-point selection
* logical export restoration
* schema compatibility
* RLS restoration
* cross-tenant isolation
* object inventory reconciliation
* object hash validation
* payment-event deduplication after restore
* agreement-document integrity
* outbox replay safety
* repository restoration
* infrastructure-state recovery
* secret rotation and service reconnection
* degraded-mode behavior
* production-provider isolation
* recovery access auditing

## 44. Cross-Tenant Recovery Test

```text
Given:
A production backup is restored into an isolated recovery environment.

When:
A user scoped to Organization A queries protected records.

Then:
Application authorization and PostgreSQL RLS prevent access to
Organization B records.

And:
Recovery does not weaken tenant isolation.
```

## 45. Payment Replay Test

```text
Given:
A restored recovery point contains a pending payment-provider event.

And:
The provider transaction was already processed externally after that point.

When:
Financial recovery reconciliation executes.

Then:
Habitacores identifies the provider state.

And:
It does not recognize, allocate, or receipt the payment twice.
```

## 46. Agreement Integrity Test

```text
Given:
An executed agreement and its evidence package are restored.

When:
Integrity validation executes.

Then:
The stored document hashes match the authoritative agreement records.

And:
Any mismatch prevents the agreement from being declared fully recovered.
```

## 47. Storage Recovery Test

```text
Given:
PostgreSQL contains a critical document version.

When:
The recovery process validates storage.

Then:
The corresponding object exists.

And:
Its size and cryptographic hash match the authoritative metadata.
```

## 48. Production-Isolation Test

```text
Given:
Production data is restored for a disaster-recovery exercise.

When:
Workers and application services start.

Then:
Email, SMS, push, payment, signature, AI, and other external effects
remain disabled or isolated.

And:
No production user receives a test action.
```

## 49. Codex Boundaries

Every Codex backup or recovery work package must specify:

* Workload
* data owner
* recovery tier
* RPO
* RTO
* backup method
* encryption
* storage boundary
* retention
* access
* restore procedure
* validation
* provider isolation
* reconciliation
* observability
* tests

Codex must not:

* Assume provider backup equals complete recovery
* store backups in Git
* include plaintext secrets in backup scripts
* restore production data into an unrestricted preview
* enable external side effects during restore tests
* weaken RLS for convenience
* treat infrastructure state as a database backup
* treat database backup as object-storage backup
* overwrite production without authorization
* automatically replay external actions
* delete backups without retention review
* declare recovery successful without validation
* let AI select or approve a production restore

## 50. Prohibited Patterns

The following are not approved:

* Backup without restore testing
* production database backup stored only inside the production project
* local unencrypted production dumps
* unrestricted developer access to backups
* one backup covering every workload without inventory
* shared production and test recovery credentials
* production data restored into ordinary preview environments
* external communications enabled during recovery tests
* RLS disabled after restoration
* provider events replayed without reconciliation
* agreements recovered without document-integrity checks
* automatic production overwrite from a scheduled job
* undocumented manual records during an outage
* AI-declared recovery success
* backup retention defeated by ordinary application credentials

## 51. Options Considered

### Option A — Provider Backups Only

Advantages:

* Low operational burden
* provider integration
* convenient restoration

Disadvantages:

* Shared provider-account failure boundary
* project-deletion exposure
* incomplete protection for source, configuration, and external providers
* limited independent evidence
* portability risk

Decision:

Rejected as the complete strategy.

### Option B — Daily Logical Database Dump Only

Advantages:

* Portable
* independently controlled
* straightforward inspection

Disadvantages:

* RPO may be too large
* object storage is not covered
* slower restoration
* incomplete provider configuration
* insufficient for active high-value production data

Decision:

Rejected as the complete strategy.

### Option C — Replication Only

Advantages:

* Fast failover
* reduced outage duration

Disadvantages:

* Replicates accidental deletion and corruption
* does not preserve historical recovery points by itself
* is not an independent backup
* increases complexity and cost

Decision:

Rejected as a backup replacement.

### Option D — Layered Backup, Recovery, and Continuity Architecture

Advantages:

* Provider and independent recovery sources
* PITR and portable logical exports
* workload-specific recovery targets
* object and document integrity validation
* financial and provider reconciliation
* tested restoration
* manual continuity
* protection against multiple failure modes

Disadvantages:

* Additional cost
* storage and encryption responsibilities
* recurring restore testing
* operational ownership required
* recovery procedures require maintenance

Decision:

Approved.

## 52. Positive Consequences

* Recovery expectations become measurable.
* provider backup limitations are acknowledged.
* financial and agreement integrity are explicitly protected.
* source code and infrastructure remain independently recoverable.
* object storage receives first-class recovery controls.
* restore tests demonstrate actual recoverability.
* business operations can degrade safely.
* external actions are reconciled after recovery.
* recovery does not bypass tenant isolation or authorization.

## 53. Negative Consequences

* PITR and independent backups add cost.
* restore exercises require time and expertise.
* backup encryption creates key-management duties.
* provider limitations may restrict achievable RPO and RTO.
* cross-system reconciliation is operationally complex.
* production-like recovery environments require strict data controls.
* continuity procedures require training and maintenance.

## 54. Risks

### 54.1 Backups Exist but Cannot Be Restored

Controls:

* Routine restore testing
* integrity checks
* documented tooling
* pinned compatible versions
* recovery evidence

### 54.2 Provider Account Compromise

Controls:

* Independent copies
* separate credentials
* MFA
* least privilege
* backup deletion protection
* external repository archives

### 54.3 Cross-Tenant Exposure During Recovery

Controls:

* Isolated environment
* restored RLS
* restricted access
* cross-tenant tests
* no ordinary preview exposure

### 54.4 Duplicate External Actions

Controls:

* Idempotency
* external-provider reconciliation
* worker pause
* outbox review
* controlled resumption

### 54.5 Lost Encryption Keys

Controls:

* Key inventory
* separate secure recovery
* rotation procedure
* recovery exercises
* multiple authorized custodians where appropriate

### 54.6 Incomplete Recovery Declaration

Controls:

* Formal validation checklist
* financial reconciliation
* document hashes
* audit review
* incident authority approval

## 55. Implementation Sequence

```text
1. Inventory every production workload and provider dependency
2. classify data and assign recovery tiers
3. approve initial RPO and RTO targets
4. select production backup and PITR capabilities
5. implement encrypted independent PostgreSQL logical exports
6. establish backup storage and deletion protections
7. implement object inventory and hash reconciliation
8. establish source-repository archives
9. integrate ADR-019 state and key recovery
10. document configuration and provider recovery inventory
11. implement isolated recovery environments
12. disable external side effects in recovery environments
13. create database and object restore validation
14. create payment, agreement, audit, and outbox reconciliation procedures
15. define manual business-continuity workflows
16. implement backup and recovery monitoring
17. perform the first logical database restoration
18. perform the first object-integrity recovery test
19. perform the first repository and infrastructure recovery tests
20. perform a full production-like disaster-recovery exercise
21. record achieved RPO and RTO
22. remediate gaps before production scale
```

## 56. Deferred Decisions

This ADR does not decide:

* Exact Supabase production plan
* exact PITR retention period
* exact backup-storage provider
* exact independent backup region
* exact immutable-storage mechanism
* exact logical-export schedule
* exact retention periods
* exact encryption-key service
* exact recovery-environment hosting
* exact failover architecture
* exact read-replica architecture
* exact multi-region strategy
* exact cold, warm, or hot standby topology
* exact customer notification thresholds
* exact business-continuity staffing
* exact legal breach-notification process
* exact cyber-insurance requirements

These require bounded implementation, cost, legal, and operational decisions.

## 57. Supersession Conditions

This ADR should be reviewed when:

* Habitacores enters production.
* financial or agreement volume materially increases.
* a second database or storage provider is introduced.
* regional recovery becomes mandatory.
* enterprise customers require contractual recovery objectives.
* a multi-region deployment is introduced.
* regulatory retention requirements change.
* a major recovery exercise fails.
* a provider’s backup or deletion model changes.
* Habitacores introduces custodial funds, escrow, or regulated financial activity.

## 58. Final Decision Statement

Habitacores will implement a layered backup, restoration, disaster-recovery, and business-continuity architecture covering PostgreSQL, object storage, authentication relationships, source repositories, infrastructure state, configuration, keys, and external provider dependencies.

Recovery objectives will be defined by workload and data classification. Tier 0 financial, agreement, identity, audit, and evidentiary records will receive the strongest recovery targets and validation.

Production PostgreSQL will use provider-managed backups and Point-in-Time Recovery where approved, supplemented by encrypted logical exports retained outside the production project’s immediate destruction boundary.

Database backup will not be treated as object-storage backup. Habitacores will preserve authoritative object inventories, immutable keys, classifications, cryptographic hashes, relationships, retention state, and legal holds, and will validate restored objects against PostgreSQL metadata.

Provider-managed backups will remain recovery sources, not the sole Habitacores recovery authority. Project deletion, provider-account compromise, storage loss, repository deletion, lost infrastructure state, and credential loss will be addressed through independent controls.

Recovery exercises will ordinarily restore production data into isolated, restricted environments where production email, SMS, push, payment, signature, AI, webhook, and worker side effects are disabled.

A restore will not be declared successful merely because the database starts. Habitacores will validate schema compatibility, constraints, RLS, tenant isolation, financial invariants, agreement hashes, document objects, audit records, application health, outbox state, provider relationships, and recovery access controls.

After recovery, external payment, signature, notification, and other provider states will be reconciled before dependent processing resumes. Duplicate webhooks, jobs, payments, allocations, receipts, messages, or signatures must not be created.

Habitacores will maintain degraded and manual business-continuity procedures for critical workflows. Manual activity must use controlled references, evidence, authorization, secure storage, later reconciliation, and final audit records.

Backups will be encrypted, access controlled, monitored, integrity checked, retained according to policy, protected from ordinary application deletion authority, and subject to legal holds where required.

Recovery Point Objectives and Recovery Time Objectives are targets that must be demonstrated through testing. Habitacores will record achieved RPO, achieved RTO, validation results, discrepancies, and remediation from recovery exercises.

AI may assist with summarization and investigation but will not select a production recovery point, authorize restoration, declare data complete, resolve financial or evidentiary discrepancies, rotate credentials independently, perform production cutover, or close a recovery incident.

No provider, application runtime, preview environment, worker, developer credential, coding agent, or AI model may silently restore production, delete all recovery copies, bypass tenant isolation, replay external actions without reconciliation, weaken RLS, expose recovered data, or declare successful recovery outside the authorized Habitacores process.