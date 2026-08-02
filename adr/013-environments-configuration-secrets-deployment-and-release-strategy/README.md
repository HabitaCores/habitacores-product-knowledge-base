# ADR-013 — Environments, Configuration, Secrets, Deployment, and Release Strategy

## Decision Record

| Field | Value |
|---|---|
| Product | Habitacores |
| Decision | Environments, Configuration, Secrets, Deployment, and Release Strategy |
| ADR | ADR-013 |
| Version | 0.1 |
| Status | Proposed |
| Decision Type | Foundational Platform, Security, and Delivery Architecture |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Web Deployment Platform | Vercel |
| Source-Control and CI Authority | GitHub and GitHub Actions |
| Production Branch | `main` |
| Deployment Artifact Model | Immutable deployment tied to Git commit |
| Environment Model | Local, test, preview, staging, and production |
| Configuration Model | Typed, schema validated, and fail fast |
| Secret Storage | Approved environment-scoped secret systems |
| Client-Exposed Configuration | Explicit public allowlist only |
| Database Migration Model | Expand, migrate, contract |
| Application Rollback | Supported through immutable deployment promotion or rollback |
| Database Rollback | Forward repair by default |
| Production Release | Protected, traceable, and health verified |
| Feature Flags | Controlled release only; not authorization |
| Production Data in Non-Production | Prohibited by default |
| Last Updated | 1 August 2026 |

## 1. Context

Habitacores will operate a multi-tenant real-estate platform containing:

- Personal information
- organization records
- property records
- rental applications
- identity documents
- payment evidence
- financial transactions
- receipts
- agreements
- occupancy records
- maintenance records
- support records
- Responsible AI operations
- audit and evidentiary history

The product will include:

- A Next.js web application
- PostgreSQL
- Supabase Auth
- Supabase Storage
- background workers
- scheduled jobs
- transactional outbox processing
- external provider integrations
- document-processing services
- Responsible AI providers
- observability exporters

These components require explicit environment separation, configuration governance, secret handling, deployment controls, migration sequencing, release evidence, and recovery procedures.

Without a defined release architecture, Habitacores risks:

- Deploying unreviewed code
- exposing secrets to browsers
- using production credentials in preview environments
- allowing preview deployments to modify production data
- applying destructive database migrations before compatible code exists
- releasing workers and web applications with incompatible message contracts
- deploying code that cannot be traced to a reviewed commit
- confusing application rollback with database rollback
- leaking production data into local or test environments
- treating environment variables as unvalidated strings
- enabling features without authorization or tenant-isolation controls
- losing evidence of who deployed what and when

## 2. Decision Drivers

The decision is driven by the need to:

1. Preserve strict production isolation.
2. support rapid preview and review workflows.
3. keep releases traceable to source control.
4. prevent secrets from entering Git or browser bundles.
5. validate required configuration before serving traffic.
6. support safe PostgreSQL migrations.
7. support web and worker compatibility.
8. support application rollback.
9. preserve audit and observability.
10. prevent preview environments from using production data.
11. support controlled feature rollout.
12. support emergency fixes without abandoning governance.
13. minimize deployment-platform lock-in.
14. support future mobile and API deployments.
15. support infrastructure recovery.
16. protect provider credentials.
17. support deterministic builds.
18. support release verification.
19. define Codex implementation boundaries.
20. prevent environment ambiguity.

## 3. Decision

Habitacores will use a source-controlled, environment-isolated, immutable-deployment release architecture.

The approved flow is:

```text
Feature branch
→ Pull request
→ Static validation
→ Tests
→ Security checks
→ Preview deployment
→ Human review
→ Merge to main
→ Production release checks
→ Compatible database migration
→ Application deployment
→ Worker deployment
→ Health verification
→ Release observation
→ Completion or rollback
````

Every production deployment must be attributable to:

* Git commit
* repository
* workflow run
* release initiator
* approving authority where required
* deployment environment
* application version
* database migration state
* deployment time
* health result

## 4. Initial Deployment Platform

The Next.js web application will initially deploy to Vercel.

Vercel is an implementation platform, not the permanent architectural contract.

Habitacores deployment abstractions must preserve:

* Git-based source authority
* standard Next.js application structure
* portable environment configuration
* provider-neutral observability
* provider-neutral domain code
* independent PostgreSQL ownership
* provider-neutral workers and integrations

## 5. CI and Deployment Governance

GitHub Actions will provide the primary continuous-integration and release-governance workflow.

GitHub Actions should perform:

* Dependency installation
* lockfile verification
* formatting checks
* linting
* type checking
* unit tests
* integration tests
* migration validation
* security scanning
* build verification
* artifact metadata generation
* deployment verification
* release evidence collection

Vercel may build and deploy the web application, but GitHub remains the reviewed source and workflow authority.

## 6. Source-Control Authority

Only committed, reviewed source code may become an ordinary production release.

Production deployments must originate from:

```text
main
```

or an explicitly approved immutable release commit or tag.

Production deployment directly from an unreviewed developer working tree is prohibited.

## 7. Environment Model

Habitacores will distinguish:

```text
LOCAL
TEST
PREVIEW
STAGING
PRODUCTION
```

These environments have different purposes and must not be treated as interchangeable.

## 8. Local Environment

Local development is for:

* Developer implementation
* rapid feedback
* local testing
* migration development
* synthetic data
* isolated provider testing
* controlled emulation

Local development must not require routine production access.

## 9. Test Environment

The test environment supports:

* Automated unit tests
* integration tests
* contract tests
* migration tests
* security tests
* tenant-isolation tests
* property-based tests
* deterministic fixtures

Tests should use ephemeral or isolated databases where practical.

## 10. Preview Environment

A preview environment is associated with a branch, pull request, or commit.

It supports:

* Product review
* UX review
* stakeholder demonstration
* acceptance testing
* integration verification
* accessibility review
* deployment verification

Preview environments are untrusted pre-production environments.

They must not receive production secrets or unrestricted production data.

## 11. Staging Environment

Staging is the stable pre-production integration environment.

It should approximate production in:

* Application topology
* runtime configuration
* authentication behavior
* database schema
* storage structure
* worker architecture
* observability
* provider adapters

Staging must use separate credentials, databases, storage, identity records, webhooks, and provider environments.

## 12. Production Environment

Production contains authoritative customer and business data.

Production access, deployment, secret use, migration, repair, export, and operational intervention require the highest controls.

Production must not be used as an ordinary testing environment.

## 13. Environment Identity

Every deployable application and worker must receive an explicit Habitacores environment identifier.

Example:

```text
HABITACORES_ENV=production
```

The application must not infer its complete security environment solely from:

```text
NODE_ENV
```

`NODE_ENV` describes framework/runtime behavior and is not sufficient to identify preview, staging, or production authority.

## 14. Environment Isolation

Each environment must have separate or appropriately isolated:

* PostgreSQL database
* database credentials
* Supabase Auth configuration
* storage buckets or projects
* webhook secrets
* email configuration
* payment-provider configuration
* AI-provider credentials
* observability context
* encryption keys
* scheduled jobs
* worker identities
* domain names
* callback URLs

## 15. Production Data Outside Production

Production personal, financial, legal, agreement, identity, and document data must not be copied into local, test, preview, or staging environments by default.

Approved exceptions require:

* Documented purpose
* authorization
* minimization
* masking or anonymization
* secure transfer
* bounded retention
* deletion verification
* audit

## 16. Synthetic Data

Non-production environments should use synthetic data representing:

* Organizations
* properties
* units
* applications
* payments
* documents
* agreements
* occupancies
* maintenance records
* AI operations

Synthetic data must not accidentally reproduce real secrets or personal records.

## 17. Preview Database Strategy

Preview deployments must not connect to production PostgreSQL.

Possible approved preview approaches include:

* Shared non-production integration database with branch-safe behavior
* isolated preview database
* schema-per-preview where supported and secure
* ephemeral database created for selected pull requests

The exact topology is deferred, but production connectivity is prohibited.

## 18. Preview Side Effects

Preview environments must not send real customer-facing effects by default.

Controls include:

* Email sink or allowlisted recipients
* sandbox payment providers
* test webhook destinations
* non-production storage
* disabled or isolated scheduled jobs
* test AI configuration
* visible environment labeling

## 19. Environment Domains

Domains should clearly communicate environment.

Directional examples:

```text
habitacores.com
app.habitacores.com
staging.habitacores.com
preview-generated-domain
```

Exact public domains remain a later implementation decision.

## 20. Configuration Classification

Configuration values will be classified as:

```text
PUBLIC
INTERNAL_NON_SECRET
SECRET
HIGHLY_RESTRICTED_SECRET
DYNAMIC_OPERATIONAL_CONFIGURATION
FEATURE_FLAG
```

Classification determines storage, exposure, validation, rotation, audit, and access.

## 21. Public Configuration

Public configuration may be exposed to browser code only when disclosure is intended.

Examples may include:

* Public application URL
* public analytics identifier
* intentionally public provider key
* public feature-display setting

A value being called a “key” does not necessarily mean it is secret, but exposure must be explicitly approved.

## 22. `NEXT_PUBLIC_` Rule

Any value exposed through a `NEXT_PUBLIC_` variable must be treated as public.

It must not contain:

* Database credentials
* service-role keys
* provider secrets
* private API keys
* webhook secrets
* encryption keys
* authentication administrative credentials
* signing keys
* storage administrative credentials

## 23. Server-Only Configuration

Server-only configuration must be read exclusively in trusted server or worker code.

Client Components must not import server configuration modules.

The build must fail where a server-only secret is imported into a browser boundary.

## 24. Typed Configuration

Habitacores will implement a central typed configuration package.

Conceptual interface:

```typescript
interface ApplicationConfiguration {
  environment: HabitacoresEnvironment;
  applicationUrl: URL;
  databaseUrl: string;
  identityProvider: IdentityProviderConfiguration;
  storage: StorageConfiguration;
  observability: ObservabilityConfiguration;
}
```

Sensitive values must not be exposed through client configuration types.

## 25. Configuration Schema

Configuration must be validated against an explicit schema.

Validation should confirm:

* Required fields
* data type
* URL syntax
* permitted enumeration
* minimum and maximum bounds
* environment-specific requirements
* forbidden combinations
* public-versus-secret boundary
* production safety conditions

## 26. Fail-Fast Configuration

A service with missing or invalid required configuration must fail before accepting production traffic or processing jobs.

It must not silently:

* Fall back to development
* connect to an arbitrary database
* disable authentication
* disable tenant isolation
* disable audit
* use a default cryptographic secret
* use a test provider in production
* treat a missing secret as an empty string

## 27. Configuration Access

Configuration should be loaded once through a controlled package or process boundary.

Application code should not read arbitrary `process.env` values throughout the codebase.

Approved:

```typescript
const config = getServerConfiguration();
```

Discouraged:

```typescript
const value = process.env.SOME_VARIABLE;
```

inside unrelated domain and feature modules.

## 28. Configuration Registry

Habitacores should maintain a registry describing each configuration entry.

The registry should include:

* Name
* classification
* owner
* purpose
* required environments
* format
* client exposure
* rotation requirement
* source system
* default policy
* validation rule
* restart or redeployment requirement

## 29. Defaults

Defaults are permitted only for values whose default behavior is safe and unambiguous.

Production-sensitive settings must not receive convenience defaults.

Examples requiring explicit values include:

* Environment identity
* database URL
* authentication endpoint
* service-role credentials
* webhook secrets
* application base URL
* storage administrative configuration
* encryption configuration

## 30. `.env.example`

The repository may contain:

```text
.env.example
```

with:

* Variable names
* non-secret examples
* descriptions
* safe placeholders

It must not contain real credentials or realistic secret material.

## 31. Local Secret Files

Files such as:

```text
.env.local
.env.development.local
.env.test.local
```

must remain excluded from version control where they contain local secrets.

Repository secret scanning must help detect accidental commits.

## 32. No Secrets in Git

Secrets must never be committed to:

* Source files
* Markdown documentation
* ADR examples
* tests
* fixtures
* screenshots
* notebooks
* CI workflow literals
* generated artifacts
* Git history

Removing a secret from the latest commit does not remove it from prior Git history or eliminate the need for rotation.

## 33. Secret Storage

Production and staging secrets must reside in approved environment-scoped secret systems.

Potential initial stores include:

* Vercel environment-secret controls
* GitHub environment secrets
* provider-native secret systems
* a dedicated secrets manager introduced later

The selected storage must match the consumer and threat model.

## 34. Secret Scope

Secrets should be scoped by:

* Environment
* service
* provider
* operation
* organization where required
* lifetime
* access role

A universal secret shared by every application and worker is prohibited unless technically unavoidable and explicitly approved.

## 35. Least-Privilege Credentials

Web, worker, migration, reconciliation, and operational processes should use separate credentials.

Examples:

```text
Web runtime database role
Worker database role
Migration database role
Storage scanner role
Webhook signer
Deployment identity
Observability exporter
```

## 36. Secret Rotation

Every long-lived production secret must have:

* Owner
* rotation procedure
* expected rotation interval
* dependent services
* validation process
* rollback process
* compromise response

Rotation should support overlapping validity where the provider permits it.

## 37. Secret Versioning

Secret versions must be tracked operationally without storing their values in application records.

A deployment may identify:

* Secret identifier
* version label
* rotation generation
* effective date

It must not emit secret contents.

## 38. Secret Revocation

A suspected compromised secret must be:

1. Revoked or disabled.
2. replaced.
3. propagated to required environments.
4. validated.
5. monitored for misuse.
6. investigated.
7. documented and audited.

## 39. Deployment Identity

CI and deployment systems should use dedicated machine identities.

Personal developer tokens should not be the long-term production deployment authority.

Where supported, short-lived federated credentials are preferred over permanent broad tokens.

## 40. CI Secret Access

Pull-request validation from untrusted branches or forks must not receive production secrets.

Workflow design must prevent untrusted code from:

* Printing secrets
* transmitting secrets
* modifying release workflows
* accessing production resources
* deploying to production

## 41. Build Reproducibility

Production builds should be derived from:

* Git commit
* root lockfile
* pinned Node.js major version
* pinned package-manager version
* reviewed build configuration
* known environment configuration

Dependency installation must use the frozen lockfile.

## 42. Build-Time and Runtime Configuration

Habitacores must distinguish:

```text
BUILD_TIME
RUNTIME
```

Build-time public values may become embedded in the application bundle.

Runtime secrets must remain server-side.

A value that may change without a rebuild should not be accidentally compiled into a client bundle.

## 43. Immutable Deployment

Each deployment must be treated as an immutable artifact tied to a commit.

A deployed artifact must not be modified manually in place.

A change requires:

* New commit
* new build
* new deployment
* new release record

## 44. Release Version

Every web application and worker deployment must expose a safe release identifier.

Directional representation:

```text
Git commit SHA
Application semantic version where used
Deployment ID
Build time
```

The release identifier should appear in observability and operational health data.

## 45. Pull-Request Checks

Before merge, the pull request should pass:

* Repository validation
* formatting
* linting
* type checking
* unit tests
* integration tests
* migration checks
* tenant-isolation checks
* security checks
* build
* preview deployment where applicable

Required checks should prevent accidental merge when critical validation fails.

## 46. Branch Protection

The `main` branch should require:

* Pull request
* required checks
* resolved review discussion
* no uncontrolled force push
* appropriate review
* up-to-date or merge-queue policy where selected

Direct pushes to `main` should be prohibited except through an explicit emergency process.

## 47. Production Deployment Protection

Production deployment should require:

* Commit from approved source
* successful required checks
* compatible migration state
* production configuration validation
* authorized deployment identity
* protected environment
* release metadata
* health verification

Manual approval requirements should reflect team maturity, GitHub plan capabilities, and deployment risk.

## 48. Self-Approval

For high-risk production releases, the person initiating the deployment should not be the sole approval authority when the team has sufficient staffing and tooling.

During the founding stage, constrained-team exceptions must remain documented rather than falsely claiming separation that does not exist.

## 49. Automated Deployment

After sufficient controls are established, a merge to `main` may trigger production deployment automatically.

Automation does not remove:

* Review
* tests
* migration safety
* release evidence
* rollback capability
* post-deployment verification

## 50. Release Candidate

A release candidate may be represented by:

* Approved commit
* preview deployment
* staging deployment
* immutable build artifact
* release tag

The exact release-candidate process may evolve with product scale.

## 51. Database Migration Authority

Database migrations remain reviewed source-controlled artifacts.

Only the approved migration process may apply production migrations.

The web runtime and ordinary workers must not receive schema-owner credentials.

## 52. Migration Sequence

The standard production sequence is:

```text
Validate migration
→ Confirm backup and recovery readiness
→ Apply backward-compatible expansion
→ Deploy compatible application and workers
→ Migrate or backfill data
→ Verify
→ Remove obsolete structures in a later release
```

## 53. Expand-Migrate-Contract

Habitacores will use the expand-migrate-contract pattern.

### Expand

Introduce additive, backward-compatible database structures.

### Migrate

Move or backfill data while old and new versions remain compatible.

### Contract

Remove obsolete structures only after no deployed application or worker depends on them.

## 54. Destructive Migration

A destructive production migration requires explicit review.

Examples:

* Dropping column
* dropping table
* changing meaning of stored value
* narrowing a data type
* deleting records
* changing currency representation
* rewriting identifiers
* modifying immutable financial or audit history

## 55. Migration and Deployment Compatibility

During rolling or asynchronous deployment, more than one application version may operate temporarily.

Migrations must account for:

* Existing web deployment
* new web deployment
* old workers
* new workers
* pending outbox messages
* provider callbacks
* scheduled jobs
* cached records

## 56. Message Compatibility

Worker deployments must continue supporting active approved outbox-message versions.

A message version may be removed only after proving no eligible messages remain or after an approved migration.

## 57. API Compatibility

Internal and external API changes should remain backward compatible during deployment windows.

Breaking changes require:

* New version or transition strategy
* consumer inventory
* rollout plan
* migration plan
* deprecation period
* observability
* rollback plan

## 58. Application Rollback

The web application may be rolled back by restoring traffic to a previously known-good immutable deployment.

Rollback must preserve:

* Deployment identity
* operator
* reason
* time
* health verification
* incident linkage

## 59. Database Rollback

Applied production migrations will not ordinarily be reversed mechanically.

The default strategy is:

```text
Forward repair
```

because reversing a database migration may lose data or become incompatible with events already processed.

## 60. Rollback Compatibility

An application rollback is safe only when the previous application version remains compatible with:

* Current database schema
* active configuration
* message versions
* provider contracts
* feature flags

Migration review must explicitly consider rollback compatibility.

## 61. Feature Flags

Feature flags may control:

* Gradual release
* internal testing
* selected organization pilots
* kill switches
* provider transition
* UI exposure
* new workflow activation

Feature flags do not replace source control or release review.

## 62. Feature Flags Are Not Authorization

A feature flag must never grant:

* Organization membership
* permission
* financial authority
* support authority
* document access
* agreement authority
* occupancy authority
* tenant access

A disabled UI feature may still require server-side authorization and domain enforcement.

## 63. Flag Evaluation

Security-sensitive feature evaluation must occur server side.

Client-side flags may control presentation only.

## 64. Flag Ownership

Every feature flag must have:

* Owner
* purpose
* creation date
* intended audience
* default
* production behavior
* removal condition
* expiry or review date
* telemetry

## 65. Flag Lifecycle

Directional lifecycle:

```text
PROPOSED
DEVELOPMENT
INTERNAL
PILOT
GENERAL_AVAILABILITY
RETIRED
```

Stale flags must be removed after rollout or abandonment.

## 66. Organization Pilots

A feature may be enabled for selected organizations.

The organization allowlist must use stable internal IDs and remain protected.

Organization names must not be hardcoded into application logic.

## 67. Kill Switches

A kill switch may disable a risky external or asynchronous capability, such as:

* AI generation
* outbound webhook delivery
* optional email delivery
* document conversion
* selected provider integration

A kill switch must not corrupt authoritative state or bypass required financial, audit, legal, or security controls.

## 68. Dynamic Operational Configuration

Settings requiring rapid runtime change may be stored in a controlled operational-configuration system rather than environment variables.

Examples may include:

* Provider circuit state
* bounded rate limits
* feature flags
* maintenance mode
* approved model configuration
* worker concurrency

These values require validation, authorization, audit, and caching rules.

## 69. Maintenance Mode

Maintenance mode may restrict selected operations while preserving:

* Secure authentication behavior
* read-only access where safe
* clear user communication
* administrative recovery
* audit
* health endpoints

Maintenance mode must not expose unrestricted administrative access.

## 70. Production Change Categories

Production changes should be classified as:

```text
STANDARD
ELEVATED
EMERGENCY
```

### Standard

Low-risk reviewed deployment using established automation.

### Elevated

Migration, provider change, security-sensitive configuration, or high-impact workflow change.

### Emergency

Urgent remediation of active production harm or material vulnerability.

## 71. Emergency Change

An emergency change may shorten ordinary approval steps, but it must preserve:

* Named actor
* reason
* incident reference
* minimal scope
* testing proportionate to urgency
* deployment evidence
* health verification
* post-change review
* follow-up pull request where necessary

Emergency does not mean unaudited.

## 72. Release Record

Every production release should create or preserve a release record containing:

* Release ID
* commit SHA
* deployment ID
* application version
* workflow run
* initiating actor
* approver where applicable
* migration identifiers
* configuration-version references
* feature-flag changes
* start time
* completion time
* health result
* rollback relationship
* incident reference where applicable

## 73. Release Audit

Material release actions should create audit events such as:

```text
PRODUCTION_DEPLOYMENT_STARTED
PRODUCTION_DEPLOYMENT_COMPLETED
PRODUCTION_DEPLOYMENT_FAILED
PRODUCTION_ROLLBACK_INITIATED
PRODUCTION_ROLLBACK_COMPLETED
DATABASE_MIGRATION_APPLIED
PRODUCTION_SECRET_ROTATED
FEATURE_FLAG_CHANGED
EMERGENCY_CHANGE_EXECUTED
```

Audit events must not contain secret values.

## 74. Deployment Observability

Every deployment must emit or expose:

* Release version
* deployment identifier
* environment
* deployment time
* migration state
* service health
* error-rate comparison
* latency comparison
* queue health
* provider health

## 75. Post-Deployment Verification

Production release verification should cover:

* Application health
* authentication
* PostgreSQL connectivity
* tenant-isolation context
* audit append
* outbox append
* worker processing
* storage access
* critical provider configuration
* error rates
* latency
* critical synthetic journey where available

## 76. Observation Window

High-risk releases should have an explicit observation window.

During the window, the release owner reviews:

* Error rate
* latency
* database health
* outbox lag
* terminal failures
* authentication failures
* document failures
* payment integration failures
* AI safety failures
* security signals

## 77. Release Completion

A deployment is not considered operationally complete merely because the hosting platform reports a successful build.

Completion requires:

* Production traffic updated
* required migrations applied
* services healthy
* workers compatible
* required verification passed
* no unresolved critical regression
* release evidence recorded

## 78. Release Failure

A failed release must result in one of:

* Deployment halted
* application rollback
* feature disabled
* forward fix
* incident declaration
* controlled degraded operation

Failure must not be hidden by repeated unreviewed deployments.

## 79. Backup Awareness

High-risk migration and release procedures must verify that recovery mechanisms are available.

A backup’s existence must not be assumed to prove restoration capability.

Restoration procedures require periodic testing under a later resilience decision.

## 80. Deployment Concurrency

Only one production database migration sequence should operate at a time.

Deployment workflows should prevent conflicting production releases from racing.

Potential controls include:

* Workflow concurrency groups
* deployment locks
* migration advisory locks
* release-state validation

## 81. Worker Deployment

Workers must be deployed with:

* Release identity
* message-version compatibility
* configuration validation
* least-privilege credentials
* readiness checks
* graceful shutdown
* observability
* rollback or forward-repair strategy

## 82. Scheduled Jobs During Deployment

Scheduled jobs must tolerate deployments.

Controls may include:

* Leader election
* durable job records
* idempotency
* maintenance windows
* temporary scheduler suspension
* current-state validation

## 83. Provider Configuration Change

Changing an identity, storage, payment, email, malware, AI, or observability provider configuration is a production change.

It requires:

* Compatibility review
* secret update
* environment validation
* test request
* monitoring
* rollback or dual-provider plan where appropriate
* audit

## 84. Webhook Configuration

Production webhook changes must preserve:

* Correct environment
* endpoint URL
* signing secret
* provider account
* event allowlist
* deduplication
* test event
* monitoring

A staging webhook must not send production events.

## 85. Callback URL Governance

Authentication, payment, storage, signing, and integration callback URLs must be:

* Environment specific
* allowlisted
* HTTPS in production
* source controlled where appropriate
* validated during deployment

Preview callback behavior requires explicit provider support and security review.

## 86. Domain and DNS Changes

Production domain and DNS changes require:

* Named authority
* review
* rollback information
* TTL awareness
* certificate verification
* security-header validation
* audit

DNS credentials must remain separate from ordinary application deployment credentials.

## 87. Configuration Drift

Habitacores should detect configuration drift between intended and deployed state.

Drift may include:

* Missing environment variable
* unexpected provider endpoint
* incorrect callback URL
* stale secret generation
* wrong feature-flag state
* production service using staging configuration
* worker version mismatch

## 88. Configuration Fingerprint

A service may expose a safe configuration fingerprint derived from non-secret configuration identities and secret-version labels.

The fingerprint must not permit secret recovery.

It can support confirming that web and workers use the intended configuration generation.

## 89. Secret Logging

Application startup and deployment logs must never print secret values.

Safe output may include:

```text
DATABASE_URL: configured
PAYMENT_WEBHOOK_SECRET: configured
AI_PROVIDER_KEY: missing
```

It must not print the value.

## 90. Error Messages

Configuration errors should identify:

* Missing variable name
* invalid classification
* invalid format
* unsafe environment combination

They must not echo secret contents.

## 91. Security Scanning

CI should progressively include:

* Dependency vulnerability scanning
* secret scanning
* static analysis
* lockfile integrity
* license review where required
* container or artifact scanning where applicable
* infrastructure configuration review

A scanner finding must be triaged rather than ignored or automatically presented as confirmed exploitation.

## 92. Dependency Updates

Dependency changes require the same pull-request validation as application code.

Automated update tools may open pull requests but must not deploy directly to production without required checks.

## 93. Supply-Chain Controls

Release workflows should protect against:

* Unpinned untrusted actions
* malicious package scripts
* lockfile tampering
* compromised CI credentials
* artifact substitution
* unreviewed workflow changes
* dependency confusion

Detailed supply-chain architecture may require a later ADR.

## 94. CI Workflow Changes

Changes to production workflow files are security-sensitive.

They require heightened review because workflow code may access:

* Repository contents
* environment secrets
* deployment identities
* release systems
* production infrastructure

## 95. Preview Security

Preview deployments may be accessible through generated URLs.

Sensitive previews should use appropriate protection or authorization.

A difficult-to-guess preview URL is not itself sufficient access control.

## 96. Preview Lifecycle

Preview resources should expire or be cleaned up after:

* Pull request closure
* branch deletion
* configured inactivity
* supersession

Cleanup must avoid deleting shared non-production resources still in use.

## 97. Cost Controls

Preview and staging environments should have:

* Resource limits
* lifecycle cleanup
* safe AI budgets
* safe email restrictions
* worker concurrency limits
* observability separation
* storage cleanup

Cost controls must not cause production secrets or data to be reused for convenience.

## 98. Responsible AI Deployment

AI configuration changes include:

* Model provider
* model identifier
* prompt version
* safety policy
* schema version
* retrieval-policy version
* token budget
* fallback model
* evaluation threshold

These changes must be versioned, tested, observable, and auditable.

## 99. AI Model Release

A new model configuration must not become production authority merely because it performs well in an ad hoc test.

Release requires:

* Registered use case
* evaluation
* safety review
* privacy review
* cost assessment
* schema validation
* human-review requirements
* pilot or controlled rollout
* rollback or kill switch

## 100. AI Prompt Deployment

Material prompt changes should be version controlled or stored in an approved prompt registry.

A prompt update must preserve:

* Prompt ID
* version
* owner
* use case
* model compatibility
* evaluation evidence
* deployment time
* rollback target

## 101. AI Secrets

AI-provider keys must remain server or worker secrets.

They must not enter:

* Client Components
* browser environment variables
* logs
* audit metadata
* prompts
* outbox payloads
* generated documents
* source control

## 102. Testing

Release-architecture tests should cover:

* Configuration validation
* missing production configuration
* unsafe public variable
* browser-secret import prevention
* environment identity
* preview-to-production isolation
* migration compatibility
* application rollback compatibility
* message-version compatibility
* worker configuration
* feature-flag authorization independence
* secret redaction
* deployment metadata
* provider callback separation

## 103. Production Configuration Test

```text
Given:
The production application is missing the required database configuration.

When:
The service starts.

Then:
It fails before accepting traffic.

And:
It does not fall back to a local, test, preview, or staging database.
```

## 104. Browser Secret Test

```text
Given:
A secret configuration value is declared server only.

When:
The Next.js application builds.

Then:
The value is not included in a client bundle.

And:
Importing the server configuration from a Client Component fails validation.
```

## 105. Preview Isolation Test

```text
Given:
A pull request creates a preview deployment.

When:
The preview application starts.

Then:
It uses non-production identity, database, storage, provider, and observability configuration.

And:
It cannot access production customer data or production service credentials.
```

## 106. Migration Rollback Test

```text
Given:
A production migration adds a new nullable column.

And:
The new application version is deployed.

When:
The application is rolled back to the previous version.

Then:
The previous version continues operating safely against the expanded schema.
```

## 107. Feature-Flag Authorization Test

```text
Given:
A finance feature flag is enabled for Organization A.

And:
A user lacks finance.allocate-payment permission.

When:
The user attempts to allocate a payment.

Then:
The operation is denied.

And:
Feature enablement does not grant authorization.
```

## 108. Release Traceability Test

```text
Given:
A production deployment is active.

When:
An operator inspects its release record.

Then:
The deployment resolves to its Git commit, workflow run, deployment ID,
migration state, initiating actor, environment, release time, and health result.
```

## 109. Secret-Rotation Test

```text
Given:
A provider secret is rotated.

When:
The replacement is deployed.

Then:
The service uses the new secret generation.

And:
The old secret is revoked after validation.

And:
Neither value appears in logs, audit metadata, Git, or release records.
```

## 110. Migration Requirements

Migrations supporting this ADR may introduce:

* Release records
* configuration-version references
* feature-flag records
* flag-audience records
* deployment audit relationships
* migration execution records
* operational configuration records

Any such tables must follow existing identifier, RLS, audit, and migration ADRs.

## 111. Codex Boundaries

Every Codex deployment or configuration work package must specify:

* Environment
* configuration classification
* secret boundary
* client-versus-server exposure
* CI checks
* deployment trigger
* database migration sequence
* web and worker compatibility
* feature-flag behavior
* rollback strategy
* audit events
* observability
* tests

Codex must not:

* Commit secrets
* create real credentials in examples
* expose secret values with `NEXT_PUBLIC_`
* connect preview deployments to production
* give production secrets to pull-request workflows
* read arbitrary environment variables throughout domain code
* introduce unsafe production defaults
* apply destructive migrations automatically
* treat database rollback as trivial
* use feature flags as authorization
* deploy unreviewed working-tree code
* modify deployed artifacts in place
* print configuration values during startup
* bypass branch or deployment protection
* send real customer email from previews by default

## 112. Prohibited Patterns

The following are not approved:

* Production credentials in Git
* production credentials in preview
* production data copied to development by convenience
* `NEXT_PUBLIC_` used for secrets
* unvalidated `process.env` access throughout the codebase
* empty-string secret defaults
* preview deployment connected to production PostgreSQL
* web runtime holding migration-owner credentials
* destructive migration and application deployment as one irreversible step
* application rollback without schema compatibility review
* manual mutation of deployed artifacts
* personal developer tokens as permanent deployment authority
* feature flags granting permissions
* unreviewed direct push to `main`
* production testing through real customer effects
* emergency deployment without later review and evidence
* logs containing secret values

## 113. Options Considered

### Option A — Manual Local Deployment

Advantages:

* Minimal setup
* direct operator control

Disadvantages:

* Weak traceability
* inconsistent builds
* personal credential dependency
* difficult rollback
* high human-error risk
* unsuitable for production governance

Decision:

Rejected.

### Option B — Vercel-Only Deployment Without Independent CI Governance

Advantages:

* Simple Next.js deployment
* rapid preview generation
* low initial setup

Disadvantages:

* Insufficient independent test and migration governance
* release semantics overly coupled to hosting
* weaker worker and database coordination
* limited provider portability

Decision:

Rejected as the complete architecture.

### Option C — Custom Container Platform Immediately

Advantages:

* Maximum runtime control
* unified web and worker infrastructure
* broad portability

Disadvantages:

* Greater operational complexity
* slower MVP delivery
* infrastructure burden
* unnecessary before measured runtime requirements

Decision:

Deferred.

### Option D — Vercel Web Deployment With GitHub-Governed Delivery

Advantages:

* Strong Next.js delivery path
* pull-request previews
* immutable deployments
* rapid rollback
* source-controlled CI
* independent migration governance
* provider-neutral workers and application semantics
* practical MVP operations

Disadvantages:

* Two platforms participate in delivery.
* environment mapping requires discipline.
* plan-dependent protection features must be verified.
* workers require a separate hosting decision.
* Vercel-specific deployment behavior requires adapters and documentation.

Decision:

Approved.

## 114. Positive Consequences

* Production releases become traceable.
* pull requests receive realistic preview deployments.
* secrets remain environment scoped.
* client-visible configuration is explicit.
* invalid configuration fails before serving traffic.
* production and non-production data remain separated.
* migrations support rolling compatibility.
* application rollback remains practical.
* workers and web deployments remain coordinated.
* feature rollout becomes controlled and observable.
* provider migration remains possible.

## 115. Negative Consequences

* Multiple environments increase cost and administration.
* staging and preview data require maintenance.
* release workflows require engineering effort.
* migration compatibility may require multi-step releases.
* secret rotation requires operational procedures.
* application rollback does not automatically reverse database changes.
* feature flags require governance and cleanup.
* private-repository deployment protections may depend on GitHub plan capabilities.

## 116. Risks

### 116.1 Secret Exposure

Controls:

* Environment-scoped secret storage
* public-variable allowlist
* client-boundary tests
* secret scanning
* redaction
* rotation

### 116.2 Environment Confusion

Controls:

* Explicit Habitacores environment identity
* separate resources
* visible non-production labeling
* startup validation
* environment-specific domains

### 116.3 Migration Incompatibility

Controls:

* Expand-migrate-contract
* migration tests
* rolling-version review
* worker compatibility
* forward repair

### 116.4 Preview Side Effects

Controls:

* Sandbox providers
* recipient allowlists
* isolated storage and databases
* disabled schedules
* environment tests

### 116.5 Unsafe Rollback

Controls:

* Schema compatibility
* immutable deployment history
* release records
* health verification
* forward-fix option

### 116.6 Feature-Flag Drift

Controls:

* Ownership
* expiry
* audit
* telemetry
* removal criteria
* configuration review

## 117. Acceptance Criterion — Production Source

```text
Given:
A production release is requested.

When:
The deployment workflow executes.

Then:
The deployment originates from an approved commit on main or an approved
immutable release reference.

And:
The active production artifact is traceable to that source.
```

## 118. Acceptance Criterion — Configuration Safety

```text
Given:
A production configuration identifies a development provider endpoint.

When:
Configuration validation executes.

Then:
The deployment fails before accepting production traffic.

And:
The unsafe combination is reported without exposing secret values.
```

## 119. Acceptance Criterion — Atomic Release Evidence

```text
Given:
A production release completes successfully.

When:
The release is finalized.

Then:
Habitacores preserves the commit, deployment, workflow, migration,
environment, actor, time, and health evidence.

And:
The release can be correlated with observability and audit records.
```

## 120. Acceptance Criterion — Emergency Change

```text
Given:
An active production vulnerability requires an urgent fix.

When:
An emergency release is executed.

Then:
The release remains attributable, minimally scoped, tested proportionately,
health verified, and linked to an incident.

And:
A post-change review is required.
```

## 121. Implementation Sequence

Recommended implementation order:

```text
1. Establish explicit Habitacores environment identifiers
2. create typed server and client configuration packages
3. create configuration schema and registry
4. create safe .env.example documentation
5. establish local secret-file exclusions and secret scanning
6. configure isolated Vercel preview and production environments
7. establish isolated Supabase non-production and production resources
8. establish GitHub Actions CI checks
9. establish branch and required-check protection
10. establish production deployment workflow
11. establish separate migration credentials and workflow
12. implement expand-migrate-contract validation
13. establish release identity and release records
14. instrument deployment and migration telemetry
15. establish post-deployment verification
16. establish application rollback procedure
17. implement controlled feature-flag service
18. establish emergency-change procedure
19. add preview-isolation and client-secret tests
20. establish staging when integration maturity requires it
```

## 122. Deferred Decisions

This ADR does not decide:

* Exact worker-hosting provider
* exact staging creation date
* exact preview-database topology
* exact GitHub plan
* exact production approval count
* exact merge-queue strategy
* exact feature-flag provider
* exact secrets-manager provider
* exact infrastructure-as-code tool
* exact release-tagging convention
* exact semantic-version policy
* exact canary deployment strategy
* exact blue-green strategy
* exact maintenance-window policy
* exact backup provider
* exact disaster-recovery procedure
* exact mobile-application release process
* exact container strategy
* exact software-bill-of-materials tooling
* exact artifact-signing strategy

These require bounded implementation decisions or later ADRs.

## 123. Supersession Conditions

This ADR should be reviewed when:

* Vercel no longer meets production requirements.
* the web application moves to containerized hosting.
* regional deployment becomes mandatory.
* enterprise customers require dedicated environments.
* deployment frequency requires canary or progressive delivery.
* worker infrastructure becomes independently deployed at scale.
* a dedicated secrets manager becomes necessary.
* infrastructure as code becomes authoritative.
* mobile clients require coordinated release trains.
* regulatory requirements mandate stronger release controls.
* supply-chain security requires signed deployment attestations.

## 124. Final Decision Statement

Habitacores will use an environment-isolated, source-controlled, immutable-deployment release architecture.

The Next.js web application will initially deploy through Vercel, while GitHub and GitHub Actions remain the reviewed source, CI, and release-governance authority.

Habitacores will distinguish local, test, preview, staging, and production environments. Each environment will use isolated credentials, data stores, identity configuration, storage, provider endpoints, webhook secrets, observability context, workers, and scheduled operations as appropriate.

Production data and credentials will not be reused in local, test, preview, or staging environments by convenience.

Configuration will be centrally defined, typed, schema validated, classified, and loaded through approved server or client boundaries. Services with missing, malformed, or unsafe required configuration will fail before accepting traffic or processing production work.

Any value exposed through a browser-accessible configuration mechanism will be treated as public. Database credentials, service-role keys, provider secrets, webhook secrets, signing keys, encryption keys, administrative identity credentials, storage credentials, and AI-provider secrets will remain server or worker only.

Secrets will reside in approved environment-scoped secret systems. They will not be committed to Git, embedded in documentation, printed in logs, stored in audit metadata, included in outbox payloads, sent to AI providers, or exposed through client bundles.

Production deployments will be immutable and traceable to an approved Git commit, workflow run, deployment ID, application version, migration state, initiating actor, environment, deployment time, and health result.

Database migrations will follow expand-migrate-contract. The migration role will remain separate from ordinary web and worker roles. Destructive changes will require explicit review and will not be coupled casually to application release.

Application rollback may restore a previously known-good immutable deployment. Applied database migrations will use forward repair by default. Every release must evaluate compatibility with previous web versions, workers, message versions, provider callbacks, scheduled jobs, and current database schema.

Feature flags may control rollout, pilots, experiments, provider transitions, and kill switches. They will not replace authentication, authorization, tenant isolation, permissions, assignments, workflow state, financial controls, document controls, agreement controls, occupancy controls, audit, or Responsible AI safeguards.

Production releases will pass required validation, use protected deployment authority, preserve release evidence, perform post-deployment health verification, and remain observable. High-risk releases will receive an explicit observation window.

Emergency changes may shorten ordinary process only to prevent greater harm. They will remain attributable, minimally scoped, tested proportionately, auditable, health verified, incident linked, and subject to post-change review.

No developer, browser, preview deployment, GitHub workflow, hosting platform, environment variable, feature flag, migration, worker, deployment credential, support operator, coding agent, provider, or AI model may silently expose secrets, connect non-production code to production data, bypass release controls, make destructive schema changes without review, grant authorization, alter immutable deployment artifacts, or obscure the source and outcome of a Habitacores production release.