# Skills 技能模块库

存放可复用的 AI 技能 Prompt，按场景拆分能力模块，支持 Agent 自动调用。

## 目录结构

```
skills/
├── writing/          # 内容生成
├── coding/           # 开发辅助
├── ai-workflow/      # AI 工作流
├── design/           # 设计能力
├── research/         # 研究分析
├── productivity/     # 效率管理
├── automation/       # 自动化能力
├── README.md         # 项目说明
└── skills-index.md   # 技能索引
```

## 分类说明

### writing/ — 内容生成
小红书文案、爆款标题、PRD 撰写、产品文档、SEO 长文、Twitter Thread 等社交内容。

### coding/ — 开发辅助
React 组件、Python 脚本、API 设计、Debug 定位与修复。包含 HTML→Vue 研发转化 Skill，支持将投标演示界面自动转化为符合 WiNEX-BI 规范的 Vue 2.7 生产组件。

### ai-workflow/ — AI 工作流
Prompt Engineering 技巧、RAG 检索增强生成流程、Multi-Agent 协作模板、评测标准与效果验证。

### design/ — 设计能力
UI 界面分析与改进建议、落地页/Banner 设计、视觉系统与设计规范。

### research/ — 研究分析
竞品分析与对比、用户调研与需求洞察、行业报告与趋势分析、AI 产品趋势追踪。

### productivity/ — 效率管理
日报/周报/复盘总结、任务拆解与优先级规划、Roadmap 规划与执行跟踪、会议纪要与要点整理。

### automation/ — 自动化能力
n8n 工作流模板、Dify 应用开发模板、Webhook 配置与集成、批量生成与自动化任务。

## 使用方式

每个 Skill 目录下包含 SKILL.md 文件，描述该技能的用途、触发条件和执行流程。Agent 会根据 SKILL.md 中的 description 自动选择合适的技能。

## 贡献指南

1. 在对应分类目录下创建新的 Skill 目录
2. 编写 SKILL.md，遵循 Agent Skills 规范
3. 确保包含必要的 scripts/、references/、assets/ 子目录
4. 更新 skills-index.md 索引
