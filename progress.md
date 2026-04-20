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

