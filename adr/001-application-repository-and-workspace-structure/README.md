# ADR-001 — Application Repository and Workspace Structure

## Decision Record

| Field | Value |
|---|---|
| Product | Habitacores |
| Decision | Application Repository and Workspace Structure |
| ADR | ADR-001 |
| Version | 0.1 |
| Status | Proposed |
| Decision Type | Foundational Architecture |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Architecture | Modular monolith |
| Repository Model | TypeScript monorepo |
| Primary Application | Next.js responsive web application |
| Primary Language | TypeScript |
| Primary System of Record | PostgreSQL |
| Responsible AI Model | Centralized AI control plane with isolated runtime capability |
| Initial Deployment Model | Small number of coordinated deployable processes |
| Last Updated | 30 July 2026 |

## 1. Context

Habitacores is being designed as a multi-tenant real-estate operating ecosystem for the Nigerian market, with future expansion into broader SaaS, marketplace, financial, professional-service, analytics, and Responsible AI capabilities.

The approved Product Knowledge Base defines significant product and architectural requirements, including:

- Organization-level tenant isolation
- role-based and assignment-based authorization
- property and inventory management
- public listings
- inquiries and inspections
- applications and screening
- charges and financial records
- payment verification and allocation
- agreement versioning and signatures
- occupancy management
- resident operations
- maintenance and vendor workflows
- document protection
- communications
- support operations
- external integrations
- observability
- Responsible AI controls
- future multi-organization expansion

The application repository must support rapid MVP delivery without creating structural limitations that would later undermine:

- Domain integrity
- tenant isolation
- provider replaceability
- AI governance
- testing
- operational support
- future worker processes
- future applications
- controlled service extraction

The repository structure must also support implementation through engineers and AI coding agents such as Codex without allowing architectural boundaries to become ambiguous.

## 2. Decision Drivers

The decision is driven by the following requirements:

1. Deliver the MVP without premature distributed-system complexity.
2. Preserve explicit domain boundaries.
3. Support a responsive web application.
4. Support deterministic background processing.
5. Support isolated AI-dependent processing.
6. Centralize reusable infrastructure and contracts.
7. Enforce server-side authorization consistently.
8. prevent provider SDKs from entering core domain logic.
9. support comprehensive automated testing.
10. support future applications and runtime processes.
11. provide clear work-package boundaries for Codex.
12. preserve organization-level tenant isolation.
13. support a Responsible AI gateway and governance layer.
14. permit later service extraction where operational evidence justifies it.
15. avoid maintaining multiple repositories prematurely.
16. support controlled infrastructure and deployment configuration.
17. keep product architecture documentation separate from application implementation.
18. maintain practical developer experience for a small initial team.

## 3. Decision

Habitacores will use a **TypeScript monorepo built around a modular-monolith architecture**.

The repository will contain:

- A primary Next.js web application
- a deterministic background worker
- an independently bounded AI worker
- shared internal packages
- Responsible AI infrastructure
- infrastructure definitions
- engineering tooling
- implementation documentation
- scripts
- automated tests

The MVP will remain a modular monolith from a business-domain perspective.

The existence of multiple runtime processes does not make the system a microservices architecture.

The primary business domains will remain part of one coordinated application architecture and one authoritative transactional system of record.

## 4. Repository Name

The recommended production application repository name is:

```text
habitacores
````

The repository should be created under the GitHub organization:

```text
HabitaCores
```

Expected repository path:

```text
HabitaCores/habitacores
```

The repository should initially be private.

The existing repository:

```text
HabitaCores/habitacores-product-knowledge-base
```

will remain the canonical product and architecture source of truth.

## 5. Approved Top-Level Structure

The approved initial structure is:

```text
habitacores/
├── apps/
│   ├── web/
│   ├── worker/
│   └── ai-worker/
│
├── packages/
│   ├── authorization/
│   ├── audit/
│   ├── config/
│   ├── contracts/
│   ├── database/
│   ├── domain/
│   ├── events/
│   ├── integrations/
│   ├── jobs/
│   ├── observability/
│   ├── security/
│   ├── testing/
│   └── ui/
│
├── ai/
│   ├── evaluations/
│   ├── gateway/
│   ├── model-registry/
│   ├── policies/
│   ├── prompts/
│   ├── redaction/
│   ├── retrieval/
│   ├── safety/
│   └── test-fixtures/
│
├── infrastructure/
│   ├── environments/
│   ├── modules/
│   ├── monitoring/
│   └── policies/
│
├── docs/
│   ├── architecture/
│   ├── development/
│   ├── operations/
│   ├── responsible-ai/
│   └── security/
│
├── scripts/
├── tooling/
├── adr/
├── package.json
├── tsconfig.json
└── README.md
```

Directories may be introduced incrementally.

Empty speculative packages should not be created merely to match this diagram.

A directory should be added when an approved implementation requirement needs it.

## 6. Application Runtime Structure

### 6.1 Web Application

Path:

```text
apps/web
```

Responsibilities:

* Public web experience
* applicant portal
* resident portal
* organization portal
* platform administration interface
* server-rendered pages
* authenticated application endpoints
* request validation
* authentication integration
* organization-context resolution
* authorization enforcement
* application-service invocation
* response rendering
* secure file-access initiation
* user-facing error handling

The web application must not contain material business rules solely inside:

* React components
* page components
* route handlers
* browser-side validation
* server actions
* middleware

Protected business behavior must be delegated to approved application or domain services.

### 6.2 Deterministic Worker

Path:

```text
apps/worker
```

Responsibilities may include:

* Email delivery
* notification processing
* provider webhook processing
* file-processing coordination
* malware-scan coordination
* scheduled workflow checks
* report generation
* reconciliation
* import and export processing
* retryable deterministic jobs
* dead-letter processing

The worker must use the same approved:

* Authorization context where applicable
* domain rules
* contracts
* audit infrastructure
* observability conventions
* idempotency controls

### 6.3 AI Worker

Path:

```text
apps/ai-worker
```

Responsibilities may include:

* Application summaries
* document-orientation summaries
* maintenance classification suggestions
* communication drafts
* AI evaluation execution
* model regression jobs
* structured-output validation
* human-review preparation

The AI worker must not independently execute protected domain commands.

It must not:

* Approve or reject applications
* verify payments
* allocate funds
* approve refunds
* activate agreements
* activate occupancy
* terminate agreements
* issue legal decisions
* reveal unrelated restricted data
* send material external communication without approved human review

The AI worker may be deployed separately because AI workloads have distinct:

* Latency
* provider dependencies
* cost characteristics
* rate limits
* retry behavior
* resource requirements
* safety requirements
* observability requirements
* failure modes

## 7. Modular-Monolith Boundary

The business system will remain a modular monolith during the MVP.

Initial domain modules include:

```text
Identity
Organizations
Properties
Listings
Leasing
Finance
Agreements
Occupancy
Maintenance
Documents
Communications
Support
Platform
```

These domains may exist as bounded directories or packages.

They must not be split into independently deployed microservices during the MVP without a separately approved ADR.

## 8. Domain Package

Path:

```text
packages/domain
```

The domain package will contain deterministic business concepts and rules.

Proposed internal structure:

```text
packages/domain/
├── identity/
├── organizations/
├── properties/
├── listings/
├── leasing/
├── finance/
├── agreements/
├── occupancy/
├── maintenance/
├── documents/
├── communications/
├── support/
└── platform/
```

Domain code may include:

* Entities
* value objects
* aggregates where useful
* domain services
* commands
* business-rule evaluation
* state transitions
* domain errors
* domain events
* policy-neutral validation

The domain package must not directly import:

* Next.js
* React
* database clients
* ORM clients
* provider SDKs
* email SDKs
* payment SDKs
* signature SDKs
* AI SDKs
* object-storage SDKs
* queue-provider SDKs
* browser APIs

## 9. Contracts Package

Path:

```text
packages/contracts
```

The contracts package may contain:

* Command schemas
* query schemas
* event envelopes
* domain-event contracts
* job payloads
* provider-neutral interfaces
* API schemas
* structured-output schemas
* shared validated identifiers

Contracts must be versioned where cross-process compatibility requires it.

## 10. Authorization Package

Path:

```text
packages/authorization
```

Responsibilities:

* Permission catalogue
* role-permission mapping
* authorization-policy evaluation
* organization context
* resource-organization validation
* assignment validation
* record-state checks
* separation-of-duty checks
* field-access policy
* authorization test helpers

The package must not rely on client-visible navigation or role labels as proof of authorization.

## 11. Audit Package

Path:

```text
packages/audit
```

Responsibilities:

* Audit-event contract
* event classification
* append interface
* safe metadata rules
* restricted-event access
* correlation linkage
* audit-query contracts
* test utilities

Ordinary application logs must not replace authoritative audit records.

## 12. Events Package

Path:

```text
packages/events
```

Responsibilities:

* Domain-event definitions
* event envelopes
* event versioning
* event metadata
* outbox contracts
* event-consumer contracts
* idempotency identifiers
* correlation propagation

Illustrative events:

```text
ApplicationSubmitted
ApplicationDecisionRecorded
PaymentEvidenceSubmitted
PaymentVerified
PaymentAllocated
AgreementIssued
AgreementSigned
AgreementActivated
OccupancyActivated
MaintenanceRequestCreated
```

## 13. Database Package

Path:

```text
packages/database
```

Responsibilities:

* Database client configuration
* migration access
* repository implementations
* transaction utilities
* connection lifecycle
* query helpers
* test database support
* outbox persistence
* audit persistence
* tenant-safe data access conventions

The final ORM or query-builder decision will be made in a later ADR.

## 14. Integration Package

Path:

```text
packages/integrations
```

Proposed structure:

```text
packages/integrations/
├── payments/
├── signatures/
├── email/
├── messaging/
├── scanning/
├── identity/
├── storage/
└── ai/
```

Responsibilities:

* Provider-neutral ports
* provider adapters
* normalized provider responses
* provider error mapping
* timeout behavior
* retry classification
* webhook verification
* safe provider telemetry
* provider simulators

Provider SDKs must remain within adapter implementations.

## 15. Jobs Package

Path:

```text
packages/jobs
```

Responsibilities:

* Job contracts
* enqueue interfaces
* worker handlers
* retry rules
* idempotency
* dead-letter behavior
* scheduled-job definitions
* correlation propagation
* job observability

The final queue and worker framework will be selected through a later ADR.

## 16. Security Package

Path:

```text
packages/security
```

Responsibilities may include:

* Data-classification types
* redaction utilities
* secure token helpers
* hashing utilities
* webhook-signature verification
* encryption interfaces
* secure headers
* input-hardening helpers
* sensitive-value detection

The security package does not replace domain authorization.

## 17. Observability Package

Path:

```text
packages/observability
```

Responsibilities:

* Structured logging
* correlation identifiers
* trace propagation
* metric contracts
* safe telemetry context
* redaction integration
* health-check helpers
* provider instrumentation
* AI instrumentation

## 18. Testing Package

Path:

```text
packages/testing
```

Responsibilities:

* Test factories
* fixture builders
* synthetic organizations
* authorization matrices
* database-test helpers
* provider simulators
* deterministic clocks
* mock identity contexts
* AI evaluation fixtures

Real customer documents, payment evidence, signed agreements, or identity records must not be stored in testing fixtures.

## 19. UI Package

Path:

```text
packages/ui
```

Responsibilities:

* Design tokens
* accessible components
* form components
* status indicators
* layout primitives
* tables
* dialogs
* navigation components
* loading, empty, error, and pending states

The UI package must not own authoritative domain-state transitions.

## 20. Configuration Package

Path:

```text
packages/config
```

Responsibilities:

* Environment schema
* runtime configuration
* feature configuration
* environment classification
* safe startup validation
* public-versus-secret separation
* provider configuration contracts

## 21. Responsible AI Infrastructure

Responsible AI infrastructure will be treated as a first-class architectural capability.

It will not be implemented as scattered direct model calls throughout the application.

The approved structure is:

```text
Habitacores Domain
→ Approved AI Use Case
→ Authorization
→ Organization Scope
→ Data Classification
→ Retrieval Policy
→ Redaction and Minimization
→ Prompt Registry
→ AI Gateway
→ Model Adapter
→ Safety Validation
→ Structured-Output Validation
→ Human Review
→ Deterministic Domain Command
→ Audit and Evaluation
```

## 22. AI Gateway

Path:

```text
ai/gateway
```

The AI Gateway will be the only approved path for external-model invocation.

Responsibilities:

* Use-case validation
* organization-context validation
* actor-context validation
* provider selection
* model selection
* model-version selection
* prompt-version selection
* token limits
* timeouts
* retries
* cost controls
* structured-output enforcement
* safety checks
* correlation identifiers
* safe audit metadata
* fallback behavior

No product module may call an AI provider SDK directly.

## 23. AI Use-Case Registry

Every AI capability must be explicitly registered.

Illustrative use cases:

```text
application.summary
document.orientation
maintenance.classification
communication.draft
support.knowledge-assistance
```

Each registration must define:

* Use-case identifier
* business owner
* approved roles
* approved permissions
* approved resource types
* approved input fields
* prohibited input fields
* data-classification ceiling
* approved model configurations
* prompt version
* output schema
* human-review requirement
* retention rule
* evaluation suite
* failure behavior
* cost limit
* operational owner

A generic unrestricted AI interface is not approved for the MVP.

## 24. Model Registry

Path:

```text
ai/model-registry
```

The model registry will describe approved model configurations.

A model record should include:

* Internal configuration ID
* provider
* provider model name
* approved use cases
* status
* data-classification limit
* retention setting
* regional setting where available
* maximum context
* output capabilities
* approval date
* approved by
* review date
* retirement status

Business code must reference internal model-configuration identifiers rather than hardcoded provider model names.

## 25. Prompt Registry

Path:

```text
ai/prompts
```

Prompts must be:

* Versioned
* reviewable
* testable
* attributable
* use-case specific
* schema bound
* evaluation bound

A prompt record should define:

* Prompt ID
* version
* use case
* system instructions
* task instructions
* approved variables
* untrusted-content boundaries
* expected output schema
* safety rules
* evaluation suite
* approval state
* change history

Material prompt changes must be reviewed like software changes.

## 26. AI Retrieval Layer

Path:

```text
ai/retrieval
```

The retrieval layer must enforce:

* Authenticated actor
* organization context
* permission
* assignment
* resource scope
* field allowlist
* document sensitivity
* version validity
* retention status

The model must never independently query unrestricted databases, search indexes, or vector stores.

Any search or vector infrastructure remains subordinate to the application authorization layer.

## 27. AI Redaction and Minimization

Path:

```text
ai/redaction
```

Responsibilities:

* Remove unnecessary personal identifiers
* mask sensitive identifiers
* remove secrets
* filter prohibited fields
* enforce data-classification limits
* minimize document excerpts
* preserve source references
* record redaction outcome safely

Restricted data must not be sent to an external model merely because it is available in the database.

## 28. AI Safety Layer

Path:

```text
ai/safety
```

Responsibilities may include:

* Prompt-injection detection
* untrusted instruction separation
* prohibited-output checks
* cross-tenant reference detection
* unsupported-decision detection
* sensitive-output detection
* model refusal handling
* malformed-output handling

Safety checks complement authorization; they do not replace it.

## 29. AI Evaluation Infrastructure

Path:

```text
ai/evaluations
```

Every approved AI use case must have an evaluation suite.

Evaluation dimensions should include:

* Factual accuracy
* grounding
* source attribution
* completeness
* hallucination
* privacy leakage
* tenant isolation
* prohibited behavior
* prompt-injection resistance
* bias and fairness
* consistency
* schema compliance
* latency
* cost
* human acceptance
* human edit rate
* human rejection rate

Evaluation fixtures should include:

* Synthetic applications
* incomplete records
* conflicting records
* unknown values
* adversarial embedded instructions
* Nigerian-English language variations
* invalid documents
* cross-tenant attack attempts
* missing-source cases

## 30. AI Release Gate

A model, prompt, retrieval, or safety-policy change must follow:

```text
Proposed Change
→ Offline Evaluation
→ Safety Evaluation
→ Privacy Evaluation
→ Tenant-Isolation Tests
→ Cost Review
→ Human Review
→ Staging Validation
→ Approval
→ Controlled Release
→ Production Monitoring
```

A successful API response is not sufficient release evidence.

## 31. AI Human-Authority Rule

The approved pattern is:

```text
AI proposes
→ Human reviews
→ Authorized human decides
→ Deterministic domain command executes
→ Audit records the result
```

AI output must not independently produce a protected domain-state transition.

## 32. AI Failure Isolation

AI features must degrade independently.

When AI is unavailable:

* Manual workflows remain accessible.
* authoritative records remain available.
* no protected state is falsely completed.
* the user receives a clear status.
* retry may occur where approved.
* operational monitoring records the failure.

## 33. AI Observability

AI operations should record safe metadata including:

* Use-case ID
* prompt version
* model-configuration ID
* organization ID
* actor ID where appropriate
* resource type
* input classification
* token use
* latency
* output-validation status
* safety-validation status
* human acceptance
* human edit
* human rejection
* correlation ID
* estimated cost

Raw prompts and raw outputs must not automatically enter ordinary logs.

## 34. Package Dependency Direction

The approved dependency direction is:

```text
User Interface
↓
Application Services
↓
Domain
↓
Ports and Contracts
```

Infrastructure provides implementations:

```text
Database Adapter
Storage Adapter
Queue Adapter
Provider Adapter
AI Adapter
Email Adapter
Signature Adapter
Payment Adapter
```

Dependencies must point inward toward stable business abstractions.

## 35. Forbidden Dependency Directions

The following are prohibited:

* Domain importing Next.js
* domain importing React
* domain importing a database client
* domain importing provider SDKs
* domain importing AI provider SDKs
* authorization depending on UI navigation
* finance depending directly on payment-provider objects
* agreements depending directly on signature-provider objects
* business modules invoking external AI models directly
* UI bypassing application services for protected writes
* worker code duplicating domain rules
* AI worker owning authoritative business state

## 36. Internal Package Publication

Internal packages will initially remain private to the repository.

They will not be published publicly or to an external package registry unless an approved need arises.

Workspace packages may use a naming convention such as:

```text
@habitacores/domain
@habitacores/database
@habitacores/authorization
@habitacores/audit
@habitacores/integrations
@habitacores/observability
@habitacores/ui
```

The final package-manager and workspace-tool decision will be made in a later ADR.

## 37. Event and Outbox Strategy

Habitacores will use domain events and an outbox-based delivery pattern for material asynchronous processing.

Initial flow:

```text
Domain Transaction
→ Authoritative State Change
→ Outbox Event Written in Same Transaction
→ Worker Reads Event
→ Idempotent Consumer
→ External or Internal Side Effect
→ Processing Result Recorded
```

This supports:

* Reliable notifications
* webhook processing
* reconciliation
* analytics
* AI processing
* future event-streaming integration
* future service extraction

A distributed event-streaming platform is not required for the MVP.

## 38. Database Strategy Within the Repository

The initial application will use one PostgreSQL system of record.

The database will contain:

* Organization-scoped business records
* audit records
* outbox records
* job metadata
* integration-event metadata
* AI-operation metadata
* evaluation metadata where appropriate

Separate databases per domain are not approved for the MVP.

Database access must still respect modular ownership boundaries.

## 39. Vector and Semantic Retrieval Strategy

No independent vector database is approved by this ADR.

Where semantic retrieval becomes necessary, the initial preference is:

1. Preserve authorization and document metadata in PostgreSQL.
2. use a retrieval abstraction.
3. evaluate PostgreSQL-compatible vector functionality.
4. introduce an independent vector service only when a validated use case requires it.

No semantic-search system may independently determine tenant access.

## 40. Infrastructure Placement

Path:

```text
infrastructure
```

The repository may contain:

* Environment definitions
* reusable infrastructure modules
* monitoring configuration
* security policies
* deployment definitions
* infrastructure documentation

The final infrastructure-as-code tool will be selected through a later ADR.

## 41. Documentation Placement

The application repository may contain implementation-focused documentation, including:

* Local development
* repository architecture
* module boundaries
* API usage
* migrations
* testing
* provider setup
* deployment
* runbooks
* security operations
* Responsible AI implementation

The complete Product Knowledge Base will remain in the separate knowledge-base repository.

The application repository should reference approved Product Knowledge Base documents and ADRs rather than duplicating thousands of specification lines.

## 42. ADR Placement

Application-specific ADRs may be mirrored or maintained in:

```text
adr/
```

The authoritative ADR location must remain explicit.

Until a later governance decision changes it, ADR approval will occur in the Product Knowledge Base repository.

Approved ADR summaries may then be copied or linked into the application repository.

## 43. Test Placement

Tests may be colocated with modules where this improves maintainability.

Repository-wide tests may be stored in dedicated paths.

Recommended model:

```text
Unit tests:
Colocated with source

Integration tests:
Module-specific integration directories

End-to-end tests:
Central application test directory

AI evaluations:
ai/evaluations

Provider contract tests:
packages/integrations/<provider-category>/tests
```

## 44. Generated Code

Generated code must be clearly separated and reproducible.

Generated artifacts must not be manually edited unless explicitly permitted.

Examples may include:

* Database client output
* API clients
* schema-derived types
* test reports
* build artifacts

Generated files should not be committed unless repository policy explicitly requires them.

## 45. Repository Ownership

Initial ownership areas should include:

* Web experience
* domain architecture
* database
* authorization
* security
* integrations
* infrastructure
* Responsible AI
* testing
* operations

Formal CODEOWNERS rules may be introduced when the team structure is known.

## 46. Branch and Pull-Request Model

The application repository should use:

* Protected `main`
* short-lived feature branches
* pull requests
* required checks
* reviewed migrations
* reviewed infrastructure changes
* reviewed Responsible AI changes
* no direct production-secret commits

Suggested branch examples:

```text
feat/organization-memberships
feat/property-hierarchy
fix/payment-allocation-conflict
security/webhook-replay-protection
infra/staging-database
ai/application-summary-evaluation
```

## 47. Codex Boundaries

Codex work packages should specify:

* Repository path
* allowed applications
* allowed packages
* exact files where known
* architectural source
* business rules
* authorization rules
* data model
* test requirements
* excluded scope
* validation commands

Codex must not reorganize the monorepo or create new packages without an approved architecture requirement.

## 48. Future Application Expansion

The repository structure supports future applications such as:

```text
apps/
├── web/
├── worker/
├── ai-worker/
├── partner-api/
├── admin-operations/
└── mobile-backend/
```

No future application is approved merely because the structure allows it.

Each new deployable application requires architectural review.

## 49. Future Service Extraction

A domain may be considered for independent service extraction only when evidence shows that separate deployment provides material benefit.

Potential evidence includes:

* Independent scaling pressure
* materially different availability requirements
* specialized security boundary
* specialized data-residency requirement
* independent team ownership
* provider isolation
* sustained deployment contention
* measured performance constraint

Service extraction must not occur solely because microservices are fashionable.

## 50. Options Considered

### Option A — Single Next.js Repository

Example:

```text
habitacores/
├── src/
├── tests/
├── database/
└── docs/
```

#### Advantages

* Simple initial setup
* low tooling overhead
* easy for a very small prototype
* fewer package boundaries

#### Disadvantages

* Easier for business logic to enter route handlers
* weaker internal boundaries
* worker evolution becomes awkward
* AI calls may become scattered
* provider contamination risk
* less precise Codex scope
* harder future application expansion
* shared testing and infrastructure become less organized

#### Decision

Rejected as the primary long-term structure.

Its simplicity does not sufficiently support Habitacores’ domain breadth and Responsible AI requirements.

### Option B — TypeScript Monorepo With Modular Monolith

#### Advantages

* Strong internal boundaries
* shared contracts
* shared authorization
* shared testing
* shared observability
* coordinated changes
* future worker support
* Responsible AI isolation
* future application expansion
* easier refactoring
* precise Codex work packages
* no premature network distribution

#### Disadvantages

* Requires workspace discipline
* may introduce tooling complexity
* packages can become poorly bounded without governance
* build configuration requires care
* developers may create unnecessary packages

#### Decision

Approved.

### Option C — Multiple Independent Repositories

Possible repositories:

* Web application
* worker
* AI service
* infrastructure
* shared contracts
* UI library

#### Advantages

* Strong repository separation
* independent deployment
* independent ownership
* potentially independent access controls

#### Disadvantages

* Version coordination
* contract drift
* duplicated tooling
* more CI/CD pipelines
* more dependency management
* slower refactoring
* operational complexity
* premature service boundaries
* harder small-team development
* increased Codex coordination burden

#### Decision

Rejected for the MVP.

### Option D — Microservices From Initial Launch

#### Advantages

* Independent runtime scaling
* independent deployment
* strong process isolation

#### Disadvantages

* Distributed transactions
* network failure
* event consistency complexity
* observability complexity
* deployment complexity
* security complexity
* higher cost
* slower MVP delivery
* uncertain service boundaries
* premature operational burden

#### Decision

Rejected.

## 51. Consequences

### Positive Consequences

* Habitacores can move quickly without becoming structurally careless.
* Domain rules can remain separated from frameworks.
* AI behavior can be governed centrally.
* Provider integrations can remain replaceable.
* workers can evolve independently.
* tests can share controlled fixtures.
* tenant isolation can be tested consistently.
* Codex can receive bounded package-level tasks.
* future applications can be introduced without repository migration.
* future service extraction remains possible.

### Negative Consequences

* The repository will require workspace tooling.
* dependency rules must be enforced.
* package ownership must remain disciplined.
* changes may affect multiple packages.
* build caching and CI design require attention.
* the team must resist unnecessary package proliferation.
* Responsible AI infrastructure introduces governance work before advanced AI features launch.

### Neutral Consequences

* Some directories will be introduced only when required.
* the exact package manager remains unresolved.
* the exact build-orchestration tool remains unresolved.
* the exact application-hosting model remains unresolved.
* provider selections remain unresolved.
* the exact database library remains unresolved.

## 52. Risks

### 52.1 Monorepo Becomes a Monolith Without Boundaries

Control:

* Dependency rules
* module ownership
* architecture tests
* package contracts
* pull-request review

### 52.2 Excessive Package Fragmentation

Control:

* Create packages only for stable boundaries.
* keep small domain modules together initially.
* require architectural justification for new top-level packages.

### 52.3 AI Gateway Becomes a Generic Bypass

Control:

* Use-case registry
* role and permission checks
* approved schemas
* data-classification limits
* evaluation gates
* audit

### 52.4 AI Worker Gains Domain Authority

Control:

* No direct protected-state transitions
* commands require authorized deterministic services
* explicit human review
* domain packages remain authoritative

### 52.5 Cross-Tenant Retrieval

Control:

* Authorization before retrieval
* organization metadata
* field allowlists
* adversarial tests
* retrieval evaluation
* no unrestricted vector search

### 52.6 Provider Lock-In

Control:

* Internal model registry
* provider adapters
* normalized contracts
* provider-specific code isolation
* later provider-selection ADRs

### 52.7 Premature Service Extraction

Control:

* Separate ADR
* evidence threshold
* operational readiness
* measured scaling requirement

### 52.8 Product Knowledge Base Drift

Control:

* Traceability
* ADR links
* implementation documentation
* architecture review
* Product Knowledge Base updates when approved decisions change

## 53. Architecture Enforcement

The repository should eventually enforce boundaries through:

* TypeScript project references where appropriate
* package export maps
* lint rules
* dependency rules
* architecture tests
* CI checks
* CODEOWNERS where appropriate
* pull-request templates
* Codex work-package scopes

Specific tooling will be determined later.

## 54. Acceptance Criteria

### Repository Structure

```text
Given:
The Habitacores application repository is initialized.

When:
The root structure is inspected.

Then:
It supports applications, internal packages, AI infrastructure, infrastructure definitions, tooling, scripts, documentation, and ADR references.

And:
The structure does not require microservices.
```

### Domain Independence

```text
Given:
A core finance domain module exists.

When:
Its dependencies are inspected.

Then:
It does not import Next.js, React, a payment SDK, an AI SDK, or a database provider directly.
```

### AI Gateway

```text
Given:
An approved AI use case needs model inference.

When:
The application invokes the use case.

Then:
The request passes through the centralized AI Gateway.

And:
The use case, organization, actor, model configuration, prompt version, data classification, output schema, and correlation identifier are validated.
```

### Human Authority

```text
Given:
AI produces an application summary or recommendation.

When:
The output is returned.

Then:
It is labeled as AI-assisted.

And:
It cannot approve or reject the application.

And:
An authorized human must make the deterministic decision.
```

### Tenant-Safe Retrieval

```text
Given:
A user belongs only to Organization A.

When:
An AI use case retrieves supporting content.

Then:
Only records authorized for that user within Organization A are included.

And:
No Organization B metadata, text, embedding result, or document content is returned.
```

### AI Failure

```text
Given:
The AI provider or AI worker is unavailable.

When:
A user opens an approved workflow.

Then:
The authoritative workflow remains available.

And:
No protected state is falsely completed.

And:
The AI capability reports a clear degraded or unavailable state.
```

### Event Reliability

```text
Given:
A domain transaction creates a material asynchronous side effect.

When:
The transaction commits.

Then:
The corresponding outbox event is persisted in the same transaction.

And:
The worker may process it idempotently.
```

### Pilot Neutrality

```text
Given:
The founding property is configured.

When:
The repository and domain modules are reviewed.

Then:
No application, package, route, worker, AI prompt, or database access rule assumes that every organization has Block A, Block B, six units, a masjid, a second building, or a water feature.
```

## 55. Implementation Guidance

The repository should not be fully scaffolded in one uncontrolled coding task.

Recommended implementation sequence:

```text
1. Create repository and workspace root
2. Create apps/web
3. Create essential shared configuration
4. Create test foundation
5. Create database foundation
6. Create domain foundation
7. Create authorization foundation
8. Create audit and observability foundations
9. Create worker only when the first asynchronous use case requires it
10. Create AI infrastructure only when the first approved AI work package begins
11. Create ai-worker only when asynchronous AI execution is required
12. Add infrastructure directories when the infrastructure ADR is approved
```

This avoids empty architecture theatre while preserving the approved structure.

## 56. Deferred Decisions

The following are not decided by ADR-001:

* Package manager
* workspace orchestration tool
* Node.js version
* Next.js version
* database ORM or query builder
* migration framework
* authentication provider
* authorization library
* row-level-security strategy
* identifier format
* money representation
* object-storage provider
* queue provider
* hosting provider
* monitoring provider
* payment provider
* signature provider
* email provider
* AI provider
* AI model
* vector capability
* feature-flag provider
* infrastructure-as-code tool
* CI provider configuration
* native mobile architecture

These require subsequent ADRs.

## 57. Supersession Conditions

This ADR should be reviewed when:

* A new major application is introduced.
* multiple engineering teams require independent repository ownership.
* domain extraction becomes operationally necessary.
* a regulatory boundary requires separate infrastructure or repository access.
* a dedicated AI platform becomes independently operated.
* build or CI performance becomes materially unacceptable.
* repository access controls cannot satisfy organizational requirements.
* the modular monolith no longer supports measured production needs.

## 58. Final Decision Statement

Habitacores will use a TypeScript monorepo containing a modular-monolith business architecture, a Next.js web application, deterministic background-processing capability, an independently bounded AI-processing capability, shared internal packages, infrastructure definitions, engineering tooling, and implementation documentation.

Responsible AI capabilities will use:

* A centralized AI Gateway
* an approved use-case registry
* a model registry
* a prompt registry
* tenant-safe retrieval
* data classification
* redaction and minimization
* structured-output validation
* safety controls
* human review
* evaluation suites
* AI-specific observability
* provider-neutral adapters

AI will remain a supporting capability and will not own authoritative Habitacores business state.

The repository will support future applications, workers, event processing, and service extraction while preserving a simple MVP deployment model.

Microservices, multiple application repositories, unrestricted model access, ungoverned vector retrieval, and direct AI-provider calls from product modules are not approved for the MVP.