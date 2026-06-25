# Vibeguard Scorecard

This is the scorecard template. Your AI tool uses this format to assess your project and produce a completed scorecard: either as output in the conversation or written as `vibeguard-scorecard.md` in your project. You do not fill this in manually, and the AI does not edit this file.

To run an assessment, ask your AI tool:

> *"Read the Vibeguard standard and assess this project against all 24 domains in docs/failure-taxonomy.md. Output a completed Vibeguard Scorecard using the template in checklist.md."*

---

## Scorecard

**Project:** ___
**Assessed:** ___
**Gate status:** ___

| Status | When |
|--------|------|
| 🟢 **READY** | All T1 domains are ✅ or ➖ |
| 🟡 **REVIEW** | T1 domains clear; one or more T2 domains are ❌ |
| 🔴 **BLOCKED** | Any T1 domain is ❌ |

T3 gaps are noted but do not affect gate status.

---

| Domain | Taxonomy | Tier | Applies | Status | Critical gaps |
|--------|----------|:----:|:-------:|:------:|---------------|
| **GOV**: Governance, Criticality and Ownership | [§4.1](docs/failure-taxonomy.md#41-gov--governance-criticality-and-ownership) | T3 | | | |
| **REQ**: Requirements, Scope and Acceptance | [§4.2](docs/failure-taxonomy.md#42-req--requirements-scope-and-acceptance) | T2 | | | |
| **ARC**: Architecture, Boundaries and Evolvability | [§4.3](docs/failure-taxonomy.md#43-arc--architecture-boundaries-and-evolvability) | T3 | | | |
| **DOM**: Domain Logic and Functional Correctness | [§4.4](docs/failure-taxonomy.md#44-dom--domain-logic-and-functional-correctness) | T2 | | | |
| **COD**: Code Quality, Maintainability and Resource Safety | [§4.5](docs/failure-taxonomy.md#45-cod--code-quality-maintainability-and-resource-safety) | T2 | | | |
| **API**: APIs, Contracts and Integrations | [§4.6](docs/failure-taxonomy.md#46-api--apis-contracts-and-integrations) | T2 | | | |
| **DAT**: Data, Storage, Caching and Migrations | [§4.7](docs/failure-taxonomy.md#47-dat--data-storage-caching-and-migrations) | T1 | | | |
| **IAM**: Identity, Authentication, Authorization and Sessions | [§4.8](docs/failure-taxonomy.md#48-iam--identity-authentication-authorization-and-sessions) | T1 | | | |
| **SEC**: Application Security and Abuse Resistance | [§4.9](docs/failure-taxonomy.md#49-sec--application-security-and-abuse-resistance) | T1 | | | |
| **PRV**: Privacy and Data Protection | [§4.10](docs/failure-taxonomy.md#410-prv--privacy-and-data-protection) | T2 | | | |
| **SAF**: Safety, Human Impact and Responsible Automation | [§4.11](docs/failure-taxonomy.md#411-saf--safety-human-impact-and-responsible-automation) | T2 | | | |
| **SUP**: Dependencies, Provenance and Software Supply Chain | [§4.12](docs/failure-taxonomy.md#412-sup--dependencies-provenance-and-software-supply-chain) | T1 | | | |
| **BLD**: Build, CI, Artifact Integrity and Repository Controls | [§4.13](docs/failure-taxonomy.md#413-bld--build-ci-artifact-integrity-and-repository-controls) | T2 | | | |
| **CFG**: Configuration, Secrets and Feature Flags | [§4.14](docs/failure-taxonomy.md#414-cfg--configuration-secrets-and-feature-flags) | T2 | | | |
| **INF**: Infrastructure, Runtime, Network and Platform Security | [§4.15](docs/failure-taxonomy.md#415-inf--infrastructure-runtime-network-and-platform-security) | T3 | | | |
| **REL**: Reliability, Resilience and Distributed Systems | [§4.16](docs/failure-taxonomy.md#416-rel--reliability-resilience-and-distributed-systems) | T2 | | | |
| **PER**: Performance, Scalability, Capacity and Cost | [§4.17](docs/failure-taxonomy.md#417-per--performance-scalability-capacity-and-cost) | T3 | | | |
| **TST**: Verification, Testing and Independent Assurance | [§4.18](docs/failure-taxonomy.md#418-tst--verification-testing-and-independent-assurance) | T1 | | | |
| **OBS**: Observability, Auditability and Diagnostics | [§4.19](docs/failure-taxonomy.md#419-obs--observability-auditability-and-diagnostics) | T2 | | | |
| **DPL**: Deployment, Release and Change Management | [§4.20](docs/failure-taxonomy.md#420-dpl--deployment-release-and-change-management) | T2 | | | |
| **OPS**: Operations, Incident Response, Backup and Recovery | [§4.21](docs/failure-taxonomy.md#421-ops--operations-incident-response-backup-and-recovery) | T2 | | | |
| **UXA**: User Experience, Accessibility, Internationalization | [§4.22](docs/failure-taxonomy.md#422-uxa--user-experience-accessibility-internationalization-and-compatibility) | T2 | | | |
| **KNO**: Documentation, Knowledge and Supportability | [§4.23](docs/failure-taxonomy.md#423-kno--documentation-knowledge-and-supportability) | T3 | | | |
| **AIA**: AI-Assisted Development and Coding-Agent Safety | [§4.24](docs/failure-taxonomy.md#424-aia--ai-assisted-development-and-coding-agent-safety) | T1 | | | |

*All failure patterns and evidence requirements: [`docs/failure-taxonomy.md`](docs/failure-taxonomy.md)*
