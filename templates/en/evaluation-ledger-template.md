# 🧾 COMPLIANCE EVALUATION · ADR-[XXX]

*Decision-by-decision audit ledger*

```yaml
target_adr: ADR-[XXX]
implementation_commit: [commit_sha]
evaluator: Reflective Agent 
evaluation_level: [Level 2 (Reported by Executor) | Level 4 (Directly Measured by Evaluator)]
date: YYYY-MM-DD
final_tier: [done@structural | done@functional | REJECTED]
```

---

## 1. Decision-by-Decision Verdict

| ADR Decision Clause | Verdict | Verified Evidence | Observations & Gaps |
|---|:---:|---|---|
| **D1:** [Decision Title] | `MET` | `service/routes.py::require_bearer` | Bearer token validation implemented as specified. |
| **D2:** [Decision Title] | `PARTIAL` | `auth/jwt.py::verify_audience` | Audience validated, but omitted revoked token check. |
| **D3:** [Decision Title] | `NOT-MET` | — | No database table created for session persistence. |
| **D4:** [Decision Title] | `NOT-CERTIFIABLE` | `staging cluster` | Cluster unreachable from this session to benchmark latency. |

---

## 2. Ledger Balance
- **Total Decisions Audited:** `[N]`
- **Decisions Met:** `[M]`
- **Decisions Pending (Partial / Not-Met):** `[P]`

---

## 3. Delegation record (only with `delegation: tiered`)

One row per task packet. Without context-level division, delete this section.

| Packet | Level | Executor (model) | Checker (model) | Gate on 1st | Attempts | Escalated to | Cost |
|---|---|---|---|:---:|:---:|---|---|
| `ARC-XXX-T01` | local | eee-local-executor (haiku) | eee-local-checker (sonnet) | ✅ | 1 | — | [tokens] |

- **Local-level escalation rate:** `[escalated / local packets]` · limit declared in the charter: `[__ %]`
- Above the limit: revise the classification rule, not the models.

---

## 4. Actionable Next Step
> [Single, unambiguous next action: promote to done@functional or issue corrective Brief].
