# claude-code/ — Claude Code 与 Codex CLI 专属

> 这里的东西**只有当你使用 Claude Code 或 Codex CLI 这类支持 SKILL/agent 协议的 agent 客户端时才有用**。
> 不用 CC 也能玩 `core/`,只是少了 skill 自动发现 + sub-agent 角色分发的便利。

## 文件去向

把整个目录的子文件夹按下面位置拷到你的项目仓库根:

| 来源 | 放到目标仓的 | 客户端 |
|---|---|---|
| `skills/` | `.claude/skills/`  | Claude Code |
| `skills/` | `.codex/skills/`   | Codex CLI |
| `agents/` | `.claude/agents/`  | Claude Code |
| `agents/` | `.codex/agents/`   | Codex CLI |
| `CLAUDE.md.template` | `CLAUDE.md`(或 `AGENTS.md` 给 Codex) | 都用 |

`tools/install.sh` 会自动放到 `.claude/`(Claude Code 默认),若你用 Codex 用 `--codex` 参数。

## skills(6 个)

每个都是 `<name>/SKILL.md`,顶部 YAML front-matter 让 agent 自动按描述触发:

- `architect-task-writer` — 把模糊想法变成结构化任务 prompt
- `pr-investigator` — 对自动建的工单做根因调查
- `feature-flag-setup` — 加新功能必走的开关流程
- `api-endpoint-creator` — 加新 HTTP 端点的标准范式
- `triage-severity-scorer` — 九维打分规则
- `weekly-comprehension-check` — **写给人的反认知投降仪式**(agent 只能提醒,不能代做)

## agents(7 个)

| name | 模型分层定位 | 默认 provider |
|---|---|---|
| explorer | 廉价档(Haiku / gpt-4o-mini) | anthropic |
| implementer | 主力档(Sonnet / gpt-5.5) | anthropic |
| verifier-quality | 主力档 | anthropic |
| verifier-security | **旗舰档,这里别省**(Opus / gpt-5.5 / deepseek-reasoner) | anthropic |
| verifier-dependency | 廉价档 | anthropic |
| triage-scorer | 主力档 | anthropic |
| checker | 与 implementer **不同的模型**(maker/checker 不能混) | anthropic |

`provider` 字段是默认值;env `LLM_PROVIDER` 覆盖它。也就是说**改一个 env 就让 7 个 agent 全部换厂商**。

## SKILL.md 的写法

```yaml
---
name: <kebab-case>
description: <一段紧凑、无聊、可被 LLM 解析的功能描述>
when_to_use: <具体触发条件>
when_NOT_to_use: <反触发条件,防越界>
---
```

description 要**无聊**——Addy 反复强调"一段紧凑无聊的 description 比聪明的 description 更容易被准确触发"。
