# 🧠⚡ 01 · O Fluxo de Trabalho de Dois Agentes

*Divisão estrutural de papéis: modo reflexivo versus modo executivo*

> [!NOTE]
> Este documento instancia os papéis da EEE (*quem decide*, *quem constrói*, *quem verifica*) com dois agentes de IA. Os fundamentos (hierarquia da verdade, pilha de DONE, regras R1–R30, construção evolutiva) estão em [EEEMIAD](https://github.com/fuzaro/EEEMIAD) e não são repetidos aqui.

---

## 1. A premissa

O mesmo modelo de linguagem exibe posturas operacionais radicalmente diferentes conforme cinco fatores: histórico acumulado da sessão, ferramentas disponíveis, padrão de uso do operador, laço de feedback visível e modo explícito (plano × edição).

- **Modo reflexivo:** sessões longas, orientadas a planejamento, com visão transversal, crítica explícita e ponderação de trade-offs.
- **Modo executivo:** sessões focadas em código, terminal, diffs cirúrgicos, testes e refatoração.

Em vez de forçar um único agente a ser "médio em tudo", instrumenta-se a **divisão em dois papéis com handoff explícito**.

```mermaid
sequenceDiagram
    autonumber
    participant Op as Operador humano
    participant R as 🧠 Agente reflexivo
    participant Git as 📜 Repositório (ADR / Spec)
    participant E as ⚡ Agente executivo
    participant Run as 🏗️ Runtime / testes
    Op->>R: 1. Demanda de arquitetura ou mudança
    R->>R: 2. Análise de impacto e alternativas (≥ 2)
    R->>Git: 3. ADR PROPOSED; ratificação em artefato separado
    R->>E: 4. Brief com pré-voo (raio de explosão, precondição, composição)
    Note over E: Gate obrigatório: reality-check antes de editar
    E->>Run: 5. Sonda superfícies reais (sem editar)
    E->>R: 6. Reality-check: GREENLIGHT ou drift report
    alt drift material
        R->>Git: emenda ADR / spec antes de codar
    else terreno confirmado
        E->>Run: 7. Implementa; testes com par discordável
        E->>Git: 8. Commit "closes ADR-xxx" com comando e saída
        E->>R: 9. Reporta medição
    end
    R->>Git: 10. Avaliação de cumprimento decisão por decisão
```

## 2. Divisão de papéis

| Dimensão | 🧠 Reflexivo | ⚡ Executivo |
|---|---|---|
| **Atua em** | ADRs, specs, estratégia, análise crítica de planos, documentação metodológica, avaliação longitudinal | Implementação de spec aceita, refatoração dentro de spec, correção de bug com causa identificada, testes para spec existente, build, migrações derivadas de schema aceito |
| **Postura** | Cética, adversarial, reflexão antes da ação, riscos e consequências explícitos | Cirúrgica, disciplinada, guiada por ferramentas, evidência de código |
| **Permissão de escrita** | `Plan/`, `reality/`, `achados/`, minutas de ADR. **Nunca edita código de produção.** | Código, scripts, infraestrutura, testes |
| **Primeiro artefato da sessão** | Mapeamento de premissas e invariantes afetadas | **Reality-check** contra código e runtime reais |

### Zona cinzenta

| Situação | Quem começa |
|---|---|
| Spec vaga, precisa de decisões | Reflexivo primeiro, depois executivo |
| Spec clara, implementação com alternativas técnicas | Reflexivo esboça, executivo implementa |
| Coisa nova arquitetural | **Sempre** reflexivo primeiro |
| Bug em produção sem causa raiz conhecida | Executivo investiga; reflexivo decide se requer ADR |
| Feature pequena com spec implícita mas clara | Executivo direto |
| Renomeação ou movimentação sem mudança semântica | Executivo direto |

## 3. O gate do reality-check

Uma sessão executiva **não começa editando arquivos**. O primeiro artefato prova que as âncoras do brief existem no código vivo:

- Nomes de função, símbolos, schemas e variáveis citados são **hipóteses a verificar, nunca fatos**. Âncora é símbolo; linha é conveniência (R14).
- **As afirmações de estado do brief também são âncoras**: "X já aplicado", "infra estável". O reflexivo verifica as próprias premissas; o reality-check do executivo é a rede.
- Se um commit intermediário moveu uma função ou alterou um schema, a implementação **para**: emite-se um *drift report* para o reflexivo.
- **Antes de briefar uma fase, verificar que o mecanismo da fase existe** (R26): nomear "flip para enforce" numa ADR não constrói o caminho de enforce.

## 4. O brief e o pré-voo

Todo brief que toque produção (deploy, migração, credencial, flag) carrega **no topo** o bloco de raio de explosão:

```markdown
### Raio de explosão (pré-voo obrigatório)
1. Efeitos colaterais além do alvo declarado (credenciais, estado de cluster, dados destrutivos): ______
2. Reversível? [SIM/NÃO]. Se NÃO → STOP; exige decisão humana explícita por ação.
3. Rollback ensaiado: snapshot / chave anterior existe ANTES de rodar? Tempo de recuperação? ______
4. Composição de flips: quais OUTROS gates mudaram nesta janela? O PRODUTO foi exercitado? (comando + resultado) ______
5. Precondição de estado: depende de tupla, grant, credencial ou flag que ainda não existe?
   - Comando que verifica: ______
   - Resultado observado:  ______
```

> [!CAUTION]
> **Brief sem o bloco preenchido não é brief.** O executivo recusa antes de começar, citando o campo vazio. O pré-voo é de quem decide, não de quem executa.

Regras do brief:
- **Propriedade, não mecanismo (R11).** O brief especifica o que deve valer e a condição de refutação; a consulta é do executivo.
- **Controle negativo contra arrasto.** Toda mudança que afrouxa validação carrega o caso vizinho que deve seguir barrado.
- **Calibragem de idas e voltas.** O número de round-trips sem código escala com o raio de explosão: reversível, erra barato; irreversível, certeza integral antes de tocar.

## 5. Artefatos de handoff

### Reflexivo → executivo

| Saída do reflexivo | Entrada do executivo |
|---|---|
| ADR aceita (`ACCEPTED`) | Implementação |
| Spec aprovada | Código |
| Plano de refatoração | Execução por etapas |
| Plano de testes | Escrita dos testes |
| Critério de aceite | Validação pós-implementação |

### Executivo → reflexivo

| Saída do executivo | Entrada do reflexivo |
|---|---|
| Implementação completa com medição | **Avaliação de cumprimento da ADR** |
| Logs e consultas de iteração | Redação de achados |
| Anomalias em build ou teste | Análise se requer nova ADR |
| Conflitos não resolvidos | Decisão arquitetural |

### Commit canônico

```
<scope>: <subject> (closes ADR-XXX)

Implements decision documented in Plan/decisions/ADR-XXX.md (status=ACCEPTED)
Alternatives considered: <resumo>
Consequences accepted: <resumo>
Evidence: <comando de teste e saída, exit code>
```

## 6. Avaliação de cumprimento

Após a entrega, o reflexivo **não faz revisão genérica**. Audita cláusula por cláusula da ADR: se há sete decisões (D1 a D7), a avaliação tem sete linhas com vereditos tipados:

| Veredito | Significado |
|---|---|
| `CUMPRIDA` | Evidência por comando ou símbolo verificado |
| `PARCIAL` | Parte satisfeita; o que falta é nomeado |
| `NÃO-CUMPRIDA` | Violada ou omitida |
| `NÃO-CERTIFICÁVEL` | O avaliador não alcança o ambiente para medir; **proibido inferir** |

O rodapé declara o **nível da avaliação**: reportado pelo executor (nível 2) ou medido pelo avaliador (nível 4).

> [!NOTE]
> **Por que o executor não avalia o próprio trabalho?** Quem implementa tem os mesmos pontos cegos que geraram os defeitos. Quando o executor passa a commitar direto, o humano que lia cada diff sai do laço por mudança de topologia, não por decisão; a avaliação de cumprimento **repõe esse controle**. Não é cerimônia.

Template: [`templates/pt/evaluation-ledger-template.md`](../../templates/pt/evaluation-ledger-template.md).

## 7. Anti-padrões do handoff

- **Workflow universal:** um agente que tenta ser bom em tudo tende a ser médio em tudo.
- **Brief que crava âncora como fato:** toda âncora é ponteiro a verificar.
- **Brief sem precondição de estado:** três briefs consecutivos assumindo credencial que ninguém verificou.
- **Avaliação genérica:** "código parece bom" em vez de D1..Dn com veredito.
- **Prescrever mecanismo em vez de propriedade:** inclusive prescrever *como medir*.
- **Executor que decide arquitetura implicitamente:** topologia fixada num plano de implementação sem ADR.

## 8. Resumo em uma frase

**Use o reflexivo para decidir o que construir. Use o executivo para construir o que foi decidido. ADRs e specs são a moeda de handoff entre eles.**
