# ADR-006 — Money Representation and Financial Arithmetic

## Decision Record

| Field | Value |
|---|---|
| Product | Habitacores |
| Decision | Money Representation and Financial Arithmetic |
| ADR | ADR-006 |
| Version | 0.1 |
| Status | Proposed |
| Decision Type | Foundational Finance and Data Architecture |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Authoritative Amount Representation | Integer minor units |
| PostgreSQL Storage Type | `bigint` |
| TypeScript Arithmetic Type | `bigint` |
| Currency Representation | Explicit ISO 4217 code |
| Primary Initial Currency | NGN |
| Financial Mutation Model | Append, allocate, adjust, reverse |
| Floating-Point Arithmetic | Prohibited for authoritative money |
| AI Financial Authority | Prohibited |
| Last Updated | 1 August 2026 |

## 1. Context

Habitacores will manage financial records including:

- Annual rent charges
- security deposits
- service charges
- administrative fees
- utility-related charges
- payment evidence
- payment transactions
- payment verification
- payment allocations
- partial payments
- overpayments
- credits
- refunds
- reversals
- receipts
- adjustments
- organization balances
- resident balances
- agreement financial prerequisites
- occupancy financial prerequisites
- reconciliation records
- provider settlements
- future commissions and professional-service charges

Financial representation must remain correct across:

- PostgreSQL
- TypeScript
- APIs
- forms
- reports
- exports
- receipts
- payment-provider integrations
- asynchronous workers
- audits
- future multi-currency expansion
- Responsible AI features

The system must avoid floating-point errors, hidden rounding, currency ambiguity, destructive edits, and inconsistent balance calculations.

ADR-004 established PostgreSQL as the authoritative system of record.

ADR-005 established UUIDv7 as the default identifier strategy.

ADR-006 must now define how Habitacores represents, stores, parses, calculates, compares, allocates, formats, audits, and reconciles money.

## 2. Decision Drivers

The decision is driven by the need to:

1. Avoid floating-point rounding errors.
2. preserve exact financial arithmetic.
3. support Nigerian naira accurately.
4. support future currencies.
5. keep currency explicit.
6. support partial payments.
7. support overpayments and credits.
8. support payment allocation across charges.
9. support refunds and reversals.
10. support immutable audit history.
11. support database constraints.
12. support transactional concurrency.
13. support provider reconciliation.
14. prevent browser-side authority.
15. prevent AI-generated arithmetic from becoming authoritative.
16. support receipts and legal records.
17. maintain consistency across web and workers.
18. support safe exports and reporting.
19. provide clear Codex implementation boundaries.
20. avoid future financial-data migration caused by an inadequate representation.

## 3. Decision

Habitacores will represent authoritative monetary values as integer minor units.

The approved model is:

```text
Amount:
Signed integer minor units

Database:
PostgreSQL bigint

Application arithmetic:
TypeScript bigint

Currency:
Explicit ISO 4217 code

Scale:
Resolved from approved currency metadata

Formatting:
Presentation-only conversion

Parsing:
Validated server-side conversion from user input

Authoritative arithmetic:
Money value object and approved finance services
````

For Nigerian naira:

```text
Major-unit display:
₦1,250.50

Currency:
NGN

Minor-unit amount:
125050

Scale:
2
```

The system must never infer currency solely from locale, organization, symbol, or payment provider.

## 4. PostgreSQL Representation

Authoritative monetary columns will normally use:

```sql
amount_minor bigint not null
currency_code varchar(3) not null
```

Illustrative names include:

```text
charge_amount_minor
payment_amount_minor
allocated_amount_minor
refund_amount_minor
credit_amount_minor
fee_amount_minor
tax_amount_minor
balance_amount_minor
```

The exact naming should remain context specific.

## 5. Why `bigint`

PostgreSQL `bigint` is selected because it:

* Stores exact integer values
* avoids binary floating-point behavior
* supports large monetary values
* supports database constraints
* supports transactional arithmetic
* maps naturally to TypeScript `bigint`
* remains suitable for future higher-value institutional operations

The application must validate business-specific upper bounds even though `bigint` provides a large technical range.

## 6. TypeScript Representation

Authoritative monetary arithmetic will use TypeScript `bigint`.

Illustrative value object:

```typescript
type CurrencyCode = string;

interface Money {
  readonly minorAmount: bigint;
  readonly currency: CurrencyCode;
}
```

A class or factory-based implementation may be used to enforce invariants.

Raw `bigint` values should not be passed around without currency context in finance-domain code.

## 7. Money Value Object

The approved Money abstraction should provide operations such as:

```typescript
interface MoneyValue {
  readonly minorAmount: bigint;
  readonly currency: CurrencyCode;

  add(other: MoneyValue): MoneyValue;
  subtract(other: MoneyValue): MoneyValue;
  negate(): MoneyValue;
  compare(other: MoneyValue): number;
  isZero(): boolean;
  isPositive(): boolean;
  isNegative(): boolean;
}
```

All binary monetary operations must validate matching currencies.

## 8. Currency Mismatch

The following operation is prohibited:

```text
NGN 100000
+
USD 100000
```

without an explicit approved conversion process.

A currency mismatch must fail with a stable domain error such as:

```text
CURRENCY_MISMATCH
```

The system must not silently convert or add values across currencies.

## 9. Currency Codes

Currencies will use uppercase ISO 4217 alphabetic codes.

Examples:

```text
NGN
USD
CAD
GBP
EUR
```

The primary launch currency is:

```text
NGN
```

The architecture must not assume NGN is the only currency forever.

## 10. Currency Metadata

Habitacores should maintain an approved currency catalogue containing at least:

* Currency code
* display name
* symbol
* minor-unit scale
* active status
* formatting metadata where needed

Illustrative:

```typescript
interface CurrencyDefinition {
  code: CurrencyCode;
  name: string;
  symbol?: string;
  minorUnitScale: number;
  active: boolean;
}
```

Currency scale must not be derived from symbol length or locale.

## 11. Currency Scale

For a currency with scale `2`:

```text
1 major unit = 100 minor units
```

For NGN:

```text
₦1.00 = 100 kobo
```

The system must support currencies whose scales differ.

It must not hardcode multiplication by 100 throughout the codebase.

## 12. Parsing User Input

User-entered monetary values normally arrive as decimal strings.

Example:

```text
"1250.50"
```

Approved parsing flow:

```text
Raw string
→ Trim and normalize approved input
→ Validate decimal syntax
→ Resolve currency scale
→ Reject excessive fractional digits
→ Convert exactly to minor-unit bigint
→ Create Money value
```

The parser must not use JavaScript floating-point multiplication.

## 13. Prohibited Parsing Pattern

The following is prohibited:

```typescript
const minor = Math.round(Number(input) * 100);
```

This can introduce binary floating-point behavior, unsafe-range issues, and hidden rounding.

## 14. Exact Parsing Example

For:

```text
input = "1250.50"
currency = NGN
scale = 2
```

The parser should derive:

```text
whole = 1250
fraction = 50
minorAmount = 125050n
```

The exact implementation may use validated string manipulation or an approved decimal library at external conversion boundaries.

## 15. Input Precision

For NGN scale `2`, inputs such as:

```text
100.123
```

must not be silently rounded by default.

The operation should either:

* Reject the value
* or apply an explicitly approved rounding policy for that workflow

The default authoritative parsing policy is rejection of excess precision.

## 16. Formatting

Formatting is a presentation concern.

The system may format:

```text
125050n NGN
```

as:

```text
₦1,250.50
```

based on:

* Currency code
* approved locale
* currency scale
* display policy

Formatting must not alter the authoritative stored amount.

## 17. Serialization

JavaScript `bigint` is not directly compatible with ordinary JSON serialization.

Therefore API and event contracts must serialize minor amounts as strings.

Example:

```json
{
  "minorAmount": "125050",
  "currency": "NGN"
}
```

The following should not be used for authoritative large amounts:

```json
{
  "minorAmount": 125050
}
```

unless the contract guarantees the value remains safely representable and the architecture explicitly permits it.

The default is string serialization.

## 18. API Money Contract

A standard API contract should resemble:

```typescript
interface MoneyDto {
  minorAmount: string;
  currency: string;
}
```

At trusted server boundaries, the string is validated and converted to `bigint`.

Clients must treat the minor amount as an exact integer string.

## 19. Database-to-TypeScript Mapping

The PostgreSQL driver may return `bigint` values as strings depending on configuration.

The database package must explicitly map authoritative money values to TypeScript `bigint`.

This conversion must occur centrally rather than differently in each feature.

## 20. No JavaScript `number`

JavaScript `number` is prohibited for:

* Authoritative charge amounts
* payment amounts
* allocations
* refunds
* balances
* credits
* deposits
* taxes
* fees
* commissions
* provider reconciliation totals
* receipt totals
* legal financial totals

`number` may be used only for non-authoritative presentation values where precision loss cannot affect financial state.

## 21. No Floating-Point Database Types

The following PostgreSQL types are prohibited for authoritative money:

```text
real
double precision
```

PostgreSQL `money` is also not selected because Habitacores requires explicit currency and controlled application semantics.

## 22. Decimal Libraries

An arbitrary-precision decimal library may be used at narrowly approved boundaries such as:

* Foreign-exchange calculations
* percentage calculations
* tax calculations
* provider decimal payload parsing
* prorated computations
* rate calculations

The final result must be converted through an explicit rounding policy into integer minor units before becoming authoritative money.

A decimal library must not create an alternative monetary representation throughout the application.

## 23. Financial Entity Structure

A financial record must normally preserve:

* Internal identifier
* organization identifier
* amount in minor units
* currency code
* financial type
* status
* effective or occurred time
* source
* actor
* correlation identifier
* related resource identifiers
* audit metadata

## 24. Charge Model

A charge represents an amount expected or owed.

A charge should preserve:

* Original amount
* currency
* charge type
* due date
* effective date
* related agreement, unit, occupancy, or application
* current status
* adjustment and reversal history
* allocated amount
* outstanding amount

The original amount must not be overwritten casually after issuance.

## 25. Payment Model

A payment represents funds received or recorded as received.

A payment should preserve:

* Gross received amount
* currency
* provider or evidence source
* verification status
* verified amount where different
* available amount
* allocated amount
* refunded amount
* reversed amount
* provider reference
* occurred date
* verification metadata

Payment evidence is not itself payment verification.

## 26. Allocation Model

An allocation links a payment amount to a charge.

An allocation should preserve:

* Payment ID
* charge ID
* allocated amount
* currency
* allocation time
* actor
* organization
* status
* reversal linkage where applicable
* audit and correlation metadata

Allocation records should be append oriented.

## 27. Allocation Currency Rule

A payment may be allocated only to a charge with the same currency unless an explicit exchange process has created a converted authoritative amount.

The following direct allocation is prohibited:

```text
USD payment
→ NGN rent charge
```

without an approved conversion record.

## 28. Allocation Constraints

For an active allocation:

```text
allocated amount > 0
```

The total active allocations against one payment must not exceed the payment’s available allocatable amount.

The total active allocations against one charge must not exceed the charge’s outstanding amount unless the workflow explicitly supports and records over-allocation.

The default is to prevent over-allocation.

## 29. Partial Payments

Partial payments are supported.

Example:

```text
Charge:
NGN 1,000,000.00

Payment:
NGN 600,000.00

Allocation:
NGN 600,000.00

Outstanding:
NGN 400,000.00
```

The charge remains partially paid until the outstanding amount reaches zero through valid active allocations or approved credits.

## 30. Overpayments

An overpayment occurs when verified funds exceed currently allocated charges.

Example:

```text
Payment:
NGN 1,200,000.00

Allocated:
NGN 1,000,000.00

Unallocated credit:
NGN 200,000.00
```

The unallocated amount must remain explicit.

It must not disappear into a generic balance or be silently treated as revenue.

## 31. Credit Balances

A resident or organization credit balance should be derived from authoritative records such as:

* Verified payments
* allocations
* refunds
* reversals
* credits
* adjustments

A stored balance may be maintained for performance only if it is transactionally updated and reconcilable to the underlying ledger records.

## 32. Refunds

A refund must reference the payment or credit from which value is returned.

Refund records should preserve:

* Requested amount
* approved amount
* executed amount
* currency
* reason
* requester
* approver
* provider reference
* status
* failure state
* execution time
* reversal relationship
* audit record

A refund must not be represented by deleting or reducing the original payment.

## 33. Reversals

Incorrect financial events should normally be corrected through reversal records.

Example:

```text
Original allocation:
+ NGN 500,000.00

Reversal allocation:
- NGN 500,000.00
```

or through an explicit reversal linkage with equivalent financial effect.

The original event remains visible for audit and reconciliation.

## 34. Adjustments

An adjustment must record:

* Amount
* direction
* currency
* reason
* actor
* approval where required
* source record
* effective time
* audit metadata

Adjustments must not become generic tools for bypassing proper charge, payment, refund, or reversal workflows.

## 35. Negative Amount Policy

Negative amounts are permitted only for record types whose semantics explicitly allow them.

Potential examples:

* Reversals
* correcting adjustments
* ledger postings
* credit entries

Primary charges, payments, and allocations should normally store positive magnitudes with direction represented by record type or posting side.

The exact sign convention must be consistent within each finance model.

## 36. Zero Amount Policy

Zero-value financial records are generally prohibited unless they have legitimate business meaning.

Examples that may justify zero:

* A waived fee recorded through a waiver event
* a zero-cost service item
* a settlement record establishing no remaining liability

A zero amount must not be used as a placeholder for unknown.

Unknown amount and zero amount are different facts.

## 37. Balance Formula

A balance must be derived from explicitly defined financial components.

Illustrative charge balance:

```text
outstanding
=
charge amount
+ active debit adjustments
- active credit adjustments
- active allocations
```

The exact formula must be documented per financial resource.

No UI component may invent its own balance formula.

## 38. Payment Availability Formula

Illustrative:

```text
available payment amount
=
verified amount
- active allocations
- executed refunds
- payment reversals
```

Any additional financial event affecting availability must be included explicitly.

## 39. Balance Source of Truth

The authoritative source is the underlying financial records.

A cached or stored balance is a projection.

If a stored balance is used, it must be:

* Updated transactionally
* protected by constraints
* reconciled regularly
* rebuildable
* traceable to source records

## 40. Transactional Integrity

Material financial operations must execute inside explicit database transactions.

Examples include:

* Verifying a payment
* allocating payment
* reversing allocation
* recording adjustment
* approving refund
* finalizing refund
* issuing receipt
* activating agreement financial prerequisites
* activating occupancy financial prerequisites

## 41. Allocation Transaction

A payment allocation transaction should:

1. Load the payment.
2. load the charge.
3. validate organization ownership.
4. validate currency equality.
5. validate payment verification state.
6. validate available amount.
7. validate charge outstanding amount.
8. validate requested allocation.
9. create allocation.
10. update approved projections.
11. append audit event.
12. append outbox event.
13. commit atomically.

## 42. Concurrency

Financial workflows must protect against:

* Double allocation
* duplicate payment verification
* duplicate refund execution
* stale charge adjustment
* multiple receipts for one issuance event
* repeated provider callbacks
* concurrent balance updates
* repeated form submission

Controls may include:

* Row locks
* conditional updates
* unique constraints
* optimistic versions
* idempotency keys
* transaction isolation
* provider-event uniqueness

## 43. Idempotency

Material financial commands must support idempotency.

Examples:

* Payment initiation
* payment verification
* allocation
* receipt issuance
* refund request
* refund execution
* provider callback processing
* reconciliation import

An idempotent retry must return or reconstruct the prior authoritative result rather than create a duplicate financial event.

## 44. Financial Event Immutability

Once finalized, financial records should not be destructively edited.

Immutable or append-oriented records include:

* Verified payment event
* allocation
* reversal
* refund execution
* receipt issuance
* reconciliation event
* financial audit event

Corrections require additional records.

## 45. Draft Financial Records

Draft or pending records may be editable before authorization or finalization.

Examples:

* Draft charge
* unsubmitted refund request
* unverified payment evidence
* draft adjustment request

The transition to finalized state must be explicit and audited.

## 46. Receipt Values

A receipt must preserve the values that were authoritative at issuance.

A receipt record should include:

* Receipt ID
* human-readable receipt reference
* payment ID
* allocated amount or receipt amount
* currency
* issue time
* issuing organization
* recipient
* relevant charge references
* immutable rendered-document reference
* status
* supersession or void history

Later changes must not silently rewrite an already issued receipt.

## 47. Agreement Financial Values

An agreement version must preserve its approved financial terms independently of later charge adjustments.

Relevant values may include:

* Annual rent
* security deposit
* service charge
* administrative fee
* total due
* currency
* due dates
* payment schedule where approved

The agreement document should derive from the frozen agreement version, not a mutable current property price.

## 48. Occupancy Financial Prerequisites

Occupancy activation may depend on deterministic financial conditions.

Examples:

* Required charge exists
* required payment has been verified
* required allocation has been completed
* required deposit condition is satisfied
* no unresolved reversal invalidates payment status

AI output, payment evidence upload, provider UI success, or browser state cannot satisfy these conditions by itself.

## 49. Annual Rent

Annual rent is a core Habitacores financial concept.

The system must model:

* Rent amount
* currency
* rental period
* agreement version
* due date
* payment status
* allocations
* outstanding amount
* receipts
* adjustments
* renewal relationship

Annual rent must not be represented merely as twelve monthly floating-point calculations unless a specific agreement requires installments.

## 50. Proration

Proration is not automatic.

A proration policy must define:

* Applicable workflow
* time basis
* inclusive or exclusive dates
* day-count method
* rounding point
* remainder allocation
* currency scale
* legal and agreement basis

Prorated amounts must resolve to exact minor units.

## 51. Percentage Calculations

Percentage-derived amounts may include:

* Fees
* commissions
* taxes
* penalties
* discounts

The calculation must define:

* Base amount
* rate representation
* rate scale
* intermediate precision
* rounding mode
* rounding stage
* minimum or maximum
* currency

## 52. Rate Representation

Rates should not use uncontrolled floating-point values.

Potential exact representations include:

```text
Basis points
Rational numerator and denominator
Fixed-scale decimal
```

Example:

```text
2.5% = 250 basis points
```

The chosen rate type should be explicit in the relevant domain module.

## 53. Rounding Policy

Rounding must be explicit.

The system must define, per calculation:

* Whether rounding is permitted
* rounding mode
* stage at which rounding occurs
* treatment of remainders
* treatment across line items
* treatment across tax components

No global implicit rounding rule should be assumed for all workflows.

## 54. Default Rounding Direction

For direct user-entered monetary amounts, excess precision should be rejected.

For approved calculated amounts, a defined deterministic rounding method must be used.

The exact default calculation rounding mode will be selected during bounded finance implementation.

## 55. Allocation of Remainders

Where a total is divided across multiple line items, rounding remainders must be allocated deterministically.

Potential methods include:

* Largest remainder
* final line adjustment
* explicit priority order

The method must ensure:

```text
sum of allocated minor units
=
authoritative total minor units
```

## 56. Tax

Tax treatment requires its own finance and compliance rules.

The architecture must support:

* Tax-inclusive or tax-exclusive pricing
* tax type
* rate
* taxable base
* tax amount
* currency
* jurisdiction
* effective date
* exemption
* rounding

This ADR does not define Nigerian tax obligations.

## 57. Fees

Fees must be represented as explicit financial components.

A payment-provider fee must not silently reduce a resident’s credited payment unless the approved business rule says so.

The system should distinguish:

* Gross amount
* provider fee
* net settlement
* resident credited amount
* organization receivable amount

## 58. Provider Amounts

Payment-provider payloads may express amounts as:

* Minor-unit integers
* decimal strings
* major-unit numbers
* currency-specific formats

Each provider adapter must normalize the provider amount into the Habitacores Money model.

The provider payload remains evidence and must not bypass verification and reconciliation.

## 59. Provider Precision Validation

A provider amount must be validated against:

* Expected currency
* expected scale
* transaction reference
* environment
* provider account
* expected amount where applicable
* provider status
* settlement status where applicable

Unexpected precision or currency must produce a controlled reconciliation exception.

## 60. Gross and Net Settlement

The architecture must distinguish:

```text
Customer payment
Provider fee
Tax or withholding
Net settlement
```

A single generic `amount` column must not represent all four concepts ambiguously.

## 61. Reconciliation

Reconciliation compares Habitacores records with external financial evidence.

A reconciliation process should preserve:

* Internal payment amount
* provider transaction amount
* provider fee
* provider settlement amount
* currency
* status
* discrepancy
* reconciliation time
* source file or event
* actor or worker
* resolution

## 62. Reconciliation Differences

A difference must never be silently ignored.

Potential states include:

```text
MATCHED
AMOUNT_MISMATCH
CURRENCY_MISMATCH
MISSING_INTERNAL_RECORD
MISSING_PROVIDER_RECORD
DUPLICATE_PROVIDER_RECORD
PENDING_SETTLEMENT
MANUAL_REVIEW_REQUIRED
```

## 63. Multi-Currency Expansion

Habitacores may support additional currencies later.

Each financial obligation must remain denominated in one explicit currency.

Multi-currency support does not mean direct arithmetic across currencies.

It requires:

* Currency-specific amounts
* conversion records
* exchange rates
* rate source
* effective time
* rounding policy
* realized gain or loss rules where applicable

## 64. Foreign Exchange

Currency conversion must produce a new authoritative Money value.

A conversion record should preserve:

* Source amount
* source currency
* destination amount
* destination currency
* exchange rate
* rate source
* effective time
* calculation precision
* rounding rule
* actor or provider
* correlation identifier

The original amount must remain unchanged.

## 65. Exchange Rate Representation

Exchange rates should use an exact decimal or rational representation with an explicitly defined scale.

A JavaScript floating-point rate must not become authoritative.

The final exchange-rate architecture is deferred until multi-currency conversion is implemented.

## 66. Organization Currency Policy

An organization may have:

* Default operating currency
* permitted currencies
* settlement currency
* reporting currency

The default currency improves workflow convenience but does not eliminate the currency field from financial records.

## 67. Property Currency Policy

A listing, property, agreement, or charge may declare an approved currency.

Currency changes must be controlled because they affect:

* Prices
* agreements
* charges
* applications
* payments
* reports
* receipts

Changing a property’s default currency must not rewrite historical financial records.

## 68. Historical Integrity

Historical records retain the currency and amount used when they were created or finalized.

A later currency-policy change must not mutate:

* Existing agreements
* issued charges
* verified payments
* allocations
* receipts
* refunds
* audit records

## 69. Authorization

Financial operations require server-side authorization.

Permissions may include:

* View financial summary
* create charge
* adjust charge
* verify payment
* allocate payment
* reverse allocation
* request refund
* approve refund
* execute refund
* issue receipt
* reconcile provider records
* export financial data

Possession of a financial record identifier does not grant authority.

## 70. Separation of Duties

Sensitive workflows may require separation of duties.

Examples:

* Submitter cannot verify their own payment evidence
* refund requester cannot approve the same refund
* refund approver may not execute above a threshold
* adjustment creator may require separate approval
* organization staff cannot alter platform settlement configuration without platform authority

The exact matrix will be implemented through authorization rules.

## 71. Audit

Material financial actions must create audit events.

Examples:

* Charge created
* charge adjusted
* payment evidence submitted
* payment verified
* payment rejected
* payment allocated
* allocation reversed
* refund requested
* refund approved
* refund executed
* receipt issued
* reconciliation exception resolved

Audit metadata must avoid storing unrestricted payment evidence contents.

## 72. Outbox Events

Financial transactions requiring asynchronous effects should create outbox events in the same transaction.

Examples:

```text
PaymentVerified
PaymentAllocated
ReceiptIssued
RefundApproved
RefundExecuted
FinancialDiscrepancyDetected
```

Outbox consumers must remain idempotent.

## 73. Responsible AI Boundary

AI may assist with:

* Summarizing financial records
* explaining transaction history
* identifying records needing human review
* drafting reconciliation notes
* categorizing support questions
* detecting potential anomalies for review

AI must not authoritatively:

* Calculate balances
* verify payments
* allocate funds
* approve refunds
* execute refunds
* set exchange rates
* determine tax liability
* issue receipts
* activate agreements
* activate occupancy
* modify authoritative financial records

## 74. AI Calculation Rule

Any amount displayed as authoritative must come from deterministic finance code or the database.

AI-generated arithmetic may be shown only as non-authoritative explanatory content and must be clearly labeled.

Where an AI summary includes amounts, it should reference authoritative source fields rather than independently recalculate them.

## 75. AI Retrieval

AI finance retrieval must be:

* Authorized
* organization scoped
* purpose limited
* field allowlisted
* minimized
* audited
* safe for provider submission
* stripped of unnecessary payment evidence and identity data

AI must not receive unrestricted ledger access.

## 76. Reporting

Financial reports must use the authoritative Money representation.

Reports should expose:

* Amount
* currency
* period
* record status
* calculation basis
* generated time
* organization
* relevant filters

Totals must group by currency unless an explicit conversion basis is used.

## 77. Multi-Currency Reports

A report containing multiple currencies must not present one combined total without an approved conversion.

Approved alternatives include:

```text
NGN total
USD total
CAD total
```

or:

```text
Converted reporting total
with documented exchange-rate basis
```

## 78. CSV and Spreadsheet Exports

Exports should include machine-safe fields.

Recommended:

```text
amount_minor
currency_code
amount_display
```

The minor amount should be exported as text where spreadsheet software may otherwise convert or truncate large integers.

## 79. Receipt and PDF Formatting

Rendered documents must format amounts consistently.

A receipt or agreement should preserve both:

* Numeric amount
* currency

Where words are included, the textual amount must be generated deterministically from the authoritative amount.

AI must not generate legal amount-in-words text without deterministic validation.

## 80. Search and Filtering

Money filtering should operate on:

* Minor-unit amounts
* currency
* record type
* status
* date

A range filter must not compare amounts across currencies as though they were equivalent.

## 81. Database Constraints

Appropriate constraints may include:

```text
currency_code length = 3
amount_minor within approved sign rule
allocation amount > 0
refund amount > 0
provider fee >= 0
```

Constraints should reflect the record’s semantics.

A generic database rule that every amount is nonnegative is not sufficient because reversal and ledger records may have different sign models.

## 82. Unique Constraints

Potential financial uniqueness constraints include:

* Provider event identity by provider and environment
* idempotency key by organization and operation
* receipt reference by issuing scope
* active allocation reversal relationship
* provider transaction reference by provider account and environment

Exact constraints will be finalized during schema design.

## 83. Indexing

Likely financial indexes include:

```text
organization_id, status, created_at
organization_id, currency_code
organization_id, charge_id
organization_id, payment_id
provider, environment, provider_transaction_id
organization_id, receipt_reference
```

Indexes must follow measured query patterns.

## 84. Financial Dates

Financial records must use explicit date and timestamp fields.

Examples:

```text
occurred_at
received_at
verified_at
allocated_at
refunded_at
issued_at
effective_at
due_at
settled_at
```

UUID timestamps, database insertion time, or provider callback time must not substitute automatically for the correct business event time.

## 85. Time Zones

Financial timestamps should be stored in an approved absolute representation.

Local display may use organization or user time zone.

Date-only financial concepts, such as a due date, should remain date values rather than midnight timestamps where possible.

## 86. Unknown Financial Values

Unknown values must remain unknown.

Examples:

* Unknown provider fee must not become zero
* unknown settlement amount must not become payment amount
* unknown exchange rate must not become one
* unverified payment must not become verified amount
* pending refund amount must not become executed amount

Zero and unknown have different meanings.

## 87. Error Codes

Finance operations should return stable errors such as:

```text
INVALID_MONEY
INVALID_CURRENCY
CURRENCY_MISMATCH
EXCESS_PRECISION
AMOUNT_OUT_OF_RANGE
PAYMENT_NOT_VERIFIED
INSUFFICIENT_PAYMENT_BALANCE
CHARGE_ALREADY_SETTLED
ALLOCATION_EXCEEDS_PAYMENT
ALLOCATION_EXCEEDS_CHARGE
REFUND_EXCEEDS_AVAILABLE_CREDIT
FINANCIAL_CONFLICT
STALE_FINANCIAL_STATE
```

## 88. Observability

Safe finance telemetry may include:

* Operation
* organization ID
* resource IDs
* currency
* result code
* duration
* idempotency outcome
* retry outcome
* reconciliation status
* correlation ID

Ordinary logs should not contain:

* Bank-account numbers
* full provider payloads
* payment evidence
* unrestricted personal data
* secret tokens

Amounts may require restricted telemetry treatment according to operational policy.

## 89. Testing

Finance tests must cover:

* Exact addition
* exact subtraction
* zero
* negative policy
* currency mismatch
* parsing
* formatting
* excess precision
* large values
* serialization
* partial payment
* overpayment
* allocation
* reversal
* refund
* concurrency
* idempotency
* provider normalization
* reconciliation mismatch
* organization isolation

## 90. Property-Based Tests

Property-based testing should be considered for Money operations.

Useful invariants include:

```text
a + 0 = a
a - a = 0
(a + b) - b = a
sum of allocations <= available payment
sum of allocations <= charge amount where over-allocation is prohibited
serialization round trip preserves amount and currency
```

## 91. Boundary Tests

Tests should cover:

* Maximum permitted business amount
* minimum permitted amount
* PostgreSQL bigint limits
* JavaScript bigint conversion
* JSON serialization
* user-input precision
* provider decimal formats
* currencies with different scales

## 92. Integration Tests

Integration tests are required for:

* Transaction rollback
* double allocation prevention
* concurrent refund prevention
* active reversal effects
* financial constraints
* provider-event idempotency
* receipt uniqueness
* balance projection consistency
* outbox consistency

## 93. Reconciliation Tests

Reconciliation tests should include:

* Exact match
* fee difference
* amount mismatch
* currency mismatch
* duplicate provider event
* delayed settlement
* missing internal payment
* missing provider transaction
* reversal after settlement

## 94. Migration Requirements

Money columns must be introduced with:

* `bigint`
* explicit currency
* appropriate nullability
* appropriate sign constraints
* appropriate defaults only where meaningful
* indexes and uniqueness where required

A migration must not introduce authoritative monetary columns as floating-point values.

## 95. Legacy Money Migration

When importing decimal or floating-point legacy amounts:

1. Identify source currency.
2. identify source precision.
3. define conversion rule.
4. define rounding policy.
5. detect invalid or excessive precision.
6. convert to minor-unit integer.
7. reconcile totals.
8. retain source references.
9. record exceptions.
10. audit the migration.

## 96. No Default Zero

A monetary column must not receive a default of zero merely to avoid null handling.

A default zero is acceptable only where zero is the true semantic default.

Examples requiring caution:

* Provider fee
* tax
* refund amount
* settlement amount
* verified amount
* allocated amount

Derived projections may default to zero where their definition guarantees it.

## 97. Codex Boundaries

Every Codex finance work package must specify:

* Currency
* amount representation
* affected financial records
* transaction boundary
* sign convention
* rounding rule
* allocation rule
* idempotency requirement
* audit event
* outbox event
* authorization
* tests
* prohibited scope

Codex must not:

* Use JavaScript `number` for authoritative money
* use floating-point database types
* omit currency
* mix currencies
* use `Math.round` for authoritative parsing
* overwrite finalized financial records
* delete financial history
* bypass transaction boundaries
* trust client-calculated balances
* trust AI-calculated balances
* invent a rounding rule
* convert unknown to zero
* permit over-allocation without explicit approval

## 98. Prohibited Patterns

The following are not approved:

* Authoritative money stored as `double precision`
* authoritative money stored as `real`
* PostgreSQL `money` as the core representation
* JavaScript `number` for authoritative arithmetic
* currency inferred from symbol
* currency omitted from financial records
* float multiplication to obtain minor units
* silent rounding of user input
* cross-currency addition
* cross-currency allocation
* destructive edits to finalized records
* deleting payments to correct errors
* deleting allocations to correct errors
* balances calculated independently in UI components
* AI deciding authoritative amounts
* generic untyped amount fields
* unknown amounts defaulted to zero
* provider net settlement treated automatically as customer payment

## 99. Options Considered

### Option A — JavaScript Number and PostgreSQL Double Precision

Advantages:

* Simple syntax
* familiar arithmetic
* common in prototypes

Disadvantages:

* Binary floating-point behavior
* precision errors
* unsafe large integers
* hidden rounding
* unsuitable for authoritative finance

Decision:

Rejected.

### Option B — PostgreSQL Numeric and Decimal Library Everywhere

Advantages:

* Exact decimal representation
* flexible precision
* suitable for rates and complex decimals

Disadvantages:

* More complex application mapping
* inconsistent scale risk
* heavier arithmetic model for ordinary currency amounts
* easier to introduce varying precision across records
* unnecessary complexity for minor-unit currencies

Decision:

Rejected as the default money representation.

Exact decimal remains approved for rates and conversion intermediates.

### Option C — PostgreSQL Money Type

Advantages:

* Built-in monetary formatting and operations
* concise database representation

Disadvantages:

* Locale-sensitive behavior
* currency not modeled explicitly per value
* less portable application semantics
* unsuitable for multi-currency architecture

Decision:

Rejected.

### Option D — Integer Minor Units With Explicit Currency

Advantages:

* Exact arithmetic
* deterministic comparisons
* strong database support
* natural allocation logic
* clear currency association
* straightforward reconciliation
* compatible with TypeScript `bigint`
* suitable for multi-currency expansion

Disadvantages:

* Requires scale metadata
* requires custom parsing and formatting
* JSON serialization requires strings
* percentage and exchange calculations need explicit decimal handling

Decision:

Approved.

## 100. Positive Consequences

* Exact financial arithmetic
* no binary floating-point error
* explicit currency
* safe partial-payment support
* safe allocation support
* strong reconciliation model
* deterministic receipts
* robust audit history
* future multi-currency capability
* clear AI exclusion from financial authority
* database constraints can protect balances and records

## 101. Negative Consequences

* Developers must understand minor units.
* JSON cannot serialize `bigint` directly.
* parsing and formatting utilities are required.
* rate calculations require separate exact-decimal handling.
* currency scale metadata must be maintained.
* financial records require more explicit fields and workflows.
* immutable correction patterns require more records than destructive editing.

## 102. Risks

### 102.1 Amount Without Currency

Controls:

* Money value object
* schema requirements
* validation
* TypeScript contracts
* tests

### 102.2 Number Conversion

Controls:

* Lint rules
* code review
* centralized parsing
* boundary tests
* Codex restrictions

### 102.3 Double Allocation

Controls:

* Transactions
* row locking or conditional updates
* constraints
* idempotency
* concurrency tests

### 102.4 Silent Rounding

Controls:

* Excess-precision rejection
* explicit rounding policy
* tests
* no float parsing

### 102.5 Balance Drift

Controls:

* Source records authoritative
* transactional projections
* reconciliation jobs
* rebuild capability
* integrity tests

### 102.6 Cross-Currency Error

Controls:

* Currency-aware value object
* database currency fields
* operation validation
* report grouping

### 102.7 AI Financial Overreach

Controls:

* Deterministic finance services
* no AI write authority
* human authorization
* audit
* source-grounded summaries

## 103. Acceptance Criterion — Exact Parsing

```text
Given:
A user enters 1250.50 NGN.

When:
The server parses the input.

Then:
The authoritative amount is 125050 minor units.

And:
No floating-point multiplication is used.
```

## 104. Acceptance Criterion — Currency Mismatch

```text
Given:
A payment is denominated in USD.

And:
A charge is denominated in NGN.

When:
An allocation is attempted.

Then:
The allocation fails with a currency-mismatch outcome.

And:
No implicit conversion occurs.
```

## 105. Acceptance Criterion — Partial Payment

```text
Given:
A charge is NGN 1,000,000.00.

And:
A verified payment is NGN 600,000.00.

When:
The payment is allocated fully to the charge.

Then:
The active allocation is NGN 600,000.00.

And:
The outstanding charge is NGN 400,000.00.

And:
The charge is not marked fully settled.
```

## 106. Acceptance Criterion — Overpayment

```text
Given:
A verified payment is NGN 1,200,000.00.

And:
The target charge is NGN 1,000,000.00.

When:
NGN 1,000,000.00 is allocated.

Then:
NGN 200,000.00 remains explicitly unallocated.

And:
The remaining amount is not silently treated as revenue or lost.
```

## 107. Acceptance Criterion — Double Allocation

```text
Given:
Two requests concurrently attempt to allocate the same available payment amount.

When:
Both transactions execute.

Then:
At most one succeeds for the conflicting amount.

And:
The total active allocations do not exceed the verified available payment.
```

## 108. Acceptance Criterion — Reversal

```text
Given:
A finalized allocation was recorded incorrectly.

When:
An authorized correction is approved.

Then:
A reversal or correcting record is created.

And:
The original allocation remains in history.

And:
The financial effect is corrected without destructive deletion.
```

## 109. Acceptance Criterion — JSON

```text
Given:
A Money value contains 125050n NGN.

When:
It is serialized through an API.

Then:
The contract contains "125050" as a string.

And:
The client does not receive an unsafe floating-point amount as the authoritative value.
```

## 110. Acceptance Criterion — Unknown Fee

```text
Given:
The provider fee has not yet been reported.

When:
The payment record is created.

Then:
The fee remains unknown or pending.

And:
It is not stored as zero merely for convenience.
```

## 111. Acceptance Criterion — AI Boundary

```text
Given:
An AI assistant summarizes a resident account.

When:
The summary displays the outstanding balance.

Then:
The amount comes from the deterministic finance read model.

And:
The AI does not independently calculate or modify the balance.
```

## 112. Implementation Sequence

Recommended implementation order:

```text
1. Establish ISO currency-code type
2. establish approved currency metadata
3. implement Money value object with bigint
4. implement exact parsing from decimal strings
5. implement deterministic formatting
6. establish JSON DTO conversion
7. establish PostgreSQL bigint mapping
8. establish charge and payment value structures
9. establish allocation transaction
10. establish balance formulas
11. establish idempotency and concurrency controls
12. establish reversal and adjustment patterns
13. establish receipt values
14. establish provider normalization
15. establish reconciliation records
16. establish finance audit and outbox events
17. establish property-based and integration tests
18. keep AI outside authoritative arithmetic
```

## 113. Deferred Decisions

This ADR does not decide:

* Exact Money class or functional implementation
* exact currency metadata source
* exact rounding mode for every calculated workflow
* tax rules
* Nigerian statutory receipt requirements
* payment-provider fee policy
* exchange-rate provider
* multi-currency conversion workflow
* reporting currency
* commission model
* late-fee policy
* deposit accounting treatment
* escrow model
* general-ledger architecture
* accounting integration provider
* revenue-recognition policy
* chart of accounts
* fiscal-year configuration
* balance materialization implementation
* financial threshold-approval matrix

These require bounded implementation decisions or later ADRs.

## 114. Supersession Conditions

This ADR should be reviewed when:

* Habitacores introduces currencies that cannot be represented adequately through minor-unit integers.
* regulatory accounting requirements require a different authoritative representation.
* a general-ledger platform becomes authoritative.
* financial scale exceeds the approved technical or business bounds.
* cryptocurrency or tokenized assets become supported.
* complex instrument valuation requires another numeric model.
* multi-currency conversion becomes a core real-time capability.
* a payment provider requires materially different settlement semantics.

## 115. Final Decision Statement

Habitacores will represent authoritative monetary values as signed integer minor units stored in PostgreSQL `bigint` columns and processed in TypeScript using `bigint`.

Every authoritative monetary value will include an explicit ISO 4217 currency code. Currency scale will be resolved through approved currency metadata rather than hardcoded throughout the application.

User input will be parsed exactly from validated decimal strings. Excess precision will be rejected unless the relevant workflow defines an explicit deterministic rounding policy. JavaScript floating-point multiplication, division, addition, subtraction, or rounding will not be used for authoritative financial amounts.

Financial state will be modeled through explicit charges, payments, allocations, credits, adjustments, refunds, reversals, receipts, reconciliation records, and related audit events. Finalized financial records will not be destructively edited to conceal corrections.

Payment allocation, reversal, refund, receipt, and balance-affecting operations will execute through authorized deterministic finance services and explicit PostgreSQL transactions with idempotency and concurrency protection.

Balances will be derived from authoritative source records. Any stored projection must remain transactionally consistent, reconcilable, and rebuildable.

Cross-currency arithmetic and allocation are prohibited without an approved currency-conversion record. Foreign-exchange rates, percentage rates, tax calculations, fee calculations, and prorated calculations will use exact representations and explicit rounding policies before conversion to minor units.

AI may summarize, explain, classify, or flag financial records for human review, but it will not calculate authoritative balances, verify payments, allocate money, approve or execute refunds, issue receipts, establish exchange rates, modify financial records, or satisfy agreement or occupancy financial prerequisites.

No browser, React component, Server Action, Route Handler, database query, payment provider, spreadsheet, report, cache, worker, AI model, coding agent, imported record, decimal library, formatting function, or human-readable document may silently redefine authoritative Habitacores amounts, currencies, balances, allocations, refunds, or financial history.