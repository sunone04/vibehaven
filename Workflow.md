# Workflow.md

> Vibe Coding 工作流程。
> 本文件定义从目标到交付的标准流程，并提供适配指引让 Agent 自主调整以适应不同项目类型。

## 核心理念

**人类掌舵，智能体执行。** 人类负责描述目标与验收标准，Agent 负责拆解、实现、审查、测试与交付。

## 标准流程

### 1. 目标接收

人类描述目标。目标可以是功能需求、Bug 修复、重构或基础设施改进。

Agent 接收目标后须确认：
- 目标是否清晰可执行（模糊则向人类澄清）
- 验收标准是否存在（无则先与人类对齐）
- 是否有关联的执行计划（查 [docs/Execution/](docs/Execution/index.md)）

### 2. 任务拆解

Agent 将目标拆解为可独立推进的构建块。每个构建块应：
- 可独立实现与测试
- 有明确的完成定义
- 推进时解锁更复杂的后续任务

拆解原则：
- **深度优先**：先完成一个构建块的完整闭环（设计→代码→测试→审查），再推进下一个
- **最小可合并**：每个构建块应能独立合并，不依赖未完成的后续块
- 复杂工作须先创建执行计划，提交到 [docs/Execution/](docs/Execution/index.md)

### 3. 实现与测试

Agent 实现构建块并**同步编写测试**。测试不是后置阶段，而是实现的一部分。

实现时须遵守：
- [ARCHITECTURE.md](ARCHITECTURE.md) 的分层约束与全局规则
- [Design.md](Design.md) 的设计令牌与组件规范
- [docs/Product/PRD.md](docs/Product/PRD.md) 中的验收标准

完成后开 PR。PR 须包含：
- 变更说明
- 关联的验收标准
- 测试结果

### 4. 审查循环（Ralph Wiggum Loop）

PR 创建后进入审查循环。这是多 Agent 协作的核心环节。

```
自审 ──► Review Agent 评审 ──► Test Agent 测试
            │                       │
            └─── 反馈 ──► 修复 ──► 重新提交
                                  │
                            回到自审 ◄──┘
                          （循环直至全部通过）
```

#### 4a. 自审

Coding Agent 在提交前先本地审核自身更改：
- 分层依赖是否合规
- 数据边界是否在入口处解析
- 横切关注点是否走 Providers
- 命名约定与文件大小是否符合约束
- 测试是否覆盖验收标准

#### 4b. Review Agent 评审

审查架构合规性与代码质量。检查项须可机械执行：

| 检查项 | 依据 |
|---|---|
| 分层依赖方向 | [ARCHITECTURE.md](ARCHITECTURE.md) 铁律 1 |
| 边界数据解析 | [ARCHITECTURE.md](ARCHITECTURE.md) 铁律 2 |
| Providers 唯一入口 | [ARCHITECTURE.md](ARCHITECTURE.md) 铁律 3 |
| 命名约定 | [ARCHITECTURE.md](ARCHITECTURE.md) 命名约定表 |
| 文件大小限制 | [ARCHITECTURE.md](ARCHITECTURE.md) 全局约束 5 |
| 结构化日志 | [ARCHITECTURE.md](ARCHITECTURE.md) 全局约束 6 |
| 设计令牌引用 | [Design.md](Design.md) 设计令牌表 |

审查反馈须包含**可执行的修复指令**，而非仅指出问题。

#### 4c. Test Agent 测试

运行测试并验证验收标准。检查项：

| 检查项 | 说明 |
|---|---|
| 运行测试套件 | 全部通过，无 flaky 测试 |
| 验证验收标准 | 逐条核对 [PRD.md](docs/Product/PRD.md) 中的验收标准 |
| 边界情况 | 空输入、超长输入、并发、错误路径 |
| 性能基线 | 关键路径响应时间符合 [ARCHITECTURE.md](ARCHITECTURE.md) 性能要求 |

Test Agent 须能独立运行应用实例（如通过 git worktree 启动），以进行端到端验证。

#### 4d. 响应反馈

Coding Agent 对 Review Agent 和 Test Agent 的反馈进行响应：
- 修复指出的问题
- 重新提交更改
- 回到自审，重新进入循环

**循环退出条件**：Review Agent 和 Test Agent 均通过，无未解决反馈。

### 5. 合并

审查循环通过后合并 PR。

- 人类仅在需要判断时介入，非每个 PR 必须人工合并
- 测试偶发失败通常通过重跑解决，不无限期阻塞
- 吞吐量优先于完美门禁——纠错成本低，等待成本高

### 6. 文档完善

合并后更新仓库知识库：
- 更新 [docs/Execution/Product-state.md](docs/Execution/Product-state.md) 的已完成功能与状态
- 如有架构变更，更新 [ARCHITECTURE.md](ARCHITECTURE.md) 或 [docs/System/HLD.md](docs/System/HLD.md)
- 如有重要决策，在 [docs/Decisions/](docs/Decisions/index.md) 记录 ADR
- 如有新增 API 或数据模型，更新 [docs/System/API-doc.md](docs/System/API-doc.md) 或 [docs/System/DB-schema.md](docs/System/DB-schema.md)
- 如有过时文档，发起修复

**文档是活工件，必须反映真实代码行为。**

## 多 Agent 角色定义

| 角色 | 职责 | 触发时机 |
|---|---|---|
| **Coding Agent** | 拆解任务、实现功能、编写测试、开 PR、响应反馈修复问题 | 接到目标后 |
| **Review Agent** | 审查架构合规性与代码质量，给出可执行修复指令 | PR 创建后 |
| **Test Agent** | 运行测试、验证验收标准、检查边界情况与性能 | PR 创建后 / Review 通过后 |
| **人类** | 描述目标、定义验收标准、判断性审查与合并决策 | 需要判断时介入 |

## 项目类型适配指引

> Agent 须根据项目类型自主调整流程与架构约束。
> 以下为常见项目类型的适配建议，Agent 应结合实际项目特征判断并调整。

### 适配原则

1. **约束保留，分层裁剪**：核心原则（仓库即记录系统、渐进式披露、强制不变量、边界解析）始终保留；分层结构按项目类型裁剪
2. **流程不变，深度调整**：标准流程六步不变，但每步的深度按项目复杂度调整
3. **角色不缺，职责伸缩**：多 Agent 角色不缺，但简单项目可由单个 Agent 兼任多角色

### 常见项目类型适配

#### 全栈 Web 应用

- **分层**：完整使用 `Types → Config → Repo → Service → Runtime → UI`
- **流程深度**：完整六步，审查循环严格执行
- **测试**：单元 + 集成 + 端到端

#### API 服务 / 后端

- **分层**：`Types → Config → Repo → Service → Runtime`（去掉 UI 层）
- **流程深度**：完整六步，重点审查 API 契约与数据边界
- **测试**：单元 + 集成 + 契约测试

#### CLI / 脚本工具

- **分层**：`Types → Config → Service → Runtime(CLI)`
- **流程深度**：简化，自审可合并到实现阶段，审查循环可缩减为一轮
- **测试**：单元 + 命令行集成测试

#### 静态站点 / 内容站

- **分层**：`Types → Config → UI`（去掉 Repo/Service/Runtime）
- **流程深度**：简化，重点审查设计令牌与响应式
- **测试**：视觉回归 + 构建验证

#### 数据管道 / ETL

- **分层**：替换为 `Source → Transform → Sink` 范式
- **流程深度**：完整六步，重点审查数据边界与错误处理
- **测试**：数据质量测试 + 管道集成测试

#### 库 / SDK

- **分层**：替换为 `Core → Public API → Tests` 范式
- **流程深度**：完整六步，重点审查公共 API 稳定性与向后兼容
- **测试**：单元 + API 兼容性测试

#### 单文件 / 原型

- **分层**：不分层
- **流程深度**：最小化，自审即可，跳过审查循环
- **测试**：核心路径测试

### 自主适配判断

当项目不属于上述任何类型时，Agent 须：
1. 识别项目的核心数据流向与依赖结构
2. 定义适合的分层范式，登记到 [ARCHITECTURE.md](ARCHITECTURE.md)
3. 确保分层依赖单向且可机械验证
4. 在 [docs/Decisions/](docs/Decisions/index.md) 记录分层决策与理由

## 吞吐量原则

在智能体吞吐量远超人类注意力的系统中：
- **纠错成本低，等待成本高**——测试偶发失败通过重跑解决，不无限阻塞
- **合并门禁最小化**——只保留硬性约束（分层合规、测试通过）
- **人类介入最小化**——仅在需要判断时介入，非每个 PR 必须人工审查
- **并行推进**——多个构建块可并行推进，各自独立 PR 与审查循环
