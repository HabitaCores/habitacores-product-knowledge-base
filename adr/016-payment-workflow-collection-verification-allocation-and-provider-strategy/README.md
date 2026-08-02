# ADR-016 — Payment Workflow, Collection, Verification, Allocation, and Provider Strategy

## Decision Record

| Field | Value |
|---|---|
| Product | Habitacores |
| ADR | ADR-016 |
| Version | 0.1 |
| Status | Proposed |
| Decision Type | Foundational Financial, Integration, and Control Architecture |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Market | Nigeria |
| Primary Currency Direction | NGN, with explicit ISO 4217 currency on every monetary record |
| Money Representation | Integer minor units under ADR-006 |
| Payment Architecture | Provider-neutral orchestration |
| Initial Collection Experience | Provider-hosted checkout and approved bank-transfer workflows |
| Initial Provider | Deferred pending due diligence |
| Provider Authority | External processing evidence only |
| Habitacores Authority | Internal payment, allocation, reconciliation, and business-state records |
| Provider Confirmation | Server-side verification required |
| Webhook Processing | Signature verified, deduplicated, idempotent, and asynchronous |
| Manual Evidence | Evidence only; not automatic verification |
| Financial History | Append-only corrections and reversals |
| Internal Wallet or Stored Value | Prohibited without a separate approved decision |
| Escrow Representation | Prohibited unless legally and operationally established |
| Card Data Storage | Prohibited |
| Financial AI Authority | Prohibited |
| Last Updated | 1 August 2026 |

## 1. Context

Habitacores will support financial workflows involving:

- Annual rent
- security deposits where applicable
- service charges
- maintenance-related charges
- application or administrative charges where approved
- agreement-related charges
- renewal payments
- partial payments where contractually permitted
- overpayments
- refunds
- reversals
- payment evidence
- receipts
- provider fees
- settlement and reconciliation

The product will serve:

- Tenants and residents
- applicants
- landlords and property owners
- agents and estate managers
- organization finance reviewers
- platform operations
- legal and support personnel where authorized

Financial workflows create substantial integrity risk.

The architecture must prevent:

- Duplicate payment recognition
- false verification
- incorrect currency or amount
- cross-tenant allocation
- provider callback spoofing
- unreviewed manual confirmation
- mutable financial history
- receipt issuance before verification
- provider outages corrupting business state
- payment evidence being treated as proof of cleared funds
- refunds exceeding the verified and available amount
- mobile or browser state becoming financial authority
- AI making authoritative financial decisions

## 2. Decision Drivers

The decision is driven by the need to:

1. Support Nigerian payment methods.
2. support annual rent as a core product workflow.
3. preserve provider portability.
4. verify provider payments server side.
5. process asynchronous payment events safely.
6. prevent duplicate recognition.
7. separate collection from allocation.
8. preserve immutable financial history.
9. support manual bank-transfer evidence safely.
10. reconcile internal records with providers and bank settlements.
11. issue accurate receipts.
12. protect tenant isolation.
13. support refunds and reversals.
14. support web and mobile clients.
15. minimize payment-card exposure.
16. support provider outages and delayed callbacks.
17. preserve auditability.
18. prohibit unauthorized wallet or escrow behavior.
19. constrain Responsible AI.
20. provide implementation-ready DevOps and testing requirements.

## 3. Decision

Habitacores will implement a provider-neutral payment orchestration architecture.

The approved high-level flow is:

```text
Chargeable obligation
→ Payment intent
→ Collection method selected
→ Provider attempt or manual-transfer workflow
→ External payment activity
→ Provider webhook or verification request
→ Signature and schema validation
→ Provider-event deduplication
→ Server-side transaction verification
→ Amount, currency, reference, recipient, and status validation
→ Habitacores payment verified
→ Payment allocation
→ Receipt issuance
→ Settlement and reconciliation
→ Correction, reversal, or refund where applicable
````

No external provider will independently define the Habitacores contractual, tenancy, agreement, occupancy, receipt, or allocation state.

## 4. Financial Concepts

Habitacores will distinguish:

```text
OBLIGATION
PAYMENT_INTENT
PAYMENT_ATTEMPT
PAYMENT
PAYMENT_EVIDENCE
PAYMENT_ALLOCATION
RECEIPT
PROVIDER_EVENT
SETTLEMENT
RECONCILIATION
REFUND
REVERSAL
```

These concepts must not be collapsed into a single `payments` status field.

## 5. Obligation

An obligation represents an amount expected under an approved business basis.

Examples:

* Annual rent
* security deposit
* service charge
* approved maintenance charge
* renewal charge

An obligation should identify:

* Organization
* payer or responsible party
* property, unit, agreement, application, or occupancy context
* charge type
* original amount
* currency
* due date
* status
* source rule or authorized creator
* adjustments
* amount allocated
* amount outstanding

## 6. Payment Intent

A payment intent represents an approved request or opportunity to collect money.

It should identify:

* Internal payment-intent ID
* organization
* payer
* currency
* requested amount
* eligible obligations
* collection method
* expiry
* idempotency key
* provider configuration
* status
* creation actor
* source workflow

A payment intent is not proof of payment.

## 7. Payment Intent States

Directional states:

```text
DRAFT
READY
PROCESSING
REQUIRES_ACTION
PENDING_EXTERNAL_CONFIRMATION
SUCCEEDED
FAILED
CANCELED
EXPIRED
```

A successful provider attempt does not by itself allocate the payment to an obligation.

## 8. Payment Attempt

Every provider or collection attempt will be represented separately.

Potential fields include:

* Payment-attempt ID
* payment-intent ID
* provider
* provider account
* provider reference
* Habitacores reference
* method
* requested amount
* currency
* attempt number
* provider status
* normalized status
* initiated time
* completed time
* failure class
* idempotency key

Repeated attempts must not overwrite one another.

## 9. Payment

A Habitacores payment represents verified value received or otherwise confirmed under an approved process.

A payment must record:

* Payment ID
* organization
* payer
* amount
* currency
* collection method
* verification method
* verified time
* verifying actor or system
* provider or bank reference
* source attempt
* status
* unallocated amount
* reversed amount
* refunded amount

## 10. Payment States

Directional payment states:

```text
PENDING
UNDER_REVIEW
VERIFIED
PARTIALLY_ALLOCATED
FULLY_ALLOCATED
PARTIALLY_REFUNDED
REFUNDED
REVERSED
DISPUTED
REJECTED
```

Transitions require explicit domain rules.

## 11. Provider-Neutral Boundary

Domain and application code will depend on Habitacores payment contracts.

Directional interface:

```typescript
interface PaymentProviderAdapter {
  createCollection(
    request: CreateCollectionRequest
  ): Promise<CreateCollectionResult>;

  verifyTransaction(
    request: VerifyTransactionRequest
  ): Promise<VerifyTransactionResult>;

  initiateRefund?(
    request: InitiateRefundRequest
  ): Promise<InitiateRefundResult>;
}
```

Provider SDK types must remain inside provider adapters.

## 12. Initial Provider Selection

The initial provider remains deferred.

Candidates may include providers capable of supporting the Nigerian market and the approved collection methods.

Selection will require evaluation of:

* Nigerian bank-transfer coverage
* card support
* USSD or other locally relevant methods
* hosted checkout
* virtual or reserved accounts
* mobile compatibility
* webhook security
* transaction verification
* idempotency
* refunds
* settlement reporting
* reconciliation exports or APIs
* sandbox quality
* API stability
* provider support
* data handling
* pricing
* merchant onboarding
* operational reliability
* contractual terms

A feature list alone is insufficient for selection.

## 13. Hosted Checkout

Habitacores should initially prefer provider-hosted checkout or another provider-controlled payment-entry interface.

This reduces direct handling of payment-card information.

Habitacores must not:

* Store complete card numbers
* store CVV values
* log card details
* send card information through Habitacores APIs
* send card information to AI providers
* include card information in audit metadata

## 14. Bank Transfer

Bank transfer is an important initial payment method.

Potential implementations include:

* Provider-generated temporary account
* provider-issued reserved or virtual account
* transfer to an approved organization account
* manual bank-transfer evidence workflow

Each implementation requires distinct verification and reconciliation rules.

## 15. Manual Bank-Transfer Evidence

A user may upload:

* Transfer receipt
* bank-generated confirmation
* transaction reference
* payment date
* payer details
* amount
* bank information

The upload creates payment evidence only.

It must not automatically:

* Verify payment
* allocate funds
* issue a final receipt
* activate an agreement
* activate occupancy
* satisfy rent
* close an obligation

## 16. Manual Verification

Manual verification may be performed only by an authorized finance reviewer.

The reviewer must compare available evidence against:

* Expected obligation
* account activity or provider evidence
* amount
* currency
* payer or transfer reference
* date
* duplicate records
* prior allocations
* unresolved discrepancies

Manual verification requires an audit event.

## 17. Separation of Duties

Where team size permits, the architecture should separate:

* Payment submission
* verification
* allocation adjustment
* refund approval
* reconciliation
* privileged correction

At the founding stage, any staffing exception must be explicit and audited.

## 18. Provider Webhooks

Payment-provider webhooks are asynchronous evidence.

Webhook processing must include:

```text
Receive
→ Preserve bounded raw evidence where approved
→ Verify provider and environment
→ Verify signature
→ Validate schema
→ Deduplicate event
→ Persist provider event
→ Acknowledge safely
→ Process asynchronously
→ Verify transaction through provider where required
→ Apply idempotent domain transition
```

## 19. Signature Verification

Unsigned, invalidly signed, malformed, replayed, or wrong-environment webhook requests must not update financial state.

Signature validation must use the provider’s approved current mechanism.

Webhook secrets must remain environment-scoped server secrets.

## 20. Webhook Deduplication

Provider events must be deduplicated using the strongest available combination of:

* Provider
* provider account
* provider event ID
* event type
* transaction reference
* payload fingerprint where necessary

Receiving the same event twice must not recognize payment twice.

## 21. Webhook Acknowledgement

Webhook endpoints should complete minimal validation and durable ingestion promptly.

Complex financial processing should occur asynchronously after durable receipt where supported by the provider’s retry behavior.

## 22. Redirect and Client Callback

A browser or mobile redirect is a user-experience signal.

It is not authoritative proof that payment succeeded.

The client may display:

```text
Confirming payment
Payment pending
Payment unsuccessful
```

It must not display an authoritative `Verified` state until the server commits verification.

## 23. Server-Side Verification

Before recognizing provider payment, Habitacores must validate:

* Provider transaction exists
* expected reference
* final acceptable status
* expected currency
* expected amount or approved variance rule
* expected merchant or provider account
* payment method where relevant
* transaction has not already been recognized
* transaction belongs to the correct Habitacores environment
* transaction is associated with the correct payment intent

## 24. Amount Validation

Habitacores must not accept provider success without validating the amount.

Possible outcomes include:

```text
EXACT_MATCH
UNDERPAYMENT
OVERPAYMENT
CURRENCY_MISMATCH
UNEXPECTED_PAYMENT
DUPLICATE_PAYMENT
```

Each outcome requires an explicit workflow.

## 25. Currency Validation

Currency must be explicit on:

* Obligations
* payment intents
* attempts
* payments
* allocations
* refunds
* settlements
* reconciliation records

A payment in one currency must not be silently allocated to an obligation in another currency.

## 26. Provider References

Habitacores will preserve:

* Internal immutable ID
* human-facing reference
* provider transaction ID
* provider reference
* bank reference where available
* settlement reference
* refund reference

Provider references do not replace Habitacores identifiers.

## 27. Idempotency

Material payment commands require idempotency.

Examples:

* Create payment intent
* initialize provider collection
* verify provider transaction
* allocate payment
* issue receipt
* initiate refund
* process webhook
* import settlement
* execute reconciliation correction

Idempotency scope and retention must be defined per operation.

## 28. Payment Allocation

Verification and allocation are separate.

A verified payment may be allocated to:

* One obligation
* several obligations
* part of an obligation
* an approved unapplied balance awaiting review

Allocation must preserve:

* Payment
* obligation
* allocated amount
* currency
* allocation sequence
* effective time
* allocating actor
* reason
* reversal relationship

## 29. Allocation Rules

Allocation must not exceed:

* Verified payment amount available for allocation
* outstanding eligible obligation amount, unless explicit overpayment handling applies

Cross-organization allocation is prohibited.

## 30. Partial Payments

Partial payment support must be explicit at the obligation or agreement level.

The platform must not assume that every annual-rent obligation permits installments.

Where partial payment is allowed, the system should distinguish:

* Amount due
* amount received
* amount allocated
* amount outstanding
* contractual completion status

## 31. Overpayments

An overpayment must not be silently discarded or automatically treated as revenue.

Potential controlled outcomes include:

* Allocate to another eligible obligation with authorization
* retain as unapplied payment pending review
* refund
* record another approved treatment

This ADR does not authorize a general-purpose stored-value wallet.

## 32. Unapplied Payments

An unapplied payment is verified money not yet fully allocated.

It requires:

* Restricted finance visibility
* reconciliation
* aging monitoring
* resolution workflow
* audit
* no misleading representation as an unrestricted user balance

## 33. Receipts

A final payment receipt may be issued only after:

* Payment verification
* applicable allocation or approved unapplied classification
* required identifiers
* immutable amount and currency
* payer and recipient resolution
* receipt number assignment

A provider checkout screen is not the Habitacores receipt.

## 34. Receipt Immutability

Issued receipts must not be silently edited.

Corrections require:

* Void or correction relationship
* replacement receipt where applicable
* reason
* authorized actor
* audit event
* preserved historical version

## 35. Refunds

Refunds require:

* Verified original payment
* refundable amount calculation
* current refund availability
* authorization
* reason
* idempotency
* provider or manual method
* status tracking
* audit
* reconciliation

## 36. Refund Limits

Total successful and pending refunds must not exceed the approved refundable amount.

Concurrent refund requests must be protected through transactional locking or equivalent consistency controls.

## 37. Refund States

Directional states:

```text
REQUESTED
UNDER_REVIEW
APPROVED
SUBMITTED
PENDING_PROVIDER
SUCCEEDED
FAILED
REJECTED
CANCELED
```

Provider acceptance is not final refund completion.

## 38. Reversals and Chargebacks

A provider reversal, bank reversal, dispute, or chargeback must not erase the original payment.

Habitacores should append a linked financial event and recalculate affected balances and allocations through controlled procedures.

## 39. Settlement

Provider settlement is distinct from customer payment verification.

A payment may be verified before the provider settles net funds to the merchant account.

Settlement records may include:

* Provider
* settlement ID
* settlement date
* gross amount
* fees
* taxes where provided
* net amount
* currency
* included transactions
* settlement status

## 40. Provider Fees

Provider fees must not be inferred from the difference between expected rent and received settlement without supporting evidence.

Fees require explicit records and classification.

The payer’s contractual obligation and the organization’s provider expense are different accounting concepts.

## 41. Reconciliation

Habitacores will reconcile:

```text
Internal payment records
↔ Provider transaction records
↔ Provider settlement records
↔ Approved bank records where available
↔ Allocations
↔ Refunds and reversals
```

Reconciliation is not optional operational cleanup. It is a required financial control.

## 42. Reconciliation Outcomes

Directional outcomes:

```text
MATCHED
MISSING_INTERNAL
MISSING_PROVIDER
AMOUNT_MISMATCH
CURRENCY_MISMATCH
STATUS_MISMATCH
DUPLICATE
UNALLOCATED
UNSETTLED
REFUND_MISMATCH
REQUIRES_REVIEW
```

## 43. Reconciliation Corrections

Reconciliation must not mutate financial history invisibly.

Corrections require:

* New corrective record
* reason
* source evidence
* authorized actor
* audit event
* relationship to original records

## 44. Annual Rent

Annual rent will be modeled as an obligation tied to the applicable:

* Organization
* property
* unit
* agreement or approved pre-agreement workflow
* responsible payer
* tenancy period
* due date
* currency
* approved amount

Payment alone must not activate occupancy unless all applicable agreement and onboarding requirements are satisfied.

## 45. Payment and Agreement Separation

Payment status and agreement status remain distinct.

Examples:

* Payment may be verified while agreement review remains pending.
* Agreement may be signed while required payment remains outstanding.
* Occupancy activation may require both plus additional conditions.

No provider callback may directly activate an agreement or occupancy.

## 46. Payment and Application Separation

An application fee, deposit, or rent payment must not automatically approve a rental application.

Application decisions remain governed by the application workflow and authorized human decision.

## 47. No Internal Wallet

Habitacores will not initially operate a general-purpose wallet, stored-value account, transfer balance, or peer-to-peer money system.

A displayed amount must clearly represent one of:

* Outstanding obligation
* verified payment
* unapplied payment under review
* allocated amount
* refundable amount
* provider settlement amount

It must not be ambiguously labeled `wallet balance`.

## 48. No Unfounded Escrow Claim

Habitacores must not describe funds as escrow unless the required legal, banking, contractual, custody, reconciliation, and release structure has been formally established.

Provider split payments or delayed settlement do not automatically create legal escrow.

## 49. Client Boundaries

Web and mobile clients may:

* Create payment intents through approved APIs
* launch hosted checkout
* display transfer instructions
* upload evidence
* request current payment state
* display verified receipts
* request an authorized refund workflow

Clients may not:

* Verify payment locally
* calculate authoritative balances independently
* allocate payments directly without server authorization
* store provider secrets
* process webhooks
* mark refunds complete
* activate agreements or occupancy from provider callbacks

## 50. Mobile Payment Flow

The mobile application will use:

```text
Mobile client
→ Habitacores API
→ Payment application service
→ Provider adapter
→ Hosted or approved provider experience
→ Provider
→ Habitacores webhook and verification
→ Mobile status refresh or notification
```

Provider credentials remain server side.

## 51. Provider Credentials

Payment credentials must be:

* Environment specific
* server or worker only
* least privilege
* excluded from Git
* excluded from client bundles
* excluded from logs
* rotated under ADR-013
* unavailable to AI models

## 52. Environment Isolation

Development, preview, test, staging, and production payment environments must remain isolated.

Non-production must use:

* Sandbox credentials
* synthetic users
* synthetic obligations
* test transactions
* test webhook secrets
* non-production callbacks
* visible environment labeling

A preview deployment must not create production charges.

## 53. Webhook Environment Validation

A sandbox webhook must not update production data.

Processing must verify the expected:

* Provider environment
* merchant account
* signing secret
* endpoint
* transaction reference namespace
* internal environment association

## 54. Audit Events

Material audit events may include:

```text
PAYMENT_INTENT_CREATED
PAYMENT_ATTEMPT_INITIATED
PAYMENT_EVIDENCE_SUBMITTED
PAYMENT_VERIFIED
PAYMENT_REJECTED
PAYMENT_ALLOCATED
PAYMENT_ALLOCATION_REVERSED
RECEIPT_ISSUED
RECEIPT_CORRECTED
REFUND_REQUESTED
REFUND_APPROVED
REFUND_SUBMITTED
REFUND_COMPLETED
PAYMENT_REVERSED
RECONCILIATION_DISCREPANCY_OPENED
RECONCILIATION_CORRECTION_APPLIED
```

Audit metadata must not contain full provider secrets or unrestricted financial evidence.

## 55. Observability

Metrics may include:

```text
payment_intents_created
payment_attempts_started
provider_attempt_failures
payment_verification_duration
payment_verification_failures
webhook_signature_failures
duplicate_provider_events
payments_verified
manual_reviews_pending
unapplied_payment_amount
reconciliation_discrepancies
refund_failures
settlement_delay
```

Payment IDs, user IDs, references, and account numbers must not become ordinary metric labels.

## 56. Alerts

Priority alerts may include:

* Webhook signature-failure spike
* provider authentication failure
* sustained payment-verification failure
* duplicate-recognition attempt
* cross-tenant allocation attempt
* growing unapplied-payment backlog
* reconciliation discrepancy spike
* refund amount invariant violation
* production callback reaching a non-production endpoint
* unexpected currency activity
* payment state updated without required audit event

## 57. Data Protection

Restricted financial data includes:

* Bank-account information
* payment evidence
* provider payloads
* transfer references
* payer contact details
* refund information
* reconciliation files

Access must be purpose limited, authorized, logged where required, and retention controlled.

## 58. Provider Payload Retention

Habitacores should not copy complete provider payloads indiscriminately into ordinary business tables or logs.

Where raw evidence is retained:

* Storage must be restricted.
* secrets must be redacted.
* retention must be bounded.
* access must be controlled.
* normalized fields remain the operational interface.

## 59. Responsible AI

AI may assist with:

* Classifying non-authoritative reconciliation discrepancies
* summarizing evidence for a finance reviewer
* explaining payment status to users
* identifying likely duplicates for review
* drafting support responses

AI may not:

* Verify payment
* allocate funds
* approve refunds
* change balances
* issue authoritative receipts
* resolve reconciliation discrepancies autonomously
* activate agreements or occupancy
* invent monetary values
* call payment providers with privileged credentials

## 60. AI Output Validation

Any AI-assisted financial output must be:

* Advisory
* grounded in authorized structured data
* schema validated
* visibly non-authoritative
* reviewed where material
* audited under the approved AI architecture

## 61. Testing

The payment architecture requires:

* Money arithmetic tests
* state-transition tests
* provider contract tests
* webhook signature tests
* webhook replay tests
* idempotency tests
* duplicate callback tests
* amount and currency mismatch tests
* cross-tenant tests
* concurrent allocation tests
* refund-limit tests
* reconciliation tests
* environment-isolation tests
* client-boundary tests
* audit tests
* AI-boundary tests

## 62. Duplicate Webhook Test

```text
Given:
A provider sends the same successful-payment event more than once.

When:
Habitacores processes the events.

Then:
One provider event is applied authoritatively.

And:
One Habitacores payment is recognized.

And:
No duplicate allocation or receipt is created.
```

## 63. Redirect Safety Test

```text
Given:
A payer returns to Habitacores through a provider success redirect.

And:
Server-side verification has not completed.

When:
The payment screen is displayed.

Then:
The payment is shown as pending confirmation.

And:
No receipt, agreement activation, or occupancy activation occurs.
```

## 64. Amount-Mismatch Test

```text
Given:
A payment intent expects NGN 1,000,000.

And:
The provider reports a successful payment of NGN 100,000.

When:
Verification executes.

Then:
Habitacores records an amount mismatch or approved underpayment state.

And:
It does not mark the full obligation satisfied.
```

## 65. Currency-Mismatch Test

```text
Given:
An obligation is denominated in NGN.

And:
The provider transaction is denominated in another currency.

When:
Verification executes.

Then:
The payment is not silently allocated to the NGN obligation.

And:
The discrepancy enters an authorized review workflow.
```

## 66. Cross-Tenant Allocation Test

```text
Given:
A payment belongs to Organization A.

And:
An obligation belongs to Organization B.

When:
An allocation is attempted.

Then:
Application authorization rejects it.

And:
PostgreSQL tenant isolation prevents the cross-organization write.
```

## 67. Concurrent Allocation Test

```text
Given:
A verified payment has NGN 500,000 available.

When:
Two concurrent requests each attempt to allocate NGN 400,000.

Then:
At most NGN 500,000 is committed as allocated.

And:
The losing request receives a deterministic conflict response.
```

## 68. Manual Evidence Test

```text
Given:
A tenant uploads a bank-transfer screenshot.

When:
The upload and malware scan complete.

Then:
Habitacores records payment evidence under review.

And:
The payment remains unverified until an approved verification process succeeds.
```

## 69. Refund Invariant Test

```text
Given:
A verified payment has NGN 100,000 refundable.

When:
Concurrent refund requests exceed NGN 100,000.

Then:
The transaction controls prevent the approved or pending refundable total
from exceeding NGN 100,000.
```

## 70. Reconciliation Test

```text
Given:
A provider settlement includes a transaction absent from Habitacores payments.

When:
Reconciliation executes.

Then:
A MISSING_INTERNAL discrepancy is created.

And:
The transaction is not silently imported and allocated without review.
```

## 71. Provider Failure Test

```text
Given:
The payment provider is unavailable.

When:
A payment intent already exists.

Then:
Habitacores preserves the internal intent safely.

And:
It returns a controlled provider-unavailable outcome.

And:
It does not fabricate a provider reference or successful payment.
```

## 72. Codex Boundaries

Every Codex payment work package must specify:

* Organization scope
* obligation
* amount and currency
* payment intent
* provider method
* idempotency
* expected state transitions
* provider verification
* webhook validation
* allocation rules
* receipt rules
* refund rules
* audit events
* observability
* authorization
* RLS
* concurrency control
* tests

Codex must not:

* Use JavaScript `number` for authoritative money
* treat redirects as payment verification
* trust unsigned webhooks
* process provider events without deduplication
* expose provider secrets
* store card details
* allow clients to verify payments
* couple domain code directly to provider SDKs
* overwrite financial history
* allocate across organizations
* issue receipts before verification
* treat uploaded evidence as cleared funds
* create an internal wallet without approval
* describe funds as escrow without an approved legal structure
* let AI approve financial outcomes

## 73. Prohibited Patterns

The following are not approved:

* Provider callback directly activating occupancy
* browser-side payment verification
* mobile-side authoritative balance calculation
* mutable payment amount after verification
* deleting verified payment history
* shared sandbox and production credentials
* unsigned webhook processing
* duplicate event processing without idempotency
* receipt generation from a redirect callback
* allocation before payment verification
* arbitrary cross-obligation allocation
* refund beyond available refundable amount
* provider dashboard as the only reconciliation record
* internal wallet terminology without a wallet architecture
* AI-generated payment amounts or decisions
* complete provider payloads in ordinary logs

## 74. Options Considered

### Option A — Direct Integration Throughout Feature Code

Advantages:

* Fast initial implementation

Disadvantages:

* Provider lock-in
* duplicated verification
* weak reconciliation
* inconsistent retries
* difficult testing
* unsafe financial coupling

Decision:

Rejected.

### Option B — Manual Bank Transfer Only

Advantages:

* Familiar to many users
* low technical integration burden

Disadvantages:

* High manual-review burden
* delayed confirmation
* duplicate and fraud risk
* weak scalability
* difficult reconciliation

Decision:

Supported as one controlled method, not the complete architecture.

### Option C — One Provider as Financial Authority

Advantages:

* Simpler initial reporting
* fewer internal models

Disadvantages:

* Provider lock-in
* provider records do not express Habitacores obligations and allocations
* difficult multi-provider migration
* weak internal audit and reconciliation
* provider outages affect business interpretation

Decision:

Rejected.

### Option D — Habitacores Payment Orchestration With Provider Adapters

Advantages:

* Provider portability
* explicit verification
* stable internal financial model
* safe allocation
* reconciliation
* web and mobile support
* auditability
* controlled manual evidence
* future provider fallback

Disadvantages:

* Greater implementation effort
* reconciliation operations required
* provider adapters require maintenance
* more explicit state machines

Decision:

Approved.

## 75. Positive Consequences

* Provider changes do not redefine the domain model.
* payment verification is server controlled.
* annual rent can be represented accurately.
* manual evidence remains safe and reviewable.
* duplicate callbacks do not duplicate payments.
* collection and allocation remain distinct.
* refunds and reversals preserve history.
* reconciliation becomes a formal control.
* web and mobile share one financial authority.
* wallet and escrow claims are prevented without approval.

## 76. Negative Consequences

* The payment model is more complex than a single status field.
* reconciliation requires ongoing operations.
* provider adapters require contract testing.
* manual transfer support creates review workload.
* partial payments and overpayments require explicit policy.
* settlement timing differs from payment timing.
* provider selection still requires due diligence.

## 77. Risks

### 77.1 False Payment Recognition

Controls:

* Server verification
* amount and currency checks
* signature validation
* provider-account validation
* idempotency
* audit

### 77.2 Duplicate Financial State

Controls:

* Unique references
* provider-event deduplication
* transactional invariants
* immutable attempts
* concurrency tests

### 77.3 Cross-Tenant Allocation

Controls:

* Organization-scoped records
* application authorization
* RLS
* transaction constraints
* cross-tenant tests

### 77.4 Provider Lock-In

Controls:

* Adapter contracts
* normalized statuses
* Habitacores-owned records
* reconciliation imports
* no provider SDK in domain code

### 77.5 Manual Verification Error

Controls:

* Role permissions
* evidence review
* duplicate search
* separation of duties
* audit
* reconciliation

### 77.6 Unauthorized Wallet or Escrow Behavior

Controls:

* Explicit prohibition
* precise terminology
* separate future legal and architectural decision
* review of unapplied-payment handling

## 78. Implementation Sequence

```text
1. Define obligation and charge-type models
2. define payment-intent and payment-attempt models
3. define verified-payment and allocation models
4. define provider-neutral contracts
5. define provider-event ingestion and deduplication
6. implement payment state machines and invariants
7. implement server-side verification
8. implement hosted-checkout workflow
9. implement approved bank-transfer workflow
10. implement manual evidence and finance review
11. implement payment allocation
12. implement immutable receipt issuance
13. implement refund and reversal workflows
14. implement settlement records
15. implement reconciliation engine
16. implement financial dashboards and alerts
17. implement web and mobile payment APIs
18. implement cross-tenant and concurrency tests
19. conduct provider sandbox evaluation
20. select and approve the initial production provider
```

## 79. Deferred Decisions

This ADR does not decide:

* Initial production payment provider
* provider pricing agreement
* exact hosted-checkout implementation
* exact virtual-account strategy
* exact eligible payment methods
* whether partial annual-rent payments are permitted
* security-deposit policy
* service-charge policy
* convenience-fee policy
* provider-fee accounting treatment
* refund approval thresholds
* reconciliation cadence
* settlement-account structure
* organization subaccount model
* split-payment use
* recurring card-token use
* direct-debit strategy
* payment-plan strategy
* legal escrow structure
* regulated wallet structure
* exact accounting-system integration
* exact tax treatment

These require bounded business, legal, finance, and implementation decisions.

## 80. Supersession Conditions

This ADR should be reviewed when:

* Habitacores enters another country.
* a second currency becomes operationally necessary.
* multiple payment providers operate simultaneously.
* enterprise customers require their own provider accounts.
* a regulated wallet or escrow structure is proposed.
* marketplace collection or landlord payouts become core.
* provider settlement volume requires dedicated financial infrastructure.
* bank-statement automation becomes authoritative.
* payment regulations materially change.
* a full accounting ledger is introduced.

## 81. Final Decision Statement

Habitacores will implement a provider-neutral payment orchestration architecture for annual rent and other approved property-related obligations.

Habitacores will maintain authoritative internal records for obligations, payment intents, payment attempts, verified payments, evidence, allocations, receipts, refunds, reversals, provider events, settlements, and reconciliation discrepancies.

External providers will process payment methods and provide evidence. They will not independently determine Habitacores payment allocation, agreement status, occupancy status, application decisions, receipts, or contractual completion.

Provider success redirects, client callbacks, uploaded screenshots, unverified webhooks, and initial API responses will not constitute authoritative payment verification.

Before recognizing provider payment, Habitacores will validate the provider transaction, reference, final status, amount, currency, merchant environment, payment intent, and duplicate history through trusted server-side processes.

Webhook processing will verify signatures, validate schemas, deduplicate provider events, preserve bounded evidence, and apply financial transitions idempotently. Complex processing will occur asynchronously where appropriate.

Verification and allocation will remain separate. A verified payment may be allocated only to eligible obligations within the same organization and currency, subject to available amount, outstanding obligation, authorization, concurrency controls, and audit.

Payment evidence uploaded through web or mobile will remain quarantined and unverified until the approved verification process succeeds. Uploading evidence will not satisfy an obligation, issue a final receipt, activate an agreement, or activate occupancy.

Financial history will be append-only. Corrections, allocation reversals, refunds, provider reversals, chargebacks, and reconciliation adjustments will preserve links to original records and their audit evidence.

Habitacores will reconcile internal payment and allocation records against provider transactions, settlements, refunds, reversals, and approved bank evidence. Discrepancies will create explicit review workflows rather than silent mutation.

The initial collection experience should prefer provider-hosted checkout and approved transfer methods. Habitacores will not store card data or expose provider secrets to browsers, mobile applications, logs, source control, audit metadata, or AI systems.

Habitacores will not initially operate a general-purpose wallet or describe funds as escrow. Any stored-value, payout, custodial, marketplace, or escrow model requires a separate approved legal, financial, compliance, and architectural decision.

AI may assist authorized users by summarizing or classifying financial evidence and discrepancies, but it will not verify payments, allocate funds, approve refunds, issue authoritative receipts, modify balances, resolve reconciliation autonomously, or activate agreements or occupancy.

No provider, webhook, redirect, browser, mobile application, uploaded evidence, finance reviewer, feature flag, support operator, coding agent, or AI model may bypass Habitacores money representation, authentication, membership, tenant isolation, authorization, verification, allocation, audit, reconciliation, concurrency, environment, or financial-history controls.