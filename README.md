# auto-iterate

给 Claude Code agent 的最小「方向锚定 + 代码写作守则」。

不是 framework、不是协议、不管节拍、不管循环 —— **只锁一件事：agent 不自创方向**。

- [`standards.md`](standards.md) · Karpathy 4 原则 + Opus 4.7 契约 + 唯一硬线「找不到 GOALS 归属就停」
- [`templates/GOALS.md.template`](templates/GOALS.md.template) · 由人写，agent 不替人决定

---

## 装到宿主项目

```bash
git clone --depth 1 https://github.com/Sun2080/auto-iterate /tmp/auto-iterate

mkdir -p .claude
cp /tmp/auto-iterate/standards.md .claude/standards.md
cp -r /tmp/auto-iterate/templates .claude/templates

rm -rf /tmp/auto-iterate
```

非 Unix shell：agent 自己换等价命令，本项目不做平台适配。

---

## 挂接 CLAUDE.md

把 [`templates/CLAUDE.md.append`](templates/CLAUDE.md.append) 的内容追加到宿主 `CLAUDE.md`。

---

## 人类要做的唯一一件事 —— 写 GOALS.md

复制 `.claude/templates/GOALS.md.template` 为宿主根的 `GOALS.md`，填：

- **Mission** —— 一句话
- **Success Criteria** —— 可打勾、带「怎么验证」
- **Non-Goals** —— 明说不要做的
- **Constraints** —— 技术/风格/流程约束

**GOALS.md 没填完，agent 不开始工作。**

---

## 怎么跑

装完 + GOALS.md 填完后，agent 看 `standards.md` 就懂。你按自己习惯让 agent 工作即可：

- 手动：说「做 X」/「做下一件事」
- 连续：用 Claude Code 原生的 `/loop <prompt>` —— 不带时间参数是动态节拍（跑完立即起），带 `Xm` 是固定周期。详见 [官方 scheduled-tasks 文档](https://code.claude.com/docs/en/scheduled-tasks.md)

**agent 每次动手前过 `standards.md` §C**：动作能在 GOALS.md 找到归属吗？找不到 → 停下问人。

---

## 这个项目不做的事

- 不强制 `progress.md` / `AGENT_MEMORY.md` / 任何记忆文件 —— 4.7 够聪明，对话上下文 + git history 够用
- 不规定循环节拍（Modify → Commit → Verify）—— 让 agent 自己判断
- 不规定 commit 时机、不强制「先 commit 再 verify」
- 不做 N 轮小回 / M 轮大回 / 停止条件 6 维表
- 不做值守模式分叉（有人 / 无人）
- 不做外部参照协议
- 不做 `/loop` 口令速查 —— 官方文档有

如果你需要上述能力，请用更重的 framework，本项目不是它。

---

## 来源

- [Andrej Karpathy](https://karpathy.ai/) 对 LLM 编码陷阱的观察（via [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills)）
- [Anthropic · Claude Opus 4.7 Best Practices](https://claude.com/blog/best-practices-for-using-claude-opus-4-7-with-claude-code)
