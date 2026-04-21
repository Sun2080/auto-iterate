# auto-iterate

一个指导 Claude Code agent「写好代码、高效自我迭代」的轻量知识包。

- **Part 1 · 写好代码** —— Karpathy 4 原则 + Opus 4.7 交互契约 → [`standards/code.md`](standards/code.md)
- **Part 2 · 高效迭代** —— AutoResearch 循环 + AGENT_MEMORY.md 复利机制 + 多维停止条件 + 参数化节拍（动态 `/loop` / 固定 `30s`-`20m`） → [`standards/iterate.md`](standards/iterate.md)

不教业务目标、不管基础设施、不做通用 framework。只讲「怎么工作」。

---

## 给人：口令速查（复制即可）

> **前提**：已完成下方「消费入口」Step 1-4（CLAUDE.md 已挂接 `iterate.md`、GOALS.md 已填）。装完之前请用第一行的首装口令。

### 🔥 想**无间隔、连续不停**迭代？复制这句：

```
/loop 下一轮
```

就这 5 个字。不带时间参数 = 动态节拍 —— 每轮跑完 Claude 立即起下一轮（自选 1min-1h），不堆积、无并发。停止条件由 `iterate.md §6` 自动兜底（连续 2 轮同错 / 3 轮无 commit / 30 轮大回 / 5h / 目标达成 都会自动停下问你）。

想停：直接说「停」。

---

装完之后 agent 开会话就懂协议，其他场景口令也很短：

| 场景 | 口令 |
|------|------|
| **装到新项目**（首次） | `拉取 https://github.com/Sun2080/auto-iterate 装到本项目，引导我写 GOALS.md` |
| **手动跑一轮** | `下一轮` |
| **连续跑 · 跑完立即起**（动态节拍，无并发） | `/loop 下一轮` |
| **自动节拍 · 10 分钟** | `/loop 10m 下一轮` |
| **自动节拍 · 5 分钟** | `/loop 5m 下一轮` |
| **自动节拍 · 1 分钟**（短任务，cache 命中最高） | `/loop 1m 下一轮` |
| **强制小结**（6 轮到了） | `做小结` |
| **强制战报**（30 轮到了） | `出战报` |
| **停** | `停` |
| **继续** | `继续` |
| **看进度** | `看进度` |

Agent 按下面「消费入口」流程执行首装。

---

## 停法（省心版）

两层都自动触发，**取先到**：

| 层 | 什么会停你 |
|---|---|
| **工具层**（Claude Code） | 关会话 / 你说「停」/ `/loop` recurring 7 天过期 / `CronDelete <id>` |
| **项目层**（`iterate.md §6`）| 连续 2 轮同错 · 连续 3 轮无 commit · 30 轮大回 · 5 小时 timebox · GOALS 全过 · 预算耗尽 |

**默认啥都不配**，`/loop 下一轮` 就行 —— §6 会在合理的点停下问你，不会一路跑到天荒地老。

### 想卡具体条件（跑 N 小时 / 到点 / N 轮）

工具层没原生参数，把条件夹进 prompt，agent 每轮末尾自检：

| 要求 | 写法 |
|---|---|
| 跑 N 小时停 | `/loop 下一轮，3 小时后停` |
| 跑到 18:00 停 | `/loop 下一轮，到 18:00 停` |
| 跑 N 轮停 | `/loop 下一轮，30 轮后停` |

Agent 到条件就不再起下一轮，循环自然结束。

### 有人 vs 无人值守

`/loop` 绑会话 —— **关窗即停**。过夜/跨天无人跑需 Anthropic 云端任务（`/schedule`），本项目暂不覆盖；要用再问。

---

## 如果你是一个 Claude Code agent（消费入口）

用户让你拉取本仓库时，按以下步骤操作。

### Step 1 · 拉仓库 + 复制文件到宿主项目

推荐方式（git-bash / 类 Unix shell 下可直接跑；其他 shell 见本段末尾说明）：

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
- `templates/GOALS.md.template` · `templates/CLAUDE.md.append` · `templates/AGENT_MEMORY.md.template` —— 模板（Step 2、3 会用）

**不拉什么**：`GOALS.md` · `AGENT_MEMORY.md` · `progress.md` —— 这些是 auto-iterate **自身**的记忆，拉进宿主会污染。

非 git-bash 环境（原生 PowerShell / cmd）下，agent 自行把 `git clone` / `cp -r` / `rm -rf` 替换成当前 shell 等价命令 —— 本项目**不做**平台适配逻辑（见 `GOALS.md` Constraints）。

### Step 2 · 挂接到宿主 CLAUDE.md

把 `.claude/templates/CLAUDE.md.append` 的内容追加到宿主项目根的 `CLAUDE.md`（没有就新建）。内容已经写好，直接拼接。

### Step 3 · 建立宿主项目的迭代基建

`iterate.md §10` 要求宿主提供 4 个文件：

| 文件 | 谁写 | 起步动作 |
|------|------|---------|
| `GOALS.md` | **必须由人写** | 把 `.claude/templates/GOALS.md.template` 复制为 `GOALS.md`，**引导用户填写**（逐字段提问，不替人决定） |
| `AGENT_MEMORY.md` | agent 维护 | 复制 `.claude/templates/AGENT_MEMORY.md.template` 为宿主根的 `AGENT_MEMORY.md`（已含反膨胀约束 + 三分区空壳） |
| `progress.md` | agent 追加 | 新建空文件 |
| `tasks.json`（可选） | 共维护 | 可省 |

**`GOALS.md` 没填完就不要进循环** —— agent 不自创方向。

### Step 4 · 回报用户 + 等信号

安装完成后告诉用户：
1. 两份标准已挂接（路径告知）
2. `GOALS.md` + `AGENT_MEMORY.md` 模板已复制就位；`GOALS.md` 需要人填 Mission / Success Criteria / Non-Goals / Constraints
3. 填完后用户三档任选：`下一轮`（手动）· `/loop 下一轮`（动态，跑完立即起）· `/loop Xm 下一轮`（固定周期），详见速查表

**不要**用户没填 GOALS.md 就自己开始跑 —— 那是 §7 跑偏。

### Step 5 · 如何真的跑起循环

GOALS.md 填完、用户说「开始」后，三种节拍可选（**推荐度递减**）：

**① 手动节拍**（早期 / 调试 / 大改动）：
用户每次说「下一轮」时，agent 按 `iterate.md` 跑一轮（Modify → Commit → Verify → Keep/Revert + progress.md 追加）。每轮节奏完全人控。

**② 动态 `/loop`**（稳定后连续跑 · **推荐首选**）：
```
/loop 按 .claude/standards/iterate.md 跑下一轮；严格遵守 §6 停止条件
```
不带时间参数。Claude 每轮结束**自选** 1min-1h 下次时间 —— 跑完立即起下一轮，不堆积、无并发隐患。内部走 `ScheduleWakeup`，是 Claude Code 原生机制。

**③ 固定周期 `/loop Xm`**（需要精确节拍时）：
```
/loop 10m 按 .claude/standards/iterate.md 跑下一轮；严格遵守 §6 停止条件
```
每 X 分钟固定触发。官方是**串行等待 + 跳过补偿**（上一轮没跑完就排队等，不并发；等期间又到 tick 则跳过不补打）—— 不会堆积，但短任务会有空转间隔。周期选择：大任务 5-10m、小任务 1m 或 30s（prompt cache 5min TTL 下短周期几乎全命中，更省 token）。

**三档共性**：`iterate.md §6` 任一停止条件命中都会自动停下问人。

**硬线**：**不要自己起 `CronCreate`**。动态 `/loop` 内部用 `ScheduleWakeup` 是用户主动触发后 agent 在 `/loop` 框架内自调速，不算违反；但在**没有用户 `/loop` 触发**的场景下 agent 私自起调度器 = 越界。

**30 轮大回停下后想过夜跑**：用户可用 `/schedule` 注册 cron trigger 定时重启下一轮（`iterate.md §6` 末段）。agent 仍不自拨。

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
