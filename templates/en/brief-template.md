# 📨 BRIEF-[XXX] · [Execution task title]

*Handoff: reflective agent (decides) → executive agent (builds)*

```yaml
target_adr: ADR-[XXX] (status=accepted)
target_spec: specs/[service].md
risk_profile: [LOW | MEDIUM | CRITICAL_DESTRUCTIVE]
expected_done_tier: done@functional
date: YYYY-MM-DD
```

---

## ⚠️ BLAST RADIUS & PRE-FLIGHT (§5.4) — MANDATORY GATE

> [!CAUTION]
> The Executive Agent is strictly forbidden from editing code before verifying and answering this block.

1. **Side-Effects:** What mutates beyond the declared target files? (Tables, keys, clusters, schemas):
   - `______`
2. **Reversible?** [YES / NO]. If NO → STOP and request explicit human authorization.
3. **Rehearsed Rollback:** Do snapshots, backups, or previous keys exist BEFORE running? Recovery time?
   - `______`
4. **Flip Composition:** What other gates were flipped in this window? Was the composed product tested?
   - `______`
5. **State Precondition:** Does this task depend on a tuple, grant, or flag that does not yet exist?
   - Verification command: `______`
   - Observed result:     `______`
6. **Target State:** Is the target environment in a state the tests never exercised (empty, just migrated, old data outside a new rule)? If so, the battery runs from that state.
   - `______`
7. **Live Reproduction:** Read or write? A write into business data only with data marked as test and removable; otherwise read only.
   - `______`
8. **Request Item:** Which item of the original request (request × arc matrix) does this brief serve?
   - `______`

---

## 1. Context and Surgical Objective
- **Referenced ADR:** `Plan/decisions/ADR-[XXX].md`
- **Scope to build:** [Unambiguous description of the vertical slice].

---

## 2. Code Anchors (Verify in Reality-Check, Do Not Trust Blindly)
- `path/to/file.py:lines` — [Function to modify]
- `path/to/schema.sql:lines` — [Table to extend]

---

## 3. Objective Acceptance Criteria
1. [ ] Reality-Check completed with zero unhandled material drifts.
2. [ ] Isolated unit test with negative control (Rule R2) passing green.
3. [ ] Zero AST syntactic violations against security invariants.
4. [ ] Exit code 0 across targeted test suite.
5. [ ] Commit follows canonical format: `<scope>: <subject> (closes ADR-[XXX])`.
