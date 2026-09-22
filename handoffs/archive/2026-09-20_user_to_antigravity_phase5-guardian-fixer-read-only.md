# Handoff: USER → Antigravity — Phase 5 Guardian + AI Fixer Read-Only

> **Arşiv notu (2026-09-22):** Bu Phase 5 uygulama talimatı tarihsel kayıttır; yeniden çalıştırılmamalıdır. Read-only sınır ve açık production kabul koşulları güncel canonical master handoff'tadır. Aşağıdaki yetki ve görev tanımı yalnız ilk verildiği tarihteki bağlamı anlatır.

**Date:** 2026-09-20  
**From:** User  
**To:** Antigravity (`agy`)  
**Status:** Authorized to implement and close Phase 5 only  
**Canonical master:** `handoffs/active/2026-09-17_jarvis-homelab-evolution.md`  
**Phase boundary:** Phase 5 read-only; Phase 6 mutation/self-healing is explicitly out of scope

---

## Objective

Complete **Phase 5 — Provider-Independent Guardian + AI Fixer Read-Only** from the canonical master. Preserve the existing system and extend it. Do not rebuild working components and do not reduce any master requirement.

The system has three standard providers:

- Claude through `ClaudeAdapter`
- Codex through `CodexAdapter`
- Antigravity through `AntigravityAdapter`; production entry point is `agy`

Gemini CLI is not Antigravity and must not be installed, restored, selected, or presented as this provider.

## Mandatory startup and inspection

1. Run `jarvis start --workspace /opt/jarvis --agent antigravity` and read its full output.
2. Read the canonical master completely, including §10–12, §17–18, §33, §42, §45, §48–53 and all Phase 5/Parallel Agent Room gates.
3. Read all active handoffs. Reconcile the two Antigravity onboarding handoffs; preserve evidence and archive completed duplicates only after their requirements are incorporated.
4. Inspect the live implementations before editing: CT100 Jarvis, CT102 Agent Box, CT104 gateway/watchdog, CT132 Command Center, provider registry, adapters, MCP/tool permissions, canonical audit store, outbox/spool, secrets references, auth boundaries and current tests.
5. Treat live/runtime and canonical audit evidence as authoritative. Graphify is derived/index/projection only.

## Required role model

Guardian and Fixer are separate roles. They may use the same provider, but they are not required to.

Command Center must provide two independent persistent dropdowns:

- **Guardian:** `None`, `Claude`, `Codex`, `Antigravity`
- **Fixer:** `None`, `Claude`, `Codex`, `Antigravity`

Required behavior:

- Guardian performs AI-level monitoring/triage, collects evidence and may create a structured handoff to the selected Fixer.
- Fixer performs root-cause analysis and produces a typed remediation proposal.
- Phase 5 permits no execution, remediation or self-healing.
- Guardian `None`: no AI provider receives automatic monitoring/triage tasks.
- Fixer `None`: Guardian may record and triage an incident, but no automatic or manual AI Fixer task starts.
- `None` is a real disabled state. There is no hidden or automatic fallback.
- If a selected provider is unavailable, show that role as unavailable/waiting and do not switch providers.
- CT104 independent watchdog, Prometheus/Alertmanager and base health/event collection continue regardless of either dropdown.
- Do not silently choose a Guardian or Fixer for the user. If no prior persisted choice exists, migrate fail-closed to `None` for each role.

## Persistence and audit contract

Persist Guardian and Fixer independently. Every selection change must append an event to the canonical audit store with at least:

- `schema_version`
- `event_id`
- `timestamp`
- `received_at`
- `correlation_id`
- `event_category`
- `role` (`guardian` or `fixer`)
- previous provider
- new provider
- actor
- source/provenance
- result

Do not store secret values. Use `secret_ref`/ID and resolve only at the authorized execution boundary. Graphify may index these events but is not authoritative.

## Guardian → Fixer handoff contract

The structured handoff must carry:

- `incident_id` and `correlation_id`
- live/canonical evidence with source and observation time
- provenance and freshness
- affected targets and verified dependencies
- confidence and unresolved questions
- known constraints
- requested output type

The Fixer response must be a proposal containing target, prerequisites, risk, expected impact, proof/approval requirements, verification and rollback strategy. A proposal is not authorization to execute.

## Provider contract

`ClaudeAdapter`, `CodexAdapter` and `AntigravityAdapter` must expose the same role-neutral contract for capability discovery, timeout, cancellation, structured result, tool-action summary, usage/cost metadata, error handling and audit metadata.

Provider selection must never change:

- tool permissions
- policy or authorization
- Phase 5 read-only boundary
- secret handling
- approval requirements
- destructive-action defaults

The UI must not bind directly to CLI/API transport details.

## Read-only enforcement

Enforce Phase 5 with tool permissions, execution identity, filesystem boundaries and policy. Prompt text alone is insufficient.

All of the following must be rejected in Phase 5 regardless of selected provider:

- config/file mutation
- service or container restart/reload/stop/start
- deletion or queue removal
- package installation/removal
- storage changes
- Home Assistant mutation
- mutation MCP calls
- unrestricted shell/root operations
- secret reveal
- approval/policy bypass

Keep `delete_file`, `remove_queue_item`, `block_release` and similar destructive actions **DEFAULT DENY + APPROVAL REQUIRED + DESTRUCTIVE IMPACT PROOF**. They remain unavailable in Phase 5.

## Command Center requirements

Extend the existing CT132 Command Center; do not create a replacement dashboard.

Show clearly:

- selected Guardian and selected Fixer
- separate availability/health for each selected provider
- `None`, unavailable and waiting states
- current phase/mode: `Phase 5 — Read-only`
- last Guardian triage and last Fixer proposal
- source and observation time
- audit correlation ID where appropriate

Preserve read-only health plane versus admin/action plane isolation. Changing Guardian/Fixer configuration is an authenticated, authorized admin action and must not be exposed through the public/read-only health surface. Do not place secrets in the browser.

## Required verification

Use fixtures/disposable sandbox where needed. Do not use production as the only experiment environment.

Prove at minimum:

1. All dropdown values persist and render correctly on desktop and mobile.
2. Guardian and Fixer can be different providers.
3. Both can independently be `None`.
4. Guardian `None` creates no AI triage task while base monitoring continues.
5. Fixer `None` permits incident/triage recording but creates no Fixer task.
6. An unavailable selected provider does not trigger fallback.
7. Guardian → Fixer handoff preserves IDs, evidence and provenance.
8. Claude, Codex and Antigravity conform to the same read-only adapter contract.
9. Mutation attempts through UI, API, MCP, adapter and prompt/tool bypass are rejected.
10. Secret redaction passes for transcript, tool output, error, audit and Discord paths.
11. One broken provider or endpoint does not prevent unrelated health cards from rendering.
12. Canonical audit events are append-only and queryable; Graphify projection can be rebuilt without becoming authoritative.
13. Existing Phase 0–4 behavior, CT104 durable spool/watchdog, Command Center navigation and production integrations remain healthy.

## Phase gate and completion

Phase 5 is complete only when implementation, tests, production-safe deployment, desktop/mobile verification, negative-test evidence, persistence updates and gate evidence all pass.

Before closing:

- update the Phase 5 evidence matrix;
- update checkpoint, todo and relevant session logs;
- commit each changed repository with clear messages;
- ensure working trees are clean or document unrelated pre-existing changes;
- archive completed Phase 4 and provider-onboarding working handoffs after confirming their requirements are incorporated;
- keep the canonical master and current Phase 5 closure/handoff active;
- write a concise Antigravity → Claude/Codex Phase 5 closure handoff for independent review.

Do not begin Phase 6. Do not enable write/remediation/self-healing. Do not stop for routine implementation confirmations; stop only for a genuinely missing credential/user decision, an unsafe irreversible action, or a contradiction that cannot be resolved from live/canonical evidence.
