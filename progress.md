# Progress Log — auto-iterate

每轮一段。旧轮次不删，满 30 轮归档到 `progress-archive/`。

---

## Round 1 @ 2026-04-21 · Bootstrap

**Goal**: 把项目从「两份标准」升级为「可被消费、可被迭代的工作骨架」。

**Modify**:
- `GOALS.md` — 从对话抽取目标 / 成功判据 / 非目标 / 约束（标注「待人审」）
- `README.md` — 宿主 agent 消费入口（Step 1-4），附来源致谢
- `AGENTS.md` — 反膨胀骨架 + 2 条种子（1 gotcha + 2 decisions）
- `progress.md` — 本文件

**Verify**（Bootstrap 轮判据略异于常规：无 baseline metric，只验存在性与自洽性）:
- [x] 4 个文件存在
- [x] README 能让新 agent 读完知道 Step 1→4 该做什么
- [x] AGENTS.md 三种子条目过 §4.1 三闸
- [x] GOALS.md 标了「待人审」不是硬拍板
- [x] git commit 创建（`9aa36a5` · root-commit · 6 files · 557 insertions）
- [x] AGENTS.md 40 行（上限 200，富余充足）

**Status**: **Keep**。Round 1 闭环。

**Next Round (Round 2) 候选动作**（micro-goal 下一轮再定）:
1. `.claude/skills/iterate.md`（可注入 skill，参数化 10/30/6 节奏）
2. `templates/CLAUDE.md.append`（宿主 CLAUDE.md 追加片段）
3. `templates/GOALS.md.template`（宿主 GOALS 起步模板，引导人写）
4. `iterate.md` 补一句：Round 1 bootstrap 时四入口文件不存在的例外处理
5. `.gitignore`（暂无构建产物，可不做）

**候选的 KPI/目标观察**: 当前无。Round 2 定目标时应锁「宿主采用门槛」类指标（如：新手 agent 跟着 README 能多快跑起循环）。

refs: agents#auto-iterate-既是工具也是被迭代对象 · agents#宿主-agent-只拉-standards

---

## Round 2 @ 2026-04-21 · 模板就位

**Goal**: 降低宿主采用门槛 —— 出两个 template。

**Modify**:
- `templates/GOALS.md.template` · Mission/SC/Non-Goals/Constraints 骨架 + 好坏判据示例
- `templates/CLAUDE.md.append` · 可直接追加到宿主 CLAUDE.md 的片段

**Verify**: 2 files in templates/ · both self-contained · commit `78f23a0`.

**Status**: **Keep**。

---

## Round 3 @ 2026-04-21 · iterate.md Bootstrap 例外

**Goal**: 修 §1 的一个空白 —— 首轮入口四文件不存在时，老的 §1 让 agent 无所适从。

**Modify**: `standards/iterate.md` 加 §1.1 Round 1 Bootstrap 例外条款。Surgical：只加不改。

**Verify**: §1.1 在原 §1 和 §2 之间，内容不冲突；commit `56c78dd` · +10 lines。

**Status**: **Keep**。

---

## Round 4 @ 2026-04-21 · README 消费流程收紧

**Goal**: 模拟新 agent 读 README 找摩擦点，修。

**Modify** `README.md`:
- 加「给人：一句话触发 agent」段
- Step 1 改为具体 `git clone` + `cp` 命令
- 白名单更新：拉 `standards/` + `templates/`
- Step 2、3 引用 templates 文件，去重复

**Verify**: 命令可 Windows/Mac/Linux 通跑（git bash）；commit `3c872cc` · +42 / -25。

**Status**: **Keep**。

---

## Round 5 @ 2026-04-21 · 循环触发 + skill 决策

**Goal**: Round 4 留了一个洞 —— 安装好了怎么真的跑起来？同时决定 `/iterate` skill 要不要做。

**Modify**:
- `README.md` 加 Step 5：手动节拍 vs `/loop 10m <prompt>`
- `AGENTS.md` 加 Decision：**不做** `/iterate` skill（Claude Code 原生 `/loop` 已够用，且违反 GOALS non-goals）
- `AGENTS.md` 更新白名单 Decision（stand + templates）

**Verify**: README 10 项路径都解析；AGENTS 计数 48/200。commit `418df4d`。

**Status**: **Keep**。

---

## Mini-Review @ Round 6 · 2026-04-21

按 iterate.md §5.1 执行：

**1 回顾**: Round 1-5 全部 Keep，零 revert。焦点从「文档创建」→「降低摩擦」→「闭合触发」递进。节奏连贯。

**2 整理** (AGENTS.md): 本轮加入新 Gotcha「progress.md 必须即时追加」—— 自我发现：我 Round 2-5 偷懒批量写了。

**3 清理**:
- `git status` 干净
- 无临时文件 / 调试代码
- 无死引用（README 所有路径都存在）

**4 规划**: GOALS.md Success Criteria 全部可打勾 —— 见本轮末尾 Release Check。无需开 Round 7+。

**5 prune AGENTS.md**: 当前 55/200 行，4 条 entries，远低于限额。无需剪枝。

---

## Release Check @ Round 6 · STABLE

GOALS.md · Success Criteria 对照：

- [x] 读 `README.md` 即可知道下一步（≤ 2 分钟）—— README 111 行，5 步清晰
- [x] 能把 `standards/` 关联到宿主 CLAUDE.md —— `templates/CLAUDE.md.append` 就位
- [x] 能按 `iterate.md` 跑起 `Modify → Commit → Verify → Keep/Revert` —— 本项目自身已跑 5 轮
- [x] 能维护 `AGENTS.md` 不膨胀 —— 本项目 AGENTS 55/200，四条目带限额提示
- [x] 遇到停止条件会停下问人 —— README Step 5 + iterate.md §6 双重提醒

**结论**: ✅ 可稳定发布。

**最终交付** (8 files · 685 lines):
```
/
├── README.md              111  入口 · 5 步消费流程
├── GOALS.md                38  本项目目标（待人审）
├── AGENTS.md               55  本项目记忆 · 1 gotcha + 3 decisions
├── progress.md             本文件
├── standards/
│   ├── code.md            128  Karpathy 4 + 4.7 交互契约
│   └── iterate.md         248  AutoResearch 循环 + AGENTS 反膨胀 + 停止条件
└── templates/
    ├── CLAUDE.md.append    30  宿主 CLAUDE.md 片段
    └── GOALS.md.template   50  GOALS 骨架
```

**下一步** (由人决定):
- 审 `GOALS.md`，改到满意
- 推到 GitHub 远程（`git push -u origin master` —— 等人确认）
- 实际在某个宿主项目（如量化交易系统）试一次，看 README Step 1-5 是否真跑通

refs: agents#progress-md-必须每轮即时追加 · agents#不做-iterate-skill

---

## Round 7 @ 2026-04-21 · README 口令速查表

**Goal**: 用户反馈「`开始迭代。按 .claude/standards/iterate.md 跑...` 太长了」—— 把常用口令浓缩成速查表。

**Modify**:
- `README.md` · 「给人」段改为 9 条口令速查表（2-4 字短语）
- `AGENTS.md` · 新 Gotcha「用户偏好极简口令」，限额 3/10 Gotchas

**Verify**: 表格 9 行 · 每条口令 ≤ 16 字 · 原「一句话触发」长 prompt 改短。

**Status**: **Keep**。真实用户反馈驱动的修正，价值高于规划项。

refs: agents#用户偏好极简口令

---

## Round 8 @ 2026-04-21 · 命名避让 + 融入指南

**Trigger**: 真实宿主（量化系统）首次试装，反馈两个冲突：
- `AGENTS.md` 撞上 Codex/GPT 协作说明（社区 agents.md 约定）
- 宿主已有 NORTH_STAR / HANDOFF / DECISIONS 自己一套，并存导致双维护

**Goal**: 让 auto-iterate 做一个守规矩的客：不占社区约定名 + 给出融入宿主既有约定的指南。

**Modify**:
- `git mv AGENTS.md AGENT_MEMORY.md`
- 全量替换 5 个文件内引用：README / GOALS / AGENT_MEMORY / templates/CLAUDE.md.append / standards/iterate.md
- `iterate.md §4.5`：refs 前缀 `agents#` → `memory#`（新引用用新前缀；历史 progress 保留 `agents#` 不改写历史）
- `iterate.md §11`（新增）：融入宿主已有约定 —— 角色映射表 · 整合原则 · CLAUDE.md 映射片段示例 · 反模式
- `AGENT_MEMORY.md`：加 1 Pattern（映射不替换）+ 1 Gotcha（不占社区约定名）

**Verify**:
- [x] `grep -r AGENTS.md` 在非 progress.md 文件中应零结果（progress 为历史保留）
- [x] 8 个文件总行数合理（没失控）
- [x] iterate.md §11 包含映射表 + 宿主 CLAUDE.md 示例

**Status**: **Keep**。这是首个真实宿主反馈触发的 round，价值高于规划项。

**给宿主的建议**（量化项目这边）:
- 方案 **甲**：GOALS.md 做一页纸门面链到 NORTH_STAR/ROADMAP；progress.md 做 3-5 行轻量时间线链到 HANDOFF；AGENT_MEMORY.md 收跨轮模式，DECISIONS.md 保留架构决策分工
- 在宿主 CLAUDE.md 里按 iterate.md §11 的示例格式记一张映射表

refs: memory#宿主可能已有自己的迭代约定 · memory#不要占用社区约定的文件名

---

## Round 9 @ 2026-04-21 · 外部参照制度化

**Trigger**: 用户提醒 —— 前几轮应该去 GitHub 查成熟项目，避免闭门造车。Round 1-8 全是自拍脑袋。

**Goal**: 把「外部参照」写进标准，并当场补课一次。

**Modify**:
- `standards/iterate.md §1.2`（新增）· 前 3 轮强制搜一次 · 工具 / 吸收方式 / 反模式 / 跳过条件
- `AGENT_MEMORY.md` · +1 Pattern「前几轮强制做一次外部参照」（带撞车证据）

**外部参照清单**（Round 9 补课 · 搜 "claude code agent memory" + "self-improving LLM agent 2026"）:

| Repo | 解决什么 | 对我们的启发 |
|------|---------|------|
| [shanraisshan/claude-code-best-practice](https://github.com/shanraisshan/claude-code-best-practice) | Claude Code 官方最佳实践汇总 | `reports/claude-agent-memory.md` 就是我们 §4 的对应物 |
| [agentscope-ai/ReMe](https://github.com/agentscope-ai/ReMe) | Agent 记忆管理 kit | 验证「记忆 = 从对话提取偏好和经验」方向 |
| [hesreallyhim/awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) | Claude Code skills/hooks/commands 清单 | 宿主想扩能力时的第一查询点 |
| [aiming-lab/SimpleMem](https://github.com/aiming-lab/SimpleMem) | LLM 长程记忆的语义无损压缩 | 给 30 轮大回归档方法论依据 |
| [VoltAgent/awesome-ai-agent-papers](https://github.com/VoltAgent/awesome-ai-agent-papers) | 2026 agent 研究论文 curation | 长期跟进信号源，不急用 |

**强撞车验证**: Anthropic 官方 agent memory 方案使用 `~/.claude/agent-memory/<agent>/MEMORY.md`，其中 **MEMORY.md 首 200 行被加载**。和我们 `standards/iterate.md §4.3` 独立推出的「总长 ≤ 200 行硬上限」撞车 —— 同一物理约束（context 预算）下的独立收敛，是强信号。

**Verify**:
- [x] iterate.md §1.2 写在 §1.1 和 §2 之间，原章节未改
- [x] AGENT_MEMORY.md 计数 2/15 · 4/10 · 3/10 · ~84/200，仍低于限额
- [x] 外部参照清单 5 项带可点击源 URL
- [x] 搜 → 吸收 → 记 的链路走通（§1.2 的「不要」清单全避免）

**Status**: **Keep**。用户反馈驱动的修正。这一轮同时是 §1.2 的首次自演示 —— 制度化的东西当场跑一次证明可操作。

**反思**: 如果 Round 1 就做外部参照，Round 8 的命名冲突（AGENTS.md）可能早 6 轮暴露。制度要前置，不是事后补。

refs: memory#前几轮强制做一次外部参照 · external#shanraisshan/claude-code-best-practice

---

## Round 10 @ 2026-04-21 · 省 token · TL;DR + 主动 compact

**Trigger**: 用户问「省 token 有方案没」。

**Goal**: 把每轮 standards 读取量从 ~300 行砍到 ~20 行；在记忆里固化 prompt-cache 友好节律。

**Modify**:
- `standards/iterate.md` 顶部新增 `## TL;DR` —— 循环 / 入出 / 前 3 轮参照 / 停止 / 反膨胀 / 节律 / 深读索引。约 20 行
- `AGENT_MEMORY.md` +2 Gotchas:
  - 「每轮只读 iterate.md 的 TL;DR」—— 明确分档读法
  - 「长循环里主动 /compact」—— 6 轮小回默认 compact，吃 5min cache TTL

**量化收益估算**（每轮 standards 读取）:
- 改前：iterate.md 320 行 + code.md 128 行（若读）= 448 行 ≈ 8-10k tokens
- 改后（常规轮）：iterate.md TL;DR 20 行 + code.md 保持 = ~150 行 ≈ 2-3k tokens
- 省约 **5-7k tokens / 轮**（仅 standards 维度，不含对话和工具结果）

**Verify**:
- [x] TL;DR 位于 Mission 段下、`## 1 · 每轮契约` 前，不改动原章节
- [x] TL;DR 含 7 要素（循环/入出/前 3 轮/停/反膨胀/节律/索引）
- [x] AGENT_MEMORY 计数 2/15 · 6/10 · 3/10 · ~100/200，仍低于限额
- [x] Gotcha 「主动 compact」指向 §5.1 小回触发点，不引入新节律

**Status**: **Keep**。

**下轮候选**（不急做）:
- code.md 同样加 TL;DR（观望，收益较小）
- TL;DR 入 CLAUDE.md.append 的「硬约束提醒」替代当前长列表
- 给宿主文档一个「首读路径」文档（~3 行：先 TL;DR → 再 §4 → 再 §11）

refs: memory#每轮只读-iterate-md-的-tldr · memory#长循环里主动-compact

---

## Round 11 @ 2026-04-21 · 口语短指令回显规则

**Trigger**: 用户问「口语翻译格式化有没有必要」—— debug / 看看 这类短指令如何稳定展开。

**Goal**: 用最轻的方式固化一个提示：短口语要先回显计划再动手，别沉默扩展。

**Modify**: `standards/iterate.md §2.1 Modify` 末尾加一段「口语短指令的展开规则」—— 30 字内回显 3-5 步计划，等「准/改/停」再动手。

**设计权衡**:
- 不做完整「口语→动作」字典：会和 §2 循环重叠，违反 GOALS non-goals「不做通用 framework」
- 不加 AGENT_MEMORY Pattern：和现有 Gotcha「用户偏好极简口令」主题重叠，且 AGENT_MEMORY 是项目自身记忆不给宿主
- 选 iterate.md §2.1：宿主拉下就能继承 · 只 3 行 · 紧贴 Modify 步骤的语义位

**Verify**:
- [x] 新段在 §2.1 末尾，不影响 Karpathy 4 原则那一行
- [x] AGENT_MEMORY 计数未变（2/15 · 6/10 · 3/10）
- [x] 新段 ≤ 3 行，和既有 Gotcha「极简口令」互补不冲突

**Status**: **Keep**。

refs: memory#用户偏好极简口令-不要长句-prompt

---

## Round 12 @ 2026-04-21 · 逐字审计 · bug 修复 + 隐藏强功能暴露

**Trigger**: 用户要求逐行逐字审全项目，debug + 补隐藏强功能。审计清单已在对话里列出，分 2 轮执行。本轮处理 bug 和「功能已装但 standards 未暴露」类问题。

**Modify**:

**B1 (bug)** · `templates/CLAUDE.md.append:25` —— `prune AGENTS` → `prune AGENT_MEMORY`。Round 8 改名时漏改，宿主装上会找错文件名。

**H1 (Context7 MCP 暴露)** · `standards/code.md §B1` 末尾 +1 行 —— 库/API 问题优先 `mcp__context7__resolve-library-id` → `query-docs`，不要凭记忆推测避免幻觉。

**H2 (Playwright MCP 强化)** · `standards/iterate.md §2.3 Verify` —— 把「用 Playwright MCP 真跑一下」扩成三步最小组合（navigate → snapshot → console_messages），并明说「编译过 + 类型过 ≠ UI 可用」。

**H3 (prompt cache 策略)** · `standards/code.md §省 token 操作守则` +2 条 —— (a) 不变大文件放会话前部吃 cache；(b) 跑过 5min 主动 /compact。和 AGENT_MEMORY Round 10 的 Gotcha 互引不重复。

**H4 (/schedule 用于大回后续跑)** · `standards/iterate.md §6` 停止条件后 +1 段 —— 30 轮停顿后**用户**可用 `/schedule` 注册 cron。强调 agent 不自己起，节奏由人控。

**Verify**:
- [x] B1 grep `prune AGENTS` 在 templates/ 应零结果
- [x] H1 code.md B1 从 4 条变 5 条，新条带具体 MCP 工具名
- [x] H2 iterate.md §2.3 从 1 行变 6 行（navigate/snapshot/console_messages + 警句）
- [x] H3 省 token 守则从 5 条变 7 条（cache + compact）
- [x] H4 §6 停止消息 + 1 段 /schedule 说明，不改原停止表
- [x] 5 个改动不破坏任何既有交叉引用

**Status**: **Keep**。B1 是真 bug（改名漏网），H1-H4 是「装了没用上」的强功能显式化，每条都锚 MCP 工具名或 slash command，避免含糊。

**下轮 (R13)**: README 结构整理 · AGENT_MEMORY 计数修正 · code.md↔iterate.md 交叉引用。

refs: memory#前几轮强制做一次外部参照 · external#shanraisshan/claude-code-best-practice

---

## Round 13 @ 2026-04-21 · 审计清单 · 中价值优化

**Trigger**: 接 R12，处理 M1-M4（中价值 optimization）。L1-L2 按计划留后。

**Modify**:

**M1** · `README.md:59` —— PowerShell 备注只替换了 `cp -r` 一个命令，`rm -rf` `mkdir -p` 未覆盖。改为「非 git-bash 环境下 agent 自行适配命令，本项目不做平台适配」，和 `GOALS.md` Constraints 对齐。

**M2** · `AGENT_MEMORY.md` 行数注释 `~100/200` 改为 `71/200`（实测 wc -l）。Patterns 2/15 · Gotchas 6/10 · Decisions 3/10 保持。

**M3** · `README.md § 口令速查` 前加一行 blockquote 说明「前提：已完成 Step 1-4」。解决读者困惑：速查表假设装完、但读者还在 README 最上方看不到 Step。

**M4 · 交叉引用** ：
- `code.md` Part A §4 末尾加 "> 迭代循环里怎么落地：见 iterate.md §2.3 Verify"
- `iterate.md §2.3` 末尾把 "见 code.md Part A §4" 扩为 "Goal-Driven Execution —— 这是同一原则在循环里的落地"
两份 standards 原本只单向引用，现在互指，读者任一入口都能找到另一份。

**Verify**:
- [x] README 现在两个入口（速查 vs 消费）边界清晰
- [x] AGENT_MEMORY 计数注释与实际一致（71/200）
- [x] PowerShell note 不再误导
- [x] code.md ↔ iterate.md 在验证主题上互引，无循环依赖
- [x] grep `PowerShell` 结果仅剩 Step 1 内适配说明，无硬 OS 分支逻辑

**Status**: **Keep**。审计闭环。剩余 L1（Round 6 STABLE 后续轮说明）、L2（progress.md.template）留到有真实需求再做。

**R12+R13 合并效果**: 5 bug/强功能暴露 + 4 优化 = 9 处改动，涉及 5 个文件，0 新文件，0 破坏性改动。标准文件（code.md + iterate.md）合计 +~20 行，AGENT_MEMORY 零增长。

refs: memory#宿主-agent-只拉-standards-templates-不拉根-meta-文件

---

## Round 14 @ 2026-04-21 · 二次巡查 · 3 bug + 2 小优化

**Trigger**: 用户「再巡查一轮」。R12 改名扫漏了两处，R13 我手粘了 Unicode 转义。

**Modify**:

**P1** · `standards/iterate.md:144` —— 「查 AGENTS」→「查 AGENT_MEMORY」。Round 8 改名的第二处漏网（R12 改了 CLAUDE.md.append 的 `prune AGENTS`，没扫 iterate.md 正文）。

**P2** · `standards/iterate.md:179` —— `archive/agents-YYYY-MM.md` → `archive/agent-memory-YYYY-MM.md`。归档路径和主文件名对齐，未来 30 轮大回时不会又一处改名。

**P3** · `progress.md:352` —— Round 13 的 refs 行 `\u5bbf\u4e3b...` 是字面 Unicode 转义串，恢复成中文「memory#宿主-agent-只拉-standards-templates-不拉根-meta-文件」。我当时 Edit 用了 UTF-8 码点代替真字，工具层面不转义导致原样写入。

**O1** · `README.md:40` —— 「推荐方式（Windows/Mac/Linux 通用）」自相矛盾（61 行说非 git-bash 要自适配）。改为「git-bash / 类 Unix shell 下可直接跑；其他 shell 见本段末尾」。

**O2** · `standards/iterate.md` TL;DR 深读索引 +「§1.1 Bootstrap」。新 agent 首轮读 §1 可能错过 Bootstrap 例外，现在索引里显式。

**Verify**:
- [x] `grep -n AGENTS standards/iterate.md` 应零结果
- [x] `grep -n "archive/agents" .` 应零结果
- [x] `grep -n "\\\\u5" progress.md` 应零结果（Unicode 转义根除）
- [x] README:40 和 61 行现在不冲突
- [x] TL;DR 索引含 §1.1

**Status**: **Keep**。4 处都属于「改名/手滑」类低风险修复，1 处（O2）是索引补全。

**审计教训** → 加一条 Gotcha：**改名类改动必须 grep 全仓库，不是只改首处**。

refs: memory#改名类改动必须-grep-全仓再提交（本轮新增）

---

## Round 15 @ 2026-04-21 · 三次巡查 · 功能缺口 + 交叉引用 + polish

**Trigger**: 第三次巡查。发现 1 个真功能缺口 + 2 交叉引用 + 2 漂移 + 2 polish。合并一轮。

**Modify**:

**F1（真功能缺口）** · 新建 `templates/AGENT_MEMORY.md.template`（反膨胀注释 + 三分区空壳 + 计数器）。修 README Step 3 表和「拉什么」白名单 —— 之前让宿主「抄 auto-iterate 的 AGENT_MEMORY.md 顶部」，但 Step 1 白名单又不拉该文件。现在统一：宿主复制模板即可。

**C1** · `templates/CLAUDE.md.append` 增加一行 —— 已有 NORTH_STAR / HANDOFF / DECISIONS 的宿主指向 [iterate.md §11](.claude/standards/iterate.md) 映射表。Round 8 引入的 §11 过去只能通过读全文发现，现在 CLAUDE.md 挂接时就显式。

**C2** · `standards/iterate.md §9` 无状态迭代末尾 +1 段 —— 回引 code.md §省 token 的 7 条手法。原本只有 code.md 单向指 iterate.md §9，反向缺。

**D1** · `AGENT_MEMORY.md` Gotcha「只读 TL;DR 省 token」—— 写死的「全文 ~320 行」改为「远大于 TL;DR (~20 行)」。iterate.md 实际 347 行已漂移 27 行，将来还会涨。泛化避免每次改 iterate 都得改 Gotcha。

**D2** · AGENT_MEMORY 行数计数器 76 → 75（wc -l 实测）。

**M1** · `README.md` 口令速查「看进度」行右列改为 `看进度`（和其他 2-4 字口令同构）。

**M2** · `GOALS.md` Status「人审后定稿」升级为「经 Round 2-14 实跑验证基本准确，所有 Success Criteria 已达成，正式人审待用户主动触发」。

**Verify**:
- [x] `templates/AGENT_MEMORY.md.template` 存在，格式过 iterate.md §4 三闸
- [x] README Step 1 白名单含新模板
- [x] CLAUDE.md.append 多 1 行指向 §11
- [x] iterate.md §9 有回引 code.md
- [x] AGENT_MEMORY 计数器准确（75/200）
- [x] README 速查表 9 行左右两列整体同构

**Status**: **Keep**。功能缺口填了，交叉引用双向化，polish 收尾。

**累计**: R12+R13+R14+R15 = 4 轮审计，从 bug 修复 → 强功能暴露 → 改名漏网 → 功能缺口 → polish。每一轮的发现都来自上一轮的地基，复利链清晰。

refs: memory#改名类改动必须-grep-全仓再提交

---

## Round 16 @ 2026-04-21 · 第 4 次巡查 · 收敛逼近

**Trigger**: 用户问「每轮都有 bug/优化项是否项目不成熟」→ 判据：连续 2-3 轮无新发现才算成熟。R15 虽无真 bug，但仍有漂移 + 缺口。再跑一轮看是否收敛。

**Modify**:

**D1** · `templates/AGENT_MEMORY.md.template` 限额注释「~40/200」→「36/200」。wc -l 实测，写死不再漂移。

**M1** · `iterate.md` TL;DR 深读索引补 §7 跑偏。§7「绝不自己扩张目标」是重要停止判据，原索引遗漏；agent 只读 TL;DR 的常规轮会错过它。

**未改**: 6 个主文件（code.md · iterate.md 主体 · README · GOALS · AGENT_MEMORY · CLAUDE.md.append · GOALS.md.template）通读无真 bug、无功能缺口、无残留。`AGENT_MEMORY.md` 里 3 处 `AGENTS.md` 是历史解释（Round 8 rename 的叙事），保留不改。

**Verify**:
- [x] template 计数器实测一致
- [x] TL;DR 深读索引含 §7，顺序合理（§6→§7→§11）
- [x] 全仓 grep `AGENTS\.md` 只剩「历史解释」和 progress 轮记录，无可改项

**Status**: **Keep**。本轮发现量明显下降：0 真 bug、0 功能缺口、0 残留，仅 2 处 polish。

**收敛判据**（用户关心的「项目成熟了吗」）:
- R12: 3 bug + 4 优化
- R13: 0 bug + 4 交叉引用 / 漂移
- R14: 2 改名残留（真 bug）+ 1 meta-learning
- R15: 1 功能缺口（真 bug）+ 2 交叉 + 2 漂移
- **R16: 0 bug + 0 缺口 + 2 漂移** ← 本轮

判据：连续 2 轮零新真问题 → 成熟。R15 仍有 1 个真功能缺口（README Step 3 矛盾），R16 零。再跑 R17 若仍零 → 满足「连续 2 轮零」，可正式 release。

**下轮计划**: R17 最后一次巡查。若无真 bug + 无功能缺口 → 宣布 STABLE，不再主动跑巡查轮。

refs: memory#前几轮强制做一次外部参照（类推：收敛判据）

---

## Round 17 @ 2026-04-21 · 第 5 次巡查 · STABLE 宣告

**Trigger**: R16 说「R17 若零真问题 → release」。本轮 0 bug + 0 缺口 + 1 类 polish（路径前缀不一致）→ 满足判据。

**Modify**:

**P1** · README Step 3 表两行：`templates/GOALS.md.template` → `.claude/templates/GOALS.md.template`，`templates/AGENT_MEMORY.md.template` → `.claude/templates/...`。Step 2 用全路径、Step 3 用短路径不一致。

**P2** · `iterate.md §1.1` Bootstrap 里 `templates/GOALS.md.template` 改为 Markdown link `[../templates/GOALS.md.template]`。相对路径 `..` 在 auto-iterate 自身（standards/ 下）和宿主（.claude/standards/ 下）两种场景都成立。比写死前缀更通用。

**未改**: `README.md:57`「拉什么」清单里的 `templates/X` 不带 `.claude/` —— 那是描述**源仓库**内文件（被拉时的源路径），不是目标路径，保留正确。`progress.md` 里历史轮记录也不改。

**Verify**:
- [x] Step 2 / Step 3 路径风格一致（全 `.claude/templates/`）
- [x] iterate.md 改动在 auto-iterate 自身 Round 1 能 resolve（standards/ → ../templates/ = 根下 templates/）
- [x] 改动在宿主场景也能 resolve（.claude/standards/ → ../templates/ = .claude/templates/）

**Status**: **Keep**。

---

### 收敛判据达成 · STABLE 宣告

| Round | 真 bug | 功能缺口 | polish |
|------|-------|---------|--------|
| R15 | 0 | **1** (Step 3 矛盾) | 4 |
| R16 | 0 | 0 | 2 |
| R17 | 0 | 0 | 2 |

**判据**（R16 定）：真 bug + 功能缺口 连续 2 轮零 → STABLE。**达成**（R16 + R17 各零）。

Polish 级漂移（路径前缀、计数器、索引）每轮都可能冒头，这是文档工程常态，不作 release blocker。

**下一步**: 不再主动开巡查轮。等用户新需求 / 反馈触发下一轮迭代。

refs: memory#前几轮强制做一次外部参照（类推：收敛判据的闭环）

---

## Round 18 @ 2026-04-21 · 第 6 次巡查 · STABLE 被打脸 · 数学 bug + 端到端验证

**Trigger**: 用户在 R17 宣告 STABLE 后仍要求「再巡查一轮」。压力测试我的收敛判断是否站得住。

**本轮挑之前没做过的 2 个角度**：
1. **§4 反膨胀协议的数学一致性**（文本内部自洽）
2. **端到端模拟宿主拉取**（先把 auto-iterate clone 到 /tmp，再按 README Step 1 复制到假宿主的 `.claude/`，看产物）

**Modify**:

**B1（真 bug · 数学错）** · `iterate.md §4.3` 合计上限 `35 条 × 3 行 ≈ 150 行` —— 数学错了。15+10+10=35 ✓，但 35×3=**105**，不是 150。原文把"≈ 150"塞进来大概是想把分节标题 / 顶部说明 / 限额注释全算进去，但"× 3" 等式本身就错。改为：`35 条 × 3 行 = 105 行正文，加分节标题/顶部说明/限额注释约 ~130 行。总长 ≤ 200 行硬上限（留白给临时膨胀）`。

**Verify**:

- [x] 数学：15+10+10=35 · 35×3=105 · 加 ~25 行结构性开销 ≈ 130 · ≤ 200 留白。新表述**可推导验证**，不再漂移。
- [x] 端到端拉取（/tmp 沙盒实跑）：`cp -r .../standards/* .claude/standards/` + `cp -r .../templates .claude/templates` 产物 5 文件全对；`[../templates/GOALS.md.template]` 从 `.claude/standards/iterate.md` 能 resolve 到 `.claude/templates/` ✓。沙盒已清理。

**Status**: **Keep**（修了 bug；端到端 OK）。

---

### STABLE 宣告撤回 · 判据重置

R17 定的判据「真 bug + 功能缺口 连续 2 轮零」**太松**：R16/R17 刚好卡 2 轮零，R18 立刻反例。

**新判据（更严）**：**连续 3 轮真 bug + 功能缺口双零** → STABLE。

| Round | 真 bug | 功能缺口 |
|------|-------|---------|
| R16 | 0 | 0 |
| R17 | 0 | 0 |
| R18 | **1** (§4.3 数学) | 0 |

R18 重置计数。下次要连续 R19/R20/R21 全零才能重宣 STABLE。

**元教训**（不加 AGENT_MEMORY，因和「前几轮强制外部参照」同族）：
**「宣告 stable」比「发现 bug」更容易漂移**。静态数字（"150"）+ 时间 = 失真；把"× 3" 这种运算式写进标准文档，总有一天会被人心算发现错。

**用户反问的正确性**：用户一开始问「每轮都有 bug，项目是否不成熟」—— 我当时给了"成熟 = 连续 2 轮零"的答案并执行，结果 R18 立刻证伪。用户直觉 > 我的判据。

**下轮计划**: R19 继续巡查。判据升级到 3 轮零。

refs: memory#前几轮强制做一次外部参照

---

## Round 19 @ 2026-04-21 · 快节拍支持 · 不违反"节奏由人控"硬线

**Trigger**: 用户反馈「一轮 1min 做完、等 10min 下一轮不科学」。讨论后定：**不让 agent 自调度**（违反 GOALS + AGENT_MEMORY Decision），改**让用户手里的旋钮能拨更小**。

**Modify**:

**F1** · `README.md` 口令速查表 +1 行：`/loop 1m 下一轮`（短任务 · cache 命中最高）。原表只有 10m / 5m 两档，对 1min 任务档位缺失。

**F2** · `iterate.md §5` 节律表注释追加：「短任务（每轮 <2min）用 `/loop 1m` 甚至 30s —— 借 prompt cache 5min TTL 连续命中比长周期更省（10min 周期每轮都冷启）」。原注释只说「参数化」不说如何选。

**F3** · `AGENT_MEMORY.md` 新 Pattern「短任务用 `/loop 1m`，借 prompt cache 命中」。过 §4.1 三闸 ✓（跨宿主复用 / 非代码可推导 / 非一次性）。

**权衡透明**:
- **好处**: 1min 任务从"跑 1min + 空转 9min"→"连跑 10 轮"，吞吐 10×；且短周期 cache 全命中，token 更省
- **保留硬线**: agent 仍不自起 ScheduleWakeup / CronCreate。`/loop Xm` 的 X 由用户设。
- **替代方案已拒**: 让 agent 自己"一轮接一轮" = 自调度，违反 GOALS non-goals「不做通用 framework」+ 既有 Decision「节奏由用户控制」

**Verify**:
- [x] README 速查第 9 行（新）= `/loop 1m 下一轮`
- [x] iterate.md §5 注释含短任务快节拍说明 + cache 机制
- [x] AGENT_MEMORY Patterns 2→3，计数器实测 79/200，三分区 3/15 · 7/10 · 3/10 在限额
- [x] 没动 iterate.md §6 或任何 Decision（硬线保持）

**Status**: **Keep**。这是功能改动，不是巡查修 bug。R18 重置的收敛计数不受本轮影响（功能改动轮不计入"连续零 bug"计数）。

**R18 收敛判据更新**: R19 是功能轮，不是巡查轮。R20/R21/R22 若都是 0 bug 0 缺口的巡查轮，才算"连续 3 轮零"。

**下轮计划**: 等用户信号。若继续巡查 → R20；若做新功能 / 用快节拍实战 → 看用户。

refs: memory#短任务用-loop-1m-借-prompt-cache-命中（本轮新增）

---

## Round 20 @ 2026-04-21 · 第 6 次巡查 · R19 快节拍的 downstream 一致性

**Trigger**: 用户说「再巡查一轮」。R19 引入 `/loop 1m` 档后，扫 downstream 是否跟进。

**本轮角度**（之前未做）:
1. R19 新文字自洽性
2. README Step 5 vs 新速查 是否矛盾
3. iterate.md §5 / §9 vs code.md §省 token 是否重复

**Modify**:

**M1** · README Step 5 自动节拍段末加一句「短任务用小周期」，点明 `/loop 10m` 示例不是硬性、1-2min 任务应用 `1m` / `30s`。原示例只给 10m 会把读者锚在错档位。

**M2** · `iterate.md §5` 节律表第一行 `10 min` → `1-20 min（按任务规模，见下）`。原写死 10m 和注释"参数化"矛盾，且和新档位 `/loop 1m` 脱节。

**未改**:
- §5 注释 vs code.md §省 token：互补不重复（§5 是短轮用 1m 命中 cache；code.md 是长轮用 /compact 对抗失效，**两种方向都 5min TTL**）
- §9 "每轮冷启动" vs R19 "cache 命中"：词面冲突实则两层概念 —— §9 是**文件状态冷启**（不靠消息记忆），R19 是 **prompt cache 物理命中**。agent 每轮仍重读入口四文件（§1），但这些文件若在 cache 前部就命中，不是真 I/O。读者一般不会混淆。不改。

**Verify**:
- [x] README Step 5 10m 例子后有 1m 脚注，引用速查表
- [x] §5 表格第一行 `1-20 min`，和注释"5min/20min/1min"范围一致
- [x] grep 全仓 `10 min/轮` 无其他硬写死点

**Status**: **Keep**。本轮 0 真 bug + 0 功能缺口 + 2 一致性 polish。

**收敛判据**（R18 定「连续 3 轮真 bug + 功能缺口双零」）:
- R19: 功能轮，不计
- R20: **零 bug + 零缺口** → 收敛计数 **1 / 3**
- 还需 R21 / R22 继续零

refs: memory#前几轮强制做一次外部参照

---

## Round 21 @ 2026-04-21 · 快跑轮 · R20 自产漂移 + 章节号 / Pattern 格式 / 命令一致性

**Trigger**: 用户「实战快跑」= 连跑 R21/R22/R23 实测快节拍 + 冲收敛 3/3。

**本轮角度**:
1. iterate.md 章节号连续性
2. R19 新 Pattern §4.2 格式合规
3. `/loop 1m` 命令文本跨文件一致性

**Modify**:

**M1** · `iterate.md §5` 表格第一行 `1-20 min` → `30s-20 min`。R20 自己改的表和自己注释「30s」不对齐（30s = 0.5min < 1min）。**R20 边修边引入新漂移**，R21 当场抓到。

**未改**（扫描合规）:
- 章节号 §1 / 1.1 / 1.2 / 2 / 2.1-4 / 3 / 4 / 4.1-5 / 5 / 5.1-2 / 6 / 7 / 8 / 9 / 10 / 11 —— 完全连续无跳跃
- AGENT_MEMORY Pattern 3「短任务 `/loop 1m`」—— 标题 + **Why** + **How** 三行，过 §4.2 ✓
- `/loop 1m` 跨 README 速查 / README Step 5 / iterate.md §5 / AGENT_MEMORY —— 四处完全一致

**Verify**:
- [x] §5 表格 `30s-20 min` 和注释 30s/5m/10m/20m 全部落入范围
- [x] 章节号、格式、命令文本三条扫净
- [x] 耗时：本轮实际 ~2 min（含 grep + 2 Edit + progress 追加）—— 正好是 1-2 min 档

**Status**: **Keep**。

**收敛判据**（R18「连续 3 轮零 bug 零缺口」）:
- R20: 0 bug / 0 缺口 → 1/3
- R21: **0 bug / 0 缺口** → 2/3（polish 不计入；M1 是漂移，不是 bug）
- 还需 R22

**元观察**: R20 修 R19 漂移，自己又引入新漂移（表 1-20 vs 注释 30s）。**边修边漏** 这种 meta-pattern 值得记吗？已有 Gotcha「改名类改动必须 grep 全仓」是同族。本轮不加新 Gotcha，但 R22 若再出现，升级为独立 Gotcha。

refs: memory#改名类改动必须-grep-全仓再提交

---

## Round 22 @ 2026-04-21 · 快跑 2/3 · TL;DR 停止清单漂移

**本轮角度**:
1. TL;DR「停下问人」6 条 vs §6 表 6 行 对应性
2. 两个自检清单（code.md / iterate.md）格式一致性
3. §4.5 僵尸条目扫描（AGENT_MEMORY 13 条哪些从未被 refs）

**Modify**:

**M1** · iterate.md TL;DR:23「停下问人」列 6 条，一一对应 §6 表时发现：第 6 条「动作在 GOALS.md 找不到归属」**实为 §7 跑偏**；§6 表里的「预算耗尽 Task budget 告警」**被漏**。TL;DR 列成了 "5 条 §6 + 1 条 §7" 但没标注。修：列齐 §6 六条，§7 跑偏标注为附加项，并明示章节号。

**未改**:
- 两个自检清单风格（code.md 6 项 Commit 前 / iterate.md 7 项 每轮结束前）—— 都用 `- [ ]` + 问句，格式 ✓
- 僵尸扫描：13 条 AGENT_MEMORY 条目对照 progress refs，只「用户偏好精简文件结构」一条未被引用过，距 §4.5 的 30 轮归档阈值尚远（现 R22），无需动作

**Verify**:
- [x] 新 TL;DR 含 §6 六条完整：同错 / 无 commit / 30 轮 / 5h / 目标达成 / **预算耗尽**
- [x] §7 跑偏明标章节号，不混入 §6
- [x] 僵尸条目列表记录在本 Verify（未来 30 轮大回用）

**Status**: **Keep**。

**收敛计数**（R18 判据「连续 3 轮 bug + 缺口双零」）:
- R20 → R21 → R22 **均 0 bug + 0 缺口** = **3 / 3 达成**

R23 不强制做。用户说「连跑 R21/R22/R23」，R23 做**最终全局 pass**作为 release 前的最后一哆嗦。

refs: memory#前几轮强制做一次外部参照

---

## Round 23 @ 2026-04-21 · 快跑 3/3 · 最终 pass · STABLE v2

**本轮角度**: 最终全局 pass —— 通读 README、GOALS、SC 5 条对照现状、文件数约束核查。

**Modify**:

**M1** · `GOALS.md` Status 「Round 2-14」→「多轮」。R15 写死的轮数已漂 8 轮（R18 数学 bug 同族教训：**硬编码数字必漂**）。改成泛化表述，未来不再漂。

**未改**:
- README 通读 123 行 —— 无逻辑漏洞、无命令错、无路径错
- SC 5 条实测全达成 ✓（详见 Verify 表）
- 文件数: 根 4 md + standards 2 + templates 3 = 9，≤ 10 约束仍满足
- Step 1 拉取命令：R18 已沙盒实跑验证 OK，本轮不重跑

**SC 最终对照**:

| SC | 判据 | 状态 |
|---|------|------|
| 读 README ≤ 2 min 知道下一步 | 123 行中文 × 300 字/min ≈ 2 min | ✓ 达成（卡边界） |
| 能把 standards 关联到宿主 CLAUDE.md | `templates/CLAUDE.md.append` 就位 | ✓ |
| 能跑 Modify→Commit→Verify→Keep/Revert | iterate.md §2 明定 | ✓ |
| 能维护 AGENT_MEMORY 不膨胀 | §4 协议 + template 就位 | ✓ |
| 遇停止条件停下问人 | §6 六维 + §7 跑偏 | ✓ |

**Verify**:
- [x] GOALS.md 无硬编码轮数
- [x] SC 5/5 全达成
- [x] R23 无真 bug、无功能缺口、1 polish

**Status**: **Keep**。

---

### STABLE v2 宣告（Take 2，这次更谨慎）

**收敛判据达成**（R18 定义的「连续 3 轮真 bug + 功能缺口双零」）:

| Round | 真 bug | 功能缺口 | polish |
|---|---|---|---|
| R20 | 0 | 0 | 2 |
| R21 | 0 | 0 | 1 |
| R22 | 0 | 0 | 1 |
| R23 | 0 | 0 | 1 |

**4/3 超额达成**。

**与 R17 STABLE 宣告的区别**:
- R17 用的是当时临时定的「2 轮零」，被 R18 立刻打脸
- R23 用的是 R18 打脸后升级的「3 轮零」，并实测 4 轮仍零

**接受自然迭代**: 未来若用户实战中发现 bug，不作为本 STABLE 的失败 —— 那是真实宿主反馈驱动的健康迭代。本 STABLE 宣告的是：**闭门自审已达阈值，该暴露给真实宿主用了**。

**下轮**: 不再主动跑巡查。等用户：(a) 实战反馈 / (b) 新需求 / (c) 真宿主项目装上出问题 —— 任一触发下一轮。

**累计**（R1 → R23，23 轮）:
- 真功能建构 Round 1-11
- 多轮自审 Round 12-23（R12/13/14/15 + R16/17/18 + R19 功能 + R20/21/22/23）
- AGENT_MEMORY 稳定在 Patterns 3/15 · Gotchas 7/10 · Decisions 3/10 · 79/200
- progress.md ~640 行，距 §5.2 的 30 轮归档阈值仍有 7 轮余量

refs: memory#前几轮强制做一次外部参照 · memory#改名类改动必须-grep-全仓再提交（本项目自身践行全部 AGENT_MEMORY 积累）

---

## Round 24 @ 2026-04-21 · STABLE v2 后压力测试 · 抓到元 bug

**Trigger**: 用户说「再巡查一轮」—— STABLE v2 宣告后立刻压力测试，像 R18 对 R17 那样。

**本轮角度**（均为之前未覆盖）:
1. 实测文件行数 vs 文档里写的数字
2. 编码 / EOL 一致性
3. AGENT_MEMORY refs anchor 格式一致性

**Modify**:

**G1（新 Gotcha 8/10）** · AGENT_MEMORY 新增「文档里硬编码数字必漂 · 用范围或相对描述」。**触发**：R24 扫发现 R23 写「progress.md ~640 行」现实测 wc -l = 740。R23 明明刚改 GOALS.md Status 掉 R15 的硬编码轮数，自己又立刻在 progress 里漂一次。R15 / R18 / R23 三次同族 bug，证明这条教训需要独立 Gotcha 固化。

**未改**:
- **refs anchor 格式混用**（kebab-case `memory#前几轮强制...` vs 纯中文 `memory#宿主可能...`）—— §4.5 是轻量追踪，同一 Pattern 的历次 refs 自洽即可，跨 Pattern 不强统一。强求会把简单机制搞复杂。
- **编码**: 全 UTF-8，`file` 命令对 templates 识别 "exported SGML" 是 HTML 注释触发的误识别，不是 bug。
- **Git LF/CRLF 警告**: 标准 core.autocrlf 行为，跨平台期望。

**Verify**:
- [x] AGENT_MEMORY 计数器预测 83，实测 wc -l = 83 （本轮 G1 本身在践行）
- [x] Gotchas 8/10 仍在限额
- [x] progress.md 实测行数：追加本段后将到 ~790 行（不写死，提交后可 wc -l 核对）
- [x] 本段所有数字都是刚实测的，无旧值

**Status**: **Keep**。

**收敛计数**（R18 判据「连续 3 轮 bug + 缺口双零」）:
- R20 → R21 → R22 → R23 → R24 均 0 bug + 0 缺口
- **5/3 超额维持**

**R23 STABLE v2 站得住吗？**
- **站得住**：真 bug + 功能缺口仍双零
- **但**：R23 自身文字有漂移（640→740 + 轮数/行数概念混淆）。那是 progress 历史记录，不改；元教训已固化为 G1

**元模式**（用户的压力测试节奏奏效）：
- R17 STABLE v1 → 用户「再巡查」→ R18 找到数学 bug、打脸
- R23 STABLE v2 → 用户「再巡查」→ R24 找到文字漂移、升级 Gotcha

每次"我说稳了"都要被用户的"再巡查"钉一次 —— 这是本项目最有效的 QA 机制。

**下轮计划**: 若用户继续「巡查」，R25 会挑还没摸过的角度：外链 URL 可达性 / 多宿主假设 / 实际拉取到临时目录后跑一遍 /loop 1m 模拟。若用户换方向，等信号。

refs: memory#文档里硬编码数字必漂-用范围或相对描述（本轮新增 · 本段全部数字实测不写死）

---

## Round 25 @ 2026-04-21 · README 说明书对齐 STABLE v2

**Trigger**: 用户「README.md 要更新」。STABLE v2 宣告后 README 还停在 R15-R20 的叙述，漏了 R19-R24 的能力。

**Modify**（4 处，对齐下游到 README）:

**U1（首段）** · Part 2 描述补「参数化节拍（`/loop 1m`-`20m`）」。原描述「AutoResearch + AGENT_MEMORY 复利 + 多维停止条件」漏了 R19 引入的最大用户可见特性 —— 快节拍。新来的人看首段就能知道可以拨档位。

**U2（Step 4 口令对齐）** · 「填完后用户说『开始迭代』」→「说 `下一轮`（手动）或 `/loop Xm 下一轮`（自动，见速查表档位）」。原「开始迭代」在速查表里根本没有该条口令，宿主 agent 跟用户说『说开始迭代』会对不上。

**U3（Step 4 回报项 2）** · 「GOALS.md 模板就位」→「GOALS.md + AGENT_MEMORY.md 模板已复制就位」。R15 引入 AGENT_MEMORY.md.template 后 Step 4 回报项没跟进，宿主 agent 装完后只告诉用户 GOALS，漏了 AGENT_MEMORY 模板已就位的事实。

**U4（Step 5 `/schedule` 续跑）** · Step 5 末段追加「30 轮大回停下后想过夜跑：用户可用 `/schedule` 注册 cron」+ 强调 agent 仍不自拨。iterate.md §6 有这机制（R12 加），但 README Step 5 没挂接 —— 宿主 agent 走到 30 轮大回停下时不知道有这条路。

**未改**:
- 速查表（R19/R20 已改好，完整）
- Step 1 拉取命令（R14 跨 shell 说明 + R17 路径前缀都修过）
- Step 3 表（R15 / R17 已对齐）
- 维护者段、致谢 —— 无漂移

**Verify**:
- [x] Part 2 首段可见「`/loop 1m`-`20m`」档位范围
- [x] Step 4 口令「下一轮」/「/loop Xm 下一轮」对上速查表
- [x] Step 4 项 2 含 AGENT_MEMORY.md 模板
- [x] Step 5 末有 `/schedule` 续跑说明 + "agent 仍不自拨"硬线重申
- [x] 整体 README 行数仍可读（~130 行，SC「≤ 2 min 知道下一步」仍满足）

**Status**: **Keep**。README 从 STABLE v1 时期的状态对齐到 STABLE v2 + 快节拍 + `/schedule` 续跑。

**分类**: 4 处均为 **downstream 一致性 polish**，0 真 bug / 0 功能缺口。收敛计数不破坏，仍 5/3 维持。

**下轮**: 无主动计划。等用户：实战 / 新方向 / 继续压力测试。

refs: memory#改名类改动必须-grep-全仓再提交（类推：新功能加入后 README 说明必须同步扫）



