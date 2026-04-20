# auto-iterate

一个指导 Claude Code agent「写好代码、高效自我迭代」的轻量知识包。

- **Part 1 · 写好代码** —— Karpathy 4 原则 + Opus 4.7 交互契约 → [`standards/code.md`](standards/code.md)
- **Part 2 · 高效迭代** —— AutoResearch 循环 + AGENT_MEMORY.md 复利机制 + 多维停止条件 → [`standards/iterate.md`](standards/iterate.md)

不教业务目标、不管基础设施、不做通用 framework。只讲「怎么工作」。

---

## 给人：口令速查（复制即可）

装完之后，`CLAUDE.md` 已经指向 `iterate.md`，agent 开会话就懂协议。口令就很短：

| 场景 | 口令 |
|------|------|
| **装到新项目**（首次） | `拉取 https://github.com/Sun2080/auto-iterate 装到本项目，引导我写 GOALS.md` |
| **手动跑一轮** | `下一轮` |
| **自动节拍 · 10 分钟** | `/loop 10m 下一轮` |
| **自动节拍 · 5 分钟** | `/loop 5m 下一轮` |
| **强制小结**（6 轮到了） | `做小结` |
| **强制战报**（30 轮到了） | `出战报` |
| **停** | `停` |
| **继续** | `继续` |
| **看进度** | `读 progress.md 末尾告诉我上一轮在干啥` |

Agent 按下面「消费入口」流程执行首装。

---

## 如果你是一个 Claude Code agent（消费入口）

用户让你拉取本仓库时，按以下步骤操作。

### Step 1 · 拉仓库 + 复制文件到宿主项目

推荐方式（Windows/Mac/Linux 通用）：

```bash
# 1. clone 到临时目录
git clone --depth 1 https://github.com/Sun2080/auto-iterate /tmp/auto-iterate

# 2. 复制「给宿主用」的两个目录到宿主项目
mkdir -p .claude/standards
cp -r /tmp/auto-iterate/standards/* .claude/standards/
cp -r /tmp/auto-iterate/templates .claude/templates

# 3. 清理
rm -rf /tmp/auto-iterate
```

**拉什么**：
- `standards/code.md` · `standards/iterate.md` —— 两份标准（必须）
- `templates/GOALS.md.template` · `templates/CLAUDE.md.append` —— 模板（Step 2、3 会用）

**不拉什么**：`GOALS.md` · `AGENT_MEMORY.md` · `progress.md` —— 这些是 auto-iterate **自身**的记忆，拉进宿主会污染。

Windows 纯 PowerShell 环境下把 `cp -r` 换成 `Copy-Item -Recurse`，其余同。

### Step 2 · 挂接到宿主 CLAUDE.md

把 `.claude/templates/CLAUDE.md.append` 的内容追加到宿主项目根的 `CLAUDE.md`（没有就新建）。内容已经写好，直接拼接。

### Step 3 · 建立宿主项目的迭代基建

`iterate.md §10` 要求宿主提供 4 个文件：

| 文件 | 谁写 | 起步动作 |
|------|------|---------|
| `GOALS.md` | **必须由人写** | 把 `templates/GOALS.md.template` 复制为 `GOALS.md`，**引导用户填写**（逐字段提问，不替人决定） |
| `AGENT_MEMORY.md` | agent 维护 | 新建空文件，含反膨胀约束注释（可从 auto-iterate 的 `AGENT_MEMORY.md` 顶部抄框架） |
| `progress.md` | agent 追加 | 新建空文件 |
| `tasks.json`（可选） | 共维护 | 可省 |

**`GOALS.md` 没填完就不要进循环** —— agent 不自创方向。

### Step 4 · 回报用户 + 等信号

安装完成后告诉用户：
1. 两份标准已挂接（路径告知）
2. `GOALS.md` 模板就位，需要人填 Mission / Success Criteria / Non-Goals / Constraints
3. 填完后用户说「开始迭代」即可进入 `iterate.md` 的循环

**不要**用户没填 GOALS.md 就自己开始跑 —— 那是 §7 跑偏。

### Step 5 · 如何真的跑起循环

GOALS.md 填完、用户说「开始」后，有两种运行方式：

**手动节拍**（推荐给早期 / 调试）：
用户每次说「下一轮」时，agent 按 `iterate.md` 跑一轮（Modify → Commit → Verify → Keep/Revert + progress.md 追加）。

**自动节拍**（稳定后可用）：
用户在 Claude Code 里用 `/loop`：
```
/loop 10m 按 .claude/standards/iterate.md 跑下一轮；严格遵守 §6 停止条件
```
每 10 分钟触发一轮。`iterate.md §6` 任一停止条件命中会自动停下问人。

**不要自己起 `CronCreate` 或开 `ScheduleWakeup`**。节奏由用户控制，agent 只管好每一轮内部的质量。

---

## 如果你是项目维护者（人）

本项目自身也按 `standards/iterate.md` 迭代：

- `GOALS.md` —— 本项目目标
- `AGENT_MEMORY.md` —— 本项目的模式/坑/决策
- `progress.md` —— 本项目的迭代日志

这些是**本项目专有**的文件，宿主 agent 拉取时要跳过（见 Step 1）。

## 来源与致谢

方法论综合自：
- [Andrej Karpathy](https://karpathy.ai/) 对 LLM 编码陷阱的观察（via [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills)）
- [Anthropic · Claude Opus 4.7 Best Practices](https://claude.com/blog/best-practices-for-using-claude-opus-4-7-with-claude-code)
- AutoResearch 模式（[uditgoenka/autoresearch](https://github.com/uditgoenka/autoresearch)）
- [Addy Osmani · Self-Improving Coding Agents](https://addyosmani.com/blog/self-improving-agents/)
