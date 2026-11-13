# 🧠⚡ 01 · The Dual-Agent Workflow

*Structural role separation: reflective mode versus executive mode*

> [!NOTE]
> This document instantiates EEE's roles (*who decides*, *who builds*, *who verifies*) with two AI agents. The foundations (hierarchy of truth, DONE stack, rules R1–R30, evolutionary construction) are in [EEEMIAD](https://github.com/fuzaro/EEEMIAD) and are not repeated here.

---

## 1. The premise

The same language model exhibits radically different operational postures depending on five factors: accumulated session history, available tools, the operator's usage pattern, the visible feedback loop and the explicit mode (plan × edit).

- **Reflective mode:** long, planning-oriented sessions with cross-cutting view, explicit critique and trade-off weighing.
- **Executive mode:** sessions focused on code, terminal, surgical diffs, tests and refactoring.

Instead of forcing one agent to be "average at everything", the **division into two roles with explicit handoff** is instrumented.

```mermaid
sequenceDiagram
    autonumber
    participant Op as Human operator
    participant R as 🧠 Reflective agent
    participant Git as 📜 Repository (ADR / Spec)
    participant E as ⚡ Executive agent
    participant Run as 🏗️ Runtime / tests
    Op->>R: 1. Architecture or change request
    R->>R: 2. Impact analysis and alternatives (≥ 2)
    R->>Git: 3. ADR PROPOSED; ratification in a separate artefact
    R->>E: 4. Brief with pre-flight (blast radius, precondition, composition)
    Note over E: Mandatory gate: reality-check before editing
    E->>Run: 5. Probes real surfaces (no edits)
    E->>R: 6. Reality-check: GREENLIGHT or drift report
    alt material drift
        R->>Git: amends ADR / spec before coding
    else terrain confirmed
        E->>Run: 7. Implements; tests with disagreeable pair
        E->>Git: 8. Commit "closes ADR-xxx" with command and output
        E->>R: 9. Reports measurement
    end
    R->>Git: 10. Decision-by-decision compliance evaluation
```

## 2. Role division

| Dimension | 🧠 Reflective | ⚡ Executive |
|---|---|---|
| **Works on** | ADRs, specs, strategy, critical analysis of plans, methodological documentation, longitudinal evaluation | Implementation of accepted spec, refactoring within spec, bug fix with identified cause, tests for existing spec, build, migrations derived from accepted schema |
| **Posture** | Sceptical, adversarial, reflection before action, explicit risks and consequences | Surgical, disciplined, tool-driven, code evidence |
| **Write permission** | `Plan/`, `reality/`, `findings/`, ADR drafts. **Never edits production code.** | Code, scripts, infrastructure, tests |
| **First artefact of the session** | Premise mapping and affected invariants | **Reality-check** against real code and runtime |

### Grey zone

| Situation | Who starts |
|---|---|
| Vague spec, decisions needed | Reflective first, then executive |
| Clear spec, implementation with technical alternatives | Reflective sketches, executive implements |
| New architectural thing | **Always** reflective first |
| Production bug without known root cause | Executive investigates; reflective decides whether an ADR is required |
| Small feature with implicit but clear spec | Executive directly |
| Rename or move without semantic change | Executive directly |

## 3. The reality-check gate

An executive session **does not start by editing files**. The first artefact proves that the brief's anchors exist in the live code:

- Function names, symbols, schemas and variables cited are **hypotheses to verify, never facts**. An anchor is a symbol; a line is a convenience (R14).
- **The brief's state claims are anchors too**: "X already applied", "infra stable". The reflective agent verifies its own premises; the executive's reality-check is the safety net.
- If an intermediate commit moved a function or changed a schema, implementation **stops**: a *drift report* goes to the reflective agent.
- **Before briefing a phase, verify that the phase mechanism exists** (R26): naming "flip to enforce" in an ADR does not build the enforce path.

## 4. The brief and the pre-flight

Every brief touching production (deploy, migration, credential, flag) carries the blast-radius block **at the top**:

```markdown
### Blast radius (mandatory pre-flight)
1. Side effects beyond the declared target (credentials, cluster state, destructive data): ______
2. Reversible? [YES/NO]. If NO → STOP; requires explicit per-action human decision.
3. Rehearsed rollback: snapshot / previous key exists BEFORE running? Recovery time? ______
4. Flip composition: which OTHER gates changed in this window? Was the PRODUCT exercised? (command + result) ______
5. State precondition: does it depend on a tuple, grant, credential or flag that does not yet exist?
   - Verification command: ______
   - Observed result:      ______
```

> [!CAUTION]
> **A brief without the block filled in is not a brief.** The executive refuses before starting, naming the empty field. The pre-flight belongs to whoever decides, not to whoever executes.

Brief rules:
- **Property, not mechanism (R11).** The brief states what must hold and the refutation condition; the query belongs to the executive.
- **Negative control against drag.** Every change that loosens validation carries the neighbouring case that must stay barred.
- **Round-trip calibration.** The number of code-free round trips scales with the blast radius: reversible, fail cheaply; irreversible, full certainty before touching.

## 5. Handoff artefacts

### Reflective → executive
| Reflective output | Executive input |
|---|---|
| Accepted ADR (`ACCEPTED`) | Implementation |
| Approved spec | Code |
| Refactoring plan | Step-by-step execution |
| Test plan | Writing the tests |
| Acceptance criterion | Post-implementation validation |

### Executive → reflective
| Executive output | Reflective input |
|---|---|
| Complete implementation with measurement | **ADR compliance evaluation** |
| Iteration logs and queries | Findings |
| Build or test anomalies | Analysis of whether a new ADR is needed |
| Unresolved conflicts | Architectural decision |

### Canonical commit

```
<scope>: <subject> (closes ADR-XXX)

Implements decision documented in Plan/decisions/ADR-XXX.md (status=ACCEPTED)
Alternatives considered: <summary>
Consequences accepted: <summary>
Evidence: <test command and output, exit code>
```

## 6. Compliance evaluation

After delivery, the reflective agent **does not perform a generic review**. It audits the ADR clause by clause: if there are seven decisions (D1 to D7), the evaluation has seven rows with typed verdicts:

| Verdict | Meaning |
|---|---|
| `MET` | Evidence by command or verified symbol |
| `PARTIAL` | Partly satisfied; what is missing is named |
| `NOT-MET` | Violated or omitted |
| `NOT-CERTIFIABLE` | The evaluator cannot reach the environment to measure; **inference forbidden** |

The footer declares the **evaluation level**: reported by the executor (level 2) or measured by the evaluator (level 4).

> [!NOTE]
> **Why does the executor not evaluate its own work?** Whoever implements has the same blind spots that produced the defects. When the executor starts committing directly, the human who read every diff leaves the loop by a change of topology, not by decision; the compliance evaluation **restores that control**. It is not ceremony.

Template: [`templates/en/evaluation-ledger-template.md`](../../templates/en/evaluation-ledger-template.md).

## 7. Handoff anti-patterns

- **Universal workflow:** one agent trying to be good at everything tends to be average at everything.
- **Brief pinning an anchor as fact:** every anchor is a pointer to verify.
- **Brief without state precondition:** three consecutive briefs assuming a credential nobody verified.
- **Generic evaluation:** "code looks good" instead of D1..Dn with a verdict.
- **Prescribing mechanism instead of property:** including prescribing *how to measure*.
- **Executor deciding architecture implicitly:** topology fixed in an implementation plan without an ADR.

## 8. One-sentence summary

**Use the reflective agent to decide what to build. Use the executive agent to build what was decided. ADRs and specs are the handoff currency between them.**
