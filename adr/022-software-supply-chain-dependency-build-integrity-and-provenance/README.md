# ADR-022 — Software Supply Chain, Dependency, Build Integrity, Artifact Provenance, and Vulnerability Management Strategy

## Decision Record

| Field | Value |
|---|---|
| Product | Habitacores |
| ADR | ADR-022 |
| Version | 0.1 |
| Status | Proposed |
| Decision Type | Foundational Application Security, CI/CD, Build, and Supply-Chain Architecture |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Source Authority | Protected Habitacores Git repositories |
| Package Manager | pnpm 11 under ADR-003 |
| Dependency Locking | One authoritative root `pnpm-lock.yaml` |
| CI Installation | Frozen lockfile |
| Dependency Review | Required for material dependency changes |
| Automated Updates | Controlled pull requests only |
| GitHub Actions | Least privilege and immutable pinning for sensitive workflows |
| Deployment Authentication | OIDC where supported |
| Release Identity | Immutable Git commit plus release metadata |
| Artifact Integrity | Checksums, provenance, and attestations where supported |
| SBOM | Required for production release artifacts |
| Initial SLSA Direction | Build Level 1 capability target |
| Production Promotion | Promote the reviewed artifact; do not rebuild uncontrolled variants |
| Vulnerability Handling | Risk-based triage, remediation, exception, and verification |
| Secret Scanning | Required |
| Code Scanning | Required |
| IaC Scanning | Required |
| License Review | Required for new or materially changed dependencies |
| Coding-Agent Authority | Pull-request contributor only |
| Direct Agent Production Access | Prohibited |
| Last Updated | 2 August 2026 |

## 1. Context

Habitacores will be built with:

- TypeScript
- Next.js
- React Native and Expo
- Node.js
- pnpm
- Turborepo
- Drizzle ORM
- PostgreSQL
- GitHub Actions
- OpenTofu
- provider SDKs
- open-source libraries
- container or platform build systems
- mobile native dependencies
- generated code
- AI-assisted coding tools

The product will also integrate with:

- Supabase
- Vercel
- payment providers
- signature providers
- communication providers
- observability providers
- feature-management providers
- AI providers
- mobile application stores

Each dependency, workflow action, build tool, provider SDK, generated artifact, and publishing mechanism expands the software supply chain.

Threats include:

- Malicious packages
- dependency confusion
- typosquatting
- compromised maintainers
- vulnerable transitive dependencies
- compromised GitHub Actions
- workflow-token abuse
- untrusted pull-request code
- leaked package or deployment credentials
- altered build artifacts
- unsigned releases
- irreproducible builds
- compromised build runners
- unreviewed generated code
- licensing incompatibility
- stale dependencies
- hidden post-install scripts
- mobile native-package compromise
- untraceable emergency releases

## 2. Decision Drivers

The strategy must:

1. Preserve dependency integrity.
2. make dependency changes reviewable.
3. prevent untrusted code from gaining privileged workflow authority.
4. ensure production artifacts are traceable to reviewed source.
5. reduce long-lived CI credentials.
6. protect deployment and publishing workflows.
7. detect vulnerable dependencies.
8. detect secrets and insecure code.
9. control licensing risk.
10. generate production SBOMs.
11. preserve build provenance.
12. support web, worker, infrastructure, and mobile artifacts.
13. constrain coding agents.
14. support vulnerability response.
15. protect emergency-release procedures.
16. separate build, approval, and deployment authority.
17. preserve evidence for investigation and recovery.
18. support incremental supply-chain maturity.
19. avoid unsupported compliance claims.
20. provide executable CI and repository controls.

## 3. Decision

Habitacores will implement a defense-in-depth software-supply-chain architecture.

The approved flow is:

```text
Approved source change
→ Protected pull request
→ Static and dependency checks
→ Human review
→ Merge to protected branch
→ Trusted CI build
→ Tests and security validation
→ SBOM generation
→ Artifact checksum
→ Build provenance and attestation
→ Release approval
→ Promotion of the reviewed artifact
→ Deployment
→ Post-deployment verification
→ Vulnerability and dependency monitoring
````

Production artifacts must remain attributable to approved source, build instructions, dependency resolution, and workflow identity.

## 4. Core Concepts

Habitacores will distinguish:

```text
SOURCE_COMMIT
DEPENDENCY_MANIFEST
DEPENDENCY_LOCKFILE
BUILD_WORKFLOW
BUILD_ARTIFACT
SBOM
PROVENANCE
ATTESTATION
RELEASE
DEPLOYMENT
VULNERABILITY
SECURITY_EXCEPTION
DEPENDENCY_EXCEPTION
LICENSE_EXCEPTION
```

A deployment record is not equivalent to build provenance, and an SBOM is not equivalent to a vulnerability scan.

## 5. Source Repositories

Habitacores source repositories must use:

* Private-by-default access where appropriate
* organization-managed ownership
* MFA for privileged maintainers
* protected default branches
* pull-request review
* required status checks
* restricted force pushes
* restricted branch deletion
* attributable commits
* least-privilege teams
* recovery controls under ADR-021

Personal forks or local copies must not become production source authority.

## 6. Protected Branches

The authoritative production branch must require:

* Pull request
* required checks
* review
* conversation resolution
* current branch state
* restricted bypass
* no ordinary force push
* no direct unreviewed production commit

Emergency bypasses require an incident or emergency-change record and retrospective review.

## 7. Commit Integrity

Production releases must reference an immutable commit SHA.

Release records should preserve:

* Repository
* commit SHA
* branch
* pull request
* approving reviewers
* workflow run
* artifact identifier
* attestation
* deployment
* environment
* release time

A mutable branch name alone is insufficient release identity.

## 8. Dependency Manifests

Every dependency must be declared through approved package manifests.

For the TypeScript workspace, these include:

* root `package.json`;
* workspace package manifests;
* one root `pnpm-lock.yaml`;
* approved pnpm workspace configuration.

Undeclared runtime downloads are prohibited unless separately approved.

## 9. Lockfile Authority

The root `pnpm-lock.yaml` is authoritative for Node.js dependency resolution.

CI must install with frozen-lockfile enforcement.

CI must fail when:

* A manifest changed without the lockfile.
* the lockfile is invalid.
* dependency resolution would modify the lockfile.
* an unsupported lockfile format is introduced.
* the configured pnpm version does not match project policy.

## 10. Lockfile Changes

Lockfile changes require review.

Review should identify:

* New direct dependencies
* removed dependencies
* major upgrades
* source or registry changes
* unexpected transitive expansion
* lifecycle scripts
* native binaries
* checksum or integrity changes
* license changes
* maintainer or project-risk signals where relevant

A large generated lockfile diff must not be approved solely because it is machine generated.

## 11. Dependency Sources

Production dependencies must come from approved registries or approved internal sources.

Dependencies from:

* arbitrary Git branches;
* personal archives;
* unverified URLs;
* mutable external locations;
* local filesystem paths;

are prohibited for production unless explicitly reviewed and pinned to immutable content.

## 12. Package Namespaces

Habitacores should reserve relevant package namespaces and internal package names where practical.

Internal packages must use approved organization scope or workspace-only resolution.

The build must not silently replace an internal package with a public registry package of the same name.

## 13. New Dependency Approval

A new material dependency requires evaluation of:

* Necessity
* functionality overlap
* maintenance status
* release activity
* security history
* ownership and maintainers
* transitive dependency count
* install scripts
* binary downloads
* license
* TypeScript support
* Node.js compatibility
* React Native or Expo compatibility where applicable
* data-handling behavior
* network behavior
* replacement difficulty
* bundle and performance impact

A dependency should not be added when a small, well-tested internal implementation is safer and simpler.

## 14. Dependency Minimization

Habitacores will prefer:

* Platform capabilities
* standard-library capabilities
* existing approved packages
* small bounded internal modules

over unnecessary new dependencies.

Dependency minimization must not justify writing custom cryptography, authentication, signature verification, or other security-critical primitives without qualified review.

## 15. Production and Development Dependencies

Dependencies must be classified accurately.

Runtime production packages must not be hidden as development dependencies.

Build-only or test-only tooling must not enter runtime bundles unnecessarily.

The release process should verify the effective production dependency set.

## 16. Lifecycle Scripts

Package lifecycle scripts are high-risk execution points.

The dependency policy must identify packages that execute:

* `preinstall`
* `install`
* `postinstall`
* native compilation
* binary download
* code generation
* external network access

Unexpected lifecycle-script changes require elevated review.

## 17. Native Dependencies

React Native, Expo, Node native modules, and binary packages require additional scrutiny.

Review must consider:

* Supported platforms
* binary provenance
* code-signing implications
* native permissions
* build-service behavior
* architecture support
* update cadence
* vulnerability response
* store-policy impact

## 18. Automated Dependency Updates

Automated dependency tooling may open pull requests.

Automated update pull requests must still pass:

* lockfile validation
* unit and integration tests
* type checking
* linting
* dependency review
* vulnerability scanning
* build checks
* mobile compatibility checks where applicable
* human review according to risk

Automated tooling may not merge high-risk major upgrades directly into production.

## 19. Update Grouping

Dependency updates should be grouped only where grouping preserves reviewability.

Appropriate groupings may include:

* patch-level development tooling;
* related framework packages;
* tested provider SDK families.

Unrelated major upgrades must not be bundled merely to reduce pull-request count.

## 20. Vulnerability Intelligence

Habitacores will monitor known vulnerabilities through approved sources and repository tooling.

Vulnerability records should preserve:

* Advisory identifier
* affected package
* affected versions
* installed path
* direct or transitive relationship
* reachable or potentially reachable use
* severity
* exploitability
* affected environments
* remediation
* exception where applicable
* verification
* closure time

## 21. Severity Is Not the Only Factor

Remediation priority should consider:

* Exploit availability
* reachability
* internet exposure
* data sensitivity
* privilege
* tenant-isolation impact
* financial or legal impact
* compensating controls
* affected version
* ease of upgrade
* provider deployment status

A high nominal severity may be unreachable, while a lower-severity issue may be urgent in the Habitacores context.

## 22. Directional Remediation Targets

Initial directional targets:

| Risk                                       |                                          Target |
| ------------------------------------------ | ----------------------------------------------: |
| Critical, actively exploitable, applicable | Immediate containment and emergency remediation |
| Critical, applicable                       |                                        72 hours |
| High, applicable                           |                                         14 days |
| Medium, applicable                         |                                         45 days |
| Low                                        |                       Planned maintenance cycle |

These are targets, not claims of current operational capability.

Exceptions require explicit risk acceptance and expiry.

## 23. Vulnerability Exceptions

A vulnerability exception must include:

* Advisory
* affected component
* business justification
* applicability analysis
* compensating controls
* risk owner
* approval
* expiry
* monitoring
* planned resolution

A scanner-suppression comment without a risk record is insufficient.

## 24. License Governance

New dependencies must undergo license identification.

The policy must detect:

* Unknown licenses
* missing licenses
* restrictive copyleft implications
* incompatible commercial terms
* source-disclosure obligations
* attribution requirements
* non-commercial restrictions
* custom licenses

License acceptance is a legal and product decision, not merely a scanner result.

## 25. Secret Scanning

Repositories and pull requests must be scanned for secrets.

Potential findings include:

* API keys
* database credentials
* signing keys
* webhook secrets
* private keys
* cloud credentials
* provider tokens
* mobile signing materials
* private configuration values

A detected committed secret must be treated as compromised and rotated where applicable. Removing it from the latest commit alone is insufficient.

## 26. Code Scanning

Habitacores will run static analysis for:

* injection risks
* unsafe deserialization
* authentication mistakes
* authorization omissions
* insecure cryptography
* path traversal
* secret exposure
* unsafe file handling
* SSRF
* command execution
* web and API security defects

Findings must be triaged rather than ignored wholesale.

## 27. Infrastructure Scanning

OpenTofu and workflow definitions must be scanned for:

* Public exposure
* excessive IAM permissions
* plaintext secrets
* missing encryption
* unrestricted ingress
* destructive configuration
* unsafe GitHub Actions
* environment crossover
* unpinned providers or actions
* missing ownership metadata

ADR-019 remains authoritative for infrastructure application.

## 28. GitHub Actions Trust Boundary

Workflow files are production-relevant code.

Changes to sensitive workflows require elevated review.

Sensitive workflows include:

* Production deployment
* infrastructure apply
* package publishing
* mobile signing and submission
* secret rotation
* backup and recovery
* artifact attestation
* release promotion

## 29. Workflow Permissions

Every GitHub Actions workflow must declare explicit permissions.

The default should be read-only unless write authority is required.

Permissions such as:

* `contents: write`
* `packages: write`
* `id-token: write`
* `deployments: write`
* `attestations: write`
* `security-events: write`

must be granted only to jobs that require them.

## 30. Action Pinning

Third-party GitHub Actions used in sensitive workflows must be pinned to immutable commit SHAs.

Version tags may be recorded in comments for maintainability, but tags alone are mutable references.

Pinned actions require update monitoring and review.

## 31. First-Party Actions

GitHub-owned actions must also be version controlled and reviewed.

Habitacores should use supported major versions and monitor security advisories and deprecations.

Sensitive workflows may pin them by commit SHA according to the same immutable-reference policy.

## 32. Pull-Request Workflow Safety

Untrusted pull-request code must not gain access to production secrets or privileged tokens.

Workflows must distinguish between:

* Trusted merged source
* same-repository pull requests
* external or forked pull requests
* automated dependency pull requests
* privileged deployment workflows

A workflow must not check out and execute untrusted pull-request code under a privileged context.

## 33. `pull_request_target`

Use of `pull_request_target` with untrusted code is prohibited unless a documented threat model and safe pattern are approved.

It must never be used casually to expose secrets to forked pull-request code.

## 34. OIDC Authentication

Deployment and infrastructure workflows should use GitHub Actions OIDC federation where supported.

OIDC trust must be constrained by:

* Repository
* branch or tag
* workflow
* environment
* audience
* protected deployment context

`id-token: write` allows requesting an identity token; it does not itself grant provider authorization without the provider trust relationship.

## 35. Long-Lived Tokens

Where OIDC is unavailable, tokens must be:

* Environment specific
* least privilege
* short-lived where possible
* stored in protected secrets
* rotated
* monitored
* revocable
* unavailable to pull-request workflows

Personal administrator tokens are prohibited as permanent release authority.

## 36. Build Environment

Production builds must execute in an approved environment.

The build environment must define:

* Operating system
* architecture
* Node.js version
* pnpm version
* environment variables
* package registry
* network requirements
* build command
* source commit
* lockfile
* cache behavior
* artifact outputs

## 37. Build Isolation

The build process should minimize:

* Unrestricted credentials
* broad production network access
* persistence between unrelated builds
* writable shared caches
* mutable external inputs
* access to unrelated repositories

Build jobs should receive only the credentials and data needed for the build.

## 38. Build Caches

Caches are performance aids, not trusted release authority.

Caches must be:

* Appropriately scoped
* keyed to relevant inputs
* protected against cross-environment contamination
* safely invalidated
* excluded from containing secrets
* replaceable without affecting correctness

A compromised or stale cache must not silently change dependency resolution.

## 39. Build Reproducibility

Habitacores should reduce uncontrolled build variability through:

* Pinned runtime and package-manager versions
* frozen lockfiles
* immutable source commits
* controlled environment variables
* pinned actions
* constrained provider tooling
* deterministic generation where practical
* recorded build inputs

Perfect byte-for-byte reproducibility is not initially required, but unexplained material artifact differences are unacceptable.

## 40. Build Artifacts

A release artifact should have:

* Artifact ID
* artifact type
* source commit
* build workflow
* workflow run ID
* build time
* environment class
* checksum
* SBOM
* provenance
* attestation where supported
* retention
* deployment relationships

## 41. Artifact Checksums

Production artifacts must receive cryptographic checksums.

Checksums support integrity verification during:

* Storage
* promotion
* download
* deployment
* investigation
* recovery

A checksum does not establish who built the artifact; provenance and attestation address that additional question.

## 42. Software Bill of Materials

Habitacores will generate an SBOM for production release artifacts.

The SBOM should identify:

* Direct and transitive components
* versions
* package identifiers
* dependency relationships
* artifact relationship
* generation tool
* generation time
* source commit

The exact format may use an approved standard such as SPDX or CycloneDX.

## 43. SBOM Handling

SBOMs should be:

* Associated with the release artifact
* retained
* integrity protected
* available for vulnerability investigation
* generated automatically
* validated for basic completeness

Public release of internal SBOMs is not required by this ADR.

## 44. Provenance

Build provenance records how an artifact was produced.

It should identify:

* Build platform
* source repository
* source commit
* build workflow
* build instructions
* declared inputs
* resolved dependencies or relevant references
* output artifact
* invocation identity

The initial architectural target is generation and retention of provenance for production artifacts.

## 45. SLSA Direction

Habitacores will use SLSA as a maturity framework, not as an unsupported certification label.

Initial direction:

```text
Phase 1:
Generate provenance and associate it with production artifacts.

Phase 2:
Use hosted and isolated build workflows with stronger provenance guarantees.

Phase 3:
Verify provenance before production promotion or deployment.
```

Habitacores must not claim a SLSA level until its applicable requirements have been assessed and demonstrated.

## 46. Artifact Attestations

Where supported, GitHub artifact attestations should bind:

* Artifact digest
* repository
* workflow
* source commit
* build identity
* provenance statement

Verification should occur before sensitive artifact promotion where technically practical.

## 47. Build Once, Promote

The preferred release flow is:

```text
Build reviewed artifact
→ Validate
→ Attest
→ Approve
→ Promote the same artifact
→ Deploy
```

Production promotion must not rebuild an uncontrolled artifact from the same source and assume equivalence.

Where a platform necessarily rebuilds during deployment, that build must remain attributable, controlled, and included in the provenance model.

## 48. Web Deployment

For Vercel-hosted web applications, Habitacores must preserve:

* Source commit
* Vercel deployment ID
* build logs
* framework and runtime configuration
* environment
* deployment protection
* release approval
* production alias assignment
* post-deployment health verification

A preview deployment must not become production solely through possession of a URL.

## 49. Worker Deployment

Worker artifacts must preserve:

* Source commit
* build artifact
* runtime image or package digest
* SBOM
* provenance
* environment
* deployment version
* rollback or forward-repair relationship

Floating container tags such as `latest` are prohibited as production identity.

## 50. Container Images

Where containers are introduced, production references must use immutable image digests.

Images should undergo:

* Base-image review
* vulnerability scanning
* SBOM generation
* provenance generation
* signature or attestation
* non-root execution where practical
* minimal runtime contents
* regular base-image maintenance

## 51. Mobile Builds

Android and iOS builds require supply-chain controls covering:

* Source commit
* Expo and React Native versions
* native dependencies
* build-service configuration
* app version
* build number
* signing identity
* build artifact
* checksums
* SBOM where supported
* store-submission identity
* release track

Mobile signing keys and store credentials must remain outside source control.

## 52. Package Publishing

Habitacores should avoid publishing internal packages publicly unless there is a defined product need.

Where npm publishing is approved:

* Organization-scoped package names must be used.
* OIDC trusted publishing should be preferred.
* provenance should be generated.
* 2FA and maintainer controls must be enabled.
* package contents must be reviewed.
* publishing must occur from an approved workflow.
* long-lived publish tokens should be avoided.

## 53. Release Approval

Release approval is distinct from successful build completion.

Production release requires:

* Approved source
* required tests
* security checks
* reviewed artifact
* known vulnerability status
* required migrations
* release notes
* environment approval
* artifact identity
* post-deployment validation plan

## 54. Security Gates

Not every scanner finding must automatically block every build.

Blocking policy should consider:

* Severity
* applicability
* exploitability
* new versus existing finding
* affected artifact
* approved exception
* environment
* release risk

New critical applicable findings must block production unless an authorized emergency-risk decision exists.

## 55. Baseline Findings

Existing findings must not be hidden indefinitely by treating them as baseline.

Each accepted baseline finding requires:

* Ownership
* risk classification
* remediation plan
* expiry or review
* evidence of applicability
* tracking

The pipeline should prevent uncontrolled regression beyond the approved baseline.

## 56. Generated Code

Generated code includes output from:

* Drizzle tooling
* OpenAPI tools
* GraphQL tools
* Expo tooling
* AI coding agents
* schema generators
* client generators

Generated code must be:

* Reproducible where practical
* attributable to a tool and version
* reviewed according to risk
* tested
* scanned
* prevented from containing secrets
* regenerated through documented commands

## 57. Coding Agents

Codex and other coding agents may:

* Propose code
* modify files in an isolated workspace
* create tests
* draft migrations
* draft workflows
* open pull requests where approved
* explain dependency changes

They remain contributors, not production authorities.

## 58. Coding-Agent Prohibitions

Coding agents must not:

* Merge their own production changes
* bypass required checks
* approve their own pull requests
* obtain production deployment credentials
* apply production infrastructure
* publish packages
* access production databases
* access production backups
* rotate production credentials
* suppress security findings without review
* introduce a dependency without declared rationale
* modify lockfiles without corresponding manifest review
* execute untrusted package scripts outside controlled environments

## 59. Agent-Generated Dependency Changes

Every agent-generated dependency change must clearly identify:

* Package
* requested version
* purpose
* alternatives considered
* direct or transitive impact
* license
* relevant security status
* lockfile change
* testing performed
* rollback implications

“Required by generated code” is not sufficient justification.

## 60. Security Scanning Schedule

Habitacores should scan:

* On pull requests
* on merges to the protected branch
* during production builds
* on a recurring schedule
* after major advisories
* after dependency or runtime changes
* before material releases

Recurring scans are necessary because a dependency may become known as vulnerable after it was merged.

## 61. Release Monitoring

After release, Habitacores must be able to determine:

* Which deployments contain an affected package
* which mobile versions contain it
* which worker artifacts contain it
* which environments are affected
* whether the vulnerable path is reachable
* which replacement artifact remediates it

SBOM and release metadata support this investigation.

## 62. Emergency Security Release

An emergency security release may accelerate normal review but must preserve:

* Attributable source
* minimum required review
* tests appropriate to the incident
* artifact identity
* security rationale
* deployment authorization
* post-deployment verification
* retrospective review
* follow-up cleanup

Emergency status does not authorize unverifiable binaries or direct untracked production edits.

## 63. Dependency Removal

Removing a dependency requires verification that:

* Imports are removed
* runtime use is removed
* transitive presence is understood
* lockfile is updated
* generated artifacts are updated
* SBOM no longer includes it where expected
* tests pass
* replacement behavior is secure

## 64. Deprecated Dependencies

A deprecated or abandoned dependency requires review.

Potential responses include:

* Upgrade
* replace
* isolate
* fork temporarily under approved governance
* remove
* accept temporarily with expiry

Habitacores must not silently depend indefinitely on abandoned security-critical packages.

## 65. Repository Security Events

Material events may include:

```text
DEPENDENCY_ADDED
DEPENDENCY_REMOVED
DEPENDENCY_EXCEPTION_APPROVED
VULNERABILITY_DETECTED
VULNERABILITY_EXCEPTION_APPROVED
SECRET_DETECTED
SECRET_ROTATED
BUILD_ARTIFACT_CREATED
SBOM_CREATED
PROVENANCE_CREATED
ARTIFACT_ATTESTED
RELEASE_APPROVED
RELEASE_DEPLOYED
EMERGENCY_SECURITY_RELEASE_COMPLETED
SUPPLY_CHAIN_INCIDENT_OPENED
```

These may be represented through repository, release, security, or Habitacores audit evidence as appropriate.

## 66. Observability

Metrics may include:

```text
dependency_changes
dependency_review_failures
known_vulnerabilities
vulnerability_age
secret_scan_findings
code_scan_findings
unlicensed_dependencies
build_failures
build_duration
artifact_attestation_failures
sbom_generation_failures
unverified_artifact_deployments
unpinned_action_findings
security_exception_age
```

Package names may appear in security systems but should not create unbounded general metrics.

## 67. Alerts

Priority alerts include:

* Critical applicable vulnerability
* secret committed
* privileged workflow changed unexpectedly
* unpinned action in a production workflow
* production deployment from an unattested artifact
* dependency lockfile bypass
* production package published outside the approved workflow
* OIDC trust used by an unexpected repository or branch
* build artifact checksum mismatch
* missing SBOM
* unsupported Node.js or pnpm version
* deployment artifact not traceable to an approved commit

## 68. Incident Response

A supply-chain incident may require:

* Dependency or provider suspension
* build shutdown
* credential rotation
* artifact revocation
* deployment rollback or forward repair
* release replacement
* package removal
* forensic preservation
* tenant-impact analysis
* provider notification
* legal or privacy escalation
* recovery under ADR-021

## 69. Testing

Required tests include:

* Frozen-lockfile enforcement
* unauthorized registry rejection
* dependency-review blocking
* secret-scanning tests
* code-scanning tests
* workflow-permission tests
* untrusted-pull-request isolation
* action-pinning checks
* artifact-checksum verification
* SBOM generation
* provenance generation
* attestation verification
* deployment-to-source traceability
* coding-agent boundary tests
* emergency-release procedure tests

## 70. Lockfile Test

```text
Given:
A package manifest changes without an updated lockfile.

When:
CI installs dependencies.

Then:
The frozen-lockfile installation fails.

And:
No build artifact is approved.
```

## 71. Dependency Review Test

```text
Given:
A pull request introduces a dependency with an applicable critical vulnerability.

When:
Dependency review executes.

Then:
The security gate fails.

And:
The dependency cannot enter the protected branch without an authorized exception.
```

## 72. Untrusted Workflow Test

```text
Given:
A forked pull request modifies source code.

When:
Pull-request checks execute.

Then:
The code receives no production secrets or deployment credentials.

And:
It cannot apply infrastructure, publish packages, or deploy production.
```

## 73. Artifact Traceability Test

```text
Given:
A production web deployment is running.

When:
Its release record is inspected.

Then:
Habitacores can identify the source commit, workflow run, build artifact,
checksum, SBOM, approval, and deployment identifier.
```

## 74. Artifact Integrity Test

```text
Given:
A built artifact is modified after attestation.

When:
Verification executes.

Then:
The artifact digest no longer matches.

And:
Production promotion is denied.
```

## 75. Coding-Agent Boundary Test

```text
Given:
A coding agent creates a dependency update and workflow modification.

When:
The change is proposed.

Then:
It remains an unmerged pull-request contribution.

And:
The agent cannot approve, merge, publish, deploy, or access production credentials.
```

## 76. Production Promotion Test

```text
Given:
A tested and attested release artifact was approved.

When:
Production deployment begins.

Then:
The approved artifact or its verified platform-equivalent build is deployed.

And:
An uncontrolled replacement artifact is not silently rebuilt and substituted.
```

## 77. Codex Boundaries

Every Codex software-supply-chain work package must specify:

* Repository
* affected package or workflow
* dependency rationale
* package source
* exact version policy
* lockfile effect
* lifecycle scripts
* license
* vulnerability status
* build effect
* SBOM effect
* provenance effect
* workflow permissions
* credentials
* tests
* rollback or replacement plan

Codex must not:

* Add undeclared dependencies
* remove frozen-lockfile enforcement
* use floating production versions
* add arbitrary Git dependencies
* expose workflow secrets
* add privileged `pull_request_target` execution
* grant broad default workflow permissions
* use unpinned third-party actions in sensitive workflows
* suppress vulnerability findings without an exception
* generate or commit credentials
* publish packages
* deploy production
* approve or merge its own changes
* claim SLSA compliance without verification

## 78. Prohibited Patterns

The following are not approved:

* `pnpm install` in CI without frozen-lockfile enforcement
* deleting the lockfile to resolve dependency conflicts
* floating GitHub Action references in sensitive workflows
* untrusted pull-request code with production secrets
* broad repository write permissions for every workflow
* personal deployment tokens as permanent CI authority
* public package publication from a developer laptop
* production release without source traceability
* mutable `latest` artifact identity
* production promotion through uncontrolled rebuild
* missing SBOM for production artifacts
* unsigned or unverifiable emergency binary
* automatic major dependency upgrades directly into production
* ignored license obligations
* AI-generated code bypassing review and scanning
* unsupported SLSA-level claims

## 79. Options Considered

### Option A — Depend on Package-Manager Defaults

Advantages:

* Minimal setup
* fast initial development

Disadvantages:

* Weak governance
* poor vulnerability response
* no artifact provenance
* limited workflow protection
* insufficient release traceability

Decision:

Rejected.

### Option B — Scanner-Only Supply-Chain Security

Advantages:

* Easy automation
* identifies known findings

Disadvantages:

* Does not secure workflows
* does not establish provenance
* does not prevent token abuse
* does not protect artifact promotion
* does not govern dependencies or licensing

Decision:

Rejected as the complete architecture.

### Option C — Fully Reproducible Hermetic Builds Immediately

Advantages:

* Strong integrity
* reduced environmental variation
* advanced provenance capability

Disadvantages:

* High initial complexity
* may conflict with managed Vercel and Expo build models
* substantial tooling and operational cost
* premature for the founding phase

Decision:

Deferred as an immediate requirement.

### Option D — Incremental Defense-in-Depth Supply-Chain Architecture

Advantages:

* Practical initial implementation
* frozen dependency resolution
* dependency and vulnerability governance
* protected CI
* OIDC
* SBOM
* provenance and attestations
* artifact traceability
* clear maturity path

Disadvantages:

* Adds CI complexity
* requires continuing dependency review
* scanners produce false positives
* managed platforms may limit full reproducibility
* provenance verification requires operational discipline

Decision:

Approved.

## 80. Positive Consequences

* Dependencies become reviewable and traceable.
* CI credentials receive tighter controls.
* production artifacts can be mapped to source.
* vulnerability response becomes measurable.
* SBOMs support impact analysis.
* provenance reduces uncertainty about artifact origin.
* coding agents remain subordinate to repository governance.
* web, worker, infrastructure, and mobile builds receive consistent controls.
* emergency releases retain evidence.

## 81. Negative Consequences

* Builds and pull requests gain additional checks.
* security findings require triage.
* dependency upgrades may take longer.
* action pinning requires maintenance.
* SBOM and provenance tooling adds complexity.
* managed build platforms may prevent perfect reproducibility.
* license review requires occasional legal judgment.

## 82. Risks

### 82.1 Malicious Dependency

Controls:

* Dependency review
* lockfile review
* approved registries
* lifecycle-script scrutiny
* vulnerability monitoring
* dependency minimization

### 82.2 Compromised Workflow Action

Controls:

* Immutable action pinning
* least privilege
* workflow review
* restricted secrets
* trusted build contexts

### 82.3 Credential Theft

Controls:

* OIDC
* scoped secrets
* environment protection
* no secrets in pull-request workflows
* rotation
* audit

### 82.4 Artifact Substitution

Controls:

* Checksums
* provenance
* attestations
* build-once promotion
* immutable deployment identity

### 82.5 Scanner Fatigue

Controls:

* Risk-based triage
* ownership
* applicability review
* bounded exceptions
* regression prevention

### 82.6 Coding-Agent Supply-Chain Error

Controls:

* Pull-request-only authority
* dependency rationale
* human review
* tests
* scans
* no production credentials

## 83. Implementation Sequence

```text
1. Protect authoritative repositories and branches
2. enforce Node.js, pnpm, and frozen-lockfile versions
3. add dependency review to pull requests
4. configure automated dependency update pull requests
5. add secret scanning and push protection where supported
6. add code scanning
7. add dependency vulnerability monitoring
8. add license identification
9. define security finding and exception workflows
10. pin sensitive GitHub Actions to immutable commits
11. define explicit workflow permissions
12. separate untrusted pull-request and privileged workflows
13. replace long-lived deployment credentials with OIDC where supported
14. define release artifact metadata
15. generate checksums for production artifacts
16. generate SBOMs
17. generate build provenance and artifact attestations
18. verify artifact identity during production promotion
19. extend controls to workers and containers
20. extend controls to Expo, Android, and iOS builds
21. establish emergency vulnerability response
22. test artifact-to-source traceability
23. assess achieved SLSA requirements without overclaiming
24. remediate gaps before production scale
```

## 84. Deferred Decisions

This ADR does not decide:

* Exact SBOM format
* exact SBOM generator
* exact vulnerability scanner
* exact license scanner
* exact static-analysis suite
* exact artifact-signing mechanism
* exact provenance-storage location
* exact attestation-verification tool
* exact container registry
* exact private-package registry
* exact dependency-update bot
* exact approval threshold for major upgrades
* exact SLSA level ultimately targeted
* exact mobile SBOM mechanism
* exact artifact-retention period
* exact security-exception approval count
* exact bug-bounty or vulnerability-disclosure program

These require bounded implementation decisions.

## 85. Supersession Conditions

This ADR should be reviewed when:

* Habitacores publishes public packages.
* containerized workers become production critical.
* enterprise customers require formal SBOM delivery.
* contractual SLSA requirements apply.
* mobile-build infrastructure changes.
* self-hosted GitHub runners are introduced.
* multiple registries are used.
* signed release verification becomes mandatory.
* a major software-supply-chain incident occurs.
* new legislation imposes software-component disclosure obligations.

## 86. Final Decision Statement

Habitacores will implement a defense-in-depth software-supply-chain architecture covering source repositories, dependencies, lockfiles, GitHub Actions, build environments, generated code, artifacts, SBOMs, provenance, attestations, deployments, vulnerabilities, licenses, and coding agents.

The protected Habitacores repositories and immutable source commits will be the authority for production software. Production releases will remain attributable to an approved commit, reviewed pull request, trusted workflow, identified build environment, release artifact, checksum, SBOM, provenance record, approval, and deployment.

The TypeScript workspace will use one authoritative root `pnpm-lock.yaml`. CI will use frozen-lockfile installation and will fail when manifests and dependency resolution are inconsistent.

New and materially changed dependencies will be reviewed for necessity, source, maintenance, security, transitive impact, install behavior, license, compatibility, data handling, and replacement risk. Automated dependency tools may propose pull requests but will not bypass review and release controls.

Sensitive GitHub Actions workflows will use explicit least-privilege permissions and immutable action references. Untrusted pull-request code will not receive production secrets, deployment credentials, infrastructure authority, publishing authority, or privileged tokens.

Deployment and publishing workflows will prefer OIDC federation where supported. Long-lived tokens will be narrowly scoped, environment specific, protected, rotatable, and used only where federation is unavailable.

Production release artifacts will receive cryptographic checksums and associated SBOMs. Habitacores will generate and retain build provenance and use artifact attestations where supported.

The preferred release model is build once, validate, attest, approve, and promote the same artifact. Where a managed platform performs its own build, that build must remain controlled and attributable to the approved source and platform workflow.

Habitacores will use SLSA as an incremental maturity framework. It will not claim a SLSA level until the relevant requirements have been implemented, assessed, and demonstrated.

Known vulnerabilities will be triaged according to applicability, reachability, exploitability, exposure, data sensitivity, tenant impact, financial impact, and legal impact. Exceptions will require an owner, rationale, compensating controls, expiry, monitoring, and resolution plan.

Secret, code, dependency, license, workflow, and infrastructure findings will enter governed remediation workflows. Deleting a finding, suppressing a scanner, or removing a secret from the latest commit will not substitute for risk analysis and credential rotation.

Codex and other coding agents may propose source, tests, dependency changes, workflows, and documentation through controlled pull requests. They will not approve, merge, publish, deploy, access production credentials, apply infrastructure, alter production data, or suppress security controls independently.

No developer, coding agent, dependency bot, package maintainer, GitHub Action, build runner, registry, deployment provider, or release operator may bypass Habitacores source review, dependency locking, security checks, workflow permissions, artifact integrity, provenance, environment protection, release authorization, or post-deployment verification.