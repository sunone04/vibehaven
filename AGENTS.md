# AGENTS.md

> 本文件是 Agent 的导航地图，不是百科全书。
> 遵循渐进式披露：从这里出发，按需深入 `docs/` 中的真实信息源。
> 上下文是稀缺资源——只读你当前任务需要的部分。
> 与用户交流后，自行填充并完善docs中的内容

## 仓库定位

VibeHaven 是面向 Agent 的 Vibe Coding 工程底座。拉取后即可作为任意项目的起点。
核心理念：**人类掌舵，智能体执行。** 仓库即记录系统，所有知识版本化存放于此。

## 阅读选择（按任务类型选择入口）

| 你要做什么 | 先读这里 |
|---|---|
| 理解工作流程与多 Agent 协作 | [Workflow.md](Workflow.md) |
| 理解系统全貌与分层约束 | [ARCHITECTURE.md](ARCHITECTURE.md) |
| 理解页面与交互设计风格 | [Design.md](Design.md) |
| 理解产品需求与验收标准 | [docs/Product/index.md](docs/Product/index.md) |
| 理解技术设计与接口契约 | [docs/System/HLD.md](docs/System/HLD.md) |
| 查看当前迭代状态与计划 | [docs/Execution/index.md](docs/Execution/index.md) |
| 查阅历史决策与理由 | [docs/Decisions/index.md](docs/Decisions/index.md) |
| 查找参考资料与示例 | [docs/References/articles.md](docs/References/articles.md) |

## 架构铁律（必须遵守）

1. **分层依赖单向**：`Types → Config → Repo → Service → Runtime → UI`，禁止逆向依赖。详见 [ARCHITECTURE.md](ARCHITECTURE.md)。
2. **边界处解析数据形状**：在系统边界（入口、外部 API、数据源）解析并校验数据，内部信任已解析的类型。不内部重复校验。
3. **横切关注点走 Providers**：认证、遥测、功能开关、外部连接器只能通过唯一的 Providers 接口进入，禁止在业务层直接引用。
4. **不手动编写文档外的隐式约定**：所有约束要么写进本文档体系，要么编码为 linter / 结构测试。存在于聊天记录或人脑中的规则对 Agent 不存在。

## 工作方式

- **任务拆解**：将大目标拆为更小的构建模块（设计→代码→评审→测试），逐个推进并解锁更复杂任务。
- **计划即工件**：复杂工作记录为执行计划，提交到 `docs/Execution/`，附带进度与决策日志。轻量变更可用临时计划。
- **遇到困难是信号**：当无法推进时，识别缺失的内容（工具、约束、文档），将其反馈进仓库，而非硬编码绕过。
- **自审循环**：提交前在本地审核自身更改，响应审查反馈，循环直至通过。

## 文档维护

- 文档是活工件，必须反映真实代码行为。发现过时文档应发起修复。
- 每个文档目录有 `index.md` 作为该域入口，列出文档清单与状态。
- 新增文档须交叉链接、归类到对应目录，并在该目录 `index.md` 登记。

## 文档地图

```
AGENTS.md              ← 你在这里（导航地图）
Workflow.md            ← 工作流程、多 Agent 协作、项目类型适配
ARCHITECTURE.md        ← 系统分层架构、技术栈、数据流向、全局约束
Design.md              ← 页面设计风格与设计原则
docs/
├── Product/           ← 产品需求、功能设计、验收标准
│   ├── index.md
│   ├── PRD.md
│   └── Product-sense.md
├── System/            ← 系统与技术设计
│   ├── HLD.md         ← 高层设计、模块划分、依赖边界
│   ├── API-doc.md     ← 接口契约
│   └── DB-schema.md   ← 数据模型
├── Execution/         ← 迭代计划与执行过程
│   ├── index.md
│   └── Product-state.md
├── Decisions/         ← 团队决策记录（ADR）
│   └── index.md
└── References/        ← 参考资料与示例
    ├── articles.md
    └── 同类参考.md
```

## 快速行动清单

开始任何编码任务前，确认你已：
1. 读完 [Workflow.md](Workflow.md) 的工作流程与项目类型适配指引
2. 读完 [ARCHITECTURE.md](ARCHITECTURE.md) 的分层约束与全局规则
3. 查阅 [docs/Execution/Product-state.md](docs/Execution/Product-state.md) 了解当前状态
4. 确认任务对应的执行计划是否存在（无则先在 `docs/Execution/` 创建）
5. 检查相关模块的依赖边界是否清晰
