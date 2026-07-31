# ADR-002 — Application Architecture and Next.js Runtime Model

## Decision Record

| Field | Value |
|---|---|
| Product | Habitacores |
| Decision | Application Architecture and Next.js Runtime Model |
| ADR | ADR-002 |
| Version | 0.1 |
| Status | Proposed |
| Decision Type | Foundational Application Architecture |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Repository Model | TypeScript monorepo |
| Business Architecture | Modular monolith |
| Web Framework | Next.js |
| Routing Model | App Router |
| Primary Rendering Model | Server Components |
| Primary Runtime | Node.js |
| Client Architecture | Selective Client Components |
| First-Party Mutation Model | Server Actions through application services |
| External HTTP Interface | Route Handlers |
| Background Processing | Dedicated worker |
| AI Processing | Governed AI Gateway and independently bounded AI worker |
| Primary System of Record | PostgreSQL |
| Last Updated | 30 July 2026 |

## 1. Context

ADR-001 approved a TypeScript monorepo containing:

- A Next.js web application
- a deterministic background worker
- an independently bounded AI worker
- shared internal packages
- Responsible AI infrastructure
- infrastructure definitions
- testing and engineering tooling

ADR-002 must determine how the Next.js web application will operate internally.

Habitacores requires more than a public property-listing website.

The web application must support:

- Public property discovery
- prospect inquiries
- inspection requests
- applicant registration
- structured rental applications
- application review
- annual-rent financial workflows
- payment evidence
- payment verification
- payment allocation
- receipts
- agreement generation
- signatures
- occupancy activation
- resident self-service
- maintenance requests
- vendor work orders
- legal assignments
- organization administration
- platform administration
- support sessions
- Responsible AI assistance

The application architecture must prevent framework capabilities from bypassing:

- Organization isolation
- authorization
- payment controls
- agreement controls
- occupancy prerequisites
- document protections
- audit requirements
- Responsible AI controls

## 2. Decision Drivers

The decision is driven by the need to:

1. Support secure server-rendered experiences.
2. Minimize browser JavaScript for mobile and constrained networks.
3. preserve server-side authorization.
4. preserve modular-monolith boundaries.
5. avoid business logic in React components.
6. distinguish user-interface mutations from public or machine-facing APIs.
7. support webhooks and external integrations.
8. support future mobile and partner clients.
9. provide predictable caching rules.
10. prevent tenant data from entering shared caches incorrectly.
11. support background jobs.
12. support direct-to-storage uploads.
13. support streaming without false workflow completion.
14. support accessible forms and progressive enhancement.
15. maintain stable application-service boundaries.
16. support Responsible AI without direct model calls from UI code.
17. permit future extraction without premature microservices.
18. provide clear implementation boundaries for Codex.

## 3. Decision

Habitacores will use the Next.js App Router as the primary web-application architecture.

The approved model is:

```text
Browser
→ Next.js App Router
→ Server Component, Server Action, or Route Handler
→ Request Context
→ Authentication
→ Organization Resolution
→ Authorization
→ Application Service
→ Domain
→ Repository or Integration Port
→ Transaction
→ Audit and Outbox
→ Response
````

The architecture will use:

* Server Components by default
* Client Components selectively
* Server Actions for approved first-party UI mutations
* Route Handlers for external, protocol-level, upload, webhook, and reusable HTTP interfaces
* Node.js runtime by default
* application services as the protected workflow boundary
* deterministic domain services for authoritative business actions
* background workers for durable asynchronous work
* an AI Gateway for approved AI use cases
* explicit caching rules
* explicit revalidation after successful mutations

## 4. App Router

The application will use:

```text
apps/web/app/
```

The Pages Router is not approved for new Habitacores implementation.

The App Router will provide:

* Nested layouts
* route groups
* Server Components
* Client Components
* loading boundaries
* error boundaries
* not-found handling
* Route Handlers
* Server Actions
* streaming
* metadata
* route-level runtime configuration

## 5. Proposed Application Directory

The following is the approved directional structure:

```text
apps/web/
├── app/
│   ├── (public)/
│   ├── (auth)/
│   ├── (portal)/
│   ├── (organization)/
│   ├── (platform)/
│   ├── api/
│   ├── actions/
│   ├── error.tsx
│   ├── global-error.tsx
│   ├── layout.tsx
│   ├── loading.tsx
│   └── not-found.tsx
│
├── components/
├── features/
├── lib/
├── server/
├── styles/
├── tests/
├── next.config.ts
└── instrumentation.ts
```

The exact structure may evolve without changing the architectural principles in this ADR.

## 6. Route Groups

Route groups should separate experience boundaries without necessarily changing public URLs.

Recommended groups:

```text
(public)
(auth)
(portal)
(organization)
(platform)
```

### `(public)`

May contain:

* Public home
* public property discovery
* listing search
* listing details
* inquiry
* inspection request
* public information pages

### `(auth)`

May contain:

* Registration
* email verification
* login
* password recovery
* MFA
* invitation acceptance

### `(portal)`

May contain:

* Applicant portal
* guarantor portal
* resident portal
* personal documents
* personal notifications
* personal support

### `(organization)`

May contain:

* Organization dashboard
* property operations
* applications
* finance
* agreements
* occupancy
* maintenance
* vendors
* legal workspace
* organization administration

### `(platform)`

May contain:

* Platform operations
* organization onboarding
* security-event review
* support operations
* controlled provider configuration
* platform administration

## 7. Server Components by Default

React components in the App Router will remain Server Components unless browser execution is required.

Server Components should be used for:

* Authenticated page composition
* data retrieval
* organization-context resolution
* permission-aware view construction
* initial form data
* dashboards
* listing pages
* detail views
* read-only financial views
* agreement views
* occupancy views
* reports
* secure server-only dependency access

Benefits for Habitacores include:

* Reduced client JavaScript
* improved mobile performance
* server-side access to protected services
* no browser exposure of credentials
* simpler initial data loading
* easier composition of secure read models

## 8. Client Components

A component should use `"use client"` only when it requires:

* Browser event handlers
* local interactive state
* lifecycle behavior
* browser APIs
* client-side form enhancement
* drag-and-drop
* file-selection interaction
* complex table interaction
* local optimistic presentation
* rich editor behavior
* map interaction
* device-specific functionality

Client Components must remain as narrow as practical.

A Client Component must not receive more protected information than it needs.

## 9. Client Boundary Rule

The `"use client"` directive creates a client-execution boundary.

Therefore:

* Server-only packages must not be imported below that boundary.
* secrets must not be passed as props.
* unrestricted database records must not be serialized.
* authorization must not be delegated to client logic.
* provider credentials must not enter client bundles.
* restricted document storage keys must not be exposed.
* AI-provider configuration must not enter client bundles.

## 10. Server-Only Modules

Sensitive application modules should be marked or organized as server only.

Server-only modules include:

* Database repositories
* authorization evaluators
* audit writers
* secret access
* payment adapters
* signature adapters
* email adapters
* file-scanning adapters
* AI Gateway
* support-session evaluation
* privileged platform operations

Build or lint controls should prevent accidental client imports.

## 11. Rendering Model

Habitacores will use three broad rendering strategies:

### Static or Cached Public Rendering

Suitable for:

* Public informational pages
* approved public marketing content
* approved public listings where controlled caching is safe
* legal information pages
* public help content

### Dynamic Server Rendering

Required for:

* Applicant portal
* resident portal
* organization operations
* finance
* agreements
* occupancy
* maintenance
* platform administration
* support sessions
* permission-sensitive pages

### Streaming

May be used for:

* Dashboards
* slow report panels
* secondary analytics
* non-authoritative AI assistance
* non-critical provider status
* large read-only views

Streaming must not represent a pending protected action as completed.

## 12. Request Context

Every protected server request should establish a request context.

Illustrative structure:

```typescript
interface RequestContext {
  correlationId: string;
  actorId: string;
  sessionId: string;
  activeOrganizationId?: string;
  membershipId?: string;
  supportSessionId?: string;
  authenticationStrength: string;
  requestTime: Date;
}
```

The context must not be constructed from untrusted client claims alone.

## 13. Authentication Boundary

Authentication should occur before protected application services execute.

Authentication integration must provide:

* User identity
* account status
* session status
* MFA or step-up status
* session revocation state
* relevant authentication metadata

The exact authentication provider will be selected in a later ADR.

## 14. Organization Resolution

For organization-owned workflows, the application must resolve:

* Requested organization
* authenticated user
* active membership
* membership status
* requested resource
* resource organization
* applicable assignment
* support-session context where applicable

Organization context must not be inferred solely from:

* URL shape
* browser state
* client-submitted organization ID
* cached navigation state
* role label

## 15. Authorization Enforcement

Authorization must execute server side before:

* Reading protected data
* submitting a protected command
* generating a signed document URL
* downloading a restricted document
* invoking an AI use case
* exporting data
* changing organization configuration
* performing finance operations
* approving agreements
* activating occupancy
* entering a support session

## 16. Authorization in Server Components

Server Components may call permission-aware query services.

They must not:

* Query unrestricted repositories and filter afterward in the UI.
* reveal hidden records in serialized props.
* treat a missing button as authorization.
* trust a role name without evaluating resource scope.

## 17. Authorization in Server Actions

Every Server Action must independently:

1. Parse input.
2. authenticate the actor.
3. resolve organization context.
4. authorize the requested action.
5. validate business prerequisites.
6. invoke an application service.
7. record required audit events.
8. return a safe result.

A Server Action must not assume authorization occurred because the action originated from an authorized page.

## 18. Authorization in Route Handlers

Every protected Route Handler must independently:

* Authenticate the caller
* determine caller type
* validate organization scope
* evaluate permission or integration authority
* validate request schema
* invoke approved application services
* return a safe response

Webhook authentication follows provider-signature rules rather than user-session authentication.

## 19. Application-Service Boundary

Application services are the primary orchestration boundary for Habitacores workflows.

A protected state transition should normally follow:

```text
Framework Entry Point
→ Application Service
→ Domain Validation
→ Repository Transaction
→ Audit
→ Outbox
→ Result
```

Application services may coordinate:

* Authentication context
* authorization
* repositories
* domain objects
* transactions
* audit
* outbox events
* integration ports
* return models

## 20. Application Service Responsibilities

An application service may:

* Load required records
* confirm organization ownership
* call authorization policies
* invoke domain rules
* manage transactions
* persist changes
* append audit records
* append outbox events
* construct safe results

It must not embed provider-specific response handling unless operating through an integration abstraction.

## 21. Domain Responsibilities

The domain layer remains responsible for deterministic rules such as:

* Valid state transitions
* payment verification eligibility
* allocation limits
* agreement version rules
* signature prerequisites
* occupancy activation prerequisites
* conflicting occupancy prevention
* maintenance workflow transitions
* unknown-value integrity

## 22. Read Architecture

Read operations should use permission-aware query services.

A query service may return a view-specific read model rather than a complete domain object.

Examples:

* Listing details
* application review summary
* payment verification queue item
* resident financial summary
* maintenance dashboard card
* agreement status view

## 23. Write Architecture

Protected writes must use explicit commands.

Illustrative commands:

```text
SubmitApplication
RequestApplicationInformation
RecordApplicationDecision
CreateCharge
SubmitPaymentEvidence
VerifyPayment
AllocatePayment
IssueAgreement
RecordSignatureCompletion
ActivateAgreement
ActivateOccupancy
CreateMaintenanceRequest
AssignWorkOrder
```

Commands should express business intent rather than database operations.

## 24. Command Result

A command result should communicate:

* Success or failure
* stable result code
* resource identifier
* new authoritative state
* safe validation errors
* conflict status
* correlation identifier
* relevant next action

## 25. Server Actions

Server Actions are approved for first-party UI mutations where:

* The caller is the Habitacores web application.
* The mutation originates from a controlled form or interface.
* no reusable public HTTP contract is needed.
* the operation remains behind an application service.
* input size is appropriate.
* authorization is performed inside the action.
* failure can be represented in the UI safely.

Suitable examples:

* Accept invitation
* update profile
* submit inquiry from a controlled form
* save application draft
* submit application
* assign reviewer
* request additional information
* record decision
* update notification preferences
* create a maintenance request

## 26. Server Action Wrapper

Habitacores should establish a standard Server Action wrapper.

Conceptual flow:

```text
Parse
→ Request Context
→ Authenticate
→ Authorize
→ Validate
→ Execute Application Service
→ Audit or Outbox Within Service
→ Revalidate Approved Cache
→ Return Typed Result
```

The wrapper must not obscure which permission is required.

## 27. Server Action Security

Server Actions must be treated as remotely callable server endpoints.

Controls must include:

* Input validation
* authentication
* authorization
* rate limiting where needed
* origin protection
* CSRF-aware configuration
* request-size controls
* safe error handling
* idempotency for material commands
* audit for protected actions

## 28. Server Action Restrictions

Server Actions should not be the primary interface for:

* Payment-provider webhooks
* signature-provider callbacks
* public partner APIs
* mobile APIs
* large file uploads
* health checks
* OAuth callbacks requiring protocol-level handling
* provider reconciliation endpoints
* bulk imports
* streaming downloads
* external machine clients

## 29. Route Handlers

Route Handlers are approved for explicit HTTP interfaces.

They may support:

* Webhooks
* OAuth callbacks
* payment-provider callbacks
* signature-provider callbacks
* upload-intent endpoints
* upload-finalization endpoints
* health checks
* readiness checks
* secure downloads
* public APIs
* partner APIs
* mobile-facing APIs
* external integrations
* import and export initiation
* machine-readable status
* AI streaming where specifically approved

## 30. Route Handler Structure

Recommended conceptual structure:

```text
Route Handler
→ HTTP and Protocol Validation
→ Authentication or Signature Verification
→ Schema Validation
→ Request Context
→ Application Service
→ HTTP Response Mapping
```

Route Handlers should remain thin.

## 31. Route Handler Versioning

Reusable or external HTTP interfaces should use explicit versioning where required.

Example:

```text
/api/v1/...
/api/partners/v1/...
/api/webhooks/payments/...
```

Internal first-party Route Handlers do not automatically become public APIs.

## 32. Server Actions Versus Route Handlers

Use a Server Action when:

* The web UI owns the interaction.
* form integration is beneficial.
* a public HTTP contract is unnecessary.
* the action can remain first-party.

Use a Route Handler when:

* HTTP semantics matter.
* a provider or machine calls the endpoint.
* a future mobile or partner client may reuse it.
* payload or response control is required.
* webhook signature verification is required.
* streaming or file behavior is required.

Both must call the same application services for equivalent protected behavior.

## 33. No Direct Route-to-Database Writes

A Route Handler or Server Action must not directly modify authoritative tables for material workflows.

Prohibited pattern:

```text
Route Handler
→ Raw database update
→ Success
```

Approved pattern:

```text
Route Handler
→ Application Service
→ Domain Rules
→ Transaction
→ Audit and Outbox
→ Success
```

## 34. Runtime Decision

The Node.js runtime is the default for Habitacores application routes.

Reasons include:

* PostgreSQL access
* database drivers
* cryptography
* provider SDKs
* observability SDKs
* file-processing support
* complex authentication
* AI provider integration
* mature npm compatibility
* transactional application services

## 35. Edge Runtime

The Edge runtime is not the default.

It may be approved for a narrowly scoped route when:

* No database transaction is required.
* no incompatible Node.js package is required.
* no restricted provider credential is used improperly.
* latency benefit is measurable.
* observability remains sufficient.
* security controls remain equivalent.
* the route has a documented fallback.
* a technical review approves it.

Potential future examples:

* Lightweight public redirects
* geographically distributed public metadata
* narrowly scoped request filtering
* simple public personalization

## 36. Runtime Declaration

Runtime selection should be explicit for sensitive or provider-dependent routes.

Example:

```typescript
export const runtime = "nodejs";
```

The architecture must not depend on accidental platform defaults for critical workflows.

## 37. Middleware or Proxy Boundary

Request middleware or framework proxy functionality may support:

* Coarse route protection
* correlation propagation
* locale handling
* safe redirects
* basic request normalization
* public versus authenticated routing

It must not become the authoritative location for:

* Resource authorization
* payment decisions
* agreement decisions
* occupancy decisions
* support-session authorization
* database-dependent policy evaluation

Full authorization must occur at the application-service boundary.

## 38. Caching Principle

Caching must never weaken:

* Organization isolation
* authorization
* current financial state
* current agreement state
* occupancy integrity
* support-session expiry
* account suspension
* document access restrictions

## 39. Public Caching

Approved public data may be cached when:

* It contains only public fields.
* publication status is validated.
* organization-specific private data is excluded.
* invalidation behavior is defined.
* stale content risk is acceptable.
* unpublication can invalidate access promptly.

Potential examples:

* Public marketing pages
* approved listing summaries
* approved amenity descriptions
* public help pages

## 40. Protected Data Caching

Protected data should default to dynamic or explicitly scoped retrieval.

Shared persistent caching must not be used casually for:

* Applications
* payment evidence
* payment verification queues
* charges and balances
* agreements
* occupancies
* legal matters
* support sessions
* security events
* restricted documents
* AI retrieval packages

## 41. Tenant-Safe Cache Key

Any approved cache containing organization-specific information must include sufficient context such as:

* Environment
* organization ID
* resource ID
* user or permission scope where required
* record version
* policy version
* language where relevant

A URL alone may be insufficient as a secure cache key.

## 42. User-Specific Caching

User-specific or support-session-specific results should not enter a shared cache unless:

* Isolation is technically guaranteed.
* expiry behavior is correct.
* membership revocation is handled.
* support-session expiry is handled.
* cache invalidation is tested.

The default position is not to persistently cache sensitive user-specific views.

## 43. Revalidation

After a successful mutation, the application may revalidate affected:

* Paths
* cache tags
* listing pages
* dashboards
* queues
* resource views

Revalidation occurs only after the authoritative transaction succeeds.

## 44. Cache Tags

Where framework caching is used, cache tags should represent stable resource or query categories.

Illustrative tags:

```text
public-listing:{listingId}
public-property:{propertyId}
organization-units:{organizationId}
application:{applicationId}
```

Sensitive tags do not independently make cached content secure.

## 45. Cache Invalidation and Transactions

Cache invalidation must not occur before a transaction is committed.

Preferred order:

```text
Validate
→ Authorize
→ Commit Authoritative State
→ Persist Audit and Outbox
→ Invalidate or Update Approved Cache
→ Return Result
```

## 46. Static Generation

Static generation may be used for:

* Product information
* public legal pages
* documentation
* approved marketing content
* public content with controlled revalidation

Authenticated operational pages should not be statically generated with tenant data.

## 47. Incremental Static Regeneration

Incremental regeneration may be used for public listing and content pages when:

* Staleness tolerance is documented.
* publication changes trigger invalidation.
* unpublishing is handled safely.
* no restricted data enters the generated output.
* fallback behavior is defined.

## 48. Data Fetching

Server Components may obtain data through:

* Permission-aware application query services
* approved provider-neutral fetch services
* framework-supported server data access

They should not create uncontrolled duplicated data-access patterns.

## 49. Database Calls From Server Components

Direct ORM access from arbitrary Server Components is discouraged.

Preferred:

```text
Server Component
→ Query Service
→ Repository
→ Database
```

This maintains:

* Tenant checks
* permission checks
* reusable read models
* observability
* consistent error handling
* testability

## 50. React Request Deduplication

Request-scoped deduplication may be used for repeated safe reads during one render.

It must not be confused with:

* Authorization
* durable caching
* cross-request caching
* transaction isolation
* distributed cache consistency

## 51. Suspense and Streaming

Suspense boundaries may isolate slower secondary content.

Examples:

* Operational metrics
* non-critical provider status
* report summaries
* AI-assisted summaries
* secondary activity feeds

The core authoritative state should render clearly without depending on an AI or optional provider response.

## 52. Loading States

Loading states must distinguish:

* Loading
* pending domain action
* pending provider result
* pending verification
* successful completion
* failed completion

A skeleton or spinner must not imply a payment, signature, or occupancy action has succeeded.

## 53. Error Boundaries

The application should use:

* Route-level error boundaries
* global error boundary
* component-level recovery where appropriate
* typed domain-error handling
* correlation identifiers
* safe retry guidance

Error boundaries do not replace application-service error handling.

## 54. Not-Found Behavior

Not-found behavior must avoid revealing whether a protected cross-tenant record exists.

For protected resources, the application may use a consistent response that avoids tenant-resource enumeration.

## 55. Error Classification

Errors should be classified as:

* Validation
* authentication
* authorization
* not found
* conflict
* stale version
* business-rule violation
* provider unavailable
* provider rejection
* rate limited
* internal
* security

## 56. Safe Error Results

User-facing errors should provide:

* Safe message
* stable code
* field errors where appropriate
* suggested next step
* correlation identifier where useful

They must not expose:

* SQL
* stack traces
* secrets
* provider credentials
* private storage keys
* another organization’s identifiers
* unrestricted provider payloads

## 57. Optimistic User Interfaces

Optimistic UI may be used only when temporary presentation can be reversed safely.

Suitable examples:

* Mark notification as read
* local sorting
* low-impact preference changes
* non-authoritative draft presentation

Optimistic UI should not portray final success for:

* Payment verification
* payment allocation
* refund execution
* application decisions
* agreement signatures
* agreement activation
* occupancy activation
* legal termination
* restricted-document deletion

## 58. Idempotency

Material mutations must support idempotency where repeated submission could cause duplication.

Examples:

* Application submission
* payment initiation
* payment verification
* allocation
* receipt generation
* refund request
* agreement issue
* signature callback
* occupancy activation
* import
* export
* notification send

## 59. Concurrency

The application must detect or prevent stale concurrent writes for:

* Payment allocation
* charge adjustment
* agreement issue
* agreement activation
* occupancy activation
* unit reservation
* role assignment
* support-session status
* workflow decision

The exact database concurrency strategy will be finalized in later ADRs.

## 60. Forms

Forms should support:

* Server-side validation
* accessible labels
* field-level errors
* error summary
* draft preservation
* submission status
* duplicate prevention
* unsaved-change handling
* mobile usability
* low-bandwidth resilience

Client validation may improve usability but cannot replace server validation.

## 61. Form Result Model

A typed form result may include:

```typescript
interface ActionResult<T> {
  ok: boolean;
  code: string;
  data?: T;
  fieldErrors?: Record<string, string[]>;
  message?: string;
  correlationId: string;
}
```

The exact type will be finalized during implementation.

## 62. Draft Workflows

Long forms such as rental applications should support explicit draft persistence.

Draft persistence should:

* Be authorized
* preserve organization and user ownership
* support versions
* indicate save status
* tolerate interrupted connections
* avoid duplicate submission
* expire under approved policy

## 63. File Upload Architecture

Large or restricted files should not normally pass through a Server Action body.

Preferred flow:

```text
Client Requests Upload Intent
→ Route Handler Authenticates and Authorizes
→ Server Issues Short-Lived Upload Authorization
→ Client Uploads Directly to Private Storage
→ Client Requests Finalization
→ Server Validates Metadata
→ File Enters Scan-Pending State
→ Worker Coordinates Malware Scan
→ Document Becomes Available Only After Approved Result
```

## 64. Upload Intent

Upload intent must validate:

* User
* organization
* permission
* resource relationship
* document purpose
* allowed content type
* maximum size
* storage environment
* expiry

## 65. Upload Finalization

Finalization must validate:

* Expected object exists
* expected organization path
* expected resource
* file metadata
* content type
* size
* checksum where used
* intent not expired
* intent not reused improperly

## 66. File Availability

A file must remain unavailable or restricted while:

* Upload is incomplete
* finalization is incomplete
* malware scan is pending
* scan failed
* scan timed out
* document relationship is invalid
* authorization is unresolved

## 67. Downloads

Restricted downloads should use:

* Server-side authorization
* short-lived signed access
* safe content disposition
* approved cache headers
* access audit where required

Permanent public storage URLs are not approved for restricted files.

## 68. Background Work

Durable asynchronous work must be sent to the deterministic worker.

Examples:

* Email
* provider event processing
* scan coordination
* reports
* reconciliation
* scheduled reminders
* large imports
* exports
* document generation

## 69. Background Handoff

Preferred flow:

```text
Domain Transaction
→ Outbox Event
→ Transaction Commit
→ Worker Claims Event
→ Idempotent Handler
→ Side Effect
→ Result and Retry State
```

## 70. Request-After Work

Framework functionality that executes after the response may be used only for non-critical best-effort activity such as:

* Additional logging
* low-risk analytics
* non-authoritative telemetry

It must not replace durable jobs for:

* Financial effects
* notification obligations
* agreement processing
* signature processing
* occupancy activation
* reconciliation
* document scanning
* AI work that must be tracked

## 71. Webhook Processing

Webhook Route Handlers should:

1. Enforce payload-size limits.
2. read the raw body where required.
3. verify provider signature.
4. verify timestamp and replay window.
5. validate provider account and environment.
6. persist the event.
7. acknowledge receipt quickly.
8. queue asynchronous processing.
9. apply idempotency.
10. invoke application services.
11. audit material state changes.

## 72. External API Architecture

Future external APIs should reuse application services rather than duplicate business rules.

Potential clients include:

* Native mobile applications
* partner systems
* approved property operators
* accounting systems
* support integrations

A separate partner or mobile API may be introduced later without changing domain ownership.

## 73. Internal API Policy

The web application should not create HTTP calls to itself merely to reach server-side business logic.

Preferred:

```text
Server Component
→ Query Service
```

rather than:

```text
Server Component
→ Internal HTTP Route
→ Query Service
```

HTTP should be used when a true protocol boundary exists.

## 74. Responsible AI Invocation

Approved AI use cases must follow:

```text
UI
→ Server Action or Route Handler
→ Application Service
→ Authorization
→ Use-Case Registry
→ Retrieval Service
→ Data Minimization
→ AI Gateway
→ AI Worker or Provider Adapter
→ Output Validation
→ Human Review
```

## 75. Synchronous AI Invocation

Synchronous AI may be used when:

* The result is optional.
* latency is acceptable.
* timeout is controlled.
* output remains advisory.
* manual workflow remains available.
* no material transaction waits for AI authority.

## 76. Asynchronous AI Invocation

The AI worker should be used when:

* Document processing is long running.
* evaluation is required.
* provider latency is variable.
* retries are required.
* cost or usage must be queued.
* multiple source documents are processed.
* human review occurs later.
* the UI should not wait for completion.

## 77. AI Streaming

AI output streaming may be introduced for approved drafting or assistance experiences.

Streaming must not:

* Bypass complete-output validation for material use.
* expose another tenant’s content.
* write partial output into authoritative records automatically.
* imply that an unfinished answer is approved.
* execute tools or commands without governed authorization.

## 78. AI Result Persistence

AI results should be stored only when required.

Persisted records should include:

* Use-case ID
* prompt version
* model configuration
* source references
* output schema version
* generated time
* validation status
* review status
* reviewer
* accepted, edited, or rejected outcome
* retention classification

## 79. AI Human Review

Material AI-generated content must have an explicit review state such as:

* Generated
* awaiting review
* accepted
* accepted with edits
* rejected
* expired
* superseded

AI completion is not domain approval.

## 80. AI Failure Handling

When AI fails:

* The deterministic workflow remains usable.
* no authoritative status changes.
* a safe message is presented.
* retry status is visible.
* operational telemetry is recorded.
* raw provider errors are not exposed.
* manual processing remains available.

## 81. Audit Integration

Material application services must create audit records for actions such as:

* Role assignment
* application submission
* application decision
* payment verification
* payment allocation
* refund approval
* agreement issue
* signature completion
* agreement activation
* occupancy activation
* restricted-document access
* support-session use
* AI-assisted output acceptance

## 82. Outbox Integration

Where a transaction creates a required asynchronous effect, the outbox record should be persisted in the same database transaction.

Examples:

* Application submitted → acknowledgment notification
* payment verified → finance notification
* agreement issued → signature request
* occupancy activated → resident notification
* maintenance assigned → vendor notification

## 83. Observability

Framework entry points should propagate:

* Correlation ID
* trace context
* environment
* application version
* route or action identifier
* organization ID where safe
* actor ID where appropriate
* operation
* duration
* result code

## 84. Server Action Observability

Server Actions should record safe operational metadata such as:

* Action name
* correlation ID
* duration
* outcome
* validation failure
* authorization denial
* conflict
* application-service result

Sensitive form data must not be copied into ordinary logs.

## 85. Route Handler Observability

Route Handlers should additionally record:

* HTTP method
* response status
* caller category
* provider category
* webhook-event ID where applicable
* idempotency outcome
* rate-limit outcome
* payload-size category

## 86. Health Routes

The application should provide distinct health endpoints for:

* Liveness
* readiness
* deployment verification

Liveness should remain inexpensive.

Readiness may evaluate required dependencies and migration compatibility.

Health responses must not reveal secrets or internal topology unnecessarily.

## 87. Security Headers

Next.js configuration and application responses must support approved security headers, including:

* Strict Transport Security
* Content Security Policy
* frame restrictions
* content-type protection
* referrer policy
* permissions policy
* appropriate cache controls

Exact policy values will be finalized during security implementation.

## 88. Content Security Policy

The Content Security Policy must account for approved:

* Application assets
* image sources
* payment providers
* signature providers
* analytics
* maps
* AI interfaces where client connections are explicitly approved

Broad wildcard sources should be avoided.

## 89. Rate Limiting

Rate limiting should be applied at suitable infrastructure or application boundaries.

Priority targets include:

* Registration
* login
* password recovery
* verification resend
* public inquiry
* inspection request
* application submission
* upload intent
* webhooks
* exports
* expensive reports
* AI requests

## 90. Accessibility

Next.js architecture must support:

* Server-rendered semantic HTML
* keyboard navigation
* predictable focus
* loading announcements
* error summaries
* accessible forms
* accessible dialogs
* reduced client-side disruption
* progressive enhancement where practical

## 91. Mobile and Low-Bandwidth Design

The architecture should reduce unnecessary client JavaScript.

Critical workflows should:

* Render meaningful HTML from the server.
* load interactive islands selectively.
* paginate large records.
* optimize media.
* preserve drafts.
* show upload state.
* recover from interrupted requests.
* avoid repeated full-page data transfers.
* avoid requiring AI for completion.

## 92. SEO Boundary

SEO is relevant primarily for:

* Public listings
* public property pages
* marketing pages
* public help content

Authenticated portals should not be indexed.

## 93. Metadata

Public route metadata may include:

* Approved title
* description
* canonical URL
* social preview
* listing image
* public location

Metadata must not expose:

* Applicant names
* resident names
* private unit details
* financial status
* internal notes
* restricted documents

## 94. Deployment Boundary

The Next.js web application is one deployable application within the monorepo.

It should not host durable workers inside normal request execution.

Approved deployment units are:

```text
apps/web
apps/worker
apps/ai-worker when required
```

They may initially share:

* Repository
* database
* contracts
* domain packages
* deployment platform
* monitoring

## 95. Horizontal Scaling

The web application should remain stateless between requests except through approved shared services.

It must not depend on:

* Process-local sessions
* process-local job queues
* local uploaded files
* in-memory financial locks
* in-memory workflow authority
* one specific server instance

## 96. Local File System

The runtime local file system must not be treated as durable storage.

Files must use approved object storage.

Temporary files may be used only when:

* Environment supports it.
* lifecycle is controlled.
* no durability is assumed.
* sensitive data is cleaned safely.
* memory and storage limits are respected.

## 97. Deployment Promotion

The application should be built and validated through controlled environments.

Preferred flow:

```text
Pull Request
→ Automated Checks
→ Preview
→ Merge
→ Staging
→ Migration Validation
→ Smoke Tests
→ Production Approval
→ Production
→ Health and Smoke Validation
```

## 98. Framework Upgrade Policy

Next.js and React upgrades must be treated as controlled technical changes.

Upgrade review should include:

* Release notes
* security fixes
* caching changes
* Server Action behavior
* runtime changes
* deployment compatibility
* provider compatibility
* test results
* performance impact
* rollback or forward-recovery plan

## 99. Experimental Features

Experimental framework features must not become critical production dependencies without:

* Documented benefit
* compatibility review
* fallback
* test coverage
* operational review
* Chief Product Architect approval where material

## 100. Prohibited Architecture Patterns

The following are not approved:

* Business rules embedded only in React components
* authorization enforced only by navigation
* direct database writes from arbitrary route code
* direct provider calls from Client Components
* direct AI-provider calls from product modules
* unrestricted ORM queries in UI components
* shared caching of unscoped tenant data
* durable work implemented as best-effort post-response callbacks
* uploaded restricted files stored publicly
* financial success inferred from UI state
* agreement activation inferred from signature UI alone
* occupancy activation without deterministic application service
* Edge runtime used by default for critical workflows
* one HTTP endpoint calling another endpoint in the same application without a real protocol boundary

## 101. Options Considered

### Option A — Pages Router

Advantages:

* Mature historical ecosystem
* familiar patterns
* conventional API routes

Disadvantages:

* Does not align with the selected modern application model.
* weaker integration with Server Components.
* would require maintaining older and newer routing models.
* offers less architectural benefit for this new application.

Decision:

Rejected.

### Option B — App Router With Framework-Centric Business Logic

Advantages:

* Rapid initial implementation
* fewer explicit layers
* small prototypes appear simple

Disadvantages:

* Business rules become mixed with routes and components.
* authorization becomes inconsistent.
* testing becomes framework dependent.
* workers duplicate logic.
* future clients become difficult.
* AI and providers become scattered.

Decision:

Rejected.

### Option C — App Router With Application-Service and Domain Boundaries

Advantages:

* Modern server rendering
* selective client JavaScript
* explicit business boundaries
* reusable application services
* secure Route Handlers
* controlled Server Actions
* worker reuse
* future client support
* better testing
* Responsible AI governance

Disadvantages:

* More structure
* requires dependency discipline
* may appear slower than direct database access initially
* requires standard wrappers and conventions

Decision:

Approved.

### Option D — Separate Backend API From Initial Launch

Advantages:

* Strong network boundary
* independent frontend and backend
* immediate support for multiple clients

Disadvantages:

* Additional deployment
* duplicated contracts
* additional authentication complexity
* more network failure modes
* slower MVP delivery
* premature separation

Decision:

Rejected for the MVP.

A separate external API may be added when a real client or partner requires it.

## 102. Positive Consequences

* Protected logic stays server side.
* Mobile users receive less unnecessary JavaScript.
* application services can be reused by workers and future APIs.
* first-party forms can use Server Actions effectively.
* providers can use stable Route Handlers.
* tenant isolation remains centralized.
* caching becomes explicit.
* Responsible AI calls remain governed.
* public pages can still use modern rendering and caching.
* the modular monolith remains deployable as a practical MVP.

## 103. Negative Consequences

* Developers must understand Server and Client Component boundaries.
* Server Actions and Route Handlers require separate conventions.
* caching requires disciplined review.
* framework upgrades require careful testing.
* application services add structure.
* direct ORM usage from arbitrary pages is restricted.
* some simple mutations may require more files than a prototype.
* runtime differences must be understood.

## 104. Risks

### 104.1 Server Action Becomes an Authorization Shortcut

Control:

* Every action performs authentication and authorization.
* standard action wrapper
* action-level tests
* no trust in page origin

### 104.2 Tenant Data Enters Shared Cache

Control:

* Protected data dynamic by default
* organization-scoped cache keys
* cache-review checklist
* cross-tenant cache tests
* explicit invalidation

### 104.3 Client Bundle Leaks Server Code

Control:

* Narrow Client Components
* server-only markers
* lint and build checks
* secret scanning
* bundle review

### 104.4 Durable Work Is Lost

Control:

* Outbox and worker
* no critical post-response callbacks
* job monitoring
* retries and dead-letter handling

### 104.5 Framework Logic Replaces Domain Logic

Control:

* Application-service boundary
* domain commands
* architecture tests
* code review
* Codex guardrails

### 104.6 AI Blocks Core Workflows

Control:

* Optional AI presentation
* timeouts
* asynchronous processing
* manual workflow
* independent health status

### 104.7 Edge Incompatibility

Control:

* Node.js default
* explicit runtime declarations
* Edge exception review
* compatibility tests

## 105. Acceptance Criteria — Server Component

```text
Given:
An authorized organization user opens a payment-verification page.

When:
The page retrieves payment information.

Then:
The Server Component invokes a permission-aware query service.

And:
The page does not query unrestricted payment records and filter them in the browser.
```

## 106. Acceptance Criteria — Server Action

```text
Given:
A finance reviewer submits a payment-verification decision.

When:
The Server Action receives the request.

Then:
It authenticates the user.

And:
It resolves the organization and membership.

And:
It verifies the required permission and separation-of-duty rules.

And:
It invokes the payment-verification application service.

And:
It does not update the payment table directly.
```

## 107. Acceptance Criteria — Route Handler

```text
Given:
A payment provider sends a transaction callback.

When:
The Route Handler receives the callback.

Then:
It validates the raw payload, signature, timestamp, provider account, and environment.

And:
It persists the provider event.

And:
It acknowledges receipt quickly.

And:
It queues idempotent processing.

And:
The callback does not directly allocate payment or activate an agreement.
```

## 108. Acceptance Criteria — Cache Isolation

```text
Given:
Organization A and Organization B each have payment records.

When:
Organization A requests a cached or deduplicated operational view.

Then:
No Organization B record, count, identifier, balance, or metadata appears.

And:
The cache or query key preserves the required tenant scope.
```

## 109. Acceptance Criteria — File Upload

```text
Given:
An applicant uploads payment evidence.

When:
The upload completes.

Then:
The file remains private.

And:
The document remains scan pending.

And:
The payment remains unverified.

And:
The charge remains outstanding.

And:
No agreement or occupancy activates.
```

## 110. Acceptance Criteria — AI Invocation

```text
Given:
An authorized reviewer requests an application summary.

When:
The request executes.

Then:
The application service validates the reviewer, organization, resource, permission, and approved AI use case.

And:
Only approved minimized fields are retrieved.

And:
The request passes through the AI Gateway.

And:
The output remains advisory and reviewable.
```

## 111. Acceptance Criteria — AI Failure

```text
Given:
The AI worker or provider is unavailable.

When:
A reviewer opens the application workflow.

Then:
The authoritative application remains accessible.

And:
Manual review remains available.

And:
No application status changes.

And:
The AI panel indicates that assistance is unavailable.
```

## 112. Acceptance Criteria — Background Work

```text
Given:
An agreement is issued successfully.

When:
The issuing transaction commits.

Then:
The agreement version, audit record, and signature-request outbox event are persisted consistently.

And:
A failed signature-provider request does not reverse or corrupt the issued agreement.
```

## 113. Acceptance Criteria — Occupancy Activation

```text
Given:
A user submits an occupancy-activation form.

When:
The Server Action executes.

Then:
It invokes the occupancy application service.

And:
The service validates agreement, payment, unit readiness, conflict, actor authority, and current state.

And:
The UI cannot bypass a failed prerequisite.
```

## 114. Acceptance Criteria — Unknown Pilot Information

```text
Given:
Block B inventory remains unconfirmed.

When:
A Server Component, Route Handler, Server Action, cache, report, or AI use case processes the property hierarchy.

Then:
The inventory remains null or pending.

And:
No framework default converts it to zero.

And:
No placeholder unit or floor is generated.
```

## 115. Implementation Guidance

Recommended implementation order:

```text
1. Establish App Router root and route groups
2. establish server-only module rules
3. establish request context
4. establish typed error model
5. establish application-service convention
6. establish query-service convention
7. establish Server Action wrapper
8. establish Route Handler wrapper
9. establish organization-resolution boundary
10. establish authorization integration
11. establish correlation and observability
12. establish protected caching policy
13. establish upload-intent pattern
14. establish outbox and worker handoff
15. add AI invocation only with an approved use case
```

## 116. Deferred Decisions

This ADR does not select:

* Exact Next.js version
* exact React version
* package manager
* monorepo orchestration tool
* authentication provider
* session library
* ORM or query builder
* database provider
* row-level-security strategy
* queue provider
* object-storage provider
* file-scanning provider
* observability vendor
* hosting provider
* payment provider
* signature provider
* AI provider
* feature-flag provider
* CSS or component implementation
* exact caching feature configuration
* exact Server Action wrapper library
* exact API schema generator
* exact rate-limiting provider

These require later ADRs or implementation decisions.

## 117. Supersession Conditions

This ADR should be reviewed when:

* Next.js materially changes the App Router execution model.
* Server Actions become unsuitable for required security or deployment needs.
* a native mobile application requires a comprehensive external API.
* a partner API becomes a primary product capability.
* web and backend release cycles require independent deployment.
* application-service boundaries prove inadequate.
* caching behavior cannot satisfy tenant-safety requirements.
* a regulated deployment requires another runtime model.
* the modular-monolith deployment model is superseded.

## 118. Final Decision Statement

Habitacores will use the Next.js App Router with Server Components as the default rendering and data-composition model.

Client Components will be introduced only for required browser interactivity.

First-party user-interface mutations may use Server Actions, but every action must authenticate, resolve organization context, authorize, validate, and invoke an application service.

Route Handlers will be used for webhooks, provider callbacks, upload workflows, OAuth callbacks, health endpoints, external APIs, partner APIs, mobile-facing interfaces, and other explicit HTTP boundaries.

The Node.js runtime will be the default. Edge execution requires a narrowly scoped, evidence-based exception.

Authoritative business behavior will be implemented through application services and deterministic domain rules rather than React components, Server Actions, Route Handlers, middleware, provider callbacks, or AI output.

Protected data will remain dynamic or explicitly tenant scoped. Shared caching must never weaken organization isolation, authorization, financial integrity, agreement integrity, occupancy integrity, document security, membership revocation, or support-session expiry.

Durable asynchronous effects will use transactional outbox records and workers rather than best-effort request-lifecycle callbacks.

Responsible AI requests will pass through approved application services, authorization, data minimization, the AI Gateway, output validation, and human review.

No Server Component, Client Component, Server Action, Route Handler, cache entry, middleware rule, provider callback, AI response, optimistic interface, successful HTTP response, or framework rendering state may independently authorize, verify, allocate, sign, activate, terminate, reveal, refund, reverse, or otherwise finalize a protected Habitacores business action.