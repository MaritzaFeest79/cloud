# TerminusNavigator

TerminusNavigator 是一个面向技术决策者与基础设施工程师的轻量级导航与资源聚合系统。该项目定位为“技术外链的语义化索引层”，旨在解决复杂技术生态中官方文档、社区工具、镜像站与第三方服务链接分散、版本滞后、检索效率低下的问题。TerminusNavigator 不提供内容托管，仅作为结构化引用枢纽，通过纯静态页面与可编程 API 响应，为内部团队、开源社区及自动化运维流水线提供稳定、可校验的外部资源引用入口。

目标用户包括：需要快速定位特定版本二进制分发的 SRE 工程师、维护多镜像源配置的包维护者、撰写技术方案需要引用权威参考链接的架构师，以及搭建聚合仪表板的技术运营人员。项目自身采用 Markdown 驱动的配置模式，所有外链资源以版本化清单形式管理，支持通过轻量级脚本进行可达性检测与变更审计，确保引用的长期有效性。

## 功能概览

- **语义化资源分组**：支持按技术领域、供应商、地区镜像或内部项目代号对链接进行多级标签分组，每组可附带用途说明与更新周期注释。

- **版本锚点引用**：允许为同一工具的不同主流版本（如 LTS、Stable、Edge）分别记录独立 URL，并在页面渲染时标记推荐版本与废弃版本。

- **自动可达性探测**：集成异步 HTTP HEAD 请求机制，定期对已收录链接进行状态码检查，并在管理界面中高亮显示异常条目，辅助维护者及时清理或更新。

- **自定义重写规则**：提供简单的路径重写引擎，可将短别名（如 `/bifeng`）映射至完整的原始 URL，便于内部文档使用稳定短链。

- **只读 API 输出**：支持以 JSON 格式输出全量资源清单或按分组筛选的子集，方便下游监控系统、CI/CD 流水线或 ChatOps 机器人动态获取最新外链数据。

- **静态站点生成**：内置模板引擎可将资源数据渲染为响应式 HTML 仪表板，无需动态后端服务即可部署至任意 Web 服务器或对象存储桶。

- **变更审计日志**：每次资源增删改操作均记录时间戳与操作者（基于环境变量注入），日志以结构化格式存储，便于回溯历史配置状态。

## 应用场景

- **内部技术仓库的依赖引用标准化**：企业内多个项目组维护独立的 Dockerfile 或部署脚本时，常出现硬编码外部下载地址且版本混乱的情况。TerminusNavigator 可作为统一引用源，各项目通过调用 API 获取推荐的下载链接，确保全网使用一致的官方或镜像地址。

- **开源项目 README 与文档的链接维护**：开源项目维护者需要在 README、Wiki 或用户手册中反复引用多个社区资源、插件仓库或基准测试站点。使用 TerminusNavigator 生成的外链表格可嵌入文档，当外部站点变更时仅需更新一处配置，无需逐个修改历史文档版本。

- **离线环境与多地域部署的镜像配置管理**：在金融、政务等受限网络环境中，运维团队需维护一组内部可访问的镜像站列表。TerminusNavigator 支持按地域或网络区域分组，配合可达性探测快速识别当前可用的最优镜像入口，加速初始部署与灾难恢复流程。

- **技术社区导航页的快速重建**：技术运营人员为线上黑客松、培训工作坊或文档日搭建临时资源导航页时，可借助 TerminusNavigator 的静态站点生成能力，在数分钟内输出包含全部必要外链的独立页面，活动结束后即可下线，避免长期维护成本。

- **自动化测试套件的外部依赖预检**：CI 流水线在执行集成测试前，可调用 TerminusNavigator 的探测接口对本次测试所需的外部服务（如测试用 OCI 仓库、指标上报端点）进行连通性预检，若发现不可用则提前失败，避免无效测试运行时间浪费。

## 快速开始

以下指令适用于 Linux/macOS 环境，Windows 用户可使用 WSL2 或 Git Bash 执行。项目依赖 Git、Node.js 18+ 与 npm。

```bash
# 克隆项目仓库至本地
git clone https://github.com/terminus-navigator/tn-core.git
cd tn-core

# 安装项目依赖（包括探测脚本、模板引擎与测试工具）
npm install

# 复制示例配置文件并编辑资源清单（resources.example.yaml -> resources.yaml）
cp config/resources.example.yaml config/resources.yaml

# 执行静态站点生成与本地预览（默认监听 8080 端口）
npm run build
npm run serve
```

执行完毕后，访问 `http://localhost:8080` 即可查看生成的导航仪表板。若需仅输出 JSON 数据供脚本调用，可使用 `npm run export:json`，输出文件位于 `dist/registry.json`。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Node.js | 18.17.0 或更高 | 运行时环境，用于执行构建脚本、API 服务与探测任务 |
| npm | 9.0.0 或更高 | 依赖管理工具，用于安装项目所需第三方库 |
| Git | 2.30.0 或更高 | 版本控制工具，用于克隆仓库及提交配置变更 |
| 网络访问（出站） | TCP/443 与 TCP/80 可达 | 用于可达性探测及静态资源（如字体、CSS 框架）的 CDN 加载 |
| 文件系统权限 | 读写权限（项目目录） | 用于写入构建产物、日志及缓存文件 |
| 内存 | 最低 512 MB，推荐 1 GB | 用于支持并发探测任务及模板渲染开销 |
| 操作系统 | Linux、macOS、Windows（WSL2） | 未在原生 Windows PowerShell 环境下完整测试，建议使用 WSL2 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户指南 | `docs/user-guide/` | 如何配置资源分组、设置版本锚点、自定义页面标题与页脚信息 |
| 运维手册 | `docs/operations/` | 如何部署至 Nginx、S3 或 CDN；如何设置周期性探测任务与告警通知 |
| API 参考 | `docs/api/` | 各 API 端点的请求参数、响应格式、状态码含义及速率限制说明 |
| 开发指南 | `docs/development/` | 项目目录结构说明、新增分组类型的扩展方式、单元测试编写规范 |
| 贡献规范 | `CONTRIBUTING.md` | 提交 PR 的流程、Commit Message 格式要求、代码风格检查命令 |
| 安全策略 | `SECURITY.md` | 报告安全漏洞的联系方式、响应时间承诺及已公布的安全更新记录 |

## 资源列表

以下为 TerminusNavigator 当前版本收录的全部外部资源链接。所有条目均保留用户原始输入格式，未做任何协议补全或域名规范化处理。

### 主数据源（实时比分与动态信息）

- <code>shishibifenw.org.cn</code>

### 备用镜像与历史版本存档

- <code>qiutanzuqiubifenjiuban.net.cn</code>
- <code>qiutanzuqiubifen777.org.cn</code>
- <code>qiutanzuqiubifen500.org.cn</code>

### 社区贡献节点与测试端点

- <code>qiutanzuqiubifengw.org.cn</code>
- <code>qiutanzuqiubifenwz.org.cn</code>
- <code>qiutanzuqiubifengf.org.cn</code>

上述资源链接已纳入默认分组 `external/community` 与 `external/mirror`。维护者可依据实际网络策略调整各条目的启用状态或添加自定义标签。

## 项目结构

```
tn-core/
├── config/                         # 配置目录
│   ├── resources.example.yaml      # 示例资源清单（含分组与标签）
│   ├── probe.settings.json         # 探测超时、重试次数、并发数设置
│   └── rewrite.rules.example       # 短链重写规则示例
├── src/                            # 核心源码
│   ├── loader/                     # YAML 配置解析与校验模块
│   │   ├── index.js                # 入口加载器
│   │   └── schema.validator.js     # JSON Schema 校验器
│   ├── probe/                      # 可达性探测引擎
│   │   ├── http-checker.js         # 异步 HEAD/GET 请求实现
│   │   ├── scheduler.js            # 定时任务调度器
│   │   └── result-cache.js         # 探测结果内存缓存
│   ├── renderer/                   # 输出渲染引擎
│   │   ├── html-builder.js         # 基于 EJS 的 HTML 页面生成
│   │   ├── json-exporter.js        # JSON 格式数据导出
│   │   └── static-assets/          # 内置 CSS 与 JavaScript 资源
│   ├── api/                        # 只读 HTTP API 服务
│   │   ├── server.js               # Express 应用启动入口
│   │   ├── routes/                 # 路由定义（分组、全量、状态）
│   │   └── middleware/             # 日志与速率限制中间件
│   └── cli/                        # 命令行工具入口
│       ├── build.js                # 构建命令实现
│       ├── serve.js                # 预览服务命令
│       └── probe-run.js            # 手动触发探测命令
├── tests/                          # 单元测试与集成测试
│   ├── unit/                       # 各模块单元测试（Mocha + Chai）
│   └── integration/                # API 端到端测试（Supertest）
├── docs/                           # 完整文档（参见文档导航）
├── dist/                           # 构建输出目录（生成后存在）
│   ├── index.html                  # 静态仪表板主页
│   ├── registry.json               # 全量资源 JSON 导出
│   └── assets/                     # 经过压缩的静态资源副本
├── logs/                           # 运行日志及审计日志存储
│   ├── access.log                  # API 访问日志
│   └── audit.log                   # 资源变更与探测事件日志
├── .env.example                    # 环境变量示例（端口、日志级别、管理员邮箱）
├── package.json                    # npm 依赖清单与脚本定义
├── README.md                       # 项目说明（本文件）
└── LICENSE                         # MIT 许可证文本
```

## 贡献指南

1. **提交 Issue 讨论**：在发起 Pull Request 前，请先在 GitHub Issues 中创建对应议题，简要描述拟解决的问题或新增功能，并标注 `enhancement`、`bug` 或 `docs` 标签。核心维护者将在 2 个工作日内反馈可行性意见。

2. **派生仓库并创建功能分支**：从主仓库派生至个人账户，然后基于 `main` 分支创建新分支，分支命名规则为 `feat/功能简述`、`fix/问题简述` 或 `docs/文档章节`。禁止直接向 `main` 分支提交。

3. **遵循代码规范与测试要求**：JavaScript 代码需通过 ESLint 配置（基于 Airbnb 风格）检查，新增功能必须附带对应的单元测试或集成测试用例，且测试通过率不得低于 95%。提交前执行 `npm run lint` 与 `npm test`。

4. **更新文档与资源清单示例**：若修改了配置结构、API 响应字段或环境变量，请同步更新 `docs/` 下相关文档以及 `config/resources.example.yaml` 中的注释说明，确保新用户可通过示例快速上手。

5. **发起 Pull Request 并接受 Code Review**：将分支推送至派生仓库后，向主仓库 `main` 分支发起 Pull Request，描述中需关联对应 Issue 编号，并勾选自检清单（测试通过、文档更新、无合并冲突）。至少一名核心维护者审批通过后方可合并。

## 常见问题

**问：可达性探测会产生大量对外请求，是否会被目标站点限流或封禁？**

答：TerminusNavigator 的探测模块默认采用指数退避重试策略，且并发请求数限制为 5，单个目标的最小探测间隔为 1 小时。同时支持用户自定义 `probe.settings.json` 中的 `rateLimit` 字段，调整为更保守的并发值或延长间隔。建议在内部网络或通过代理进行探测，避免使用单一 IP 高频访问公共站点。

**问：如何迁移已有的大量链接至 TerminusNavigator？是否支持从 CSV 或 JSON 导入？**

答：项目本身不提供自动迁移工具，但 `config/resources.example.yaml` 中定义了清晰的分组与字段映射结构。用户可编写简单脚本将现有 CSV（列映射为 `name`、`url`、`group`、`tags`）转换为 YAML 格式。社区维护了一份 Python 转换脚本参考，存放于 `contrib/import-tools/` 目录下，可供修改使用。

**问：静态生成的 HTML 仪表板是否支持暗色主题与移动端适配？**

答：内置模板已包含响应式 CSS 框架，默认跟随系统主题偏好（`prefers-color-scheme`），同时提供手动切换按钮。移动端布局自动折叠侧边栏，触摸目标尺寸适配触屏操作。所有样式均内联在构建产物中，无需额外加载外部样式库。

## 许可证

本项目采用 MIT 许可证授权。允许任何个人或组织自由使用、复制、修改、合并、发布、分发、再许可及销售本软件的副本，但需保留原始版权声明与许可声明。详细条款请参阅项目根目录下的 `LICENSE` 文件。

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:20
