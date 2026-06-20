# Workflow.md

从目标到交付的标准流程。Agent 须按项目复杂度自主调整每步深度，但六步顺序不变。

## Agent 自主性边界

Agent 须在以下场景**自主决策**，不阻塞用户：
- 约束内有明确答案的实现选择（命名、文件组织、代码结构）
- 已有执行计划覆盖的任务推进
- 测试编写与修复
- 文档同步更新
- 约束内的 Bug 修复与重构

Agent 须在以下场景**暂停并询问用户**：
- 产品决策（新增功能、改变用户行为、调整优先级）
- 技术选型（引入新依赖、更换框架或数据库）
- 破坏性操作（数据库迁移、删除数据、重构公共 API）
- 约束外的架构选择（分层范式调整、新增横切关注点）
- 验收标准模糊且无法从现有文档推断

判断原则：**纠错成本低则自主，不可逆或影响产品方向则询问。** 当不确定时，倾向于询问而非假设——错误的方向假设比一次澄清提问代价更大。

## 标准流程

### 1. 目标接收

确认目标清晰可执行（模糊则向人类澄清）、验收标准存在（无则先对齐）、是否有关联执行计划（查 [docs/Execution/](docs/Execution/index.md)）。

### 2. 任务拆解

将目标拆为可独立实现与测试的构建块。原则：
- **深度优先**：先完成一个构建块的完整闭环，再推进下一个
- **最小可合并**：每个构建块能独立合并，不依赖未完成的后续块
- 复杂工作先创建执行计划，提交到 [docs/Execution/](docs/Execution/index.md)

### 3. 实现与测试

实现构建块并**同步编写测试**——测试是实现的一部分，不是后置阶段。遵守 [ARCHITECTURE.md](ARCHITECTURE.md) 与 [Design.md](Design.md) 的约束，对照 [docs/Product/PRD.md](docs/Product/PRD.md) 的验收标准。

### 4. 自审与审查

提交前自审：分层依赖是否合规、数据边界是否在入口处解析、命名与文件大小是否符合约束、测试是否覆盖验收标准。

**多 Agent 场景（可选）**：若环境支持，可由独立 Review Agent 审查架构合规性与代码质量，Test Agent 运行测试并验证验收标准。审查反馈须包含可执行的修复指令。修复后重新自审，循环直至通过。

**单 Agent 场景**：自审即审查。提交前对照 [ARCHITECTURE.md](ARCHITECTURE.md) 约束逐项检查，运行全部测试。

### 5. 合并

审查通过后合并。人类仅在需要判断时介入。测试偶发失败通过重跑解决，不无限期阻塞。

### 6. 文档完善

合并后更新仓库知识库：
- 更新 [docs/Execution/Product-state.md](docs/Execution/Product-state.md) 的已完成功能与状态
- 架构变更更新 [ARCHITECTURE.md](ARCHITECTURE.md) 或 [docs/System/HLD.md](docs/System/HLD.md)
- 重要决策在 [docs/Decisions/](docs/Decisions/index.md) 记录 ADR
- 新增 API 或数据模型更新 [docs/System/API-doc.md](docs/System/API-doc.md) 或 [docs/System/DB-schema.md](docs/System/DB-schema.md)

## 项目类型适配

核心原则（仓库即记录系统、渐进式披露、边界解析、依赖单向）始终保留。分层结构按项目类型裁剪：

| 项目类型 | 分层范式 | 流程深度 |
|---|---|---|
| 全栈 Web 应用 | `Types → Config → Repo → Service → Runtime → UI` | 完整六步 |
| API 服务 / 后端 | `Types → Config → Repo → Service → Runtime` | 完整，重点审查 API 契约 |
| CLI / 脚本工具 | `Types → Config → Service → Runtime(CLI)` | 简化，审查循环缩减为一轮 |
| 静态站点 / 内容站 | `Types → Config → UI` | 简化，重点审查设计令牌 |
| 数据管道 / ETL | `Source → Transform → Sink` | 完整，重点审查数据边界 |
| 库 / SDK | `Core → Public API → Tests` | 完整，重点审查 API 稳定性 |
| 单文件 / 原型 | 不分层 | 最小化，自审即可 |

不在表中的项目类型：识别核心数据流向，定义可机械验证的分层范式，登记到 [ARCHITECTURE.md](ARCHITECTURE.md)，并在 [docs/Decisions/](docs/Decisions/index.md) 记录决策。

## 吞吐量原则

在智能体吞吐量远超人类注意力的系统中：**纠错成本低，等待成本高。** 合并门禁只保留硬性约束（分层合规、测试通过）；人类介入最小化；多个构建块可并行推进，各自独立 PR。
