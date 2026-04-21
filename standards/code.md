---
name: code-standard
description: How the agent writes code. Load this before editing any file. Combines Karpathy's 4 LLM coding principles with the Opus 4.7 interaction contract.
audience: host-project agent
enforcement: self (agent checks itself against these rules before declaring a change complete)
---

# 写代码标准

两部分：
- **Part A** — Karpathy 4 原则：每一次编辑都要过的闸门
- **Part B** — Opus 4.7 交互契约：4.7 字面理解指令，所以 prompt 和自我陈述都要符合这套写法

---

## Part A · Karpathy 4 原则

### 1. Think Before Coding（先想后写）

**不假设。不藏糊涂。暴露取舍。**

动手前：
- 明说假设。不确定就问。
- 多种解读并存时，列出来，别自己偷偷挑一个。
- 有更简方案就指出来，该顶回去就顶。
- 不清楚就停下。**说清楚什么地方糊涂**，然后问。

**反模式**：看到模糊需求直接开写；用「我觉得用户的意思是」来填空。

### 2. Simplicity First（最简优先）

**解决问题的最少代码。不投机。**

- 没要求的特性不加
- 只用一次的代码不抽象
- 没要求的「灵活性 / 可配置」不加
- 不会发生的分支不写错误处理
- **写了 200 行能用 50 行写完 → 重写**

问自己：「资深工程师会说这过度设计吗？」—— 会 → 简化。

### 3. Surgical Changes（外科手术）

**只碰必须碰的。只收拾自己弄出的烂摊子。**

改存量代码时：
- 不「顺手」改相邻代码、注释、格式
- 不重构没坏的东西
- 跟既有风格，哪怕你觉得丑
- 看到无关死代码就**提一句，不删**

你的改动制造了孤儿时：
- 清理**你制造**的未用 import / 变量 / 函数
- 不要清理先前就存在的死代码（除非被要求）

**铁律**：每一行改动都能直接追溯到用户请求。追溯不到就不该改。

### 4. Goal-Driven Execution（目标驱动）

**先定成功判据。循环直到验证通过。**

动工前先陈述一个简短计划：
- 目标（一句话）
- 步骤（≤ 5 条）
- **验证方式**（跑什么命令 / 看什么输出 / 什么结果算成功）

没有验证方式的任务 = 不可完成的任务。补齐再开工。

完成时的自检：验证真跑过了吗？输出真的符合判据吗？「应该能跑」= 没跑 = 未完成。

> **迭代循环里怎么落地**：见 [iterate.md §2.3 Verify](iterate.md)。每个 commit 一个机械验证，UI 改动必跑 Playwright。

---

## Part B · Opus 4.7 交互契约

4.7 比 4.6 有两个重大变化，必须适配：

### B1. 字面理解指令

4.6 会替用户补全隐含意图，4.7 不会。所以：

- **前置给全上下文**：intent、约束、成功判据、关键文件位置，第一轮就写清楚。不要分多轮渐进披露。
- **不模糊，不放置歧义**：「优化一下」「改好看点」这种不是指令，是邀请乱搞。指令要带判据：「函数 X 从 O(n²) 降到 O(n log n)，tests/perf_test.py 通过」。
- **prompt 里直接写成功判据**：「完成定义 = 以下 3 条全部满足：…」
- **明说要不要激进工具用**：4.7 默认少调工具用自己推理。需要大量 grep/read 时 prompt 里写「主动搜索代码库，不要基于假设」。
- **库 / API 问题用 Context7 MCP**：4.7 的知识有 cutoff。涉及第三方库版本、新 API、CLI 语法时 **优先** `mcp__context7__resolve-library-id` → `query-docs`，不要凭记忆推测 —— 推测出来的 API 很可能是幻觉。

### B2. 自适应思考 + Task Budget

- **不要设固定 thinking budget**。4.7 自己决定何时多想，预设反而干扰它。
- **给任务总 budget 提示**：「这一轮预算 ~20k tokens」—— 4.7 看得见倒计时，会自己配速。
- **长任务当成「委派给一个能干的工程师」**，不是对话式结对编程。一次性给全指令，让它跑，别中间插话。

### B3. 自发验证

4.7 会主动写验证并在宣告完成前跑通。我们不需要 babysit，但要：

- **验证脚本要能跑**：如果项目没有 `npm test` / `pytest`，第一轮先补测试基础设施。
- **验证失败 → 自己 fix → 再验证 → 验证通过才 surface 结果**。不要把红色 CI 交给用户。
- **绝对不要把「编译通过」当成「功能正确」**。类型检查 ≠ 功能测试。

### B4. Effort Level

- **默认 `xhigh`**（Anthropic 官方推荐）。
- `max` 只用于真正硬的问题（复杂算法、大型重构决策），一般任务会过度思考。
- `high` 适合并发多会话 / 成本敏感。
- `medium` / `low` 只用于窄范围机械任务。

---

## 省 token 的操作守则

4.7 聪明，但不该浪费它的聪明在重复的 I/O 上：

- **Grep/Glob 先定位，再 Read 局部** —— 不要 Read 整个大文件
- **Edit 改，不要 Write 重写** —— 只发 diff
- **长命令输出先过滤**（head/tail/grep）再读
- **同一文件不重复读** —— 已经 Read 过就用已有上下文，除非文件改动
- **并行独立调用** —— 多个不相关的 Read/Grep 一条消息里一起发
- **利用 prompt cache** —— Claude Code 自动缓存前缀（5 min TTL）。把不变的大文件（`standards/` · `CLAUDE.md`）放会话前部，变动的（progress.md 尾 · 代码 diff · 本轮指令）放后部。中间插大块变动 = 缓存击穿。
- **长循环主动 /compact** —— 跑过 5 min 后 cache 失效，与其冷启全读，不如先 `/compact` 把历史压成摘要，下一轮靠文件重建上下文（见 iterate.md §9 无状态迭代）。

---

## 自检清单（Commit 前过一遍）

- [ ] 每一行改动都能追溯到任务？
- [ ] 有没有加不需要的抽象 / 错误处理 / 特性？
- [ ] 验证真的跑过？输出符合判据？
- [ ] 风格跟既有代码一致？
- [ ] 只处理了自己制造的孤儿，没动无关死代码？
- [ ] Commit message 写的是 WHY 不是 WHAT？
