# ADR-008 — Authorization, Tenant Isolation, and PostgreSQL Row-Level Security

## Decision Record

| Field | Value |
|---|---|
| Product | Habitacores |
| Decision | Authorization, Tenant Isolation, and PostgreSQL Row-Level Security |
| ADR | ADR-008 |
| Version | 0.1 |
| Status | Proposed |
| Decision Type | Foundational Security and Multi-Tenancy Architecture |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Authorization Authority | Habitacores application policy engine |
| Tenant-Isolation Defense | PostgreSQL Row-Level Security |
| Runtime Database Role | Non-owner, no `BYPASSRLS` |
| RLS Context | Transaction-local trusted context |
| Membership Source of Truth | Habitacores PostgreSQL records |
| Token Claims | Authentication hints, not sole authorization authority |
| Browser Database Access | Not approved for authoritative business tables |
| Privileged Access | Explicit, scoped, time-bound, audited |
| Responsible AI Access | Same authorization and RLS restrictions |
| Last Updated | 1 August 2026 |

## 1. Context

Habitacores is a multi-tenant real-estate operating ecosystem.

Its users may include:

- Prospects
- applicants
- guarantors
- residents
- property owners
- landlords
- agents
- estate managers
- finance reviewers
- maintenance personnel
- vendors
- estate lawyers
- support operators
- platform administrators

A user may:

- Belong to no organization
- belong to one organization
- belong to multiple organizations
- hold different roles in different organizations
- receive temporary assignments
- act personally as an applicant or resident
- act professionally on behalf of an organization
- receive controlled support access
- lose membership while retaining a valid authentication session

Habitacores must prevent:

- Cross-organization data disclosure
- cross-organization writes
- stale-role authorization
- assignment bypass
- support-session abuse
- privilege escalation
- unauthorized financial operations
- unauthorized agreement actions
- unauthorized occupancy actions
- unrestricted AI retrieval
- accidental tenant leakage through application defects

ADR-007 established that authentication proves identity, while Habitacores owns memberships, roles, permissions, assignments, and resource authorization.

ADR-008 defines how those policies are enforced through both application code and PostgreSQL.

## 2. Decision Drivers

The decision is driven by the need to:

1. Enforce strong organization isolation.
2. protect against missing tenant filters.
3. preserve current membership and permission state.
4. support users belonging to multiple organizations.
5. support resource and assignment-based policies.
6. support separation of duties.
7. support support-session access.
8. protect financial and legal records.
9. prevent stale token claims from granting access.
10. avoid RLS bypass by ordinary runtime roles.
11. remain compatible with pooled database connections.
12. preserve explicit application-service authorization.
13. protect workers and AI processes.
14. support public and personal records that are not organization-owned.
15. maintain auditable privileged access.
16. support future direct API clients safely.
17. avoid authorization logic scattered through UI code.
18. provide deterministic testing.
19. provide clear Codex boundaries.
20. maintain operationally manageable policies.

## 3. Decision

Habitacores will implement defense-in-depth authorization through:

```text
Authentication
→ Internal User Resolution
→ Account Status
→ Organization Resolution
→ Membership Validation
→ Permission Evaluation
→ Assignment and Resource Policy
→ Authentication-Assurance Check
→ Application Service
→ Transaction-Local Database Context
→ PostgreSQL RLS
→ Domain Rules
→ Audit and Outbox
````

Application authorization and PostgreSQL RLS have complementary responsibilities.

### Application authorization

Responsible for:

* Business permissions
* workflow state
* assignments
* role relationships
* separation of duties
* authentication assurance
* support-session scope
* field-level disclosure
* command eligibility
* human authority

### PostgreSQL RLS

Responsible primarily for:

* Tenant row visibility
* tenant row ownership
* preventing cross-organization reads
* preventing cross-organization inserts
* preventing cross-organization updates
* preventing cross-organization deletes
* defense against missing tenant predicates

RLS does not replace application authorization.

## 4. Core Authorization Model

Habitacores authorization will combine:

```text
RBAC
+ resource scope
+ assignment
+ relationship
+ record state
+ separation of duties
+ authentication assurance
+ explicit privileged context
```

A role name alone is insufficient.

## 5. Authorization Question

Every protected operation must answer:

```text
May this authenticated actor
perform this specific operation
on this specific resource
within this organization
in the resource's current state
under the actor's current assignment
with the required authentication assurance
at this time?
```

## 6. Internal Authorization Context

A trusted authorization context may contain:

```typescript
interface AuthorizationContext {
  actorId: UserId;
  organizationId?: OrganizationId;
  membershipId?: MembershipId;
  supportSessionId?: SupportSessionId;
  authenticationAssurance: AuthenticationAssurance;
  correlationId: CorrelationId;
  requestTime: Date;
}
```

The context must be constructed server side.

It must not be trusted directly from:

* Browser state
* URL parameters
* request JSON
* JWT custom role claims
* selected-organization cookies
* local storage
* AI output

## 7. Membership Source of Truth

Current organization membership will be resolved from Habitacores records.

A membership should preserve:

* User
* organization
* status
* effective time
* expiry where applicable
* suspension
* revocation
* invitation source
* role assignments
* audit history

A valid authentication session does not imply active membership.

## 8. Role Assignments

Roles will be organization scoped unless explicitly platform scoped.

Illustrative roles may include:

```text
Organization Owner
Organization Administrator
Property Manager
Agent
Finance Reviewer
Agreement Officer
Maintenance Coordinator
Vendor
Estate Lawyer
Resident
Platform Support
Platform Administrator
```

The exact permission catalogue remains authoritative over labels.

## 9. Permission Catalogue

Permissions should express operations such as:

```text
organization.view
organization.manage-memberships
property.view
property.manage
application.review
application.decide
finance.view
finance.verify-payment
finance.allocate-payment
finance.request-refund
finance.approve-refund
agreement.issue
agreement.activate
occupancy.activate
maintenance.assign
document.view-restricted
support.open-session
platform.manage-organization
```

Permissions must not be inferred from UI placement.

## 10. Resource Scope

A permission may apply to:

* Entire organization
* assigned properties
* assigned communities
* assigned buildings
* assigned applications
* assigned legal matters
* personal records
* support-session resources
* platform-wide resources

The resource scope must be evaluated explicitly.

## 11. Assignment-Based Authorization

Some users must see only assigned resources even when they hold a broader role category.

Examples:

* Agent assigned to selected properties
* lawyer assigned to selected matters
* vendor assigned to selected work orders
* reviewer assigned to selected applications
* maintenance staff assigned to selected buildings

RLS may enforce organization isolation, while the application service enforces assignment.

High-risk assignment boundaries may also receive database policies where practical and maintainable.

## 12. Relationship-Based Authorization

Personal users may receive access through relationships such as:

* Applicant owns application
* resident is party to occupancy
* guarantor is linked to application
* user is agreement party
* vendor is assigned to work order

These relationships must be current and database verifiable.

## 13. Record-State Authorization

Authorization may depend on state.

Examples:

* Draft application may be edited by applicant.
* submitted application may no longer be freely edited.
* verified payment cannot be destructively modified.
* issued agreement cannot be overwritten.
* completed work order may require reopening authority.
* expired support session grants no access.

## 14. Separation of Duties

Sensitive commands may require different actors.

Examples:

* Payment evidence submitter cannot verify the same evidence.
* refund requester cannot approve the same refund.
* refund approver may require another executor.
* privileged role grant may require elevated approval.
* support operator cannot expand their own session.
* agreement preparer may require independent approval.

Separation-of-duty checks remain application and domain policy.

## 15. Authentication Assurance

Sensitive permissions may require:

* MFA
* recent step-up authentication
* active secure session
* non-anonymous identity
* verified email
* additional identity verification where approved

RLS is not responsible for the complete assurance policy.

## 16. PostgreSQL RLS Decision

RLS will be enabled on organization-owned authoritative tables.

Examples include:

* Organization memberships
* properties
* communities
* buildings
* blocks
* floors
* units
* listings
* applications
* charges
* payments
* allocations
* agreements
* occupancies
* maintenance records
* documents
* organization communications
* organization support records
* AI-operation records containing organization data

## 17. Tables Requiring Classification

Every table must be classified as one of:

```text
ORGANIZATION_OWNED
PERSONAL
PUBLIC
PLATFORM
GLOBAL_REFERENCE
SYSTEM_INTERNAL
MIXED_OR_DERIVED
```

The classification determines:

* RLS requirement
* ownership columns
* database grants
* query patterns
* audit expectations
* retention policy
* test coverage

## 18. Organization Ownership Column

Organization-owned rows should normally contain:

```text
organization_id
```

directly.

The architecture should avoid relying on long multi-table joins merely to discover tenant ownership for every policy.

A direct organization identifier improves:

* Policy clarity
* indexing
* testing
* query planning
* operational inspection
* defense against orphaned relationships

## 19. Denormalized Ownership Integrity

Where `organization_id` is repeated on a child record, it must remain consistent with its parent.

Controls may include:

* Composite foreign keys
* application validation
* database constraints
* controlled triggers where justified
* transaction tests

RLS safety must not depend on a mutable, unverified denormalized value.

## 20. RLS Default-Deny Principle

For protected tables:

```text
RLS enabled
+ no applicable policy
= no access
```

Every migration introducing an organization-owned table must define:

* Ownership classification
* RLS status
* applicable policies
* grants
* tests

## 21. Force Row-Level Security

Protected tables should use:

```sql
ALTER TABLE table_name ENABLE ROW LEVEL SECURITY;
ALTER TABLE table_name FORCE ROW LEVEL SECURITY;
```

where consistent with the approved ownership and migration model.

The migration owner and runtime user must remain separate.

## 22. Runtime Database Role

The normal application runtime role must:

* Not own protected tables
* not be a superuser
* not have `BYPASSRLS`
* not have uncontrolled schema modification rights
* receive only required table and function privileges
* be environment specific

The same principle applies to ordinary worker roles.

## 23. Migration Owner

A separate migration role may:

* Own application schemas and tables
* create policies
* alter schemas
* apply approved migrations

Migration credentials must not be used by ordinary web requests or workers.

## 24. Service-Role Credentials

Supabase service-role credentials or equivalent administrative credentials must not be:

* Exposed to browser code
* used by ordinary user-request data access
* embedded in Client Components
* logged
* placed in coding-agent prompts
* sent to AI providers

Administrative bypass credentials require explicit operational use and audit.

## 25. Database Context Strategy

The server will install trusted authorization context inside a database transaction.

Conceptual flow:

```text
Begin transaction
→ Set actor context locally
→ Set organization context locally
→ Set membership or support context locally where needed
→ Execute repositories and queries
→ Commit or roll back
→ Context disappears
```

## 26. Transaction-Local Context

Context must use transaction-local behavior.

Conceptual PostgreSQL configuration keys:

```text
habitacores.actor_id
habitacores.organization_id
habitacores.membership_id
habitacores.support_session_id
habitacores.authorization_mode
```

Exact names will be finalized during implementation.

## 27. Connection-Pool Safety

Persistent session-level tenant context is prohibited for pooled runtime connections.

Prohibited pattern:

```text
Check out pooled connection
→ SET organization context for session
→ return connection to pool
```

This risks one request inheriting another request’s context.

Approved pattern:

```text
Begin transaction
→ SET LOCAL or equivalent transaction-local configuration
→ execute
→ commit or rollback
```

## 28. Context Validation

Before context is installed, the application must validate:

* Authenticated internal user
* account status
* requested organization
* active membership
* membership relationship
* support session where applicable
* required assurance

The database must not accept an arbitrary organization ID supplied by the browser as trusted context.

## 29. RLS Context Functions

RLS policies should use narrowly scoped helper functions.

Conceptual examples:

```sql
habitacores.current_actor_id()
habitacores.current_organization_id()
habitacores.current_membership_id()
habitacores.current_support_session_id()
```

Functions must:

* Return typed validated values
* fail safely when context is absent or malformed
* use controlled `search_path`
* avoid dynamic SQL
* avoid leaking other tenant information

## 30. Basic Organization Policy

A directional policy may resemble:

```sql
USING (
  organization_id = habitacores.current_organization_id()
)
WITH CHECK (
  organization_id = habitacores.current_organization_id()
)
```

The final policy must be migration reviewed and tested.

## 31. `USING` and `WITH CHECK`

Policies must distinguish:

* Which existing rows may be read or targeted
* which new or modified rows may be created

A read-safe policy does not automatically make writes safe unless the corresponding write checks are correct.

## 32. Command-Specific Policies

Policies should be command specific when this improves clarity:

```text
SELECT
INSERT
UPDATE
DELETE
```

Broad `FOR ALL` policies should not be used automatically.

Deletion may be denied entirely for append-only or legally significant records.

## 33. Permissive and Restrictive Policies

Policy-combination behavior must be deliberate.

Where multiple policies exist, reviewers must understand whether they combine through permissive or restrictive behavior.

Adding another permissive policy must not accidentally broaden access.

## 34. Application Authorization Remains Mandatory

Even where RLS permits a row, application authorization must still determine whether the actor can:

* Perform the command
* see restricted fields
* act in the current workflow state
* satisfy separation of duties
* satisfy assignment rules
* satisfy authentication assurance
* perform a high-risk transition

RLS row visibility is not equivalent to complete business authorization.

## 35. Field-Level Security

RLS protects rows, not complete field disclosure policy.

Sensitive fields may require:

* Dedicated query services
* restricted views
* column grants
* separate tables
* encryption
* redacted read models
* field-level application policies

Examples:

* Government identification
* bank information
* legal notes
* payment evidence
* support notes
* AI safety findings
* provider secrets

## 36. Public Records

Public listing data should be exposed through:

* Approved public query services
* controlled views
* intentionally public API contracts

The public surface must contain only approved fields.

A public listing does not make the underlying property, owner, agreement, applicant, or financial records public.

## 37. Personal Records

Personal records may use ownership relationships instead of organization membership.

Examples:

* Personal profile
* applicant-owned draft
* resident personal notifications
* personal document access

Policies must clearly distinguish personal ownership from organization ownership.

## 38. Platform Records

Platform-wide records require dedicated platform authorization.

A platform administrator must not receive unrestricted database-owner access merely because they administer the product.

Platform access should use:

* Explicit platform permissions
* application services
* scoped operational tools
* audit
* step-up authentication
* least privilege

## 39. Support Sessions

Support access must use explicit support-session records.

A support session should define:

* Support actor
* target organization
* target user where applicable
* reason
* permitted resource categories
* permitted operations
* start time
* expiry
* approval
* termination
* audit

## 40. Support Context

A valid support session may establish restricted access beyond ordinary membership.

It must not:

* Grant unlimited organization authority
* bypass financial separation of duties
* reveal unrelated tenants
* survive expiry
* permit hidden impersonation
* allow the operator to alter their own scope

## 41. Background Workers

Workers must operate under one of:

* Original actor context
* explicit system operation context
* organization-scoped workload context
* approved platform operation

A worker must not use an unrestricted bypass role merely for convenience.

## 42. System Operations

Some internal operations may require controlled elevated access, such as:

* Outbox dispatch
* scheduled expiry
* reconciliation
* retention enforcement
* integrity checks
* platform migration

Such operations require:

* Dedicated database role or controlled function
* operation identifier
* narrowly defined scope
* no browser exposure
* observability
* audit where material

## 43. AI Workers

AI workers receive no general tenant-bypass access.

Approved flow:

```text
Approved AI use case
→ actor or workload identity
→ organization scope
→ permission-aware retrieval
→ RLS-protected transaction
→ field allowlist
→ minimization and redaction
→ AI Gateway
```

AI retrieval must fail safely when tenant context is absent.

## 44. AI Cross-Tenant Protection

AI evaluation suites must include:

* Valid foreign-tenant UUIDs
* semantic-search cross-tenant attempts
* prompt-injection attempts requesting other organizations
* support-session expiry
* stale membership
* unauthorized document references
* cached retrieval leakage

## 45. Direct Browser Database Access

Direct browser access to authoritative Habitacores business tables is not approved for the MVP.

The browser will use:

* Server Actions
* Route Handlers
* server-rendered query services
* approved upload flows

This allows application authorization and domain policies to execute before database access.

## 46. Future Direct Access

Direct client-to-database access may be reconsidered only for a narrowly scoped use case where:

* RLS policies fully express access
* field exposure is controlled
* rate limiting exists
* abuse protection exists
* schema coupling is accepted
* revocation behavior is tested
* no protected domain command is bypassed
* a separate review approves it

## 47. Drizzle Integration

Drizzle must execute protected queries through the approved transaction wrapper.

Conceptual flow:

```typescript
await database.authorizedTransaction(context, async (tx) => {
  return repository.findApplication(tx, applicationId);
});
```

The transaction wrapper should:

1. Begin the transaction.
2. install trusted local context.
3. verify expected context where appropriate.
4. execute the callback.
5. commit or roll back.
6. clear context through transaction completion.

## 48. Repository Requirements

Repositories for organization-owned resources must require:

* Authorized transaction
* organization-aware context
* typed identifiers

Repositories must not create hidden unscoped connections.

## 49. Query Services

Query services must:

* Define the requested organization
* evaluate application authorization
* select permitted fields
* execute within RLS context
* return safe read models

A query service must not load all organizations and filter afterward.

## 50. Cross-Organization Platform Queries

Legitimate platform-wide queries must use dedicated, reviewed paths.

Examples:

* Security investigation
* organization onboarding
* aggregate platform health
* abuse detection

They must not reuse ordinary tenant repository methods with RLS casually disabled.

## 51. Database Views

Views may provide:

* Public projections
* restricted operational projections
* reporting models
* redacted records

View security behavior must be reviewed carefully.

A view must not unintentionally execute with privileges that bypass the intended RLS boundary.

## 52. Security-Definer Functions

Security-definer functions are permitted only when:

* The use case cannot be expressed safely otherwise.
* input is validated.
* `search_path` is fixed safely.
* privileges are minimal.
* the function is narrowly scoped.
* SQL injection is impossible.
* audit is included where material.
* tests prove tenant isolation.

They must not become a generic bypass API.

## 53. Stored Procedures

Stored procedures may be used for narrowly defined atomic operations.

They do not replace application services by default.

A stored procedure must still preserve:

* Authorization inputs
* tenant scope
* domain invariants
* audit
* idempotency
* error mapping
* migration governance

## 54. RLS and Foreign Keys

Foreign-key checks and referential behavior must be reviewed separately from ordinary row visibility.

The system must not assume RLS alone prevents every cross-tenant relationship.

Composite ownership constraints or explicit validation may be required.

## 55. Cross-Tenant Foreign-Key Prevention

For tenant-owned parent and child records, the database should prevent relationships such as:

```text
Organization A payment
→ Organization B charge
```

Potential control:

```text
Composite unique key:
organization_id, id

Composite foreign key:
child.organization_id, child.parent_id
→ parent.organization_id, parent.id
```

## 56. Membership Revocation

After membership revocation:

* New protected requests must fail.
* new RLS context must not be installed.
* stale JWT claims must not restore access.
* long-running jobs must revalidate where required.
* shared caches must not continue exposing data.
* active privileged sessions may be revoked according to policy.

## 57. Role Changes

Role and permission changes take effect from current Habitacores records.

Token refresh must not be required for authoritative permission removal.

JWT claims may remain optimization hints only.

## 58. Cache Interaction

Cached protected results must not outlive:

* Membership revocation
* account suspension
* role removal
* assignment removal
* support-session expiry
* record confidentiality change

RLS does not protect a value already leaked into an unsafe application cache.

## 59. Audit

Material authorization events should be audited, including:

* Role grant
* role removal
* permission override
* assignment
* assignment removal
* membership suspension
* membership revocation
* support-session creation
* support-session use
* support-session extension
* authorization denial for high-risk action
* privileged platform access
* system bypass operation

## 60. Authorization Denials

Not every denial requires a permanent audit record.

Security-significant denials may include:

* Repeated cross-tenant attempts
* privileged-operation denial
* support-session misuse
* step-up bypass attempt
* restricted-document enumeration
* financial separation-of-duty violation

Operational telemetry and audit policy should avoid overwhelming noise.

## 61. Safe Error Behavior

Protected requests may return a consistent inaccessible result rather than revealing whether another tenant’s resource exists.

Potential outcomes:

```text
AUTHENTICATION_REQUIRED
ACCOUNT_INACTIVE
ORGANIZATION_ACCESS_DENIED
PERMISSION_DENIED
ASSIGNMENT_REQUIRED
STEP_UP_REQUIRED
RESOURCE_NOT_ACCESSIBLE
SEPARATION_OF_DUTY_VIOLATION
SUPPORT_SESSION_REQUIRED
SUPPORT_SESSION_EXPIRED
```

## 62. RLS Error Mapping

Database errors resulting from RLS or constraints must be converted to stable application outcomes.

Raw PostgreSQL policy or table details must not be exposed to users.

## 63. Testing Strategy

Authorization testing must include:

* Permission matrix tests
* role tests
* assignment tests
* record-state tests
* separation-of-duty tests
* MFA and step-up tests
* organization-isolation tests
* RLS policy tests
* missing-context tests
* malformed-context tests
* support-session tests
* worker tests
* AI retrieval tests
* service-role exposure tests

## 64. Cross-Tenant Matrix

For every organization-owned resource, tests should evaluate:

```text
Organization A actor + Organization A resource
Organization A actor + Organization B resource
No organization context + Organization A resource
Revoked member + former organization resource
Platform actor without required permission + tenant resource
Expired support session + target resource
```

## 65. CRUD Policy Tests

Each RLS-protected table should test:

* SELECT permitted
* SELECT denied
* INSERT permitted
* INSERT wrong organization denied
* UPDATE permitted
* UPDATE ownership change denied
* DELETE permitted where supported
* DELETE denied where prohibited
* absent context denied

## 66. Table-Owner Bypass Tests

Tests must verify that the ordinary runtime role:

* Does not own protected tables
* does not have `BYPASSRLS`
* remains subject to RLS
* cannot disable policies
* cannot alter protected schemas

## 67. Migration Tests

A migration adding an organization-owned table should fail review or CI when:

* `organization_id` is missing without justification.
* RLS is not enabled.
* required policies are missing.
* grants are too broad.
* policy columns are not indexed where required.
* runtime ownership is incorrect.
* cross-tenant tests are missing.

## 68. Performance

RLS policy columns must be indexed according to query and policy usage.

Likely indexes include:

```text
organization_id
organization_id, status
organization_id, resource_id
user_id, organization_id, status
support_session_id, expires_at
```

Policy performance must be evaluated with representative data.

## 69. Policy Complexity

RLS policies should remain simple.

Preferred:

```text
Direct tenant ownership
Simple personal ownership
Narrow support-session predicate
```

Avoid embedding the complete Habitacores permission matrix into every table policy.

Complex business authorization belongs in application services.

## 70. Operational Inspection

Operations should be able to inspect:

* Tables with RLS enabled
* tables missing RLS classification
* policy definitions
* table ownership
* roles with `BYPASSRLS`
* excessive grants
* policy test coverage
* recently changed policies

## 71. Privilege Review

Database privileges should be reviewed regularly.

Review should identify:

* Superusers
* table owners
* `BYPASSRLS` roles
* schema create privileges
* direct login roles
* service-role use
* stale credentials
* excessive grants

## 72. Emergency Access

Emergency database access requires:

* Named authorized operator
* incident or change reference
* time-bounded credentials where possible
* step-up authentication
* audit
* commands or queries recorded according to policy
* post-access review
* credential rotation where necessary

Emergency access must not become routine operations.

## 73. Data Exports

Exports must use application authorization and RLS-protected retrieval.

A broad export requires:

* Explicit permission
* organization scope
* field allowlist
* reason
* size controls
* audit
* secure delivery
* expiry

## 74. Analytics

Analytics pipelines must not use unrestricted production access casually.

Approved analytics access requires:

* Purpose limitation
* minimized data
* tenant or aggregate scope
* service identity
* controlled extraction
* retention
* security review

## 75. Responsible AI Boundary

AI must never determine authorization.

AI may:

* Explain permission-denial messages
* summarize authorized policy information
* help operators review policy documentation
* classify suspected access incidents

AI must not:

* Grant access
* select an organization context
* create support sessions
* expand support scope
* override RLS
* determine membership
* assign roles
* receive bypass credentials

## 76. Codex Boundaries

Every Codex authorization work package must specify:

* Resource type
* ownership classification
* organization column
* required permissions
* assignment rules
* state rules
* separation-of-duty rules
* assurance requirement
* RLS policies
* grants
* indexes
* audit requirements
* test matrix

Codex must not:

* Disable RLS
* grant `BYPASSRLS`
* make the runtime role a table owner
* use a service role in browser code
* trust user metadata for authorization
* trust stale JWT claims as final authority
* use session-level pooled tenant context
* omit `WITH CHECK`
* create broad permissive policies
* bypass application services
* expose administrative credentials
* give AI unrestricted retrieval

## 77. Prohibited Patterns

The following are not approved:

* Client-side authorization as final authority
* hidden navigation treated as access control
* role labels without permission evaluation
* JWT claims as sole authorization authority
* user-editable metadata storing authorization
* active-organization cookies as tenant authority
* missing organization predicates
* RLS disabled on organization-owned exposed tables
* runtime role with `BYPASSRLS`
* runtime role owning protected tables
* session-persistent tenant context in pooled connections
* service-role credentials in browser code
* unrestricted support impersonation
* RLS used as the complete business-policy engine
* direct browser writes to authoritative finance or agreement tables
* AI-generated authorization decisions

## 78. Options Considered

### Option A — Application Authorization Only

Advantages:

* Simpler database configuration
* all policy logic in TypeScript
* fewer PostgreSQL concepts

Disadvantages:

* A missing tenant predicate can expose data.
* repository defects become tenant-boundary defects.
* scripts and workers may bypass consistent filters.
* weaker defense in depth.

Decision:

Rejected.

### Option B — RLS as the Complete Authorization System

Advantages:

* Database-centered enforcement
* consistent row filtering
* useful for direct database APIs

Disadvantages:

* Complex workflow rules become difficult to maintain.
* field-level and stateful authorization become awkward.
* separation of duties and step-up authentication do not fit cleanly.
* policies become difficult to test and evolve.

Decision:

Rejected.

### Option C — Application Authorization Plus RLS Tenant Isolation

Advantages:

* Strong defense in depth
* application retains business-policy clarity
* database prevents cross-tenant row access
* compatible with workers and future clients
* limits impact of missing tenant filters
* supports explicit privileged access

Disadvantages:

* More implementation work
* database-role design is required
* pooled context must be handled carefully
* policy performance requires testing
* two enforcement layers must remain aligned

Decision:

Approved.

### Option D — Separate Database Per Organization

Advantages:

* Strong physical tenant separation
* easier tenant-level backup or deletion in some cases

Disadvantages:

* Operationally expensive
* migration coordination
* connection management
* cross-tenant platform operations become harder
* premature for the MVP
* inefficient for many smaller organizations

Decision:

Rejected for the MVP.

## 79. Positive Consequences

* Stronger tenant isolation
* defense against missing application filters
* current membership remains authoritative
* role changes can take effect without token refresh
* ordinary runtime processes remain least privileged
* support and platform access become explicit
* workers and AI remain scoped
* future APIs can reuse the same boundary
* cross-tenant security becomes systematically testable

## 80. Negative Consequences

* RLS policies require specialized knowledge.
* transaction-local context requires infrastructure.
* database roles and ownership must be managed carefully.
* debugging authorization may involve two layers.
* policy migrations require elevated review.
* representative performance testing is necessary.
* direct browser database access remains limited.

## 81. Risks

### 81.1 Runtime Role Bypasses RLS

Controls:

* Non-owner runtime role
* no `BYPASSRLS`
* privilege tests
* automated inspection
* separate migration credentials

### 81.2 Pooled Connection Context Leakage

Controls:

* Transaction-local context
* no session-persistent tenant settings
* rollback testing
* pool reuse tests

### 81.3 Stale JWT Permissions

Controls:

* Current membership database lookup
* application policy evaluation
* claims treated as hints
* session revocation when required

### 81.4 Overly Broad Policy

Controls:

* Command-specific policies
* policy review
* cross-tenant tests
* default deny
* restrictive policy where appropriate

### 81.5 Complex RLS Performance

Controls:

* Simple ownership policies
* indexed predicates
* query-plan review
* representative load tests

### 81.6 AI Retrieval Leakage

Controls:

* Same RLS context
* field allowlists
* no bypass role
* adversarial evaluations
* audit

## 82. Acceptance Criterion — Cross-Tenant Read

```text
Given:
A user is an active member of Organization A.

And:
An application belongs to Organization B.

When:
The user or an application defect attempts to retrieve the Organization B application under Organization A context.

Then:
PostgreSQL RLS returns no accessible row.

And:
The application does not reveal whether the resource exists.
```

## 83. Acceptance Criterion — Wrong-Tenant Insert

```text
Given:
The trusted transaction context is Organization A.

When:
Code attempts to insert a payment with Organization B ownership.

Then:
The database rejects the insert.

And:
No Organization B record is created.
```

## 84. Acceptance Criterion — Revoked Membership

```text
Given:
A user retains a technically valid authentication token.

And:
The user's Organization A membership has been revoked.

When:
The user requests an Organization A resource.

Then:
Habitacores refuses to install an authorized Organization A transaction context.

And:
The request is denied.
```

## 85. Acceptance Criterion — Pool Reuse

```text
Given:
Request 1 uses a pooled connection for Organization A.

And:
The connection later serves Request 2 for Organization B.

When:
Request 2 begins.

Then:
No Organization A context remains.

And:
Only transaction-local Organization B context applies.
```

## 86. Acceptance Criterion — Runtime Role

```text
Given:
The production runtime database role is inspected.

Then:
It is not a superuser.

And:
It does not own protected tables.

And:
It does not have BYPASSRLS.

And:
It cannot disable RLS.
```

## 87. Acceptance Criterion — Application Policy

```text
Given:
RLS permits a finance reviewer to see an Organization A payment row.

When:
The reviewer attempts to approve their own submitted evidence.

Then:
The application service rejects the command under separation-of-duty policy.

And:
Row visibility does not imply command authority.
```

## 88. Acceptance Criterion — AI Retrieval

```text
Given:
An approved AI use case operates for Organization A.

When:
The retrieval service requests supporting records.

Then:
The transaction uses Organization A RLS context.

And:
No Organization B row is retrievable.

And:
The AI worker has no bypass credential.
```

## 89. Acceptance Criterion — Missing Context

```text
Given:
A protected repository is called without trusted organization context.

When:
The query executes.

Then:
The operation fails safely or returns no protected rows.

And:
It does not default to unrestricted access.
```

## 90. Implementation Sequence

Recommended implementation order:

```text
1. Finalize table ownership classifications
2. create migration-owner and runtime-role model
3. ensure runtime role does not own tables
4. create transaction-local context functions
5. create authorized transaction wrapper
6. implement organization-membership validation
7. implement application permission evaluator
8. enable and force RLS on initial organization-owned tables
9. create command-specific ownership policies
10. create composite tenant-integrity constraints
11. create RLS policy indexes
12. establish support-session context
13. establish worker operation contexts
14. keep AI workers under ordinary scoped access
15. create cross-tenant policy tests
16. create pooled-connection leakage tests
17. create role and privilege inspection checks
18. keep direct browser access disabled
```

## 91. Deferred Decisions

This ADR does not decide:

* Exact initial permission catalogue
* exact role-to-permission mapping
* exact authorization library
* exact database role names
* exact PostgreSQL context-variable names
* exact support-session approval matrix
* exact field-level encryption model
* exact column-level grant strategy
* exact public-view implementation
* exact analytics-access architecture
* exact emergency-access tooling
* exact policy-inspection tooling
* exact direct-client database use cases
* database-per-tenant enterprise tier
* regional tenant isolation
* organization-specific encryption keys

These require bounded implementation decisions or later ADRs.

## 92. Supersession Conditions

This ADR should be reviewed when:

* Habitacores introduces direct client-to-database access.
* database-per-tenant isolation becomes required.
* regulatory requirements require physical tenant separation.
* RLS performance becomes unacceptable.
* multiple independently deployed services own separate databases.
* enterprise customers require dedicated environments.
* support access requirements materially change.
* a new data platform replaces PostgreSQL.
* the authorization model moves beyond the current role, assignment, and relationship structure.

## 93. Final Decision Statement

Habitacores will use defense-in-depth authorization consisting of server-side application authorization and PostgreSQL Row-Level Security.

Habitacores application services will remain authoritative for permissions, resource scope, assignment, workflow state, separation of duties, authentication assurance, support access, field disclosure, and protected domain commands.

PostgreSQL RLS will provide mandatory row-level tenant isolation for organization-owned authoritative tables.

Protected tables will be owned by a migration or schema-owner role. Ordinary web and worker runtime roles will not own protected tables, will not be superusers, and will not possess `BYPASSRLS`.

Trusted actor, organization, membership, support, and operation context will be installed transaction locally. Persistent session-level tenant context will not be used with pooled connections.

Organization-owned records will normally carry a directly accessible `organization_id`. Database constraints will prevent cross-tenant parent-child relationships where practical.

Authentication-provider claims, custom JWT claims, user-editable metadata, selected-organization cookies, route parameters, browser state, UI visibility, URL possession, and AI output will not independently establish authorization.

Current Habitacores account, membership, role, permission, assignment, support-session, resource, and authentication-assurance state will be evaluated for protected operations.

Direct browser access to authoritative Habitacores business tables is not approved for the MVP. Browser interactions will pass through server-controlled application boundaries.

Support operators, platform administrators, workers, integrations, exports, analytics processes, and AI workers will use explicit identities, scopes, and audited authorization paths rather than unrestricted tenant-bypass credentials.

RLS will not be treated as the complete business-policy engine. A row visible through RLS may still be prohibited from a particular operation by application authorization or domain rules.

No identity provider, JWT, cookie, browser, database role, table owner, migration, connection pool, worker, support session, service credential, query, cache, coding agent, or AI process may bypass Habitacores tenant isolation, membership status, permission policy, assignment, separation of duties, authentication assurance, financial controls, agreement controls, occupancy controls, document security, audit requirements, or Responsible AI safeguards.