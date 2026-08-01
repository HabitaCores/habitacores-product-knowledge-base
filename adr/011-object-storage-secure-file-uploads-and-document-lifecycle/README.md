# ADR-011 — Object Storage, Secure File Uploads, and Document Lifecycle

## Decision Record

| Field | Value |
|---|---|
| Product | Habitacores |
| Decision | Object Storage, Secure File Uploads, and Document Lifecycle |
| ADR | ADR-011 |
| Version | 0.1 |
| Status | Proposed |
| Decision Type | Foundational Storage, Security, and Document Architecture |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Object-Storage Provider | Supabase Storage |
| Metadata Authority | Habitacores PostgreSQL |
| Binary Authority | Immutable object in private object storage |
| Upload Model | Server-authorized direct or resumable upload |
| Download Model | Server-authorized short-lived signed access |
| Default Bucket Visibility | Private |
| Object-Key Authority | Server generated |
| File Versioning | Habitacores document-version records and immutable object keys |
| Malware Policy | Quarantine until approved scanning completes |
| Ordinary Object Overwrite | Prohibited |
| Tenant Authorization | Application authorization plus RLS |
| AI Access | Permission-aware, purpose-limited, and mediated |
| Last Updated | 1 August 2026 |

## 1. Context

Habitacores will manage documents and files including:

- Property photographs
- unit photographs
- rental application documents
- proof of identity
- proof of address
- income or employment evidence
- guarantor documents
- payment evidence
- receipts
- tenancy agreements
- agreement attachments
- legal notices
- property ownership evidence
- maintenance photographs
- inspection reports
- vendor documents
- organization branding assets
- support attachments
- exported reports
- generated documents
- Responsible AI source documents
- future electronic-signature evidence

These files vary significantly in:

- Sensitivity
- ownership
- purpose
- retention period
- permitted viewers
- legal significance
- mutability
- processing requirements
- malware risk
- acceptable format
- maximum size

Habitacores must prevent:

- Cross-organization file disclosure
- unauthorized upload
- unauthorized download
- predictable storage paths
- malicious-file execution
- disguised file types
- public exposure of sensitive documents
- stale signed-link reuse
- destructive replacement of issued documents
- orphaned storage objects
- untracked deletion
- unrestricted AI access
- object-storage metadata becoming the product domain model

## 2. Decision Drivers

The decision is driven by the need to:

1. Keep binary files outside ordinary application database rows.
2. maintain PostgreSQL authority over document identity and business relationships.
3. enforce organization and personal access rules.
4. support private storage.
5. support secure direct uploads.
6. support resumable uploads for larger files.
7. prevent client-selected storage paths.
8. validate file type and size.
9. scan untrusted files.
10. quarantine files before ordinary access.
11. preserve immutable document versions.
12. support legal and financial evidence.
13. generate time-limited access.
14. prevent storage-provider lock-in from spreading through domain modules.
15. support document retention and legal holds.
16. support asynchronous processing.
17. support audit and evidentiary history.
18. minimize sensitive data exposure to AI.
19. support operational reconciliation.
20. provide clear Codex implementation boundaries.

## 3. Decision

Habitacores will use Supabase Storage as its initial managed object-storage provider.

Habitacores PostgreSQL will remain authoritative for:

- Document identity
- document category
- document purpose
- owning organization
- owning or related person
- related business resources
- document versions
- original filename
- detected media type
- declared media type
- size
- cryptographic digest
- upload status
- scan status
- review status
- confidentiality classification
- retention classification
- legal-hold state
- current version
- supersession
- deletion eligibility
- audit relationships

Supabase Storage will store binary objects under server-generated immutable object keys.

## 4. Architectural Boundary

The approved document flow is:

```text
Authenticated actor
→ Application authorization
→ Upload intent
→ Server-generated document and version IDs
→ Server-generated object key
→ Time-limited upload authorization
→ Private object storage
→ Upload completion verification
→ File validation
→ Malware and safety processing
→ Document release
→ Authorized retrieval
````

Authentication or possession of an object key does not grant document access.

## 5. Provider Abstraction

Storage-provider behavior must remain behind a Habitacores adapter.

Conceptual interface:

```typescript
interface ObjectStorageProvider {
  createUploadAuthorization(
    request: UploadAuthorizationRequest
  ): Promise<UploadAuthorization>;

  createDownloadAuthorization(
    request: DownloadAuthorizationRequest
  ): Promise<DownloadAuthorization>;

  statObject(objectKey: string): Promise<StoredObjectMetadata | null>;

  copyObject(sourceKey: string, destinationKey: string): Promise<void>;

  deleteObject(objectKey: string): Promise<void>;
}
```

Domain and application modules must not depend directly on Supabase SDK types.

## 6. PostgreSQL Is the Metadata Authority

Object-storage metadata is not the authoritative Habitacores document record.

The application must not treat a storage object as a valid document merely because the object exists.

A valid Habitacores document requires an authoritative PostgreSQL record and an approved lifecycle state.

## 7. Binary Storage

Authoritative document binaries will not normally be stored in PostgreSQL `bytea` or PostgreSQL large-object structures.

PostgreSQL may store:

* Small cryptographic signatures
* bounded previews where explicitly approved
* digests
* extracted text references
* bounded structured metadata

General uploaded and generated files belong in object storage.

## 8. Private Buckets

Sensitive Habitacores files will reside in private buckets.

Public buckets may be used only for intentionally public assets such as approved listing images or organization branding.

A public listing image must remain separate from:

* Ownership evidence
* applicant documents
* payment evidence
* agreements
* legal notices
* maintenance evidence containing personal information

## 9. Bucket Strategy

The initial bucket design should remain bounded.

Directional categories may include:

```text
private-documents
quarantine
generated-documents
public-assets
```

Separate buckets may be introduced when materially different controls require them.

Buckets must not be created per organization merely as the primary tenant-isolation mechanism.

## 10. Object-Key Strategy

Object keys will be generated by trusted server-side code.

Directional format:

```text
<environment>/<classification>/<organization-or-scope>/<document-id>/<version-id>/<object-id>
```

The exact format may change.

Object keys must not include unnecessary:

* Names
* email addresses
* government identifiers
* phone numbers
* property addresses
* payment references
* document titles

## 11. Original Filename

The user’s original filename may be stored as metadata for display.

It must not become:

* The storage key
* the authorization boundary
* a trusted media-type signal
* a filesystem path
* an executable instruction

Original filenames must be normalized and escaped before display.

## 12. Immutable Object Keys

A finalized document version must point to an immutable object key.

Ordinary replacement of an existing object at the same key is prohibited.

A changed file requires:

* New document-version ID
* new object key
* new digest
* new validation
* new scan
* new audit history

## 13. Document Model

A directional document record may contain:

```text
documents
- id
- organization_id
- owner_user_id
- document_type
- purpose
- confidentiality
- status
- current_version_id
- retention_class
- legal_hold_status
- created_by
- created_at
- updated_at
```

Not every document will have both organization and personal ownership.

## 14. Document-Version Model

A directional document-version record may contain:

```text
document_versions
- id
- document_id
- version_number
- storage_provider
- bucket
- object_key
- original_filename
- declared_media_type
- detected_media_type
- size_bytes
- sha256_digest
- upload_status
- validation_status
- malware_scan_status
- review_status
- supersedes_version_id
- created_by
- created_at
- finalized_at
```

## 15. Document Relationships

Documents should relate to business resources through explicit typed relationships.

Examples:

```text
Application → Identity document
Application → Income evidence
Payment → Payment evidence
Agreement version → Generated agreement
Occupancy → Inspection report
Maintenance request → Maintenance image
Property → Public listing image
Legal matter → Legal notice
```

A generic unattached file collection is not the primary product model.

## 16. Upload Intent

Every upload begins with an authorized upload-intent command.

The command should establish:

* Actor
* organization or personal scope
* document type
* purpose
* related resource
* allowed media types
* maximum size
* confidentiality
* retention classification
* expected upload method

## 17. Upload Authorization

The application may issue a short-lived signed upload authorization after validating the upload intent.

The authorization must be restricted to:

* One object key
* one bucket
* one intended operation
* a short validity period
* approved size and type controls where supported

## 18. Direct Upload

The browser may upload directly to object storage using server-issued authorization.

Direct upload does not bypass Habitacores authorization because:

* The server chooses the object key.
* the server creates the document-version record.
* the server restricts the intended operation.
* the upload remains untrusted until finalized and scanned.

## 19. Standard and Resumable Uploads

Small files may use ordinary signed uploads.

Larger or interruption-sensitive uploads should use resumable upload support.

The exact threshold will be implementation configured and may reflect:

* File size
* connection reliability
* mobile usage
* provider guidance
* user experience

## 20. Upload Lifecycle States

Directional upload states include:

```text
INTENT_CREATED
UPLOAD_AUTHORIZED
UPLOADING
UPLOADED_UNVERIFIED
VALIDATING
QUARANTINED
SCANNING
AVAILABLE
REJECTED
FAILED
EXPIRED
CANCELLED
```

Exact names may be refined.

## 21. Upload Completion

A browser success response is not final proof that the expected file is safe or valid.

After upload, a trusted server or worker must verify:

* Object exists
* expected object key
* expected bucket
* actual size
* declared type
* detected type
* digest
* upload ownership
* lifecycle state

## 22. File-Size Validation

File-size limits must be defined by document type.

Examples may differ for:

* Listing images
* identity documents
* payment evidence
* agreements
* maintenance videos
* exports

Limits must be enforced server side and, where possible, at the storage boundary.

## 23. Extension Allowlist

Each document type must have an explicit permitted extension allowlist.

The application must not accept every file type and attempt to block only known-dangerous extensions.

Extension validation remains one signal rather than complete proof.

## 24. Media-Type Validation

Validation should compare:

* Declared media type
* extension
* detected content type
* file signature or magic bytes
* expected document category

A mismatch must result in rejection or controlled manual review.

## 25. Double Extensions

Names such as:

```text
agreement.pdf.exe
identity.jpg.html
receipt.png.php
```

must not be accepted merely because an earlier suffix appears safe.

## 26. Archive Files

Archive formats are prohibited by default.

When an approved workflow permits an archive, processing must defend against:

* Path traversal
* decompression bombs
* excessive file counts
* nested archives
* executable content
* symbolic links
* filename encoding attacks

## 27. Active Content

Files capable of containing active content require additional controls.

Examples include:

* HTML
* SVG
* office documents with macros
* PDFs containing scripts or embedded files

Habitacores should prefer safer formats and sanitized rendering where practical.

## 28. Malware Scanning

Untrusted uploads must remain quarantined until malware scanning and required validation complete.

The scan workflow should execute asynchronously through ADR-010 infrastructure.

Directional flow:

```text
Upload finalized
→ FileValidationRequested
→ MalwareScanRequested
→ Worker retrieves quarantined object
→ Scan and validation
→ AVAILABLE or REJECTED
```

## 29. Scan Provider

The exact malware-scanning engine or provider is deferred.

The selected solution must support:

* Private processing
* controlled data handling
* bounded file sizes
* deterministic result codes
* retry behavior
* signature or engine version
* auditability
* no unauthorized public file submission

## 30. Scan States

Directional malware states include:

```text
NOT_REQUESTED
PENDING
SCANNING
CLEAN
MALICIOUS
SUSPICIOUS
SCAN_FAILED
UNSUPPORTED
MANUAL_REVIEW_REQUIRED
```

A technical scan failure must not be treated as clean.

## 31. Quarantine

Quarantined objects must not be downloadable through ordinary document-access workflows.

Quarantine access is limited to:

* Approved scanner workloads
* security operators
* bounded operational tools

## 32. Scan Result Evidence

The scan result should preserve:

* Document-version ID
* object digest
* scanner identity
* engine or signature version
* scan time
* result
* reason code
* attempt
* correlation ID

A scan result applies only to the exact binary identified by its digest.

## 33. Digest

Habitacores should calculate and store a SHA-256 digest for finalized file versions.

The digest supports:

* Integrity verification
* duplicate investigation
* scan-result binding
* generated-document verification
* export evidence
* operational reconciliation

A digest does not replace authorization or malware scanning.

## 34. Duplicate Files

Duplicate detection may identify identical binary content.

The system must not automatically merge document records across:

* Organizations
* users
* business purposes
* confidentiality classes

Content equality does not establish shared authority.

## 35. Document Availability

A document becomes ordinarily available only when:

* Upload is complete.
* storage object is verified.
* validation succeeds.
* required malware scan is clean.
* document relationship is valid.
* required human review is complete.
* lifecycle state permits access.

## 36. Authorization

Every document operation must evaluate:

* Actor
* account status
* organization
* membership
* permission
* resource relationship
* document purpose
* confidentiality
* lifecycle state
* support-session scope
* authentication assurance
* legal-hold or retention restriction where relevant

## 37. Storage RLS

Supabase Storage RLS may provide an additional storage-layer boundary.

It must not become the only Habitacores document authorization system.

Complex business authorization remains in application services.

## 38. Direct Client Reads

Direct browser reads using broad persistent storage credentials are prohibited.

The browser may receive:

* A short-lived authorized download URL
* a controlled streamed response
* a safe public URL for intentionally public assets

## 39. Signed Download URLs

Signed download URLs must be:

* Short lived
* generated only after current authorization
* limited to the intended object
* omitted from ordinary logs
* absent from durable audit metadata
* treated as bearer capabilities until expiry

## 40. Revocation Limitation

A signed URL may remain usable until it expires.

Therefore:

* Expiry should be short.
* URLs should be generated on demand.
* highly sensitive downloads may use authenticated streaming instead.
* a signed URL must not be treated as immediately revocable unless provider behavior guarantees it.

## 41. Authenticated Streaming

For highly restricted documents, Habitacores may stream the object through a server-controlled endpoint.

This supports:

* Current authorization at request time
* controlled response headers
* download auditing
* additional redaction or transformation
* no long-lived reusable URL

The scalability cost must be considered.

## 42. Response Headers

Document responses should use safe headers, including:

* Correct `Content-Type`
* safe `Content-Disposition`
* controlled filename
* anti-sniffing behavior
* appropriate cache controls
* no unintended inline rendering for dangerous formats

## 43. Inline Versus Attachment

Untrusted or active-content formats should generally download as attachments.

Inline rendering should be limited to approved safe formats and controlled viewers.

## 44. Public Assets

Public assets require a deliberate publication workflow.

Publication should validate:

* Resource is intended to be public
* file is clean
* file type is approved
* metadata is safe
* no sensitive embedded information remains
* publication is authorized
* public version is appropriate

## 45. Image Processing

Public or user-facing images may be transformed to:

* Remove metadata
* normalize format
* limit dimensions
* generate thumbnails
* reduce file size
* prevent active content

The transformed output becomes a separately identified generated object.

## 46. EXIF and Embedded Metadata

Uploaded images may contain location, device, date, or identity metadata.

Habitacores should remove unnecessary embedded metadata from public derivatives.

The original may be retained only according to purpose and access policy.

## 47. Generated Documents

Generated files must reference the authoritative source version used for generation.

Examples:

* Receipt
* agreement
* notice
* export
* inspection report

A retry must not silently regenerate a legally significant document from newer mutable data.

## 48. Generated-Document Integrity

A finalized generated document should preserve:

* Source resource
* source version
* generation-template version
* renderer version where material
* object digest
* generated time
* generating workload
* approving actor where required
* supersession or void history

## 49. Document Versioning

Habitacores will implement application-level document versioning.

A new document version must not overwrite the prior object.

The document record may identify one current version while historical versions remain preserved according to policy.

## 50. Version Numbers

Human-readable version numbers may be sequential within a document.

The authoritative identity remains the document-version UUID.

Concurrency controls must prevent duplicate current versions.

## 51. Supersession

When a new version supersedes another:

* Both versions remain identifiable.
* the relationship is explicit.
* the former version is not silently deleted.
* access policy may distinguish current and historical versions.
* audit records preserve the responsible actor.

## 52. Finalized Documents

Issued, signed, verified, or legally significant document versions are immutable.

Corrections require:

* New version
* replacement document
* void or supersession record
* reason
* authorization
* audit

## 53. Deletion

Deleting a database record does not automatically mean immediate physical object deletion.

Deletion must account for:

* Retention policy
* legal hold
* financial history
* agreement history
* dispute status
* scan evidence
* backup behavior
* provider deletion semantics

## 54. Soft Deletion

Document metadata may enter a logically deleted or inaccessible state before physical deletion eligibility.

Soft deletion must not be used to evade required retention.

## 55. Physical Deletion

Physical object deletion requires an approved lifecycle process.

The process should:

1. Confirm authorization.
2. confirm retention eligibility.
3. confirm no legal hold.
4. confirm no authoritative resource still references the version.
5. append audit evidence.
6. request object deletion.
7. verify provider result.
8. reconcile metadata state.

## 56. Provider Deletion

The application must not claim deletion is complete merely because a request was submitted.

Deletion status should distinguish:

```text
DELETION_REQUESTED
DELETION_IN_PROGRESS
DELETED
DELETION_FAILED
```

## 57. Orphan Prevention

Habitacores must prevent:

* Object exists without a document-version record
* document version points to a missing object
* superseded record points to overwritten content
* completed upload remains permanently unfinalized

## 58. Reconciliation

A storage-reconciliation process should identify:

* Missing objects
* orphan objects
* size mismatches
* digest mismatches
* wrong bucket
* wrong object key
* stale upload intents
* scan records bound to another digest
* deletion mismatches

## 59. Upload Expiry

Unused upload intents and incomplete uploads should expire.

Expiry should:

* Mark the intent expired
* revoke or allow authorization to lapse
* remove temporary objects when safe
* preserve required operational evidence
* avoid creating ordinary available documents

## 60. Retention

Every document category should map to a retention classification.

Retention may depend on:

* Document type
* organization
* application lifecycle
* agreement lifecycle
* occupancy lifecycle
* financial record
* legal dispute
* identity-security requirement
* statutory obligations

Exact retention periods remain deferred.

## 61. Legal Hold

A legal hold prevents ordinary deletion or destructive transformation of covered document versions.

A hold should preserve:

* Scope
* reason
* authority
* start time
* end time
* affected resources
* audit history

## 62. Encryption

Object storage must use provider-supported encryption in transit and at rest.

Additional application-level encryption may be considered for highly restricted classes.

The exact key-management architecture is deferred.

## 63. Secrets

Storage administrative credentials must remain in approved secret storage.

They must not appear in:

* Client bundles
* public environment variables
* database records
* upload payload metadata
* object keys
* logs
* screenshots
* Git
* coding-agent prompts
* AI requests

## 64. Environment Isolation

Development, test, staging, and production storage must remain isolated.

Production files must not be copied into ordinary development environments.

Synthetic files should be used for automated tests.

## 65. Audit

Material document events include:

```text
DOCUMENT_UPLOAD_INTENT_CREATED
DOCUMENT_UPLOAD_COMPLETED
DOCUMENT_VALIDATION_FAILED
DOCUMENT_QUARANTINED
DOCUMENT_SCAN_COMPLETED
DOCUMENT_RELEASED
DOCUMENT_VIEWED_RESTRICTED
DOCUMENT_DOWNLOADED_RESTRICTED
DOCUMENT_VERSION_CREATED
DOCUMENT_SUPERSEDED
DOCUMENT_PUBLICATION_APPROVED
DOCUMENT_DELETION_REQUESTED
DOCUMENT_DELETED
DOCUMENT_LEGAL_HOLD_APPLIED
```

## 66. Audit Metadata

Document audit events should identify:

* Actor
* organization
* document ID
* version ID
* operation
* purpose where required
* support session where applicable
* result
* correlation ID

They must not contain:

* File contents
* permanent storage URL
* active signed URL
* unrestricted extracted text
* secrets

## 67. Responsible AI Access

AI receives no direct unrestricted object-storage access.

Approved AI document processing must follow:

```text
Approved use case
→ Current authorization
→ Document eligibility
→ Scan-clean requirement
→ Field and page selection
→ extraction or bounded retrieval
→ minimization and redaction
→ AI Gateway
→ validation
→ human review where required
```

## 68. AI Document Restrictions

AI must not automatically receive:

* Government identity documents
* full signed agreements
* raw payment evidence
* bank statements
* legal privileged material
* unrestricted support attachments
* quarantined files
* malware-suspicious files

Any approved exception requires purpose-specific review.

## 69. Extracted Text

Extracted document text should be stored separately from the binary.

The record should preserve:

* Source document version
* extraction method
* extraction version
* generated time
* page or section mapping
* confidence where applicable
* sensitivity
* access policy

Extracted text inherits the source document’s authorization unless an explicitly stricter policy applies.

## 70. OCR

OCR output is derived data and may contain errors.

It must not replace the original document as evidence.

Critical values require deterministic validation or human review.

## 71. Search Indexing

Search indexes may contain only approved extracted fields.

Search must preserve:

* Organization isolation
* document permissions
* confidentiality
* lifecycle state
* deletion and legal-hold behavior

A search result must not reveal an inaccessible document’s existence or contents.

## 72. Support Access

Support access to documents requires:

* Approved support session
* explicit scope
* purpose
* current authorization
* expiry
* document-access audit
* restricted download behavior

Support activity must remain attributed to the support operator.

## 73. Financial Evidence

Payment evidence is not itself payment verification.

Uploading a bank receipt, screenshot, or transfer record must not automatically:

* Verify payment
* allocate funds
* issue receipt
* activate agreement
* activate occupancy

## 74. Identity Documents

Identity-document handling requires heightened controls.

The system should support:

* Restricted classification
* narrow access
* no public URL
* limited AI use
* defined retention
* access auditing
* secure deletion when eligible

This ADR does not define legal identity-verification requirements.

## 75. Agreement Documents

An issued agreement document must bind to an immutable agreement version.

A later property-price, party-profile, or template change must not silently alter the issued document.

## 76. Receipt Documents

A receipt document must bind to the authoritative receipt record and exact Money values used at issuance.

A regenerated copy should reproduce the same authoritative content unless explicitly superseded.

## 77. File Processing Workers

Document processing will use bounded workers such as:

```text
File Validation Worker
Malware Scan Worker
Image Processing Worker
Document Generation Worker
Text Extraction Worker
Storage Reconciliation Worker
Retention Worker
```

Workers must use scoped workload identities.

## 78. Worker Access

Workers must not receive general unrestricted storage credentials when a narrower mechanism is available.

A worker should access only:

* Required bucket
* required object
* required operation
* required duration
* approved organization scope

## 79. Processing Idempotency

File-processing jobs must be idempotent.

The idempotency key should include the immutable document-version or object digest.

A retry must not create uncontrolled duplicate versions or derivatives.

## 80. Failure Handling

A processing failure must preserve:

* Document version
* processing type
* attempt
* error code
* retryability
* final state
* correlation ID

A failure must not cause the file to become available by default.

## 81. Observability

Safe storage telemetry may include:

* Upload-intent count
* upload completion
* upload expiry
* validation failure
* scan queue age
* scan result counts
* generated-document latency
* missing-object count
* orphan-object count
* signed-download issuance
* deletion failures
* storage usage

Telemetry must not expose signed URLs or sensitive filenames unnecessarily.

## 82. User-Facing Errors

Stable outcomes may include:

```text
FILE_TYPE_NOT_ALLOWED
FILE_TOO_LARGE
FILE_CONTENT_MISMATCH
UPLOAD_AUTHORIZATION_EXPIRED
UPLOAD_INCOMPLETE
FILE_VALIDATION_FAILED
FILE_QUARANTINED
MALWARE_DETECTED
SCAN_FAILED
DOCUMENT_NOT_AVAILABLE
DOCUMENT_ACCESS_DENIED
DOCUMENT_VERSION_CONFLICT
DOCUMENT_RETENTION_BLOCKED
DOCUMENT_LEGAL_HOLD_ACTIVE
STORAGE_OPERATION_FAILED
```

## 83. Testing

Tests must cover:

* Upload authorization
* server-generated paths
* wrong object key
* wrong organization
* expired authorization
* oversized upload
* extension mismatch
* media-type mismatch
* malformed file
* malware detection
* scan failure
* quarantine access denial
* release after clean scan
* signed-download expiry
* immutable version creation
* overwrite prevention
* supersession
* deletion restrictions
* cross-tenant access
* support access
* AI access restrictions
* reconciliation

## 84. Adversarial Upload Tests

Security tests should include:

* Double extensions
* null-byte filename attempts
* path traversal
* misleading MIME types
* polyglot files
* malformed PDFs
* active SVG
* office macros
* archive bombs
* nested archives
* malicious filenames
* EICAR test file
* excessive image dimensions
* embedded metadata
* repeated upload replay

## 85. Cross-Tenant Test

```text
Given:
A document belongs to Organization B.

And:
An actor belongs only to Organization A.

When:
The actor knows the document ID, object key, or former signed URL path.

Then:
Habitacores does not issue current download authorization.

And:
The Organization B document remains inaccessible.
```

## 86. Quarantine Test

```text
Given:
A newly uploaded document has not completed malware scanning.

When:
An ordinary user requests the file.

Then:
The request is denied.

And:
The document remains unavailable until its required checks succeed.
```

## 87. Immutable Version Test

```text
Given:
An agreement document version has been issued.

When:
A corrected file is required.

Then:
A new document version and new object key are created.

And:
The original object is not overwritten.
```

## 88. Signed-Link Test

```text
Given:
An authorized user receives a short-lived signed download URL.

When:
The URL expires.

Then:
The storage provider refuses access through that URL.

And:
A new URL requires current Habitacores authorization.
```

## 89. Malware Test

```text
Given:
A test upload contains a recognized malware test signature.

When:
The scan worker processes it.

Then:
The document enters a rejected or malicious state.

And:
It never becomes ordinarily downloadable.
```

## 90. Provider-Abstraction Test

```text
Given:
A domain service manages a document.

Then:
The domain service depends on a Habitacores storage interface.

And:
It does not depend directly on Supabase Storage SDK types.
```

## 91. Migration Requirements

The initial database migration should define:

* Documents
* document versions
* document relationships
* upload intents
* scan results
* processing attempts where selected
* organization ownership
* current-version integrity
* object-key uniqueness
* digest fields
* RLS
* indexes
* lifecycle constraints
* append-oriented history

## 92. Likely Indexes

Directional indexes include:

```text
organization_id, status, created_at
document_id, version_number
document_id, created_at
object_key
sha256_digest
malware_scan_status, created_at
related_resource_type, related_resource_id
retention_class, status
```

Indexes must follow measured access patterns.

## 93. Codex Boundaries

Every Codex file or document work package must specify:

* Document type
* purpose
* ownership
* organization scope
* related resource
* allowed extensions
* allowed detected types
* size limit
* upload method
* object-key generation
* quarantine policy
* malware-scan requirement
* availability state
* download policy
* retention class
* audit events
* asynchronous jobs
* tests

Codex must not:

* Use user-provided storage paths
* create public buckets for sensitive documents
* store permanent signed URLs
* overwrite finalized objects
* trust extensions or MIME headers alone
* skip malware scanning where required
* mark scan failure as clean
* expose administrative storage credentials
* place personal data in object keys
* store unrestricted files in PostgreSQL
* make storage metadata the business authority
* allow AI unrestricted storage access
* delete objects without retention and legal-hold checks

## 94. Prohibited Patterns

The following are not approved:

* Public storage for sensitive documents
* user-controlled object keys
* original filename as storage identity
* predictable personal-data paths
* permanent signed URLs stored in database records
* browser service-role credentials
* extension-only validation
* MIME-header-only validation
* ordinary access before scanning
* scan failure treated as clean
* overwrite-based legal-document versioning
* silent object deletion
* database metadata deleted while objects remain unmanaged
* AI directly browsing private buckets
* payment evidence treated as verified payment
* issued agreement regenerated from mutable current data

## 95. Options Considered

### Option A — Store Uploaded Files in PostgreSQL

Advantages:

* Database transaction alignment
* unified backups
* familiar authorization model

Disadvantages:

* Database growth
* inefficient large-file delivery
* increased backup and replication burden
* less suitable for direct and resumable uploads
* greater application-server load

Decision:

Rejected as the general file architecture.

### Option B — Public Object Storage With Obscure URLs

Advantages:

* Simple delivery
* strong caching
* minimal authorization infrastructure

Disadvantages:

* URL possession grants access
* sensitive-document exposure
* weak revocation
* unsuitable for financial, identity, agreement, and legal files

Decision:

Rejected.

### Option C — Direct Supabase Storage Domain Model

Advantages:

* Rapid implementation
* storage RLS
* fewer application records

Disadvantages:

* Provider metadata becomes business authority
* weak document relationships and version semantics
* provider lock-in
* insufficient retention and evidentiary modeling
* difficult provider migration

Decision:

Rejected.

### Option D — Supabase Storage With Habitacores-Owned Document Domain

Advantages:

* Managed private object storage
* signed and resumable uploads
* storage-layer access controls
* PostgreSQL-owned business metadata
* explicit document lifecycle
* provider abstraction
* future provider migration
* strong tenant, audit, and retention modeling

Disadvantages:

* Requires metadata and storage reconciliation
* malware scanning requires separate processing
* application-level versioning is required
* signed-link expiry requires careful policy
* two systems must remain consistent

Decision:

Approved.

## 96. Positive Consequences

* Binary storage remains scalable and separate from PostgreSQL rows.
* document identity and authorization remain under Habitacores control.
* sensitive files remain private.
* large uploads can be resumable.
* malicious files can be quarantined.
* legal and financial document versions remain immutable.
* short-lived access reduces permanent-link exposure.
* future storage-provider migration remains possible.
* AI access remains mediated and auditable.

## 97. Negative Consequences

* Document metadata and storage objects require reconciliation.
* malware scanning adds latency.
* signed-link behavior requires careful expiry choices.
* workers are required for validation and processing.
* application-level versioning adds records.
* retention and legal-hold implementation require later work.
* storage and database transactions cannot commit atomically together.

## 98. Risks

### 98.1 Orphaned Objects

Controls:

* Upload intents
* completion verification
* expiry cleanup
* reconciliation
* object-key uniqueness

### 98.2 Unauthorized Download

Controls:

* Private buckets
* application authorization
* short-lived signed URLs
* authenticated streaming for highly restricted files
* access audit

### 98.3 Malicious Upload

Controls:

* Allowlisted types
* size limits
* content detection
* quarantine
* malware scanning
* safe rendering
* adversarial tests

### 98.4 File Overwrite

Controls:

* Immutable object keys
* new version per change
* object-key uniqueness
* storage permission restrictions

### 98.5 Signed-Link Leakage

Controls:

* Short expiry
* no ordinary logging
* no durable storage
* current authorization before issuance
* streaming for higher-risk classes

### 98.6 AI Data Leakage

Controls:

* No direct bucket access
* approved use cases
* source eligibility
* minimization
* redaction
* AI Gateway
* audit

## 99. Acceptance Criterion — Upload Intent

```text
Given:
An authorized applicant uploads an approved document type.

When:
Habitacores creates the upload intent.

Then:
The server assigns the document ID, version ID, bucket, object key,
allowed type, size limit, confidentiality, and expiry.

And:
The browser does not choose an arbitrary storage path.
```

## 100. Acceptance Criterion — Validation

```text
Given:
A file declares itself as application/pdf.

And:
Its content signature indicates another prohibited format.

When:
Validation executes.

Then:
The version is rejected or placed into manual review.

And:
It does not become available merely because its extension is .pdf.
```

## 101. Acceptance Criterion — Document Version

```text
Given:
A payment-evidence document already has Version 1.

When:
An authorized user submits a corrected file.

Then:
Version 2 receives a new UUID and immutable object key.

And:
Version 1 remains preserved according to policy.

And:
The current-version relationship is updated transactionally.
```

## 102. Acceptance Criterion — Payment Boundary

```text
Given:
A resident uploads payment evidence successfully.

When:
The document becomes scan clean and available.

Then:
The payment remains unverified until an authorized deterministic
payment-verification workflow completes.

And:
No receipt, allocation, agreement activation, or occupancy activation
occurs solely because the file was uploaded.
```

## 103. Acceptance Criterion — AI Boundary

```text
Given:
An approved AI use case requires information from a document.

When:
The AI workflow executes.

Then:
Habitacores confirms authorization, document eligibility, clean scan,
purpose, minimization, and source scope.

And:
The AI provider receives no general private-bucket credentials.
```

## 104. Implementation Sequence

Recommended implementation order:

```text
1. Establish storage-provider abstraction
2. configure environment-isolated private buckets
3. create documents and document_versions
4. create document-resource relationships
5. implement server-generated object keys
6. implement upload intents
7. implement signed standard uploads
8. implement resumable uploads
9. implement upload completion verification
10. implement extension, media-type, signature, and size validation
11. establish quarantine
12. implement malware-scan job boundary
13. implement document availability transitions
14. implement authorized signed downloads
15. implement restricted authenticated streaming
16. implement application-level document versioning
17. implement generated-document integrity
18. implement storage reconciliation
19. implement retention and deletion eligibility
20. keep AI access mediated through approved document services
```

## 105. Deferred Decisions

This ADR does not decide:

* Exact bucket names
* exact upload-size limits
* exact resumable-upload threshold
* exact signed-URL lifetime
* exact malware-scanning provider
* exact content-disarm-and-reconstruction provider
* exact OCR provider
* exact image-transformation service
* exact PDF-rendering engine
* exact document-preview architecture
* exact encryption-key hierarchy
* exact retention periods
* exact legal-hold workflow
* exact electronic-signature provider
* exact virus-definition update policy
* exact object-storage region
* exact backup and replication strategy
* exact public image CDN configuration
* exact provider migration tooling

These require bounded implementation decisions or later ADRs.

## 106. Supersession Conditions

This ADR should be reviewed when:

* Supabase Storage no longer meets reliability or residency requirements.
* enterprise customers require dedicated storage.
* regional storage becomes mandatory.
* customer-managed encryption keys become required.
* legal evidence requires specialized immutable storage.
* object volume or bandwidth materially changes provider economics.
* a dedicated document-management platform becomes authoritative.
* direct mobile storage access becomes a major product surface.
* regulated workloads require stronger isolation.

## 107. Final Decision Statement

Habitacores will use Supabase Storage as its initial object-storage provider while retaining authoritative document identity, relationships, ownership, versions, classifications, lifecycle, access policy, scan status, retention, legal-hold status, and audit history in Habitacores PostgreSQL records.

Sensitive files will be stored in private buckets. Public buckets will contain only deliberately published and approved assets.

Every upload will begin through an authorized Habitacores upload intent. Trusted server-side code will generate the document identity, document-version identity, bucket, immutable object key, allowed formats, size limit, confidentiality, related resource, and upload expiry.

The browser may upload directly using a short-lived signed or resumable authorization, but an upload-provider success response will not make the document authoritative, safe, verified, or available.

Uploaded files will undergo size, extension, declared-type, detected-type, content-signature, digest, relationship, and required malware validation. Files requiring scanning will remain quarantined until a clean approved result is bound to the exact object digest.

A technical scan failure, unsupported scan, missing result, or ambiguous result will not be treated as clean.

Finalized document versions will be immutable. A corrected, regenerated, or replacement document will receive a new document-version ID and new object key. Issued agreements, receipts, notices, payment evidence, identity evidence, inspection reports, and other legally or financially significant files will not be silently overwritten.

Downloads will require current server-side authorization. Ordinary private-document access will use short-lived signed URLs or authenticated server streaming. Signed URLs will not be stored as permanent business data or exposed in ordinary logs and audit metadata.

Storage objects will not become the Habitacores domain model. A binary object is not an authorized Habitacores document unless the corresponding PostgreSQL record exists and its lifecycle state permits use.

Document access will remain subject to current account state, organization membership, permission, assignment, relationship, confidentiality, support-session scope, authentication assurance, RLS, retention, and legal-hold controls.

Payment evidence will not become verified payment merely because it was uploaded or scan clean. Uploaded documents will not independently approve applications, issue receipts, activate agreements, activate occupancy, satisfy legal requirements, or grant access.

AI will receive no unrestricted object-storage access. AI document use will require an approved use case, current authorization, an eligible scan-clean document version, purpose-limited retrieval, minimization, redaction, AI Gateway processing, validation, and human review where required.

Physical deletion will occur only through an authorized, retention-aware, legal-hold-aware, auditable, asynchronous process with provider-result verification and storage reconciliation.

No browser, user, organization administrator, provider SDK, storage credential, signed URL, object key, worker, support operator, coding agent, or AI model may bypass Habitacores document ownership, tenant isolation, validation, quarantine, malware controls, version immutability, confidentiality, retention, legal hold, audit, financial controls, agreement controls, occupancy controls, or Responsible AI safeguards.