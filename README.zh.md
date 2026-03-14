<p align="center">
  <img src="assets/banner.png" alt="Context Handoff Engine" width="750">
</p>

[![MIT License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Built with Claude Code](https://img.shields.io/badge/built%20with-Claude%20Code-blueviolet)](https://docs.anthropic.com/en/docs/claude-code)
[![Companion to recursive-drift](https://img.shields.io/badge/companion-recursive--drift-orange)](https://github.com/shawnla90/recursive-drift)

[English](README.md) | **中文**

---

## 问题

Claude Code 每次启动都是零记忆。随着使用规模扩大，五种失败模式会不断叠加：

1. **重启后上下文归零。** 周一早上打开终端，花 10 分钟重新解释周五写了什么。每次会话都要来一遍。

2. **多终端竞争条件。** 你开了 4 个终端，其中两个同时写 handoff 文件，最后一个覆盖前面的。三个会话的上下文直接蒸发。

3. **记忆被截断。** MEMORY.md 写到 400 行，Claude 只加载前 200 行。一半的项目知识悄无声息地消失了。

4. **Agent 交接丢信息。** 你启动了一个 subagent，它完成了任务，但父级只拿到一份摘要，不是完整的决策记录。下一个 agent 又把已经做过的决定重新争论一遍。

5. **团队决策漂移。** 三个 agent 并行工作。Agent A 选了 snake_case，Agent B 选了 camelCase，Agent C 随便选了一个。没人记录这个决定。

这个仓库就是解决以上所有问题的基础设施。

---

## 快速上手

### Tier 1：只用 Handoff（5 分钟）

把 `templates/claude-md-minimal.md` 复制到你的项目里作为 `CLAUDE.md`。搞定。

```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/shawnla90/context-handoff-engine/main/templates/claude-md-minimal.md
mkdir -p ~/.claude/handoffs
```

现在你就有了并行安全的会话交接。每个会话写自己的文件，启动时读取所有未消费的 handoff，读完标记为已完成。

### Tier 2：Handoff + 记忆 + 自我改进（15 分钟）

```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/shawnla90/context-handoff-engine/main/templates/claude-md-with-memory.md
mkdir -p ~/.claude/handoffs tasks
touch tasks/lessons.md tasks/todo.md
```

把记忆索引模板复制到你的 auto-memory 目录：

```bash
mkdir -p ~/.claude/projects/$(pwd | tr '/' '-')/memory
curl -o ~/.claude/projects/$(pwd | tr '/' '-')/memory/MEMORY.md \
  https://raw.githubusercontent.com/shawnla90/context-handoff-engine/main/memory/memory-index-template.md
```

现在你有了 handoff + 结构化持久记忆 + 跨会话积累经验的自我改进循环。

### Tier 3：完整引擎（30 分钟）

```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/shawnla90/context-handoff-engine/main/templates/claude-md-full.md
mkdir -p ~/.claude/handoffs ~/.claude/teams tasks
touch tasks/lessons.md tasks/todo.md
```

然后根据你的项目定制团队约束和路由文件。参考 `teams/` 和 `routing/` 目录下的模板。

---

## 架构

引擎分为 6 层。每层解决一个具体的失败模式。按需使用。

```
Layer 6: 路由 ─────────── 这个任务适合哪种执行模式？
Layer 5: 团队 ─────────── 并行 agent 如何协调？
Layer 4: Agent 交接 ───── 上下文如何在 agent 之间传递？
Layer 3: 自我改进 ─────── 错误如何变成规则？
Layer 2: 记忆 ─────────── 知识如何跨会话持久化？
Layer 1: Handoff ─────── 会话状态如何传递？
```

### Layer 1：并行安全的会话交接

**解决的问题：** 多个终端同时写 handoff 时的竞争条件。

每个会话写入 `~/.claude/handoffs/<timestamp>_<slug>.md`。不会互相覆盖。启动时，agent 读取所有未消费的 handoff，打印摘要，然后把文件名加上 `_done` 后缀。

**4 个操作：**
- **写入：** `~/.claude/handoffs/YYYY-MM-DD_HHMMSS_<slug>.md`
- **读取：** `ls ~/.claude/handoffs/*.md | grep -v '_done.md$'`
- **消费：** 读取后将 `file.md` 重命名为 `file_done.md`
- **清理：** `find ~/.claude/handoffs -name '*_done.md' -mtime +7 -delete`

详见 [`handoffs/`](handoffs/)，包含完整模板和迁移指南。

### Layer 2：结构化记忆持久化

**解决的问题：** 记忆文件无限增长导致被截断。

记忆索引（`MEMORY.md`）控制在 200 行以内。它包含快速查阅的事实和指向主题文件（`identity.md`、`infrastructure.md`、`completed-work.md`）的链接，细节都在主题文件里。Claude 自动加载 MEMORY.md，主题文件在需要时按需加载。

**结构：**
```
~/.claude/projects/<project>/memory/
├── MEMORY.md              # 始终加载（控制在 200 行以内）
├── identity.md            # 身份、背景
├── infrastructure.md      # 模型、路径、服务
├── completed-work.md      # 已完成工作的归档
└── patterns.md            # 常用解决方案
```

详见 [`memory/`](memory/)，包含模板和示例。

### Layer 3：自我改进循环

**解决的问题：** 同样的错误在不同会话中反复出现。

每次用户纠正后，agent 将教训写入 `tasks/lessons.md`，包含日期、场景和规则。启动时，agent 读取所有教训并遵循它们。随着规则的积累，犯错率逐渐下降。

**循环：** 纠正 -> 教训 -> 规则 -> 预防

详见 [`self-improvement/`](self-improvement/)，包含模板。

### Layer 4：Agent 间上下文传递

**解决的问题：** subagent 在交接上下文时丢失决策信息。

在 agent 之间交接时，生成一份独立的上下文文档，包含 6 个部分：背景、成果、关键文件、待解决问题、下一步、工作流钩子。接收方 agent 无需任何前置对话即可直接工作。

详见 [`agent-handoffs/`](agent-handoffs/)，包含模板和示例。

### Layer 5：多 Agent 团队协调

**解决的问题：** 并行 agent 做出相互冲突的决策。

9 条规则，防止多个 agent 同时工作时出现混乱：
1. 文件所有权（每个文件每轮只有一个写入者）
2. 共享决策日志
3. 先读后写
4. 波次纪律（基于依赖关系排序）
5. 构建关卡（验证通过才能部署）
6. 行动前先读上下文
7. 范围隔离
8. 每个执行者使用新上下文
9. 需要避免的反模式

详见 [`teams/`](teams/)，包含通用化的约束系统。

### Layer 6：路由决策框架

**解决的问题：** 该用团队的时候用了 subagent，该并行的时候单线程硬扛。

从 5 个维度（文件数量、关注点分离、交接需求、评审需求、质量关卡）给任务打分，路由到合适的执行模式：
- **模式 A：** 单会话专注执行
- **模式 B：** 并行 subagent
- **模式 C：** Agent 团队

详见 [`routing/`](routing/)，包含评分框架和速查表。

---

## 对比

| 方案 | 重启后保留上下文？ | 并行安全？ | 从错误中学习？ | Agent 协调？ |
|------|-------------------|-----------|---------------|-------------|
| 不做 handoff | 否 | 不适用 | 否 | 否 |
| 单个 handoff 文件 | 是 | 否（后写覆盖） | 否 | 否 |
| 仅系统提示 | 部分 | 是 | 否 | 否 |
| RAG / 向量检索 | 是 | 是 | 否 | 否 |
| **Context Handoff Engine** | **是** | **是** | **是** | **是** |

---

## 目录结构

```
context-handoff-engine/
├── README.md                        # 英文文档
├── README.zh.md                     # 中文文档（你在这里）
├── handoffs/                        # Layer 1：并行安全的会话交接
├── memory/                          # Layer 2：结构化记忆持久化
├── self-improvement/                # Layer 3：纠错积累器
├── agent-handoffs/                  # Layer 4：Agent 间上下文传递
├── teams/                           # Layer 5：多 Agent 协调
├── routing/                         # Layer 6：决策框架
├── templates/                       # 即拿即用的 CLAUDE.md 文件（从这里开始）
├── examples/                        # 可运行的目录结构示例
└── guides/                          # 分步设置指南
```

## Recursive Drift 的配套项目

这个仓库是 [recursive-drift](https://github.com/shawnla90/recursive-drift) 的基础设施层。Recursive drift 定义了方法论，这个引擎负责底层管道 - 确保上下文持久化、agent 之间协调、错误转化为规则。

你可以单独使用这个引擎，也可以单独使用 recursive-drift。但两者配合效果更好。

---

## 贡献

参见 [CONTRIBUTING.md](CONTRIBUTING.md)。模板应该能直接复制粘贴使用，并在实际的 Claude Code 会话中验证过。

## 许可证

[MIT](LICENSE)
