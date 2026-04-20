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



