# auto-iterate

一个指导 Claude Code agent「写好代码、高效自我迭代」的轻量知识包。

- **Part 1 · 写好代码** —— Karpathy 4 原则 + Opus 4.7 交互契约 → [`standards/code.md`](standards/code.md)
- **Part 2 · 高效迭代** —— AutoResearch 循环 + AGENTS.md 复利机制 + 多维停止条件 → [`standards/iterate.md`](standards/iterate.md)

不教业务目标、不管基础设施、不做通用 framework。只讲「怎么工作」。

---

## 如果你是一个 Claude Code agent（消费入口）

用户让你拉取 `https://github.com/Sun2080/auto-iterate` 时，按以下步骤操作。

### Step 1 · 拉文件到宿主项目

**只拉 `standards/` 下的两份文件**。其它是本项目自身记忆，不拉。

推荐放置位置（任选其一）：

```
<宿主项目>/.claude/standards/code.md
<宿主项目>/.claude/standards/iterate.md
```

或者直接放项目根下的 `standards/`。两者等价，挑和宿主项目现有结构最协调的。

### Step 2 · 挂接到宿主 CLAUDE.md

在宿主项目根的 `CLAUDE.md` 里加一段（没有就新建一个）：

```markdown
## 代码与迭代标准

每次动手前读 `.claude/standards/code.md`（4.7 交互契约 + Karpathy 4 原则）。
开始迭代循环时读 `.claude/standards/iterate.md`（核心循环 + 复利 + 停止条件）。
```

### Step 3 · 建立宿主项目的迭代基建

`iterate.md §10` 要求宿主提供 4 个文件：

| 文件 | 谁写 | 起步状态 |
|------|------|---------|
| `GOALS.md` | **必须由人写** | 目标 + 成功判据 |
| `AGENTS.md` | 人起 · agent 维护 | 可空 或 少量种子条目 |
| `progress.md` | agent 每轮追加 | 可空 |
| `tasks.json`（可选） | 人 / agent 共维护 | 可省 |

`GOALS.md` 没写就不要开始迭代 —— 这是我们「不管目标」的落地点，agent 不自创方向。

### Step 4 · 回报用户

安装完成后告诉用户：
- 两份标准已挂接
- `GOALS.md` 需要人写（附空模板或引导提问）
- 写好 GOALS.md 后可以让本 agent（或下一个 agent）按 `iterate.md` 开跑

---

## 如果你是项目维护者（人）

本项目自身也按 `standards/iterate.md` 迭代：

- `GOALS.md` —— 本项目目标
- `AGENTS.md` —— 本项目的模式/坑/决策
- `progress.md` —— 本项目的迭代日志

这些是**本项目专有**的文件，宿主 agent 拉取时要跳过（见 Step 1）。

## 来源与致谢

方法论综合自：
- [Andrej Karpathy](https://karpathy.ai/) 对 LLM 编码陷阱的观察（via [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills)）
- [Anthropic · Claude Opus 4.7 Best Practices](https://claude.com/blog/best-practices-for-using-claude-opus-4-7-with-claude-code)
- AutoResearch 模式（[uditgoenka/autoresearch](https://github.com/uditgoenka/autoresearch)）
- [Addy Osmani · Self-Improving Coding Agents](https://addyosmani.com/blog/self-improving-agents/)
