---
name: eee-gate-checker
description: Intermediate-level skill for checking an EEE task packet blindly against its mechanical gate. Used by the eee-local-checker agent when the operator enabled tiered delegation. Runs the gate first, then compares the output with the spec section; never reads the executor's reasoning and never edits.
version: 0.3.0
author: EEEMIAD
---

# ✅ EEE Gate Checker (blind, gate-first)

## Purpose
In tiered delegation the intermediate model does not re-do or re-read the work: it **re-runs the decision**. The gate declared in the packet is the primary verdict; reading comes after, and only against the spec. This keeps the check independent of the executor, which is the whole point (EEEMIAD ch. 06 §4, independence at the functional tier).

## When to activate
Only inside a charter with `delegation: tiered`, for packets classified as **local** by `eee-task-router`.

## Inputs (and nothing else)
- The task packet.
- The executor's report (`PACKET / CHANGED / GATE / GATE OUTPUT / STATUS`).
- Read access to the repository to run the gate and to read the spec section the packet cites.

Not an input: the executor's chat, reasoning or notes.

## Procedure
1. **Run the gate yourself.** Copy the exact command from the packet, not from the executor's report. The packet's gate is a subset of the project's single gate; never accept a weaker command in its place. Red gate → `RETURNED` with the literal failure. Stop.
2. **Scope.** `git diff --stat` must list only the packet's declared change points. Anything else → `RETURNED (out of scope)`.
3. **Contract.** Compare the output with the packet's "Output contract" and with the cited spec section, clause by clause.
4. **Refutation.** Try the packet's refutation condition. If it is observed while the gate is green, the gate is too weak: `ESCALATE (weak gate)`. That is a system-level defect.
5. **Attempt count.** On the packet's `escalate_after`-th failure → `ESCALATE`.

## Verdicts

| Verdict | When | Who acts next |
|---|---|---|
| `APPROVED` | gate green, scope clean, contract met, refutation not observed | the router records it |
| `RETURNED` | gate red, out of scope, contract not met | the executor, with the literal reason |
| `ESCALATE` | attempts exhausted, packet not closed, or weak gate | the system level |

## Core rules
1. Gate before reading.
2. No edits: a checker that fixes the work becomes a second author.
3. No trust in reported output: every command is re-run.
4. Report in the shape fixed by the `eee-local-checker` agent.
