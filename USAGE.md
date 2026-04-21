# USAGE · Claude Code 常用口令速查（人类版）

**给会用 auto-iterate 的人**查常用操作。Agent 侧协议见 [standards/](standards/)。

> 只列**贴 auto-iterate 工作流**的常用命令/按键，约 25 个。全量见官方 [commands](https://code.claude.com/docs/en/commands) · [interactive-mode](https://code.claude.com/docs/en/interactive-mode)。

---

## Tier 1 · 日常高频

| 口令 | 用途 | 何时用 |
|---|---|---|
| `/loop 下一轮` | 动态节拍，跑完立即起下一轮（1m-1h 自选） | 稳定后连续跑的**首选** |
| `/loop Xm 下一轮` | 固定 X 分钟周期（串行等 + 跳补，不并发） | 大任务 5-10m · 短任务 1m |
| `/loop 下一轮 · 无人值守` | 过夜跑模式（§6.1 行为分叉） | 不等人类反馈自救 |
| `/compact [指令]` | 压缩历史，保留最近 + 摘要 | 单轮 >5min 后 cache 失效 · 6 轮小回结束 |
| `/clear` · `/new` · `/reset` | 开新会话（上一会话仍可 `/resume`） | 大回停下后换主题 |
| `/resume [session]` · `/continue` | 恢复既有会话（ID / 名字 / 选单） | 接回昨天的 /loop |
| `/context` | 可视化当前 context 占用 + 建议 | compact 前看一眼 |
| `/status` | 版本 / 模型 / 账号 / 连接 | 调试怪问题起手 |
| `/cost` | 本会话 token 用量 | 预算核算 |

## ⌨ 按键（不需要打字）

| 键 | 动作 | 场景 |
|---|---|---|
| `Esc` | **停 `/loop` 等待中的下一轮**（清空 pending wakeup） | 想立刻停循环但不关会话 |
| `Esc` `Esc`（连按） | Rewind 回到某步 / 抽摘要 | 走错了想回溯 |
| `Ctrl+C` | Cancel 当前输入或生成 | 常规中断 |
| `Ctrl+D` | 退出 Claude Code | 关会话 |
| `Shift+Tab` | 切权限模式（default · acceptEdits · plan · ...） | 进 plan 模式预览 |
| `Ctrl+O` | 切 transcript viewer | 看详细工具调用 |
| `Ctrl+T` | 切 task list 显示 | 看 TodoWrite 进度 |
| `Ctrl+B` | 后台化当前 bash 任务 | 编译/长命令 |
| `Ctrl+L` | 清输入框 + 重画屏幕 | 屏幕糊了 |
| `Ctrl+R` | 反向搜索命令历史 | 找刚才输的长 prompt |

## 📝 输入前缀

| 前缀 | 作用 | 例 |
|---|---|---|
| `/` | 命令 / skill | `/compact` |
| `@` | 文件路径补全 | `@src/main.py` |
| `!` | bash 模式（直接执行，不经 Claude 解释） | `!git status` |

---

## Tier 2 · 偶尔（配置 / 切换）

| 口令 | 用途 |
|---|---|
| `/model [model]` | 切模型（Opus / Sonnet / Haiku） |
| `/effort [level]` | 切 effort 档（low / medium / high / **xhigh** / max；xhigh 是官方推荐默认） |
| `/memory` | 编辑 CLAUDE.md 记忆文件 |
| `/init` | 初始化项目 CLAUDE.md |
| `/plan [描述]` | 进 plan 模式起手 |
| `/review [PR]` | 本地 PR 审查 |
| `/rewind` · `/checkpoint` · `/undo` | 回到某 checkpoint（比 git revert 轻量） |
| `/diff` | 交互式 diff 查看器（上下看变更） |
| `/schedule [描述]` · `/routines` | 云端定时任务（最小 1h，跨会话生存） |
| `/agents` | 管理 subagents |
| `/mcp` | 管理 MCP server |
| `/btw <问>` | 侧问一句不进对话历史（省 context） |

## Tier 3 · 调试 / 配置

| 口令 | 用途 |
|---|---|
| `/config` · `/settings` | 设置面板（主题 / 模型 / output style） |
| `/permissions` | 权限规则（allow / ask / deny） |
| `/hooks` | hook 配置 |
| `/doctor` | 诊断安装 + 配置（按 `f` 让 Claude 修） |
| `/insights` | 近期会话使用分析 |
| `/fast [on\|off]` | 切 fast mode（Opus 4.6 更快） |
| `/recap` | 本会话一句话总结 |

---

## 贴 auto-iterate 工作流

| 场景 | 口令 / 做法 |
|---|---|
| 开始无人值守过夜跑 | `/loop 下一轮 · 无人值守` |
| 单轮跑超 5min | `/compact` 后下一轮靠文件重建上下文（[iterate.md §9](standards/iterate.md)） |
| 6 轮小回结束 | `/compact` + 更新 [AGENT_MEMORY.md](AGENT_MEMORY.md)（[§5.1](standards/iterate.md)） |
| 30 轮大回要过夜续跑 | 用户用 `/schedule` 注册 cron（云端，最小 1h），agent 不自起（[§6.1](standards/iterate.md)） |
| 实验失败要回 | `/rewind` 选回某步，或 `git revert <sha>`（[§2.4](standards/iterate.md)） |
| 看 context 快满 | `/context` 看占用 → `/compact` 压 |
| /loop 跑着不想等了 | **按 `Esc`** 停下一轮（不关会话）· 或关窗 · 或 `CronDelete <id>` |
| /loop 跑完想发 bug 报告 | 在末轮 commit 前说「停」+ agent 追加状态到 progress.md |

---

## 不在这里的

- **/loop 深解 + 停法全维度**：[README.md](README.md) 速查表 + [standards/iterate.md §5/§6/§6.1](standards/iterate.md)
- **装到新项目**：[README.md](README.md) 「消费入口」Step 1-5
- **Vim 编辑模式 / 多行输入 / Background tasks**：官方 [interactive-mode](https://code.claude.com/docs/en/interactive-mode)
- **全量命令（含 `/ultraplan` `/teleport` `/stickers` 等）**：官方 [commands](https://code.claude.com/docs/en/commands)
