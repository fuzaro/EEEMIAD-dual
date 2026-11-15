---
name: eee-task-router
description: Optional system-level skill that splits an EEE arc into task packets by context level (local, module, system) and dispatches them to agents of matching model size. Activates ONLY after the operator answers yes to the delegation question in the questions phase; otherwise it records `delegation: none` and stops.
version: 0.2.0
author: EEEMIAD
---

# 🧭 EEE Task Router (optional, opt-in)

## Purpose
Most of an EEE arc, once the charter, the spec and the M11 are written, is **closed work**: small, local and decided by a mechanical gate. This skill sends that work to a smaller model, has an intermediate model check it **against the gate**, and keeps at the system level only what requires a decision. The principle is in EEEMIAD ch. 08 §6. This skill is its instantiation with Claude Code agents.

## When to activate
- At the **questions phase** of a project or an arc, before the first charter. That is the only moment it asks.
- Never on its own afterwards. If the answer was no, or there was no answer, the arc runs the usual dual-agent flow.

## Step 0 · Ask (mandatory, once)
Ask the operator, using the host's question tool when it has one:

> "Adopt task division by context level in this arc? Local tasks with a mechanical gate will be executed by a smaller model (`eee-local-executor`, haiku) and checked by an intermediate one (`eee-local-checker`, sonnet), escalating after two failures. Module work goes to `eee-module-builder` (sonnet), oracles to `eee-oracle-writer` (a different model). Decisions stay here. Extra cost: one packet per task."

Options: **Yes, tiered** · **No, keep the dual flow** (recommended when the arc is small or the gates are not mechanical yet).

Record the answer in the charter: `delegation: tiered` or `delegation: none`, with the date and the operator's words. **Without an explicit yes, stop here.**

## Step 1 · Check the agents exist
The four agent definitions live in this repository's `agents/` directory. They must be copied to `.claude/agents/` of the target project (or `~/.claude/agents/`). If they are missing, tell the operator the copy command and do not dispatch:

```bash
mkdir -p .claude/agents && cp <EEEMIAD-dual>/agents/eee-*.md .claude/agents/
```

## Step 2 · Split the arc into packets
For each checklist line of the charter, write one task packet (EEEMIAD `templates/en/task-packet-template.md`) and classify it **in this order**:

1. Needs a **decision** (contract, flag, ADR, conflict between layers, pre-flight answer) → **system**: stays in this session.
2. Reads **more than one module** → **module** → `eee-module-builder`.
3. Has **no mechanical gate** → cannot be local → module.
4. Otherwise → **local** → `eee-local-executor`, checked by `eee-local-checker`.

The oracle of any arc with a `shadow` phase is its own packet → `eee-oracle-writer`. Its model must differ from the builder's.

## Step 3 · Dispatch and check
- Local: send the packet to `eee-local-executor`. Send **only** the packet, the executor's report and the gate to `eee-local-checker`, never the executor's reasoning.
- Module: send the charter phase to `eee-module-builder`. At the gates (probe-and-revert, pair 3, refutation), check here, at the system level.
- Escalation: after `escalate_after` failures (default 2), move the packet up one level. A failure caused by an unclosed packet (missing input, ambiguous contract) is a **spec defect**: fix the packet or the spec here, then re-dispatch.

## Step 4 · Record
Every packet gets one row in the evaluation ledger, section "Delegation record" (this repository's `templates/en/evaluation-ledger-template.md`): level, executor model, checker model, gate on first attempt, attempts, escalation and cost when the tool reports it. At the arc's closure, report the escalation rate of the local level against the limit declared in the charter. Above the limit, the classification rule needs fixing, not the models.

## Core rules
1. **Opt-in only.** No yes, no delegation.
2. **The gate decides, not reading.** A local task without a mechanical gate is a classification error.
3. **Blind checking.** The checker never sees the executor's reasoning.
4. **Different author for the oracle.** Builder and oracle writer never share a model.
5. **Decisions do not travel down.** Nothing that needs a decision is packed for a smaller model.
6. **The human closes `done@gate`.** No agent, at any level, ratifies its own work.
