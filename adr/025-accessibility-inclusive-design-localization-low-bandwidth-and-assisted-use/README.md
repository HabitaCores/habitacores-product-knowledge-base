# ADR-025 — Accessibility, Inclusive Design, Localization, Low-Bandwidth, and Assisted-Use Strategy

## Decision Record

| Field | Value |
|---|---|
| Product | Habitacores |
| ADR | ADR-025 |
| Version | 0.1 |
| Status | Proposed |
| Decision Type | Foundational Accessibility, Inclusive Experience, Localization, and Resilience Architecture |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Market | Nigeria |
| Web Accessibility Target | WCAG 2.2 Level AA |
| Mobile Accessibility Direction | WCAG 2.2-informed native mobile accessibility |
| Accessibility Scope | Public web, authenticated web, Android, iOS, emails, documents, and support workflows |
| Conformance Claim | Prohibited until defined scope is tested and documented |
| Design-System Accessibility | Mandatory |
| Keyboard Access | Mandatory for web functionality |
| Screen-Reader Support | Mandatory for material workflows |
| Text Scaling and Reflow | Mandatory |
| Color-Only Communication | Prohibited |
| Gesture-Only Essential Interaction | Prohibited |
| Automated Testing | Required but insufficient alone |
| Manual Assistive-Technology Testing | Required |
| Disabled-User Research | Required before material production scale |
| Plain Language | Required for material user-facing workflows |
| Localization Architecture | Required from initial implementation |
| Initial Product Language | English |
| Additional Languages | Deferred pending user research and translation governance |
| Low-Bandwidth Support | Required |
| Offline Authority | Restricted under ADR-014 |
| Assisted Use | Permitted only with consent, attribution, authorization, and privacy controls |
| AI Accessibility Authority | Advisory only |
| Last Updated | 2 August 2026 |

## 1. Context

Habitacores will support people who may have:

- Blindness or low vision
- color-vision differences
- deafness or reduced hearing
- limited motor control
- temporary or permanent mobility limitations
- cognitive or learning disabilities
- attention or memory limitations
- speech limitations
- reduced literacy
- limited familiarity with digital services
- older or low-performance devices
- intermittent or expensive connectivity
- limited access to private devices
- language or cultural differences
- temporary impairments caused by injury, stress, noise, glare, or illness

Material workflows include:

- Account creation
- authentication and MFA
- organization enrollment
- property discovery
- rental applications
- document upload
- payment initiation and confirmation
- agreement review and signing
- maintenance reporting
- notifications
- support
- privacy requests
- security and recovery communications

Failure to design these workflows accessibly may prevent users from obtaining
housing information, completing contracts, making payments, understanding
obligations, reporting problems, or exercising legal and privacy rights.

## 2. Decision Drivers

The strategy must:

1. Provide substantially equivalent access to material product capabilities.
2. integrate accessibility into design and engineering.
3. support assistive technologies.
4. make web workflows keyboard operable.
5. support screen readers and magnification.
6. preserve readability under text scaling and reflow.
7. prevent color, sound, gesture, or timing from becoming the only information channel.
8. support accessible authentication and error recovery.
9. support accessible payments and agreement execution.
10. support Android and iOS accessibility APIs.
11. make generated communications and documents accessible.
12. support plain language and cognitive accessibility.
13. prepare the architecture for localization.
14. support low-bandwidth and intermittent connectivity.
15. govern assisted use without weakening consent or privacy.
16. establish repeatable accessibility testing.
17. provide remediation and exception governance.
18. avoid unsupported conformance claims.
19. keep AI non-authoritative.
20. create implementation-ready controls for design, engineering, QA, and operations.

## 3. Decision

Habitacores will implement accessibility and inclusive design throughout:

```text
User research
→ Accessibility requirements
→ Content and interaction design
→ Accessible design-system components
→ Implementation
→ Automated testing
→ Manual keyboard and assistive-technology testing
→ Disabled-user usability testing
→ Remediation
→ Release review
→ Production monitoring and feedback
→ Periodic reassessment
````

Accessibility is a release-quality requirement, not an optional enhancement.

## 4. Accessibility Standard

Habitacores will target WCAG 2.2 Level AA for its web content and web
applications.

The target applies to the defined conformance scope, including:

* Public marketing pages
* account and authentication workflows
* tenant and applicant portals
* landlord and property-management portals
* administrative workflows
* privacy and support workflows
* web-rendered documents and notices where applicable

The target does not constitute a claim that conformance has already been
achieved.

## 5. Mobile Accessibility

Android and iOS applications will apply WCAG principles together with native
platform accessibility capabilities.

Mobile requirements include:

* Semantic accessibility roles
* meaningful labels
* logical reading order
* focus management
* scalable text
* sufficient target sizes
* assistive-technology announcements
* alternatives to complex gestures
* orientation tolerance where appropriate
* accessible validation and errors
* support for platform screen readers
* support for platform contrast and motion preferences

Native mobile accessibility must not be inferred solely from web compliance.

## 6. Accessibility Scope

Accessibility applies to:

```text
WEB_CONTENT
WEB_APPLICATIONS
ANDROID_APPLICATION
IOS_APPLICATION
TRANSACTIONAL_EMAIL
SMS_CONTENT
PUSH_NOTIFICATIONS
GENERATED_DOCUMENTS
DOWNLOADABLE_DOCUMENTS
SUPPORT_PROCESSES
IDENTITY_AND_MFA
PAYMENTS
AGREEMENTS
PRIVACY_REQUESTS
SECURITY_NOTICES
```

Provider-hosted experiences must also be assessed where they form part of a
material Habitacores workflow.

## 7. Accessibility Ownership

Every material product surface must identify:

* Product owner
* design owner
* engineering owner
* content owner
* accessibility reviewer
* test owner
* remediation owner
* provider dependency

At founding scale, one person may hold several responsibilities, but ownership
must remain explicit.

## 8. Design-System Authority

Accessible interaction patterns must be implemented primarily through the
Habitacores design system.

The design system should provide approved components for:

* Buttons
* links
* inputs
* field groups
* checkboxes
* radio groups
* select controls
* dialogs
* alerts
* navigation
* tabs
* tables
* pagination
* status messages
* file upload
* progress
* loading
* error summaries
* notifications
* date and currency input

Feature teams should not repeatedly create untested custom controls where an
approved component exists.

## 9. Semantic Structure

Web interfaces must use semantic HTML and platform capabilities before custom
accessibility attributes.

Required practices include:

* Correct heading hierarchy
* landmarks
* lists
* labels
* buttons for actions
* links for navigation
* tables for tabular data
* fieldsets and legends for related inputs
* accessible names and descriptions
* meaningful page titles
* programmatically determinable status and state

ARIA must not be used to compensate unnecessarily for avoidable non-semantic
markup.

## 10. Keyboard Access

All material web functionality must be operable through a keyboard without
requiring a mouse or touch input.

The interface must provide:

* Logical focus order
* visible focus indicators
* no keyboard traps
* usable menus and dialogs
* skip navigation where appropriate
* keyboard-operable controls
* focus restoration after modal interactions
* predictable focus after route or state changes

Custom keyboard behavior must follow established platform conventions.

## 11. Focus Management

Dynamic interfaces must manage focus intentionally.

Examples include:

* Moving focus to an error summary after failed submission
* announcing newly loaded content
* returning focus after a dialog closes
* focusing the main heading after relevant route changes
* preserving focus during non-destructive updates
* preventing focus from entering hidden content

Focus changes must not create unexpected disorientation.

## 12. Screen-Reader Support

Material workflows must be tested with representative screen-reader and
browser or operating-system combinations.

Testing should verify:

* Accessible names
* roles
* states
* descriptions
* reading order
* form labels
* validation messages
* live status announcements
* table relationships
* dialog behavior
* navigation
* payment status
* agreement status
* upload progress
* notification state

Visual appearance does not prove screen-reader accessibility.

## 13. Text Alternatives

Meaningful non-text content requires an equivalent text alternative.

Decorative images should be hidden from assistive technologies.

Charts, diagrams, property plans, status icons, and document previews require
appropriate alternatives based on their purpose.

Automatically generated descriptions must not be treated as correct without
review where the content is material.

## 14. Color and Contrast

Color must not be the only means of communicating:

* Status
* errors
* required fields
* selection
* payment outcome
* agreement state
* warning
* availability
* priority

Text and essential visual elements must meet the approved contrast
requirements.

Brand colors that fail accessibility requirements must not override readable
interaction design.

## 15. Text Scaling and Reflow

Habitacores must support browser zoom, system text scaling, and reflow without
loss of material content or functionality.

Interfaces must avoid:

* Fixed-height text containers
* clipped labels
* overlapping controls
* horizontally inaccessible forms
* unreadable tables
* hidden actions
* non-scaling text in images

Complex tabular information may use an accessible responsive alternative.

## 16. Motion and Animation

Habitacores will respect reduced-motion preferences where supported.

Animation must not:

* Be required to understand essential content
* create unnecessary vestibular discomfort
* flash in unsafe patterns
* block completion
* repeatedly distract users
* prevent reduced-motion operation

Essential progress should also be communicated through text or accessible
status updates.

## 17. Audio and Video

Where Habitacores introduces audio or video, the applicable content must
provide appropriate:

* Captions
* transcripts
* audio descriptions
* playback controls
* pause and stop controls
* non-audio alternatives

Autoplay with sound is prohibited.

## 18. Touch and Target Size

Interactive targets must be large enough and sufficiently separated for users
with motor impairments.

Critical actions such as:

* Payment
* signature
* deletion
* rejection
* cancellation
* consent
* account recovery

must not rely on small or closely packed controls.

## 19. Gesture Alternatives

Essential functions must not rely only on:

* Swiping
* dragging
* pinching
* shaking
* device movement
* multi-finger gestures
* path-based gestures

A simpler control or equivalent alternative must be available.

## 20. Orientation

Mobile workflows should operate in portrait and landscape unless a specific
orientation is essential to the capability.

An aesthetic preference does not establish essentiality.

## 21. Time Limits

Time limits should be avoided where they are not necessary.

Where a security or business requirement creates a limit, the workflow should,
where safely possible:

* Warn the user
* permit extension
* preserve entered information
* explain expiration
* support safe recovery
* avoid forcing complete restart

Security controls must not be weakened, but accessibility must be considered
in session and transaction design.

## 22. Accessible Authentication

Authentication must avoid unnecessary cognitive tests or inaccessible
challenges.

Users should be able to:

* Paste passwords where safe
* use password managers
* reveal a password temporarily
* receive clear MFA instructions
* recover from expired codes
* understand validation errors
* access alternative verification channels where approved

Authentication must remain consistent with ADR-007 security requirements.

## 23. CAPTCHA and Bot Protection

A CAPTCHA must not become the only path to material functionality.

Where bot protection is necessary, Habitacores should prefer risk-based,
invisible, or alternative mechanisms that do not exclude assistive-technology
users.

Any interactive challenge requires an accessible alternative and security
review.

## 24. Forms

Forms must provide:

* Persistent labels
* clear instructions
* required-field identification
* appropriate autocomplete attributes
* expected formats
* input assistance
* error prevention
* error summaries
* field-level errors
* preserved valid input
* accessible grouping
* confirmation before irreversible actions

Placeholder text must not replace a persistent label.

## 25. Error Handling

Error messages must:

* Identify what failed
* explain how to correct it
* avoid unnecessary technical jargon
* preserve user input where safe
* be programmatically associated with affected controls
* be announced to assistive technologies
* avoid relying solely on color
* distinguish validation errors from provider or system failures

Generic messages such as `Something went wrong` are insufficient for
recoverable user errors.

## 26. Error Prevention

High-impact actions require safeguards appropriate to their consequences.

Examples include:

* Reviewing payment amount and currency
* confirming the agreement version
* reviewing signers
* confirming deletion scope
* warning before account or document removal
* confirming recipient and communication channel
* enabling correction before submission

Accessibility must not reduce financial, legal, privacy, or security
verification.

## 27. Status and Progress

Users must receive accessible status for:

* File upload
* malware scanning
* payment confirmation
* agreement signing
* application progress
* background processing
* document generation
* notification delivery
* privacy requests

Visual spinners alone are insufficient.

## 28. Tables and Data-Dense Interfaces

Tables must preserve:

* Header relationships
* captions where required
* logical navigation
* readable cell contents
* sorting state
* filtering state
* pagination context
* responsive alternatives where necessary

A visual grid must not be implemented as an inaccessible collection of
unrelated elements.

## 29. Charts and Dashboards

Charts require an accessible alternative such as:

* Summary text
* data table
* downloadable structured data
* meaningful labels
* keyboard-accessible interaction where interaction is necessary

Color alone must not distinguish series or statuses.

## 30. Accessible Payments

Payment workflows must support:

* Screen-reader-readable amount and currency
* accessible payment-method selection
* understandable transfer instructions
* clear pending, verified, failed, and review states
* accessible provider handoff
* safe return from hosted checkout
* error recovery
* final receipt access
* no time pressure beyond justified provider or security constraints

A provider experience that blocks disabled users must not be accepted merely
because the provider processes payments successfully.

## 31. Accessible Agreements

Agreement workflows must support:

* Accessible document review
* heading and clause navigation
* zoom and text scaling
* screen-reader-readable content
* clear identification of signer role
* explicit consent
* accessible signature or acceptance methods
* accessible decline and support paths
* downloadable accessible copies
* status communication

A visually rendered signature page is not sufficient if the underlying terms
cannot be read with assistive technology.

## 32. Accessible Documents

Generated and downloadable documents should be accessible by design.

Where PDF is used, the generation pipeline should preserve:

* Document title
* language
* heading structure
* reading order
* tagged content
* table structure
* alternative text
* meaningful links
* sufficient contrast
* selectable text
* form-field labels where applicable

Scanned-image-only documents require remediation or an accessible alternative
where Habitacores is responsible for the document.

## 33. Email, SMS, and Push Accessibility

Transactional email should use:

* Semantic structure
* readable text
* meaningful link text
* sufficient contrast
* descriptive subject lines
* text alternatives
* responsive layout
* plain-text fallback where appropriate

SMS and push notifications should:

* Use understandable language
* avoid unexplained abbreviations
* avoid emoji-only meaning
* protect sensitive content
* direct users to an accessible authenticated workflow

## 34. Plain Language

Material user-facing content should use language appropriate to the audience.

Content should:

* State the main action clearly
* use familiar words
* explain technical or legal terms
* use short, structured sections
* avoid unnecessary complexity
* distinguish obligations from recommendations
* explain consequences accurately
* provide actionable error and recovery instructions

Plain language must not distort legal or contractual meaning.

## 35. Cognitive Accessibility

Habitacores should reduce unnecessary cognitive burden through:

* Consistent navigation
* predictable interaction
* stepwise complex workflows
* visible progress
* saved drafts
* confirmation
* plain language
* stable terminology
* limited distractions
* sufficient review time
* recovery from mistakes

Users must not be forced to remember information that the system can safely
retain or display.

## 36. Localization Architecture

The initial product language may be English, but the application must be
localization ready.

User-facing text must not be scattered as ungoverned literals throughout
feature code.

The architecture should support:

* Message keys
* locale files
* pluralization
* variable interpolation
* date formatting
* time formatting
* number formatting
* currency formatting
* address formatting
* phone-number formatting
* text expansion
* right-to-left readiness where future markets require it

## 37. Translation Governance

Translation requires:

* Source-language owner
* message context
* professional or qualified review for material text
* legal review for agreements and notices
* versioning
* effective date
* fallback language
* quality assurance
* update synchronization

Machine translation may assist drafting but must not independently publish
material legal, financial, safety, privacy, or security content.

## 38. Language Selection

Language selection should be:

* Easy to locate
* keyboard accessible
* screen-reader accessible
* persistent where appropriate
* independent of country assumptions
* reversible
* safely applied to the relevant user context

The system must not infer a user’s preferred language solely from name,
ethnicity, or location.

## 39. Locale and Formatting

Habitacores must treat locale separately from:

* Citizenship
* nationality
* residence
* organization
* currency
* legal jurisdiction
* preferred language

Examples:

* A user may prefer English while operating in Nigeria.
* NGN may remain the contract currency regardless of display-language choice.
* Legal agreement language may require separate approval.
* Phone and address inputs must not assume a single rigid format.

## 40. Names and Identity

The product must not assume that every person has:

* Exactly one first name
* exactly one last name
* a middle name
* a Western name order
* an ASCII-only name
* a name that fits a short fixed length

Legal names, preferred display names, and provider identity records must be
distinguished where necessary.

## 41. Addresses

Nigerian address handling must accommodate:

* Structured and unstructured components
* estates and communities
* streets without uniform numbering
* landmarks
* local government areas
* states
* postal codes where available
* property-specific directions

The platform must not reject a valid housing relationship merely because an
address does not fit a foreign formatting assumption.

## 42. Phone Numbers

Phone numbers must be stored and validated through an approved normalized
model while preserving appropriate display behavior.

The user interface should:

* Accept Nigerian local entry where appropriate
* support international format
* explain expected input
* avoid destructive assumptions
* distinguish verified from unverified numbers

Phone numbers must not be used as immutable human identity.

## 43. Dates and Time

The system must avoid ambiguous numeric date presentation.

Material dates should use formats that clearly distinguish:

* Day
* month
* year
* time zone where relevant

Examples include rent due dates, agreement commencement, notice deadlines,
payment times, and signature times.

Authoritative timestamps remain governed by server-side records.

## 44. Currency

Currency must always remain explicit for authoritative financial values.

Localization may affect presentation but must not:

* Change the authoritative currency
* alter minor-unit representation
* round contractual amounts incorrectly
* infer currency solely from language
* convert amounts silently

ADR-006 and ADR-016 remain authoritative.

## 45. Low-Bandwidth Design

Habitacores will support constrained connectivity through:

* Performance budgets
* compressed assets
* responsive images
* limited initial JavaScript
* server-rendered content where appropriate
* pagination
* resumable uploads
* explicit progress
* retry controls
* preserved drafts
* bounded polling
* network-failure messaging
* reduced unnecessary background activity

A visually elaborate interface must not prevent completion on a typical
lower-performance mobile device.

## 46. Connection Interruption

Material workflows should recover safely after interruption.

The system should distinguish:

```text
NOT_SUBMITTED
SUBMITTING
SERVER_ACCEPTED
PENDING_EXTERNAL_CONFIRMATION
SUCCEEDED
FAILED
RETRYABLE
EXPIRED
```

A lost connection must not cause users to repeat a payment, signature, or
irreversible submission without checking authoritative server state.

## 47. Draft Preservation

Long or complex workflows should support safe draft preservation where
appropriate.

Draft storage must define:

* Scope
* device or server location
* authentication
* encryption
* expiry
* synchronization
* conflict handling
* privacy
* final submission state

Sensitive documents or identity information must not be placed into
unprotected browser or mobile storage.

## 48. Offline Boundaries

Offline functionality may support:

* Cached authorized reference information
* non-authoritative drafts
* queued non-material input where explicitly approved
* previously downloaded documents under secure controls

Offline state may not authoritatively:

* Verify payment
* allocate funds
* issue receipts
* execute agreements
* activate occupancy
* alter permissions
* complete privacy requests
* resolve legal or financial disputes

ADR-014 remains authoritative for mobile offline boundaries.

## 49. Assisted Use

Some users may require assistance from:

* Family members
* caregivers
* interpreters
* agents
* support personnel
* accessibility assistants

Assistance must not silently transfer legal or account authority.

## 50. Assistance and Consent

An assisted workflow must distinguish:

* The data subject
* the authenticated account holder
* the assisting person
* the authorized representative
* the action being assisted
* the action requiring personal consent or signature
* the evidence of authority

An assistant may help navigate or enter information without automatically
acquiring the authority to consent, sign, pay, or access unrelated records.

## 51. Authorized Representatives

Where representation is permitted, the system must preserve:

* Representative identity
* subject represented
* authority basis
* scope
* effective period
* restrictions
* revocation
* actions taken
* audit events

Support staff must not simulate the user or obtain the user’s password.

## 52. Support Accessibility

Habitacores support should provide practical alternatives where possible,
such as:

* Accessible digital support
* email
* phone
* text-based communication
* assisted document guidance
* escalation for accessibility barriers

Support must preserve authentication, privacy, consent, and organization
boundaries.

## 53. Accessibility Feedback

Habitacores will maintain an accessible mechanism for reporting barriers.

A report should preserve:

* Affected surface
* workflow
* device and platform where available
* assistive technology where voluntarily provided
* description
* severity
* workaround
* owner
* remediation
* verification
* response status

Users must not be required to disclose a medical diagnosis to report an
accessibility problem.

## 54. Accessibility Defect Severity

Directional severity levels:

| Severity | Description                                                                                                                                                                 |
| -------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Critical | A material workflow is inaccessible with no reasonable alternative, including authentication, payment, agreement, housing application, privacy rights, or security response |
| High     | Major function is inaccessible or severely impaired for an affected user group                                                                                              |
| Medium   | Significant barrier exists, but a workable equivalent path remains                                                                                                          |
| Low      | Limited usability or non-material accessibility defect                                                                                                                      |

Final service targets require operational approval.

## 55. Accessibility Exceptions

An accessibility exception requires:

* Specific requirement
* affected surface
* affected users
* business and technical justification
* accessible alternative
* remediation owner
* due date
* risk approval
* expiry
* monitoring

Permanent unowned exceptions are prohibited.

Third-party limitations do not eliminate Habitacores’ obligation to assess,
document, mitigate, and seek alternatives.

## 56. Third-Party Providers

Providers must be assessed for accessibility where their interfaces are part
of the user journey.

Examples include:

* Hosted payment checkout
* electronic-signature ceremonies
* identity verification
* support widgets
* authentication screens
* file viewers
* consent tools

Provider selection must consider:

* Keyboard support
* screen-reader support
* text scaling
* mobile accessibility
* error handling
* language support
* documentation
* accessibility statement or evidence
* remediation responsiveness
* alternative workflow

## 57. Automated Testing

Automated accessibility testing should run in:

* Component development
* unit or component tests
* pull requests
* integration tests
* selected end-to-end tests
* production monitoring where appropriate

Automated tools may identify issues such as:

* Missing labels
* invalid semantics
* contrast failures
* duplicate IDs
* inaccessible names
* selected ARIA misuse

Automated testing alone cannot establish conformance.

## 58. Manual Testing

Manual testing must include, as applicable:

* Keyboard-only operation
* visible focus
* zoom and reflow
* screen-reader operation
* high-contrast or contrast settings
* reduced motion
* text scaling
* touch-target review
* orientation
* error recovery
* mobile accessibility services
* accessible document review

## 59. Assistive-Technology Matrix

Habitacores should maintain a practical test matrix covering representative:

* Browsers
* operating systems
* desktop screen readers
* Android screen reader
* iOS screen reader
* magnification or zoom
* keyboard interaction
* voice or switch access where material

The exact supported matrix is deferred to implementation planning.

## 60. User Testing

Before material production scale, Habitacores should test critical workflows
with users who have relevant disabilities and access needs.

Research must:

* Compensate participants appropriately
* protect privacy
* avoid exploitative recruitment
* capture actual barriers
* include diverse devices and connectivity where possible
* convert findings into owned remediation work

Simulating disability is not a replacement for participation by disabled
users.

## 61. Accessibility Release Gates

Critical workflows may not enter general production availability when they
contain an unresolved critical accessibility barrier without an approved
equivalent alternative and time-bounded exception.

Applicable workflows include:

* Registration
* authentication
* application
* payment
* agreement review and signing
* support
* privacy rights
* critical notices

## 62. Conformance Claims

Habitacores must not claim:

* WCAG 2.2 conformance
* Level AA conformance
* complete accessibility
* legal accessibility compliance
* universal usability

unless the claim:

* Defines the tested scope
* identifies the version
* follows the applicable conformance requirements
* is supported by current evidence
* lists known limitations where appropriate
* has received appropriate review

An automated score is not a conformance determination.

## 63. Accessibility Statement

Before public scale, Habitacores should publish an accessibility statement
describing:

* Accessibility commitment
* target standard
* covered surfaces
* known limitations
* contact channel
* feedback process
* response approach
* statement date
* review date

The statement must reflect actual product status.

## 64. Audit Events

Potential events include:

```text
ACCESSIBILITY_REVIEW_COMPLETED
ACCESSIBILITY_EXCEPTION_APPROVED
ACCESSIBILITY_EXCEPTION_EXPIRED
CRITICAL_ACCESSIBILITY_DEFECT_OPENED
CRITICAL_ACCESSIBILITY_DEFECT_RESOLVED
ACCESSIBILITY_USER_TEST_COMPLETED
ACCESSIBILITY_STATEMENT_PUBLISHED
TRANSLATION_VERSION_PUBLISHED
ASSISTED_ACCESS_AUTHORITY_GRANTED
ASSISTED_ACCESS_AUTHORITY_REVOKED
```

Audit metadata must not unnecessarily disclose disability or health
information.

## 65. Observability

Accessibility metrics may include:

```text
accessibility_test_failures
critical_accessibility_defects
high_accessibility_defects
accessibility_exception_age
keyboard_flow_failures
accessible_name_failures
document_accessibility_failures
translation_missing_keys
localization_fallbacks
upload_retry_failures
low_bandwidth_completion_failures
```

Accessibility telemetry must avoid identifying or profiling users by presumed
disability.

## 66. Responsible AI

AI may assist with:

* Drafting alternative-text suggestions
* plain-language suggestions
* translation drafts
* accessibility test interpretation
* summarizing user feedback
* identifying possible semantic issues

AI may not:

* Declare WCAG conformance
* certify accessibility
* infer disability from user behavior
* decide that an inaccessible alternative is acceptable
* publish material translation without required review
* impersonate an authorized representative
* sign or consent for a user
* suppress accessibility findings
* determine legal compliance

## 67. Testing Requirements

Required tests include:

* Semantic structure
* keyboard navigation
* focus management
* screen-reader labels
* text scaling
* zoom and reflow
* contrast
* non-color status
* reduced motion
* mobile screen-reader behavior
* form validation
* payment-status accessibility
* agreement review and signature accessibility
* accessible document generation
* language switching
* locale-safe dates and currency
* interruption and retry
* assisted-use authorization
* third-party provider accessibility

## 68. Keyboard-Only Test

```text
Given:
A user operates the web application without a pointing device.

When:
The user registers, signs in, opens an application, completes required fields,
reviews errors, saves a draft, and submits.

Then:
Every required action is keyboard operable.

And:
Focus remains visible, logical, and free from traps.
```

## 69. Screen-Reader Payment Test

```text
Given:
A tenant uses a supported screen reader.

When:
The tenant reviews and initiates a rent payment.

Then:
The amount, currency, obligation, payment method, confirmation state,
and errors are announced meaningfully.

And:
Visual color or layout is not the only source of information.
```

## 70. Agreement Accessibility Test

```text
Given:
An agreement requires tenant review and acceptance.

When:
A tenant uses a screen reader and enlarged text.

Then:
The agreement terms, headings, signer role, consent statement,
and final action remain accessible.

And:
The tenant is not forced to sign content that cannot be independently reviewed.
```

## 71. Text-Reflow Test

```text
Given:
A user increases text size or browser zoom substantially.

When:
The user completes a material workflow.

Then:
Text and controls reflow without clipping, overlap, hidden actions,
or loss of essential information.
```

## 72. Low-Bandwidth Payment Test

```text
Given:
A user initiates payment over an unstable connection.

When:
Connectivity drops after the provider receives the request.

Then:
Habitacores checks authoritative server and provider state before offering
another attempt.

And:
The user is not encouraged to create an uncontrolled duplicate payment.
```

## 73. Assisted-Use Test

```text
Given:
A tenant receives navigation assistance from another person.

When:
A legally significant consent or signature is required.

Then:
Habitacores distinguishes assistance from signer authority.

And:
The assisting person cannot silently consent or sign as the tenant.
```

## 74. Localization Test

```text
Given:
User-facing text is rendered through the localization system.

When:
A longer translated message is used.

Then:
The interface supports text expansion without truncating labels,
hiding controls, or breaking reading order.
```

## 75. Codex Boundaries

Every Codex accessibility or localization work package must specify:

* User workflow
* target surface
* applicable accessibility requirements
* semantic structure
* keyboard behavior
* focus behavior
* screen-reader behavior
* scaling and reflow
* contrast
* errors and status
* mobile behavior
* localization keys
* low-bandwidth behavior
* testing
* known provider limitations

Codex must not:

* Replace semantic controls with inaccessible custom elements
* remove focus indicators
* use color as the only status signal
* create mouse-only or gesture-only workflows
* hardcode user-facing text throughout feature code
* place legal text inside inaccessible images
* disable zoom
* block password-manager use unnecessarily
* represent a spinner as sufficient status
* claim accessibility conformance
* use machine translation as final legal translation
* infer disability
* bypass consent or signer authority during assisted use
* suppress accessibility failures to pass CI

## 76. Prohibited Patterns

The following are not approved:

* Accessibility review only before launch
* inaccessible custom controls where semantic controls exist
* keyboard traps
* hidden or absent focus indicators
* color-only errors
* placeholder-only labels
* unannounced dynamic status
* image-only agreement documents
* text embedded in images without alternatives
* automatic audio playback
* mandatory complex gestures
* disabled text zoom
* inaccessible CAPTCHA with no alternative
* fixed short time limits without justified recovery
* user-facing strings scattered without localization governance
* production personal data used in translation tools without approval
* accessibility claims based only on automated scanning
* support staff signing or consenting for users without authority

## 77. Options Considered

### Option A — Accessibility After Initial Launch

Advantages:

* Faster initial visual development
* lower immediate testing effort

Disadvantages:

* Accessibility defects become embedded in architecture
* remediation becomes more expensive
* critical users may be excluded
* design-system defects spread throughout the product

Decision:

Rejected.

### Option B — Automated Scanner as the Accessibility Program

Advantages:

* Fast
* repeatable
* CI compatible

Disadvantages:

* Cannot assess complete keyboard usability
* cannot determine screen-reader comprehension
* cannot assess cognitive burden
* cannot establish conformance
* misses many mobile and document barriers

Decision:

Rejected as the complete strategy.

### Option C — Separate Simplified Accessible Site

Advantages:

* May isolate accessibility implementation
* reduced impact on the primary interface

Disadvantages:

* Creates unequal experiences
* duplicates development
* falls out of synchronization
* may omit critical functionality
* treats accessibility as a separate product

Decision:

Rejected as the default architecture.

### Option D — Integrated Accessibility and Inclusive-Experience Architecture

Advantages:

* Accessibility enters the design system
* critical workflows remain equivalent
* web and mobile share requirements
* localization becomes feasible
* low-bandwidth users receive resilient workflows
* defects become testable
* assisted use remains governed

Disadvantages:

* Requires continuing design and testing effort
* manual assistive-technology testing takes time
* third-party providers may create limitations
* disabled-user research requires planning and budget
* accessible document generation can be complex

Decision:

Approved.

## 78. Positive Consequences

* More people can independently use Habitacores.
* critical housing workflows become more resilient.
* the design system embeds accessible patterns.
* web and mobile receive consistent requirements.
* legal, financial, and privacy actions become easier to understand.
* localization does not require architectural reconstruction.
* low-bandwidth failures are treated as product states.
* assisted use preserves accountability and consent.
* accessibility barriers become owned and measurable.

## 79. Negative Consequences

* Accessibility testing adds delivery effort.
* provider selection may become more restrictive.
* generated documents require additional validation.
* manual screen-reader and mobile testing requires expertise.
* translation and plain-language review add operational work.
* legacy or third-party interfaces may require alternatives.
* conformance evidence must be maintained as the product changes.

## 80. Risks

### 80.1 Accessibility Becomes a Checklist

Controls:

* Disabled-user research
* manual testing
* workflow-level requirements
* defect ownership
* release gates

### 80.2 Third-Party Provider Barrier

Controls:

* Provider assessment
* contractual expectations
* accessible alternative
* escalation
* provider replacement capability

### 80.3 Inaccurate Conformance Claim

Controls:

* Defined scope
* evidence
* independent review
* no automated-score claims
* dated accessibility statement

### 80.4 Localization Changes Legal Meaning

Controls:

* Qualified translation
* version control
* legal review
* source-language authority
* effective-date management

### 80.5 Assisted-Use Impersonation

Controls:

* Explicit actor representation
* authority records
* consent boundaries
* no password sharing
* audit

### 80.6 Low-Bandwidth Duplicate Action

Controls:

* Idempotency
* authoritative state refresh
* preserved attempt references
* clear pending states
* safe retry rules

## 81. Implementation Sequence

```text
1. Assign accessibility ownership
2. define the supported accessibility and assistive-technology test matrix
3. create the accessibility requirements checklist
4. audit the design system and interaction patterns
5. implement accessible foundational components
6. establish localization keys and message governance
7. establish Nigerian locale, date, currency, phone, name, and address rules
8. add automated accessibility checks to component and pull-request workflows
9. create manual keyboard and screen-reader test scripts
10. implement accessible authentication and account recovery
11. implement accessible forms, errors, alerts, and status messages
12. implement accessible payment workflows
13. implement accessible agreement review and execution
14. implement accessible document-generation controls
15. implement low-bandwidth and interruption-recovery behavior
16. implement assisted-use and representative-authority boundaries
17. assess third-party providers
18. test Android and iOS accessibility
19. conduct disabled-user usability testing
20. remediate critical and high barriers
21. publish an accurate accessibility statement before public scale
22. conduct independent accessibility review before claiming conformance
```

## 82. Deferred Decisions

This ADR does not decide:

* Exact accessibility-testing tools
* exact assistive-technology support matrix
* exact independent accessibility auditor
* final conformance scope
* whether a formal WCAG conformance claim will be made
* final Nigerian legal accessibility obligations
* initial additional language
* Yoruba, Hausa, Igbo, Pidgin, or other translation sequence
* exact translation provider
* exact language-review personnel
* exact accessible-PDF generation library
* exact disability-research partners
* exact accessibility defect service targets
* exact supported minimum device profile
* exact network performance budgets
* exact authorized-representative legal framework

These require bounded product, legal, accessibility, engineering, and user
research decisions.

## 83. Supersession Conditions

This ADR should be reviewed when:

* Habitacores enters production.
* a formal conformance claim is proposed.
* another language becomes supported.
* another country becomes an operational market.
* WCAG or applicable accessibility law materially changes.
* a new mobile architecture is adopted.
* biometric or voice interaction is introduced.
* an inaccessible third-party provider becomes material.
* disabled-user testing identifies structural barriers.
* a material accessibility complaint or legal issue occurs.

## 84. Final Decision Statement

Habitacores will integrate accessibility, inclusive design, localization,
low-bandwidth resilience, and assisted-use controls throughout its web,
Android, iOS, communication, document, support, payment, agreement, privacy,
and security experiences.

Habitacores will target WCAG 2.2 Level AA for the defined web conformance scope
and will apply WCAG-informed requirements together with native accessibility
capabilities to Android and iOS.

Accessibility will be embedded in the Habitacores design system through
semantic structure, keyboard operability, visible focus, accessible names,
logical reading order, sufficient contrast, text scaling, reflow, reduced
motion, status announcements, error recovery, and assistive-technology
support.

Critical functionality will not depend solely on color, sound, visual layout,
dragging, complex gestures, animation, fine motor precision, device
orientation, or short time limits.

Authentication, MFA, payments, agreements, uploads, notices, privacy requests,
support, and security recovery will receive explicit accessibility
requirements and testing.

Generated and downloadable documents will preserve meaningful structure,
reading order, language, headings, tables, links, alternatives, contrast, and
selectable text where Habitacores controls the artifact.

Automated accessibility testing will be used throughout development but will
not substitute for manual keyboard, screen-reader, magnification, mobile, and
disabled-user testing.

Habitacores will not claim WCAG conformance, complete accessibility, or legal
accessibility compliance unless the claim defines its scope and is supported
by current testing, evidence, review, and an accurate statement of known
limitations.

The initial product may operate in English, but user-facing text will be
localization ready. Material translations will be versioned and reviewed, and
machine translation will not independently publish legal, financial, privacy,
security, or safety content.

Names, addresses, phone numbers, dates, currencies, and language preferences
will be modeled without unsafe foreign-format or identity assumptions.
Language selection will not alter authoritative currency, jurisdiction,
contract terms, or business records.

Habitacores will support constrained connectivity through efficient delivery,
resumable transfers, saved drafts, explicit pending states, safe retries,
idempotency, and authoritative server-state reconciliation.

Offline and client-cached state will not verify payments, allocate funds,
issue receipts, execute agreements, activate occupancy, grant access, or
complete other authoritative legal or financial actions.

Assisted use will distinguish navigation support from legal authority.
Caregivers, family members, agents, support personnel, and interpreters will
not silently acquire the authority to consent, sign, pay, access unrelated
records, or impersonate a user.

AI may assist with drafts, alternative-text suggestions, plain-language
suggestions, translation drafts, testing interpretation, and feedback
summaries. AI will not certify accessibility, declare conformance, infer
disability, approve exceptions, publish material translations without review,
or act as a user’s representative.

No developer, provider, administrator, support operator, coding agent, design
preference, brand requirement, analytics system, or AI model may bypass
Habitacores accessibility requirements, equivalent-access controls,
localization governance, low-bandwidth safeguards, user consent, authority,
privacy, security, or release review.