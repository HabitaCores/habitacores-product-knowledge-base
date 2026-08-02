# ADR-019 — Infrastructure as Code, State Management, Provisioning, and Drift-Control Strategy

## Decision Record

| Field | Value |
|---|---|
| Product | Habitacores |
| ADR | ADR-019 |
| Version | 0.1 |
| Status | Proposed |
| Decision Type | Foundational Infrastructure, Security, and DevOps Architecture |
| Product Visionary | Idris O. Junaid |
| Chief Product Architect | AI-assisted product architecture function |
| Infrastructure-as-Code Engine | OpenTofu |
| Configuration Language | HCL |
| Infrastructure Source Authority | Git repository |
| Automation Platform | GitHub Actions |
| State Model | Remote, encrypted, environment isolated, and locked |
| Production Apply | Protected workflow with explicit authorization |
| Pull-Request Apply | Prohibited |
| Drift Detection | Scheduled and on-demand |
| Provider Versions | Constrained and lock-file controlled |
| Authentication | Short-lived OIDC where supported; scoped secrets only where required |
| Existing Resources | Imported before authoritative IaC management |
| Database Schema Migration | Separate from infrastructure provisioning |
| Application Deployment | Separate from infrastructure provisioning |
| Secrets Values in State | Minimized and prohibited where avoidable |
| Manual Production Changes | Emergency exception only; must be reconciled into code |
| Destructive Changes | Explicit review and protection required |
| Initial Managed Platforms | Vercel, Supabase, GitHub, DNS where supported, and future worker infrastructure |
| Last Updated | 1 August 2026 |

## 1. Context

Habitacores will operate infrastructure and platform configuration across:

- Vercel
- Supabase
- GitHub
- domain and DNS services
- transactional email infrastructure
- mobile-build infrastructure
- payment providers
- signature providers
- observability providers
- feature-management providers
- background-worker hosting
- future cloud services
- security and networking controls

ADR-013 established:

- Environment isolation
- typed configuration
- secret boundaries
- immutable deployments
- protected releases
- migration sequencing
- rollback and forward-repair principles

Those principles require a reproducible infrastructure-management mechanism.

Without Infrastructure as Code, Habitacores risks:

- undocumented manual configuration;
- inconsistent staging and production environments;
- unrecoverable platform settings;
- configuration drift;
- excessive administrator privileges;
- accidental destructive changes;
- production resources with unknown ownership;
- missing audit history;
- secrets embedded in scripts;
- environments that cannot be recreated;
- deployment workflows dependent on one person’s account;
- provider settings that cannot be reviewed before application.

## 2. Decision Drivers

The decision is driven by the need to:

1. Make infrastructure reproducible.
2. place infrastructure definitions under review.
3. isolate environments and state.
4. detect configuration drift.
5. protect production changes.
6. support disaster recovery.
7. reduce undocumented manual administration.
8. preserve provider portability where practical.
9. constrain provider and tool versions.
10. protect credentials.
11. support existing-resource import.
12. distinguish infrastructure from application and database changes.
13. support safe destructive-change review.
14. establish ownership and operational accountability.
15. support future worker infrastructure.
16. support audit and compliance evidence.
17. prevent preview environments from controlling production.
18. preserve recovery from state loss or corruption.
19. provide stable implementation boundaries for Codex.
20. establish DevOps controls from the beginning.

## 3. Decision

Habitacores will use OpenTofu as its initial Infrastructure-as-Code engine.

Infrastructure definitions will:

- use declarative HCL;
- reside in a reviewed Git repository;
- use reusable modules;
- maintain isolated environment roots;
- use constrained provider versions;
- commit dependency lock files;
- store state remotely;
- encrypt state and plans;
- use state locking;
- execute through protected GitHub Actions workflows;
- preserve plan and apply evidence;
- detect and reconcile drift.

## 4. Architectural Flow

```text
Infrastructure requirement
→ Architecture and security review
→ Module or environment configuration
→ Pull request
→ Formatting and validation
→ Static and policy checks
→ Provider initialization using locked versions
→ Environment-specific plan
→ Human review
→ Merge
→ Protected production apply
→ Post-apply verification
→ State persistence
→ Observability and audit
→ Drift detection
````

## 5. OpenTofu Selection

OpenTofu is selected as the initial engine because it supports:

* Declarative infrastructure definitions
* HCL
* reusable modules
* provider-based integrations
* dependency lock files
* remote backends
* state locking through supported backends
* plan and apply workflows
* import of existing resources
* state and plan encryption
* CI/CD integration

This decision does not assume every Terraform provider behaves correctly under OpenTofu.

Every provider used by Habitacores must pass compatibility and acceptance testing.

## 6. Provider Compatibility

Before approving a provider for production, Habitacores must verify:

* OpenTofu initialization
* provider installation
* authentication
* plan accuracy
* import behavior
* create behavior
* update behavior
* deletion behavior
* sensitive-value handling
* state representation
* provider upgrade behavior
* drift detection
* documentation quality
* maintenance status

Provider branding or registry availability alone is insufficient approval.

## 7. Infrastructure Repository Placement

The initial direction is to keep infrastructure definitions within the Habitacores engineering workspace.

Directional monorepo structure:

```text
infra/
  modules/
    vercel-project/
    supabase-project/
    github-repository/
    dns-zone/
    worker-service/
    observability/
  environments/
    local/
    test/
    preview/
    staging/
    production/
  policies/
  scripts/
  tests/
```

The exact structure may be refined during implementation.

A separate infrastructure repository may be introduced later if access control or organizational scale requires it.

## 8. Infrastructure Boundaries

Infrastructure as Code may manage:

* Platform projects
* environment configuration
* deployment protection
* DNS records
* firewall configuration
* service identities
* cloud resources
* queues
* worker services
* monitoring integrations
* supported repository settings
* approved provider configuration
* storage and database project-level configuration

Infrastructure as Code does not automatically manage:

* PostgreSQL business schema migrations
* application code
* mobile application releases
* agreement templates
* communication templates
* payment transactions
* user records
* tenant business data
* feature-flag runtime values unless explicitly designed
* production secrets in plaintext

## 9. Database Boundary

OpenTofu may provision:

* A Supabase project
* supported project settings
* networking configuration
* supported platform-level resources
* database infrastructure dependencies

Drizzle migrations remain authoritative for:

* Application tables
* constraints
* indexes
* RLS policies
* functions
* triggers
* application views
* business-schema evolution

OpenTofu must not compete with Drizzle for ownership of the same database object.

## 10. Application Deployment Boundary

Infrastructure as Code may configure:

* Vercel project
* domains
* environment associations
* deployment protections
* firewall controls
* integration settings

The application deployment pipeline remains governed by ADR-013.

An infrastructure apply does not automatically imply a production application release.

## 11. Feature-Management Boundary

ADR-018 remains authoritative for:

* Feature definitions
* targeting
* progressive rollout
* kill switches
* dynamic operational configuration

OpenTofu may provision a feature-management project or provider integration.

It must not become the ordinary mechanism for emergency feature evaluation changes unless specifically approved.

## 12. Secret Boundary

Infrastructure code may reference secret identifiers and destinations.

It must not store plaintext production secrets in:

* HCL files
* variable files committed to Git
* pull-request comments
* workflow logs
* plan artifacts
* documentation
* module outputs

Sensitive values must be delivered through approved secret systems and protected workflow inputs.

## 13. State Sensitivity

Infrastructure state must be treated as sensitive.

State may contain:

* Resource identifiers
* endpoints
* provider metadata
* configuration values
* generated credentials
* secret-derived values
* infrastructure topology

Marking a variable as sensitive does not guarantee that the value is absent from state.

The design must minimize sensitive state content rather than relying only on output masking.

## 14. State Model

Habitacores will use remote state for shared and production environments.

State must provide:

* Encryption at rest
* secure transport
* access control
* versioning or recovery history
* locking or equivalent concurrency protection
* backup
* integrity monitoring
* environment isolation
* audit where available

Local production state is prohibited.

## 15. State Separation

State will be separated by environment and material failure boundary.

Directional examples:

```text
habitacores/staging/platform
habitacores/staging/data-platform
habitacores/production/platform
habitacores/production/data-platform
habitacores/production/workers
habitacores/production/observability
```

The exact topology will balance isolation against operational complexity.

## 16. No Shared Production and Non-Production State

Production and non-production resources must not share one state file.

A preview or staging apply must not gain the authority to modify production resources through shared state.

## 17. State Locking

Concurrent state-changing operations must be prevented.

The state backend or automation platform must support:

* Lock acquisition
* lock visibility
* bounded lock timeout
* safe recovery
* controlled force-unlock procedure

Force unlock is an exceptional operational action requiring evidence that no valid apply is running.

## 18. State Encryption

State and plan artifacts will be encrypted at rest.

Encryption keys must be:

* Stored outside the state
* access controlled
* backed up
* recoverable
* rotated under an approved procedure
* unavailable to ordinary application runtimes

Loss of the encryption key may make the state unrecoverable.

Key recovery must therefore be tested.

## 19. State Backup and Recovery

Habitacores must preserve recoverable state history.

Recovery testing should verify:

* Previous state version retrieval
* encryption-key availability
* backend access
* lock recovery
* restoration procedure
* provider refresh after restore
* absence of unauthorized resource recreation

A state backup is not a substitute for provider-level data backup.

## 20. State Access

Production state access must use least privilege.

Potential roles include:

```text
INFRASTRUCTURE_PLANNER
INFRASTRUCTURE_APPLIER
INFRASTRUCTURE_ADMINISTRATOR
STATE_RECOVERY_OPERATOR
SECURITY_REVIEWER
```

Not every developer requires state-download permission.

## 21. Provider Credentials

Provider credentials should use:

* Short-lived OIDC identities where supported;
* environment-specific service identities;
* least-privilege roles;
* scoped API tokens only where federation is unavailable;
* rotation and revocation procedures.

Personal administrator credentials must not become permanent CI authority.

## 22. GitHub Actions

GitHub Actions will run infrastructure automation.

Directional workflows:

```text
infra-check.yml
infra-plan.yml
infra-apply-staging.yml
infra-apply-production.yml
infra-drift-detection.yml
infra-import-validation.yml
```

Exact workflow names may be refined.

## 23. Pull-Request Validation

Infrastructure pull requests should run:

* `tofu fmt -check`
* `tofu init`
* `tofu validate`
* provider lock-file verification
* module tests where available
* static analysis
* policy checks
* secret scanning
* environment-reference validation
* plan generation for eligible environments
* destructive-change detection

## 24. Pull-Request Plans

A pull request may generate a non-destructive infrastructure plan using restricted planning credentials.

The plan must:

* target the intended environment;
* use protected state access;
* redact sensitive values;
* be retained according to policy;
* identify additions, modifications, replacements, and deletions;
* be invalidated when the source commit changes.

A plan generated for one commit must not be applied to a different commit.

## 25. Pull-Request Apply Prohibition

Ordinary pull-request workflows must not apply production infrastructure.

Untrusted or unmerged code must not receive production apply authority.

## 26. Production Apply

Production apply requires:

* Approved source commit
* successful validation
* reviewed plan
* protected GitHub environment
* authorized actor
* correct production state
* approved credentials
* concurrency lock
* change record
* post-apply verification

High-risk changes may require additional approval.

## 27. Saved Plans

Where technically practical, production should apply the reviewed saved plan rather than regenerating an uncontrolled plan immediately before application.

A saved plan must be:

* Bound to the source commit
* bound to the environment
* encrypted
* access controlled
* short lived
* invalidated when state materially changes

If a fresh plan is required, it must receive equivalent review.

## 28. Destructive Changes

Destructive changes include:

* Resource deletion
* forced replacement
* database-project deletion
* storage deletion
* DNS-zone deletion
* production-domain removal
* credential invalidation
* state-backend modification
* logging or security-control removal

Such changes require explicit identification and approval.

## 29. Destruction Protection

Critical resources should use available protection mechanisms such as:

* lifecycle prevention
* provider deletion protection
* platform retention controls
* separate administrative workflows
* multi-step decommissioning

Protection mechanisms do not eliminate the need for backups and recovery plans.

## 30. Resource Ownership

Every managed resource should have:

* Environment
* owner
* service
* product
* purpose
* cost center where applicable
* data classification
* lifecycle
* repository reference
* module reference

Tags or labels should be used where providers support them.

## 31. Naming Convention

Resource names should be deterministic and environment aware.

Directional pattern:

```text
habitacores-<environment>-<service>-<purpose>
```

Names must not contain sensitive customer or tenant information.

## 32. Modules

Reusable modules should represent stable infrastructure capabilities.

A module should have:

* Clear responsibility
* typed variables
* validation
* documented outputs
* safe defaults
* versioning
* tests
* upgrade notes
* examples using synthetic values

Modules must not hide destructive behavior.

## 33. Module Composition

Environment roots should compose modules.

They should not duplicate complete infrastructure definitions unnecessarily across environments.

Environment differences must remain explicit rather than buried in uncontrolled conditionals.

## 34. Provider Versioning

Providers must have constrained versions.

The dependency lock file must be committed.

Provider upgrades require:

* Pull request
* release-note review
* compatibility validation
* plan review
* state-schema consideration
* staging verification
* production rollout

Habitacores must not automatically install an unrestricted latest provider in production.

## 35. OpenTofu Versioning

The OpenTofu version must be pinned or constrained consistently across:

* Local development
* CI
* staging
* production operations
* recovery procedures

Version upgrades require compatibility review and state-backup verification.

## 36. Existing Resources

Existing manually created resources must not be recreated blindly.

The transition process is:

```text
Inventory existing resource
→ Confirm ownership
→ Write matching configuration
→ Import resource
→ Refresh state
→ Generate plan
→ Resolve differences
→ Verify no unintended replacement
→ Approve IaC authority
```

## 37. Import Safety

Import does not prove the configuration matches the existing resource.

After import, the team must review the plan for:

* Replacement
* deletion
* reset values
* unsupported properties
* provider defaults
* hidden drift
* secret regeneration

## 38. Brownfield Adoption

Habitacores already has resources such as:

* `habitacores.com`
* DNS configuration
* Zoho Mail records
* GitHub repositories
* potentially Vercel and Supabase resources as implementation progresses

These are brownfield resources.

They require inventory and selective import rather than assumed recreation.

## 39. Domain Protection

The registered domain:

```text
habitacores.com
```

is a critical asset.

Infrastructure automation must not be given domain-transfer, registrar-ownership, or destructive DNS authority unless specifically required and protected.

Initial DNS automation may manage selected records while registrar control remains separately secured.

## 40. Zoho Mail DNS

Existing Zoho Mail records include operational mail dependencies such as:

* MX
* SPF
* DKIM
* verification records

Infrastructure adoption must import or preserve these records.

A plan that removes or replaces them unexpectedly must be blocked.

## 41. Vercel Infrastructure

Subject to provider support and testing, IaC may manage:

* Vercel projects
* domain associations
* environment configuration references
* deployment protections
* selected firewall configuration
* supported project settings

Application deployments remain controlled separately.

## 42. Supabase Infrastructure

Subject to provider support and testing, IaC may manage:

* Supabase projects
* supported project settings
* supported branches
* selected platform configuration

Drizzle remains authoritative for the application database schema.

Supabase provider behavior must be tested because provider coverage may be partial.

## 43. GitHub Infrastructure

Subject to provider support and organizational policy, IaC may manage:

* Repositories
* branch protections
* environments
* teams
* selected repository settings
* deployment variables that are non-secret
* required workflows where supported

Care must be taken to avoid locking administrators out of the repository used to repair the configuration.

## 44. Worker Infrastructure

When the worker-hosting provider is selected, IaC should manage:

* Worker services
* runtime configuration references
* service identities
* networking
* autoscaling bounds
* queues where applicable
* health checks
* logging and telemetry integration
* deployment permissions

Worker application code remains deployed through its release pipeline.

## 45. Preview Infrastructure

Ephemeral preview infrastructure may be introduced selectively.

It requires:

* Unique naming
* non-production credentials
* restricted data
* lifecycle expiry
* cost limits
* cleanup
* no production connectivity
* no production DNS mutation
* no production provider events

Not every pull request requires a complete isolated infrastructure stack.

## 46. Drift

Drift means managed infrastructure differs from its approved configuration or state.

Drift may result from:

* Manual console changes
* provider-side defaults
* external automation
* compromised credentials
* emergency intervention
* provider behavior changes
* incomplete import
* resource replacement outside IaC

## 47. Drift Detection

Habitacores will run drift detection:

* On a schedule
* before high-risk production changes
* after emergency manual intervention
* after suspected provider compromise
* on demand during investigation

Drift checks should use read-only or planning authority where possible.

## 48. Drift Outcomes

Directional outcomes:

```text
NO_DRIFT
EXPECTED_PROVIDER_NORMALIZATION
UNAPPROVED_CONFIGURATION_CHANGE
RESOURCE_MISSING
UNMANAGED_RESOURCE
STATE_MISMATCH
PROVIDER_ERROR
REQUIRES_REVIEW
```

Drift must not be automatically corrected in production without review.

## 49. Drift Remediation

Approved remediation options include:

* Change provider resource back to code-defined state
* update code to reflect an approved emergency change
* import an approved unmanaged resource
* remove an unauthorized resource
* repair state under a controlled procedure
* defer with a documented exception

The selected response depends on risk.

## 50. Manual Production Changes

Routine manual production infrastructure changes are prohibited.

An emergency manual change may occur only when:

* Delay would create greater harm;
* the operator is authorized;
* the scope is minimized;
* the change is recorded;
* verification is performed;
* drift detection follows;
* the approved configuration is updated promptly;
* a post-incident review occurs.

The console must not become a parallel infrastructure authority.

## 51. Change Records

Material infrastructure changes should preserve:

* Change ID
* source commit
* pull request
* environment
* plan identifier
* actor
* approver where required
* apply workflow
* start and completion time
* affected resources
* outcome
* post-apply verification
* rollback or recovery relationship
* incident reference

## 52. Audit Events

Potential audit events include:

```text
INFRASTRUCTURE_PLAN_CREATED
INFRASTRUCTURE_APPLY_STARTED
INFRASTRUCTURE_APPLY_COMPLETED
INFRASTRUCTURE_APPLY_FAILED
INFRASTRUCTURE_DESTROY_REQUESTED
INFRASTRUCTURE_RESOURCE_IMPORTED
INFRASTRUCTURE_DRIFT_DETECTED
INFRASTRUCTURE_DRIFT_RESOLVED
INFRASTRUCTURE_STATE_RECOVERED
INFRASTRUCTURE_FORCE_UNLOCKED
EMERGENCY_INFRASTRUCTURE_CHANGE_EXECUTED
```

Audit metadata must not contain state contents or secrets.

## 53. Observability

Infrastructure automation should expose:

* Plan duration
* apply duration
* apply result
* changed-resource count
* replacement count
* deletion count
* drift result
* provider errors
* state-lock duration
* authentication failure
* post-apply verification result

Resource identifiers must be restricted where sensitive.

## 54. Alerts

Priority alerts may include:

* Production drift detected
* missing critical resource
* unexpected destructive plan
* state-backend access failure
* state lock held beyond threshold
* provider authentication failure
* production apply from unauthorized source
* production apply using non-production credentials
* DNS mail records removed
* domain association changed unexpectedly
* state-encryption failure
* critical post-apply health failure

## 55. Rollback

Infrastructure rollback is not always equivalent to applying an earlier commit.

A prior configuration may be incompatible with:

* Current provider state
* migrated resources
* new data
* irreversible platform actions
* deleted resources
* rotated credentials

Every elevated infrastructure change requires a recovery strategy.

## 56. Forward Repair

Forward repair is preferred when reverting would:

* Delete current data
* recreate resources
* rotate identifiers
* break application dependencies
* invalidate provider contracts
* worsen an incident

The recovery plan must distinguish configuration rollback from service restoration.

## 57. Resource Decommissioning

Decommissioning requires:

1. Usage inventory.
2. dependency review.
3. data export or migration.
4. retention and legal-hold review.
5. traffic removal.
6. credential revocation.
7. observation period.
8. approved destruction.
9. state removal.
10. audit and cost verification.

Removing a resource from HCL without a decommissioning plan is prohibited.

## 58. Policy Enforcement

Habitacores may introduce policy-as-code checks for rules such as:

* No public database exposure
* no production resource without owner
* no unencrypted storage
* no unrestricted network ingress
* no deletion of protected resources
* no production configuration in preview
* no missing environment tags
* no broad administrative identity
* no plaintext secret variables
* no unbounded autoscaling cost

The exact policy engine is deferred.

## 59. Testing

Infrastructure tests should cover:

* Formatting
* initialization
* validation
* module inputs
* module outputs
* provider compatibility
* environment isolation
* destructive-change detection
* import behavior
* state encryption
* lock behavior
* drift detection
* OIDC or credential scope
* DNS preservation
* post-apply health
* disaster recovery

## 60. Production-Isolation Test

```text
Given:
A staging infrastructure workflow executes.

When:
It authenticates and loads its state.

Then:
It can access only staging resources and staging state.

And:
It cannot plan or modify production infrastructure.
```

## 61. Pull-Request Safety Test

```text
Given:
An unmerged pull request changes production infrastructure definitions.

When:
The pull-request workflow executes.

Then:
It may validate and produce an authorized plan.

And:
It cannot apply production infrastructure.
```

## 62. State-Recovery Test

```text
Given:
The latest production state becomes unavailable or corrupted.

When:
The approved recovery procedure executes.

Then:
An authorized operator can recover a verified prior state version.

And:
The operator can generate a plan without unintentionally recreating
existing production resources.
```

## 63. DNS-Preservation Test

```text
Given:
Existing Zoho Mail DNS records serve habitacores.com.

When:
DNS resources are imported into Infrastructure as Code.

Then:
The resulting plan preserves the approved MX, SPF, DKIM, and verification records.

And:
No mail-critical record is removed without explicit approval.
```

## 64. Destructive-Plan Test

```text
Given:
A proposed change would delete the production Supabase project.

When:
The plan is generated.

Then:
The destructive change is clearly identified and blocked by applicable controls.

And:
An ordinary production apply cannot proceed.
```

## 65. Drift Test

```text
Given:
A production firewall setting is changed manually.

When:
Scheduled drift detection executes.

Then:
The difference is reported.

And:
Habitacores does not silently accept or overwrite the change without review.
```

## 66. Existing-Resource Import Test

```text
Given:
A Vercel project was created manually before Infrastructure as Code adoption.

When:
The resource is imported.

Then:
The next plan is reviewed for replacement, deletion, and provider defaults.

And:
The project is not recreated under a new identity.
```

## 67. Codex Boundaries

Every Codex infrastructure work package must specify:

* Environment
* provider
* resource owner
* module
* state boundary
* authentication method
* secret handling
* plan behavior
* destructive-change risk
* import requirement
* rollback or recovery strategy
* post-apply verification
* drift behavior
* observability
* tests

Codex must not:

* Store production state locally
* commit state or plan files
* commit secrets
* apply production from a pull request
* use personal tokens as permanent production authority
* combine staging and production state
* manage application schema through competing tools
* recreate existing resources without import review
* remove DNS records blindly
* disable deletion protection casually
* use unrestricted provider versions
* omit the dependency lock file
* print state contents in CI
* auto-correct production drift without review
* assume infrastructure rollback is always reversible
* create resources without ownership and environment metadata

## 68. Prohibited Patterns

The following are not approved:

* Click-only production infrastructure
* local production state
* plaintext production secrets in HCL
* committed `.tfstate` or plan files
* one state file for every environment
* unreviewed production apply
* pull-request production apply
* unrestricted provider upgrades
* infrastructure controlled by one personal account
* automatic resource recreation after state loss
* direct schema competition between OpenTofu and Drizzle
* destructive change hidden inside a reusable module
* manual production change left unreconciled
* drift auto-remediation without authorization
* deleting imported resources to make the plan clean
* treating state backup as a database-data backup

## 69. Options Considered

### Option A — Manual Provider Consoles

Advantages:

* Immediate
* low initial setup
* visually accessible

Disadvantages:

* Poor reproducibility
* weak review
* configuration drift
* personal-account dependence
* difficult recovery
* limited automation

Decision:

Rejected as the authoritative infrastructure method.

### Option B — Deployment-Platform Configuration Only

Advantages:

* Simple for web hosting
* integrated with Vercel workflows

Disadvantages:

* Does not manage Supabase, DNS, GitHub, workers, or future infrastructure
* provider lock-in
* incomplete drift control
* fragmented change history

Decision:

Rejected as the complete architecture.

### Option C — Terraform

Advantages:

* Mature ecosystem
* broad provider support
* established operating model
* substantial documentation

Disadvantages:

* Licensing and ecosystem-governance considerations
* state encryption may require backend-specific controls
* unnecessary coupling when an open compatible engine meets requirements

Decision:

Not selected initially.

Terraform-provider compatibility remains relevant and must be tested.

### Option D — Pulumi

Advantages:

* General-purpose programming languages
* TypeScript alignment
* strong abstraction capabilities
* broad provider ecosystem

Disadvantages:

* Infrastructure logic may become overly imperative
* additional runtime and dependency complexity
* greater risk of application-style abstractions hiding infrastructure behavior
* less direct alignment with the desired HCL review model

Decision:

Not selected initially.

### Option E — OpenTofu With GitHub-Governed Automation

Advantages:

* Declarative HCL
* open-source governance
* remote state support
* provider ecosystem
* dependency locking
* state and plan encryption
* Git-based review
* portability
* compatibility with established Terraform-style workflows

Disadvantages:

* Provider compatibility still requires testing
* remote-state backend must be selected
* state encryption keys require recovery procedures
* SaaS providers may expose incomplete resource coverage
* infrastructure expertise is required

Decision:

Approved.

## 70. Positive Consequences

* Infrastructure becomes reproducible.
* production changes become reviewable.
* environments remain isolated.
* provider versions remain controlled.
* existing resources can be adopted safely.
* drift becomes detectable.
* emergency manual changes must be reconciled.
* state becomes protected and recoverable.
* web, database-platform, DNS, and worker infrastructure can converge under one governance model.
* DevOps controls are established before production scale.

## 71. Negative Consequences

* Infrastructure code requires maintenance.
* state storage becomes a critical dependency.
* provider limitations may require documented manual exceptions.
* imports require careful work.
* platform changes may require provider upgrades.
* production plans require knowledgeable review.
* state encryption introduces key-recovery responsibilities.
* IaC does not eliminate provider-specific operational knowledge.

## 72. Risks

### 72.1 State Exposure

Controls:

* Remote encrypted state
* least privilege
* no CI output
* restricted downloads
* short retention for plan artifacts
* key management
* audit

### 72.2 Accidental Destruction

Controls:

* Plan review
* lifecycle protection
* protected apply
* destructive-change detection
* decommissioning process
* backups
* post-apply verification

### 72.3 Provider Incompatibility

Controls:

* Acceptance tests
* pinned versions
* staging validation
* upgrade review
* documented manual exception where necessary

### 72.4 Configuration Drift

Controls:

* Scheduled plans
* restricted console access
* emergency-change reconciliation
* ownership
* alerts

### 72.5 Lost Encryption Key

Controls:

* Key backup
* recovery testing
* controlled rotation
* separation of state and key storage
* documented recovery ownership

### 72.6 Infrastructure Lockout

Controls:

* Break-glass procedure
* independent recovery identity
* repository protection review
* state-recovery access
* staged adoption
* no circular dependency without recovery path

## 73. Implementation Sequence

```text
1. Inventory all existing Habitacores infrastructure and accounts
2. classify resources by environment, owner, sensitivity, and IaC eligibility
3. establish the infra directory and naming convention
4. pin the OpenTofu version
5. establish provider requirements and dependency lock files
6. select the remote-state backend
7. implement state encryption and locking
8. establish state backup and recovery procedures
9. configure GitHub Actions validation
10. establish restricted planning credentials
11. establish protected staging apply
12. import existing non-production resources
13. validate provider behavior
14. establish protected production apply
15. import production Vercel, Supabase, GitHub, and DNS resources selectively
16. protect habitacores.com and Zoho Mail DNS records
17. implement destructive-plan detection
18. implement post-apply verification
19. implement scheduled drift detection
20. establish emergency manual-change reconciliation
21. add module and policy tests
22. provision future worker infrastructure through approved modules
23. test full state recovery
24. document operational runbooks and ownership
```

## 74. Deferred Decisions

This ADR does not decide:

* Exact remote-state backend
* exact state-lock implementation
* exact state-encryption key provider
* exact OpenTofu version
* exact provider versions
* exact policy-as-code engine
* exact worker-hosting provider
* exact secrets manager
* exact cloud account structure
* exact DNS provider
* whether the domain registrar will be managed through IaC
* exact infrastructure repository split
* exact production approval count
* exact saved-plan retention
* exact drift-detection cadence
* exact ephemeral-preview topology
* exact cost-management provider
* exact disaster-recovery region
* exact break-glass identity platform

These require bounded implementation decisions.

## 75. Supersession Conditions

This ADR should be reviewed when:

* OpenTofu no longer meets operational requirements.
* provider compatibility materially limits infrastructure coverage.
* Habitacores adopts multiple cloud providers.
* regional infrastructure isolation becomes mandatory.
* enterprise customers require dedicated infrastructure.
* worker scale requires Kubernetes or another orchestration platform.
* a dedicated infrastructure repository becomes necessary.
* a managed infrastructure automation platform is adopted.
* stronger policy-as-code controls become mandatory.
* supply-chain attestations become required.
* infrastructure state must be hosted in a regulated region.

## 76. Final Decision Statement

Habitacores will use OpenTofu and declarative HCL as its initial Infrastructure-as-Code architecture.

Infrastructure definitions will reside in Git, use reusable modules, constrain provider versions, commit dependency lock files, and pass formatting, validation, security, policy, and plan review before application.

Production and non-production infrastructure will use separate remote states, credentials, environment roots, and authorization boundaries. Production state will not be stored locally or shared with preview, test, or staging environments.

State and plan artifacts will be treated as sensitive, encrypted at rest, access controlled, recoverable, and protected through locking and backup procedures. Habitacores will minimize secret values in state rather than relying only on output masking.

GitHub Actions will provide infrastructure validation, planning, protected application, evidence collection, and drift detection. Ordinary pull-request workflows will not apply production infrastructure.

Short-lived OIDC credentials will be preferred wherever providers support them. Scoped, environment-specific, rotatable API credentials may be used only where federation is unavailable.

Existing resources will be inventoried, represented in configuration, imported, refreshed, and reviewed before Infrastructure as Code becomes authoritative. Habitacores will not recreate manually established production resources merely to obtain a clean initial plan.

The `habitacores.com` domain, Zoho Mail DNS records, GitHub resources, Vercel projects, Supabase projects, and future worker infrastructure will be adopted carefully according to provider capability and tested compatibility.

Infrastructure as Code will remain separate from PostgreSQL application-schema migrations, application deployment, mobile-store releases, runtime feature flags, secret values, and authoritative business data.

Production infrastructure applies will require approved source, reviewed plans, protected workflows, correct state, authorized identities, change evidence, concurrency protection, and post-apply verification.

Destructive changes will be identified explicitly and protected through lifecycle controls, provider safeguards, additional review, decommissioning procedures, backups, and recovery plans.

Routine manual production changes are prohibited. Emergency manual intervention must remain attributable, minimally scoped, verified, followed by drift detection, reconciled into code, and reviewed afterward.

Drift will be detected on a schedule and after material emergency intervention. Habitacores will not automatically accept or overwrite production drift without determining whether the code, state, or live resource represents the approved condition.

No developer, provider console, pull request, preview workflow, personal token, coding agent, or AI model may silently apply production infrastructure, expose state, commit secrets, bypass environment isolation, remove protected resources, overwrite approved configuration, recreate existing infrastructure, or establish a competing infrastructure authority outside the reviewed Habitacores process.