# VibeHaven

面向 AI Agent 的 Vibe Coding 工程底座。拉取后即可作为任意项目起点，以知识库为项目长期记忆，让纯 AI Coding 也能稳定交付中大型项目。

灵感来源于 [OpenAI 的 Harness Engineering 实践](https://openai.com/zh-Hans-CN/index/harness-engineering/)——在智能体优先的世界中，工程师的工作从编写代码转向设计环境、明确意图和构建反馈回路。

## 核心理念

**人类掌舵，智能体执行。**

- 仓库即记录系统——所有知识版本化存放，Agent 运行时无法访问的即不存在
- AGENTS.md 是地图而非百科全书——遵循渐进式披露，按需深入
- 约束边界而非微观管理——在边界内允许实现自由
- 计划即工件——执行计划版本化存放于仓库

## 快速开始

```bash
git clone https://github.com/sunone04/vibehaven.git your-project
cd your-project
```

然后：

1. 在 [ARCHITECTURE.md](ARCHITECTURE.md) 中登记技术栈与命名约定
2. 在 [docs/Product/PRD.md](docs/Product/PRD.md) 中填写产品需求与验收标准
3. 在 [Design.md](Design.md) 中登记设计令牌
4. 用你的 AI Coding 工具（Codex、Cursor、Trae 等）开始开发，Agent 从 [AGENTS.md](AGENTS.md) 进入

## 仓库结构

```
AGENTS.md              Agent 入口（导航地图）
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

## 技术无关

本底座不绑定任何具体技术栈。所有占位以 `_待填写_` 标记，拉取后填入你的选型即可。技术选型偏好：可组合性、API 稳定性、在训练集中表现良好（"枯燥"的技术更易建模）。

## License

MIT
