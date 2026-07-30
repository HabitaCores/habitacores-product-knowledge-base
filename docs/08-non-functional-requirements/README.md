# Document 08 — Non-Functional Requirements

## Document Control

| Field | Value |
|---|---|
| Product | Habitacores |
| Document | Non-Functional Requirements |
| Version | 0.1 |
| Status | Draft for approval |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Market | Nigeria |
| Primary Currency | Nigerian naira |
| Primary Operational Time Zone | West Africa Time |
| Last Updated | 30 July 2026 |

## 1. Purpose

This document defines the non-functional requirements for Habitacores.

It establishes the measurable quality attributes, operational constraints, security expectations, resilience targets, usability standards, engineering requirements, and governance controls that must apply across the Habitacores platform.

This document complements Document 07 — Functional Requirements.

Document 07 defines what the system must do.

Document 08 defines how well, how safely, how reliably, and under what operating constraints the system must perform those functions.

The requirements in this document apply to:

- Public marketplace experiences
- Prospect and applicant workflows
- Resident experiences
- Property-organization operations
- Estate-agent workflows
- Legal-professional workflows
- Finance operations
- Maintenance and vendor operations
- Platform administration
- APIs
- Background jobs
- Integrations
- Databases
- File storage
- Notifications
- AI-assisted functions
- Audit systems
- Operational tooling

## 2. Non-Functional Objectives

Habitacores must be designed to:

1. Protect personal, financial, legal, property, and organizational data.
2. Preserve strict multi-organization isolation.
3. Remain usable on mobile devices and constrained Nigerian network conditions.
4. Provide reliable residential-property operations.
5. Preserve financial and legal integrity.
6. Provide verifiable auditability.
7. Scale from the founding-team pilot to a multi-tenant SaaS platform.
8. Remain maintainable by a growing engineering organization.
9. Support controlled change without destabilizing production.
10. Recover from failures without silent data loss.
11. Provide accessible and understandable experiences.
12. Support human accountability for high-impact decisions.
13. Minimize dependence on any single external provider.
14. Provide measurable operational service levels.
15. Avoid premature enterprise complexity while preserving architectural extensibility.

## 3. Requirement Classification

Each non-functional requirement is assigned a priority.

### 3.1 Must Have

Required for MVP launch, data integrity, security, legal integrity, financial integrity, or core operational reliability.

### 3.2 Should Have

Important for production maturity and should be implemented during or shortly after the MVP phase.

### 3.3 Could Have

A valuable enhancement that may be introduced as scale or operational complexity increases.

### 3.4 Deferred

Explicitly outside the initial MVP.

## 4. Requirement Identifier Convention

Non-functional requirements use:

```text
NFR-[DOMAIN]-[NUMBER]
Examples:

NFR-SEC-001
NFR-PRIV-001
NFR-PERF-001
NFR-REL-001
NFR-ACC-001
NFR-OBS-001
NFR-AI-001

Identifiers must remain stable after approval.

Retired identifiers must not be reused.

5. Non-Functional Scope

This document covers:

Security
Privacy
Multi-tenant isolation
Identity and access assurance
Performance
Scalability
Availability
Reliability
Resilience
Backup and recovery
Data integrity
Financial integrity
Auditability
Observability
Logging
Accessibility
Usability
Responsive design
Low-bandwidth operation
Compatibility
Maintainability
Modularity
Testability
Deployability
Configuration management
Data retention
Interoperability
Localization
Compliance readiness
File and document handling
Notification reliability
AI safety and governance
Supportability
Vendor and external-service dependency
Environmental and cost efficiency
Service levels
Operational readiness
6. Security Requirements
NFR-SEC-001 — Secure-by-Default Design

Priority: Must Have

All components must use secure defaults.

A newly created:

User
Organization
Role
Property
Document
Integration
API endpoint
Feature
Storage location

must not receive broader access than explicitly required.

NFR-SEC-002 — Server-Side Authorization

Priority: Must Have

All protected operations must enforce authorization on the server.

Client-side visibility controls may improve user experience but must not be treated as a security boundary.

NFR-SEC-003 — Deny by Default

Priority: Must Have

Access must be denied unless an explicit valid permission, role, scope, relationship, or assignment allows it.

NFR-SEC-004 — Least Privilege

Priority: Must Have

Users, services, integrations, and background jobs must receive only the minimum permissions required.

NFR-SEC-005 — Strong Authentication for Privileged Roles

Priority: Must Have

Privileged roles must support stronger authentication controls, including multi-factor authentication.

Privileged roles include:

Platform Owner
Platform Administrator
Platform Security Administrator
Organization Owner
Organization Administrator
Property Owner Administrator
Finance Reviewer with verification authority
Role Administrator
Sensitive-data auditor
NFR-SEC-006 — Password Protection

Priority: Must Have

Where passwords are used:

Passwords must never be stored in plaintext.
Passwords must be processed using an approved adaptive password-hashing algorithm.
Password-reset tokens must be time limited and single use.
Passwords must not appear in logs or analytics.
NFR-SEC-007 — Session Security

Priority: Must Have

Authenticated sessions must support:

Secure session identifiers
Expiration
Revocation
Idle timeout where appropriate
Device or session visibility
Reauthentication for sensitive actions
Protection against session fixation and hijacking
NFR-SEC-008 — Transport Encryption

Priority: Must Have

All production traffic carrying private, authenticated, financial, legal, or operational information must use encrypted transport.

NFR-SEC-009 — Encryption at Rest

Priority: Must Have

Sensitive production data must be encrypted at rest using approved infrastructure controls.

This applies to:

Databases
Backups
Object storage
Government identification
Payment evidence
Legal documents
Agreement documents
Secrets
NFR-SEC-010 — Secret Management

Priority: Must Have

Secrets must be stored in an approved secret-management mechanism.

Secrets must not appear in:

Source code
Git history
Product documentation
Issue descriptions
Build output
Client-side bundles
Audit logs
Application logs
NFR-SEC-011 — Input Validation

Priority: Must Have

All untrusted input must be validated server side.

Validation must address:

Type
Format
Length
Allowed values
File content
Malicious payloads
Unexpected structure
Resource ownership
NFR-SEC-012 — Output Encoding

Priority: Must Have

User-controlled content must be safely encoded or sanitized before display.

NFR-SEC-013 — Injection Protection

Priority: Must Have

The system must protect against injection attacks affecting:

Database queries
Command execution
Template rendering
Search queries
File paths
External requests
AI prompts
NFR-SEC-014 — Cross-Site Request Protection

Priority: Must Have

State-changing browser requests must use appropriate cross-site request protections.

NFR-SEC-015 — Rate Limiting

Priority: Must Have

Appropriate rate limits must protect:

Authentication
Password reset
Registration
Public inquiries
Inspection requests
File upload
Search
Notification triggers
Payment evidence submission
AI-assisted features
Administrative actions
NFR-SEC-016 — Abuse Protection

Priority: Should Have

Public workflows should include abuse controls appropriate to the risk.

Possible measures include:

Rate limiting
Challenge mechanisms
Email verification
Phone verification
Device signals
Duplicate detection
Moderation queues
NFR-SEC-017 — Secure File Access

Priority: Must Have

Private files must not be accessible through permanent public URLs.

File access must be:

Authorized
Time limited where possible
Scope aware
Auditable for sensitive files
NFR-SEC-018 — Malware Scanning

Priority: Must Have

Uploaded files must be scanned or validated using an approved malicious-content detection process before being treated as trusted.

NFR-SEC-019 — Dependency Security

Priority: Must Have

Software dependencies must be:

Version controlled
Regularly reviewed
Scanned for known vulnerabilities
Updated through a controlled process
Removed when no longer required
NFR-SEC-020 — Security Headers

Priority: Must Have

Production web applications must use appropriate browser security headers.

NFR-SEC-021 — Administrative Action Protection

Priority: Must Have

High-impact administrative actions should require:

Recent authentication
Explicit confirmation
Reason
Audit event
Secondary approval where configured
NFR-SEC-022 — Secure Error Handling

Priority: Must Have

Error responses must not expose:

Stack traces
Database details
Secrets
Internal network information
Inaccessible record existence
Sensitive business logic
NFR-SEC-023 — Security Testing

Priority: Must Have

Security testing must include:

Authorization tests
Cross-organization access tests
Input validation tests
File-upload tests
Session tests
Privilege-escalation tests
Dependency scans
Secret scans
NFR-SEC-024 — Vulnerability Response

Priority: Should Have

Habitacores should maintain a documented process for:

Receiving vulnerability reports
Classifying severity
Assigning ownership
Remediating findings
Verifying fixes
Communicating material impact
NFR-SEC-025 — Production Access Control

Priority: Must Have

Production administrative access must be limited to authorized personnel and must be auditable.

7. Privacy Requirements
NFR-PRIV-001 — Privacy by Design

Priority: Must Have

Privacy requirements must be considered during product design, implementation, testing, and change review.

NFR-PRIV-002 — Data Minimization

Priority: Must Have

Habitacores must collect only information necessary for approved business purposes.

NFR-PRIV-003 — Purpose Limitation

Priority: Must Have

Personal data must not be used for unrelated purposes without a lawful and approved basis.

NFR-PRIV-004 — Sensitive Data Classification

Priority: Must Have

The system must distinguish sensitive information, including:

Government identification
Financial eligibility records
Payment evidence
Legal records
Agreement records
Household information
Authentication data
Private communications
Support-session content
NFR-PRIV-005 — Minimum Necessary Display

Priority: Must Have

Interfaces must display only the minimum sensitive information required for the current task.

NFR-PRIV-006 — Field-Level Protection

Priority: Must Have

Highly sensitive fields must support more restrictive access than the parent record.

NFR-PRIV-007 — Privacy-Safe Notifications

Priority: Must Have

Email, SMS, WhatsApp, and push notifications must not expose unnecessary sensitive details.

NFR-PRIV-008 — Privacy-Safe Logs

Priority: Must Have

Logs must not contain unnecessary:

Identity-document numbers
Passwords
Authentication tokens
Full bank details
Sensitive legal content
Application-document contents
NFR-PRIV-009 — Data Export Control

Priority: Must Have

Exports containing personal or sensitive information must require authorization and audit logging.

NFR-PRIV-010 — Data Subject Requests

Priority: Should Have

The platform should support controlled workflows for requests involving:

Access
Correction
Export
Restriction
Deletion where legally permitted
Consent withdrawal
NFR-PRIV-011 — Retention Limitation

Priority: Must Have

Personal data must not be retained indefinitely without an approved business, legal, security, or contractual reason.

NFR-PRIV-012 — Privacy During Support Access

Priority: Must Have

Support access must be:

Purpose limited
Time limited
Scope limited
Audited
Revoked when complete
NFR-PRIV-013 — No Religious Inference

Priority: Must Have

Habitacores must not infer a user’s religion, belief, or religious identity from:

Use of the community masjid
Masjid-related notifications
Community activity
Proximity to the facility
Names
Language
NFR-PRIV-014 — No Unauthorized Profiling

Priority: Must Have

Habitacores must not create unauthorized profiles based on sensitive or inferred characteristics.

NFR-PRIV-015 — Privacy Review for New Features

Priority: Must Have

Features involving new personal-data collection or new use of sensitive information require privacy-impact review.

8. Multi-Tenant Isolation Requirements
NFR-TEN-001 — Organization Isolation

Priority: Must Have

Organization-owned data must remain isolated from unrelated organizations.

NFR-TEN-002 — Explicit Organization Context

Priority: Must Have

Every organization-owned operation must execute within an explicit organization context.

NFR-TEN-003 — Tenant-Aware Queries

Priority: Must Have

Database and service queries must include tenant or organization scoping where required.

NFR-TEN-004 — No Client-Supplied Trust

Priority: Must Have

The platform must not trust a client-supplied organization identifier without validating the user’s authority within that organization.

NFR-TEN-005 — Cross-Tenant Relationship Validation

Priority: Must Have

Cross-organization relationships must be explicitly represented and authorized.

NFR-TEN-006 — Cross-Tenant Test Coverage

Priority: Must Have

Automated tests must verify that users cannot:

Read
Search
Update
Delete
Export
Reference
Attach documents to

records belonging to unrelated organizations.

NFR-TEN-007 — Tenant-Safe Caching

Priority: Must Have

Caches must not return one organization’s data to another organization.

NFR-TEN-008 — Tenant-Safe Background Jobs

Priority: Must Have

Background jobs must preserve organization context and must not process unrelated tenant records together without explicit platform authority.

NFR-TEN-009 — Tenant-Safe Analytics

Priority: Must Have

Organization dashboards and reports must use tenant-scoped data.

NFR-TEN-010 — Platform Aggregate Data

Priority: Should Have

Platform-wide aggregate reporting should minimize exposure of organization-identifiable information unless required and authorized.

NFR-TEN-011 — Tenant-Safe AI

Priority: Must Have

AI-assisted workflows must not mix information from unrelated organizations.

NFR-TEN-012 — Tenant-Safe File Storage

Priority: Must Have

File paths, metadata, and access rules must preserve organization isolation.

9. Identity and Access Assurance
NFR-IAM-001 — Stable User Identity

Priority: Must Have

A user must have one stable platform identity independent of organization membership.

NFR-IAM-002 — Membership Independence

Priority: Must Have

Revoking one organization membership must not automatically disable the user’s valid memberships in other organizations.

NFR-IAM-003 — Active Context Visibility

Priority: Must Have

The interface must clearly display the active organization and relevant role or scope.

NFR-IAM-004 — Context Switching Safety

Priority: Must Have

Switching organizations must clear or revalidate:

Cached data
Navigation context
Selected records
Search results
Authorization state
NFR-IAM-005 — Privileged Access Review

Priority: Should Have

Privileged roles should be periodically reviewed.

NFR-IAM-006 — Inactive Account Review

Priority: Should Have

Organizations should be able to identify inactive members and stale access.

NFR-IAM-007 — Timely Revocation

Priority: Must Have

Suspended or revoked roles and memberships must lose operational access promptly.

NFR-IAM-008 — Assignment Expiration

Priority: Must Have

Temporary assignments must support effective start and end dates.

NFR-IAM-009 — Support Impersonation Visibility

Priority: Must Have if impersonation is implemented

A support impersonation session must be clearly visible to the support user and fully audited.

NFR-IAM-010 — No Shared Privileged Accounts

Priority: Must Have

Privileged operational access must use attributable user identities rather than shared accounts.

10. Performance Requirements
NFR-PERF-001 — Core Page Response Target

Priority: Must Have

Under normal operating conditions, primary authenticated pages should begin rendering useful content within approximately three seconds for the majority of requests on supported broadband or mobile connections.

NFR-PERF-002 — Public Listing Response Target

Priority: Must Have

Public listing search and listing-detail pages should provide useful content within approximately three seconds under normal conditions.

NFR-PERF-003 — API Response Target

Priority: Should Have

For ordinary synchronous API operations, the platform should target:

Median response below 500 milliseconds
95th percentile below 1.5 seconds

excluding external-provider latency, large exports, file processing, and AI generation.

NFR-PERF-004 — User Feedback for Slow Operations

Priority: Must Have

Operations expected to exceed two seconds should provide visible progress, pending status, or asynchronous completion feedback.

NFR-PERF-005 — Search Performance

Priority: Must Have

Scoped search should return initial results within approximately two seconds under normal MVP load.

NFR-PERF-006 — Dashboard Performance

Priority: Must Have

Operational dashboards should return useful summary data within approximately three seconds under normal conditions.

NFR-PERF-007 — File Upload Feedback

Priority: Must Have

File uploads must show:

Progress
Completion
Validation status
Failure state
NFR-PERF-008 — Background Processing

Priority: Must Have

Long-running activities should use background processing where appropriate.

Examples:

Notification delivery
File scanning
Large imports
Report generation
AI processing
Expiration checks
NFR-PERF-009 — Pagination

Priority: Must Have

Large record sets must use pagination, cursor-based retrieval, or an equivalent bounded-query mechanism.

NFR-PERF-010 — Query Efficiency

Priority: Must Have

Production database queries must avoid unbounded retrieval and known repeated-query inefficiencies.

NFR-PERF-011 — Media Optimization

Priority: Must Have

Images and media should be resized, compressed, and served appropriately for the requesting device.

NFR-PERF-012 — Performance Monitoring

Priority: Should Have

The platform should monitor:

Page load
API latency
Database latency
External-provider latency
Background-job duration
Error rate
NFR-PERF-013 — No Performance Target at Integrity Expense

Priority: Must Have

Performance optimization must not bypass:

Authorization
Audit
Validation
Financial controls
Agreement integrity
Tenant isolation
11. Scalability Requirements
NFR-SCALE-001 — Pilot-to-SaaS Growth

Priority: Must Have

The architecture must support growth from the founding pilot to multiple organizations without redesigning the core domain model.

NFR-SCALE-002 — No Pilot Hardcoding

Priority: Must Have

The following must not be hardcoded as universal limits:

One property
One community
Two buildings
Two blocks
Six units
One estate agent
One lawyer
One vendor
One organization
NFR-SCALE-003 — Horizontal Application Scaling

Priority: Should Have

Stateless application services should support horizontal scaling where practical.

NFR-SCALE-004 — Independent Background Workers

Priority: Should Have

Background processing should be independently scalable from interactive web traffic.

NFR-SCALE-005 — Storage Scalability

Priority: Must Have

Database and object storage must support increasing:

Organizations
Properties
Units
Documents
Media
Audit events
Payments
Communications
NFR-SCALE-006 — Search Scalability

Priority: Should Have

The search architecture should support migration to a specialized search service without changing the business model.

NFR-SCALE-007 — Notification Scalability

Priority: Should Have

Notification processing should support batching, queueing, retry, and rate-control mechanisms.

NFR-SCALE-008 — Tenant Growth Monitoring

Priority: Could Have

The platform may monitor unusually large tenant usage to protect overall service quality.

NFR-SCALE-009 — Data Partitioning Readiness

Priority: Should Have

Data models should preserve future options for partitioning by:

Organization
Region
Time
Record category

without prematurely introducing distributed complexity.

12. Availability Requirements
NFR-AVL-001 — MVP Availability Target

Priority: Must Have

The initial production service should target at least 99.5% monthly availability, excluding approved maintenance windows.

NFR-AVL-002 — Future Availability Target

Priority: Should Have

As the platform matures, core production services should target 99.9% monthly availability.

NFR-AVL-003 — Planned Maintenance

Priority: Must Have

Planned maintenance that materially affects users should be communicated in advance where practical.

NFR-AVL-004 — Health Checks

Priority: Must Have

Production services must provide health checks sufficient to identify:

Application availability
Database connectivity
Queue health
Storage availability
Critical integration status
NFR-AVL-005 — Graceful Degradation

Priority: Must Have

Failure of a non-critical external service should not make unrelated core functionality unavailable.

Examples:

WhatsApp failure should not block agreement access.
AI failure should not block manual application review.
SMS failure should not corrupt payment processing.
Analytics failure should not block move-in.
NFR-AVL-006 — Maintenance Mode

Priority: Should Have

The platform should support controlled maintenance mode for affected functionality.

NFR-AVL-007 — Status Communication

Priority: Should Have

Material service incidents should support status communication to affected users.

13. Reliability Requirements
NFR-REL-001 — Deterministic Business Rules

Priority: Must Have

Mandatory business rules must produce consistent outcomes for equivalent valid inputs.

NFR-REL-002 — Transactional Integrity

Priority: Must Have

Operations that must succeed or fail together should use transactional or compensating controls.

Examples:

Payment reversal and allocation reversal
Move-in activation and occupancy update
Agreement activation and related status update
Role assignment and audit creation
NFR-REL-003 — Idempotent Processing

Priority: Must Have

Retryable operations should be idempotent where duplicate execution could create harm.

Examples:

Payment-provider callbacks
Notification jobs
Imports
Agreement issuance
Refund execution records
Webhooks
NFR-REL-004 — Duplicate Event Protection

Priority: Must Have

The system must safely handle duplicate external events.

NFR-REL-005 — Retry Policy

Priority: Must Have

Transient failures must use bounded retries with appropriate delay.

NFR-REL-006 — Dead-Letter Handling

Priority: Should Have

Repeatedly failing background jobs should be isolated for investigation rather than retried indefinitely.

NFR-REL-007 — Partial Failure Visibility

Priority: Must Have

Partial completion must be visible and must not be represented as complete success.

NFR-REL-008 — External Confirmation Status

Priority: Must Have

Actions awaiting external confirmation must remain pending.

NFR-REL-009 — Referential Integrity

Priority: Must Have

Critical relationships must not reference missing or unauthorized records.

NFR-REL-010 — Status Transition Integrity

Priority: Must Have

Invalid lifecycle transitions must be blocked.

NFR-REL-011 — Concurrency Protection

Priority: Must Have

The platform must protect against conflicting concurrent updates to critical records.

Examples:

Two users approving the same application differently
Two allocations exceeding one payment
Two occupancies activating for one unit
Two agreement versions becoming current
Two users issuing the same refund
14. Resilience Requirements
NFR-RESIL-001 — Failure Isolation

Priority: Must Have

Failures should be isolated to the affected service or workflow where practical.

NFR-RESIL-002 — Timeout Controls

Priority: Must Have

External service calls must use appropriate timeouts.

NFR-RESIL-003 — Circuit Protection

Priority: Should Have

Repeated external-service failures should trigger controlled circuit-breaking or equivalent protective behaviour.

NFR-RESIL-004 — Queue Durability

Priority: Must Have

Critical background jobs must use durable queueing or an equivalent recoverable process.

NFR-RESIL-005 — Retry Safety

Priority: Must Have

Retries must not create duplicate charges, allocations, agreements, notifications, or refunds.

NFR-RESIL-006 — Degraded Operation

Priority: Must Have

Manual workflows must remain available where AI or optional automation is unavailable.

NFR-RESIL-007 — Dependency Failure Documentation

Priority: Should Have

Critical integration dependencies should have documented failure behaviour and recovery procedures.

15. Backup and Recovery Requirements
NFR-BACKUP-001 — Automated Backups

Priority: Must Have

Production databases must have automated backups.

NFR-BACKUP-002 — Backup Encryption

Priority: Must Have

Backups containing sensitive information must be encrypted.

NFR-BACKUP-003 — Backup Retention

Priority: Must Have

Backup retention periods must be documented and configurable according to operational and legal requirements.

NFR-BACKUP-004 — Recovery Point Objective

Priority: Must Have

The MVP should target a recovery point objective of no more than 24 hours.

For financial, agreement, and occupancy records, the architecture should support a future target of one hour or less.

NFR-BACKUP-005 — Recovery Time Objective

Priority: Must Have

The MVP should target restoration of critical service within 24 hours after a severe recoverable failure.

A future mature target should be four hours or less for core operations.

NFR-BACKUP-006 — Backup Restoration Testing

Priority: Should Have

Backup restoration must be tested periodically.

NFR-BACKUP-007 — File Backup Strategy

Priority: Must Have

Critical documents and media must be included in the recovery strategy.

NFR-BACKUP-008 — Configuration Recovery

Priority: Must Have

Production configuration required for service restoration must be recoverable without relying on undocumented individual knowledge.

NFR-BACKUP-009 — Recovery Runbook

Priority: Should Have

The platform should maintain a documented recovery runbook.

16. Data Integrity Requirements
NFR-DATA-001 — Authoritative Records

Priority: Must Have

Each domain must have a defined authoritative record.

Examples:

Habitacores agreement status
Habitacores payment-verification status
Habitacores occupancy status
Habitacores role assignment
Habitacores maintenance status

External communication channels must not become the sole authoritative record.

NFR-DATA-002 — Immutable Signed Agreements

Priority: Must Have

Signed agreement versions must remain immutable.

NFR-DATA-003 — Historical Preservation

Priority: Must Have

Critical historical changes must remain traceable.

NFR-DATA-004 — Controlled Deletion

Priority: Must Have

Critical business records must not be hard deleted through ordinary workflows.

NFR-DATA-005 — Soft Deletion or Archival

Priority: Must Have

Where deletion is operationally needed, the system should use controlled archival, deactivation, anonymization, or soft deletion as appropriate.

NFR-DATA-006 — Permanent Identifier Stability

Priority: Must Have

Permanent identifiers must remain stable when display names or public labels change.

NFR-DATA-007 — Time Accuracy

Priority: Must Have

Critical events must record accurate timestamps and relevant time-zone context.

NFR-DATA-008 — Currency Accuracy

Priority: Must Have

Financial records must preserve currency and must not silently mix currencies.

NFR-DATA-009 — Decimal Accuracy

Priority: Must Have

Financial amounts must use precise decimal representation rather than floating-point approximation.

NFR-DATA-010 — Data Migration Verification

Priority: Must Have

Production data migrations must include validation and rollback or recovery planning.

NFR-DATA-011 — Import Integrity

Priority: Must Have

Bulk imports must provide:

Validation
Preview
Error reporting
Audit record
Duplicate handling
Organization-scoping validation
NFR-DATA-012 — No Silent Normalization of Unknown Facts

Priority: Must Have

Unknown or provisional facts must remain distinguishable from confirmed values.

17. Financial Integrity Requirements
NFR-FIN-001 — Separation of Financial Concepts

Priority: Must Have

The platform must maintain distinct records for:

Charge
Payment
Allocation
Deposit
Credit
Adjustment
Reversal
Refund
Revenue recognition where later supported
NFR-FIN-002 — Immutable Financial History

Priority: Must Have

Verified or settled financial records must not be silently overwritten.

NFR-FIN-003 — Allocation Accuracy

Priority: Must Have

Payment allocations must not exceed:

Valid payment amount
Remaining unallocated amount
Outstanding charge amount unless approved overpayment handling applies
NFR-FIN-004 — Currency Consistency

Priority: Must Have

Allocations must use the same currency as the related payment and charge in the MVP.

NFR-FIN-005 — Financial Authorization

Priority: Must Have

Verification, reversal, adjustment, waiver, and refund actions must require authorized roles.

NFR-FIN-006 — Financial Audit Trail

Priority: Must Have

Financial actions must record:

Actor
Time
Amount
Currency
Previous state
New state
Reason
Approval where applicable
NFR-FIN-007 — Duplicate Payment Protection

Priority: Must Have

The platform should identify probable duplicate payments or payment-provider events.

NFR-FIN-008 — External Payment Reconciliation

Priority: Should Have

Payment integrations should support reconciliation between provider records and Habitacores records.

NFR-FIN-009 — No Screenshot Verification

Priority: Must Have

A payment screenshot alone must not produce automatic verified status.

NFR-FIN-010 — Deposit Segregation

Priority: Must Have

Refundable deposits must remain distinguishable from rent and other income categories.

18. Auditability Requirements
NFR-AUD-001 — Material Action Audit

Priority: Must Have

Material actions must create audit records.

NFR-AUD-002 — Audit Event Integrity

Priority: Must Have

Ordinary application users must not modify or delete audit events.

NFR-AUD-003 — Actor Attribution

Priority: Must Have

User-driven events must identify the responsible user.

NFR-AUD-004 — Automated Actor Attribution

Priority: Must Have

Automated events must identify the responsible:

Service
Job
Integration
AI system
Platform component
NFR-AUD-005 — Before-and-After Values

Priority: Must Have

Material data changes should record previous and new values where appropriate.

NFR-AUD-006 — Audit Reason

Priority: Must Have

High-impact decisions must record a reason.

NFR-AUD-007 — Correlation Identifier

Priority: Should Have

Related events should share a correlation identifier.

NFR-AUD-008 — Audit Searchability

Priority: Must Have

Authorized users must be able to search audit events by:

Organization
Actor
Resource
Action
Date
Correlation identifier
NFR-AUD-009 — Audit Retention

Priority: Must Have

Audit retention must follow approved legal, operational, and security requirements.

NFR-AUD-010 — Audit Export Protection

Priority: Must Have

Audit exports must respect authorization and sensitivity controls.

NFR-AUD-011 — Clock Consistency

Priority: Must Have

Systems generating related audit events must use synchronized time sources.

NFR-AUD-012 — No Secrets in Audit Events

Priority: Must Have

Audit data must not contain secrets or unnecessary sensitive content.

19. Observability Requirements
NFR-OBS-001 — Production Monitoring

Priority: Must Have

Production services must be monitored for:

Availability
Error rate
Latency
Resource use
Queue backlog
Database health
Storage health
Integration health
NFR-OBS-002 — Structured Logging

Priority: Must Have

Production applications should use structured logs.

NFR-OBS-003 — Request Correlation

Priority: Should Have

Requests spanning multiple services should support correlation identifiers.

NFR-OBS-004 — Error Tracking

Priority: Must Have

Unexpected production errors must be captured in an approved error-tracking system.

NFR-OBS-005 — Alerting

Priority: Must Have

Critical failures must trigger alerts to responsible operators.

NFR-OBS-006 — Alert Severity

Priority: Should Have

Alerts should be classified by severity and urgency.

NFR-OBS-007 — Business Workflow Monitoring

Priority: Should Have

The platform should monitor delayed or failed business workflows, including:

Notification queues
Payment verification
Agreement issuance
Document processing
Renewal reminders
File scanning
NFR-OBS-008 — Dashboard Monitoring

Priority: Should Have

Operational dashboards should show key system-health indicators.

NFR-OBS-009 — Privacy-Safe Telemetry

Priority: Must Have

Telemetry must minimize personal and sensitive data.

NFR-OBS-010 — Production Diagnostic Access

Priority: Must Have

Diagnostic access to production information must be authorized and auditable.

20. Logging Requirements
NFR-LOG-001 — Log Levels

Priority: Must Have

Logs should use consistent severity levels such as:

Debug
Information
Warning
Error
Critical
NFR-LOG-002 — Environment-Specific Logging

Priority: Must Have

Production logging must avoid excessive debug content.

NFR-LOG-003 — Log Retention

Priority: Must Have

Operational log retention must be documented.

NFR-LOG-004 — Log Access Control

Priority: Must Have

Production logs must be accessible only to authorized operational personnel.

NFR-LOG-005 — Log Redaction

Priority: Must Have

Sensitive values must be redacted.

NFR-LOG-006 — No Authentication Secrets

Priority: Must Have

Logs must never contain:

Passwords
Session tokens
API keys
Private keys
Password-reset secrets
One-time authentication codes
NFR-LOG-007 — Useful Failure Context

Priority: Must Have

Errors must include sufficient operational context for investigation without exposing sensitive data.

21. Accessibility Requirements
NFR-ACC-001 — Accessibility Standard

Priority: Must Have

Core user experiences should target WCAG 2.2 Level AA conformance.

NFR-ACC-002 — Keyboard Navigation

Priority: Must Have

Core workflows must be operable using a keyboard.

NFR-ACC-003 — Screen Reader Support

Priority: Must Have

Semantic structure, labels, headings, forms, and status messages must support screen-reader use.

NFR-ACC-004 — Colour Independence

Priority: Must Have

Information must not be communicated by colour alone.

NFR-ACC-005 — Contrast

Priority: Must Have

Text and critical interface elements must meet approved contrast requirements.

NFR-ACC-006 — Form Labels

Priority: Must Have

All form controls must have clear programmatic labels.

NFR-ACC-007 — Error Identification

Priority: Must Have

Validation errors must be:

Clearly identified
Associated with the affected field
Explained in understandable language
Navigable by assistive technology
NFR-ACC-008 — Focus Management

Priority: Must Have

Dialogs, validation, navigation changes, and asynchronous updates must manage keyboard focus appropriately.

NFR-ACC-009 — Text Resizing

Priority: Must Have

Core content must remain usable when text is enlarged.

NFR-ACC-010 — Motion Control

Priority: Should Have

Non-essential motion should respect reduced-motion preferences.

NFR-ACC-011 — Accessible Documents

Priority: Should Have

System-generated documents should use accessible structure where the format permits.

NFR-ACC-012 — Accessibility Testing

Priority: Must Have

Accessibility testing must include automated checks and manual review of critical workflows.

22. Usability Requirements
NFR-USE-001 — Clear Role Context

Priority: Must Have

Users must understand:

Which organization they are using
Which property or community is selected
Which role or assignment applies
Why an action is unavailable
NFR-USE-002 — Consistent Terminology

Priority: Must Have

The interface must use terminology consistent with the Product Knowledge Base.

NFR-USE-003 — Actionable Errors

Priority: Must Have

Error messages must explain what happened and what the user can do next.

NFR-USE-004 — Status Clarity

Priority: Must Have

Business statuses must be understandable and visually distinguishable.

NFR-USE-005 — Confirmation for Destructive Actions

Priority: Must Have

Destructive or high-impact actions must require explicit confirmation.

NFR-USE-006 — Progress Preservation

Priority: Must Have

Long forms, including applications, should preserve valid progress.

NFR-USE-007 — Save and Resume

Priority: Must Have

Applicants must be able to save and resume supported application workflows.

NFR-USE-008 — Required Information Visibility

Priority: Must Have

Users should understand required fields and documents before attempting submission.

NFR-USE-009 — Plain-Language Public Experience

Priority: Must Have

Public, applicant, and resident interfaces must avoid unnecessary legal or technical jargon.

NFR-USE-010 — Professional Detail Where Needed

Priority: Must Have

Professional and administrative interfaces may use domain-specific terminology where precision is required.

NFR-USE-011 — No Misleading Success

Priority: Must Have

The interface must distinguish among:

Completed
Submitted
Pending
Awaiting approval
Failed
Partially completed
NFR-USE-012 — Undo or Recovery

Priority: Should Have

Where safe and practical, users should be able to recover from accidental low-risk actions.

23. Responsive and Mobile Requirements
NFR-MOB-001 — Responsive Layout

Priority: Must Have

Core workflows must function on supported desktop, tablet, and mobile browser sizes.

NFR-MOB-002 — Mobile-First Public Experience

Priority: Must Have

Public listing search, inquiry, and inspection-request workflows must be designed for mobile use.

NFR-MOB-003 — Mobile Applicant Experience

Priority: Must Have

Application workflows must be usable on mobile devices.

NFR-MOB-004 — Mobile Resident Experience

Priority: Must Have

Residents must be able to:

View agreements
View charges
View payments
Submit maintenance requests
View notices

using supported mobile browsers.

NFR-MOB-005 — Mobile Vendor Experience

Priority: Must Have

Vendor workers must be able to:

View assignments
Review location instructions
Update work status
Capture and upload evidence

from mobile devices.

NFR-MOB-006 — Touch Target Size

Priority: Must Have

Interactive elements must be appropriately sized and spaced for touch use.

NFR-MOB-007 — Orientation Support

Priority: Should Have

Core mobile workflows should remain usable in portrait and landscape orientations where appropriate.

NFR-MOB-008 — No Desktop-Only Critical Workflow

Priority: Must Have

No core MVP workflow may require a desktop device without a documented operational exception.

24. Low-Bandwidth Requirements
NFR-NET-001 — Constrained Network Support

Priority: Must Have

Core workflows must remain usable under constrained mobile-network conditions.

NFR-NET-002 — Progressive Loading

Priority: Must Have

Pages should prioritize essential content before non-critical media.

NFR-NET-003 — Media Compression

Priority: Must Have

Images must be compressed and served in appropriate sizes.

NFR-NET-004 — Avoid Unnecessary Transfers

Priority: Must Have

The platform should minimize repeated downloads of unchanged assets and data.

NFR-NET-005 — Upload Resilience

Priority: Should Have

File uploads should support retry or resumable behaviour where technically practical.

NFR-NET-006 — Draft Preservation During Failure

Priority: Must Have

Supported long-form workflows should preserve progress when the network temporarily fails.

NFR-NET-007 — Clear Offline or Failure State

Priority: Must Have

The interface must distinguish network failure from validation or authorization failure.

NFR-NET-008 — Lightweight Notifications

Priority: Should Have

Notification links should lead users directly to relevant, optimized views.

NFR-NET-009 — Optional Media Deferral

Priority: Should Have

Non-essential media should not block access to critical textual information.

25. Browser and Device Compatibility
NFR-COMP-001 — Supported Browsers

Priority: Must Have

The platform must support current stable versions of major modern browsers used by the target market.

NFR-COMP-002 — Graceful Unsupported-Browser Handling

Priority: Should Have

Users on unsupported browsers should receive an understandable compatibility message.

NFR-COMP-003 — Device Independence

Priority: Must Have

Core functionality must not depend on one device manufacturer or operating system.

NFR-COMP-004 — File Compatibility

Priority: Must Have

Supported uploads and downloads must use documented file formats.

NFR-COMP-005 — Time-Zone Compatibility

Priority: Must Have

Dates and times must be interpreted in the applicable organization or property time zone.

26. Maintainability Requirements
NFR-MAINT-001 — Modular Architecture

Priority: Must Have

The application should be organized into clear domain modules.

Possible modules include:

Identity
Organizations
Properties
Listings
Applications
Finance
Agreements
Occupancy
Maintenance
Documents
Notifications
Audit
NFR-MAINT-002 — Separation of Concerns

Priority: Must Have

Business logic, persistence, authorization, presentation, and integration concerns should remain appropriately separated.

NFR-MAINT-003 — Centralized Business Rules

Priority: Must Have

Critical business rules must not be duplicated inconsistently across controllers, pages, background jobs, and client code.

NFR-MAINT-004 — Typed Interfaces

Priority: Must Have

Where supported by the implementation language, internal and external interfaces should use strong typing.

NFR-MAINT-005 — Code Quality Checks

Priority: Must Have

The codebase must use automated:

Formatting
Linting
Type checking
Testing
Build validation
NFR-MAINT-006 — Documentation

Priority: Must Have

Material modules, workflows, integrations, and operational procedures must be documented.

NFR-MAINT-007 — Architectural Decision Records

Priority: Should Have

Material technical decisions should be recorded using architectural decision records or an equivalent process.

NFR-MAINT-008 — Dependency Boundaries

Priority: Must Have

Domain modules should avoid uncontrolled circular dependencies.

NFR-MAINT-009 — Configuration over Hardcoding

Priority: Must Have

Organization-specific rules should use approved configuration rather than tenant-specific source-code branches.

NFR-MAINT-010 — Deprecation Process

Priority: Should Have

Deprecated APIs, fields, and workflows should use a documented transition process.

NFR-MAINT-011 — Code Review

Priority: Must Have

Production code changes must receive review before merging.

NFR-MAINT-012 — Small Reversible Changes

Priority: Should Have

Engineering changes should be scoped to reduce deployment and rollback risk.

27. Testability Requirements
NFR-TEST-001 — Automated Test Coverage

Priority: Must Have

Critical business rules and workflows must have automated tests.

NFR-TEST-002 — Test Layers

Priority: Must Have

The test strategy should include:

Unit tests
Integration tests
Authorization tests
API tests
End-to-end tests
Data-migration tests
Security checks
NFR-TEST-003 — Tenant Isolation Tests

Priority: Must Have

Cross-organization isolation must be tested automatically.

NFR-TEST-004 — Financial Integrity Tests

Priority: Must Have

Financial tests must cover:

Allocation limits
Partial allocation
Reversal
Refund
Currency validation
Duplicate events
Concurrent processing
NFR-TEST-005 — Lifecycle Transition Tests

Priority: Must Have

Invalid and valid lifecycle transitions must be tested.

NFR-TEST-006 — Authorization Matrix Tests

Priority: Must Have

Permissions must be tested by:

Role
Scope
Assignment
Organization
Record state
NFR-TEST-007 — File Upload Tests

Priority: Must Have

File testing must include:

Invalid type
Excessive size
Corrupt file
Malicious content
Unauthorized access
Expired link
NFR-TEST-008 — Accessibility Tests

Priority: Must Have

Critical workflows must receive accessibility testing.

NFR-TEST-009 — Performance Tests

Priority: Should Have

Critical APIs and high-volume workflows should receive performance testing.

NFR-TEST-010 — Recovery Tests

Priority: Should Have

Backup, restore, retry, and failure-recovery procedures should be tested.

NFR-TEST-011 — Test Data Isolation

Priority: Must Have

Test environments must not rely on unapproved production personal data.

NFR-TEST-012 — Deterministic Tests

Priority: Must Have

Automated tests should produce repeatable results and avoid uncontrolled dependence on live external providers.

28. Deployability Requirements
NFR-DEP-001 — Automated Build

Priority: Must Have

Every proposed production release must pass an automated build.

NFR-DEP-002 — Deployment Environments

Priority: Must Have

The platform should maintain separated environments for:

Local development
Testing
Staging or preview
Production
NFR-DEP-003 — Environment Isolation

Priority: Must Have

Production secrets and data must not be shared with non-production environments without approved controls.

NFR-DEP-004 — Deployment Validation

Priority: Must Have

Deployment must require successful:

Formatting
Linting
Type checking
Tests
Build
Migration validation
Security checks where configured
NFR-DEP-005 — Repeatable Deployment

Priority: Must Have

Deployments must be automated or sufficiently scripted to be repeatable.

NFR-DEP-006 — Rollback or Forward Recovery

Priority: Must Have

Production releases must have a rollback or forward-recovery strategy.

NFR-DEP-007 — Database Migration Safety

Priority: Must Have

Database migrations must be:

Version controlled
Reviewed
Tested
Backward compatible where possible
Recoverable
NFR-DEP-008 — Feature Flags

Priority: Should Have

Risky or incomplete functionality should support controlled feature activation.

NFR-DEP-009 — Deployment Audit

Priority: Should Have

Production deployments should record:

Version
Commit
Time
Initiator
Environment
Result
NFR-DEP-010 — Zero or Low Downtime

Priority: Should Have

Routine production deployments should minimize downtime.

29. Configuration Requirements
NFR-CONF-001 — Externalized Configuration

Priority: Must Have

Environment-specific configuration must be external to source code.

NFR-CONF-002 — Configuration Validation

Priority: Must Have

Invalid or incomplete critical configuration must fail clearly.

NFR-CONF-003 — Configuration History

Priority: Must Have

Material organization configuration changes must retain history.

NFR-CONF-004 — Safe Defaults

Priority: Must Have

Configuration defaults must favor security, privacy, and financial integrity.

NFR-CONF-005 — Scope-Aware Configuration

Priority: Must Have

Configuration may apply at:

Platform
Organization
Property
Community
NFR-CONF-006 — Configuration Inheritance

Priority: Should Have

Lower scopes should inherit higher-scope configuration unless an approved override exists.

NFR-CONF-007 — Configuration Approval

Priority: Should Have

High-risk configuration changes should support approval.

NFR-CONF-008 — No Secret Exposure

Priority: Must Have

Administrative interfaces must not reveal secret values after storage.
30. Data Retention Requirements
NFR-RET-001 — Retention Schedule

Priority: Must Have

Habitacores must maintain a documented retention schedule by record category.

NFR-RET-002 — Agreement Retention

Priority: Must Have

Signed agreements and amendments must be retained according to applicable legal and business requirements.

NFR-RET-003 — Financial Retention

Priority: Must Have

Charges, payments, allocations, refunds, and reversals must be retained according to approved financial-record requirements.

NFR-RET-004 — Application Retention

Priority: Must Have

Applicant records must not be retained longer than justified.

NFR-RET-005 — Identity-Document Retention

Priority: Must Have

Government identification must use a more restrictive retention policy than ordinary profile data where appropriate.

NFR-RET-006 — Audit Retention

Priority: Must Have

Audit records must use a documented retention period.

NFR-RET-007 — Former Resident Retention

Priority: Must Have

Former-resident access and data retention must follow approved policy.

NFR-RET-008 — Legal Hold

Priority: Should Have

Records subject to legal hold must not be deleted through ordinary retention processes.

NFR-RET-009 — Secure Disposal

Priority: Must Have

Expired data must be securely deleted, anonymized, or made inaccessible according to approved policy.

NFR-RET-010 — Backup Retention Alignment

Priority: Must Have

Retention processes must account for backup copies and restoration behaviour.

31. Interoperability Requirements
NFR-INT-001 — API-Based Integration

Priority: Should Have

External integrations should use documented APIs, webhooks, or approved file exchanges.

NFR-INT-002 — Stable External Identifiers

Priority: Must Have

Records exposed to integrations must use stable identifiers.

NFR-INT-003 — Versioned APIs

Priority: Should Have

Public or partner APIs should support versioning.

NFR-INT-004 — Integration Authentication

Priority: Must Have

Integrations must use secure, revocable credentials.

NFR-INT-005 — Integration Scope

Priority: Must Have

Integration credentials must be limited to approved organizations and capabilities.

NFR-INT-006 — Webhook Verification

Priority: Must Have

Inbound webhooks must be authenticated or cryptographically verified where supported.

NFR-INT-007 — Webhook Idempotency

Priority: Must Have

Duplicate webhook delivery must not create duplicate business outcomes.

NFR-INT-008 — Integration Monitoring

Priority: Should Have

Critical integrations should be monitored for failure and delay.

NFR-INT-009 — Provider Abstraction

Priority: Should Have

Critical provider integrations should be isolated behind internal interfaces where practical.

NFR-INT-010 — Export Format

Priority: Must Have

Supported exports must use documented and interoperable formats.

32. Localization Requirements
NFR-LOC-001 — Initial Locale

Priority: Must Have

The initial market configuration must support Nigeria.

NFR-LOC-002 — Currency Display

Priority: Must Have

The initial market must support Nigerian naira display and processing.

NFR-LOC-003 — Time Zone

Priority: Must Have

The initial operational time zone must support West Africa Time.

NFR-LOC-004 — Date Presentation

Priority: Must Have

Date presentation must be understandable to the Nigerian target market and must avoid ambiguity.

NFR-LOC-005 — Address Flexibility

Priority: Must Have

The address model must support Nigerian address formats without requiring a standardized street number for every property.

NFR-LOC-006 — Phone Number Handling

Priority: Must Have

The system must support Nigerian phone-number formats and international representation.

NFR-LOC-007 — Language Readiness

Priority: Should Have

The user-interface architecture should support future translation without rewriting core workflows.

NFR-LOC-008 — Locale-Aware Formatting

Priority: Should Have

Amounts, dates, times, and numbers should use locale-aware formatting.

NFR-LOC-009 — No Hardcoded English Business Logic

Priority: Should Have

Human-readable labels should be separable from permanent system values.

33. Compliance Readiness
NFR-COMP-001 — Compliance-Aware Architecture

Priority: Must Have

The platform must support future legal and regulatory obligations without requiring uncontrolled redesign.

NFR-COMP-002 — Consent Records

Priority: Must Have

Where consent is relied upon, the platform must record:

User
Purpose
Version
Time
Status
Withdrawal where applicable
NFR-COMP-003 — Data Processing Records

Priority: Should Have

Habitacores should maintain records of material personal-data processing activities.

NFR-COMP-004 — Access Traceability

Priority: Must Have

Sensitive access must be attributable to authorized users or systems.

NFR-COMP-005 — Policy Versioning

Priority: Must Have

Terms, privacy notices, community rules, and material policies must support versioning and effective dates.

NFR-COMP-006 — Compliance Export

Priority: Should Have

Authorized users should be able to export approved compliance evidence.

NFR-COMP-007 — No False Compliance Claims

Priority: Must Have

Habitacores must not represent itself, an organization, a property, or a workflow as certified or compliant without verified basis.

NFR-COMP-008 — Jurisdiction Review

Priority: Must Have

Legal, privacy, tenancy, payment, and retention rules must receive jurisdiction-specific review before production activation where required.

34. File and Document Requirements
NFR-FILE-001 — Supported Formats

Priority: Must Have

Supported file formats must be documented and configurable.

NFR-FILE-002 — File-Size Limits

Priority: Must Have

Upload size limits must be enforced.

NFR-FILE-003 — File Integrity

Priority: Must Have

The platform must detect incomplete or corrupt uploads.

NFR-FILE-004 — File Metadata

Priority: Must Have

Uploaded files must record:

Organization
Uploader
Related resource
Type
Sensitivity
Version
Upload time
Validation status
NFR-FILE-005 — Private-by-Default Storage

Priority: Must Have

Uploaded files must be private by default.

NFR-FILE-006 — Signed URL Expiry

Priority: Must Have

Temporary file-access links must expire.

NFR-FILE-007 — Version Preservation

Priority: Must Have

Controlled document replacement must preserve prior versions.

NFR-FILE-008 — Signed Document Immutability

Priority: Must Have

Signed documents must not be overwritten.

NFR-FILE-009 — Government ID Protection

Priority: Must Have

Government identification must receive heightened storage and access controls.

NFR-FILE-010 — Media Derivatives

Priority: Should Have

Listing and property media may use optimized derivatives while preserving an approved source file where required.

NFR-FILE-011 — File Deletion Controls

Priority: Must Have

Deletion must respect:

Retention
Legal hold
Agreement history
Audit requirements
Active workflows
35. Notification Reliability Requirements
NFR-NOT-001 — Queued Delivery

Priority: Must Have

Notifications must be processed through a reliable queued or equivalent asynchronous mechanism.

NFR-NOT-002 — Delivery Status

Priority: Must Have

Where supported, the system must record:

Queued
Sent
Delivered
Failed
Retrying
Cancelled
NFR-NOT-003 — Retry Control

Priority: Must Have

Transient delivery failures must use bounded retry.

NFR-NOT-004 — Duplicate Suppression

Priority: Should Have

The system should prevent unnecessary duplicate notification delivery.

NFR-NOT-005 — Channel Independence

Priority: Must Have

Failure of one channel must not corrupt the underlying workflow.

NFR-NOT-006 — Authoritative Status

Priority: Must Have

A notification must not be treated as proof that the recipient completed the requested business action.

NFR-NOT-007 — Notification Timing

Priority: Must Have

Scheduled notifications must use the relevant organization or property time zone.

NFR-NOT-008 — Delivery Escalation

Priority: Should Have

Critical failed notifications should support escalation.

NFR-NOT-009 — Template Versioning

Priority: Should Have

Material notification templates should support version history.

36. AI Safety and Governance Requirements
NFR-AI-001 — Human Accountability

Priority: Must Have

A responsible human must remain accountable for high-impact decisions.

NFR-AI-002 — Prohibited Autonomous Decisions

Priority: Must Have

AI must not independently finalize:

Applicant approval
Applicant rejection
Agreement termination
Eviction
Payment verification
Refund denial
Ownership verification
Vendor suspension
Legal conclusions
NFR-AI-003 — AI Output Identification

Priority: Must Have

AI-generated or AI-assisted content must be identifiable to relevant users.

NFR-AI-004 — Human Review

Priority: Must Have

High-impact AI-assisted output must require human review before becoming authoritative.

NFR-AI-005 — Tenant Isolation

Priority: Must Have

AI context must remain scoped to authorized organization data.

NFR-AI-006 — Sensitive Data Minimization

Priority: Must Have

AI requests must use the minimum necessary sensitive data.

NFR-AI-007 — Prompt Injection Protection

Priority: Must Have

AI systems processing uploaded or user-controlled content must treat that content as untrusted.

NFR-AI-008 — Tool Authorization

Priority: Must Have

AI agents must not receive broader tool or data access than the initiating user or approved service role.

NFR-AI-009 — No Secret Disclosure

Priority: Must Have

AI prompts and outputs must not expose secrets or unrelated sensitive records.

NFR-AI-010 — AI Failure Degradation

Priority: Must Have

Failure of an AI provider must not prevent manual completion of core workflows.

NFR-AI-011 — AI Audit Record

Priority: Should Have

Material AI-assisted activity should record:

Feature
Model or service
Time
Input reference
Output
Human reviewer
Final action
NFR-AI-012 — Accuracy Communication

Priority: Must Have

AI-generated summaries or suggestions must not be presented as guaranteed factual conclusions.

NFR-AI-013 — Source Grounding

Priority: Should Have

AI output used for material review should reference the authoritative records used as context where practical.

NFR-AI-014 — Model Change Review

Priority: Should Have

Changing an AI provider or model for a material workflow should require evaluation and approval.

NFR-AI-015 — Bias Review

Priority: Must Have

AI-assisted applicant, resident, vendor, or professional workflows must be reviewed for unfair or discriminatory effects.

NFR-AI-016 — Religious Inference Prohibition

Priority: Must Have

AI must not infer religious identity from masjid-related data or community activity.

NFR-AI-017 — AI Retention Control

Priority: Must Have

Sensitive AI input and output retention must follow approved policy.

37. Supportability Requirements
NFR-SUP-001 — Support Reference

Priority: Must Have

Material user-facing failures should provide a support reference or correlation identifier.

NFR-SUP-002 — Support Case Context

Priority: Should Have

Support workflows should allow authorized personnel to identify:

User
Organization
Affected workflow
Error reference
Time
Relevant system events
NFR-SUP-003 — Controlled Support Tools

Priority: Must Have

Support tools must use restricted and audited access.

NFR-SUP-004 — No Default Sensitive Access

Priority: Must Have

Support users must not receive unrestricted access to customer data by default.

NFR-SUP-005 — Operational Runbooks

Priority: Should Have

Runbooks should exist for:

Authentication issues
Notification failures
Payment integration failures
File-processing failures
Data restoration
Tenant-access incidents
Deployment rollback
NFR-SUP-006 — User-Facing Help

Priority: Should Have

Core workflows should provide contextual help and clear escalation paths.

38. External Provider Requirements
NFR-EXT-001 — Provider Inventory

Priority: Must Have

Habitacores must maintain an inventory of critical external providers.

NFR-EXT-002 — Provider Purpose

Priority: Must Have

Each provider must have a documented purpose and approved data scope.

NFR-EXT-003 — Provider Credential Isolation

Priority: Must Have

Provider credentials must be environment specific and securely stored.

NFR-EXT-004 — Provider Failure Handling

Priority: Must Have

Critical provider failures must produce clear pending or failed states.

NFR-EXT-005 — Provider Substitution Readiness

Priority: Should Have

Critical provider integrations should avoid unnecessary lock-in where practical.

NFR-EXT-006 — Provider Data Minimization

Priority: Must Have

External providers must receive only required data.

NFR-EXT-007 — Provider Monitoring

Priority: Should Have

Critical provider performance and errors should be monitored.

NFR-EXT-008 — Provider Change Review

Priority: Should Have

Replacing a critical provider must include:

Security review
Privacy review
Data-migration review
Reliability review
Cost review
Rollback planning
39. Cost and Resource Efficiency
NFR-COST-001 — MVP Cost Discipline

Priority: Must Have

The initial architecture must avoid unnecessary infrastructure complexity and recurring cost.

NFR-COST-002 — Usage Visibility

Priority: Should Have

The platform should monitor material usage and cost drivers, including:

Database
Storage
Bandwidth
Notifications
File processing
AI usage
External integrations
NFR-COST-003 — Bounded AI Usage

Priority: Must Have

AI-assisted features must use configurable limits and must not create uncontrolled provider cost.

NFR-COST-004 — Media Efficiency

Priority: Must Have

Media processing and delivery must use optimized formats and sizes.

NFR-COST-005 — Archive Strategy

Priority: Should Have

Older records and media may use lower-cost storage where access and retention requirements permit.

NFR-COST-006 — Cost Does Not Override Integrity

Priority: Must Have

Cost reduction must not weaken:

Security
Privacy
Tenant isolation
Financial integrity
Agreement preservation
Backup requirements
40. Service-Level Objectives
40.1 Availability

Initial MVP target:

99.5% monthly availability

Future mature target:

99.9% monthly availability
40.2 Interactive Performance

Normal operating target:

Most primary pages provide useful content within approximately 3 seconds.
40.3 API Performance

Target for ordinary synchronous operations:

Median: below 500 ms
95th percentile: below 1.5 seconds

External-provider and long-running operations are excluded.

40.4 Recovery

Initial targets:

Recovery Point Objective: 24 hours or less
Recovery Time Objective: 24 hours or less

Future critical-service targets:

Recovery Point Objective: 1 hour or less
Recovery Time Objective: 4 hours or less
40.5 Notification Processing

Under normal provider conditions:

Operational notifications should enter delivery processing within 5 minutes.

Critical notifications may require tighter future targets.

40.6 Support Access Revocation

Temporary support access should expire automatically at the approved time and must be revocable immediately.

40.7 Financial Processing Integrity

No performance or availability objective may permit:

Duplicate payment allocation
Silent payment loss
Untraceable reversal
Unapproved refund
Cross-organization financial exposure
41. Operational Readiness Requirements
NFR-OPS-001 — Production Readiness Review

Priority: Must Have

A production release must receive readiness review appropriate to its risk.

NFR-OPS-002 — Readiness Checklist

Priority: Must Have

Production readiness should verify:

Build success
Test success
Security checks
Migration safety
Backup availability
Monitoring
Alerting
Rollback plan
Documentation
Support ownership
NFR-OPS-003 — Named Operational Owner

Priority: Must Have

Every production service and critical integration must have an operational owner.

NFR-OPS-004 — Incident Classification

Priority: Should Have

Incidents should be classified by severity.

NFR-OPS-005 — Incident Timeline

Priority: Should Have

Material incidents should retain a timeline of:

Detection
Impact
Response
Mitigation
Recovery
Follow-up
NFR-OPS-006 — Post-Incident Review

Priority: Should Have

Material incidents should receive a blameless post-incident review.

NFR-OPS-007 — Change Control

Priority: Must Have

High-risk production changes must follow controlled approval and deployment processes.

NFR-OPS-008 — Emergency Change

Priority: Must Have

Emergency changes must still be documented and reviewed after implementation.

42. MVP Non-Functional Baseline

The initial MVP must meet the following baseline.

42.1 Security
Server-side authorization
Deny-by-default access
Organization isolation
Secure authentication
Multi-factor authentication for privileged roles
Secure secret storage
Encrypted transport
Protected file access
Input validation
Audit logging
42.2 Reliability
Valid status transitions
Transactional financial operations
Idempotent external callbacks
Durable notification processing
Visible partial failures
Safe retry behaviour
42.3 Performance
Mobile-responsive core workflows
Useful page response within approximately three seconds under normal conditions
Pagination for large record sets
Optimized media
Background processing for slow tasks
42.4 Recovery
Automated database backups
Document and media recovery strategy
Documented initial recovery targets
Restoration testing plan
42.5 Usability and Accessibility
Responsive browser support
Keyboard-accessible core workflows
Clear form labels and errors
Plain-language applicant and resident experiences
Low-bandwidth consideration
No desktop-only core workflow
42.6 Engineering Quality
Formatting
Linting
Type checking
Automated testing
Build validation
Reviewed database migrations
Production monitoring
Controlled deployment
42.7 AI Governance
Human approval for high-impact decisions
Tenant-scoped AI context
Sensitive-data minimization
AI output identification
Manual fallback
Prohibited autonomous decisions
43. Future Non-Functional Targets

Future maturity may include:

99.9% or higher service availability
Multi-region disaster recovery
One-hour or lower recovery point objective
Four-hour or lower recovery time objective
Advanced security monitoring
Dedicated security operations
Independent penetration testing
Formal compliance certifications
Specialized search infrastructure
Distributed queueing
Regional data controls
Advanced tenant resource isolation
Native mobile performance targets
Advanced offline operation
Predictive scaling
Formal vendor-risk management
Dedicated data-governance team
Automated compliance evidence collection

These are not MVP commitments unless separately approved.

44. Pilot-Specific Non-Functional Context

The initial pilot must support:

Property
└── Community 001
    ├── Existing Building
    │   ├── Block A
    │   │   └── Six confirmed residential units
    │   └── Block B
    ├── Functional Masjid
    └── Building Under Construction
        └── Waterside context

The implementation must not assume:

Block B has a confirmed unit count
The second building has a confirmed unit count
Floors are confirmed
Public building names are final
The water feature has a confirmed geographic classification

Pilot data must remain isolated from future customer organizations.

45. Non-Functional Acceptance Criteria

Every material non-functional requirement should eventually have measurable acceptance criteria.

Example:

Requirement:
NFR-TEN-006 — Cross-Tenant Test Coverage

Given:
User A belongs only to Organization A.
Property B belongs to Organization B.

When:
User A requests Property B through the API, search, export, or direct identifier.

Then:
Access is denied.
No Property B details are returned.
The result does not disclose unnecessary record existence.
The denial is logged where appropriate.

Example:

Requirement:
NFR-FIN-003 — Allocation Accuracy

Given:
A verified payment of ₦1,000,000 has ₦400,000 already allocated.

When:
A Finance Reviewer attempts to allocate an additional ₦700,000.

Then:
The allocation is rejected.
The payment remains unchanged.
The user receives a clear validation message.
No partial allocation occurs.
An audit or diagnostic event is recorded where required.

Example:

Requirement:
NFR-AI-002 — Prohibited Autonomous Decisions

Given:
AI has summarized an application.

When:
The AI workflow completes.

Then:
The application remains awaiting human decision.
No approval or rejection status is created automatically.
The reviewer can inspect the summary.
The final decision requires an authorized human.
46. Non-Functional Testing Requirements

Testing must include:

Cross-organization access
Role escalation
Assignment expiration
Authentication and session protection
File access
File validation
Sensitive-data redaction
Performance under expected load
Concurrent financial operations
Duplicate webhook delivery
Notification retry
Backup restoration
Invalid deployment configuration
Database migration failure
Accessibility
Mobile-browser operation
Constrained network simulation
AI-provider failure
External-payment-provider failure
Audit-event integrity
Recovery from partial workflow failure
Log and telemetry privacy
47. Traceability Requirements

Each non-functional requirement should eventually map to:

Product principle
Business rule
Functional requirement
Technical control
Monitoring signal
Test
Runbook
Responsible owner

Example:

Non-Functional Requirement:
NFR-TEN-001 — Organization Isolation

Related Product Principle:
Multi-tenant isolation by design

Related Business Rules:
BR-GEN-001
BR-GEN-002
BR-ROLE-003

Related Functional Requirements:
FR-ORG-007
FR-ACCESS-004
FR-VAL-007

Technical Controls:
Organization-scoped queries
Server-side authorization
Tenant-aware file access
Tenant-safe caching

Tests:
Cross-tenant API test
Cross-tenant search test
Cross-tenant export test
Cross-tenant document-access test
48. Unresolved Non-Functional Decisions

The following remain pending:

Final cloud infrastructure provider
Final hosting region
Final identity provider
Final authentication session architecture
Final multi-factor authentication method
Final production database provider
Final object-storage provider
Final queue and background-job provider
Final email provider
Final SMS provider
Final WhatsApp provider
Final payment provider
Final error-tracking platform
Final logging platform
Final uptime-monitoring platform
Final analytics platform
Exact production availability commitment
Exact production recovery targets
Exact backup retention period
Exact log retention period
Exact audit retention period
Exact applicant-data retention period
Exact government-ID retention period
Exact file-size limits
Exact supported browser versions
Exact performance-test load
Exact notification retry schedule
Exact privileged-session timeout
Exact support-access duration
Exact accessibility verification process
Exact penetration-testing cadence
Exact vulnerability-remediation targets
Exact incident-severity definitions
Exact service-status communication process
Exact AI provider and model controls
Exact AI input and output retention
Exact cost and usage thresholds

These decisions must be resolved before the affected production controls are activated.

49. Non-Functional Requirement Governance

Changes to approved non-functional requirements require:

Requirement identifier
Proposed change
Business rationale
Affected functional requirements
Security impact
Privacy impact
Tenant-isolation impact
Financial-integrity impact
Availability impact
Performance impact
Recovery impact
Accessibility impact
Operational impact
Cost impact
Test impact
Migration impact
Product Visionary approval
Chief Product Architect approval
Effective version
Transition plan where required
50. Current Non-Functional Requirements Decision

Habitacores will be built as a secure, privacy-aware, auditable, responsive, resilient, and maintainable multi-tenant SaaS platform.

The quality model is:

Security
→ Privacy
→ Tenant Isolation
→ Data Integrity
→ Financial Integrity
→ Reliability
→ Availability
→ Performance
→ Accessibility
→ Maintainability
→ Operational Readiness

The initial MVP must prioritize:

Organization isolation
Server-side authorization
Financial correctness
Agreement preservation
Auditability
Mobile usability
Low-bandwidth operation
Human accountability
Secure file handling
Reliable notifications
Recoverable infrastructure
Maintainable implementation
Measurable service quality

No performance, convenience, automation, cost reduction, or delivery deadline may override an approved security, privacy, tenant-isolation, financial-integrity, or legal-integrity requirement.