# ROADMAP · EEEMIAD-dual

## v0.1.0 — Bases (2026-11-13) ✅
- [x] Documento do padrão dual-agent (PT/EN), movido de EEEMIAD e reescrito sem exemplos específicos de plataforma.
- [x] Templates de brief e de avaliação de cumprimento (PT/EN).
- [x] Skills `eee-architect` e `eee-executor` com prompts de sessão.

## v0.2.0 — Delegação por nível de contexto (2026-11-14) ✅
- [x] Guia opcional `docs/{pt,en}/02-*`: ativação por resposta positiva na fase de perguntas, três níveis, regra de classificação, laço de uma tarefa local, medição, anti-padrões.
- [x] Agentes `eee-local-executor` (haiku), `eee-local-checker` (sonnet), `eee-module-builder` (sonnet), `eee-oracle-writer` (modelo diferente do builder).
- [x] Skills `eee-task-router` (pergunta, classifica, despacha, registra) e `eee-gate-checker` (gate antes da leitura, sem edição).
- [x] `eee-architect` passo 0 (pergunta) e `eee-executor` modo pacote; seção "Registro de delegação" no ledger de avaliação.
- [ ] Primeira medição real: taxa de escalonamento e custo por nível num arco com gates mecânicos.

## v0.3.0 — Comunicação entre sessões (2026-11-15) ✅
- [x] Guia `docs/{pt,en}/03-*`: quando usar, ligar e encontrar a sessão, pacote fechado, duas chaves, segredo que não viaja, reinício da sessão remota, protocolo de correção, verificação do que volta, anti-padrões.
- [x] Template de mensagem entre sessões (PT/EN) com correção e registro no charter.
- [x] `eee-task-router`: critério 0, pacote remoto (`executes_at`, `action`).

## v0.4.0 — Profundidade
- [ ] Capítulo "Zona cinzenta": tabela de quem começa (spec vago, bug sem causa raiz, feature pequena, renomeação).
- [ ] Capítulo "Como preservar o modo reflexivo": prompt inicial, skills a habilitar e a evitar, padrão de resposta.
- [ ] Capítulo "Como disciplinar o modo executivo": ADR-first, plan mode, contexto inicial, reality-check como gate.
- [ ] Anti-padrões específicos do handoff (workflow universal, brief que crava âncora como fato, avaliação genérica em vez de cláusula por cláusula, brief sem precondição de estado).
- [ ] Exemplo ponta a ponta: ADR → brief → reality-check → implementação → avaliação, com artefatos reais e genéricos.

## v0.5.0 — Medição
- [ ] Métricas do padrão: taxa de retrabalho, ADRs sem implementação, drifts pegos pelo RC, tempo de ratificação.
- [ ] Quando reavaliar o padrão (mudanças nos agentes, modo plano persistente, outro agente no papel reflexivo).
