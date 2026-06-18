# VibeHaven

> 面向 Agent 的 Vibe Coding 工程底座。
> 拉取后即可作为任意项目的起点，让 AI Coding 构建出稳定、持续、高质量的项目。

## 这是什么

VibeHaven 是一个**面向 AI Agent 的工程脚手架**。它不包含任何业务代码，而是提供一套结构化的约束体系与知识库模板，让 Agent 在仓库内就能推理出完整的业务领域，产出符合架构约束的代码。

灵感来源于 [OpenAI 的 Harness Engineering 实践](https://openai.com/zh-Hans-CN/index/harness-engineering/)——在智能体优先的世界中，工程师的工作从编写代码转向设计环境、明确意图和构建反馈回路。

## 核心理念

**人类掌舵，智能体执行。**

- 仓库即记录系统——所有知识版本化存放，Agent 运行时无法访问的即不存在
- AGENTS.md 是地图而非百科全书——遵循渐进式披露，按需深入
- 强制不变量而非微观管理——约束边界，在边界内允许实现自由
- 计划即工件——执行计划版本化存放于仓库

## 仓库结构

```
AGENTS.md              Agent 导航地图（入口）
Workflow.md            工作流程与多 Agent 协作定义
ARCHITECTURE.md        分层架构范式、技术栈、数据流向、全局约束
Design.md              页面设计风格与设计令牌
docs/
├── Product/           产品需求、功能设计、验收标准
├── System/            系统与技术设计（HLD、API、DB Schema）
├── Execution/         迭代计划与执行过程
├── Decisions/         团队决策记录（ADR）
└── References/        参考资料与示例
```

## 架构范式

采用分层依赖单向架构，依赖方向可机械验证：

```
Types → Config → Repo → Service → Runtime → UI
```

横切关注点（认证、遥测、功能开关、外部连接器）通过唯一的 Providers 接口进入业务层。

该范式适用于全栈 Web 应用，并可按项目类型裁剪适配（API 服务、CLI、静态站点、数据管道、SDK 等）。详见 [Workflow.md](Workflow.md) 的项目类型适配指引。

## 工作流程

```
1. 目标接收 → 2. 任务拆解 → 3. 实现与测试 → 4. 审查循环 → 5. 合并 → 6. 文档完善
```

审查循环支持多 Agent 协作：

| 角色 | 职责 |
|---|---|
| Coding Agent | 拆解任务、实现功能、编写测试、开 PR |
| Review Agent | 审查架构合规性与代码质量 |
| Test Agent | 运行测试、验证验收标准、检查边界情况 |
| 人类 | 描述目标、定义验收标准、判断性审查 |

## 快速开始

```bash
git clone https://github.com/sunone04/vibehaven.git your-project
cd your-project
```

然后：

1. 在 [ARCHITECTURE.md](ARCHITECTURE.md) 中登记你的技术栈
2. 在 [docs/Product/PRD.md](docs/Product/PRD.md) 中填写产品需求与验收标准
3. 在 [Design.md](Design.md) 中登记设计令牌
4. 将 [Workflow.md](Workflow.md) 中的项目类型适配到你的场景
5. 开始用你的 AI Coding 工具（Codex、Cursor、Trae 等）进行开发

## 技术无关

本底座不绑定任何具体技术栈。所有占位以 `_待填写_` 标记，拉取后填入你的选型即可。

技术选型偏好原则：可组合性、API 稳定性、在训练集中表现良好（"枯燥"的技术更易建模）。

## License

MIT
