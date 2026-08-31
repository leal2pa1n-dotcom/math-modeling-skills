# Agent 工具链：Codex / DeepSeek Harness / Skills

> 来源：B 站「五条数模」《2026 数模国赛零基础最快速成！十分钟学会用 Codex + DeepSeek harness + skills 拿下国赛国奖》（BV1F58H6ME6Y，5 集）+ 开源生态调研。核心思想：**把「读题→建模→求解→写作→自查」这条获奖流水线，沉淀成可安装的 Agent skill，让 AI 按固定工序干活，人只做关键决策与验收**。与本 skill 的 9-Phase 工作流（见 `现代工作流与工具链.md`）互补——后者讲「每阶段怎么做」，本文讲「用哪个 Agent 工具 + 装哪些 skill 来承载这套工序」。

## 一、三类 Agent 工具（选一个，别都装）

| 工具 | 驱动模型 | 安装 | 装 skill 方式 | 适用 |
|------|---------|------|--------------|------|
| **DeepSeek Harness (DSH)** | DeepSeek 模型（可接任意 OpenAI 兼容端点） | `npm i -g @deepseek-ai/dsh` | `dsh plugin add <repo>` | 想用 DeepSeek、预算敏感 |
| **Codex CLI** | OpenAI 模型 | `npm i -g @openai/codex` | `npx skills add <repo> --agent codex` | 用 GPT/Codex 模型 |
| **Claude Code** | Claude 模型 | 官方 CLI | 项目/全局 `skills/` 目录 + `SKILL.md` | 本 skill 即跑在它上面 |

三者都支持「skills / plugins」——即把一份份 `SKILL.md` 指令包注入 Agent，让它按固定流程工作。**skill 的核心价值不是模型更强，而是把「获奖流程」固化成可复用的工序，避免每次从零乱试。**

## 二、DeepSeek Harness (DSH) 速装

DeepSeek AI 开源的 Agent 运行时（MIT，2026-08-13 发布，预览版 0.1.0-rc.6，接口可能有破坏性变更），「一切皆插件」，基于 Cordis 微内核。

```bash
# 前置：Node.js 22.19+（建议 24 LTS）
npm install -g @deepseek-ai/dsh
dsh web            # 浏览器打开 http://127.0.0.1:3080
```

- 首次启动：Settings → Models 填 DeepSeek API Key（或其它 OpenAI 兼容端点）→ 选工作区目录。
- 四种模式：Web UI（新手）、TUI（`dsh --profile cli`）、Headless（`dsh --profile headless "任务"`，脚本/CI）、Python SDK（`pip install deepseek-harness-sdk`）。
- 装 skill：`dsh plugin add <owner>/<repo>`。
- 密钥存在 `$DSH_HOME/.credentials.yaml`，不上传。

## 三、现成数模 skill 生态（直接装，别从零写）

| 项目 | 定位 | 平台 | 安装 |
|------|------|------|------|
| **GateCraft** `Crayonnan/dsh-math-modeling-skills-Gatecraft-` | 门控式 9-skill 套件，**不做无脑端到端**，每阶段人审 + `paper-gate` 13 项机械验收（版心/图表公式编号对账/AI 痕迹扫描/摘要数字量级哨兵/篇幅均衡/错别字库），FAIL 未清零禁止提交 | DSH | `dsh plugin add Crayonnan/dsh-math-modeling-skills-Gatecraft-` |
| **Mathodology** `sweetcornna/mathodology` | 9 skills + 9 subagents + 2 workflow，含免费免 key 的 `free-search-mcp` 证据检索 | Claude Code / Codex | `npx skills add sweetcornna/mathodology --skill '*' --agent codex claude-code` |
| **math-modeling-skills** `y22762095-hub/math-modeling-skills` | 读题/建模/Python/验证/三角色交接/论文/提交前审查 | Codex | `npx skills add y22762095-hub/math-modeling-skills --agent codex --skill '*'` |
| **MathModelAgent**（LINUX DO 开源） | `/1start-mathmodel` 端到端，17 套 Typst 模板 + 建模知识库 + 9 步自动验收 | Codex/Claude/Pi | 按仓库说明 |

> 选型建议：DSH 用户选 **GateCraft**（门控 + 强制验收最贴合国赛「人主导」新规）；Claude Code 用户直接用**本 skill**（已覆盖更全的题型方法论与自查清单），可再叠加 Mathodology 的 `free-search-mcp` 补文献检索。

## 四、Agent 做数模的正确姿势（人主导，AI 执行）

视频核心主张：**十分钟完成一道 C 题 ≠ 全自动交卷**，而是「skill 把工序排好，Agent 按序执行，人只在门控点拍板」。与本 skill 的 `AI辅助与自查.md` 五法则完全一致：

1. **读题**：题目原文完整粘贴给 Agent，让它做 6 维分流（评价/预测/优化/分类/生存/机理）+ 列 3 个候选方向 + 问「最容易踩的坑」。人拍板走哪条。
2. **建模**：让 Agent 给出「模型名 + 完整推导 + 物理意义 + 前提假设 + 失效条件」，人核对假设是否成立。
3. **求解**：Agent 写代码 → 人核对（已知结果对一遍、极端值、量纲、可复现）→ 简单基线先跑，复杂方法做对比。
4. **写作**：骨架论点人写，Agent 扩写润色；摘要按评委标准让 Agent 挑套话，改不改人定。
5. **验收**：交稿前跑机械自查（GateCraft 的 `paper-gate` 或本 skill 的 26 维 AI 自查表），FAIL 清零才交。

## 五、AI 降重（视频 P5：有效降低 AI 重复率）

国赛 2026 新规 + 查重系统双重压力下，**降低 AI 重复率**的实操：

1. **别整段交给 AI 生成正文**。AI 出骨架、列数据、写代码、画图，正文用自己的话组织——这是降 AI 率最根本的一条。
2. **改写三原则**：换句式（主动↔被动、长句拆短句）、换语序（结论前置/后置）、换术语表述（同义但非模板化的说法），而非简单同义词替换。
3. **数字与公式不靠 AI 抄**：每个数字从自己的代码结果读出来，公式手写/自己排版，AI 生成的公式系数务必手算小例子验证（防编造）。
4. **保留可追溯记录**：按新规提交《AI 工具使用详情》，记录「用了什么工具、哪个环节、提示过程、采纳核验情况」——透明标注本身比「藏 AI 味」更安全，评委现在专门识别未标注的 AI 痕迹。
5. **自查**：把成品丢给另一个大模型做「AI 味检测 + 降重建议」，逐条改完再交（与本 skill `AI辅助与自查.md` 的 26 维自查联动）。

## 六、2026 国赛 AI 新规（硬约束，见 `竞赛规则与提交规范.md`）

- 允许用 AI，但**核心建模与分析必须参赛队主导**；用 AI 需在支撑材料交《AI 工具使用详情.pdf》（工具、版本、目的/环节、提示过程、采纳核验）。
- 未用 AI 则在参考文献后声明「本参赛队未使用任何 AI 工具」。
- 推论：上述 Agent 工具链的正确定位是「**人主导、AI 辅助、可追溯**」，不是无监督全自动交卷。
