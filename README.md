# AI 编码智能体规范 · 开源项目精选清单

> 从公开研究报告中精选，并对全部仓库做了 GitHub 实时核验（2026-10-18，API/页面实测数据）。
> 用途：为日常开发与项目提供一套"拿来即用"的 AI 编码智能体规范资料源，减少跑偏、误操作与返工。

## 核心使用原则：三层闭环

规则文件解决"项目上下文从哪来"，规格与技能解决"复杂任务如何分步推进"，CI 与策略门禁解决"智能体不能做什么"。三者互补，缺一不可。

**落地顺序**：最小规则文件 + CI 门禁 → 规格驱动 → 技能与 hooks → 记忆与多智能体。复杂度按需递增，小任务不要套完整流程。

## 1. 规范层：规则文件标准与模板

| 仓库 | Stars（实测） | 用法与建议 |
|---|---:|---|
| [agentsmd/agents.md](https://github.com/agentsmd/agents.md) | 25.0k | 规则文件事实标准："给智能体的 README"。写安装/测试命令、架构边界、禁改路径、提交要求；支持子目录就近覆盖。**每个项目从这里开始。** |
| [github/awesome-copilot](https://github.com/github/awesome-copilot) | 40.1k | Copilot 官方社区清单：instructions / agents / skills / hooks / workflows，可信度高 |
| [PatrickJS/awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules) | 40.9k | Cursor 模板合集。挑短模板改写成项目专属版，勿整包导入 |
| [LessUp/awesome-cursorrules-zh](https://github.com/LessUp/awesome-cursorrules-zh) | 239 | 中文 Cursor 规则，适合中文团队起步，需项目化修改 |

规则文件写什么：可执行命令（如 `pnpm --filter @app/api test`）、3–5 条不可协商规则、依赖约束、人工审批边界。不写：项目哲学、通用风格、长篇历史。

## 2. 流程层 A：规格驱动开发（先写规格再写码）

| 仓库 | Stars（实测） | 用法与建议 |
|---|---:|---|
| [github/spec-kit](https://github.com/github/spec-kit) | 141.7k | GitHub 官方。constitution → specify → clarify → plan → tasks → analyze → implement。适合新项目严格门控。官方明确支持 Claude Code、Copilot、Cursor、Codex CLI、Gemini CLI、Cline、Kiro CLI、opencode、Qwen Code、Zed 等集成 |
| [Fission-AI/OpenSpec](https://github.com/Fission-AI/OpenSpec) | 72.3k | 增量修改存量系统首选：spec deltas（ADDED/MODIFIED/REMOVED）→ apply → verify → archive；支持 30+ 工具，跨工具团队适用 |
| [bmad-code-org/BMAD-METHOD](https://github.com/bmad-code-org/BMAD-METHOD) | 54.3k | 完整敏捷生命周期多角色模拟，适合绿地/复杂系统；上下文与 token 成本高，小任务勿用 |

## 3. 流程层 B：技能与工作流

| 仓库 | Stars（实测） | 用法与建议 |
|---|---:|---|
| [anthropics/skills](https://github.com/anthropics/skills) | 181.8k | Anthropic 官方 Agent Skills：规范、模板、示例（文档 / webapp-testing / MCP 等） |
| [anthropics/claude-plugins-official](https://github.com/anthropics/claude-plugins-official) | 41.8k | 官方插件市场：feature-dev、code-review、pr-review-toolkit、security-guidance、hookify |
| [obra/superpowers](https://github.com/obra/superpowers) | 301.3k | 刚性工作流：头脑风暴 → 批准设计 → worktree 隔离 → 计划 → TDD → 双阶段审查；显著降低"直接写码并声称完成"的概率 |
| [addyosmani/agent-skills](https://github.com/addyosmani/agent-skills) | 107.9k | 生产级工程技能包：规划 / 测试 / 调试 / 审查 / 安全 / 发布 |
| [hesreallyhim/awesome-claude-code](https://github.com/hesreallyhim/awesome-claude-code) | 55.9k | Claude Code 综合发现目录：skills / hooks / commands / agents / plugins |
| [VoltAgent/awesome-claude-code-subagents](https://github.com/VoltAgent/awesome-claude-code-subagents) | 25.8k | 100+ 子智能体角色模板；研究类代理默认只读，最小权限 |
| [jnMetaCode/superpowers-zh](https://github.com/jnMetaCode/superpowers-zh) | 8.3k | Superpowers 中文增强版；先当学习源，抽取核心技能改写为内部版并人工审查后再入生产 |

技能用法：描述是路由入口、正文才是行为定义；只安装少数经人工审查的技能；按版本提交、按季度复核。

## 4. 记忆与任务追踪

| 仓库 | Stars（实测） | 用法与建议 |
|---|---:|---|
| [thedotmack/claude-mem](https://github.com/thedotmack/claude-mem) | 101.4k | 跨会话持久记忆（SQLite/FTS5 + Chroma 混合检索）。注意：自动记忆不是事实源，安全/密钥/客户数据上下文默认排除 |
| [eyaltoledano/claude-task-master](https://github.com/eyaltoledano/claude-task-master) | 28.2k | AI 驱动任务管理：PRD → 任务 → 依赖 → 执行，适配 Cursor / Roo / Windsurf 等（补充项，原报告遗漏） |
| Cline memory bank 模式 | — | 见 [cline/cline](https://github.com/cline/cline) 文档：跨工具最普及的项目记忆实践（补充项） |

## 5. 校验层：治理与门禁（确定性约束在模型之外）

| 仓库 | Stars（实测） | 用法与建议 |
|---|---:|---|
| [microsoft/agent-governance-toolkit](https://github.com/microsoft/agent-governance-toolkit) | 6.4k | 策略执行 / 零信任身份 / 沙箱 / SRE / 合规映射，多语言 SDK。Public Preview：采用前验证策略加载与失败默认行为；不能替代 Secret 扫描与权限控制 |
| Claude Code hooks（机制） | — | 官方文档：编辑后自动格式化、提交前 lint、security-guidance 监控文件编辑、hookify 自定义规则 |
| CI 必需状态检查 | — | lint、类型检查、单元测试、构建、依赖审计、Secret 扫描设为必需检查；受保护分支 + CODEOWNERS + 双人审批 |

## 6. 多智能体编排（进阶）

| 仓库 | Stars（实测） | 用法与建议 |
|---|---:|---|
| [ruvnet/ruflo](https://github.com/ruvnet/ruflo)（原 claude-flow） | 76.1k | 多智能体蜂群编排、自适应记忆、向量 RAG（补充项） |

建议：先采用"一个实现代理 + 一个独立审查代理"的双代理结构；并行审查的输入来源必须独立；每个角色最小权限；设置最大重试、超时与人工检查点。

## 慎用 / 不建议直接入生产

| 仓库 | Stars（实测） | 原因 |
|---|---:|---|
| [multica-ai/multica](https://github.com/multica-ai/multica) | 53.1k | 自定义 Multica License；star 增长存在争议 |
| [alirezarezvani/claude-skills](https://github.com/alirezarezvani/claude-skills) | 28.3k | 规模大、质量参差；逐项审查脚本、依赖、网络访问、许可证后才能用 |
| [vijaythecoder/awesome-claude-agents](https://github.com/vijaythecoder/awesome-claude-agents) | 4.4k | 最后提交 2025-10-30（约一年前）；作者标注实验性，高 token 消耗 |
| [travisvn/awesome-claude-skills](https://github.com/travisvn/awesome-claude-skills) | 15.5k | 2026-04 后无显著提交；仅作参考 |
| [Lay4U/awesome-ai-rules](https://github.com/Lay4U/awesome-ai-rules) | 5 | 已停滞 |

## 按场景选型速查

| 场景 | 规范层 | 规格层 | 记忆/编排 | 不建议 |
|---|---|---|---|---|
| 个人小项目 / 原型 | AGENTS.md + 本地 lint/test | 按需 OpenSpec | 不用或轻量 claude-mem | 照搬万字模板 |
| 长期维护单体服务 | CLAUDE.md/AGENTS.md + CI | OpenSpec | claude-mem + 审查代理 | 让记忆覆盖真实规格 |
| 多平台团队 | AGENTS.md 统一层 + 各平台原生指令 | OpenSpec | task-master + 独立评审 | 为每种工具维护冲突规则 |
| 安全敏感项目 | 最小权限 + 必需 CI + 受保护分支 | Spec Kit / OpenSpec | 最小日志 | 默认允许生产命令 |
| 复杂绿地项目 | 规范文件 + 架构决策记录 | Spec Kit 或 BMAD-METHOD | 技术负责人 + 实现者 + 审查者 | 让模型直接设计、实现并部署 |

## 最小可行治理

个人开发者 4 条核心规则：

1. 建立可执行的"安装—测试—验证"命令清单
2. 使用 Git worktree 或分支隔离所有智能体改动
3. 默认只给代理读权限和受限写入权限
4. 每次提交必须附测试或验证证据

团队试点复盘指标：代理合并请求占比、首次 CI 通过率、人工返工时间、误删或权限事故、规则冲突数、token 与费用。

## 核验说明与勘误

- 所有 star 为 2026-10-18 经 GitHub API / 页面实测的近似值，仅作发现信号，不作官方榜单
- 对原研究报告的三处更正：
  1. `anthropics/claude-plugins-official` 实测 41.8k stars（原报告 9.4k 有误）
  2. Spec Kit 官方 integrations 文档明确支持 Codex CLI 等 10+ 集成（原报告"对 Codex CLI 的支持有限"不成立）
  3. 原报告"27 个项目"系跨类别重复计数，去重后为 21 个；本清单另补充 task-master、ruflo、Cline memory bank 三个缺口项
- 任何项目入库前审查：脚本、依赖、网络访问、权限要求、许可证；先在隔离环境试用，再进入生产
