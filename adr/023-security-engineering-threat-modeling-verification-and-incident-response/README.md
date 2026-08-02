# ADR-023 — Security Engineering, Threat Modeling, Verification, Penetration Testing, and Incident Response Strategy

## Decision Record

| Field | Value |
|---|---|
| Product | Habitacores |
| ADR | ADR-023 |
| Version | 0.1 |
| Status | Proposed |
| Decision Type | Foundational Security Engineering, Assurance, Detection, and Incident-Response Architecture |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Secure-Development Reference | NIST Secure Software Development Framework |
| Application Verification Reference | OWASP Application Security Verification Standard |
| Incident-Response Reference | NIST SP 800-61 Revision 3 |
| Threat Modeling | Mandatory for material capabilities and trust-boundary changes |
| Security Verification | Automated and human verification |
| Penetration Testing | Required before material production scale and periodically thereafter |
| Vulnerability Intake | Centralized, attributable, risk based, and time bounded |
| Incident Authority | Authorized Habitacores incident-response process |
| Evidence Preservation | Required |
| Tenant-Boundary Validation | Mandatory |
| Production Containment | Explicitly authorized and audited |
| Customer Communication | Fact based and legally reviewed where required |
| Vulnerability Disclosure | Coordinated process required |
| AI Security Authority | Prohibited |
| Last Updated | 2 August 2026 |

## 1. Context

Habitacores will operate a multi-tenant property platform containing:

- Identity and membership records
- property and unit information
- rental applications
- agreements and execution evidence
- payment and reconciliation records
- private documents
- audit events
- communications
- maintenance and support records
- AI-assisted workflows
- web, mobile, API, and worker surfaces
- third-party provider integrations

Its principal security risks include:

- Cross-tenant data disclosure
- unauthorized privilege elevation
- account takeover
- insecure direct-object access
- injection
- server-side request forgery
- webhook forgery or replay
- malicious file upload
- payment and agreement manipulation
- secret exposure
- supply-chain compromise
- cloud or infrastructure misconfiguration
- administrator abuse
- AI prompt injection and data leakage
- denial of service
- destructive data modification
- insufficient incident detection
- incomplete or misleading incident communication

Security cannot be limited to vulnerability scanning after implementation.

It must be integrated into product design, development, review, deployment,
operation, incident response, recovery, and lessons learned.

## 2. Decision Drivers

The strategy must:

1. Protect tenant isolation.
2. protect identities and privileged access.
3. protect financial and agreement integrity.
4. identify threats before implementation.
5. verify controls through tests and review.
6. detect suspicious activity.
7. support fast and safe containment.
8. preserve forensic evidence.
9. coordinate incident recovery with ADR-021.
10. govern vulnerability intake and remediation.
11. support independent penetration testing.
12. constrain administrators, workers, providers, and AI.
13. prevent security changes from bypassing release controls.
14. support legally appropriate communications.
15. improve architecture after incidents and exercises.
16. preserve an implementation-ready security backlog.

## 3. Decision

Habitacores will implement a security-engineering lifecycle:

```text
Product proposal
→ Data and trust-boundary identification
→ Threat model and abuse cases
→ Security requirements
→ Architecture review
→ Secure implementation
→ Automated verification
→ Human security review
→ Release-risk review
→ Controlled deployment
→ Detection and monitoring
→ Vulnerability management
→ Incident response and recovery
→ Lessons learned
→ Control and architecture improvement
````

Security review must occur proportionally to risk throughout the lifecycle.

## 4. Security Concepts

Habitacores will distinguish:

```text
SECURITY_REQUIREMENT
THREAT
THREAT_ACTOR
ASSET
TRUST_BOUNDARY
ATTACK_SURFACE
ABUSE_CASE
VULNERABILITY
SECURITY_FINDING
SECURITY_EXCEPTION
SECURITY_EVENT
SECURITY_ALERT
SECURITY_INCIDENT
DATA_BREACH_ASSESSMENT
CONTAINMENT_ACTION
FORENSIC_ARTIFACT
RECOVERY_ACTION
LESSON_LEARNED
```

A scanner finding is not automatically an incident.

A suspicious event is not automatically a confirmed breach.

## 5. Security Ownership

Every material service or capability must identify:

* Product owner
* engineering owner
* security owner
* data owner
* operational owner
* incident escalation path
* provider dependencies
* recovery dependencies

At founding scale, one person may hold several responsibilities, but ownership
must remain explicit.

## 6. Threat Modeling

Threat modeling is required for:

* Authentication and session changes
* authorization and RLS changes
* organization and membership workflows
* payment workflows
* agreement and signature workflows
* document uploads and processing
* public APIs and webhooks
* privileged administration
* mobile authentication and deep links
* AI retrieval or tool access
* data exports
* provider integrations
* infrastructure trust-boundary changes
* backup and recovery workflows
* new externally reachable services

## 7. Threat-Model Contents

A threat model should identify:

* System purpose
* assets
* actors
* entry points
* data flows
* trust boundaries
* external providers
* privileges
* authentication assumptions
* authorization assumptions
* tenant-isolation assumptions
* abuse cases
* threats
* existing controls
* residual risk
* test requirements
* owners
* review date

## 8. Threat-Model Method

Habitacores may use structured methods such as:

* STRIDE
* misuse and abuse cases
* attack trees
* data-flow analysis
* trust-boundary analysis
* provider and supply-chain analysis

No single method is mandatory for every case.

The selected method must be understandable, reviewable, and linked to concrete
requirements and tests.

## 9. Abuse Cases

Security requirements must include malicious and accidental misuse.

Examples include:

```text
A user changes an organization ID in a request.
A former property manager reuses an active session.
A webhook is replayed after a valid transaction.
A malicious document attempts to exploit a parser.
A support operator searches unrelated tenants.
A mobile user modifies locally cached permissions.
An attacker requests password resets for many accounts.
A compromised worker attempts cross-tenant processing.
An uploaded document instructs an AI model to expose other records.
A restored environment sends production notifications.
```

## 10. Security Requirements

Threat-model outputs must become verifiable requirements.

A security requirement should contain:

* Stable identifier
* threat or risk addressed
* affected component
* required control
* implementation owner
* test method
* severity
* release condition
* residual risk
* related ADR or issue

A threat model without implementation or verification consequences is
insufficient.

## 11. Application Security Verification

Habitacores will maintain a tailored application-security verification
baseline.

The baseline must cover, as applicable:

* Architecture
* authentication
* sessions
* authorization
* validation
* cryptography
* error handling
* logging
* data protection
* communications
* files and resources
* APIs and web services
* configuration
* business logic
* web frontend
* mobile clients
* background workers

The exact requirement set and target assurance level are deferred for bounded
implementation review.

## 12. Verification Evidence

Security verification evidence may include:

* Unit tests
* integration tests
* authorization tests
* RLS tests
* API contract tests
* dynamic security tests
* static analysis
* dependency review
* infrastructure plans
* configuration review
* manual code review
* penetration-test reports
* provider attestations
* recovery exercises

Passing one scanner does not constitute comprehensive verification.

## 13. Tenant-Isolation Verification

Every tenant-scoped capability requires negative tests proving that an actor
from one organization cannot access or modify another organization’s:

* Users
* memberships
* properties
* units
* applications
* payments
* agreements
* documents
* communications
* audit records
* AI retrieval context

Application authorization and PostgreSQL RLS must be tested independently.

## 14. Privileged Access

Privileged operations include:

* Role and permission administration
* organization-level administration
* production support access
* payment correction
* refund approval
* agreement administration
* data export
* legal-hold administration
* production configuration
* secret administration
* backup access
* security containment
* incident recovery

They require stronger authentication, narrow authorization, audit, and
periodic access review.

## 15. Security Review Gates

A material capability may require security approval before:

* Implementation begins
* public preview
* production release
* expanded tenant rollout
* privileged-data access
* provider integration
* autonomous processing
* removal of a compensating control

Security review must not be reduced to a ceremonial checkbox.

## 16. Security Testing in CI

CI should include appropriate combinations of:

* Static analysis
* dependency scanning
* secret scanning
* license review
* IaC scanning
* workflow scanning
* authorization tests
* RLS tests
* security regression tests
* API validation tests
* upload validation tests
* webhook verification tests
* AI-boundary tests

ADR-022 remains authoritative for supply-chain security controls.

## 17. Dynamic Security Testing

Dynamic testing should evaluate deployed non-production environments for:

* Authentication bypass
* authorization bypass
* insecure direct-object access
* injection
* request smuggling where relevant
* server-side request forgery
* unsafe redirects
* cross-site scripting
* cross-site request forgery
* insecure CORS
* rate-limit failures
* upload abuse
* exposed administrative interfaces
* sensitive error responses
* security-header weaknesses

Tests must not target production destructively without explicit approval.

## 18. Penetration Testing

Habitacores will conduct independent or appropriately separated penetration
testing:

* Before material production scale
* after major identity or authorization redesign
* after major payment or agreement changes
* after material public API expansion
* after significant infrastructure change
* periodically according to risk
* after a serious security incident where appropriate

## 19. Penetration-Test Scope

A test scope may include:

* Web application
* mobile APIs
* Android and iOS clients
* authentication
* sessions
* authorization
* tenant isolation
* privileged operations
* payment integrations
* agreement workflows
* document handling
* webhooks
* cloud configuration
* workers
* AI retrieval boundaries
* business-logic abuse

Denial-of-service, social-engineering, or production testing requires explicit
written scope.

## 20. Penetration-Test Handling

Penetration-test findings must enter the security-finding workflow.

Reports must be:

* Access controlled
* retained appropriately
* linked to remediation
* validated after fixes
* protected from unrestricted distribution

A test is not complete merely because a report was delivered.

## 21. Vulnerability Intake

Habitacores will maintain a controlled intake channel for:

* Internal findings
* automated findings
* penetration-test findings
* provider advisories
* dependency advisories
* user reports
* researcher reports
* employee reports
* incident discoveries

Every credible report must receive an identifier and initial triage.

## 22. Coordinated Vulnerability Disclosure

Habitacores should publish a responsible reporting path before public scale.

The disclosure process should define:

* Authorized contact method
* expected report contents
* acknowledgement target
* safe-testing boundaries
* researcher communication
* remediation coordination
* disclosure timing
* prohibited destructive activity
* legal and privacy escalation

A public bug-bounty program is not approved by this ADR.

## 23. Security Finding

A security finding should preserve:

* Finding ID
* source
* affected component
* affected environments
* description
* evidence
* severity
* exploitability
* tenant impact
* financial or legal impact
* owner
* remediation
* due date
* exception
* validation
* closure status

## 24. Risk Assessment

Risk assessment should consider:

* Likelihood
* impact
* exploitability
* exposure
* privileges required
* user interaction
* tenant reach
* data sensitivity
* financial impact
* legal impact
* detectability
* reversibility
* compensating controls

A numeric scanner severity must not be the sole risk decision.

## 25. Security Exceptions

A security exception requires:

* Specific finding or requirement
* business justification
* risk assessment
* compensating controls
* accountable owner
* approval
* expiry
* monitoring
* remediation plan

Permanent unowned exceptions are prohibited.

## 26. Security Events

Potential security events include:

```text
AUTHENTICATION_FAILURE_SPIKE
MFA_FAILURE_SPIKE
SESSION_REPLAY_SUSPECTED
PRIVILEGE_CHANGE
RLS_DENIAL_SPIKE
CROSS_TENANT_ACCESS_ATTEMPT
WEBHOOK_SIGNATURE_FAILURE
SECRET_USE_ANOMALY
BACKUP_ACCESS
PRODUCTION_EXPORT
MALWARE_DETECTION
SUSPICIOUS_FILE_PROCESSING
AI_DATA_BOUNDARY_VIOLATION
PRODUCTION_CONFIGURATION_CHANGE
SECURITY_CONTROL_DISABLED
```

Not every event becomes an alert or incident.

## 27. Detection Engineering

Every priority threat should map where practical to:

* Preventive control
* detectable signal
* detection rule
* owner
* severity
* response action
* test
* false-positive review
* retention requirement

A control with no detection or verification path may leave failures unnoticed.

## 28. Incident Definition

A security incident is an event or series of events that threatens or causes
material compromise of:

* Confidentiality
* integrity
* availability
* authentication
* authorization
* tenant isolation
* financial integrity
* legal or evidentiary integrity
* production infrastructure
* source or build integrity
* provider trust

## 29. Incident Severity

Directional severity levels:

| Level | Description                                                                                                                                                                      |
| ----- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SEV-1 | Confirmed or highly probable critical compromise, broad tenant exposure, financial or agreement integrity compromise, destructive production event, or prolonged critical outage |
| SEV-2 | Material security impact with bounded scope, privileged compromise, significant provider or operational disruption                                                               |
| SEV-3 | Limited confirmed incident or high-risk suspicious activity requiring coordinated response                                                                                       |
| SEV-4 | Low-impact event, policy violation, or suspicious condition requiring investigation                                                                                              |

Final thresholds and response times require operational approval.

## 30. Incident Roles

Directional incident roles include:

```text
INCIDENT_COMMANDER
SECURITY_LEAD
TECHNICAL_LEAD
FORENSIC_LEAD
RECOVERY_LEAD
FINANCIAL_RECONCILIATION_LEAD
LEGAL_AND_PRIVACY_LEAD
COMMUNICATIONS_LEAD
PRODUCT_OWNER
SCRIBE
```

At founding scale, roles may be combined, but command and accountability must
remain explicit.

## 31. Incident Lifecycle

Habitacores will manage incidents through:

```text
Preparation
→ Detection
→ Triage
→ Declaration
→ Containment
→ Investigation
→ Eradication
→ Recovery
→ Communication
→ Lessons learned
→ Control improvement
```

Activities may overlap and iterate.

## 32. Incident Declaration

Incident declaration should record:

* Incident ID
* detection source
* declaration time
* severity
* known scope
* affected environments
* affected organizations where known
* incident commander
* communication channel
* immediate containment decisions
* evidence-preservation requirements

## 33. Containment

Containment may include:

* Revoking sessions
* disabling accounts
* rotating credentials
* blocking provider credentials
* suspending a feature
* disabling a provider adapter
* pausing workers
* blocking malicious traffic
* restricting privileged access
* entering read-only mode
* isolating an environment
* suspending AI operations
* preserving affected resources

Containment must minimize further harm without destroying necessary evidence.

## 34. Containment Authority

Production containment requires authorized incident roles.

Emergency containment must be:

* Narrowly scoped
* attributable
* time recorded
* reversible where practical
* monitored
* reviewed
* followed by permanent remediation

Feature flags may assist containment but remain subordinate to ADR-018.

## 35. Evidence Preservation

Potential evidence includes:

* Audit events
* application logs
* authentication events
* traces
* provider events
* database records
* object versions
* workflow runs
* infrastructure plans and state history
* deployment records
* artifact attestations
* access logs
* affected files
* communication records
* timeline notes

Evidence must be protected against unauthorized alteration and deletion.

## 36. Evidence Handling

Evidence handling should preserve:

* Source
* collection time
* collector
* cryptographic hash where appropriate
* storage location
* access history
* relationship to the incident
* transformations
* retention or legal hold

Habitacores must not claim formal forensic chain of custody unless the relevant
procedures have actually been established and followed.

## 37. Investigation

Investigation should determine, as evidence permits:

* Initial access
* affected identities
* affected tenants
* affected resources
* attacker activity
* duration
* persistence
* data accessed
* data modified
* external actions
* detection gaps
* control failures
* recovery requirements

Uncertainty must be recorded explicitly.

## 38. Breach Assessment

A security incident is not automatically a legally reportable data breach.

The legal and privacy assessment must consider:

* Data involved
* affected persons
* access or acquisition evidence
* encryption
* jurisdiction
* contractual obligations
* risk of harm
* notification requirements
* regulatory requirements

Qualified legal review is required where reporting obligations may exist.

## 39. Communication

Incident communication must be:

* Fact based
* timely
* audience appropriate
* approved by authorized roles
* consistent across channels
* clear about knowns and unknowns
* updated as material facts change
* preserved as incident evidence

Habitacores must not speculate or falsely assure users that no data was
affected before sufficient investigation.

## 40. Recovery

Security recovery must coordinate with ADR-021.

Recovery may require:

* Clean infrastructure
* patched application artifacts
* credential rotation
* session revocation
* data restoration
* financial reconciliation
* agreement-integrity validation
* provider reconciliation
* monitoring enhancement
* staged service restoration
* customer support preparation

Normal service must not resume solely because an attacker appears inactive.

## 41. Post-Incident Review

Material incidents require review covering:

* Timeline
* impact
* root and contributing causes
* detection
* response effectiveness
* communication
* recovery
* control failures
* architectural weaknesses
* process weaknesses
* required remediations
* owners and due dates

The purpose is operational improvement, not concealment or blame avoidance.

## 42. Tabletop Exercises

Habitacores will conduct tabletop exercises covering scenarios such as:

* Cross-tenant disclosure
* administrator account takeover
* payment-provider compromise
* leaked production secret
* malicious dependency
* destructive database action
* ransomware or backup access loss
* agreement-document tampering
* compromised mobile release
* AI data leakage
* DNS or domain compromise

## 43. Initial Exercise Cadence

Directional cadence:

* Critical incident contacts: reviewed quarterly
* focused tabletop exercise: semiannually
* full cross-functional exercise: annually
* additional exercise after material architecture change
* follow-up exercise after serious control failure

Final cadence requires operational approval.

## 44. Security Metrics

Metrics may include:

```text
open_security_findings
critical_finding_age
high_finding_age
security_exception_age
threat_models_completed
security_requirements_verified
penetration_test_findings
mean_time_to_triage
mean_time_to_contain
mean_time_to_recover
security_incidents
cross_tenant_attempts
privileged_access_anomalies
incident_action_completion
```

Tenant IDs and personal information must not become unrestricted metric labels.

## 45. Security Alerts

Priority alerts may include:

* Confirmed cross-tenant retrieval
* RLS unexpectedly disabled
* privileged account takeover
* production secret exposure
* unsigned payment or signature webhook accepted
* unauthorized production export
* audit-event interruption
* backup deletion attempt
* production deployment without approved provenance
* unexpected database-owner use
* mass document access
* AI retrieval outside permitted scope
* security kill switch bypass

## 46. Responsible AI

AI may assist with:

* Alert summarization
* log correlation proposals
* incident-timeline drafting
* finding categorization
* runbook retrieval
* post-incident report drafting

AI may not:

* Declare an incident conclusively without authorized review
* determine legal breach status
* contain production independently
* revoke users independently
* rotate credentials independently
* notify affected users independently
* disclose vulnerability details publicly
* declare recovery complete
* close a security incident

## 47. Testing

Required tests include:

* Threat-to-control traceability
* authentication abuse cases
* session revocation
* authorization bypass attempts
* cross-tenant access attempts
* RLS disablement detection
* privileged-operation audit
* webhook forgery and replay
* upload abuse
* SSRF protections
* rate-limit behavior
* security-event generation
* incident containment
* evidence preservation
* recovery coordination
* AI-authority boundaries

## 48. Cross-Tenant Incident Test

```text
Given:
An Organization A user attempts to retrieve an Organization B agreement.

When:
The request reaches the application and database.

Then:
Application authorization rejects the operation.

And:
PostgreSQL RLS prevents access.

And:
The attempt produces the approved security telemetry without exposing
Organization B content.
```

## 49. Session-Revocation Test

```text
Given:
A privileged account is suspected to be compromised.

When:
An authorized containment action revokes the account's sessions.

Then:
Existing sessions can no longer access protected operations.

And:
The containment action is audited.
```

## 50. Webhook-Attack Test

```text
Given:
An attacker sends a forged or replayed provider webhook.

When:
The webhook endpoint processes the request.

Then:
Signature, environment, schema, and deduplication controls prevent
unauthorized domain-state changes.

And:
The attempt produces bounded security evidence.
```

## 51. Evidence-Preservation Test

```text
Given:
A SEV-1 incident is declared.

When:
Relevant logs, audit records, provider events, and artifacts are preserved.

Then:
They are access controlled, integrity protected, attributable, and retained
according to the incident hold.
```

## 52. AI-Incident-Boundary Test

```text
Given:
AI analysis labels an event as a confirmed breach.

When:
The incident workflow receives the result.

Then:
The output remains advisory.

And:
Only an authorized human role may declare the incident or approve external
communication.
```

## 53. Codex Boundaries

Every Codex security work package must specify:

* Asset
* actor
* trust boundary
* threat
* abuse case
* security requirement
* authorization and tenant impact
* control
* detection
* audit
* failure behavior
* test
* incident consequence
* residual risk

Codex must not:

* Mark security review complete automatically
* remove a security control to make a test pass
* disable RLS
* weaken authorization
* suppress findings without an approved exception
* generate production credentials
* conduct destructive production testing
* classify legal breach status
* notify users
* authorize containment
* close incidents
* expose forensic evidence
* treat AI analysis as incident authority

## 54. Prohibited Patterns

The following are not approved:

* Security review only after implementation
* threat models with no linked requirements
* penetration-test reports without remediation tracking
* scanner severity as the only risk assessment
* permanent unowned security exceptions
* destructive production testing without written authorization
* disabling logging during an incident
* deleting suspicious records before evidence preservation
* restoring service without security validation
* customer notification based on speculation
* hiding uncertainty in breach assessments
* AI-authorized containment or disclosure
* claims of forensic integrity without supporting procedures

## 55. Options Considered

### Option A — Scanner-Led Security

Advantages:

* Easy automation
* low initial staffing burden

Disadvantages:

* Misses business-logic abuse
* weak tenant-isolation analysis
* weak incident readiness
* insufficient architecture review

Decision:

Rejected as the complete strategy.

### Option B — Annual Penetration Test Only

Advantages:

* Independent assessment
* recognizable assurance activity

Disadvantages:

* Point-in-time coverage
* security defects remain during development
* limited incident-response readiness
* limited continuous verification

Decision:

Rejected as the complete strategy.

### Option C — Informal Founder-Led Response

Advantages:

* Fast early-stage decisions
* minimal process overhead

Disadvantages:

* unclear authority
* weak evidence preservation
* inconsistent communication
* key-person dependency
* poor repeatability

Decision:

Rejected as the long-term operating model.

### Option D — Integrated Security Engineering and Incident Response

Advantages:

* Threats addressed before release
* testable requirements
* tenant-boundary verification
* continuous detection
* governed vulnerabilities
* coordinated response
* preserved evidence
* measurable improvement

Disadvantages:

* Requires ongoing engineering effort
* independent testing adds cost
* response exercises require time
* security telemetry requires tuning
* legal review may be required during incidents

Decision:

Approved.

## 56. Positive Consequences

* Security becomes part of product architecture.
* tenant-isolation threats receive explicit treatment.
* findings gain owners and remediation deadlines.
* penetration testing complements automated controls.
* incidents receive defined authority and evidence handling.
* recovery and communication become coordinated.
* AI remains advisory.
* lessons learned feed back into architecture and testing.

## 57. Negative Consequences

* Threat modeling and verification add delivery work.
* security findings may delay releases.
* penetration testing and exercises add cost.
* detection systems require maintenance.
* false-positive alerts require triage.
* incident procedures require training.
* legal and privacy uncertainty may remain during investigation.

## 58. Risks

### 58.1 Security Process Becomes Ceremonial

Controls:

* Linked requirements
* named owners
* test evidence
* release gates
* remediation tracking

### 58.2 Excessive Security Friction

Controls:

* Risk-based depth
* reusable threat models
* automated verification
* approved patterns
* clear severity thresholds

### 58.3 Missing Cross-Tenant Vulnerability

Controls:

* Negative authorization tests
* RLS tests
* penetration testing
* detection telemetry
* architecture review

### 58.4 Incident Evidence Loss

Controls:

* Retention
* incident holds
* restricted access
* preservation runbooks
* integrity checks

### 58.5 Premature External Communication

Controls:

* Authorized communications lead
* legal and privacy review
* known-versus-unknown distinction
* preserved approvals

### 58.6 AI Overreach

Controls:

* Advisory-only design
* no production containment tools
* human declarations
* audit
* authority tests

## 59. Implementation Sequence

```text
1. Define security ownership and escalation contacts
2. establish the threat-model template and security-requirement registry
3. tailor an application-security verification baseline
4. add security review to feature and architecture workflows
5. implement cross-tenant and privileged-operation test suites
6. map priority threats to detection signals
7. establish vulnerability intake and triage
8. establish security-exception governance
9. define incident severity and declaration procedures
10. define containment and evidence-preservation runbooks
11. establish incident communication and legal-review procedures
12. create initial security dashboards and alerts
13. perform focused web, API, RLS, and mobile security testing
14. conduct the first incident tabletop
15. conduct independent penetration testing before material production scale
16. remediate and retest findings
17. publish a coordinated vulnerability-reporting channel
18. exercise cross-tenant, credential-compromise, and provider-compromise scenarios
19. integrate incident recovery with ADR-021
20. review metrics and improve controls continuously
```

## 60. Deferred Decisions

This ADR does not decide:

* Exact OWASP ASVS assurance level
* exact threat-modeling tool
* exact vulnerability-management platform
* exact SIEM or security analytics provider
* exact penetration-testing vendor
* exact bug-bounty program
* exact security contact address
* exact incident-response service provider
* exact forensic tooling
* exact alert thresholds
* exact incident response-time objectives
* exact log-retention periods
* exact breach-notification requirements
* exact cyber-insurance coverage
* exact privacy-officer or security-officer staffing
* exact 24-hour on-call model

These require bounded implementation, legal, operational, and commercial
decisions.

## 61. Supersession Conditions

This ADR should be reviewed when:

* Habitacores begins material production operations.
* regulated financial or escrow activity is introduced.
* enterprise customers require formal security assurance.
* a Security Operations Center is introduced.
* a material security incident occurs.
* a public bug-bounty program is proposed.
* additional jurisdictions impose different incident obligations.
* autonomous AI agents receive tool access.
* security monitoring or penetration testing identifies structural gaps.
* the adopted security standards materially change.

## 62. Final Decision Statement

Habitacores will integrate security engineering into product design,
implementation, testing, deployment, operation, incident response, recovery,
and continuous improvement.

Material features and trust-boundary changes will receive threat modeling,
abuse-case analysis, security requirements, named ownership, verification
methods, residual-risk review, and linked implementation work.

Habitacores will maintain a tailored application-security verification
baseline informed by recognized secure-development and application-security
standards. Passing automated scans alone will not constitute security
approval.

Authentication, authorization, tenant isolation, privileged access,
payments, agreements, documents, webhooks, workers, infrastructure,
recovery, mobile clients, and AI retrieval boundaries will receive explicit
negative and adversarial testing.

Independent or appropriately separated penetration testing will occur before
material production scale and after high-risk architectural changes according
to approved scope.

Security findings will be attributable, risk assessed, assigned, time bounded,
remediated, and independently validated where required. Exceptions will
require accountable ownership, compensating controls, approval, expiry, and
monitoring.

Security incidents will be declared and managed through an authorized
Habitacores process with severity, command roles, containment, investigation,
evidence preservation, recovery, communication, and lessons learned.

Containment actions will be narrowly scoped, attributable, audited, monitored,
and reconciled with permanent remediation. Containment will not silently
destroy financial, agreement, audit, or forensic evidence.

Recovery will coordinate with ADR-021 and will include credential review,
tenant-isolation validation, financial reconciliation, agreement-integrity
validation, provider reconciliation, monitoring enhancement, and staged
service restoration.

External incident communication will distinguish confirmed facts,
reasonable assessments, and unresolved uncertainty. Legal and privacy review
will determine applicable breach and notification obligations.

AI may assist with summarization, correlation, drafting, and runbook retrieval.
AI will not declare incidents, determine reportable-breach status, contain
production independently, revoke users, rotate credentials, contact affected
persons, disclose vulnerabilities, declare recovery complete, or close an
incident.

No developer, provider, administrator, worker, support operator, coding agent,
scanner, penetration tester, or AI model may bypass Habitacores
authentication, tenant isolation, authorization, security review, evidence
preservation, incident authority, release governance, recovery controls, or
legally required communication review.