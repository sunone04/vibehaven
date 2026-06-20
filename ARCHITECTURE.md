# ARCHITECTURE.md

系统架构与工程约束。技术无关，但结构可预测。Agent 在此获取"什么能依赖什么"的确定性答案。

## 设计哲学

**约束边界，在边界内允许实现自由。** 依赖方向单向且可机械验证；在边界处解析数据形状，内部信任已解析类型；选择可完全内化于仓库中推理的依赖与抽象。

## 默认分层范式

每个业务域内，代码只能"向前"（向下）依赖一组固定的层。依赖方向严格单向。

```
UI          表现层：组件、页面、路由
Runtime     应用运行时：启动、中间件、编排
Service     业务逻辑：领域服务、用例编排、事务边界
Repo        数据访问：仓储、数据源、持久化边界
Config      配置形状：静态常量、特性开关默认值
Types       类型定义：领域模型、数据形状（零依赖基石）
```

| 层 | 职责 | 可依赖 |
|---|---|---|
| **Types** | 类型定义、领域模型、枚举 | 无 |
| **Config** | 配置形状定义、静态常量、特性开关默认值 | Types |
| **Repo** | 仓储接口、数据源适配、持久化边界 | Types, Config |
| **Service** | 业务逻辑、用例编排、事务边界 | Types, Config, Repo |
| **Runtime** | 服务启动、中间件、依赖注入、编排 | Service（及以下） |
| **UI** | 组件、页面、路由、交互状态 | Runtime, Service（仅查询） |

**这是默认范式，不是铁律。** 按项目类型裁剪，见 [Workflow.md](Workflow.md) 的项目类型适配表。裁剪后须在本文档登记实际分层，并确保依赖单向可机械验证。

Config 层只定义配置形状与静态值。环境变量与外部配置的实际读取在 Runtime 层启动时完成，或通过 Providers 注入——避免 I/O 散落到各层。UI 可直接调用 Service 的查询方法；写操作（命令）须通过 Runtime 层编排，避免业务逻辑散落 UI 层。

### 横切关注点：Providers

认证、遥测、功能开关、外部连接器等横切关注点，建议通过统一的 Providers 接口进入业务层，而非在业务层直接 import 具体实现。Providers 定义在 Runtime 层，业务层只依赖其在 Types 层的接口契约。

简单项目可不必引入此模式；复杂项目引入后可避免横切逻辑散落各处。

## 数据流向

请求自上而下穿过各层，数据形状在边界处被解析：

- 数据进入 Service 层前，必须已被解析为已知类型（parse, don't validate）
- Service 层返回已定型的领域对象，UI 层无需再次校验
- 错误在产生处包装为领域错误类型，向上传播时保持类型信息

## 工程约束

以下约束应尽量编码为 linter / 结构测试机械强制执行。**未编码为工具的约束只是建议。** 登记工具配置位置后，约束才生效。

### 依赖与边界

1. **依赖方向单向**：禁止逆向依赖，禁止跨层跳跃（如 UI 直接引用 Repo）
2. **边界解析**：系统入口处必须有数据形状解析，禁止"裸数据"进入 Service
3. **同层不直接互调**：同层模块通过 Service 层编排，不横向直接依赖
4. **接口定义在 Types 层**：跨层依赖的是接口契约，而非具体实现
5. **UI 不直接编排写操作**：UI 可直接调用 Service 的查询方法，写操作须通过 Runtime 层编排

### 代码规范

6. **命名约定**：遵循下方登记的项目命名规范
7. **文件大小**：单文件不超过 _N_ 行（按项目设定），超出须拆分
8. **结构化日志**：日志须结构化输出，禁止裸字符串日志

### Git 工作流

9. **分支**：主分支保持可发布；功能开发用独立分支
10. **提交**：一个提交做一件事，提交信息说明"为什么"而非"做了什么"
11. **Secrets**：密钥与凭证不进仓库，通过环境变量或密钥管理服务注入

### 测试

12. **同步编写**：测试与实现同步，不是后置阶段
13. **分层测试**：单元测试覆盖 Service/Repo；集成测试覆盖 Runtime；端到端覆盖关键用户路径
14. **验收标准可验证**：PRD 中的每条验收标准须有对应测试

## 技术栈

> 拉取后在此登记技术选型。偏好原则：可组合性、API 稳定性、在训练集中表现良好（"枯燥"的技术更易建模）。

| 层 | 技术选型 | 版本 | 备注 |
|---|---|---|---|
| 语言/运行时 | _待填写_ | | |
| UI 框架 | _待填写_ | | |
| Runtime/服务框架 | _待填写_ | | |
| 数据验证 | _待填写_ | | 边界处解析 |
| ORM/数据访问 | _待填写_ | | |
| 数据库 | _待填写_ | | |
| 认证 | _待填写_ | | 通过 Providers 注入 |
| 遥测 | _待填写_ | | 通过 Providers 注入 |
| 测试框架 | _待填写_ | | |
| 包管理器 | _待填写_ | | |
| CI/CD | _待填写_ | | |

## 命名约定

> 登记项目命名规范。

| 对象 | 约定 | 示例 |
|---|---|---|
| 类型/接口 | _待填写_ | |
| 文件名 | _待填写_ | |
| 数据库表 | _待填写_ | |
| API 路由 | _待填写_ | |
| 组件 | _待填写_ | |

## 架构验证

> 上述约束应编码为工具机械强制执行。**未编码为工具的约束只是建议。** 以下为起步模板，拉取后按技术栈填入并登记实际配置位置。

### 依赖方向检查

按技术栈选择依赖分析工具：

| 技术栈 | 工具 | 配置位置 |
|---|---|---|
| JavaScript/TypeScript | [dependency-cruiser](https://github.com/sverweij/dependency-cruiser) | `.dependency-cruiser.js` |
| Python | [import-linter](https://import-linter.readthedocs.io/) | `importlinter.toml` |
| Java/Kotlin | [ArchUnit](https://www.archunit.org/) | 测试代码 |
| Go | [go-arch-lint](https://github.com/fe3dback/go-arch-lint) | `.go-arch-lint.yml` |
| Rust | [cargo-deny](https://github.com/EmbarkStudios/cargo-deny) + 自定义脚本 | `deny.toml` |

最小规则示例（dependency-cruiser，对应上述分层）：

```js
// .dependency-cruiser.js
module.exports = {
  forbidden: [
    { name: 'ui-to-repo',    from: { path: 'src/ui/' },      to: { path: 'src/repo/' } },
    { name: 'ui-to-config',  from: { path: 'src/ui/' },      to: { path: 'src/config/' } },
    { name: 'repo-to-ui',    from: { path: 'src/repo/' },    to: { path: 'src/ui/' } },
    { name: 'service-to-ui', from: { path: 'src/service/' }, to: { path: 'src/ui/' } },
    { name: 'types-depends', from: { path: 'src/types/' },   to: { path: 'src/(ui|runtime|service|repo|config)/' } },
    // 按实际分层补充
  ]
};
```

### CI 校验

最小 CI 作业须包含：依赖方向检查、测试套件、构建/类型检查。

```yaml
# .github/workflows/ci.yml
name: CI
on: [pull_request]
jobs:
  check:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4   # 按技术栈替换
        with: { node-version: '20' }
      - run: npm ci
      - run: npx dependency-cruiser src   # 依赖方向
      - run: npm test                     # 测试
      - run: npm run build                # 构建/类型检查
```

拉取后按你的技术栈替换工具与命令，并在上方表格登记实际使用的工具。

## 相关文档

- 高层设计与模块划分：[docs/System/HLD.md](docs/System/HLD.md)
- 接口契约：[docs/System/API-doc.md](docs/System/API-doc.md)
- 数据模型：[docs/System/DB-schema.md](docs/System/DB-schema.md)
- 架构决策记录：[docs/Decisions/index.md](docs/Decisions/index.md)
