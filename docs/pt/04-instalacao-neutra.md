# 🧩 04 · Instalação neutra quanto ao harness

*Como instalar os papéis, as skills e os agentes deste repositório num projeto, de modo que outra ferramenta, outro modelo ou outra pessoa continue o trabalho a partir do repositório e não da memória de uma sessão*

> [!NOTE]
> O princípio (diário permanente, estado operacional, `AGENTS.md`, adaptadores finos, gate único) está em [EEEMIAD · 08 §8](https://github.com/fuzaro/EEEMIAD/blob/main/docs/pt/08-guia-de-aplicacao.md#8-continuidade-diário-estado-e-gate-único). Aqui está a instalação concreta das peças deste repositório, aprendida numa validação por uso em que o estado do projeto vivia só na memória local de uma sessão do Claude Code.

---

## 1. O problema

As skills e os agentes deste repositório são escritos para o Claude Code: citam ferramentas (`Read`, `Edit`, `Bash`) e modelos (`haiku`, `sonnet`). Copiados tal qual para um projeto, eles funcionam numa ferramenta e são ilegíveis para as demais. E o que a sessão aprende (flags ligadas, credenciais e onde vivem, decisões do operador) fica na memória dela.

## 2. A instalação

```text
projeto/
├── AGENTS.md                      entrada canônica (template no EEEMIAD)
├── CLAUDE.md                      adaptador: @AGENTS.md
├── eee/
│   ├── playbooks/                 as skills deste repositório reescritas sem nome de ferramenta ou modelo
│   │   ├── architect.md  executor.md  gate-checker.md  task-router.md  cross-session.md
│   ├── state/                     STATE.md (datado), decisões do operador, ambientes (onde vive cada credencial)
│   ├── kit/                       templates do EEEMIAD e deste repositório, com VERSION (origem + versão)
│   └── log.md                     diário de evidências permanente
├── .claude/
│   ├── skills/eee-*/SKILL.md      adaptadores (≤ 20 linhas) → eee/playbooks/*
│   └── agents/eee-*.md            adaptadores: modelo e ferramentas do Claude Code → eee/playbooks/*
└── tools/gate.sh                  gate único
```

| Peça deste repositório | No projeto (canônico) | Adaptador do Claude Code |
|---|---|---|
| `skills/eee-architect` | `eee/playbooks/architect.md` | `.claude/skills/eee-architect/SKILL.md` |
| `skills/eee-executor` | `eee/playbooks/executor.md` | `.claude/skills/eee-executor/SKILL.md` |
| `skills/eee-gate-checker` | `eee/playbooks/gate-checker.md` | `.claude/skills/eee-gate-checker/SKILL.md` |
| `skills/eee-task-router` | `eee/playbooks/task-router.md` | `.claude/skills/eee-task-router/SKILL.md` |
| `docs/*/03` | `eee/playbooks/cross-session.md` | `.claude/skills/eee-cross-session/SKILL.md` |
| `agents/eee-*.md` | o papel fica no playbook | `.claude/agents/eee-*.md` (só `model`, `tools` e o ponteiro) |
| `templates/*` | `eee/kit/templates/` + `eee/kit/VERSION` | — |

## 3. Reescrever uma skill como playbook

- Troque o nome da ferramenta pela ação: "leia", "rode", "edite", "pergunte ao operador".
- Troque o nome do modelo pelo nível de contexto: pequeno, intermediário, grande. O mapeamento para modelos fica **só** no adaptador.
- Troque caminhos deste repositório pelos do projeto (`eee/kit/templates/...`).
- Acrescente o que o projeto já aprendeu (ex.: erros já cometidos entre sessões). O playbook é do projeto; o original continua aqui.

## 4. O adaptador

```markdown
---
name: eee-executor
description: Papel EEE de quem constrói: reality-check, vermelho antes do verde, par discordável, probe-and-revert.
---

Adaptador do Claude Code. Siga `eee/playbooks/executor.md`.
Antes: `AGENTS.md` e `eee/state/STATE.md`.
```

Um agente leva também a configuração do harness (`model`, `tools`). Nada de regra, procedimento ou estado: o gate do projeto reprova adaptador com mais de 20 linhas, sem ponteiro para `AGENTS.md`/`eee/` ou com caminho morto.

## 5. Memória da sessão e estado do projeto

- A memória do agente é **atalho**: aponta para `eee/state/STATE.md` e `eee/log.md`. Quando conflitam, o repositório vale.
- **Ao fim de cada mudança de estado** (arco, flip, deploy, decisão), a sessão atualiza `STATE.md` e escreve a linha do diário no mesmo commit. Não deixa para o fim da sessão: uma sessão pode acabar sem aviso.
- Em sessões que trabalham juntas (doc 03), os fatos do ambiente remoto (onde está o token, como carregá-lo) vão para `eee/state/environments/`, nunca o valor.

## 6. Gate

- O executor roda `tools/gate.sh` antes de entregar; o conferente roda o gate do pacote, que é um subconjunto do gate único; quem decide roda o gate único nos pontos de gate do arco.
- Um gate que pula por falta de dependência é falso verde: o passo reprova.

## 7. Anti-padrões

- **Estado só na memória:** o projeto só continua na mesma sessão, na mesma máquina.
- **Adaptador gordo:** a mesma regra em `CLAUDE.md`, `.cursor/rules` e `AGENTS.md`, cada uma numa versão.
- **Playbook com nome de ferramenta:** outra ferramenta lê e não sabe o que fazer.
- **Cópia sem versão:** o template ou o script copiado diverge do original e ninguém percebe.

## 8. Checklist de instalação

```markdown
- [ ] AGENTS.md na raiz, a partir do template do EEEMIAD
- [ ] eee/playbooks/ com os papéis em uso, sem nome de ferramenta ou modelo
- [ ] eee/state/STATE.md datado; decisões do operador; ambientes sem valor secreto
- [ ] eee/kit/VERSION com origem e versão de cada template e ferramenta copiados
- [ ] Adaptadores ≤ 20 linhas, só ponteiros; o gate verifica
- [ ] tools/gate.sh roda tudo e reprova dependência ausente
- [ ] Memória da sessão aponta para o repositório
```
