# Goals — auto-iterate

> **Status**: Agent 从 2026-04-21 对话中抽取草案；经多轮实跑验证基本准确（本项目自身达成所有 Success Criteria）。正式人审待用户主动触发修订。

## Mission

轻量、可拉取的指导包。宿主项目（如量化交易系统）的 Claude Code agent 拉取本仓库后，获得「写好代码 + 高效自我迭代」的能力。

本项目**只管方法论，不管业务目标**。

## Success Criteria

宿主 agent 拉取本仓库后：

- [ ] 读 `README.md` 即可知道下一步（≤ 2 分钟）
- [ ] 能把 `standards/` 关联到宿主项目的 CLAUDE.md
- [ ] 能按 `standards/iterate.md` 跑起 `Modify → Commit → Verify → Keep/Revert` 循环
- [ ] 能维护 `AGENT_MEMORY.md` 不膨胀（≤ 200 行，35 条上限）
- [ ] 遇到停止条件会停下问人，而非无限重试

## Non-Goals

- 不管宿主项目的业务目标（由宿主 `GOALS.md` 提供）
- 不管宿主项目的安全边界（分支策略、审批流、PR 规则）
- 不提供模型 / API / 基础设施
- 不做通用 agent framework —— 只做「教 agent 怎么工作」的薄包
- 不做 Windows/Mac/Linux 适配逻辑 —— 全部相对路径 + 让宿主 agent 自己解决环境

## Constraints

- **仓库小**：文件数 ≤ 11，根目录平铺 + `standards/` 一层子目录（R37 扩 10→11 给 USAGE.md 人类版速查；再扩需新决策）
- **升级**：宿主重拉即可，不做向后兼容、不做版本号 SemVer
- **语言**：中文为主（用户母语）；关键术语保留英文
- **依赖**：零运行时依赖。只是 Markdown 指导文档

## 自我迭代契约

本项目也用自己的 `iterate.md` 迭代自己。`AGENT_MEMORY.md`、`progress.md` 是本项目的迭代记忆，**不要被宿主项目拉走**（README 会说明这点）。
