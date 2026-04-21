# Usage Guide — auto-iterate

> 截至 **2026-04-21** 核对过官方文档。本文只讲「怎么用」，不重复解释 `standards/` 的设计原理。

适用对象：
- **Claude Code**
- **Codex**

先记一句总原则：
- 想保留当前聊天上下文继续跑，用**会话内循环**
- 想后台定时跑，用**自动化**
- 想电脑关了还继续跑，优先选**云端自动化**

时间写法也先统一：
- 本文所有完整 prompt 示例，都用 `2026-04-22 09:00 Asia/Shanghai` 作为**真实绝对时间示范**
- 你复制时，通常只要把这一个时间改成你自己的结束时间
- 不要写「今晚」「明早」「12 小时后」这种相对时间

---

## 1. 先看这个：连续 vs 定时

这两个词不要混：

- **连续** = 一轮结束后，立刻或尽量立刻继续下一轮
- **定时** = 按 `1m` / `5m` / `15m` 这种节拍回来再跑

截至 **2026-04-21** 我核过的官方文档里：
- **Claude**：`/loop 下一轮` 最接近“连续”。但官方写的是 Claude 每轮后会在 **1 分钟到 1 小时**之间自选下一次时间，所以它**不是数学意义上的零间隔保证**
- **Codex**：公开文档里的 `thread automation` / `standalone automation` 都是**定时唤醒**，没有看到和 Claude `/loop` 完全对等的“上一轮刚结束立刻下一轮”的公开机制

所以如果你要的真的是：

> 不要 5 分钟，不要 15 分钟，我要一轮完就下一轮

那结论是：
- **Claude**：用 `/loop 下一轮`
- **Codex**：当前公开文档里**没有完全对等的官方方案**

---

## 2. 先选模式

| 你想做什么 | Claude Code | Codex | 备注 |
|------|------|------|------|
| 手动跑 1 轮 | `下一轮` | `下一轮` | 最稳，推荐先跑通一次 |
| 最接近连续跑 | `/loop 下一轮` | 当前公开文档未见完全对等方案 | Claude 最接近；Codex 暂无公开等价能力 |
| 每隔几分钟回来一次 | `/loop 5m 下一轮` | `thread automation` | 这是**定时**，不是连续 |
| 后台定时跑 | `/schedule` / routine | `standalone automation` | 适合巡检、过夜跑；也是**定时** |
| 电脑关了还继续跑 | **routine** | 当前官方文档里没有和 Claude routine 完全对等的云端本地项目方案 | Codex 项目自动化要求 app 运行、项目在磁盘上可用 |

---

## 3. 使用前提

开始前先确认这 4 件事：

1. 已把 `standards/` 装进宿主项目
2. Claude 宿主已挂 `CLAUDE.md`；Codex 宿主已挂 `AGENTS.md`
3. `GOALS.md` 已由人填完
4. 先手动跑通过至少 1 轮，再开自动化

没填 `GOALS.md` 就不要进循环。

---

## 4. Claude Code 怎么用

> 下面都给完整句子；默认示例结束时间统一写成 `2026-04-22 09:00 Asia/Shanghai`。复制时通常只改这一处。

### 4.1 手动跑一轮

直接说：

```text
下一轮
```

适合：
- 刚装完
- 先试 prompt 是否对
- 任务风险高，不想自动连跑

### 4.2 当前会话里最接近连续跑

直接说：

```text
/loop 下一轮
```

这代表：
- 在**当前 CLI 会话**里循环
- Claude 每轮后会自己选下一次唤醒时间
- 任务是**session-scoped**

按当前官方文档，动态 `/loop` 省略 interval 时，Claude 会在 **1 分钟到 1 小时**之间自选下一次时间。  
所以：
- 它是 Claude 侧**最接近连续跑**的官方方式
- 但它**不是严格零间隔保证**

如果你就是想“尽量别停，持续优化”，优先用它。

### 4.3 固定节拍跑

例如：

```text
/loop 5m 下一轮
```

或：

```text
/loop 1m 下一轮
```

适合：
- 你想自己控制频率
- 短任务想高频轮询
- 希望每轮之间节拍稳定

注意：这类写法是**定时**，不是“连续不断”。

### 4.4 无人值守跑 12 小时，但电脑开着

如果你只是离开座位，**电脑开着、Claude 会话也开着**，可以直接这样写。

下面先给完整可复制示例；如果结束时间不是 `2026-04-22 09:00 Asia/Shanghai`，只改这一处再发：

```text
/loop 按 .claude/standards/iterate.md 跑下一轮；严格遵守 §6 停止条件；如果当前时间已经超过 2026-04-22 09:00 Asia/Shanghai，请输出最终总结并停止
```

这版更贴近“尽量连续跑”。  
如果你接受固定节拍，而不是追求连续，再用下面这个：

```text
/loop 5m 按 .claude/standards/iterate.md 跑下一轮；严格遵守 §6 停止条件；如果当前时间已经超过 2026-04-22 09:00 Asia/Shanghai，请输出最终总结并停止
```

所以：
- **想尽量连续**：用不带 interval 的 `/loop`
- **想自己控节拍**：再用 `1m` / `5m` / `10m`

### 4.5 无人值守跑 12 小时，电脑可能关掉

这时不要用 `/loop`。按当前官方文档，`/loop` 和 CLI scheduled tasks 都是**当前会话/本机**相关；如果你要机器关了也继续跑，用 **routine**。

可以直接对 Claude 说。

下面先给完整可复制示例；如果结束时间不是 `2026-04-22 09:00 Asia/Shanghai`，只改这一处再发：

```text
请用 /schedule 为当前仓库创建一个 routine。

要求：
- 每小时运行一次
- 持续做代码迭代和验证
- 按 .claude/standards/iterate.md 执行
- 严格遵守 §6 停止条件
- 如果当前时间已经超过 2026-04-22 09:00 Asia/Shanghai，请输出最终总结并停止继续推进
- 只做与 GOALS.md 对齐的动作，不自创方向
```

如果你只想夜里跑 12 小时，建议再做两件事：
- 在 prompt 里写**绝对结束时间**
- 第二天手动检查并停用 / 删除这个 routine

### 4.6 停止 / 查看

- 停：`停`
- 看进度：`看进度`
- 强制小结：`做小结`
- 强制战报：`出战报`

---

## 5. Codex 怎么用

> 下面都给完整句子；默认示例结束时间统一写成 `2026-04-22 09:00 Asia/Shanghai`。复制时通常只改这一处。

### 5.1 手动跑一轮

直接说：

```text
下一轮
```

### 5.2 当前线程里“尽量连续”怎么理解

Codex 官方文档当前没有和 Claude `/loop` 完全同名、完全等价的 CLI 命令。  
如果你想“在这条线程里反复回来继续做”，官方给的是 **thread automation**。

最接近“高频持续跟进”的写法：

```text
请在当前线程创建一个 thread automation。

要求：
- 每 1 分钟回到这条线程一次
- 按 .claude/standards/iterate.md 跑下一轮
- 严格遵守 §6 停止条件
- 保留当前线程上下文
- 如果没有重要进展，就简短汇报
- 如果遇到需要我决定的事，就停下问我
```

这已经是 Codex 侧最接近“连续不停”的官方做法了。  
但要明确：它仍然是**按分钟唤醒**，不是“这一轮刚结束就零间隔立刻起下一轮”的 `/loop` 语义。

也就是说：
- 它适合“高频回来继续”
- 不适合把它描述成“无间隔连续跑”

如果你只是坐在当前会话前，想让 Codex别停地继续干，可以直接在当前线程说：

```text
从现在开始按 .claude/standards/iterate.md 持续跑下一轮。
不要创建 5 分钟或 15 分钟的 automation。
一轮结束就继续下一轮，直到命中 §6 停止条件，或者到达 2026-04-22 09:00 Asia/Shanghai 再停下。
```

但这属于“让当前会话持续工作”，**不是** Codex 官方 automation 文档里定义的定时机制。

### 5.3 thread automation 适合什么

适合：
- 想保留当前线程上下文
- 想让 Codex 继续跟同一个任务
- 想每几分钟回来继续一轮

不适合：
- 每次运行都应该独立
- 跨多个项目跑
- 想让结果作为独立后台任务汇总

### 5.4 standalone automation 适合什么

standalone automation = 每次都是**独立 fresh run**。

适合：
- 定时巡检
- 周期性 review
- 无人值守定时跑
- 想把结果作为单独 automation runs 看

如果是 Git 仓库，按官方文档，最好优先考虑：
- **worktree**：隔离自动化改动，避免撞到你当前未完成的本地工作
- **local project**：直接改主工作区，风险更高

### 5.5 无人值守跑 12 小时

如果你是 Codex，想**无人值守** 12 小时，推荐用 **standalone automation**。

注意这里是“无人值守定时跑”，不是“无间隔连续跑”。

可以直接这样说。

下面先给完整可复制示例；如果结束时间不是 `2026-04-22 09:00 Asia/Shanghai`，只改这一处再发：

```text
请创建一个 standalone automation。

要求：
- 在当前项目上运行
- 每 15 分钟执行一次
- 使用 worktree，避免影响我当前工作区
- 按 .claude/standards/iterate.md 跑下一轮
- 严格遵守 §6 停止条件
- 如果当前时间已经超过 2026-04-22 09:00 Asia/Shanghai，只输出最终总结，不再继续修改
- 只做与 GOALS.md 对齐的动作，不自创方向
```

如果你更想保留当前线程上下文，而不是独立 fresh run，就把它改成 thread automation。

下面先给完整可复制示例；如果结束时间不是 `2026-04-22 09:00 Asia/Shanghai`，只改这一处再发：

```text
请在当前线程创建一个 thread automation。

要求：
- 每 5 分钟回到这条线程一次
- 按 .claude/standards/iterate.md 跑下一轮
- 严格遵守 §6 停止条件
- 保留当前线程上下文
- 如果当前时间已经超过 2026-04-22 09:00 Asia/Shanghai，请输出最终总结并提醒我删除该 automation
```

### 5.6 Codex 的重要限制

按当前官方文档，Codex app 的 project-scoped automations 有这些前提：
- app 需要在运行
- 目标项目需要在磁盘上可用

所以如果你问的是：

> 我把电脑关了，还想让 Codex 继续在我的本地项目上跑 12 小时

按我截至 **2026-04-21** 查到的官方文档，**没有看到**和 Claude cloud routine 完全等价的方案。

换句话说：
- Claude 有 cloud routine，电脑关了还能跑
- Codex 现在公开文档里，自动化更像“本地 / app 后台调度”

### 5.7 停止 / 查看

直接说：
- `停`
- `看进度`
- `做小结`
- `出战报`

如果是 automation，本身也建议到 automation 列表里停用或删除。

---

## 6. 现成可复制模板

### 6.1 Claude：当前会话持续跑

```text
/loop 下一轮
```

### 6.2 Claude：无人值守 12 小时，本机开着，尽量连续

下面这句已经是完整示例；如果结束时间不是 `2026-04-22 09:00 Asia/Shanghai`，只改这一处再发。

```text
/loop 按 .claude/standards/iterate.md 跑下一轮；严格遵守 §6 停止条件；如果当前时间已经超过 2026-04-22 09:00 Asia/Shanghai，请输出最终总结并停止
```

### 6.3 Claude：无人值守 12 小时，电脑可能关掉

下面这句已经是完整示例；如果结束时间不是 `2026-04-22 09:00 Asia/Shanghai`，只改这一处再发。

```text
请用 /schedule 为当前仓库创建一个 routine：每小时运行一次，按 .claude/standards/iterate.md 跑下一轮；严格遵守 §6 停止条件；如果当前时间已经超过 2026-04-22 09:00 Asia/Shanghai，请输出最终总结并停止继续推进
```

### 6.4 Codex：当前线程高频继续（定时，不是零间隔）

```text
请在当前线程创建一个 thread automation，每 1 分钟回到这条线程一次，按 .claude/standards/iterate.md 跑下一轮；严格遵守 §6 停止条件；保留当前线程上下文；如果遇到需要我决定的事，就停下问我
```

### 6.5 Codex：当前会话里持续干，不走 automation

下面这句已经是完整示例；如果结束时间不是 `2026-04-22 09:00 Asia/Shanghai`，只改这一处再发。

```text
从现在开始按 .claude/standards/iterate.md 持续跑下一轮。
不要创建 5 分钟或 15 分钟的 automation。
一轮结束就继续下一轮，直到命中 §6 停止条件，或者到达 2026-04-22 09:00 Asia/Shanghai 再停下。
```

### 6.6 Codex：无人值守 12 小时（定时，不是零间隔）

下面这句已经是完整示例；如果结束时间不是 `2026-04-22 09:00 Asia/Shanghai`，只改这一处再发。

```text
请创建一个 standalone automation，在当前项目上每 15 分钟执行一次，使用 worktree，按 .claude/standards/iterate.md 跑下一轮；严格遵守 §6 停止条件；如果当前时间已经超过 2026-04-22 09:00 Asia/Shanghai，只输出最终总结，不再继续修改
```

---

## 7. 常见问题

### 想“不要间隔 10 分钟”

- **Claude**：直接用 `/loop 下一轮`
- **Codex**：如果要官方 automation，只能降到 `thread automation` + `每 1 分钟一次`；如果你盯着当前会话，可以直接要求“这轮结束就继续下一轮”

### 想“真的零间隔，一轮结束立刻下一轮”

- **Claude**：`/loop 下一轮` 最接近这个语义，但官方仍是动态调度，不是零间隔书面保证
- **Codex**：当前官方文档没有看到和 Claude `/loop` 完全对等的公开命令；automation 只能做到分钟级唤醒

### 想跑 12 小时，但只跑这 12 小时

最稳的写法是：
1. 在 prompt 里写**绝对结束时间**
2. 到时间后人工停用 / 删除 automation

### 想夜里跑，但第二天再看结果

- **Claude**：优先 routine
- **Codex**：优先 standalone automation

### 想保留当前线程里的上下文

- **Claude**：`/loop`
- **Codex**：thread automation

### 想每次都独立 fresh run

- **Claude**：routine / scheduled task
- **Codex**：standalone automation

---

## 8. 官方来源

- Claude Code scheduled tasks: https://code.claude.com/docs/en/scheduled-tasks
- Claude Code routines: https://code.claude.com/docs/en/web-scheduled-tasks
- Claude Code overview: https://code.claude.com/docs
- Codex automations: https://developers.openai.com/codex/app/automations
- Codex CLI slash commands: https://developers.openai.com/codex/cli/slash-commands
- Codex AGENTS.md: https://developers.openai.com/codex/guides/agents-md
