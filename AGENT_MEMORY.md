# AGENT_MEMORY.md — auto-iterate 自身的迭代记忆

> 本文件是 **auto-iterate 项目自身**的模式/坑/决策记忆，**不是**给宿主项目用的。
> 宿主项目要自己建 `AGENT_MEMORY.md`（见 `standards/iterate.md §4` 和 §10）。
> Round 8 从 `AGENTS.md` 重命名而来，避开社区 agents.md 约定的命名冲突。

**反膨胀硬约束**（见 `standards/iterate.md §4`）：
- 总长 ≤ 200 行
- Patterns ≤ 15 · Gotchas ≤ 10 · Decisions ≤ 10
- 每条 ≤ 3 行（标题 + Why + How）
- 每 6 轮小结剪枝；30 轮大回归档

---

## Patterns

### 宿主可能已有自己的迭代约定，映射不替换
**Why**: 宿主（量化系统）已有 NORTH_STAR / HANDOFF / DECISIONS / LOOP_STATE 一整套；auto-iterate 是客，强求改名会抛弃现成工作流。
**How**: iterate.md §11 给映射表模板。auto-iterate 四入口是**角色**（目标/时间线/记忆/git），不是硬字面文件名。

---

## Gotchas

### 用户偏好精简文件结构
**Why**: Round 1 前曾写过 `guides/setup-plugins-and-mcp.md`，用户悄悄删了 —— 信号：不希望把操作手册混进标准库。
**How**: 不主动恢复 `guides/`。新增任何非 `standards/` 的目录前先问。

### progress.md 必须每轮即时追加，禁止批量
**Why**: Round 2-5 我攒到 Round 6 才批量写，违反了 iterate.md §1「每轮必产 progress.md 追加」。批量写容易漏、失真、打破「冷启动靠文件重建上下文」原则。
**How**: 每轮 commit 里必须包含 progress.md 的这一轮条目，和 feature 文件一起提交。不允许只提 feature 不提 progress。

### 不要占用社区约定的文件名
**Why**: 初版用 `AGENTS.md` 命名，和宿主项目既有的 `AGENTS.md`（Codex/GPT 协作说明，agents.md 社区约定）撞了。让宿主改名是越界。
**How**: 新建文件前查社区约定（agents.md、CLAUDE.md、CONTRIBUTING.md 等）。用途不撞也要避名。

### 用户偏好极简口令，不要长句 prompt
**Why**: 用户连续反馈「太长了」「必须这么说？」—— 装完后 CLAUDE.md 已挂接，agent 有协议上下文，口令可以短到 2-4 字。
**How**: 对用户的操作性说明写成表格速查（2-4 字口令 + 场景），解释和理由塞在别处。不要要求用户复读完整 prompt。

---

## Decisions

### auto-iterate 既是工具也是被迭代对象
**Why**: `iterate.md` 原本假设「我们」在「宿主」之外；但本项目也要用 iterate.md 迭代自己，角色重叠。
**How**: 读 iterate.md 时把「宿主」当通用占位符。本项目自身就是第一个宿主（`GOALS.md` / `progress.md` / `AGENT_MEMORY.md` 放仓库根，非 `standards/` 内）。

### 宿主 agent 只拉 `standards/` + `templates/`，不拉根 meta 文件
**Why**: `GOALS.md` / `progress.md` / `AGENT_MEMORY.md` 是 auto-iterate 自身记忆，拉进宿主会污染。而 `templates/` Round 2 后对宿主有用。
**How**: `README.md` Step 1 精确列白名单。新增对宿主有用的文件放 `standards/` 或 `templates/`，不放根。

### 不做 `/iterate` skill
**Why**: Claude Code 原生有 `/loop` / `CronCreate` / `ScheduleWakeup`，包一层是冗余。且 GOALS 明说「不做通用 framework」。
**How**: README Step 5 直接教用户用 `/loop 10m <prompt>`，agent 不自己起调度。

---

<!-- 限额提示：Patterns 1/15 · Gotchas 4/10 · Decisions 3/10 · 总行数约 75/200 -->
