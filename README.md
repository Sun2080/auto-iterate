# auto-iterate

一个指导 Claude Code / Codex agent「写好代码、高效自我迭代」的轻量知识包。

- **Part 1 · 写好代码** —— Karpathy 4 原则 + 平台无关的 agent 交互契约 → [`standards/code.md`](standards/code.md)
- **Part 2 · 高效迭代** —— AutoResearch 循环 + AGENT_MEMORY.md 复利机制 + 多维停止条件 + 平台适配的节拍 / 自动化 → [`standards/iterate.md`](standards/iterate.md)

不教业务目标、不管基础设施、不做通用 framework。只讲「怎么工作」。

**设计原则**：`standards/` 保持平台中性；平台差异只放在两层：
- **宿主指令文件**：Claude 用 `CLAUDE.md`，Codex 用 `AGENTS.md`
- **循环触发方式**：Claude 常用 `/loop` / `/schedule`，Codex 常用 Automations

详细场景说明（手动 / 连续跑 / 定时自动化 / 12 小时无人值守 / Claude / Codex 示例）见 [`USAGE.md`](USAGE.md)；其中完整模板统一用绝对时间示例，可直接复制后只改结束时间。

---

## 给人：口令速查（按平台复制）

> **前提**：已完成下方「消费入口」Step 1-4（Claude 挂接 `CLAUDE.md`；Codex 挂接 `AGENTS.md`；`GOALS.md` 已填）。装完之前请用第一行的首装口令。

### Claude Code

🔥 想**无间隔、连续不停**迭代？复制这句：

```
/loop 下一轮
```

就这 5 个字。不带时间参数 = 动态节拍，是 Claude 侧最接近“连续跑”的官方方式；但它不是书面保证的零间隔。停止条件由 `iterate.md §6` 自动兜底。

### Codex

手动跑一轮：

```
下一轮
```

会话内自动循环（当前官方文档以 Automations 为主）：

```text
请创建一个 thread automation，每 1 分钟回到当前线程，按 .claude/standards/iterate.md 跑下一轮；严格遵守 §6 停止条件
```

后台定时自动化：

```text
请创建一个 standalone automation，按 <schedule> 在该项目上执行：按 .claude/standards/iterate.md 跑下一轮；严格遵守 §6 停止条件
```

按当前官方文档，Codex CLI 的 slash commands 列表里没有和 Claude `/loop` 同名的命令；Codex 侧更适合用 Automations 承接**定时**循环。想看“连续 vs 定时”的区别，直接读 `USAGE.md`。

想停：直接说「停」。

---

装完之后 agent 开会话就懂协议，其他场景口令也很短：

| 场景 | Claude Code | Codex |
|------|------|------|
| **装到新项目**（首次） | `拉取 https://github.com/Sun2080/auto-iterate 装到本项目，引导我写 GOALS.md` | `拉取 https://github.com/Sun2080/auto-iterate 装到本项目，引导我写 GOALS.md` |
| **手动跑一轮** | `下一轮` | `下一轮` |
| **最接近连续跑** | `/loop 下一轮` | 当前公开文档未见完全对等官方方案 |
| **固定节拍** | `/loop 5m 下一轮` / `/loop 10m 下一轮` | `请创建一个 automation，每 5 分钟 / 10 分钟运行下一轮` |
| **强制小结**（6 轮到了） | `做小结` | `做小结` |
| **强制战报**（30 轮到了） | `出战报` | `出战报` |
| **停** | `停` | `停` |
| **继续** | `继续` | `继续` |
| **看进度** | `看进度` | `看进度` |

Agent 按下面「消费入口」流程执行首装。

---

## 如果你是一个 coding agent（消费入口）

用户让你拉取本仓库时，按以下步骤操作。Claude Code 和 Codex 共用同一套 `standards/`；只在宿主指令文件和自动化方式上分平台。

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
- `templates/GOALS.md.template` · `templates/CLAUDE.md.append` · `templates/AGENTS.md.append` · `templates/AGENT_MEMORY.md.template` —— 模板（Step 2、3 会用）

**不拉什么**：`GOALS.md` · `AGENT_MEMORY.md` · `progress.md` —— 这些是 auto-iterate **自身**的记忆，拉进宿主会污染。

非 git-bash 环境（原生 PowerShell / cmd）下，agent 自行把 `git clone` / `cp -r` / `rm -rf` 替换成当前 shell 等价命令 —— 本项目**不做**平台适配逻辑（见 `GOALS.md` Constraints）。

### Step 2 · 挂接到宿主指令文件

- **Claude Code**：把 `.claude/templates/CLAUDE.md.append` 的内容追加到宿主项目根的 `CLAUDE.md`（没有就新建）
- **Codex**：把 `.claude/templates/AGENTS.md.append` 的内容追加到宿主项目根的 `AGENTS.md`（没有就新建）

两边都只是薄 adapter；真正的工作方法都在 `.claude/standards/`。

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
3. 填完后告诉用户：手动可说 `下一轮`；Claude 可用 `/loop` / `/schedule`；Codex 可用 Automations，详见速查表

**不要**用户没填 GOALS.md 就自己开始跑 —— 那是 §7 跑偏。

### Step 5 · 如何真的跑起循环

GOALS.md 填完、用户说「开始」后，先选**模式**，再按平台套壳：

**① 手动节拍**（Claude / Codex 通用，推荐起步）：
用户每次说「下一轮」时，agent 按 `iterate.md` 跑一轮（Modify → Commit → Verify → Keep/Revert + `progress.md` 追加）。每轮节奏完全人控。

**② 会话内自动循环**（稳定后再开）：
- **Claude Code**：优先 `/loop`。动态 `/loop 下一轮` 是 Claude 侧最接近“连续跑”的官方形态；固定节拍用 `/loop 5m 下一轮`
- **Codex**：若接受分钟级唤醒，优先 thread automation。当前公开文档里没有和 Claude `/loop` 完全对等的官方机制

**③ 后台定时自动化**（需要脱离当前对话 / 过夜跑）：
- **Claude Code**：用户显式要求时，用 `/schedule` / routines 这类平台自动化
- **Codex**：用户显式要求时，用 standalone automation

**三档共性**：
- 先手动跑通至少 1 轮，再开自动化
- `iterate.md §6` 任一停止条件命中都要停下问人
- **没有用户明确授权，不要自己起自动化**

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
- [Anthropic · Claude Code Docs](https://code.claude.com/docs)
- [OpenAI Developers · Codex Docs](https://developers.openai.com/codex)
- AutoResearch 模式（[uditgoenka/autoresearch](https://github.com/uditgoenka/autoresearch)）
- [Addy Osmani · Self-Improving Coding Agents](https://addyosmani.com/blog/self-improving-agents/)
