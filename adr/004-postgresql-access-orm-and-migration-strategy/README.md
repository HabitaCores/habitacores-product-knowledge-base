# ADR-004 — PostgreSQL Access, ORM, and Migration Strategy

## Decision Record

| Field | Value |
|---|---|
| Product | Habitacores |
| Decision | PostgreSQL Access, ORM, and Migration Strategy |
| ADR | ADR-004 |
| Version | 0.1 |
| Status | Proposed |
| Decision Type | Foundational Data Architecture |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Primary Database | PostgreSQL |
| Type-Safe Database Toolkit | Drizzle ORM |
| Migration Toolkit | Drizzle Kit |
| Initial PostgreSQL Driver | node-postgres (`pg`) |
| Migration Artifact | Reviewed SQL files |
| Runtime Connection | Pooled application connection |
| Migration Connection | Direct administrative connection |
| Production Migration Model | Forward-only, immutable, reviewed |
| Schema Ownership | Habitacores application repository |
| Tenant Isolation | Application authorization plus separately approved database controls |
| Last Updated | 1 August 2026 |

## 1. Context

Habitacores is a multi-tenant real-estate operating ecosystem whose authoritative records will include:

- Organizations
- organization memberships
- properties
- communities
- buildings
- blocks
- floors
- units
- listings
- inquiries
- inspections
- rental applications
- application decisions
- charges
- payments
- payment evidence
- payment allocations
- receipts
- agreements
- signatures
- occupancies
- maintenance requests
- work orders
- documents
- communications
- support sessions
- audit events
- outbox events
- integration events
- Responsible AI operation metadata

ADR-001 established a TypeScript monorepo and modular-monolith architecture.

ADR-002 established application-service and domain boundaries around the Next.js application.

ADR-003 selected Node.js 24 LTS, pnpm 11, pnpm workspaces, and Turborepo.

ADR-004 must determine:

- How TypeScript code interacts with PostgreSQL
- which database access toolkit is used
- how database schemas are represented
- how migrations are generated and reviewed
- where transaction boundaries reside
- how connection pooling is handled
- how native PostgreSQL features remain available
- how tenant-safe database access is enforced
- how destructive schema changes are controlled
- how background workers share database infrastructure
- how AI functionality is prevented from bypassing data boundaries

## 2. Decision Drivers

The decision is driven by the need to:

1. Preserve PostgreSQL as the authoritative system of record.
2. maintain strong TypeScript support.
3. produce readable and reviewable SQL migrations.
4. retain access to native PostgreSQL capabilities.
5. support explicit transactions.
6. support organization-scoped queries.
7. support future row-level security.
8. support financial and agreement integrity.
9. support append-only audit and outbox records.
10. avoid hidden ORM behavior.
11. prevent uncontrolled direct database access.
12. support workers and application services consistently.
13. support production-safe schema evolution.
14. support local, test, staging, and production environments.
15. support connection pooling.
16. permit optimized SQL for complex reports and workflows.
17. preserve database portability only where it does not weaken PostgreSQL usage.
18. provide clear implementation boundaries for Codex.
19. prevent AI processes from gaining unrestricted database access.
20. support future analytics and read-model evolution.

## 3. Decision

Habitacores will use:

```text
Database:
PostgreSQL

Type-safe schema and routine query toolkit:
Drizzle ORM

Migration generation and execution toolkit:
Drizzle Kit

Initial Node.js PostgreSQL driver:
node-postgres (`pg`)

Migration artifact:
Committed SQL migration files

Runtime access:
Pooled application connection

Migration and administrative access:
Direct connection

Production migration policy:
Forward-only, reviewed, immutable after application

Database access boundary:
Repositories and approved query services

Transaction boundary:
Application service or dedicated unit-of-work abstraction

Advanced PostgreSQL functionality:
Approved parameterized SQL where appropriate
````

The database architecture will remain PostgreSQL-first.

Drizzle is a development and access tool. It will not replace:

* Domain rules
* application services
* authorization
* audit requirements
* database constraints
* transaction design
* migration review
* tenant isolation
* operational database governance

## 4. PostgreSQL as the System of Record

PostgreSQL will remain authoritative for transactional Habitacores state.

The system must not treat the following as an alternative authoritative database:

* Browser state
* React state
* framework caches
* search indexes
* vector stores
* analytics warehouses
* payment-provider dashboards
* signature-provider dashboards
* email-provider records
* AI-provider history
* worker memory
* process-local state

External systems may provide evidence or status, but approved application services must reconcile that information into authoritative PostgreSQL records.

## 5. Drizzle ORM

Habitacores will use Drizzle ORM for:

* Type-safe schema declarations
* routine inserts
* routine updates
* routine deletes where permitted
* routine selections
* joins
* parameter binding
* transaction execution
* repository implementations
* reusable query fragments
* database-generated TypeScript types

Drizzle must remain behind approved database-access boundaries.

## 6. Why Drizzle

Drizzle is selected because it provides:

* Strong TypeScript integration
* a SQL-oriented mental model
* explicit schema definitions
* reviewable query behavior
* explicit transaction APIs
* native PostgreSQL support
* generated SQL migrations
* the ability to use native SQL where needed
* relatively low abstraction over PostgreSQL
* compatibility with a modular repository architecture

The choice supports a team that needs type safety without surrendering control of SQL and PostgreSQL behavior.

## 7. Initial PostgreSQL Driver

The initial driver will be:

```text
node-postgres (`pg`)
```

The driver will be isolated inside:

```text
packages/database
```

Application and domain packages must not depend directly on `pg`.

Replacing or supplementing the driver later should require changes primarily inside the database infrastructure package.

## 8. Driver Reconsideration

The driver decision should be reviewed if:

* The selected hosting platform requires another connection mechanism.
* serverless connection behavior makes persistent pools unsuitable.
* a managed connection proxy requires different configuration.
* performance testing demonstrates a material limitation.
* deployment moves to an execution environment incompatible with `pg`.
* another driver provides a verified operational advantage.

A driver change must not alter domain rules or repository contracts unnecessarily.

## 9. Database Package Location

Database infrastructure will reside in:

```text
packages/database
```

Directional structure:

```text
packages/database/
├── src/
│   ├── client/
│   ├── schema/
│   ├── repositories/
│   ├── queries/
│   ├── transactions/
│   ├── tenant-context/
│   ├── outbox/
│   ├── audit/
│   └── index.ts
├── migrations/
├── seeds/
├── tests/
├── drizzle.config.ts
└── package.json
```

Directories should be introduced only when required.

## 10. Schema Organization

The Drizzle schema should be divided by stable business ownership rather than maintained as one unbounded file.

Directional structure:

```text
schema/
├── identity.ts
├── organizations.ts
├── properties.ts
├── listings.ts
├── leasing.ts
├── finance.ts
├── agreements.ts
├── occupancy.ts
├── maintenance.ts
├── documents.ts
├── communications.ts
├── support.ts
├── audit.ts
├── outbox.ts
├── integrations.ts
├── responsible-ai.ts
└── index.ts
```

Schema files do not create independently deployed services.

They organize one PostgreSQL system of record according to the modular-monolith boundaries.

## 11. Naming Conventions

The database should use consistent PostgreSQL naming.

Recommended conventions:

```text
Tables:
snake_case, plural

Columns:
snake_case

Primary keys:
id

Foreign keys:
<resource>_id

Timestamps:
created_at
updated_at
deleted_at where approved
effective_at where appropriate
occurred_at for events
```

Examples:

```text
organizations
organization_memberships
properties
rental_applications
payment_allocations
agreement_versions
audit_events
outbox_events
```

Names must express business meaning rather than framework or UI terminology.

## 12. Identifiers

The exact identifier format will be selected in a separate ADR.

ADR-004 requires only that:

* Identifiers be database-enforced.
* identifiers remain stable when display names change.
* foreign keys use compatible types.
* internal identifiers not encode mutable business meaning.
* identifier generation not depend on browser trust.
* external references be distinguishable from internal primary keys where necessary.

## 13. Money

The exact money representation will be selected in a separate ADR.

Until that decision is approved:

* Floating-point database types are prohibited for authoritative monetary values.
* application code must not assume JavaScript floating-point arithmetic is authoritative.
* currency must remain explicit.
* financial constraints must be enforceable transactionally.

## 14. Unknown Values

Unknown business facts must be stored as unknown.

The database must not convert:

* Unknown counts to zero
* unknown dates to current time
* unknown names to placeholders
* unknown payment status to paid
* unknown unit inventory to empty inventory
* unknown decisions to rejected
* unknown booleans to false without business justification

For the pilot property, unconfirmed Block B inventory must remain null, pending, or absent according to the approved domain model.

## 15. Database Constraints

Critical invariants should be supported through PostgreSQL constraints where appropriate.

Controls may include:

* Primary keys
* foreign keys
* unique constraints
* check constraints
* not-null constraints
* exclusion constraints
* indexes supporting uniqueness or concurrency
* restricted delete behavior

Application validation cannot replace database integrity.

## 16. Constraints and Domain Rules

The database should enforce invariants that are unambiguous at the persistence layer.

Examples may include:

* Unique membership relationships
* nonnegative permitted amounts
* valid currency-code shape
* allocation amount greater than zero
* unique provider-event identity
* unique idempotency key within its scope
* one active version where structurally required
* non-overlapping occupancy where PostgreSQL can enforce it safely

Complex workflow authorization remains an application and domain responsibility.

## 17. Foreign Keys

Foreign keys should be used for authoritative relational records unless a documented exception exists.

Foreign-key behavior must be selected deliberately.

`CASCADE` must not be applied as a convenience to records involving:

* Finance
* agreements
* occupancy
* audit
* legal evidence
* provider reconciliation
* identity evidence

Deletion behavior must follow retention and legal requirements.

## 18. Soft Deletion

Soft deletion is not a universal default.

A record should use archival, deactivation, revocation, supersession, or soft deletion only when the domain requires it.

The architecture must avoid adding `deleted_at` to every table automatically.

Records such as financial events and audit events may require append-only or reversal-based models rather than deletion.

## 19. Database Access Boundary

Only approved server-side infrastructure may access PostgreSQL.

Permitted callers include:

* Application services through repositories
* permission-aware query services
* deterministic workers
* approved migration tooling
* controlled operational tooling
* AI workers through narrowly scoped application services or approved data-access interfaces

Prohibited direct callers include:

* React Client Components
* browser code
* arbitrary Server Components
* arbitrary Server Actions
* arbitrary Route Handlers
* AI-provider SDKs
* unapproved scripts
* public clients

## 20. Repository Pattern

Repositories will implement domain-oriented persistence contracts.

Illustrative contract:

```typescript
interface ApplicationRepository {
  findById(
    context: RepositoryContext,
    applicationId: ApplicationId
  ): Promise<RentalApplication | null>;

  save(
    transaction: DatabaseTransaction,
    application: RentalApplication
  ): Promise<void>;
}
```

Repository contracts should express business resources rather than generic table operations.

## 21. Generic Repository Restriction

A universal generic repository such as:

```text
create
read
update
delete
```

for every table is not approved as the primary persistence architecture.

Generic repositories often conceal:

* Aggregate boundaries
* organization checks
* permitted transitions
* concurrency behavior
* audit requirements
* specialized query behavior

Shared low-level utilities may exist, but domain-facing repositories must remain intentional.

## 22. Query Services

Read-heavy views may use dedicated query services rather than reconstructing full domain aggregates.

Examples:

* Public listing search
* application review queue
* payment verification queue
* resident balance summary
* agreement status
* occupancy dashboard
* maintenance operations board
* audit investigation view

Query services must still enforce:

* Organization scope
* actor permissions
* assignment
* data classification
* record state
* support-session constraints

## 23. Raw SQL

Parameterized raw SQL is approved when it is clearer or safer than an ORM expression.

Suitable uses may include:

* Complex reports
* common table expressions
* window functions
* advisory locks
* exclusion constraints
* partial indexes
* advanced PostgreSQL operators
* row-level security policies
* optimized batch operations
* database functions
* migration operations
* diagnostic queries

Raw SQL must remain:

* Parameterized
* reviewed
* tested
* organization scoped where required
* observable
* contained inside approved database infrastructure

## 24. SQL Injection Prevention

User-controlled values must never be interpolated directly into SQL strings.

Dynamic SQL identifiers require explicit allowlists or safe library mechanisms.

The following must not be accepted from untrusted input without strict mapping:

* Table names
* column names
* sort expressions
* operators
* SQL fragments
* schema names
* function names

## 25. Transaction Boundary

Material state transitions must run inside an explicit database transaction.

The transaction boundary should normally be controlled by:

* An application service
* a dedicated unit-of-work abstraction
* an approved worker handler invoking an application service

The transaction boundary must not be hidden inside unrelated repository methods when the workflow spans multiple records.

## 26. Transaction Example

A payment allocation may require one transaction to:

1. Load the payment.
2. load the target charge.
3. validate organization ownership.
4. validate available payment amount.
5. validate outstanding charge amount.
6. create the allocation.
7. update derived financial state where approved.
8. append an audit event.
9. append an outbox event.
10. commit atomically.

Failure before commit must leave no partial authoritative allocation.

## 27. Transaction Restrictions

Database transactions must not remain open while waiting unnecessarily for:

* User input
* email providers
* signature providers
* payment-provider network requests
* file uploads
* malware scanning
* external AI inference
* long-running report generation

External operations should generally use:

* Pre-transaction validation
* short authoritative transaction
* outbox
* asynchronous provider execution
* reconciliation

## 28. Isolation and Concurrency

The default transaction isolation level and exceptions will be finalized during implementation and concurrency testing.

Material workflows must explicitly consider:

* Lost updates
* duplicate submissions
* double allocation
* conflicting occupancy
* stale agreement versions
* duplicate provider events
* repeated webhook delivery
* concurrent role changes
* simultaneous application decisions

Controls may include:

* Unique constraints
* optimistic version columns
* conditional updates
* row locks
* advisory locks
* stronger isolation
* idempotency keys

## 29. No Network Calls Inside Critical Transactions

A critical transaction should not call an external provider while holding database locks unless a separately reviewed use case proves that it is necessary and safe.

Preferred flow:

```text
Validate
→ Commit authoritative intent and outbox event
→ Worker calls provider
→ Reconcile provider result
→ Commit resulting state
```

## 30. Connection Strategy

Habitacores will distinguish:

### Runtime connection

Used by:

* `apps/web`
* `apps/worker`
* `apps/ai-worker` through approved boundaries

This connection may use a controlled pool or managed pooling endpoint.

### Direct administrative connection

Used by:

* Migrations
* controlled maintenance
* approved schema inspection
* emergency administration
* selected operational tooling

The migration connection must not be exposed to ordinary request processing.

## 31. Connection Pooling

Each deployable process must use a controlled pool appropriate to:

* Deployment concurrency
* provider limits
* database capacity
* request duration
* worker concurrency
* transaction duration

Pool size must not be copied blindly across environments.

The total possible connection count across all replicas must remain below the safe database limit with operational headroom.

## 32. Connection Ownership

The database package will own:

* Pool construction
* connection configuration
* retry classification
* transaction creation
* query instrumentation
* safe shutdown
* test connection behavior

Individual feature modules must not create independent pools.

## 33. Connection Secrets

Database credentials must:

* Remain in approved secret storage
* be environment specific
* not be committed
* not be exposed to browser bundles
* not appear in logs
* use least-privilege roles
* be rotated under approved procedures

Production credentials must not be used for ordinary local development or automated tests.

## 34. Database Roles

The final role structure will be formalized with the row-level-security decision.

Directional roles may include:

* Migration owner
* application runtime
* worker runtime
* read-only operations
* monitoring
* backup or restore
* emergency administration

Ordinary application roles should not own protected tables or possess unrestricted bypass privileges without a documented reason.

## 35. Tenant Context

Organization context must be resolved before organization-owned database operations execute.

A repository context should include sufficient trusted scope such as:

```typescript
interface RepositoryContext {
  actorId: string;
  organizationId?: string;
  membershipId?: string;
  supportSessionId?: string;
  correlationId: string;
}
```

The exact type will be established during implementation.

## 36. Tenant Filter Requirement

Until and unless database row-level security independently enforces isolation, every organization-owned query must include the approved organization scope.

The architecture must not rely on developers remembering ad hoc filters.

Controls should include:

* Repository APIs requiring organization context
* query helpers
* architecture tests
* integration tests
* code review
* future RLS
* cross-tenant adversarial tests

## 37. Row-Level Security

PostgreSQL row-level security is directionally supported but is not fully decided by ADR-004.

A later ADR must determine:

* Which tables require RLS
* role ownership
* `BYPASSRLS` restrictions
* session or transaction tenant context
* connection-pool compatibility
* background-worker access
* support-session behavior
* platform administration
* migration behavior
* test strategy
* failure behavior

Application authorization remains required even when RLS is adopted.

## 38. AI Database Access

AI infrastructure must not receive an unrestricted general-purpose database client.

Approved AI access must pass through:

```text
Approved AI Use Case
→ Authentication
→ Organization Resolution
→ Authorization
→ Permission-Aware Retrieval Service
→ Field Allowlist
→ Redaction and Minimization
→ AI Gateway
```

The AI worker may persist AI-operation metadata and approved outputs through dedicated repositories.

It must not query all tenants or arbitrary tables merely because it runs on the server.

## 39. Migration Source of Truth

Committed SQL migration files will be the authoritative history of schema evolution.

Drizzle TypeScript schema declarations represent the desired application schema.

The migration history records how environments reached that schema.

Neither should be changed independently without review.

## 40. Migration Generation

Normal development flow:

```text
Change Drizzle schema
→ Generate SQL migration
→ Inspect SQL
→ Modify deliberately where necessary
→ Add migration tests
→ Review pull request
→ Apply to disposable development database
→ Apply to staging
→ Apply through production deployment process
```

Generated SQL must not be accepted without human inspection.

## 41. Migration Directory

Migrations will be committed under:

```text
packages/database/migrations
```

A migration should have a stable ordered name.

Illustrative:

```text
0001_create_organizations.sql
0002_create_property_hierarchy.sql
0003_add_organization_memberships.sql
```

The exact naming format will follow the selected Drizzle Kit configuration.

## 42. Migration Immutability

After a migration has been applied to any shared environment, it must not be edited to represent a different history.

Corrections require a new migration.

Shared environments include:

* Integration
* staging
* production
* any persistent collaborative development database

Editing already-applied migrations can create undetectable environment drift and is prohibited.

## 43. Forward-Only Production Migration

Production migrations will be forward only.

Rollback will normally mean:

* Deploying corrective application code
* applying a new forward migration
* restoring from backup only when incident severity justifies it

Automatic down migrations are not assumed to be safe for production data.

## 44. Development Reset

Disposable local and automated-test databases may be dropped and rebuilt.

Shared or production-like environments must not use destructive reset commands.

Reset commands must:

* Refuse production targets
* require explicit environment classification
* avoid accepting ambiguous database URLs
* be excluded from normal deployment credentials

## 45. Schema Push Restriction

Direct schema push commands may be used only against explicitly disposable local databases.

They are prohibited for:

* Shared development
* integration
* staging
* production
* persistent preview environments containing meaningful data

Shared environments must use committed migrations.

## 46. Migration Review

Every migration pull request should review:

* Tables and columns affected
* nullability
* defaults
* constraints
* foreign keys
* indexes
* lock behavior
* table rewrite risk
* data backfill
* compatibility with running application versions
* RLS implications
* rollback or forward-recovery strategy
* estimated duration
* operational monitoring
* sensitive-data implications

## 47. Expand-and-Contract

Potentially disruptive production changes should use an expand-and-contract sequence.

Example:

```text
1. Add new nullable column.
2. deploy code capable of reading both representations.
3. backfill in controlled batches.
4. write both representations where required.
5. validate data.
6. switch reads to the new representation.
7. stop writing the old representation.
8. remove the old column in a later deployment.
```

Schema changes must remain compatible with the application versions that can run during deployment.

## 48. Destructive Migration Policy

The following require elevated review:

* Dropping tables
* dropping columns
* narrowing data types
* adding non-null constraints to populated tables
* changing primary keys
* changing identifier types
* removing indexes used by critical workflows
* rewriting large tables
* changing financial precision
* changing audit-event structure
* changing tenant ownership
* changing RLS policies

The migration must include a data-preservation and recovery plan.

## 49. Data Migrations

Large data migrations should not necessarily run as one schema-migration transaction.

They may require:

* Bounded batches
* checkpoints
* resumability
* idempotency
* throttling
* progress visibility
* verification queries
* controlled worker execution
* operational cancellation

Schema and data migration phases should be separated when that reduces risk.

## 50. Migration Execution

Production migrations must execute from a controlled deployment or release job.

They must not run automatically:

* When a web process starts
* when a worker process starts
* during every request
* from a developer laptop without authorization

Only one migration executor should control a target environment at a time.

## 51. Migration Connection

Migrations should use a direct database connection suitable for:

* DDL
* advisory locking where required
* controlled timeouts
* schema ownership
* migration history recording

A transaction-pooling endpoint that cannot support the required migration semantics must not be used for administrative migration execution.

## 52. Migration Timeouts

Production migration execution should configure deliberate:

* Lock timeout
* statement timeout
* connection timeout

The deployment should fail safely rather than waiting indefinitely while blocking critical application activity.

Exact values depend on the migration and environment.

## 53. Migration Verification

Before production:

* Migration history must be consistent.
* migrations must apply to a clean database.
* migrations must apply to a representative existing schema.
* application build must pass.
* integration tests must pass.
* sensitive queries must remain tenant safe.
* critical indexes and constraints must exist.
* staging validation must complete.

## 54. Drift Detection

The project should detect differences between:

* Committed schema declarations
* committed migration history
* actual environment schema

Production drift must not be repaired casually by generating a migration from the unexpected state.

The cause must first be investigated and documented.

## 55. Index Strategy

Indexes must be created from measured or strongly justified access patterns.

Priority candidates include:

* Organization foreign keys
* resource status
* organization plus status
* organization plus created time
* unique external provider identifiers
* outbox processing state
* idempotency keys
* active agreement or occupancy lookup
* document ownership
* audit correlation identifiers

Indexes should not be added to every column automatically.

## 56. Composite Indexes

Multi-tenant operational queries will often require organization-leading composite indexes.

Illustrative:

```text
organization_id, status, created_at
organization_id, property_id
organization_id, unit_id, status
organization_id, external_reference
```

Index design must reflect actual query predicates and ordering.

## 57. Query Plans

Critical queries should be evaluated with PostgreSQL query-planning tools in representative environments.

Priority workflows include:

* Listing discovery
* application review queues
* payment verification
* allocation
* resident balances
* active occupancy
* maintenance operations
* audit investigation
* outbox claiming
* AI retrieval preparation

Production query plans may differ materially from empty local databases.

## 58. Pagination

Large collections must use controlled pagination.

Cursor-based pagination is preferred where:

* Records change frequently
* stable ordering exists
* deep page offsets are expensive
* duplicate or skipped results would cause operational confusion

Offset pagination may remain suitable for limited stable datasets.

## 59. N+1 Query Prevention

Repositories and query services must avoid uncontrolled per-record query loops.

Controls may include:

* Explicit joins
* batched queries
* carefully scoped relation loading
* query-count tests
* tracing
* performance budgets

Avoiding N+1 behavior must not produce unrestricted cross-tenant joins.

## 60. Audit Records

Audit records should be append oriented.

Application code must not silently update historical audit meaning.

Audit records should preserve:

* Event type
* actor
* organization
* resource
* operation
* occurred time
* correlation identifier
* safe metadata
* source
* support-session context where applicable

Database-level audit triggers are not selected by this ADR as the primary business audit mechanism.

## 61. Outbox Records

Outbox records must be written in the same transaction as the authoritative state change that requires an asynchronous effect.

The worker must claim and process records idempotently.

Outbox records should support:

* Event identifier
* event type
* schema version
* aggregate or resource identifier
* organization identifier
* payload
* occurred time
* available time
* attempt count
* processing state
* correlation identifier

## 62. Seeding

Seed processes must be divided into:

### Structural seeds

Examples:

* Stable permission catalogue
* stable system classifications
* supported currency catalogue where approved
* platform configuration defaults

### Development fixtures

Examples:

* Synthetic organizations
* synthetic properties
* synthetic applicants
* synthetic financial records

Development fixtures must never contain real personal information.

## 63. Production Seeding

Production seed operations must be:

* Idempotent
* reviewed
* environment aware
* auditable where material
* safe to rerun
* separate from unrestricted development fixture loading

Production must not receive sample users or fictional financial data accidentally.

## 64. Test Databases

Automated tests should use isolated databases or schemas according to the test level.

Tests must support:

* Clean migrations
* deterministic fixtures
* transaction behavior
* constraint validation
* organization isolation
* concurrency
* idempotency
* RLS behavior when approved
* failure recovery

Mocking the repository alone is insufficient for critical persistence behavior.

## 65. Integration Tests

Integration tests are required for:

* Repository behavior
* transaction rollback
* foreign keys
* unique constraints
* check constraints
* migration application
* payment-allocation concurrency
* occupancy conflicts
* provider-event idempotency
* outbox behavior
* tenant isolation

## 66. Production Data in Tests

Production data must not be copied into local or CI databases without a separately approved sanitization process.

Synthetic data is the default.

Any production-derived dataset must be:

* Authorized
* minimized
* de-identified
* access controlled
* retention limited
* documented

## 67. Database Observability

Database instrumentation should record safe metadata such as:

* Operation name
* repository or query identifier
* duration
* success or failure
* timeout
* transaction outcome
* pool saturation
* connection acquisition time
* affected-row count where safe
* correlation identifier

Ordinary telemetry must not include:

* Passwords
* database URLs
* unrestricted query parameters
* identity-document contents
* payment evidence
* signed agreements
* raw AI retrieval text

## 68. Slow Queries

The system should establish environment-specific slow-query thresholds.

A slow-query review should consider:

* Query plan
* indexes
* lock waits
* row count
* tenant scope
* pagination
* transaction duration
* connection-pool contention
* external processing accidentally occurring inside the transaction

## 69. Database Errors

Database errors must be mapped into stable application outcomes.

Examples:

* Unique violation → conflict
* foreign-key violation → invalid relationship or conflict
* serialization failure → retryable conflict
* lock timeout → controlled retry or unavailable
* connection exhaustion → dependency unavailable
* check violation → invariant violation

Raw SQL errors must not be exposed to users.

## 70. Retry Policy

Only errors known to be transient and safe to repeat may be retried automatically.

Retries must consider:

* Transaction idempotency
* external side effects
* duplicate writes
* lock contention
* serialization failures
* connection interruption

A retry must not duplicate a payment allocation, receipt, agreement, notification, or provider action.

## 71. Backup and Recovery

The hosting and operational ADRs will determine exact backup infrastructure.

ADR-004 requires:

* Automated backups
* point-in-time recovery where supported
* encrypted backup storage
* restore testing
* documented recovery objectives
* controlled restore authority
* post-restore integrity validation

A backup that has never been restore tested is not sufficient recovery evidence.

## 72. Schema Documentation

The repository should document:

* Table purpose
* owning module
* sensitive fields
* organization ownership
* major relationships
* retention expectations
* append-only behavior
* important constraints
* RLS status when approved

Generated diagrams may supplement but not replace written ownership.

## 73. Codex Database Boundaries

Every Codex database work package must specify:

* Owning module
* schema files permitted
* repositories permitted
* migration requirement
* business invariants
* organization-scoping rule
* transaction boundary
* constraints and indexes
* test requirements
* prohibited destructive changes
* validation commands

Codex must not:

* Use schema push against shared environments
* edit applied migrations
* interpolate SQL
* add unrestricted database clients
* bypass repositories from UI code
* weaken constraints to make tests pass
* use floating-point money
* invent unknown pilot facts
* grant AI unrestricted database access
* create a generic CRUD layer over all tables
* add destructive cascading deletes without approval

## 74. Prohibited Patterns

The following are not approved:

* Direct database access from browser code
* direct database access from Client Components
* arbitrary ORM calls inside UI components
* material raw updates inside Route Handlers
* unrestricted ORM access in AI code
* shared production owner credentials for runtime traffic
* one database pool per feature
* mutable applied migrations
* production schema push
* automatic migration on application startup
* non-parameterized SQL
* long external calls inside database transactions
* floating-point authoritative money
* unknown data converted to defaults
* production data copied casually into tests
* universal cascade deletion
* database constraints omitted solely because application validation exists

## 75. Options Considered

### Option A — Prisma ORM and Prisma Migrate

Advantages:

* Strong developer tooling
* generated client
* broad adoption
* PostgreSQL support
* migration SQL history
* accessible relation APIs

Disadvantages:

* A higher-level data-access model than desired for PostgreSQL-heavy workflows
* greater risk that the generated client becomes the application architecture
* native PostgreSQL and specialized query behavior may require additional escape paths
* less direct SQL-oriented schema representation for the intended team model

Decision:

Not selected as the primary Habitacores toolkit.

### Option B — Kysely

Advantages:

* Strong SQL-oriented type-safe query building
* low abstraction
* explicit queries
* migration primitives
* good fit for advanced PostgreSQL work

Disadvantages:

* Requires a separately maintained or generated database type model
* provides less integrated TypeScript schema declaration
* would require more custom schema and migration conventions initially
* may increase foundational work for the small initial team

Decision:

Not selected as the primary toolkit.

Kysely may be reconsidered if Drizzle cannot support critical query requirements cleanly.

### Option C — Drizzle ORM and Drizzle Kit

Advantages:

* Type-safe TypeScript schema
* SQL-oriented behavior
* reviewable generated SQL migrations
* native PostgreSQL driver support
* explicit transactions
* raw SQL escape path
* relatively low abstraction
* strong fit for the selected monorepo

Disadvantages:

* Requires disciplined migration review
* API and toolkit upgrades require validation
* complex workflows still require deliberate SQL knowledge
* generated SQL must not be trusted blindly
* repository boundaries must be designed by Habitacores

Decision:

Approved.

### Option D — Raw `pg` and Handwritten SQL Only

Advantages:

* Maximum PostgreSQL control
* no ORM abstraction
* minimal runtime dependency
* explicit SQL

Disadvantages:

* More manual type synchronization
* more repetitive mapping
* greater opportunity for inconsistent query conventions
* more foundational work for routine operations
* reduced development speed for the initial team

Decision:

Rejected as the default.

Handwritten parameterized SQL remains approved within the Drizzle-based database package.

## 76. Positive Consequences

* PostgreSQL remains central rather than hidden.
* TypeScript schema and query safety improve.
* migrations remain visible as SQL.
* transaction boundaries remain explicit.
* advanced PostgreSQL capabilities remain available.
* repositories can preserve domain boundaries.
* the web and workers can share database infrastructure.
* organization-scoping controls can be standardized.
* AI database access can remain narrowly controlled.
* future RLS remains possible.
* migration behavior can be tested and reviewed.

## 77. Negative Consequences

* Developers must understand PostgreSQL and SQL.
* Drizzle does not eliminate migration design.
* repository and query-service code must be maintained.
* complex migrations require operational planning.
* pooling must match the deployment platform.
* native SQL and ORM types can drift if used carelessly.
* RLS requires a separate, detailed decision.
* production-safe migrations may require multiple deployments.

## 78. Risks

### 78.1 Missing Tenant Filter

Controls:

* Required repository context
* query helpers
* code review
* integration tests
* future RLS
* cross-tenant adversarial testing

### 78.2 ORM Becomes the Domain Model

Controls:

* Repository contracts
* domain entities and value objects
* application services
* no ORM records returned directly to UI by default

### 78.3 Unsafe Migration

Controls:

* Reviewed SQL
* staging
* lock analysis
* expand-and-contract
* backups
* forward-recovery plan

### 78.4 Connection Exhaustion

Controls:

* Central pool ownership
* replica-aware capacity planning
* monitoring
* managed pooling where appropriate
* worker concurrency limits

### 78.5 Long Transactions

Controls:

* Transaction instrumentation
* no external network calls
* short application-service transactions
* lock timeout
* operational alerts

### 78.6 AI Data Overreach

Controls:

* No general AI database client
* approved retrieval services
* authorization
* field allowlists
* redaction
* audit
* tenant-isolation evaluations

### 78.7 Applied Migration Mutation

Controls:

* Migration checks
* code review
* new corrective migration
* drift detection
* protected main branch

## 79. Acceptance Criterion — Repository Boundary

```text
Given:
A Server Action submits an application decision.

When:
The implementation is inspected.

Then:
The Server Action invokes an application service.

And:
The application service uses approved repositories.

And:
The Server Action does not import Drizzle or the PostgreSQL driver directly.
```

## 80. Acceptance Criterion — Tenant Isolation

```text
Given:
A repository request is made for an Organization A application.

When:
The database query executes.

Then:
The trusted Organization A scope is part of the query or approved database policy.

And:
No Organization B record can satisfy the query.
```

## 81. Acceptance Criterion — Transaction

```text
Given:
A payment is allocated to a charge.

When:
The allocation succeeds.

Then:
The allocation, financial state, audit event, and outbox event commit consistently.

And:
A failure before commit leaves no partial authoritative result.
```

## 82. Acceptance Criterion — Migration

```text
Given:
A schema change is proposed.

When:
The pull request is reviewed.

Then:
A committed SQL migration is present.

And:
The SQL has been inspected.

And:
The migration applies to a clean database.

And:
The migration applies to the representative previous schema.
```

## 83. Acceptance Criterion — Applied Migration

```text
Given:
A migration has been applied to staging.

When:
A correction is needed.

Then:
The applied migration is not edited.

And:
A new corrective migration is created.
```

## 84. Acceptance Criterion — AI Retrieval

```text
Given:
An approved AI use case needs application information.

When:
The AI worker requests the data.

Then:
It uses an approved permission-aware retrieval service.

And:
It does not receive a general unrestricted Drizzle or PostgreSQL client.

And:
Only authorized minimized fields are returned.
```

## 85. Acceptance Criterion — Unknown Pilot Data

```text
Given:
The number of Block B units remains unconfirmed.

When:
The property hierarchy is persisted.

Then:
No zero count or placeholder units are generated.

And:
The database preserves the value as unknown or pending according to the domain model.
```

## 86. Acceptance Criterion — Schema Push

```text
Given:
A migration targets staging or production.

When:
Deployment begins.

Then:
Committed migration files are applied.

And:
A schema-push command is not used.
```

## 87. Implementation Sequence

Recommended implementation order:

```text
1. Pin approved Drizzle ORM, Drizzle Kit, and pg versions
2. create packages/database
3. establish connection configuration
4. establish runtime and migration connection separation
5. establish schema naming conventions
6. create initial organization and identity schema
7. configure migration generation
8. generate and review the baseline SQL migration
9. establish clean-database migration tests
10. establish repository context
11. establish transaction abstraction
12. establish repository and query-service conventions
13. establish audit and outbox persistence
14. establish database observability
15. establish integration-test database lifecycle
16. add CI migration checks
17. keep shared-environment schema push prohibited
18. address RLS in a separate ADR
```

## 88. Deferred Decisions

This ADR does not decide:

* Exact Drizzle ORM version
* exact Drizzle Kit version
* exact `pg` version
* PostgreSQL hosting provider
* PostgreSQL major version
* connection-pool provider
* database-region topology
* identifier strategy
* money representation
* complete row-level-security model
* exact database roles
* encryption-key management
* field-level encryption
* backup provider
* recovery objectives
* read replicas
* analytical warehouse
* vector-extension strategy
* full-text-search strategy
* partitioning strategy
* database proxy
* data-retention schedule
* production anonymization tooling

These require later ADRs or bounded implementation decisions.

## 89. Supersession Conditions

This ADR should be reviewed when:

* Drizzle cannot represent critical PostgreSQL behavior safely.
* migration tooling becomes operationally unreliable.
* the selected deployment platform requires a different driver.
* database scale requires read replicas or partitioning.
* the application is split into independently owned services.
* the system adopts multiple authoritative databases.
* regulatory requirements require stronger database separation.
* PostgreSQL is no longer the appropriate authoritative system.
* RLS or connection identity requires a materially different access architecture.

## 90. Final Decision Statement

Habitacores will use PostgreSQL as its authoritative transactional system of record.

Drizzle ORM will provide TypeScript schema definitions and type-safe routine database access. Drizzle Kit will generate SQL migration files that must be committed, inspected, tested, and reviewed. The initial PostgreSQL driver will be `node-postgres`.

Database access will remain behind repositories and permission-aware query services. Application services will own material transaction boundaries. React components, browser code, arbitrary framework entry points, AI providers, and unapproved scripts will not receive unrestricted database access.

Production schema evolution will use immutable, forward-only, committed migrations. Shared environments will not use direct schema push. Potentially disruptive changes will use expand-and-contract deployment patterns, data verification, operational monitoring, and explicit recovery planning.

The application runtime will use a controlled pooled connection. Migrations and approved administrative operations will use a separate direct connection and least-privilege database roles.

Parameterized native SQL remains approved when it better expresses PostgreSQL behavior, performance, constraints, reporting, row-level security, concurrency, or operational requirements.

Application authorization remains mandatory. PostgreSQL row-level security is directionally supported but requires a separate ADR defining roles, policies, tenant context, connection-pool behavior, workers, support access, platform administration, and testing.

AI functionality will not receive a general-purpose database client. AI retrieval must pass through approved use cases, authentication, organization resolution, authorization, field allowlists, minimization, redaction, audit, and the Responsible AI Gateway.

No ORM record, SQL query, migration command, database role, connection pool, worker, framework route, coding agent, AI process, provider event, cache, or administrative tool may bypass Habitacores domain invariants, organization isolation, authorization, financial integrity, agreement integrity, occupancy integrity, audit requirements, migration governance, or Responsible AI controls.