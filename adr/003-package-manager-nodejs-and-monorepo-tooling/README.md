# ADR-003 — Package Manager, Node.js Version, and Monorepo Tooling

## Decision Record

| Field | Value |
|---|---|
| Product | Habitacores |
| Decision | Package Manager, Node.js Version, and Monorepo Tooling |
| ADR | ADR-003 |
| Version | 0.1 |
| Status | Proposed |
| Decision Type | Foundational Engineering Toolchain |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Repository Model | TypeScript monorepo |
| Business Architecture | Modular monolith |
| Node.js Baseline | Node.js 24 LTS |
| Package Manager | pnpm 11 |
| Workspace Model | pnpm workspaces |
| Task Orchestration | Turborepo |
| Lockfile Model | One committed root lockfile |
| Dependency Linking | pnpm isolated workspace model |
| CI Installation | Frozen and deterministic |
| Initial Build Cache | Local |
| Remote Cache | Deferred pending security review |
| Last Updated | 1 August 2026 |

## 1. Context

ADR-001 established that Habitacores will use a TypeScript monorepo containing:

- A Next.js web application
- a deterministic background worker
- an independently bounded AI worker
- shared internal packages
- Responsible AI infrastructure
- infrastructure definitions
- engineering tooling
- automated tests
- implementation documentation

ADR-002 established the Next.js App Router and Node.js runtime model.

ADR-003 must now select the foundational JavaScript and TypeScript toolchain used to:

- Run the application
- install dependencies
- manage workspaces
- execute repository tasks
- coordinate builds
- coordinate tests
- enforce package boundaries
- provide deterministic development environments
- support continuous integration
- support secure dependency management
- support Codex implementation work
- support future expansion

The selected toolchain must remain practical for a small initial team while supporting the larger workspace defined by ADR-001.

## 2. Decision Drivers

The decision is driven by the need to:

1. Use a supported long-term Node.js release.
2. support modern Next.js.
3. support TypeScript throughout the repository.
4. manage multiple applications and internal packages.
5. produce deterministic installations.
6. minimize dependency duplication.
7. enforce declared package dependencies.
8. maintain one authoritative lockfile.
9. support local and CI task orchestration.
10. support incremental builds.
11. support affected-package execution.
12. support secure dependency lifecycle-script controls.
13. reduce supply-chain risk.
14. prevent developer toolchain drift.
15. support macOS, Linux, and CI environments.
16. support future engineers without complicated onboarding.
17. support Codex with stable commands and boundaries.
18. avoid unnecessary proprietary lock-in.
19. avoid premature custom build infrastructure.
20. support future remote caching without making it an MVP dependency.

## 3. Decision

Habitacores will use:

```text
Runtime:
Node.js 24 LTS

Package manager:
pnpm 11

Workspace manager:
pnpm workspaces

Task orchestrator:
Turborepo

Lockfile:
One committed root pnpm-lock.yaml

CI install mode:
pnpm install --frozen-lockfile

Workspace dependency protocol:
workspace:*

Initial task cache:
Local Turborepo cache

Remote cache:
Disabled until separately reviewed and approved
````

The repository will pin the exact approved pnpm version.

The repository will target the Node.js 24 major release and will use an approved exact minor and patch version in CI and reproducible development configuration.

## 4. Node.js Version Decision

Habitacores will use Node.js 24 LTS as its primary JavaScript runtime baseline.

Node.js 24 will be used by:

* Local development
* automated tests
* build processes
* Next.js
* deterministic workers
* AI workers
* migration tooling
* repository scripts
* CI pipelines
* production runtimes where supported

## 5. Node.js Version Policy

The repository will distinguish:

### Major Runtime Policy

```text
Node.js 24 LTS
```

This is the approved compatibility line.

### Exact Runtime Pin

An exact approved release will be recorded in runtime-management files and CI.

Illustrative value:

```text
24.x.y
```

The exact patch should be selected when the application repository is initialized.

The repository must not use an unbounded value such as:

```text
latest
```

for production or CI.

## 6. Runtime Declaration

The root `package.json` should declare the supported Node.js range.

Illustrative configuration:

```json
{
  "engines": {
    "node": ">=24.0.0 <25"
  }
}
```

Where supported and approved, the repository may additionally use:

```json
{
  "devEngines": {
    "runtime": {
      "name": "node",
      "version": "24.x.y",
      "onFail": "download"
    }
  }
}
```

Exact syntax must be validated against the selected pnpm release during repository initialization.

## 7. Runtime Pinning Files

The application repository should include at least one broadly supported runtime-version file.

Preferred:

```text
.node-version
```

Potential supplementary file:

```text
.nvmrc
```

Both should contain the same approved exact Node.js release.

Example:

```text
24.x.y
```

The files must not contradict:

* `package.json`
* CI configuration
* production configuration
* container configuration
* pnpm runtime configuration

## 8. Node.js Upgrade Policy

Node.js updates are divided into:

### Patch Update

Normally allowed through a standard dependency and runtime maintenance pull request after:

* CI passes
* build passes
* tests pass
* security checks pass
* staging validation passes

### Minor Update

Requires:

* Release-note review
* framework compatibility validation
* provider SDK validation
* database library validation
* worker validation
* staging validation

### Major Update

Requires:

* Architecture review
* compatibility assessment
* migration plan
* dependency assessment
* CI and hosting validation
* rollback or forward-recovery plan
* Chief Product Architect review

## 9. Unsupported Runtime Prevention

The project should fail clearly when an unsupported runtime is used.

Enforcement may include:

* `engines.node`
* package-manager engine strictness
* CI version validation
* startup validation
* development setup script
* preflight command

An unsupported runtime must not silently produce a different lockfile or build.

## 10. Package Manager Decision

Habitacores will use pnpm 11.

pnpm is selected because it provides:

* Native workspace support
* deterministic lockfile behavior
* efficient shared content storage
* explicit dependency declarations
* workspace dependency protocols
* recursive and filtered commands
* lifecycle-script controls
* modern supply-chain protections
* strong monorepo support

## 11. pnpm Version Pinning

The root `package.json` must contain an exact package-manager declaration.

Illustrative configuration:

```json
{
  "packageManager": "pnpm@11.x.y"
}
```

The exact approved pnpm version must be used instead of:

```text
pnpm@latest
pnpm@11
pnpm@^11
```

This prevents developers and CI from silently using materially different package-manager behavior.

## 12. Package-Manager Activation

The selected pnpm version should be activated through an approved runtime-managed mechanism.

Preferred options include:

* Corepack where stable and supported
* pnpm runtime management
* an explicitly installed pinned pnpm binary in CI

The repository must not assume whichever globally installed pnpm version happens to exist on a developer machine.

## 13. Intel macOS Consideration

Some pnpm installation mechanisms may have platform-specific limitations.

Habitacores should use a supported installation method such as:

* Corepack
* npm installation of the pinned pnpm version
* Homebrew for local setup where appropriate
* the official supported pnpm installation mechanism for the developer platform

The standalone installation script must not be treated as the only supported installation path.

## 14. Workspace Definition

The workspace will be defined in:

```text
pnpm-workspace.yaml
```

Illustrative structure:

```yaml
packages:
  - "apps/*"
  - "packages/*"
  - "tooling/*"
```

Additional paths may be added only when they contain package-managed projects.

The following directories do not automatically require workspace package status:

* `docs`
* `adr`
* `scripts`
* `infrastructure`
* `ai`

A directory should become a package only when it has a real independently managed dependency boundary.

## 15. Root Package

The root `package.json` will be private.

Illustrative configuration:

```json
{
  "name": "habitacores",
  "private": true,
  "packageManager": "pnpm@11.x.y",
  "engines": {
    "node": ">=24.0.0 <25"
  }
}
```

The root package must not be accidentally published.

## 16. One Root Lockfile

Habitacores will use one root:

```text
pnpm-lock.yaml
```

The lockfile will be:

* Committed to Git
* reviewed in pull requests
* updated only through approved dependency operations
* enforced in CI
* treated as security-sensitive implementation metadata
* regenerated only deliberately

Applications and internal packages must not maintain independent pnpm lockfiles inside the monorepo.

## 17. Lockfile Integrity

CI will use:

```bash
pnpm install --frozen-lockfile
```

The command must fail when:

* A package manifest changed without the lockfile.
* workspace dependency resolution drifted.
* relevant installation settings changed without lockfile reconciliation.
* the committed lockfile cannot satisfy the workspace.

CI must not silently rewrite the lockfile.

## 18. Lockfile Change Review

A pull request changing `pnpm-lock.yaml` should identify:

* Direct dependencies added
* direct dependencies removed
* major upgrades
* security-related upgrades
* new transitive dependency families
* lifecycle-script implications
* native dependency implications
* licensing implications where material

Large unexplained lockfile changes should not be merged.

## 19. Workspace Protocol

Internal package dependencies will use the workspace protocol.

Example:

```json
{
  "dependencies": {
    "@habitacores/domain": "workspace:*",
    "@habitacores/authorization": "workspace:*"
  }
}
```

This ensures that workspace packages resolve to approved local packages rather than silently resolving to an external registry package with the same name.

## 20. Internal Package Scope

Internal workspace packages should use:

```text
@habitacores/*
```

Examples:

```text
@habitacores/domain
@habitacores/database
@habitacores/authorization
@habitacores/audit
@habitacores/contracts
@habitacores/events
@habitacores/integrations
@habitacores/jobs
@habitacores/observability
@habitacores/security
@habitacores/testing
@habitacores/ui
```

All internal packages will initially remain private.

## 21. Dependency Declaration Rule

Every package must declare the dependencies it imports.

A package must not rely accidentally on a dependency that is available only because another workspace package installed it.

This rule applies to:

* Production dependencies
* development dependencies
* peer dependencies
* optional dependencies

## 22. Dependency Categories

### `dependencies`

Used when the package needs the dependency at runtime.

### `devDependencies`

Used for package-local:

* Tests
* linting
* type checking
* code generation
* local development tooling

### `peerDependencies`

Used only when the consuming application is expected to provide the dependency and shared-instance behavior is required.

### `optionalDependencies`

Used only when failure to install the package is an explicitly supported condition.

## 23. Dependency Pinning Policy

The repository should use controlled version ranges.

Recommended direction:

* Exact versions for critical build and security tooling where reproducibility matters strongly
* controlled compatible ranges for selected libraries where appropriate
* one source of version policy where dependency catalogues are adopted
* lockfile as the exact resolved dependency record

Unbounded dependency specifications are prohibited.

Prohibited examples:

```text
*
latest
>=0
```

## 24. Dependency Catalogues

pnpm catalogues may be considered to align commonly shared dependency versions across packages.

Potential shared dependencies include:

* TypeScript
* React
* Next.js
* validation library
* test framework
* lint tooling
* observability contracts

Catalogue use should remain simple and must not obscure which versions are in use.

The exact catalogue strategy may be finalized during repository initialization.

## 25. Lifecycle Script Security

Third-party dependency lifecycle scripts must not execute automatically without approved trust.

The installation policy should default toward denying dependency build scripts unless explicitly approved.

An approval record should exist for dependencies that require installation-time execution.

Examples may include:

* Native database drivers
* image-processing libraries
* browser binaries
* cryptographic native modules
* code-generation binaries

## 26. Approved Build Dependencies

The repository should maintain an explicit allowlist for dependency build scripts.

Illustrative configuration:

```yaml
allowBuilds:
  approved-package-name: true
```

The exact pnpm 11 syntax must be confirmed during implementation.

Approval should consider:

* Why the script is required
* package ownership
* package reputation
* release provenance
* script behavior
* filesystem access
* network behavior
* alternatives
* version
* review date

## 27. Supply-Chain Protection

Habitacores will preserve pnpm supply-chain safeguards rather than disabling them globally.

Controls should include:

* Minimum package release age
* integrity verification
* lockfile verification
* dependency build-script restrictions
* signature auditing where supported
* dependency scanning
* secret scanning
* provenance review for critical dependencies
* controlled registry configuration

## 28. Minimum Release Age

Newly published package versions should not enter Habitacores immediately by default.

The repository should preserve an approved minimum release age unless an urgent, reviewed security update requires an exception.

Exceptions must be:

* Package specific
* version specific
* documented
* reviewed
* removed after use where applicable

## 29. Package Signature Audit

The dependency-maintenance process should use package-signature verification where supported.

Potential command:

```bash
pnpm audit signatures
```

Signature verification supplements but does not replace:

* vulnerability scanning
* integrity checking
* maintainership review
* lifecycle-script review
* lockfile review

## 30. Vulnerability Audit

The repository should provide a standard command such as:

```bash
pnpm security:audit
```

The command may coordinate:

* pnpm audit
* signature audit
* dependency scanning
* license checks
* repository security tools

A vulnerability result must be triaged according to:

* Exploitability
* runtime exposure
* affected package
* affected environment
* available fix
* workaround
* business impact

## 31. Registry Configuration

The default registry should remain explicitly controlled.

Private package registries may be introduced later where required.

Registry credentials must:

* Never be committed
* use narrowly scoped tokens
* remain environment specific
* be stored in approved secret management
* avoid broad write access
* be rotated

Committed `.npmrc` files must not contain credentials.

## 32. Turborepo Decision

Habitacores will use Turborepo for repository task orchestration.

Turborepo will coordinate:

* Builds
* type checking
* linting
* unit tests
* integration tests
* development tasks
* generated outputs
* dependency-aware task ordering
* affected-package execution
* caching

Turborepo will not define product or domain architecture.

## 33. Why Turborepo

Turborepo is selected because the repository requires:

* Multiple applications
* multiple internal packages
* dependency-aware task execution
* repeatable root commands
* task output caching
* scalable CI
* clear task graphs
* future remote cache compatibility
* integration with the selected TypeScript monorepo approach

## 34. Turborepo Configuration

The primary configuration will be:

```text
turbo.json
```

Illustrative configuration:

```json
{
  "$schema": "https://turbo.build/schema.json",
  "tasks": {
    "build": {
      "dependsOn": ["^build"],
      "outputs": [
        ".next/**",
        "dist/**",
        "build/**"
      ]
    },
    "typecheck": {
      "dependsOn": ["^typecheck"],
      "outputs": []
    },
    "lint": {
      "dependsOn": ["^lint"],
      "outputs": []
    },
    "test": {
      "dependsOn": ["^build"],
      "outputs": ["coverage/**"]
    },
    "dev": {
      "cache": false,
      "persistent": true
    }
  }
}
```

The final task graph must reflect actual package needs.

## 35. Root Scripts

The root `package.json` should expose stable repository commands.

Recommended initial commands:

```json
{
  "scripts": {
    "dev": "turbo run dev",
    "build": "turbo run build",
    "typecheck": "turbo run typecheck",
    "lint": "turbo run lint",
    "test": "turbo run test",
    "test:unit": "turbo run test:unit",
    "test:integration": "turbo run test:integration",
    "test:e2e": "turbo run test:e2e",
    "format": "turbo run format",
    "format:check": "turbo run format:check",
    "security:audit": "pnpm audit && pnpm audit signatures",
    "clean": "turbo run clean"
  }
}
```

Exact scripts may evolve, but command meanings must remain stable.

## 36. Task Naming

Workspace packages should use consistent task names.

Approved common names include:

```text
dev
build
typecheck
lint
test
test:unit
test:integration
format
format:check
clean
generate
```

Packages must not create ambiguous synonyms such as:

```text
check-types
tscheck
types-test
verify-ts
```

without a documented need.

## 37. Task Dependency Direction

Turborepo task dependencies should follow workspace dependencies.

Example:

```text
apps/web build
→ depends on internal package builds
```

A domain package must not depend on the web application's build.

The task graph must preserve the package dependency architecture approved in ADR-001.

## 38. Development Tasks

Long-running development processes must be marked:

```json
{
  "cache": false,
  "persistent": true
}
```

Examples:

* Next.js development server
* deterministic worker watch mode
* AI worker watch mode
* local provider emulator
* local queue worker

Development tasks must not be treated as cacheable build outputs.

## 39. Task Outputs

Cacheable tasks must declare their outputs accurately.

Examples:

* Next.js output
* compiled package output
* generated types
* coverage output where useful
* generated API contracts

The cache must not capture:

* Secrets
* `.env` files
* production credentials
* raw customer data
* payment evidence
* signed agreements
* private AI prompts
* unrestricted AI outputs
* local database contents

## 40. Environment Variables and Task Hashes

Environment variables affecting task output must be included in task hashing or otherwise handled explicitly.

Examples may include:

* Build-time public configuration
* feature compilation flags
* runtime target
* framework mode
* schema generation configuration

Secrets must not be copied into cache artifacts or logs.

A cache hit must never substitute an artifact built under materially different security or tenant assumptions.

## 41. Initial Cache Policy

Habitacores will initially use local Turborepo caching.

Local caching provides:

* Faster repeated local tasks
* simpler setup
* no external cache credentials
* no cross-user artifact sharing
* reduced initial security surface

## 42. Remote Cache Decision

Remote caching is deferred.

It may be introduced only after reviewing:

* Cache provider
* authentication
* access control
* organization ownership
* artifact encryption
* artifact retention
* logs as cached artifacts
* environment-variable hashing
* secret exclusion
* incident response
* cache poisoning risk
* employee offboarding
* CI permissions

## 43. Remote Cache Restrictions

Remote caching must not be enabled casually for tasks that may expose:

* Environment files
* secrets
* customer data
* database snapshots
* restricted documents
* AI request contents
* private provider payloads
* sensitive test fixtures
* production source maps where restricted

A separate implementation review is required before activation.

## 44. Cache Correctness

A task may be cached only when:

* Inputs are complete.
* outputs are declared.
* relevant environment variables are included.
* dependency versions are represented.
* generated output is deterministic.
* secrets are excluded.
* cached results are safe to reuse.

Incorrect caching is worse than no caching.

## 45. CI Installation Sequence

The preferred CI setup is:

```text
Checkout
→ Validate runtime
→ Activate exact pnpm version
→ Restore safe pnpm store cache
→ pnpm install --frozen-lockfile
→ Run required Turborepo tasks
→ Store approved task artifacts
```

The dependency store cache and Turborepo task cache are separate concerns.

## 46. CI Commands

Initial pull-request validation should include:

```bash
pnpm install --frozen-lockfile
pnpm format:check
pnpm lint
pnpm typecheck
pnpm test
pnpm build
```

Security and specialized suites may be separate required checks.

## 47. Continuous Integration Determinism

CI must not:

* Install from an uncommitted lockfile state
* select an unpinned package manager
* use an unsupported Node.js version
* rewrite manifests
* accept lifecycle scripts automatically
* depend on developer-global tools
* use production credentials for ordinary tests

## 48. Filtered Commands

pnpm and Turborepo filtering may be used for local development and targeted CI.

Examples:

```bash
pnpm --filter @habitacores/domain test
pnpm --filter web dev
pnpm turbo run test --filter=@habitacores/finance
```

Filtered execution must not replace full required release validation.

## 49. Affected-Work Execution

Turborepo may execute tasks only for affected packages during suitable pull-request checks.

Full validation remains required for:

* Release candidates
* production promotion
* broad dependency upgrades
* workspace configuration changes
* TypeScript configuration changes
* lockfile tooling changes
* task-graph changes

## 50. Package Build Strategy

Not every internal package requires a separately emitted JavaScript build.

Packages may be:

* Consumed as TypeScript source by approved applications
* compiled independently
* generated
* server only
* browser compatible

The choice must reflect:

* Runtime use
* framework compatibility
* package boundary
* testability
* publishing status
* build performance

This ADR does not require every workspace directory to produce `dist`.

## 51. TypeScript Configuration

The monorepo should use shared TypeScript configuration.

Potential structure:

```text
tooling/typescript/
├── base.json
├── nextjs.json
├── node-library.json
├── react-library.json
└── test.json
```

Package configurations should extend approved shared configurations.

## 52. TypeScript Strictness

The repository should enable strict TypeScript behavior.

The approved direction includes:

* `strict`
* controlled `noUncheckedIndexedAccess`
* controlled exact optional-property behavior
* no uncontrolled `any`
* explicit server and browser library targets
* project-specific module settings

Exact compiler options will be finalized during repository initialization.

## 53. Dependency Boundary Enforcement

Tooling should eventually verify that:

* Domain does not import framework packages.
* domain does not import provider SDKs.
* UI does not import database packages.
* Client Components do not import server-only packages.
* workers use shared application and domain services.
* AI infrastructure does not gain financial or occupancy authority.
* workspace packages declare imported dependencies.

Enforcement may use:

* ESLint
* package export maps
* TypeScript project references
* dependency-cruising tests
* custom architecture tests
* CI rules

## 54. Workspace Export Maps

Internal packages should expose intentional public entry points.

Illustrative:

```json
{
  "name": "@habitacores/domain",
  "private": true,
  "exports": {
    "./finance": "./src/finance/index.ts",
    "./agreements": "./src/agreements/index.ts",
    "./occupancy": "./src/occupancy/index.ts"
  }
}
```

Uncontrolled deep imports should be discouraged.

## 55. Clean Installation Test

CI should periodically validate installation from a clean environment.

A clean-install test should confirm:

* Runtime pin works.
* package-manager pin works.
* lockfile is sufficient.
* dependency scripts follow policy.
* generation steps are declared.
* builds do not depend on developer-local files.

## 56. Reproducibility

A reproducible build requires consistency across:

* Node.js version
* pnpm version
* lockfile
* workspace configuration
* environment variables
* generated files
* task graph
* build command
* operating-system assumptions

Bit-for-bit reproducibility is desirable where practical but is not declared an MVP requirement by this ADR.

## 57. Developer Setup

The root README should provide a minimal setup sequence.

Illustrative:

```bash
git clone <approved-repository>
cd habitacores

# Activate approved Node.js version.
# Activate the exact pnpm version declared in package.json.

pnpm install --frozen-lockfile
pnpm typecheck
pnpm test
pnpm dev
```

Developers should not need to install each workspace package manually.

## 58. Preflight Command

The repository should provide a command such as:

```bash
pnpm doctor
```

or:

```bash
pnpm repo:check
```

It should validate:

* Node.js version
* pnpm version
* environment setup
* required local services
* lockfile status
* generated files
* database accessibility where applicable

The exact name will be decided during implementation.

## 59. Codex Toolchain Boundary

Every Codex work package should state the approved commands.

Example:

```text
Install:
pnpm install --frozen-lockfile

Type check:
pnpm typecheck

Test:
pnpm test

Build:
pnpm build
```

Codex must not:

* Change the package manager
* regenerate the lockfile unnecessarily
* use npm or Yarn for repository dependencies
* upgrade Node.js
* upgrade pnpm
* add a monorepo orchestrator
* enable remote caching
* approve dependency build scripts
* change root workspace patterns

without explicit work-package authorization.

## 60. Dependency Addition by Codex

When adding a dependency, Codex must:

1. Add it to the correct workspace package.
2. explain why it is needed.
3. avoid adding it to the root without justification.
4. use pnpm.
5. update the lockfile.
6. identify lifecycle scripts.
7. run applicable tests.
8. avoid duplicate libraries providing the same capability.
9. avoid provider-specific SDKs outside adapters.
10. avoid abandoned or unmaintained dependencies.

## 61. Dependency Removal

Dependency removal should include:

* Source-reference search
* package-manifest update
* lockfile update
* build validation
* test validation
* generated-file validation
* configuration cleanup
* documentation cleanup

## 62. Dependency Upgrade Automation

Automated dependency update pull requests may be introduced later.

Automation must be configured to:

* Group low-risk related updates
* separate major updates
* identify security updates
* avoid uncontrolled lockfile churn
* run required checks
* respect minimum release age
* avoid automatic merging of high-risk dependencies

Critical dependencies include:

* Next.js
* React
* Node.js
* pnpm
* Turborepo
* authentication
* database libraries
* cryptography
* authorization
* payment SDKs
* signature SDKs
* AI SDKs
* storage SDKs

## 63. Toolchain Upgrade Review

A toolchain upgrade should evaluate:

* Security
* compatibility
* lockfile format
* workspace behavior
* lifecycle-script behavior
* build caching
* environment hashing
* CI behavior
* developer platform support
* deployment support
* rollback or forward recovery

## 64. Toolchain Change Classification

### Routine

Examples:

* pnpm patch update
* Turborepo patch update
* low-risk lint tooling patch

### Material

Examples:

* pnpm minor with installation behavior change
* Turborepo minor affecting cache behavior
* TypeScript minor affecting compilation
* Node.js minor affecting runtime behavior

### Architectural

Examples:

* New Node.js major
* new pnpm major
* replacing pnpm
* replacing Turborepo
* enabling remote caching with external storage
* changing workspace model
* splitting lockfiles

Architectural changes require a superseding ADR or formal amendment.

## 65. Prohibited Toolchain Patterns

The following are not approved:

* Multiple package managers in the repository
* committed `package-lock.json`
* committed `yarn.lock`
* package-local pnpm lockfiles
* unpinned pnpm versions
* production use of Node.js Current rather than approved LTS
* CI using `latest`
* CI modifying the lockfile
* globally trusted dependency scripts
* credentials committed in `.npmrc`
* hidden package installation outside manifests
* arbitrary root dependencies
* remote cache enabled without review
* cache artifacts containing secrets
* Codex changing toolchain versions incidentally
* direct editing of `pnpm-lock.yaml`

## 66. Alternatives Considered

### Option A — npm Workspaces

Advantages:

* Bundled with Node.js
* familiar
* lower initial conceptual overhead

Disadvantages:

* Less aligned with the selected advanced monorepo and dependency-control requirements
* weaker workspace protocol ergonomics
* less efficient shared dependency storage
* less explicit lifecycle-script governance for this architecture

Decision:

Rejected.

### Option B — Yarn Workspaces

Advantages:

* Mature workspace support
* Plug'n'Play option
* broad ecosystem

Disadvantages:

* Additional package-manager model
* Plug'n'Play compatibility considerations
* no material advantage over pnpm for Habitacores
* team would still require separate task orchestration

Decision:

Rejected.

### Option C — pnpm Workspaces Without Turborepo

Advantages:

* Simpler tool count
* pnpm can run recursive commands
* no additional task orchestrator

Disadvantages:

* Less expressive dependency-aware task graph
* weaker cross-package task caching
* less scalable affected-work execution
* more custom CI scripting over time

Decision:

Rejected as the primary model.

### Option D — pnpm Workspaces With Turborepo

Advantages:

* Strong workspace support
* deterministic lockfile
* efficient storage
* dependency-aware tasks
* local caching
* future secure remote caching
* clear root commands
* strong fit for Next.js and TypeScript
* good Codex command boundaries

Disadvantages:

* Additional tooling
* cache correctness requires discipline
* environment-variable configuration requires care
* task graph requires maintenance

Decision:

Approved.

### Option E — Nx

Advantages:

* Extensive monorepo capabilities
* generators
* dependency graph
* affected execution
* plugins

Disadvantages:

* Broader framework and convention footprint
* more platform complexity than currently required
* potential duplication with the chosen engineering approach
* greater governance and upgrade surface

Decision:

Not selected for the MVP.

Nx may be reconsidered if Habitacores later requires capabilities not met by pnpm and Turborepo.

## 67. Positive Consequences

* One consistent JavaScript toolchain
* supported LTS runtime
* deterministic dependency installation
* efficient monorepo dependency storage
* one authoritative lockfile
* clear internal package linking
* strong task orchestration
* faster repeated local tasks
* stable CI commands
* improved Codex boundaries
* stronger supply-chain controls
* future remote caching remains possible

## 68. Negative Consequences

* Developers must learn pnpm behavior.
* Turborepo configuration must remain accurate.
* lifecycle-script approvals require maintenance.
* Node.js and pnpm versions require coordinated updates.
* strict dependency boundaries may expose undeclared imports.
* remote caching cannot be adopted without additional review.
* exact reproducibility requires several aligned configuration files.

## 69. Risks

### 69.1 Lockfile Churn

Control:

* Exact package-manager pin
* frozen CI
* controlled upgrades
* lockfile review

### 69.2 Undeclared Dependencies

Control:

* pnpm isolated linking
* lint rules
* clean-install testing
* package-level dependency declarations

### 69.3 Malicious Dependency Script

Control:

* Default-deny build scripts
* explicit allowlist
* review
* signature and vulnerability audits

### 69.4 Cache Poisoning

Control:

* Local cache initially
* remote cache deferred
* environment hashing
* access control
* artifact review

### 69.5 Secret Leakage Through Cache

Control:

* Output allowlists
* no environment files in outputs
* log review
* remote-cache security gate

### 69.6 Developer Version Drift

Control:

* Runtime files
* package-manager pin
* preflight command
* CI enforcement

### 69.7 Tooling Becomes Architecture

Control:

* Domain boundaries remain tool independent.
* Turborepo coordinates tasks only.
* pnpm manages packages only.
* architectural rules remain documented and tested.

## 70. Acceptance Criterion — Runtime

```text
Given:
A developer or CI runner opens the Habitacores repository.

When:
The runtime is validated.

Then:
Node.js 24 LTS is required.

And:
An unsupported major version fails clearly.

And:
The production build does not rely on Node.js Current.
```

## 71. Acceptance Criterion — Package Manager

```text
Given:
The root package manifest declares the approved pnpm version.

When:
Dependencies are installed.

Then:
The exact approved pnpm 11 release is used.

And:
No npm, Yarn, or unpinned pnpm lockfile is generated.
```

## 72. Acceptance Criterion — Frozen Lockfile

```text
Given:
A package manifest changes without an updated lockfile.

When:
CI executes pnpm install --frozen-lockfile.

Then:
The installation fails.

And:
CI does not rewrite the committed lockfile.
```

## 73. Acceptance Criterion — Workspace Dependency

```text
Given:
apps/web depends on @habitacores/domain.

When:
The dependency is declared.

Then:
It uses the workspace protocol.

And:
It resolves to the local approved workspace package.

And:
It does not resolve silently to an external registry package.
```

## 74. Acceptance Criterion — Dependency Isolation

```text
Given:
Package A imports a library that it does not declare.

When:
A clean installation and package validation run.

Then:
The undeclared dependency is detected or the package fails.

And:
The dependency is not accepted merely because another workspace package installed it.
```

## 75. Acceptance Criterion — Lifecycle Script

```text
Given:
A newly added dependency requests an installation-time build script.

When:
pnpm installs the dependency.

Then:
The script is denied unless explicitly approved.

And:
Approval is version aware and documented.
```

## 76. Acceptance Criterion — Task Graph

```text
Given:
apps/web depends on @habitacores/domain.

When:
The repository build runs.

Then:
Turborepo executes required dependency tasks in the correct order.

And:
The domain package does not depend on the web application.
```

## 77. Acceptance Criterion — Cache Safety

```text
Given:
A cacheable build uses environment-dependent input.

When:
The relevant environment input changes.

Then:
The task does not incorrectly reuse an incompatible cache result.

And:
Secrets do not appear in the cached artifact.
```

## 78. Acceptance Criterion — Codex

```text
Given:
Codex receives a bounded implementation work package.

When:
It installs or changes dependencies.

Then:
It uses pnpm.

And:
It changes only the appropriate workspace package.

And:
It does not replace Node.js, pnpm, Turborepo, workspace patterns, or cache policy without explicit authorization.
```

## 79. Implementation Sequence

Recommended implementation order:

```text
1. Select exact Node.js 24 LTS patch release
2. Select exact pnpm 11 release
3. Create package.json with runtime and package-manager declarations
4. Create .node-version
5. Create .nvmrc if retained
6. Create pnpm-workspace.yaml
7. Create initial applications and essential packages
8. Generate and commit one root pnpm-lock.yaml
9. Add Turborepo as a pinned development dependency
10. Create turbo.json
11. Establish root scripts
12. Establish frozen-lockfile CI
13. Establish dependency build-script policy
14. Establish audit commands
15. Establish clean-install validation
16. Keep remote caching disabled
```

## 80. Deferred Decisions

This ADR does not decide:

* Exact Node.js 24 patch release
* exact pnpm 11 patch release
* exact Turborepo version
* exact TypeScript version
* exact ESLint configuration
* exact formatter
* exact unit-test framework
* exact browser-test framework
* exact CI provider configuration
* exact remote-cache provider
* exact dependency-update bot
* exact SBOM storage process
* exact license policy
* exact package catalogue design
* exact architecture-boundary tool
* exact container base image
* exact application-hosting provider

These will be selected during subsequent ADRs or bounded implementation decisions.

## 81. Supersession Conditions

This ADR should be reviewed when:

* Node.js 24 approaches end of support.
* Next.js no longer supports the approved runtime.
* pnpm 11 approaches end of support.
* a new pnpm major offers material benefits or required security changes.
* Turborepo cannot satisfy repository task requirements.
* remote caching becomes operationally necessary.
* repository scale causes unacceptable task performance.
* multiple independently governed repositories are introduced.
* another runtime becomes a required production platform.
* package-management security requirements materially change.

## 82. Final Decision Statement

Habitacores will use Node.js 24 LTS, pnpm 11, pnpm workspaces, and Turborepo as its foundational JavaScript and TypeScript monorepo toolchain.

The repository will use one committed root `pnpm-lock.yaml`, exact package-manager pinning, an approved Node.js 24 runtime pin, workspace-protocol dependencies, deterministic frozen-lockfile CI installation, explicit dependency declarations, and controlled dependency lifecycle scripts.

Turborepo will coordinate dependency-aware builds, tests, type checking, linting, development processes, and approved cacheable tasks.

Local task caching is approved initially. Remote caching is deferred until authentication, access control, environment hashing, artifact sensitivity, retention, cache poisoning, secret leakage, and incident-response requirements have been reviewed.

Supply-chain safeguards must remain enabled unless a narrow, documented exception is approved.

No developer, CI pipeline, coding agent, package script, dependency installer, workspace package, cache provider, or deployment platform may silently replace the approved runtime, package manager, lockfile, workspace graph, lifecycle-script policy, or build-task security controls.