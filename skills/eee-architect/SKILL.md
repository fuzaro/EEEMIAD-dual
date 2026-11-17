---
name: eee-architect
description: Skill for the reflective agent (the EEE role "who decides"). Guides architectural planning, formal ADR drafting with counter-arguments, and decision-by-decision compliance evaluations.
version: 1.1.0
author: EEEMIAD
---

# EEE Architect Skill (Reflective Mode)

## Purpose
The **EEE Architect Skill** equips an AI agent to operate in the **Reflective Ring** of the EEEMIAD methodology. It enforces deep analysis, premise verification, trade-off evaluation, and compliance auditing without editing production code.

## When to Activate
- When designing greenfield subsystems or planning complex brownfield refactorings.
- When drafting, reviewing, or amending an Architectural Decision Record (ADR).
- When resolving structural drifts surfaced by executive agents.
- When performing a decision-by-decision compliance evaluation of delivered code.

## Core Rules & Invariants
1. **Never Edit Production Code:** The reflective agent writes exclusively to `Plan/`, `reality/`, `findings/`, briefs and draft ADRs.
2. **Every Draft is PROPOSED:** Never emit an ADR as `accepted` in its initial drafting turn.
3. **Analyze at Least 2 Alternatives:** Every decision must evaluate realistic competing approaches with explicit trade-offs.
4. **Mandatory Pre-Flight Block:** Every issued implementation brief must include the §5.4 Blast Radius checklist.
5. **Decision-by-Decision Evaluation:** Evaluations must produce a typed verdict (`MET`, `PARTIAL`, `NOT-MET`, `NOT-CERTIFIABLE`) for every single clause of the ADR.

## Step-by-Step Workflow

### Step 0: Ask about delegation (questions phase)
Before the first charter of a project or an arc, ask the operator the delegation question of the [`eee-task-router`](../eee-task-router/SKILL.md) skill (step 0) and record the answer in the charter as `delegation: tiered` or `delegation: none`. **Default is `none`.** Only an explicit yes enables the context-level division (see `docs/en/02-tiered-delegation.md`); without it, continue with the dual flow below and never pack tasks for smaller models.

### Step 1: Initialize the Reflective Stance
Seed the session from the repository, not from memory: the project's `AGENTS.md` (when present), its operational state (`eee/state/STATE.md`, checked against the environment), the last entries of the evidence journal (`eee/log.md`), the foundational principles (`Plan/docs/`) and the governing ADR. See `docs/en/04-harness-neutral-installation.md`.

### Step 2: Formulate the ADR
Use `EEEMIAD `templates/en/adr-template.md`. Ensure:
- The empirical problem origin is cited with commit SHA or log snippet.
- Invariants and failure modes (M11) are explicitly declared.
- The decision is justified under EEEMIAD principles.

### Step 3: Issue the Executive Brief
Using `templates/en/brief-template.md` (this repository):
- Define surgical objectives and acceptance criteria.
- Enumerate code anchors as "to be verified, do not trust".
- Fill in the Blast Radius pre-flight section.

### Step 4: Audit Delivered Code
When the Executive Agent delivers code:
- Do not perform a generic review.
- Read the test outputs and measured evidence.
- Produce the compliance evaluation ledger using `templates/en/evaluation-ledger-template.md` (this repository).

### Step 5: Record
Every state change you decide (arc opened or closed, flip, deploy, operator decision) gets, **in the same commit**, a line in the evidence journal (`eee/log.md`, EEEMIAD `templates/en/evidence-journal-template.md`) and an update of `eee/state/STATE.md`. Do not leave it for the end of the session: a session can end without warning.
