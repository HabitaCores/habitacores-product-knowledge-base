# ADR-015 — Notifications, Email, SMS, Push, and Communication Delivery Architecture

## Decision Record

| Field | Value |
|---|---|
| Product | Habitacores |
| Decision | Notifications, Email, SMS, Push, and Communication Delivery Architecture |
| ADR | ADR-015 |
| Version | 0.1 |
| Status | Proposed |
| Decision Type | Foundational Communication, Reliability, and Consent Architecture |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Corporate Domain | `habitacores.com` |
| Corporate Mail Provider | Zoho Mail |
| Notification Architecture | Provider-neutral orchestration layer |
| Durable Dispatch | Transactional outbox under ADR-010 |
| Supported Channels | In-app, email, mobile push, SMS, and approved future channels |
| Initial Mobile Push Direction | Expo Push Service through an adapter |
| Transactional Email Provider | Deferred |
| SMS Provider | Deferred |
| In-App Notification Store | Habitacores PostgreSQL |
| Template Authority | Versioned Habitacores template registry |
| Preference Authority | Habitacores notification-preference records |
| Consent Authority | Habitacores consent and suppression records |
| Delivery Semantics | At least once internally; effectively once per provider request through idempotency |
| Material Communication | Durable business record plus delivery attempts |
| AI-Generated Communication | Draft only unless an approved use case allows bounded generation and human review |
| Last Updated | 1 August 2026 |

## 1. Context

Habitacores will communicate with:

- Prospects
- applicants
- guarantors
- tenants
- residents
- landlords
- property owners
- agents
- estate managers
- maintenance personnel
- vendors
- finance reviewers
- legal professionals
- support personnel
- organization administrators

Communications may concern:

- Account verification
- authentication and security
- organization invitations
- property enquiries
- application submissions
- application-review updates
- document requests
- payment evidence
- payment verification
- receipts
- agreements
- occupancy
- rent reminders
- maintenance requests
- inspections
- legal notices
- support cases
- system incidents
- privacy and security events

Habitacores must support communication across web and mobile while avoiding:

- Duplicate messages
- silent delivery failure
- unauthorized disclosure
- cross-tenant communication
- marketing without consent
- provider lock-in
- unversioned templates
- uncontrolled AI-generated messages
- legally significant actions represented only by ephemeral push notifications
- sensitive information exposed on device lock screens
- retries that send the same message repeatedly
- preview environments contacting real users

## 2. Decision Drivers

The decision is driven by the need to:

1. Support web, Android, and iOS users.
2. support multiple communication channels.
3. separate business events from delivery providers.
4. preserve durable delivery intent.
5. track delivery attempts and outcomes.
6. prevent duplicate sends.
7. honor user preferences, consent, and opt-outs.
8. preserve legally and operationally important communication evidence.
9. protect private information.
10. support tenant-aware communication.
11. support organization branding without weakening platform controls.
12. support retries and provider outages.
13. preserve provider portability.
14. support communication localization.
15. support accessibility.
16. support low-bandwidth users.
17. distinguish transactional from marketing communication.
18. coordinate web and mobile deep links.
19. constrain Responsible AI.
20. provide clear Codex implementation boundaries.

## 3. Decision

Habitacores will implement a centralized, provider-neutral communication orchestration layer.

The approved flow is:

```text
Authoritative business transaction
→ Domain or application decision
→ Audit event where required
→ Transactional outbox message
→ Notification orchestration worker
→ Recipient and policy resolution
→ Preference, consent, and suppression evaluation
→ Template-version resolution
→ Channel selection
→ Provider adapter
→ Provider request
→ Delivery status callback or receipt
→ Delivery-attempt update
→ Retry, suppression, fallback, or completion
````

External communication providers will not determine Habitacores business state.

## 4. Communication Categories

Communications will be classified as:

```text
SECURITY
TRANSACTIONAL
OPERATIONAL
LEGAL_OR_EVIDENTIARY
SERVICE_ANNOUNCEMENT
MARKETING
INTERNAL_OPERATIONS
```

The classification controls:

* Whether opt-out is permitted
* whether consent is required
* eligible channels
* message urgency
* retention
* escalation
* template review
* audit requirements
* privacy restrictions

## 5. Security Communications

Security communications may include:

* Email verification
* password recovery
* MFA changes
* unusual login alerts
* session revocation
* account lock
* credential change
* sensitive-profile change
* suspected compromise

Users may not disable security messages necessary to protect their account.

Security messages must not disclose credentials, secrets, or excessive account data.

## 6. Transactional Communications

Transactional communications are directly related to a user-requested or contractual workflow.

Examples:

* Application submitted
* document requested
* payment evidence received
* payment verified
* receipt available
* agreement ready
* maintenance request updated
* organization invitation

Transactional classification must not be used to disguise marketing.

## 7. Operational Communications

Operational communications may include:

* Service interruption
* planned maintenance
* delayed processing
* provider outage
* document-scanning delay
* significant workflow degradation

Operational messages should be limited to affected users or organizations where possible.

## 8. Legal and Evidentiary Communications

Legally or evidentially significant communication may include:

* Formal notice
* agreement issuance
* renewal notice
* payment demand
* tenancy-related notice
* compliance request
* dispute-related communication
* approved legal correspondence

A push notification, SMS, or ordinary delivery-provider status alone is not sufficient proof of the underlying legal act.

Habitacores must preserve:

* Authoritative notice record
* notice version
* issuer
* recipient
* organization
* subject resource
* creation time
* approved content
* delivery channels
* delivery attempts
* provider references
* applicable acknowledgement
* supporting audit events

## 9. Marketing Communications

Marketing communications require appropriate consent and preference enforcement.

Examples:

* Promotions
* property recommendations not directly requested
* product newsletters
* referral campaigns
* sponsored communications
* non-essential engagement campaigns

Marketing must remain distinct from security and transactional messaging.

## 10. Channel Model

Habitacores will support the following channel types:

```text
IN_APP
EMAIL
PUSH
SMS
```

Future channels may include:

```text
WHATSAPP
VOICE
WEBHOOK
```

A new channel requires a bounded architecture and compliance review.

## 11. In-App Notifications

In-app notifications will be stored in Habitacores PostgreSQL.

They may include:

* Notification ID
* recipient user
* organization context
* category
* event type
* title
* safe summary
* action type
* resource reference
* created time
* read time
* archived time
* expiry time
* priority
* template version
* source event
* deduplication key

In-app notifications remain subject to tenant isolation and authorization.

## 12. In-App Notification Authority

An in-app notification is a presentation record.

It does not independently:

* Verify payment
* approve an application
* activate an agreement
* activate occupancy
* change membership
* grant permission
* create a legal notice

It references authoritative backend state.

## 13. Notification Center

Web and mobile clients should expose a consistent notification center.

The interface may support:

* Unread count
* category filters
* read and unread state
* archive
* deep-linked action
* notification history
* security notices
* organization-specific context

The notification center must not reveal inaccessible resources after permission changes.

## 14. Email

Email will support:

* Account and security workflows
* transactional updates
* document and agreement availability
* receipts
* operational notices
* approved marketing communication
* internal operational alerts

The transactional email provider is deferred.

## 15. Zoho Mail Boundary

Zoho Mail is the approved corporate mailbox provider for the `habitacores.com` domain.

It supports human-operated mailboxes such as:

```text
support@habitacores.com
legal@habitacores.com
privacy@habitacores.com
admin@habitacores.com
```

Zoho Mail is not automatically the authoritative product-notification provider.

Corporate mailbox delivery and application-generated transactional communication are separate architectural concerns.

## 16. Email Sending Domains

Product-generated email should use an approved sending domain or subdomain.

Directional examples:

```text
notify.habitacores.com
mail.habitacores.com
updates.habitacores.com
```

Exact names are deferred.

The sending arrangement must preserve corporate mailbox operation and domain reputation.

## 17. Email Authentication

Production email delivery must use appropriately configured:

* SPF
* DKIM
* DMARC
* provider-domain verification
* return-path configuration
* bounce handling

Exact policy values require implementation verification.

## 18. Email Identity

Every sender identity must have:

* Approved display name
* approved address
* communication category
* owning function
* reply behavior
* escalation path

Examples:

```text
Habitacores Security
Habitacores Notifications
Habitacores Support
Habitacores Legal
```

## 19. Reply Handling

Messages must declare whether they are:

* Replyable
* routed to support
* routed to an organization
* no-reply with an alternative contact path

Habitacores should avoid no-reply addresses where a legitimate response is expected.

Inbound reply processing requires a later bounded design where automation is involved.

## 20. Mobile Push

Mobile push notifications will initially use the Expo notification ecosystem through a Habitacores provider adapter.

The adapter must hide provider-specific token and receipt structures from domain code.

Push delivery is best effort.

It must not be the only channel or authoritative record for critical legal, financial, or security communication.

## 21. Push Device Registration

Push tokens will be associated with application-level device registrations.

The record may contain:

* Device-registration ID
* user ID
* installation ID
* platform
* application version
* environment
* provider
* token reference
* token status
* registered time
* last successful delivery
* last failure
* revoked time

Push tokens must not appear in ordinary logs.

## 22. Push Token Lifecycle

Push tokens may:

* Rotate
* expire
* become invalid
* become associated with a reinstalled application
* become unavailable after permission revocation

Permanent token assumptions are prohibited.

Invalid or unregistered-device responses must deactivate or retire the applicable registration.

## 23. Push Privacy

Lock-screen content must be minimized.

Push messages should avoid displaying:

* Full payment amounts where unnecessary
* payment evidence details
* bank details
* government identifiers
* agreement clauses
* application-review findings
* legal dispute details
* private messages
* document filenames where sensitive

A push should generally invite the authenticated user to open Habitacores for details.

## 24. SMS

SMS will be used selectively where:

* Delivery urgency justifies cost
* a user has provided an eligible phone number
* applicable consent or transactional basis exists
* the country and sender arrangement permit the message
* content can be safely minimized
* another channel is insufficient or unavailable

The SMS provider is deferred.

## 25. SMS Content

SMS must be concise and privacy aware.

SMS should not include:

* Full identity numbers
* complete payment credentials
* private document URLs
* long-lived access tokens
* complete agreement text
* unrestricted personal information

## 26. SMS Consent and Opt-Out

Marketing SMS requires recorded consent and opt-out enforcement.

Habitacores must record:

* Recipient
* channel
* purpose
* consent state
* source
* time
* evidence
* withdrawal time
* applicable organization
* jurisdiction where needed

Provider opt-out features complement but do not replace the Habitacores consent record.

## 27. Recipient Resolution

Recipients must be resolved from current authoritative state.

Resolution may consider:

* User identity
* verified contact point
* organization membership
* role
* assignment
* resource relationship
* communication category
* user preference
* consent
* suppression
* legal requirement

A stale email or phone number copied into an event payload must not automatically remain authoritative.

## 28. Contact Verification

Communication-sensitive contact points should carry verification state.

Directional states:

```text
UNVERIFIED
PENDING
VERIFIED
REVOKED
BOUNCED
COMPLAINT
INVALID
```

Sensitive communication should not be sent to an unverified or invalid address unless an approved workflow specifically requires it.

## 29. Notification Preferences

Habitacores will maintain notification preferences by:

* User
* category
* event type where necessary
* channel
* organization context where appropriate

Preferences may include:

```text
ENABLED
DISABLED
REQUIRED
INHERITED
```

Required security or legal communications may override ordinary user preferences where justified.

## 30. Preference Defaults

Defaults must be explicit.

A missing preference must not be interpreted inconsistently by different channels.

Defaults should distinguish:

* Required communication
* expected transactional communication
* optional operational communication
* marketing communication

## 31. Suppression

Habitacores will maintain suppression records for conditions including:

* Hard bounce
* repeated soft bounce
* complaint
* opt-out
* invalid phone number
* invalid push token
* administrative suppression
* security hold
* legal restriction

Suppression evaluation occurs before provider dispatch.

## 32. Preference and Suppression Authority

Provider suppression lists may be consulted, but Habitacores must maintain its own normalized suppression state.

This preserves:

* Multi-provider portability
* user preference enforcement
* channel coordination
* auditability
* provider migration

## 33. Templates

All product-generated communications will use a versioned template registry.

A template should define:

* Template ID
* version
* channel
* category
* event type
* locale
* subject where applicable
* content structure
* required variables
* optional variables
* owning team
* review state
* effective date
* retirement date
* sensitivity classification

## 34. Template Immutability

A template version used for a material communication should remain reproducible.

Editing a published template creates a new version rather than silently changing historical meaning.

## 35. Template Rendering

Templates must render from validated structured data.

Approved:

```text
recipient_display_name
property_display_name
application_reference
due_date
safe_action_url
```

Unrestricted objects and arbitrary provider payloads must not be injected directly into templates.

## 36. Template Validation

Template validation should detect:

* Missing variables
* unknown variables
* unsafe HTML
* broken links
* invalid locale
* unsupported channel
* prohibited sensitive values
* invalid sender identity
* missing text alternative
* oversized SMS or push content

## 37. Localization

Templates should support locale-specific versions.

Localization must preserve:

* Business meaning
* legal meaning
* date format
* currency format
* number format
* appropriate terminology
* accessibility

Machine translation must not be used automatically for legally significant communication without approved review.

## 38. Tenant-Aware Branding

Organizations may receive controlled branding capability.

Allowed branding may include:

* Organization display name
* approved logo
* approved color accents
* support contact
* property or community name

Organization-provided content must be sanitized and must not replace Habitacores security, legal, or platform identity requirements.

## 39. Platform and Organization Sender Identity

The communication must make clear whether it originates from:

* Habitacores
* a participating organization through Habitacores
* a named authorized user
* a system workflow

The architecture must prevent sender impersonation.

## 40. Communication Intent

A durable notification intent will represent the decision to communicate.

Directional fields:

```text
notification_intent_id
event_type
category
organization_id
recipient_user_id
template_id
template_version
priority
scheduled_at
expires_at
deduplication_key
source_event_id
source_resource_type
source_resource_id
status
```

Contact details and rendered content should be managed according to classification and retention requirements.

## 41. Delivery Attempts

Each provider attempt should be recorded separately.

Directional fields:

```text
delivery_attempt_id
notification_intent_id
channel
provider
provider_message_id
attempt_number
requested_at
accepted_at
delivered_at
failed_at
status
failure_class
next_retry_at
provider_event_id
```

## 42. Notification States

Directional notification states:

```text
PENDING
SUPPRESSED
SCHEDULED
PROCESSING
PARTIALLY_DISPATCHED
DISPATCHED
COMPLETED
FAILED
CANCELED
EXPIRED
```

The exact state machine will be refined during implementation.

## 43. Delivery States

Directional channel-delivery states:

```text
QUEUED
PROVIDER_ACCEPTED
SENT
DELIVERED
READ
FAILED_TEMPORARY
FAILED_PERMANENT
BOUNCED
COMPLAINT
UNSUBSCRIBED
CANCELED
UNKNOWN
```

Not every provider or channel supports every state.

## 44. Accepted Is Not Delivered

Provider acceptance means the provider accepted the request.

It does not prove:

* Device receipt
* inbox delivery
* human reading
* legal acknowledgement
* successful action

The user interface and audit records must use precise terminology.

## 45. Read State

In-app read state may be authoritative for the Habitacores notification center.

Email open tracking, push interaction, and similar provider signals are probabilistic and privacy-sensitive.

They must not be represented as definitive human acknowledgement.

## 46. Deduplication

Each material notification intent must have a stable deduplication strategy.

A directional key may combine:

```text
event type
recipient
resource
template version
communication purpose
```

The deduplication window must match the workflow.

## 47. Idempotency

Provider adapters should use provider-supported idempotency where available.

Habitacores must also enforce internal idempotency because provider behavior may differ.

Retries must not create uncontrolled duplicate messages.

## 48. Transactional Outbox

Notification intent begins from ADR-010 transactional outbox processing.

Where communication is triggered by an authoritative state change, the business state, required audit record, and outbox message should be committed in the same database transaction where applicable.

The provider network request occurs later in a worker.

## 49. No Provider Calls in Authoritative Transactions

Authoritative database transactions must not wait on:

* Email APIs
* push APIs
* SMS APIs
* WhatsApp APIs
* template-provider APIs

Provider outages must not hold database locks or create ambiguous business commits.

## 50. Retry Policy

Retry policy will distinguish:

* Temporary network failure
* provider timeout
* provider rate limit
* transient provider error
* invalid recipient
* hard bounce
* opt-out
* invalid token
* malformed request
* prohibited content

Permanent failures must not be retried indefinitely.

## 51. Retry Scheduling

Retries should use:

* Bounded attempts
* exponential backoff
* jitter
* provider-specific limits
* message expiry
* current-state revalidation

A delayed communication may no longer be appropriate after its event expires.

## 52. Current-State Revalidation

Before delayed dispatch or retry, the worker may need to revalidate:

* Recipient membership
* contact validity
* user preference
* consent
* suppression
* resource status
* message expiry
* organization relationship

Habitacores must not send stale sensitive communication merely because an old outbox message exists.

## 53. Terminal Failure

After bounded retries, a delivery attempt enters a terminal outcome.

Terminal failure may trigger:

* Alternative channel
* in-app alert
* operational review
* contact correction request
* legal-delivery procedure
* support task
* no further action

The response depends on communication category.

## 54. Channel Fallback

Channel fallback must be explicit.

Directional example:

```text
Security event
→ In-app
→ Email
→ SMS only when configured and justified
```

Fallback must not bypass:

* Consent
* channel eligibility
* privacy
* cost controls
* legal requirements
* user preferences where applicable

## 55. Priority

Directional priorities:

```text
LOW
NORMAL
HIGH
URGENT
```

Priority may influence:

* Queue order
* eligible channel
* retry delay
* escalation
* expiry

Priority does not bypass authorization, consent, or suppression.

## 56. Scheduling

Communications may be scheduled for:

* Rent reminders
* inspection reminders
* renewal windows
* application deadlines
* payment due dates
* maintenance appointments
* planned service notices

Scheduled messages must be canceled or recalculated when authoritative state changes.

## 57. Time Zones

Scheduled communication must use explicit time-zone rules.

The architecture should distinguish:

* Stored instant
* organization time zone
* property time zone
* recipient preference
* legally defined local time

Time-zone inference from device settings alone is insufficient for authoritative deadlines.

## 58. Quiet Hours

Optional communications may respect user or organization quiet hours.

Security, emergency, or legally required communications may use separate rules.

Quiet hours must not silently change statutory or contractual delivery requirements.

## 59. Deep Links and Action URLs

Messages may contain links into:

* `habitacores.com`
* Habitacores Web
* Habitacores Android
* Habitacores iOS

Links must use approved domains and revalidate authentication, membership, permission, and resource access.

## 60. Secure Links

Messages must not contain permanent bearer access to protected resources.

Sensitive links should be:

* Short lived
* purpose bound
* single use where appropriate
* revocable
* auditable
* redirected through authenticated Habitacores workflows

Signed storage URLs must not be embedded in long-lived email templates.

## 61. Provider Webhooks

Providers may send asynchronous events for:

* Delivery
* bounce
* complaint
* failure
* unsubscribe
* read where supported
* interaction
* invalid token

Webhook endpoints must use:

* Signature verification
* replay protection
* provider-event deduplication
* schema validation
* environment verification
* bounded payload storage
* observability
* audit where material

## 62. Webhook Authority

A provider webhook updates delivery evidence.

It does not independently change unrelated Habitacores business state.

For example, an email delivery event does not:

* Verify payment
* activate an agreement
* approve an application
* prove legal acknowledgement

## 63. Provider Event Deduplication

Provider webhook events must be deduplicated using:

* Provider
* provider account
* provider event ID
* event type where needed

Repeated callbacks must remain safe.

## 64. Environment Isolation

Communication providers and credentials must be environment specific.

Non-production environments must use:

* Test providers
* sandbox accounts
* recipient allowlists
* email sinks
* disabled external delivery
* development push projects
* test phone numbers

Preview deployments must not contact real customers by default.

## 65. Test Recipient Allowlist

Where a non-production system can send externally, recipients must be restricted to an approved allowlist.

The system must visibly label non-production communications.

## 66. Sensitive Data

Ordinary communication must minimize:

* Identity information
* financial information
* document information
* application-review details
* legal details
* maintenance-access details
* private notes

Messages should direct authenticated users to Habitacores for protected content.

## 67. Message Content Retention

Habitacores should preserve enough information to reproduce or evidence material communications.

Retention may store:

* Template and version
* structured rendering variables
* rendered snapshot where required
* recipient reference
* sender identity
* delivery attempts
* timestamps
* provider references

Highly sensitive content requires restricted access and retention controls.

## 68. Logging and Observability

Communication telemetry must follow ADR-012.

Metrics may include:

```text
notification_intents_created
notifications_suppressed
delivery_attempts
provider_accepted
delivery_confirmed
delivery_failed
hard_bounces
complaints
invalid_push_tokens
sms_opt_outs
notification_queue_age
template_render_failures
webhook_verification_failures
```

Recipient email addresses, phone numbers, and message contents must not become metric labels.

## 69. Delivery Monitoring

Dashboards should distinguish:

* Channel
* provider
* environment
* event type
* category
* result
* retry state
* queue age
* template version

Tenant identities must not appear on unrestricted dashboards.

## 70. Alerts

Priority alerts may include:

* Security email outage
* sustained notification backlog
* provider authentication failure
* widespread template-render failure
* webhook signature failure spike
* bounce or complaint spike
* invalid push-token spike
* cross-environment sending
* unexpected marketing delivery
* secret exposure in rendered content

## 71. Rate Limits

Communication dispatch must respect:

* Provider limits
* recipient limits
* organization limits
* event limits
* abuse controls
* budget limits

Rate limits must not silently suppress legally required communication without an escalation path.

## 72. Abuse Prevention

Controls should address:

* Invitation spam
* repeated OTP requests
* repeated password recovery
* property-enquiry spam
* notification flooding
* malicious recipient entry
* organization misuse
* SMS cost abuse

## 73. Cost Governance

Communication cost should be controlled through:

* Channel policy
* recipient deduplication
* SMS eligibility
* retry limits
* provider metrics
* organization quotas where appropriate
* template-size controls
* invalid-recipient suppression

Cost controls must not corrupt or conceal communication records.

## 74. Internal Notifications

Operational personnel may receive internal alerts through:

* In-app operations dashboard
* approved email
* approved incident platform
* future collaboration integrations

Internal operational alerts must remain separate from customer notification records where their purpose differs.

## 75. Support Communication

Support personnel must communicate through approved support context.

Support communication must preserve:

* Named actor
* authorized organization
* support-session context where required
* recipient
* channel
* content classification
* time
* related support case
* audit

## 76. Organization-Generated Communication

Authorized organization users may initiate approved communications.

The server must verify:

* Organization membership
* permission
* recipient relationship
* event type
* template eligibility
* channel eligibility
* consent and suppression
* rate limit
* organization status

Free-form mass communication is not approved by default.

## 77. Bulk Communication

Bulk communication requires:

* Defined audience
* authorized purpose
* communication classification
* consent basis
* template approval
* bounded batch size
* rate limiting
* suppression
* preview
* final approval
* audit
* cancellation capability
* delivery reporting

## 78. AI-Assisted Communication

AI may assist with:

* Drafting support responses
* simplifying approved content
* suggesting subject lines
* generating bounded template variants
* translation drafts
* summarizing non-authoritative information

AI output remains subject to the Responsible AI architecture.

## 79. AI Communication Restrictions

AI must not autonomously:

* Issue a legal notice
* make a payment demand
* approve or reject an application
* confirm payment verification
* activate an agreement
* activate occupancy
* invent dates or amounts
* select recipients without policy
* send material communications without approved review
* override consent or suppression

## 80. AI Content Grounding

AI-assisted drafts must be grounded in:

* Approved structured data
* approved templates
* current authoritative state
* approved policy
* permission-aware context

Raw database access and unrestricted document access remain prohibited.

## 81. Accessibility

Communication templates should support:

* Plain-text email alternative
* meaningful link labels
* readable structure
* sufficient contrast
* screen-reader compatibility
* non-image-only meaning
* understandable action instructions
* localization

## 82. Email HTML

Email HTML must be sanitized and generated from approved components or templates.

Untrusted organization or user HTML is prohibited.

## 83. Attachments

Direct email attachments should be minimized.

Sensitive documents should generally be retrieved through authenticated Habitacores access.

Where an attachment is required, it must follow:

* Authorization
* malware scanning
* classification
* size limits
* encryption policy where applicable
* retention
* audit

## 84. Legal Acknowledgement

Where acknowledgement is required, Habitacores should capture an explicit authenticated action.

Examples:

* Notice viewed
* document downloaded
* agreement reviewed
* agreement accepted
* receipt acknowledged

Email open pixels or push taps are not sufficient substitutes.

## 85. Data Model Direction

Potential PostgreSQL entities include:

```text
notification_intents
notification_recipients
notification_delivery_attempts
notification_provider_events
notification_templates
notification_template_versions
notification_preferences
communication_consents
communication_suppressions
device_registrations
in_app_notifications
communication_acknowledgements
```

Exact schemas require implementation design and must follow existing identifier, RLS, audit, and migration ADRs.

## 86. Tenant Isolation

Tenant-scoped communication records require:

* Organization ID
* RLS
* current membership validation
* permission-aware access
* restricted operational access

A platform security message may be global rather than tenant scoped, but this must be explicit.

## 87. Cross-Tenant Communication Test

```text
Given:
An agent belongs to Organization A.

And:
The agent knows a user ID associated only with Organization B.

When:
The agent attempts to send an organization communication to that user.

Then:
The operation is denied.

And:
No notification intent or provider request is created.
```

## 88. Duplicate Delivery Test

```text
Given:
A payment-verification transaction produces one notification outbox message.

And:
The worker crashes after the provider accepts the email.

When:
The message is retried.

Then:
Habitacores reuses the approved idempotency and deduplication context.

And:
The recipient does not receive uncontrolled duplicate communication.
```

## 89. Preference Test

```text
Given:
A user has disabled optional property-recommendation email.

When:
A recommendation campaign is prepared.

Then:
The email attempt is suppressed.

And:
Required security and transactional communications remain governed
by their separate categories.
```

## 90. Push Privacy Test

```text
Given:
A payment has been verified.

When:
A mobile push notification is created.

Then:
The lock-screen content contains only a minimized safe summary.

And:
The user must authenticate and pass authorization before viewing
protected payment information.
```

## 91. Invalid Token Test

```text
Given:
A push provider reports that a device token is no longer registered.

When:
The provider event is processed.

Then:
The applicable device registration is deactivated.

And:
Future push attempts do not continue indefinitely to that token.
```

## 92. Legal Notice Test

```text
Given:
An authorized legal user issues a formal tenancy notice.

When:
The transaction completes.

Then:
Habitacores preserves the approved notice content, version, issuer,
recipient, subject resource, audit event, and delivery intents.

And:
Provider delivery status is recorded separately from legal acknowledgement.
```

## 93. Preview Isolation Test

```text
Given:
A pull-request preview creates a notification event.

When:
The notification worker processes it.

Then:
Only the approved test sink or allowlisted test recipient may receive it.

And:
Production users, production provider credentials, and production push
registrations remain inaccessible.
```

## 94. Template Version Test

```text
Given:
Template version 3 was used for a material communication.

And:
Template version 4 is later published.

When:
An operator investigates the earlier communication.

Then:
Habitacores can reproduce or retrieve the version-3 content and rendering context.

And:
The historical communication is not silently represented as version 4.
```

## 95. Provider Portability

Domain and application code must depend on Habitacores communication contracts rather than provider SDK types.

Directional interface:

```typescript
interface NotificationChannelAdapter {
  send(request: ChannelDeliveryRequest): Promise<ChannelDeliveryResult>;
}
```

Provider-specific details remain inside adapters.

## 96. Provider Selection

The initial transactional email and SMS providers remain deferred.

Provider evaluation should consider:

* Nigeria delivery coverage
* international delivery coverage
* domain authentication
* API quality
* idempotency
* delivery webhooks
* suppression management
* data residency
* privacy
* compliance
* cost
* support
* rate limits
* portability
* sandbox capability

## 97. Failure Isolation

Failure of one provider or channel must not necessarily stop other eligible channels.

Examples:

* Push outage should not stop in-app notifications.
* SMS outage should not prevent email where email is eligible.
* marketing-provider failure must not block security communication.

## 98. Provider Circuit Breakers

Adapters may use circuit breakers where appropriate.

A circuit breaker may:

* Pause calls to a failing provider
* reduce retry storms
* route eligible messages to a fallback provider
* preserve queue state
* trigger operational alerts

It must not mark undelivered communication as delivered.

## 99. Codex Boundaries

Every Codex notification work package must specify:

* Triggering event
* communication category
* recipients
* organization scope
* template and version
* eligible channels
* preference behavior
* consent behavior
* suppression behavior
* deduplication
* idempotency
* retry policy
* expiry
* provider callback handling
* audit requirements
* sensitive-data limits
* telemetry
* tests

Codex must not:

* Call providers inside authoritative transactions
* send directly from UI code
* embed provider credentials
* bypass the outbox
* use marketing content for required transactional messages
* ignore user consent or suppression
* use email addresses or phone numbers as metric labels
* expose protected content in push notifications
* use permanent private-document URLs
* treat provider acceptance as delivery
* treat delivery as human acknowledgement
* let AI autonomously send material communication
* use preview environments to contact production users
* place raw provider payloads into ordinary logs
* create free-form bulk messaging without authorization and review

## 100. Prohibited Patterns

The following are not approved:

* Direct provider SDK usage throughout domain code
* email or SMS calls inside database transactions
* sending without a durable notification intent
* unversioned material templates
* provider suppression as the only preference authority
* marketing without applicable consent
* full sensitive content in push or SMS
* private permanent URLs in messages
* provider acceptance represented as delivery
* email open represented as legal acknowledgement
* duplicate retries without idempotency
* production delivery from preview environments
* shared credentials across all environments
* organization users sending arbitrary mass messages
* AI issuing legal or financial communications autonomously
* push notification as the sole record of a critical event

## 101. Options Considered

### Option A — Direct Provider Calls From Features

Advantages:

* Fast initial implementation
* minimal abstraction

Disadvantages:

* Duplicate logic
* provider lock-in
* weak retry consistency
* no unified consent model
* difficult observability
* unsafe transaction coupling

Decision:

Rejected.

### Option B — Email-Only Communication

Advantages:

* Simpler architecture
* broad user familiarity

Disadvantages:

* Weak mobile immediacy
* unsuitable for all urgent events
* no unified in-app history
* ignores Android and iOS product direction

Decision:

Rejected.

### Option C — Provider-Managed Marketing Platform as Authority

Advantages:

* Campaign tooling
* provider-managed templates
* analytics and suppression

Disadvantages:

* Provider becomes business authority
* weak transactional integration
* tenant-isolation concerns
* difficult legal evidence
* portability limitations

Decision:

Rejected as the authoritative architecture.

### Option D — Habitacores Notification Orchestration With Provider Adapters

Advantages:

* Durable intent
* provider portability
* consistent preferences and consent
* cross-channel coordination
* centralized templates
* delivery evidence
* retries and deduplication
* strong audit and privacy controls
* web and mobile support

Disadvantages:

* Requires additional data model
* worker complexity
* provider webhook processing
* template governance
* preference and suppression maintenance

Decision:

Approved.

## 102. Positive Consequences

* Communication becomes durable and traceable.
* web and mobile use one communication policy.
* providers remain replaceable.
* duplicate delivery is controlled.
* consent and preferences are centralized.
* templates remain versioned and reproducible.
* push, email, SMS, and in-app delivery can be coordinated.
* legally important communication remains distinct from provider telemetry.
* Zoho corporate mail remains separate from product delivery.
* sensitive information is minimized.

## 103. Negative Consequences

* Notification orchestration adds implementation complexity.
* delivery states differ across providers.
* SMS and push require lifecycle management.
* template review requires governance.
* provider webhooks require secure ingestion.
* consent and suppression require ongoing maintenance.
* delivery does not guarantee reading or acknowledgement.
* multi-channel communication creates cost.

## 104. Risks

### 104.1 Duplicate Communication

Controls:

* Outbox idempotency
* notification deduplication keys
* provider idempotency
* attempt records
* crash-recovery tests

### 104.2 Sensitive Information Exposure

Controls:

* Minimal content
* authenticated detail retrieval
* template validation
* redaction
* restricted data model
* lock-screen-safe push

### 104.3 Consent Violation

Controls:

* Communication classification
* authoritative consent records
* preference checks
* suppression
* audit
* provider opt-out synchronization

### 104.4 Provider Outage

Controls:

* Durable queue
* bounded retries
* circuit breakers
* provider adapters
* fallback channels
* operational alerts

### 104.5 Cross-Tenant Delivery

Controls:

* Current recipient resolution
* organization membership validation
* permission checks
* RLS
* tenant-aware tests

### 104.6 AI-Generated Miscommunication

Controls:

* Registered use cases
* structured grounding
* approved templates
* human review
* no autonomous material delivery

## 105. Acceptance Criterion — Durable Dispatch

```text
Given:
An application submission commits successfully.

When:
The transaction completes.

Then:
The application state, required audit event, and notification outbox message
are committed consistently.

And:
External provider calls occur only after commit.
```

## 106. Acceptance Criterion — Provider Independence

```text
Given:
Habitacores changes its transactional email provider.

When:
The new adapter is enabled.

Then:
Domain events, notification intents, templates, preferences, consent,
suppression, and historical delivery records remain Habitacores-owned.

And:
Feature code does not require provider-specific rewriting.
```

## 107. Acceptance Criterion — Communication Precision

```text
Given:
An email provider accepts a receipt notification.

When:
Habitacores displays delivery status.

Then:
The system records Provider Accepted unless a later verified delivery
event is received.

And:
It does not claim that the recipient read or acknowledged the receipt.
```

## 108. Acceptance Criterion — Mobile and Web Consistency

```text
Given:
A resident receives a maintenance update.

When:
The update is viewed on web and mobile.

Then:
Both clients reference the same authoritative maintenance state and
notification intent.

And:
A push notification does not create a separate business outcome.
```

## 109. Implementation Sequence

Recommended implementation order:

```text
1. Define communication categories and event registry
2. define notification-intent and delivery-attempt schemas
3. establish preference, consent, and suppression schemas
4. establish the template registry and version model
5. create the provider-neutral channel contracts
6. integrate notification creation with ADR-010 outbox
7. implement the notification orchestration worker
8. implement in-app notifications
9. implement transactional email adapter
10. configure approved product sending domain
11. implement provider webhook ingestion and deduplication
12. implement mobile push adapter and device-token lifecycle
13. implement delivery dashboards and alerts
14. implement channel preferences and suppression
15. implement secure action links and deep links
16. implement SMS only for approved use cases
17. implement tenant-aware branding
18. implement legally significant communication evidence
19. implement non-production delivery isolation
20. add retry, duplicate, consent, privacy, and cross-tenant tests
```

## 110. Deferred Decisions

This ADR does not decide:

* Exact transactional email provider
* exact SMS provider
* exact WhatsApp provider
* exact email sending subdomain
* exact DMARC enforcement level
* exact SMS sender identity
* exact Nigeria SMS routing provider
* exact fallback-provider arrangement
* exact push-notification limits
* exact notification preference defaults
* exact retention periods
* exact legal-notice delivery requirements
* exact quiet-hour policy
* exact communication frequency caps
* exact inbound email-processing architecture
* exact bulk campaign tooling
* exact translation provider
* exact email component library
* exact provider cost budgets

These require bounded implementation decisions or later ADRs.

## 111. Supersession Conditions

This ADR should be reviewed when:

* A regulated legal-delivery provider becomes necessary.
* WhatsApp becomes a primary product channel.
* enterprise customers require customer-owned communication providers.
* regional data residency requires separate delivery infrastructure.
* communication volume requires dedicated streaming infrastructure.
* the transactional outbox no longer meets throughput requirements.
* mobile push moves from Expo to direct FCM and APNs operation.
* Habitacores introduces two-way automated messaging.
* communication regulations materially change in a target market.

## 112. Final Decision Statement

Habitacores will implement a centralized, provider-neutral notification and communication-delivery architecture supporting in-app notifications, transactional email, mobile push, SMS where justified, and approved future channels.

Zoho Mail will remain the corporate mailbox provider for `habitacores.com`. Corporate human-operated mailboxes and product-generated transactional communication will remain separate architectural concerns.

Material communication will originate from authoritative Habitacores workflows. Required business state, audit evidence, and transactional outbox messages will be committed consistently where applicable. External email, push, SMS, or messaging-provider calls will occur asynchronously after commit.

Habitacores will preserve durable notification intents, recipient resolution, communication category, template version, delivery attempts, provider references, retries, failures, suppressions, acknowledgements, and applicable audit records.

In-app notifications will be stored in Habitacores PostgreSQL and protected through authentication, authorization, tenant isolation, and RLS. They will reference authoritative business state rather than redefining it.

Email, push, SMS, and future providers will be accessed through Habitacores channel adapters. Provider-specific SDKs and data structures will not become domain or application contracts.

Habitacores will maintain its own authoritative notification preferences, consent records, and suppression records. Provider-managed suppression and opt-out systems will complement rather than replace these records.

Security, transactional, operational, legal, marketing, and internal communications will remain separately classified. Marketing will not be disguised as transactional communication. Required security and legally justified communication will follow explicit policy rather than ordinary marketing preferences.

Templates will be versioned, validated, localized where required, and rendered from bounded structured data. Material template versions will remain reproducible. Untrusted HTML, unrestricted payloads, secrets, and private storage URLs will not be inserted into messages.

Push and SMS content will be minimized. Protected financial, identity, document, application, agreement, legal, and support details will generally be retrieved through authenticated Habitacores experiences.

Provider acceptance, sending, delivery, opening, interaction, and acknowledgement will remain distinct states. Habitacores will not represent provider acceptance as delivery, delivery as reading, or reading as legal acknowledgement.

Retries will be bounded, idempotent, deduplicated, observable, and sensitive to current recipient eligibility, preference, consent, suppression, resource state, and message expiry.

Preview, test, staging, and production communication infrastructure will remain isolated. Non-production environments will not contact production users by default.

AI may support bounded drafting, simplification, summarization, or translation workflows. AI will not autonomously select recipients, issue legal notices, make payment demands, confirm financial decisions, override consent, or send materially consequential communication without the approved human and policy controls.

No browser, mobile client, provider, template, organization user, notification worker, push token, email address, phone number, deep link, feature flag, coding agent, or AI model may bypass Habitacores authentication, membership, tenant isolation, authorization, consent, suppression, privacy, audit, transaction, template, delivery, or Responsible AI controls.