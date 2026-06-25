# Holistic Software Failure Taxonomy

This taxonomy starts with a more useful question than “What should good code look like?”:

> **How can this system be wrong, harmful, unavailable, insecure, unrecoverable, unauditable, or impossible to change safely?**

The hierarchy is intentionally broader than application security. Enterprise failures emerge from misunderstood requirements, valid-but-dangerous inputs, domain-rule errors, architecture coupling, unsafe configuration, dependency compromise, release blast radius, inaccessible interfaces, missing ownership, failed restoration, and AI-agent behavior as often as from a classic vulnerability.

Each subcategory has a stable identifier and three fields:

- **What can go wrong**: the failure surface.
- **Enterprise hardening intent**: the outcome controls should create.
- **Expected evidence**: artifacts that can support a pass/fail decision.

These are taxonomy entries, not yet atomic controls. Each domain maps to a row in the Vibeguard Scorecard ([`checklist.md`](../checklist.md)), where an AI tool assesses the project's current state against it.

## 1. Failure primitives

The domains below can be analyzed using a common set of failure primitives.

| Primitive | Meaning |
|---|---|
| `OMISSION` | A required behavior, check, state, path, or owner does not exist. |
| `INCORRECT` | The system produces a wrong result or state despite appearing to operate. |
| `INCONSISTENT` | Equivalent inputs, replicas, services, versions, tenants, or times produce conflicting outcomes. |
| `UNAUTHORIZED` | An actor gains access or performs an action beyond intended authority. |
| `DISCLOSURE` | Sensitive information is exposed to an unintended party or context. |
| `TAMPERING` | Code, data, configuration, artifacts, evidence, or decisions are altered without valid authority or detection. |
| `UNAVAILABLE` | A required service or capability cannot be used within its objective. |
| `EXHAUSTION` | Finite resources, budgets, quotas, or human attention are consumed without safe bounds. |
| `IRRECOVERABLE` | State or service cannot be restored completely and within the required objective. |
| `UNOBSERVABLE` | Material behavior or failure cannot be detected, attributed, explained, or audited. |
| `INCOMPATIBLE` | A change breaks a consumer, producer, stored state, protocol, platform, or operational process. |
| `UNSAFE_CHANGE` | A valid change is released with excessive blast radius, inadequate verification, or no viable reversal. |
| `ABUSE` | Legitimate functionality is used at scale or in sequence to create harm. |
| `NONCOMPLIANT` | Behavior or evidence violates law, contract, policy, accessibility, safety, or records obligations. |
| `UNMAINTAINABLE` | The system becomes too difficult, fragile, costly, or knowledge-dependent to change safely. |

## 2. Cross-cutting lenses

Every domain should be challenged through each lens below. Many severe incidents live in the gap between two ordinary components.

### 2.1 State and input lens

- Missing, null, empty, malformed, oversized, duplicated, stale, corrupted, reordered, conflicting, ambiguous, locale-sensitive, and adversarial input.
- Input that is individually valid but unsafe in combination, frequency, sequence, scale, timing, or propagation.
- State before, during, and after partial execution.
- Mixed versions, mixed schemas, mixed configuration, and interrupted migrations.
- Clock skew, daylight-saving changes, leap behavior, expiry, delay, and replay.
- Precision, rounding, overflow, underflow, unit, currency, and timezone errors.

### 2.2 Actor and authority lens

- Anonymous user, authenticated user, tenant administrator, internal operator, support agent, developer, service identity, dependency, compromised account, malicious insider, and automated agent.
- Horizontal, vertical, cross-tenant, cross-environment, and confused-deputy escalation.
- Accidental misuse by an authorized actor.
- Bulk or scripted use of legitimate functionality.

### 2.3 Failure and dependency lens

- Slow, unavailable, incorrect, stale, compromised, rate-limited, partitioned, or partially successful dependency.
- Retry storms, feedback loops, synchronized recovery, cascading failure, and thundering herds.
- Queue backlog, poison messages, duplicate delivery, dropped delivery, and out-of-order events.
- Loss of DNS, identity provider, control plane, certificate authority, secrets manager, telemetry, or feature-flag service.
- Failure of the mechanism intended to detect or recover from failure.

### 2.4 Change lens

- Developer error, generated-code error, malicious change, stale branch, incorrect merge, incomplete rollout, mixed version, bad data/configuration, dependency update, infrastructure drift, emergency bypass, rollback, and decommission.
- Change that is syntactically valid, reviewed, and test-passing but unsafe at production scale.
- A local change that propagates globally before humans can understand it.

### 2.5 Consequence lens

- Wrong business outcome.
- Unauthorized action or disclosure.
- Human or physical harm.
- Financial loss or fraud.
- Legal, contractual, accessibility, or records violation.
- Data corruption or irrecoverable loss.
- Service unavailability or degraded trust.
- Operational overload and responder fatigue.
- Lock-in, unmaintainability, or inability to evolve.

## 3. Taxonomy summary

| Domain | ID range | Subcategories | Primary failure question |
|---|---:|---:|---|
| **GOV: Governance, Criticality and Ownership** | `GOV-01–GOV-10` | 10 | Could the team build, approve, operate, or retire the wrong thing without accountable risk decisions? |
| **REQ: Requirements, Scope and Acceptance** | `REQ-01–REQ-10` | 10 | Could the implementation be technically polished yet solve the wrong problem, omit a constraint, or have no objective definition of done? |
| **ARC: Architecture, Boundaries and Evolvability** | `ARC-01–ARC-10` | 10 | Could the system's structure amplify defects, blur trust boundaries, lock in unsafe choices, or make change and recovery disproportionately risky? |
| **DOM: Domain Logic and Functional Correctness** | `DOM-01–DOM-10` | 10 | Could the code execute exactly as written while producing a materially wrong business result? |
| **COD: Code Quality, Maintainability and Resource Safety** | `COD-01–COD-10` | 10 | Could the implementation be correct today but fragile, opaque, leak-prone, nondeterministic, or prohibitively expensive to change? |
| **API: APIs, Contracts and Integrations** | `API-01–API-10` | 10 | Could an interface expose, corrupt, over-consume, or break systems because its contract, trust, compatibility, and failure semantics are weak? |
| **DAT: Data, Storage, Caching and Migrations** | `DAT-01–DAT-10` | 10 | Could persistent state become inconsistent, unrecoverable, exposed, or incompatible even when application tests pass? |
| **IAM: Identity, Authentication, Authorization and Sessions** | `IAM-01–IAM-10` | 10 | Could an attacker, departed user, service, or tenant act as someone else or retain more access than intended? |
| **SEC: Application Security and Abuse Resistance** | `SEC-01–SEC-12` | 12 | Could malicious or malformed input, unsafe defaults, exceptional conditions, or business abuse compromise confidentiality, integrity, or availability? |
| **PRV: Privacy and Data Protection** | `PRV-01–PRV-10` | 10 | Could the system use, infer, expose, retain, or share personal data beyond justified expectations and obligations? |
| **SAF: Safety, Human Impact and Responsible Automation** | `SAF-01–SAF-10` | 10 | Could software behavior, automation, or failure cause physical, financial, legal, social, or systemic harm beyond ordinary defects? |
| **SUP: Dependencies, Provenance and Software Supply Chain** | `SUP-01–SUP-12` | 12 | Could code, packages, tools, services, or artifacts be malicious, vulnerable, counterfeit, abandoned, legally unusable, or different from what was reviewed? |
| **BLD: Build, CI, Artifact Integrity and Repository Controls** | `BLD-01–BLD-12` | 12 | Could the development and build system alter, leak, or approve code and artifacts outside the intended review and trust process? |
| **CFG: Configuration, Secrets and Feature Flags** | `CFG-01–CFG-12` | 12 | Could configuration or generated policy behave like untested code, propagate globally, expose secrets, or make rollback impossible? |
| **INF: Infrastructure, Runtime, Network and Platform Security** | `INF-01–INF-12` | 12 | Could insecure or coupled runtime foundations expose the application, amplify failure, or prevent safe operation and recovery? |
| **REL: Reliability, Resilience and Distributed Systems** | `REL-01–REL-14` | 14 | Could routine component failure, delay, duplication, overload, or recovery behavior cascade into prolonged or incorrect service failure? |
| **PER: Performance, Scalability, Capacity and Cost** | `PER-01–PER-12` | 12 | Could normal growth, adversarial input, or inefficient design violate latency and throughput objectives or create runaway resource and cost consumption? |
| **TST: Verification, Testing and Independent Assurance** | `TST-01–TST-15` | 15 | Could defects survive because tests are narrow, self-fulfilling, flaky, nonrepresentative, or controlled by the same code and agent they evaluate? |
| **OBS: Observability, Auditability and Diagnostics** | `OBS-01–OBS-12` | 12 | Could users be harmed while the system appears healthy, or could responders lack safe, correlated, privacy-preserving evidence to diagnose it? |
| **DPL: Deployment, Release and Change Management** | `DPL-01–DPL-12` | 12 | Could a correct change become an incident because it is too large, inconsistently deployed, globally propagated, incompatible, or hard to stop? |
| **OPS: Operations, Incident Response, Backup and Recovery** | `OPS-01–OPS-14` | 14 | Could the service be impossible to support, contain, restore, reconcile, communicate about, or retire when something goes wrong? |
| **UXA: User Experience, Accessibility, Internationalization and Compatibility** | `UXA-01–UXA-12` | 12 | Could users fail, be excluded, misunderstand risk, lose work, or make unsafe choices because the interface and supported environments are weak? |
| **KNO: Documentation, Knowledge and Supportability** | `KNO-01–KNO-12` | 12 | Could the system depend on vanished chat context or a few individuals because its architecture, operation, decisions, and contracts are not durable? |
| **AIA: AI-Assisted Development and Coding-Agent Safety** | `AIA-01–AIA-16` | 16 | Could an AI coding tool confidently misread intent, obey malicious context, invent dependencies, weaken controls, or take excessive action without accountable verification? |

## 4. Detailed hierarchy

The ordering follows the lifecycle from intent and governance through architecture, implementation, operation, and AI-assisted change. It is not a priority ranking.

## 4.1 GOV: Governance, Criticality and Ownership

**Failure question:** Could the team build, approve, operate, or retire the wrong thing without accountable risk decisions?

**Typical accountable owners:** Product owner, Engineering lead, Security, Risk/Compliance, Service owner

**Primary failure primitives:** `OMISSION`, `NONCOMPLIANT`, `UNOBSERVABLE`, `UNSAFE_CHANGE`, `UNMAINTAINABLE`

**Lifecycle phases:** `govern`, `plan`, `review`, `operate`

### GOV-01: System criticality and impact classification

- **What can go wrong:** The system is treated like a prototype despite handling production, regulated, financial, safety-relevant, or high-blast-radius workloads; controls are selected by habit rather than consequence.
- **Enterprise hardening intent:** Classify every system and change by business impact, data sensitivity, user reach, reversibility, and regulatory or safety exposure; make the classification drive mandatory controls and approval depth.
- **Expected evidence:** Approved criticality profile; data classification; business-impact analysis; control applicability record.

### GOV-02: Ownership and accountability

- **What can go wrong:** No one owns the service, control, dependency, data set, alert, or remediation; shared responsibility becomes shared ambiguity.
- **Enterprise hardening intent:** Assign one accountable owner and named operational, security, data, and product roles; define escalation paths and backup owners.
- **Expected evidence:** RACI or ownership map; CODEOWNERS; service catalog entry; escalation roster.

### GOV-03: Policy and instruction hierarchy

- **What can go wrong:** Conflicting prompts, local conventions, delivery pressure, or tool defaults silently override enterprise requirements.
- **Enterprise hardening intent:** Define deterministic precedence for law, organizational policy, repository policy, profiles, stack overlays, directory rules, and task instructions; prohibit silent override of mandatory controls.
- **Expected evidence:** Instruction precedence document; resolved policy manifest; agent-read confirmation.

### GOV-04: Risk register and threat or hazard model

- **What can go wrong:** Known risks remain tribal knowledge; design addresses happy paths but not attackers, operator mistakes, dependency failure, or harmful outcomes.
- **Enterprise hardening intent:** Maintain a living risk register and threat, abuse, privacy, and hazard models proportionate to criticality; link each material risk to controls, tests, owners, and residual risk.
- **Expected evidence:** Risk register; threat model; misuse cases; hazard log; review record.

### GOV-05: Control applicability and profiles

- **What can go wrong:** A universal checklist creates noise, while teams mark difficult controls not applicable without justification.
- **Enterprise hardening intent:** Use composable profiles for system type, criticality, data class, deployment model, and regulation; require a reason and approver for every not-applicable decision.
- **Expected evidence:** Project profile; applicability matrix; N/A rationale; profile version.

### GOV-06: Exceptions, waivers, and risk acceptance

- **What can go wrong:** Temporary exceptions become permanent; the developer who created a risk also approves it; expiry and compensating controls are absent.
- **Enterprise hardening intent:** Require documented scope, rationale, owner, compensating controls, expiry, review date, and independent approval for every exception; automatically surface expired waivers.
- **Expected evidence:** Exception record; approval; expiry monitor; compensating-control evidence.

### GOV-07: Segregation of duties and approvals

- **What can go wrong:** One person or one agent can author, approve, release, and alter production or evidence; compromised credentials have unlimited reach.
- **Enterprise hardening intent:** Separate authoring, review, release, privileged access, and risk acceptance for material changes; use protected environments and two-person controls where impact warrants.
- **Expected evidence:** Branch and environment protection; approval logs; privileged-action audit.

### GOV-08: Evidence and traceability

- **What can go wrong:** A team claims compliance but cannot show which requirement led to which design, code, test, artifact, deployment, or risk decision.
- **Enterprise hardening intent:** Maintain bidirectional traceability from requirement and risk to control, implementation, verification, evidence, release, and exception; prefer generated evidence over screenshots.
- **Expected evidence:** Traceability matrix; immutable CI records; signed attestations; release dossier.

### GOV-09: Metrics, maturity, and continuous improvement

- **What can go wrong:** Teams optimize test counts or ticket closure instead of escaped defects, recovery, user impact, and risk reduction; recurring failures are not converted into controls.
- **Enterprise hardening intent:** Track outcome-oriented measures, control coverage, escaped defects, change failure, time to detect and recover, waiver age, and action-item closure; review trends and update the framework.
- **Expected evidence:** Scorecard; maturity assessment; trend review; control-change history.

### GOV-10: Third-party and lifecycle governance

- **What can go wrong:** Critical vendors, libraries, services, or maintainers fail, change terms, lose support, or become compromised; systems linger past end-of-life.
- **Enterprise hardening intent:** Inventory third parties and lifecycle dates; define due diligence, support expectations, exit plans, replacement paths, end-of-life gates, and secure retirement.
- **Expected evidence:** Vendor assessment; dependency owner; support matrix; exit plan; retirement record.


## 4.2 REQ: Requirements, Scope and Acceptance

**Failure question:** Could the implementation be technically polished yet solve the wrong problem, omit a constraint, or have no objective definition of done?

**Typical accountable owners:** Product owner, Business analyst, Architect, Engineering lead, QA lead

**Primary failure primitives:** `OMISSION`, `INCORRECT`, `INCONSISTENT`, `NONCOMPLIANT`

**Lifecycle phases:** `discover`, `plan`, `design`, `accept`

### REQ-01: Problem statement, outcomes, and scope

- **What can go wrong:** The request is ambiguous; adjacent features creep in; exclusions are unstated; success is measured by implementation output instead of user or business outcome.
- **Enterprise hardening intent:** State the problem, intended outcomes, in-scope and out-of-scope behavior, actors, system boundary, and success measures before coding.
- **Expected evidence:** Approved brief; scope statement; outcome metrics; out-of-scope list.

### REQ-02: Stakeholders, actors, and operating context

- **What can go wrong:** Important users, administrators, support staff, downstream systems, attackers, or accessibility needs are omitted.
- **Enterprise hardening intent:** Identify primary, secondary, privileged, machine, and adversarial actors plus their environments, capabilities, incentives, and constraints.
- **Expected evidence:** Actor catalog; personas; context scenarios; stakeholder sign-off.

### REQ-03: Functional requirements

- **What can go wrong:** Core workflows, state transitions, business rules, and permissions are incomplete, contradictory, or implemented from unstated assumptions.
- **Enterprise hardening intent:** Express observable behavior, preconditions, postconditions, invariants, permissions, and examples; resolve contradictions before implementation.
- **Expected evidence:** Numbered requirements; examples; decision table; state model.

### REQ-04: Quality attributes and service objectives

- **What can go wrong:** Reliability, latency, security, privacy, accessibility, maintainability, cost, and recovery are deferred until after launch.
- **Enterprise hardening intent:** Set measurable quality-attribute scenarios and targets, including SLOs, performance budgets, RTO, RPO, support windows, and data-protection expectations.
- **Expected evidence:** NFR catalog; SLO document; performance and recovery budgets.

### REQ-05: Acceptance criteria and testability

- **What can go wrong:** Terms such as fast, secure, scalable, robust, and user-friendly have no threshold; tests prove implementation details rather than outcomes.
- **Enterprise hardening intent:** Write binary or bounded acceptance conditions with inputs, expected outputs, tolerances, environments, and evidence; reject unverifiable adjectives.
- **Expected evidence:** Acceptance tests; examples; threshold table; evidence specification.

### REQ-06: Assumptions, constraints, and unknowns

- **What can go wrong:** The code relies on undocumented traffic, data, clock, ordering, identity, platform, or dependency assumptions that fail in production.
- **Enterprise hardening intent:** Record assumptions and constraints; validate high-risk assumptions with spikes or measurements; convert unresolved unknowns into explicit risks and stop conditions.
- **Expected evidence:** Assumption log; constraint list; spike results; unresolved-unknown register.

### REQ-07: Misuse, abuse, and negative requirements

- **What can go wrong:** The system only specifies what allowed users should do, not what must be prevented when users, bots, insiders, or dependencies behave badly.
- **Enterprise hardening intent:** Define abuse cases, prohibited actions, resource limits, fraud controls, fail-closed conditions, and expected behavior for malformed, repeated, delayed, and adversarial inputs.
- **Expected evidence:** Abuse-case catalog; negative requirements; rate and quota policy.

### REQ-08: Legal, privacy, residency, and records requirements

- **What can go wrong:** Data is collected, moved, retained, logged, or deleted without knowing applicable obligations or contractual boundaries.
- **Enterprise hardening intent:** Identify applicable jurisdictions, contracts, records duties, consent needs, residency, accessibility, and retention obligations; map them to technical controls and evidence.
- **Expected evidence:** Obligation register; data-flow and residency map; counsel or compliance review.

### REQ-09: Compatibility, versioning, and migration requirements

- **What can go wrong:** A change breaks clients, schemas, events, stored data, automation, or rollback because compatibility expectations were never stated.
- **Enterprise hardening intent:** Define supported versions, compatibility window, deprecation policy, migration path, rollback limits, and mixed-version behavior.
- **Expected evidence:** Compatibility matrix; version policy; migration and deprecation plan.

### REQ-10: Failure, recovery, and support semantics

- **What can go wrong:** No one knows what partial success, retry, cancellation, timeout, degraded operation, customer notification, or support escalation should mean.
- **Enterprise hardening intent:** Specify failure modes, idempotency, retry ownership, user-visible errors, compensation, safe degradation, operational handoff, and recovery objectives.
- **Expected evidence:** Failure-mode table; support model; recovery acceptance tests; escalation rules.


## 4.3 ARC: Architecture, Boundaries and Evolvability

**Failure question:** Could the system's structure amplify defects, blur trust boundaries, lock in unsafe choices, or make change and recovery disproportionately risky?

**Typical accountable owners:** Architect, Engineering lead, Security architect, Platform/SRE, Data architect

**Primary failure primitives:** `INCOMPATIBLE`, `UNAVAILABLE`, `UNSAFE_CHANGE`, `UNMAINTAINABLE`

**Lifecycle phases:** `design`, `build`, `evolve`

### ARC-01: System context and boundaries

- **What can go wrong:** The repository is designed in isolation; upstream, downstream, human, network, and administrative dependencies are invisible.
- **Enterprise hardening intent:** Maintain a context diagram that identifies actors, systems, trust zones, data stores, external services, administrative paths, and ownership boundaries.
- **Expected evidence:** Context diagram; service catalog links; boundary assumptions.

### ARC-02: Trust boundaries and threat modeling

- **What can go wrong:** Sensitive operations cross boundaries without re-authentication, validation, authorization, encryption, or audit.
- **Enterprise hardening intent:** Mark every trust transition; define threats and controls for identity, data, commands, events, files, and administrative actions crossing it.
- **Expected evidence:** Data-flow diagram; threat model; trust-boundary review.

### ARC-03: Component decomposition and coupling

- **What can go wrong:** A monolith, microservice split, shared library, or plugin architecture creates hidden coupling, duplicated policy, or distributed failure without clear benefit.
- **Enterprise hardening intent:** Choose boundaries by ownership, change cadence, data consistency, security, and failure isolation; minimize synchronous coupling and shared mutable state.
- **Expected evidence:** Architecture decision record; dependency graph; coupling metrics.

### ARC-04: State ownership and data flow

- **What can go wrong:** Multiple components believe they own the same state; writes race; derived copies drift; sensitive data spreads without purpose.
- **Enterprise hardening intent:** Assign a system of record, write authority, replication semantics, lineage, retention, and reconciliation for each stateful entity and data flow.
- **Expected evidence:** State ownership matrix; lineage map; consistency contract.

### ARC-05: Dependency map and failure propagation

- **What can go wrong:** Transitive technical and organizational dependencies create unmodeled single points of failure and cascading outages.
- **Enterprise hardening intent:** Map runtime, build, identity, network, data, and operational dependencies; model timeout, degradation, substitution, and recovery behavior.
- **Expected evidence:** Dependency graph; failure-mode analysis; fallback decision.

### ARC-06: Tenancy and isolation

- **What can go wrong:** Tenant identifiers are trusted from clients; data, cache keys, queues, logs, limits, or encryption contexts leak across tenants. Application-code-only tenant filtering: filtering by `tenantId` in WHERE clauses without a storage-layer backstop: fails silently when a query omits the tenant filter or filters by user identity but not tenant identity (BOLA: Broken Object-Level Authorization). A single missing WHERE clause leaks all tenant data.
- **Enterprise hardening intent:** Make tenancy an architectural primitive; enforce isolation at authorization, storage, cache, compute, observability, and administrative layers. For systems at C2 and above, application-code-only tenant filtering is insufficient as the sole isolation mechanism. A storage-layer backstop: database row security policies, separate schemas, or equivalent: MUST be implemented independently of application code. The combination MUST be tested with authenticated cross-tenant access attempts that bypass application-layer filtering. BOLA patterns where an object is filtered by user identity but not tenant identity MUST be explicitly tested.
- **Expected evidence:** Tenant threat model; cross-tenant access tests with authenticated Tenant A attempting to read Tenant B data; storage-layer isolation evidence (RLS policy, schema separation); BOLA test suite; tenant-aware schema and policies.

### ARC-07: Availability and failure domains

- **What can go wrong:** Redundant components share the same region, control plane, credential, network, deployment, or operator failure domain.
- **Enterprise hardening intent:** Identify correlated failure domains and isolate data plane, control plane, observability, recovery access, and deployment mechanisms where justified.
- **Expected evidence:** Failure-domain map; redundancy analysis; out-of-band access design.

### ARC-08: Scalability and capacity architecture

- **What can go wrong:** The architecture cannot partition load, bound fan-out, shed work, or avoid hot keys; scaling multiplies cost or contention.
- **Enterprise hardening intent:** Define expected load shape, partition keys, concurrency model, capacity limits, headroom, backpressure, and degradation before scale is needed.
- **Expected evidence:** Capacity model; partition analysis; load-shape scenarios.

### ARC-11: Stateless design and horizontal parallelizability

- **What can go wrong:** Handlers hold in-process session state, local file locks, or per-node caches that prevent running multiple instances; scaling requires architectural surgery rather than adding nodes. AI-generated code frequently introduces implicit statefulness (module-level variables, singleton caches, open file handles) that is invisible until a second instance is started.
- **Enterprise hardening intent:** Services SHOULD be designed stateless by default, with all shared state externalized to a database, cache, or queue. Horizontal scaling SHOULD require no code change, only configuration. Stateful exceptions (websocket servers, scheduled workers, leader-election roles) MUST be explicitly identified and designed for. Operations SHOULD be idempotent so parallel execution and retries produce the same result.
- **Expected evidence:** Statelessness review; multi-instance smoke test; idempotency test for mutating operations; explicit inventory of stateful exceptions.

### ARC-09: Evolvability and replaceability

- **What can go wrong:** Interfaces, schemas, vendors, and internal modules cannot change independently; every migration becomes a big-bang event.
- **Enterprise hardening intent:** Use explicit contracts, adapters, versioning, reversible migrations, modular boundaries, and deprecation windows; document intentional lock-in.
- **Expected evidence:** ADRs; compatibility strategy; replacement or exit plan.

### ARC-10: Technology selection and proof

- **What can go wrong:** Tools are chosen because the model knows them, they are fashionable, or a demo succeeded; operational maturity and team fit are ignored.
- **Enterprise hardening intent:** Evaluate security, support, ecosystem health, operability, cost, performance, licensing, portability, and team capability; run targeted proof for high-risk assumptions.
- **Expected evidence:** Decision matrix; proof results; total-cost and exit analysis.


## 4.4 DOM: Domain Logic and Functional Correctness

**Failure question:** Could the code execute exactly as written while producing a materially wrong business result?

**Typical accountable owners:** Domain owner, Product owner, Engineering lead, QA, Data/Finance owner as applicable

**Primary failure primitives:** `INCORRECT`, `INCONSISTENT`, `ABUSE`, `NONCOMPLIANT`

**Lifecycle phases:** `design`, `build`, `verify`, `operate`

### DOM-01: Business-rule correctness

- **What can go wrong:** Rules are mistranslated, incomplete, duplicated across services, or based on obsolete policy; examples cover only the happy path.
- **Enterprise hardening intent:** Represent material rules explicitly with source, version, owner, decision tables, executable examples, and change tests.
- **Expected evidence:** Rule catalog; source citation; golden examples; domain-owner approval.

### DOM-02: Invariants and constraints

- **What can go wrong:** Impossible states are representable; validation occurs only in the UI; downstream code assumes invariants that no layer enforces.
- **Enterprise hardening intent:** Define invariants close to the domain model and enforce them at every untrusted boundary plus durable storage where feasible.
- **Expected evidence:** Invariant list; model and database constraints; negative tests.

### DOM-03: State machines and workflows

- **What can go wrong:** Illegal transitions, repeated commands, skipped approvals, orphaned work, and partially completed workflows are possible.
- **Enterprise hardening intent:** Model states and transitions explicitly; guard transitions, persist intent, handle compensation, and test interruption and replay.
- **Expected evidence:** State diagram; transition table; workflow tests; compensation plan.

### DOM-04: Money, numeric precision, units, and rounding

- **What can go wrong:** Binary floating point, implicit currency conversion, unit mismatch, overflow, rounding order, or tolerance errors create silent loss.
- **Enterprise hardening intent:** Use explicit money and unit types, bounded numeric ranges, defined rounding modes, decimal arithmetic where required, and reconciliation.
- **Expected evidence:** Numeric policy; unit types; edge tests; reconciliation report.

### DOM-05: Time, calendars, and time zones

- **What can go wrong:** Local time, daylight-saving changes, leap behavior, calendar boundaries, clock skew, and event-time versus processing-time semantics are confused.
- **Enterprise hardening intent:** Store instants and zones explicitly; define governing clocks and calendars; test transitions, late events, future dates, and historical rule changes.
- **Expected evidence:** Temporal contract; timezone policy; boundary and replay tests.

### DOM-06: Identity, deduplication, and idempotency

- **What can go wrong:** The same real-world entity or command is processed twice, merged incorrectly, or assigned unstable identifiers.
- **Enterprise hardening intent:** Define identity keys, collision handling, idempotency scope, deduplication window, replay behavior, and merge or split governance.
- **Expected evidence:** Identity contract; idempotency tests; duplicate-resolution audit.

### DOM-07: Concurrency, ordering, and races

- **What can go wrong:** Check-then-act logic, lost updates, double spending, stale reads, out-of-order events, and concurrent approvals violate business rules.
- **Enterprise hardening intent:** Choose concurrency control and ordering semantics explicitly; use atomic operations, version checks, locks, queues, or commutative designs as appropriate.
- **Expected evidence:** Concurrency model; race tests; transaction and ordering evidence.

### DOM-08: Reconciliation and auditability

- **What can go wrong:** Inputs, outputs, ledgers, inventory, balances, or side effects cannot be independently reconciled after partial failure or dispute.
- **Enterprise hardening intent:** Create control totals, immutable identifiers, audit trails, compensating entries, and source-to-target reconciliation with tolerances and owners.
- **Expected evidence:** Reconciliation report; ledger or audit events; exception workflow.

### DOM-09: Boundary, empty, partial, and default cases

- **What can go wrong:** Zero, one, maximum, missing, duplicate, partial, stale, and default values take unintended branches or silently create records.
- **Enterprise hardening intent:** Enumerate boundary partitions and define explicit behavior for absence, partial success, defaults, and unknown values; do not infer acceptance from truthiness.
- **Expected evidence:** Boundary matrix; decision tests; default-value review.

### DOM-10: Decision provenance, explanation, and override

- **What can go wrong:** Material automated decisions cannot be explained, contested, reproduced, or safely overridden; manual overrides bypass controls.
- **Enterprise hardening intent:** Record inputs, rule or model version, rationale, confidence, approver, and override history; design controlled review and appeal paths.
- **Expected evidence:** Decision record; explanation test; override audit; reproducibility check.


## 4.5 COD: Code Quality, Maintainability and Resource Safety

**Failure question:** Could the implementation be correct today but fragile, opaque, leak-prone, nondeterministic, or prohibitively expensive to change?

**Typical accountable owners:** Engineering lead, Code owner, Reviewer, Platform engineer

**Primary failure primitives:** `INCORRECT`, `EXHAUSTION`, `UNAVAILABLE`, `UNMAINTAINABLE`

**Lifecycle phases:** `build`, `review`, `maintain`

### COD-01: Readability, naming, and local consistency

- **What can go wrong:** Code is clever, misleadingly named, inconsistent with repository idioms, or understandable only with the generating conversation.
- **Enterprise hardening intent:** Prefer intention-revealing names, repository conventions, small coherent units, and comments that explain why rather than restate syntax.
- **Expected evidence:** Lint and format results; review record; complexity and naming checks.

### COD-02: Simplicity and complexity control

- **What can go wrong:** Nested branches, implicit control flow, magic behavior, and premature abstraction hide defects and frustrate review.
- **Enterprise hardening intent:** Choose the simplest design meeting stated requirements; bound function and module complexity; split responsibilities before adding exceptions.
- **Expected evidence:** Complexity report; design review; small-change history.

### COD-03: Modularity, cohesion, and coupling

- **What can go wrong:** Modules mix policy, I/O, state, presentation, and infrastructure; changes ripple through unrelated code.
- **Enterprise hardening intent:** Separate pure domain logic from side effects; use narrow interfaces, dependency inversion where useful, and explicit ownership.
- **Expected evidence:** Module diagram; dependency rules; architecture tests.

### COD-04: Duplication, dead code, and deprecation

- **What can go wrong:** Copied rules drift; dormant code is accidentally reactivated; feature flags and compatibility paths never disappear.
- **Enterprise hardening intent:** Centralize material policy, delete unreachable code, time-box deprecations, and track flag or compatibility cleanup as release work.
- **Expected evidence:** Duplicate and dead-code scan; deprecation register; deletion tests.

### COD-05: Types, nullability, and contracts

- **What can go wrong:** Dynamic shapes, unchecked casts, sentinel values, nullable fields, and broad dictionaries move failures to runtime.
- **Enterprise hardening intent:** Use the strongest practical types and schemas; model optionality and variants explicitly; validate at boundaries and reject impossible states.
- **Expected evidence:** Type-check output; schema validation; contract tests.

### COD-06: Exceptions and fail-safe behavior

- **What can go wrong:** Errors are swallowed, converted to success, retried indefinitely, exposed verbatim, or handled so broadly that corruption continues.
- **Enterprise hardening intent:** Catch only what can be handled; preserve cause and context; classify retryability; fail closed for security and integrity; surface actionable errors.
- **Expected evidence:** Negative tests; error taxonomy; log and response review.

### COD-07: Resource lifecycle and leak prevention

- **What can go wrong:** Files, sockets, transactions, locks, threads, temporary data, GPU memory, or handles leak on error and accumulate under load.
- **Enterprise hardening intent:** Use structured ownership and cleanup, bounded pools, cancellation, finally or context patterns, and leak or soak tests.
- **Expected evidence:** Resource-leak tests; pool metrics; static analysis; shutdown tests.

### COD-08: Concurrency, async, and thread safety

- **What can go wrong:** Shared state, blocking calls in async paths, unsafe callbacks, cancellation gaps, or unbounded task creation cause races and starvation.
- **Enterprise hardening intent:** Document concurrency model; avoid unsynchronized shared mutable state; bound work; propagate cancellation and context; test race-sensitive paths.
- **Expected evidence:** Concurrency tests; race detector; thread or event-loop profile.

### COD-09: Determinism, portability, and environment assumptions

- **What can go wrong:** Behavior depends on iteration order, locale, architecture, filesystem case, process working directory, random seed, clock, or undocumented environment variables.
- **Enterprise hardening intent:** Make dependencies explicit; control randomness and time in tests; normalize platform assumptions; test supported environments.
- **Expected evidence:** Reproducible test run; support matrix; environment contract.

### COD-10: Technical debt and maintainability management

- **What can go wrong:** Generated patches accumulate special cases, TODOs, suppressions, and dependency drift faster than teams can understand them.
- **Enterprise hardening intent:** Require debt records with owner and expiry; prohibit unexplained suppressions; budget refactoring; measure change hotspots and escaped defects.
- **Expected evidence:** Debt register; suppression audit; maintainability trend; refactoring plan.

### COD-11: Prefer maintained libraries over custom implementations

- **What can go wrong:** AI-generated code builds custom implementations of well-solved problems: modal dialogs, date pickers, dropdown menus, form validation, authentication flows, markdown renderers, CSV parsers, encryption wrappers. Custom implementations miss edge cases that battle-tested libraries handle (focus traps, keyboard navigation, locale-aware date parsing, timing-safe comparison), accumulate bugs the original authors already fixed, and create maintenance burden with no benefit.
- **Enterprise hardening intent:** For any common pattern with a well-maintained open-source solution, SHOULD use the library rather than a custom implementation. UI primitives (modals, popovers, comboboxes, date pickers, drag-and-drop) SHOULD use an accessible component library (Radix UI, Headless UI, shadcn/ui, or equivalent) rather than a hand-rolled implementation. Cryptographic operations, authentication flows, and data parsing MUST use established libraries. Custom implementations of common patterns require documented justification.
- **Expected evidence:** Dependency audit showing use of maintained libraries for solved problems; documented justification for any custom implementation of a pattern with existing library solutions.


## 4.6 API: APIs, Contracts and Integrations

**Failure question:** Could an interface expose, corrupt, over-consume, or break systems because its contract, trust, compatibility, and failure semantics are weak?

**Typical accountable owners:** API owner, Engineering lead, Security, Integration owner, SRE

**Primary failure primitives:** `INCOMPATIBLE`, `UNAUTHORIZED`, `EXHAUSTION`, `UNAVAILABLE`, `ABUSE`

**Lifecycle phases:** `design`, `build`, `verify`, `operate`, `retire`

### API-01: Inventory, ownership, and discoverability

- **What can go wrong:** Unknown, shadow, test, deprecated, or duplicate endpoints remain reachable; no owner monitors consumers or exposure.
- **Enterprise hardening intent:** Maintain a versioned inventory of public, partner, internal, admin, event, and callback interfaces with owner, consumers, data class, and lifecycle.
- **Expected evidence:** API catalog; route inventory; owner and exposure scan.

### API-02: Schemas, contracts, and validation

- **What can go wrong:** Clients and servers disagree on types, required fields, limits, enums, defaults, or unknown fields; validation occurs after side effects.
- **Enterprise hardening intent:** Publish machine-readable contracts; validate requests and responses before use; set explicit size, depth, count, and format bounds.
- **Expected evidence:** OpenAPI or equivalent schema; contract tests; malformed-input tests.

### API-03: Authentication and authorization

- **What can go wrong:** Possession of an identifier grants access; function, field, object, tenant, and administrative permissions are not enforced server-side.
- **Enterprise hardening intent:** Authenticate each trust boundary and authorize every object, function, property, and action using server-derived identity and tenant context.
- **Expected evidence:** Authorization matrix; BOLA and BFLA tests; negative tenant tests.

### API-04: Versioning and compatibility

- **What can go wrong:** A field rename, enum addition, error change, event change, or stricter validation silently breaks consumers.
- **Enterprise hardening intent:** Define compatibility rules, additive-change policy, tolerant-reader limits, version negotiation, deprecation telemetry, and sunset process.
- **Expected evidence:** Compatibility suite; consumer contract tests; deprecation dashboard.

### API-05: Idempotency, retries, and duplicate effects

- **What can go wrong:** Client, gateway, queue, or network retries create duplicate payments, jobs, emails, records, or mutations.
- **Enterprise hardening intent:** Define idempotency keys and scope, deduplication persistence, replay response, expiration, and conflict behavior for every retryable mutation.
- **Expected evidence:** Idempotency contract; duplicate-submission tests; dedup metrics.

### API-06: Rate limits, quotas, and resource bounds

- **What can go wrong:** One client or request consumes unbounded CPU, memory, storage, fan-out, records, or downstream calls; limits are global rather than tenant-aware.
- **Enterprise hardening intent:** Apply authenticated and anonymous quotas, concurrency limits, payload bounds, pagination, cost accounting, and graceful throttling at multiple layers.
- **Expected evidence:** Limit policy; load and abuse tests; quota dashboards.

### API-07: Errors, status, and correlation

- **What can go wrong:** The API returns 200 on failure, leaks internals, changes error shapes, obscures retryability, or cannot correlate a user report with backend work.
- **Enterprise hardening intent:** Use stable error codes and schemas, correct protocol status, safe messages, correlation IDs, retry guidance, and partial-result semantics.
- **Expected evidence:** Error catalog; negative contract tests; log-correlation evidence.

### API-08: Pagination, filtering, sorting, and bulk operations

- **What can go wrong:** Unbounded listings, unstable ordering, injection-prone filters, offset drift, or partial bulk failures cause data loss and denial of service.
- **Enterprise hardening intent:** Require bounded pagination, deterministic sort, validated query grammar, cursor integrity, per-item results, and resumable bulk behavior.
- **Expected evidence:** Query grammar; pagination tests; bulk replay tests.

### API-09: Events, queues, webhooks, and asynchronous contracts

- **What can go wrong:** Messages are lost, duplicated, reordered, replayed, forged, or poison consumers; schema and delivery semantics are implicit. Webhook receivers that skip signature verification accept forged events from any caller. Webhook receivers that return 5xx on error trigger indefinite provider retry, creating a denial-of-correctness storm. Payment and state-mutation webhooks that are not idempotent double-process on normal at-least-once delivery.
- **Enterprise hardening intent:** Version event schemas; authenticate senders; define ordering, deduplication, retry, dead-letter, replay, retention, and consumer compatibility. Webhook receivers MUST verify the provider-signed HMAC or signature before processing any state change; verification failure MUST return 4xx, not 5xx, to prevent retry storms. Receivers MUST be idempotent: keyed on the provider event ID with a deduplication store: because most providers deliver at-least-once. Idempotency MUST be designed before the signature check is considered complete.
- **Expected evidence:** Event catalog; HMAC signature verification test; duplicate-event test confirming 200 and no re-execution; replay and poison-message tests.

### API-10: External API consumption and egress

- **What can go wrong:** Third-party responses are trusted as safe; calls lack deadlines, bounds, schema checks, certificate validation, or egress restrictions; URLs enable SSRF.
- **Enterprise hardening intent:** Treat external systems as untrusted; validate responses, constrain destinations, set timeouts and budgets, isolate credentials, cache safely, and degrade deliberately.
- **Expected evidence:** Egress policy; timeout and failure tests; response validation; dependency SLO.

### API-11: Real-time and event-driven client contracts

- **What can go wrong:** WebSocket or SSE connections drop silently; the client shows stale data without indicating loss of connection; reconnection floods the server; presence indicators lie; optimistic UI updates are never rolled back on failure; collaborative features produce conflicting state with no resolution strategy.
- **Enterprise hardening intent:** Define reconnection behavior with exponential backoff and jitter; resync client state on reconnect rather than assuming continuity; show connection status to the user when the real-time channel is degraded; roll back optimistic updates on confirmed failure; define conflict resolution strategy for collaborative features; test behavior under disconnect, reconnect, and simultaneous edit scenarios.
- **Expected evidence:** Reconnection and resync test; stale-state indicator review; optimistic-update rollback test; conflict-resolution design.


## 4.7 DAT: Data, Storage, Caching and Migrations

**Failure question:** Could persistent state become inconsistent, unrecoverable, exposed, or incompatible even when application tests pass?

**Typical accountable owners:** Data owner, Database engineer, Engineering lead, Security/Privacy, SRE

**Primary failure primitives:** `INCORRECT`, `INCONSISTENT`, `DISCLOSURE`, `IRRECOVERABLE`, `UNAVAILABLE`

**Lifecycle phases:** `design`, `build`, `migrate`, `operate`, `recover`, `retire`

### DAT-01: Schema, integrity, and constraints

- **What can go wrong:** Application-only validation is bypassed; nulls, duplicates, invalid references, oversized values, and incompatible schema changes enter durable state.
- **Enterprise hardening intent:** Enforce critical invariants with schemas and storage constraints; version contracts; validate evolution and quarantine incompatible writes.
- **Expected evidence:** DDL or schema; constraint tests; schema-compatibility report.

### DAT-02: Transactions, isolation, and atomicity

- **What can go wrong:** Partial writes, lost updates, write skew, dirty reads, and cross-system side effects leave impossible state.
- **Enterprise hardening intent:** Choose transaction and isolation semantics per invariant; use atomic writes, optimistic versions, outbox or saga patterns, and reconciliation for cross-system work.
- **Expected evidence:** Transaction design; isolation tests; compensation and reconciliation evidence.

### DAT-03: Migrations, backfills, and expand-contract changes

- **What can go wrong:** A migration locks production, truncates data, assumes homogeneous versions, cannot resume, or makes rollback impossible.
- **Enterprise hardening intent:** Use backward-compatible expand-migrate-contract steps; estimate locks and volume; checkpoint backfills; verify counts; rehearse forward and reverse paths.
- **Expected evidence:** Migration plan; dry run; lock analysis; backup; verification and rollback record.

### DAT-04: Backup, restore, and corruption detection

- **What can go wrong:** Backups are missing, unreadable, co-located, unencrypted, silently corrupt, or impossible to restore within objectives.
- **Enterprise hardening intent:** Define immutable, separated backups; verify integrity; test full and selective restores; detect corruption; document recovery order and credentials.
- **Expected evidence:** Backup reports; restore drill; checksum or integrity evidence; RTO/RPO result.

### DAT-05: Caches, replicas, search indexes, and derived stores

- **What can go wrong:** Stale or poisoned cache entries, replication lag, and index drift produce incorrect authorization or business results.
- **Enterprise hardening intent:** Define source of truth, freshness and invalidation semantics, tenant-safe keys, lag limits, rebuild process, and reconciliation for every derived store.
- **Expected evidence:** Cache contract; lag dashboard; rebuild and reconciliation test.

### DAT-06: Partitioning, sharding, and hotspots

- **What can go wrong:** Poor keys create hot partitions, cross-shard transactions, uneven cost, data skew, or inability to rebalance.
- **Enterprise hardening intent:** Model cardinality and access patterns; select stable keys; bound fan-out; test skew; plan resharding and tenant movement.
- **Expected evidence:** Partition analysis; skew load test; rebalancing runbook.

### DAT-07: Query safety and efficiency

- **What can go wrong:** Dynamic queries enable injection; missing indexes, N+1 access, full scans, or unbounded result sets exhaust the database.
- **Enterprise hardening intent:** Parameterize queries; use query budgets, plans, indexes, timeouts, pagination, read-only roles, and regression monitoring.
- **Expected evidence:** SAST or review; query-plan checks; slow-query dashboard; load test.

### DAT-08: Retention, archival, legal hold, and deletion

- **What can go wrong:** Data is retained indefinitely, deleted too early, left in replicas and backups, or impossible to place on hold.
- **Enterprise hardening intent:** Implement policy-driven lifecycle, defensible deletion, legal hold, archive integrity, tombstone propagation, and evidence of completion.
- **Expected evidence:** Retention schedule; deletion workflow; hold tests; deletion evidence.

### DAT-09: Lineage, provenance, and reconciliation

- **What can go wrong:** The origin, transformation, owner, and version of data cannot be established; downstream corrections are incomplete.
- **Enterprise hardening intent:** Capture lineage and transformation versions; use stable identifiers and control totals; reconcile sources, stores, events, and outputs.
- **Expected evidence:** Lineage graph; reconciliation report; correction audit.

### DAT-10: Test data and production-data use

- **What can go wrong:** Production PII or secrets leak into development, tests, fixtures, screenshots, or model prompts; test data lacks edge cases.
- **Enterprise hardening intent:** Use synthetic or irreversibly masked data by default; control extracts; expire copies; validate anonymization; build representative boundary datasets.
- **Expected evidence:** Test-data policy; masking validation; access log; fixture coverage.


## 4.8 IAM: Identity, Authentication, Authorization and Sessions

**Failure question:** Could an attacker, departed user, service, or tenant act as someone else or retain more access than intended?

**Typical accountable owners:** Identity owner, Security, Application owner, Platform/SRE, Compliance

**Primary failure primitives:** `UNAUTHORIZED`, `DISCLOSURE`, `ABUSE`, `TAMPERING`

**Lifecycle phases:** `design`, `build`, `operate`, `recover`

### IAM-01: Identity lifecycle

- **What can go wrong:** Accounts are orphaned, duplicated, shared, or retained after role or employment changes; service identities lack owners.
- **Enterprise hardening intent:** Automate joiner, mover, leaver flows; require unique identities, owners, review cadence, expiration, and prompt deprovisioning.
- **Expected evidence:** Identity inventory; lifecycle tests; access review; orphan report.

### IAM-02: Authentication and phishing resistance

- **What can go wrong:** Weak, custom, or downgradeable authentication permits credential stuffing, replay, interception, or bypass.
- **Enterprise hardening intent:** Use established identity providers and protocols; require risk-appropriate MFA, phishing-resistant factors for privileged access, throttling, and secure fallback.
- **Expected evidence:** Authentication architecture; MFA policy; attack tests; provider configuration.

### IAM-03: Credentials, passwords, and keys

- **What can go wrong:** Secrets are hard-coded, long-lived, reused, weakly stored, exposed in logs, or transmitted insecurely. Third-party OAuth access tokens and refresh tokens obtained on behalf of users are stored as plaintext VARCHAR columns, exposing all delegated user access in a single database breach. Platforms that pass stored OAuth tokens to user-controlled URLs or integrations allow trivial credential exfiltration.
- **Enterprise hardening intent:** Use approved secret stores, strong password hashing, short-lived credentials, rotation, scope, revocation, and leak detection. Third-party OAuth access tokens and refresh tokens stored on behalf of users MUST be encrypted at field level before persistence, using a key managed independently of the database. Plaintext tokens MUST NOT appear in logs, error messages, or any durable store. Systems that pass stored tokens to user-controlled URLs MUST explicitly validate that the destination is an authorized recipient of that credential before transmitting.
- **Expected evidence:** Secret scan; credential policy; rotation evidence; hash configuration; field-level encryption verification for delegated OAuth tokens; token-destination authorization test.

### IAM-04: Authorization model and enforcement

- **What can go wrong:** Role names substitute for policy; checks are missing at object, field, function, tenant, or state-transition level.
- **Enterprise hardening intent:** Define centralized policy and deny-by-default enforcement; authorize server-side using trusted context at every sensitive operation.
- **Expected evidence:** Permission matrix; policy tests; access-control review.

### IAM-05: Least privilege and workload identity

- **What can go wrong:** Applications run as administrator; CI, jobs, containers, and cloud services share broad static credentials. Platforms that request broad OAuth scopes from users to cover multiple optional features expose more user data and access than any single capability requires. A single bundled consent scope means a compromised token grants access to every delegated capability at once.
- **Enterprise hardening intent:** Use distinct workload identities, minimal scopes, just-in-time grants, audience-bound tokens, and environment separation. When a platform requests OAuth delegation from end users to act on their behalf, requested scopes SHOULD be the minimum necessary for each specific integration or agent capability. Broad scopes SHOULD NOT be bundled into a single consent to cover multiple optional features. The platform SHOULD disclose to users, at the point of authorization, which specific scopes each enabled capability requires, and SHOULD allow users to revoke individual capability access without revoking the entire delegation.
- **Expected evidence:** IAM policy analysis; unused-permission report; workload identity configuration; OAuth scope inventory showing per-capability minimization; user-facing scope disclosure review.

### IAM-06: Sessions, tokens, and cookies

- **What can go wrong:** Tokens are overly long-lived, stored unsafely, accepted for the wrong audience, not rotated, or exposed through URLs and clients.
- **Enterprise hardening intent:** Set secure cookie attributes; validate issuer, audience, signature, time, and nonce; rotate sessions on privilege change; bind and revoke where appropriate.
- **Expected evidence:** Session policy; token validation tests; cookie scan; logout and revocation test.

### IAM-07: Tenant isolation, delegation, and impersonation

- **What can go wrong:** Administrative support, delegated access, or tenant switching bypasses normal authorization and audit.
- **Enterprise hardening intent:** Make tenant context immutable and server-derived; constrain delegation and impersonation by purpose, duration, approval, notification, and audit.
- **Expected evidence:** Tenant boundary tests; delegation record; impersonation audit.

### IAM-08: Account recovery and sensitive changes

- **What can go wrong:** Recovery is weaker than login; email, phone, MFA, payout, or ownership changes can be hijacked without reauthentication.
- **Enterprise hardening intent:** Apply strong identity proofing, step-up authentication, delay or notification for high-risk changes, and recovery abuse monitoring.
- **Expected evidence:** Recovery flow tests; step-up rules; notification evidence.

### IAM-09: Privileged access and break-glass

- **What can go wrong:** Admin paths are hidden, unaudited, always-on, or unavailable during an outage; break-glass credentials become routine.
- **Enterprise hardening intent:** Separate privileged interfaces and identities; require strong MFA, JIT access, session recording, dual control where needed, tested emergency access, and post-use review.
- **Expected evidence:** PAM logs; break-glass drill; privileged route inventory.

### IAM-10: Revocation, caching, and continuous authorization

- **What can go wrong:** Revoked access remains effective in caches, tokens, sessions, replicas, or long-running jobs.
- **Enterprise hardening intent:** Define revocation latency; propagate policy changes; use short lifetimes and introspection for high-risk actions; reauthorize long workflows.
- **Expected evidence:** Revocation SLO; propagation tests; stale-access monitoring.

### IAM-11: Session continuity and re-authentication UX

- **What can go wrong:** A session expires while the user is mid-task and in-progress work is silently lost; re-authentication redirects destroy form state, unsaved drafts, or multi-step wizard progress; "remember me" is not implemented, forcing re-login on every visit; cross-tab session state is inconsistent; a logout in one tab leaves other tabs in a phantom-authenticated state.
- **Enterprise hardening intent:** Preserve in-progress work across session expiry using local draft storage or server-side state; prompt for re-authentication without navigation when possible; define and test cross-tab session consistency; define "remember me" lifetime and security boundaries explicitly; ensure logout propagates across all active tabs and windows.
- **Expected evidence:** Session expiry UX test with in-progress form; cross-tab logout test; remember-me lifetime and scope documentation.


## 4.9 SEC: Application Security and Abuse Resistance

**Failure question:** Could malicious or malformed input, unsafe defaults, exceptional conditions, or business abuse compromise confidentiality, integrity, or availability?

**Typical accountable owners:** Security, Engineering lead, Application owner, Platform/SRE

**Primary failure primitives:** `UNAUTHORIZED`, `DISCLOSURE`, `TAMPERING`, `ABUSE`, `UNAVAILABLE`

**Lifecycle phases:** `design`, `build`, `verify`, `operate`, `respond`

### SEC-01: Injection and contextual encoding

- **What can go wrong:** SQL, command, template, expression, LDAP, path, header, or log injection occurs because strings are composed across trust boundaries.
- **Enterprise hardening intent:** Use parameterized APIs and allowlisted grammars; encode for the exact output context; avoid interpreters and shell execution where possible.
- **Expected evidence:** SAST; code review; injection tests; safe-API inventory.

### SEC-02: Browser and client-side request integrity

- **What can go wrong:** XSS, CSRF, clickjacking, unsafe DOM use, insecure storage, or malicious deep links execute actions or expose data.
- **Enterprise hardening intent:** Use safe rendering, CSP, anti-CSRF controls, frame restrictions, secure storage, origin validation, and server-side authorization.
- **Expected evidence:** Browser security scan; CSP tests; CSRF and DOM-XSS tests.

### SEC-03: Files, paths, parsers, and deserialization

- **What can go wrong:** Uploads contain active content or bombs; names escape directories; parsers and object deserialization instantiate unsafe types.
- **Enterprise hardening intent:** Validate type by content, size, depth, count, and structure; randomize storage names; isolate scanning and parsing; use safe serialization formats.
- **Expected evidence:** Upload and archive-bomb tests; path traversal tests; parser sandbox evidence.

### SEC-04: SSRF, egress, and protocol confusion

- **What can go wrong:** User-controlled URLs reach metadata, internal services, alternate schemes, redirects, or smuggled requests. User-configurable outbound webhook URLs: where an end user enters a URL the platform will POST to: are a primary SSRF attack surface, allowing access to cloud metadata endpoints, RFC-1918 internal services, or link-local addresses. DNS rebinding allows a URL that passes validation to resolve to a prohibited address at request time.
- **Enterprise hardening intent:** Use destination allowlists, DNS and redirect revalidation, network egress controls, hardened HTTP stacks, metadata protection, and protocol normalization. User-configurable outbound webhook URLs MUST be validated against: scheme allowlist (https only in production); explicit block of RFC-1918 and link-local ranges (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16, 169.254.0.0/16, ::1); DNS resolution at validation time AND at request time to prevent rebinding; redirect-following limits. Cloud metadata endpoints (169.254.169.254 and equivalent) MUST be explicitly blocked regardless of DNS resolution outcome.
- **Expected evidence:** SSRF tests targeting RFC-1918 ranges and metadata endpoints; user-configurable webhook URL validation test suite; DNS rebinding test; egress policy; proxy and request-smuggling tests.

### SEC-05: Cryptography, TLS, and key use

- **What can go wrong:** Custom crypto, obsolete algorithms, nonce reuse, unauthenticated encryption, weak randomness, disabled certificate checks, or mixed key purposes expose data.
- **Enterprise hardening intent:** Use current approved libraries and protocols; separate key purposes; manage rotation and destruction; validate certificates; obtain specialist review for novel use.
- **Expected evidence:** Crypto inventory; configuration scan; key lifecycle evidence; review.

### SEC-06: Secrets and sensitive-information exposure

- **What can go wrong:** Credentials, tokens, personal data, stack traces, source maps, backups, debug endpoints, or metadata leak through code, logs, errors, caches, or artifacts.
- **Enterprise hardening intent:** Minimize and classify sensitive values; redact by construction; disable production debug surfaces; scan repositories, images, logs, and artifacts.
- **Expected evidence:** Secret scans; error and log review; exposure test; artifact scan.

### SEC-07: Memory safety, unsafe code, and code execution

- **What can go wrong:** Buffer errors, out-of-bounds access, use-after-free, unsafe FFI, plugin execution, eval, or sandbox escape yields arbitrary code execution. Platforms that execute user-supplied code: scripts, formulas, agent tool implementations, notebook cells, or configuration-as-code: using `shell=True`, `eval()`, `exec()`, or equivalent constructs without process isolation give users arbitrary code execution on the platform's infrastructure, with access to host network, other users' data, and stored credentials.
- **Enterprise hardening intent:** Prefer memory-safe languages and APIs; isolate unavoidable unsafe code; validate lengths and interfaces; apply sanitizers, fuzzing, and sandboxing. User-supplied code MUST execute in an isolated process or container with: no access to the host network; no access to other users' data or credentials; enforced CPU and memory limits; a read-only or empty filesystem outside explicitly granted mounts; and a maximum execution time. `shell=True`, `eval()`, `exec()`, and equivalent constructs applied to user-controlled input are prohibited without these controls in place.
- **Expected evidence:** Unsafe-code inventory; sandbox design with network isolation, filesystem restriction, and resource limit evidence; sandbox escape test; sanitizer and fuzz results.

### SEC-08: Abuse, fraud, bots, and sensitive business flows

- **What can go wrong:** Valid API calls are combined at scale to scrape, enumerate, spam, manipulate prices, drain inventory, or bypass business intent.
- **Enterprise hardening intent:** Model economic and workflow abuse; add velocity, reputation, anomaly, step-up, confirmation, and manual-review controls without relying on secrecy.
- **Expected evidence:** Abuse cases; fraud rules; simulation; monitoring and response plan.

### SEC-09: Denial of service and resource exhaustion

- **What can go wrong:** Large, nested, adversarial, repeated, or computationally explosive inputs exhaust CPU, memory, threads, storage, queues, or downstream quotas.
- **Enterprise hardening intent:** Bound every input and fan-out; apply timeouts, quotas, complexity limits, streaming, load shedding, and algorithmic worst-case tests.
- **Expected evidence:** Abuse load test; limit configuration; ReDoS or complexity scan; saturation dashboard.

### SEC-10: Security configuration and secure defaults

- **What can go wrong:** Default credentials, permissive CORS, open storage, broad network exposure, unsafe headers, debug mode, or optional security controls reach production.
- **Enterprise hardening intent:** Ship deny-by-default configurations; validate startup policy; enforce environment baselines as code; fail deployment on insecure or unknown settings.
- **Expected evidence:** Configuration policy tests; baseline scan; startup validation.

### SEC-11: Exceptional conditions and fail-open behavior

- **What can go wrong:** Malformed state, dependency errors, validation failures, overflow, or unexpected exceptions bypass authorization, integrity checks, or cleanup.
- **Enterprise hardening intent:** Define fail-safe outcomes; handle exceptional paths explicitly; preserve atomicity and cleanup; test fault paths and prevent error-to-success conversion.
- **Expected evidence:** Fault-injection tests; negative authorization tests; exception coverage.

### SEC-12: Vulnerability management and disclosure

- **What can go wrong:** Known weaknesses remain untriaged; reachable vulnerabilities and exploited flaws are treated like routine backlog; researchers lack a safe reporting path.
- **Enterprise hardening intent:** Maintain inventory, severity and reachability triage, patch SLAs, CISA KEV escalation, compensating controls, coordinated disclosure, and recurrence analysis.
- **Expected evidence:** Vulnerability register; remediation SLA; VEX where used; SECURITY.md; recurrence review.

### SEC-13: User-generated content safety

- **What can go wrong:** User-submitted text, HTML, markdown, images, or files are rendered in other users' browsers without sanitization, producing stored XSS; uploaded files bypass content-type checks and are served with permissive MIME types; malware in uploaded documents reaches other users' devices; link unfurling fetches attacker-controlled URLs from the server (SSRF); AI-generated vibe-coded apps commonly render user content as raw innerHTML or dangerouslySetInnerHTML without review.
- **Enterprise hardening intent:** Sanitize all user-generated content before rendering using an allowlist-based sanitizer (DOMPurify or equivalent); never use innerHTML or dangerouslySetInnerHTML with unsanitized user input; validate file content-type at read time, not only at upload time; serve user-uploaded files from an isolated origin or storage bucket, not the application domain; scan uploaded files for malware before delivery; treat link unfurling as a user-controlled SSRF vector and apply the same controls as outbound webhooks.
- **Expected evidence:** XSS test with stored user input; file content-type validation test; upload domain isolation review; SSRF test for link unfurling.


## 4.10 PRV: Privacy and Data Protection

**Failure question:** Could the system use, infer, expose, retain, or share personal data beyond justified expectations and obligations?

**Typical accountable owners:** Privacy, Data owner, Security, Product owner, Legal/Compliance

**Primary failure primitives:** `DISCLOSURE`, `NONCOMPLIANT`, `TAMPERING`, `UNOBSERVABLE`

**Lifecycle phases:** `discover`, `design`, `build`, `operate`, `retire`

### PRV-01: Data inventory and classification

- **What can go wrong:** Personal and sensitive data appears in fields, free text, events, logs, backups, embeddings, or derived features without being known.
- **Enterprise hardening intent:** Maintain field- and flow-level inventory with sensitivity, subject, purpose, owner, location, and downstream copies; scan for unclassified data.
- **Expected evidence:** Data inventory; classification schema; flow map; discovery scan.

### PRV-02: Purpose, lawful basis, and consent

- **What can go wrong:** Data is reused for a new purpose, bundled consent, or processing continues after withdrawal without a documented basis.
- **Enterprise hardening intent:** Bind processing to explicit purpose and authority; record consent where applicable; enforce purpose and withdrawal in downstream systems.
- **Expected evidence:** Purpose register; consent record; enforcement tests.

### PRV-03: Minimization and privacy by default

- **What can go wrong:** The system collects full records, precise locations, identifiers, or long histories because they might be useful later.
- **Enterprise hardening intent:** Collect and expose the minimum fields, granularity, population, and duration required; make the least revealing option the default.
- **Expected evidence:** Field-necessity review; minimized schema; default-setting tests.

### PRV-04: Access, transparency, correction, and portability

- **What can go wrong:** Individuals cannot discover, access, correct, export, or understand relevant data and decisions; responses omit derived or replicated copies.
- **Enterprise hardening intent:** Design identity verification, search, export, correction, propagation, and transparent notices into the data architecture.
- **Expected evidence:** Rights-request workflow; completeness test; notice and response records.

### PRV-05: Retention, deletion, and legal hold

- **What can go wrong:** Personal data persists in caches, logs, replicas, backups, analytics, and third parties after deletion or beyond policy.
- **Enterprise hardening intent:** Encode retention and hold rules; propagate deletion or tombstones; bound backup aging; verify downstream and processor completion.
- **Expected evidence:** Retention policy; deletion lineage; completion evidence; backup policy.

### PRV-06: De-identification and re-identification risk

- **What can go wrong:** Hashing or removing names is treated as anonymization despite linkage, rare attributes, or external data enabling re-identification.
- **Enterprise hardening intent:** Assess singling-out and linkage risk; use aggregation, tokenization, differential privacy, or access controls appropriate to use; retest as data changes.
- **Expected evidence:** Re-identification assessment; transformation specification; utility and privacy tests.

### PRV-07: Logs, telemetry, analytics, and support data

- **What can go wrong:** URLs, headers, payloads, prompts, screenshots, traces, and support exports capture secrets or personal data by default.
- **Enterprise hardening intent:** Use allowlisted telemetry fields, redaction before emission, sampling, access control, retention limits, and safe support tooling.
- **Expected evidence:** Telemetry schema; redaction tests; access and retention audit.

### PRV-08: Sharing, processors, and cross-border movement

- **What can go wrong:** Data flows to SDKs, SaaS, subprocessors, regions, or model providers without contract, minimization, residency, or deletion controls.
- **Enterprise hardening intent:** Inventory recipients and regions; enforce egress and contractual controls; minimize payloads; monitor subprocessor and destination changes.
- **Expected evidence:** Processor register; data-transfer map; egress controls; contract evidence.

### PRV-09: Profiling, sensitive inference, and automated decisions

- **What can go wrong:** The system infers health, identity, behavior, risk, or protected traits not explicitly provided; users cannot contest consequential profiling.
- **Enterprise hardening intent:** Limit inference by purpose; test proxy and disparate effects; provide explanation, review, and opt-out or appeal where required.
- **Expected evidence:** Inference inventory; impact assessment; decision review evidence.

### PRV-10: Privacy incident readiness

- **What can go wrong:** A breach cannot be scoped because data, recipients, subjects, and retention are unknown; notification clocks and evidence collection start late.
- **Enterprise hardening intent:** Define privacy incident classification, data and subject scoping, preservation, processor coordination, notification decision, and lessons-learned process.
- **Expected evidence:** Privacy playbook; tabletop exercise; breach data map; notification decision log.

### PRV-11: Legal and regulatory compliance controls

- **What can go wrong:** A system collects personal data without a documented lawful basis or user consent; cookie tracking fires before consent is granted; data subject requests (right to deletion, portability, correction) have no implementation path; breach notification timelines are missed because the process doesn't exist; systems subject to HIPAA, PCI-DSS, GDPR, or CCPA are built without the specific controls those regimes require. AI-generated code commonly skips these entirely because they are process controls, not functional requirements.
- **Enterprise hardening intent:** Document the lawful basis for each category of personal data collected; implement and test cookie consent that blocks non-essential tracking until consent is granted; build and test a data subject request workflow (deletion, export, correction) before going live with any personal data collection; document breach notification timelines and responsible contacts; identify applicable regulatory regimes at project classification time and verify specific controls are in place before production.
- **Expected evidence:** Lawful basis register; cookie consent implementation test confirming pre-consent blocking; data subject request workflow test; regulatory control checklist for applicable regimes; breach notification runbook.


## 4.11 SAF: Safety, Human Impact and Responsible Automation

**Failure question:** Could software behavior, automation, or failure cause physical, financial, legal, social, or systemic harm beyond ordinary defects?

**Typical accountable owners:** Safety/Risk owner, Product owner, Engineering, Human factors, Independent reviewer

**Primary failure primitives:** `INCORRECT`, `UNSAFE_CHANGE`, `IRRECOVERABLE`, `UNOBSERVABLE`, `NONCOMPLIANT`

**Lifecycle phases:** `discover`, `design`, `verify`, `operate`, `respond`

### SAF-01: Hazard analysis and safety case

- **What can go wrong:** Hazards are discovered only after deployment; claims of safety are unsupported by structured evidence and assumptions.
- **Enterprise hardening intent:** Identify hazards, severity, exposure, controllability, causal paths, and mitigations; maintain a safety case proportional to potential harm.
- **Expected evidence:** Hazard log; fault or event tree; safety argument; review.

### SAF-02: Unsafe actions and output constraints

- **What can go wrong:** The system generates or executes actions outside approved limits, including irreversible, high-value, or physically consequential operations.
- **Enterprise hardening intent:** Constrain action space, values, destinations, frequency, and prerequisites; validate outputs independently before execution.
- **Expected evidence:** Action policy; constraint tests; independent validator evidence.

### SAF-03: Fail-safe defaults, interlocks, and limits

- **What can go wrong:** On sensor, dependency, model, configuration, or validation failure, the system continues in a dangerous mode.
- **Enterprise hardening intent:** Define safe state, interlocks, rate and value limits, redundancy, and fail-stop or fail-operational behavior based on hazard analysis.
- **Expected evidence:** Fail-safe specification; fault-injection and interlock tests.

### SAF-04: Human approval, override, and kill switch

- **What can go wrong:** Automation removes meaningful review; operators cannot pause, reverse, or contain a harmful process quickly. Autonomous AI agents that send communications, create records in third-party systems, make purchases, or post public content on a schedule take irreversible real-world actions with no human review by default, making the platform the actor in any harm the agent causes. A kill switch that halts the execution loop but not pending queued actions does not stop harm already in motion.
- **Enterprise hardening intent:** Require approval for defined high-impact actions; provide tested pause, cancel, rollback, isolation, and emergency-stop mechanisms. Autonomous agents authorized to take irreversible external-world actions (sending emails, creating calendar events, posting messages, making purchases) MUST default to human-review-required before execution. Bypassing human review MUST be an explicit, per-capability opt-out with a documented user acknowledgment: review-required is the default, not opt-in. The kill switch MUST halt not just the execution loop but all pending irreversible actions in the queue. Time-to-halt from operator command to confirmed cessation MUST be defined and tested.
- **Expected evidence:** Approval matrix; human-review gate test confirming default-on behavior; opt-out acknowledgment record; kill-switch drill confirming queued action cancellation; override audit.

### SAF-05: Automation bias and operator experience

- **What can go wrong:** Confident presentation causes users to accept wrong output; alerts overload operators; interfaces hide uncertainty or consequences.
- **Enterprise hardening intent:** Display uncertainty, source, consequence, and alternatives; design confirmation and escalation for high-risk decisions; test human factors.
- **Expected evidence:** Human-factors review; usability study; warning and confirmation tests.

### SAF-06: Explanation, contestability, and recourse

- **What can go wrong:** Affected people and operators cannot understand, challenge, or correct a consequential decision.
- **Enterprise hardening intent:** Provide fit-for-purpose reasons, evidence, review channels, correction, and appeal; log decisions without exposing sensitive internals.
- **Expected evidence:** Explanation specification; appeal workflow; sample decision audit.

### SAF-07: Vulnerable populations and distributional impact

- **What can go wrong:** Defaults or failure modes disproportionately harm children, elderly users, disabled users, low-resource users, or protected groups.
- **Enterprise hardening intent:** Conduct impact assessment and representative testing; add safeguards, accessible alternatives, and monitoring for disparate outcomes.
- **Expected evidence:** Impact assessment; subgroup tests; mitigation and monitoring plan.

### SAF-08: Dual use and foreseeable misuse

- **What can go wrong:** Capabilities intended for legitimate use are easily repurposed for surveillance, fraud, harassment, intrusion, or dangerous automation.
- **Enterprise hardening intent:** Assess capability misuse, access tiers, identity and purpose checks, limits, monitoring, and refusal or review paths.
- **Expected evidence:** Misuse assessment; access policy; red-team results; response plan.

### SAF-09: Monitoring, near misses, and safe degradation

- **What can go wrong:** Only catastrophic events are measured; near misses and precursor signals are lost; degraded mode silently becomes unsafe normal operation.
- **Enterprise hardening intent:** Capture safety indicators and near misses; define safe degradation duration and exit; alert accountable humans and review trends.
- **Expected evidence:** Safety dashboard; near-miss log; degraded-mode test.

### SAF-10: Safety-critical change and independent assurance

- **What can go wrong:** A small code, model, data, configuration, or dependency change invalidates prior safety assumptions without renewed review.
- **Enterprise hardening intent:** Identify safety-relevant items; require impact analysis, independent verification, staged rollout, and updated safety evidence for changes.
- **Expected evidence:** Safety change assessment; independent sign-off; regression and rollout evidence.

### SAF-11: Algorithmic fairness and bias

- **What can go wrong:** A recommendation, ranking, scoring, credit, pricing, hiring, or content-moderation system produces systematically different outcomes for different demographic groups; training data encodes historical bias; proxy features (zip code, name, device type) correlate with protected characteristics; fairness is never measured because no one asked for it; AI-generated ranking or filtering code has no fairness test.
- **Enterprise hardening intent:** For any system that makes or influences consequential decisions about people, define fairness criteria and measure disparate impact across relevant groups before launch; document the choice of fairness metric and known trade-offs; test for proxy discrimination through correlated features; monitor fairness metrics post-launch as model and data distributions shift; provide a contestability path for affected individuals.
- **Expected evidence:** Fairness criteria definition; disparate impact analysis; proxy feature audit; post-launch fairness monitoring; contestability workflow.


## 4.12 SUP: Dependencies, Provenance and Software Supply Chain

**Failure question:** Could code, packages, tools, services, or artifacts be malicious, vulnerable, counterfeit, abandoned, legally unusable, or different from what was reviewed?

**Typical accountable owners:** Engineering lead, Security, Build/Release, Procurement/Legal, Dependency owner

**Primary failure primitives:** `TAMPERING`, `DISCLOSURE`, `UNAVAILABLE`, `NONCOMPLIANT`, `UNMAINTAINABLE`

**Lifecycle phases:** `select`, `build`, `release`, `monitor`, `retire`

### SUP-01: Dependency inventory and SBOM

- **What can go wrong:** Direct, transitive, build-time, runtime, optional, container, plugin, and service dependencies are unknown, so exposure cannot be scoped.
- **Enterprise hardening intent:** Generate and retain an SBOM for each releasable artifact; include versions, hashes, source, relationship, scope, owner, and deployment mapping.
- **Expected evidence:** SBOM; deployment inventory; component ownership; inventory diff.

### SUP-02: Source and registry authenticity

- **What can go wrong:** Packages are fetched from public or untrusted registries, mutable URLs, branches, or mirrors; namespace and source rules are ambiguous.
- **Enterprise hardening intent:** Allowlist registries and repositories; use namespace controls, internal proxies, verified publishers, TLS, immutable references, and source-policy enforcement.
- **Expected evidence:** Registry policy; source allowlist; resolver configuration; provenance check.

### SUP-03: Pinning, lockfiles, and reproducibility

- **What can go wrong:** Floating versions, broad ranges, tags, and transitive resolution cause different code to build later or in another environment.
- **Enterprise hardening intent:** Commit lockfiles where appropriate; pin immutable versions and digests; record toolchain versions; verify deterministic resolution.
- **Expected evidence:** Lockfile; digest pins; reproducibility test; dependency diff.

### SUP-04: Provenance, signing, and attestations

- **What can go wrong:** An artifact cannot be linked to reviewed source, authorized build, inputs, or builder; signatures are absent or not verified.
- **Enterprise hardening intent:** Produce verifiable build provenance and signatures; protect signing identities; verify policy before promotion and deployment.
- **Expected evidence:** Signed artifact; provenance attestation; verification log; key policy.

### SUP-05: Vulnerability and exploited-risk management

- **What can go wrong:** Known vulnerabilities are ignored because severity is low, exploitability is unknown, or a fix is inconvenient; deployed reach is unclear.
- **Enterprise hardening intent:** Continuously scan and map findings to deployed artifacts; prioritize reachable and known-exploited weaknesses; define patch and mitigation SLAs.
- **Expected evidence:** SCA report; reachability or VEX record; KEV check; remediation evidence.

### SUP-06: License, intellectual property, and export constraints

- **What can go wrong:** Copyleft, incompatible, unapproved, copyrighted, patented, or export-controlled components enter deliverables without review.
- **Enterprise hardening intent:** Enforce license and source policies; preserve notices; scan generated and copied code; obtain approval for restricted or unclear material.
- **Expected evidence:** License inventory; policy result; notices; legal approval.

### SUP-07: Maintainer health and end of life

- **What can go wrong:** A critical package is abandoned, single-maintainer, unresponsive, compromised, or near end-of-support with no replacement.
- **Enterprise hardening intent:** Assess project activity, ownership, release cadence, security process, bus factor, support horizon, and fork or replacement options.
- **Expected evidence:** Health assessment; support date; owner and exit plan.

### SUP-08: Transitive, optional, and runtime loading

- **What can go wrong:** Hidden plugins, post-install scripts, dynamic downloads, optional features, or transitive packages execute with broad privilege.
- **Enterprise hardening intent:** Disable unnecessary hooks and features; review install scripts and plugins; minimize dependency graph; constrain runtime loading and network access.
- **Expected evidence:** Dependency graph; hook inventory; sandbox or egress evidence.

### SUP-09: Dependency confusion, typosquatting, and AI package hallucination

- **What can go wrong:** A misspelled, private, or fabricated package name resolves to attacker-controlled code, especially when an AI invents plausible dependencies.
- **Enterprise hardening intent:** Verify every new package exists, is intended, approved, and sourced correctly; reserve private namespaces; block unknown names and automatic installation.
- **Expected evidence:** Package verification record; namespace controls; allowlist; review of AI-added dependencies.

### SUP-10: Update and automation policy

- **What can go wrong:** Automated update bots merge breaking or malicious changes; stale dependencies accumulate; major changes arrive without migration review.
- **Enterprise hardening intent:** Separate patch, minor, and major policy; require tests, provenance, changelog and risk review; stage updates and retain rollback.
- **Expected evidence:** Update policy; bot configuration; test and review record; rollback artifact.

### SUP-11: Third-party services and SaaS dependencies

- **What can go wrong:** A service changes behavior, region, retention, API, pricing, ownership, or security posture; outage and exit paths are untested. Multiple critical features depend on the same vendor, creating a concentration risk where one outage fails the entire product. The application has no defined behavior when a SaaS dependency is down: it crashes or shows raw errors rather than degrading gracefully.
- **Enterprise hardening intent:** Document data and credential scope, SLOs, limits, portability, fallback, deletion, incident notification, and exit procedures. Inventory vendor concentration: identify which vendors are single points of failure for the product and design explicit fallback behavior. Test the application's behavior when each critical SaaS dependency is unavailable; define what users see and what functionality remains.
- **Expected evidence:** Vendor record; concentration risk inventory; contract controls; failure simulation test for each critical dependency; documented and tested fallback behavior.

### SUP-12: Vendored, copied, generated, and snippet code

- **What can go wrong:** Code copied from forums, models, samples, or old repositories bypasses normal provenance, license, security, and maintenance review.
- **Enterprise hardening intent:** Treat all non-original code as a dependency; record origin and license; scan and test it; assign an owner and update method.
- **Expected evidence:** Origin record; license review; security scan; ownership entry.


## 4.13 BLD: Build, CI, Artifact Integrity and Repository Controls

**Failure question:** Could the development and build system alter, leak, or approve code and artifacts outside the intended review and trust process?

**Typical accountable owners:** Build/Release, Engineering lead, Security, Repository administrator, Platform

**Primary failure primitives:** `TAMPERING`, `DISCLOSURE`, `UNSAFE_CHANGE`, `UNOBSERVABLE`, `INCOMPATIBLE`

**Lifecycle phases:** `build`, `verify`, `release`

### BLD-01: Source control and branch protection

- **What can go wrong:** Direct pushes, force-pushes, unsigned releases, deleted history, or bypassable checks let unreviewed code become authoritative.
- **Enterprise hardening intent:** Protect default and release branches; require review and status checks; restrict bypass; preserve history; sign tags or releases where appropriate.
- **Expected evidence:** Branch rules; bypass audit; signed tag or release; review history.

### BLD-02: CI identity and least privilege

- **What can go wrong:** CI tokens can administer repositories, read all secrets, modify cloud resources, or persist beyond a job.
- **Enterprise hardening intent:** Use job-scoped, short-lived identities and minimal permissions; separate read, test, publish, and deploy roles; deny unused capabilities.
- **Expected evidence:** Workflow permission manifest; IAM analysis; token lifetime evidence.

### BLD-03: Untrusted contributions and fork isolation

- **What can go wrong:** Pull requests or artifacts from untrusted forks execute before approval with secrets, writable tokens, privileged runners, or network access.
- **Enterprise hardening intent:** Separate untrusted and trusted stages; use read-only tokens, no secrets, isolated runners, safe event triggers, and explicit promotion.
- **Expected evidence:** Fork workflow tests; permission scan; runner isolation evidence.

### BLD-04: Secrets in CI, logs, and artifacts

- **What can go wrong:** Environment variables, command traces, test fixtures, caches, coverage uploads, or build outputs expose credentials and sensitive data.
- **Enterprise hardening intent:** Fetch secrets only when needed; mask and avoid echoing; scope by environment; scan logs and artifacts; rotate on suspected exposure.
- **Expected evidence:** Secret scan; log review; environment protection; rotation test.

### BLD-05: Hermetic and reproducible builds

- **What can go wrong:** Build output depends on undeclared network downloads, time, mutable tools, host state, or hidden environment values.
- **Enterprise hardening intent:** Declare and pin inputs; isolate builds; restrict network; normalize time and metadata where feasible; compare repeated outputs.
- **Expected evidence:** Build manifest; network policy; reproducibility result; input inventory.

### BLD-06: Build scripts, plugins, and toolchain

- **What can go wrong:** Makefiles, package scripts, compiler plugins, actions, images, and code generators execute arbitrary unreviewed code.
- **Enterprise hardening intent:** Inventory and pin build tooling; review privileged scripts; prefer trusted reusable workflows; sandbox code generation and post-install steps.
- **Expected evidence:** Toolchain lock; action or image digests; script review; sandbox policy.

### BLD-07: Automated quality and security gates

- **What can go wrong:** Tests, lint, type checks, SAST, SCA, secret scanning, and policy checks are optional, flaky, skipped, or run after merge.
- **Enterprise hardening intent:** Define required gates by profile; fail closed on missing or errored checks; prevent code from weakening its own gate without independent approval.
- **Expected evidence:** Required-check configuration; gate result; bypass and flake metrics.

### BLD-08: Artifact signing, storage, and promotion

- **What can go wrong:** Mutable artifact names are rebuilt per environment; registries permit overwrite; promotion does not verify provenance.
- **Enterprise hardening intent:** Build once, sign, store immutably, scan, and promote the same digest through environments with policy verification.
- **Expected evidence:** Artifact digest; signature; immutable registry settings; promotion log.

### BLD-09: Caches, workspaces, and poisoning

- **What can go wrong:** Shared caches or persistent workspaces mix tenants, branches, credentials, or attacker-controlled outputs across jobs.
- **Enterprise hardening intent:** Namespace and validate caches; avoid caching secrets; isolate workspaces; clean runners; verify restored artifacts before execution.
- **Expected evidence:** Cache policy; isolation test; cleanup record; integrity check.

### BLD-10: Release identity, versioning, and retention

- **What can go wrong:** Versions are ambiguous or reused; release source and artifact cannot be reconstructed; rollback artifacts disappear.
- **Enterprise hardening intent:** Use unique immutable versions and source references; generate changelogs; retain approved artifacts, metadata, SBOM, provenance, and rollback versions.
- **Expected evidence:** Release manifest; version policy; retention configuration.

### BLD-11: Runner and builder hardening

- **What can go wrong:** Long-lived, privileged builders become persistence points; Docker socket, host mounts, or broad network access expose the estate.
- **Enterprise hardening intent:** Use ephemeral, patched, isolated builders; minimize host access; constrain network and mounts; monitor builder integrity.
- **Expected evidence:** Builder baseline; ephemeral-run evidence; network and mount policy.

### BLD-12: Environment parity and build-once discipline

- **What can go wrong:** Different environments compile different code, dependencies, flags, or assets; production defects cannot be reproduced.
- **Enterprise hardening intent:** Externalize environment configuration; promote identical artifacts; validate runtime compatibility; maintain representative pre-production environments.
- **Expected evidence:** Artifact digest comparison; parity checks; environment manifest.


## 4.14 CFG: Configuration, Secrets and Feature Flags

**Failure question:** Could configuration or generated policy behave like untested code, propagate globally, expose secrets, or make rollback impossible?

**Typical accountable owners:** Service owner, Platform/SRE, Security, Release manager, Configuration owner

**Primary failure primitives:** `INCORRECT`, `DISCLOSURE`, `UNSAFE_CHANGE`, `UNAVAILABLE`, `TAMPERING`

**Lifecycle phases:** `design`, `build`, `release`, `operate`, `recover`

### CFG-01: Configuration inventory, schema, and ownership

- **What can go wrong:** Settings exist in code, files, environment variables, consoles, databases, and flags without a canonical source or owner.
- **Enterprise hardening intent:** Inventory configuration sources; define schema, type, sensitivity, owner, default, environment scope, and change path.
- **Expected evidence:** Configuration catalog; schema; owner map; source-of-truth declaration.

### CFG-02: Secure defaults and required values

- **What can go wrong:** Missing configuration silently enables insecure, destructive, expensive, or production-incompatible behavior.
- **Enterprise hardening intent:** Choose safe defaults; require explicit acknowledgement for dangerous modes; fail startup or deployment when mandatory values are absent or inconsistent.
- **Expected evidence:** Default review; startup validation tests; dangerous-setting inventory.

### CFG-03: Type, range, size, and cardinality validation

- **What can go wrong:** Syntactically valid configuration produces huge files, duplicate entries, invalid combinations, resource exhaustion, or unsafe thresholds.
- **Enterprise hardening intent:** Validate semantic constraints, relationships, uniqueness, count, size, complexity, and generated-output limits before publication.
- **Expected evidence:** Schema and semantic tests; generated-size budget; adversarial config test.

### CFG-04: Environment separation and override control

- **What can go wrong:** Development defaults, debug flags, sandbox endpoints, or test credentials leak into production; override precedence is unclear.
- **Enterprise hardening intent:** Define explicit environments and precedence; block cross-environment references; diff effective configuration before promotion.
- **Expected evidence:** Effective-config diff; environment policy; cross-reference scan.

### CFG-05: Secret storage, distribution, and rotation

- **What can go wrong:** Secrets are committed, copied into config, baked into images, logged, or shared across environments and services.
- **Enterprise hardening intent:** Use managed secret stores and workload identity; deliver at runtime; scope, rotate, revoke, audit, and prevent secret values in normal configuration.
- **Expected evidence:** Secret-store policy; scan; rotation record; access audit.

### CFG-06: Configuration as code and review

- **What can go wrong:** Console edits and database rows change production without review, history, testing, or reproducibility.
- **Enterprise hardening intent:** Version configuration; require peer review, validation, change linkage, and immutable history; reconcile out-of-band changes.
- **Expected evidence:** Repository history; approval; drift report; change ticket.

### CFG-07: Progressive rollout, stop, and rollback

- **What can go wrong:** A bad configuration is pushed globally and repeatedly; the system cannot stop propagation or restore last-known-good state.
- **Enterprise hardening intent:** Canary configuration by cell or cohort; monitor health; automatically halt on thresholds; retain and test rollback to a hermetic prior version.
- **Expected evidence:** Canary record; stop policy; rollback drill; last-known-good artifact.

### CFG-08: Feature flag lifecycle and cleanup

- **What can go wrong:** Flags bypass testing, create combinatorial states, expose incomplete features, or remain indefinitely with stale permissions.
- **Enterprise hardening intent:** Classify flags; assign owner and expiry; secure evaluation; test critical combinations; remove code and data paths after rollout.
- **Expected evidence:** Flag inventory; expiry check; combination tests; cleanup change.

### CFG-09: Dynamic configuration consistency

- **What can go wrong:** Nodes observe different policy versions; partial propagation creates split behavior; stale caches persist after revocation.
- **Enterprise hardening intent:** Version and atomically publish configuration; define convergence and rollback semantics; expose active version and lag metrics.
- **Expected evidence:** Version telemetry; convergence test; stale-config alert.

### CFG-10: Drift detection and reconciliation

- **What can go wrong:** Runtime, cloud, database, or console state diverges from declared configuration without detection.
- **Enterprise hardening intent:** Continuously compare desired and effective state; alert or reconcile safely; preserve approved emergency changes before overwrite.
- **Expected evidence:** Drift report; reconciliation policy; exception capture.

### CFG-11: User-supplied and tenant configuration

- **What can go wrong:** Valid customer templates, regexes, rules, queries, plugins, or workflows trigger latent bugs or consume unbounded resources.
- **Enterprise hardening intent:** Treat customer configuration as adversarial code; constrain language and resources; lint, simulate, fuzz, sandbox, and canary it.
- **Expected evidence:** Configuration sandbox; complexity tests; tenant limits; simulation result.

### CFG-12: Bootstrap, recovery, and out-of-band configuration

- **What can go wrong:** A control-plane outage prevents operators from changing the setting needed for recovery; bootstrap secrets or defaults are unsafe.
- **Enterprise hardening intent:** Design minimal, protected, tested bootstrap and break-glass paths independent of the failed plane; audit and rotate after use.
- **Expected evidence:** Recovery configuration runbook; out-of-band drill; bootstrap credential audit.


## 4.15 INF: Infrastructure, Runtime, Network and Platform Security

**Failure question:** Could insecure or coupled runtime foundations expose the application, amplify failure, or prevent safe operation and recovery?

**Typical accountable owners:** Platform/SRE, Cloud/Infrastructure owner, Security, Network, Service owner

**Primary failure primitives:** `UNAUTHORIZED`, `UNAVAILABLE`, `DISCLOSURE`, `TAMPERING`, `IRRECOVERABLE`

**Lifecycle phases:** `design`, `provision`, `operate`, `recover`, `retire`

### INF-01: Infrastructure as code and drift

- **What can go wrong:** Cloud and platform resources are created manually, cannot be reproduced, and drift from reviewed definitions.
- **Enterprise hardening intent:** Manage infrastructure declaratively; review and plan changes; scan policy; detect drift; import or remove unmanaged resources.
- **Expected evidence:** IaC repository; plan and policy result; drift report.

### INF-02: Account, project, subscription, and environment isolation

- **What can go wrong:** Production, development, tenants, billing, and administrative workloads share blast radius, quotas, credentials, or network.
- **Enterprise hardening intent:** Use separate accounts or equivalent boundaries proportionate to risk; centralize guardrails while isolating credentials, quotas, and failure domains.
- **Expected evidence:** Environment topology; organization policies; cross-environment access review.

### INF-03: Network segmentation, ingress, and egress

- **What can go wrong:** Services are broadly reachable; internal traffic is trusted; outbound connections and DNS can reach sensitive destinations.
- **Enterprise hardening intent:** Minimize exposed ports and paths; authenticate service traffic; segment tiers; control egress and DNS; log material network decisions.
- **Expected evidence:** Network policy; reachability scan; egress test; firewall review.

### INF-04: Compute and operating-system hardening

- **What can go wrong:** Hosts run unsupported software, default services, broad privileges, weak filesystem permissions, or delayed patches.
- **Enterprise hardening intent:** Use hardened immutable images where practical; minimize packages and services; patch to policy; enforce host protection, time sync, and integrity monitoring.
- **Expected evidence:** Baseline scan; patch compliance; image manifest; integrity alerts.

### INF-05: Containers, images, and process privilege

- **What can go wrong:** Containers run as root, use mutable tags, include tools and secrets, mount host paths, or grant excessive capabilities.
- **Enterprise hardening intent:** Use minimal signed images, non-root users, read-only filesystems, dropped capabilities, resource limits, and digest pins.
- **Expected evidence:** Image scan; runtime security context; capability and mount audit.

### INF-06: Orchestrator and Kubernetes controls

- **What can go wrong:** Workloads bypass namespace, admission, pod security, network policy, quota, and secret controls; control-plane access is broad.
- **Enterprise hardening intent:** Apply enforced admission and pod-security standards, RBAC least privilege, network policy, quotas, secure service accounts, and audit logging.
- **Expected evidence:** Admission policy; RBAC analysis; pod-security and network tests.

### INF-07: Cloud IAM, metadata, and workload identity

- **What can go wrong:** Instances and functions receive broad roles; metadata credentials are reachable through SSRF; long-lived cloud keys are embedded.
- **Enterprise hardening intent:** Use workload identity, minimal roles, metadata protection, short sessions, organization guardrails, and continuous permission analysis.
- **Expected evidence:** IAM report; metadata test; key inventory; unused-permission finding.

### INF-08: Storage, databases, encryption, and public access

- **What can go wrong:** Buckets, snapshots, databases, queues, or backups are public, weakly authenticated, unencrypted, or shared across trust zones.
- **Enterprise hardening intent:** Block public access by policy; use private endpoints and encryption; separate duties; monitor sharing, snapshots, and exposure changes.
- **Expected evidence:** Exposure scan; encryption and endpoint configuration; access audit.

### INF-09: DNS, TLS, certificates, and time

- **What can go wrong:** Expired or misissued certificates, insecure DNS, weak TLS, clock drift, and unmanaged domains break trust and availability.
- **Enterprise hardening intent:** Automate inventory, issuance, renewal, validation, and expiry alerting; enforce current TLS; secure DNS and authoritative ownership; synchronize time.
- **Expected evidence:** Certificate and domain inventory; renewal test; TLS scan; time-sync health.

### INF-10: Quotas, autoscaling, and platform limits

- **What can go wrong:** Scaling is blocked by account quotas, or runaway workloads scale cost and failure without upper bounds.
- **Enterprise hardening intent:** Model quotas and dependencies; set requests, limits, budgets, max scale, priority, and load-shedding; test quota exhaustion.
- **Expected evidence:** Quota register; autoscaling test; budget alert; saturation exercise.

### INF-11: Regions, zones, control planes, and correlated failure

- **What can go wrong:** Redundancy shares one region, identity provider, deployment system, DNS, or administrative control plane.
- **Enterprise hardening intent:** Map correlated dependencies; isolate critical control and data paths; test zone and region loss; maintain out-of-band recovery access.
- **Expected evidence:** Failure-domain map; failover drill; dependency analysis.

### INF-12: Administrative access and recovery paths

- **What can go wrong:** Production access is always-on, shared, unrecorded, or dependent on the impaired network and identity plane.
- **Enterprise hardening intent:** Use JIT privileged access, strong MFA, session audit, bastions or controlled paths, and tested emergency access independent of primary failure.
- **Expected evidence:** Access logs; JIT policy; emergency access drill.


## 4.16 REL: Reliability, Resilience and Distributed Systems

**Failure question:** Could routine component failure, delay, duplication, overload, or recovery behavior cascade into prolonged or incorrect service failure?

**Typical accountable owners:** SRE, Service owner, Architect, Engineering lead, Operations

**Primary failure primitives:** `UNAVAILABLE`, `EXHAUSTION`, `IRRECOVERABLE`, `UNOBSERVABLE`, `INCONSISTENT`

**Lifecycle phases:** `design`, `build`, `verify`, `operate`, `recover`

### REL-01: SLIs, SLOs, and error budgets

- **What can go wrong:** Availability is claimed without user-centered measurements; teams optimize uptime of components that do not reflect successful outcomes.
- **Enterprise hardening intent:** Define service-level indicators and objectives for critical user journeys and correctness; use error budgets to balance change and reliability work.
- **Expected evidence:** SLO document; measurement query; error-budget policy; review.

### REL-02: Timeouts, deadlines, and cancellation

- **What can go wrong:** Calls wait indefinitely; upstream timeouts are shorter than downstream work; abandoned requests continue consuming resources.
- **Enterprise hardening intent:** Set end-to-end deadlines and smaller downstream budgets; propagate cancellation; distinguish queue, connect, read, and operation timeouts.
- **Expected evidence:** Timeout budget; cancellation tests; stuck-call metrics.

### REL-03: Retries, backoff, jitter, and retry budgets

- **What can go wrong:** Layered retries multiply load, repeat non-idempotent work, synchronize storms, and extend latency past user deadlines.
- **Enterprise hardening intent:** Retry only classified transient failures at one responsible layer; use exponential backoff, jitter, caps, deadlines, and retry budgets.
- **Expected evidence:** Retry policy; fault test; retry-volume dashboard.

### REL-04: Circuit breakers, bulkheads, and load shedding

- **What can go wrong:** A failing dependency consumes every thread, connection, worker, or queue; low-value work crowds out recovery and critical work.
- **Enterprise hardening intent:** Partition resources; trip on defined signals; shed or degrade low-priority work; preserve recovery and control capacity.
- **Expected evidence:** Bulkhead design; breaker and shedding tests; saturation metrics.

### REL-05: Idempotency and duplicate delivery

- **What can go wrong:** At-least-once networks and queues create duplicate effects; exactly-once is assumed without durable coordination.
- **Enterprise hardening intent:** Make operations replay-safe with idempotency keys, deduplication, commutative updates, or compensating records; document limits.
- **Expected evidence:** Replay tests; idempotency store metrics; duplicate audit.

### REL-06: Queues, backpressure, and poison work

- **What can go wrong:** Producers overwhelm consumers; queues grow without bound; one toxic message loops forever; lag is invisible.
- **Enterprise hardening intent:** Bound queues; propagate backpressure; set retry and dead-letter policy; isolate poison work; expose age, lag, and drain estimates.
- **Expected evidence:** Queue policy; overload test; dead-letter replay runbook; lag dashboard.

### REL-07: Consistency and distributed transactions

- **What can go wrong:** Partial commits, stale reads, and reordering violate business invariants because consistency is implicit.
- **Enterprise hardening intent:** Choose consistency model per operation; use outbox, saga, versioning, reconciliation, and user-visible pending states where atomicity is unavailable.
- **Expected evidence:** Consistency contract; failure and compensation tests; reconciliation.

### REL-08: Coordination, leader election, and split brain

- **What can go wrong:** Multiple leaders act concurrently, stale leases persist, or fencing is absent during network partitions and failover.
- **Enterprise hardening intent:** Use proven coordination systems, leases with fencing tokens, monotonic epochs, quorum assumptions, and split-brain tests.
- **Expected evidence:** Coordination design; partition test; fencing evidence.

### REL-09: Partial failure and graceful degradation

- **What can go wrong:** A secondary feature, analytics path, optional dependency, or stale cache takes down the primary journey or returns incorrect success. The UI shows a blank screen or raw error instead of communicating degraded state; partial results are displayed without indicating they are incomplete; a broken recommendation widget crashes the entire page; the app is all-or-nothing when it could be partially functional.
- **Enterprise hardening intent:** Classify essential and optional functions; isolate failures using error boundaries, try/catch at feature boundaries, and fallback UI components; return explicit degraded behavior rather than silent failure; prevent stale or partial data from masquerading as complete; show users what is unavailable and what still works rather than failing the whole page. Progressive enhancement: core functionality SHOULD work even when non-essential features, analytics, or third-party widgets fail.
- **Expected evidence:** Degradation matrix classifying essential vs. optional functions; dependency fault tests with UI verification; error boundary test confirming page survives component failure; user-message review confirming degraded state is communicated.

### REL-10: Redundancy, failover, and state recovery

- **What can go wrong:** Standbys are stale, capacity is insufficient, failover is manual and undocumented, or failback corrupts state.
- **Enterprise hardening intent:** Size and monitor redundancy; test automatic and manual failover, data loss, failback, and capacity; reconcile after recovery.
- **Expected evidence:** Failover drill; replication health; capacity and reconciliation result.

### REL-11: Startup, shutdown, readiness, and draining

- **What can go wrong:** Instances receive traffic before ready, terminate active work, replay startup actions, or never recover from dependency ordering.
- **Enterprise hardening intent:** Separate liveness and readiness; make initialization idempotent; drain connections and work; bound startup; support dependency unavailability.
- **Expected evidence:** Lifecycle tests; probe configuration; graceful-shutdown evidence.

### REL-12: Chaos, fault injection, and game days

- **What can go wrong:** Resilience mechanisms exist only in diagrams and fail during the first real incident.
- **Enterprise hardening intent:** Inject representative latency, errors, loss, corruption, quota, zone, credential, and control-plane faults in safe environments and controlled production scopes.
- **Expected evidence:** Experiment plan; safety guardrails; results; remediation actions.

### REL-13: Clock, ordering, and monotonicity

- **What can go wrong:** Wall-clock jumps, skew, timestamp collisions, and unordered delivery break leases, expiry, windows, and reconciliation.
- **Enterprise hardening intent:** Use monotonic clocks for durations, synchronize time, include sequence or version semantics, and tolerate bounded skew and reordering.
- **Expected evidence:** Clock policy; skew and reorder tests; time-sync alerts.

### REL-14: Cascading failure and feedback loops

- **What can go wrong:** Autoscaling, health checks, retries, cache misses, reconnects, and recovery traffic reinforce failure and create thundering herds.
- **Enterprise hardening intent:** Model feedback loops; add jitter, warmup, admission control, recovery prioritization, and rate limits; test recovery surge.
- **Expected evidence:** Cascade analysis; recovery-load test; herd-control metrics.

### REL-15: Transactional notification delivery

- **What can go wrong:** A password reset email, email verification link, 2FA code, billing receipt, or critical alert is never delivered and the failure is silent; the application has no way to know whether an email was delivered, bounced, or marked as spam; a user cannot complete account creation because the verification email went to junk; SPF, DKIM, or DMARC is not configured and transactional email is rejected by major providers; delivery failures are not monitored; there is no retry or fallback channel for failed critical notifications.
- **Enterprise hardening intent:** Monitor delivery status for critical transactional emails (password reset, verification, 2FA, billing) using provider webhooks or delivery receipts; alert on elevated bounce or spam rates; configure SPF, DKIM, and DMARC records before going live; define a retry strategy and, for critical paths (account access, payment confirmation), a fallback channel; surface delivery failures to users with actionable guidance rather than silent timeout. Do not use a marketing ESP for transactional email without verifying delivery priority and separation.
- **Expected evidence:** Email delivery monitoring configuration; SPF/DKIM/DMARC verification; bounce and complaint rate alert; password reset and verification email delivery test; documented fallback for critical notification paths.


## 4.17 PER: Performance, Scalability, Capacity and Cost

**Failure question:** Could normal growth, adversarial input, or inefficient design violate latency and throughput objectives or create runaway resource and cost consumption?

**Typical accountable owners:** Performance owner, SRE, Engineering lead, Database engineer, FinOps

**Primary failure primitives:** `EXHAUSTION`, `UNAVAILABLE`, `UNMAINTAINABLE`, `ABUSE`

**Lifecycle phases:** `design`, `build`, `verify`, `operate`

**Tier note:** PER is T3 by default, meaning gaps are noted but do not block release. Projects with defined concurrency, throughput, or latency requirements SHOULD treat PER as T2. At scale, performance failures are availability failures.

### PER-01: Performance objectives and budgets

- **What can go wrong:** Teams optimize arbitrary microbenchmarks; no latency percentile, throughput, payload, concurrency, or resource budget exists.
- **Enterprise hardening intent:** Set user- and system-level budgets by journey and load shape, including percentiles, warm and cold behavior, and error conditions.
- **Expected evidence:** Performance SLO; budget table; representative workload model.

### PER-02: Algorithm and data-structure complexity

- **What can go wrong:** Code works on small examples but becomes quadratic, exponential, or backtracking under large or crafted inputs.
- **Enterprise hardening intent:** Analyze worst-case time and space; set input bounds; choose appropriate structures; test adversarial sizes and algorithmic complexity.
- **Expected evidence:** Complexity review; benchmark curve; ReDoS or worst-case test.

### PER-03: CPU, memory, disk, network, and accelerator use

- **What can go wrong:** Allocations, copies, serialization, logging, temporary files, GPU memory, or network chatter saturate resources or leak over time.
- **Enterprise hardening intent:** Profile representative workloads; stream and batch deliberately; pool safely; bound memory; monitor saturation and leaks.
- **Expected evidence:** Profiles; resource budgets; soak test; saturation dashboard.

### PER-04: Database access, queries, and indexes

- **What can go wrong:** N+1 queries, missing indexes, large joins, lock contention, chatty transactions, and connection exhaustion dominate latency.
- **Enterprise hardening intent:** Review plans and cardinality; batch access; tune indexes; bound transactions; pool connections; monitor regressions.
- **Expected evidence:** Query plans; benchmark; connection and lock metrics.

### PER-05: Caching, CDN, and invalidation

- **What can go wrong:** Caches hide stale or unauthorized data, stampede on expiry, grow unbounded, or fail to improve the actual bottleneck.
- **Enterprise hardening intent:** Define correctness and freshness first; use tenant-safe keys, TTL and invalidation, size bounds, request coalescing, and fallback behavior.
- **Expected evidence:** Cache contract; hit and staleness metrics; stampede test.

### PER-06: Concurrency, parallelism, and contention

- **What can go wrong:** More workers reduce throughput due to locks, context switching, downstream limits, or non-thread-safe code.
- **Enterprise hardening intent:** Measure scaling curves; bound concurrency; minimize critical sections; align worker count with downstream and resource capacity.
- **Expected evidence:** Concurrency benchmark; contention profile; limit settings.

### PER-07: Load, stress, spike, and soak behavior

- **What can go wrong:** A system passes average-load tests but fails at peaks, sudden bursts, long duration, or recovery after saturation.
- **Enterprise hardening intent:** Test expected, peak, spike, overload, endurance, and recovery workloads using production-like data shape and dependency behavior.
- **Expected evidence:** Load-test report; breaking point; soak and recovery result.

### PER-08: Horizontal scale, skew, and affinity

- **What can go wrong:** State, sessions, hot keys, tenant skew, or locality assumptions prevent even scaling or cause one shard to fail first.
- **Enterprise hardening intent:** Design stateless or explicitly stateful scaling; test skew; rebalance safely; expose per-partition saturation.
- **Expected evidence:** Scale-out test; skew analysis; partition metrics.

### PER-09: Capacity, headroom, and autoscaling

- **What can go wrong:** Capacity is based on current average; autoscaling is too slow, uses misleading signals, or depends on unavailable quota.
- **Enterprise hardening intent:** Forecast demand; define headroom and launch latency; test scale signals, quota, cooldown, and maximum safe capacity.
- **Expected evidence:** Capacity plan; autoscaling exercise; quota and headroom dashboard.

### PER-10: Client, web, and mobile performance

- **What can go wrong:** Large bundles, blocking work, excessive requests, slow rendering, battery use, and poor low-bandwidth behavior harm users despite fast servers.
- **Enterprise hardening intent:** Set client budgets; optimize critical path, caching, rendering, image and asset delivery; test real devices and constrained networks.
- **Expected evidence:** Core journey measurements; bundle report; device and network tests.

### PER-11: Cost and consumption guardrails

- **What can go wrong:** An endpoint, job, tenant, query, model call, or retry loop creates unbounded cloud, API, storage, or egress spend.
- **Enterprise hardening intent:** Attribute cost; set budgets, quotas, circuit breakers, and alerts; model unit economics and failure cost; require approval for material changes.
- **Expected evidence:** Cost dashboard; unit-cost baseline; budget and anomaly alerts.

### PER-12: Performance regression and profiling

- **What can go wrong:** Performance decays through small changes; benchmarks are noisy, unrepresentative, or not tied to releases.
- **Enterprise hardening intent:** Maintain stable representative benchmarks and budgets; compare distributions; profile regressions; block material unexplained degradation.
- **Expected evidence:** Benchmark history; regression gate; profile and root-cause record.


## 4.18 TST: Verification, Testing and Independent Assurance

**Failure question:** Could defects survive because tests are narrow, self-fulfilling, flaky, nonrepresentative, or controlled by the same code and agent they evaluate?

**Typical accountable owners:** QA/Test lead, Engineering lead, Security, SRE, Independent reviewer for high criticality

**Primary failure primitives:** `OMISSION`, `INCORRECT`, `UNOBSERVABLE`, `UNSAFE_CHANGE`

**Lifecycle phases:** `plan`, `build`, `verify`, `release`

### TST-01: Risk-based test strategy and traceability

- **What can go wrong:** Tests are selected by ease or framework convention rather than requirements, risks, critical paths, and historical failures.
- **Enterprise hardening intent:** Map each material requirement, invariant, threat, hazard, and incident lesson to one or more verification methods and owners.
- **Expected evidence:** Test strategy; traceability matrix; coverage-gap list.

### TST-02: Unit and component verification

- **What can go wrong:** Business logic is tested only through slow end-to-end paths, or units mock away the behavior that matters.
- **Enterprise hardening intent:** Test pure logic, boundaries, invariants, and error paths close to the code; use realistic collaborators where contract behavior matters.
- **Expected evidence:** Unit results; boundary cases; mutation or fault sensitivity.

### TST-03: Integration and contract testing

- **What can go wrong:** Components individually pass but disagree on schema, auth, timeouts, transactions, versions, or error semantics.
- **Enterprise hardening intent:** Exercise real protocol and storage boundaries; use provider and consumer contracts; test supported mixed versions and dependency failure.
- **Expected evidence:** Integration environment results; contract suite; compatibility matrix.

### TST-04: End-to-end, smoke, and critical-journey tests

- **What can go wrong:** Only isolated functions are tested; deployment wiring, identity, configuration, data, and user journeys fail together.
- **Enterprise hardening intent:** Maintain a small stable set of production-like critical journeys plus post-deploy smoke tests with explicit cleanup and ownership.
- **Expected evidence:** E2E results; smoke record; journey inventory.

### TST-05: Negative, boundary, and property-based testing

- **What can go wrong:** Examples confirm expected inputs but do not explore invalid, extreme, duplicate, reordered, partial, or invariant-breaking cases.
- **Enterprise hardening intent:** Partition input space; test limits and invalid combinations; express invariants as properties; shrink and retain discovered counterexamples.
- **Expected evidence:** Boundary matrix; property-test results; regression corpus.

### TST-06: Fuzzing, mutation, and fault injection

- **What can go wrong:** Parsers, validators, serializers, exception paths, and tests appear robust because inputs and faults are predictable.
- **Enterprise hardening intent:** Continuously fuzz exposed parsers and protocols; use mutation testing to assess assertion quality; inject dependency, I/O, and state faults.
- **Expected evidence:** Fuzz corpus and crashes; mutation score; fault-injection report.

### TST-07: Security testing and analysis

- **What can go wrong:** Security is reduced to one scanner; logic, auth, configuration, dependencies, secrets, and runtime behavior escape coverage.
- **Enterprise hardening intent:** Combine threat-model review, SAST, SCA, secret scanning, IaC and container checks, DAST, manual testing, and targeted penetration testing.
- **Expected evidence:** Security test plan; tool results; manual findings; remediation record.

### TST-08: Concurrency, determinism, and flakiness

- **What can go wrong:** Races pass locally; tests depend on timing, order, shared state, network, or retries; failures are rerun until green.
- **Enterprise hardening intent:** Use race detectors, controlled schedulers or repeated stress; isolate state; eliminate sleeps; quarantine only with owner and expiry.
- **Expected evidence:** Flake trend; race results; quarantine register; deterministic rerun.

### TST-09: Performance, resilience, and chaos testing

- **What can go wrong:** Functional tests pass while load, saturation, failover, retries, queues, and recovery collapse under realistic conditions.
- **Enterprise hardening intent:** Test performance budgets, overload, dependencies, failover, recovery surge, and degradation with production-like topology and observability.
- **Expected evidence:** Load and chaos reports; SLO impact; recovery evidence.

### TST-10: Migration, rollback, restore, and upgrade testing

- **What can go wrong:** Forward deployment succeeds but rollback, data downgrade, restore, mixed versions, and retry after interruption are untested.
- **Enterprise hardening intent:** Rehearse expand-contract migration, checkpoint resume, rollback or roll-forward, backup restore, failback, and version skew.
- **Expected evidence:** Rehearsal record; data verification; restore and rollback timing.

### TST-11: Accessibility, compatibility, and usability testing

- **What can go wrong:** Automated happy-path browser tests miss keyboard, screen-reader, zoom, localization, device, and error-recovery failures.
- **Enterprise hardening intent:** Combine automated checks with manual assistive-technology and representative-user testing across supported platforms and locales.
- **Expected evidence:** Accessibility report; browser and device matrix; usability findings.

### TST-12: Test data and environment isolation

- **What can go wrong:** Tests use shared mutable accounts or production data; environment drift and hidden dependencies make results misleading.
- **Enterprise hardening intent:** Use controlled representative data, isolated resources, seeded versions, cleanup, environment manifests, and explicit external dependency stubs.
- **Expected evidence:** Environment manifest; data provenance; isolation and cleanup checks.

### TST-13: Coverage quality and escaped-defect analysis

- **What can go wrong:** High line coverage creates false confidence; critical decisions, error paths, and integrations remain untested.
- **Enterprise hardening intent:** Measure requirement and risk coverage, branch and mutation sensitivity, defect escape, and change hotspots rather than a single percentage.
- **Expected evidence:** Coverage dashboard; escape review; missing-test actions.

### TST-14: Independent review, adversarial testing, and red teaming

- **What can go wrong:** The authoring team and AI validate their own assumptions; shared blind spots survive every automated gate.
- **Enterprise hardening intent:** Require independent design or code review and adversarial testing proportional to criticality, novelty, exposure, and impact.
- **Expected evidence:** Independent sign-off; red-team or penetration report; remediation retest.

### TST-15: Production verification and synthetic monitoring

- **What can go wrong:** Pre-production cannot reproduce traffic, data, topology, and dependencies; deployment success is inferred from process completion.
- **Enterprise hardening intent:** Use canary analysis, post-deploy verification, synthetic journeys, shadow or replay where safe, and business outcome checks.
- **Expected evidence:** Canary result; synthetic dashboard; release verification record.


## 4.19 OBS: Observability, Auditability and Diagnostics

**Failure question:** Could users be harmed while the system appears healthy, or could responders lack safe, correlated, privacy-preserving evidence to diagnose it?

**Typical accountable owners:** SRE, Service owner, Security, Data/Privacy, Operations

**Primary failure primitives:** `UNOBSERVABLE`, `DISCLOSURE`, `EXHAUSTION`, `NONCOMPLIANT`

**Lifecycle phases:** `design`, `build`, `operate`, `respond`

### OBS-01: Structured application logging

- **What can go wrong:** Free-form logs omit context, change shape, over-log internals, or make high-volume events impossible to query.
- **Enterprise hardening intent:** Use structured schemas, stable event names, severity rules, timestamps, service and version identity, and bounded fields.
- **Expected evidence:** Log schema; lint or contract test; sample query.

### OBS-02: Metrics for service, resource, and business outcomes

- **What can go wrong:** Only CPU and uptime are measured; correctness, queue age, tenant impact, saturation, and critical business outcomes are invisible.
- **Enterprise hardening intent:** Instrument request, error, duration, saturation, dependency, queue, data quality, and business outcome metrics tied to SLOs.
- **Expected evidence:** Metric catalog; dashboard; SLO mapping.

### OBS-03: Distributed tracing and context propagation

- **What can go wrong:** Cross-service latency and failure cannot be attributed; correlation IDs break at async boundaries or include untrusted values.
- **Enterprise hardening intent:** Propagate validated trace context through synchronous and asynchronous work; record spans for material dependencies and state transitions.
- **Expected evidence:** Trace sample; propagation tests; sampling policy.

### OBS-04: Profiles and safe diagnostics

- **What can go wrong:** CPU, memory, lock, and allocation problems require production guessing; debug tooling is either absent or dangerously exposed.
- **Enterprise hardening intent:** Provide access-controlled, rate-limited profiling and diagnostics with privacy review and automatic expiration.
- **Expected evidence:** Profile capability; access policy; diagnostic drill.

### OBS-05: Correlation, deployment, and configuration markers

- **What can go wrong:** Responders cannot tell which version, flag, configuration, tenant, region, or dependency change coincided with failure.
- **Enterprise hardening intent:** Attach stable correlation and change metadata to telemetry; record deploy and config events on relevant dashboards and traces.
- **Expected evidence:** Release markers; active-version telemetry; correlation query.

### OBS-06: User-centered SLO dashboards

- **What can go wrong:** Dashboards show green averages while a region, tenant, percentile, or critical journey is failing.
- **Enterprise hardening intent:** Visualize SLOs and error budgets by meaningful dimensions without unbounded cardinality; include correctness and freshness where relevant.
- **Expected evidence:** SLO dashboard; slice review; error-budget report.

### OBS-07: Actionable alerting and routing

- **What can go wrong:** Alerts are noisy, unactionable, routed to nobody, based on causes rather than user impact, or ignored like routine email.
- **Enterprise hardening intent:** Alert on material symptoms and fast-burn risk; include owner, severity, context, runbook, and escalation; test routing and response.
- **Expected evidence:** Alert catalog; paging test; precision and response metrics.

### OBS-08: Audit and security event integrity

- **What can go wrong:** Privileged, identity, data, policy, and financial actions are not recorded or can be altered by the actor being audited.
- **Enterprise hardening intent:** Record who, what, when, where, target, outcome, and reason; protect logs from tampering; monitor gaps and high-risk patterns.
- **Expected evidence:** Audit schema; immutability controls; sample investigation.

### OBS-09: Redaction, privacy, access, and retention

- **What can go wrong:** Telemetry exposes secrets and personal data to broad teams or vendors; deletion and retention rules are absent.
- **Enterprise hardening intent:** Use allowlisted fields and redaction before emission; separate sensitive streams; enforce least access, retention, deletion, and export policy.
- **Expected evidence:** Redaction tests; access review; retention configuration.

### OBS-10: Health, readiness, synthetics, and dependency checks

- **What can go wrong:** Health endpoints return success when critical journeys fail, or deep checks overload dependencies and trigger restarts.
- **Enterprise hardening intent:** Separate liveness, readiness, startup, and synthetic journey checks; make dependencies and degraded states explicit and bounded.
- **Expected evidence:** Probe contract; synthetic tests; false-positive review.

### OBS-11: Cardinality, sampling, volume, and cost

- **What can go wrong:** User IDs, URLs, error strings, and dynamic labels explode telemetry cost and destabilize the monitoring system; sampling hides rare critical events.
- **Enterprise hardening intent:** Define label and event budgets; aggregate or hash carefully; sample by policy while retaining errors and audit events; monitor telemetry itself.
- **Expected evidence:** Cardinality budget; sampling policy; cost and drop metrics.

### OBS-12: Independent and out-of-band observability

- **What can go wrong:** The same outage disables dashboards, logs, status pages, identity, and communications needed to recover.
- **Enterprise hardening intent:** Identify critical monitoring and communication dependencies; maintain isolated or external paths and local evidence for major failure modes.
- **Expected evidence:** Observability dependency map; out-of-band drill; status-path test.


## 4.20 DPL: Deployment, Release and Change Management

**Failure question:** Could a correct change become an incident because it is too large, inconsistently deployed, globally propagated, incompatible, or hard to stop?

**Typical accountable owners:** Release manager, Engineering lead, SRE, Product owner, Security

**Primary failure primitives:** `UNSAFE_CHANGE`, `UNAVAILABLE`, `INCOMPATIBLE`, `IRRECOVERABLE`, `TAMPERING`

**Lifecycle phases:** `plan`, `release`, `observe`, `rollback`

### DPL-01: Small, reviewable, reversible changes

- **What can go wrong:** Large mixed-purpose releases hide risk, make review superficial, and make rollback discard unrelated value.
- **Enterprise hardening intent:** Prefer cohesive small changes with clear description, risk, tests, and rollback; separate refactors, behavior changes, migrations, and generated churn.
- **Expected evidence:** Change description; size and scope review; rollback unit.

### DPL-02: Preflight and release readiness

- **What can go wrong:** Release begins with missing approvals, capacity, backups, observability, support coverage, flags, or rollback artifacts.
- **Enterprise hardening intent:** Use a profile-driven preflight that validates artifacts, dependencies, data changes, capacity, monitoring, communication, and rollback prerequisites.
- **Expected evidence:** Readiness checklist; signed release manifest; open-risk review.

### DPL-03: Environment promotion and build-once deployment

- **What can go wrong:** Code is rebuilt or re-resolved per environment; staging success does not prove the production artifact.
- **Enterprise hardening intent:** Promote the same signed digest through environments; externalize configuration; verify policy and environment compatibility at each gate.
- **Expected evidence:** Digest chain; promotion log; environment verification.

### DPL-04: Progressive delivery, canary, and blue-green

- **What can go wrong:** A latent bug reaches every user, region, machine, or tenant before health can be assessed.
- **Enterprise hardening intent:** Roll out by bounded cohorts or cells; compare canary to control on SLO and business signals; pause between stages; protect vulnerable cohorts.
- **Expected evidence:** Rollout plan; canary analysis; cohort and stage log.

### DPL-05: Automatic stop, rollback, and roll-forward

- **What can go wrong:** Deployment continues despite worsening signals; rollback is manual, slow, or impossible because data and configuration moved forward.
- **Enterprise hardening intent:** Define halt and rollback thresholds; automate safe stop; retain prior artifacts and configuration; choose tested rollback or roll-forward for stateful changes.
- **Expected evidence:** Abort policy; rollback drill; recovery timing; artifact retention.

### DPL-06: Fleet convergence and mixed-version behavior

- **What can go wrong:** Some nodes miss the deployment, run stale config, or cannot interoperate with new schema and protocol versions.
- **Enterprise hardening intent:** Measure active versions and configuration; set convergence SLO; test N and N-1 interoperability; quarantine or repair stragglers.
- **Expected evidence:** Fleet inventory; convergence dashboard; version-skew tests.

### DPL-07: Code, schema, data, configuration, and model coordination

- **What can go wrong:** Non-code artifacts bypass release rigor or change in an order that breaks compatibility and rollback.
- **Enterprise hardening intent:** Treat every behavior-changing artifact as a release; define dependency order, version contract, validation, canary, and rollback for each.
- **Expected evidence:** Multi-artifact release plan; version manifest; coordinated test.

### DPL-08: Feature flags and kill switches

- **What can go wrong:** A flag cannot be changed during failure, is unauthorized, or disables only UI while backend side effects continue.
- **Enterprise hardening intent:** Provide secure server-side evaluation, scoped and audited changes, tested kill behavior, default-safe state, and independent access path.
- **Expected evidence:** Kill-switch test; flag access audit; default behavior evidence.

### DPL-09: Approvals, change windows, and segregation

- **What can go wrong:** High-risk changes deploy without accountable review, during unsupported periods, or by the same identity that authored them.
- **Enterprise hardening intent:** Set approval and window requirements by risk; use protected environments and emergency procedures; preserve evidence without creating ceremonial review.
- **Expected evidence:** Approval log; environment rules; change calendar; emergency record.

### DPL-10: Post-deploy verification

- **What can go wrong:** Pipeline success is mistaken for service success; silent correctness and tenant-specific failures persist.
- **Enterprise hardening intent:** Verify critical journeys, SLOs, data, logs, queues, version, configuration, and business outcomes immediately and through a defined observation window.
- **Expected evidence:** Release verification report; observation-window result.

### DPL-11: Emergency and hotfix process

- **What can go wrong:** Incidents justify skipping testing, review, traceability, or later cleanup; emergency access becomes a normal path.
- **Enterprise hardening intent:** Define minimum non-bypassable safety checks, incident authority, rapid review, rollback, documentation, and mandatory follow-up.
- **Expected evidence:** Hotfix record; emergency approvals; retrospective test and cleanup.

### DPL-12: Release notes and stakeholder communication

- **What can go wrong:** Operators, customers, support, and downstream consumers do not know behavior, migration, risk, deprecation, or rollback implications.
- **Enterprise hardening intent:** Publish audience-specific notes with impact, actions, compatibility, known issues, support path, and status communication.
- **Expected evidence:** Release notes; consumer acknowledgment; support briefing.


## 4.21 OPS: Operations, Incident Response, Backup and Recovery

**Failure question:** Could the service be impossible to support, contain, restore, reconcile, communicate about, or retire when something goes wrong?

**Typical accountable owners:** Service owner, SRE/Operations, Security incident response, Data owner, Business continuity

**Primary failure primitives:** `UNAVAILABLE`, `IRRECOVERABLE`, `UNOBSERVABLE`, `UNSAFE_CHANGE`, `NONCOMPLIANT`

**Lifecycle phases:** `operate`, `respond`, `recover`, `learn`, `retire`

### OPS-01: Service ownership, on-call, and support model

- **What can go wrong:** No staffed owner receives alerts or understands the system; support hours and escalation do not match business criticality.
- **Enterprise hardening intent:** Define operational ownership, coverage, escalation, handoffs, support SLOs, and dependency contacts; test contact paths.
- **Expected evidence:** Service catalog; on-call roster; escalation test; support agreement.

### OPS-02: Runbooks and operational playbooks

- **What can go wrong:** Responders improvise destructive commands under stress; procedures omit prerequisites, validation, rollback, and expected outputs.
- **Enterprise hardening intent:** Write task-oriented runbooks with guardrails, dry-run, bounded scope, verification, rollback, and links to dashboards; exercise them.
- **Expected evidence:** Runbook; execution drill; command safeguards; review date.

### OPS-03: Incident command, roles, and coordination

- **What can go wrong:** Everyone debugs; no one owns mitigation, communication, decisions, or timeline; parallel actions conflict.
- **Enterprise hardening intent:** Use defined incident roles, severity, command channel, action log, decision authority, handoffs, and escalation.
- **Expected evidence:** Incident process; role roster; exercise or incident record.

### OPS-04: Detection, triage, mitigation, and communication

- **What can go wrong:** Teams chase root cause before reducing impact, misdiagnose internal failure as attack, or leave customers uninformed.
- **Enterprise hardening intent:** Prioritize user impact and safe generic mitigation; establish hypotheses and evidence; communicate status and uncertainty on a cadence.
- **Expected evidence:** Triage checklist; mitigation options; communication templates; timeline.

### OPS-05: Backup strategy, immutability, and separation

- **What can go wrong:** Backups replicate deletion or ransomware, share credentials and location with production, or exclude configuration and documentation.
- **Enterprise hardening intent:** Back up data, state, configuration, and recovery metadata to separated, protected, versioned storage; monitor success and integrity.
- **Expected evidence:** Backup architecture; immutable settings; job and integrity reports.

### OPS-06: Restore testing, RTO, and RPO

- **What can go wrong:** Backup jobs are green but data cannot be restored, credentials are unavailable, dependencies are missing, or objectives are missed.
- **Enterprise hardening intent:** Define RTO and RPO by service and data; conduct full and selective restore tests; measure, reconcile, and remediate gaps.
- **Expected evidence:** Restore drill; measured RTO/RPO; data verification; action plan.

### OPS-07: Disaster recovery and business continuity

- **What can go wrong:** Regional, provider, identity, network, staffing, or control-plane loss has no tested operating alternative.
- **Enterprise hardening intent:** Plan prioritized recovery order, alternate capacity, dependencies, manual workarounds, communications, and return to normal; run game days.
- **Expected evidence:** DR plan; dependency and recovery sequence; exercise result.

### OPS-08: Vulnerability, patch, certificate, and lifecycle operations

- **What can go wrong:** Patches, certificates, domains, keys, agents, and runtimes expire or remain vulnerable because operational ownership is unclear.
- **Enterprise hardening intent:** Maintain inventory and renewal or patch SLOs; automate safely; stage critical updates; monitor expiry and unsupported versions.
- **Expected evidence:** Patch dashboard; certificate test; lifecycle and EOL report.

### OPS-09: Postmortems and corrective-action closure

- **What can go wrong:** Reviews blame individuals, stop at proximate cause, or produce action items without owner, priority, due date, and verification.
- **Enterprise hardening intent:** Run blameless, evidence-based analysis of technical and organizational contributors; prioritize systemic controls and track completion and effectiveness.
- **Expected evidence:** Postmortem; action register; effectiveness review; recurring-pattern analysis.

### OPS-10: Data correction and reconciliation

- **What can go wrong:** After failure, teams restore service but leave duplicate, missing, stale, or unauthorized side effects in data and downstream systems.
- **Enterprise hardening intent:** Define correction, replay, compensation, notification, and reconciliation procedures with business owner approval and audit.
- **Expected evidence:** Correction runbook; reconciliation report; customer-impact record.

### OPS-11: Operational access and degraded operation

- **What can go wrong:** Responders cannot access systems during identity or network failure, or broad emergency access creates new risk.
- **Enterprise hardening intent:** Provide least-privileged normal access and controlled, tested break-glass paths; define safe degraded and manual modes.
- **Expected evidence:** Access drill; break-glass audit; degraded-mode procedure.

### OPS-12: Decommissioning and secure disposal

- **What can go wrong:** Retired endpoints, domains, buckets, queues, credentials, data, integrations, and alerts remain active or are abandoned to takeover.
- **Enterprise hardening intent:** Inventory and remove traffic, identities, secrets, data, DNS, certificates, dependencies, monitors, backups, and contracts; verify disposal.
- **Expected evidence:** Retirement checklist; deletion evidence; takeover scan; owner sign-off.

### OPS-13: Capacity, maintenance, and toil

- **What can go wrong:** Manual repetitive work and deferred maintenance consume responders and increase error; capacity work begins only during incidents.
- **Enterprise hardening intent:** Measure and reduce toil; schedule maintenance; forecast demand; automate with guardrails; preserve engineering time for reliability.
- **Expected evidence:** Toil and maintenance backlog; capacity review; automation safety evidence.

### OPS-14: Forensic readiness and evidence preservation

- **What can go wrong:** Logs roll over, clocks disagree, volatile data disappears, or responders alter evidence before scope and cause are understood.
- **Enterprise hardening intent:** Define evidence sources, retention, time synchronization, legal and privacy handling, chain of custody, and collection procedures.
- **Expected evidence:** Forensic plan; retention and clock checks; collection exercise.


## 4.22 UXA: User Experience, Accessibility, Internationalization and Compatibility

**Failure question:** Could users fail, be excluded, misunderstand risk, lose work, or make unsafe choices because the interface and supported environments are weak?

**Typical accountable owners:** Product/UX, Accessibility owner, Engineering, QA, Support

**Primary failure primitives:** `INCORRECT`, `INCOMPATIBLE`, `NONCOMPLIANT`, `ABUSE`, `DISCLOSURE`

**Lifecycle phases:** `design`, `build`, `verify`, `operate`

### UXA-01: Usability and task completion

- **What can go wrong:** The interface mirrors internal architecture, requires hidden knowledge, or makes common tasks slow and error-prone.
- **Enterprise hardening intent:** Design around user goals and mental models; test representative journeys, learnability, efficiency, and recovery.
- **Expected evidence:** Journey map; usability test; task success measures.

### UXA-02: Accessibility baseline

- **What can go wrong:** Features are unusable for people with visual, auditory, motor, cognitive, or speech disabilities; conformance is assumed from automated scans.
- **Enterprise hardening intent:** Target the applicable WCAG level; include accessibility in design, implementation, automated checks, manual review, and acceptance.
- **Expected evidence:** Accessibility statement; conformance report; issue register.

### UXA-03: Keyboard, focus, semantics, and screen readers

- **What can go wrong:** Interactive elements lack semantics, focus order, labels, announcements, or keyboard access; custom widgets trap users.
- **Enterprise hardening intent:** Use native elements first; implement correct roles, names, states, focus management, shortcuts, and live-region behavior.
- **Expected evidence:** Keyboard walkthrough; screen-reader test; semantic scan.

### UXA-04: Visual contrast, zoom, motion, and media

- **What can go wrong:** Low contrast, color-only meaning, fixed layouts, flashing, motion, or uncaptioned media excludes users or causes harm.
- **Enterprise hardening intent:** Meet contrast and resize targets; provide non-color cues, reduced-motion support, captions, transcripts, and safe media controls.
- **Expected evidence:** Visual audit; zoom and motion tests; media accessibility record.

### UXA-05: Error prevention, recovery, and preservation

- **What can go wrong:** Validation appears late; errors are vague; users lose entered work; retry creates duplicate effects.
- **Enterprise hardening intent:** Validate at useful points without relying on client checks; identify field and system errors; preserve input; support safe retry and undo.
- **Expected evidence:** Error-message review; interrupted-flow test; duplicate-action test.

### UXA-06: Destructive and sensitive actions

- **What can go wrong:** One click deletes, pays, publishes, grants access, or exposes data; confirmations are generic and habituating.
- **Enterprise hardening intent:** Use consequence-specific confirmation, reauthentication, preview, cooling-off, approval, undo, or delayed execution based on impact.
- **Expected evidence:** Sensitive-action matrix; confirmation and undo tests.

### UXA-07: Internationalization, localization, and right-to-left support

- **What can go wrong:** Strings are embedded; layouts break; sorting and pluralization are wrong; untranslated text leaks into critical flows.
- **Enterprise hardening intent:** Externalize content; use locale-aware formatting and collation; test expansion, plural rules, RTL layout, and fallback language.
- **Expected evidence:** Locale test matrix; pseudo-localization; translation coverage.

### UXA-08: Dates, numbers, currency, units, and time zones

- **What can go wrong:** Users misread ambiguous dates, decimal separators, currencies, units, or scheduling zones and take wrong action.
- **Enterprise hardening intent:** Display unambiguous locale-aware values with currency and units; preserve source zone and show conversion context.
- **Expected evidence:** Formatting specification; locale and timezone boundary tests.

### UXA-09: Device, browser, client, and API compatibility

- **What can go wrong:** Supported clients differ from tested clients; progressive enhancement, offline behavior, and older versions fail unpredictably.
- **Enterprise hardening intent:** Publish support matrix; test representative versions and assistive technologies; use capability detection and graceful fallback.
- **Expected evidence:** Support matrix; compatibility suite; deprecation telemetry.

### UXA-10: Responsive, offline, and constrained-network behavior

- **What can go wrong:** Small screens, intermittent connections, high latency, and low bandwidth cause data loss, duplicate actions, or inaccessible controls.
- **Enterprise hardening intent:** Design responsive layouts, resumable operations, local state protection, explicit sync, bounded assets, and offline or degraded messaging.
- **Expected evidence:** Device and network tests; resume and sync tests; asset budget.

### UXA-11: Privacy, security cues, and dark-pattern avoidance

- **What can go wrong:** Consent, permissions, defaults, urgency, or visual hierarchy manipulates users into sharing more or taking risk.
- **Enterprise hardening intent:** Use truthful language and balanced choices; explain permissions and consequences; make privacy-preserving actions as easy as permissive ones.
- **Expected evidence:** UX ethics and privacy review; consent-flow test; copy approval.

### UXA-12: Help, support, status, and transparency

- **What can go wrong:** Users cannot diagnose service state, find help, understand changes, or distinguish their error from a system incident.
- **Enterprise hardening intent:** Provide contextual help, accessible support, status, error references, change notices, and clear ownership of user-impact communications.
- **Expected evidence:** Help-content review; support analytics; status integration test.


## 4.23 KNO: Documentation, Knowledge and Supportability

**Failure question:** Could the system depend on vanished chat context or a few individuals because its architecture, operation, decisions, and contracts are not durable?

**Typical accountable owners:** Engineering lead, Service owner, Technical writer, Architect, Support

**Primary failure primitives:** `OMISSION`, `INCONSISTENT`, `UNMAINTAINABLE`, `UNSAFE_CHANGE`

**Lifecycle phases:** `plan`, `build`, `operate`, `learn`

### KNO-01: Repository README and onboarding

- **What can go wrong:** A new engineer or agent cannot build, test, run, understand, or safely modify the repository without oral history.
- **Enterprise hardening intent:** Document purpose, architecture entry points, prerequisites, setup, commands, test strategy, security cautions, and contribution path.
- **Expected evidence:** README verification; clean-environment onboarding test.

### KNO-02: Architecture and context documentation

- **What can go wrong:** Diagrams omit dependencies, trust zones, data stores, deployment, or failure domains and become decorative or stale.
- **Enterprise hardening intent:** Maintain lightweight current context, container or component, data-flow, deployment, and dependency views linked to owners and ADRs.
- **Expected evidence:** Diagrams; automated link or freshness check; review date.

### KNO-03: Architecture decision records

- **What can go wrong:** Why a constraint, vendor, pattern, or compromise exists is lost; future changes repeat rejected options or break assumptions.
- **Enterprise hardening intent:** Record significant decisions, context, alternatives, consequences, status, and supersession; link to implementation and risks.
- **Expected evidence:** ADR set; decision index; supersession links.

### KNO-04: API, schema, event, and contract documentation

- **What can go wrong:** Consumers reverse-engineer behavior; examples contradict schemas; errors, limits, auth, and compatibility are undocumented.
- **Enterprise hardening intent:** Generate reference from canonical contracts where possible; add human examples, limits, lifecycle, failure, and migration guidance.
- **Expected evidence:** Published contract; example tests; docs-to-schema diff.

### KNO-05: Code comments and docstrings

- **What can go wrong:** Comments restate syntax, preserve obsolete behavior, or omit non-obvious invariants and security assumptions.
- **Enterprise hardening intent:** Document intent, constraints, side effects, invariants, units, error and concurrency semantics; keep public interfaces discoverable.
- **Expected evidence:** Documentation lint; review; stale-comment check.

### KNO-06: Runbooks and operational knowledge

- **What can go wrong:** Documentation explains architecture but not how to diagnose, mitigate, restore, rotate, scale, or recover under pressure.
- **Enterprise hardening intent:** Maintain symptom-oriented runbooks with commands, safeguards, expected results, rollback, escalation, and verification.
- **Expected evidence:** Runbook drill; review date; linked alert and dashboard.

### KNO-07: Configuration and deployment documentation

- **What can go wrong:** Operators cannot determine effective settings, precedence, safe values, rollout order, or rollback limits.
- **Enterprise hardening intent:** Document config schema, defaults, sensitivity, environment differences, deployment sequence, compatibility, and recovery.
- **Expected evidence:** Config reference; deployment guide; tested examples.

### KNO-08: Ownership, support, and escalation documentation

- **What can go wrong:** Files, services, dashboards, data, dependencies, and customer commitments have no discoverable owner.
- **Enterprise hardening intent:** Publish ownership and support boundaries with contacts, coverage, dependency escalation, and backup owners; automate from source where possible.
- **Expected evidence:** Ownership registry; escalation test; support matrix.

### KNO-09: Changelog, migration, and deprecation guidance

- **What can go wrong:** Consumers discover breaking changes after deployment; no path or date exists for moving from old behavior.
- **Enterprise hardening intent:** Maintain user-oriented changelog, upgrade and rollback instructions, deprecation dates, telemetry, and removal criteria.
- **Expected evidence:** Changelog; migration guide; deprecation notices and usage data.

### KNO-10: Examples, tutorials, and reference implementations

- **What can go wrong:** Examples use insecure shortcuts, obsolete APIs, hard-coded secrets, or toy assumptions that users copy into production.
- **Enterprise hardening intent:** Test examples in CI; make safe behavior the easiest path; label non-production simplifications; version examples with the product.
- **Expected evidence:** Example tests; security review; version and deprecation check.

### KNO-11: Documentation freshness and generation

- **What can go wrong:** Generated docs are not regenerated; manual docs drift from code; broken links and commands erode trust.
- **Enterprise hardening intent:** Define canonical sources; generate and diff where useful; execute documented commands and examples; require review dates for manual claims.
- **Expected evidence:** Docs CI; link and command tests; freshness dashboard.

### KNO-12: Knowledge concentration and training

- **What can go wrong:** Only one person understands deployment, data repair, security, or recovery; absence turns routine incidents into crises.
- **Enterprise hardening intent:** Track critical knowledge and bus factor; rotate on-call and reviews; pair on high-risk changes; conduct drills and onboarding.
- **Expected evidence:** Knowledge-risk register; training and drill records; backup-owner coverage.

### KNO-13: Developer experience and local reproducibility

- **What can go wrong:** A new developer cannot run the project locally without help from the person who built it; setup requires undocumented environment variables, manual database seeding steps, or OS-specific incantations; the local environment diverges silently from production because dependencies are not pinned or the runtime is not containerized; there is no fast feedback loop (tests take minutes, no hot reload); the only working dev environment belongs to one person. AI-generated projects fail this consistently because the AI builds for the happy path, not for the second developer.
- **Enterprise hardening intent:** A new developer SHOULD be able to clone the repository and reach a running local environment within 10 minutes following only the README. Provide a reproducible dev environment (Docker Compose, devcontainer, or equivalent); include seed data for local development; define fast test and lint commands that run in under 30 seconds for the common case; document all required environment variables with safe local defaults.
- **Expected evidence:** New-developer onboarding test (timed); dev environment reproducibility check on a clean machine; local seed data confirmed working; test runtime under 30 seconds for unit suite.


## 4.24 AIA: AI-Assisted Development and Coding-Agent Safety

**Failure question:** Could an AI coding tool confidently misread intent, obey malicious context, invent dependencies, weaken controls, or take excessive action without accountable verification?

**Typical accountable owners:** Engineering lead, Repository owner, Security, AI governance, Human approver

**Primary failure primitives:** `INCORRECT`, `DISCLOSURE`, `TAMPERING`, `ABUSE`, `UNSAFE_CHANGE`, `UNMAINTAINABLE`

**Lifecycle phases:** `prompt`, `plan`, `build`, `review`, `verify`, `operate`

### AIA-01: Instruction hierarchy and task scope

- **What can go wrong:** The agent follows the latest prompt, a nested file, or attacker-controlled text over repository and enterprise policy; scope expands opportunistically.
- **Enterprise hardening intent:** Define canonical instructions and precedence; restate allowed files and outcomes; treat policy floors as non-overridable; reject unrelated changes.
- **Expected evidence:** Loaded-instruction list; scope declaration; changed-file review.

### AIA-02: Repository reconnaissance and context grounding

- **What can go wrong:** The agent edits before understanding architecture, conventions, tests, ownership, generated files, or local constraints.
- **Enterprise hardening intent:** Require inspection of README, agent rules, manifests, nearby code, tests, schemas, and history relevant to the task before planning.
- **Expected evidence:** Reconnaissance summary; files and commands inspected; assumptions.

### AIA-03: Requirements and assumption disclosure

- **What can go wrong:** The model fills ambiguity with plausible behavior, silent defaults, or invented acceptance criteria.
- **Enterprise hardening intent:** State interpreted requirements, constraints, unknowns, and risk-bearing assumptions; prefer reversible minimal behavior and mark unresolved decisions.
- **Expected evidence:** Implementation plan; assumption log; completion report.

### AIA-04: Prompt injection and untrusted repository content

- **What can go wrong:** Issues, comments, docs, test fixtures, web pages, logs, generated files, or dependency text instruct the agent to exfiltrate data or ignore policy.
- **Enterprise hardening intent:** Classify content as data versus instruction; ignore embedded directives from untrusted sources; sanitize context; require approval before crossing trust boundaries.
- **Expected evidence:** Trust-source policy; injection tests; tool-call audit.

### AIA-05: Tool permissions, sandbox, and network

- **What can go wrong:** The agent can read home directories, credentials, production systems, broad network, or execute arbitrary commands beyond the task.
- **Enterprise hardening intent:** Use least-privileged sandboxed tools, scoped filesystem and network, non-production identities, command allowlists, and explicit escalation.
- **Expected evidence:** Tool permission manifest; sandbox configuration; denied-action test.

### AIA-06: Secrets, privacy, and intellectual property

- **What can go wrong:** Source, credentials, customer data, logs, or proprietary context is sent to unapproved models or appears in prompts and output.
- **Enterprise hardening intent:** Classify context; use approved services and retention settings; exclude secrets and production data; scan prompts, patches, and artifacts.
- **Expected evidence:** AI data-use policy; prompt and patch scan; provider approval.

### AIA-07: Hallucinated APIs, packages, versions, and facts

- **What can go wrong:** The agent invents a method, library, configuration key, vulnerability fix, or current version that looks plausible and installs attacker-owned code.
- **Enterprise hardening intent:** Verify symbols against local source or authoritative docs; verify every dependency in an approved registry; never auto-install an unverified name.
- **Expected evidence:** Verification links or local references; dependency approval; compile and contract tests.

### AIA-08: Generated-code security and quality review

- **What can go wrong:** Syntactically correct code contains known weaknesses, weak error handling, poor maintainability, or hidden nonfunctional regressions.
- **Enterprise hardening intent:** Apply the same or stronger review, static analysis, tests, threat model, and ownership as human code; increase scrutiny for sensitive and unfamiliar code.
- **Expected evidence:** Human review; SAST and tests; risk-based assurance record.

### AIA-09: Gate tampering and reward hacking

- **What can go wrong:** The agent disables tests, loosens assertions, adds exclusions, suppresses warnings, hard-codes fixtures, or changes expected output merely to obtain green status.
- **Enterprise hardening intent:** Prohibit weakening verification without independent rationale and approval; diff tests and policies separately; detect new skips, suppressions, and coverage drops.
- **Expected evidence:** Gate-diff report; suppression audit; independent review.

### AIA-10: Destructive and privileged actions

- **What can go wrong:** The agent deletes data, rewrites history, rotates secrets, changes IAM, runs migrations, deploys, or modifies production because it appears necessary. Agents that send communications, create records in external systems, or take actions with real-world consequence on behalf of users operate autonomously without review, making the platform the accountable actor for harm.
- **Enterprise hardening intent:** Require human approval and backups for destructive, irreversible, privileged, production, identity, cryptographic, and legal-impact actions; default to dry-run. For agents operating autonomously on a schedule, human approval MUST be required by default before any irreversible external-world action (sending communications, creating records in third-party systems, making purchases). Suppression of approval MUST be an explicit per-action opt-out with a recorded user acknowledgment. Per-execution budgets for tokens, wall-clock time, and external API calls MUST be defined and enforced to prevent runaway agents from consuming unbounded external resources or incurring unbounded cost.
- **Expected evidence:** Approval record; dry-run output; backup or rollback evidence; human-review gate test for external-world actions; per-agent cost and execution budget configuration; budget enforcement test.

### AIA-11: Minimal change and architectural drift

- **What can go wrong:** The agent refactors broadly, introduces a new framework, duplicates abstractions, or changes patterns beyond the requested outcome.
- **Enterprise hardening intent:** Prefer the smallest coherent patch; reuse existing patterns; justify new dependencies and architecture; separate opportunistic cleanup.
- **Expected evidence:** Diff-size and scope review; ADR for material change; dependency delta.

### AIA-12: Provenance, attribution, and session record

- **What can go wrong:** No one can determine which model, instructions, tools, sources, or approvals produced a consequential patch.
- **Enterprise hardening intent:** Record tool and model identity where policy requires, loaded instructions, significant external sources, commands, test evidence, and human approvals.
- **Expected evidence:** Generation record; source attribution; signed completion report.

### AIA-13: Model and tool drift, reproducibility

- **What can go wrong:** The same prompt produces materially different code after model, tool, retrieval, or policy updates; old guidance silently becomes obsolete.
- **Enterprise hardening intent:** Version critical agent configurations and prompts; pin toolchain where practical; retain tests and expected behavior; revalidate on material upgrades.
- **Expected evidence:** Agent configuration version; regression suite; upgrade assessment.

### AIA-14: Loops, token, compute, and cost budgets

- **What can go wrong:** Autonomous retries, searches, builds, or agent chains consume unbounded time, API spend, CI minutes, or external quotas.
- **Enterprise hardening intent:** Set iteration, command, network, compute, and cost budgets; stop on repeated failure; summarize blockers rather than looping.
- **Expected evidence:** Budget configuration; loop detector; usage report.

### AIA-15: Multi-agent conflict and memory poisoning

- **What can go wrong:** Agents overwrite each other, trust stale shared memory, amplify false assumptions, or create incompatible changes.
- **Enterprise hardening intent:** Partition ownership; use reviewed shared state; validate handoffs; serialize conflicting edits; treat memory and summaries as untrusted until checked.
- **Expected evidence:** Handoff record; conflict detection; shared-context provenance.

### AIA-16: Completion evidence, uncertainty, and stop conditions

- **What can go wrong:** The agent declares success after editing without running relevant checks, or hides unverified behavior behind confident prose.
- **Enterprise hardening intent:** Require a completion report listing changes, requirements met, tests run and not run, security and compatibility impact, residual risks, and explicit uncertainty.
- **Expected evidence:** Completion report; command outputs; unresolved-risk list; reviewer decision.

### AIA-17: LLM output quality and safety in applications

- **What can go wrong:** An application that calls an LLM renders model output directly to users without validation; hallucinated facts, fabricated citations, or made-up product information are presented as authoritative; LLM output rendered as HTML is an XSS vector; model responses are used to drive application logic (database queries, API calls, decisions) without schema validation or safety checks; token budgets are not set per request, allowing runaway costs; prompts are not versioned, so model behavior changes invisibly when the provider updates; the application treats model confidence as a measure of correctness.
- **Enterprise hardening intent:** Validate and sanitize LLM output before rendering or using it in logic: treat model output as untrusted user input, not authoritative data; never render model output as raw HTML; validate structured outputs (JSON, code, decisions) against a schema before acting on them; set token limits per request; version and test prompts as code; monitor output quality metrics (refusals, format failures, user corrections) in production; define what the application does when the model returns an unexpected or low-confidence response; do not use model confidence, explanation, or self-assessment as sole evidence of correctness.
- **Expected evidence:** Output sanitization test; structured-output schema validation; token budget configuration; prompt version control; output quality monitoring; fallback behavior for unexpected model responses.


## 5. Coverage self-assessment

Evaluate every domain and each applicable subcategory. Do not use an empty cell to mean “probably fine.”

| Taxonomy ID | Status | Criticality | Tier of highest gap | Owner | Evidence | Exception | Target date |
|---|---|---|---|---|---|---|---|
| `REL-03` | `PARTIAL` | `C2` | `T1` | Platform Reliability | test report / dashboard |: | YYYY-MM-DD |

Allowed status values:

- `IMPLEMENTED`
- `PARTIAL`
- `GAP`
- `NOT_APPLICABLE`
- `ACCEPTED_RISK`
- `NOT_VERIFIED`

### 5.1 Evidence questions

For every `IMPLEMENTED` claim, ask:

1. What exact requirement is being met?
2. Which versions, environments, paths, tenants, and actors are in scope?
3. What artifact proves it?
4. Can the artifact be reproduced or independently inspected?
5. Does the evidence test semantic behavior or merely code shape?
6. Is the control active in production, or only in source?
7. When does the evidence expire or become stale?
8. Who is alerted when the control stops operating?

### 5.2 `NOT_APPLICABLE` test

A taxonomy item is not applicable only when:

- Its triggering condition is demonstrably absent.
- Reasoning is specific to the declared scope.
- No indirect or future path activates it.
- An accountable reviewer accepts the rationale.
- The status is revisited after material changes.

“Not in the ticket,” “handled by the framework,” and “the AI did not touch that” are not applicability arguments.

## 6. Lifecycle completeness test

Before declaring the taxonomy review complete, confirm coverage for:

1. **Discover**: problem, actors, harm, obligations, assumptions.
2. **Design**: boundaries, data, trust, failure, scale, recovery.
3. **Build**: code, dependencies, configuration, infrastructure, artifacts.
4. **Verify**: tests, review, adversarial analysis, evidence.
5. **Release**: migration, compatibility, staged rollout, rollback.
6. **Operate**: SLOs, telemetry, alerts, access, support, cost.
7. **Respond**: containment, forensics, communication, recovery.
8. **Learn**: incident action items, control improvement, knowledge.
9. **Retire**: data disposition, access removal, dependency shutdown, archival.

A feature that has a build path but no failure, rollback, or retirement path is not lifecycle-complete.

## 7. What this taxonomy deliberately catches

The taxonomy is built to expose recurring “unknown knowns”:

- **Valid-but-dangerous input:** schemas pass, but size, combination, timing, or propagation causes failure.
- **Configuration as executable behavior:** a permission, flag, rule, model, or feature file can have code-sized blast radius.
- **Control-plane coupling:** the same outage disables the tools needed to diagnose or recover.
- **Untested recovery:** backups exist, but restore permissions, integrity, dependencies, or objectives fail.
- **Alert theater:** messages are emitted but lack owner, routing, urgency, context, or action.
- **Green-pipeline illusion:** checks pass while relevant paths, production configuration, or integration behavior remain unverified.
- **Supply-chain transitivity:** one mutable script, action, image, plugin, or build credential reaches many organizations.
- **AI authority confusion:** generated text is treated as a decision, retrieved content becomes an instruction, or confident syntax becomes evidence.
- **Human factors:** an authorized user can make a catastrophic mistake because scope, preview, confirmation, and recovery are weak.
- **Retirement debt:** old endpoints, identities, data, flags, keys, and dependencies survive beyond ownership.

## 8. Extension rules

Organizations may add domains or subcategories, but SHOULD:

- Preserve canonical IDs and meanings.
- Use an organization prefix for local additions, such as `ACME-SEC-01`.
- Map local additions to one or more canonical taxonomy IDs.
- Keep failure mode, hardening intent, and evidence fields.
- Publish aliases and migration notes when reorganizing.
- Avoid creating stack-specific taxonomy entries when a stack overlay control is sufficient.
