# auto-iterate

**Claude Code 的 AI 基座**。新项目一句话装完,老项目一句话清旧重建。

> 📖 装完怎么用 → [USAGE.md](USAGE.md)(两种模式 · 启动口令 · 常见问题)

---

## 装(一句话给 agent)

**新项目**:
```
装 https://github.com/Sun2080/auto-iterate 作为本项目 AI 基座
```

**老项目**(已有其他 AI 规则 / 旧 iterate 协议需清掉):
```
装 https://github.com/Sun2080/auto-iterate · 清旧重建 AI 基座
```

Agent 按下方 [给 agent · 安装协议](#给-agent--安装协议) 自动执行:拉 repo → 清旧(仅老项目)→ 拷文件 → 追加 CLAUDE.md → 自报装完。

装完后 → 看 [USAGE.md](USAGE.md)。

---

## 给 agent · 安装协议

用户说"装 https://github.com/Sun2080/auto-iterate ..."时,按本协议执行。

### Step 1 · 拉仓库到临时目录

```bash
# Unix / git-bash
git clone --depth 1 https://github.com/Sun2080/auto-iterate /tmp/auto-iterate

# Windows 原生 PowerShell (示例)
git clone --depth 1 https://github.com/Sun2080/auto-iterate $env:TEMP\auto-iterate
```

Agent 按当前 shell 选命令,本项目不做平台适配。

### Step 2 · (仅老项目)清旧 AI 基座

**触发**: 用户说"清旧重建"/"清掉旧基座"/"重建"/ 老项目改造。

**扫这些候选**(在宿主项目根):

| 文件 / 目录 | 含义 | 默认动作 |
|---|---|---|
| `CLAUDE.md` 里"agent 规则 / 迭代协议 / 写代码守则"段 | Claude Code 自建规则 | **列出来问用户 y/n**,不自删 |
| `AGENT_MEMORY.md` / `AGENT_RULES.md` / `AGENTS.md` | 宿主自建的 agent 规则 | 列出来问 y/n |
| `ITERATION*.md` / `ITER_RULES.md` / `HANDOFF.md`(若是迭代日志性质) | 旧迭代协议 | 列出来问 y/n |
| `.claude/standards/` / `.claude/skills/` / `.claude/templates/` | 旧安装残余 | 列出来问 y/n |
| `progress.md` / `progress-archive/`(若是 agent 轨迹) | 旧 agent 日志 | 列出来问 y/n |

**绝对不动**(不扫、不问、不删):

- `.cursor/` / `.cursorrules` / `.windsurfrules` / `.aider.conf.yml` / `.github/copilot-instructions.md` / `.continue/` / `.codeium/` / `.cline*` / `.roo/` / `GEMINI.md` —— **其他 agent 工具的配置,用户可能还在用**
- 业务代码 / 业务文档(README.md 主体 / docs/ / src/ / tests/ ...)
- `GOALS.md`(已存在时) —— 若用户明说"重写 GOALS 也一起清",才列候选
- git 历史 / `.gitignore` / 依赖锁 / 配置文件

**清理流程**:

1. Agent 扫描,一次性列出所有候选(带文件路径 + 一句话说这是什么)
2. 呈现给用户:「发现以下 N 项疑似旧 AI 基座,建议删(a)/保留(k)/你指定:」
3. 用户确认后 `git rm -r` + commit message:`chore: 清旧 AI 基座,为装 auto-iterate`

**特殊情况** —— `CLAUDE.md` 里混有旧规则段 + 用户要保留的业务约定:
- 不整个删文件
- Agent 只删"规则段",保留业务段
- 改动前让用户预览 diff

### Step 3 · 拷文件

```bash
mkdir -p .claude
cp /tmp/auto-iterate/standards.md .claude/standards.md
cp /tmp/auto-iterate/autopilot.md .claude/autopilot.md
cp -r /tmp/auto-iterate/templates .claude/templates
```

### Step 4 · 追加 CLAUDE.md

把 `.claude/templates/CLAUDE.md.append` 的内容追加到宿主项目根 `CLAUDE.md`(没有就新建)。

**打装完标记**:追加时把模板首行占位替换成真实值:
```
<!-- auto-iterate installed · sha <clone 到的 HEAD sha> · <YYYY-MM-DD> -->
```
下次 agent 扫到这个注释即知"装过了、版本 X",重装时用这个定位老段落便于替换。

### Step 5 · 清临时目录 + 回报

```bash
rm -rf /tmp/auto-iterate
```

回报用户:
```
✅ auto-iterate 装完 (sha <X>)
- 新增: .claude/standards.md · .claude/autopilot.md · .claude/templates/
- 追加: CLAUDE.md(尾部加了 auto-iterate 段 + 装完标记)
- 清旧: <N> 项(如有)
下一步看 USAGE.md。简短版:
- 有人值守 → 填 GOALS.md 后说「做 X」
- Autopilot → `自动迭代:<mission>,限 <上限>`(首启) 或填 GOALS.md 后 `自动迭代 <上限>`
```

### Step 6 · GOALS.md 处理

- **新项目且用户未启动 autopilot** → 复制 `.claude/templates/GOALS.md.template` 为 `GOALS.md`,引导用户填 Mission / Criteria / Non-Goals / Constraints,**不自填**。
- **新项目且用户已启动 autopilot** → 不建 GOALS.md,首轮 bootstrap 时由 autopilot.md §2.2 自建。
- **老项目已有 GOALS.md** → 不动。

---

## 这个项目不做的事

- 不规定循环节拍 / 6-30 回 / 停止条件 6 维表
- 不强制 `AGENT_MEMORY.md` / 反膨胀协议(autopilot 仅用 `progress.md`,有人值守不用)
- 不做 `/loop` 口令速查 —— 官方文档有
- 不做平台适配(Windows / Unix shell 差异,agent 自选命令)
- 不干涉其他 agent 工具(Cursor / Aider / Copilot / Gemini CLI ...)的配置

需要上述能力 → 更重的 framework,本项目不是它。

---

## 来源

- [Andrej Karpathy](https://karpathy.ai/) 对 LLM 编码陷阱的观察(via [forrestchang/andrej-karpathy-skills](https://github.com/forrestchang/andrej-karpathy-skills))
- [Anthropic · Claude Opus 4.7 Best Practices](https://claude.com/blog/best-practices-for-using-claude-opus-4-7-with-claude-code)
- AutoResearch 模式(autopilot 循环结构参考)
