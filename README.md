# investor-framework

个人价值投资知识库 **investor-analyzer** 的**抽象规则层**镜像——流程宪法、模板、skills。
**不含**任何公司数据、大V原文、主观判断记录。

> ⚠️ **本库是只读镜像，不要直接在这里改文件。**
> 真源在主库 investor-analyzer：改规则 = 改主库 → 跑 `bash scripts/export-rules.sh` 导出到这里。
> 每个 commit message 形如 `sync from investor-analyzer@<hash>`，指向主库对应版本。

## 这套框架是什么

分析一家公司，按「判断的性质」正交切成三个模块，落盘纪律互不越界：

| 模块 | 回答什么 | 规则文件 |
|---|---|---|
| ① 客观分析 | 这家公司客观长什么样（零主观） | `knowledge/frameworks/研究新公司-分析流程.md` + `finance/_模板/` |
| ② 大V理念 | 别人（大V）怎么看——来源/灵感，不是招牌 | `gurus/_schema/` + `gurus/_模板/` |
| ③ 主观判断 | 值多少 / 我懂不懂 / 买卖点 | `knowledge/frameworks/主观分析-workflow.md` + `judgments/_模板/` |

核心理念：**数据沉淀 > 一次性结论 · 只要事实不要传闻 · 不预测只归纳 · 先建事实底座再分析**。

## 目录结构

```
.
├── .claude/skills/          # 编排层：7 个 Claude Code skill（见下表）
├── knowledge/
│   ├── frameworks/          # 流程宪法：研究新公司-分析流程 + 主观分析-workflow
│   ├── companies/_模板/     # 公司事实编年模板（时间·主体·事件，只记事实）
│   └── fraud-cases/         # _案例模板 + 造假模式库.md（64 案例反向蒸馏的红旗库）
├── finance/_模板/           # 客观层模板：模板说明 / 财报阅读规则 / 分析模板 /
│                            #   关注要点行业模板 / 财务数据README模板 / 美股缩写对照
├── judgments/_模板/         # 主观层模板：估值 / 估值记录 / 能力圈 / 观点变更
└── gurus/
    ├── _schema/             # 大V知识库结构规范
    └── _模板/               # 大V目录骨架（profile / current-view / style / coverage /
                             #   posts / holdings / calls）
```

| skill | 职责 |
|---|---|
| `company-analysis` | 财报数据提取 → 建/刷新客观三件套（强制先提取再结论） |
| `judgment-flow` | 主观分析六步（估值 / 能力圈 / 买卖点，硬 gate 保 fresh 底座） |
| `daily-news` | 每日新闻哨兵：事实核验后追加事实编年 + 出日报 |
| `guru-query` | 通用大V视角查询（一个 skill 服务所有大V） |
| `ingest-guru` / `ingest-book` / `ingest-fraud-case` | 三个录入器：单篇 / 整本 PDF / 造假案例 |

## 怎么用它起一个新库

1. clone（或复制）本库作为新工作库的骨架；
2. **目录路径保持原样**——skills 与 canonical/模板之间是硬路径引用，镜像路径开箱即用；
3. 用 Claude Code 打开，喂第一家公司的财报，`company-analysis` 会按模板生成 `finance/<公司>/`；
4. 数据层目录（`finance/<公司>`、`judgments/<公司>`、`gurus/<slug>`、`knowledge/companies/` 等）在你自己的库里生长，不回流本库。

## 同步机制（维护者备忘）

- **白名单**：主库 `scripts/rules-manifest.txt` —— 唯一权威，白名单制保证数据层永不误发；
- **导出**：主库 `scripts/export-rules.sh` —— 全量镜像受管路径 → commit（带主库 hash）→ push；
- **提醒**：主库 post-commit hook（`scripts/git-hooks/post-commit`）检测到规则变更即提示同步；
- 本库自有文件（`README.md` / `LICENSE` / `.gitignore`）不在白名单内，导出永不覆盖。
