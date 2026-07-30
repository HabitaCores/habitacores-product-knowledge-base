# Document 10 — Data Architecture and Database Design

## Document Control

| Field | Value |
|---|---|
| Product | Habitacores |
| Document | Data Architecture and Database Design |
| Version | 0.1 |
| Status | Draft for approval |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Market | Nigeria |
| Primary Currency | Nigerian naira |
| Primary Operational Time Zone | West Africa Time |
| Proposed Database | PostgreSQL |
| Proposed Managed Data Platform | Supabase |
| Last Updated | 30 July 2026 |

## 1. Purpose

This document defines the proposed data architecture and logical database design for Habitacores.

It translates the approved:

- Business architecture
- Domain model
- User and role model
- Business rules
- Functional requirements
- Non-functional requirements
- System architecture

into a structured data model suitable for implementation in PostgreSQL.

This document defines:

- Data-design principles
- Tenant-scoping strategy
- Core entities
- Aggregate boundaries
- Table ownership
- Identifiers
- Business keys
- Relationship patterns
- Financial data modeling
- Agreement and occupancy history
- Status-history modeling
- Document metadata
- Audit-event storage
- Soft deletion and archival
- Constraints
- Indexing
- Row Level Security
- Transaction boundaries
- Migration strategy
- Reference data
- Reporting views
- Retention readiness
- Partitioning readiness
- MVP schema scope
- Future schema extensions

This document is a logical and architectural specification.

It is not yet the final executable SQL schema.

The production schema will be implemented through reviewed database migrations and validated against this document.

## 2. Data Architecture Objectives

The Habitacores data architecture must:

1. Preserve strict organization-level tenant isolation.
2. Represent the complete property hierarchy.
3. Support the Nigerian annual-rent lifecycle.
4. Separate identity from organization membership.
5. Separate roles from assignments and permissions.
6. Preserve financial correctness.
7. Preserve signed agreement history.
8. Prevent incompatible occupancy conflicts.
9. Maintain traceable status transitions.
10. Support controlled documents and file metadata.
11. Preserve auditability.
12. Avoid silent overwriting of high-impact records.
13. Support the founding pilot without hardcoding it.
14. Support future multi-organization SaaS growth.
15. Remain understandable, testable, and maintainable.
16. Support future APIs, reporting, analytics, and integrations.
17. Preserve unknown, pending, provisional, and confirmed facts distinctly.
18. Support incremental schema evolution through migrations.

## 3. Database Technology Decision

Habitacores will use PostgreSQL as the proposed system-of-record database.

PostgreSQL is suitable because it supports:

- Relational integrity
- Transactions
- Foreign keys
- Check constraints
- Unique constraints
- Precise numeric types
- JSON support where appropriate
- Full-text search
- Row Level Security
- Views
- Materialized views
- Advisory and row-level locking
- Indexing
- Partitioning
- Mature migration tooling
- Strong operational support

Supabase-managed PostgreSQL remains the proposed managed platform, subject to formal Architecture Decision Records.

## 4. Data Architecture Principles

### 4.1 Relational First

Core business records should use normalized relational structures.

JSON should not replace relationships that require:

- Referential integrity
- Queryability
- Authorization
- Reporting
- Constraints
- Status validation

### 4.2 Organization Scope by Design

Tenant-owned records must contain or inherit an organization scope.

### 4.3 Stable Permanent Identifiers

Permanent identifiers must remain unchanged when display names or public labels change.

### 4.4 Explicit Business Keys

Where users or external systems need human-readable references, separate business keys should be used.

### 4.5 Historical Traceability

Critical records must preserve history through:

- Immutable versions
- Status-history tables
- Audit events
- Financial event records
- Archived relationships

### 4.6 No Silent Destructive Updates

High-impact records must not be silently overwritten or deleted.

### 4.7 Precise Financial Representation

Money must use exact numeric or minor-unit representation.

Floating-point types must not be used for authoritative financial calculations.

### 4.8 Explicit Lifecycle State

Business lifecycle state must be represented using approved statuses and validated transitions.

### 4.9 Unknown Is Not Zero

Unknown values must not be stored as zero, false, empty string, or an invented default unless that meaning is accurate.

### 4.10 Configuration over Tenant-Specific Schema

Tenant-specific differences should use configuration rather than separate table structures or source-code branches.

## 5. Naming Conventions

### 5.1 Table Names

Table names should use:

```text
snake_case
plural nouns
Examples:
organizations
organization_memberships
properties
communities
buildings
units
applications
payments
payment_allocations
audit_events
5.2 Column Names
Column names should use:
snake_case
Examples:
organization_id
created_at
effective_from
verification_status
5.3 Primary Keys
Primary keys should use:
id
5.4 Foreign Keys
Foreign keys should use:
<singular_entity>_id
Examples:
organization_id
property_id
community_id
unit_id
application_id
5.5 Timestamps
Standard timestamps should use:
created_at
updated_at
archived_at
deleted_at
effective_from
effective_to
5.6 Boolean Columns
Boolean names should read naturally.
Preferred:
is_primary
is_active
requires_approval
is_refundable
Avoid ambiguous names such as:
flag
status_check
active_value
6. Identifier Strategy
Habitacores should support three identifier layers.
6.1 Internal Identifier
Each principal record should use a globally unique internal identifier.
Proposed type:
UUID
Internal identifiers should:
	•	Be immutable
	•	Not encode business meaning
	•	Not depend on tenant name
	•	Not depend on physical hierarchy
	•	Not change during renaming
6.2 Business Identifier
Selected records should have human-readable business identifiers.
Examples:
HAB-ORG-000001
HAB-PROP-000001
HAB-COM-000001
HAB-UNIT-000001
HAB-LIST-000001
HAB-APP-000001
HAB-PAY-000001
HAB-AGR-000001
HAB-WO-000001
Business identifiers should be:
	•	Stable
	•	Unique within the approved scope
	•	Non-recyclable
	•	Generated centrally or transactionally
6.3 Display Label
Records may also have user-facing names or labels.
Examples:
Community 001
Existing Building
Block A
Unit 01
Display labels may change without changing permanent identifiers.
7. Standard Record Metadata
Most business tables should support:
	•	id
	•	organization_id
	•	created_at
	•	created_by
	•	updated_at
	•	updated_by
	•	archived_at
	•	archived_by
	•	record_version
Not every table requires every field.
Append-only tables may not use updated_at.
Junction tables may use a simplified metadata structure.
8. Tenant and Organization Scoping
The primary tenant boundary is the Organization.
Most operational records must include:
organization_id
Examples:
	•	Properties
	•	Communities
	•	Buildings
	•	Blocks
	•	Units
	•	Listings
	•	Applications
	•	Charges
	•	Payments
	•	Agreements
	•	Occupancies
	•	Maintenance requests
	•	Work orders
	•	Documents
	•	Notifications
	•	Audit events
8.1 Direct Scope
A record is directly scoped when it stores organization_id.
Example:
properties.organization_id
8.2 Inherited Scope
A child record may inherit organization scope from its parent, but should still store organization_id when this improves:
	•	Authorization
	•	Query safety
	•	Indexing
	•	Reporting
	•	Row Level Security
8.3 Scope Consistency
The database must prevent inconsistent relationships.
Example:
A unit belonging to Organization A must not reference a block belonging to Organization B.
8.4 Cross-Organization Relationships
Cross-organization relationships must be represented explicitly.
Examples:
	•	Property organization and estate agency
	•	Property organization and legal firm
	•	Property organization and vendor
	•	Platform organization and customer organization
9. Core Data Domains
The logical schema is divided into the following domains:
	1	Identity
	2	Organizations
	3	Roles and permissions
	4	Property hierarchy
	5	Development
	6	Amenities and facilities
	7	Listings and prospects
	8	Inspections
	9	Applications
	10	Offers
	11	Finance
	12	Agreements
	13	Occupancy
	14	Resident households
	15	Maintenance
	16	Vendors
	17	Legal matters
	18	Documents
	19	Communications
	20	Notifications
	21	Configuration
	22	Audit
	23	AI activity
	24	Reporting
10. Identity Domain
10.1 users
Represents a global Habitacores user identity.
Suggested fields:
id
business_id
email
phone_number
full_name
preferred_name
status
email_verified_at
phone_verified_at
preferred_language
time_zone
last_active_at
created_at
updated_at
The user table must not contain organization-specific roles directly.
10.2 user_profiles
Optional extended profile information.
Suggested fields:
user_id
profile_photo_document_id
preferred_contact_method
locale
created_at
updated_at
10.3 user_identity_verifications
Tracks identity-verification processes.
Suggested fields:
id
user_id
verification_type
provider
status
submitted_at
reviewed_at
reviewed_by
expires_at
reason
created_at
10.4 user_sessions
Authentication providers may manage sessions externally.
If application-level session metadata is stored, it may include:
id
user_id
session_reference
device_label
ip_hash
last_seen_at
expires_at
revoked_at
created_at
11. Organization Domain
11.1 organizations
Suggested fields:
id
business_id
name
legal_name
organization_type_id
country_code
currency_code
time_zone
status
verification_status
primary_contact_user_id
created_at
created_by
updated_at
updated_by
archived_at
11.2 organization_types
Reference table.
Examples:
	•	Platform
	•	Property owner
	•	Property operator
	•	Estate agency
	•	Legal firm
	•	Vendor
	•	Development company
	•	Contractor
11.3 organization_memberships
Connects a user to an organization.
Suggested fields:
id
organization_id
user_id
status
invited_at
invited_by
accepted_at
effective_from
effective_to
suspended_at
suspended_by
revoked_at
revoked_by
revocation_reason
created_at
updated_at
Constraints:
	•	A user should not have duplicate active memberships in the same organization.
	•	Historical memberships must remain traceable.
11.4 organization_relationships
Represents approved relationships between organizations.
Suggested fields:
id
source_organization_id
target_organization_id
relationship_type_id
status
effective_from
effective_to
approved_by
created_at
updated_at
Examples:
	•	Estate agency engagement
	•	Legal services engagement
	•	Vendor engagement
	•	Property-management agreement
	•	Development contractor relationship
11.5 organization_verifications
Suggested fields:
id
organization_id
verification_type
status
submitted_at
reviewed_at
reviewed_by
expires_at
reason
created_at
12. Role and Permission Domain
12.1 roles
Canonical role catalogue.
Suggested fields:
id
code
name
description
category
is_privileged
status
created_at
updated_at
Examples:
organization_owner
property_operator
finance_reviewer
estate_agent
estate_lawyer
primary_resident
vendor_worker
12.2 permissions
Suggested fields:
id
code
name
description
resource_type
action
status
created_at
Examples:
listing.approve
application.review
payment.verify
agreement.activate
maintenance.assign
role.grant
12.3 role_permissions
Junction table.
Suggested fields:
role_id
permission_id
created_at
created_by
12.4 membership_role_assignments
Connects membership to role and scope.
Suggested fields:
id
organization_membership_id
role_id
scope_type
scope_id
status
effective_from
effective_to
assigned_by
approved_by
assignment_reason
revocation_reason
created_at
updated_at
12.5 resource_assignments
Represents assignment-specific access.
Suggested fields:
id
organization_id
user_id
membership_id
assignment_type
resource_type
resource_id
role_id
status
effective_from
effective_to
assigned_by
approved_by
created_at
updated_at
Examples:
	•	Agent assigned to listing
	•	Lawyer assigned to legal matter
	•	Vendor worker assigned to work order
	•	Finance reviewer assigned to payment review
12.6 role_conflicts
Optional configuration table.
Suggested fields:
id
organization_id
role_a_id
role_b_id
resource_type
conflict_policy
requires_secondary_approval
created_at
updated_at
13. Property Hierarchy Domain
The canonical physical hierarchy is:
Organization
→ Property
→ Community
→ Building
→ Block
→ Floor
→ Unit
13.1 properties
Suggested fields:
id
business_id
organization_id
operating_organization_id
operational_label
public_name
description
country_code
state_region
city
address_line_1
address_line_2
location_notes
latitude
longitude
authority_status
verification_status
status
created_at
created_by
updated_at
updated_by
archived_at
13.2 communities
Suggested fields:
id
business_id
organization_id
property_id
operational_label
public_name
description
status
address_notes
created_at
created_by
updated_at
updated_by
archived_at
13.3 buildings
Suggested fields:
id
business_id
organization_id
community_id
operational_label
public_name
building_type_id
description
construction_status
operational_status
readiness_status
waterside_context_status
created_at
created_by
updated_at
updated_by
archived_at
The water feature must not be classified beyond confirmed information.
13.4 blocks
Suggested fields:
id
business_id
organization_id
building_id
code
operational_label
public_name
description
confirmed_unit_count
unit_count_status
construction_status
operational_status
created_at
created_by
updated_at
updated_by
archived_at
confirmed_unit_count must permit null.
For the pilot:
	•	Block A may store confirmed_unit_count = 6.
	•	Block B must retain a pending or unknown count until confirmed.
13.5 floors
Suggested fields:
id
business_id
organization_id
building_id
block_id
code
display_name
sort_order
status
created_at
updated_at
archived_at
A floor relationship may remain optional until the physical structure is confirmed.
13.6 units
Suggested fields:
id
business_id
organization_id
property_id
community_id
building_id
block_id
floor_id
operational_code
public_name
unit_type_id
bedroom_count
bathroom_count
description
construction_status
readiness_status
listing_status
occupancy_status
maintenance_status
status
created_at
created_by
updated_at
updated_by
archived_at
Constraints:
	•	Operational code must be unique within the approved scope.
	•	Unit hierarchy must belong to the same organization.
	•	Floor may be nullable.
	•	Retired units must remain historically queryable.
13.7 unit_types
Reference table.
Examples:
	•	Studio
	•	One-bedroom
	•	Two-bedroom
	•	Three-bedroom
	•	Duplex
	•	Apartment
	•	Shop
	•	Office
	•	Other
The initial MVP may activate only residential types.
13.8 physical_entity_status_history
Generic or domain-specific history table for property hierarchy statuses.
Suggested fields:
id
organization_id
resource_type
resource_id
status_dimension
previous_status
new_status
reason
changed_by
changed_at
correlation_id
14. Development Domain
14.1 developments
Suggested fields:
id
business_id
organization_id
property_id
community_id
name
description
status
planned_start_date
actual_start_date
planned_completion_date
actual_completion_date
created_at
updated_at
14.2 development_phases
Suggested fields:
id
organization_id
development_id
name
sequence_number
status
planned_start_date
planned_end_date
actual_start_date
actual_end_date
created_at
updated_at
14.3 development_milestones
Suggested fields:
id
organization_id
development_id
phase_id
milestone_type
title
description
status
target_date
completed_at
completed_by
created_at
updated_at
14.4 development_progress_updates
Suggested fields:
id
organization_id
development_id
phase_id
progress_percentage
stage
notes
recorded_at
recorded_by
created_at
15. Amenity and Facility Domain
15.1 amenities
Suggested fields:
id
business_id
organization_id
property_id
community_id
building_id
block_id
unit_id
amenity_type_id
name
description
status
operating_hours
restrictions
responsible_organization_id
created_at
updated_at
archived_at
15.2 amenity_types
Reference table.
Examples:
	•	Masjid
	•	Parking
	•	Security post
	•	Water supply
	•	Recreational area
	•	Garden
	•	Community hall
	•	Other
15.3 amenity_status_history
Suggested fields:
id
organization_id
amenity_id
previous_status
new_status
reason
changed_by
changed_at
The functional masjid must be represented operationally without inferring user religion.
16. Listing Domain
16.1 listings
Suggested fields:
id
business_id
organization_id
unit_id
title
description
annual_rent_amount
currency_code
availability_date
status
publication_status
approved_by
approved_at
published_at
paused_at
closed_at
closure_reason
primary_agent_assignment_id
created_at
created_by
updated_at
updated_by
record_version
16.2 listing_versions
Stores material listing-history snapshots.
Suggested fields:
id
organization_id
listing_id
version_number
title
description
annual_rent_amount
currency_code
availability_date
status
snapshot_data
created_at
created_by
16.3 listing_media
Suggested fields:
id
organization_id
listing_id
document_id
media_type
caption
sort_order
is_primary
created_at
16.4 listing_charge_disclosures
Links listing to disclosed charges.
Suggested fields:
id
organization_id
listing_id
charge_type_id
amount
currency_code
refundability
description
is_mandatory
created_at
updated_at
16.5 listing_assignments
Suggested fields:
id
organization_id
listing_id
agent_user_id
agency_organization_id
status
effective_from
effective_to
assigned_by
created_at
17. Prospect and Inquiry Domain
17.1 prospects
Suggested fields:
id
business_id
organization_id
user_id
full_name
email
phone_number
preferred_contact_method
source
status
created_at
updated_at
A prospect may exist before an authenticated user account is created.
17.2 inquiries
Suggested fields:
id
business_id
organization_id
prospect_id
listing_id
unit_id
message
source
status
assigned_user_id
assigned_at
closed_at
closure_reason
created_at
updated_at
17.3 inquiry_notes
Suggested fields:
id
organization_id
inquiry_id
note
visibility
created_by
created_at
17.4 inquiry_status_history
Suggested fields:
id
organization_id
inquiry_id
previous_status
new_status
reason
changed_by
changed_at
18. Inspection Domain
18.1 inspections
Suggested fields:
id
business_id
organization_id
prospect_id
inquiry_id
listing_id
unit_id
assigned_user_id
scheduled_start
scheduled_end
time_zone
status
meeting_instructions
attendance_status
outcome
created_at
created_by
updated_at
updated_by
18.2 inspection_reschedules
Suggested fields:
id
organization_id
inspection_id
previous_start
previous_end
new_start
new_end
reason
requested_by
approved_by
created_at
18.3 inspection_notes
Suggested fields:
id
organization_id
inspection_id
note
visibility
created_by
created_at
19. Application Domain
19.1 applications
Suggested fields:
id
business_id
organization_id
unit_id
listing_id
primary_applicant_user_id
status
submitted_at
withdrawn_at
expired_at
decision_status
decision_reason
decision_at
decision_by
created_at
created_by
updated_at
updated_by
record_version
19.2 application_parties
Represents applicants, co-applicants, guarantors, and household parties.
Suggested fields:
id
organization_id
application_id
user_id
party_type
full_name
email
phone_number
status
created_at
updated_at
Party types may include:
	•	Primary applicant
	•	Co-applicant
	•	Guarantor
	•	Adult household member
	•	Minor dependant
	•	Authorized occupant
19.3 application_household_members
Suggested fields:
id
organization_id
application_id
application_party_id
relationship_to_primary_applicant
date_of_birth
is_minor
occupancy_intent
created_at
updated_at
Sensitive fields must use appropriate protection.
19.4 application_sections
Optional flexible application-section structure.
Suggested fields:
id
organization_id
application_id
section_type
status
completed_at
created_at
updated_at
19.5 application_answers
Suggested fields:
id
organization_id
application_id
question_definition_id
answer_text
answer_number
answer_boolean
answer_date
answer_json
created_at
updated_at
JSON should be used only when a structured relational type is not more appropriate.
19.6 application_information_requests
Suggested fields:
id
organization_id
application_id
requested_by
request_type
message
status
due_at
responded_at
created_at
updated_at
19.7 application_reviews
Suggested fields:
id
organization_id
application_id
reviewer_user_id
review_type
status
summary
started_at
completed_at
created_at
updated_at
19.8 application_decisions
Final decisions should use an append-only record.
Suggested fields:
id
organization_id
application_id
decision
reason_internal
reason_applicant
conditions
decided_by
decided_at
supersedes_decision_id
created_at
19.9 application_status_history
Suggested fields:
id
organization_id
application_id
previous_status
new_status
reason
changed_by
changed_at
correlation_id
20. Application Document Requirements
20.1 document_requirement_definitions
Suggested fields:
id
organization_id
requirement_code
name
description
applies_to_party_type
is_required
expires_after_days
status
created_at
updated_at
20.2 application_document_requirements
Suggested fields:
id
organization_id
application_id
application_party_id
requirement_definition_id
status
document_id
reviewed_by
reviewed_at
rejection_reason
created_at
updated_at
21. Offer Domain
21.1 offers
Suggested fields:
id
business_id
organization_id
application_id
unit_id
status
version_number
annual_rent_amount
currency_code
proposed_start_date
proposed_end_date
payment_deadline
expires_at
conditions
approved_by
approved_at
issued_at
accepted_at
declined_at
created_at
created_by
21.2 offer_charges
Suggested fields:
id
organization_id
offer_id
charge_type_id
amount
currency_code
refundability
recipient_organization_id
description
is_mandatory
created_at
21.3 offer_responses
Suggested fields:
id
organization_id
offer_id
responded_by
response
response_notes
responded_at
created_at
22. Finance Domain
Financial data requires strict separation of concepts.
The principal entities are:
Charge
Payment
Payment Allocation
Adjustment
Credit
Deposit
Refund
Reversal
Receipt
23. Charge Data Model
23.1 charge_types
Reference or organization-configurable table.
Suggested fields:
id
organization_id
code
name
description
default_refundability
default_recipient_type
status
created_at
updated_at
Examples:
	•	Annual rent
	•	Service charge
	•	Agency fee
	•	Legal fee
	•	Deposit
	•	Utility charge
	•	Maintenance charge
	•	Platform fee
23.2 charges
Suggested fields:
id
business_id
organization_id
charge_type_id
payer_user_id
payer_organization_id
recipient_organization_id
application_id
offer_id
agreement_id
occupancy_id
unit_id
amount
currency_code
due_date
refundability
status
description
created_at
created_by
approved_at
approved_by
updated_at
record_version
23.3 charge_adjustments
Append-only adjustment records.
Suggested fields:
id
business_id
organization_id
charge_id
adjustment_type
amount
currency_code
reason
status
requested_by
approved_by
approved_at
created_at
23.4 charge_status_history
Suggested fields:
id
organization_id
charge_id
previous_status
new_status
reason
changed_by
changed_at
24. Payment Data Model
24.1 payments
Suggested fields:
id
business_id
organization_id
payer_user_id
payer_organization_id
amount
currency_code
payment_method_id
provider
provider_reference
bank_reference
payment_date
recorded_at
recorded_by
verification_status
verified_at
verified_by
rejection_reason
reversal_status
created_at
record_version
24.2 payment_methods
Reference table.
Examples:
	•	Bank transfer
	•	Card
	•	Cash
	•	Payment gateway
	•	Other approved method
24.3 payment_evidence
Suggested fields:
id
organization_id
payment_id
document_id
evidence_type
submitted_by
submitted_at
review_status
reviewed_by
reviewed_at
review_notes
24.4 payment_allocations
Append-only financial relationship.
Suggested fields:
id
business_id
organization_id
payment_id
charge_id
amount
currency_code
status
allocated_at
allocated_by
reversed_at
reversed_by
reversal_reason
created_at
Constraints:
	•	Allocation amount must be greater than zero.
	•	Payment and charge currencies must match.
	•	Payment and charge must belong to an authorized organization context.
	•	Active allocations must not exceed the payment amount.
	•	Active allocations must not exceed charge outstanding balance unless overpayment handling applies.
24.5 payment_reversals
Suggested fields:
id
business_id
organization_id
payment_id
reason
status
requested_by
approved_by
executed_by
executed_at
created_at
24.6 refunds
Suggested fields:
id
business_id
organization_id
payment_id
charge_id
deposit_id
amount
currency_code
reason
status
recipient_user_id
recipient_organization_id
requested_by
approved_by
approved_at
executed_by
executed_at
provider_reference
created_at
24.7 deposits
Suggested fields:
id
business_id
organization_id
charge_id
payment_allocation_id
agreement_id
occupancy_id
original_amount
currency_code
status
held_from
released_at
forfeited_amount
refunded_amount
created_at
updated_at
24.8 receipts
Suggested fields:
id
business_id
organization_id
payment_id
receipt_number
issued_at
issued_by
document_id
created_at
25. Financial Balance Calculation
Authoritative balances should be derived from financial records.
Charge outstanding balance may be represented conceptually as:
original charge
+ approved positive adjustments
- approved credits
- active allocations
Payment unallocated balance may be represented as:
verified payment amount
- active allocations
- executed refunds not linked through reversal
Stored balance fields may be used for performance only if:
	•	They are transactionally maintained.
	•	They can be recalculated.
	•	Reconciliation tools exist.
	•	Tests validate consistency.
26. Agreement Domain
26.1 agreement_templates
Suggested fields:
id
organization_id
name
agreement_type
version_number
status
effective_from
effective_to
document_id
created_at
created_by
approved_at
approved_by
26.2 agreements
Represents the agreement identity and lifecycle.
Suggested fields:
id
business_id
organization_id
unit_id
application_id
offer_id
status
current_version_id
start_date
end_date
activated_at
terminated_at
termination_reason
created_at
created_by
updated_at
record_version
26.3 agreement_versions
Immutable after issuance or signature.
Suggested fields:
id
organization_id
agreement_id
version_number
template_id
annual_rent_amount
currency_code
terms_snapshot
status
issued_at
issued_by
approved_at
approved_by
supersedes_version_id
document_id
created_at
26.4 agreement_parties
Suggested fields:
id
organization_id
agreement_id
agreement_version_id
user_id
organization_party_id
party_type
legal_name
signatory_name
status
created_at
26.5 agreement_signatures
Suggested fields:
id
organization_id
agreement_version_id
agreement_party_id
signature_method
status
signed_at
signer_user_id
provider_reference
evidence_document_id
created_at
26.6 agreement_amendments
Suggested fields:
id
business_id
organization_id
agreement_id
source_version_id
replacement_version_id
reason
status
requested_by
approved_by
created_at
26.7 agreement_status_history
Suggested fields:
id
organization_id
agreement_id
previous_status
new_status
reason
changed_by
changed_at
27. Occupancy Domain
27.1 occupancies
Suggested fields:
id
business_id
organization_id
unit_id
agreement_id
primary_resident_user_id
status
planned_start_date
actual_start_date
planned_end_date
actual_end_date
move_in_id
move_out_id
created_at
created_by
updated_at
record_version
Constraints:
	•	Incompatible active occupancies must not overlap for the same unit.
	•	Occupancy start must precede occupancy end.
	•	Occupancy and agreement must reference the same unit unless explicitly approved.
27.2 occupancy_parties
Suggested fields:
id
organization_id
occupancy_id
user_id
party_type
status
effective_from
effective_to
created_at
updated_at
Party types may include:
	•	Primary resident
	•	Co-resident
	•	Adult household member
	•	Minor dependant
	•	Authorized occupant
27.3 occupancy_status_history
Suggested fields:
id
organization_id
occupancy_id
previous_status
new_status
reason
changed_by
changed_at
28. Move-In Domain
28.1 move_ins
Suggested fields:
id
business_id
organization_id
occupancy_id
unit_id
status
scheduled_at
approved_at
approved_by
completed_at
completed_by
resident_acknowledged_at
created_at
updated_at
28.2 move_in_checklist_items
Suggested fields:
id
organization_id
move_in_id
checklist_definition_id
status
notes
completed_by
completed_at
created_at
updated_at
28.3 property_condition_records
Reusable for move-in and move-out.
Suggested fields:
id
business_id
organization_id
unit_id
occupancy_id
inspection_type
recorded_at
recorded_by
resident_acknowledged_at
summary
created_at
28.4 property_condition_items
Suggested fields:
id
organization_id
condition_record_id
area
item
condition_status
description
responsibility
created_at
28.5 access_items
Tracks keys, cards, codes, or devices.
Suggested fields:
id
organization_id
unit_id
occupancy_id
access_item_type
identifier
status
issued_at
issued_by
returned_at
returned_to
created_at
updated_at
29. Renewal Domain
29.1 renewal_cases
Suggested fields:
id
business_id
organization_id
agreement_id
occupancy_id
status
review_due_at
owner_decision
owner_decided_at
owner_decided_by
resident_response
resident_responded_at
created_at
updated_at
29.2 renewal_offers
Suggested fields:
id
business_id
organization_id
renewal_case_id
annual_rent_amount
currency_code
proposed_start_date
proposed_end_date
expires_at
status
approved_by
approved_at
issued_at
created_at
29.3 renewal_reminders
Suggested fields:
id
organization_id
renewal_case_id
reminder_type
scheduled_at
sent_at
status
notification_id
created_at
30. Move-Out Domain
30.1 move_outs
Suggested fields:
id
business_id
organization_id
occupancy_id
agreement_id
unit_id
status
notice_date
requested_move_out_date
actual_move_out_date
reason
final_inspection_id
deposit_decision_id
completed_at
completed_by
created_at
updated_at
30.2 deposit_decisions
Suggested fields:
id
business_id
organization_id
move_out_id
deposit_id
original_amount
deduction_amount
refund_amount
currency_code
decision_reason
status
proposed_by
approved_by
approved_at
dispute_status
created_at
30.3 deposit_deductions
Suggested fields:
id
organization_id
deposit_decision_id
deduction_type
amount
currency_code
description
evidence_document_id
created_at
31. Maintenance Domain
31.1 maintenance_requests
Suggested fields:
id
business_id
organization_id
property_id
community_id
building_id
block_id
unit_id
amenity_id
occupancy_id
reported_by
category_id
priority
description
access_notes
status
assigned_coordinator_id
submitted_at
triaged_at
completed_at
verified_at
closed_at
created_at
updated_at
record_version
31.2 maintenance_categories
Reference or organization-configurable table.
Examples:
	•	Electrical
	•	Plumbing
	•	Structural
	•	Security
	•	Water
	•	General repair
	•	Masjid facility
	•	Other
31.3 maintenance_status_history
Suggested fields:
id
organization_id
maintenance_request_id
previous_status
new_status
reason
changed_by
changed_at
31.4 maintenance_comments
Suggested fields:
id
organization_id
maintenance_request_id
comment
visibility
created_by
created_at
32. Work-Order Domain
32.1 work_orders
Suggested fields:
id
business_id
organization_id
maintenance_request_id
source_type
source_id
title
work_scope
priority
status
scheduled_start
scheduled_end
estimated_cost
approved_cost
currency_code
assigned_vendor_organization_id
assigned_worker_user_id
approved_by
approved_at
completed_at
verified_at
closed_at
created_at
updated_at
record_version
32.2 work_order_assignments
Suggested fields:
id
organization_id
work_order_id
vendor_organization_id
worker_user_id
status
assigned_at
assigned_by
accepted_at
declined_at
decline_reason
effective_from
effective_to
created_at
32.3 work_order_quotes
Suggested fields:
id
business_id
organization_id
work_order_id
vendor_organization_id
amount
currency_code
description
status
submitted_at
approved_at
approved_by
created_at
32.4 work_order_completion_records
Suggested fields:
id
organization_id
work_order_id
completion_notes
parts_used
actual_cost
currency_code
completed_by
completed_at
verification_status
verified_by
verified_at
created_at
33. Vendor Domain
33.1 vendor_profiles
Suggested fields:
id
organization_id
service_categories
coverage_area
verification_status
status
created_at
updated_at
33.2 vendor_compliance_documents
Suggested fields:
id
organization_id
vendor_organization_id
document_type
document_id
issued_at
expires_at
status
reviewed_at
reviewed_by
created_at
33.3 vendor_performance_events
Suggested fields:
id
organization_id
vendor_organization_id
work_order_id
event_type
metric_name
metric_value
notes
recorded_by
recorded_at
created_at
Performance records must be based on documented events.
34. Legal-Matter Domain
34.1 legal_matters
Suggested fields:
id
business_id
organization_id
matter_type
property_id
unit_id
application_id
agreement_id
occupancy_id
status
confidentiality_level
assigned_lawyer_user_id
assigned_legal_organization_id
opened_at
closed_at
outcome
created_at
updated_at
34.2 legal_matter_parties
Suggested fields:
id
organization_id
legal_matter_id
user_id
organization_party_id
party_type
created_at
34.3 legal_matter_notes
Suggested fields:
id
organization_id
legal_matter_id
note
confidentiality_level
created_by
created_at
34.4 legal_notices
Suggested fields:
id
business_id
organization_id
legal_matter_id
notice_type
document_id
issued_by
issued_at
effective_at
delivery_status
created_at
35. Document Domain
Files remain in object storage.
The database stores metadata and authorization context.
35.1 documents
Suggested fields:
id
business_id
organization_id
document_type_id
title
description
sensitivity_level
status
current_version_id
expires_at
retention_policy_id
created_at
created_by
updated_at
archived_at
35.2 document_versions
Suggested fields:
id
organization_id
document_id
version_number
storage_key
file_name
mime_type
file_size
checksum
validation_status
malware_scan_status
uploaded_by
uploaded_at
approved_by
approved_at
supersedes_version_id
is_immutable
created_at
35.3 document_links
Generic relationship between documents and business records.
Suggested fields:
id
organization_id
document_id
resource_type
resource_id
relationship_type
created_at
created_by
35.4 document_access_events
Sensitive file-access events.
Suggested fields:
id
organization_id
document_id
document_version_id
user_id
action
accessed_at
correlation_id
created_at
35.5 document_types
Reference or configurable table.
Examples:
	•	Government ID
	•	Property authority document
	•	Agreement
	•	Payment evidence
	•	Inspection photo
	•	Maintenance evidence
	•	Legal notice
	•	Vendor licence
	•	Receipt
36. Communication Domain
36.1 communication_threads
Suggested fields:
id
business_id
organization_id
resource_type
resource_id
subject
status
created_at
updated_at
36.2 communications
Suggested fields:
id
organization_id
thread_id
sender_user_id
sender_organization_id
channel
direction
recipient_reference
subject
message_body
visibility
status
provider_reference
sent_at
delivered_at
failed_at
created_at
36.3 internal_notes
A generic internal-note model may be used carefully.
Suggested fields:
id
organization_id
resource_type
resource_id
note
visibility
created_by
created_at
updated_at
Generic tables must not weaken authorization or referential integrity.
37. Notification Domain
37.1 notification_templates
Suggested fields:
id
organization_id
code
name
channel
subject_template
body_template
version_number
status
effective_from
effective_to
created_at
approved_at
approved_by
37.2 notifications
Suggested fields:
id
business_id
organization_id
user_id
resource_type
resource_id
template_id
channel
recipient
status
scheduled_at
queued_at
sent_at
delivered_at
failed_at
failure_reason
provider_reference
correlation_id
created_at
37.3 notification_attempts
Suggested fields:
id
organization_id
notification_id
attempt_number
provider
status
started_at
completed_at
failure_reason
provider_reference
created_at
38. Configuration Domain
38.1 configuration_definitions
Suggested fields:
id
code
name
description
value_type
default_value
allowed_scope
is_sensitive
status
created_at
updated_at
38.2 configuration_values
Suggested fields:
id
configuration_definition_id
scope_type
scope_id
organization_id
value_text
value_number
value_boolean
value_json
effective_from
effective_to
status
created_by
approved_by
created_at
updated_at
38.3 configuration_history
Suggested fields:
id
configuration_value_id
previous_value
new_value
changed_by
approved_by
reason
changed_at
39. Audit Domain
39.1 audit_events
Suggested fields:
id
organization_id
actor_user_id
actor_type
actor_reference
action
resource_type
resource_id
previous_values
new_values
reason
source
ip_hash
user_agent_summary
correlation_id
occurred_at
created_at
Audit events should generally be append-only.
39.2 audit-event principles
Audit records must:
	•	Identify the actor
	•	Identify the organization
	•	Identify the resource
	•	Identify the action
	•	Record relevant before-and-after values
	•	Record the reason where required
	•	Avoid secrets
	•	Avoid unnecessary sensitive document content
	•	Resist ordinary modification or deletion
39.3 audit-event partitioning readiness
At scale, audit events may be partitioned by:
	•	Time
	•	Organization
	•	Record category
The MVP may use one indexed table initially.
40. AI Activity Domain
40.1 ai_activity_records
Suggested fields:
id
business_id
organization_id
user_id
feature
resource_type
resource_id
provider
model
prompt_template_version
input_reference
output_text
output_classification
human_review_status
reviewed_by
reviewed_at
final_action_reference
token_usage
estimated_cost
status
created_at
40.2 ai_usage_limits
Suggested fields:
id
organization_id
feature
period_type
usage_limit
cost_limit
status
created_at
updated_at
Sensitive raw prompts should not be stored without an approved purpose.
41. Reference Data
Reference data should be stored in controlled tables rather than embedded inconsistently in source code.
Reference domains may include:
	•	Organization types
	•	Role definitions
	•	Permissions
	•	Unit types
	•	Building types
	•	Amenity types
	•	Charge types
	•	Payment methods
	•	Maintenance categories
	•	Document types
	•	Legal matter types
	•	Status reasons
	•	Relationship types
	•	Notification channels
Reference data should use stable codes.
Example:
code: annual_rent
display_name: Annual Rent
Display names may be localized later.
42. Status Modeling
Status fields should use approved controlled values.
Examples:
Application
draft
submitted
under_review
information_requested
conditionally_approved
approved
rejected
withdrawn
expired
Payment
recorded
pending_verification
verified
rejected
partially_allocated
fully_allocated
reversed
Agreement
draft
under_review
approved
issued
partially_signed
signed
active
expired
terminated
superseded
Occupancy
planned
pending_move_in
active
pending_move_out
ended
cancelled
Maintenance
submitted
triaged
assigned
in_progress
awaiting_parts
awaiting_approval
completed
verified
closed
reopened
cancelled
43. Status Transition Enforcement
Status transitions should be enforced through:
	•	Application services
	•	Database constraints where feasible
	•	Transition tables or policy definitions
	•	Transactional updates
	•	Authorization checks
	•	Audit events
Direct unrestricted updates to critical status fields should be avoided.
Example:
draft
→ submitted
→ under_review
→ approved
The database must not allow a status to become active when required conditions are missing.
44. Soft Deletion, Archival, and Retirement
44.1 Soft Deletion
Soft deletion may be used for low-risk records where recovery is useful.
Suggested field:
deleted_at
44.2 Archival
Archival is preferred for historical business records.
Suggested fields:
archived_at
archived_by
archive_reason
44.3 Retirement
Physical entities such as units may be retired.
Retirement must preserve:
	•	Agreements
	•	Occupancies
	•	Payments
	•	Listings
	•	Maintenance
	•	Documents
	•	Audit events
44.4 Hard Deletion
Hard deletion should be limited to:
	•	Invalid test data
	•	Unpersisted temporary records
	•	Approved privacy deletion
	•	Expired transient data
	•	Records without retention obligations
Hard deletion of high-impact records requires explicit governance.
45. Temporal Data
The architecture should distinguish:
	•	Record creation time
	•	Record update time
	•	Business effective time
	•	Status transition time
	•	External provider event time
	•	Local property time zone
Critical dates should use timezone-aware timestamps.
Dates without times should use date types.
Examples:
	•	Agreement start date: date
	•	Payment verification time: timestamp with time zone
	•	Inspection start: timestamp with time zone
	•	Offer expiry: timestamp with time zone
46. Financial Data Types
Financial amounts should use one of the following approved strategies:
Option A — Numeric
numeric(19, 2)
Option B — Minor Units
bigint
representing kobo.
The final approach will be documented in an ADR.
Regardless of approach:
	•	Currency must be stored explicitly.
	•	Rounding rules must be consistent.
	•	Floating-point types must not be used.
	•	Financial calculations must be tested.
47. Currency Model
The MVP will primarily support:
NGN
Each financial record must store currency.
Cross-currency allocation is not supported in the MVP.
The schema must not assume that all future records will always use NGN.
48. Data Constraints
The database should use constraints for high-value invariants.
Examples:
	•	Positive financial amounts
	•	Valid date order
	•	Unique active unit codes
	•	Unique active memberships
	•	Valid allocation currency
	•	Valid hierarchy ownership
	•	One current agreement version
	•	One active primary resident per occupancy
	•	No incompatible active occupancy overlap
	•	Unique provider event identifiers where available
Constraints must complement application logic rather than replace authorization.
49. Unique Constraint Strategy
Examples of proposed uniqueness:
organizations.business_id
properties.business_id
units.business_id
listings.business_id
applications.business_id
payments.business_id
agreements.business_id
work_orders.business_id
Additional scoped uniqueness:
blocks: unique building_id + code
floors: unique block_id + code
units: unique organization_id + operational_code
roles: unique code
permissions: unique code
Partial unique indexes may be used for active-only uniqueness.
50. Indexing Strategy
Indexes should support:
	•	Tenant-scoped queries
	•	Status queues
	•	Date-based reminders
	•	Public listing search
	•	Relationship traversal
	•	Audit search
	•	Financial reconciliation
	•	Expiry tracking
Common composite-index patterns may include:
organization_id + status
organization_id + created_at
organization_id + resource_id
organization_id + business_id
organization_id + due_date
organization_id + expires_at
unit_id + occupancy status
payment_id + allocation status
Indexing must be guided by measured query patterns.
Over-indexing should be avoided.
51. Row Level Security
PostgreSQL Row Level Security may provide an additional tenant-isolation layer.
Potential RLS context includes:
	•	Current user
	•	Current organization
	•	Membership status
	•	Platform support session
	•	Service role
51.1 RLS Principles
RLS must not be treated as the only authorization layer.
The architecture should combine:
	•	Application authorization
	•	Tenant-aware queries
	•	Database constraints
	•	RLS
	•	Tests
51.2 Service Role Restrictions
Highly privileged database credentials must:
	•	Remain server side
	•	Be limited to trusted services
	•	Never appear in client bundles
	•	Be monitored
	•	Be used only when required
51.3 RLS Test Requirements
Tests must cover:
	•	Same-organization access
	•	Cross-organization denial
	•	Assignment-scoped access
	•	Support access
	•	Revoked membership
	•	Archived organization
	•	Public data access
	•	Sensitive document access
52. Transaction Boundaries
Transactions are required for operations that must succeed or fail together.
52.1 Payment Allocation
Validate payment
→ Validate charge
→ Lock relevant records
→ Create allocation
→ Recalculate balances
→ Update statuses
→ Create audit event
→ Commit
52.2 Payment Reversal
Validate reversal authority
→ Lock payment
→ Reverse active allocations
→ Update charge balances
→ Update payment status
→ Create reversal record
→ Create audit event
→ Commit
52.3 Agreement Activation
Validate approved version
→ Validate signatures
→ Validate required payments
→ Validate unit
→ Activate agreement
→ Create occupancy plan
→ Update statuses
→ Create audit event
→ Commit
52.4 Move-In Completion
Validate agreement
→ Validate occupancy
→ Validate unit readiness
→ Complete checklist
→ Activate occupancy
→ Update unit occupancy status
→ Activate resident role
→ Create audit event
→ Commit
53. Concurrency Control
Concurrency protection may use:
	•	Row locks
	•	Optimistic version columns
	•	Transaction isolation
	•	Unique constraints
	•	Idempotency keys
	•	Advisory locks
High-risk concurrent operations include:
	•	Application decisions
	•	Payment allocations
	•	Refund approvals
	•	Unit reservation
	•	Agreement activation
	•	Occupancy activation
	•	Role elevation
	•	Work-order cost approval
54. Idempotency
Idempotency keys should be supported for operations that may be retried.
Examples:
	•	Payment webhooks
	•	Refund execution
	•	Notification dispatch
	•	Agreement issue request
	•	Import execution
	•	AI job submission
	•	External provider callbacks
Suggested fields:
idempotency_key
request_hash
status
response_reference
expires_at
created_at
55. Data-Access Architecture
The application should use controlled repositories, query services, or data-access functions.
Tenant-scoped methods should require organization context.
Preferred conceptual shape:
getUnit({
  organizationId,
  unitId,
  actorContext,
});
Avoid:
getUnit(unitId);
for organization-owned records unless tenant scope is derived safely.
56. Query Safety
Queries must:
	•	Include organization scope
	•	Enforce authorization
	•	Use bounded results
	•	Avoid untrusted dynamic SQL
	•	Avoid returning sensitive fields unnecessarily
	•	Use pagination
	•	Preserve field-level restrictions
	•	Avoid N+1 query patterns
	•	Use transactions where needed
57. Reporting Views
Reporting may use:
	•	SQL views
	•	Materialized views
	•	Read models
	•	Precomputed summaries
Potential reporting views include:
Unit Availability View
property
community
building
block
unit
readiness
listing status
occupancy status
maintenance status
Financial Position View
charge amount
allocated amount
outstanding amount
payment status
deposit status
refund status
Application Funnel View
listing
inquiry
inspection
application
decision
offer
agreement
occupancy
Renewal Pipeline View
agreement expiry
renewal status
owner decision
resident response
new agreement status
Views must remain tenant scoped.
58. Materialized Views
Materialized views may be used later for:
	•	Occupancy summaries
	•	Financial summaries
	•	Listing funnel analytics
	•	Vendor performance
	•	Maintenance workload
	•	Audit aggregates
Materialized views must not become the sole source of authoritative state.
Refresh strategy must be documented.
59. Search Data Architecture
Initial search may use PostgreSQL full-text and indexed filtering.
Searchable entities may include:
	•	Properties
	•	Communities
	•	Buildings
	•	Units
	•	Listings
	•	Applications
	•	Residents
	•	Agreements
	•	Payments
	•	Maintenance requests
	•	Vendors
	•	Documents
Search must not index or reveal restricted sensitive fields without authorization.
60. Reference and Seed Data
Seed data may include:
	•	Canonical roles
	•	Canonical permissions
	•	Organization types
	•	Unit types
	•	Charge types
	•	Payment methods
	•	Maintenance categories
	•	Document types
	•	Relationship types
	•	Status values
	•	Nigeria country and currency defaults
Seed operations must be:
	•	Idempotent
	•	Version controlled
	•	Environment aware
	•	Safe for existing data
Pilot data must not be included as universal production seed data.
61. Pilot Data Representation
The initial pilot should be entered using normal records.
Property
└── Community 001
    ├── Existing Building
    │   ├── Block A
    │   │   └── Six confirmed residential units
    │   └── Block B
    ├── Functional Masjid
    └── Building Under Construction
        └── Waterside context
The database must preserve the following pending facts as null, pending, or provisional:
	•	Public community name
	•	Public building names
	•	Block B unit count
	•	Second-building unit inventory
	•	Floor structure
	•	Unit numbering
	•	Exact water-feature classification
The schema must not create invented data to satisfy non-null constraints.
62. Migration Strategy
Database changes must use version-controlled migrations.
Each migration should include:
	•	Purpose
	•	Forward change
	•	Data impact
	•	Backward-compatibility assessment
	•	Validation
	•	Rollback or forward-recovery plan
	•	Expected runtime
	•	Locking considerations
	•	Deployment order
62.1 Safe Migration Sequence
Preferred sequence:
Add new structure
→ Deploy compatible application code
→ Backfill data
→ Validate data
→ Switch reads and writes
→ Remove deprecated structure later
62.2 Prohibited Migration Behaviour
Migrations must not:
	•	Delete required history without approval
	•	Convert unknown values into invented values
	•	Remove organization scope
	•	Reassign ownership silently
	•	Rewrite signed agreements
	•	Alter verified payment history
	•	Break active occupancy
	•	Introduce unbounded production locks without review
63. Data Backfill
Backfills must be:
	•	Organization scoped
	•	Idempotent
	•	Observable
	•	Restartable
	•	Validated
	•	Audited when business state changes
Large backfills should run in controlled batches.
64. Data Import
Supported imports should use staging and validation.
Suggested flow:
Upload file
→ Validate file
→ Parse into staging records
→ Show preview and errors
→ Approve import
→ Create or update records
→ Produce import report
→ Create audit event
64.1 import_jobs
Suggested fields:
id
business_id
organization_id
import_type
document_id
status
record_count
valid_count
invalid_count
created_by
approved_by
started_at
completed_at
created_at
64.2 import_rows
Suggested fields:
id
organization_id
import_job_id
row_number
raw_data
normalized_data
validation_status
error_details
target_resource_type
target_resource_id
created_at
65. Data Export
Sensitive exports should record:
id
business_id
organization_id
export_type
filters
status
requested_by
approved_by
document_id
requested_at
completed_at
expires_at
created_at
Export files must use protected access.
66. Retention Architecture
Each record category should map to a retention policy.
66.1 retention_policies
Suggested fields:
id
code
name
record_category
retention_period
retention_action
legal_hold_supported
status
created_at
updated_at
66.2 legal_holds
Suggested fields:
id
organization_id
resource_type
resource_id
reason
status
placed_by
placed_at
released_by
released_at
created_at
Retention execution must account for:
	•	Legal holds
	•	Active agreements
	•	Financial obligations
	•	Open disputes
	•	Audit obligations
	•	Backup lifecycle
67. Partitioning Readiness
The MVP does not require immediate partitioning for most tables.
Potential future partition candidates include:
	•	Audit events
	•	Notifications
	•	Communications
	•	AI activity
	•	Provider events
	•	Status histories
	•	Large file-access histories
Partitioning may use:
	•	Time
	•	Organization
	•	Region
	•	Record category
Partitioning should be introduced only when supported by measured need.
68. Archival Readiness
Older records may move to lower-cost or archival storage where:
	•	Access requirements allow it.
	•	Retention is preserved.
	•	Auditability remains intact.
	•	Restoration is tested.
	•	User access expectations are met.
Signed agreements and financial records must remain retrievable.
69. Backup and Recovery
The database architecture must support:
	•	Automated backups
	•	Point-in-time recovery where available
	•	Encrypted backup storage
	•	Restore testing
	•	Backup retention
	•	Recovery documentation
Recovery must include:
	•	Database records
	•	Object-storage metadata
	•	Document files
	•	Configuration
	•	Migration history
	•	Reference data
70. Data Reconciliation
The platform should support reconciliation for:
	•	Charge totals
	•	Payment totals
	•	Allocation totals
	•	Unallocated payment balances
	•	Deposit balances
	•	Refund totals
	•	Occupancy and unit status
	•	Agreement and occupancy alignment
	•	Document metadata and storage objects
Reconciliation discrepancies must be visible and investigated.
71. Data Quality Rules
The database should support validation for:
	•	Required fields
	•	Valid status
	•	Valid hierarchy
	•	Valid dates
	•	Valid currency
	•	Valid financial amount
	•	Unique business identifiers
	•	Unique active membership
	•	Duplicate provider reference
	•	Duplicate active listing
	•	Conflicting occupancy
	•	Expired role assignment
	•	Missing organization scope
72. Sensitive Data Handling
Sensitive columns may require:
	•	Field-level authorization
	•	Encryption
	•	Hashing
	•	Redaction
	•	Tokenization
	•	Restricted export
	•	Shorter retention
	•	Access-event logging
Examples:
	•	Government ID numbers
	•	Bank references
	•	Applicant income information
	•	Legal notes
	•	Authentication recovery data
	•	Support-session information
73. Data Masking
Non-production environments must not use unapproved production personal data.
Where production-like data is needed, it should be:
	•	Synthetic
	•	Masked
	•	Anonymized
	•	Minimized
Sensitive identifiers must not be copied casually into test environments.
74. Audit and Business History Distinction
Audit history and business history serve different purposes.
Audit History
Answers:
	•	Who changed the record?
	•	When?
	•	From what?
	•	To what?
	•	Why?
Business History
Answers:
	•	Which agreement version was active?
	•	Which role assignment applied?
	•	Which occupancy was active?
	•	Which charge was outstanding?
	•	Which decision superseded another?
Both are required.
Audit events must not be the only mechanism for reconstructing core business state.
75. Event Data
Domain events may be persisted where useful.
75.1 domain_events
Suggested fields:
id
organization_id
event_type
aggregate_type
aggregate_id
event_version
payload
occurred_at
published_at
status
correlation_id
created_at
The MVP may use transactional outbox patterns where external delivery reliability is required.
76. Transactional Outbox
A transactional outbox may support reliable event and notification publishing.
76.1 outbox_events
Suggested fields:
id
organization_id
event_type
aggregate_type
aggregate_id
payload
status
attempt_count
available_at
processed_at
correlation_id
created_at
The outbox event should be created in the same transaction as the authoritative business change.
77. External Provider Events
77.1 provider_events
Suggested fields:
id
provider
provider_event_id
organization_id
event_type
payload_hash
processing_status
received_at
processed_at
failure_reason
created_at
Constraints:
	•	Provider event IDs should be unique where supported.
	•	Duplicate delivery must not create duplicate business outcomes.
	•	Raw payload retention must follow privacy and security policy.
78. Database Access Roles
The database should use separate credentials or roles for:
	•	Migration execution
	•	Application runtime
	•	Background workers
	•	Read-only reporting
	•	Administrative maintenance
	•	Backup and recovery
Privileges should follow least privilege.
Client applications must not receive unrestricted database credentials.
79. Database Observability
The platform should monitor:
	•	Query latency
	•	Error rate
	•	Connection count
	•	Lock waits
	•	Long-running transactions
	•	Storage growth
	•	Index use
	•	Replication or backup health
	•	Failed migrations
	•	RLS denial anomalies
Sensitive query parameters must not be exposed unnecessarily in logs.
80. Database Performance
Performance practices should include:
	•	Appropriate indexes
	•	Bounded queries
	•	Pagination
	•	Query plans
	•	Connection management
	•	Avoidance of repeated queries
	•	Selective eager loading
	•	Read-model optimization
	•	Background aggregation
	•	Media stored outside the database
81. Database Testing
Database testing must include:
	•	Foreign-key enforcement
	•	Unique constraints
	•	Check constraints
	•	Tenant isolation
	•	RLS
	•	Transaction rollback
	•	Concurrent financial updates
	•	Occupancy conflict prevention
	•	Agreement immutability
	•	Migration safety
	•	Seed idempotency
	•	Import validation
	•	Audit creation
82. MVP Data Scope
The initial MVP schema must support:
Identity and Organizations
	•	Users
	•	Organizations
	•	Memberships
	•	Roles
	•	Permissions
	•	Role assignments
	•	Resource assignments
Property Structure
	•	Properties
	•	Communities
	•	Buildings
	•	Blocks
	•	Optional floors
	•	Units
	•	Amenities
	•	Functional masjid
	•	Waterside context
Leasing
	•	Listings
	•	Prospects
	•	Inquiries
	•	Inspections
	•	Applications
	•	Application parties
	•	Reviews
	•	Decisions
	•	Offers
Finance
	•	Charge types
	•	Charges
	•	Payments
	•	Payment evidence
	•	Allocations
	•	Deposits
	•	Reversals
	•	Refunds
	•	Receipts
Agreement and Occupancy
	•	Agreement templates
	•	Agreements
	•	Agreement versions
	•	Agreement parties
	•	Signatures
	•	Occupancies
	•	Occupancy parties
	•	Move-in
	•	Renewal
	•	Move-out
Operations
	•	Maintenance requests
	•	Work orders
	•	Vendors
	•	Documents
	•	Communications
	•	Notifications
	•	Audit events
	•	Configuration
83. Deferred Data Capabilities
The following are deferred:
	•	Property-sale transactions
	•	Mortgage accounts
	•	Rent-financing accounts
	•	Insurance policies
	•	Investment syndication
	•	Full general ledger
	•	Multi-currency settlement
	•	Public vendor marketplace
	•	Government records integration
	•	Smart-building telemetry
	•	Biometric records
	•	Advanced warehouse analytics
	•	Cross-region data replication
	•	Automated applicant scoring
	•	Autonomous decision records
Deferred capabilities must not distort the MVP schema.
84. Schema Evolution Principles
Schema evolution must:
	•	Preserve stable identifiers
	•	Preserve historical records
	•	Avoid tenant-specific branching
	•	Avoid premature generic abstraction
	•	Use migrations
	•	Remain backward compatible where practical
	•	Support data validation
	•	Include rollback or forward recovery
	•	Update documentation
	•	Update tests
	•	Update APIs
85. Avoiding Over-Generalization
Generic resource tables should be used selectively.
Risks of excessive generic modeling include:
	•	Weak foreign keys
	•	Difficult authorization
	•	Poor query performance
	•	Ambiguous ownership
	•	Hard-to-understand relationships
	•	Reduced type safety
Core domains should use explicit relational tables.
Generic models may be appropriate for:
	•	Audit events
	•	Document links
	•	Internal notes
	•	Configuration
	•	Status history
	•	Assignments
where carefully governed.
86. Data Ownership
Each principal table should have one owning domain module.
Examples:
Table
Owning Module
users
Identity
organizations
Organization
properties
Property
listings
Marketplace
applications
Application
charges
Finance
payments
Finance
agreements
Agreement
occupancies
Occupancy
maintenance_requests
Maintenance
documents
Document
audit_events
Audit
Other modules should access owned data through approved services or read interfaces rather than uncontrolled updates.
87. Cross-Domain References
Cross-domain relationships are permitted where business meaning requires them.
Examples:
	•	Application references unit
	•	Charge references offer
	•	Agreement references application
	•	Occupancy references agreement
	•	Maintenance request references occupancy
	•	Legal matter references agreement
Cross-domain updates should occur through explicit workflow services.
88. Data Traceability
Each implemented table should eventually map to:
	•	Domain entity
	•	Business rule
	•	Functional requirement
	•	Authorization rule
	•	API resource
	•	User-interface flow
	•	Audit event
	•	Retention policy
	•	Automated test
Example:
Table:
payment_allocations

Domain Entity:
Payment Allocation

Business Rules:
BR-PAY-005
BR-PAY-006
BR-PAY-007

Functional Requirements:
FR-PAY-007
FR-PAY-008
FR-PAY-009

Authorization:
payment.allocate

Audit:
PaymentAllocated
PaymentAllocationReversed
89. Data Architecture Acceptance Criteria
Example:
Requirement:
Tenant-scoped unit access

Given:
Unit A belongs to Organization A.
User B belongs only to Organization B.

When:
User B requests Unit A by internal identifier, business identifier, search, export, or relationship traversal.

Then:
The request is denied.
No Unit A data is returned.
No related private data is disclosed.
The denial is recorded where appropriate.
Example:
Requirement:
Payment-allocation integrity

Given:
A verified payment has ₦500,000 unallocated.
A charge has ₦400,000 outstanding.

When:
A Finance Reviewer attempts to allocate ₦450,000.

Then:
The allocation is rejected.
No partial allocation is stored.
The payment balance remains unchanged.
The charge balance remains unchanged.
Example:
Requirement:
Unknown Block B inventory

Given:
The Block B unit count has not been confirmed.

When:
Block B is created.

Then:
The unit count is stored as pending or null.
The system does not assign zero as confirmed inventory.
The system does not create placeholder units automatically.
90. Data Architecture Risks
90.1 Missing Tenant Scope
Risk:
A query omits organization_id.
Controls:
	•	Required tenant-aware repositories
	•	RLS
	•	Composite indexes
	•	Authorization tests
	•	Code review
90.2 Financial Denormalization Errors
Risk:
Stored balances drift from underlying transactions.
Controls:
	•	Transactional updates
	•	Reconciliation
	•	Derived balances
	•	Tests
	•	Append-only financial records
90.3 Excessive Generic Modeling
Risk:
Relationships become ambiguous and hard to authorize.
Controls:
	•	Explicit core tables
	•	Domain ownership
	•	Architecture review
	•	Limited polymorphic relationships
90.4 Uncontrolled Soft Deletion
Risk:
Deleted records remain active in queries.
Controls:
	•	Centralized query filters
	•	Archival status
	•	Partial indexes
	•	Tests
	•	Explicit lifecycle rules
90.5 Agreement Mutation
Risk:
Signed agreement content is overwritten.
Controls:
	•	Immutable versions
	•	Document checksum
	•	Version constraints
	•	Restricted updates
	•	Audit
90.6 Occupancy Overlap
Risk:
Two incompatible occupancies become active for one unit.
Controls:
	•	Transactions
	•	Exclusion constraints where feasible
	•	Application validation
	•	Concurrency tests
90.7 Sensitive Data Exposure
Risk:
Restricted columns appear in logs, exports, search, or APIs.
Controls:
	•	Field-level authorization
	•	Redaction
	•	Export approval
	•	Data classification
	•	Test coverage
91. Unresolved Data Decisions
The following remain pending:
	•	Final UUID version
	•	Final business-identifier generation method
	•	Final ORM or query-builder
	•	Final migration tool
	•	Final financial amount representation
	•	Final Row Level Security strategy
	•	Final database-role design
	•	Final status-enum implementation
	•	Final audit-event storage model
	•	Final history-table pattern
	•	Final soft-delete strategy by table
	•	Final polymorphic-link strategy
	•	Final search indexing method
	•	Final document checksum algorithm
	•	Final encryption strategy for selected columns
	•	Final retention periods
	•	Final partitioning thresholds
	•	Final materialized-view strategy
	•	Final report-read model
	•	Final external-event payload retention
	•	Final application-question model
	•	Final legal-hold implementation
	•	Final tenant-aware cache architecture
	•	Final transaction isolation levels
	•	Final occupancy-overlap constraint
	•	Final database backup retention
	•	Final read-replica strategy
	•	Final staging-data retention
	•	Final AI activity retention
	•	Final support-access database context
These decisions must be resolved through Architecture Decision Records and schema-design review.
92. Data Architecture Governance
Changes to the approved data architecture require:
	1	Change description
	2	Business rationale
	3	Affected entities
	4	Tenant-isolation impact
	5	Authorization impact
	6	Financial-integrity impact
	7	Agreement-integrity impact
	8	Occupancy impact
	9	Privacy impact
	10	Security impact
	11	Migration impact
	12	API impact
	13	Reporting impact
	14	Retention impact
	15	Performance impact
	16	Backup and recovery impact
	17	Test impact
	18	Rollback or forward-recovery strategy
	19	Architecture Decision Record where material
	20	Product Visionary approval
	21	Chief Product Architect approval
	22	Effective version
93. Current Data Architecture Decision
Habitacores will use PostgreSQL as the proposed relational system of record.
The canonical data structure is:
Global User Identity
→ Organization Membership
→ Role and Assignment
→ Organization-Scoped Business Records
→ Domain Workflows
→ Historical Records
→ Audit Events
The physical-property hierarchy is:
Organization
→ Property
→ Community
→ Building
→ Block
→ Floor
→ Unit
The financial model is:
Charge
→ Payment
→ Payment Allocation
→ Deposit, Credit, Adjustment, Reversal, or Refund
The residential lifecycle is:
Listing
→ Inquiry
→ Inspection
→ Application
→ Decision
→ Offer
→ Charge
→ Payment
→ Agreement
→ Occupancy
→ Move-In
→ Renewal or Move-Out
The database design must preserve:
	•	Organization isolation
	•	Stable identifiers
	•	Explicit hierarchy
	•	Unknown and provisional values
	•	Precise financial records
	•	Immutable signed agreements
	•	Occupancy integrity
	•	Role and assignment history
	•	Controlled documents
	•	Status history
	•	Auditability
	•	Retention readiness
	•	Migration safety
	•	Future SaaS extensibility
No schema shortcut, denormalization, provider limitation, migration convenience, or delivery deadline may override approved tenant-isolation, financial-integrity, agreement-integrity, privacy, security, or historical-traceability requirements.