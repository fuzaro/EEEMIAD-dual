# 📨 BRIEF-[XXX] · [Título da tarefa de execução]

*Handoff: agente reflexivo (decide) → agente executivo (constrói)*

```yaml
target_adr: ADR-[XXX] (status=accepted)
target_spec: specs/[servico].md
risk_profile: [BAIXO | MEDIO | CRITICO_DESTRUTIVO]
expected_done_tier: done@funcional
date: YYYY-MM-DD
```

---

## ⚠️ RAIO DE EXPLOSÃO & PRÉ-VOO (§5.4) — GATE OBRIGATÓRIO

> [!CAUTION]
> O Agente Executivo está expressamente proibido de editar código antes de validar e responder este bloco.

1. **Efeitos Colaterais:** O que muda além dos arquivos declarados? (Tabelas, credenciais, clusters):
   - `______`
2. **Reversível?** [SIM / NÃO]. Se NÃO → Parar e solicitar aprovação humana explícita.
3. **Rollback Ensaiado:** Snapshot, backup ou flag anterior existem ANTES de rodar? Tempo de recuperação?
   - `______`
4. **Composição de Flips:** Quais outros gates foram flipados nesta mesma janela? O produto foi testado?
   - `______`
5. **Precondição de Estado:** Este item depende de tupla, grant ou flag que ainda não existe?
   - Comando que verifica: `______`
   - Resultado observado:  `______`

---

## 1. Contexto e Objetivo Cirúrgico
- **ADR Referenciada:** `Plan/decisions/ADR-[XXX].md`
- **O que deve ser construído:** [Descrição exata da fatia vertical sem ambiguidade].

---

## 2. Âncoras de Código (A Verificar no Reality-Check, Não Confiar)
- `caminho/do/arquivo.py:linhas` — [Função a modificar]
- `caminho/do/schema.sql:linhas` — [Tabela a estender]

---

## 3. Critérios de Aceite Objetivos
1. [ ] Reality-Check concluído com relatório emitido e sem drifts materiais.
2. [ ] Teste unitário estanque com controle negativo (Regra R2) passando verde.
3. [ ] Nenhuma violação sintática AST nas invariantes de segurança.
4. [ ] Exit code 0 na suíte de testes.
5. [ ] Commit seguindo o padrão canônico: `<scope>: <subject> (closes ADR-[XXX])`.
