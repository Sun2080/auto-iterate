---
name: code-standard
description: How the agent writes code. Load this before editing any file. Combines Karpathy's 4 LLM coding principles with a cross-agent interaction contract.
audience: host-project agent
enforcement: self (agent checks itself against these rules before declaring a change complete)
---

# 写代码标准

两部分：
- **Part A** — Karpathy 4 原则：每一次编辑都要过的闸门
- **Part B** — Agent 交互契约：抽取 Claude Code / Codex 这类 coding agent 的共性约束，避免工作流绑死在单一平台

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

> **迭代循环里怎么落地**：见 [iterate.md §2.3 Verify](iterate.md)。每个 commit 一个机械验证，UI 改动必跑可用的 browser / computer-use / Playwright 类能力。

---

## Part B · Agent 交互契约（Claude / Codex 兼容）

平台不同，细节会变；但下面 4 条是共通硬线：

### B1. 指令必须字面、完整、可验证

- **前置给全上下文**：intent、约束、成功判据、关键文件位置，第一轮就写清楚。不要分多轮渐进披露。
- **不模糊，不放置歧义**：「优化一下」「改好看点」这种不是指令，是邀请乱搞。指令要带判据：「函数 X 从 O(n²) 降到 O(n log n)，`tests/perf_test.py` 通过」。
- **prompt 里直接写成功判据**：「完成定义 = 以下 3 条全部满足：…」。
- **明说要不要主动搜索代码库 / 联网 / 用工具**。需要大量 grep/read 或外部验证时，直接写出来，别让 agent 猜。
- **库 / API 问题优先查当前官方文档**：涉及第三方库版本、新 API、CLI 语法时，优先用平台已有的 docs / MCP / search 工具或官方文档，不要凭记忆推测。

### B2. 思考强度与预算

- **默认用平台里的高思考档**。Claude / Codex 这类 agent 都更适合用 `high` 或 `xhigh` 处理真实编码任务。
- **除非平台要求，不要把 thinking budget 写死**。比起手搓 token 数，更该给清楚的目标、边界和完成定义。
- **给任务总 budget 或 timebox 提示**：例如「这一轮预算 ~20k tokens / 15 分钟」，方便 agent 自己配速。
- **长任务当成“委派给一个能干的工程师”**：一次性给全指令，让它跑；不要每 30 秒插一句打断主线。

### B3. 自发验证

- **验证脚本要能跑**：如果项目没有 `npm test` / `pytest`，第一轮先补测试基础设施。
- **验证失败 → 自己 fix → 再验证 → 验证通过才 surface 结果**。不要把红色 CI 交给用户。
- **绝对不要把“编译通过”当成“功能正确”**。类型检查 ≠ 功能测试。

### B4. 平台差异放 adapter，不放标准本体

- `standards/` 只写共性的工作方法：目标、验证、记忆、节律、停机。
- `CLAUDE.md` / `AGENTS.md`、`/loop` / Automations 这类平台专属载体，放在 README 和模板层。
- 这样 Claude Code 和 Codex 共用一套标准，只换外层接入方式。

---

## 省 token 的操作守则

Agent 再强，也不该把上下文预算浪费在重复 I/O 上：

- **Grep/Glob 先定位，再 Read 局部** —— 不要 Read 整个大文件
- **Edit 改，不要 Write 重写** —— 只发 diff
- **长命令输出先过滤**（head/tail/grep）再读
- **同一文件不重复读** —— 已经 Read 过就用已有上下文，除非文件改动
- **并行独立调用** —— 多个不相关的 Read/Grep 一条消息里一起发
- **利用 prompt cache / 前缀复用** —— 把不变的大文件（`standards/` · `CLAUDE.md` / `AGENTS.md`）放会话前部，变动的（`progress.md` 尾 · 代码 diff · 本轮指令）放后部。
- **长循环主动 compact** —— Claude / Codex 都有 `/compact` 类能力；跑久了不如先压摘要，再靠文件重建上下文（见 iterate.md §9 无状态迭代）。

---

## 自检清单（Commit 前过一遍）

- [ ] 每一行改动都能追溯到任务？
- [ ] 有没有加不需要的抽象 / 错误处理 / 特性？
- [ ] 验证真的跑过？输出符合判据？
- [ ] 风格跟既有代码一致？
- [ ] 只处理了自己制造的孤儿，没动无关死代码？
- [ ] Commit message 写的是 WHY 不是 WHAT？
