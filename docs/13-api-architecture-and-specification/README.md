
# Document 13 — API Architecture and Specification

## Document Control

| Field | Value |
|---|---|
| Product | Habitacores |
| Document | API Architecture and Specification |
| Version | 0.1 |
| Status | Draft for approval |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Market | Nigeria |
| Primary Currency | Nigerian naira |
| Primary Operational Time Zone | West Africa Time |
| Primary Tenant Boundary | Organization |
| Proposed Application Framework | Next.js and TypeScript |
| Proposed System of Record | PostgreSQL |
| Last Updated | 30 July 2026 |

## 1. Purpose

This document defines the API architecture and initial logical API specification for Habitacores.

It translates the approved:

- Product vision
- Product principles
- Business architecture
- Domain model
- User personas and roles
- Business rules
- Functional requirements
- Non-functional requirements
- System architecture
- Data architecture
- Permission matrix and authorization model
- User journeys and workflow specifications

into a consistent application-programming interface model.

This document defines:

- API design principles
- API boundaries
- Internal and external API categories
- Resource naming
- Route structures
- Request and response conventions
- Authentication
- Organization context
- Authorization
- Scope and assignment enforcement
- Command and query operations
- Pagination
- Filtering
- Sorting
- Search
- Validation
- Error responses
- Idempotency
- Concurrency controls
- Status-transition endpoints
- Financial APIs
- Agreement and occupancy APIs
- Document upload and retrieval APIs
- Notification APIs
- Webhooks
- Provider integrations
- API versioning
- Rate limiting
- Observability
- Security
- Mobile readiness
- API testing
- Acceptance criteria
- Traceability
- Governance

This document is a logical specification.

It does not yet define every production payload, database query, framework implementation, or external provider contract.

Detailed implementation will be completed through reviewed code, schemas, contract tests, and Architecture Decision Records.

## 2. API Architecture Objectives

The Habitacores API architecture must:

1. Enforce organization-level tenant isolation.
2. Enforce server-side authorization.
3. Represent approved domain workflows explicitly.
4. Prevent unrestricted direct record mutation.
5. Preserve financial correctness.
6. Preserve agreement and occupancy integrity.
7. Support responsive web and future mobile clients.
8. Support low-bandwidth use.
9. Provide stable, understandable resource contracts.
10. Support safe retries.
11. Prevent duplicate business actions.
12. Support controlled external-provider integration.
13. Support reliable webhooks.
14. Provide consistent errors.
15. Preserve auditability.
16. Support observability and troubleshooting.
17. Remain extensible without premature complexity.
18. Support future partner APIs.
19. Separate public, authenticated, privileged, and internal operations.
20. Preserve backward compatibility where externally required.

## 3. API Style

Habitacores will initially use a resource-oriented HTTP API with explicit command endpoints for material business transitions.

The API will combine:

- Resource queries
- Resource creation
- Controlled resource updates
- Explicit domain commands
- Background-job requests
- Webhook receivers
- Internal provider adapters

The architecture should not expose unrestricted generic CRUD operations for critical records.

Example:

```text
Preferred:
POST /payments/{paymentId}/verify

Avoid:
PATCH /payments/{paymentId}
{
  "verificationStatus": "verified"
}
````

The explicit command endpoint better supports:

* Authorization
* State validation
* Separation of duties
* Audit events
* Business invariants
* Idempotency
* Clear intent

## 4. API Categories

Habitacores APIs may be divided into:

### 4.1 Public APIs

Available without an authenticated organization membership.

Examples:

* Public listing search
* Public listing details
* Account registration
* Login initiation
* Inquiry creation
* Inspection request

### 4.2 Authenticated User APIs

Available to authenticated users acting on their own records.

Examples:

* Applicant application
* Resident occupancy
* Resident maintenance request
* Guarantor document submission
* User notification preferences

### 4.3 Organization APIs

Available to users with active organization memberships and permissions.

Examples:

* Property administration
* Listing management
* Application review
* Payment verification
* Agreement management
* Maintenance operations

### 4.4 Platform APIs

Available to privileged Habitacores platform users.

Examples:

* Organization lifecycle
* Platform configuration
* Support sessions
* Security investigations
* Platform audit

### 4.5 Internal APIs

Used between trusted Habitacores modules, background workers, and infrastructure adapters.

### 4.6 External Partner APIs

Potential future APIs exposed to approved partners such as:

* Payment providers
* Electronic-signature providers
* Communication providers
* Estate agencies
* Property operators
* Enterprise customers

External partner APIs must use explicit versioning and stronger contract governance.

## 5. API Boundary Principles

### 5.1 Server Authority

The API is the authoritative enforcement boundary for protected business operations.

### 5.2 Client Inputs Are Untrusted

The API must not trust:

* Client-supplied organization identifiers
* Client-supplied role names
* Client-supplied permission claims
* Client-supplied status transitions
* Client-supplied financial balances
* Client-supplied ownership assertions
* Client-supplied provider-verification results

### 5.3 Explicit Business Intent

Material state changes must use explicit operations.

### 5.4 Domain Ownership

Each API resource should have a clearly identified owning domain module.

### 5.5 Minimal Data Exposure

Responses must include only fields required for the authorized use case.

### 5.6 Stable Contracts

External contracts should remain backward compatible within a supported API version.

## 6. Base URL Strategy

Illustrative production base URL:

```text
https://api.habitacores.com
```

The initial Next.js implementation may expose APIs from the primary application domain:

```text
https://app.habitacores.com/api
```

The final public domain and route topology remain subject to infrastructure approval.

## 7. Versioning Strategy

External and mobile APIs should use explicit major-version prefixes.

Example:

```text
/api/v1/listings
/api/v1/applications
/api/v1/payments
```

Internal framework-bound operations may evolve without public versioning where:

* They are used only by the same deployed application.
* They are not public contracts.
* Changes are deployed atomically.
* Tests preserve expected behavior.

Once an API is consumed by:

* Native mobile applications
* Partner systems
* Public developers
* Independently deployed services

it must use formal versioning.

## 8. API Version Policy

A major API version change may be required when:

* A required field is removed.
* A field meaning changes materially.
* A response type changes incompatibly.
* An endpoint is removed.
* Authentication semantics change.
* Pagination semantics change.
* Error semantics change.
* Workflow behavior changes incompatibly.

Compatible changes may include:

* Adding optional fields
* Adding new endpoints
* Adding new optional filters
* Adding new error metadata
* Adding new enum values where clients are designed to tolerate them

## 9. Route Naming Conventions

Routes should use:

```text
lowercase
plural nouns
hyphenated compound words
```

Examples:

```text
/organizations
/organization-memberships
/maintenance-requests
/payment-allocations
/agreement-versions
```

Avoid:

```text
/getApplications
/process_payment
/ApproveAgreement
```

## 10. Resource Identifier Strategy

Routes should use immutable resource identifiers.

Example:

```text
/units/{unitId}
/applications/{applicationId}
/payments/{paymentId}
```

Business identifiers may be supported through lookup endpoints or query parameters.

Example:

```text
/applications?businessId=HAB-APP-000001
```

Clients must not assume that business identifiers reveal internal database structure.

## 11. Organization Context

Protected organization APIs must resolve an active organization context.

Possible mechanisms include:

* Organization identifier in route
* Organization identifier in verified header
* Organization identifier in authenticated session context
* Explicit organization switch followed by session validation

Preferred route pattern for organization-owned collections:

```text
/api/v1/organizations/{organizationId}/properties
/api/v1/organizations/{organizationId}/applications
/api/v1/organizations/{organizationId}/payments
```

The server must independently verify:

* The organization exists.
* The user has an active membership.
* The membership applies to the requested action.
* The resource belongs to that organization or an approved related organization.

## 12. Active Organization Header

An optional explicit header may be used:

```http
X-Habitacores-Organization-Id: <organization-id>
```

The header is contextual input only.

It is not proof of authority.

The server must validate the organization against the authenticated user and requested resource.

## 13. Authentication

Protected APIs must require an authenticated identity.

Potential mechanisms include:

* Secure HTTP-only session cookie
* Bearer token
* Signed identity-provider token
* Service credential for trusted internal calls

The final authentication mechanism will be defined through an Architecture Decision Record.

## 14. Authentication Headers

Illustrative bearer-token request:

```http
Authorization: Bearer <access-token>
```

Illustrative correlation request:

```http
X-Correlation-Id: <client-generated-id>
```

The server may accept or replace a client-provided correlation identifier.

## 15. Authorization

Every protected endpoint must evaluate:

1. Authenticated user
2. Active account
3. Active organization
4. Active membership
5. Required permission
6. Resource organization
7. Role scope
8. Assignment
9. Record state
10. Approval requirement
11. Separation of duties
12. Sensitive-field access
13. Support-session context
14. Authorization exception

The presence of an authenticated token alone does not grant access.

## 16. Permission-to-Endpoint Mapping

Each endpoint must declare one or more required permissions.

Example:

```text
POST /payments/{paymentId}/verify
Permission:
payment.verify
```

Example:

```text
POST /applications/{applicationId}/decision
Permission:
application.decide
```

Example:

```text
GET /legal-matters/{legalMatterId}
Permission:
legal_matter.read
Additional:
active assignment or authorized organization scope
```

## 17. Scope and Assignment Enforcement

Professional-user endpoints must validate assignments.

Examples:

### Estate Agent

```text
GET /listings/{listingId}/inquiries
```

requires:

* `inquiry.read`
* Active assignment to the listing or inquiry
* Approved agency relationship

### Estate Lawyer

```text
GET /legal-matters/{legalMatterId}
```

requires:

* `legal_matter.read`
* Active legal-matter assignment
* Approved legal-organization relationship

### Vendor Worker

```text
GET /work-orders/{workOrderId}
```

requires:

* `work_order.read`
* Active worker assignment
* Approved vendor relationship

## 18. Request Content Type

JSON APIs should use:

```http
Content-Type: application/json
Accept: application/json
```

File upload operations may use:

```http
multipart/form-data
```

or signed direct-upload workflows.

## 19. Standard Response Envelope

A consistent response envelope may be used:

```json
{
  "data": {},
  "meta": {
    "correlationId": "01HXYZ...",
    "timestamp": "2026-07-30T20:00:00Z"
  }
}
```

Collection response:

```json
{
  "data": [],
  "meta": {
    "page": 1,
    "pageSize": 25,
    "totalCount": 120,
    "totalPages": 5,
    "correlationId": "01HXYZ..."
  }
}
```

The final envelope design remains subject to implementation review.

## 20. Standard Error Envelope

Illustrative error response:

```json
{
  "error": {
    "code": "PAYMENT_NOT_VERIFIED",
    "message": "This payment must be verified before it can be allocated.",
    "details": [],
    "correlationId": "01HXYZ..."
  }
}
```

Validation error:

```json
{
  "error": {
    "code": "VALIDATION_FAILED",
    "message": "The request contains invalid fields.",
    "details": [
      {
        "field": "amount",
        "code": "AMOUNT_MUST_BE_POSITIVE",
        "message": "Amount must be greater than zero."
      }
    ],
    "correlationId": "01HXYZ..."
  }
}
```

## 21. HTTP Status Codes

Preferred semantics:

| Status | Use                                         |
| ------ | ------------------------------------------- |
| 200    | Successful read or command                  |
| 201    | Resource created                            |
| 202    | Accepted for asynchronous processing        |
| 204    | Successful operation without response body  |
| 400    | Malformed request                           |
| 401    | Missing or invalid authentication           |
| 403    | Authenticated but unauthorized              |
| 404    | Resource absent or intentionally concealed  |
| 409    | Current-state or concurrency conflict       |
| 412    | Precondition failure                        |
| 422    | Semantically invalid request                |
| 428    | Approval, version, or precondition required |
| 429    | Rate limited                                |
| 500    | Internal server failure                     |
| 502    | Upstream provider failure                   |
| 503    | Temporarily unavailable                     |
| 504    | Upstream timeout                            |

## 22. Error-Code Conventions

Error codes should use stable uppercase identifiers.

Examples:

```text
AUTHENTICATION_REQUIRED
MEMBERSHIP_INACTIVE
PERMISSION_DENIED
RESOURCE_NOT_FOUND
ORGANIZATION_SCOPE_MISMATCH
ASSIGNMENT_REQUIRED
ASSIGNMENT_EXPIRED
INVALID_RECORD_STATE
APPROVAL_REQUIRED
SELF_APPROVAL_PROHIBITED
VERSION_CONFLICT
PAYMENT_NOT_VERIFIED
PAYMENT_ALREADY_ALLOCATED
OCCUPANCY_CONFLICT
AGREEMENT_NOT_SIGNED
FILE_VALIDATION_FAILED
PROVIDER_UNAVAILABLE
RATE_LIMIT_EXCEEDED
```

## 23. Validation

Every endpoint must validate:

* Route parameters
* Query parameters
* Headers
* Request body
* Data types
* Required fields
* Enum values
* String lengths
* Date order
* Financial values
* Currency
* Business-state conditions
* Organization consistency

Schema validation may use a library such as Zod.

## 24. Request Size Limits

The API must define size limits for:

* JSON bodies
* File uploads
* Bulk imports
* Search queries
* Export filters
* AI inputs
* Webhook payloads

Oversized requests should be rejected before expensive processing.

## 25. Date and Time Format

API timestamps should use ISO 8601.

Example:

```text
2026-07-30T20:00:00Z
```

Date-only values:

```text
2026-08-01
```

Time-zone-aware operational dates should preserve:

* UTC timestamp
* Relevant operational time zone where needed
* Original provider timestamp where relevant

## 26. Currency and Money Representation

The API must never use binary floating-point assumptions for authoritative money.

Illustrative numeric representation:

```json
{
  "amount": "500000.00",
  "currency": "NGN"
}
```

Alternative minor-unit representation:

```json
{
  "amountMinor": 50000000,
  "currency": "NGN"
}
```

The final financial representation will be defined through an Architecture Decision Record.

## 27. Boolean and Null Semantics

The API must distinguish:

* `false`
* `null`
* omitted
* unknown
* not applicable
* pending confirmation

Example:

```json
{
  "confirmedUnitCount": null,
  "unitCountStatus": "pending"
}
```

The API must not represent unknown Block B inventory as zero.

## 28. Pagination

Collection endpoints must use bounded pagination.

Supported models may include:

### Offset Pagination

```text
?page=1&pageSize=25
```

### Cursor Pagination

```text
?cursor=<cursor>&limit=25
```

Cursor pagination is preferred for:

* Audit events
* Notifications
* Communications
* Large activity feeds

## 29. Pagination Limits

Suggested defaults:

```text
Default page size: 25
Maximum ordinary page size: 100
```

Larger exports should use asynchronous export workflows rather than unbounded API responses.

## 30. Filtering

Filtering should use explicit query parameters.

Examples:

```text
/applications?status=under_review
/payments?verificationStatus=pending_verification
/agreements?expiresBefore=2027-01-01
/units?occupancyStatus=vacant
```

Filters must be:

* Validated
* Indexed where important
* Organization scoped
* Authorization aware

## 31. Sorting

Sorting may use:

```text
?sort=createdAt
?sort=-createdAt
```

or:

```text
?sortBy=createdAt&sortDirection=desc
```

Only approved sortable fields should be accepted.

## 32. Search

Search endpoints must be:

* Tenant scoped
* Permission aware
* Paginated
* Rate limited
* Protected from sensitive-field exposure

Example:

```text
GET /organizations/{organizationId}/search?q=HAB-APP-000001
```

Search results should include only authorized resource summaries.

## 33. Field Selection

Future APIs may support limited field selection:

```text
?fields=id,businessId,status
```

Field selection must not bypass field-level authorization.

## 34. Resource Expansion

Controlled expansion may be supported:

```text
?include=unit,listing
```

The server must authorize each included resource.

Unbounded nested expansion should not be permitted.

## 35. Optimistic Concurrency

Mutable resources may include a version value.

Example response:

```json
{
  "id": "resource-id",
  "recordVersion": 4
}
```

Update request:

```http
If-Match: "4"
```

or:

```json
{
  "recordVersion": 4
}
```

A stale request should return:

```text
409 Conflict
VERSION_CONFLICT
```

## 36. Idempotency

Retryable state-changing operations should support:

```http
Idempotency-Key: <unique-key>
```

Examples:

* Payment recording
* Payment-provider callback
* Refund execution
* Agreement issue
* Notification submission
* Import creation
* AI job submission

The server should associate the key with:

* User or client
* Organization
* Endpoint
* Request hash
* Result
* Expiry

## 37. Idempotency Response

If the same idempotency key and request are repeated:

* The original result should be returned where possible.
* A duplicate business event must not be created.

If the key is reused with a different request:

```text
409 Conflict
IDEMPOTENCY_KEY_REUSED
```

## 38. Command Endpoints

Critical domain transitions should use commands.

Examples:

```text
POST /applications/{applicationId}/submit
POST /applications/{applicationId}/request-information
POST /applications/{applicationId}/decide
POST /offers/{offerId}/issue
POST /offers/{offerId}/accept
POST /payments/{paymentId}/verify
POST /payments/{paymentId}/allocate
POST /agreements/{agreementId}/issue
POST /agreements/{agreementId}/activate
POST /occupancies/{occupancyId}/activate
POST /maintenance-requests/{requestId}/triage
POST /work-orders/{workOrderId}/verify-completion
```

## 39. Command Response Structure

Illustrative command response:

```json
{
  "data": {
    "resourceId": "application-id",
    "previousStatus": "under_review",
    "newStatus": "approved",
    "command": "application.decide"
  },
  "meta": {
    "correlationId": "01HXYZ...",
    "timestamp": "2026-07-30T20:00:00Z"
  }
}
```

## 40. Asynchronous Command Responses

Long-running command:

```http
202 Accepted
```

Illustrative response:

```json
{
  "data": {
    "jobId": "job-id",
    "status": "queued",
    "statusUrl": "/api/v1/jobs/job-id"
  }
}
```

## 41. Public Listing APIs

### List Public Listings

```text
GET /api/v1/public/listings
```

Supported filters may include:

* Location
* Unit type
* Bedroom count
* Minimum annual rent
* Maximum annual rent
* Availability date
* Approved amenities

### Read Public Listing

```text
GET /api/v1/public/listings/{listingId}
```

### Create Inquiry

```text
POST /api/v1/public/listings/{listingId}/inquiries
```

Illustrative request:

```json
{
  "fullName": "Prospect Name",
  "email": "prospect@example.com",
  "phoneNumber": "+234...",
  "message": "I would like to inspect this unit.",
  "preferredContactMethod": "email"
}
```

## 42. Authentication APIs

Potential endpoints:

```text
POST /api/v1/auth/register
POST /api/v1/auth/login
POST /api/v1/auth/logout
POST /api/v1/auth/verify-email
POST /api/v1/auth/request-password-reset
POST /api/v1/auth/reset-password
POST /api/v1/auth/mfa/challenge
POST /api/v1/auth/mfa/verify
GET  /api/v1/auth/session
```

Authentication provider implementation may alter the final route structure.

## 43. User APIs

```text
GET   /api/v1/users/me
PATCH /api/v1/users/me
GET   /api/v1/users/me/memberships
GET   /api/v1/users/me/notifications
PATCH /api/v1/users/me/notification-preferences
GET   /api/v1/users/me/support-access
```

User APIs must not expose other users’ private information without authority.

## 44. Organization APIs

```text
POST  /api/v1/organizations
GET   /api/v1/organizations/{organizationId}
PATCH /api/v1/organizations/{organizationId}
POST  /api/v1/organizations/{organizationId}/archive
POST  /api/v1/organizations/{organizationId}/restore
```

Privileged organization actions:

```text
POST /api/v1/organizations/{organizationId}/transfer-ownership
POST /api/v1/organizations/{organizationId}/suspend
POST /api/v1/organizations/{organizationId}/reactivate
```

## 45. Membership APIs

```text
GET  /api/v1/organizations/{organizationId}/memberships
POST /api/v1/organizations/{organizationId}/memberships/invitations
GET  /api/v1/organizations/{organizationId}/memberships/{membershipId}
POST /api/v1/organizations/{organizationId}/memberships/{membershipId}/suspend
POST /api/v1/organizations/{organizationId}/memberships/{membershipId}/revoke
```

Invitation acceptance:

```text
POST /api/v1/membership-invitations/{invitationId}/accept
POST /api/v1/membership-invitations/{invitationId}/decline
```

## 46. Role and Assignment APIs

```text
GET  /api/v1/organizations/{organizationId}/roles
GET  /api/v1/organizations/{organizationId}/permissions
POST /api/v1/organizations/{organizationId}/role-assignments
POST /api/v1/organizations/{organizationId}/role-assignments/{assignmentId}/approve
POST /api/v1/organizations/{organizationId}/role-assignments/{assignmentId}/revoke
```

Resource assignments:

```text
POST /api/v1/organizations/{organizationId}/resource-assignments
POST /api/v1/organizations/{organizationId}/resource-assignments/{assignmentId}/revoke
```

## 47. Organization Relationship APIs

```text
GET  /api/v1/organizations/{organizationId}/relationships
POST /api/v1/organizations/{organizationId}/relationships
POST /api/v1/organization-relationships/{relationshipId}/approve
POST /api/v1/organization-relationships/{relationshipId}/reject
POST /api/v1/organization-relationships/{relationshipId}/revoke
```

Relationship activation must not automatically grant access to unrelated resources.

## 48. Property APIs

```text
GET  /api/v1/organizations/{organizationId}/properties
POST /api/v1/organizations/{organizationId}/properties
GET  /api/v1/organizations/{organizationId}/properties/{propertyId}
PATCH /api/v1/organizations/{organizationId}/properties/{propertyId}
POST /api/v1/organizations/{organizationId}/properties/{propertyId}/archive
```

## 49. Community APIs

```text
GET  /api/v1/organizations/{organizationId}/properties/{propertyId}/communities
POST /api/v1/organizations/{organizationId}/properties/{propertyId}/communities
GET  /api/v1/organizations/{organizationId}/communities/{communityId}
PATCH /api/v1/organizations/{organizationId}/communities/{communityId}
```

## 50. Building APIs

```text
GET  /api/v1/organizations/{organizationId}/communities/{communityId}/buildings
POST /api/v1/organizations/{organizationId}/communities/{communityId}/buildings
GET  /api/v1/organizations/{organizationId}/buildings/{buildingId}
PATCH /api/v1/organizations/{organizationId}/buildings/{buildingId}
POST /api/v1/organizations/{organizationId}/buildings/{buildingId}/change-status
```

## 51. Block APIs

```text
GET  /api/v1/organizations/{organizationId}/buildings/{buildingId}/blocks
POST /api/v1/organizations/{organizationId}/buildings/{buildingId}/blocks
GET  /api/v1/organizations/{organizationId}/blocks/{blockId}
PATCH /api/v1/organizations/{organizationId}/blocks/{blockId}
```

Unknown unit counts must be represented as null or pending.

## 52. Floor APIs

```text
GET  /api/v1/organizations/{organizationId}/blocks/{blockId}/floors
POST /api/v1/organizations/{organizationId}/blocks/{blockId}/floors
GET  /api/v1/organizations/{organizationId}/floors/{floorId}
PATCH /api/v1/organizations/{organizationId}/floors/{floorId}
```

Floor creation must remain optional where physical structure is not confirmed.

## 53. Unit APIs

```text
GET  /api/v1/organizations/{organizationId}/units
POST /api/v1/organizations/{organizationId}/units
GET  /api/v1/organizations/{organizationId}/units/{unitId}
PATCH /api/v1/organizations/{organizationId}/units/{unitId}
POST /api/v1/organizations/{organizationId}/units/{unitId}/mark-ready
POST /api/v1/organizations/{organizationId}/units/{unitId}/mark-available
POST /api/v1/organizations/{organizationId}/units/{unitId}/retire
POST /api/v1/organizations/{organizationId}/units/{unitId}/restore
```

## 54. Amenity APIs

```text
GET  /api/v1/organizations/{organizationId}/amenities
POST /api/v1/organizations/{organizationId}/amenities
GET  /api/v1/organizations/{organizationId}/amenities/{amenityId}
PATCH /api/v1/organizations/{organizationId}/amenities/{amenityId}
POST /api/v1/organizations/{organizationId}/amenities/{amenityId}/change-status
```

The pilot masjid may be represented as an amenity or facility without inferring a user’s religion.

## 55. Listing APIs

```text
GET  /api/v1/organizations/{organizationId}/listings
POST /api/v1/organizations/{organizationId}/listings
GET  /api/v1/organizations/{organizationId}/listings/{listingId}
PATCH /api/v1/organizations/{organizationId}/listings/{listingId}
POST /api/v1/organizations/{organizationId}/listings/{listingId}/submit-for-approval
POST /api/v1/organizations/{organizationId}/listings/{listingId}/approve
POST /api/v1/organizations/{organizationId}/listings/{listingId}/reject
POST /api/v1/organizations/{organizationId}/listings/{listingId}/publish
POST /api/v1/organizations/{organizationId}/listings/{listingId}/pause
POST /api/v1/organizations/{organizationId}/listings/{listingId}/close
```

## 56. Listing Media APIs

```text
POST   /api/v1/organizations/{organizationId}/listings/{listingId}/media
PATCH  /api/v1/organizations/{organizationId}/listing-media/{mediaId}
DELETE /api/v1/organizations/{organizationId}/listing-media/{mediaId}
POST   /api/v1/organizations/{organizationId}/listing-media/{mediaId}/set-primary
```

Media deletion must preserve historical references where required.

## 57. Inquiry APIs

```text
GET  /api/v1/organizations/{organizationId}/inquiries
GET  /api/v1/organizations/{organizationId}/inquiries/{inquiryId}
PATCH /api/v1/organizations/{organizationId}/inquiries/{inquiryId}
POST /api/v1/organizations/{organizationId}/inquiries/{inquiryId}/assign
POST /api/v1/organizations/{organizationId}/inquiries/{inquiryId}/close
```

## 58. Inspection APIs

```text
POST /api/v1/listings/{listingId}/inspection-requests
GET  /api/v1/users/me/inspections
GET  /api/v1/organizations/{organizationId}/inspections
GET  /api/v1/organizations/{organizationId}/inspections/{inspectionId}
POST /api/v1/organizations/{organizationId}/inspections/{inspectionId}/confirm
POST /api/v1/organizations/{organizationId}/inspections/{inspectionId}/reschedule
POST /api/v1/organizations/{organizationId}/inspections/{inspectionId}/cancel
POST /api/v1/organizations/{organizationId}/inspections/{inspectionId}/complete
```

## 59. Applicant Application APIs

```text
POST  /api/v1/applications
GET   /api/v1/applications/{applicationId}
PATCH /api/v1/applications/{applicationId}
POST  /api/v1/applications/{applicationId}/submit
POST  /api/v1/applications/{applicationId}/withdraw
```

Applicant access must be own-record scoped.

## 60. Application Party APIs

```text
GET  /api/v1/applications/{applicationId}/parties
POST /api/v1/applications/{applicationId}/parties
POST /api/v1/applications/{applicationId}/parties/{partyId}/invite
POST /api/v1/applications/{applicationId}/parties/{partyId}/resend-invitation
POST /api/v1/applications/{applicationId}/parties/{partyId}/remove
```

## 61. Application Document APIs

```text
GET  /api/v1/applications/{applicationId}/document-requirements
POST /api/v1/applications/{applicationId}/documents
GET  /api/v1/applications/{applicationId}/documents/{documentId}
POST /api/v1/applications/{applicationId}/documents/{documentId}/replace
```

Review endpoints:

```text
POST /api/v1/organizations/{organizationId}/application-documents/{documentId}/approve
POST /api/v1/organizations/{organizationId}/application-documents/{documentId}/reject
```

## 62. Application Review APIs

```text
GET  /api/v1/organizations/{organizationId}/application-review-queue
POST /api/v1/organizations/{organizationId}/applications/{applicationId}/assign-reviewer
GET  /api/v1/organizations/{organizationId}/applications/{applicationId}/reviews
POST /api/v1/organizations/{organizationId}/applications/{applicationId}/reviews
POST /api/v1/organizations/{organizationId}/applications/{applicationId}/request-information
POST /api/v1/organizations/{organizationId}/applications/{applicationId}/record-recommendation
```

## 63. Application Decision API

```text
POST /api/v1/organizations/{organizationId}/applications/{applicationId}/decision
```

Illustrative request:

```json
{
  "decision": "approved",
  "internalReason": "Application requirements satisfied.",
  "applicantMessage": "Your application has been approved.",
  "conditions": []
}
```

The API must validate:

* Decision authority
* Application state
* Conflict rules
* Required review completion
* Lawful decision reason
* Organization scope

## 64. Offer APIs

```text
POST /api/v1/organizations/{organizationId}/applications/{applicationId}/offers
GET  /api/v1/organizations/{organizationId}/offers/{offerId}
PATCH /api/v1/organizations/{organizationId}/offers/{offerId}
POST /api/v1/organizations/{organizationId}/offers/{offerId}/submit-for-approval
POST /api/v1/organizations/{organizationId}/offers/{offerId}/approve
POST /api/v1/organizations/{organizationId}/offers/{offerId}/issue
POST /api/v1/offers/{offerId}/accept
POST /api/v1/offers/{offerId}/decline
POST /api/v1/organizations/{organizationId}/offers/{offerId}/replace
POST /api/v1/organizations/{organizationId}/offers/{offerId}/expire
```

## 65. Charge APIs

```text
GET  /api/v1/organizations/{organizationId}/charges
POST /api/v1/organizations/{organizationId}/charges
GET  /api/v1/organizations/{organizationId}/charges/{chargeId}
PATCH /api/v1/organizations/{organizationId}/charges/{chargeId}
POST /api/v1/organizations/{organizationId}/charges/{chargeId}/approve
POST /api/v1/organizations/{organizationId}/charges/{chargeId}/adjust
POST /api/v1/organizations/{organizationId}/charges/{chargeId}/waive
POST /api/v1/organizations/{organizationId}/charges/{chargeId}/cancel
```

Applicant and resident view:

```text
GET /api/v1/users/me/charges
GET /api/v1/users/me/charges/{chargeId}
```

## 66. Payment APIs

Applicant or resident:

```text
POST /api/v1/users/me/payments
GET  /api/v1/users/me/payments
GET  /api/v1/users/me/payments/{paymentId}
POST /api/v1/users/me/payments/{paymentId}/evidence
```

Organization finance:

```text
GET  /api/v1/organizations/{organizationId}/payments
GET  /api/v1/organizations/{organizationId}/payments/{paymentId}
POST /api/v1/organizations/{organizationId}/payments/{paymentId}/request-information
POST /api/v1/organizations/{organizationId}/payments/{paymentId}/verify
POST /api/v1/organizations/{organizationId}/payments/{paymentId}/reject
POST /api/v1/organizations/{organizationId}/payments/{paymentId}/allocate
POST /api/v1/organizations/{organizationId}/payments/{paymentId}/reverse
```

## 67. Payment Verification Request

Illustrative request:

```json
{
  "verificationSource": "bank_reference_review",
  "verificationNotes": "Recipient, date, reference, and amount confirmed.",
  "recordVersion": 3
}
```

The server must not accept a client-supplied final balance.

## 68. Payment Allocation Request

Illustrative request:

```json
{
  "allocations": [
    {
      "chargeId": "charge-id",
      "amount": "400000.00",
      "currency": "NGN"
    }
  ],
  "recordVersion": 4
}
```

The server must validate:

* Payment is verified.
* Currency matches.
* Payment has sufficient unallocated value.
* Charge has sufficient outstanding value.
* Organization context is valid.
* Concurrent allocation has not changed the balance.

## 69. Refund APIs

```text
POST /api/v1/organizations/{organizationId}/refunds
GET  /api/v1/organizations/{organizationId}/refunds
GET  /api/v1/organizations/{organizationId}/refunds/{refundId}
POST /api/v1/organizations/{organizationId}/refunds/{refundId}/review
POST /api/v1/organizations/{organizationId}/refunds/{refundId}/approve
POST /api/v1/organizations/{organizationId}/refunds/{refundId}/reject
POST /api/v1/organizations/{organizationId}/refunds/{refundId}/execute
```

Refund execution should require idempotency.

## 70. Deposit APIs

```text
GET  /api/v1/organizations/{organizationId}/deposits
GET  /api/v1/organizations/{organizationId}/deposits/{depositId}
POST /api/v1/organizations/{organizationId}/deposits/{depositId}/propose-decision
POST /api/v1/organizations/{organizationId}/deposit-decisions/{decisionId}/approve
POST /api/v1/organizations/{organizationId}/deposit-decisions/{decisionId}/reject
POST /api/v1/deposit-decisions/{decisionId}/dispute
```

## 71. Receipt APIs

```text
GET  /api/v1/users/me/receipts
GET  /api/v1/users/me/receipts/{receiptId}
POST /api/v1/organizations/{organizationId}/payments/{paymentId}/issue-receipt
```

Receipt document access must be authorized and protected.

## 72. Agreement Template APIs

```text
GET  /api/v1/organizations/{organizationId}/agreement-templates
POST /api/v1/organizations/{organizationId}/agreement-templates
GET  /api/v1/organizations/{organizationId}/agreement-templates/{templateId}
PATCH /api/v1/organizations/{organizationId}/agreement-templates/{templateId}
POST /api/v1/organizations/{organizationId}/agreement-templates/{templateId}/approve
POST /api/v1/organizations/{organizationId}/agreement-templates/{templateId}/archive
```

## 73. Agreement APIs

```text
GET  /api/v1/organizations/{organizationId}/agreements
POST /api/v1/organizations/{organizationId}/agreements
GET  /api/v1/organizations/{organizationId}/agreements/{agreementId}
PATCH /api/v1/organizations/{organizationId}/agreements/{agreementId}
POST /api/v1/organizations/{organizationId}/agreements/{agreementId}/submit-for-legal-review
POST /api/v1/organizations/{organizationId}/agreements/{agreementId}/approve
POST /api/v1/organizations/{organizationId}/agreements/{agreementId}/issue
POST /api/v1/organizations/{organizationId}/agreements/{agreementId}/activate
POST /api/v1/organizations/{organizationId}/agreements/{agreementId}/amend
POST /api/v1/organizations/{organizationId}/agreements/{agreementId}/terminate
```

## 74. Agreement Version APIs

```text
GET /api/v1/organizations/{organizationId}/agreements/{agreementId}/versions
GET /api/v1/organizations/{organizationId}/agreement-versions/{versionId}
```

Issued and signed versions should not support direct mutation.

## 75. Signature APIs

```text
GET  /api/v1/users/me/signature-requests
GET  /api/v1/signature-requests/{signatureRequestId}
POST /api/v1/signature-requests/{signatureRequestId}/sign
POST /api/v1/signature-requests/{signatureRequestId}/decline
```

Provider callback:

```text
POST /api/v1/webhooks/signatures/{provider}
```

## 76. Occupancy APIs

```text
GET  /api/v1/organizations/{organizationId}/occupancies
POST /api/v1/organizations/{organizationId}/occupancies
GET  /api/v1/organizations/{organizationId}/occupancies/{occupancyId}
POST /api/v1/organizations/{organizationId}/occupancies/{occupancyId}/activate
POST /api/v1/organizations/{organizationId}/occupancies/{occupancyId}/end
```

Resident view:

```text
GET /api/v1/users/me/occupancies
GET /api/v1/users/me/occupancies/{occupancyId}
```

## 77. Household APIs

```text
GET  /api/v1/occupancies/{occupancyId}/household
POST /api/v1/occupancies/{occupancyId}/household-change-requests
GET  /api/v1/organizations/{organizationId}/household-change-requests
POST /api/v1/organizations/{organizationId}/household-change-requests/{requestId}/approve
POST /api/v1/organizations/{organizationId}/household-change-requests/{requestId}/reject
```

## 78. Move-In APIs

```text
POST /api/v1/organizations/{organizationId}/occupancies/{occupancyId}/move-in
GET  /api/v1/organizations/{organizationId}/move-ins/{moveInId}
PATCH /api/v1/organizations/{organizationId}/move-ins/{moveInId}
POST /api/v1/organizations/{organizationId}/move-ins/{moveInId}/schedule
POST /api/v1/organizations/{organizationId}/move-ins/{moveInId}/complete-checklist
POST /api/v1/organizations/{organizationId}/move-ins/{moveInId}/complete
```

## 79. Renewal APIs

```text
GET  /api/v1/organizations/{organizationId}/renewal-cases
POST /api/v1/organizations/{organizationId}/agreements/{agreementId}/renewal-case
GET  /api/v1/organizations/{organizationId}/renewal-cases/{renewalCaseId}
POST /api/v1/organizations/{organizationId}/renewal-cases/{renewalCaseId}/decide
POST /api/v1/organizations/{organizationId}/renewal-cases/{renewalCaseId}/offers
POST /api/v1/renewal-offers/{renewalOfferId}/accept
POST /api/v1/renewal-offers/{renewalOfferId}/decline
```

## 80. Move-Out APIs

```text
POST /api/v1/users/me/occupancies/{occupancyId}/move-out-request
POST /api/v1/organizations/{organizationId}/occupancies/{occupancyId}/move-out
GET  /api/v1/organizations/{organizationId}/move-outs/{moveOutId}
POST /api/v1/organizations/{organizationId}/move-outs/{moveOutId}/schedule-inspection
POST /api/v1/organizations/{organizationId}/move-outs/{moveOutId}/record-condition
POST /api/v1/organizations/{organizationId}/move-outs/{moveOutId}/propose-deposit-decision
POST /api/v1/organizations/{organizationId}/move-outs/{moveOutId}/complete
```

## 81. Maintenance Request APIs

Resident:

```text
POST /api/v1/users/me/maintenance-requests
GET  /api/v1/users/me/maintenance-requests
GET  /api/v1/users/me/maintenance-requests/{requestId}
POST /api/v1/users/me/maintenance-requests/{requestId}/request-reopen
```

Organization:

```text
GET  /api/v1/organizations/{organizationId}/maintenance-requests
GET  /api/v1/organizations/{organizationId}/maintenance-requests/{requestId}
POST /api/v1/organizations/{organizationId}/maintenance-requests/{requestId}/triage
POST /api/v1/organizations/{organizationId}/maintenance-requests/{requestId}/close
POST /api/v1/organizations/{organizationId}/maintenance-requests/{requestId}/reopen
```

## 82. Work-Order APIs

```text
POST /api/v1/organizations/{organizationId}/maintenance-requests/{requestId}/work-orders
GET  /api/v1/organizations/{organizationId}/work-orders
GET  /api/v1/organizations/{organizationId}/work-orders/{workOrderId}
POST /api/v1/organizations/{organizationId}/work-orders/{workOrderId}/assign-vendor
POST /api/v1/organizations/{organizationId}/work-orders/{workOrderId}/approve-quote
POST /api/v1/organizations/{organizationId}/work-orders/{workOrderId}/verify-completion
POST /api/v1/organizations/{organizationId}/work-orders/{workOrderId}/close
```

Vendor-facing:

```text
GET  /api/v1/vendors/me/work-orders
GET  /api/v1/vendors/me/work-orders/{workOrderId}
POST /api/v1/vendors/me/work-orders/{workOrderId}/accept
POST /api/v1/vendors/me/work-orders/{workOrderId}/decline
POST /api/v1/vendors/me/work-orders/{workOrderId}/submit-quote
POST /api/v1/vendors/me/work-orders/{workOrderId}/request-change
POST /api/v1/vendors/me/work-orders/{workOrderId}/update-status
POST /api/v1/vendors/me/work-orders/{workOrderId}/submit-completion
```

## 83. Vendor APIs

```text
GET  /api/v1/organizations/{organizationId}/vendors
POST /api/v1/organizations/{organizationId}/vendors
GET  /api/v1/organizations/{organizationId}/vendors/{vendorId}
PATCH /api/v1/organizations/{organizationId}/vendors/{vendorId}
POST /api/v1/organizations/{organizationId}/vendors/{vendorId}/verify
POST /api/v1/organizations/{organizationId}/vendors/{vendorId}/suspend
POST /api/v1/organizations/{organizationId}/vendors/{vendorId}/restore
```

## 84. Legal Matter APIs

```text
GET  /api/v1/organizations/{organizationId}/legal-matters
POST /api/v1/organizations/{organizationId}/legal-matters
GET  /api/v1/organizations/{organizationId}/legal-matters/{legalMatterId}
PATCH /api/v1/organizations/{organizationId}/legal-matters/{legalMatterId}
POST /api/v1/organizations/{organizationId}/legal-matters/{legalMatterId}/assign
POST /api/v1/organizations/{organizationId}/legal-matters/{legalMatterId}/close
```

Legal notes:

```text
GET  /api/v1/organizations/{organizationId}/legal-matters/{legalMatterId}/notes
POST /api/v1/organizations/{organizationId}/legal-matters/{legalMatterId}/notes
```

## 85. Legal Notice APIs

```text
POST /api/v1/organizations/{organizationId}/legal-matters/{legalMatterId}/notices
POST /api/v1/organizations/{organizationId}/legal-notices/{noticeId}/approve
POST /api/v1/organizations/{organizationId}/legal-notices/{noticeId}/issue
```

## 86. Document APIs

```text
POST /api/v1/organizations/{organizationId}/documents/upload-intent
POST /api/v1/organizations/{organizationId}/documents/finalize-upload
GET  /api/v1/organizations/{organizationId}/documents/{documentId}
POST /api/v1/organizations/{organizationId}/documents/{documentId}/replace
POST /api/v1/organizations/{organizationId}/documents/{documentId}/approve
POST /api/v1/organizations/{organizationId}/documents/{documentId}/reject
POST /api/v1/organizations/{organizationId}/documents/{documentId}/archive
```

## 87. Document Upload Intent

Illustrative request:

```json
{
  "fileName": "payment-evidence.pdf",
  "mimeType": "application/pdf",
  "fileSize": 450000,
  "documentType": "payment_evidence",
  "resourceType": "payment",
  "resourceId": "payment-id"
}
```

Illustrative response:

```json
{
  "data": {
    "uploadId": "upload-id",
    "uploadUrl": "short-lived-signed-url",
    "expiresAt": "2026-07-30T21:00:00Z"
  }
}
```

## 88. Document Finalization

```text
POST /documents/finalize-upload
```

The server should verify:

* Upload intent
* Storage object
* File size
* MIME type
* Checksum
* Malware-scan status
* Organization scope
* Related-resource access

## 89. Sensitive Document Access

```text
POST /api/v1/organizations/{organizationId}/documents/{documentId}/access
```

Response may provide a short-lived URL.

The request must validate:

* `document.read_sensitive`
* Scope
* Assignment
* Related resource
* Document classification
* Retention and legal-hold state

## 90. Communication APIs

```text
GET  /api/v1/organizations/{organizationId}/communications
POST /api/v1/organizations/{organizationId}/communications
GET  /api/v1/organizations/{organizationId}/communication-threads/{threadId}
POST /api/v1/organizations/{organizationId}/communication-threads/{threadId}/messages
```

## 91. Notification APIs

```text
GET  /api/v1/users/me/notifications
POST /api/v1/users/me/notifications/{notificationId}/mark-read
GET  /api/v1/organizations/{organizationId}/notifications
POST /api/v1/organizations/{organizationId}/notifications/{notificationId}/retry
POST /api/v1/organizations/{organizationId}/notifications/{notificationId}/cancel
```

## 92. Notification Template APIs

```text
GET  /api/v1/organizations/{organizationId}/notification-templates
POST /api/v1/organizations/{organizationId}/notification-templates
PATCH /api/v1/organizations/{organizationId}/notification-templates/{templateId}
POST /api/v1/organizations/{organizationId}/notification-templates/{templateId}/approve
```

## 93. Audit APIs

```text
GET /api/v1/organizations/{organizationId}/audit-events
GET /api/v1/organizations/{organizationId}/audit-events/{auditEventId}
POST /api/v1/organizations/{organizationId}/audit-exports
```

Audit APIs should generally be read-only.

## 94. Configuration APIs

```text
GET  /api/v1/organizations/{organizationId}/configuration
PATCH /api/v1/organizations/{organizationId}/configuration
POST /api/v1/organizations/{organizationId}/configuration/{configurationId}/approve
GET  /api/v1/organizations/{organizationId}/configuration-history
```

## 95. AI APIs

```text
POST /api/v1/organizations/{organizationId}/applications/{applicationId}/ai-summary
POST /api/v1/organizations/{organizationId}/maintenance-requests/{requestId}/ai-classification
POST /api/v1/organizations/{organizationId}/communications/ai-draft
POST /api/v1/organizations/{organizationId}/listings/ai-description
GET  /api/v1/organizations/{organizationId}/ai-activity
```

AI responses must be labeled as generated assistance.

## 96. AI Asynchronous Response

Illustrative response:

```json
{
  "data": {
    "jobId": "ai-job-id",
    "status": "queued",
    "resultUrl": "/api/v1/jobs/ai-job-id"
  }
}
```

AI failure must not change the underlying business resource.

## 97. Job APIs

```text
GET  /api/v1/jobs/{jobId}
POST /api/v1/jobs/{jobId}/cancel
POST /api/v1/jobs/{jobId}/retry
```

Job access must remain organization and actor scoped.

## 98. Import APIs

```text
POST /api/v1/organizations/{organizationId}/imports
GET  /api/v1/organizations/{organizationId}/imports
GET  /api/v1/organizations/{organizationId}/imports/{importId}
POST /api/v1/organizations/{organizationId}/imports/{importId}/validate
POST /api/v1/organizations/{organizationId}/imports/{importId}/approve
POST /api/v1/organizations/{organizationId}/imports/{importId}/execute
```

## 99. Export APIs

```text
POST /api/v1/organizations/{organizationId}/exports
GET  /api/v1/organizations/{organizationId}/exports
GET  /api/v1/organizations/{organizationId}/exports/{exportId}
POST /api/v1/organizations/{organizationId}/exports/{exportId}/cancel
```

Sensitive exports may require approval and step-up authentication.

## 100. Reporting APIs

```text
GET /api/v1/organizations/{organizationId}/reports/unit-availability
GET /api/v1/organizations/{organizationId}/reports/application-funnel
GET /api/v1/organizations/{organizationId}/reports/financial-position
GET /api/v1/organizations/{organizationId}/reports/maintenance-performance
GET /api/v1/organizations/{organizationId}/reports/renewal-pipeline
```

Large reports should use asynchronous export APIs.

## 101. Platform Administration APIs

```text
GET  /api/v1/platform/organizations
GET  /api/v1/platform/organizations/{organizationId}
POST /api/v1/platform/organizations/{organizationId}/suspend
POST /api/v1/platform/organizations/{organizationId}/restore
GET  /api/v1/platform/users
GET  /api/v1/platform/security-events
GET  /api/v1/platform/audit-events
```

Platform endpoints must not grant unrestricted customer-data access by default.

## 102. Support Session APIs

```text
POST /api/v1/platform/support-sessions
GET  /api/v1/platform/support-sessions/{supportSessionId}
POST /api/v1/platform/support-sessions/{supportSessionId}/approve
POST /api/v1/platform/support-sessions/{supportSessionId}/activate
POST /api/v1/platform/support-sessions/{supportSessionId}/terminate
```

Illustrative request:

```json
{
  "organizationId": "organization-id",
  "supportCaseId": "case-id",
  "scope": [
    "configuration.read",
    "workflow_metadata.read"
  ],
  "purpose": "Investigate failed agreement notification.",
  "requestedDurationMinutes": 60
}
```

## 103. Webhook Architecture

Habitacores may receive webhooks from:

* Payment providers
* Electronic-signature providers
* Email providers
* SMS providers
* WhatsApp providers
* Identity providers
* File-scanning providers
* AI providers where asynchronous
* Monitoring systems

## 104. Webhook Routes

Examples:

```text
POST /api/v1/webhooks/payments/{provider}
POST /api/v1/webhooks/signatures/{provider}
POST /api/v1/webhooks/email/{provider}
POST /api/v1/webhooks/sms/{provider}
POST /api/v1/webhooks/whatsapp/{provider}
POST /api/v1/webhooks/identity/{provider}
```

## 105. Webhook Security

Webhook receivers must validate:

* Provider
* Signature
* Timestamp
* Replay window
* Expected content type
* Payload size
* Event identifier
* Provider account or tenant
* Environment
* Supported event type

Webhook endpoints must not trust payload content before verification.

## 106. Webhook Idempotency

Each provider event should use:

```text
provider
+ provider_event_id
```

as an idempotency basis where available.

Duplicate delivery must not create duplicate:

* Payments
* Refunds
* Signatures
* Notifications
* Identity-verification outcomes

## 107. Webhook Processing

Preferred flow:

```text
Receive
→ Verify
→ Record provider event
→ Acknowledge
→ Process asynchronously
→ Update business state through domain service
→ Record audit
```

A webhook must not bypass domain authorization and state rules.

## 108. Webhook Response Timing

Webhook receivers should return a timely provider-compatible response.

Expensive processing should occur asynchronously after secure event persistence.

## 109. External Provider Adapter APIs

Application domain modules should call internal provider interfaces.

Example:

```typescript
interface PaymentProvider {
  verifyWebhook(input: ProviderWebhookInput): Promise<VerifiedProviderEvent>;
  initiateRefund(input: RefundRequest): Promise<RefundResult>;
  queryTransaction(reference: string): Promise<TransactionStatus>;
}
```

Domain code should not depend directly on a provider SDK.

## 110. Payment Provider Integration

Payment-provider integration should support:

* Transaction initiation where enabled
* Provider references
* Pending state
* Webhook confirmation
* Duplicate-event handling
* Reconciliation
* Refund execution
* Failure classification

Provider confirmation does not independently override Habitacores authorization and allocation controls.

## 111. Signature Provider Integration

Signature-provider integration should support:

* Signature request creation
* Signer identity
* Agreement version
* Signature status
* Provider reference
* Webhook verification
* Evidence document
* Decline status
* Expiry
* Cancellation

The signed Habitacores agreement version must remain immutable.

## 112. Communication Provider Integration

Provider adapters should support:

* Send request
* Provider reference
* Delivery status
* Failure status
* Retry classification
* Template mapping
* Rate-limit handling

Communication-provider failure must not erase the underlying business event.

## 113. Rate Limiting

Rate limiting should protect:

* Login
* Registration
* Password reset
* Verification challenges
* Public inquiry submission
* Public inspection requests
* Search
* File upload intents
* AI generation
* Export generation
* Webhook receivers
* Support-access requests

## 114. Rate-Limit Scope

Rate limits may apply by:

* IP address
* User
* Organization
* API key
* Endpoint
* Provider
* Feature
* Time window

Rate limits must avoid unfairly blocking an entire shared network without reasonable safeguards.

## 115. Rate-Limit Response

```http
429 Too Many Requests
Retry-After: 60
```

Illustrative response:

```json
{
  "error": {
    "code": "RATE_LIMIT_EXCEEDED",
    "message": "Too many requests. Please try again later.",
    "retryAfterSeconds": 60,
    "correlationId": "01HXYZ..."
  }
}
```

## 116. API Security Controls

The API architecture must support:

* TLS
* Secure session handling
* Token validation
* CSRF protection where cookie authentication is used
* CORS restrictions
* Input validation
* Output encoding
* Rate limiting
* Secret management
* Dependency security
* Webhook verification
* Request-size limits
* File validation
* Authorization
* Audit
* Logging
* Monitoring

## 117. Cross-Origin Resource Sharing

CORS must allow only approved origins.

Production configuration should not use unrestricted:

```text
Access-Control-Allow-Origin: *
```

for authenticated APIs.

## 118. CSRF Protection

Where cookie-based authentication is used, state-changing requests must use approved CSRF controls.

Potential controls include:

* SameSite cookies
* CSRF tokens
* Origin validation
* Referer validation
* Framework-level protections

## 119. Injection Protection

APIs must protect against:

* SQL injection
* Command injection
* Template injection
* Header injection
* Path traversal
* Prompt injection
* Malicious file metadata
* Untrusted webhook payloads

Parameterized queries and validated schemas are mandatory.

## 120. Sensitive Response Protection

Sensitive responses should use:

```http
Cache-Control: no-store
```

where appropriate.

Sensitive fields should not be included in:

* Public caches
* Shared caches
* Browser history where avoidable
* Analytics payloads
* Error logs
* Client telemetry

## 121. Cache Policy

Public listing APIs may use controlled caching.

Tenant-owned APIs must use cache keys that include:

* Organization
* Permission-sensitive context
* Resource
* Version
* Relevant filter

Financial, agreement, occupancy, and authorization state should not use stale caches that can produce incorrect actions.

## 122. Correlation Identifiers

Every API request should have a correlation identifier.

Preferred response header:

```http
X-Correlation-Id: 01HXYZ...
```

The identifier should connect:

* API request
* Domain command
* Database transaction
* Background job
* External provider call
* Audit event
* Error record

## 123. Logging

API logs should include:

* Correlation ID
* Route
* Method
* Status code
* Duration
* Actor identifier where lawful
* Organization identifier where appropriate
* Error code
* Provider dependency
* Deployment environment

Logs should not include:

* Passwords
* Authentication tokens
* Full government identification
* Raw payment evidence
* Full signed agreements
* Unredacted legal notes
* Secrets

## 124. Metrics

API metrics may include:

* Request count
* Error rate
* Latency
* Authorization denial rate
* Rate-limit count
* Provider failure rate
* Webhook duplicate rate
* File-upload failure rate
* Queue delay
* Database query duration

## 125. API Performance Targets

Ordinary synchronous endpoints should target:

```text
Median response time:
Less than 500 milliseconds

95th-percentile response time:
Less than 1.5 seconds
```

These targets exclude:

* Large file transfer
* AI generation
* Report generation
* External payment confirmation
* Electronic-signature processing
* Bulk import
* Bulk export

## 126. Timeout Policy

External calls must use explicit timeouts.

Example categories:

```text
Database query timeout
Provider connection timeout
Provider response timeout
AI generation timeout
Webhook processing timeout
File-scanning timeout
```

Timeout values must be defined through implementation and provider-specific ADRs.

## 127. Retry Policy

Retries may be used for:

* Safe idempotent reads
* Provider network failures
* Notification sends
* Webhook background processing
* File scanning
* Report jobs

Retries must not blindly repeat non-idempotent financial or agreement actions.

## 128. Circuit Breakers

Circuit breakers may be used for unstable external providers.

When a provider circuit is open:

* The API should return a clear pending or unavailable result.
* Manual workflows should remain available where possible.
* Business state must not be falsified.

## 129. API Mobile Readiness

The API must support future Android and iOS applications through:

* Stable versioned contracts
* Compact payloads
* Pagination
* Safe retry
* Idempotency
* Explicit status
* Direct resource links
* Token-based authentication compatibility
* Low-bandwidth responses

## 130. Low-Bandwidth API Design

The API should support constrained networks through:

* Small response bodies
* Selective fields
* Pagination
* Compressed responses
* Resumable or retryable uploads
* Avoidance of unnecessary nested resources
* Asynchronous long-running operations
* Stable retry semantics

## 131. Accessibility Support

Although accessibility primarily concerns the interface, APIs should support it by providing:

* Clear field labels
* Structured validation errors
* Stable status descriptions
* Human-readable messages
* Machine-readable codes
* Ordered workflow steps
* Accessible notice content

## 132. Localization

API contracts should separate:

* Stable code
* User-facing label
* Localized message

Example:

```json
{
  "status": "pending_verification",
  "statusLabel": "Pending verification"
}
```

Stable codes must not change when display language changes.

## 133. API Documentation

The implementation should produce machine-readable API documentation.

Potential standard:

```text
OpenAPI 3.1
```

Documentation should include:

* Route
* Method
* Authentication
* Permissions
* Parameters
* Request schema
* Response schema
* Errors
* Examples
* Idempotency
* Rate limits
* Audit behavior
* Version status

## 134. API Contract Ownership

Each endpoint should have:

* Owning module
* Business owner
* Technical owner
* Permission mapping
* Data mapping
* Test coverage
* Version status

## 135. API Deprecation

Deprecated endpoints should provide:

* Deprecation notice
* Replacement endpoint
* Sunset date
* Migration guidance
* Monitoring of remaining consumers

Potential headers:

```http
Deprecation: true
Sunset: Wed, 30 Jun 2027 23:59:59 GMT
```

## 136. Internal API Compatibility

Internal APIs may evolve faster, but changes must still preserve:

* Domain invariants
* Authorization
* Tests
* Deployment compatibility
* Background-worker compatibility
* Migration sequencing

## 137. API Test Strategy

API testing must include:

1. Schema validation
2. Authentication
3. Authorization
4. Organization isolation
5. Assignment scope
6. Field-level protection
7. State-transition validation
8. Approval requirements
9. Separation of duties
10. Financial invariants
11. Agreement immutability
12. Occupancy conflict
13. Idempotency
14. Concurrency
15. Pagination
16. Filtering
17. Sorting
18. Rate limiting
19. Webhook signatures
20. Duplicate webhook events
21. Provider failure
22. File upload
23. Sensitive document access
24. Audit-event generation
25. Error consistency

## 138. Contract Tests

External provider integrations should use contract tests for:

* Payment webhooks
* Refund execution
* Signature callbacks
* Email delivery events
* SMS delivery events
* WhatsApp delivery events
* Identity verification
* File scanning
* AI provider responses

## 139. Tenant-Isolation API Tests

Every organization-owned endpoint must test:

* Same-organization access
* Cross-organization denial
* Missing membership
* Suspended membership
* Expired assignment
* Invalid related-organization relationship
* Public-access denial
* Support-session scope
* Platform-role restrictions

## 140. Financial API Tests

Tests must include:

* Payment verification
* Payment rejection
* Duplicate provider reference
* Allocation within limits
* Allocation above payment balance
* Allocation above charge balance
* Currency mismatch
* Concurrent allocation
* Reversal
* Refund approval
* Duplicate refund execution
* Receipt generation

## 141. Agreement API Tests

Tests must include:

* Draft creation
* Legal review
* Approval
* Issue
* Issued-version immutability
* Signature
* Activation without signature
* Activation without payment
* Amendment
* Termination
* Cross-organization denial

## 142. Occupancy API Tests

Tests must include:

* Planned occupancy
* Activation
* Conflicting occupancy
* Unit readiness failure
* Agreement mismatch
* Household change
* End occupancy
* Resident own-record access
* Co-Resident restricted access

## 143. Webhook Tests

Webhook tests must include:

* Valid signature
* Invalid signature
* Expired timestamp
* Duplicate event
* Unknown event type
* Unsupported provider
* Oversized payload
* Processing failure
* Retry
* Cross-environment event rejection

## 144. API Acceptance Criterion — Tenant Isolation

```text
Given:
User A belongs only to Organization A.
Payment B belongs to Organization B.

When:
User A sends a direct request for Payment B.

Then:
The API denies the request.
No private Payment B information is returned.
No related payment evidence is exposed.
```

## 145. API Acceptance Criterion — Payment Allocation

```text
Given:
A verified payment has ₦400,000 unallocated.
A charge has ₦500,000 outstanding.

When:
A Finance Reviewer attempts to allocate ₦450,000.

Then:
The API returns a validation or conflict response.
No allocation is created.
Payment and charge balances remain unchanged.
```

## 146. API Acceptance Criterion — Idempotent Payment Webhook

```text
Given:
A valid payment-provider event has already been processed.

When:
The provider delivers the same event again.

Then:
The API acknowledges the duplicate safely.
No duplicate payment or verification event is created.
```

## 147. API Acceptance Criterion — Agreement Immutability

```text
Given:
Agreement Version 3 has been issued.

When:
A user attempts to update the issued terms directly.

Then:
The API rejects the update.
The user must create an amendment or replacement version.
Version 3 remains unchanged.
```

## 148. API Acceptance Criterion — Occupancy Conflict

```text
Given:
Unit 001 has an active incompatible occupancy.

When:
An authorized user calls the occupancy activation endpoint for another incompatible occupancy.

Then:
The API returns 409 Conflict.
No second occupancy becomes active.
The unit status remains unchanged.
```

## 149. API Acceptance Criterion — Assignment Scope

```text
Given:
Vendor Worker A is assigned only to Work Order 001.

When:
Vendor Worker A requests Work Order 002.

Then:
The API denies access.
No Work Order 002 information is returned.
```

## 150. API Acceptance Criterion — Unknown Pilot Data

```text
Given:
Block B unit count is not confirmed.

When:
Block B is created through the API.

Then:
The API accepts a null confirmed unit count with pending status.
The API does not require zero.
The API does not create placeholder units.
```

## 151. API Acceptance Criterion — AI Failure

```text
Given:
An authorized reviewer requests an AI summary.

When:
The AI provider is unavailable.

Then:
The API returns a clear unavailable or pending response.
The application remains accessible.
No application status changes.
```

## 152. API Traceability

Each endpoint should map to:

* User journey
* Role
* Permission
* Business rule
* Functional requirement
* Domain entity
* Database table
* Audit event
* Automated test
* Retention rule

Example:

```text
Endpoint:
POST /payments/{paymentId}/verify

Journey:
JRN-FIN-005

Role:
Finance Reviewer

Permission:
payment.verify

Domain:
Payment

Data:
payments
payment_evidence
audit_events

Audit:
PaymentVerified

Tests:
Authorized verification
Unauthorized verification
Self-verification restriction
Cross-tenant denial
Duplicate reference
```

## 153. MVP API Scope

The MVP must include APIs for:

### Public and Identity

* Public listings
* Registration
* Authentication
* Inquiry
* Inspection request

### Organizations and Access

* Organizations
* Memberships
* Roles
* Assignments
* Relationships

### Property

* Properties
* Communities
* Buildings
* Blocks
* Optional floors
* Units
* Amenities

### Leasing

* Listings
* Inquiries
* Inspections
* Applications
* Application parties
* Documents
* Reviews
* Decisions
* Offers

### Finance

* Charges
* Payments
* Evidence
* Verification
* Allocation
* Receipts
* Reversals
* Refunds
* Deposits

### Agreements and Occupancy

* Agreement templates
* Agreements
* Versions
* Signatures
* Activation
* Occupancies
* Move-in
* Household
* Renewal
* Move-out

### Operations

* Maintenance requests
* Work orders
* Vendors
* Legal matters
* Documents
* Communications
* Notifications
* Audit
* Configuration
* Support sessions

## 154. Deferred API Capabilities

The following may be deferred:

* Public developer API
* API-key administration
* OAuth client registration
* Enterprise identity federation
* GraphQL
* Property-sale APIs
* Mortgage APIs
* Insurance APIs
* Rent-financing APIs
* Investment APIs
* Multi-currency settlement
* Government integration
* Smart-building telemetry
* Biometric APIs
* Automated applicant scoring
* Autonomous approval APIs
* Public vendor marketplace
* Advanced policy-simulation APIs

Deferred APIs must not weaken the MVP contract.

## 155. API Risks

### 155.1 Generic Mutation Risk

Risk:

A generic update endpoint bypasses domain workflows.

Controls:

* Explicit commands
* State validation
* Permission mapping
* Audit
* Contract tests

### 155.2 Cross-Tenant Identifier Risk

Risk:

A valid identifier from another tenant is accepted.

Controls:

* Organization context
* Resource ownership validation
* RLS
* Tenant-isolation tests

### 155.3 Overexposed Response Risk

Risk:

A response includes sensitive fields not needed by the user.

Controls:

* Response schemas
* Field-level authorization
* Resource-specific serializers
* Security tests

### 155.4 Duplicate Financial Action Risk

Risk:

Retry creates duplicate payment, allocation, refund, or receipt.

Controls:

* Idempotency keys
* Provider event identifiers
* Transactions
* Unique constraints
* Tests

### 155.5 Provider Callback Risk

Risk:

Forged webhook changes financial or agreement state.

Controls:

* Signature verification
* Timestamp validation
* Event uniqueness
* Provider-account validation
* Asynchronous domain processing

### 155.6 Version Drift Risk

Risk:

Web and mobile clients interpret an endpoint differently.

Controls:

* API versioning
* OpenAPI contracts
* Compatibility tests
* Deprecation policy

### 155.7 Authorization Cache Risk

Risk:

Revoked access remains active through stale cache.

Controls:

* Short cache lifetime
* Invalidation
* Session revalidation
* Sensitive-command live checks

## 156. Unresolved API Decisions

The following remain pending:

* Final public API domain
* Final API route prefix
* Final API version format
* Final session versus bearer-token strategy
* Final organization-context mechanism
* Final response envelope
* Final error schema
* Final pagination strategy
* Final filtering syntax
* Final sorting syntax
* Final field-selection support
* Final resource-expansion support
* Final financial amount representation
* Final idempotency storage
* Final concurrency-header strategy
* Final API documentation generator
* Final OpenAPI tooling
* Final API gateway
* Final rate-limit provider
* Final request-size limits
* Final upload-size limits
* Final webhook queue
* Final provider retry policy
* Final timeout values
* Final CORS origins
* Final CSRF implementation
* Final support-session token design
* Final API audit granularity
* Final sensitive-response cache policy
* Final mobile-token lifecycle
* Final partner authentication
* Final deprecation window
* Final endpoint ownership model
* Final GraphQL decision
* Final bulk-operation strategy
* Final report API strategy
* Final search API design
* Final webhook raw-payload retention

These decisions must be resolved before the affected API becomes production dependent.

## 157. API Governance

Changes to approved API architecture require:

1. Change description
2. Business rationale
3. Affected endpoint
4. Affected user journey
5. Affected role
6. Permission impact
7. Tenant-isolation impact
8. Data impact
9. Financial impact
10. Agreement impact
11. Occupancy impact
12. Privacy impact
13. Security impact
14. Provider impact
15. Backward-compatibility impact
16. Mobile-client impact
17. Documentation impact
18. Audit impact
19. Monitoring impact
20. Migration impact
21. Test impact
22. Deprecation plan where required
23. Rollback or forward-recovery strategy
24. Product Visionary approval
25. Chief Product Architect approval
26. Effective version

## 158. Current API Architecture Decision

Habitacores will use a resource-oriented HTTP API with explicit command endpoints for critical domain transitions.

The primary API flow is:

```text
Authenticated Request
→ Organization Context
→ Permission
→ Scope
→ Assignment
→ Record-State Validation
→ Approval and Separation-of-Duties Validation
→ Domain Command or Query
→ Database Transaction
→ Audit Event
→ Notification or Background Job
→ Structured Response
```

The API architecture must preserve:

* Organization isolation
* Server-side authorization
* Assignment-specific access
* Explicit workflow commands
* Stable resource identifiers
* Precise financial values
* Payment verification
* Transactional allocation
* Immutable agreement versions
* Occupancy conflict prevention
* Secure documents
* Reliable webhook processing
* Idempotency
* Concurrency control
* Consistent errors
* Observability
* Mobile readiness
* Low-bandwidth performance
* Backward-compatible evolution
* Future SaaS extensibility

No client-supplied organization identifier, role title, payment screenshot, provider callback, AI output, direct status value, browser restriction, cached permission, pilot-team relationship, or external integration may independently authorize or finalize a protected Habitacores business action.
