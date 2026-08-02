# ADR-017 — Agreement Generation, Electronic Signature, Acceptance, and Evidentiary Record Strategy

## Decision Record

| Field | Value |
|---|---|
| Product | Habitacores |
| ADR | ADR-017 |
| Version | 0.1 |
| Status | Proposed |
| Decision Type | Foundational Legal-Document, Signature, and Evidence Architecture |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Market | Nigeria |
| Agreement Architecture | Provider-neutral orchestration |
| Agreement Authority | Habitacores agreement and execution records |
| Initial Signature Provider | Deferred pending due diligence |
| Document Model | Immutable, versioned agreement artifacts |
| Signing Model | Explicit signer roles, order, authority, consent, and evidence |
| Document Integrity | Cryptographic digest for every issued and executed version |
| Signature Completion | Verified provider or Habitacores execution evidence |
| Agreement Activation | Separate authoritative domain decision |
| Post-Signature Mutation | Prohibited |
| Corrections | New version, amendment, cancellation, or replacement |
| Mobile and Web Support | Required |
| Offline Binding Signature | Prohibited initially |
| AI Authority | Advisory and drafting assistance only |
| Legal Validation | Required before production template activation |
| Last Updated | 1 August 2026 |

## 1. Context

Habitacores will manage agreements involving:

- Residential tenancy
- lease or rental arrangements
- renewals
- guarantor commitments
- property-management arrangements
- maintenance or vendor services
- payment obligations
- deposits
- notices
- amendments
- termination or surrender
- organization-authorized property documents

These agreements may involve:

- Tenants
- residents
- landlords
- property owners
- agents
- property managers
- guarantors
- witnesses
- lawyers
- organization signatories
- platform personnel where specifically authorized

The platform must support electronic workflows without weakening:

- Signer identity
- authority
- informed consent
- document integrity
- template approval
- multi-party execution
- audit history
- tenant isolation
- evidentiary preservation
- agreement activation controls
- legal review

## 2. Decision Drivers

The decision is driven by the need to:

1. Support web, Android, and iOS agreement workflows.
2. preserve the exact content accepted by every party.
3. distinguish drafting from execution.
4. prove who acted, in what role, and under what authority.
5. support multiple signers and signing sequences.
6. prevent agreement substitution or silent editing.
7. preserve provider portability.
8. support authenticated electronic consent.
9. preserve evidence independently of a provider dashboard.
10. support witnesses and guarantors where required.
11. separate execution from business activation.
12. prevent unauthorized organizational signatures.
13. support corrections without rewriting history.
14. protect sensitive legal documents.
15. preserve accessibility and reasonable review time.
16. support low-bandwidth users.
17. support eventual legal disputes and investigations.
18. constrain AI involvement.
19. provide clear implementation and DevOps controls.
20. require legal validation before production activation.

## 3. Decision

Habitacores will implement a provider-neutral agreement-generation, signature, acceptance, and evidence architecture.

The approved high-level flow is:

```text
Approved agreement template
→ Structured agreement data
→ Draft generation
→ Internal review
→ Legal or authorized approval where required
→ Immutable agreement version
→ Agreement issuance
→ Signer and authority resolution
→ Signing invitations
→ Identity and authentication checks
→ Consent and document review
→ Signature or authenticated acceptance
→ Provider-event or internal-event verification
→ Multi-party completion
→ Evidence-package generation
→ Final executed document
→ Activation eligibility evaluation
→ Authorized agreement activation
````

Execution completion and agreement activation remain distinct.

## 4. Agreement Concepts

Habitacores will distinguish:

```text
AGREEMENT_TEMPLATE
AGREEMENT_TEMPLATE_VERSION
AGREEMENT
AGREEMENT_VERSION
AGREEMENT_PARTY
SIGNER_ASSIGNMENT
SIGNATURE_REQUEST
SIGNING_EVENT
SIGNATURE
ACCEPTANCE
WITNESS_ATTESTATION
EXECUTED_DOCUMENT
EVIDENCE_PACKAGE
AGREEMENT_ACTIVATION
AMENDMENT
TERMINATION
```

These concepts must not be collapsed into one `signed` boolean.

## 5. Agreement Template

A template represents approved reusable legal-document structure.

It should contain:

* Template ID
* agreement class
* jurisdiction
* owner
* legal-review status
* effective date
* retirement date
* required roles
* optional roles
* required clauses
* configurable clauses
* prohibited modifications
* signature requirements
* witness requirements
* activation requirements
* language
* version

## 6. Legal Approval

A template may not become production-active merely because it was technically implemented.

Production activation requires:

* Identified agreement class
* applicable jurisdiction
* legal-owner approval
* reviewed clauses
* required signatories
* witness or attestation requirements
* cancellation and amendment rules
* privacy review
* retention requirements
* approval record

## 7. Template Versioning

Published template versions are immutable.

Changing:

* wording;
* payment obligations;
* dates;
* responsibilities;
* notice requirements;
* renewal rules;
* termination rights;
* signature requirements;
* dispute provisions;

creates a new template version.

Historical agreements must continue referencing the version from which they were created.

## 8. Agreement Draft

An agreement draft may be regenerated while it remains in a draft state.

Drafts must be clearly marked:

```text
DRAFT — NOT EXECUTED
```

A draft is not a binding executed agreement and must not be represented as one.

## 9. Agreement Version

Before issuance, Habitacores creates an immutable agreement version containing:

* Agreement-version ID
* template-version ID
* structured input snapshot
* rendered-document reference
* document digest
* organization
* property
* unit
* applicable parties
* currency and financial references
* term
* issuance time
* issuing actor
* status

## 10. Document Digest

Every issued and executed agreement artifact must have a cryptographic digest.

The digest should be calculated from the exact stored document bytes.

It supports detection of:

* Replacement
* modification
* corruption
* accidental mismatch
* provider export mismatch

The digest is evidence of integrity, not proof of signer identity by itself.

## 11. Agreement States

Directional states:

```text
DRAFT
INTERNAL_REVIEW
LEGAL_REVIEW
APPROVED_FOR_ISSUANCE
ISSUED
SIGNING_IN_PROGRESS
PARTIALLY_SIGNED
FULLY_EXECUTED
ACTIVATION_PENDING
ACTIVE
REJECTED
DECLINED
EXPIRED
CANCELED
SUPERSEDED
TERMINATED
```

Transitions must be explicit and auditable.

## 12. Agreement Parties

Every party must be represented independently.

Potential roles include:

```text
TENANT
CO_TENANT
LANDLORD
PROPERTY_OWNER
AUTHORIZED_AGENT
PROPERTY_MANAGER
GUARANTOR
WITNESS
LEGAL_REVIEWER
ORGANIZATION_SIGNATORY
```

One user may hold more than one role only where policy explicitly permits it.

## 13. Signer Assignment

A signer assignment must identify:

* Agreement version
* party
* signer role
* internal user where available
* verified contact point
* signing order
* whether signature is mandatory
* authority basis
* authentication assurance
* signature method
* status
* expiry
* replacement history

## 14. Organizational Authority

A person signing for an organization must have current authority.

Habitacores must validate:

* Organization membership
* assigned role
* signing permission
* authority scope
* applicable property or agreement relationship
* separation-of-duty restrictions
* step-up authentication where required

A typed organization name or possession of an invitation link does not establish authority.

## 15. Individual Signer Identity

Identity evidence may include:

* Authenticated Habitacores account
* verified email address
* verified phone number
* MFA or recent authentication
* provider identity verification where approved
* government-identification review where legally justified
* role and relationship evidence

The required assurance level depends on agreement type and risk.

## 16. Signature Methods

Potential approved methods include:

```text
EXTERNAL_SIGNATURE_PROVIDER
AUTHENTICATED_CLICK_ACCEPTANCE
DRAWN_SIGNATURE
TYPED_SIGNATURE
UPLOADED_SIGNATURE_IMAGE
MANUAL_WET_SIGNATURE_IMPORT
```

Not every method is approved for every agreement class.

A method registry must define:

* Eligible agreement classes
* required authentication
* evidentiary strength
* limitations
* review requirements
* provider
* retention
* jurisdiction

## 17. Authenticated Acceptance

A controlled Habitacores acceptance workflow may be used only where approved for the agreement class.

It should require:

* Authenticated user
* current signer assignment
* exact agreement-version display
* opportunity to review and download
* explicit consent statement
* affirmative action
* timestamp
* network and device metadata within privacy limits
* agreement digest
* authentication assurance
* immutable event record

## 18. Consent Statement

The signer must affirmatively confirm that:

* They reviewed or had the opportunity to review the agreement.
* they intend to sign or accept it electronically.
* the electronic action is attributable to them.
* they understand the action has legal significance.
* the contact and identity information presented is theirs or authorized.

Preselected consent is prohibited.

## 19. Signature Provider

An external provider may support:

* Signature requests
* signer authentication
* signing order
* reminders
* signing ceremonies
* event webhooks
* executed-document generation
* evidence certificates
* document download
* cancellation
* expiry

The provider remains an execution service, not the sole Habitacores authority.

## 20. Provider-Neutral Boundary

Application and domain code should use Habitacores contracts.

Directional interface:

```typescript
interface AgreementSignatureProvider {
  createSignatureRequest(
    request: CreateSignatureRequest
  ): Promise<CreateSignatureResult>;

  cancelSignatureRequest(
    request: CancelSignatureRequest
  ): Promise<CancelSignatureResult>;

  retrieveExecutionEvidence(
    request: RetrieveExecutionEvidenceRequest
  ): Promise<ExecutionEvidenceResult>;
}
```

Provider SDK structures remain inside adapters.

## 21. Initial Provider Selection

The initial provider remains deferred.

Evaluation should include:

* Nigerian user availability
* web and mobile signing experience
* signer authentication
* multi-party signing
* signing order
* witness support
* API maturity
* webhook signatures
* event deduplication
* executed-document export
* evidence-certificate export
* data retention
* data residency
* privacy terms
* audit-log quality
* sandbox support
* provider continuity
* pricing
* branding
* accessibility
* low-bandwidth behavior
* provider exit capability

## 22. No Provider Dashboard Dependency

Habitacores must preserve its own:

* Agreement version
* party records
* signer assignments
* signature requests
* signing events
* provider references
* executed documents
* evidence packages
* status history
* audit events

The provider dashboard must not be the only record.

## 23. Signing Invitations

Invitations must be:

* Purpose bound
* signer bound
* agreement-version bound
* expiring
* revocable
* non-transferable where practical
* protected against enumeration
* auditable

An invitation does not itself grant document access beyond its approved purpose.

## 24. Invitation Links

Invitation links must not contain:

* Database credentials
* permanent document credentials
* provider secrets
* refresh tokens
* unrestricted bearer access

A link must resolve through controlled verification.

## 25. Multi-Party Signing

Habitacores must support:

* Parallel signing
* ordered signing
* optional signers
* mandatory signers
* replacement signers
* declined signing
* expired signing
* witness-after-signer sequences
* organization countersignature

The approved sequence depends on the agreement template.

## 26. Partial Signature

A partially signed agreement is not automatically fully executed.

The system must show:

* Required signers completed
* required signers outstanding
* optional signers
* declined signers
* expired assignments
* invalidated signatures

## 27. Signer Replacement

Replacing a signer requires:

* Authorization
* reason
* cancellation of the prior assignment
* current agreement-version validation
* new assignment
* audit
* review of already collected signatures

A signer must not be silently replaced.

## 28. Decline

A signer may decline where the workflow permits.

Decline should record:

* Signer assignment
* time
* reason where provided
* agreement version
* resulting agreement state
* notification
* audit event

## 29. Expiry

Signature requests may expire.

Expiry must not:

* Delete historical events
* remove the issued agreement version
* transform partial signatures into completed execution
* silently create a new agreement

Reissuance requires an explicit workflow.

## 30. Agreement Modification After Issuance

An issued agreement version must not be edited.

A required change creates:

* A new agreement version;
* a replacement agreement;
* or an amendment;

depending on legal and product rules.

Existing signatures may be invalidated where the signed content changes.

## 31. Executed Document

A fully executed document must preserve:

* Exact agreement content
* signer representations
* signature marks where applicable
* completion details
* provider certificate where applicable
* document digest
* execution time
* agreement-version ID
* relationship to evidence package

## 32. Evidence Package

The evidence package should contain, as applicable:

* Agreement ID and version
* template and version
* document digest
* executed-document digest
* signer assignments
* signer roles
* identity and authentication evidence
* consent statement version
* signature method
* signing-event timestamps
* provider event IDs
* provider request IDs
* network metadata within approved limits
* device and application context within approved limits
* invitation history
* cancellation or replacement history
* executed-document reference
* audit-event references

## 33. Time Evidence

The system should preserve:

* Server-received time
* transaction-commit time
* provider-reported time
* client-reported time where useful
* time-zone context for display

Authoritative workflow ordering should rely on trusted server and provider evidence, not the device clock alone.

## 34. Provider Webhooks

Signature-provider webhooks must be:

* Signature verified
* environment verified
* schema validated
* deduplicated
* durably recorded
* processed idempotently
* correlated with an existing signature request
* reconciled against provider APIs where necessary

## 35. Provider Callback Is Evidence, Not Automatic Activation

A provider event such as `completed` may support execution completion.

It must not directly:

* Activate occupancy
* start tenancy
* satisfy payment
* hand over possession
* approve an application
* create organization membership
* change permissions

Those outcomes require separate domain evaluation.

## 36. Agreement Completion

An agreement may become fully executed only when:

* Correct agreement version is confirmed.
* all mandatory parties are satisfied.
* signatures remain valid.
* assignments were not canceled.
* provider or internal evidence is verified.
* the executed document matches the issued content.
* no unresolved integrity discrepancy exists.

## 37. Agreement Activation

Activation is a separate authoritative command.

Activation may require:

* Fully executed agreement
* verified required payment
* approved application
* required identity checks
* unit availability
* approved commencement date
* no conflicting active occupancy
* authorized activator
* required audit event

## 38. Payment Separation

Signing an agreement does not prove payment.

Payment verification remains governed by ADR-016.

Payment-provider success does not prove agreement execution.

## 39. Occupancy Separation

Agreement execution does not automatically activate occupancy.

Occupancy requires its own eligibility and authorization controls.

## 40. Application Separation

Signing does not retroactively approve an application.

Application decisions remain separately governed.

## 41. Guarantors

A guarantor workflow must preserve:

* Guarantor identity
* exact obligation accepted
* agreement or guarantee version
* relationship to tenant
* effective period
* applicable limits
* authenticated consent
* signature evidence

Habitacores must not assume every tenancy requires a guarantor.

## 42. Witnesses

Where a witness is required, the system must represent:

* Witness identity
* witnessed party
* sequence
* attestation statement
* method
* time
* applicable agreement version
* authentication evidence

A witness role must not be simulated by a system account.

## 43. Wet-Signed Agreements

Habitacores may support importing a wet-signed agreement.

The import must preserve:

* Scanned document
* upload actor
* source
* signing parties
* declared signing date
* verification status
* reviewer
* document digest
* legal review where required
* relationship to the agreement record

Uploading a scanned signature does not automatically prove authenticity.

## 44. Offline Signing

Authoritative offline electronic execution is not approved initially.

Users may download or review agreements offline, but final electronic signature or acceptance requires server connectivity and current-state validation.

## 45. Mobile Signing

Mobile signing must use:

```text
Mobile application
→ Habitacores API
→ Agreement service
→ Signature provider or controlled acceptance workflow
→ Verified event processing
→ Evidence-package generation
```

Mobile local state cannot mark an agreement executed.

## 46. Document Access

Access to draft, issued, and executed agreements must depend on:

* Authentication
* organization context
* party relationship
* permission
* agreement state
* support context where applicable
* legal or operational role

Possession of a document ID does not grant access.

## 47. Download

Sensitive agreement downloads should use:

* Authenticated streaming; or
* short-lived authorized links;

according to ADR-011.

Permanent private-object URLs are prohibited.

## 48. Data Minimization

Signature providers should receive only data required for execution.

Habitacores should not send unrelated:

* Application notes
* payment evidence
* support notes
* identity documents
* property records
* organization records

## 49. Provider Credentials

Provider credentials must be:

* Server or worker only
* environment specific
* excluded from Git
* excluded from clients
* excluded from logs
* least privilege
* rotated
* inaccessible to AI

## 50. Environment Isolation

Development, preview, staging, and production signature environments must be isolated.

Non-production must use:

* Test accounts
* synthetic agreements
* test signers
* test provider credentials
* non-production callbacks
* obvious test markings

A preview deployment must not issue a production tenancy agreement.

## 51. Audit Events

Material audit events may include:

```text
AGREEMENT_DRAFT_CREATED
AGREEMENT_VERSION_CREATED
AGREEMENT_APPROVED_FOR_ISSUANCE
AGREEMENT_ISSUED
SIGNER_ASSIGNED
SIGNER_REPLACED
SIGNATURE_REQUEST_CREATED
SIGNING_INVITATION_SENT
AGREEMENT_VIEWED
SIGNATURE_COMPLETED
SIGNATURE_DECLINED
SIGNATURE_REQUEST_EXPIRED
AGREEMENT_FULLY_EXECUTED
EVIDENCE_PACKAGE_CREATED
AGREEMENT_ACTIVATION_REQUESTED
AGREEMENT_ACTIVATED
AGREEMENT_CANCELED
AGREEMENT_SUPERSEDED
AGREEMENT_AMENDED
AGREEMENT_TERMINATED
```

## 52. Observability

Metrics may include:

```text
agreements_drafted
agreements_issued
signature_requests_created
signature_completion_duration
signature_declines
signature_expiries
provider_webhook_failures
provider_event_duplicates
document_digest_mismatches
execution_evidence_failures
activation_failures
```

Signer identities and agreement IDs must not become ordinary metric labels.

## 53. Alerts

Priority alerts may include:

* Signature-provider authentication failure
* webhook verification failure spike
* executed-document digest mismatch
* provider-completed agreement missing evidence package
* agreement activated without full execution
* signing request tied to wrong environment
* signer authority failure
* production agreement issued from preview
* executed document unavailable
* unauthorized post-issuance mutation attempt

## 54. Agreement Corrections

Executed agreements must not be edited in place.

Corrections require one of:

```text
AMENDMENT
REPLACEMENT
CANCELLATION
TERMINATION
RECTIFICATION
```

The applicable method requires legal and business rules.

## 55. Amendment

An amendment must identify:

* Original agreement
* affected clauses
* reason
* effective date
* parties required to accept
* amendment document version
* signatures
* evidence package
* activation conditions

## 56. Termination

Termination must preserve:

* Authority
* legal or contractual basis
* effective date
* notice requirements
* associated document
* parties
* audit
* impact on occupancy and obligations

Termination must not delete the agreement.

## 57. Retention

Agreement and execution evidence retention must consider:

* Contract duration
* tenancy duration
* dispute period
* statutory requirements
* legal holds
* privacy
* litigation
* provider retention limits

Exact periods require legal approval.

## 58. Legal Hold

An agreement, executed document, evidence package, or signing history under legal hold must not be deleted through ordinary retention processing.

## 59. Responsible AI

AI may assist with:

* Drafting non-binding template suggestions
* comparing template versions
* summarizing clauses
* explaining approved terms
* detecting missing structured fields
* identifying inconsistent dates for review

AI may not:

* Approve a legal template
* invent binding clauses
* determine signer authority
* sign for a party
* generate a signature
* record consent
* mark an agreement executed
* activate an agreement
* terminate an agreement
* provide final legal advice
* modify an executed document

## 60. AI Clause Explanations

AI-generated explanations must:

* Be grounded in the exact agreement version
* distinguish explanation from legal advice
* preserve access controls
* avoid changing legal meaning
* remain non-authoritative
* reference the relevant clause
* be reviewable where materially relied upon

## 61. Testing

The architecture requires:

* Agreement state-transition tests
* template-version tests
* document-digest tests
* signer-authority tests
* multi-party completion tests
* signing-order tests
* provider-webhook signature tests
* provider-event deduplication tests
* executed-document reconciliation tests
* activation-gate tests
* cross-tenant access tests
* invitation-expiry tests
* signer-replacement tests
* environment-isolation tests
* AI-boundary tests

## 62. Document Integrity Test

```text
Given:
An agreement version has been issued.

When:
The stored document bytes are changed.

Then:
Digest validation fails.

And:
The agreement cannot be treated as the originally issued version.
```

## 63. Partial-Signature Test

```text
Given:
A tenancy agreement requires tenant and landlord signatures.

And:
Only the tenant has signed.

When:
Execution status is evaluated.

Then:
The agreement remains partially signed.

And:
It cannot become fully executed or active.
```

## 64. Wrong-Version Test

```text
Given:
Agreement version 2 was issued for signature.

When:
A provider callback references an earlier version.

Then:
Habitacores rejects or quarantines the completion event.

And:
Version 2 remains unsigned until valid evidence is received.
```

## 65. Signer-Authority Test

```text
Given:
A user previously had organization signing authority.

And:
That authority was revoked before signature.

When:
The signing operation is attempted.

Then:
Current authority validation fails.

And:
The signature is not accepted as an authorized organizational signature.
```

## 66. Duplicate-Provider-Event Test

```text
Given:
A signature provider sends the same completed event repeatedly.

When:
Habitacores processes the events.

Then:
One completion transition is applied.

And:
One executed-document and evidence-package workflow is created.
```

## 67. Activation-Gate Test

```text
Given:
An agreement is fully executed.

And:
The required payment remains unverified.

When:
Agreement activation is requested.

Then:
Activation is denied or remains pending.

And:
Execution status is preserved without falsely activating occupancy.
```

## 68. Cross-Tenant Agreement Test

```text
Given:
A user belongs to Organization A.

And:
An agreement belongs to Organization B.

When:
The user requests the agreement by ID.

Then:
Application authorization denies access.

And:
PostgreSQL RLS prevents retrieval.
```

## 69. Preview-Isolation Test

```text
Given:
A pull-request preview generates an agreement.

When:
The signing workflow begins.

Then:
Only test signers and non-production provider credentials may be used.

And:
The document is visibly marked non-production.
```

## 70. Codex Boundaries

Every Codex agreement work package must specify:

* Agreement class
* jurisdiction
* template and version
* parties and signer roles
* authority requirements
* signing order
* identity assurance
* signature method
* consent statement
* document digest
* provider events
* completion conditions
* activation conditions
* audit events
* authorization
* RLS
* retention
* tests

Codex must not:

* Use a single `signed` boolean as the agreement model
* edit issued or executed documents
* accept a provider callback without validation
* equate provider completion with agreement activation
* allow mobile or browser state to become signature authority
* expose provider credentials
* embed permanent private-document URLs
* bypass signer-authority checks
* fabricate timestamps
* generate signatures
* permit preview environments to issue production agreements
* let AI approve, sign, activate, amend, or terminate agreements
* delete execution evidence during ordinary corrections

## 71. Prohibited Patterns

The following are not approved:

* Editable executed agreements
* unversioned legal templates
* signatures detached from an agreement version
* provider dashboard as the only execution record
* one-party signature represented as full execution where more are required
* email possession treated as organizational authority
* agreement activation from a webhook alone
* payment success treated as signature
* signature treated as payment
* uploaded signature image treated as sufficient proof without policy
* permanent signing links
* unsigned webhook processing
* no document-integrity digest
* AI-generated binding clauses used without review
* deleting superseded agreements or evidence packages

## 72. Options Considered

### Option A — Upload Scanned Agreements Only

Advantages:

* Simple implementation
* compatible with wet signatures

Disadvantages:

* Weak workflow visibility
* manual identity verification
* limited evidence structure
* difficult multi-party coordination
* poor scalability

Decision:

Supported as an exception workflow, not the primary architecture.

### Option B — Signature Provider as Complete Agreement Authority

Advantages:

* Faster integration
* provider-managed signing ceremony
* provider evidence package

Disadvantages:

* Provider lock-in
* weak integration with Habitacores authority and activation
* provider retention dependency
* limited portability
* insufficient internal tenant and workflow controls

Decision:

Rejected.

### Option C — Simple Checkbox Acceptance for Every Agreement

Advantages:

* Low implementation cost
* simple mobile experience

Disadvantages:

* Insufficient for every agreement class
* weak identity and authority handling
* no witness support
* weak evidentiary differentiation

Decision:

Rejected as a universal method.

### Option D — Habitacores Agreement Authority With Provider-Neutral Execution

Advantages:

* Immutable agreement versions
* provider portability
* explicit signer authority
* multi-party workflows
* strong evidence preservation
* web and mobile support
* separate activation controls
* support for electronic and wet-signature exceptions

Disadvantages:

* More complex state machines
* provider integration and reconciliation required
* legal-template governance required
* evidence retention adds storage and operational duties

Decision:

Approved.

## 73. Positive Consequences

* Exact signed content remains provable.
* signer identity and authority are explicit.
* provider changes do not destroy agreement history.
* multi-party signing is supported.
* agreement execution remains distinct from activation.
* mobile and web share one authoritative workflow.
* legal templates remain versioned.
* corrections preserve history.
* evidence packages support investigation and dispute handling.
* AI remains non-authoritative.

## 74. Negative Consequences

* Agreement workflows require substantial implementation.
* legal review is required.
* provider integration requires ongoing maintenance.
* evidence packages increase storage.
* older mobile versions must support active signing flows.
* witnesses and guarantors add workflow complexity.
* electronic-signature suitability may vary by document class.

## 75. Risks

### 75.1 Invalid or Disputed Signature

Controls:

* Authentication assurance
* signer authority
* explicit consent
* immutable document version
* digest
* evidence package
* timestamps
* audit

### 75.2 Document Substitution

Controls:

* Immutable object
* document digest
* version binding
* provider reconciliation
* integrity tests

### 75.3 Unauthorized Organizational Signature

Controls:

* Current membership validation
* signing permission
* authority scope
* step-up authentication
* audit

### 75.4 Provider Lock-In

Controls:

* Adapter boundary
* Habitacores-owned records
* executed-document export
* evidence-package export
* provider references stored separately

### 75.5 Premature Agreement Activation

Controls:

* Separate activation command
* eligibility checks
* payment verification
* occupancy checks
* authorization
* audit

### 75.6 AI Legal Error

Controls:

* AI advisory only
* approved templates
* legal review
* version grounding
* human authority
* no autonomous execution

## 76. Implementation Sequence

```text
1. Define agreement classes and legal-review registry
2. define template and template-version models
3. define agreement and immutable version models
4. define party and signer-assignment models
5. define agreement state machines
6. define signature-method registry
7. implement document generation and digesting
8. implement agreement issuance
9. implement signer authority and authentication checks
10. define provider-neutral signature contracts
11. implement controlled authenticated acceptance where approved
12. implement external-provider sandbox adapter
13. implement webhook verification and deduplication
14. implement multi-party completion
15. generate executed documents and evidence packages
16. implement activation eligibility and command
17. implement amendments, cancellation, and termination
18. implement web and mobile signing experiences
19. implement audit, dashboards, alerts, and reconciliation
20. conduct Nigerian legal and provider due diligence
21. approve production templates and signature methods
22. select the initial production provider
```

## 77. Deferred Decisions

This ADR does not decide:

* Initial production signature provider
* agreement classes eligible for click acceptance
* exact identity-verification provider
* exact witness requirements
* exact guarantor requirements
* exact tenancy-template clauses
* exact state-specific tenancy rules
* exact stamping or registration requirements
* exact provider data-residency region
* exact signature assurance levels
* exact retention periods
* exact legal-hold procedures
* exact wet-signature verification workflow
* exact notary or commissioner-for-oaths integration
* exact biometric signature policy
* exact signature-image policy
* exact legal-advice delivery process
* exact dispute-resolution clauses

These require legal, operational, business, and implementation decisions.

## 78. Supersession Conditions

This ADR should be reviewed when:

* Habitacores enters another jurisdiction.
* Nigerian electronic-signature requirements materially change.
* a regulated trust or certificate service becomes necessary.
* notarization becomes a core requirement.
* enterprise customers require their own signature providers.
* offline authoritative execution becomes necessary.
* blockchain-based document evidence is proposed.
* the selected provider cannot export complete execution evidence.
* tenancy registration or stamping becomes fully integrated.

## 79. Final Decision Statement

Habitacores will maintain authoritative, immutable, versioned agreement records and will orchestrate electronic signature, authenticated acceptance, wet-signature import, and execution evidence through provider-neutral application services.

Every issued agreement will be bound to an immutable agreement version, approved template version, structured-data snapshot, stored document artifact, and cryptographic document digest.

Agreement drafting, legal approval, issuance, signing, full execution, activation, amendment, cancellation, termination, payment, and occupancy will remain distinct states and operations.

Every signer will be represented through an explicit signer assignment containing the applicable agreement version, party role, signing order, authority basis, authentication assurance, signature method, expiry, and status.

Possession of an invitation link, verified email, typed name, uploaded signature image, provider redirect, browser state, or mobile state will not independently establish signer authority or agreement completion.

External signature providers may facilitate signing ceremonies, authentication, reminders, document execution, callbacks, and evidence certificates. They will not become the sole authority for Habitacores agreement state, party authority, activation, payment, or occupancy.

Habitacores will preserve executed documents, agreement and template versions, cryptographic digests, signer assignments, authentication and consent evidence, provider events, signing timestamps, completion history, evidence packages, and audit-event references.

Provider webhooks will be signature verified, environment checked, schema validated, deduplicated, persisted, reconciled, and processed idempotently.

A fully executed agreement will not automatically become active. Activation requires a separate authorized command and validation of all applicable payment, application, unit, commencement, identity, agreement, and occupancy conditions.

Issued and executed agreement documents will not be edited in place. Corrections will use an amendment, replacement, cancellation, rectification, or termination workflow that preserves the original document and evidence.

Web and mobile clients will access agreement workflows through authenticated Habitacores APIs. They will not hold provider secrets, create authoritative local signatures, modify agreement versions, or activate agreements independently.

AI may support bounded drafting, comparison, explanation, and consistency review. AI will not approve legal templates, invent binding terms without review, determine signer authority, generate a signature, record consent, execute, activate, amend, terminate, or provide authoritative legal advice.

Production agreement templates, signing methods, identity-assurance levels, witness requirements, retention periods, and jurisdiction-specific rules require legal approval before activation.

No provider, webhook, mobile application, browser, uploaded signature image, organization user, feature flag, coding agent, or AI model may bypass Habitacores authentication, organizational authority, signer assignment, tenant isolation, document integrity, agreement versioning, consent, execution evidence, legal review, payment, activation, audit, retention, or Responsible AI controls.