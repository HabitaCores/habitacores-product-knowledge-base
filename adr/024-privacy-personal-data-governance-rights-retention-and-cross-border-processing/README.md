# ADR-024 — Privacy, Personal-Data Governance, Data-Subject Rights, Retention, and Cross-Border Processing Strategy

## Decision Record

| Field | Value |
|---|---|
| Product | Habitacores |
| ADR | ADR-024 |
| Version | 0.1 |
| Status | Proposed |
| Decision Type | Foundational Privacy, Data-Governance, Compliance, and Lifecycle Architecture |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Market | Nigeria |
| Primary Legal Direction | Nigeria Data Protection Act 2023 and applicable NDPC directives |
| Privacy Model | Privacy by design and by default |
| Processing Inventory | Mandatory |
| Records of Processing Activities | Mandatory |
| Purpose and Lawful Basis | Required before production processing |
| Privacy Notices | Versioned, purpose-specific, and attributable |
| Consent | Used only where appropriate; not the universal lawful basis |
| Data Minimization | Mandatory |
| High-Risk Processing Assessment | Mandatory before applicable processing begins |
| Data-Subject Rights | Authenticated and auditable workflows |
| Retention | Purpose, law, contract, dispute, and evidence based |
| Legal Holds | Override ordinary deletion |
| Processor Governance | Contractual and operational due diligence required |
| Cross-Border Processing | Transfer assessment and approved safeguards required |
| Production Data in Development | Prohibited by default |
| AI Processing | Subordinate to ADR-020 and this ADR |
| Privacy Breach Response | Coordinated with ADR-023 |
| Privacy Authority of AI | Prohibited |
| Last Updated | 2 August 2026 |

## 1. Context

Habitacores will process personal data relating to:

- Rental applicants
- tenants and residents
- landlords and property owners
- agents and property managers
- guarantors
- witnesses
- organization administrators
- employees and contractors
- service providers
- support contacts
- website and mobile users

Potential personal-data classes include:

- Names
- contact information
- account and authentication information
- identity-verification information
- property and tenancy relationships
- rental applications
- agreements
- signatures and execution evidence
- payment and refund records
- bank-transfer evidence
- communications
- support cases
- maintenance records
- device and network metadata
- audit records
- behavioral and product-usage data
- uploaded documents
- AI inputs and outputs

These records may affect housing access, contractual obligations, payments,
legal rights, personal safety, reputation, and financial interests.

Habitacores therefore requires an explicit privacy architecture rather than
isolated privacy-policy text.

## 2. Decision Drivers

The strategy must:

1. Establish accountability for personal-data processing.
2. identify what personal data exists and why it is processed.
3. define controller and processor roles.
4. establish lawful, fair, and transparent processing.
5. minimize unnecessary collection.
6. provide meaningful privacy notices.
7. support data-subject rights.
8. govern consent where used.
9. assess high-risk processing before launch.
10. prevent uncontrolled secondary use.
11. define retention and deletion.
12. preserve legal and evidentiary records appropriately.
13. govern processors and subprocessors.
14. assess international transfers.
15. protect production data outside production.
16. coordinate privacy and security incidents.
17. govern AI use of personal data.
18. preserve verifiable privacy evidence.
19. support regulatory and legal review.
20. avoid unsupported compliance claims.

## 3. Decision

Habitacores will implement privacy by design and by default throughout:

```text
Product proposal
→ Processing-purpose definition
→ Personal-data inventory
→ Controller and processor role determination
→ Lawful-basis assessment
→ Data minimization
→ Privacy-risk screening
→ DPIA where required
→ Notice and consent design
→ Technical and organizational controls
→ Implementation and verification
→ Production approval
→ Rights handling
→ Retention and deletion
→ Monitoring and periodic review
````

No material personal-data workflow should enter production without a documented
purpose, data classification, role determination, lawful-basis assessment,
notice position, retention treatment, and security controls.

## 4. Core Privacy Concepts

Habitacores will distinguish:

```text
DATA_SUBJECT
PERSONAL_DATA
SENSITIVE_PERSONAL_DATA
PROCESSING_PURPOSE
LAWFUL_BASIS
DATA_CONTROLLER
JOINT_CONTROLLER
DATA_PROCESSOR
SUBPROCESSOR
CONSENT_RECORD
PRIVACY_NOTICE_VERSION
PROCESSING_ACTIVITY
DATA_PROTECTION_IMPACT_ASSESSMENT
DATA_SUBJECT_REQUEST
RETENTION_RULE
LEGAL_HOLD
INTERNATIONAL_TRANSFER
PRIVACY_INCIDENT
```

These concepts must not be collapsed into one privacy-policy document.

## 5. Accountability

Habitacores must be able to demonstrate:

* What personal data it processes
* whose data is processed
* why it is processed
* the lawful basis
* where it is stored
* who may access it
* which providers receive it
* whether it crosses borders
* how long it is retained
* how rights are fulfilled
* how risks are mitigated
* who approved the processing

Privacy compliance must be supported by system behavior and records, not only
public-facing statements.

## 6. Processing Inventory

Habitacores will maintain a current inventory of processing activities.

Each activity should identify:

* Stable processing-activity ID
* business purpose
* product workflow
* data subjects
* personal-data categories
* sensitive-data categories
* collection source
* controller or processor role
* lawful basis
* recipients
* subprocessors
* systems and storage locations
* international transfers
* retention rule
* data-subject rights
* automated or AI involvement
* security classification
* owner
* approval state
* review date

## 7. Record of Processing Activities

The processing inventory will support a formal Record of Processing Activities.

The record must be updated when:

* A new data class is collected.
* a new purpose is introduced.
* a new provider receives personal data.
* data moves to a new jurisdiction.
* retention changes.
* AI processing changes.
* a new user category is introduced.
* a material workflow is redesigned.
* controller or processor roles change.

## 8. Controller and Processor Roles

Habitacores must determine its legal role for each processing activity.

Potential roles include:

```text
INDEPENDENT_CONTROLLER
JOINT_CONTROLLER
PROCESSOR
SUBPROCESSOR
```

The role must be determined from actual decision-making and processing
relationships, not only contractual labels.

## 9. Organization Relationships

An organization using Habitacores may determine some purposes and means of
processing relating to its properties, applicants, tenants, staff, or agents.

Habitacores may independently determine other purposes, including:

* Platform security
* authentication
* fraud prevention
* service operation
* billing
* audit
* legal compliance
* product reliability

The allocation of responsibilities must be documented in applicable customer,
processor, and privacy terms.

## 10. Processing Purpose

Every collection and use must have a defined, specific purpose.

Examples may include:

* Account creation
* organization membership
* rental application processing
* agreement generation and execution
* payment collection and reconciliation
* occupancy administration
* document management
* maintenance coordination
* user support
* security monitoring
* regulatory compliance
* product reliability

Generic purposes such as “business improvement” or “other useful purposes”
are insufficient for unrestricted processing.

## 11. Purpose Limitation

Personal data collected for one purpose must not be reused for an incompatible
purpose without appropriate assessment and authority.

Examples:

* Payment evidence must not become marketing data.
* identity documents must not become general AI-training data.
* support messages must not become tenant scoring data.
* agreement contents must not become public analytics.
* maintenance records must not become unrelated profiling data.

## 12. Lawful Basis

Every processing activity must identify an applicable lawful basis before
production use.

Potential lawful bases may include:

* Consent
* performance of a contract or pre-contractual steps
* compliance with legal obligations
* protection of vital interests
* performance of a public-interest task where applicable
* legitimate interests subject to assessment

The exact basis requires qualified legal and privacy review.

## 13. Consent Is Not Universal

Habitacores will not use consent as a default answer for every processing
activity.

Consent may be inappropriate where:

* Processing is necessary to perform an agreement.
* processing is required by law.
* the individual cannot meaningfully refuse.
* withdrawing consent cannot realistically stop the core processing.
* another lawful basis accurately describes the activity.

## 14. Consent Requirements

Where consent is used, it must be:

* Freely given
* specific
* informed
* unambiguous
* attributable
* purpose bound
* distinguishable from unrelated terms
* withdrawable through a practical process

Preselected consent and bundled unnecessary consent are prohibited.

## 15. Consent Records

A consent record should preserve:

* Data subject
* purpose
* notice version
* consent text version
* affirmative action
* collection channel
* timestamp
* relevant account or session
* withdrawal status
* withdrawal time
* resulting processing consequences

A generic `consented = true` field is insufficient.

## 16. Withdrawal

Withdrawing consent must be as practicable as giving it.

Withdrawal must:

* Stop future processing based solely on that consent.
* preserve processing already lawful before withdrawal.
* preserve records required by law, contract, security, dispute, or legal hold.
* avoid falsely deleting records that must remain.
* clearly explain resulting service limitations.

## 17. Privacy Notices

Habitacores will provide clear notices appropriate to the workflow and audience.

Notices should explain:

* Who is responsible for processing
* contact and privacy channels
* data collected
* purposes
* lawful bases
* recipients
* providers
* transfers
* retention direction
* rights
* complaint mechanisms
* consequences of required-data refusal
* automated or AI involvement where material
* notice effective date and version

## 18. Layered Notices

Habitacores may use:

* A general platform privacy notice
* account-creation notice
* application notice
* tenant or agreement notice
* payment notice
* document-upload notice
* cookie or analytics notice
* AI-processing notice
* employee or contractor notice

A general privacy policy must not conceal material workflow-specific processing.

## 19. Notice Versioning

Published notice versions are immutable.

Habitacores must preserve:

* Notice ID
* version
* effective date
* language
* applicable audience
* applicable processing activities
* publication channel
* approval
* retirement date

Historical processing should remain attributable to the notice effective at the
relevant time.

## 20. Data Minimization

Habitacores will collect and expose only data reasonably necessary for the
approved purpose.

Every proposed field should be assessed for:

* Necessity
* proportionality
* sensitivity
* optionality
* access scope
* retention
* alternatives
* harm if exposed
* provider disclosure

“May be useful later” is not sufficient justification.

## 21. Required and Optional Fields

Interfaces must distinguish:

* Required information
* optional information
* conditionally required information
* prohibited information

Optional information must not be represented as required through deceptive
design.

## 22. Sensitive Personal Data

Sensitive personal data requires elevated controls.

Processing must identify:

* Necessity
* applicable lawful condition
* restricted access
* explicit purpose
* provider handling
* retention
* DPIA requirement
* logging restrictions
* rights impact
* breach impact

Habitacores should avoid collecting sensitive data that does not have a
demonstrated product or legal necessity.

## 23. Identity Documents

Government-issued identification must not be requested as a routine default
for every Habitacores workflow.

Where identity documents are required, the workflow must define:

* Legal or operational necessity
* permitted document types
* fields required
* redaction opportunities
* verification process
* reviewer authorization
* provider involvement
* retention
* deletion
* access logging
* fraud and impersonation controls

## 24. Children and Minors

Habitacores may process information relating to children who reside at a
property or appear in tenancy-related records.

Before collecting information directly from children or creating child-facing
accounts, Habitacores requires a separate legal, privacy, safety, identity,
consent, and product assessment.

The platform should minimize information about minor occupants and avoid
unnecessary profiling.

## 25. High-Risk Processing Screening

Every material processing activity must undergo privacy-risk screening.

Potential high-risk indicators include:

* Large-scale processing
* sensitive personal data
* housing eligibility effects
* systematic monitoring
* automated evaluation or profiling
* identity verification
* behavioral tracking
* combining unrelated datasets
* novel technology
* vulnerable data subjects
* cross-border disclosure
* AI-supported consequential decisions
* high-impact security failure

## 26. Data Protection Impact Assessment

A DPIA must be completed before processing begins where the proposed activity
is likely to create high risk to individuals’ rights and freedoms.

The DPIA should include:

* Processing description
* purpose
* necessity
* proportionality
* data flows
* data subjects
* risks
* likelihood and impact
* security controls
* privacy controls
* residual risk
* alternatives
* consultation
* ownership
* approval
* review triggers

The NDPA expressly contemplates DPIAs for high-risk processing. ([TNP][2])

## 27. DPIA Outcomes

Directional outcomes:

```text
APPROVED
APPROVED_WITH_CONTROLS
REQUIRES_REDESIGN
REQUIRES_LEGAL_REVIEW
REQUIRES_REGULATORY_CONSULTATION
REJECTED
SUSPENDED
```

A DPIA must be capable of stopping or redesigning a workflow.

## 28. Data-Subject Rights

Habitacores will create authenticated workflows for applicable rights,
including as legally required:

* Access
* information about processing
* correction
* deletion
* restriction
* objection
* withdrawal of consent
* portability where applicable
* complaint
* review of applicable automated processing

The exact scope of each right depends on law, role, lawful basis, exemptions,
legal obligations, and the processing context.

## 29. Rights Request Record

Each request should preserve:

* Request ID
* data subject
* request type
* intake channel
* identity-verification status
* scope
* controller or organization relationship
* systems searched
* exemptions considered
* decision
* response
* completion time
* responsible reviewer
* audit events

## 30. Identity Verification for Rights Requests

Habitacores must verify requesters proportionately.

Verification must not:

* Demand excessive identity data.
* disclose whether an unrelated person has an account.
* expose another organization’s records.
* become more burdensome than necessary.
* permit support staff to bypass access controls.

## 31. Access Requests

An access response may require retrieval from:

* User and identity records
* memberships
* applications
* tenancy and occupancy records
* payment records
* agreements
* documents
* communications
* support records
* consent records
* AI-operation records where applicable
* provider records where Habitacores remains responsible

The response must not reveal another person’s protected information without a
lawful basis.

## 32. Correction Requests

Correction must distinguish:

* Incorrect current information
* disputed information
* immutable historical records
* financial corrections
* agreement corrections
* audit records
* provider records

Immutable financial, agreement, and audit history must not be silently edited.
Corrections must use the applicable amendment, reversal, annotation, or linked
correction workflow.

## 33. Deletion Requests

A deletion request does not automatically authorize immediate deletion of all
associated records.

Habitacores must assess:

* Identity and authority
* applicable processing role
* lawful basis
* contractual necessity
* legal obligation
* dispute or legal claim
* financial retention
* agreement evidence
* security records
* fraud prevention
* legal hold
* third-party processor deletion
* backup treatment

## 34. Restriction and Objection

Where applicable, Habitacores should be able to:

* Mark processing as restricted.
* prevent optional downstream use.
* suspend marketing or analytics.
* preserve records without ordinary use.
* record an objection and decision.
* communicate the outcome.

Restriction must not be represented as deletion.

## 35. Portability and Export

Where portability applies, exports should be:

* Authenticated
* scoped
* machine readable where required
* encrypted or securely delivered
* time limited
* auditable
* protected against cross-tenant inclusion
* free from internal secrets and unrelated records

## 36. Request Timelines

Request deadlines must be configured from current legal requirements and
qualified privacy guidance.

The system must support:

* Intake timestamp
* due date
* extension where legally allowed
* reason for extension
* reminders
* escalation
* response evidence

Exact deadlines are deferred to the privacy implementation register rather
than hardcoded casually throughout feature code.

## 37. Retention

Every personal-data class must have an approved retention rule.

Retention must consider:

* Processing purpose
* contractual duration
* tenancy duration
* financial obligations
* legal requirements
* limitation and dispute periods
* fraud and security needs
* audit
* legal holds
* provider limitations
* user expectations
* data minimization

“Retain forever” is prohibited as a default.

## 38. Retention Rule

A retention rule should identify:

* Record class
* triggering event
* active period
* archival period
* deletion or anonymization action
* legal basis
* owner
* exceptions
* legal-hold behavior
* provider deletion behavior
* backup treatment
* evidence of execution

## 39. Retention Trigger

Retention may begin from events such as:

* Account closure
* application conclusion
* agreement termination
* occupancy end
* payment settlement
* dispute closure
* document supersession
* consent withdrawal
* provider-contract termination

Creation date alone may be the wrong trigger.

## 40. Deletion and Anonymization

Approved lifecycle actions may include:

```text
DELETE
CRYPTOGRAPHIC_ERASURE
ANONYMIZE
AGGREGATE
RESTRICT
ARCHIVE
PRESERVE_UNDER_LEGAL_HOLD
```

Pseudonymization is not necessarily anonymization.

Data must not be declared anonymous when reidentification remains reasonably
possible.

## 41. Deletion Orchestration

Deletion may involve:

* PostgreSQL records
* object storage
* search indexes
* caches
* analytics systems
* AI artifacts
* notification providers
* support systems
* identity providers
* payment and signature providers
* backups according to policy

Deletion must be idempotent, auditable, and resilient to partial provider
failure.

## 42. Deletion Evidence

Deletion evidence should record:

* Request or retention trigger
* affected record classes
* systems
* execution time
* result
* exceptions
* legal holds
* provider confirmations
* failures and retries
* final disposition

The evidence must not reproduce the deleted personal data unnecessarily.

## 43. Legal Holds

A legal hold suspends ordinary deletion for defined information.

A legal hold must identify:

* Authority
* reason
* scope
* custodian or subject
* record classes
* effective date
* reviewer
* review cadence
* release authority

Legal holds must remain restricted and auditable.

## 44. Backups

Expired or deleted records may remain in protected backups until backup
rotation, where consistent with the approved legal and technical policy.

Restored data must not silently reactivate records that were already deleted,
restricted, or subject to a later lifecycle event.

ADR-021 remains authoritative for backup restoration and reconciliation.

## 45. Processor Governance

Before a provider processes personal data, Habitacores must assess:

* Service purpose
* data categories
* processing locations
* retention
* security
* subprocessors
* breach process
* deletion and return
* audit rights
* confidentiality
* international transfers
* model-training or secondary use
* continuity and exit
* contractual terms

## 46. Processor Agreements

Applicable processor terms should address:

* Documented instructions
* confidentiality
* security
* subprocessing
* rights assistance
* breach cooperation
* DPIA assistance
* deletion or return
* audit evidence
* international transfers
* termination
* liability and responsibility

The exact contractual terms require legal review.

## 47. Subprocessor Register

Habitacores will maintain a register containing:

* Provider
* service
* data categories
* purpose
* processing location
* transfer mechanism
* effective date
* approval
* contract
* subprocessors
* exit procedure
* current status

Material changes require review and, where applicable, notice or approval.

## 48. Cross-Border Processing

Personal data must not be transferred or made remotely accessible across
borders without assessment.

The assessment should determine:

* Origin
* destination
* provider
* purpose
* data classes
* legal transfer basis
* destination safeguards
* contractual controls
* access risks
* onward transfers
* government-access considerations
* data-subject information
* retention
* remediation and suspension

## 49. Data Location

Data residency and data processing are not identical.

A database located in one region may still involve:

* Remote support access
* global subprocessors
* telemetry exports
* content-delivery networks
* AI processing
* backup storage
* provider administration
* cross-border incident response

The full processing chain must be assessed.

## 50. Production Data Outside Production

Production personal data must not be copied into:

* Local developer devices
* ordinary preview deployments
* public issue trackers
* coding-agent prompts
* test fixtures
* training demonstrations
* analytics notebooks
* unrestricted support channels

unless a documented exceptional process authorizes it.

## 51. Test and Development Data

Non-production environments should use:

* Synthetic records
* generated documents
* synthetic identities
* provider sandbox records
* masked or irreversibly transformed datasets where approved

Masking must be validated against reidentification and relationship leakage.

## 52. Analytics

Analytics processing must define:

* Purpose
* events collected
* identifiers
* retention
* provider
* lawful basis
* notice
* consent requirements where applicable
* cross-border processing
* user controls
* internal access
* deletion treatment

Session replay or highly granular behavioral tracking requires elevated review.

## 53. Cookies and Similar Technologies

Web and mobile tracking technologies must be inventoried.

They may not be introduced solely because a provider SDK enables them.

The implementation must determine:

* Necessity
* purpose
* storage duration
* recipient
* consent or other applicable basis
* notice
* withdrawal
* regional behavior

## 54. Marketing

Marketing processing must remain distinct from:

* Account operation
* agreement administration
* payment communication
* security messages
* legally necessary notices

Users must not be forced to accept optional marketing to access core tenancy
functions.

## 55. AI Processing

ADR-020 remains authoritative for AI architecture.

Every AI use case involving personal data must additionally define:

* Privacy purpose
* lawful basis
* data classes
* minimization
* provider role
* training and retention position
* international processing
* DPIA status
* transparency
* rights implications
* human review
* deletion and retention

## 56. No General AI Training Right

Habitacores will not treat collection of personal data as permission to train
general models.

Using personal data for:

* Provider model training
* Habitacores model training
* fine-tuning
* broad evaluation
* unrelated prompt datasets

requires separate approval, lawful-basis assessment, minimization, privacy
review, and notice.

## 57. Automated Decisions and Profiling

Habitacores will not initially deploy solely automated decisions that approve,
reject, rank, or materially disadvantage rental applicants or tenants.

Any future proposal requires a separate ADR addressing:

* Legal basis
* necessity
* fairness
* discrimination
* explainability
* contestability
* human review
* DPIA
* validation
* monitoring
* appeal

## 58. Privacy and Security Incidents

ADR-023 governs incident command and security response.

Privacy assessment must additionally determine:

* Personal data involved
* affected data subjects
* likely consequences
* controller and processor responsibilities
* regulator-notification requirements
* data-subject-notification requirements
* contractual notifications
* timing
* documentation
* remediation

The NDPC provides a breach-reporting channel, and current Nigerian guidance
reflects a 72-hour notification framework for applicable personal-data
breaches. Exact reportability must be assessed from the actual incident and
current legal requirements. ([Multilaw][3])

## 59. Privacy Incident Record

The incident record should preserve:

* Discovery and awareness times
* affected systems
* data classes
* approximate subjects affected
* encryption and safeguards
* likely consequences
* containment
* processor communications
* legal assessment
* notification decisions
* notification times
* delay explanations
* remediation
* final review

## 60. Privacy Governance Roles

Directional roles include:

```text
PRIVACY_OWNER
DATA_PROTECTION_LEAD
DATA_OWNER
PROCESSING_ACTIVITY_OWNER
SECURITY_LEAD
LEGAL_REVIEWER
RIGHTS_REQUEST_REVIEWER
PROCESSOR_MANAGER
```

Whether Habitacores must formally appoint a Data Protection Officer or register
under a particular regulatory classification remains subject to qualified
assessment.

## 61. Access Governance

Privacy-sensitive access must follow least privilege.

Particular scrutiny applies to:

* Identity documents
* payment evidence
* agreements
* support cases
* data exports
* rights requests
* legal holds
* production analytics
* AI inputs and outputs
* backup data

Privileged access must remain attributable and auditable.

## 62. Privacy Audit Events

Potential events include:

```text
PROCESSING_ACTIVITY_APPROVED
PRIVACY_NOTICE_PUBLISHED
CONSENT_RECORDED
CONSENT_WITHDRAWN
DPIA_COMPLETED
DATA_SUBJECT_REQUEST_RECEIVED
DATA_SUBJECT_REQUEST_COMPLETED
RETENTION_RULE_CHANGED
RETENTION_ACTION_EXECUTED
LEGAL_HOLD_APPLIED
LEGAL_HOLD_RELEASED
PROCESSOR_APPROVED
SUBPROCESSOR_CHANGED
INTERNATIONAL_TRANSFER_APPROVED
PRIVACY_INCIDENT_ASSESSED
REGULATORY_NOTIFICATION_SUBMITTED
```

Audit metadata must avoid unnecessary reproduction of sensitive personal data.

## 63. Observability

Privacy metrics may include:

```text
processing_activities_pending_review
dpias_due
data_subject_requests
rights_request_age
rights_request_deadline_risk
retention_actions
retention_failures
deletion_provider_failures
legal_holds_active
unapproved_processors
international_transfer_reviews_due
privacy_incidents
```

Personal identities and request contents must not become ordinary metric labels.

## 64. Alerts

Priority alerts may include:

* Processing activity without an approved purpose
* high-risk processing started without DPIA
* unapproved provider receiving personal data
* rights request approaching deadline
* failed deletion workflow
* retention rule missing
* production data detected in preview
* legal-hold data selected for deletion
* unauthorized data export
* cross-border destination changed
* AI provider receiving prohibited data
* privacy breach assessment not initiated

## 65. Testing

Required tests include:

* Collection minimization
* required-versus-optional field behavior
* notice-version attribution
* consent and withdrawal
* rights-request authentication
* cross-tenant export isolation
* correction without historical destruction
* deletion orchestration
* legal-hold precedence
* retention execution
* provider deletion failure
* backup-restoration lifecycle reconciliation
* non-production-data controls
* AI-data minimization
* privacy-incident escalation

## 66. Consent Withdrawal Test

```text
Given:
A user consented to optional product analytics.

When:
The user withdraws that consent.

Then:
Future optional analytics based solely on that consent stops.

And:
Core tenancy, security, agreement, and payment processing governed by other
valid bases is not falsely deleted.
```

## 67. Cross-Tenant Access Export Test

```text
Given:
An Organization A user submits an access request.

When:
Habitacores assembles the authorized response.

Then:
Organization B records are excluded.

And:
Application authorization and PostgreSQL RLS remain effective throughout
the export process.
```

## 68. Legal-Hold Test

```text
Given:
A tenancy agreement is within a valid legal hold.

When:
Its ordinary retention period expires.

Then:
The agreement and associated evidence remain preserved.

And:
The attempted lifecycle action is recorded without deleting the held data.
```

## 69. Deletion-Restoration Test

```text
Given:
A personal-data record was validly deleted after a backup was created.

When:
That backup is later restored during recovery.

Then:
Lifecycle reconciliation identifies the later deletion.

And:
The deleted record is not silently returned to ordinary production use.
```

## 70. Production-Data Isolation Test

```text
Given:
A pull-request preview starts.

When:
Its test data is initialized.

Then:
It uses synthetic or approved non-production data.

And:
It cannot query or copy production tenant records.
```

## 71. AI-Minimization Test

```text
Given:
AI is requested to summarize one authorized agreement clause.

When:
Context is assembled.

Then:
Only the required clause and approved metadata are sent.

And:
Unrelated identity documents, payment records, and other tenant agreements
are excluded.
```

## 72. Codex Boundaries

Every Codex privacy-related work package must specify:

* Processing activity
* purpose
* data subjects
* personal-data categories
* controller or processor role
* lawful-basis direction
* notice
* consent where applicable
* minimization
* access
* providers
* transfer
* retention
* rights
* DPIA status
* audit
* tests

Codex must not:

* Invent a lawful basis
* declare legal compliance
* use consent universally
* add personal-data fields without purpose
* copy production data into test fixtures
* weaken rights-request identity checks
* delete financial or agreement history improperly
* bypass legal holds
* introduce a processor without review
* send private records to AI providers casually
* create automated housing decisions without a separate ADR
* determine regulatory breach notification independently

## 73. Prohibited Patterns

The following are not approved:

* Privacy policy as the only privacy control
* collecting data without a documented purpose
* consent bundled into unrelated mandatory terms
* preselected optional consent
* identity documents collected by default
* indefinite retention without justification
* silent secondary use
* production records in preview environments
* unrestricted support downloads
* deletion that bypasses legal holds
* rights exports containing another tenant’s data
* provider use without processor review
* cross-border processing without assessment
* AI training on personal data by default
* automated rental rejection without a separate decision
* unsupported claims of NDPA compliance

## 74. Options Considered

### Option A — Privacy Policy Only

Advantages:

* Low implementation effort
* visible public document

Disadvantages:

* No processing inventory
* no rights workflow
* no retention enforcement
* no processor governance
* no operational accountability

Decision:

Rejected.

### Option B — Security Controls as Privacy Compliance

Advantages:

* Reuses security investment
* protects confidentiality and integrity

Disadvantages:

* Does not establish purpose or lawful basis
* does not provide transparency or rights
* does not control retention or secondary use
* does not govern international transfers

Decision:

Rejected.

### Option C — Consent for All Processing

Advantages:

* Superficially simple
* one common interface

Disadvantages:

* Consent may be invalid or inappropriate
* withdrawal conflicts with essential processing
* creates misleading user expectations
* obscures actual contractual or legal bases

Decision:

Rejected.

### Option D — Integrated Privacy-by-Design Architecture

Advantages:

* Processing becomes attributable and reviewable
* purposes and lawful bases become explicit
* rights become operational
* retention becomes enforceable
* providers and transfers become governed
* high-risk processing is assessed before launch
* AI remains constrained
* privacy evidence becomes demonstrable

Disadvantages:

* Requires legal and privacy expertise
* processing inventory requires maintenance
* deletion across providers is complex
* rights requests require operational capacity
* final retention periods require legal assessment

Decision:

Approved.

## 75. Positive Consequences

* Habitacores knows what personal data it processes and why.
* unnecessary collection is reduced.
* privacy notices reflect actual workflows.
* consent is used accurately.
* high-risk processing receives advance assessment.
* rights requests become controlled and auditable.
* retention and deletion become systematic.
* providers and transfers receive governance.
* AI use remains purpose limited.
* privacy incidents coordinate with security response.

## 76. Negative Consequences

* Product development gains privacy-review requirements.
* rights and deletion orchestration add engineering work.
* processor contracts require review.
* cross-border assessments add operational complexity.
* data inventories require continuous maintenance.
* legal advice is required for unresolved obligations.
* retention and evidentiary needs may conflict and require careful resolution.

## 77. Risks

### 77.1 Privacy Governance Becomes Documentation Only

Controls:

* Linked processing registry
* technical tests
* retention jobs
* rights workflows
* provider controls
* audit evidence

### 77.2 Excessive Data Collection

Controls:

* Field-level purpose
* minimization review
* optional-field controls
* DPIA
* periodic inventory review

### 77.3 Improper Deletion

Controls:

* Retention rules
* legal holds
* financial and agreement exceptions
* idempotent orchestration
* recovery reconciliation

### 77.4 Unlawful Secondary Use

Controls:

* Purpose registry
* compatibility assessment
* notice review
* provider restrictions
* AI-use-case governance

### 77.5 Cross-Border Exposure

Controls:

* Provider register
* transfer assessment
* contractual safeguards
* location inventory
* suspension capability

### 77.6 Rights Request Data Leakage

Controls:

* Identity verification
* authorization
* RLS
* reviewer controls
* export validation
* secure delivery

## 78. Implementation Sequence

```text
1. Appoint interim privacy ownership and qualified advisers
2. inventory all planned processing activities and data classes
3. establish the Record of Processing Activities
4. determine controller and processor roles
5. document purposes and lawful-basis assessments
6. create the privacy-notice and consent registries
7. identify high-risk processing and complete initial DPIAs
8. establish provider and subprocessor due diligence
9. assess international data flows
10. define the retention-schedule framework
11. implement legal-hold precedence
12. implement data-subject rights intake and case management
13. implement authenticated access and export workflows
14. implement correction, restriction, objection, and deletion workflows
15. implement provider deletion and reconciliation
16. enforce synthetic-data use in non-production
17. integrate AI privacy requirements with ADR-020
18. integrate privacy incidents with ADR-023
19. implement privacy audit, metrics, and alerts
20. conduct legal and privacy readiness review before production
```

## 79. Deferred Decisions

This ADR does not decide:

* Final regulatory classification of Habitacores
* whether Habitacores is a data controller of major importance
* formal DPO appointment requirements
* specific DPO or privacy adviser
* final lawful basis for every workflow
* exact retention periods
* exact rights-request deadlines
* exact international-transfer mechanisms
* exact hosting region
* exact cookie-consent platform
* exact identity-verification retention
* exact analytics provider
* exact employee privacy program
* exact child-data workflow
* exact regulatory filing obligations
* exact privacy-audit provider

These require bounded legal, privacy, operational, and implementation review.

## 80. Supersession Conditions

This ADR should be reviewed when:

* Habitacores enters production.
* another country becomes an operational market.
* automated tenant scoring is proposed.
* child-facing accounts are proposed.
* biometric verification is introduced.
* a new sensitive-data category is collected.
* material AI training or fine-tuning is proposed.
* major processors or hosting regions change.
* Nigerian privacy requirements materially change.
* a significant privacy incident occurs.

## 81. Final Decision Statement

Habitacores will implement privacy by design and by default across product,
engineering, operations, providers, analytics, AI, support, security, and data
lifecycle management.

Every material processing activity will have an identified purpose, data
subjects, personal-data categories, controller or processor role, lawful-basis
assessment, notice position, consent position where applicable, recipients,
processors, transfers, retention treatment, rights treatment, security
classification, owner, and review status.

Habitacores will maintain a current processing inventory and Record of
Processing Activities. Product documentation and privacy notices will remain
aligned with actual system behavior.

Habitacores will collect only data reasonably necessary for approved purposes.
Optional data will be distinguished from required data, and sensitive
personal data will receive heightened necessity, access, retention, provider,
and risk review.

Consent will be used only where it is an appropriate lawful basis. Where used,
it will be specific, informed, affirmative, attributable, purpose bound,
versioned, and practically withdrawable.

Processing likely to create high risk to individuals will undergo a Data
Protection Impact Assessment before production activation. A DPIA may require
additional controls, redesign, legal review, consultation, suspension, or
rejection.

Habitacores will provide authenticated, tenant-isolated, auditable workflows
for applicable access, correction, deletion, restriction, objection,
withdrawal, portability, complaint, and automated-processing rights.

Deletion will not silently destroy immutable financial, agreement, audit,
security, or evidentiary history. Applicable records will use correction,
reversal, amendment, annotation, restriction, retention, or legal-hold
workflows.

Every record class will receive a retention rule based on purpose, contract,
law, dispute, security, audit, and evidentiary needs. Indefinite retention will
not be the default.

Legal holds will override ordinary deletion. Backup restoration will reconcile
later deletion, restriction, and legal-hold events before restored records
return to ordinary production use.

Processors and subprocessors will undergo privacy, security, retention,
deletion, transfer, breach, secondary-use, continuity, and contractual review.
Provider marketing statements alone will not establish approval.

International processing and remote access will be assessed across the full
provider and subprocessor chain. Data residency claims will not replace
analysis of support access, telemetry, backups, AI processing, and onward
transfers.

Production personal data will not be used routinely in local development,
preview deployments, coding-agent prompts, demonstrations, or test fixtures.
Synthetic or appropriately transformed information will be used instead.

AI processing will remain subordinate to this ADR and ADR-020. Personal data
will not be treated as available for general model training, fine-tuning, or
unrelated evaluation merely because Habitacores collected it.

Habitacores will not initially use solely automated AI systems to approve,
reject, rank, or materially disadvantage applicants or tenants. Any such
proposal requires a separate legal, privacy, fairness, explainability,
contestability, and human-governance decision.

Privacy incidents will be coordinated through ADR-023. Habitacores will
evaluate actual incident facts, affected data, likely consequences, controller
and processor responsibilities, regulator notification, data-subject
notification, contractual obligations, timing, and remediation.

AI may assist with organization, summarization, and workflow support, but it
will not determine lawful basis, approve a DPIA, resolve rights requests,
authorize deletion, release a legal hold, approve international transfers,
determine reportability, submit regulatory notifications, or declare privacy
compliance.

No developer, organization, provider, administrator, support operator, coding
agent, analytics system, or AI model may bypass Habitacores purpose
limitation, lawful-basis review, consent controls, minimization, tenant
isolation, rights workflows, retention, legal holds, processor governance,
transfer assessment, incident authority, or privacy audit controls.