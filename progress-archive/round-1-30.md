# Progress Archive · Round 1-30

> R30 大回归档 · R1-R30 压缩摘要。主 `progress.md` 保留完整逐轮记录（见本文件平级目录）；本文件是给后来者/新宿主用的**导览快照**。

---

## 骨架（R1-R8）· 从 0 到两份标准

| 轮 | 动作 |
|---|---|
| R1 | Bootstrap：`GOALS.md` · `README.md` · `AGENTS.md`（后改名） · `progress.md` 四文件骨架 |
| R2 | `templates/GOALS.md.template` + `CLAUDE.md.append` |
| R3 | `iterate.md §1.1` Bootstrap 例外条款 |
| R4-R7 | `standards/code.md` Karpathy 4 原则 + 4.7 契约；§2-§11 打磨 |
| R8 | `AGENTS.md` → `AGENT_MEMORY.md` 改名（避社区命名冲突）+ `templates/AGENT_MEMORY.md.template` |

---

## 外部参照 + audit 循环（R9-R16）· 闭门造车的代价

| 轮 | 动作 |
|---|---|
| R9 | **首次外部参照**（`iterate.md §1.2` 硬线的由来）：查到 Anthropic 官方 `~/.claude/agent-memory/<agent>/MEMORY.md` 首 200 行加载机制，和我们 §4 200 行硬上限撞车 → 强信号 |
| R10 | `iterate.md` TL;DR + `/compact` 节律 省 token |
| R11 | §2.1 口语短指令回显规则 |
| R12 | audit pass 1：prune bug + Context7/Playwright/cache/schedule 暴露 |
| R13 | audit pass：README/count/cross-refs 优化 |
| R14 | audit pass 2：3 真 bug + 2 小优化（含 AGENT_MEMORY 改名残留 2 处）|
| R15 | audit pass 3：功能缺口 + 双向交叉引用 + polish |
| R16 | audit pass 4：0 bug / 0 gap / 2 drift · convergence 信号 |

---

## STABLE 三进三退（R17-R26）· 收版的艰难

| 轮 | 动作 | 结局 |
|---|---|---|
| R17 | audit pass 5：path prefix polish · 宣 STABLE v1 | 被 R18 打脸 |
| R18 | fix §4.3 数学 bug：35×3=105 不是 150 · STABLE v1 撤回 | — |
| R19 | feat：快节拍支持 · `/loop 1m` 档位 + cache 命中说明 | — |
| R20 | audit pass 6：R19 downstream 一致性修复 | — |
| R21-R23 | 三连快跑：§5 表格 `30s-20min` · TL;DR vs §6 对齐 · 最终 pass → 宣 STABLE v2 | 被 R24 抓到 R23 自产漂移 |
| R24 | 压力测试抓 R23 元 bug · +1 Gotcha（「改名类改动必须 grep 全仓」）· STABLE v2 撤回 | — |
| R25 | README 说明书对齐 STABLE v2 · 4 处下游补齐 | — |
| R26 | **收版轮 · 真·零发现 · FROZEN**（用户明确终止条件 + 实测零） | **STABLE v3** |

---

## 收版后的真实驱动（R27-R29）· STABLE 的可复活性

| 轮 | 触发 | 动作 |
|---|---|---|
| R27 | 用户问「`/loop 1m` 会不会堆积」 | 查 scheduled-tasks 官方 → 确认串行等待 + 跳过补偿；补动态 `/loop`（无参数）档位；iterate.md §5 + AGENT_MEMORY Pattern + README palette 三处对齐 |
| R28 | 用户：「更新说明书」 | README Part 2 intro · Step 4 · Step 5 重排，三档（手动/动态/固定）+ 动态升为推荐首选 |
| R29 | 用户：「我要无间隔连续跑 · 你没在说明书写」 | 虽然 R27+R28 铺了 6 处，埋在表格/段落找不到 → README 首屏加 🔥 零前提速答块 · 独立代码块 `/loop 下一轮` |

---

## R30 · Big-Review · 战报

### 基线 vs 现状

| 维度 | R1 | R30 |
|---|---|---|
| 文件数 | 4 | 10（根 4 + standards 2 + templates 3 + progress-archive/ 1） |
| `standards/` | 无 | `code.md` · `iterate.md` |
| `templates/` | 无 | 3 份（GOALS · CLAUDE.md.append · AGENT_MEMORY） |
| AGENT_MEMORY 条目 | 0 | Patterns 3 · Gotchas 9 · Decisions 3 |
| STABLE 宣告次数 | 0 | 3（v1 被打脸 · v2 被打脸 · v3 FROZEN） |
| 累计 revert | 0 | 0（30 轮全 Keep） |
| 用户未解投诉 | — | 0 |

### 关键决策（全部沉淀在 AGENT_MEMORY Decisions）

1. **auto-iterate 既是工具也是被迭代对象**（R1）· 本项目 dogfooding 自己的 iterate.md
2. **宿主只拉 `standards/` + `templates/`，不拉根 meta 文件**（R1）· 避免污染
3. **不做 `/iterate` skill**（R1）· Claude Code 原生已有 `/loop`、`CronCreate`、`ScheduleWakeup`

### 未决问题 / 下一阶段触发条件

- **真实宿主装用验证尚缺**：30 轮全是自 dogfooding；真·第三方装用还未发生
- **动态 `/loop` 实战体感**：R27 技术事实已经铺好，真实工作流反馈未到
- **GOALS「遇停止条件会停下问人」无真实触发场景**：本项目 30 轮全 Keep，停止分支未真跑过
- **触发条件**（任一满足才开新轮）：真实宿主问题 / 新功能诉求 / 第三方反馈 / PR

### 反模式复盘（三次打脸的元教训）

- **R17/R23 自宣 STABLE 都被打脸**：自己审自己是结构性盲点；只有用户明确终止条件（R26）+ 真实问题驱动（R27+）才是收敛信号
- **R29 用户找不到 `/loop 下一轮`**：铺 6 处 ≠ 入口清晰；新增 Gotcha 固化
- **三次硬编码数字漂移（R15/R18/R23）**：R23 明知教训还犯 → Gotcha「硬编码数字必漂」active defense 才在 R27 生效

### 归档决定

- **本文件 (`round-1-30.md`)**：导览快照，新读者 5min 可扫完 30 轮脉络
- **主 `progress.md`**：保留 R1-R30 完整逐轮记录不截断（977 行，未超负荷）· 不做主文件裁剪
- 下次大回（R60）若主 progress 过长再做裁剪，届时重读本文件的写作模板

### STOP 状态

按 `iterate.md §5.2` 大回终点：**不开下一轮**。等用户说「继续」或新触发信号（§7）。
