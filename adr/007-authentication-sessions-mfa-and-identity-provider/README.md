# ADR-007 — Authentication, Sessions, MFA, and Identity-Provider Strategy

## Decision Record

| Field | Value |
|---|---|
| Product | Habitacores |
| Decision | Authentication, Sessions, MFA, and Identity-Provider Strategy |
| ADR | ADR-007 |
| Version | 0.1 |
| Status | Proposed |
| Decision Type | Foundational Identity and Security Architecture |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Identity Provider | Supabase Auth |
| Web Session Model | Secure cookie-based session |
| Application Identity | Habitacores-owned user and identity-link records |
| Organization Membership | Habitacores-owned |
| Authorization | Habitacores-owned and server enforced |
| Initial MFA Method | TOTP |
| Privileged-Role MFA | Required |
| Sensitive-Action Authentication | Step-up authentication |
| Provider Abstraction | Required |
| Password Storage | Identity provider only |
| Last Updated | 1 August 2026 |

## 1. Context

Habitacores will serve users including:

- Prospects
- rental applicants
- guarantors
- residents
- landlords and property owners
- organization administrators
- estate managers
- agents
- finance reviewers
- maintenance personnel
- vendors
- estate lawyers
- support operators
- platform administrators

A person may:

- Belong to no organization
- belong to one organization
- belong to multiple organizations
- hold different roles in different organizations
- act personally as an applicant or resident
- act professionally for an organization
- receive temporary or scoped assignments
- require elevated authentication for sensitive actions

Authentication must remain separate from:

- Organization membership
- role assignment
- permission evaluation
- resource authorization
- support-session authority
- financial authority
- agreement authority
- occupancy authority

The identity architecture must support secure MVP delivery while preserving future migration to another identity provider, enterprise SSO, passkeys, additional MFA factors, and machine identities.

## 2. Decision Drivers

The decision is driven by the need to:

1. Avoid implementing password storage and credential security from scratch.
2. support the Next.js App Router.
3. support secure server-rendered sessions.
4. support email verification.
5. support password recovery.
6. support MFA.
7. support session expiry and revocation.
8. preserve Habitacores-owned user identity.
9. preserve Habitacores-owned organization membership.
10. preserve provider-neutral authorization.
11. support users belonging to multiple organizations.
12. support privileged platform roles.
13. support step-up authentication.
14. support account suspension.
15. support identity-provider migration.
16. avoid provider organization models becoming the product domain.
17. support future enterprise federation.
18. support auditable identity events.
19. prevent AI from gaining authentication authority.
20. provide clear implementation boundaries for Codex.

## 3. Decision

Habitacores will use Supabase Auth as its initial managed identity provider.

Supabase Auth will be responsible for:

- Credential verification
- password handling
- email verification
- password recovery
- authentication challenges
- MFA enrollment and verification
- access and refresh-token issuance
- identity-provider session lifecycle
- supported future federated identity methods

Habitacores will remain responsible for:

- Internal user identity
- person profiles
- identity-provider links
- organization membership
- invitations
- roles
- permissions
- assignments
- resource authorization
- account business status
- support-session authority
- separation of duties
- audit
- application-specific security policy

## 4. Architectural Boundary

The approved authentication flow is:

```text
User
→ Habitacores authentication interface
→ Supabase Auth
→ Verified provider identity
→ Habitacores identity-link resolution
→ Internal user-account validation
→ Session context
→ Organization resolution
→ Authorization
→ Application service
````

Authentication success does not itself grant access to an organization or protected resource.

## 5. Why Supabase Auth

Supabase Auth is selected initially because it provides:

* Next.js App Router integration
* cookie-based server-rendered authentication
* access and refresh-token session handling
* MFA support
* Authenticator Assurance Level information
* email and password authentication
* recovery workflows
* alignment with the planned PostgreSQL ecosystem
* a managed security boundary
* a practical MVP implementation path

The provider is selected for authentication convenience and security maturity, not as the owner of the Habitacores domain model.

## 6. Provider-Neutral Identity Adapter

Provider-specific code must remain behind an identity-provider adapter.

Conceptual interface:

```typescript
interface IdentityProvider {
  getCurrentIdentity(): Promise<ProviderIdentity | null>;
  verifySession(): Promise<VerifiedProviderSession | null>;
  revokeSession(sessionId: string): Promise<void>;
  revokeAllUserSessions(providerUserId: string): Promise<void>;
  getAuthenticationAssurance(): Promise<AuthenticationAssurance>;
}
```

Application and domain packages must not depend directly on Supabase SDK types.

## 7. Internal User Identity

Habitacores will maintain an internal user record.

Illustrative structure:

```text
users
- id
- status
- primary_email
- email_verified_at
- created_at
- updated_at
- suspended_at
- disabled_at
```

The internal user identifier will use the approved UUIDv7 strategy.

The provider user identifier will not become the Habitacores primary key.

## 8. Identity Links

External identity-provider subjects will be stored separately.

Illustrative structure:

```text
identity_links
- id
- user_id
- provider
- provider_tenant
- provider_subject
- linked_at
- last_authenticated_at
- status
```

A uniqueness constraint should cover the provider identity scope.

This supports:

* Provider migration
* multiple login methods
* future enterprise federation
* account recovery
* identity-link auditing
* separation of authentication identity from application identity

## 9. Provider Subject

The provider subject identifier is evidence of an authenticated external identity.

It must not be used directly as:

* Organization membership ID
* role assignment ID
* resident ID
* applicant ID
* agreement party ID
* payment actor ID
* support authority
* platform authorization

It must first resolve to an active Habitacores internal user.

## 10. User and Person Separation

The architecture should distinguish:

```text
User account
Person profile
Organization membership
Applicant profile
Resident profile
Professional assignment
```

One user account may relate to several business contexts.

A person profile must not be duplicated merely because the person belongs to multiple organizations.

## 11. Authentication Methods

Initial approved authentication methods are:

* Email and password
* email verification
* password recovery
* TOTP MFA

Additional methods may be introduced later, including:

* Passkeys
* magic links
* social identity
* enterprise SAML
* enterprise OIDC
* phone-based verification

Each additional method requires security and account-linking review.

## 12. Email Verification

Email verification is required before a user may complete protected onboarding unless an approved invitation or administrative workflow provides equivalent assurance.

Email verification must not automatically:

* Approve an application
* create organization membership
* activate a resident
* grant a role
* grant financial authority
* grant platform authority

## 13. Password Handling

Habitacores application code must never:

* Store plaintext passwords
* log passwords
* transmit passwords to application telemetry
* place passwords in audit metadata
* copy password hashes into Habitacores domain tables
* expose recovery tokens
* inspect provider password hashes

Credential handling remains inside the approved identity provider.

## 14. Password Policy

Password policy should be configured through the identity provider and complemented by application guidance.

The system should favor:

* Sufficient password length
* breached-password protection where available
* rate limiting
* MFA for privileged accounts
* secure recovery
* session revocation after compromise

Complexity rules must not encourage predictable password patterns.

## 15. Session Model

The Next.js web application will use secure cookie-based sessions.

Session handling must support:

* Server-side validation
* access-token refresh
* refresh-token protection
* session expiry
* session revocation
* multi-device sessions
* authentication assurance
* secure logout
* account-status validation

Protected pages must not rely solely on stale client-side authentication state.

## 16. Cookie Requirements

Authentication cookies must use appropriate controls, including:

* `HttpOnly` where applicable
* `Secure` in production
* appropriate `SameSite`
* restricted path and domain
* controlled expiry
* no sensitive application data beyond the required session material

The exact cookie configuration must follow the selected provider’s supported server-side integration.

## 17. Server-Side Session Validation

Every protected server boundary must validate the current session.

Applicable boundaries include:

* Server Components
* Server Actions
* Route Handlers
* upload-intent endpoints
* secure downloads
* platform operations
* support-session entry
* Responsible AI invocation

Client-visible authentication state is not sufficient proof.

## 18. Session Context

A validated Habitacores request context may include:

```typescript
interface AuthenticationContext {
  userId: UserId;
  providerSessionId: string;
  providerSubject: string;
  authenticationTime?: Date;
  assuranceLevel: string;
  mfaSatisfied: boolean;
  correlationId: string;
}
```

Organization and authorization information must be resolved separately.

## 19. No Authoritative Active Organization in Session

A provider or browser session may carry a selected organization for interface convenience.

It must not become the authoritative organization scope.

Every protected organization request must validate:

1. Requested organization
2. internal user
3. active membership
4. membership status
5. required permission
6. resource organization
7. assignment where applicable
8. support-session context where applicable

This protects against:

* Stale organization selection
* multi-tab organization switching
* token claim drift
* revoked membership
* manipulated client state

## 20. Organization Membership

Organization membership remains in Habitacores PostgreSQL records.

An organization membership should preserve:

* Internal membership ID
* organization ID
* user ID
* status
* invitation source
* joined time
* suspension or revocation time
* applicable role assignments
* audit history

The identity provider’s organization feature is not the source of truth.

## 21. Authorization Claims

JWT or provider claims may improve request efficiency but must not become the sole authority for mutable application permissions.

Claims may become stale when:

* Membership is revoked
* role changes
* assignment changes
* account is suspended
* organization is disabled
* support authority expires

Material protected actions must evaluate current Habitacores state.

## 22. Account Status

Habitacores will maintain application account states such as:

```text
PENDING
ACTIVE
SUSPENDED
DISABLED
CLOSED
```

A valid identity-provider session does not override a suspended or disabled Habitacores account.

## 23. Session Revocation

The system must support:

* Revoking one session
* revoking all user sessions
* revocation after password reset
* revocation after suspected compromise
* revocation after account suspension
* revocation after privileged-role removal where warranted
* operationally initiated revocation

Revocation actions must be audited.

## 24. Multi-Device Sessions

Users may have multiple active sessions subject to approved policy.

The user should eventually be able to review:

* Device or session label
* approximate login time
* last activity
* approximate location where safely available
* current session
* revocation option

Sensitive device fingerprinting should not be introduced without privacy review.

## 25. Session Expiry

Session policy must distinguish:

* Access-token lifetime
* refresh-token behavior
* maximum session lifetime
* inactivity timeout
* privileged-session lifetime
* step-up validity window

Exact durations will be configured during implementation and security testing.

## 26. Logout

Logout must:

* End or revoke the relevant provider session
* clear application authentication state
* avoid leaving sensitive pages available from shared caches
* redirect to a safe location
* preserve audit evidence where required

Client-side redirection without server-side session termination is insufficient.

## 27. MFA

TOTP-based MFA is the initial approved second factor.

MFA enrollment must include:

* Reauthentication where required
* secure factor registration
* verification before activation
* recovery guidance
* factor-management audit
* controlled reset

## 28. Mandatory MFA

MFA will be required for privileged roles, including at minimum:

* Platform administrators
* platform security operators
* support operators with protected-data access
* organization administrators
* finance users who verify, allocate, approve, or refund money
* users issuing or activating agreements
* users performing high-impact legal or occupancy actions

The precise role matrix will be finalized with authorization implementation.

## 29. Optional or Progressive MFA

MFA may initially be optional for lower-risk personal users such as prospects or applicants.

The system may require enrollment when a user gains elevated privileges.

Lack of MFA must never silently grant access to a function whose policy requires it.

## 30. Authentication Assurance Levels

Habitacores should evaluate the current authentication assurance level for protected actions.

Conceptually:

```text
AAL1
→ primary authentication completed

AAL2
→ primary authentication plus approved MFA
```

The application must map provider assurance information into provider-neutral Habitacores policy.

## 31. Step-Up Authentication

Sensitive actions may require recent stronger authentication even when a valid session exists.

Examples:

* Change primary email
* change authentication factors
* view recovery material
* assign privileged roles
* verify or allocate payments
* approve or execute refunds
* issue or activate agreements
* activate occupancy
* enter protected support sessions
* export restricted data
* rotate integration credentials
* change payout configuration

## 32. Step-Up Result

A step-up check should evaluate:

* Required assurance level
* actual assurance level
* authentication recency
* session status
* account status
* operation sensitivity

A stale MFA-authenticated session may still require fresh verification.

## 33. Recovery

Account recovery must be designed against account takeover.

Controls should include:

* Short-lived recovery links or tokens
* single-use behavior
* safe redirect allowlists
* rate limiting
* user notification
* session revocation after successful recovery
* audit
* no token logging
* no privileged support bypass without controlled verification

## 34. MFA Recovery

MFA recovery is a sensitive operation.

The process must not rely only on:

* Email assertion
* a support request
* easily obtained profile information
* social knowledge
* possession of an organization role

MFA reset requires a separately approved identity-verification procedure and elevated audit.

## 35. Invitations

Organization invitations must remain Habitacores business records.

An invitation should include:

* Invitation ID
* organization ID
* intended email or identity
* proposed membership
* proposed role or assignment
* inviter
* expiry
* status
* acceptance time
* revocation
* audit

Provider email delivery may transmit the invitation, but the provider does not own the membership decision.

## 36. Invitation Acceptance

Invitation acceptance must validate:

1. Invitation exists.
2. invitation is active.
3. invitation is not expired.
4. authenticated identity matches the intended recipient or approved transfer process.
5. organization remains active.
6. proposed role remains permitted.
7. separation-of-duty rules are satisfied.
8. membership does not already conflict.

## 37. Duplicate Accounts

The system must detect and safely handle cases where:

* One email is used across multiple provider identities
* social and password identities overlap
* an invitation is accepted with another identity
* email case differs
* provider account linking is attempted

Identity merging must never occur automatically based solely on matching display names.

## 38. Email Normalization

Email addresses should be normalized according to a documented policy.

The system must not apply provider-specific mailbox transformations that could incorrectly merge distinct addresses.

The original display form may be retained separately from the normalized lookup form.

## 39. Account Linking

Linking another identity provider or authentication method requires:

* Active authenticated session
* recent step-up authentication
* proof of control of the new identity
* conflict checks
* notification
* audit

Account linking must not accidentally transfer organization authority.

## 40. Support Access

Support staff must not impersonate or authenticate as a user through ordinary credential mechanisms.

Support access should use a dedicated support-session workflow with:

* Authorized support identity
* reason
* scope
* target user or organization
* approval where required
* expiry
* visible indication
* audit
* restricted actions

## 41. Administrative User Creation

Privileged administrators may invite users but should not create reusable passwords for them.

The intended user should establish or verify their own authentication method.

Temporary credentials, where unavoidable, require forced reset and separate approval.

## 42. Service and Machine Identity

Human authentication sessions must remain separate from machine identities.

Future machine access may use:

* Signed service tokens
* OAuth client credentials
* scoped API keys
* workload identity

Machine identities must have:

* Explicit owner
* explicit environment
* explicit scope
* expiry or rotation
* audit
* revocation
* no interactive user impersonation

## 43. Background Workers

Workers must not reuse browser sessions.

Workers should use:

* Trusted internal workload identity
* job actor context
* original initiating user ID where relevant
* organization context
* correlation ID
* application-service authorization rules where applicable

## 44. Webhooks

Provider webhooks authenticate through signature verification, not user sessions.

A verified webhook still cannot bypass domain rules.

Webhook identity must preserve:

* Provider
* environment
* event ID
* signature status
* received time
* processing identity

## 45. API Authentication

Future mobile or partner APIs may use bearer tokens or another approved protocol.

The application must not assume browser cookies are the only authentication mechanism forever.

All protocols must resolve into the same provider-neutral Habitacores authentication context.

## 46. Identity Audit Events

Material identity events must be audited, including:

* Registration
* email verification
* login success where appropriate
* repeated login failure or lockout signal
* password recovery
* password change notification
* MFA enrollment
* MFA removal
* MFA reset
* session revocation
* account linking
* account unlinking
* account suspension
* account reactivation
* invitation acceptance
* privileged-role grant

Secrets and raw tokens must never enter audit metadata.

## 47. Authentication Observability

Safe operational telemetry may include:

* Authentication operation
* provider
* result code
* assurance level
* user ID where appropriate
* session ID in restricted form
* duration
* rate-limit result
* correlation ID
* failure category

Telemetry must not include:

* Passwords
* access tokens
* refresh tokens
* recovery tokens
* MFA secrets
* TOTP codes
* full provider payloads

## 48. Rate Limiting and Abuse Protection

Priority authentication rate limits include:

* Registration
* login
* verification resend
* password recovery
* MFA challenge
* invitation acceptance
* identity linking
* session creation
* support recovery requests

Rate limiting must account for user, network, device, provider, and abuse-pattern signals without becoming a discriminatory access barrier.

## 49. Error Handling

Authentication errors should use safe, stable outcomes such as:

```text
AUTHENTICATION_REQUIRED
INVALID_CREDENTIALS
EMAIL_NOT_VERIFIED
MFA_REQUIRED
STEP_UP_REQUIRED
SESSION_EXPIRED
SESSION_REVOKED
ACCOUNT_SUSPENDED
ACCOUNT_DISABLED
IDENTITY_LINK_CONFLICT
INVITATION_INVALID
INVITATION_EXPIRED
```

The application should avoid revealing whether an unrelated account exists when doing so would aid enumeration.

## 50. Responsible AI Boundary

AI may assist with:

* Explaining authentication requirements
* drafting user-facing recovery guidance
* classifying support requests
* summarizing identity-security events for authorized operators

AI must not:

* Authenticate users
* verify passwords
* generate or inspect MFA secrets
* decide identity recovery
* reset MFA
* issue sessions
* grant memberships
* assign roles
* approve support impersonation
* receive access or refresh tokens
* infer authorization from conversation

## 51. AI Data Minimization

Authentication-related AI use cases must exclude:

* Passwords
* password hashes
* access tokens
* refresh tokens
* recovery tokens
* MFA seeds
* TOTP values
* raw session cookies
* full government identification
* unrestricted security-event payloads

## 52. Database Access

Authentication-provider tables or schemas must not become general-purpose application tables.

Habitacores business records should reference internal user IDs rather than directly reference provider tables throughout the domain.

Provider-specific access remains inside the identity adapter.

## 53. Authorization Independence

Authentication answers:

```text
Who is this?
```

Authorization answers:

```text
May this authenticated actor perform this operation on this resource now?
```

The two decisions must remain separate.

A valid session cannot override:

* Suspended account
* revoked membership
* missing permission
* expired assignment
* resource mismatch
* separation-of-duty rule
* insufficient assurance level

## 54. Row-Level Security Interaction

Future PostgreSQL RLS may receive trusted application identity and organization context.

RLS must not trust arbitrary JWT claims forwarded from the browser without controlled validation.

The complete RLS and database-role strategy remains a separate ADR.

## 55. Client Components

Client Components may display authentication state and invoke supported provider UI flows.

They must not:

* Make final authorization decisions
* decode claims as authoritative permissions
* store refresh tokens in application state
* expose MFA secrets
* assume selected organization is valid
* bypass server-side session validation

## 56. Server Components

Protected Server Components must obtain identity through approved server-side session helpers.

They should then invoke:

```text
Session validation
→ internal user resolution
→ account-status validation
→ permission-aware query service
```

## 57. Server Actions

Every protected Server Action must independently:

1. Validate the provider session.
2. resolve the internal user.
3. validate account status.
4. resolve organization context.
5. authorize the command.
6. enforce MFA or step-up policy.
7. invoke the application service.

## 58. Route Handlers

Protected Route Handlers must use the same identity boundary.

Provider callbacks and webhooks use their applicable signature or protocol authentication and must not be confused with interactive user sessions.

## 59. Caching

Protected authentication and membership decisions must not enter shared caches in a way that survives:

* Session revocation
* account suspension
* membership revocation
* role change
* support-session expiry
* MFA-policy change

Authentication-sensitive pages should remain dynamic unless safe cache behavior is proven.

## 60. Provider Failure

When the authentication provider is unavailable:

* Existing sessions may continue only according to validated provider behavior and approved risk policy.
* new login may be unavailable.
* protected actions must fail safely.
* no anonymous fallback may grant access.
* operational monitoring must alert.
* application data must remain protected.

## 61. Provider Migration

The identity-link architecture must support future migration.

Migration planning must preserve:

* Internal user IDs
* memberships
* roles
* assignments
* audit history
* business records
* identity-link lineage
* account recovery
* session invalidation

Provider migration must not require re-keying all Habitacores business records.

## 62. Enterprise SSO

Future enterprise SSO may use SAML or OIDC.

Enterprise federation must still resolve to:

```text
Provider identity
→ Habitacores user
→ Habitacores membership
→ Habitacores authorization
```

An enterprise identity-provider assertion does not automatically create unrestricted organization access.

## 63. Passkeys

Passkeys are directionally supported for future implementation.

They may improve phishing resistance, but require review of:

* Device recovery
* cross-device use
* user education
* provider support
* privileged-account policy
* account-linking behavior

## 64. Secrets

Identity-provider secrets must remain in approved secret storage.

They must not appear in:

* Git
* `.env.example` values
* client bundles
* logs
* ADR examples
* screenshots
* Codex prompts
* AI-provider requests

## 65. Environment Isolation

Local, test, staging, and production authentication must remain isolated.

Production provider credentials and real user sessions must not be used in ordinary development or CI.

Redirect URLs, email templates, webhook secrets, and provider settings must be environment specific.

## 66. Testing

Authentication tests must cover:

* Registration
* email verification
* login
* logout
* session refresh
* session expiry
* session revocation
* account suspension
* MFA enrollment
* MFA requirement
* step-up requirement
* recovery
* invitation acceptance
* identity linking
* multi-organization membership
* revoked membership
* stale claims
* provider outage
* cross-tenant access attempts

## 67. Security Tests

Adversarial tests should include:

* Session fixation
* token replay
* cookie theft assumptions
* CSRF
* open redirect
* account enumeration
* invitation theft
* MFA bypass
* stale-role claims
* organization switching across tabs
* support-session misuse
* identity-link takeover
* recovery-token reuse

## 68. Test Accounts

Automated tests must use synthetic identities.

Real production accounts, tokens, MFA seeds, recovery links, or provider payloads must not be used as fixtures.

## 69. Codex Boundaries

Every Codex authentication work package must specify:

* Provider adapter
* server or client boundary
* permitted authentication flow
* internal user mapping
* session validation
* MFA requirement
* step-up requirement
* organization resolution
* authorization handoff
* audit events
* tests
* prohibited secrets

Codex must not:

* Store passwords
* bypass the identity adapter
* use provider user IDs as universal primary keys
* make provider organizations authoritative
* trust JWT roles as current authorization
* store tokens in ordinary logs
* expose refresh tokens to Client Components
* disable MFA for convenience
* invent recovery procedures
* grant memberships after login automatically
* allow AI to resolve identity disputes

## 70. Prohibited Patterns

The following are not approved:

* Custom password storage
* passwords in Habitacores tables
* provider subject as the universal application primary key
* provider organization as the authoritative tenant
* authorization based only on JWT claims
* organization access based only on an active-organization cookie
* session validity treated as account eligibility
* MFA enforced only in the UI
* unrestricted support impersonation
* tokens logged or audited
* refresh tokens exposed to browser application state
* client-side authorization as final authority
* anonymous fallback during provider outage
* AI-assisted credential verification
* shared production authentication credentials in development

## 71. Alternatives Considered

### Option A — Custom Authentication

Advantages:

* Maximum control
* no managed-provider dependency

Disadvantages:

* Password-security burden
* recovery complexity
* MFA implementation burden
* session-security burden
* greater attack surface
* slower MVP delivery

Decision:

Rejected.

### Option B — Clerk

Advantages:

* Strong Next.js integration
* polished authentication UI
* organization and membership functionality
* rapid B2B SaaS implementation

Disadvantages:

* Provider organization model could compete with Habitacores’ domain model
* active-organization session context requires careful handling
* greater risk of authorization logic drifting into provider constructs
* additional migration complexity if deeply adopted

Decision:

Not selected initially.

### Option C — Auth0

Advantages:

* Mature identity platform
* enterprise federation
* MFA
* B2B organization support
* machine-to-machine capabilities

Disadvantages:

* Greater configuration and commercial complexity for the MVP
* organization features vary by plan
* potentially unnecessary enterprise overhead at launch
* provider organization model must still remain non-authoritative

Decision:

Not selected initially.

### Option D — Better Auth

Advantages:

* Self-hosted application ownership
* broad plugin model
* organization, 2FA, passkey, and session functionality
* provider independence

Disadvantages:

* Greater operational and security responsibility
* authentication database and upgrade burden
* abuse protection and delivery infrastructure require additional ownership
* less desirable for minimizing MVP identity risk

Decision:

Not selected initially.

### Option E — Supabase Auth With Habitacores-Owned Authorization

Advantages:

* Managed authentication
* Next.js server-side integration
* session lifecycle
* MFA and assurance levels
* alignment with PostgreSQL infrastructure
* practical MVP delivery
* internal tenancy and authorization remain provider neutral

Disadvantages:

* Provider dependency
* careful SSR token handling is required
* provider outages affect login
* migration requires identity-link planning
* enterprise federation may require later expansion

Decision:

Approved.

## 72. Positive Consequences

* Habitacores avoids custom credential storage.
* secure authentication can be delivered earlier.
* MFA is available for privileged roles.
* internal user IDs remain stable.
* organization membership remains under Habitacores control.
* provider migration remains feasible.
* Next.js server rendering is supported.
* authorization remains independent from authentication.
* step-up security can protect sensitive actions.
* future SSO and passkeys remain possible.

## 73. Negative Consequences

* Habitacores depends initially on Supabase Auth availability.
* provider-specific adapter code is required.
* session and refresh behavior require careful testing.
* internal user and provider identity records must be reconciled.
* MFA recovery requires an operational procedure.
* enterprise SSO is not solved by this ADR.
* multiple identity-link scenarios add complexity.

## 74. Risks

### 74.1 Provider Lock-In

Controls:

* Internal user IDs
* identity links
* provider adapter
* no provider IDs in domain foreign keys
* documented migration path

### 74.2 Stale Authorization Claims

Controls:

* Habitacores-owned membership
* current server-side policy evaluation
* no JWT-only authorization
* short-lived claims where used

### 74.3 Cross-Organization Context Confusion

Controls:

* Explicit requested organization
* current membership validation
* resource-organization validation
* multi-tab tests
* no cookie-only organization authority

### 74.4 MFA Bypass

Controls:

* Server-side assurance checks
* mandatory privileged-role policy
* step-up enforcement
* security testing
* audit

### 74.5 Account Recovery Takeover

Controls:

* Short-lived tokens
* rate limiting
* session revocation
* notification
* controlled MFA reset
* audit

### 74.6 Token Leakage

Controls:

* Secure cookies
* no ordinary logging
* no client exposure
* redaction
* secret scanning

## 75. Acceptance Criterion — Internal Identity

```text
Given:
A user authenticates successfully through Supabase Auth.

When:
Habitacores establishes the application context.

Then:
The provider subject resolves to a Habitacores internal user ID.

And:
Business records reference the internal user ID.

And:
The provider subject does not become the universal domain primary key.
```

## 76. Acceptance Criterion — Organization Authorization

```text
Given:
A user has a valid session.

And:
The user requests an Organization A resource.

When:
The request executes.

Then:
Habitacores validates the current Organization A membership and permission.

And:
A provider claim or selected-organization cookie alone is insufficient.
```

## 77. Acceptance Criterion — Revoked Membership

```text
Given:
A user still has a valid authentication session.

And:
The user's organization membership has been revoked.

When:
The user requests an organization resource.

Then:
Access is denied.

And:
Authentication success does not override membership revocation.
```

## 78. Acceptance Criterion — MFA

```text
Given:
A finance reviewer has authenticated with only the primary factor.

When:
The reviewer attempts to verify or allocate a payment.

Then:
The operation requires the approved stronger assurance level.

And:
The action does not execute until MFA or step-up requirements are satisfied.
```

## 79. Acceptance Criterion — Account Suspension

```text
Given:
A provider session remains technically valid.

And:
The Habitacores user account is suspended.

When:
A protected request is made.

Then:
The application denies the request.

And:
The session does not override the application account state.
```

## 80. Acceptance Criterion — AI Boundary

```text
Given:
A user asks an AI assistant to reset MFA or grant access.

When:
The AI processes the request.

Then:
The AI provides only approved guidance.

And:
It does not authenticate the user, reset factors, create sessions, grant memberships, or assign permissions.
```

## 81. Implementation Sequence

Recommended implementation order:

```text
1. Create the identity-provider abstraction
2. configure environment-isolated Supabase Auth
3. implement secure Next.js server-side session handling
4. create internal users
5. create identity links
6. implement provider-to-user resolution
7. implement account-status validation
8. implement organization-membership resolution
9. implement server-side authorization handoff
10. implement registration and email verification
11. implement login and logout
12. implement password recovery
13. implement session revocation
14. implement TOTP enrollment and challenge
15. enforce MFA for privileged roles
16. implement step-up checks
17. implement identity audit events
18. add authentication abuse controls
19. add adversarial security tests
20. keep provider organizations non-authoritative
```

## 82. Deferred Decisions

This ADR does not decide:

* Exact Supabase project topology
* exact access-token lifetime
* exact inactivity timeout
* exact maximum session lifetime
* exact step-up validity window
* social login providers
* phone authentication
* passkey implementation
* enterprise SAML or OIDC
* government-identity verification provider
* biometric authentication
* machine API-key architecture
* exact MFA recovery procedure
* exact suspicious-login detection
* exact email-delivery provider
* exact breached-password service
* exact CAPTCHA or bot-protection provider
* final database RLS session-context implementation

These require bounded implementation decisions or later ADRs.

## 83. Supersession Conditions

This ADR should be reviewed when:

* Supabase Auth cannot meet production security requirements.
* provider availability is unacceptable.
* enterprise federation becomes a core requirement.
* regulatory requirements mandate another identity architecture.
* passkeys become the required primary method.
* self-hosting authentication becomes strategically necessary.
* pricing materially affects product viability.
* provider migration becomes necessary.
* machine identity becomes a major product surface.

## 84. Final Decision Statement

Habitacores will use Supabase Auth as its initial managed identity provider for credential verification, email verification, password recovery, authentication sessions, refresh-token handling, and TOTP-based MFA.

Habitacores will maintain independent internal user identifiers and provider identity-link records. Supabase user identifiers will not become universal Habitacores domain primary keys.

Organization membership, invitations, roles, permissions, assignments, separation-of-duty policies, support authority, account business status, and resource authorization will remain authoritative Habitacores records and policies.

Authentication establishes who the actor is. It does not determine what the actor may access or do.

Protected requests will validate the provider session, resolve the internal user, validate account status, resolve the requested organization, validate current membership, evaluate resource authorization, and enforce the required authentication assurance level.

MFA will be mandatory for privileged platform, support, organization-administration, finance, agreement, legal, and occupancy operations according to the approved authorization matrix. Sensitive actions may require recent step-up authentication even when a valid MFA-capable session exists.

Provider organization context, JWT roles, browser state, selected-organization cookies, hidden navigation, URL possession, or client-side authentication state will not independently grant Habitacores authority.

Passwords, recovery tokens, access tokens, refresh tokens, MFA secrets, and TOTP values will not be stored in Habitacores business tables, ordinary logs, audit metadata, coding-agent prompts, or AI-provider requests.

AI may explain authentication requirements and assist authorized security operations, but it will not verify credentials, issue sessions, reset MFA, resolve account-ownership disputes, grant membership, assign permissions, or authorize protected actions.

No identity provider, cookie, token, browser, React component, framework middleware, organization switcher, support operator, worker, webhook, coding agent, or AI model may bypass Habitacores account status, organization membership, authorization, authentication assurance, audit, tenant isolation, financial controls, agreement controls, occupancy controls, or Responsible AI safeguards.