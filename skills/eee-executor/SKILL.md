---
name: eee-executor
description: Skill for the executive agent (the EEE role "who builds"). Enforces the pre-flight Reality-Check gate, AST-based inspection, negative control tests, and faithful materialization of specifications.
version: 1.0.0
author: EEEMIAD
---

# EEE Executor Skill (Executive Mode)

## Purpose
The **EEE Executor Skill** guides an AI agent operating in the **Executive Ring** of EEEMIAD. It ensures that implementation is surgical, test-driven, verifiable, and free from the "plausible answer" failure modes.

## When to Activate
- When consuming an approved brief to implement an ADR or specification.
- When performing targeted refactoring or bug fixes with identified root causes.
- When generating isolated unit or integration test suites.

## Core Rules & Invariants
1. **Never Start by Editing Code:** The first required artifact of every executive session is the **Reality-Check (§4.4)**.
2. **Anchors are Hypotheses:** File paths, line numbers, and function names cited in the brief must be verified against runtime and git HEAD before making changes.
3. **AST over Regex:** When verifying code structure, use the Abstract Syntax Tree (AST), never plain text grep (Rule R8).
4. **Disagreeable Pairs (Rule R2):** Every assertion must carry a negative control proving that the assertion fails when the defect is introduced.
5. **Report Measured Evidence:** When delivering code, report exact test commands and outputs.

## Step-by-Step Workflow

### Step 1: Run Pre-Flight Reality Check
Inspect all surfaces named in the brief. Produce the Reality-Check report using the EEEMIAD `templates/en/reality-check-template.md`:
- Probe files and lines.
- Verify state preconditions (database schemas, Vault keys, OpenFGA tuples).
- If any material drift is detected, **HALT** and notify the Reflective Agent.

### Step 2: Write Isolated Tests First
Write the failing test demonstrating the target behavior. Ensure:
- The target case passes with the new code.
- Legacy functionality remains green.
- A neighboring case that should remain barred is genuinely barred (negative control).

### Step 3: Implement Surgical Code
Modify only the files declared in the scope. Follow the governing specification faithfully.

### Step 4: Validate and Commit
Execute tests in an isolated environment. Commit using the canonical message format:

```
<scope>: <subject> (closes ADR-[XXX])

Implements decision documented in Plan/decisions/ADR-[XXX].md (status=accepted)
Evidence: <test runner command and exit-code 0 output>
```
