# Document 15 — UI Design System and Interaction Standards

## Document Control

| Field | Value |
|---|---|
| Product | Habitacores |
| Document | UI Design System and Interaction Standards |
| Version | 0.1 |
| Status | Draft for approval |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Initial Market | Nigeria |
| Primary Currency | Nigerian naira |
| Primary Operational Time Zone | West Africa Time |
| Primary Tenant Boundary | Organization |
| Primary Experience | Responsive web application |
| Accessibility Target | WCAG 2.2 Level AA |
| Future Experience Channels | Native mobile applications and approved partner interfaces |
| Last Updated | 30 July 2026 |

## 1. Purpose

This document defines the UI Design System and Interaction Standards for Habitacores.

It translates the approved:

- Product vision
- Product strategy
- Product principles
- Business architecture
- Domain model
- Users, personas, and roles
- Business rules
- Functional requirements
- Non-functional requirements
- System architecture
- Data architecture
- Permission matrix and authorization model
- User journeys and workflows
- API architecture
- UX architecture and information architecture

into a reusable visual, behavioral, and accessibility framework.

This document defines:

- Design-system principles
- Design-token architecture
- Color semantics
- Typography
- Spacing
- Sizing
- Layout
- Grid
- Breakpoints
- Elevation
- Borders
- Radius
- Icons
- Motion
- Focus
- Component structure
- Buttons
- Links
- Inputs
- Forms
- Tables
- Lists
- Navigation
- Tabs
- Dialogs
- Drawers
- Toasts
- Alerts
- Status badges
- Timelines
- Task components
- Financial components
- Agreement components
- Maintenance components
- Sensitive-data components
- AI-assisted interface components
- Loading, empty, error, conflict, and unauthorized states
- Responsive behavior
- Low-bandwidth behavior
- Accessibility requirements
- Interaction patterns
- Content standards
- Component testing
- Governance
- MVP component scope
- Deferred design-system capabilities

This document does not define final brand artwork, final logo files, final marketing illustration style, complete high-fidelity screens, or final production component code.

Those will be developed through approved design artifacts, prototypes, implementation, and later Architecture Decision Records.

## 2. Design-System Objectives

The Habitacores design system must:

1. Create a consistent interface across all portals.
2. Reduce user confusion.
3. Support complex operational workflows.
4. Preserve organization, role, assignment, and property context.
5. Make workflow state visible.
6. Distinguish evidence, review, approval, verification, and finalization.
7. Protect financial truth.
8. Preserve agreement and occupancy integrity.
9. Support mobile browsers.
10. Support low-bandwidth users.
11. Meet accessibility standards.
12. Support dense operational interfaces.
13. Support simple applicant and resident experiences.
14. Reduce implementation inconsistency.
15. Prevent unauthorized action from appearing authoritative.
16. Support future native mobile applications.
17. Support reusable testing.
18. Support progressive enhancement.
19. Remain provider independent.
20. Support controlled future branding changes.

## 3. Design-System Principles

### 3.1 Clarity

Every component must communicate its purpose and state clearly.

### 3.2 Consistency

Equivalent actions and states must use equivalent visual and behavioral patterns.

### 3.3 Accessibility

Accessibility is a default requirement, not a later enhancement.

### 3.4 Explicit State

Draft, pending, approved, verified, active, failed, archived, and other states must be visible through text.

### 3.5 Safety

High-impact actions must use stronger review and confirmation patterns than routine actions.

### 3.6 Resilience

Components must handle:

- Slow networks
- Failed requests
- Stale records
- Missing data
- Unknown data
- Partial permissions
- Expired assignments
- Provider failures

### 3.7 Reusability

Shared components must be reusable across modules without embedding domain-specific assumptions unnecessarily.

### 3.8 Progressive Disclosure

Advanced controls should appear when needed rather than overwhelming all users.

### 3.9 Content First

Visual styling must support content and workflow understanding.

### 3.10 System Truth

The interface must reflect server-confirmed state rather than optimistic assumptions for high-risk operations.

## 4. Design-System Layers

The system should use the following layers:

```text
Foundations
→ Tokens
→ Primitives
→ Components
→ Patterns
→ Templates
→ Product Screens
````

### 4.1 Foundations

Includes:

* Brand direction
* Accessibility
* Layout philosophy
* Content principles
* Interaction principles

### 4.2 Tokens

Includes:

* Color
* Typography
* Spacing
* Radius
* Elevation
* Motion
* Breakpoints
* Sizing
* Z-index

### 4.3 Primitives

Includes:

* Text
* Box
* Stack
* Grid
* Icon
* Divider
* Surface
* Visually hidden content

### 4.4 Components

Includes:

* Button
* Input
* Select
* Checkbox
* Table
* Dialog
* Badge
* Alert
* Tabs
* Card

### 4.5 Patterns

Includes:

* Approval panel
* Payment verification
* Record header
* Task item
* Status timeline
* Sensitive-data reveal
* Destructive confirmation

### 4.6 Templates

Includes:

* Dashboard
* List page
* Record detail
* Form workflow
* Review workspace
* Approval workspace
* Public listing page

## 5. Token Architecture

Design tokens should use semantic naming.

Preferred:

```text
color.text.primary
color.text.secondary
color.background.surface
color.border.default
color.status.success
spacing.4
radius.medium
shadow.overlay
```

Avoid implementation-specific names such as:

```text
blue500
gray200
marginLarge
buttonGreen
```

Semantic tokens allow future branding changes without changing component meaning.

## 6. Token Categories

The design system should define:

* Brand tokens
* Semantic color tokens
* Typography tokens
* Spacing tokens
* Size tokens
* Radius tokens
* Border tokens
* Shadow tokens
* Motion tokens
* Breakpoint tokens
* Layering tokens
* Opacity tokens
* Focus tokens

## 7. Brand Token Strategy

Brand tokens remain provisional until final brand approval.

Potential token structure:

```text
brand.primary
brand.secondary
brand.accent
brand.neutral
brand.surface
brand.critical
```

Final colors must be evaluated for:

* Accessibility
* Cultural appropriateness
* Financial trust
* Professional credibility
* Public marketplace appeal
* Operational readability

## 8. Semantic Color Roles

The system should define semantic roles rather than module-specific colors.

Required roles include:

* Primary action
* Secondary action
* Neutral action
* Informational
* Success
* Warning
* Critical
* Pending
* Disabled
* Selected
* Focus
* Surface
* Elevated surface
* Border
* Text
* Muted text
* Link
* Visited link where appropriate

## 9. Status Color Independence

Color must never be the only way to communicate status.

Each status must include at least one of:

* Text label
* Icon
* Pattern
* Position
* Accessible description

Example:

```text
Verified
```

must not be represented only by a green dot.

## 10. Suggested Semantic Status Groups

### Neutral

Examples:

* Draft
* Not started
* Archived
* Unknown

### Informational

Examples:

* Submitted
* Scheduled
* In progress
* Assigned

### Pending

Examples:

* Pending review
* Pending approval
* Pending verification
* Awaiting signature

### Success

Examples:

* Approved
* Verified
* Active
* Completed
* Fully allocated

### Warning

Examples:

* Due soon
* Information required
* Partially allocated
* Expiring

### Critical

Examples:

* Rejected
* Failed
* Overdue
* Suspended
* Reversed
* Terminated

## 11. Text Color Tokens

Required text roles:

```text
text.primary
text.secondary
text.muted
text.inverse
text.link
text.success
text.warning
text.critical
text.disabled
```

Text contrast must meet approved accessibility requirements.

## 12. Background Tokens

Required background roles:

```text
background.page
background.surface
background.surface-muted
background.surface-elevated
background.selected
background.information
background.success
background.warning
background.critical
background.disabled
```

## 13. Border Tokens

Required border roles:

```text
border.default
border.strong
border.subtle
border.focus
border.success
border.warning
border.critical
border.disabled
```

## 14. Typography Principles

Typography must support:

* High readability
* Dense operational screens
* Mobile use
* Long forms
* Legal content
* Financial content
* Multilingual expansion
* Accessible zoom
* Clear hierarchy

## 15. Typography Roles

The system should define:

* Display
* Page title
* Section title
* Subsection title
* Body
* Small body
* Label
* Caption
* Helper text
* Code or identifier
* Financial amount
* Legal content
* Button label

## 16. Typography Scale

A provisional scale may include:

```text
text-xs
text-sm
text-base
text-lg
text-xl
text-2xl
text-3xl
text-4xl
```

The final scale must preserve readable hierarchy without excessive variation.

## 17. Font Weight

Approved weights may include:

* Regular
* Medium
* Semibold
* Bold

Very light weights should be avoided for operational content.

## 18. Line Length

Long-form content should use readable line lengths.

Preferred general range:

```text
45–80 characters per line
```

Legal and agreement content may use wider layouts only when readability remains acceptable.

## 19. Numeric Typography

Financial and operational numbers should use:

* Consistent alignment
* Tabular numerals where available
* Clear separators
* Visible currency
* Clear decimal precision

Example:

```text
₦500,000.00
```

## 20. Business Identifier Typography

Business identifiers may use a visually distinct style.

Example:

```text
HAB-APP-000001
HAB-PAY-000042
HAB-AGR-000018
```

Identifiers must remain selectable and copyable.

## 21. Spacing System

The system should use a consistent spacing scale.

Illustrative scale:

```text
0
2px
4px
8px
12px
16px
20px
24px
32px
40px
48px
64px
80px
```

Spacing should be represented through tokens rather than arbitrary values.

## 22. Spacing Principles

* Related elements should be grouped closely.
* Distinct sections should use larger separation.
* Mobile layouts should remain compact but not cramped.
* Dense tables may use reduced spacing.
* Touch targets must remain sufficiently large.
* Form fields must have adequate vertical separation.

## 23. Sizing Tokens

Sizing tokens may cover:

* Icon sizes
* Control heights
* Avatar sizes
* Container widths
* Sidebar widths
* Dialog widths
* Drawer widths
* Maximum content widths
* Touch-target dimensions

## 24. Control Heights

A provisional control system may include:

```text
Compact: 32px
Default: 40px
Comfortable: 48px
```

Touch-first controls should generally use the comfortable size.

## 25. Touch Targets

Interactive targets should be at least approximately:

```text
44px × 44px
```

unless a larger accessible target is required.

## 26. Radius Tokens

Potential radius roles:

```text
radius.none
radius.small
radius.medium
radius.large
radius.round
```

Radius should communicate system consistency rather than decorative excess.

## 27. Border Standards

Default borders should use:

* One-pixel structural borders
* Stronger borders for focus or critical state
* Dividers for content grouping
* Minimal decorative borders

## 28. Elevation

Elevation should communicate stacking and interaction.

Potential elevation levels:

```text
elevation.none
elevation.surface
elevation.dropdown
elevation.sticky
elevation.dialog
elevation.toast
```

Excessive shadows should be avoided.

## 29. Layering and Z-Index

Layering tokens should define:

* Base content
* Sticky navigation
* Dropdown
* Tooltip
* Drawer
* Dialog
* Toast
* Emergency banner

Components must not use arbitrary z-index values.

## 30. Motion Principles

Motion should:

* Clarify change
* Provide feedback
* Preserve orientation
* Avoid distraction
* Respect reduced-motion settings
* Avoid blocking essential interaction

## 31. Motion Tokens

Potential motion tokens:

```text
duration.instant
duration.fast
duration.standard
duration.slow

easing.standard
easing.enter
easing.exit
```

## 32. Reduced Motion

When reduced motion is requested:

* Disable decorative animation
* Reduce transition distance
* Avoid parallax
* Avoid auto-scrolling
* Preserve essential feedback

## 33. Iconography Principles

Icons must be:

* Recognizable
* Consistent
* Simple
* Accessible
* Paired with labels for important actions
* Avoided where meaning is ambiguous

## 34. Icon Usage

Icons may support:

* Navigation
* Status
* Alerts
* Actions
* File types
* Record types
* Search
* Filters
* Sorting
* Expansion
* Context switching

Icons must not replace critical text labels without accessible names.

## 35. Illustration and Imagery

Illustrations and images should:

* Support understanding
* Reflect credible housing and community contexts
* Avoid misleading property representations
* Avoid false operational claims
* Use optimized delivery
* Include alternative text where informative

## 36. Responsive Breakpoint Strategy

The final breakpoint values remain subject to implementation review.

The system must support:

* Small mobile
* Large mobile
* Tablet
* Laptop
* Desktop
* Wide desktop

Components should adapt based on available space rather than device assumptions alone.

## 37. Layout Grid

A responsive grid may use:

```text
Mobile: 4 columns
Tablet: 8 columns
Desktop: 12 columns
```

Exact gutters and margins will be finalized in implementation.

## 38. Page Containers

Potential container categories:

* Full width
* Standard
* Narrow
* Form
* Reading
* Data intensive

Examples:

* Public listing search: full width
* Application form: narrow or form width
* Agreement reading: reading width
* Operational table: full width

## 39. Application Layout

The organization portal may use:

```text
Global header
+ Primary side navigation
+ Context header
+ Main content
+ Optional contextual panel
```

On mobile:

```text
Compact header
+ Collapsible navigation
+ Main content
+ Bottom or contextual actions
```

## 40. Surface Architecture

Primary surfaces may include:

* Page background
* Main content surface
* Cards
* Side panels
* Drawers
* Dialogs
* Popovers
* Banners
* Toasts

Each surface must have a defined role.

## 41. Component Anatomy Standards

Every component specification should define:

* Purpose
* Anatomy
* Variants
* States
* Responsive behavior
* Keyboard behavior
* Screen-reader behavior
* Content rules
* Error behavior
* Loading behavior
* Permission behavior
* Test requirements

## 42. Component State Model

Interactive components should consider:

* Default
* Hover
* Focus
* Active
* Selected
* Disabled
* Read-only
* Loading
* Success
* Warning
* Error

## 43. Button Hierarchy

Button variants should include:

* Primary
* Secondary
* Tertiary
* Ghost
* Destructive
* Link-style
* Icon-only where appropriate

## 44. Primary Button

Use for the most important action on a page or workflow step.

Examples:

* Submit application
* Verify payment
* Issue agreement
* Create work order

Each section should generally have one primary action.

## 45. Secondary Button

Use for important but non-primary actions.

Examples:

* Save draft
* Request information
* Preview
* Export

## 46. Tertiary and Ghost Buttons

Use for:

* Low-emphasis actions
* Toolbar actions
* Dismissal
* Secondary navigation
* Non-destructive inline actions

## 47. Destructive Button

Use for actions such as:

* Revoke membership
* Reject payment
* Reverse payment
* Terminate agreement
* Suspend organization
* Retire unit

Destructive buttons must use explicit labels.

Avoid:

```text
Confirm
```

Preferred:

```text
Terminate agreement
```

## 48. Button Loading State

When submitting:

* Disable repeated activation.
* Show progress.
* Preserve the action label where possible.
* Do not show success until the server confirms.
* Support retry after failure.

## 49. Icon-Only Buttons

Icon-only buttons require:

* Accessible name
* Tooltip where helpful
* Minimum target size
* Clear icon
* No use for ambiguous high-impact actions

## 50. Link Standards

Links should:

* Be visually distinguishable
* Have visible focus
* Use descriptive text
* Avoid “click here”
* Preserve visited-state behavior where appropriate
* Not imitate buttons unnecessarily

## 51. Input Components

Required input components include:

* Text input
* Email input
* Telephone input
* Password input
* Number input
* Currency input
* Date input
* Time input
* Text area
* Search input
* Select
* Combobox
* Radio group
* Checkbox
* Switch
* File upload
* Address input
* Tag input where approved

## 52. Input Anatomy

Each input may include:

* Label
* Required indicator
* Description
* Input control
* Prefix or suffix
* Character count
* Validation message
* Success message
* Helper action

## 53. Input Labels

Labels must:

* Remain visible
* Be programmatically associated
* Use plain language
* Not rely on placeholders
* Explain required format when necessary

## 54. Placeholder Use

Placeholders may provide examples but must not replace labels.

Example:

```text
Label:
Phone number

Placeholder:
+234 800 000 0000
```

## 55. Required Fields

Required fields should use:

* Visible required indication
* Programmatic required state
* Clear submission validation

Optional fields may be labeled:

```text
Optional
```

where useful.

## 56. Read-Only Fields

Read-only fields should:

* Remain legible
* Be distinguishable from disabled fields
* Permit copying where safe
* Explain why editing is unavailable when relevant

## 57. Disabled Fields

Disabled fields should not be used merely to hide authorization rules.

Where a user cannot change a field due to status or permission, provide explanatory content.

## 58. Currency Input

Currency input should support:

* Currency symbol
* Currency code
* Thousand separators
* Decimal precision
* Positive amount validation
* Maximum limits
* Clear parsing

Example:

```text
₦500,000.00
```

The interface must not use floating-point assumptions for authoritative calculations.

## 59. Date Input

Date components should support:

* Keyboard entry
* Date picker
* Clear format guidance
* Minimum and maximum values
* Invalid date feedback
* Operational time-zone context where relevant

## 60. Telephone Input

Telephone input should support:

* Country code
* Nigerian number formats
* International numbers
* Accessible formatting
* Validation without over-restrictive assumptions

## 61. Address Input

Address capture may include:

* Address line
* Area or district
* City
* State
* Country
* Landmark
* Optional geolocation later

The system must not require precise location where a broader location is sufficient.

## 62. Select Component

Use for short, predefined choices.

Must support:

* Keyboard navigation
* Visible selected value
* Error state
* Disabled options
* Placeholder or prompt
* Accessible announcement

## 63. Combobox

Use when users need to search a larger option set.

Examples:

* Property
* Unit
* Vendor
* Assigned reviewer
* Organization

Combobox results must be authorization scoped.

## 64. Radio Group

Use for mutually exclusive options when users benefit from seeing all options.

Examples:

* Approve
* Reject
* Return for revision

## 65. Checkbox

Use for independent selections.

Checkboxes should not be used for major irreversible confirmation without supporting text.

## 66. Switch

Use only for immediate binary settings.

Examples:

* Enable email notifications
* Enable SMS notifications

Do not use for:

* Agreement activation
* Payment verification
* Membership suspension

## 67. Text Area

Text areas should support:

* Clear purpose
* Appropriate minimum size
* Character guidance
* Autosave where needed
* Structured validation
* Confidentiality labels for internal notes

## 68. File Upload Component

The file-upload component should include:

* Accepted file types
* Maximum size
* Drag-and-drop where supported
* Browse action
* Upload progress
* Scan status
* Validation result
* Retry
* Remove
* Replace

## 69. File Upload Security States

File states should include:

* Selected
* Uploading
* Uploaded
* Pending scan
* Scan passed
* Scan failed
* Rejected
* Processing
* Ready
* Replacement required

## 70. Form Layout

Forms should use:

* Logical sections
* Clear headings
* Consistent labels
* Reasonable line length
* Visible actions
* Error summary
* Save status
* Responsive stacking

## 71. Single-Column Form Standard

Critical and applicant-facing forms should generally use a single primary column.

Multiple columns may be used for clearly related short fields.

## 72. Multi-Step Form Standard

Multi-step forms must provide:

* Step title
* Progress
* Current step
* Completed steps
* Save and exit
* Back
* Continue
* Review
* Submit
* Validation summary

## 73. Form Autosave

Autosave should display:

```text
Saving...
Saved
Save failed
Last saved 2 minutes ago
```

Autosave must not imply submission.

## 74. Unsaved Changes

When leaving a page with unsaved changes:

* Warn the user.
* Preserve input where practical.
* Provide leave and stay actions.
* Avoid warning when nothing changed.

## 75. Validation Timing

Validation may occur:

* After field interaction
* On step completion
* On submission
* During server response

Avoid immediate error messages before the user has had an opportunity to enter a value.

## 76. Error Summary

Forms with multiple errors should show an error summary containing:

* Number of errors
* Linked field names
* Clear correction guidance
* Focus management

## 77. Validation Message Standards

Preferred:

```text
Enter an annual rent greater than zero.
```

Avoid:

```text
Invalid amount.
```

## 78. Success Feedback

Success feedback must confirm:

* What was completed
* Which record was affected
* What happens next
* Whether another action is required

## 79. Search Component

Search should support:

* Clear label
* Search icon
* Clear action
* Keyboard submission
* Loading state
* No-result state
* Recent searches only where safe
* Authorization-aware suggestions

## 80. Filter Components

Filters may use:

* Dropdown
* Multi-select
* Date range
* Amount range
* Status chips
* Assignment filters
* Property selector

Active filters must remain visible.

## 81. Filter Drawer

On smaller screens, filters may use a drawer with:

* Current filters
* Apply
* Clear
* Result count where available
* Accessible close behavior

## 82. Sort Controls

Sort controls should show:

* Sort field
* Sort direction
* Current selection
* Available options

Only approved sortable fields should be presented.

## 83. Pagination Component

Pagination should support:

* Current page
* Total pages where known
* Previous
* Next
* Page-size control where approved
* Keyboard accessibility
* Mobile simplification

## 84. Cursor Pagination

Activity feeds and notifications may use:

* Load more
* Infinite loading only where accessible
* Visible loading
* Recoverable failure
* Stable position

## 85. Table Component

The table component should support:

* Caption
* Column headers
* Sort
* Filter
* Pagination
* Row actions
* Selection where safe
* Empty state
* Loading state
* Error state
* Responsive alternative

## 86. Table Density

Potential density modes:

* Comfortable
* Standard
* Compact

Compact mode should be reserved for operational users and must remain accessible.

## 87. Table Alignment

Recommended:

* Text: left aligned
* Numbers: right aligned
* Status: left or center consistently
* Dates: left aligned
* Actions: right aligned

## 88. Financial Table Alignment

Financial amounts should:

* Use tabular numerals
* Align decimals
* Display currency
* Avoid mixed-currency totals
* Use clear negative-value formatting

## 89. Responsive Tables

On small screens, tables may become:

* Card lists
* Stacked rows
* Expandable rows
* Priority-column views

Critical data must remain available.

## 90. Row Selection

Selection controls should:

* Clearly indicate selected rows
* Show selected count
* Permit deselection
* Avoid selection for high-impact actions unless explicitly governed

## 91. Bulk Action Bar

When rows are selected, show:

* Number selected
* Available actions
* Clear selection
* Safety warning where relevant

Bulk verification, approval, refund execution, agreement activation, and occupancy activation should generally not be supported.

## 92. List Component

Lists may be used for:

* Notifications
* Tasks
* Applications
* Work orders
* Search results
* Documents
* Activity

List items should have consistent hierarchy.

## 93. Card Component

Cards may contain:

* Title
* Supporting information
* Status
* Primary metric
* Action
* Footer
* Context

Cards should not be used for every piece of information.

## 94. Summary Card

Summary cards may show:

* Metric
* Trend
* Time period
* Definition
* Link to records

They must avoid misleading precision.

## 95. Record Header Component

Major records should use a standard record header containing:

* Record type
* Business identifier
* Primary label
* Status badge
* Context
* Primary action
* Secondary actions
* Overflow menu
* Last updated
* Assignment where relevant

## 96. Context Banner

A context banner may show:

```text
Organization:
HabitaCores Pilot Organization

Role:
Finance Reviewer

Property:
Community 001
```

It must remain compact but visible.

## 97. Status Badge Component

A status badge must provide:

* Text label
* Semantic tone
* Accessible name
* Optional icon
* Consistent size
* Approved status mapping

## 98. Status Badge Rules

Do not create a new status label solely for visual preference.

Status labels must correspond to approved domain states or approved user-facing mappings.

## 99. Timeline Component

Timelines may display:

* Workflow status
* Activity
* Approval history
* Agreement versions
* Payment events
* Maintenance updates

Each item should include:

* Event
* Actor where permitted
* Date and time
* Description
* Related action
* System or provider source where relevant

## 100. Stepper Component

Use for sequential workflows such as:

* Application completion
* Organization onboarding
* Move-in
* Move-out
* Agreement preparation

The stepper must distinguish:

* Current
* Complete
* Incomplete
* Error
* Locked
* Optional

## 101. Tabs

Tabs should:

* Organize related views
* Use concise labels
* Support keyboard navigation
* Preserve URL state where appropriate
* Avoid excessive tab counts
* Hide unauthorized tabs

## 102. Accordion

Use for:

* Long detail sections
* Optional supporting information
* FAQ
* Mobile-friendly grouped content

Do not hide critical warnings in collapsed content.

## 103. Breadcrumbs

Breadcrumbs should:

* Show hierarchy
* Support navigation
* Truncate safely
* Avoid unauthorized ancestors
* Remain readable on mobile

## 104. Side Navigation

Side navigation should support:

* Primary sections
* Active state
* Collapsed mode
* Permission-aware items
* Keyboard navigation
* Mobile alternative
* Notification counts where appropriate

## 105. Top Navigation

Top navigation may include:

* Brand
* Search
* Organization context
* Property context
* Tasks
* Notifications
* Help
* User menu

## 106. Bottom Navigation

Personal mobile portals may use bottom navigation for a limited number of high-frequency destinations.

Potential items:

* Home
* Applications or Occupancy
* Payments
* Maintenance
* More

## 107. Menu Component

Menus should:

* Use clear action labels
* Support keyboard interaction
* Separate destructive actions
* Close predictably
* Preserve accessible focus

## 108. Dropdown Component

Dropdowns are suitable for:

* Context selection
* Secondary actions
* Low-frequency navigation
* Compact option lists

They must not conceal urgent status or critical required actions.

## 109. Drawer Component

Drawers may be used for:

* Mobile navigation
* Filters
* Contextual detail
* Supporting records
* Non-destructive forms

Drawers should not replace full pages for highly complex workflows.

## 110. Dialog Component

Dialogs may be used for:

* Confirmation
* Short forms
* Focused decisions
* Destructive action review
* Approval
* Error recovery

Complex workflows should use full pages.

## 111. Dialog Accessibility

Dialogs must:

* Trap focus
* Have an accessible title
* Provide a clear close action
* Return focus to the triggering element
* Support Escape where safe
* Prevent background interaction

## 112. Confirmation Dialog

A confirmation dialog should include:

* Action title
* Consequence
* Affected resource
* Reversibility
* Required reason
* Primary action
* Cancel action

## 113. Destructive Confirmation

For high-impact actions, the system may require:

* Reason
* Typed confirmation
* Step-up authentication
* Secondary approval
* Review summary

The exact control depends on action risk.

## 114. Alert Component

Alert variants:

* Informational
* Success
* Warning
* Critical

Alerts should include:

* Title
* Message
* Optional action
* Accessible announcement

## 115. Banner Component

Banners may communicate:

* Organization suspension
* Support session
* Emergency access
* Provider outage
* Maintenance period
* Critical deadline

Persistent banners should not obscure navigation.

## 116. Toast Component

Toasts may confirm low-risk transient results.

Examples:

* Draft saved
* Filter cleared
* Link copied

Toasts must not be the only confirmation for:

* Payment verification
* Agreement activation
* Refund execution
* Occupancy activation
* Membership revocation

## 117. Tooltip Component

Tooltips may explain:

* Icons
* Abbreviations
* Secondary details
* Disabled controls

Tooltips must not contain critical information unavailable elsewhere.

## 118. Popover Component

Popovers may contain:

* Contextual information
* Lightweight forms
* Quick details
* Date selection

They must support keyboard and mobile interaction.

## 119. Progress Indicator

Progress indicators should support:

* Determinate progress
* Indeterminate progress
* File upload progress
* Long-running jobs
* Multi-step completion

Do not display fake precision.

## 120. Skeleton Loading

Skeletons may preserve layout during loading.

They must not:

* Mimic sensitive data
* Create misleading status
* Continue indefinitely without fallback

## 121. Spinner Use

Spinners should be used for brief waits.

Long-running work should provide:

* Status
* Expected next step
* Cancellation where supported
* Background-processing option
* Retry

## 122. Empty-State Component

An empty state should include:

* Clear title
* Explanation
* Relevant action
* Optional guidance
* No misleading error tone

## 123. Error-State Component

Error states should include:

* Plain-language problem
* Retry
* Safe navigation
* Correlation ID where appropriate
* Support path
* No sensitive technical detail

## 124. Conflict-State Component

Conflict states should communicate:

* The record changed
* Who or what changed it where permitted
* Required refresh
* Whether unsaved input can be preserved
* Safe next step

## 125. Unauthorized-State Component

Unauthorized states should:

* Avoid exposing protected information
* State the access problem safely
* Offer a return path
* Explain expired assignment where appropriate
* Avoid suggesting access can be bypassed

## 126. Not-Found State

The not-found state may be used when:

* The record does not exist.
* The record is intentionally concealed.
* The route is invalid.
* The user lost access.

Security policy determines how much explanation is appropriate.

## 127. Network-Loss State

The interface should distinguish:

* Offline
* Slow network
* Request failed
* Save failed
* Upload paused
* Retry available

## 128. Offline Draft Indicator

Where draft recovery is supported, show:

```text
Saved on this device
Not yet synchronized
```

The interface must not present local-only data as server-confirmed.

## 129. Task Item Component

A task item should include:

* Task title
* Resource
* Priority
* Due date
* Status
* Property or organization context
* Assignment
* Required action

## 130. Task Priority Presentation

Priority should use:

* Text
* Icon
* Ordering
* Optional semantic tone

Avoid color-only urgency.

## 131. Notification Item Component

A notification item should include:

* Event
* Related resource
* Time
* Read state
* Required action
* Secure navigation link

Sensitive data should be minimized.

## 132. Activity Item Component

An activity item may show:

* Action
* Actor
* Date
* Previous state
* New state
* Reason
* System source
* Provider source
* Correlation ID where permitted

## 133. Approval Panel Pattern

The approval panel should include:

* Requested action
* Requester
* Resource
* Evidence
* Risk warnings
* Conflict status
* Approval history
* Approve
* Reject
* Return for revision

## 134. Separation-of-Duties Warning

Where self-approval is prohibited:

```text
Another authorized reviewer must approve this request because you prepared or initiated it.
```

## 135. Payment Verification Pattern

The payment-verification pattern should include:

* Payment amount
* Currency
* Payment date
* Payer
* Recipient
* Evidence
* Provider or bank reference
* Duplicate indicator
* Charge context
* Verification history
* Verify
* Reject
* Request information

## 136. Payment Evidence Status

Successful upload must display:

```text
Evidence submitted
Pending verification
```

It must not display:

```text
Payment verified
```

until server-confirmed verification.

## 137. Payment Allocation Pattern

The allocation component should show:

* Verified amount
* Already allocated
* Available balance
* Eligible charges
* Outstanding charge balances
* Proposed allocation
* Remaining payment balance
* Remaining charge balance
* Confirmation

## 138. Financial Warning Component

Financial warnings should be prominent for:

* Currency mismatch
* Duplicate reference
* Over-allocation
* Negative balance
* Reversed payment
* Refunded payment
* Settled charge
* Cross-organization mismatch
* Self-verification conflict

## 139. Charge Summary Component

A charge summary should include:

* Charge type
* Amount
* Currency
* Due date
* Outstanding amount
* Allocation status
* Waiver or adjustment
* Related agreement
* Related unit

## 140. Receipt Component

A receipt presentation should include:

* Receipt number
* Amount
* Currency
* Payment date
* Recipient
* Allocation
* Related charges
* Secure document action

## 141. Agreement Summary Component

An agreement summary should include:

* Agreement reference
* Version
* Parties
* Unit
* Term
* Annual rent
* Charges
* Signature status
* Activation status
* Effective date
* Expiry

## 142. Agreement Version Badge

Agreement versions should be labeled clearly:

```text
Version 1 — Draft
Version 2 — Issued
Version 2 — Signed
Version 3 — Amendment draft
```

## 143. Agreement Immutability Pattern

Issued and signed versions should:

* Be read-only
* Show an immutable indicator
* Provide version history
* Offer Create Amendment or Create New Version
* Avoid generic edit controls

## 144. Signature Pattern

The signature pattern should show:

* Agreement version
* Signer identity
* Required declaration
* Consent
* Signature method
* Date
* Decline where permitted
* Download or view agreement

## 145. Occupancy Summary Component

An occupancy summary should include:

* Occupancy reference
* Unit
* Primary Resident
* Co-Residents
* Start date
* End date
* Agreement
* Status
* Move-in status
* Move-out status

## 146. Occupancy Blocker Pattern

Before activation, show blockers such as:

* Agreement not active
* Required payment not verified
* Unit not ready
* Existing incompatible occupancy
* Missing move-in checklist
* Missing approval

## 147. Household Member Component

A household member entry should include:

* Name
* Relationship to occupancy
* Authority
* Start date
* End date
* Status
* Pending change

The interface must not imply equal contractual authority.

## 148. Maintenance Request Component

A maintenance request should display:

* Reference
* Category
* Priority
* Location
* Submitted date
* Status
* Assigned team
* Scheduled visit
* Latest update
* Reopen request

## 149. Work-Order Component

A work order should display:

* Reference
* Scope
* Priority
* Vendor
* Worker
* Quote
* Approved cost
* Schedule
* Status
* Completion evidence
* Verification state

## 150. Vendor Assignment Component

A vendor assignment should expose only:

* Approved work scope
* Necessary location
* Approved access instructions
* Schedule
* Cost information required for work
* Communication
* Completion requirements

## 151. Legal Matter Component

A legal matter should display:

* Matter reference
* Type
* Related resource
* Assigned lawyer
* Confidentiality
* Status
* Deadline
* Documents
* Notes
* Notices

Sensitive legal content requires restricted presentation.

## 152. Sensitive Data Component

Sensitive data components should support:

* Masking
* Redaction
* Reveal action
* Step-up authentication
* Permission check
* Access log
* Copy restriction where appropriate
* Clear sensitivity label

## 153. Sensitive Data Masking

Examples:

```text
Bank reference:
••••••••4821

Government ID:
••••••••7294
```

The final masking strategy must preserve usability without exposing unnecessary data.

## 154. Sensitive Data Reveal

Reveal actions should:

* Be explicit
* Be time limited where appropriate
* Require permission
* Be audited
* Avoid automatic display
* Hide again after context change

## 155. Government Identification Component

Government identification should not appear in:

* General tables
* Global search snippets
* Notification content
* Ordinary activity feeds
* Analytics
* Unprotected exports

## 156. Document Viewer Pattern

The document viewer should support:

* Preview
* Metadata
* Version
* Related resource
* Sensitivity
* Review status
* Download
* Replace
* Approve or reject where authorized
* Access history where permitted

## 157. Document Review Pattern

A reviewer should see:

* Document type
* Uploader
* Related resource
* Upload date
* Scan result
* Current version
* Review checklist
* Approve
* Reject
* Request replacement
* Internal note

## 158. AI-Assisted Component Standard

Every AI-assisted component must include:

* AI-generated label
* Human-review requirement
* Source context
* Editable output where appropriate
* Regenerate action
* Failure state
* Feedback mechanism where approved
* No automatic finalization

## 159. AI Summary Component

Label:

```text
AI-generated summary — Review before use
```

The source record must remain available.

## 160. AI Draft Component

AI-generated communication should remain:

```text
Draft
```

until reviewed and sent by an authorized user.

## 161. AI Recommendation Component

AI recommendations must not use authoritative labels such as:

```text
Approved
Rejected
Verified
Eligible
```

unless they represent server-confirmed human decisions.

## 162. AI Failure Component

Preferred message:

```text
AI assistance is temporarily unavailable. You can continue this task manually.
```

## 163. Content Design Principles

UI content should be:

* Clear
* Direct
* Specific
* Respectful
* Action oriented
* Consistent
* Legally appropriate
* Free from unnecessary jargon
* Appropriate for Nigerian users
* Ready for localization

## 164. Button Content Standards

Use verb-first labels.

Preferred:

```text
Submit application
Verify payment
Issue agreement
Assign vendor
Request information
```

Avoid:

```text
Proceed
Confirm
Yes
Do it
```

when the action can be named directly.

## 165. Status Content Standards

Use stable, approved labels.

Examples:

```text
Pending verification
Information required
Awaiting signature
Partially allocated
Move-in scheduled
```

## 166. Error Content Standards

Errors should explain:

* What happened
* What the user can do
* Whether work was preserved
* Whether support is needed

## 167. Confirmation Content Standards

Confirmation content should explain:

* The action
* Immediate consequence
* Downstream consequence
* Reversibility
* Required next step

## 168. Date and Time Formatting

Preferred:

```text
30 July 2026, 6:00 PM WAT
```

Use relative time only as supporting information:

```text
Due in 2 days
```

## 169. Currency Formatting

Preferred:

```text
₦500,000.00
```

Use:

```text
NGN 500,000.00
```

when currency ambiguity exists.

## 170. Unknown Data Presentation

Unknown or pending values should display:

* Not yet confirmed
* Pending verification
* Not provided
* Not applicable
* Unknown

Do not display zero unless zero is confirmed.

## 171. Pilot Property Design Rule

The design system must support the pilot structure without hardcoding it.

Example:

```text
Community 001
├── Existing Building
│   ├── Block A
│   │   └── Six confirmed residential units
│   └── Block B
├── Functional Masjid
└── Building Under Construction
    └── Waterside context
```

Unknown values must remain visibly unconfirmed.

## 172. Accessibility Standard

Habitacores should target:

```text
WCAG 2.2 Level AA
```

## 173. Contrast

Text, controls, focus indicators, status elements, and graphical objects must meet approved contrast requirements.

## 174. Keyboard Navigation

All functionality must be usable by keyboard.

This includes:

* Navigation
* Forms
* Menus
* Tables
* Filters
* Dialogs
* Drawers
* Tabs
* File uploads
* Date selection
* Approvals
* Status actions

## 175. Focus Visibility

Focus indicators must:

* Be clearly visible
* Meet contrast requirements
* Not be removed
* Follow component shape
* Remain visible against all backgrounds

## 176. Focus Order

Focus order should follow:

* Visual order
* Reading order
* Workflow order

Hidden or disabled content must not receive focus.

## 177. Screen Reader Support

Components must provide:

* Semantic roles
* Accessible names
* Descriptions
* Error announcements
* Status announcements
* Table headers
* Heading hierarchy
* Dialog titles
* Progress labels

## 178. Live Regions

Live regions may announce:

* Form validation
* Save result
* Loading completion
* Upload progress
* Task completion
* Notification count changes

Announcements must avoid excessive repetition.

## 179. Form Accessibility

Forms must support:

* Explicit labels
* Required state
* Error association
* Help association
* Logical field order
* Keyboard submission
* Error summary
* Focus recovery

## 180. Table Accessibility

Tables must include:

* Caption
* Header associations
* Sort-state announcement
* Selection-state announcement
* Keyboard-accessible actions
* Responsive accessible alternative

## 181. Dialog Accessibility

Dialogs must:

* Move focus inside
* Trap focus
* Announce title
* Support close behavior
* Return focus
* Prevent background interaction

## 182. Status Accessibility

Status must be communicated through text and programmatic meaning.

Color-only status is prohibited.

## 183. Motion Accessibility

Reduced-motion settings must be respected.

Flashing or seizure-risk content is prohibited.

## 184. Zoom and Reflow

The interface must remain usable under browser zoom and text enlargement.

Content should reflow without:

* Lost functionality
* Overlapping text
* Hidden actions
* Unusable horizontal scrolling

## 185. Mobile Interaction Standards

Mobile experiences should:

* Prioritize primary actions
* Use large touch targets
* Reduce table density
* Support bottom sheets or drawers carefully
* Preserve form progress
* Support file upload
* Avoid hover-dependent behavior

## 186. Mobile Sticky Actions

Long forms may use sticky actions such as:

* Save and exit
* Continue
* Submit

Sticky actions must not obscure content.

## 187. Mobile Dialogs

Large dialogs may become full-screen mobile sheets.

Critical context and actions must remain visible.

## 188. Low-Bandwidth Standards

Components should:

* Load text before media
* Use optimized images
* Avoid unnecessary animation
* Avoid excessive API requests
* Support retry
* Preserve draft state
* Avoid redownloading unchanged files
* Communicate network state

## 189. Image Loading

Images should use:

* Responsive sizing
* Compression
* Lazy loading where appropriate
* Placeholder
* Alternative text
* Failure fallback

Primary listing images may load earlier than secondary gallery images.

## 190. Progressive Enhancement

Core workflows should remain usable without advanced client-side enhancements where practical.

Critical server actions must not depend solely on client-side state.

## 191. Browser Support

The supported browser policy will be finalized later.

The design system should initially support modern versions of:

* Chrome
* Safari
* Edge
* Firefox
* Mobile Safari
* Mobile Chrome

## 192. Component Performance

Components should avoid:

* Excessive rerenders
* Large JavaScript bundles
* Heavy animation libraries
* Unbounded DOM lists
* Duplicate data requests
* Client-side processing of sensitive large files

## 193. Virtualization

Large operational lists may use virtualization where:

* Accessibility is preserved
* Keyboard navigation remains usable
* Print and export are handled separately
* Row heights are predictable

## 194. Design-System Documentation

Each component should be documented with:

* Purpose
* Usage
* Do
* Do not
* Anatomy
* Variants
* States
* Accessibility
* Responsive behavior
* Code example
* Test expectations
* Change history

## 195. Component Playground

A component-development environment should support:

* Isolated rendering
* State testing
* Accessibility testing
* Responsive testing
* Visual regression testing
* Documentation

A tool such as Storybook may be evaluated through an ADR.

## 196. Design Tokens in Code

Tokens should be:

* Centralized
* Versioned
* Typed where practical
* Reusable across components
* Exportable for future mobile use
* Protected from arbitrary overrides

## 197. Component API Standards

Component APIs should:

* Use predictable names
* Prefer composition
* Avoid excessive boolean props
* Support accessibility
* Support controlled and uncontrolled patterns where justified
* Avoid domain-specific coupling in primitives

## 198. Domain Pattern Components

Domain-specific components may be built above primitives.

Examples:

* PaymentVerificationPanel
* AgreementVersionHeader
* OccupancyBlockerList
* ApplicationReviewChecklist
* WorkOrderStatusTimeline

## 199. Visual Regression Testing

Visual tests should cover:

* Default states
* Error states
* Loading states
* Mobile layouts
* High-contrast cases
* Long text
* Missing text
* Unknown data
* Permission-limited states

## 200. Component Unit Testing

Tests should verify:

* Rendering
* Interaction
* Keyboard use
* Accessibility
* Loading
* Error
* Disabled behavior
* Event callbacks
* State transitions

## 201. Accessibility Testing

Testing should include:

* Automated accessibility scans
* Keyboard-only testing
* Screen-reader testing
* Zoom testing
* Contrast testing
* Reduced-motion testing

Automated scans do not replace manual testing.

## 202. Content Testing

Content should be tested for:

* Clarity
* Consistency
* Action understanding
* Error recovery
* Financial interpretation
* Legal interpretation
* Localization readiness

## 203. Design Review Checklist

Every new component or pattern should review:

1. User purpose
2. Reuse potential
3. Existing equivalent
4. Accessibility
5. Mobile behavior
6. Low-bandwidth behavior
7. Permission behavior
8. Loading state
9. Empty state
10. Error state
11. Conflict state
12. Sensitive data
13. Content
14. Analytics impact
15. Test coverage
16. Documentation

## 204. UI Acceptance Criterion — Payment Evidence

```text
Given:
An applicant uploads payment evidence.

When:
The upload and scan succeed.

Then:
The interface displays Evidence submitted and Pending verification.

And:
The charge remains outstanding until server-confirmed allocation.

And:
No success styling implies verified payment.
```

## 205. UI Acceptance Criterion — Agreement Immutability

```text
Given:
Agreement Version 2 has been issued.

When:
An authorized user views Version 2.

Then:
The agreement is presented as read-only.

And:
The interface displays the version and issued status.

And:
Material changes require Create Amendment or Create New Version.
```

## 206. UI Acceptance Criterion — Occupancy Blockers

```text
Given:
A planned occupancy has unresolved activation blockers.

When:
An authorized user opens the occupancy record.

Then:
Each blocker is displayed clearly.

And:
The Activate Occupancy action is disabled or denied with explanation.

And:
No visual state implies active occupancy.
```

## 207. UI Acceptance Criterion — Self-Approval

```text
Given:
A user prepared a refund request.
The organization prohibits self-approval.

When:
The user opens the approval panel.

Then:
The Approve action is unavailable.

And:
The panel explains that another authorized approver is required.
```

## 208. UI Acceptance Criterion — Unknown Pilot Data

```text
Given:
Block B unit count is not confirmed.

When:
The user views Block B.

Then:
The interface displays Not yet confirmed.

And:
The interface does not display zero.

And:
The interface does not generate placeholder units.
```

## 209. UI Acceptance Criterion — AI Output

```text
Given:
An authorized reviewer requests an AI-generated application summary.

When:
The summary is returned.

Then:
The component labels the content AI-generated summary — Review before use.

And:
The source application remains accessible.

And:
No application decision changes automatically.
```

## 210. UI Acceptance Criterion — Keyboard Navigation

```text
Given:
A user navigates the payment-verification workspace using only a keyboard.

When:
The user reviews evidence, opens supporting information, selects an action, and completes confirmation.

Then:
All controls are reachable in logical order.

And:
Focus remains visible.

And:
The dialog returns focus correctly.

And:
No mouse interaction is required.
```

## 211. UI Acceptance Criterion — Mobile Application Form

```text
Given:
An applicant uses a supported mobile browser.

When:
The applicant completes a multi-step application and uploads documents.

Then:
Labels, fields, validation, progress, upload state, save state, and submission controls remain usable.

And:
No desktop-only interaction is required.
```

## 212. UI Traceability

Each component or pattern should map to:

* UX screen
* User role
* User journey
* Permission
* Business rule
* API state
* Domain entity
* Accessibility requirement
* Test
* Design token

Example:

```text
Component:
PaymentVerificationPanel

Roles:
Finance Reviewer

Permissions:
payment.read
payment.review_evidence
payment.verify
payment.reject

APIs:
GET /payments/{paymentId}
POST /payments/{paymentId}/verify
POST /payments/{paymentId}/reject

States:
Pending verification
Verified
Rejected
Information required

Tests:
Authorization-limited actions
Keyboard flow
Duplicate warning
Loading
Error
Conflict
```

## 213. MVP Foundation Scope

The MVP design system must include:

* Color tokens
* Typography tokens
* Spacing tokens
* Radius tokens
* Elevation tokens
* Breakpoints
* Focus styles
* Layout primitives
* Responsive containers
* Accessibility utilities

## 214. MVP Primitive Scope

The MVP should include:

* Text
* Box
* Stack
* Inline
* Grid
* Divider
* Icon
* Surface
* Visually hidden
* Portal layer

## 215. MVP General Component Scope

The MVP should include:

* Button
* Link
* Input
* Text area
* Select
* Combobox
* Checkbox
* Radio group
* Switch
* File upload
* Form field
* Error summary
* Card
* Table
* List
* Badge
* Alert
* Banner
* Toast
* Tooltip
* Dialog
* Drawer
* Tabs
* Accordion
* Menu
* Dropdown
* Breadcrumb
* Pagination
* Search
* Filter
* Skeleton
* Spinner
* Progress
* Empty state
* Error state
* Unauthorized state

## 216. MVP Navigation Scope

The MVP should include:

* Public header
* Authenticated header
* Organization switcher
* Role indicator
* Property context selector
* Side navigation
* Mobile navigation
* User menu
* Notification indicator
* Task indicator

## 217. MVP Domain Component Scope

The MVP should include:

* Record header
* Status timeline
* Activity timeline
* Task item
* Notification item
* Approval panel
* Application progress
* Application review checklist
* Payment summary
* Payment verification panel
* Payment allocation panel
* Charge summary
* Receipt summary
* Agreement summary
* Agreement version header
* Signature panel
* Occupancy summary
* Occupancy blocker list
* Household member list
* Maintenance request summary
* Work-order summary
* Vendor assignment summary
* Sensitive-data reveal
* Document viewer
* AI summary
* AI draft

## 218. Deferred Design-System Capabilities

The following may be deferred:

* Native mobile design tokens
* Full dark mode
* Customer-selectable themes
* White-label branding
* Advanced data visualization library
* Advanced mapping components
* Real-time collaborative editing
* Drag-and-drop workflow builder
* Voice interaction
* Biometric components
* Smart-building controls
* Spatial interfaces
* Public developer component kit
* External embeddable widgets
* Advanced motion system
* AI-generated UI
* User-created dashboards
* Custom component marketplace
* Multi-brand theming
* Full offline component framework

Deferred capabilities must not compromise MVP consistency.

## 219. Design-System Risks

### 219.1 Inconsistent Component Use

Risk:

Teams create similar components with different behavior.

Controls:

* Central component library
* Documentation
* Code review
* Linting
* Design review

### 219.2 Visual Status Misrepresentation

Risk:

A visual style implies approval, verification, or activation prematurely.

Controls:

* Approved semantic tokens
* Status mappings
* Domain tests
* Content standards

### 219.3 Accessibility Regression

Risk:

New components are inaccessible.

Controls:

* Automated tests
* Manual review
* Keyboard testing
* Screen-reader testing
* Component acceptance criteria

### 219.4 Mobile Failure

Risk:

Operational and personal workflows fail on mobile.

Controls:

* Responsive component standards
* Mobile test cases
* Touch-target standards
* Table alternatives

### 219.5 Arbitrary Styling

Risk:

Implementation uses unapproved colors, spacing, or component variants.

Controls:

* Tokens
* Component APIs
* Linting
* Pull-request review
* Visual regression tests

### 219.6 Sensitive Data Exposure

Risk:

Reusable components display sensitive data by default.

Controls:

* Masking components
* Explicit reveal
* Permission checks
* Safe defaults
* Access auditing

### 219.7 Brand Lock-In

Risk:

Early visual decisions make future branding costly.

Controls:

* Semantic tokens
* Brand abstraction
* Theme layer
* Avoid hardcoded values

### 219.8 Excessive Design Complexity

Risk:

The system becomes too large before MVP implementation.

Controls:

* MVP component scope
* Reuse thresholds
* Deferred capabilities
* Governance

## 220. Unresolved Design-System Decisions

The following remain pending:

* Final logo
* Final brand mark
* Final wordmark
* Final primary color
* Final secondary color
* Final accent color
* Final neutral palette
* Final success color
* Final warning color
* Final critical color
* Final pending-status color
* Final typography family
* Final typography scale
* Final spacing scale
* Final radius scale
* Final shadow scale
* Final icon library
* Final illustration style
* Final photography guidance
* Final breakpoint values
* Final page-container widths
* Final sidebar width
* Final navigation-collapse behavior
* Final control heights
* Final table-density defaults
* Final status-badge mapping
* Final charting library
* Final date-picker component
* Final combobox implementation
* Final file-upload library
* Final document-viewer provider
* Final rich-text editor
* Final notification behavior
* Final toast duration
* Final autosave feedback behavior
* Final skeleton strategy
* Final reduced-motion implementation
* Final design-token format
* Final CSS strategy
* Final component library strategy
* Final Storybook decision
* Final visual-regression platform
* Final accessibility-testing tools
* Final dark-mode timeline
* Final localization typography support
* Final public marketplace imagery
* Final pilot public names
* Final Block A unit labels
* Final Block B inventory
* Final second-building structure
* Final water-feature classification

These decisions must be resolved before the affected component becomes production dependent.

## 221. Design-System Governance

Changes to the approved design system require:

1. Change description
2. User problem
3. Existing component analysis
4. Proposed component or token
5. UX impact
6. Accessibility impact
7. Mobile impact
8. Low-bandwidth impact
9. Security impact
10. Sensitive-data impact
11. Financial-state impact
12. Agreement-state impact
13. Occupancy-state impact
14. API-state impact
15. Performance impact
16. Testing impact
17. Documentation impact
18. Migration impact
19. Deprecation plan
20. Product Visionary approval
21. Chief Product Architect approval
22. Effective version

## 222. Component Change Classification

Changes may be classified as:

### Patch

Examples:

* Documentation correction
* Non-breaking accessibility improvement
* Bug fix
* Visual correction within approved tokens

### Minor

Examples:

* New component variant
* New optional prop
* New pattern
* New semantic token

### Major

Examples:

* Removed component
* Changed component behavior
* Incompatible component API
* Changed status semantics
* Changed interaction model

## 223. Component Deprecation

Deprecated components should provide:

* Deprecation notice
* Replacement
* Migration guidance
* Removal version
* Usage tracking
* Compatibility period

## 224. Current UI Design-System Decision

Habitacores will use a semantic-token-based, accessible, responsive, role-aware design system organized as:

```text
Foundations
→ Tokens
→ Primitives
→ Components
→ Domain Patterns
→ Templates
→ Product Screens
```

The interaction model will preserve:

```text
User Intent
→ Visible Context
→ Authorized Control
→ Clear Input
→ Validation
→ Explicit Confirmation
→ Server-Confirmed Result
→ Status Feedback
→ Activity and Audit
```

The system must preserve:

* Organization clarity
* Role clarity
* Assignment scope
* Accessible interaction
* Mobile usability
* Low-bandwidth usability
* Explicit workflow state
* Financial truth
* Payment-verification distinction
* Agreement immutability
* Occupancy integrity
* Sensitive-data protection
* Human approval
* AI transparency
* Error recovery
* Consistent implementation
* Future brand flexibility
* Future mobile extensibility

No color, badge, animation, uploaded file, payment screenshot, disabled button, hidden navigation item, client-side state, cached page, AI output, provider callback, role label, direct URL, pilot-team relationship, or optimistic interface response may independently authorize, verify, approve, activate, settle, terminate, or otherwise finalize a protected Habitacores business action.