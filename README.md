# math-modeling-skills

数学建模竞赛（**CUMCM 国赛 / 五一赛 / 美赛 MCM-ICM**）可复用 Agent skills 合集，适用于 **Claude Code / Codex / DeepSeek Harness (DSH)**。

## 包含的 skill

| skill | 定位 | 适合谁 |
|-------|------|--------|
| [`math-modeling-contest`](math-modeling-contest/) | 论文一站式产出与自查（专家级） | 想把一道题完整做成获奖论文 |
| [`math-modeling-beginner-coach`](math-modeling-beginner-coach/) | 零基础速成教练 | 零基础、学习能力不强、需要手把手带 |

## 安装

本合集只依赖标准 `SKILL.md`（YAML 头 `name` + `description`）+ `references/`，三个主流 Agent 都能加载。

### 安装全部

- **Claude Code**：把需要的 skill 文件夹复制到项目 `.claude/skills/` 或全局 `~/.claude/skills/`。
- **Codex CLI**：
  ```bash
  npx skills add <owner>/math-modeling-skills --agent codex --skill '*' --yes
  ```
- **DeepSeek Harness (DSH)**：
  ```bash
  dsh plugin add <owner>/math-modeling-skills
  ```

### 安装单个

把命令里的 `--skill '*'` 换成具体 skill 名，例如：

```bash
npx skills add <owner>/math-modeling-skills --agent codex --skill math-modeling-contest --yes
```

## 使用

- **math-modeling-contest**：当要解数模题（含附件数据）、写论文、对已有论文做 AI 自查、做备赛规划时调用。详见其 `README.md` 与 `安装使用说明.txt`。
- **math-modeling-beginner-coach**：当用户是零基础、要手把手带时调用。详见其 `SKILL.md`。

两者共享的核心原则：

1. **人主导，AI 执行，可追溯** —— 决策你来做，AI 只执行；配合 2026 国赛 AI 新规保留使用记录。
2. **先简单后复杂** —— 简单方法先做基线，复杂方法做对比。
3. **永远验证，不盲信** —— AI 给的文献/公式/数据都要独立验证。
4. **获奖优先级** —— 数学结构 > 可解释 > 求解稳定 > 对比实验 > 图表 > 算法数量。

## 目录结构

```
math-modeling-skills/
├── README.md
├── math-modeling-contest/          # 专家级：论文产出与自查
│   ├── SKILL.md
│   ├── README.md
│   ├── 安装使用说明.txt
│   ├── agents/openai.yaml
│   └── references/                 # 12 份方法论参考
└── math-modeling-beginner-coach/   # 零基础教练
    ├── SKILL.md
    └── references/                 # 3 份速查参考
```

## 来源与致谢

方法论蒸馏自多篇真实获奖论文复盘（2025/2026 国赛 C 题、C038 农作物、2024 A 题板凳龙等）与公开数模教学（含 B 站「五条数模」「数模加油站」「数学建模老哥」等）。
