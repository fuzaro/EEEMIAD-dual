# Changelog

All notable changes to EEEMIAD-dual are documented here. Format follows [Keep a Changelog](https://keepachangelog.com/).

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
