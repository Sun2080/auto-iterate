---
name: iterate-standard
description: How the agent iterates round after round on the host project. Core loop, memory, cadence, stopping conditions. Load this at the start of every round and at every mini-review.
audience: host-project agent
enforcement: self + filesystem (AGENT_MEMORY.md / progress log / git commits are the iteration state)
---

# 自我迭代标准

本项目只教**怎么迭代**。目标由宿主项目 `GOALS.md` 提供。

---

## TL;DR（每轮只读这段；小回 / 大回 / 卡壳时才读全文）

**循环**: Modify → Commit（**先于** verify）→ Verify → Keep / Revert

**每轮入**: `GOALS.md` · `progress.md` 末 20 行 · `AGENT_MEMORY.md` · `git log -10`
**每轮出**: 至少 1 commit · `progress.md` 追加一段 · 必要时更新 `AGENT_MEMORY.md`

**前 3 轮**: 必做一次外部参照（§1.2），避免闭门造车

**停下问人**: 连续 2 轮同错 / 连续 3 轮无 commit / 大回 30 轮 / 5 小时 / 目标达成 / 动作在 `GOALS.md` 找不到归属

**反膨胀**: `AGENT_MEMORY.md` ≤ 200 行 · Patterns ≤ 15 · Gotchas ≤ 10 · Decisions ≤ 10 · 每条 ≤ 3 行（§4）

**节律**: 10 min/轮 · 6 轮小回剪枝 · 30 轮大回归档

**深读索引**: §1 契约 · §1.2 外部参照 · §2 核心循环 · §4 反膨胀 · §5 节律 · §6 停止 · §11 融入宿主

---

## 1 · 每轮契约

一轮 = 一次完整的「输入 → 动作 → 产出」。

**入（每轮开始必读）**：
1. `GOALS.md` —— 宿主项目的目标（长期不变）
2. `progress.md` 末尾 ~20 行 —— 上轮做了什么、结果、下轮计划
3. `AGENT_MEMORY.md` —— 累积的模式/坑/决策
4. `git log -10 --oneline` —— 最近 10 次提交

**出（每轮结束必产）**：
1. 至少一次 git commit（失败的实验也要 commit 再 revert，留下轨迹）
2. `progress.md` 追加一段：这轮做了什么、验证结果、下轮计划
3. 若有新的可复用发现 → 更新 `AGENT_MEMORY.md`（按反膨胀协议，见 §4）

**硬约束**：没有 commit 的一轮 = 没做事。空转要记录原因而不是装作跑了。

### 1.1 Round 1 Bootstrap 例外

首轮（Round 1）时入口四文件不存在。此时：
- 不视为错误，把本轮当 Bootstrap
- Modify 的首要动作是**创建入口文件**：`GOALS.md`（若 `README.md` 指引人类先写，则等人类）、`AGENT_MEMORY.md`、`progress.md`
- Verify 判据从「指标改善」放宽为「文件存在 + 内容自洽」
- 首 commit 是 root-commit，Keep 是默认（无 baseline 可比）

**唯一前置**：`GOALS.md` 若缺，先提示用户写（或用 `templates/GOALS.md.template`）。没有 GOALS 就不要开循环 —— agent 不自创方向。

### 1.2 外部参照（前几轮强制做一次，避免闭门造车）

**前 3 轮之内必做一次**；或 GOALS.md 首次稳定、引入新主题时补做。不做会在 AGENT_MEMORY 膨胀后才发现「别人早有成熟方案」。

**做什么**
- 搜「与本项目问题相近」的成熟项目 / awesome-list / 近一年论文仓
- 关键词 = GOALS.md 的核心术语 + 当前年份
- 只看 README / 设计文档，不啃源码
- 产出清单：3-5 个 repo + 每个一句「解决什么 · 对我们有用的点」

**工具**
- `WebSearch` + `WebFetch`（Claude Code 内置）
- `gh search repos "<keyword>" --sort=stars --limit=10`
- 优先级：官方最佳实践 > awesome-* 列表 > 近一年高星 repo

**吸收方式**
- **术语对齐 / 思路吸收** → AGENT_MEMORY.md 一条 Pattern，带源 URL
- **本轮就要用到的参考** → progress.md 当轮加 `refs: external#<repo>`
- **撞车验证**（别人独立做到同样结论）→ 是强信号，记一条 Decision 加源

**不要做**
- ❌ 搜了不吸收、只写「参考过」三个字
- ❌ 照搬别人架构、默认所有人都该这样
- ❌ 每轮重搜（token 浪费、结论几乎不变）
- ❌ 啃源码细节（时间黑洞，收益低）

**跳过条件**
- 问题非常私域（内部某模块的 bug）
- 前 2 轮刚做过且结论稳定
- GOALS 未变且无新主题

---

## 2 · 核心循环（AutoResearch 变体）

每一轮内部执行这四步，缺一不可：

```
Modify → Commit → Verify → Keep / Revert
```

### 2.1 Modify
- 基于上轮结果 + 目标挑**下一步最小改动**
- 一轮只做一件事（一个 feature / 一个 bug / 一次重构片段）
- 遵循 [code.md](code.md) 的 Karpathy 4 原则

**口语短指令的展开规则**：收到「debug / 重构 / 优化 / 看看 / 修一下」这类展开空间大的短指令时，**动手前 30 字内回显计划**（「我打算：1) X 2) Y 3) Z，动手？」），等用户「准 / 改 / 停」再动手。不要沉默扩展方向，也不要吐 5 步长计划。

### 2.2 Commit（**先于验证**）
- Commit message 格式：
  - 常规改动：`feat/fix/refactor: <what> · <why>`
  - 实验性：`experiment: <hypothesis>`
- **为什么先 commit**：失败可以 `git revert`，迭代成本趋近于零。不先 commit 就等于赌博。

### 2.3 Verify
每个 commit 都有对应的**机械验证**：
- 测试通过 / 编译通过 / lint 通过
- 目标指标改善（例如延迟、准确率、错误数）
- **UI / 前端改动**：用 Playwright MCP 真跑。最小三连：
  - `browser_navigate` 到改动页
  - `browser_snapshot` 看 DOM 结构
  - `browser_console_messages` 看有无 runtime error
  编译过 + 类型过 ≠ UI 可用。4.7 有时会吐能编译但交互坏的代码 —— 必须跑。

**不写验证 = 任务未完成**。见 [code.md](code.md) Part A §4。

### 2.4 Keep / Revert
- 验证通过 & 指标持平或改善 → **Keep**
- 验证失败 / 指标变差 → `git revert <sha>`，在 progress.md 记录失败原因
- **连续 2 轮同一处失败 → 停下问人**（见 §6 停止条件）

---

## 3 · 四条记忆通道

迭代之所以能复利，是因为每轮的发现外化成文件，下一轮冷启动能接上。四条通道：

| 通道 | 粒度 | 用途 | 生命周期 |
|------|------|------|---------|
| **git 历史** | 每次 commit | 代码变更的权威记录 | 永久 |
| **`progress.md`** | 每轮一段 | 时间线日志：做了什么、结果、下轮计划 | 追加，不清理 |
| **`tasks.json`**（可选） | 每任务一项 | 结构化 todo + 状态 | 用户可维护 |
| **`AGENT_MEMORY.md`** | 每模式一条 | 跨轮复用的模式 / 坑 / 决策 | 受控 ≤ 200 行 |

**为什么分四条而不是合一**：粒度不同 → 查询成本不同。找「昨天的代码改动」查 git；找「上轮做了啥」查 progress；找「改这个模块要注意什么」查 AGENTS。合一就只剩一个越来越长的文件，没法定位。

---

## 4 · AGENT_MEMORY.md 反膨胀协议

AGENT_MEMORY.md 是复利的核心，但不加约束就会变垃圾桶。硬规矩：

### 4.1 入门三闸（三条全过才写）

1. **不是代码能推导的** —— 能读代码发现的别写（项目结构、API 签名、文件路径）
2. **不是一次性的** —— 只对这次任务有用的写 progress，不写这里
3. **不是 code.md 已覆盖的** —— Karpathy 4 原则 / 4.7 契约不重复

### 4.2 格式强制（每条 ≤ 3 行）

```markdown
### <短标题，一句话>
**Why**: <一句话：为什么这是个问题 / 为什么这个方案对>
**How**: <一句话：下次什么情况下用这条>
```

禁止：长段落、背景故事、代码块（放 progress 里）。

### 4.3 分区限额

- `## Patterns` —— 可复用的做法，≤ 15 条
- `## Gotchas` —— 踩过的坑，≤ 10 条
- `## Decisions` —— 架构/方向决策，≤ 10 条

合计上限 **35 条 × 3 行 ≈ 150 行**。总长 ≤ 200 行硬上限。

### 4.4 剪枝（强制）

- **每 6 轮小结时**：执行「prune AGENT_MEMORY.md」步骤，合并重复、删除已不适用的
- **每 30 轮大回时**：更激进地压缩；长期不被引用（下文说明）的转到 `archive/agents-YYYY-MM.md`，不删但移出主文件

### 4.5 引用追踪（轻量）

每次实际用到某条 AGENT_MEMORY.md 条目时，在 progress.md 该轮记录里写：`refs: memory#<标题>`。30 轮大回时统计：**连续 30 轮未被引用的条目是归档候选**。

这避免了「写了没人用」的僵尸条目堆积。

---

## 5 · 节律

| 粒度 | 周期 | 动作 |
|------|------|------|
| 轮 | 10 min | Modify → Commit → Verify → Keep/Revert |
| 小回 | 6 轮 | 回顾 · 整理 · 清理 · 规划（见 §5.1） |
| 大回 | 30 轮 | 停下，产出战报，等人类指示（见 §5.2） |

**数字参数化，不写死**。量化项目可能 5min/轮，文档项目可能 20min/轮。默认值来自用户；skill 调用时可覆盖。

### 5.1 小回协议（6 轮时做）

按顺序执行，每一步都要产出可见证据：

1. **回顾** —— 读过去 6 轮的 progress.md，总结：做了 X，哪些成功、哪些失败、为什么
2. **整理** —— AGENT_MEMORY.md：新发现的模式加进去（过 §4.1 三闸），不合格的剔除
3. **清理** —— `git status` 看有没有遗漏未提交；检查有没有临时文件、调试 print、死代码应删
4. **规划** —— 下 6 轮的 micro-goal：一句话锚点 + 拆成 6 个候选动作（不一定全做，但都指向 micro-goal）
5. **prune AGENT_MEMORY.md** —— 按 §4.4 执行

小回的产出：progress.md 追加一段 `## Mini-Review @ round N` 记录以上 5 项。

### 5.2 大回协议（30 轮时做）

1. 执行一次小回
2. **战报**：追加 `## Big-Review @ round N` —— 基线 vs 现状（用指标 / 功能清单）、关键决策、未决问题
3. **归档**：过去 30 轮 progress 可以压缩为摘要放 `progress-archive/round-<start>-<end>.md`，主 progress.md 只留最近 30 轮
4. **AGENT_MEMORY.md 深度剪枝**：按 §4.4 的 archive 规则
5. **停下** —— 不开下一轮。等人类说「继续」。

---

## 6 · 停止条件（多维，任一触发即停）

| 维度 | 阈值 | 触发后做什么 |
|------|------|------|
| 大回终点 | 30 轮 | 按 §5.2 停，等人类 |
| 时间上限 | 默认 5 小时 | 产出中间战报，等人类 |
| Idle detection | 连续 3 轮无 commit | 标记「卡住」，停下问人 |
| 连续同错 | 同一验证失败 ≥ 2 轮 | 停下问人，附错误摘要 |
| 目标达成 | GOALS.md 全部判据满足 | 产出终报，停 |
| 预算耗尽 | Task budget 告警 | 优雅收尾，不开新 commit |

**停下问人时的标准消息**：
```
STOP @ round N · reason: <维度>
- 当前状态: <一句话>
- 卡点: <具体错误 / 冲突 / 歧义>
- 候选方案: <2-3 个，各带代价>
- 需要的决策: <一个问句>
```

不要写长篇大论，留给用户选择的空间。

**大回停顿后的无人值守续跑**（可选）：30 轮大回停下后，若用户想延续无人值守循环（比如过夜跑），**用户**可以用 `/schedule` 注册 cron trigger 定时重启下一轮。**agent 不自己起** `CronCreate` / `ScheduleWakeup` —— 节奏由人决定，这是 GOALS non-goals 的硬线。

---

## 7 · 跑偏检测

每轮的 Modify 步骤开始前，自问一句：

> 这一步能在 GOALS.md 的某条目标下找到归属吗？

找不到 → 停。两种可能：
- **目标过时** → 问用户要不要更新 GOALS.md
- **我在自娱自乐** → 回退到上一个仍对齐目标的 commit，重新规划

**绝不**自己扩张目标。「顺手优化」「发现了更有趣的问题」都是跑偏信号。

---

## 8 · 失败即信号（不是失败）

- 验证失败 → 正常，revert + 记录 + 继续
- 连续失败 → 停下问人（§6）
- 装作没失败 / 屏蔽错误让它绿 → **严重违规**，等同伪造

诚实的失败比粉饰的成功有价值得多。这是自我迭代能跑长的前提。

---

## 9 · 无状态但迭代

原则：**每一轮是冷启动**。不要依赖「我记得上轮」的内存。

- 每轮开始读 §1 的四个入口文件，把上下文重建起来
- 不要在消息历史里保存重要状态 —— 全部落到文件
- Token 预算紧张时：主动 compact / 让下一轮重启

**为什么**：长会话 token 通胀快、注意力稀释、缓存不友好。无状态迭代可以永远跑下去。

---

## 10 · 宿主项目需要提供的文件

本仓库被拉进宿主项目后，宿主需要（由人或首轮 agent 创建）：

| 文件 | 内容 | 谁写 |
|------|------|------|
| `GOALS.md` | 目标 + 成功判据 | **人** |
| `AGENT_MEMORY.md` | 初始为空 / 少量种子条目 | 人起 · agent 维护 |
| `progress.md` | 初始为空 | agent 每轮追加 |
| `tasks.json`（可选） | 初始 todo | 人/agent 共同维护 |

**`GOALS.md` 必须由人写**。这是「我们不管目标」的落地点：agent 不自创方向。

---

## 11 · 融入宿主已有约定

§10 的四个文件名**不是硬字面要求，是四个角色**。宿主如果已经有等价文件，**映射不替换**。

### 角色映射表

| 角色 | auto-iterate 默认 | 宿主可能已有 |
|------|------|------|
| 目标 | `GOALS.md` | `NORTH_STAR.md` · `ROADMAP.md` · `MISSION.md` |
| 时间线 | `progress.md` | `HANDOFF.md` · `ITERATION_REPORTS/` · `CHANGELOG.md` |
| 记忆 | `AGENT_MEMORY.md` | `DECISIONS.md` · `memory/*.md` · 自建 |
| git | `git log` | `git log`（总是一样） |

### 整合原则

1. **不要让宿主改名**。auto-iterate 是客，不要求主改。
2. **在宿主 CLAUDE.md 里记一张映射表**（见下方示例），告诉 agent「我们家的 X 对应 auto-iterate 的 Y」。
3. **协议按映射执行**。iterate.md 说「progress.md 每轮追加」，映射后就是「HANDOFF.md 每轮追加」。
4. **选一份为主，别两套并维护**。例如 `HANDOFF.md` 做详细交接 + `progress.md` 做 3-5 行轻量时间线；或只用一份，另一份弃。

### 宿主 CLAUDE.md 映射片段（示例）

```markdown
## auto-iterate 角色映射

| 角色 | 本项目用的文件 |
|------|------|
| 目标 | NORTH_STAR.md + ROADMAP.md（GOALS.md 做一页纸门面） |
| 每轮时间线 | HANDOFF.md（详细） + progress.md（3-5 行/轮，refs 指 HANDOFF） |
| 跨轮记忆 | AGENT_MEMORY.md（跨轮模式/坑） + DECISIONS.md（架构拍板，单次） |
```

### 反模式

- ❌ 两套并存、都维护 —— 每轮写两份日志
- ❌ 要求宿主把 `DECISIONS.md` 改名成 `AGENT_MEMORY.md`
- ❌ 抛弃宿主已有工作流去迁到 auto-iterate 默认文件
- ✔ 映射一次，永远生效

---

## 自检清单（每轮结束前过一遍）

- [ ] 做了一次具体的 Modify？
- [ ] 至少一次 commit（或 experiment commit + revert）？
- [ ] 跑了对应的验证？
- [ ] progress.md 追加了本轮记录？
- [ ] 有新模式要进 AGENT_MEMORY.md 吗？过了三闸吗？
- [ ] 这一步对得上 GOALS.md 吗？
- [ ] 要不要触发停止条件？
