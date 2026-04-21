# auto-iterate

给 Claude Code agent 的最小「方向锚定 + 代码写作守则」。

两种模式:

- **默认 · 有人值守** —— [`standards.md`](standards.md):Karpathy 4 原则 + Opus 4.7 契约 + 硬线「找不到 GOALS 归属就停下问人」
- **Autopilot · 无人参与** —— [`autopilot.md`](autopilot.md):用户只给 Mission + 上限,agent 自研究 / 自规划 / 自实现 / 自停 / 写 HANDOFF

配套:[`templates/GOALS.md.template`](templates/GOALS.md.template) · [`templates/CLAUDE.md.append`](templates/CLAUDE.md.append)

---

## 装到宿主项目

```bash
git clone --depth 1 https://github.com/Sun2080/auto-iterate /tmp/auto-iterate

mkdir -p .claude
cp /tmp/auto-iterate/standards.md .claude/standards.md
cp /tmp/auto-iterate/autopilot.md .claude/autopilot.md
cp -r /tmp/auto-iterate/templates .claude/templates

rm -rf /tmp/auto-iterate
```

非 Unix shell:agent 自己换等价命令,本项目不做平台适配。

---

## 挂接 CLAUDE.md

把 [`templates/CLAUDE.md.append`](templates/CLAUDE.md.append) 的内容追加到宿主 `CLAUDE.md`。里面含两种模式的入口规则。

---

## 模式 A · 有人值守(默认)

### 人类做的唯一一件事 —— 写 GOALS.md

复制 `.claude/templates/GOALS.md.template` 为宿主根的 `GOALS.md`,填齐:

- **Mission** —— 一句话
- **Success Criteria** —— 可打勾、带「怎么验证」
- **Non-Goals** —— 明说不要做的
- **Constraints** —— 技术/风格/流程约束

**GOALS.md 没填完,agent 不开始工作。**

### 怎么跑

- 手动:说「做 X」/「做下一件事」
- 连续:用 Claude Code 原生的 `/loop <prompt>` —— 不带时间参数是动态节拍(跑完立即起),带 `Xm` 是固定周期。详见 [官方 scheduled-tasks 文档](https://code.claude.com/docs/en/scheduled-tasks.md)

Agent 每次动手前过 `standards.md §C`:动作能在 GOALS.md 找到归属吗?找不到 → 停下问人。

---

## 模式 B · Autopilot(无人参与)

### 启动口令

消息以 `自主:` 开头即进入:

```
自主:做黄金交易辅助系统,限 4h
自主:把登录模块重构到解耦状态,限 50 commit
自主跑:给博客做一个暗色主题,限 20 commit
```

**必给 Mission + 上限**(时间 / commit / 预算任一)。缺就不启动,agent 会问你补。

### Agent 会做什么

1. **Bootstrap** —— 外部参照研究,自填 `GOALS.md`(Mission 是你给的,Criteria / Non-Goals / Constraints 它基于研究写)
2. **循环** —— Modify → Commit → Verify → Keep/Revert,每轮 progress.md 追加,有发现追加到 `## Discoveries`
3. **自救** —— 同一错 3 次就 pivot,卡壳就换候选,不无限试
4. **Mission 对齐自检** —— 每 5 commit 自评对齐度,偏离就硬停
5. **硬停 + HANDOFF** —— 上限到 / 达成 / 穷举 / 冲突 即停,写交接到 progress.md 尾

### 你回来要做的事

看 `progress.md` 尾的 HANDOFF 段:做到哪、达成度、关键 Discoveries、未做项、续跑指令。

续跑就是:`自主:继续上次,限 <新上限>`

### 硬线(autopilot.md 的三条)

- **Mission 是硬线** —— agent 不动 Mission,偏了就停
- **上限是硬线** —— 不给不启动,给了不超
- **硬停不续命** —— 不自调 cron / ScheduleWakeup,停就是停

---

## 这个项目不做的事

- 不规定循环节拍 / 6-30 回 / 停止条件 6 维表
- 不强制 `AGENT_MEMORY.md` / 反膨胀协议
- 不做 `/loop` 口令速查 —— 官方文档有
- 不做平台适配(Windows / Unix shell 差异)

需要这些能力 → 更重的 framework,本项目不是它。

---

## 来源

- [Andrej Karpathy](https://karpathy.ai/) 对 LLM 编码陷阱的观察(via [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills))
- [Anthropic · Claude Opus 4.7 Best Practices](https://claude.com/blog/best-practices-for-using-claude-opus-4-7-with-claude-code)
- AutoResearch 模式(autopilot 循环结构参考)
