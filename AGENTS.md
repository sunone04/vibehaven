# AGENTS.md

VibeHaven 是面向 AI Agent 的工程底座。拉取后作为任意项目起点，以本知识库为项目长期记忆，让 AI Coding 也能稳定交付中大型项目。

核心理念：**人类掌舵，智能体执行。仓库即记录系统——所有知识版本化存放，Agent 运行时无法访问的即不存在。**

## 按任务路由

| 你要做什么 | 读哪里 |
|---|---|
| 理解工作流程与项目类型适配 | [Workflow.md](Workflow.md) |
| 理解架构分层与工程约束 | [ARCHITECTURE.md](ARCHITECTURE.md) |
| 理解设计规范 | [Design.md](Design.md) |
| 理解要构建什么与验收标准 | [docs/Product/](docs/Product/index.md) |
| 理解技术设计与接口契约 | [docs/System/HLD.md](docs/System/HLD.md) |
| 查看当前状态与执行计划 | [docs/Execution/](docs/Execution/index.md) |
| 查阅历史决策 | [docs/Decisions/](docs/Decisions/index.md) |
| 查找参考资料 | [docs/References/](docs/References/articles.md) |

## 文档地图

```
AGENTS.md              入口（本文件）
Workflow.md            工作流程、项目类型适配
ARCHITECTURE.md        分层架构、工程约束、技术栈
Design.md              设计规范与令牌
docs/
├── Product/           产品需求与验收标准
├── System/            技术设计（HLD、API、DB Schema）
├── Execution/         执行计划与当前状态
├── Decisions/         决策记录（ADR）
└── References/        参考资料
```

## 工作原则

- **渐进式披露**：从这里出发，按需深入，只读当前任务需要的部分。上下文是稀缺资源。
- **单一信息源**：每条规则只在一处定义，其他地方只引用。发现重复或矛盾时修复。
- **文档是活工件**：必须反映真实代码行为。发现过时即修复，不要等待。
- **遇到困难是信号**：无法推进时，识别缺失的内容（工具、约束、文档），将其补充进仓库，而非硬编码绕过。
