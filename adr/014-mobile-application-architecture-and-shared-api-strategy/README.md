# ADR-014 — Mobile Application Architecture and Shared API Strategy

## Decision Record

| Field | Value |
|---|---|
| Product | Habitacores |
| Decision | Mobile Application Architecture and Shared API Strategy |
| ADR | ADR-014 |
| Version | 0.1 |
| Status | Proposed |
| Decision Type | Foundational Client, API, and Mobile Architecture |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Mobile Framework | React Native with Expo |
| Mobile Navigation | Expo Router |
| Mobile Platforms | Android and iOS |
| Mobile Codebase | Shared TypeScript application |
| API Boundary | Versioned Habitacores HTTPS API |
| Initial API Runtime | Next.js Route Handlers |
| Shared Backend | Habitacores application and domain services |
| Database Access From Mobile | Prohibited |
| Storage Administrative Access From Mobile | Prohibited |
| Mobile Credential Storage | Approved operating-system secure storage |
| Offline Model | Cached reads and controlled drafts |
| Offline Authoritative Decisions | Prohibited |
| API Compatibility | Backward compatible during supported mobile-version windows |
| Release Strategy | Controlled Android and iOS builds, staged distribution, and monitored rollout |
| Production Domain | `habitacores.com` |
| Initial API Domain Direction | `api.habitacores.com` or an approved equivalent |
| Last Updated | 1 August 2026 |

## 1. Context

Habitacores is being designed as a multi-channel product comprising:

- Habitacores Web
- Habitacores Mobile for Android
- Habitacores Mobile for iOS
- shared backend services
- shared PostgreSQL data
- shared authentication
- shared organization and tenant isolation
- shared business rules
- shared document services
- shared payment workflows
- shared audit history
- shared notifications
- shared Responsible AI capabilities

The mobile application will serve users such as:

- Prospects
- applicants
- guarantors
- residents
- landlords
- property owners
- agents
- property managers
- maintenance personnel
- vendors
- organization administrators
- finance reviewers
- legal professionals
- support personnel where specifically approved

Mobile devices create additional architectural concerns, including:

- Intermittent network connectivity
- application-version fragmentation
- secure credential storage
- device compromise
- background execution restrictions
- push notifications
- deep links
- camera and document capture
- local caching
- offline drafts
- mobile-store releases
- delayed upgrades
- platform-specific permissions
- accessibility
- low-bandwidth usage
- Android and iOS behavioral differences

Habitacores must support mobile experiences without duplicating or weakening its authoritative backend rules.

## 2. Decision Drivers

The decision is driven by the need to:

1. Deliver Android and iOS applications efficiently.
2. maintain one primary mobile codebase.
3. preserve native mobile capabilities.
4. reuse TypeScript knowledge and tooling.
5. share contracts and domain terminology with the web platform.
6. keep PostgreSQL and privileged services inaccessible from mobile clients.
7. enforce server-side authorization.
8. support mobile release-version fragmentation.
9. support intermittent connectivity.
10. protect session and refresh credentials.
11. support secure document capture and upload.
12. support push notifications and deep links.
13. preserve tenant isolation.
14. preserve financial, agreement, and occupancy controls.
15. preserve audit and Responsible AI boundaries.
16. support future migration of backend runtime independently of mobile clients.
17. minimize duplicated business logic.
18. avoid forced UI sharing where platform needs differ.
19. provide measurable mobile reliability.
20. provide clear Codex implementation boundaries.

## 3. Decision

Habitacores will build its Android and iOS applications using React Native with Expo.

Expo Router will provide mobile routing, nested navigation, deep-link mapping, and platform-specific route behavior.

The mobile application will communicate with Habitacores through a versioned HTTPS API.

The mobile client will not connect directly to:

- Authoritative PostgreSQL tables
- PostgreSQL administrative functions
- database service roles
- storage administrative APIs
- migration infrastructure
- internal worker endpoints
- unrestricted AI providers
- secrets-management systems

## 4. Product Surfaces

The approved product surfaces are:

```text
Habitacores Web
Habitacores Mobile — Android
Habitacores Mobile — iOS
Habitacores API
Habitacores Background Workers
Habitacores Administrative Operations
````

The three user-facing clients must use the same authoritative backend rules.

## 5. Mobile Framework

React Native with Expo is selected because it provides:

* One TypeScript-oriented mobile project
* Android and iOS delivery
* native-device capabilities
* managed build and release tooling
* routing integration
* development builds
* platform-specific modules where required
* compatibility with the existing TypeScript monorepo

The mobile application must use the React Native architecture supported by the selected Expo release.

## 6. Expo Router

Expo Router will manage mobile navigation.

It may support:

* Authentication flows
* organization selection
* property browsing
* application workflows
* payment views
* document capture
* agreement access
* occupancy views
* maintenance workflows
* notifications
* deep links
* modal and nested navigation

Route structure must not become authorization.

A user reaching a route does not establish permission to access its data or execute its action.

## 7. Platform-Specific Behavior

Android and iOS may require different implementations for:

* Permissions
* notification behavior
* file selection
* camera access
* background tasks
* secure storage
* biometric prompts
* deep-link handling
* store requirements
* accessibility behavior

Platform-specific modules are approved where they preserve a consistent Habitacores business outcome.

Platform divergence must not create different authorization or financial rules.

## 8. Monorepo Placement

Directional repository structure:

```text
apps/
  web/
  mobile/
  worker-notifications/
  worker-documents/
  worker-integrations/
  worker-ai/

packages/
  api-contracts/
  validation/
  domain/
  application/
  identity/
  authorization/
  money/
  documents/
  audit/
  observability/
  design-tokens/
  mobile-platform/
```

Exact names may be refined.

## 9. Sharing Strategy

Web and mobile should share:

* API contracts
* schemas
* identifiers
* Money types
* date and time conventions
* status enumerations
* error codes
* domain terminology
* permission identifiers
* feature-capability identifiers
* telemetry conventions
* selected design tokens
* pure TypeScript utilities

They should not automatically share:

* Next.js Server Components
* Next.js Server Actions
* React Native screens
* DOM-specific components
* native-device modules
* storage implementations
* routing implementations
* browser-only behavior

## 10. Shared UI Policy

The architecture will prioritize shared design language over forced universal UI components.

Shared design resources may include:

* Color tokens
* typography tokens
* spacing tokens
* icon conventions
* status treatments
* form semantics
* accessibility standards
* content terminology

Framework-specific presentation components may remain separate.

## 11. Backend Authority

The Habitacores backend remains authoritative for:

* Authentication validation
* account status
* organization membership
* permissions
* assignments
* tenant isolation
* workflow state
* financial calculations
* payment verification
* allocation
* agreement issuance
* occupancy activation
* audit
* document access
* Responsible AI policy

No mobile calculation or local state may redefine these outcomes.

## 12. API Boundary

Mobile clients will use an explicit HTTPS API.

The API should be reachable through a stable production domain such as:

```text
api.habitacores.com
```

or another approved route under:

```text
habitacores.com
```

The exact deployment topology is deferred.

## 13. Initial API Runtime

The initial API may be implemented through Next.js Route Handlers.

Route Handlers should delegate to shared Habitacores application services.

They must not contain duplicated domain policy.

Directional flow:

```text
Mobile request
→ Route Handler
→ Request parsing
→ Authentication
→ Internal user resolution
→ Organization resolution
→ Authorization
→ Application service
→ Database transaction
→ Audit and outbox
→ API response
```

## 14. API Portability

The API contract must remain portable if Habitacores later moves selected endpoints to:

* Dedicated Node.js services
* containerized services
* region-specific services
* mobile-specific backend-for-frontend services
* independent integration services

Mobile clients must depend on the published API contract rather than internal Next.js modules.

## 15. API Style

The initial API will use resource-oriented HTTPS and JSON contracts.

Supported operation patterns may include:

```text
GET
POST
PUT
PATCH
DELETE
```

Not every domain command must map mechanically to generic CRUD.

High-value actions should use explicit command endpoints where clearer.

Examples:

```text
POST /v1/applications/{applicationId}/submit
POST /v1/payments/{paymentId}/verify
POST /v1/payments/{paymentId}/allocations
POST /v1/agreements/{agreementId}/activate
POST /v1/occupancies/{occupancyId}/activate
```

## 16. API Versioning

The initial public mobile API will use an explicit major-version boundary.

Directional form:

```text
/v1/
```

Backward-compatible additions may remain within the same major version.

Breaking changes require:

* New API version
* migration path
* consumer analysis
* mobile-release coordination
* deprecation period
* observability
* retirement criteria

## 17. Mobile-Version Fragmentation

Habitacores must assume users will not update mobile applications immediately.

The backend may temporarily serve:

* Current mobile release
* previous supported release
* staged rollout release
* review-store release

The API cannot assume all users run the newest client.

## 18. Minimum Supported Version

Habitacores may define:

```text
minimum_supported_version
recommended_version
latest_version
```

A client below the minimum may receive a controlled upgrade requirement.

The requirement must not strand users from urgent access without a product and security review.

## 19. Compatibility Window

Every breaking backend change must consider:

* App Store review delay
* Google Play rollout delay
* users with automatic updates disabled
* staged distribution
* low-connectivity users
* emergency rollback

Backward compatibility is required throughout the supported-client window.

## 20. API Contract Registry

The API architecture should maintain:

* Endpoint
* version
* request schema
* response schema
* authentication requirement
* permission
* organization scope
* idempotency requirement
* error codes
* pagination
* rate limit
* audit behavior
* deprecation status

## 21. Contract Generation

Shared schemas may generate or validate:

* TypeScript client types
* server request validation
* response validation
* OpenAPI documentation
* mock clients
* integration tests

Generated types do not replace runtime validation.

## 22. Runtime Validation

Every mobile API request must be validated server side.

Validation must cover:

* Path parameters
* query parameters
* headers
* request body
* identifier format
* Money representation
* dates
* enumerations
* payload limits
* unknown fields where policy requires rejection

## 23. Response Validation

High-risk API responses should also be validated against approved schemas.

This reduces contract drift between backend and mobile clients.

## 24. Authentication

Mobile authentication will use Supabase Auth through the approved identity-provider architecture established by ADR-007.

The mobile flow must resolve:

```text
Provider identity
→ Habitacores internal user
→ account status
→ organization membership
→ authorization
```

Provider authentication does not establish organization authority.

## 25. Authorization Code Flow

OAuth or federated authentication on mobile should use secure authorization-code flows with PKCE where applicable.

Embedded long-lived provider secrets are prohibited.

## 26. Mobile Session Storage

Refresh credentials and sensitive session material must use approved operating-system secure storage.

They must not be stored in:

* Plain application preferences
* ordinary local storage
* unencrypted SQLite tables
* logs
* crash reports
* analytics
* clipboard
* source code

## 27. Access Tokens

Access tokens should be:

* Short lived
* sent only over HTTPS
* removed from logs
* refreshed through approved mechanisms
* invalidated or denied when account state requires it
* treated as authentication evidence rather than final authorization

## 28. Logout

Mobile logout must:

* Terminate or revoke the applicable provider session
* clear local session material
* clear protected caches
* clear organization context
* stop protected background synchronization
* preserve only approved non-sensitive preferences

## 29. Device Registration

Habitacores may maintain an application-level device registration containing:

* Internal device-registration ID
* user ID
* installation ID
* platform
* application version
* notification-token reference
* registration time
* last activity
* revocation status

A device registration is not a trusted device by default.

## 30. Device Identity

The application must not depend on mutable hardware identifiers as user identity.

Device identifiers may support:

* Notification delivery
* session review
* security investigation
* installation-level configuration

They do not replace authentication.

## 31. Biometrics

Device biometrics may unlock locally stored session material or trigger reauthentication convenience.

Biometric success on a device does not independently:

* Grant organization membership
* satisfy every server step-up requirement
* approve payments
* activate agreements
* activate occupancy
* reset MFA

The server determines whether the required assurance has been satisfied.

## 32. Step-Up Authentication

Sensitive mobile operations may require:

* MFA
* recent login
* fresh provider challenge
* device biometric plus server-recognized authentication state
* another approved verification step

Examples include:

* Payment verification
* refund approval
* privileged role change
* agreement activation
* occupancy activation
* restricted export
* authentication-factor change

## 33. Organization Context

The mobile client may retain a selected organization for user experience.

Every protected request must still identify and validate:

* Requested organization
* active membership
* permission
* resource ownership
* assignment
* support context where applicable

A locally selected organization is not authoritative.

## 34. Direct Database Access

Direct mobile access to authoritative PostgreSQL tables is prohibited.

This includes ordinary use of public database APIs for protected business commands.

Mobile operations must pass through the Habitacores application-service boundary.

## 35. Storage Access

Mobile clients may receive narrowly scoped upload or download authorization after server-side approval.

They must not receive:

* Storage service-role keys
* unrestricted private-bucket access
* permanent private-document URLs
* administrative storage credentials

## 36. Document Capture

Mobile document workflows may use:

* Camera
* photo library
* file picker
* document scanner
* image compression
* metadata removal
* resumable upload

The server still controls:

* Upload intent
* object key
* type
* size
* purpose
* related resource
* quarantine
* validation
* malware scanning
* availability

## 37. Camera Permissions

Camera access must be requested only when needed.

Permission prompts must explain the user benefit.

The application must remain usable for workflows that allow file selection instead of camera capture.

## 38. Location Permissions

Precise background location is not approved by default.

Location access may be introduced only for a defined use case with:

* Necessity
* consent
* platform compliance
* minimization
* retention
* authorization
* security review

Property-address selection does not automatically require device location tracking.

## 39. Contacts and Other Device Data

The mobile application must not request contacts, call history, messages, microphone, location, or other sensitive device permissions without a specific approved requirement.

Convenience alone is insufficient.

## 40. Offline Model

Mobile will support offline-aware behavior, not an independently authoritative offline system.

Approved offline capabilities may include:

* Cached public listings
* cached authorized summaries
* controlled form drafts
* queued non-authoritative uploads
* maintenance-request drafts
* local display preferences
* pending synchronization indicators

## 41. Offline Prohibitions

The following cannot become authoritative solely while offline:

* Payment verification
* payment allocation
* refund approval
* receipt issuance
* application decision
* agreement activation
* occupancy activation
* membership change
* permission assignment
* support-session creation
* legal notice issuance

## 42. Cached Data

Cached protected data must be:

* Minimized
* scoped to the active user
* scoped to the relevant organization
* classified
* encrypted or stored securely where required
* removable on logout
* invalidated when access changes
* bounded by retention and size

## 43. Cache Is Not Authority

The server remains authoritative.

Cached data may be:

* Stale
* revoked
* superseded
* deleted
* inaccessible after membership change

The user interface must communicate synchronization state where material.

## 44. Offline Drafts

A draft may be saved locally before submission.

A submitted draft must be revalidated by the server against:

* Current schema
* current organization access
* current resource state
* current eligibility
* current business rules
* current required documents

## 45. Synchronization Conflicts

Where local drafts conflict with current server state, the application must not silently overwrite authoritative records.

Potential outcomes include:

```text
SYNC_COMPLETED
SYNC_CONFLICT
DRAFT_OBSOLETE
RESOURCE_CHANGED
MEMBERSHIP_REVOKED
REAUTHENTICATION_REQUIRED
```

## 46. Idempotency

Material mobile commands must support idempotency.

This protects against:

* Repeated taps
* network retry
* client timeout
* application restart
* background resend
* duplicate submission

The mobile client should generate or receive an approved idempotency key.

## 47. Optimistic UI

Optimistic UI may be used for low-risk reversible presentation behavior.

It must not falsely present protected authoritative outcomes as completed.

For example:

* A payment verification may display `Submitting` or `Pending confirmation`.
* It must not display `Verified` before the server commits the result.

## 48. Pagination

Mobile list endpoints should use stable cursor-based pagination where practical.

Pagination contracts should support:

* Bounded page sizes
* stable ordering
* next cursor
* filtered queries
* authorization
* low-bandwidth delivery

## 49. Data Minimization

Mobile API responses should include only fields needed for the screen or workflow.

The client should not receive complete business records merely because the server can retrieve them.

## 50. Field-Level Authorization

Mobile clients must use permission-aware read models.

Examples:

* Residents do not receive internal application-review notes.
* agents do not receive unrestricted financial evidence.
* vendors do not receive unrelated resident information.
* support users receive only support-session-approved data.

## 51. Error Model

The mobile API will return stable machine-readable errors.

Directional contract:

```json
{
  "error": {
    "code": "PERMISSION_DENIED",
    "message": "You do not have permission to perform this action.",
    "correlationId": "019..."
  }
}
```

Messages must not reveal cross-tenant resource existence.

## 52. Network Resilience

Mobile requests should use:

* Bounded timeouts
* controlled retries
* retry only for appropriate methods and failures
* exponential backoff with jitter
* connectivity awareness
* cancellation
* visible pending states

Non-idempotent commands must not be retried blindly.

## 53. Rate Limiting

Mobile API rate limiting may consider:

* User
* device registration
* installation
* IP or network
* organization
* endpoint
* abuse patterns

Rate limiting must not be treated as authorization.

## 54. Push Notifications

Push notifications may support:

* Application updates
* document review requests
* payment-state notifications
* agreement notifications
* maintenance updates
* security notifications
* organization invitations

Push delivery is not guaranteed and cannot be the sole record of a legally or operationally important event.

## 55. Push Notification Privacy

Lock-screen notification content must be minimized.

Notifications should avoid displaying:

* Full payment details
* government identifiers
* private agreement contents
* sensitive application findings
* bank information
* legal notes

The application should retrieve protected details after authentication.

## 56. Push Tokens

Push tokens must be:

* Associated with an internal device registration
* environment specific
* revocable
* updated when rotated
* excluded from ordinary logs
* protected from cross-user use

A push token is not an authentication credential.

## 57. Deep Links

Deep links may route users to:

* Invitations
* applications
* payments
* documents
* agreements
* maintenance requests
* notifications

Every deep-linked destination must revalidate:

* Authentication
* current organization access
* permission
* resource state
* link purpose
* expiry where applicable

## 58. Universal and App Links

Production mobile applications should support verified web-to-app linking through approved domains under:

```text
habitacores.com
```

The exact subdomain and association files will be configured during implementation.

## 59. Deep-Link Safety

Deep links must not contain:

* Access tokens
* refresh tokens
* storage administrative credentials
* permanent document capabilities
* sensitive payloads

Short-lived invitation or recovery tokens require dedicated validation and expiry.

## 60. API Security

The mobile API requires:

* HTTPS
* secure authentication
* current authorization
* RLS-protected backend access
* input validation
* output minimization
* rate limiting
* audit
* observability
* abuse protection
* secure headers where applicable

Certificate pinning is not required initially and requires separate operational review before adoption.

## 61. App Attestation

Platform app-attestation mechanisms may be introduced as an abuse signal.

Attestation must not replace:

* User authentication
* organization membership
* authorization
* transaction validation
* fraud review

The exact implementation is deferred.

## 62. Rooted or Compromised Devices

Habitacores must assume mobile devices may be compromised.

Sensitive server decisions must not depend on the secrecy or integrity of client code.

The application may detect risk signals, but client-side tamper detection is not a complete security boundary.

## 63. Mobile Secrets

The mobile binary may contain public configuration but must not contain server secrets.

Prohibited embedded values include:

* Database credentials
* Supabase service-role key
* storage administrative credentials
* payment-provider secret
* webhook secret
* AI-provider secret
* signing private key
* migration credentials

## 64. Environment Separation

Mobile builds must distinguish:

```text
DEVELOPMENT
INTERNAL
PREVIEW OR QA
STAGING
PRODUCTION
```

Each build configuration must use the correct:

* API domain
* authentication project
* deep-link scheme
* push-notification configuration
* telemetry environment
* application identifier
* store track
* feature configuration

## 65. Application Identifiers

Android and iOS application identifiers must be stable and environment aware.

Directional examples:

```text
com.habitacores.mobile
com.habitacores.mobile.staging
```

Exact identifiers require implementation confirmation.

## 66. Development Builds

Development builds should be used for capabilities requiring native modules and realistic integration testing.

Expo Go must not be treated as the sole production-equivalent test environment.

## 67. Build Service

Expo Application Services may be used initially for:

* Android builds
* iOS builds
* credentials management
* internal distribution
* store submission support
* controlled over-the-air updates where approved

Provider-specific release details remain behind documented operational procedures.

## 68. Native Build Credentials

Android signing keys and Apple distribution credentials are highly restricted secrets.

They require:

* Named ownership
* controlled access
* backup
* rotation or recovery procedure
* no Git storage
* no ordinary developer sharing
* audit where available

## 69. Store Distribution

Mobile releases will use:

* Internal development distribution
* controlled testing tracks
* staged production rollout
* store review
* release observation
* rollback or forward-fix procedure

Android and iOS releases may progress at different rates.

## 70. Over-the-Air Updates

Over-the-air updates may be used only for changes compatible with the installed native runtime.

They must not be used to bypass:

* App Store or Google Play rules
* security review
* native permission review
* required native binary changes
* release governance
* API compatibility

## 71. Runtime Compatibility

Every over-the-air update must target an approved compatible runtime version.

An incompatible update must not be delivered to an older native binary.

## 72. Mobile Rollback

Mobile rollback differs from web rollback.

A released store binary cannot always be withdrawn immediately from every device.

Recovery may require:

* Server-side feature kill switch
* API backward compatibility
* over-the-air correction where compatible
* expedited store release
* minimum-version enforcement in severe cases
* clear user communication

## 73. Feature Flags

Mobile feature flags may support:

* Internal testing
* staged rollout
* Android-only or iOS-only readiness
* selected-organization pilots
* provider migration
* kill switches

Feature flags remain non-authoritative for permissions.

## 74. Store Review Constraints

The release plan must account for:

* Review time
* rejected submissions
* metadata requirements
* privacy disclosures
* permission declarations
* account-deletion requirements
* payment-policy requirements
* age or content classifications

The exact store-compliance process is deferred.

## 75. Accessibility

Mobile applications must support:

* Screen readers
* scalable text
* sufficient contrast
* accessible labels
* focus order
* touch-target sizing
* reduced-motion expectations
* understandable error feedback

Accessibility must be tested on both Android and iOS.

## 76. Low-Bandwidth Design

Habitacores initially targets users who may experience limited or expensive connectivity.

The mobile application should support:

* Compressed responses
* image resizing
* resumable uploads
* pagination
* deferred non-critical loading
* visible synchronization status
* local drafts
* retry controls
* no unnecessary large downloads

## 77. Performance

Mobile performance monitoring should cover:

* Application startup
* route transition
* API latency
* failed requests
* crash-free sessions
* upload duration
* synchronization delay
* rendering performance
* memory pressure
* background task completion

## 78. Observability

Mobile telemetry must integrate with ADR-012 principles.

It may include:

* Application version
* platform
* operating-system family
* normalized screen
* API operation
* duration
* result code
* crash fingerprint
* correlation ID where safe
* network class
* synchronization state

## 79. Mobile Telemetry Restrictions

Mobile telemetry must not include:

* Passwords
* tokens
* application answers
* document contents
* payment evidence
* signed URLs
* private messages
* raw agreement text
* raw AI prompts or outputs
* unrestricted user identifiers

## 80. Crash Reporting

Crash reports should be sanitized and grouped by stable fingerprints.

They must not automatically capture:

* Screen contents
* form values
* file attachments
* protected response bodies
* session credentials

## 81. Responsible AI

Mobile clients may request approved AI features through the Habitacores API.

Directional flow:

```text
Mobile request
→ Authentication
→ Authorization
→ Approved AI use case
→ AI operation record
→ Permission-aware retrieval
→ AI Gateway
→ Validation
→ Human review where required
→ Mobile-safe result
```

## 82. AI Restrictions

The mobile application must not:

* Call AI providers directly with privileged credentials
* send unrestricted local documents to AI providers
* approve applications through AI
* verify payments through AI
* activate agreements through AI
* activate occupancy through AI
* treat local AI output as authoritative

## 83. API Documentation

The versioned API should generate or maintain documentation containing:

* Endpoint purpose
* authentication
* authorization
* request schema
* response schema
* errors
* idempotency
* pagination
* rate limits
* deprecation status
* examples using synthetic data

## 84. Testing Strategy

Testing must include:

* Shared contract tests
* server request-validation tests
* mobile API integration tests
* Android behavior
* iOS behavior
* authentication
* secure-storage behavior
* organization switching
* offline drafts
* synchronization conflicts
* duplicate submission
* deep links
* push notifications
* document capture and upload
* minimum-version behavior
* feature flags
* accessibility
* low-connectivity conditions

## 85. Contract Tests

Mobile and backend CI must verify compatibility against the published API schema.

A backend change should fail validation when it breaks a supported mobile contract without an approved version transition.

## 86. Cross-Tenant Test

```text
Given:
A mobile user belongs only to Organization A.

And:
The user knows an Organization B resource ID.

When:
The mobile application requests that resource.

Then:
The API denies or conceals access.

And:
RLS prevents the backend query from returning Organization B data.
```

## 87. Offline Financial Test

```text
Given:
A finance reviewer loses network connectivity.

When:
The reviewer attempts to verify a payment.

Then:
The mobile application does not mark the payment verified locally.

And:
The action remains pending or unavailable until the server commits it.
```

## 88. Duplicate Submission Test

```text
Given:
A user taps Submit twice during a slow network response.

When:
Both requests reach the API.

Then:
The approved idempotency mechanism produces one authoritative submission.

And:
The client receives a consistent result.
```

## 89. Secure Storage Test

```text
Given:
A user authenticates through the mobile application.

When:
Session material is persisted.

Then:
Sensitive refresh material uses approved operating-system secure storage.

And:
It does not appear in plain preferences, logs, analytics, or crash reports.
```

## 90. Version Compatibility Test

```text
Given:
The current backend is deployed.

And:
A supported previous mobile release remains installed.

When:
That release calls a versioned API endpoint.

Then:
The supported contract continues to function.

Or:
The client receives a controlled documented compatibility response.
```

## 91. Deep-Link Test

```text
Given:
A user opens a deep link to an agreement.

When:
The application resolves the link.

Then:
It authenticates the user and verifies current organization and agreement access.

And:
Possession of the link alone does not grant access.
```

## 92. AI Boundary Test

```text
Given:
A resident requests an AI explanation of an agreement clause.

When:
The mobile request executes.

Then:
The server confirms the approved use case and document access.

And:
The mobile application does not send private storage credentials or the
unrestricted agreement directly to an AI provider.
```

## 93. API Migration Requirements

API-affecting changes must define:

* Current version
* new behavior
* backward compatibility
* supported mobile releases
* deprecation date
* observability
* migration documentation
* retirement criteria

## 94. Mobile Release Record

Each production mobile release should preserve:

* Platform
* application version
* build number
* runtime version
* Git commit
* build identifier
* API compatibility range
* configuration generation
* release track
* rollout percentage
* release time
* approving actor
* health observations
* rollback or supersession relationship

## 95. Codex Boundaries

Every Codex mobile work package must specify:

* Platform scope
* screen or route
* API endpoint
* request and response schema
* authentication
* organization scope
* permission
* offline behavior
* secure-storage behavior
* idempotency
* loading and error states
* accessibility
* telemetry
* tests
* prohibited native permissions

Codex must not:

* Connect mobile code directly to PostgreSQL
* embed service-role credentials
* embed server secrets
* make local state authoritative
* implement financial arithmetic with JavaScript `number`
* treat route access as authorization
* store tokens in plain preferences
* log tokens or private API responses
* retry non-idempotent commands blindly
* create unrestricted device permissions
* send private documents directly to AI providers
* bypass the shared API contract
* remove compatibility for supported mobile releases without review

## 96. Prohibited Patterns

The following are not approved:

* Separate unrelated Android and iOS business-rule implementations
* direct authoritative database access from mobile
* service-role credentials in the app binary
* permanent private-storage URLs
* plain-text refresh-token storage
* local payment verification
* offline agreement activation
* offline occupancy activation
* client-only organization authorization
* client-calculated authoritative balances
* API breaking changes without version transition
* push notifications containing excessive sensitive detail
* deep links that independently grant access
* AI-provider secrets in mobile code
* mandatory sensitive device permissions without a defined requirement
* forced sharing of every web and mobile UI component

## 97. Options Considered

### Option A — Separate Native Android and iOS Applications

Advantages:

* Maximum platform-specific control
* direct access to native ecosystems
* independent optimization

Disadvantages:

* Duplicated implementation
* duplicated engineering skills
* greater release burden
* higher business-rule drift risk
* slower MVP delivery

Decision:

Rejected initially.

### Option B — Progressive Web Application Only

Advantages:

* Shared web code
* no store release process
* rapid deployment

Disadvantages:

* Weaker native integration
* inconsistent device capability support
* limited push and background behavior on some platforms
* less suitable for document capture and native workflows
* does not fulfill the committed Android and iOS product direction

Decision:

Rejected as the only mobile strategy.

### Option C — Flutter

Advantages:

* Strong cross-platform support
* performant rendering
* mature mobile ecosystem

Disadvantages:

* Introduces Dart alongside the TypeScript platform
* reduces direct tooling and contract reuse
* separate engineering specialization
* less aligned with the approved monorepo stack

Decision:

Not selected initially.

### Option D — React Native With Expo and Versioned Habitacores API

Advantages:

* Shared Android and iOS codebase
* TypeScript alignment
* native capabilities
* managed build and distribution tooling
* platform-specific escape hatches
* shared contracts and utilities
* clear backend authority
* suitable for iterative MVP delivery

Disadvantages:

* Native dependency compatibility requires governance
* store releases remain platform dependent
* over-the-air updates require runtime discipline
* some capabilities may require custom native modules
* mobile and web UIs cannot be universally shared

Decision:

Approved.

## 98. Positive Consequences

* Habitacores gains a formal Android and iOS architecture.
* one primary mobile codebase reduces duplication.
* TypeScript contracts can be shared.
* backend rules remain authoritative.
* mobile clients cannot bypass tenant isolation.
* offline behavior is explicitly bounded.
* secure credential storage is required.
* API compatibility accounts for delayed mobile updates.
* mobile release and rollback realities become explicit.
* native document, notification, and deep-link capabilities remain available.

## 99. Negative Consequences

* A versioned API requires additional governance.
* mobile releases are slower than web releases.
* previous application versions must be supported temporarily.
* secure local caching increases implementation complexity.
* Android and iOS behavior still requires separate testing.
* Expo and React Native upgrades require compatibility work.
* mobile observability and store operations require dedicated procedures.

## 100. Risks

### 100.1 Embedded Secret Exposure

Controls:

* No server secrets in mobile
* build inspection
* configuration classification
* automated secret scanning
* provider adapters

### 100.2 Stale Mobile Version

Controls:

* Backward-compatible API
* minimum-version policy
* telemetry
* staged deprecation
* controlled upgrade messaging

### 100.3 Offline State Conflict

Controls:

* Server authority
* version checks
* idempotency
* conflict responses
* user-visible synchronization state

### 100.4 Cross-Tenant Leakage

Controls:

* Explicit organization request context
* current membership validation
* application authorization
* RLS
* response minimization
* cross-tenant tests

### 100.5 Mobile Credential Theft

Controls:

* Secure operating-system storage
* short-lived access tokens
* session revocation
* MFA
* step-up authentication
* no telemetry leakage

### 100.6 Mobile Release Failure

Controls:

* Internal testing
* staged rollout
* API compatibility
* kill switches
* over-the-air correction where compatible
* expedited forward release

## 101. Acceptance Criterion — Shared Backend Authority

```text
Given:
The same payment is accessed from Habitacores Web and Habitacores Mobile.

When:
Either client requests its balance and status.

Then:
Both clients receive values derived from the same authoritative finance services.

And:
Neither client independently calculates or stores the authoritative result.
```

## 102. Acceptance Criterion — Mobile Tenant Isolation

```text
Given:
A user changes the locally selected organization.

When:
The next protected API request executes.

Then:
The backend verifies current membership and resource ownership.

And:
The local selection does not independently establish tenant authority.
```

## 103. Acceptance Criterion — Store Rollout Compatibility

```text
Given:
A new Android release is rolled out gradually.

And:
The previous Android release and current iOS release remain active.

When:
All supported clients call the Habitacores API.

Then:
The backend honors their supported versioned contracts.

And:
A new backend deployment does not require every user to update immediately.
```

## 104. Acceptance Criterion — Mobile Document Upload

```text
Given:
A resident photographs payment evidence using the mobile application.

When:
The upload begins.

Then:
The server has already created an authorized upload intent and immutable object key.

And:
The file remains quarantined and unverified after upload.

And:
The upload alone does not verify payment.
```

## 105. Implementation Sequence

Recommended implementation order:

```text
1. Establish the versioned API contract package
2. establish API request and response validation
3. establish shared error and pagination contracts
4. create the Expo mobile application in the monorepo
5. configure Expo Router
6. configure development, staging, and production mobile environments
7. implement API client abstraction
8. implement Supabase Auth mobile adapter
9. implement operating-system secure session storage
10. implement account and organization context
11. implement protected API authorization
12. implement public property and listing experiences
13. implement application draft and submission workflows
14. implement authorized document capture and upload
15. implement payment and receipt read workflows
16. implement agreement and occupancy read workflows
17. implement maintenance workflows
18. implement push registration and deep links
19. implement offline cache and draft boundaries
20. implement mobile observability and crash reporting
21. establish Android internal distribution
22. establish iOS internal distribution
23. establish staged production release procedures
24. preserve backward-compatible API behavior
```

## 106. Deferred Decisions

This ADR does not decide:

* Exact Expo SDK version
* exact React Native version
* exact Expo Router version
* exact mobile styling library
* exact shared-component library
* exact state-management library
* exact API-client generation tool
* exact query-cache library
* exact encrypted local-database technology
* exact push-notification provider
* exact crash-reporting provider
* exact mobile analytics provider
* exact biometric policy
* exact app-attestation provider
* exact minimum-supported-version duration
* exact Android and iOS application identifiers
* exact store-account structure
* exact Expo Application Services plan
* exact over-the-air update policy
* exact certificate-pinning policy
* exact tablet-specific experience
* exact mobile payments provider integration

These require bounded implementation decisions or later ADRs.

## 107. Supersession Conditions

This ADR should be reviewed when:

* React Native with Expo cannot meet a core native requirement.
* Android and iOS require materially different product architectures.
* a native-only security or performance requirement emerges.
* mobile traffic requires a dedicated backend-for-frontend.
* the Next.js API runtime no longer meets scalability requirements.
* offline-first authoritative transactions become necessary.
* regional mobile API infrastructure becomes mandatory.
* store policies materially change the delivery model.
* a new client platform becomes a core product surface.

## 108. Final Decision Statement

Habitacores will deliver Android and iOS applications through a shared React Native application built with Expo and TypeScript.

Expo Router will provide mobile routing, navigation, and deep-link integration. Platform-specific modules may be used where Android and iOS require different native behavior, but all platforms will preserve the same Habitacores authorization, financial, legal, document, occupancy, audit, and Responsible AI rules.

Habitacores Web, Android, and iOS will use shared backend application and domain services. Mobile clients will communicate through an explicit, versioned HTTPS API initially delivered through Next.js Route Handlers and shared application-service packages.

The mobile application will not connect directly to authoritative PostgreSQL tables, database administrative functions, storage administrative services, internal workers, unrestricted AI providers, or secrets infrastructure.

Authentication will use the approved identity-provider architecture. Sensitive mobile session material will use operating-system secure storage. Provider tokens will establish authentication evidence but will not independently grant organization membership, permission, tenant access, or protected command authority.

Every protected mobile request will validate current account status, requested organization, membership, permission, assignment, resource ownership, workflow state, authentication assurance, and tenant isolation on the server.

The API will use explicit versions and backward-compatible evolution. Habitacores will account for delayed Android and iOS upgrades, store-review delays, staged rollouts, previous supported client versions, and mobile rollback limitations.

Offline behavior will be limited to controlled cached reads, drafts, queued uploads, and other non-authoritative experiences. Payment verification, fund allocation, refunds, application decisions, agreement activation, occupancy activation, membership changes, permission changes, and legally significant actions will not become authoritative solely from offline or local mobile state.

Document capture and upload will follow the approved upload-intent, immutable-object, quarantine, validation, malware-scan, authorization, and document-lifecycle architecture. Uploading payment evidence will not verify payment.

Push notifications and deep links will support navigation and awareness but will not independently grant access or serve as the sole authoritative record of material events.

Mobile builds will use isolated development, staging, and production configuration. Server credentials, database credentials, service-role keys, storage administrative credentials, payment secrets, AI-provider secrets, webhook secrets, signing private keys, and migration credentials will not be embedded in mobile binaries.

Android and iOS releases will use controlled build credentials, internal testing, staged rollout, release records, API compatibility verification, observability, kill switches, and forward-fix or rollback procedures appropriate to mobile distribution.

AI capabilities requested through mobile will remain registered, server mediated, permission aware, purpose limited, minimized, validated, auditable, and human reviewed where required. Mobile applications will not call AI providers directly with privileged credentials or treat local model output as authoritative Habitacores state.

No mobile route, local cache, deep link, push token, device identifier, biometric result, feature flag, application binary, offline draft, Expo update, API token, coding agent, or AI model may bypass Habitacores authentication, account status, organization membership, tenant isolation, authorization, financial rules, document controls, agreement rules, occupancy rules, audit requirements, API compatibility, release governance, or Responsible AI safeguards.