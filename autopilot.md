---
name: autopilot-mode
description: 无人参与的自主执行模式。用户给 Mission(来自 GOALS.md 或 inline)+ 上限,agent 自研究 / 自规划 / 自实现 / 自验证 / 自停。与 standards.md 叠加使用。
audience: host-project agent
when-loaded: 用户消息含 `自动迭代` / `自动跑` / `自主跑` / `托管` / `autopilot` 等触发词时加载本文件
---

# Autopilot 模式

**默认模式**是 `standards.md` 定义的有人值守(找不到 GOALS 归属就停下问人)。本文件**只在用户显式启用**时生效,规则**覆盖** `standards.md §C`(Mission 锚定取代 GOALS 归属锚定),其余(Karpathy 4 + 4.7 契约)**继续适用**。

---

## 1 · 启动契约

### 1.1 识别启动口令

用户消息**包含**下列任一触发词即进入本模式:

- `自动迭代` / `自动跑` / `自主跑` / `自主模式` / `托管` / `autopilot`
- 等价意图:"放手做"、"无人参与"、"不参与"

### 1.2 两种语法

**档 1 · Mission 已在 GOALS.md**(常态):
```
自动迭代 4h
自动迭代 50 commit
自动迭代 到 2026-04-22 03:00
自动迭代 到明晚 22:00
自动迭代 到明天早上 8 点
```
Mission 从宿主 `GOALS.md` 读。GOALS.md 没填或 Mission 段为空 → **不启动**,提示用户:

> 未检测到 `GOALS.md` 的 Mission。请先填 `GOALS.md`(复制 `.claude/templates/GOALS.md.template`)或改用首启语法:`自动迭代:<mission>,限 <上限>`。

**档 2 · 首启 / 无 GOALS.md**:
```
自动迭代:做黄金交易辅助系统,限 4h
自动迭代:把登录重构到解耦,限 50 commit
托管:给博客做暗色主题,限 20 commit
```
Inline Mission 在冒号后、"限"前。启动时 agent 自建 GOALS.md 把 Mission 写进去。

### 1.3 上限解析

上限必须给,缺就不启动。支持格式:

| 类型 | 示例 |
|---|---|
| 相对时间 | `4h` / `4小时` / `30m` / `30分钟` / `2d` / `2天` |
| 绝对时间 | `到 2026-04-22 03:00` / `到明天 8 点` / `到今晚 22:00` / `到下午 3 点` |
| commit 数 | `50 commit` / `50 次` / `30 提交` |
| 预算 | `$20` / `20 美元` / `20 美金` |

**绝对时间解析**: 用当前时间(agent 环境里有)推算出 ISO 时间,记录为 autopilot 停止的 deadline。启动时在首行回显解析结果,让用户一眼看出是否理解对。

**为什么强制上限**: Mission 判据由 agent 自填,没有上限 = 可能无限加功能烧钱。上限是防跑飞硬线,不是约束。

### 1.4 启动后立刻做的事

1. 回显一行:`[autopilot] Mission = <一句话>, 上限 = <U>(= <解析后 deadline/数量>), 现在进入 Bootstrap`
2. 开始 Round 1(§2)

**不要**反复问用户确认细节。autopilot = 一次性给够,让它跑。

---

## 2 · Round 1 · Bootstrap

**第一轮必做,不能跳**。产物:一个填完的 `GOALS.md` + 第一个 commit。

### 2.1 外部参照(必做一次)

用 `WebSearch` + `WebFetch` + `mcp__context7__resolve-library-id` → `query-docs`:

- Mission 领域的成熟方案 / 开源项目 / 最佳实践
- 3-5 个参考,每个一句话总结"解决啥 + 对我们有用的点"
- 只读 README / 设计文档,不啃源码

### 2.2 自填 GOALS.md

用 `templates/GOALS.md.template` 起草:

- **Mission**: 用户给的原话,不改
- **Success Criteria**: 3-7 条,**每条必须带机械判据**("跑什么 / 看什么 / 什么算过")。禁止"性能更好"这种无判据的。
- **Non-Goals**: 3-5 条,基于外部参照识别的已知坑 / 避免的方向
- **Constraints**: 技术栈 / 依赖限制(能推导就写,推不出就空)
- **Discoveries**: 空,后续追加

### 2.3 首轮 commit

即使只是建 `GOALS.md`,也要 commit。message: `autopilot: bootstrap · Mission=<M> · <N> criteria drafted`。

---

## 3 · 核心循环

每轮执行:

```
Modify → Commit → Verify → Keep / Revert
```

**每轮契约** —— 沿用 `standards.md` 的 Karpathy A4(目标驱动)和 4.7 B3(自发验证)。额外硬线:

- 每轮至少 1 commit(失败实验也 commit 再 revert,留轨迹)
- 每轮 progress.md 追加 3-5 行(做了啥 / 验证结果 / 下一步)
- 有复用价值的发现 → 追加到 `GOALS.md § Discoveries`
- UI 改动用 Playwright MCP 真跑(`browser_navigate` + `browser_snapshot` + `browser_console_messages`),没装就先装或改方案

**硬线**: 动手前自问"这一步在 **Mission** 下能找到归属吗?"
- 能 → 做
- 不能 → **不是停下问人**(没人),**是硬停**(§5)

---

## 4 · 自救机制

没人接应 = 必须自己爬出来。规则:

### 4.1 同错 pivot

- 同一验证连续失败 **3 次** → `git revert` 到上个稳定态 + 换候选方向(从 Success Criteria 挑另一条未完的)
- pivot 时 progress.md 记:`pivot #N: <from X> → <to Y>, reason: <err>`

### 4.2 idle pivot

- 连续 3 轮无 commit(查 `git log --since=<round start>`) → 标"卡住" + 换候选
- 连续 5 轮 commit 但无代码功能增量(纯 rename / 格式化) → 自评是否跑偏,偏则 revert

### 4.3 pivot 穷举

- Success Criteria 候选全部 pivot 过且无果 → **硬停**(§5)
- **不允许**自创新方向:Mission 下无其他候选 = 该收工了,不要发挥创造力

---

## 5 · 硬停触发(任一即停)

| 条件 | 判据 |
|---|---|
| 上限到 | 时间到 / commit 数到 / 预算耗尽 |
| Mission 达成 | Success Criteria 全勾 + **再跑一次全验证通过** + 自评 Mission 对齐 |
| 自救穷举 | §4.3 |
| Mission 冲突 | §6 检测到 |

**硬停不拖**。触发即:
1. 最后一个 commit 收尾(不开新改动)
2. 写 HANDOFF(§5.1)
3. 停,不 `ScheduleWakeup`

### 5.1 HANDOFF 模板

写到 `progress.md` 末尾:

```markdown
## HANDOFF @ <ISO 时间> · round <N>

**停因**: <上限到 / Mission 达成 / 自救穷举 / Mission 冲突>
**做到哪**: <一句话总览> (commit <sha>)
**达成度**: <勾了几条 / 总几条> Success Criteria
**关键 Discoveries**:
- <1>
- <2>
- <3>
**未做项**:
- <criteria 里没勾的,每条一行>
**续跑指令**: <一句话,用户回来复制即可恢复>
```

**"续跑指令"写法**:
- 未完成: `自动迭代 <新上限>`(Mission 留在 GOALS.md,agent 重读 GOALS + progress 末段就能接)
- 已完成: `(Mission 已达成,无需续跑)`
- 冲突: `需你审 Mission/criteria 后决定方向`

**停时不做的事**:
- ❌ `ScheduleWakeup` 续下一轮
- ❌ 发邮件 / 通知
- ❌ "等一下再试"

autopilot 结束 = 安静地停。用户回来看 progress.md 尾的 HANDOFF 段自行判断。

---

## 6 · Mission 对齐自检

每 **5 commit** 做一次(第 5 / 10 / 15 ...),progress.md 追加一行:

```
alignment @ round N: <高/中/低> · 已做 <X>, 风险 <Y or 无>
```

- **高对齐**: 最近 5 commit 都能直接映射到 Success Criteria 某条
- **中对齐**: 有 1-2 个 commit 不映射但在 Mission 范围内("顺手改了依赖")
- **低对齐**: 多数 commit 找不到 criteria 归属 **且** 读起来偏离 Mission → **立即硬停**(Mission 冲突),写 HANDOFF

**为什么不让 agent 自己调整 Mission**: Mission 是硬线,autopilot 模式下 agent 动 Mission 就是越权。对齐度低 = 交还给人。

---

## 7 · 禁止清单

autopilot 模式下 agent **不做**:

- ❌ 自己动 Mission(Mission 是用户意图,agent 只能实现或停)
- ❌ 超出上限继续(上限是硬线,不是建议)
- ❌ 静默跑偏(对齐度低 = 立即停,不自我粉饰)
- ❌ 伪造验证通过(未通过就是未通过,revert + pivot 或硬停)
- ❌ 在无人值守下调度 cron / ScheduleWakeup 续命(停是停,不续)
- ❌ 创造 Mission 外的候选(Non-Goals 内的不碰,Mission 外的不想)

---

## 自检清单(每 commit 前)

- [ ] 这步能映射到 Success Criteria 某条吗?
- [ ] 不能映射的话,映射到 Mission 能过吗?
- [ ] 验证真跑了?通过了?
- [ ] progress.md 追加了?
- [ ] 有 Discovery 要记吗?
- [ ] 到第 5/10/15... commit 了吗?要不要做 Mission 对齐自检?
- [ ] 要不要硬停?(上限 / 达成 / 穷举 / 冲突)
