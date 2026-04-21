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

---

## Round 26 @ 2026-04-21 · 收版轮 · 真·零发现 · FROZEN

**Trigger**: 用户「再巡查一轮，无事就收版了」。明确终止条件。

**本轮角度**（均为之前从未覆盖）:
1. Round 号连续性（progress.md 里 `## Round N` 是否 1→25 无缺）
2. CLAUDE.md.append 里 `GOALS.md#non-goals` 锚点 vs GOALS.md 实际标题
3. code.md Part A/B 编号 + 独立章节顺序

**扫描结果**:

- **Round 6 "疑似缺失"**：grep 显示 R5 跳 R7。查 progress 82-152 行发现 R6 用了 `## Mini-Review @ Round 6` + `## Release Check @ Round 6` 双节标题，不是缺失。按 iterate.md §3「progress 追加不清理」原则不改历史。**非 bug**，是我扫描规则的盲区。
- **GOALS.md 锚点**: CLAUDE.md.append:31 `GOALS.md#non-goals` 对上 GOALS.md:21 `## Non-Goals`，GitHub kebab 规则自动小写化 ✓
- **code.md 结构**: Part A (### 1-4) / Part B (### B1-B4) / 省 token (独立) / 自检清单 (独立) —— 两种编号风格在各自 Part 内一致，Part 间独立，无冲突

**真 bug: 0 · 功能缺口: 0 · polish: 0** —— 真·零发现。

**收版宣告 · STABLE v3 (FROZEN)**:

| 版本 | 判据 | 结局 |
|------|------|------|
| STABLE v1 @ R17 | 2 轮零 | 被 R18 数学 bug 打脸 |
| STABLE v2 @ R23 | 3 轮零 | 被 R24 元 bug 打脸（R23 自产漂移）|
| **STABLE v3 @ R26** | **4 轮零 bug + R26 真零发现** | **用户「无事就收版」条件达成** |

**v3 区别**:
- v1/v2 是我自己宣告，都被压测打脸
- v3 是**用户明确终止条件 + 实测零发现**触发，性质不同
- 不再自我宣告"连续 N 轮零"，不再计数

**收版不等于完美**：
- AGENT_MEMORY Gotchas 8/10 记满了主要坑，未来宿主实战可能发现新的
- progress.md 含 R1-R26 完整迭代史，是给后来人的"这个项目怎么长出来"参考
- 本项目自身是 dogfooding：用自己的 iterate.md 迭代自己，证明了协议能跑

**之后迭代触发条件**（任一满足才开新轮）:
- 真实宿主项目装上遇到问题
- 用户新功能需求
- 第三方使用反馈 / PR
- 不再有"闭门自审" 的主动轮

**不 tag · 不 SemVer**（GOALS constraint 硬线）。STABLE 只是 progress.md 里的一个标记。

**累计产出**（wc -l 刚实测，不写死泛化）:
- 9 个文件（根 4 + standards 2 + templates 3），卡 Constraint ≤ 10 的 1 个余量
- AGENT_MEMORY: 3P · 8G · 3D，远低于 15/10/10 限额
- 26 轮迭代全部 Keep，零 revert

refs: memory#前几轮强制做一次外部参照（首尾呼应 · R9 引入 · R26 收版）

---

## Round 27 · 解用户疑虑 · 补 `/loop` 动态节拍档

**Trigger**: R26 收版后用户抛问题 —— "`/loop 1m`，如果这个任务 1min 内没处理完，1min 又起下一个，这样不是会堆起来"。真实担忧，不是巡查硬找茬。

**外部参照**（遵 §1.2 撞车验证原则）: 用 claude-code-guide subagent 查 https://code.claude.com/docs/en/scheduled-tasks.md 官方说明：
> "A scheduled prompt fires between your turns, not while Claude is mid-response. If Claude is busy when a task comes due, the prompt waits until the current turn ends."

**结论**: 用户的担忧**不成立**。`/loop Xm` 是**串行等待 + 跳过补偿**（serial wait + skip tick），不并发、不堆积。

**附加发现**: Claude Code 还有**动态 `/loop`** 模式（不带时间参数） —— Claude 用 `ScheduleWakeup` 自选 1min-1h 节拍，「跑完立即起下一轮」最贴用户原始诉求。动态模式内部走 `ScheduleWakeup` 是 Claude Code 原生机制（我的工具清单里就有这个 tool），和 AGENT_MEMORY Decision「agent 不自起 cron」**不冲突** —— 是**用户起了 `/loop`，agent 在其框架内自调速**，区别于 agent 在无用户触发下私自 CronCreate。

**Modify**: 最小三处 + footer 泛化
1. [standards/iterate.md §5](standards/iterate.md#L197): 节律表下追加一段，说 `/loop Xm` 串行不堆积 + 动态 `/loop` 推荐首选，并澄清和 §6 末段的兼容关系
2. [README.md](README.md#L22) 口令速查表: 手动「下一轮」下方加一行「连续跑 · 跑完立即起（动态节拍，无并发） · `/loop 下一轮`」
3. [AGENT_MEMORY.md](AGENT_MEMORY.md#L25) Pattern「短任务用 `/loop 1m`」改为「`/loop` 三档 · 按任务规模选，不会堆积」—— 新增非堆积事实 + 动态模式推荐 + 原生性澄清
4. AGENT_MEMORY footer 限额注释把「总行数 83/200」改为「总行数实测 wc -l」—— 上次 R24 的 Gotcha 8/10 教训：硬编码数字必漂，刚好又逮到一处

**Verify**:
- `wc -l` 各文件 ✓
- 三处改动相互呼应（速查表 → iterate.md §5 → AGENT_MEMORY Pattern），读者任一入口都能接上
- 未破坏现有格式（表格行数对齐、Pattern 格式 ≤ 3 行约束仍满足 —— Why 3 行 How 2 行接近上限，但 `/loop` 三档信息密度高，合理）
- 官方 URL 是用户提供的 scheduled-tasks 文档链接，有权威性

**Status**: Keep.

**意义**:
- R26 收版后的"真实问题驱动"轮，验证收版态的**可复活性**：用户一个真问题就激活一轮，不需要"巡查找茬"
- 解了关键模糊：「agent 不自起调度」的硬线不封死动态 `/loop` —— **用户起 loop，agent 在 loop 内自调速 ≠ agent 私自起 cron**
- 又抓一处硬编码数字（AGENT_MEMORY footer "83/200"）—— R24 Gotcha 的 active defense 生效

**下一轮触发**: 真实宿主装用遇问题 / 新用户诉求 / 动态模式实战反馈 / 文档 URL 需更新。不再自触发。

refs: memory#`/loop` 三档 · 按任务规模选，不会堆积 · memory#文档里硬编码数字必漂 · 用范围或相对描述

---

## Round 28 · README 三档节拍重排 · 动态模式升到推荐首选

**Trigger**: 用户指令「更新一下使用说明书」—— R27 已在 iterate.md §5 / AGENT_MEMORY / 速查表都提了动态 `/loop`，但 README.md 正文（Part 2 首句、Step 4、Step 5）还是 R17-R25 的旧叙述，只讲固定 `/loop Xm`。不更新则 README 和其他入口说法不一致。

**Modify**（单文件 3 处 · README.md）:
1. **Part 2 intro line**: `参数化节拍（/loop 1m-20m）` → `参数化节拍（动态 /loop / 固定 30s-20m）` —— 让首屏就看到两档
2. **Step 4 回报句**: 从「手动 or `/loop Xm`」两档改为三档任选（手动 · 动态 · 固定），和速查表对齐
3. **Step 5 整段重写**: 原文只分「手动」「自动」两档 + 「短任务用小周期」附加说明。重写为三档（① 手动 ② 动态推荐首选 ③ 固定周期），每档配一段 why/when，新增：
   - 动态模式解释：Claude 自选 1min-1h，不堆积无并发，内部走 `ScheduleWakeup`
   - 固定模式解释：串行等待 + 跳过补偿（官方行为）
   - **硬线澄清**：agent 私自起 cron 仍是越界；动态 `/loop` 里的 `ScheduleWakeup` 是用户触发后 agent 在其框架内自调速，不冲突 —— 和 iterate.md §5 新加段落一致

**Verify**:
- `git diff --stat`: +15/-10 —— 说明增量不大，三档表述比原两档更准确但篇幅可控
- README 总行数: 130 → 134 行 ·· 4 行净增，合理
- 一致性检查：速查表（line 22 "连续跑 · 跑完立即起"）· Part 2 intro（"动态 /loop"）· Step 4（"三档任选"）· Step 5（"② 动态 /loop · 推荐首选"）—— 四处指向同一概念，读者任一入口都接得上
- 未触 Step 1/2/3 —— 安装流程零改动，向后兼容

**Status**: Keep.

**意义**:
- R27 是「技术事实补录」，R28 是「文档一致化」—— 两轮共同完成「动态模式从新功能 → 推荐首选」的全入口覆盖
- 证实了 R26 STABLE 后的"真实驱动"轮模式有效：用户一问 → R27 补技术事实 → R28 顺手对齐文档，两轮连打不是巡查找茬
- 强化 AGENT_MEMORY Pattern「`/loop` 三档」的可复用性 —— 下次宿主 agent 读 README + iterate.md §5 + 速查表任一处都拿到完整三档图景

**下一轮触发**: 同 R27。继续被动等真实信号。

refs: memory#`/loop` 三档 · 按任务规模选，不会堆积

---

## Round 29 · README 顶部加「立即连续跑」零前提速答块

**Trigger**: 用户直接反馈「那现在我想 无间隔 连续不停 迭代优化，发什么提示词，你也不在说明书上给我写清楚」。

关键信号：**R27+R28 把动态 `/loop` 铺到六处**（palette · Part 2 intro · Step 4 · Step 5 · iterate.md §5 · AGENT_MEMORY），但用户没找到 —— 说明铺得再多，用户没有**零前提、单眼可见**的入口。palette 那行 `/loop 下一轮` 埋在表格第 3 行、带括号注释，需要读者主动扫描才会命中。

**Modify**: [README.md:14](README.md#L14) 在 palette 表格**之上**插一个高对比的单条口令块：

```
🔥 想无间隔、连续不停迭代？复制这句：
/loop 下一轮
```

附带一句解释（5s 可读完）+ 一句停止条件兜底 + 一句「停 = 说『停』」退出招。

**Verify**:
- 用户视角重读 README：打开页面 → 滚到 `## 给人` → 第一屏看到 🔥 speed answer → 5s 拿到口令。无需翻到 palette 第 3 行或 Step 5
- 信息冗余度：palette 第 3 行保留（对已经读熟的人方便查），顶部速答块是 TL;DR —— 两套互不矛盾
- emoji 🔥 使用：本项目首次，用于视觉锚点。AGENT_MEMORY 里「用户偏好精简文件结构」/「极简口令」两条未明禁 emoji，但**新增 Gotcha 候选**：节制用 emoji，仅作单点视觉锚不做装饰

**Status**: Keep.

**教训升级为 Gotcha 候选**（下轮 AGENT_MEMORY 评估入 Gotchas 9/10）:
### 铺信息 ≠ 用户能找到 · 高频口令要**零前提 TL;DR**
**Why**: R27+R28 把动态 `/loop` 铺到 6 处，用户 R29 还问「发什么提示词」—— 铺得再多，埋在表格/段落里就是找不到。
**How**: 高频口令（`/loop 下一轮`、`下一轮`）要放在 README 首屏 `##` 标题下**独立代码块**，5s 可见。速查表是给已经上手的人查细分，不是给新用户找入口用。

**Modify 此 Gotcha**: 先在本 progress 记录，R30 或小回时再评估是否写入 AGENT_MEMORY（反膨胀原则，不立刻加）。

**下一轮触发**: 同 R27/R28。继续真实驱动。

refs: memory#用户偏好极简口令，不要长句 prompt（同族：用户对「容易找到、容易复制」有高要求）

---

## Big-Review @ Round 30 · 大回战报

**Trigger**: R30 节点触发 `iterate.md §5.2` 大回协议（小回 + 战报 + 归档 + 深度剪枝 + 停下）。用户指令 `/loop 下一轮` 进入 R30，首动作是大回不是常规 Modify。

### 小回（5 步 · §5.1）

1. **回顾 R1-R29**: 三段弧 —— 骨架 R1-R8 / audit 闭环 R9-R16 / 三进三退 STABLE R17-R26 / 真实驱动 R27-R29（详见 `progress-archive/round-1-30.md` 导览快照）
2. **整理 AGENT_MEMORY**: R29 Gotcha 候选 evaluate 通过 §4.1 三闸 → 入 Gotchas 9/10（「铺信息 ≠ 用户能找到」）
3. **清理**: `git status` clean · 未追踪 0 · 无临时/调试代码 · 9 commits ahead of origin（待用户决定推 / 不推）
4. **规划下 6 轮**: **不规划**。R26 收版后用户明确终止自触发；下 6 轮触发条件见战报末段
5. **prune**: 8 条现 Gotchas 全数有效无重复无过时；3P/3D 同样健康；不做裁剪

### 战报正文

详见 [progress-archive/round-1-30.md](progress-archive/round-1-30.md) —— 5 节结构：

- **基线 vs 现状** 表（文件数 4→10 · AGENT_MEMORY 0→3P/9G/3D · 30 轮全 Keep 0 revert）
- **关键决策**（Decisions 3 条汇总）
- **未决问题 / 下一阶段触发条件**（真实宿主装用缺口 · 动态 `/loop` 实战体感 · 停止分支无真触发）
- **反模式复盘**（三次打脸的元教训）
- **归档决定**（主 progress.md 不裁剪，本文件保留 R1-R30 完整记录；archive 只做导览快照）

### 归档产出

- **新增** [progress-archive/round-1-30.md](progress-archive/round-1-30.md) · 一页纸压缩摘要，给后来者 5min 扫清 30 轮脉络
- **主 progress.md 不裁剪**: 977 行尚未过载，下次大回（R60）若过长再裁；届时写作模板已有（archive 文件本身）

### AGENT_MEMORY 深度剪枝结论

- Patterns 3/15 · **Gotchas 9/10**（新增 R29）· Decisions 3/10
- 全量条目在 progress.md 可搜到引用（§4.5 引用追踪 30 轮阈值未达归档线）
- **不 archive 任何条目** —— 现状合格

### §5.2 点 5：停下

**按协议不开下一轮 —— 不 `ScheduleWakeup`**。等用户信号：
- `继续` / `开下一轮` → 回到真实驱动模式（R27 起的模式）
- 真实宿主装用遇问题 → 对应问题单独起一轮
- 新功能诉求 / PR 反馈 → 触发对应轮

**Verify**:
- [x] `progress-archive/round-1-30.md` 创建（压缩摘要 · 98 行 · 五节齐全）
- [x] `AGENT_MEMORY.md` 新增 R29 Gotcha + footer 限额注释 8/10 → 9/10
- [x] 主 progress.md 追加本 Big-Review 段
- [x] `git status` 验证待提交：3 files (AGENT_MEMORY + progress + archive/new)
- [x] 不触发 `ScheduleWakeup`（退出 `/loop` 动态模式）

**Status**: **Keep**。Round 30 大回闭环。

**意义**:
- 本轮是第一次「大回触发」实跑 —— 验证了 §5.2 协议可操作（5 步清单 + 不下一轮）
- 和 R26 STABLE v3 区别：R26 是「真零发现 → FROZEN」（闭循环收版），R30 是「大回节点强制停 → 等人」（节律终点）。两种停下，语义不同，都合规
- 30 轮全 Keep / 0 revert 的事实，一半是项目顺 / 一半是 dogfooding 没遇到真对抗；真实宿主装用才是下阶段的"真试金石"
- 停止条件（§6）30 轮大回终点分支在本轮首次真触发 —— `iterate.md §6` 表格第一行「30 轮 → 按 §5.2 停」不再是理论

**下一轮触发**: 用户 `继续` / 新真实诉求 / 宿主反馈。agent 不自起下一轮。

refs: memory#前几轮强制做一次外部参照 · memory#`/loop` 三档 · memory#铺信息 ≠ 用户能找到（三条共同定义「真实驱动 > 自审」的元模式）

---

## Round 31 · 吸收两次 revert · Gotchas 触顶

**Trigger**: 用户 `/loop 下一轮` 入真实驱动；`git log -10` 里最新两条就是真实信号 —— R30 大回 STOP 后曾尝试过两轮都被回滚：

- `4c3a0bc` R31-old「Claude/Codex dual-stack · decouple standards from platform adapters」(+219 / −101 / 8 文件) → **revert** `c09a612`
- `5cd7cf2` R32-old「add usage guide · clarify continuous vs timed automation」(USAGE.md +411 新文件) → **revert** `ab10e8c`

两者同族：**扩 scope**，违反 GOALS 三条硬线 ——「文件数 ≤ 10」「不做 Windows/Mac/Linux 适配逻辑」「不做通用 agent framework」。此前 AGENT_MEMORY「用户偏好精简文件结构」（R1 种子）已预警过类似坑（早年 `guides/` 被悄悄删），但未 gate 具体阈值（+100 行 / 新文件），所以重复发生。

**Modify**: 只改 `AGENT_MEMORY.md`。新增一条 Gotcha（9/10 → **10/10**，触顶）：

> **revert 是强负反馈 · 扩张变更先过 GOALS 三闸**
> Why: R31-old +219 行/8F 与 R32-old +411 行/新文件连续被 revert，同族违反文件数 / 平台适配 / framework 三约束；再犯触 §6「2 轮同错」停。
> How: 单轮 ≥ +100 行或新文件前，对照 GOALS § Non-Goals/Constraints 三闸；`git log -10` 发现 revert 就 `git show` 读原改动，把「不做什么」落进 progress / memory 再动手。

footer 限额注释同步：`Gotchas 9/10` → `Gotchas 10/10（触顶·下次新加需先 prune）`。

**Verify**:
- [x] §4.1 三闸：① 非代码可推导（流程规则） ② 跨轮复用（任何扩张轮都能用） ③ code.md 未覆盖（code.md 管单次编辑标准，不讲 revert 吸收）
- [x] §4.2 格式：标题 + Why 一行 + How 一行，符合「每条 ≤ 3 行」
- [x] §4.3 限额：Gotchas 10/10 正好触顶 · 总行数 `wc -l` ≤ 200
- [x] §7 跑偏检查：本轮归属 GOALS 的 "自我迭代契约" —— 吸收真实反馈、防止同错复发
- [x] git 历史可追溯：四个 sha 都在 `git log -10` 可见

**Status**: Keep.

**意义**:
- 继 R29「铺信息 ≠ 用户能找到」之后，封顶「文档/功能扩张」的两类同族坑 —— 一条管入口可见度，一条管 scope 闸
- Gotchas **10/10** 触顶：本项目的「坑仓」第一次满格。下一条新坑来之前，必须先 prune / 合并 —— 这是 §4.4 剪枝协议的首次真触发触发条件之一
- §7 跑偏检测首次由**外部**（用户 revert）执行，而非 agent 自检。说明真实驱动模式下「用户 revert」= 停止条件的一个隐式维度，值得纳入 §6 表（下轮候选小改）

**下一轮触发**: 继续真实驱动。候选（低优先级、等真信号才做）：
- §6 停止条件表补一行「用户 revert 当轮 commit → 立即停 + 吸收」
- AGENT_MEMORY 触顶后的 prune 演练（目前无需，但首次触顶宜留演练机会）

refs: memory#用户偏好精简文件结构 · memory#铺信息 ≠ 用户能找到 · memory#改名类改动必须 grep 全仓再提交（三条同族：扩散 / 可见度 / 收敛性自检）

---

## Round 32 · 收版状态评估 · 交人决策

**Trigger**: 用户直问「现在状态如何，可以收版没」。真实驱动信号 —— 要明确答复。

**① GOALS Success Criteria 5/5**:
- [x] README.md ≤ 2min 知下一步（R29 顶部 TL;DR 代码块）
- [x] standards/ 关联宿主 CLAUDE.md（`templates/CLAUDE.md.append`）
- [x] iterate.md 循环跑得起来（本项目 31 轮 dogfood 自证）
- [x] AGENT_MEMORY 不膨胀（91 行 / 16 条 = 3P + 10G + 3D · 远低于 35 条上限）
- [x] 停止条件能触发（R30 触 §5.2 · R31 触 §6 user-revert 分支）

**② Non-Goals / Constraints 零触线**:
- 文件数 7（≤10）· 目录平铺 + `standards/` + `templates/` + `progress-archive/` 各一层
- 零运行时依赖 · 中文为主 · 未做平台适配 / 未做通用 framework（R31-old/R32-old 越线已 revert + 入 Gotcha 10 封堵）

**③ 与 R26 FROZEN 的 delta**（post-FROZEN 增量加固层，未动核心）:
- R27: `/loop` 非堆积事实 + 动态模式（技术补录）
- R28: README 三档节律对齐
- R29: README 顶部零前提 TL;DR
- R30: 大回战报 + 归档
- R31: revert 吸收 Gotcha（10/10 触顶）

**④ 警戒线（非 blocker）**:
- Gotchas 10/10 触顶 → 下条新坑前必须 prune
- 本地 ahead origin 1 commit（R31）→ 推不推交用户
- `.claude/` 未追踪 → 本机配置，正常不入仓

**结论**: **可以收版**。当前状态 = R26 FROZEN 基线 + R27-R31 真实驱动增量加固，语义仍归属 R26 的 FROZEN 锚点 —— **不自封 STABLE v3**（遵守 AGENT_MEMORY「铺信息 ≠ 用户能找到」同族：不造冗余标签）。

**下一步三选一（交人决策）**:
1. `push` → 发布 R27-R31 增量到 origin
2. 等真信号（`继续` / 宿主反馈 / 新诉求）→ agent 不自起
3. `tag stable-v3` 或类似标签（如你确实要新版本锚点）

**Modify**: 仅本 progress 条目，零功能改动（评估本身不产新模式，不动 AGENT_MEMORY）。

**Verify**:
- [x] 过 Gotcha 10 自检：单轮 ≤ +100 行、无新文件、未扩 scope
- [x] git status 干净（commit 后）
- [x] GOALS 五条逐项核查有证据

**Status**: Keep.

**§6「目标达成」分支触发 → 不 ScheduleWakeup · 退出 /loop 动态模式**。等用户真信号。

refs: memory#revert 是强负反馈 · memory#铺信息 ≠ 用户能找到

---

## Round 33 · README 补「停法 + 时间预算」速查 · 吸收 R32-old 教训

**Trigger**: 用户收版后直问「几个用法理顺一下 —— loop 间隔、有无人值守、停止条件（轮数 / 几小时 / 到固定时间）」，并要求「写个说明书，我不操心」。这是典型真实驱动信号 —— 用户亲手要文档，不是 agent 自增。

**Gotcha 10 自检（关键一步）**:
- 对比 R32-old（被 revert 的 USAGE.md +411 行新文件）：本轮**不新建文件**，在既有 README 里加 ~25 行 —— 单轮变更远低于 +100 行阈值
- 对照 GOALS Constraints：文件数仍 7（无增）✓ · 平铺结构不动 ✓ · 中文为主 ✓ · 零依赖 ✓ · 不做平台适配 ✓
- 对照 Non-Goals：不是通用 framework（是本项目的用户手册）✓ · 不做云端调度逻辑（只用既有 Claude Code 能力）✓
- 三闸全过 —— 和 R32-old 的区别是**用户显式点播 + 极小化增量 + 嵌既有文件**，不是 agent 自创扩张

**Modify**: `README.md` 在「给人」区尾、「消费入口」前，插一节「## 停法（省心版）」：
1. 两层停法表（工具层 / 项目层 §6）—— 填补既有 README 的真实缺口
2. 子节「想卡具体条件」—— 3 行 prompt 写法（N 小时 / 到点 / N 轮）
3. 子节「有人 vs 无人值守」—— 明说 `/loop` 会话绑定、`/schedule` 本项目暂不覆盖（回用户「目前没有云端任务」）

**Verify**:
- [x] 文件数不变（7）· README 146→~175 行（soft cap 合理）· 不新建任何 .md
- [x] 内容与既有速查表 / Step 5 不重复 —— 它们讲"用什么"，本节讲"怎么停"，正交
- [x] 与 R29 顶部 🔥 TL;DR 不冲突 —— TL;DR 管入口，本节管出口
- [x] 三闸复盘通过（见上）
- [x] git diff 只改一文件，纯新增，无删改 —— 回滚成本为 0

**Status**: Keep.

**意义**:
- Gotcha 10「revert 是强负反馈 · 扩张变更先过 GOALS 三闸」的**首次正向应用** —— 从"被 revert 的教训"转化为"做之前先过闸"，闭环生效
- R32-old 被 revert 不等于"这类需求不该做"，而是"做法不对"（新文件 vs 嵌入既有文件）—— 本轮证明了精细化的"do less"是可行的
- 给下 rdr R26 FROZEN 基线 + R27-R33 增量加固层里再加一片叶，仍未触动 standards/ 核心

**下一轮触发**: 真实驱动保持。本节真落地后，若用户在新项目实装时发现停法写错 / 漏 case，会是下一轮的明确触发点。

refs: memory#revert 是强负反馈 · memory#铺信息 ≠ 用户能找到（首次把两条 Gotcha 作为 **正面决策依据** 而非事后解释）

---

## Round 34 · §6 停止条件按值守模式分叉 · 补方法论缺口

**Trigger**: 用户在 R33 刚落的「停法」节上追加真实反馈 ——「有人值守则可以多停多问等人类；如果无人值守就不要傻傻等人类反馈，这个要考虑进去」。精准指出当前 §6 是一刀切「停下问人」，没覆盖无人值守场景。

**问题诊断**: `iterate.md §6` 所有停止条件的"触发后做什么"列都写死「停下问人 / 等人类」。无人值守（过夜跑、出门跑）下这等于"傻坐到天荒地老"，违反迭代循环的基本意图（能跑就跑）。

**设计原则**（三分类）:
1. **可自救类**（连续同错、idle）: unattended → revert + pivot 到 GOALS 已对齐候选；同故障 3 次无果才硬停
2. **交接点类**（30 轮大回、目标达成、预算耗尽）: 两模式都停，只是硬停的消息格式不同
3. **时长硬线**（5h timebox）: unattended 硬停（不等），attended 产中间战报等人

**Modify**:

**a) `iterate.md §6` 扩列**:
- 停止条件表「触发后做什么」→ 拆成「有人值守 / 无人值守」双列
- 「停下问人」标准消息保留，新增「HARD STOP」标准消息格式（强制写"怎么续"一句话）

**b) `iterate.md §6.1` 新增「值守模式」小节**:
- 口令声明：`/loop 下一轮`（默认有人值守）vs `/loop 下一轮 · 无人值守`
- 无人值守三原则：自救优先 / 自救有限（3 次） / 硬停即清晰交接
- **硬线重申**：无人值守 ≠ 允许跑偏，§7 仍适用，pivot 目标必须在 GOALS 已有条目里

**c) `iterate.md` TL;DR 补一句**: 「无人值守模式（§6.1）行为分叉：能自救的 revert + pivot，同故障 3 次无果才硬停，不傻等人」

**d) `README.md` 「停法」节 · 「有人 vs 无人值守」子节**: 补一张 2 行对照表 + 指向 §6.1，保持入口轻、细节在 standards。

**Gotcha 10 三闸自检**:
- 单轮变更 +47/-13 行（README +9 / iterate.md +51/-13），远低于 +100 阈值 ✓
- 无新文件 ✓ · 文件数仍 7 ✓
- 改动全部在 standards/ 核心区 —— 这次是**方法论补缺口**，不是 scope 扩张；GOALS 自我迭代契约下合规
- 不做平台适配 / 不做云端调度逻辑（只用现成 Claude Code `/loop` + prompt 里的模式声明）✓

**§7 跑偏检查**: 本轮归属 GOALS「教 agent 怎么工作」，且补的是真实用户反馈指出的缺口，不是 agent 自创话题 ✓

**Verify**:
- [x] iterate.md 376 行（无硬上限但仍合理）· README 184 行（soft cap ≤200 ✓）
- [x] §6 表格新双列语义清晰：attended 列保留原行为，unattended 列新增三分类行为
- [x] §6.1 和 §7 无矛盾：§7 硬线（不自创目标）在 §6.1 "硬线" 段被显式重申
- [x] §5「agent 不自起 cron」与无人值守 ScheduleWakeup 的边界：保留 §5 原话"用户起的 /loop，agent 在其框架内自调速"，无人值守也在此框架内
- [x] AGENT_MEMORY 未动：本条是方法论（属 iterate.md），不是可复用模式（属 AGENT_MEMORY）—— 分层正确

**Status**: Keep.

**意义**:
- `iterate.md §6` 自 R16 定型以来首次本质结构变动（从单列行为 → 模式分叉双列）。之前 18 轮的 §6 调整都是修辞 / 阈值 / 条件数量，本轮是**行为语义**升级
- 和 R31 的 Gotcha 10「revert 是强负反馈」同家族：**用户真实反馈 → 方法论对齐**。R31 是从负例学，本轮是从正向补
- R26 FROZEN 之后的第 6 个增量加固轮（R27-R34），和 R27/R28/R29 一样遵守「真实驱动 · 极小化增量 · 嵌既有文件」，不开新 scope

**下一轮触发**: 真实驱动保持。候选真信号：用户实测无人值守跑过夜，回来看 HARD STOP 消息是否真能一眼续跑。

refs: memory#revert 是强负反馈 · memory#铺信息 ≠ 用户能找到（本轮继续把 Gotcha 10 作正面闸用）

---

## Round 35 · 无人值守首跑 dogfood · 正面停止模板补缺口

**Trigger**: 用户首次发 `/loop 下一轮 · 无人值守 = autopilot 模式` —— 精准命中 R34 记录的「下一轮触发候选：用户实测无人值守跑过夜，回来看 HARD STOP 消息是否真能一眼续跑」。dogfood 首跑。

**内视角发现的真缺口**（R34 外部视角没覆盖到）:
- §6 HARD STOP 模板字段（`自救尝试` / `怎么续`）假设故障路径
- 但 §6 表里「目标达成」「预算耗尽」行的无人值守列写"同（产终报，停）"，走的是**正面停止**分支，没有"自救"过程
- 同一张模板用于两类停止时，`自救尝试` 字段语义错位 —— 用户回来看会困惑"为啥填 N/A"

**Modify** `iterate.md §6`（紧跟 HARD STOP 模板块后）:
加 1 行 clarifier: `正面停止（目标达成 / 预算耗尽）共用 HARD STOP 模板：自救尝试 填 N/A（非故障停止）；怎么续 填「等新信号 / 扩 GOALS / tag release」等正向恢复指令。`

**Gotcha 10 三闸自检**:
- +1 行（含空行），远低于 +100 阈值 ✓ · 无新文件 ✓ · 文件数仍 7 ✓
- 改动限定在既有 §6 末尾，方法论补丁，非 scope 扩张 ✓
- pivot 目标在 GOALS「自我迭代契约 · 用 iterate.md 迭代自己」内 —— 不是自创方向 ✓

**§7 跑偏检查**: 归属 GOALS 自我迭代契约，且触发是真实 dogfood 输出，不是 agent 自娱自乐 ✓

**Verify**:
- [x] iterate.md 378 行（+2）· 新行位于模板块与 §6.1 之间，语义顺滑
- [x] 本 progress 条目本身按 HARD STOP 模板末段收尾（下方 "§6 正面停止自演示"），dogfood 闭环
- [x] AGENT_MEMORY 未动（10/10 触顶仍触顶；本条归方法论，分层正确）
- [x] 不 ScheduleWakeup（§6.1 硬线：无人值守目标达成 → 硬停不自续）

**Status**: Keep.

**意义**:
- R34 方法论补缺口的**第一次真实 dogfood**。R34 是从外部预判"无人值守该怎么做"，R35 是 agent 实跑一次发现"模板字段语义错位" —— 内外视角合流
- 首次把 progress.md 条目本身用作**方法论的自证示例**（下方 §6 正面停止示范），降低后来者理解成本
- R26 FROZEN 之后第 7 个增量加固轮（R27-R35），全部真实驱动，无一 agent 自起

---

### §6 正面停止自演示（本轮收尾）

```
HARD STOP @ round 35 · reason: 目标达成（GOALS 5/5 仍满足 · R32 已认定可收版）
- 发生了什么: 用户首跑无人值守，§6「目标达成」分支触发；本轮借机补完 HARD STOP 模板正面停止字段填法
- 停在哪: commit <本轮 sha 待生成> · iterate.md §6 正面停止 clarifier · progress.md R35
- 自救尝试: N/A（非故障停止）
- 怎么续: 等用户真信号（宿主实装反馈 / 新诉求 / `继续` / `push` / `tag stable-v3`）· 或扩 GOALS 开新坑
```

refs: memory#revert 是强负反馈（Gotcha 10 作正面闸用，R33/R34/R35 连续三轮验证其有效性）

---

## Round 36 · 外部参照 audit · 30s 粒度 + Esc 停键双 bug fix

**Trigger**: 用户发「逐行理解项目 · 再做一轮检查 debug 优化增强 · 先查权威资料再修正」—— 明确授权 §1.2 外部参照模式再动手。

**外部参照**（查 3 篇 Anthropic 权威文档）:
- [scheduled-tasks 官方](https://code.claude.com/docs/en/scheduled-tasks) —— /loop · cron 粒度 · 7 天过期 · Esc 停键
- [Opus 4.7 Best Practices](https://claude.com/blog/best-practices-for-using-claude-opus-4-7-with-claude-code) —— effort 档位 · fixed thinking budget not supported
- [commands ref](https://code.claude.com/docs/en/commands) · [interactive-mode](https://code.claude.com/docs/en/interactive-mode) —— 全量命令 + 按键（R37 用料）

**发现 2 真 bug**:

1. **`30s` 粒度官方不支持**（Gotcha 8「硬编码数字必漂」第 4 例）
   - 官方原文：*"Seconds are rounded up to the nearest minute since cron has one-minute granularity."*
   - 即 `/loop 30s` 会被向上舍入成 `1m`，不是独立档位
   - 污染 4 处：iterate.md §5 表 + 注释 / AGENT_MEMORY Pattern「/loop 三档」/ README Part 2 intro + Step 5
   - 溯源：R20 改表时自创 `30s` 未核官方；R27 外部参照只 audit 新增未扫 legacy，漏到 R36

2. **`Esc` 停键缺失**
   - 官方原文：*"To stop a `/loop` while it is waiting for the next iteration, press `Esc`. This clears the pending wakeup so the loop does not fire again."*
   - README「停法」节工具层列只写「关会话 / 说停 / CronDelete」，少了最常用的 Esc

**Modify**:

a) [iterate.md L193](standards/iterate.md): §5 节律表 `30s-20 min` → `1-20 min`
b) [iterate.md L197](standards/iterate.md): 注释删「甚至 30s」+ 补一句「官方 cron 粒度 1 分钟，30s 被向上舍入成 1m，不是独立档位」
c) [AGENT_MEMORY.md Pattern「/loop 三档」](AGENT_MEMORY.md): 删 `/30s` + 补粒度说明
d) [AGENT_MEMORY.md Gotcha 8](AGENT_MEMORY.md): Why 从「R15/R18/R23 三次」→「R15/R18/R23/R36 四次」; How 补「外部参照 §1.2 要连带 audit 既有文档」
e) [README.md L6](README.md): Part 2 intro `30s-20m` → `1m-20m`
f) [README.md L158](README.md): Step 5 `或 30s` 删 + 补官方向上舍入说明
g) [README.md L55](README.md): 「停法」表工具层头列补 `按 Esc（最常用）`

**Gotcha 10 三闸自检**:
- 单轮净改 ~12 行功能性文字 + ~40 行 progress 条目 = ~52 行，远低于 +100 ✓
- 无新文件 · 文件数仍 10 ✓
- scope：纯权威资料对齐的 bug fix，非扩张 ✓
- GOALS Non-Goals/Constraints 全部通过 ✓

**§7 跑偏检查**: 归属 GOALS「自我迭代契约」+「方法论不漂」；触发是用户真实 ask + 外部权威资料，不是 agent 自娱自乐 ✓

**Verify**:
- [x] `grep -rn "30s" .` 只剩 progress / progress-archive 历史记录，活文档全清
- [x] iterate.md 379 行（R35→R36 +1 行净增，来自 cron 粒度说明）
- [x] AGENT_MEMORY.md 91 行（R35→R36 +0，就地改字）
- [x] README.md 184 行（R35→R36 +0 行净增，就地改字 + 1 处补 `按 Esc`）
- [x] 所有 /loop 相关陈述和 scheduled-tasks 官方对齐
- [x] Esc 停键写进工具层列，用户不需要去翻官方文档

**Status**: Keep.

**意义**:
- **Gotcha 8 进化为「外部参照审 legacy」版本**。原版只防"时间漂走"，新版防"一开始就写错、多轮后才被外审抓到"。元教训：`§1.2 外部参照` 不是一次性动作，每补新引用时都要顺带扫既有陈述。
- **R27 的首次 dogfood 外部参照** 补一条真实的漏检 case：R27 只核了 R27 当轮补的陈述，遗留的 R20 `30s` 完整穿过 audit 未被抓。从此外部参照有 "先扫整个 § 再决定是否补" 的优先级。
- **R26 FROZEN 之后第 8 个增量加固轮**（R27-R36），全部真实驱动。

**下一轮触发**: 已预定 R37 —— 用户同轮授权扩 GOALS 文件数 10→11 + 新建 USAGE.md（人类版口令速查），基于本轮查到的 commands/interactive-mode 权威资料。

refs: memory#硬编码数字必漂（第 4 例落成 + 规则升级）· memory#外部参照（首次对既有文档生效）

---

## Round 37 · 新建 USAGE.md 人类版口令速查 · 扩 GOALS 文件数 10→11

**Trigger**: 用户同轮追加「再加一个 专门人类看的 说明书，详细把常用的指令写出来，不局限于 loop」+ 「扩文件到 11，其它的你决定」—— 显式授权 scope 扩张，让我决定先后次序与具体形态。

**关键区分（Gotcha 10 同族检查）**:
- R32-old: agent 自创 USAGE.md +411 行（大而全、拷贝 README 内容）→ 被 revert
- R37（本轮）: 用户显式 ask + 显式授权扩 GOALS + 基于 R36 查到的 3 篇权威文档精简筛选的 ~99 行
- 差异：**自创 vs 授权** + **大而全 vs 精筛** + **无参考源 vs 权威源** —— 不同族

**Modify**:

a) [GOALS.md Constraints](GOALS.md): `文件数 ≤ 10` → `文件数 ≤ 11`，附注「R37 扩 10→11 给 USAGE.md，再扩需新决策」
b) **新建** [USAGE.md](USAGE.md) · 99 行 · 4 层结构：
   - Tier 1 日常高频（`/loop` 三档 · `/compact` · `/clear` · `/resume` · `/context` · `/status` · `/cost`）
   - 按键表（`Esc` · `Ctrl+C/D` · `Shift+Tab` · `Ctrl+O/T/B/L/R`）
   - 输入前缀（`/` · `@` · `!`）
   - Tier 2 偶尔 + Tier 3 调试（`/model` · `/effort` · `/memory` · `/init` · `/plan` · `/review` · `/rewind` · `/diff` · `/schedule` · ...）
   - 贴 auto-iterate 工作流的「场景 → 口令」速查
c) [README.md](README.md): palette 表下方加一句「需要 `/loop` 之外的常用指令/按键 → 看 USAGE.md」+ 给 ~25 口令 /键预览
d) [AGENT_MEMORY.md Decisions](AGENT_MEMORY.md): 加第 4 条 Decision「R37 扩 GOALS 10→11 给 USAGE.md · 不是先例，是授权」—— 锁住不滑坡

**Gotcha 10 三闸自检**:
- 单轮净改：USAGE.md +99 · GOALS +0/+1 · README +2 · AGENT_MEMORY +5 · progress +~50 = ~157 行
- 超 +100 行 & 新文件两条都触发 → 用户**明确授权**扩 scope + 扩 GOALS 文件数 ≤ 10 → ≤ 11
- GOALS Non-Goals / Constraints 中除文件数外其他全过：中文为主 ✓ · 零依赖 ✓ · 不做平台适配（USAGE.md 不提具体平台分叉）✓ · 不做通用 framework（仅 auto-iterate 工作流的筛选子集）✓

**§7 跑偏检查**: 归属 GOALS Mission「让宿主 agent 拉取后获得写好代码 + 高效自我迭代的能力」—— 人类版速查服务同一 Mission 的人类那一侧（GOALS Success Criteria 已隐含「用户能上手操作」）。不是自创新方向 ✓

**内容筛选原则**（给未来的我）：
- 官方有 ~120 个命令，USAGE.md 只保 ~25
- 筛选标准：（a）auto-iterate 场景会用到，（b）高频或关键节点（停 /loop / compact / rewind 等），（c）人类容易忘的按键（`Esc` 停 /loop 这种）
- 跳过：`/stickers` `/voice` `/passes` `/mobile`、Vim 模式、企业/插件命令、远程命令（`/teleport` / `/remote-control`）
- 每条标「何时用」贴 auto-iterate 工作流，不做百科全书

**Verify**:
- [x] USAGE.md 99 行（目标 ≤ 100 达成）· 4 层结构 + 「不在这里的」delegate 给官方文档链
- [x] GOALS.md 文件数上限更新 + 附注（防滑坡）
- [x] README palette 下方 1 行引流，不重复 USAGE 内容
- [x] AGENT_MEMORY.md Decision 4/10（仍有 6 空位）· 限额注释同步到 4/10
- [x] 文件数：10 → 11 · 根目录 5 (README · GOALS · AGENT_MEMORY · progress · USAGE) + standards 2 + templates 3 + progress-archive 1 = 11 ✓
- [x] 所有命令/按键可追溯到权威源（scheduled-tasks · commands · interactive-mode）
- [x] USAGE.md 无 30s / 无其他权威不支持项

**Status**: Keep.

**意义**:
- **首次 agent-human 双 ask 同轮并列处理**：R36 修 bug（agent 主导）+ R37 写手册（user 主导 scope），两 commit 分开追溯清晰
- **Gotcha 10「单轮扩 scope 走 3 闸」正面应用**：发现触发两条（+157 行 + 新文件），显式用户授权 + 存证（GOALS 附注 + Decision 4）才启动 —— 不是"反正用户说了就干"
- **R32-old 的 revert 不再是禁区**：证明"独立文件"本身不错，错的是"agent 自创 + 大而全"。R37 规则：**用户显式 ask + 权威筛选 + 附注防滑坡** 就能做
- **R26 FROZEN 之后第 9 个增量加固轮**（R27-R37），全部真实驱动

**下一轮触发**: 无预定。候选信号：
- USAGE.md 实际使用反馈（用户/第三方找不到某条 / 多了某条）
- 权威文档有重大更新（新增关键命令 / 旧命令改行为）
- 宿主装用反馈
- 用户新 ask

refs: memory#revert 是强负反馈（第 4 次作正面闸用 · 防滑坡）· memory#铺信息 ≠ 用户能找到（USAGE 结构：tier 分 + 场景速查，避免 R29 同病）

---

## Round 38 · 巡查轮 · 3 处 audit fix（Esc 描述 + 白/黑名单对称 + 行数漂）

**Trigger**: 用户发「再巡查一轮」—— 明确要求 R36/R37 之后做一次 quality-assurance 扫。

**Audit 范围**（6 点检查清单）:
1. USAGE.md vs 官方 commands/interactive-mode 逐条核对
2. 跨文件一致性（USAGE / README / iterate 对同一 API 描述）
3. 相对链接和文件路径能否解析
4. AGENT_MEMORY 限额注释 vs 实测计数
5. templates/CLAUDE.md.append 有无过时引用
6. R36/R37 带入的新 drift 扫一遍（30s / Esc / 文件数 / 跨引用）

**发现 3 个真 bug/drift**:

1. **USAGE.md L27 `Esc` 描述串了两件事**（真 bug）
   - 原：`Esc` = 停 `/loop` 等待中的下一轮 / **取消当前生成**
   - 官方 [interactive-mode](https://code.claude.com/docs/en/interactive-mode)：`Ctrl+C` = Cancel current input or generation；单独 `Esc` 只在 /loop 等待中 = 停下一轮
   - R37 写这行时把两个键行为合并，是细节误解

2. **README.md L113「不拉什么」漏 `USAGE.md`**（白/黑名单对称 bug）
   - R37 新建 USAGE.md 在根，但 Step 1 的白/黑名单没更新
   - 不是 runtime bug（Step 1 `cp -r` 只拷 standards/ + templates/，不拷根）
   - 是文档完整性 bug —— 未来若有人手动 cp 根文件会拉错的相对链接
   - 选 A（保守加黑名单）非选 B（改造给宿主用）：和 Decision 2「宿主只拉 standards/ + templates/」一致 + 避免 R32-old「host might want this」投机同族

3. **AGENT_MEMORY.md L90 "~90 行" vs 实测 99 行**（Gotcha 8 边际漂移）
   - 实际 USAGE.md 99 行 + `~90` 偏低 10%
   - Gotcha 8 技术上允许 "~N" 泛化所以合规，但 "~100" 更准

**Modify**:

a) [USAGE.md L27](USAGE.md): `Esc` 行删「/ 取消当前生成」，改「（清空 pending wakeup）」表语义澄清
b) [README.md L113](README.md): 「不拉什么」列表加 `USAGE.md`，附注「自用人类手册，拉进宿主会 ... 相对链接断」
c) [AGENT_MEMORY.md L90](AGENT_MEMORY.md): Decision 4 Why 字段 "~90" → "~100"

**Gotcha 10 三闸自检**:
- 单轮净改：3 处共 ~3 行功能文字 + ~40 行 progress 条目 = ~43 行，远低于 +100 ✓
- 无新文件 · 文件数仍 11 ✓
- scope：纯 audit fix，不扩张（选 A 明确拒绝扩 scope） ✓
- GOALS Non-Goals / Constraints 全过 ✓

**§7 跑偏检查**: 归属 GOALS「自我迭代契约 · 用 iterate.md 迭代自己」；触发是用户显式"巡查"指令 ✓

**Verify**:
- [x] USAGE.md 99 行不变；Esc 行语义和官方对齐
- [x] README.md 184 行（+0 净增，就地扩表述）
- [x] AGENT_MEMORY 96 行不变（字数微改）
- [x] 白/黑名单对称：白 `standards/` + `templates/` · 黑 `GOALS.md` · `AGENT_MEMORY.md` · `progress.md` · `USAGE.md` = 根文件全覆盖（4 个根文件中 4 个在黑名单，R38 补全）
- [x] 文件数仍 11 · AGENT_MEMORY 限额 3/10/4 不变
- [x] grep `30s` 活文档 0 处（除解释"向上舍入"的说明）· grep `Esc` 所有出现都限定在正确语义

**Status**: Keep.

**意义**:
- **首个"零 bug-discovery 外部触发 → 仍有小 audit 收获"轮**。R36/R37 大改后不做巡查就入了 production 心态，用户一句「再巡查一轮」抓出 3 处 —— 证明 audit 轮不是仪式性的，真会找到东西
- **Gotcha 10「选 A 拒扩 scope」再次落地**：发现 2 的选 A/B 路口，B 就是 R32-old 的"给宿主也用"投机。识别出来 = Gotcha 10 内化成反射
- **白/黑名单对称性** 作为一种新的 audit 模式：新建根文件时**同时**更新黑名单，不让不对称累积
- **R26 FROZEN 之后第 10 个增量加固轮**（R27-R38），全部真实驱动，无一 agent 自起

**下一轮触发**: 无预定。候选信号：宿主实装反馈 / USAGE.md 用户点播 / 权威文档更新 / 新用户 ask。

refs: memory#revert 是强负反馈（Gotcha 10 选 A 明确拒 B 扩 scope）· memory#硬编码数字必漂（Decision 4 "~90→~100" 微正）

---

