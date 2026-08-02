# ADR-020 — AI Provider, Gateway, Model, Prompt, Retrieval, and Evaluation Architecture

## Decision Record

| Field | Value |
|---|---|
| Product | Habitacores |
| ADR | ADR-020 |
| Version | 0.1 |
| Status | Proposed |
| Decision Type | Foundational Responsible AI, Security, Data, and Integration Architecture |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| AI Architecture | Central provider-neutral AI Gateway |
| Production AI Access | Gateway only |
| Initial Provider | Deferred pending due diligence |
| Provider SDK Exposure | Gateway adapters only |
| Use-Case Authority | Habitacores AI use-case registry |
| Model Authority | Habitacores model-configuration registry |
| Prompt Authority | Versioned prompt registry |
| Retrieval Model | Permission-aware, purpose-limited, and source-attributed |
| Structured Outputs | Schema validated before application use |
| Raw AI Output Authority | None |
| Human Review | Required for materially consequential outputs |
| Direct Database Access by Models | Prohibited |
| Direct Object-Storage Access by Models | Prohibited |
| Direct Provider Access from Web or Mobile | Prohibited |
| AI Business Authority | Prohibited |
| Financial AI Authority | Prohibited |
| Legal AI Authority | Prohibited |
| Agreement and Occupancy Authority | Prohibited |
| Manual Fallback | Required for material workflows |
| Evaluation | Required before release and after material changes |
| Last Updated | 1 August 2026 |

## 1. Context

Habitacores may use artificial intelligence to assist with:

- Property-description drafting
- document summarization
- agreement-clause explanation
- support-response drafting
- maintenance-request classification
- document-field extraction
- application-document organization
- payment-evidence summarization
- reconciliation-discrepancy classification
- communication drafting
- search and knowledge retrieval
- internal operational analysis
- anomaly identification for human review

Habitacores will process sensitive information involving:

- Identities
- contact details
- rental applications
- tenancy records
- agreements
- financial records
- payment evidence
- property records
- maintenance information
- support cases
- organization records
- uploaded documents

AI integration therefore creates risks involving:

- Hallucination
- prompt injection
- cross-tenant disclosure
- excessive data disclosure
- unsupported legal or financial conclusions
- provider lock-in
- silent model changes
- non-deterministic output
- unbounded cost
- provider outages
- unreliable structured output
- inappropriate automation
- weak source attribution
- model or prompt regression
- misuse of production data for experimentation
- direct model access to privileged systems

## 2. Decision Drivers

The decision is driven by the need to:

1. Preserve AI-provider portability.
2. prevent unrestricted provider access.
3. register and approve every production use case.
4. keep models subordinate to domain authority.
5. preserve tenant isolation.
6. minimize sensitive data disclosure.
7. prevent models from receiving unrestricted database or storage access.
8. validate outputs before application use.
9. preserve source attribution and grounding.
10. support human review.
11. measure quality and safety.
12. version prompts, models, policies, and schemas.
13. manage model deprecation and provider outages.
14. control token usage and cost.
15. support web, mobile, APIs, and workers consistently.
16. support reproducible evaluation.
17. prevent silent AI-driven business transitions.
18. preserve auditability.
19. provide deterministic manual fallback.
20. establish implementation-ready engineering controls.

## 3. Decision

Habitacores will implement a centralized, provider-neutral AI Gateway.

All production AI operations will follow:

```text
Approved product workflow
→ Registered AI use case
→ Authentication and authorization
→ Organization and resource validation
→ Purpose and data-classification check
→ Permission-aware context retrieval
→ Minimization and redaction
→ Prompt and model-configuration resolution
→ AI Gateway
→ Provider adapter
→ Model response
→ Safety and schema validation
→ Grounding and business-rule validation
→ Human review where required
→ Approved application use
→ AI-operation record, audit, metrics, and cost
````

No web application, mobile application, worker, provider integration, feature module, coding agent, or browser client may bypass the AI Gateway for production AI operations.

## 4. Core Principle

AI is a supporting capability.

AI is not authoritative for:

* Authentication
* organization membership
* permissions
* tenant isolation
* payment verification
* payment allocation
* refund approval
* receipt issuance
* application approval or rejection
* agreement approval
* agreement execution
* agreement activation
* occupancy activation
* legal notice issuance
* financial reconciliation resolution
* deletion or retention decisions

## 5. AI Architecture Concepts

Habitacores will distinguish:

```text
AI_USE_CASE
AI_PROVIDER
AI_MODEL_CONFIGURATION
AI_PROMPT
AI_PROMPT_VERSION
AI_POLICY_VERSION
AI_RETRIEVAL_POLICY
AI_OPERATION
AI_INPUT_ARTIFACT
AI_OUTPUT_ARTIFACT
AI_VALIDATION_RESULT
AI_EVALUATION_SET
AI_EVALUATION_RUN
AI_HUMAN_DISPOSITION
AI_INCIDENT
```

These concepts must not be collapsed into one provider request log.

## 6. AI Gateway

The AI Gateway will provide the controlled boundary for:

* Provider selection
* model selection
* prompt resolution
* authentication
* authorization context
* data minimization
* redaction
* token budgeting
* timeout control
* retry policy
* structured-output enforcement
* safety validation
* source tracking
* observability
* cost tracking
* audit correlation
* provider fallback where approved

## 7. Provider-Neutral Contract

Application code should use Habitacores contracts.

Directional interface:

```typescript
interface AiGateway {
  execute<TInput, TOutput>(
    request: AiOperationRequest<TInput>,
    outputSchema: RuntimeSchema<TOutput>
  ): Promise<AiOperationResult<TOutput>>;
}
```

Application and domain packages must not depend directly on provider SDK types.

## 8. Provider Adapters

Provider-specific adapters may implement:

```text
OpenAI adapter
Anthropic adapter
Google adapter
Approved local-model adapter
Future approved provider adapter
```

Only providers that pass the approved due-diligence process may be enabled in production.

## 9. Initial Provider

The initial production provider remains deferred.

Evaluation must include:

* Model quality
* structured-output reliability
* context limits
* latency
* regional availability
* data-use terms
* retention behavior
* privacy and security commitments
* administrative access
* API-key controls
* model-version stability
* rate limits
* safety capabilities
* evaluation support
* incident history
* provider support
* pricing
* exit and portability

## 10. No Client-Side Provider Credentials

AI-provider credentials must not be placed in:

* Browsers
* mobile binaries
* public environment variables
* source control
* client logs
* analytics
* crash reports
* generated documents
* prompts
* outbox payloads

Provider requests must originate from trusted Habitacores server or worker infrastructure.

## 11. Use-Case Registry

Every production AI capability must have a registered use case.

A use-case record must include:

* Stable use-case ID
* name
* owner
* product purpose
* intended users
* permitted organizations
* input classification
* output classification
* allowed data sources
* prohibited data
* approved providers
* approved model configurations
* prompt versions
* output schema
* validation rules
* human-review requirements
* retention
* cost budget
* fallback
* evaluation suite
* approval state
* review date
* retirement condition

## 12. Use-Case States

Directional states:

```text
PROPOSED
RISK_REVIEW
DEVELOPMENT
EVALUATION
INTERNAL_PILOT
LIMITED_PRODUCTION
APPROVED
SUSPENDED
RETIRED
```

A proposed or suspended use case must not operate in production.

## 13. AI Risk Classification

Use cases should be classified directionally as:

```text
LOW
MODERATE
HIGH
PROHIBITED
```

Risk classification should consider:

* Data sensitivity
* user impact
* reversibility
* legal significance
* financial significance
* degree of automation
* human oversight
* likelihood and consequence of error
* external communication
* vulnerable-user impact
* scale

## 14. Prohibited Use Cases

Initially prohibited AI uses include:

* Autonomous application approval or rejection
* autonomous payment verification
* autonomous payment allocation
* autonomous refund approval
* authoritative legal advice
* autonomous agreement activation
* autonomous occupancy activation
* autonomous legal-notice issuance
* identity determination from images
* protected-attribute inference
* unrestricted tenant profiling
* tenant ranking based on opaque AI scoring
* autonomous eviction recommendations
* autonomous deletion of evidentiary records

Any proposed exception requires a new architectural and legal decision.

## 15. Model Registry

Every production model configuration must be registered.

A configuration should include:

* Configuration ID
* provider
* model identifier
* provider model version where available
* use-case eligibility
* input and output limits
* temperature or equivalent controls
* token budget
* timeout
* retry policy
* structured-output mode
* safety settings
* fallback configuration
* effective date
* evaluation evidence
* approval
* retirement status

## 16. Model Names Are Not Stable Contracts

Application code must not scatter raw provider model names.

Approved:

```typescript
modelConfigurationId: "agreement-explanation-production-v2"
```

Not approved:

```typescript
model: "provider-model-name"
```

inside unrelated feature code.

## 17. Model Change

Changing any of the following may require reevaluation:

* Provider
* model family
* model version
* context size
* reasoning mode
* temperature
* token limits
* tool availability
* safety settings
* output mode
* retrieval behavior
* fallback

A provider announcing a newer model does not authorize automatic production migration.

## 18. Prompt Registry

Material prompts will be centrally registered and versioned.

A prompt record should include:

* Prompt ID
* version
* use case
* owner
* system instructions
* developer instructions
* variable schema
* prohibited variables
* output schema
* source requirements
* safety requirements
* evaluation evidence
* effective date
* retirement status

## 19. Prompt Immutability

A published prompt version must not be edited silently.

Changes create a new version.

Historical AI operations must remain attributable to the prompt version actually used.

## 20. Prompt Composition

Prompt composition should separate:

```text
SYSTEM POLICY
DEVELOPER OR USE-CASE INSTRUCTIONS
AUTHORIZED CONTEXT
UNTRUSTED USER CONTENT
OUTPUT CONTRACT
```

Untrusted content must remain clearly delimited and must not be treated as system policy.

## 21. Prompt Injection

Habitacores must assume that:

* User text
* uploaded documents
* retrieved documents
* property descriptions
* support messages
* agreement text
* external webpages

may contain instructions intended to manipulate a model.

Controls include:

* Trust-boundary labeling
* context delimitation
* least-privilege tools
* no secrets in prompts
* restricted retrieval
* output validation
* adversarial testing
* refusing unsupported tool requests
* human review for consequential output

## 22. Data Classification

Before an AI operation, the system must determine the classification of:

* User input
* retrieved context
* generated output
* stored artifacts
* provider metadata

Classification must follow the approved Habitacores data architecture.

## 23. Data Minimization

AI requests will contain only the minimum information required for the approved purpose.

The system should prefer:

* Internal opaque identifiers where required
* extracted relevant clauses
* bounded structured fields
* redacted text
* summarized context
* selected document pages
* approved metadata

over complete unrestricted records.

## 24. Prohibited Prompt Data

Prompts must not ordinarily contain:

* Passwords
* tokens
* MFA secrets
* signing keys
* database credentials
* provider credentials
* full bank-account details
* unrestricted government-identification records
* unrelated tenant records
* cross-organization context
* private storage credentials
* signed URLs
* complete payment-provider payloads

## 25. Permission-Aware Retrieval

Retrieval must occur under current Habitacores authority.

The retrieval layer must validate:

* Authenticated actor
* organization
* membership
* permission
* resource relationship
* assignment
* document classification
* use-case purpose
* legal hold or restriction
* permitted retrieval scope

The model itself does not decide what data it may retrieve.

## 26. No Unrestricted Database Access

AI providers and models will not receive:

* PostgreSQL credentials
* arbitrary SQL tools
* unrestricted database clients
* database-owner roles
* migration access
* RLS-bypass roles

All context must be selected through approved application services.

## 27. No Unrestricted Object-Storage Access

AI providers and models will not receive:

* Storage service-role credentials
* bucket-administration credentials
* permanent private-object URLs
* unrestricted document indexes

Documents must be retrieved, authorized, minimized, and transformed through controlled Habitacores services.

## 28. Retrieval Sources

Approved retrieval may draw from:

* Authorized property data
* approved user-facing policy
* applicable agreement clauses
* authorized document extracts
* approved support knowledge
* structured workflow state
* authorized financial summaries
* approved public regulatory or product references

Every use case must define its source classes.

## 29. Source Attribution

Grounded outputs should retain references to the source records or document sections used.

Sources must identify:

* Source type
* internal source ID
* version
* relevant section or chunk
* retrieval time
* authorization context
* confidence or relevance metadata where useful

Source attribution does not automatically prove that the AI interpreted the source correctly.

## 30. Retrieval Freshness

The system must consider whether retrieved information may be stale.

High-risk operations may require:

* Current database state
* current document version
* current agreement version
* current payment state
* current organization membership
* current policy version

Cached context must not silently override current authoritative state.

## 31. Structured Outputs

Application-consumed AI outputs must use defined runtime schemas wherever feasible.

Schemas should constrain:

* Required fields
* enumerations
* types
* maximum lengths
* nested structures
* nullable fields
* unsupported extra fields
* citations or source references
* confidence representation where approved

## 32. Schema Validation

A provider returning parseable JSON is not sufficient.

Habitacores must validate:

* Schema conformance
* business invariants
* authorized values
* source references
* monetary representation
* identifiers
* dates
* enum membership
* prohibited content

Invalid output must not update authoritative state.

## 33. Output States

Directional validation states:

```text
GENERATED
SCHEMA_VALID
SCHEMA_INVALID
SAFETY_REJECTED
GROUNDING_FAILED
BUSINESS_RULE_FAILED
HUMAN_REVIEW_REQUIRED
HUMAN_ACCEPTED
HUMAN_EDITED
HUMAN_REJECTED
EXPIRED
```

## 34. Raw Output

Raw model output is untrusted.

It must not directly:

* Execute database mutations
* initiate payments
* verify payments
* allocate money
* create receipts
* send legal notices
* execute agreements
* activate occupancy
* change permissions
* delete documents
* trigger external communication without policy

## 35. Human Review

Human review is required where output may materially influence:

* Applicant treatment
* financial review
* legal interpretation
* agreement communication
* payment evidence
* tenancy disputes
* sensitive support
* external consequential communication
* fraud or anomaly investigation

## 36. Human Disposition

Review outcomes should include:

```text
ACCEPTED
ACCEPTED_WITH_EDITS
REJECTED
ESCALATED
NOT_USED
```

The reviewer, time, reason, and final applied content should be preserved where material.

## 37. Human Review Is Not Ceremonial

The interface must enable the reviewer to:

* Inspect source material
* inspect AI output
* understand limitations
* edit or reject
* identify unsupported claims
* escalate
* complete the workflow manually

A forced approval click without meaningful inspection is not adequate human oversight.

## 38. Manual Fallback

Every material AI-assisted workflow must have a deterministic non-AI path.

Examples:

* AI summarization unavailable → reviewer reads the source.
* AI classification unavailable → manual classification queue.
* AI drafting unavailable → approved template workflow.
* AI extraction unavailable → manual data entry.
* AI clause explanation unavailable → original agreement remains accessible.

AI failure must not make essential tenancy operations impossible.

## 39. Provider Failure

Provider failure must result in a controlled outcome such as:

```text
AI_TEMPORARILY_UNAVAILABLE
MANUAL_REVIEW_REQUIRED
FALLBACK_MODEL_USED
REQUEST_DEFERRED
```

The system must not fabricate a response or reuse an unrelated stale output.

## 40. Fallback Models

Fallback models may be used only when:

* Approved for the use case
* evaluated
* schema compatible
* privacy compatible
* policy compatible
* cost bounded
* observable

Automatic fallback to an unapproved provider or weaker safety configuration is prohibited.

## 41. Retries

AI retries must be bounded.

Retries should consider:

* Provider timeout
* rate limit
* transient server error
* invalid structured response
* request idempotency
* cost impact
* duplicate output
* user cancellation
* workflow expiry

Repeatedly retrying deterministic invalid output without changing strategy is prohibited.

## 42. Idempotency

Material AI operations should use an idempotency or deduplication context.

The system should prevent:

* Repeated user taps
* worker retries
* duplicate outbox events
* network timeouts

from creating uncontrolled duplicate cost or conflicting review records.

## 43. Tools and Function Calls

Model-requested tools must be registered explicitly.

A tool definition must specify:

* Stable tool ID
* approved use cases
* authorization requirements
* input schema
* output schema
* read or write behavior
* idempotency
* audit
* rate limits
* confirmation requirements
* failure behavior

## 44. Read Tools

Read tools may retrieve bounded authorized information.

They must not provide unrestricted search across:

* All tenants
* all documents
* all payments
* all agreements
* all user records

## 45. Write Tools

Direct model-controlled write tools are prohibited initially for material domain operations.

An AI may propose an action, but an application service and authorized human must independently validate and execute it where permitted.

## 46. No Autonomous Agents With Broad Authority

Habitacores will not initially deploy autonomous agents with:

* Broad database access
* unrestricted web access
* payment authority
* agreement authority
* communication authority
* deletion authority
* privilege-management authority
* arbitrary code execution

Any future agentic capability requires a separate ADR and threat model.

## 47. External Web Access

AI web access is not approved by default.

Where an approved use case needs external information, Habitacores should use:

* Approved source allowlists
* server-side retrieval
* content validation
* provenance
* freshness checks
* citation preservation
* prompt-injection controls

The model should not browse unrestricted websites with Habitacores credentials.

## 48. Evaluation

Every production use case requires evaluation before activation.

Evaluation must measure, as applicable:

* Task correctness
* schema validity
* grounding
* citation accuracy
* hallucination
* safety
* privacy leakage
* cross-tenant resistance
* prompt-injection resistance
* human usefulness
* latency
* cost
* fallback behavior

## 49. Evaluation Sets

Evaluation datasets must contain:

* Representative normal cases
* edge cases
* ambiguous cases
* adversarial cases
* missing-data cases
* stale-data cases
* cross-tenant attack cases
* prompt-injection cases
* invalid-document cases
* provider-failure cases

Production data should not be copied into evaluation datasets without an approved privacy process.

## 50. Evaluation Baselines

Every material use case should define:

* Current manual baseline
* previous model baseline
* required quality threshold
* unacceptable failure categories
* review acceptance rate
* edit rate
* rejection rate
* latency ceiling
* cost ceiling

A model should not be approved merely because its outputs appear impressive.

## 51. Regression Testing

Evaluation must rerun after material changes to:

* Model
* provider
* prompt
* retrieval policy
* output schema
* safety policy
* tool definition
* source corpus
* preprocessing
* context assembly

## 52. Online Monitoring

Production monitoring should detect:

* Schema-validation failures
* grounding failures
* user complaints
* reviewer rejection
* high edit rates
* unexpected latency
* token spikes
* cost spikes
* provider errors
* safety failures
* prompt-injection attempts
* cross-tenant retrieval attempts

## 53. AI Operation Record

Every material AI call should create an AI-operation record containing:

* Operation ID
* use-case ID
* organization where applicable
* requesting actor
* provider
* model-configuration ID
* prompt-version ID
* retrieval-policy version
* input classification
* output classification
* source references
* start and completion time
* token estimates
* estimated cost
* validation results
* human-review requirement
* human disposition
* final application outcome
* trace and audit references

## 54. Content Retention

Raw prompts and outputs should not be retained by default merely because they are available.

Retention must be defined by:

* Use case
* classification
* investigation need
* evaluation need
* user expectation
* provider terms
* legal requirement
* storage cost
* deletion policy

## 55. Restricted AI Artifacts

Where raw content must be retained, it should use:

* Restricted storage
* tenant isolation
* access control
* encryption
* retention
* legal hold
* access audit
* redaction where possible

Raw content must not be placed in ordinary logs or metrics.

## 56. Provider Data Handling

Before production use, provider review must determine:

* Whether API data is used for model training
* default retention
* configurable retention
* geographic processing
* subprocessors
* deletion process
* abuse-monitoring retention
* enterprise controls
* administrative access
* contractual commitments

Provider marketing statements must not replace contractual and technical review.

## 57. Cost Governance

Every use case must define:

* Token limits
* request limits
* per-user or per-organization limits
* daily and monthly budgets
* timeout
* model tier
* caching policy
* fallback
* alert thresholds
* cost owner

## 58. Cost Is Not Business Authority

AI token or provider cost data is operational information.

It must not be used as the authoritative basis for rent, payments, refunds, or organization financial balances.

## 59. Caching

AI output caching may be used only when:

* Inputs are safely fingerprinted.
* authorization context is respected.
* organization boundaries are preserved.
* source versions are included.
* prompt and model versions are included.
* expiry is defined.
* output remains suitable for reuse.

Cross-tenant AI-output cache reuse is prohibited unless the content is explicitly public and tenant independent.

## 60. AI Feature Flags

ADR-018 governs AI feature rollout.

A feature flag may disable or pilot an approved use case.

It cannot:

* Approve an unregistered use case
* bypass data policy
* bypass human review
* convert AI output into authority
* expose a prohibited model
* weaken tenant isolation

## 61. AI Audit Events

Material audit events may include:

```text
AI_USE_CASE_APPROVED
AI_USE_CASE_SUSPENDED
AI_MODEL_CONFIGURATION_APPROVED
AI_PROMPT_VERSION_PUBLISHED
AI_OPERATION_REQUESTED
AI_OUTPUT_REVIEWED
AI_OUTPUT_ACCEPTED
AI_OUTPUT_EDITED
AI_OUTPUT_REJECTED
AI_PROVIDER_CHANGED
AI_SAFETY_INCIDENT_OPENED
AI_KILL_SWITCH_ACTIVATED
```

Audit metadata must not contain unrestricted prompts or private documents.

## 62. Observability

ADR-012 governs AI telemetry.

Metrics may include:

```text
ai_operations
ai_operation_duration
ai_provider_errors
ai_schema_validation_failures
ai_grounding_failures
ai_safety_failures
ai_human_acceptances
ai_human_edits
ai_human_rejections
ai_prompt_injection_detections
ai_input_tokens
ai_output_tokens
ai_estimated_cost
ai_manual_fallbacks
```

User IDs, organization IDs, prompt text, and document contents must not become ordinary metric labels.

## 63. Alerts

Priority alerts may include:

* AI provider authentication failure
* sudden schema-validation failure
* safety-control bypass
* cross-tenant retrieval attempt
* prohibited data sent to a provider
* unexpected model-configuration change
* cost spike
* token spike
* human rejection spike
* grounding failure spike
* AI operation missing required human review
* raw AI content appearing in logs
* unregistered use-case invocation

## 64. Incident Response

An AI incident may require:

* Use-case suspension
* model suspension
* provider suspension
* prompt rollback
* retrieval-policy restriction
* kill-switch activation
* preserved evidence
* affected-user analysis
* output correction
* security or privacy escalation
* post-incident evaluation

## 65. AI Kill Switch

Every material use case must support a bounded disablement mechanism.

Disabling AI must not:

* Delete records
* mark work complete
* approve applications
* change payments
* alter agreements
* block manual operation indefinitely
* conceal prior AI involvement

## 66. Web and Mobile

Web and mobile clients will request AI capabilities through approved Habitacores APIs.

They must not:

* Call providers directly
* hold provider secrets
* submit unrestricted private storage URLs
* determine final provider or model policy
* treat local output as authoritative
* bypass server-side authorization

## 67. Support and Communication

AI-generated customer communication remains a draft unless the approved use case explicitly permits bounded automated communication.

Material financial, legal, agreement, occupancy, security, or dispute communication requires approved human review.

## 68. Payment Workflows

AI may summarize payment evidence for a reviewer.

It may not:

* Verify that funds cleared
* determine the authoritative amount
* allocate payment
* issue receipts
* approve refunds
* close reconciliation discrepancies

ADR-016 remains authoritative.

## 69. Agreement Workflows

AI may explain or compare approved agreement clauses.

It may not:

* Approve legal templates
* invent binding terms without review
* determine signer authority
* sign
* record consent
* mark execution complete
* activate or terminate agreements

ADR-017 remains authoritative.

## 70. Application Workflows

AI may organize or summarize submitted information.

It may not autonomously approve, reject, rank, or score applicants for tenancy.

Any future decision-support scoring requires a separate fairness, legal, explainability, and human-governance decision.

## 71. Document Workflows

AI processing begins only after:

* Upload authorization
* quarantine
* file validation
* malware scanning
* document availability
* use-case authorization

AI processing does not make a document authentic or legally verified.

## 72. Testing

Required tests include:

* Gateway-boundary tests
* provider-adapter contract tests
* use-case authorization tests
* prompt-version tests
* model-registry tests
* data-minimization tests
* redaction tests
* cross-tenant retrieval tests
* prompt-injection tests
* output-schema tests
* source-attribution tests
* human-review tests
* fallback tests
* provider-outage tests
* budget tests
* AI-authority prohibition tests
* audit and observability tests

## 73. Gateway-Only Test

```text
Given:
A production feature requests AI generation.

When:
The operation executes.

Then:
The request passes through the Habitacores AI Gateway.

And:
The feature cannot call the provider SDK directly.
```

## 74. Cross-Tenant Retrieval Test

```text
Given:
A user belongs only to Organization A.

And:
The prompt attempts to request Organization B information.

When:
Context retrieval executes.

Then:
Application authorization and PostgreSQL RLS prevent retrieval.

And:
Organization B content is not sent to the model.
```

## 75. Prompt-Injection Test

```text
Given:
An uploaded document contains instructions telling the model to reveal
system prompts and retrieve unrelated records.

When:
The document is processed for an approved summarization use case.

Then:
The document content remains classified as untrusted data.

And:
The model receives no additional permissions or unrelated context.
```

## 76. Structured-Output Test

```text
Given:
A model returns output that does not match the approved schema.

When:
Validation executes.

Then:
The output is rejected or routed to the approved recovery path.

And:
It cannot update authoritative Habitacores state.
```

## 77. Human-Review Test

```text
Given:
AI drafts a payment-evidence summary.

When:
The operation completes.

Then:
An authorized finance reviewer must inspect the evidence and output.

And:
The AI result cannot verify payment independently.
```

## 78. Provider-Outage Test

```text
Given:
The approved AI provider is unavailable.

When:
A reviewer opens an AI-assisted workflow.

Then:
The system exposes the deterministic manual workflow.

And:
It does not fabricate an AI response or mark the review completed.
```

## 79. Model-Change Test

```text
Given:
A provider releases a newer model.

When:
The model becomes available through the provider API.

Then:
Habitacores continues using the approved model configuration.

And:
The new model is not used in production until evaluation and approval.
```

## 80. Codex Boundaries

Every Codex AI work package must specify:

* Use-case ID
* risk classification
* input and output classification
* actor and organization scope
* permitted sources
* prohibited data
* provider and model configuration
* prompt version
* retrieval policy
* output schema
* safety checks
* human-review requirement
* fallback
* budget
* retention
* audit
* observability
* evaluation
* tests

Codex must not:

* Call provider SDKs outside adapters
* embed provider keys
* place AI keys in browsers or mobile applications
* give models database credentials
* give models storage administrative credentials
* send unrestricted private records
* accept raw output as authoritative
* permit unregistered production use cases
* scatter model names or prompts throughout feature code
* bypass output validation
* bypass human review
* log raw prompts or outputs by default
* allow AI to verify payments
* allow AI to approve applications
* allow AI to sign or activate agreements
* allow AI to activate occupancy
* deploy broad autonomous agents without a separate ADR

## 81. Prohibited Patterns

The following are not approved:

* Direct AI-provider calls from feature modules
* provider keys in client applications
* unrestricted RAG across every tenant
* models with direct SQL access
* models with unrestricted object-storage access
* unversioned prompts
* raw provider model names throughout application code
* silent provider or model upgrades
* output accepted without runtime validation
* consequential AI output without human review
* raw prompt and response logging by default
* AI-generated legal or financial authority
* autonomous broad tool execution
* production experiments without evaluation
* no manual fallback
* provider dashboard as the only AI-operation record

## 82. Options Considered

### Option A — Direct Provider Integration in Every Feature

Advantages:

* Fast initial implementation
* low gateway-development effort

Disadvantages:

* Provider lock-in
* duplicated security and redaction
* inconsistent prompts
* weak cost control
* weak evaluation
* difficult audit
* high risk of data leakage

Decision:

Rejected.

### Option B — Single Provider as Permanent AI Architecture

Advantages:

* Simpler initial operations
* fewer adapters

Disadvantages:

* Vendor lock-in
* model deprecation risk
* pricing dependency
* regional and privacy dependency
* difficult migration
* provider concepts leak into the product

Decision:

Rejected as the permanent architecture.

### Option C — AI With Direct Database and Tool Access

Advantages:

* Flexible agent behavior
* rapid prototype development

Disadvantages:

* Severe tenant-isolation risk
* uncontrolled actions
* prompt-injection exposure
* weak authorization
* difficult audit
* incompatible with Habitacores authority boundaries

Decision:

Rejected.

### Option D — Habitacores AI Gateway With Registered Use Cases

Advantages:

* Provider portability
* centralized security
* data minimization
* prompt and model versioning
* permission-aware retrieval
* structured-output validation
* cost governance
* human oversight
* evaluation and observability
* deterministic fallback

Disadvantages:

* Greater initial engineering work
* gateway becomes a critical service
* provider differences require adapters
* evaluations require ongoing maintenance
* human review increases operational effort

Decision:

Approved.

## 83. Positive Consequences

* AI usage becomes governed and traceable.
* providers remain replaceable.
* prompts and models remain versioned.
* tenant isolation is preserved before retrieval.
* private data is minimized before provider transmission.
* outputs are validated before use.
* high-impact decisions remain human controlled.
* manual workflows remain available.
* AI cost and quality become measurable.
* provider and model changes require evaluation.

## 84. Negative Consequences

* Gateway development adds complexity.
* evaluations require continuous maintenance.
* provider adapters require testing.
* human review may reduce automation speed.
* retaining selected AI evidence adds storage obligations.
* output quality remains probabilistic.
* provider portability cannot eliminate all provider differences.

## 85. Risks

### 85.1 Cross-Tenant Disclosure

Controls:

* Authorization before retrieval
* RLS
* purpose-limited context
* gateway-only access
* cross-tenant tests
* no unrestricted vector search

### 85.2 Hallucination

Controls:

* Grounding
* source attribution
* structured outputs
* business validation
* human review
* manual fallback

### 85.3 Prompt Injection

Controls:

* Trust boundaries
* context delimitation
* restricted tools
* adversarial testing
* no secrets
* no privilege escalation through content

### 85.4 Provider Lock-In

Controls:

* Gateway contracts
* adapters
* internal registries
* portable evaluation sets
* no provider types in domain code

### 85.5 Cost Escalation

Controls:

* Budgets
* token limits
* rate limits
* model tiers
* caching
* alerts
* feature kill switches

### 85.6 AI Becoming Business Authority

Controls:

* Explicit prohibitions
* domain-service validation
* human approval
* no direct write tools
* acceptance tests
* audit

## 86. Implementation Sequence

```text
1. Define AI governance and use-case taxonomy
2. create AI use-case registry
3. create provider-neutral gateway contracts
4. create model-configuration registry
5. create versioned prompt registry
6. define data-classification and redaction policies
7. implement permission-aware retrieval
8. implement output-schema validation
9. implement AI-operation records
10. implement one sandbox provider adapter
11. implement token, timeout, retry, and cost budgets
12. implement human-review disposition workflows
13. implement observability and audit correlation
14. create evaluation datasets and baseline tests
15. implement adversarial and prompt-injection tests
16. implement provider-failure and manual-fallback behavior
17. implement AI feature kill switches
18. conduct provider privacy, security, and commercial review
19. approve initial low-risk internal use cases
20. conduct limited production pilots
21. select and approve the initial production provider
22. expand only after measured evaluation
```

## 87. Deferred Decisions

This ADR does not decide:

* Initial production AI provider
* exact production model
* exact embedding provider
* exact vector-search implementation
* exact gateway deployment topology
* exact prompt-registry storage
* exact evaluation framework
* exact moderation provider
* exact redaction library
* exact token budgets
* exact use-case risk thresholds
* exact content-retention periods
* exact regional-processing requirements
* exact AI cost budget
* exact local-model strategy
* exact multimodal strategy
* exact agent architecture
* exact external web-retrieval architecture
* exact model-fallback order

These require bounded implementation decisions.

## 88. Supersession Conditions

This ADR should be reviewed when:

* Habitacores proposes autonomous agents.
* a model requires direct tool execution.
* a local or self-hosted model becomes operationally necessary.
* AI becomes subject to new Nigerian regulation.
* Habitacores enters another jurisdiction with materially different AI rules.
* enterprise customers require dedicated models or providers.
* regional processing becomes mandatory.
* AI supports formal tenancy eligibility decisions.
* a provider cannot meet required privacy or audit controls.
* model volume requires a dedicated inference platform.

## 89. Final Decision Statement

Habitacores will implement a centralized, provider-neutral AI Gateway as the only approved production path to external or internally hosted AI models.

Every production AI capability will correspond to an approved and registered use case with a defined owner, purpose, risk classification, authorized users, permitted data, prohibited data, provider and model configuration, prompt version, retrieval policy, output schema, validation requirements, human-review requirements, evaluation suite, cost budget, retention policy, manual fallback, and retirement condition.

Web applications, mobile applications, workers, feature modules, provider integrations, and coding agents will not call production AI providers directly or hold provider credentials.

All AI context will be retrieved through authenticated, authorized, tenant-isolated Habitacores services. Models will not receive unrestricted PostgreSQL, object-storage, provider, or secrets-management access.

Data sent to AI providers will be purpose limited, minimized, classified, and redacted where required. Passwords, tokens, signing keys, database credentials, provider credentials, private storage credentials, and unrelated tenant records will never be included.

Prompts, model configurations, retrieval policies, safety policies, and output schemas will be versioned. Provider or model availability will not authorize silent production upgrades.

Raw AI output will remain untrusted. Application-consumed output will pass runtime schema validation, source and grounding checks where applicable, business-rule validation, and required human review before use.

AI may assist users by drafting, summarizing, extracting, explaining, organizing, or classifying information. AI will not independently authenticate users, grant organization access, approve applications, verify payments, allocate funds, approve refunds, issue receipts, create binding legal decisions, determine signer authority, execute agreements, activate agreements, activate occupancy, issue legal notices, or delete evidentiary records.

Material AI operations will preserve use-case, provider, model, prompt, retrieval-policy, source, validation, human-disposition, cost, trace, and audit references. Raw prompts and outputs will not enter ordinary logs or metrics.

Every material AI-assisted workflow will preserve a deterministic manual path. Provider outage, model failure, schema failure, safety failure, or AI kill-switch activation will not fabricate completion or prevent authorized users from continuing through the approved manual workflow.

AI use cases and model changes will be evaluated using representative, edge, adversarial, prompt-injection, privacy, cross-tenant, grounding, safety, latency, cost, and failure cases before production activation and after material changes.

No provider, model, prompt, retrieved document, feature flag, browser, mobile application, worker, support operator, coding agent, or AI-generated output may bypass Habitacores authentication, organization membership, tenant isolation, RLS, authorization, data minimization, financial controls, agreement controls, document controls, human review, evaluation, audit, or Responsible AI safeguards.