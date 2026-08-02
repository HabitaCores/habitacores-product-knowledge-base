# Implementation Baseline 001 — Repository Governance and CI Quality Gates

## Implementation Record

| Field | Value |
|---|---|
| Product | Habitacores |
| Work Package | Implementation Baseline 001 |
| Version | 0.1 |
| Status | Proposed |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Implementation Area | GitHub governance and continuous integration |
| Authoritative Branch | `main` |
| Merge Method | Pull request only |
| Runtime | Node.js 24 LTS |
| Package Manager | pnpm 11 |
| Monorepo Orchestration | Turborepo |
| CI Platform | GitHub Actions |
| Lockfile Policy | One root `pnpm-lock.yaml`; frozen in CI |
| Production Credentials in PR CI | Prohibited |
| Direct Production Deployment | Not included in this baseline |
| Implementation Status | Not yet implemented |
| Last Updated | 2 August 2026 |

## 1. Purpose

This work package converts approved Habitacores architecture decisions into
enforceable source-control and continuous-integration controls.

It implements the initial engineering guardrails required before substantial
application code, database migrations, infrastructure definitions, mobile
code, provider adapters, or AI capabilities are generated.

This document does not claim that the controls already exist. Completion
requires verified GitHub configuration, committed workflows, successful CI
runs, and recorded evidence.

## 2. Governing Decisions

This baseline implements controls derived from:

- ADR-001 — Application Repository and Workspace Structure
- ADR-003 — Package Manager, Node.js Version, and Monorepo Tooling
- ADR-004 — PostgreSQL Access, ORM, and Migration Strategy
- ADR-013 — Environments, Configuration, Secrets, Deployment, and Release Strategy
- ADR-018 — Feature Flags, Dynamic Configuration, Progressive Delivery, and Kill Switches
- ADR-019 — Infrastructure as Code, State Management, Provisioning, and Drift Control
- ADR-022 — Software Supply Chain, Dependency, Build Integrity, and Provenance
- ADR-023 — Security Engineering, Threat Modeling, Verification, and Incident Response
- ADR-025 — Accessibility, Inclusive Design, Localization, Low-Bandwidth, and Assisted Use

## 3. Implementation Outcome

After completion:

```text
Contributor change
→ Feature branch
→ Pull request
→ Required CI checks
→ Human review
→ Protected merge
→ Immutable commit on main
→ Traceable implementation evidence
````

No ordinary contributor, coding agent, dependency bot, or pull-request
workflow will be able to bypass the required checks or independently deploy
production.

## 4. Repository Protection

The authoritative `main` branch must require:

* Pull requests before merging
* at least one approving human review initially
* dismissal or reevaluation of stale approval after material changes
* resolution of review conversations
* required status checks
* branch up to date before merge where practical
* restricted force pushes
* restricted branch deletion
* restricted bypass
* attributable merge history

Direct unreviewed commits to `main` are prohibited.

## 5. Administrative Bypass

Repository administrators should not routinely bypass branch protection.

An emergency bypass requires:

* Identified emergency
* accountable actor
* reason
* minimal change
* validation evidence
* retrospective pull request or review
* post-change verification
* follow-up remediation where controls were skipped

## 6. CODEOWNERS

The repository should introduce a `.github/CODEOWNERS` file.

Directional ownership:

```text
/.github/                         @HabitaCores/platform-maintainers
/infra/                           @HabitaCores/platform-maintainers
/apps/web/                        @HabitaCores/application-maintainers
/apps/mobile/                     @HabitaCores/mobile-maintainers
/apps/workers/                    @HabitaCores/platform-maintainers
/packages/auth*/                  @HabitaCores/security-maintainers
/packages/authorization*/         @HabitaCores/security-maintainers
/packages/payments*/              @HabitaCores/finance-maintainers
/packages/agreements*/            @HabitaCores/legal-workflow-maintainers
/packages/ai*/                    @HabitaCores/responsible-ai-maintainers
/drizzle/                         @HabitaCores/data-maintainers
/adr/                             @HabitaCores/architecture-maintainers
/implementation/                  @HabitaCores/architecture-maintainers
```

Exact GitHub teams will be created only when the corresponding maintainers
exist. Until then, individual repository owners may be used temporarily.

## 7. Pull-Request Template

Every implementation pull request must state:

* Purpose
* scope
* governing ADRs
* affected applications and packages
* database impact
* infrastructure impact
* security impact
* privacy impact
* accessibility impact
* AI impact
* test evidence
* deployment or migration considerations
* rollback or forward-repair direction
* screenshots or recordings where relevant
* unresolved risks

A coding agent must not mark human review items complete merely because it
generated the code.

## 8. Issue and Work-Package Traceability

Material implementation must link to:

* An approved ADR or product requirement
* an implementation work package
* a GitHub issue or equivalent tracked task
* the resulting pull request
* test and release evidence

Architecture approval does not imply implementation completion.

## 9. Runtime Enforcement

The repository will declare Node.js 24 and pnpm 11 through approved mechanisms
such as:

* `package.json` engines
* `packageManager`
* `.nvmrc` or equivalent developer-runtime declaration
* CI setup actions
* optional Corepack enforcement

Local and CI runtime declarations must agree.

## 10. Lockfile Enforcement

The repository will use one root:

```text
pnpm-lock.yaml
```

CI installation must use:

```bash
pnpm install --frozen-lockfile
```

CI must fail when:

* A package manifest changes without the lockfile.
* dependency resolution attempts to mutate the lockfile.
* the wrong package-manager version is used.
* an unsupported lockfile version is introduced.
* a second authoritative lockfile appears in the workspace.

## 11. Initial Required CI Checks

The initial required checks are:

```text
repository-policy
format
lint
typecheck
unit-tests
integration-tests
build
dependency-review
secret-scan
code-security
workflow-security
documentation-validation
```

Infrastructure, database, mobile, accessibility, and end-to-end checks will be
required when the corresponding directories and implementations exist.

## 12. Repository Policy Check

The repository-policy job should verify:

* Supported Node.js version
* supported pnpm version
* one root lockfile
* no prohibited lockfiles
* no committed `.env` secrets
* no committed state or plan files
* no production credentials
* approved workspace structure
* no undeclared generated artifacts
* registered feature keys where applicable
* registered AI use cases where applicable
* documentation links remain valid where practical

## 13. Formatting

Formatting must be deterministic.

CI should run the repository-approved formatter in check mode.

The exact formatter configuration will be committed and version controlled.

Automatic formatting may be available locally, but CI must not silently alter
the submitted pull-request source.

## 14. Linting

Linting will cover:

* TypeScript
* React
* Next.js
* React Native
* Node.js workers
* security-sensitive patterns
* accessibility-sensitive JSX patterns
* import boundaries
* package boundaries
* prohibited provider-SDK usage where enforceable

Linting does not replace type checking or security testing.

## 15. Type Checking

Every TypeScript workspace package must participate in type checking.

CI must fail on:

* Type errors
* invalid project references
* incompatible shared contracts
* missing generated types required by the build
* accidental client imports of server-only modules

## 16. Unit Tests

Unit tests must cover deterministic domain behavior.

Priority areas include:

* Money arithmetic
* identifier handling
* authorization decisions
* workflow state machines
* payment invariants
* agreement invariants
* feature evaluation defaults
* AI output validation
* retention calculations
* localization formatting

## 17. Integration Tests

Integration tests should cover:

* PostgreSQL behavior
* RLS
* transaction-local tenant context
* migrations
* outbox behavior
* idempotency
* provider-adapter contracts
* storage metadata relationships
* audit-event creation
* worker retries and crash recovery

Integration tests must use isolated non-production resources.

## 18. Build Validation

The pull request must build every affected deployable surface.

Directional targets:

```text
apps/web
apps/mobile
apps/workers/*
packages/*
```

The build must not use production credentials or production tenant data.

## 19. Path-Based Execution

CI may use path-based change detection to avoid unnecessary work.

However:

* Shared-package changes must trigger dependent applications.
* configuration changes must trigger all affected checks.
* workflow changes must trigger workflow-security review.
* migration changes must trigger database checks.
* authorization changes must trigger tenant-isolation tests.
* design-system changes must trigger accessibility checks.
* lockfile changes must trigger dependency review.

Path optimization must not permit unsafe check omission.

## 20. Dependency Review

Pull requests changing dependencies must receive:

* Manifest review
* lockfile review
* vulnerability analysis
* license identification
* lifecycle-script review
* native-binary review where applicable
* bundle or build impact
* provider-SDK boundary review

Critical applicable vulnerabilities block merge unless an approved,
time-bounded exception exists.

## 21. Secret Scanning

CI and repository controls must detect:

* API keys
* database URLs
* private keys
* signing secrets
* provider credentials
* webhook secrets
* mobile signing material
* infrastructure credentials
* access tokens

A committed production secret must be treated as compromised and rotated.

Deleting it from the latest commit alone is insufficient.

## 22. Code Security

Static security analysis should cover:

* Injection
* insecure deserialization
* unsafe file handling
* SSRF
* command execution
* authorization omissions
* cryptographic misuse
* credential exposure
* unsafe redirect handling
* cross-site scripting
* insecure client/server boundaries

Findings require triage and ownership.

## 23. Workflow Security

GitHub Actions validation must detect:

* Broad default permissions
* unpinned sensitive third-party actions
* unsafe use of `pull_request_target`
* secrets exposed to pull-request code
* production authority in ordinary PR workflows
* uncontrolled shell interpolation
* untrusted artifact execution
* environment crossover
* mutable deployment identity

## 24. Workflow Permissions

Every workflow must declare explicit permissions.

The default should be:

```yaml
permissions:
  contents: read
```

Additional permissions must be granted only to the specific job requiring
them.

Pull-request validation workflows must not receive:

* Production deployment authority
* infrastructure apply authority
* package-publishing authority
* backup authority
* mobile-store submission authority
* production database credentials

## 25. GitHub Action Pinning

Sensitive workflows must pin third-party actions to immutable commit SHAs.

A version comment may be included for maintainability.

Action upgrades require a reviewed pull request and successful validation.

## 26. Untrusted Pull Requests

Untrusted pull-request code must execute without production secrets.

The workflow must not:

* Deploy production
* apply infrastructure
* publish packages
* access production state
* access production data
* access production backups
* submit mobile applications
* invoke production provider operations

## 27. Concurrency

Pull-request CI should cancel superseded runs for the same branch where safe.

Production or migration workflows must use separate concurrency controls and
must not be canceled in ways that leave authoritative operations ambiguous.

## 28. CI Caching

Caches may improve performance but must not become build authority.

Caching must be:

* Keyed to relevant inputs
* environment scoped
* free of secrets
* replaceable
* safely invalidated
* prevented from changing dependency resolution

The build must remain correct without a cache.

## 29. CI Artifacts

Permitted CI artifacts may include:

* Test reports
* coverage reports
* accessibility reports
* build metadata
* non-sensitive logs
* generated SBOMs
* security scan summaries
* migration validation results

Artifacts must not contain:

* Production secrets
* unrestricted environment files
* private tenant data
* raw production database content
* private signing material
* unrestricted AI prompts or outputs

## 30. Documentation Validation

Documentation checks should verify:

* Required headings
* ADR numbering
* approval blocks
* relative links
* file naming
* duplicate identifiers
* markdown quality
* prohibited implementation claims
* roadmap and index consistency

A documentation check must not alter the decision status automatically.

## 31. Migration Validation

When migrations are introduced, CI must verify:

* Migration files are committed.
* generated migration metadata is consistent.
* migrations apply to a clean test database.
* migrations apply from the previous supported schema state.
* schema drift is detected.
* destructive changes are identified.
* RLS remains enabled where required.
* runtime roles remain non-owner and without `BYPASSRLS`.

Production migration execution is outside ordinary pull-request CI.

## 32. Infrastructure Validation

When `infra/` is introduced, CI must run:

* OpenTofu formatting
* initialization
* validation
* provider-lock verification
* static scanning
* policy checks
* plan generation with restricted credentials where approved
* destructive-change detection

Pull-request CI must not apply production infrastructure.

## 33. Accessibility Validation

When user interfaces are introduced, CI must add automated accessibility
checks for:

* Semantic markup
* labels
* accessible names
* selected contrast issues
* keyboard-sensitive component rules
* ARIA misuse
* document-generation checks where feasible

Manual accessibility testing remains separately required.

## 34. Mobile Validation

When mobile implementation begins, CI must validate:

* TypeScript
* Expo configuration
* route configuration
* native dependency compatibility
* permissions
* bundle identifiers
* build-number rules
* secure-storage boundaries
* mobile accessibility checks
* absence of server secrets in the mobile bundle

Production signing and store submission remain outside ordinary PR CI.

## 35. AI Boundary Validation

When AI implementation begins, CI should verify:

* Provider SDKs appear only in approved adapters.
* production use cases are registered.
* prompts are versioned.
* model names are not scattered in feature code.
* output schemas exist.
* manual fallback tests exist.
* protected workflows cannot accept raw AI output authoritatively.
* provider keys cannot enter clients.

## 36. Required Check Naming

Required check names should remain stable.

Renaming a required check requires coordinated GitHub branch-protection
updates.

A workflow must not bypass a required check merely by changing the job name.

## 37. Merge Strategy

The initial merge strategy should preserve:

* Pull-request identity
* commit attribution
* approved change history
* simple release traceability

Squash merge may be used where the final commit message accurately represents
the pull request and its evidence.

The exact merge-method policy must remain consistent across the engineering
repository.

## 38. Coding-Agent Boundaries

Codex and other coding agents may:

* Create branches
* modify source
* add tests
* run checks
* generate pull-request drafts
* explain failures
* propose dependency changes

They may not:

* Approve their own work
* merge to `main`
* bypass checks
* alter branch protection
* obtain production credentials
* deploy production
* apply production infrastructure
* execute production migrations
* suppress security findings without authorization

## 39. Initial Workflow Structure

Directional structure:

```text
.github/
  CODEOWNERS
  pull_request_template.md
  workflows/
    ci.yml
    dependency-review.yml
    security.yml
    documentation.yml
```

The structure may be split further when applications and infrastructure are
introduced.

## 40. CI Workflow Direction

The main CI workflow should follow:

```text
Checkout immutable pull-request source
→ Set up Node.js 24
→ Enable approved pnpm 11
→ Install with frozen lockfile
→ Restore safe cache
→ Validate repository policy
→ Format check
→ Lint
→ Type check
→ Unit tests
→ Integration tests where available
→ Build affected workspaces
→ Upload bounded test evidence
```

## 41. Implementation Evidence

Completion evidence must include:

* GitHub branch-protection screenshot or exported rules
* committed `CODEOWNERS`
* committed pull-request template
* committed CI workflows
* successful CI run on a test pull request
* failed CI demonstration for an intentional policy violation
* confirmation that PR workflows have no production credentials
* confirmation that direct ordinary pushes to `main` are blocked
* list of required checks
* unresolved limitations and follow-up issues

## 42. Acceptance Criteria

This baseline is complete only when:

* `main` is protected.
* ordinary direct pushes are blocked.
* pull requests are required.
* at least one human approval is required.
* required checks are active.
* Node.js and pnpm versions are enforced.
* frozen-lockfile installation is enforced.
* formatting, linting, type checking, tests, and builds execute.
* dependency and secret checks execute.
* workflow permissions are explicit.
* untrusted pull-request code has no production authority.
* implementation evidence is recorded.
* all exceptions have owners and expiry.

## 43. Not Included

This baseline does not implement:

* Production Vercel deployment
* Supabase production provisioning
* database production migrations
* worker hosting
* mobile store release
* payment providers
* signature providers
* AI providers
* production observability
* production backups
* full SBOM and artifact-provenance promotion
* complete penetration testing
* formal accessibility conformance

Those belong to subsequent implementation packages.

## 44. Risks

### 44.1 Excessive CI Duration

Controls:

* Path-aware execution
* Turborepo caching
* parallel independent jobs
* cancellation of superseded PR runs
* performance budgets

### 44.2 False Confidence

Controls:

* Manual review
* integration testing
* security review
* accessibility testing
* production verification
* no claim that CI proves complete correctness

### 44.3 Administrator Bypass

Controls:

* Restricted bypass
* audit
* emergency-change procedure
* retrospective review

### 44.4 Agent-Generated Unsafe Change

Controls:

* Human approval
* required checks
* dependency review
* no production credentials
* protected branch

### 44.5 Required Check Evasion

Controls:

* Stable check names
* branch-protection review
* workflow-change ownership
* repository-policy tests

## 45. Implementation Sequence

```text
1. Confirm GitHub organization ownership and MFA
2. create the application repository from ADR-001
3. establish the Node.js 24 and pnpm 11 root configuration
4. establish pnpm workspaces and Turborepo
5. create CODEOWNERS
6. create the pull-request template
7. configure main branch protection
8. implement the repository-policy check
9. implement format, lint, and type-check jobs
10. implement unit-test and initial integration-test jobs
11. implement affected-workspace build validation
12. implement dependency review
13. implement secret and code-security scanning
14. implement workflow-security checks
15. implement documentation validation
16. verify explicit workflow permissions
17. verify untrusted pull-request isolation
18. demonstrate a successful controlled pull request
19. demonstrate blocked unsafe changes
20. record evidence and unresolved follow-up work
```

## 46. Final Implementation Statement

Habitacores will establish protected source-control and continuous-integration
controls before substantial production application development proceeds.

The `main` branch will accept ordinary changes only through reviewed pull
requests that pass stable required checks.

The repository will enforce Node.js 24, pnpm 11, one root lockfile, and
frozen-lockfile dependency installation.

CI will validate repository policy, formatting, linting, types, tests, builds,
dependencies, secrets, source security, workflow security, and documentation.
Additional migration, infrastructure, accessibility, mobile, and AI checks
will become required when those implementation surfaces exist.

Pull-request workflows will use explicit least-privilege permissions and will
not receive production deployment, infrastructure, database, backup,
publishing, mobile-store, or provider authority.

Coding agents may contribute through branches and pull requests but will not
approve, merge, bypass checks, alter repository protection, or access
production authority.

Completion will be demonstrated through working GitHub protections,
committed workflows, successful and intentionally failed CI runs, and recorded
implementation evidence.

Architecture approval is not implementation completion. This work package
will remain proposed until the controls are configured, tested, and verified.