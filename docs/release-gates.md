# Enterprise Release Gates

Release gates convert controls and evidence into decisions. They are not ceremonial meetings and they are not synonymous with CI stages. A gate can be automated, human, or hybrid, but its decision MUST be attributable and evidence-based.

## 1. Gate statuses

| Status | Meaning |
|---|---|
| `READY` | Required controls pass, evidence is current, approvals are complete, and residual risk is within policy. |
| `REVIEW` | No unexcepted Tier 1 failure exists, but a material uncertainty, Tier 2 issue, manual decision, or conditional approval remains. |
| `BLOCKED` | An applicable Tier 1 control fails or is not verified; a mandatory approval is missing; an exception is invalid; or safe release/operation cannot be established. |

Rules:

- Any applicable, unexcepted `T1` `FAIL`, `ERROR`, `GAP`, or `NOT_VERIFIED` produces `BLOCKED`.
- A `T1` `ACCEPTED_RISK` is valid only if the control permits exceptions and the designated authority approved a current, narrow exception.
- Material `T2` failures produce at least `REVIEW`.
- A gate MUST NOT become `READY` by suppressing, skipping, relabeling, or reducing the scope of failed verification.
- The gate record MUST identify the evaluated source revision, artifacts, configuration, environment, and intended deployment scope.
- A downstream gate may discover new evidence and reverse an upstream decision.

## 2. Gate record

Each decision SHOULD be recorded as:

```yaml
gate_id: G4
system: example-service
change_id: PR-1842
source_revision: 9f2c...
artifact_digest: sha256:...
configuration_revision: cfg-781
criticality: C3
decision: REVIEW
decided_at: 2026-06-25T18:00:00Z
decided_by:
  - release-owner
evidence_manifest: EVD-MANIFEST-20260625-017
blocking_findings: []
review_findings:
  - SEC-08-002
exceptions:
  - EXC-2026-019
conditions:
  - Roll out to 1% canary before 10%.
  - Security owner reviews telemetry after 30 minutes.
expires_at: 2026-06-26T18:00:00Z
```

## 3. Lifecycle gates

## G0: Scope, ownership, and criticality

**Decision:** Is the work sufficiently understood, owned, and classified to begin safely?

### Required inputs

- Problem statement and intended outcome.
- In-scope and out-of-scope behavior.
- Accountable product/system owner.
- System and change criticality.
- Data classification and affected populations.
- Initial taxonomy selection.
- Constraints, dependencies, and known unknowns.

### `READY` criteria

- [ ] Acceptance criteria are testable.
- [ ] System boundary and affected components are identified.
- [ ] Criticality is approved at the required authority.
- [ ] Sensitive, regulated, financial, privileged, and safety-relevant impacts are identified.
- [ ] An accountable owner and implementation owner exist.
- [ ] High-risk decisions have assigned reviewers.
- [ ] No prohibited use or unresolvable obligation is apparent.

### Blockers

- No accountable owner.
- Unknown production/sensitive-data impact.
- Ambiguous request with materially different possible outcomes.
- Work requires authority the requester does not possess.
- The proposed purpose is unlawful, deceptive, unsafe, or contrary to policy.

### Primary domains

`GOV`, `REQ`, `PRV`, `SAF`, `AIA`

---

## G1: Requirements and design readiness

**Decision:** Does the proposed design address applicable correctness, abuse, failure, scale, privacy, safety, and operational concerns?

### Required inputs

- Functional and non-functional requirements.
- Context and data-flow diagrams where applicable.
- Domain invariants and authorization model.
- Threat, misuse, privacy, and hazard analysis proportional to criticality.
- API/data/schema contracts.
- Dependency and build-impact assessment.
- Failure-mode, capacity, observability, migration, and recovery design.
- Verification plan.
- Rollout and rollback concept.

### `READY` criteria

- [ ] Normal, edge, misuse, partial-failure, and recovery behavior is specified.
- [ ] Trust boundaries and authorization decisions are explicit.
- [ ] Data purpose, minimization, retention, and deletion are defined.
- [ ] Public and persistent contracts have compatibility plans.
- [ ] Remote dependencies have timeout, retry, and degraded-mode behavior.
- [ ] Resource and scale limits are defined.
- [ ] Dangerous actions have prevention and recovery controls.
- [ ] Verification can falsify the design's critical claims.
- [ ] The rollout constrains blast radius.
- [ ] Human approval points are identified.

### Blockers

- Security/privacy/safety model absent for a `C3` or `C4` change.
- Irreversible migration without validated recovery.
- Authorization semantics delegated to the client.
- Critical dependency or failure behavior unspecified.
- No viable method to verify a Tier 1 requirement.
- Design requires a globally instantaneous, unbounded rollout with no stop mechanism.

### Primary domains

`REQ`, `ARC`, `DOM`, `API`, `DAT`, `IAM`, `SEC`, `PRV`, `SAF`, `REL`, `PER`, `OBS`

---

## G2: Implementation complete

**Decision:** Is the implementation internally complete and ready for independent review?

### Required inputs

- Source and configuration changes.
- Tests and local verification.
- Schema/migration changes.
- Documentation and runbook updates.
- Dependency/artifact changes.
- Completed change plan and preliminary completion report.

### `READY` criteria

- [ ] Code follows approved design and repository conventions.
- [ ] No unresolved critical TODO, stub, bypass, debug mode, or placeholder control remains.
- [ ] Input, output, errors, authorization, and resource bounds are implemented.
- [ ] Tests cover semantic requirements and negative cases.
- [ ] Configuration schema and safe defaults are present.
- [ ] Dependencies are verified and inventoried.
- [ ] Migrations are restartable and compatible where required.
- [ ] Telemetry and audit behavior is implemented.
- [ ] Documentation matches behavior.
- [ ] The implementation owner has recorded known limitations.

### Blockers

- Compilation/build failure.
- Known secret or sensitive data in source/artifacts/logs.
- Missing server-side authorization on a protected action.
- Unbounded resource/retry behavior on a material path.
- Destructive behavior without guardrails or recovery.
- Test bypass or scanner suppression without approved rationale.
- Fabricated dependency/API or unresolved provenance.
- Generated code that the author/reviewer cannot explain.

### Primary domains

`COD`, `DOM`, `API`, `DAT`, `IAM`, `SEC`, `SUP`, `CFG`, `BLD`, `TST`, `OBS`, `KNO`, `AIA`

---

## G3: Merge readiness

**Decision:** Has the change been independently reviewed and verified sufficiently to enter the protected branch?

### Required inputs

- Peer/domain review.
- Automated CI evidence.
- Security/privacy/reliability review as triggered.
- Control evaluation results.
- Updated risk/exception records.
- Review of generated code and dependency changes.

### `READY` criteria

- [ ] Required reviewers are independent and authorized.
- [ ] Mandatory formatting, linting, type, build, test, and policy checks pass.
- [ ] Relevant security, dependency, secret, infrastructure, and license scans pass.
- [ ] Contract, migration, concurrency, and failure tests pass where applicable.
- [ ] Review comments and material findings are resolved.
- [ ] Diff is coherent and contains no unrelated or unexplained change.
- [ ] Branch protections and provenance requirements are met.
- [ ] Evidence is bound to the reviewed commit.

### Blockers

- Required check missing, skipped, indeterminate, or detached from the reviewed revision.
- Unreviewed generated file, binary, vendored code, workflow, or lockfile change.
- Privileged CI path can be influenced by untrusted code.
- Tier 1 finding without valid disposition.
- Reviewer lacks sufficient context to explain critical logic.
- Approval predates a material code change and was not renewed.

### Primary domains

`GOV`, `COD`, `SUP`, `BLD`, `TST`, `AIA`

---

## G4: Release readiness

**Decision:** Is the artifact, configuration, operational posture, and transition plan ready for controlled production release?

### Required inputs

- Immutable build artifact and provenance.
- Release notes and known issues.
- Production configuration and secret references.
- Migration/backfill plan.
- SLOs, dashboards, alerts, and audit signals.
- Capacity and performance evidence.
- Deployment, canary, stop, rollback, and recovery procedures.
- On-call and stakeholder readiness.
- Current exceptions.

### `READY` criteria

- [ ] Artifact is traceable to reviewed source and verified dependencies.
- [ ] Environment-specific configuration is validated.
- [ ] Secrets, certificates, keys, and permissions are provisioned safely.
- [ ] Capacity and performance meet budgets under representative load.
- [ ] Dashboards and actionable alerts exist before exposure.
- [ ] On-call owner can diagnose and mitigate likely failures.
- [ ] Migration order is compatible with old and new versions.
- [ ] Rollback/recovery is tested or otherwise evidenced at the required fidelity.
- [ ] Canary population and promotion thresholds are defined.
- [ ] Customer/support/legal communications are prepared when required.

### Blockers

- Mutable or unverifiable artifact.
- Production-only path not tested or reviewed.
- No monitoring for critical behavior.
- No safe rollback or recovery for a high-impact change.
- Unknown data migration duration or lock/resource impact.
- Alert routes to no staffed owner.
- Expired exception or unsupported runtime/dependency.

### Primary domains

`SUP`, `BLD`, `CFG`, `INF`, `REL`, `PER`, `OBS`, `DPL`, `OPS`, `KNO`

---

## G5: Production rollout control

**Decision:** Should exposure expand, pause, roll back, or remain at the current stage?

### Required inputs

- Deployment telemetry by version/cohort.
- User-centered SLI and business-correctness signals.
- Error, saturation, latency, retry, dependency, and audit signals.
- Migration/backfill health.
- Security/privacy/safety indicators.
- Incident and support reports.
- Predefined promotion and stop thresholds.

### `READY` criteria for promotion

- [ ] Deployment completed for the current cohort.
- [ ] Health and correctness remain within threshold for the observation window.
- [ ] No unexplained regression, tenant skew, or safety/security signal exists.
- [ ] Rollback remains viable.
- [ ] Data and schema state remain compatible.
- [ ] Required human checkpoint approved promotion.
- [ ] Evidence is segmented enough to avoid fleet averages hiding a bad cohort.

### Immediate stop/rollback conditions

- Breach of a Tier 1 safety, security, privacy, data-integrity, or availability threshold.
- Unknown or accelerating error pattern.
- Loss of observability or inability to distinguish versions.
- Rollback window at risk of closing.
- Migration corruption or irreconcilable divergence.
- Unexpected privilege, data, tenant, or financial behavior.
- Canary harm that could grow with exposure.

### Primary domains

`DPL`, `OBS`, `REL`, `PER`, `SEC`, `PRV`, `SAF`, `OPS`

---

## G6: Operational acceptance

**Decision:** Has the released change demonstrated stable operation and transferred into normal ownership?

### Required inputs

- Post-release observation.
- SLO and business-outcome evidence.
- Alert quality and incident/support review.
- Cost/capacity impact.
- Security/privacy/audit signals.
- Completed migrations and cleanup.
- Updated runbooks and ownership.
- Residual risk and follow-up actions.

### `READY` criteria

- [ ] Release has operated for the defined stabilization period.
- [ ] Error budget, performance, correctness, and cost remain acceptable.
- [ ] Alerts are actionable and routed; noisy or missing signals are corrected.
- [ ] No hidden backlog, data inconsistency, failed job, or stranded state remains.
- [ ] Temporary permissions, flags, compatibility paths, and debug telemetry have owners/expiry.
- [ ] Support and incident learning has been incorporated.
- [ ] The operational owner explicitly accepts the service/change.
- [ ] Follow-up actions have severity, owner, and due date.

### Blockers

- Release appears healthy only because monitoring is incomplete.
- Backfill or migration state is unverified.
- Temporary elevated access or fail-open behavior remains.
- Material support/incident pattern has no owner.
- Operational team cannot execute recovery.

### Primary domains

`OPS`, `OBS`, `REL`, `PER`, `CFG`, `KNO`, `GOV`

---

## G7: Retirement and disposition

**Decision:** Can the component, endpoint, data set, model, dependency, identity, feature flag, or system be safely retired?

### Required inputs

- Inventory of consumers and dependencies.
- Data retention, archival, deletion, and legal-hold requirements.
- Access/identity/key/certificate removal plan.
- Traffic and usage evidence.
- Customer and operational communication.
- Rollback/reinstatement boundary.
- Evidence and record-retention plan.

### `READY` criteria

- [ ] Known consumers migrated or explicitly terminated.
- [ ] Data disposition is approved and verified.
- [ ] Secrets, service identities, permissions, DNS, routes, jobs, alerts, and infrastructure are removed.
- [ ] Billing, licensing, support, and vendor relationships are closed.
- [ ] Documentation and inventory show the retired state.
- [ ] Required evidence and audit records remain available.
- [ ] Residual traffic or calls produce a safe, observable response.
- [ ] Reinstatement is either possible within the agreed window or explicitly impossible by design.

### Blockers

- Unknown consumers.
- Legal hold or retention conflict.
- Orphaned data, credentials, privileged identities, or public endpoints.
- Silent deletion of required audit evidence.
- Retirement would make another system unrecoverable.

### Primary domains

`GOV`, `API`, `DAT`, `IAM`, `SUP`, `INF`, `OPS`, `KNO`

## 4. Risk-based gate depth

| Criticality | Minimum gate depth |
|---|---|
| `C0` | G0, G2; local verification; no production release |
| `C1` | G0–G4; lightweight G5/G6 for shared deployment |
| `C2` | G0–G7 as applicable; automated CI and staged production release |
| `C3` | Full gates; specialized reviews; independent Tier 1 verification; recovery evidence |
| `C4` | Full gates plus formal change authority, safety assurance, independent assessment, constrained automation, and explicit go/no-go checkpoints |

Skipping a gate requires the same exception rigor as skipping the controls it protects.

## 5. Emergency change path

Emergency work MAY compress timing but MUST NOT erase accountability.

Required minimums:

1. Named incident/change commander.
2. Declared emergency scope and affected systems.
3. Criticality and blast-radius assessment.
4. Two-person review for privileged or high-impact action where practicable.
5. Smallest reversible change.
6. Live monitoring and explicit rollback trigger.
7. Command/action log.
8. Post-change verification.
9. Retrospective review and permanent remediation.
10. Removal of temporary bypasses and privileges.

“Emergency” MUST NOT become a reusable route around normal controls.

## 6. Separation of duties

For high-consequence changes, no single actor (including an AI agent) should be able to:

- Author the change.
- Approve it.
- alter the verification.
- Release it.
- suppress the alert.
- accept the risk.

At `C3` and `C4`, organizations SHOULD enforce technical separation for source, artifacts, deployment authority, production access, and exception approval.

## 7. Gate anti-patterns

- **Checklist laundering:** every box is checked, but no artifact proves behavior.
- **Pipeline absolutism:** CI is green, therefore production is safe.
- **Approval inheritance:** a reviewer approved an earlier commit and later changes silently retain approval.
- **Average masking:** fleet-wide metrics hide one tenant, region, device class, or canary cohort failing.
- **Rollback fiction:** rollback is documented but cannot operate after schema/configuration change.
- **Exception sediment:** temporary waivers become permanent architecture.
- **Alert optimism:** a dashboard exists, but nobody is paged or knows what action to take.
- **AI self-certification:** the agent writes the change, writes the test, interprets the test, and declares the control satisfied without independent challenge.
