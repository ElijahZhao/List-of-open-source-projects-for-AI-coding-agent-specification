# 审查工作流工具组合建议（Review Toolkit）

> 配套 [README.md](README.md) 使用。说明在一个"项目审查 / 交付前收尾"工作流中，哪些环节交给成熟开源工具、哪些环节适合用自定义提示词承担、哪里需要两者配合。所有工具均经 2026-10-18 GitHub 实测核验。

## 分工原则

- **确定性检查交给工具**：秘密扫描、依赖审计、版本管理——工具比提示词可靠、可复现、可进 CI
- **语义判据交给提示词**：什么算"过程痕迹"、报告怎么组织、伦理红线怎么划——需要上下文判断，工具做不了
- **观测类用工具，披露类用词表**：AI 使用记录先用观测工具采集，对外声明时参考开放词表与项目规定

## 五层组合

| 层 | 工具承担 | 提示词承担 |
|---|---|---|
| 1. 决策与证据采集 | ADR 模板（[architecture-decision-record](https://github.com/architecture-decision-record/architecture-decision-record)、[MADR](https://github.com/adr/madr)）、实验记录（[DVC](https://github.com/treeverse/dvc) / [Aim](https://github.com/aimhubio/aim)） | 台账字段设计、被否方案的挖掘追问、负结果追问（"失败要带数据存档"） |
| 2. AI 使用观测 | [claude-devtools](https://github.com/matt1398/claude-devtools)（会话 / token）、[ai-credit](https://github.com/debugtheworldbot/ai-credit)（代码贡献归属） | 使用记录模板（每任务一条）、披露声明起草 |
| 3. 脱敏与清理 | [gitleaks](https://github.com/gitleaks/gitleaks) / [trufflehog](https://github.com/trufflesecurity/trufflehog)（秘密扫描）、CI lint（绝对路径 / 环境变量检查） | 环境痕迹的语义识别与改写；"删的是环境、留的是技术事实"判据（失败与异常必须保留） |
| 4. 审查执行 | [claude-plugins-official](https://github.com/anthropics/claude-plugins-official) 的 code-review / pr-review-toolkit | 检查矩阵、覆盖率账本、强制换视角与盲区声明、统一报告模板 |
| 5. 归档与复现 | Git tag / Zenodo DOI（Zenodo 可为 GitHub 仓库签发 DOI）、[showyourwork](https://github.com/showyourwork/showyourwork)（科研场景） | 素材按产出物章节归位、单一事实源一致性核对（README / 文档 / 论文数字对齐） |

## 空白地图（截至 2026-10-18 实测）

以下环节在开源生态**没有可用承接**（概念级项目存在但均 <10★ 或已停更），需要用自定义提示词 / 内部流程承担：

1. **过程产物与展示内容的治理编排**——什么进主仓库、什么存档、怎么中性化改写，无工具可判定
2. **面向通用文本 / 代码的"去 AI 腔"治理**——现状仅有意大利语单语技能 [humanizer-it](https://github.com/ilyautov/humanizer-it)（5★，52 模式 + 15 硬禁令 + 无依赖扫描器的形态可借鉴）
3. **负结果 / 失败尝试的系统化存档**——最近邻为 SRE 复盘模板（[dastergon/postmortem-templates](https://github.com/dastergon/postmortem-templates)，1.5k，2023 年后停更）
4. **"事件发生时间 vs 记录时间"的抢救式素材整理规范**——防止事后追记被虚构成因果链
5. **针对（提示词 / 知识）库本身的元层审查与交叉核实**——轮换视角自审、多 AI 双盲交叉核实、差异仲裁规则、回写落地验证，均无工具承接；最近邻仅为"让另一个代理独立审查代码 / PR"（superpowers 双阶段审查、pr-review-toolkit），对象是代码而非库内容

> 伦理边界：任何"去 AI 腔"类工作只许用于提升表达质量，不得用于规避 AI 检测或隐瞒应当披露的 AI 使用情况——披露义务永远优先于保密偏好。

## 选型速查

| 需求 | 首选 | 备注 |
|---|---|---|
| 重大决策留痕 | ADR（MADR 格式） | 从 architecture-decision-record 示例库抄模板起步 |
| 实验可复现 | Aim（轻）/ DVC（重） | 个人项目先 Aim，数据集大再上 DVC |
| 发布前扫秘密 | gitleaks | pre-commit + CI 双挂 |
| AI 使用声明 | claude-devtools + ai-credit | 观测在前、声明在后；声明内容以项目 / 期刊规定为准 |
| 科研出论文 | showyourwork + Git tag/DOI | 可复现包面向审稿人开放 |
