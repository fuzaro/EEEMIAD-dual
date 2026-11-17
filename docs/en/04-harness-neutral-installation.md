# 🧩 04 · Harness-neutral installation

*How to install this repository's roles, skills and agents in a project so that another tool, another model or another person can continue the work from the repository and not from one session's memory*

> [!NOTE]
> The principle (permanent journal, operational state, `AGENTS.md`, thin adapters, single gate) is in [EEEMIAD · 08 §8](https://github.com/fuzaro/EEEMIAD/blob/main/docs/en/08-application-guide.md#8-continuity-journal-state-and-single-gate). This is the concrete installation of this repository's pieces, learned in a validation by use where the project's state lived only in one Claude Code session's local memory.

---

## 1. The problem

This repository's skills and agents are written for Claude Code: they name tools (`Read`, `Edit`, `Bash`) and models (`haiku`, `sonnet`). Copied as they are into a project, they work in one tool and are unreadable to the others. And what the session learns (flags that are on, credentials and where they live, the operator's decisions) stays in its memory.

## 2. The installation

```text
project/
├── AGENTS.md                      canonical entry point (template in EEEMIAD)
├── CLAUDE.md                      adapter: @AGENTS.md
├── eee/
│   ├── playbooks/                 this repository's skills rewritten without tool or model names
│   │   ├── architect.md  executor.md  gate-checker.md  task-router.md  cross-session.md
│   ├── state/                     STATE.md (dated), operator decisions, environments (where each credential lives)
│   ├── kit/                       templates from EEEMIAD and from this repository, with VERSION (origin + version)
│   └── log.md                     permanent evidence journal
├── .claude/
│   ├── skills/eee-*/SKILL.md      adapters (≤ 20 lines) → eee/playbooks/*
│   └── agents/eee-*.md            adapters: Claude Code model and tools → eee/playbooks/*
└── tools/gate.sh                  single gate
```

| Piece of this repository | In the project (canonical) | Claude Code adapter |
|---|---|---|
| `skills/eee-architect` | `eee/playbooks/architect.md` | `.claude/skills/eee-architect/SKILL.md` |
| `skills/eee-executor` | `eee/playbooks/executor.md` | `.claude/skills/eee-executor/SKILL.md` |
| `skills/eee-gate-checker` | `eee/playbooks/gate-checker.md` | `.claude/skills/eee-gate-checker/SKILL.md` |
| `skills/eee-task-router` | `eee/playbooks/task-router.md` | `.claude/skills/eee-task-router/SKILL.md` |
| `docs/*/03` | `eee/playbooks/cross-session.md` | `.claude/skills/eee-cross-session/SKILL.md` |
| `agents/eee-*.md` | the role lives in the playbook | `.claude/agents/eee-*.md` (only `model`, `tools` and the pointer) |
| `templates/*` | `eee/kit/templates/` + `eee/kit/VERSION` | — |

## 3. Rewriting a skill as a playbook

- Replace the tool name with the action: "read", "run", "edit", "ask the operator".
- Replace the model name with the context level: small, intermediate, large. The mapping to models lives **only** in the adapter.
- Replace this repository's paths with the project's (`eee/kit/templates/...`).
- Add what the project has already learned (e.g. mistakes already made across sessions). The playbook belongs to the project; the original stays here.

## 4. The adapter

```markdown
---
name: eee-executor
description: EEE role of who builds: reality-check, red before green, disagreeable pair, probe-and-revert.
---

Claude Code adapter. Follow `eee/playbooks/executor.md`.
First: `AGENTS.md` and `eee/state/STATE.md`.
```

An agent also carries the harness configuration (`model`, `tools`). No rule, procedure or state: the project's gate fails an adapter longer than 20 lines, without a pointer to `AGENTS.md`/`eee/`, or with a dead path.

## 5. Session memory and project state

- The agent's memory is a **shortcut**: it points to `eee/state/STATE.md` and `eee/log.md`. When they conflict, the repository wins.
- **At the end of each state change** (arc, flip, deploy, decision), the session updates `STATE.md` and writes the journal line in the same commit. Not at the end of the session: a session can end without warning.
- In sessions working together (doc 03), the facts about the remote environment (where the token is, how to load it) go into `eee/state/environments/`, never the value.

## 6. Gate

- The executor runs `tools/gate.sh` before delivering; the checker runs the packet's gate, a subset of the single gate; whoever decides runs the single gate at the arc's gate points.
- A gate that skips for lack of a dependency is a false green: the step fails.

## 7. Anti-patterns

- **State only in memory:** the project can only continue in the same session, on the same machine.
- **Fat adapter:** the same rule in `CLAUDE.md`, `.cursor/rules` and `AGENTS.md`, each in its own version.
- **Playbook with tool names:** another tool reads it and does not know what to do.
- **Copy without version:** the copied template or script drifts from the original and nobody notices.

## 8. Installation checklist

```markdown
- [ ] AGENTS.md at the root, from the EEEMIAD template
- [ ] eee/playbooks/ with the roles in use, without tool or model names
- [ ] eee/state/STATE.md dated; operator decisions; environments without secret values
- [ ] eee/kit/VERSION with origin and version of each copied template and tool
- [ ] Adapters ≤ 20 lines, pointers only; the gate checks
- [ ] tools/gate.sh runs everything and fails on a missing dependency
- [ ] Session memory points to the repository
```
