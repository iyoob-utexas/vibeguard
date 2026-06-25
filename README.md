# Vibeguard

> **Vibe can start the code. Evidence must finish it.**

Vibeguard is a coding standard for AI-assisted development. Point your AI tool at this repo and it will produce code that is secure, scalable, modular, and production-ready — then assess your project and tell you exactly where the gaps are.

---

## How to use it

**One step: configure your AI tool.**

Paste this into your AI tool's system prompt, project instruction file, or equivalent configuration:

```
Read and follow the Vibeguard coding standard before writing or modifying any code in this project.

Start by reading:
1. AGENTS.md — the operating contract for all code changes
2. docs/failure-taxonomy.md — the full risk domain catalog
3. checklist.md — the Vibeguard Scorecard template; assess this project and output a completed scorecard

Do not claim a rule passes if you have not verified it. Do not mark a domain ✅ without concrete evidence.
```

That's it. The AI reads this repo, follows the rules while it codes, and can assess your project against all 24 risk domains on request.

---

## Getting a scorecard

Ask your AI tool at any time:

> *"Assess this project against the Vibeguard failure taxonomy and output a completed Vibeguard Scorecard."*

It will scan your project, determine which of the 24 risk domains apply, and produce a scorecard like this:

| Domain | Tier | Applies | Status | Critical gaps |
|--------|:----:|:-------:|:------:|---------------|
| SEC — Application Security | T1 | Yes | ⚠️ | API keys found in committed .env file |
| TST — Testing | T1 | Yes | ❌ | No tests exist for the auth flow |
| IAM — Identity and Auth | T1 | Yes | ✅ | |
| UXA — User Experience | T2 | Yes | ⚠️ | No loading or error states on data fetch |
| PER — Performance | T3 | No | ➖ | |
| ... | | | | |

**Gate status:**

| Status | When |
|--------|------|
| 🟢 **READY** | All T1 domains pass or don't apply |
| 🟡 **REVIEW** | T1 clear; T2 gaps remain |
| 🔴 **BLOCKED** | Any T1 domain has critical gaps |

---

## What's in this repo

| File | What it's for |
|------|---------------|
| [`AGENTS.md`](AGENTS.md) | The full operating contract your AI tool reads and follows |
| [`checklist.md`](checklist.md) | The 24-domain scorecard template your AI uses to produce assessments |
| [`docs/failure-taxonomy.md`](docs/failure-taxonomy.md) | 24 risk domains and 279 failure patterns — the rules the scorecard is based on |
| [`docs/web-ui.md`](docs/web-ui.md) | Detailed web UI rules: responsiveness, modularization, edge states, stack choices |
| [`docs/framework-standard.md`](docs/framework-standard.md) | Criticality levels C0–C4 and what each requires |
| [`docs/release-gates.md`](docs/release-gates.md) | The decision logic behind READY / REVIEW / BLOCKED |
| [`docs/incident-lessons.md`](docs/incident-lessons.md) | 14 real-world incidents and the rules they produced |

---

## Roadmap

**Integration overlays** — Pre-mapped controls for common integrations (Stripe, Auth0/Clerk, Supabase, OpenAI, SendGrid) so the AI knows exactly what to verify for each. Stripe webhook HMAC, idempotency requirements, OAuth token encryption, rate limiting on AI endpoints — all specified per-integration, not inferred from general rules.

**Stack overlays** — Framework-specific enforcement for Next.js, FastAPI, React Native, and Go services. The taxonomy describes what must be true; overlays describe how to achieve it in the specific framework the project uses.

**Agentic platform module** — Expanded controls for platforms that *run* AI agents on users' behalf: tool sandboxing, OAuth delegation scope management, irreversible action gates, cost budgets, and audit trail requirements. Goes beyond coding-agent safety (AIA) into the full lifecycle of a platform that executes autonomous workflows.

**CI integration** — A GitHub Action that runs a Vibeguard assessment on every PR and posts the scorecard as a check. Gate status becomes part of the merge decision, not a separate conversation.

**Exception tracking** — A structured format for recording accepted risks — what was waived, why, by whom, and when it expires. Makes the gap between "assessed" and "production-ready" visible and time-bound.

---

## Design principles

- **Rules live in the taxonomy.** The scorecard points to them — it does not duplicate them.
- **The AI determines applicability.** It reads the project and decides which domains matter. You answer no questions.
- **Evidence over assertion.** A domain is not ✅ because it looks fine. It passes when the AI has read the code that makes it pass.
- **One canonical source.** Your AI tool reads `AGENTS.md` here. It does not copy or fork the rules.
- **Generated code is untrusted until assessed.** Fluency is not evidence. The scorecard is.

