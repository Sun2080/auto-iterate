# AGENTS.md — auto-iterate 自身的迭代记忆

> 本文件是 **auto-iterate 项目自身**的模式/坑/决策记忆，**不是**给宿主项目用的。
> 宿主项目要自己建 `AGENTS.md`（见 `standards/iterate.md §4` 和 §10）。

**反膨胀硬约束**（见 `standards/iterate.md §4`）：
- 总长 ≤ 200 行
- Patterns ≤ 15 · Gotchas ≤ 10 · Decisions ≤ 10
- 每条 ≤ 3 行（标题 + Why + How）
- 每 6 轮小结剪枝；30 轮大回归档

---

## Patterns

*（暂无）*

---

## Gotchas

### 用户偏好精简文件结构
**Why**: Round 1 前曾写过 `guides/setup-plugins-and-mcp.md`，用户悄悄删了 —— 信号：不希望把操作手册混进标准库。
**How**: 不主动恢复 `guides/`。新增任何非 `standards/` 的目录前先问。

---

## Decisions

### auto-iterate 既是工具也是被迭代对象
**Why**: `iterate.md` 原本假设「我们」在「宿主」之外；但本项目也要用 iterate.md 迭代自己，角色重叠。
**How**: 读 iterate.md 时把「宿主」当通用占位符。本项目自身就是第一个宿主（`GOALS.md` / `progress.md` / `AGENTS.md` 放仓库根，非 `standards/` 内）。

### 宿主 agent 只拉 `standards/`，不拉 meta 文件
**Why**: `GOALS.md` / `progress.md` / `AGENTS.md` 是 auto-iterate 的自身记忆，拉进宿主会污染宿主记忆空间。
**How**: `README.md` 明确写「只拉 `standards/` 下的文件」。未来新增对宿主有用的文件，放 `standards/` 或 `templates/`，不放根。

---

<!-- 限额提示：Patterns 0/15 · Gotchas 1/10 · Decisions 2/10 · 总行数约 40/200 -->
