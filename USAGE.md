# auto-iterate · 使用说明

装完后怎么用。两种模式,按需选。

> 安装参见 [README.md](README.md)。安装流程由 agent 自动执行。
>
> **Agent 和你对话默认中文**(强制写在 `CLAUDE.md`,代码/注释/commit 仍按项目既有语言)。

---

## 装完后宿主项目长这样

```
.claude/
├── standards.md          # 默认模式规则
├── autopilot.md          # Autopilot 模式协议
└── templates/
    ├── GOALS.md.template
    └── CLAUDE.md.append
CLAUDE.md                 # 追加了 auto-iterate 段 + 装完标记
GOALS.md                  # 有人值守需人填;autopilot 首启会自建
```

5 个 auto-iterate 文件 + 1 个 GOALS.md。没有 `progress.md` / `AGENT_MEMORY.md` 等强制记忆文件。autopilot 模式跑起来时会自建 `progress.md` 记轨迹,非 autopilot 不要求。

---

## 模式 A · 有人值守(默认)

### 启动前一次性做的事 —— 填 GOALS.md

Agent 装完后会复制 `.claude/templates/GOALS.md.template` 为宿主根的 `GOALS.md`,引导你填四段:

| 段 | 写什么 | 示例 |
|---|---|---|
| **Mission** | 一句话,项目要做什么 | 「为 QMT 量化平台提供监控告警子系统」 |
| **Success Criteria** | 可打勾、带「怎么验证」 | `tests/perf.py` 通过,P99 < 200ms |
| **Non-Goals** | 明说不要做的 | 不做通用 alert framework |
| **Constraints** | 技术 / 风格 / 流程 | Python 3.11 · 不引 Django |

**GOALS.md 没填完,agent 不开始工作**。

### 启动方式

- **手动**:说「做 X」/「做下一件事」/「修一下 Y」
- **连续**:用 Claude Code 原生 `/loop <prompt>` —— 不带时间参数是动态节拍(跑完立即起);带 `Xm` 是固定周期。详见 [官方 scheduled-tasks 文档](https://code.claude.com/docs/en/scheduled-tasks.md)

### Agent 的硬线

每次动手前过 `.claude/standards.md §C`:**"这一步能在 GOALS.md 的某条下找到归属吗?"** 找不到 → 停下问人,**不自己扩张方向**。

---

## 模式 B · Autopilot · 无人参与

用户完全不参与 · agent 自研究 / 自规划 / 自实现 / 自验证 / 自停 + 写 HANDOFF 交接。适合过夜跑、你不懂领域让 agent 自探索、做原型。

### 启动口令

**Mission 已在 GOALS.md**(常态,最简):
```
自动迭代 4h
自动迭代 50 commit
自动迭代 到 2026-04-22 03:00
自动迭代 到明晚 22:00
自动迭代 到明天早上 8 点
```

**首启 / 还没 GOALS.md**(inline Mission):
```
自动迭代:做黄金交易辅助系统,限 4h
托管:把登录模块重构到解耦状态,限 50 commit
自主跑:给博客做一个暗色主题,限 20 commit
```

### 触发词(任一即进入 autopilot)

`自动迭代` / `自动跑` / `自主跑` / `自主模式` / `托管` / `autopilot`

等价意图也认:"放手做"、"无人参与"、"不参与"。

### 上限(必给)

| 类型 | 示例 |
|---|---|
| 相对时间 | `4h` / `4小时` / `30m` / `30分钟` / `2d` / `2天` |
| 绝对时间 | `到 2026-04-22 03:00` / `到明晚 22:00` / `到明天 8 点` / `到下午 3 点` |
| commit 数 | `50 commit` / `50 次` / `30 提交` |
| 预算 | `$20` / `20 美元` / `20 美金` |

**缺上限 → 不启动**,agent 会问你补。缺 Mission(档 1 下 GOALS.md 空)→ agent 提示你先填 GOALS.md 或改用首启语法。

### Agent 会做什么

1. **Bootstrap**:外部参照研究 → 自填 `GOALS.md`(Mission 用你给的,Criteria / Non-Goals / Constraints 基于研究写)
2. **循环**:Modify → Commit → Verify → Keep/Revert · `progress.md` 追加 · 发现追加到 `GOALS.md § Discoveries`
3. **自救**:同一错 3 次 pivot · 卡壳换候选 · 穷举无果 → 硬停
4. **Mission 对齐自检**:每 5 commit 自评对齐度,偏离立即硬停
5. **硬停 + HANDOFF**:上限到 / Mission 达成 / 自救穷举 / Mission 冲突 即停,写交接到 `progress.md` 尾

### 你回来要做的事

看 `progress.md` 尾的 HANDOFF 段:

- **停因**(上限到 / 达成 / 穷举 / 冲突)
- **做到哪**(一句话 + commit sha)
- **达成度**(勾了几条 / 总几条)
- **关键 Discoveries**
- **未做项**
- **续跑指令**(一句话,复制即恢复)

续跑:`自动迭代 <新上限>`(Mission 在 GOALS.md 不用重打)。

### Autopilot 硬线

- **Mission 是硬线** —— agent 不动 Mission,偏了就硬停
- **上限是硬线** —— 不给不启动,给了不超
- **硬停不续命** —— 不自调 cron / `ScheduleWakeup`,停就是停,交由你决定

---

## 常见问题

### 想中途停 autopilot?
- 若你在会话里:按 `Esc`(Claude Code 原生,停当前 round)· 或直接说「停」
- 若你关了会话:`/loop` 绑会话,关窗即停
- Agent 自己永远不 `ScheduleWakeup` 续下一轮

### autopilot 跑完了我想继续探索?
HANDOFF 段尾的"续跑指令"直接复制。或:
```
自动迭代 4h
```
Mission 还在 GOALS.md,progress.md 末段有上次轨迹,agent 接着跑。

### Mission 不对想改?
直接编辑 `GOALS.md` 的 Mission 段。再说 `自动迭代 <上限>`,agent 会用新 Mission。

### autopilot 跑偏了没自己停?
不应该发生(§6 Mission 对齐自检)。若发生:按 `Esc` 或说「停」,看 progress.md 末段找偏离点,反馈给 maintainer(这是 auto-iterate 的 bug)。

### 能不能绕过 GOALS.md 直接 autopilot?
用首启语法 `自动迭代:<mission>,限 <上限>`,agent 会给你自建 GOALS.md。

### 宿主项目原来就有的 agent 规则(Cursor / Aider 等)会被清吗?
**不会**。auto-iterate 只管 Claude Code 范围(`CLAUDE.md` · `.claude/`)。其他 agent 工具的配置(`.cursor/` · `.cursorrules` · `.aider.conf.yml` · `.windsurfrules` · `GEMINI.md` · `.github/copilot-instructions.md` 等)**绝对不动**,你继续用。

---

## 不覆盖的场景

- 官方 `/loop` / `/schedule` / `/compact` / `/clear` / `Esc` / `@file` 等 Claude Code 通用口令 → 看 [Claude Code 官方文档](https://code.claude.com/docs/)
- 更复杂的迭代协议(6 轮小回 / 30 轮大回 / AGENT_MEMORY 反膨胀 / 停止条件 6 维表)→ auto-iterate 不做,选更重的 framework
