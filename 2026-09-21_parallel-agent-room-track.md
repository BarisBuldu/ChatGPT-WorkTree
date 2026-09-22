# Handoff: Parallel Agent Room Track - Provider Parity, Shared Policy and Durable Task Identity

**Date:** 2026-09-21  
**Status:** Historical plan; F0–F9 base implementation reported CLOSED, original-mode/recovery acceptance remains to verify  
**Scope owner:** Jarvis / Agent Box  
**Execution backends:** Claude Code, Codex CLI, Google Antigravity  
**Canonical master:** `handoffs/active/2026-09-17_jarvis-homelab-evolution.md`  
**Recommended system path (historical):** `docs/handoff-2026-09-17/parallel-agent-room-track.md`

> **2026-09-22 güncellik notu:** Aşağıdaki durum tablosu 22 Eylül uygulayıcı raporuna göre güncellendi; F0–F8 ayrıntılı iş listeleri ilk implementation kontratının tarihsel referansıdır. Son uygulayıcı raporuna göre üç provider registry, Agent Box keşfi, CT100 durable task/dispatcher ve ayrı Agents’ Room UI production'da çalışıyor; dispatcher systemd servisi aktif. Kullanıcı üç ajanlı konuşma ve handoff'u denedi. Buna rağmen master §20'deki ONLY×3/Dual/Review/Debate/Consensus/Implementation Loop kiplerinin her birinin ayrı kabul kanıtı ile CT102 kaybında durable replay/fencing kanıtı açık kalır. Parallel track master kapsamındadır; ayrı proje değildir. Read-only provider politikası Phase 6 güvenlik kapısı geçmeden gevşetilmez. Tamamlanmış fazları sıfırdan başlatma; güncel otorite canonical master §18–25/Phase Gate'tir.

## 1. Purpose and authority

This document tracks the Parallel Agent Room Track already contained in the canonical Jarvis Homelab Evolution master scope. It does not replace the canonical master, create a separate project, or change the dependency order of the main Evolution phases.

The target architecture is:

```text
Jarvis / Agent Box identity
        |
        +-- canonical context
        +-- canonical policy
        +-- canonical MCP/tool inventory
        +-- canonical durable task state and audit
        |
        +-- Claude Code execution backend
        +-- Codex CLI execution backend
        +-- Google Antigravity execution backend
```

Changing the provider must change only the execution engine. It must not change what Jarvis knows, which task is being executed, where state is stored, which policies apply, which tools are available, or which Agent Box features are exposed.

## 2. Current reported baseline (2026-09-22)

This is an **implementer-reported** production snapshot, not independent runtime verification in this workspace:

- Provider registry v2 and dynamic Agent Box expose Claude, Codex and Antigravity; `agy` is the Antigravity launcher, not Gemini CLI.
- CT100 canonical task store, dispatcher systemd service and separate Agents’ Room UI are reported live. The user tried a real three-provider conversation and generated a handoff.
- Shared read-only provider policy and denial tests were reported. Phase 6 remediation library passed sandbox tests, but the Guardian→Fixer→typed executor production chain remains unconnected. Do not loosen read-only policy to make an Agent Room test pass.
- The basic F0–F9 track was marked CLOSED by the implementer. That label does **not** prove every original mode and recovery scenario below. ONLY×3, Dual, Review, Debate, Consensus and Implementation Loop each need explicit acceptance evidence; CT102 loss/replay, duplicate ownership/fencing, long history, cost/timeout and role-specific context remain open until proven.
- Main Evolution Phase 7 is OPEN: scanner/UI progress was reported, but adblock, continuous restore, durable Discord result delivery and other user-facing gates remain.
- In this local mirror, only the canonical master remains under `handoffs/active/`; the old Phase 1/5 working handoffs were archived. Live CT100 parity still requires checking.

## 3. Non-negotiable boundaries

1. Inspect and preserve existing runtime behavior before editing.
2. Prefer registry-driven and provider-agnostic changes.
3. Do not break working Claude or Codex paths while adding parity.
4. Do not broaden permissions to make a parity test pass.
5. Jarvis policy is authoritative; provider-native controls are enforcement adapters.
6. Graphify remains a derived/index/projection layer, not authoritative state.
7. Provider sessions are execution details, not canonical task state.
8. Secrets are carried as references and never written to evidence or prompts.
9. No unauthenticated root-SSH-backed HTTP endpoint may be introduced.
10. No real scheduled prompt or live provider-switch drill is allowed until its explicit verification gate.
11. Phase 6 remediation must not open before the shared policy gate is closed.
12. Every phase needs evidence, persistence updates, rollback information and a clean repository state.

## 4. Current tracking table

| Phase | Original scope | 2026-09-22 status | Remaining acceptance |
|---|---|---|---|
| F0 | Reality/baseline | Reported complete | Reconcile current runtime only where changed |
| F1 | Provider registry | Reported live | Provenance and drift on all three providers |
| F2 | Dynamic Agent Box UI | Reported live | Desktop/mobile and unavailable provider state |
| F3 | Registry-driven backend | Reported live | No hidden hardcoded fallback |
| F4 | Shared policy/MCP inventory | Reported applied with negative tests | Runtime enforcement and drift/rollback evidence |
| F5 | Shared bootstrap/topology | Reported complete | Fresh role-specific context proof |
| F6 | Durable task identity | Reported live | CT102 loss/replay and split-owner/fencing |
| F7 | Discord/reporting/observability parity | Partly reported live | Durable result delivery, cost/usage and quota attribution |
| F8–F9 | Live parity and separate Agents’ Room UI | Basic three-agent task/handoff reported live | Each original execution mode, long history and implementation loop |

The detailed F0–F8 sections below preserve the original implementation contract for review; they are **not instructions to restart completed phases**. The canonical master §18–25 and Parallel Phase Gate own current acceptance.

## 5. F0 - Reality and baseline

### Objective

Record the actual provider, container, session, scheduler, policy, MCP and bootstrap topology before implementation.

### Work

- Inspect `/opt/jarvis`, `/opt/agent-box` and related webapp repositories.
- Verify CT102 Agent Box and CT100 execution/session relationships.
- Verify SSH and tmux session behavior for Claude, Codex and Antigravity.
- Inventory provider registry, `/health`, scheduler, adapters, bootstrap files, native permission files and MCP definitions.
- Classify prior audit findings as confirmed, stale or false-positive.
- Record rollback points and current commit hashes.
- Create the system copy of this document under `docs/handoff-2026-09-17/`.
- Update checkpoint, todo and session persistence.

### Acceptance gate

- No runtime behavior changed.
- Current topology is supported by command or file evidence.
- All confirmed hardcoded provider lists are enumerated.
- F0 evidence and persistence validation pass.

## 6. F1 - Authoritative provider registry

### Objective

Make `/opt/jarvis/context/provider_registry.json` the single authoritative provider inventory.

### Required provider fields

- `id`
- `display_name`
- `enabled`
- `default`
- `adapter`
- `scheduler_capable`
- `execution_command`
- `capabilities`
- `policy_class`
- `mcp_bundle`
- `bootstrap_entry`
- `health_check`

### Work

- Validate Claude, Codex and Antigravity entries against live commands and adapters.
- Add schema validation.
- Remove hardcoded fallback behavior from `providers.py`.
- Fail closed with an explicit error if the registry is missing or invalid.
- Detect duplicate provider identifiers and multiple defaults.
- Add a fourth-provider fixture proving unrelated files do not require edits.

### Acceptance gate

- All provider consumers resolve the same enabled inventory.
- Invalid registry data is rejected without silently coercing to Claude.
- Existing adapter behavior remains intact.
- Registry and provider contract tests pass.

## 7. F2 - Dynamic Agent Box frontend discovery

### Objective

Populate Agent Box scheduler and session choices dynamically from the verified provider inventory.

### Work

- Remove static provider options from Agent Box frontend code.
- Remove Claude/Codex-only ternaries and normalization.
- Accept stored selection only when the provider is enabled and scheduler-capable.
- Use the registry-defined default only when stored state is invalid.
- Never coerce an unknown provider into Claude.
- Disable scheduling with an understandable error if provider inventory cannot be loaded.
- Render Antigravity through the same frontend path as Claude and Codex.
- Test desktop, mobile and a fourth-provider fixture.
- Build scheduler requests in dry-run only.

### Acceptance gate

- UI options equal the authoritative enabled scheduler inventory.
- Claude, Codex and Antigravity request bodies preserve the selected provider ID.
- Inventory failure is fail-closed.
- No real provider prompt is dispatched.

## 8. F3 - Registry-driven Agent Box backend

### Objective

Remove backend dependence on static provider/session lists.

### Work

- Make scheduler validation use authoritative inventory.
- Return explicit 4xx errors for unknown, disabled or non-scheduler providers.
- Make `/health` provider and session output consistent with the registry.
- Use an authenticated service or safely synchronized read-only projection from CT100.
- Do not expose a root-SSH-backed unauthenticated endpoint.
- Correct stale topology assumptions in `env.py`.
- Do not invent per-provider systemd services that do not exist.
- Trace Antigravity submit behavior without sending a live prompt.
- Add scheduler request and backend validation fixtures for all three providers.

### Acceptance gate

- Frontend, backend and registry inventories match.
- Invalid providers fail explicitly.
- Existing Claude and Codex scheduler paths remain valid.
- Antigravity request construction passes dry-run tests.
- Live Antigravity submit-key behavior remains a documented deferred test if not proven non-invasively.

## 9. F5 - Shared bootstrap contract and topology documentation

### Objective

Remove drift between Claude, Codex and Antigravity startup context.

### Work

- Move common Jarvis instructions to one shared bootstrap source.
- Keep provider entry files limited to identity, native differences and a shared bootstrap reference.
- Validate that each provider can load the complete shared contract.
- Warn or fail explicitly when required shared context is absent.
- Rewrite Agent Box README for CT102, SSH, tmux and three providers.
- Remove stale CT106, ttyd and obsolete port descriptions.
- Add `run-antigravity.sh` only when it matches the existing helper model.
- Add required-file and context parity tests.
- Do not alter native permissions in this phase.

### Acceptance gate

- All three providers receive the same mandatory Jarvis rules and context list.
- Provider-specific files contain only justified native differences.
- Documentation matches live topology.
- Startup and required-context validation pass.

## 10. F4 - Shared policy and MCP inventory

### Objective

Make Jarvis policy and MCP inventory authoritative across all providers.

### Stage A - Mandatory dry-run design

- Define canonical `provider_policy.json` schema.
- Define canonical `mcp_inventory.json` schema.
- Design a renderer for Claude, Codex and Antigravity native formats.
- Produce redacted current-versus-proposed diffs.
- Add drift detection and rollback design.
- Compare effective permission and MCP inventories.
- Preserve the Phase 5 read-only restriction.

Stop after Stage A and obtain explicit approval before writing production native configs.

### Stage B - Production application after approval

- Back up each native provider configuration.
- Render configs only from canonical sources.
- Never weaken Claude or Codex controls to reach parity.
- Ensure Antigravity is not broader than Jarvis policy.
- Validate secrets remain references.
- Run negative mutation tests and read-only MCP parity tests.
- Roll back immediately if a provider loses mandatory safe access or gains unauthorized capability.

### Acceptance gate

- Canonical policy and MCP inventory generate all native configs deterministically.
- Drift is detected.
- Equivalent actions receive equivalent allow/deny decisions.
- Phase 5 mutation denial still passes for all providers.
- This gate must close before main Evolution Phase 6 begins.

## 11. F6 - Durable Jarvis task identity

### Objective

Allow one Jarvis task to continue across providers without making provider sessions authoritative.

### Authoritative flow

```text
Command Center authenticated API
        |
        v
CT100 Jarvis canonical task store + append-only audit
        |
        v
execution attempts
        |
        +-- provider
        +-- provider_session_id
```

Command Center is an authenticated API and projection layer. CT100 Jarvis persistence and canonical audit remain authoritative. CT132 loss must not destroy task history.

### Minimum task model

- `task_id`
- objective and scope
- lifecycle status
- created/updated timestamps
- current checkpoint
- required context references
- policy class
- approvals and denials
- execution attempts
- provider and provider session ID
- artifacts and evidence
- audit correlation IDs
- recovery metadata

### Work

- Reuse the existing authenticated action-plane security model.
- Add create/read/update/complete task operations with authorization and audit.
- Keep operational Agent Box state durably synchronized with Jarvis persistence.
- Add conflict detection and idempotency.
- Recover state without relying on a live provider session.
- Build a dry-run provider-switch fixture before any live drill.

### Acceptance gate

- A task survives provider session loss and CT132 restart.
- Execution attempts are append-only and attributable.
- Provider switching preserves one task ID and context lineage.
- Unauthorized state mutation is rejected and audited.
- Live Claude to Antigravity to Codex drill requires separate explicit approval.

## 12. F7 - Discord, reporting and observability parity

### Objective

Ensure provider choice does not change reporting, audit or observability behavior.

### Work

- Route provider lifecycle events through the existing structured event and CT104 durable spool path.
- Show provider, adapter, task ID, attempt ID, correlation ID, outcome and duration.
- Keep successful routine events out of the problem count while retaining them in System Activities.
- Add Antigravity metrics where equivalent metrics exist for Claude and Codex.
- Correct the Codex daily token metric gap.
- Use optional/configurable report routing and schedules.
- Preserve Discord destructive-action guardrails.

### Acceptance gate

- Equivalent provider executions create equivalent event and audit shapes.
- CT104 replay/dedup behavior is unchanged.
- Provider failure is visible without losing canonical task state.
- Reports contain no secrets or raw sensitive payloads.

## 13. F8 - Controlled live parity suite and closure

### Objective

Prove that provider selection changes the execution backend while preserving Jarvis identity, policy, tools and state.

### Required controlled tests

1. Execute the same harmless read-only registry task separately through Claude, Codex and Antigravity.
2. Verify equal response schema with provider-specific identity fields.
3. Verify effective shared context.
4. Verify Graphify and required MCP tool inventory.
5. Verify equivalent mutation denial.
6. Schedule one controlled message per provider.
7. Verify Discord/reporting and Command Center observability.
8. Run one task through Claude, then Antigravity, then Codex using the same canonical `task_id`.
9. Verify recovery and audit lineage.

These tests require explicit approval because they inject real prompts into provider sessions.

### Final gate

- All required parity tests pass with evidence.
- Intentional provider differences are documented.
- No unresolved security or state-authority gap remains.
- Repositories are clean and persistence verification passes.
- Completed working handoffs are archived while the canonical master remains the sole active handoff.

## 14. Relationship to main Evolution phases

- F0-F3 and F5 may proceed after Phase 5 closure.
- F4 must close before main Phase 6 controlled remediation opens.
- F6 remains a parallel track and must not silently move canonical task state into CT132.
- F7 can proceed after F4 and F6 foundations.
- F8 is the final live provider parity gate.
- This track does not remove or replace main Phase 6, Phase 7 or Phase 8 requirements.

## 15. Evidence and persistence requirements

Each phase must record:

- files changed
- architecture before and after
- tests and exact outcomes
- runtime evidence where authorized
- security boundaries
- rollback steps
- commit hashes
- remaining risks and deferrals
- checkpoint, todo and session updates
- clean working tree proof

## 16. Next acceptance action

Do not restart F0–F5. Inspect the existing production evidence for each original Agent Room execution mode, CT102 recovery/replay/fencing and role-specific provider context. Run only targeted missing tests or fixes, preserving the working three-provider room. Record outcome under the canonical master Parallel Phase Gate. Main Evolution's active implementation phase remains Phase 7; this parallel acceptance does not authorize Phase 6 mutation or defer Phase 7 adblock/restore work.
