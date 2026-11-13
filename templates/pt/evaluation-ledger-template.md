# 🧾 AVALIAÇÃO DE CUMPRIMENTO · ADR-[XXX]

*Livro-razão de auditoria decisão por decisão*

```yaml
target_adr: ADR-[XXX]
implementation_commit: [sha_do_commit]
evaluator: Agente Reflexivo 
evaluation_level: [Nivel 2 (Reportado pelo Executor) | Nivel 4 (Medido Pelo Avaliador)]
date: YYYY-MM-DD
final_tier: [done@estrutural | done@funcional | REJEITADO]
```

---

## 1. Veredito Cláusula por Cláusula

| Cláusula da ADR | Veredito | Evidência Comprovada | Comentários & Gaps |
|---|:---:|---|---|
| **D1:** [Título da decisão] | `CUMPRIDA` | `service/routes.py::require_bearer` | Validação de bearer implementada conforme contrato. |
| **D2:** [Título da decisão] | `PARCIAL` | `auth/jwt.py::verify_audience` | Valida audiência, mas omitiu suporte a tokens revogados. |
| **D3:** [Título da decisão] | `NÃO-CUMPRIDA` | — | Nenhuma tabela criada para persistência de sessões. |
| **D4:** [Título da decisão] | `NÃO-CERTIFICÁVEL` | `staging cluster` | Cluster inacessível a partir desta sessão para medir latência. |

---

## 2. Balanço do Livro-Razão
- **Total de Decisões Auditadas:** `[N]`
- **Decisões Cumpridas:** `[C]`
- **Decisões com Pendência (Parcial / Não-Cumprida):** `[P]`

---

## 3. Próximo Passo Acionável
> [Definição única e clara: promover para done@funcional ou emitir novo Brief corretivo].
