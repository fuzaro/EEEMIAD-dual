# Changelog

All notable changes to EEEMIAD-dual are documented here. Format follows [Keep a Changelog](https://keepachangelog.com/).

## [0.4.1] — 2026-11-17

### Changed
- Brief template (PT/EN): pre-flight gains *target state* (6), *live reproduction read or write* (7) and *request item* (8), following EEEMIAD 1.6.0.
- `eee-executor` 1.2.0: every stateful battery declares and prepares its starting state; probe-and-revert declares the layers it removes, with one test per redundant layer.
- `eee-module-builder`: report lines for the layers removed and the starting state.

## [0.4.0] — 2026-11-17

### Added
- **Harness-neutral installation** guide (`docs/pt/04-instalacao-neutra.md`, `docs/en/04-harness-neutral-installation.md`): how to install this repository's roles in a project so that another tool, model or person continues from the repository. `AGENTS.md` as entry point; skills rewritten as neutral playbooks in `eee/playbooks/`; Claude Code skills and agents as adapters of at most 20 lines checked by the single gate; operational state and evidence journal in the repository; session memory as a shortcut. Mapping table, adapter example, anti-patterns, checklist.

### Changed
- `eee-architect` 1.1.0: step 1 seeds from the repository (`AGENTS.md`, operational state, journal); new step 5, record the journal line and the state in the same commit.
- `eee-executor` 1.1.0: step 4 runs the project's single gate; journal line on state changes.
- `eee-gate-checker` 0.3.0: the packet's gate is a subset of the single gate, never a weaker command.
- `eee-task-router` 0.3.0: agents installed as adapters in harness-neutral projects; journal line at the arc's closure.
- Agents: read the project's `AGENTS.md` first; its hard rules prevail.
- Cross-session guide and message template: the source of a remote credential is recorded in the operational state (where, never the value); rounds that change an environment get a journal line.

Principle (tool-neutral) in EEEMIAD 1.5.0, ch. 08 §8.

## [0.3.0] — 2026-11-15

### Added
- **Cross-session communication** guide (`docs/pt/03-comunicacao-entre-sessoes.md`, `docs/en/03-cross-session-communication.md`): the protocol used to run a pre-flight, a deploy and a timed rollback from a second Claude Code session (Remote Control) that held the cluster credential. Closed-packet messages, two keys for writes, secrets that do not travel, restart of the remote session, correction protocol, verification of what comes back, anti-patterns.
- `templates/{pt,en}/cross-session-message-template.md`.
- `eee-task-router`: criterion 0, remote packet.

Principle (tool-neutral) in EEEMIAD 1.4.0, ch. 08 §7.

## [0.2.0] — 2026-11-14

### Added
- **Optional delegation by context level**, enabled only by the operator's *yes* at the questions phase and recorded in the charter (`delegation: tiered`); default `none`. Guide in `docs/pt/02-delegacao-por-nivel.md` and `docs/en/02-tiered-delegation.md`.
- `agents/`: Claude Code agent definitions with declared model and tools: `eee-local-executor` (haiku), `eee-local-checker` (sonnet, no edit tools), `eee-module-builder` (sonnet), `eee-oracle-writer` (a model different from the builder's).
- Skills `eee-task-router` (ask, classify, dispatch, record) and `eee-gate-checker` (gate before reading, never edits).
- `eee-architect` step 0 (the question) and `eee-executor` packet mode.
- Evaluation ledger: "Delegation record" section (level, models, gate on first attempt, attempts, escalation, cost).

Principle (tool-neutral) in EEEMIAD 1.3.0, ch. 08 §6, and its `task-packet-template.md`.

## [0.1.0] — 2026-11-13
- Foundations: dual-agent workflow (PT/EN), brief and evaluation-ledger templates, `eee-architect` and `eee-executor` skills.
