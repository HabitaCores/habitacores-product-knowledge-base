# ADR-018 — Feature Flags, Dynamic Configuration, Progressive Delivery, and Kill-Switch Architecture

## Decision Record

| Field | Value |
|---|---|
| Product | Habitacores |
| ADR | ADR-018 |
| Version | 0.1 |
| Status | Proposed |
| Decision Type | Foundational Release, Runtime-Control, and Operational-Safety Architecture |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Evaluation Contract | OpenFeature-compatible provider-neutral abstraction |
| Initial Provider | Deferred |
| Primary Evaluation Location | Trusted server and worker boundaries |
| Client Evaluation | Presentation-only, using sanitized resolved values |
| Flag Configuration | Environment specific |
| Progressive Delivery | Controlled cohorts and deterministic percentage rollout |
| Tenant Targeting | Stable internal organization IDs only |
| Permission Authority | Prohibited |
| RLS Authority | Prohibited |
| Financial Authority | Prohibited |
| Agreement Authority | Prohibited |
| AI Policy Authority | Prohibited |
| Kill Switches | Approved for bounded operational capabilities |
| Change History | Immutable and auditable |
| Flag Ownership | Mandatory |
| Expiry and Removal | Mandatory |
| Failure Behavior | Explicit per flag; fail-safe or last-known-safe |
| Mobile Compatibility | Required |
| Last Updated | 1 August 2026 |

## 1. Context

Habitacores will release capabilities across:

- Habitacores Web
- Habitacores Mobile for Android
- Habitacores Mobile for iOS
- versioned APIs
- background workers
- notification delivery
- document processing
- payment integrations
- electronic-signature integrations
- Responsible AI operations
- organization-specific pilots
- internal administrative workflows

Deploying code and making a capability available are separate concerns.

Habitacores needs controlled mechanisms to:

- release unfinished code safely while keeping it inaccessible;
- enable pilots for selected organizations;
- roll out features gradually;
- disable failing providers or optional capabilities;
- coordinate web and mobile release timing;
- protect previous supported mobile versions;
- conduct internal testing;
- limit high-cost AI operations;
- suspend risky asynchronous processing;
- respond to incidents without an emergency code deployment where practical.

Without a governed feature-management architecture, Habitacores risks:

- flags becoming hidden authorization rules;
- inconsistent behavior across web, mobile, APIs, and workers;
- stale flags accumulating permanently;
- production changes without audit evidence;
- percentage rollouts assigning users inconsistently;
- client-controlled flag manipulation;
- preview flags affecting production;
- flags bypassing financial, legal, security, or tenant-isolation controls;
- provider failure disabling the entire application;
- mobile applications depending on flags they cannot evaluate safely;
- emergency kill switches silently corrupting authoritative workflows.

## 2. Decision Drivers

The decision is driven by the need to:

1. Separate deployment from release.
2. support gradual production rollout.
3. support organization-specific pilots.
4. coordinate web and mobile compatibility.
5. support operational kill switches.
6. preserve provider portability.
7. prevent feature flags from becoming authorization.
8. preserve tenant isolation.
9. preserve financial and legal invariants.
10. make flag changes traceable and auditable.
11. define safe failure behavior.
12. control configuration caching.
13. prevent stale flags.
14. support preview and staging isolation.
15. prevent uncontrolled targeting using personal data.
16. support workers and asynchronous processes.
17. support provider migrations.
18. constrain AI runtime configuration.
19. support measurable progressive delivery.
20. provide clear implementation boundaries for Codex.

## 3. Decision

Habitacores will implement a centralized, provider-neutral feature-management architecture using OpenFeature-compatible evaluation contracts.

The approved flow is:

```text
Approved flag definition
→ Environment-specific configuration
→ Authorized change
→ Configuration validation
→ Immutable change record
→ Flag-provider publication
→ Server or worker evaluation
→ Deterministic result
→ Observability and exposure record where required
→ Progressive rollout, continuation, rollback, or retirement
````

Application and domain code must depend on Habitacores feature contracts rather than directly on a provider SDK.

## 4. Architectural Boundary

The approved boundary is:

```text
Product and operational policy
→ Habitacores feature registry
→ OpenFeature-compatible evaluation abstraction
→ Approved provider or internal implementation
→ Web, API, worker, and mobile-safe resolved behavior
```

The feature-management provider does not become:

* the identity provider;
* the authorization system;
* the tenant-membership authority;
* the financial ledger;
* the agreement authority;
* the audit authority;
* the AI-governance authority.

## 5. Feature-Management Concepts

Habitacores will distinguish:

```text
FEATURE_DEFINITION
FEATURE_VARIANT
FEATURE_ENVIRONMENT_CONFIGURATION
TARGETING_RULE
SEGMENT
ROLLOUT
EVALUATION_CONTEXT
EVALUATION_RESULT
EXPOSURE_EVENT
KILL_SWITCH
DYNAMIC_OPERATIONAL_CONFIGURATION
FLAG_CHANGE
FLAG_RETIREMENT
```

These concepts must not be collapsed into scattered environment variables or arbitrary database booleans.

## 6. Feature Definition

Every feature definition must include:

* Stable key
* human-readable name
* description
* owner
* purpose
* flag type
* creation date
* environments
* default value
* failure behavior
* target audience
* expected lifetime
* expiry or review date
* telemetry requirement
* removal condition
* security classification
* related ADR, issue, or release
* mobile compatibility requirement where applicable

## 7. Flag Types

Approved directional flag types include:

```text
RELEASE
EXPERIMENT
OPERATIONAL
PERMISSION_ADJACENT_PRESENTATION
MIGRATION
PROVIDER_TRANSITION
KILL_SWITCH
MOBILE_COMPATIBILITY
AI_CONFIGURATION
```

The type affects review, targeting, retention, and failure behavior.

## 8. Release Flags

Release flags control whether deployed functionality is available.

Examples:

* New application workflow
* redesigned dashboard
* maintenance scheduling interface
* new document-upload experience
* new notification center

Release flags should normally be temporary and removed after rollout stabilizes.

## 9. Experiment Flags

Experiment flags may compare approved experience variants.

Experiments require:

* Defined hypothesis
* eligible population
* privacy review
* success metrics
* duration
* sample policy
* exclusion criteria
* analysis owner
* termination criteria

Experiments must not alter legal rights, financial outcomes, tenant access, or safety controls merely to measure engagement.

## 10. Operational Flags

Operational flags control bounded runtime behavior.

Examples:

* Worker concurrency
* provider routing
* non-critical retry settings
* optional notification fallback
* document-conversion availability
* maintenance mode
* synchronization interval

Operational flags require validated ranges and safe defaults.

## 11. Migration Flags

Migration flags may support temporary compatibility during:

* Database transitions
* API version changes
* provider replacement
* storage migration
* message-version migration
* algorithm transition

Migration flags must have explicit retirement conditions.

## 12. Provider-Transition Flags

A provider-transition flag may control:

* Percentage routed to a new provider
* selected organizations using a new provider
* fallback routing
* read-only shadow comparison
* rollback to an established provider

Provider routing must still preserve idempotency, reconciliation, and authoritative state.

## 13. Mobile-Compatibility Flags

Mobile-compatibility flags may account for:

* Android release availability
* iOS release availability
* minimum supported app version
* runtime compatibility
* store-review delay
* staged rollout
* over-the-air update compatibility

The backend must not expose a workflow to a client version that cannot safely complete it.

## 14. AI Configuration Flags

AI-related flags may control:

* Approved use-case availability
* provider routing
* model configuration
* token limits
* optional summarization
* cost ceilings
* fallback behavior
* human-review requirement escalation

Feature flags must not independently authorize an AI use case that has not passed the Responsible AI registry and approval process.

## 15. Kill Switches

Kill switches may disable bounded capabilities during:

* Provider outage
* security incident
* unsafe AI behavior
* document-processing defect
* notification-delivery defect
* payment-initialization problem
* electronic-signature provider failure
* uncontrolled resource consumption

A kill switch must define:

* Scope
* owner
* authorized operators
* activation condition
* expected user experience
* business-state effect
* recovery procedure
* observability
* audit
* maximum permitted duration

## 16. Kill-Switch Limitations

A kill switch may pause or disable an optional capability.

It must not:

* delete authoritative data;
* mark unfinished work completed;
* verify payments;
* allocate funds;
* fabricate receipts;
* activate agreements;
* activate occupancy;
* grant membership;
* grant permissions;
* bypass audit;
* bypass RLS;
* conceal legally required state;
* rewrite immutable history.

## 17. Feature Flags Are Not Authorization

A flag answers:

```text
Should this capability or variant be made available under the approved
release policy?
```

Authorization answers:

```text
May this authenticated actor perform this action on this resource
within this organization and current workflow state?
```

Both checks may be required.

A successful feature evaluation does not establish authorization.

## 18. Required Evaluation Sequence

For a protected operation, the directional sequence is:

```text
Authentication
→ Account status
→ Organization membership
→ Tenant context
→ Permission
→ Resource relationship
→ Workflow eligibility
→ Feature availability
→ Domain invariants
→ Authoritative transaction
```

The exact ordering may be optimized, but no feature evaluation may replace the security and domain checks.

## 19. No RLS Control by Feature Flag

PostgreSQL RLS policies must not depend on mutable third-party feature-provider availability.

Flags may affect which application workflow is offered, but RLS continues enforcing the approved tenant boundary independently.

## 20. No Financial Authority

Feature flags must not determine:

* Whether money was received
* payment amount
* currency
* verification status
* allocation
* refund completion
* reconciliation outcome
* receipt validity

A flag may enable a payment method or provider adapter, but ADR-016 remains authoritative.

## 21. No Agreement Authority

Feature flags must not determine:

* Whether an agreement was signed
* whether all parties completed execution
* signer authority
* agreement validity
* agreement activation
* termination

A flag may enable a signing provider or user interface, but ADR-017 remains authoritative.

## 22. Server-Side Evaluation

Security-sensitive and tenant-sensitive flags will be evaluated inside trusted server or worker boundaries.

This includes flags affecting:

* organization pilots;
* finance workflows;
* agreements;
* documents;
* privileged administration;
* AI operations;
* provider selection;
* internal operations;
* paid capabilities.

The browser or mobile application must not be trusted to supply the final flag result.

## 23. Client-Side Evaluation

Client-side evaluation may be used only for presentation behavior where manipulation does not grant protected access or alter authoritative state.

Examples:

* Layout variant
* onboarding illustration
* non-sensitive visual treatment
* informational banner

Every protected server operation still performs its own authoritative checks.

## 24. Mobile Evaluation

Mobile clients should ordinarily receive:

* a resolved capability document;
* approved sanitized flag values;
* or API responses reflecting server evaluation.

Mobile binaries must not receive:

* provider administrative keys;
* unrestricted flag-management credentials;
* hidden targeting rules containing sensitive data;
* tenant-wide configuration beyond their authorization.

## 25. Evaluation Context

Potential approved evaluation attributes include:

* Environment
* internal organization ID
* internal user ID where justified
* application platform
* application version
* API version
* deployment version
* role category where non-authoritative
* approved cohort
* provider region
* synthetic-account marker
* staff or internal-testing marker

Evaluation context must be minimized.

## 26. Prohibited Evaluation Attributes

Flags must not ordinarily target using:

* Name
* email address
* phone number
* street address
* government identifier
* payment reference
* agreement contents
* document filename
* raw application answers
* health information
* protected personal attributes
* unrestricted AI prompt content

Internal identifiers may be used only where justified and access controlled.

## 27. Organization Targeting

Selected-organization pilots must use stable internal organization IDs.

Organization display names must not be hardcoded into application logic or provider rules.

Organization targeting does not override:

* Membership
* permissions
* resource ownership
* subscription or contractual eligibility
* legal requirements

## 28. User Targeting

User-specific targeting should be rare and purpose limited.

Appropriate uses may include:

* Internal testers
* designated pilot users
* support reproduction accounts
* accessibility testing

The architecture should prefer durable cohorts or segments rather than extensive user-by-user rules.

## 29. Segments

A segment represents a reusable targeting cohort.

Potential segments include:

```text
INTERNAL_STAFF
PILOT_ORGANIZATIONS
ANDROID_BETA
IOS_BETA
PROPERTY_MANAGER_PILOT
SYNTHETIC_MONITORING
```

Segment membership must have:

* Owner
* purpose
* source
* environment
* effective date
* expiry
* review process

## 30. Percentage Rollouts

Percentage rollouts must be deterministic.

The same evaluation subject should ordinarily receive the same variant throughout a rollout unless:

* The rollout changes;
* the flag is reset intentionally;
* the subject’s approved evaluation key changes;
* a migration requires reassignment.

Random evaluation on every request is prohibited.

## 31. Rollout Evaluation Key

A percentage rollout may use an approved stable key such as:

* Organization ID for organization-coherent rollout
* User ID for user-level rollout
* Installation ID for selected presentation experiments

The chosen key must match the desired consistency boundary.

For workflows requiring organization-wide consistency, user-level randomization may be inappropriate.

## 32. Progressive Delivery

A progressive rollout may proceed through:

```text
INTERNAL
→ SYNTHETIC
→ SELECTED_ORGANIZATIONS
→ SMALL_PERCENTAGE
→ EXPANDED_PERCENTAGE
→ GENERAL_AVAILABILITY
```

Each stage should define:

* Entry criteria
* duration
* monitored metrics
* acceptable error rate
* rollback threshold
* responsible owner
* approval requirement

## 33. Rollout Observation

A rollout owner must review relevant signals, including:

* Error rate
* latency
* authorization denials
* tenant-isolation anomalies
* business completion rate
* worker backlog
* provider errors
* mobile crash rate
* document failures
* payment discrepancies
* agreement failures
* AI validation and safety failures
* support volume

## 34. Automatic Rollout

Automatic percentage expansion is deferred.

Initial progressive delivery should use controlled human-authorized stages.

Automation may be introduced later only with:

* Reliable metrics
* rollback rules
* ownership
* bounded scope
* protection against noisy data
* auditability

## 35. Flag Values

Flags may return:

```text
BOOLEAN
STRING
INTEGER
DECIMAL_CONFIGURATION
JSON_CONFIGURATION
VARIANT
```

Complex values require explicit schemas.

Authoritative monetary values must not be represented using unrestricted feature-flag values.

## 36. Typed Evaluation

Flag evaluations must use typed contracts.

Directional interface:

```typescript
interface FeatureEvaluationService {
  getBoolean(
    key: FeatureKey,
    defaultValue: boolean,
    context: EvaluationContext
  ): Promise<FeatureEvaluation<boolean>>;

  getString(
    key: FeatureKey,
    defaultValue: string,
    context: EvaluationContext
  ): Promise<FeatureEvaluation<string>>;
}
```

Domain code must not parse arbitrary provider payloads.

## 37. Flag Registry

The monorepo should maintain a typed registry.

Directional structure:

```text
packages/
  feature-management/
    registry/
    evaluation/
    providers/
    context/
    telemetry/
    testing/
```

Each known flag key must be declared centrally.

## 38. Dynamic Operational Configuration

Dynamic configuration differs from ordinary release flags.

It may include:

* Worker concurrency
* provider timeout
* retry limit
* rate limit
* circuit-breaker threshold
* AI token ceiling
* temporary maintenance window
* safe page-size limit

Every value requires:

* Schema
* minimum
* maximum
* unit
* owner
* default
* failure behavior
* environment
* audit
* rollout or effective time

## 39. Unsafe Dynamic Configuration

Dynamic configuration must not directly define:

* Unreviewed rent amounts
* arbitrary payment balances
* refund eligibility
* agreement clauses
* permission grants
* RLS predicates
* encryption keys
* authentication secrets
* migration SQL
* legal-retention periods without approval

## 40. Environment Isolation

Feature configuration must remain environment specific.

Directional environments:

```text
LOCAL
TEST
PREVIEW
STAGING
PRODUCTION
```

Enabling a flag in preview must not enable it in production.

Production configuration must not be copied automatically from non-production.

## 41. Preview Behavior

Preview environments should support deterministic local or preview configuration.

They must not require production flag-management credentials.

Preview flags should be visibly distinguishable from production behavior.

## 42. Default Values

Every flag evaluation requires an explicit code-level default.

Defaults must be safe if:

* Provider initialization fails
* network access fails
* configuration is missing
* a flag key is unknown
* a value has the wrong type
* evaluation times out

A missing flag must not silently enable risky behavior.

## 43. Failure Modes

Each flag must specify one of the approved failure strategies:

```text
FAIL_CLOSED
FAIL_OPEN_FOR_NON_CRITICAL_PRESENTATION
LAST_KNOWN_SAFE
STATIC_DEFAULT
DEGRADE_TO_MANUAL_WORKFLOW
```

The strategy must reflect the capability’s risk.

## 44. Fail-Closed

Fail-closed is appropriate when enabling the capability could create:

* Security exposure
* tenant leakage
* unsafe payment behavior
* unauthorized agreement behavior
* unreviewed AI action
* destructive processing

Failure results in the protected capability remaining unavailable.

## 45. Last-Known-Safe

Last-known-safe may be appropriate where:

* Sudden disablement would interrupt an established safe workflow.
* cached configuration is integrity protected.
* staleness is bounded.
* expiry is defined.
* the fallback remains compatible with deployed code.

Last-known-safe must not persist indefinitely without visibility.

## 46. Manual Workflow Degradation

Some automated capabilities should degrade to a deterministic manual workflow.

Examples:

* AI summarization disabled → manual review remains available
* signature provider unavailable → signing remains pending
* automated reconciliation unavailable → finance review queue remains available
* document conversion unavailable → original file remains quarantined or accessible under policy

## 47. Evaluation Caching

Flag evaluation may use:

* In-process cache
* provider SDK cache
* edge-safe cache for approved presentation flags
* worker-local cache
* last-known-safe store

Caching must specify:

* TTL
* invalidation
* consistency expectation
* stale behavior
* environment
* failure behavior

## 48. Cache Consistency

Habitacores must not assume all services observe a flag change simultaneously.

Changes affecting multiple services require compatibility during propagation.

Examples include:

* Web and worker versions
* mobile and API behavior
* producer and consumer message versions
* provider transitions

## 49. Real-Time Flag Updates

Real-time update streams may be used when supported, but the architecture must remain safe when updates are delayed.

Critical emergency actions should include verification that relevant services received or evaluated the change.

## 50. Flag Change Authority

Production flag changes require an authorized role.

Potential roles include:

```text
FEATURE_OWNER
RELEASE_MANAGER
INCIDENT_COMMANDER
PLATFORM_OPERATOR
SECURITY_OPERATOR
```

Role names and permissions will be finalized through the authorization model.

## 51. Separation of Duties

High-risk flag changes should require additional review where staffing permits.

Examples:

* Enabling production payments
* changing payment-provider routing
* activating agreement execution
* enabling AI for a new use case
* disabling a security control
* enabling a new data export
* changing a critical kill switch

Founding-stage exceptions must remain explicit and audited.

## 52. Change Request

A material flag change should include:

* Flag
* environment
* previous value
* proposed value
* purpose
* affected audience
* expected duration
* related release or incident
* validation evidence
* rollback condition
* requester
* approver where required
* execution time

## 53. Change Audit

Material audit events may include:

```text
FEATURE_FLAG_CREATED
FEATURE_FLAG_CONFIGURATION_CHANGED
FEATURE_ROLLOUT_STARTED
FEATURE_ROLLOUT_EXPANDED
FEATURE_ROLLOUT_PAUSED
FEATURE_ROLLOUT_ROLLED_BACK
KILL_SWITCH_ACTIVATED
KILL_SWITCH_DEACTIVATED
FEATURE_FLAG_RETIRED
DYNAMIC_CONFIGURATION_CHANGED
```

Audit records must not contain provider secrets.

## 54. Provider Webhooks and Change Feeds

Where a provider offers change notifications, incoming events must be:

* Authenticated
* environment checked
* schema validated
* deduplicated
* monitored
* reconciled with expected configuration

Provider callbacks must not bypass Habitacores change authority.

## 55. Configuration Reconciliation

Habitacores should periodically reconcile:

```text
Approved Habitacores flag registry
↔ Provider definitions
↔ Environment configurations
↔ Application references
↔ Ownership and expiry records
```

Potential discrepancies include:

```text
UNKNOWN_PROVIDER_FLAG
MISSING_PROVIDER_FLAG
UNREGISTERED_APPLICATION_REFERENCE
EXPIRED_FLAG_ACTIVE
OWNER_MISSING
ENVIRONMENT_DRIFT
TYPE_MISMATCH
DEFAULT_MISMATCH
```

## 56. Observability

Feature evaluation telemetry should include bounded fields such as:

* Flag key
* variant
* environment
* application
* platform
* evaluation reason
* provider
* error class
* cache status
* release version

Organization IDs and user IDs must not become unrestricted metric labels.

## 57. Exposure Events

An exposure event may be recorded where the system needs to know that a user or organization actually experienced a variant.

Exposure should be recorded only when:

* Required for rollout analysis;
* required for experiment analysis;
* bounded by privacy policy;
* deduplicated appropriately;
* linked to an approved purpose.

An evaluation that is never presented or used may not qualify as meaningful exposure.

## 58. Metrics

Priority metrics may include:

```text
feature_evaluations
feature_evaluation_errors
feature_provider_timeouts
feature_cache_hits
feature_cache_stale
feature_unknown_keys
feature_type_mismatches
feature_rollout_exposures
kill_switch_activations
expired_flags_active
```

## 59. Alerts

Priority alerts may include:

* Feature-provider outage
* critical evaluation-error spike
* missing production default
* unknown production flag
* stale last-known-safe cache
* unauthorized flag-change attempt
* production flag changed from preview credentials
* kill switch activated without incident reference
* expired flag still active
* finance or agreement flag evaluated client side
* environment-configuration drift

## 60. Deployment Relationship

Code deployment and feature release remain separate but coordinated.

A safe rollout generally requires:

```text
Deploy code with feature disabled
→ Verify compatibility
→ Enable internally
→ Observe
→ Expand rollout
→ Reach general availability
→ Remove obsolete code and flag
```

## 61. Database Migration Relationship

A feature flag must not make an incompatible database migration safe by itself.

Database changes must still follow ADR-004 and ADR-013.

A common sequence is:

```text
Expand schema
→ Deploy compatible code
→ Enable new behavior gradually
→ Migrate data
→ Confirm old behavior unused
→ Remove flag
→ Contract schema in a later release
```

## 62. Worker Compatibility

Flags controlling asynchronous workflows must account for:

* Existing outbox messages
* jobs already claimed
* old worker versions
* new worker versions
* retries
* terminal failures
* message-version compatibility

Disabling a producer does not automatically cancel already committed work.

## 63. Payment Feature Flags

Payment flags may control:

* Visibility of a payment method
* provider routing for new attempts
* sandbox pilot
* selected-organization enablement
* temporary suspension of new initialization

They must not change already verified payment facts or silently reroute an existing attempt without idempotency and reconciliation controls.

## 64. Agreement Feature Flags

Agreement flags may control:

* Availability of a signature method
* selected template pilot
* new provider routing
* mobile signing interface
* reminder delivery

They must not alter issued agreement content, signer assignments, collected signatures, or execution evidence.

## 65. Communication Feature Flags

Communication flags may control:

* Optional channel availability
* provider routing
* template pilot
* fallback behavior
* non-essential communication suspension

They must not bypass consent, suppression, legal-delivery rules, or authoritative communication evidence.

## 66. AI Feature Flags

AI flags may control only registered and approved AI capabilities.

They must preserve:

* Use-case registration
* data-classification rules
* permission-aware retrieval
* prompt and model registry
* schema validation
* human review
* audit
* cost limits
* manual fallback

A feature flag cannot convert AI output into authoritative state.

## 67. Mobile Release Safety

Before enabling a feature that requires a new mobile capability, the rollout must confirm:

* Required Android version available
* required iOS version available
* supported API contract
* minimum-version policy
* store rollout status
* backward compatibility
* deep-link compatibility
* safe behavior for older clients

## 68. Flag Lifecycle

Directional lifecycle:

```text
PROPOSED
DEVELOPMENT
INTERNAL
PILOT
PROGRESSIVE_ROLLOUT
GENERAL_AVAILABILITY
RETIRING
RETIRED
```

Not every operational flag follows the same lifecycle, but every flag requires an explicit state.

## 69. Expiry

Temporary flags require an expiry or review date.

At expiry, the system should:

* Alert the owner
* prevent unnoticed indefinite extension
* require review
* create cleanup work
* identify the intended permanent behavior

## 70. Flag Retirement

Retirement requires:

1. Determine the permanent variant.
2. remove alternative code paths.
3. remove targeting rules.
4. remove provider configuration.
5. remove registry entry where appropriate.
6. remove obsolete tests.
7. preserve historical audit and release evidence.
8. verify no mobile or worker version still requires the flag.

## 71. Stale-Flag Control

CI should identify:

* Flags past expiry
* source references to retired flags
* flags without owners
* registry flags with no source usage
* source flag keys absent from the registry
* flags with missing defaults
* flags without test coverage

## 72. Testing

The architecture requires:

* Default-value tests
* provider-failure tests
* type-mismatch tests
* deterministic-rollout tests
* cross-environment tests
* organization-targeting tests
* client-manipulation tests
* authorization-independence tests
* RLS-independence tests
* cache-staleness tests
* mobile-compatibility tests
* worker-propagation tests
* kill-switch tests
* retirement tests
* AI-boundary tests

## 73. Authorization-Independence Test

```text
Given:
A finance feature is enabled for Organization A.

And:
A user lacks the required finance permission.

When:
The user attempts the protected operation.

Then:
The server denies the operation.

And:
The enabled flag does not grant authority.
```

## 74. Tenant-Isolation Test

```text
Given:
A feature is enabled for Organization A.

And:
A user belongs only to Organization B.

When:
The user attempts to access Organization A resources.

Then:
Application authorization denies access.

And:
PostgreSQL RLS prevents retrieval or mutation.
```

## 75. Deterministic-Rollout Test

```text
Given:
A feature is enabled for 10 percent of organizations.

When:
The same organization is evaluated repeatedly across compatible services.

Then:
It receives a stable result for the rollout configuration.

And:
The result does not change randomly on each request.
```

## 76. Provider-Failure Test

```text
Given:
The feature-management provider is unavailable.

When:
A critical AI capability is evaluated.

Then:
The approved fail-closed or manual-fallback behavior applies.

And:
The system does not silently enable the AI capability.
```

## 77. Client-Manipulation Test

```text
Given:
A mobile user modifies a locally cached flag value.

When:
The user invokes a protected server operation.

Then:
The server performs its own evaluation and authorization.

And:
The modified local value cannot grant access or alter authoritative state.
```

## 78. Payment Kill-Switch Test

```text
Given:
New payment initialization is disabled through an authorized kill switch.

When:
A user attempts to create a new provider collection.

Then:
The system returns a controlled unavailable outcome.

And:
Existing verified payments, allocations, receipts, and reconciliation records
remain unchanged.
```

## 79. Worker Kill-Switch Test

```text
Given:
A document-conversion worker capability is disabled.

When:
A new conversion job becomes eligible.

Then:
The job remains safely pending or enters the approved controlled state.

And:
The system does not falsely mark the conversion completed.
```

## 80. Mobile Compatibility Test

```text
Given:
A new workflow requires mobile version 2.0.

And:
A user runs supported mobile version 1.9.

When:
The capability is evaluated.

Then:
The backend does not expose an incompatible workflow.

And:
The user receives the approved fallback or upgrade guidance.
```

## 81. Codex Boundaries

Every Codex feature-flag work package must specify:

* Flag key
* flag type
* owner
* purpose
* environment
* default
* evaluation location
* context attributes
* target population
* failure strategy
* expiry
* rollout stages
* telemetry
* cleanup condition
* authorization independence
* RLS independence
* tests

Codex must not:

* Create undeclared string flag keys throughout the codebase
* use flags as permissions
* use flags to bypass RLS
* evaluate protected finance or agreement flags only in clients
* expose provider administrative credentials
* use personal data as targeting attributes without approval
* default risky functionality to enabled
* create random percentage assignment per request
* permit preview configuration to affect production
* hide feature changes from audit
* leave temporary flags without expiry
* use flags to alter immutable financial or agreement history
* let flags bypass Responsible AI controls
* assume disabling a producer cancels durable queued work

## 82. Prohibited Patterns

The following are not approved:

* Feature flags as role assignments
* feature flags as organization membership
* feature flags as RLS policy
* feature flags as payment verification
* feature flags as agreement execution
* feature flags as legal approval
* unrestricted client-side protected evaluation
* provider SDK calls throughout domain code
* production flag keys absent from a registry
* unsafe enable-by-default behavior
* personal emails used as long-term production targeting rules
* random reassignment on every request
* environment-shared production configuration
* unowned or expiry-free temporary flags
* kill switches that rewrite historical records
* flags permanently masking dead code

## 83. Provider Selection

The initial provider remains deferred.

Potential implementation directions include:

* Vercel feature-management tooling
* an OpenFeature-compatible managed provider
* an OpenFeature-compatible self-hosted provider
* an initial Habitacores-owned PostgreSQL-backed implementation
* a hybrid provider abstraction

Evaluation must consider:

* OpenFeature compatibility
* Next.js support
* Node.js worker support
* React Native and mobile delivery requirements
* environment separation
* targeting
* deterministic percentage rollout
* audit history
* approvals
* API quality
* outage behavior
* local development
* caching
* data residency
* security
* pricing
* export capability
* provider exit

## 84. Options Considered

### Option A — Environment Variables Only

Advantages:

* Simple
* already supported by deployment platforms
* appropriate for static configuration

Disadvantages:

* Requires redeployment
* weak targeting
* weak progressive rollout
* poor mobile coordination
* limited change history
* unsuitable for rapid kill switches

Decision:

Rejected as the complete architecture.

### Option B — Provider-Specific SDK Throughout the Codebase

Advantages:

* Fast access to provider capabilities
* minimal abstraction work

Disadvantages:

* Vendor lock-in
* inconsistent evaluation semantics
* difficult provider migration
* provider types leak into domain code
* harder testing

Decision:

Rejected.

### Option C — Ad Hoc PostgreSQL Booleans

Advantages:

* Direct control
* simple persistence
* no external provider

Disadvantages:

* No standard evaluation API
* weak lifecycle governance
* limited rollout and segmentation
* inconsistent caching
* likely authorization confusion

Decision:

Rejected as an ungoverned pattern.

### Option D — OpenFeature-Compatible Habitacores Feature Architecture

Advantages:

* Provider portability
* typed evaluation
* consistent web and worker semantics
* controlled mobile delivery
* progressive rollout
* kill switches
* explicit defaults
* lifecycle governance
* strong testing boundary

Disadvantages:

* Requires central registry
* provider integration still required
* caching and propagation require design
* flag cleanup requires discipline
* OpenFeature compatibility does not eliminate provider-specific operational differences

Decision:

Approved.

## 85. Positive Consequences

* Deployment and release become independently controllable.
* risky capabilities can be introduced gradually.
* provider-specific code remains isolated.
* mobile and API compatibility can be coordinated.
* production incidents gain bounded kill switches.
* flag changes become observable and auditable.
* environment isolation becomes explicit.
* stale flags receive ownership and expiry controls.
* authorization and tenant isolation remain independent.
* AI, payment, agreement, and document capabilities retain their governing controls.

## 86. Negative Consequences

* Feature management adds operational complexity.
* multiple code paths increase testing burden.
* provider outages require fallback behavior.
* stale flags require active cleanup.
* service-level propagation may not be instantaneous.
* mobile compatibility increases rollout planning.
* organization targeting creates privacy and cardinality considerations.

## 87. Risks

### 87.1 Flags Becoming Authorization

Controls:

* Explicit prohibition
* separate authorization service
* server-side protected checks
* tests
* code review
* RLS independence

### 87.2 Provider Outage

Controls:

* Explicit defaults
* caching
* last-known-safe where appropriate
* fail-closed for high-risk capabilities
* manual fallback
* observability

### 87.3 Stale Flags

Controls:

* Ownership
* expiry
* CI checks
* registry reconciliation
* cleanup requirements
* periodic review

### 87.4 Cross-Environment Change

Controls:

* Environment-specific credentials
* protected production access
* change audit
* configuration reconciliation
* preview isolation

### 87.5 Inconsistent Rollout

Controls:

* Stable evaluation keys
* deterministic hashing
* shared evaluation contract
* compatibility testing
* bounded cache behavior

### 87.6 Kill-Switch Misuse

Controls:

* Scoped permissions
* impact definition
* audit
* incident relationship
* activation runbook
* no historical mutation

## 88. Implementation Sequence

```text
1. Define feature-management taxonomy and naming convention
2. create the typed feature registry
3. implement OpenFeature-compatible evaluation contracts
4. establish local and test providers
5. define evaluation-context allowlists
6. define explicit defaults and failure strategies
7. implement server and worker evaluation
8. implement safe mobile capability delivery
9. implement environment-specific configuration
10. implement audit and change records
11. implement deterministic percentage rollout
12. implement organization and cohort targeting
13. implement caching and last-known-safe controls
14. implement bounded kill switches
15. implement evaluation telemetry and dashboards
16. implement stale-flag and registry CI checks
17. implement progressive-delivery runbooks
18. evaluate production providers
19. select and approve the initial provider
20. migrate ADR-013 temporary flag concepts into this governed architecture
```

## 89. Deferred Decisions

This ADR does not decide:

* Initial production feature-management provider
* exact OpenFeature SDK version
* exact provider adapter
* exact administrative user interface
* exact approval count for production changes
* exact rollout percentages
* exact rollout observation windows
* exact cache TTLs
* exact experiment-analysis platform
* exact exposure-event retention
* exact automated rollout policy
* exact dynamic-configuration storage
* exact provider data region
* exact feature entitlement and subscription architecture
* exact pricing-plan architecture
* exact mobile capability-document format

These require bounded implementation decisions.

## 90. Supersession Conditions

This ADR should be reviewed when:

* Enterprise customers require customer-managed flags.
* regional configuration isolation becomes mandatory.
* configuration throughput exceeds the provider architecture.
* mobile clients require offline authoritative configuration.
* a dedicated experimentation platform becomes necessary.
* feature entitlements become a formal commercial product.
* automated progressive delivery becomes a core requirement.
* OpenFeature no longer meets portability requirements.
* a selected provider cannot meet audit or availability requirements.

## 91. Final Decision Statement

Habitacores will implement a centralized, provider-neutral feature-management architecture using OpenFeature-compatible evaluation contracts.

Feature flags will separate code deployment from controlled product release. They may support internal testing, selected-organization pilots, progressive rollout, mobile compatibility, provider migration, bounded experiments, operational configuration, and emergency kill switches.

Feature flags will not grant authentication, organization membership, permissions, resource ownership, financial authority, agreement authority, occupancy authority, document access, support authority, legal authority, or Responsible AI approval.

Protected evaluations will occur in trusted server or worker boundaries. Browser and mobile values may control presentation but will not independently authorize protected operations or alter authoritative state.

Every feature will have a registered key, owner, purpose, type, environment, default, failure strategy, audience, expected lifetime, expiry or review date, telemetry requirement, and removal condition.

Production configuration will remain environment specific. Preview, development, test, and staging configurations will not affect production.

Percentage rollouts will use stable, deterministic evaluation keys appropriate to the desired consistency boundary. Organization-wide workflows will not be randomly divided at user level where that would create inconsistent business behavior.

Every evaluation will have an explicit safe default. High-risk capabilities will fail closed or degrade to deterministic manual workflows. Last-known-safe configuration may be used only with bounded staleness, integrity controls, observability, and compatibility.

Kill switches may pause bounded capabilities, provider usage, or optional automation. They will not delete records, fabricate completion, verify payments, allocate funds, issue receipts, execute agreements, activate occupancy, grant access, bypass audit, or rewrite immutable history.

Feature changes will be authorized, validated, observable, environment scoped, and auditable. High-risk production changes will receive additional review where staffing and tooling permit.

Temporary flags will have expiry and retirement requirements. General availability will be followed by removal of obsolete code paths, provider rules, registry entries, and tests after supported web, worker, Android, and iOS versions no longer depend on the flag.

AI-related flags will remain subordinate to the approved AI use-case, provider, model, prompt, safety, permission, validation, human-review, audit, and cost controls.

No developer, browser, mobile application, provider, organization user, worker, support operator, feature-management administrator, coding agent, or AI model may use a feature flag to bypass Habitacores authentication, tenant isolation, authorization, RLS, financial controls, agreement controls, document controls, audit, release governance, or Responsible AI safeguards.