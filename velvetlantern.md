# NovaIndex

NovaIndex 是一个面向技术团队与独立开发者的轻量级外链资源导航系统，专为高效组织、分类呈现与快速检索外部技术资源而设计。其核心定位并非传统的书签管理器，而是一个可版本化、可协同维护的“结构化资源索引层”，适用于将分散于各处的官方文档、社区论坛、API 参考、数据看板与运维工具统一收纳于一个可自托管的门户中。

本项目主要解决以下问题：团队内部常因资源链接散落在聊天记录、邮件或浏览器收藏夹中，导致新成员上手慢、文档查找耗时、重要监控入口被遗忘。NovaIndex 通过纯静态 Markdown 驱动的索引页生成机制，将资源按业务域、技术层级或使用频次进行归类，并结合自动化构建流程，确保资源列表始终与项目迭代同步。目标用户包括技术负责人、运维工程师、技术文档撰写者以及任何希望建立有序外链资产体系的中小型团队。

## 功能概览

- **多层级资源分类**：支持按技术领域、项目阶段或数据粒度自定义分类层级，每个分类下可挂载任意数量的外链，并自动生成层级锚点导航。

- **链接元数据标注**：每条资源可附带说明文本、维护责任人、最后检查日期与状态标签（如稳定、待废弃、外部依赖），便于后续批量审计。

- **版本化索引管理**：基于 Git 存储资源清单，每次增删改均产生可追溯的提交记录，支持回滚、分支管理与变更差异对比。

- **自动健康检查**：内置可配置的链接可达性探测工具（基于 HTTP HEAD 请求），每日定时检测失效链接并生成报告，标记异常状态。

- **快速模糊检索**：提供客户端实时搜索功能，支持按链接标题、描述关键词或分类路径进行即时过滤，无需刷新页面。

- **响应式展示布局**：索引页在桌面端采用多栏网格结构，移动端自适应为单栏列表，确保各类终端下的阅读体验一致。

- **开放数据导出**：支持将资源列表导出为 JSON、CSV 或 YAML 格式，便于接入其他监控系统、文档生成工具或自动化脚本。

## 应用场景

- **微服务架构下的服务发现辅助**：团队维护多个微服务仓库时，可将各服务的 Swagger 文档、健康检查端点、日志查询界面与监控面板集中收录，形成服务地图，减少排障时的入口查找时间。

- **数据中台的指标字典门户**：数据团队可将各类数据源连接串、BI 看板地址、数据质量报告链接以及调度任务状态页整合为单一入口，业务方无需逐一询问即可自助获取所需数据资源。

- **开源项目外部依赖索引**：开源项目维护者可将依赖的第三方库主页、API 参考、社区讨论区与问题跟踪器统一列出，降低贡献者查阅外部资料的认知负担。

- **技术培训与新员工入职指引**：将内部代码规范、CI/CD 流水线视图、环境配置手册与常用运维工具入口整理为结构化清单，作为新员工第一周的必读导航页。

## 快速开始

以下命令演示了从克隆仓库到启动本地开发服务器的完整流程。请确保系统已安装 Git 与 Node.js 18.x 及以上版本。

```bash
# 克隆项目仓库
git clone https://github.com/novaindex/novaindex-core.git

# 进入项目根目录
cd novaindex-core

# 安装依赖（使用 npm 或 yarn）
npm install

# 启动开发模式，默认监听 localhost:3000
npm run dev
```

执行上述步骤后，打开浏览器访问 <code>http://localhost:3000</code> 即可预览资源索引页面。如需构建生产静态文件，请运行 <code>npm run build</code>，输出目录为 <code>dist/</code>。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Node.js | 18.x 或 20.x LTS | 运行时环境，用于执行构建脚本与开发服务器 |
| npm | 9.x 或以上 | 包管理器，用于安装项目依赖 |
| Git | 2.30 或以上 | 版本控制工具，用于克隆仓库与提交变更 |
| 操作系统 | Linux (x86_64) / macOS 12+ / Windows 11 | 开发与部署均可，生产环境推荐 Linux |
| 内存 | 最低 512 MB，推荐 1 GB | 构建过程内存占用，大型资源列表（万条以上）需增加 |
| 磁盘空间 | 200 MB 可用空间 | 包含源代码、依赖及构建产物 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 入门指南 | <code>docs/getting-started.md</code> | 如何在一分钟内完成初始配置并生成第一个资源索引页？ |
| 配置参考 | <code>docs/configuration.md</code> | 分类模板、元数据字段与健康检查参数如何自定义？ |
| 维护流程 | <code>docs/maintenance.md</code> | 批量添加链接、更新状态标签以及处理失效链接的推荐工作流是什么？ |
| API 接口 | <code>docs/api/export.md</code> | 如何通过 HTTP 接口获取 JSON 格式的资源全量数据以供外部系统调用？ |

## 资源列表

### 赛事数据域

<code>hejiasheshoubang.asia</code>

<code>hejiasaicheng.asia</code>

<code>hejiaqianzhan.asia</code>

<code>hejialiansai.asia</code>

<code>hejiajishibifen.asia</code>

<code>hejiajifenbang.asia</code>

<code>hejiafenxi.asia</code>

## 项目结构

```text
novaindex-core/
├── src/                           # 主要源代码目录
│   ├── core/                      # 核心逻辑模块
│   │   ├── parser.js              # 资源清单解析器，将 YAML 转为内部数据模型
│   │   └── validator.js           # 链接格式与元数据完整性校验
│   ├── generators/                # 输出生成器
│   │   ├── html-renderer.js       # 将数据模型渲染为静态 HTML 索引页
│   │   └── json-exporter.js       # 导出 JSON 格式数据供外部使用
│   ├── checker/                   # 链接健康检查子模块
│   │   ├── http-client.js         # 封装 HTTP HEAD 请求与超时重试
│   │   └── reporter.js            # 生成检查报告（控制台输出 + 文件日志）
│   └── cli/                       # 命令行入口
│       ├── index.js               # CLI 主程序，解析参数并调度各模块
│       └── commands/              # 子命令实现（build, check, export）
├── config/                        # 配置文件目录
│   ├── default.yaml               # 默认分类模板与检查间隔参数
│   └── schema.json                # 资源清单的 JSON Schema 校验定义
├── data/                          # 用户资源数据存放目录（git 管理）
│   ├── resources.yaml             # 主资源清单文件，按分类组织链接
│   └── metadata/                  # 每条链接的附加元数据（可选）
├── docs/                          # 用户文档
│   ├── getting-started.md
│   ├── configuration.md
│   └── maintenance.md
├── tests/                         # 单元测试与集成测试
│   ├── unit/
│   └── integration/
├── package.json                   # npm 项目配置与脚本定义
├── README.md                      # 项目说明文档（本文件）
└── LICENSE                        # MIT 许可证文本
```

## 贡献指南

1. 在 <code>data/resources.yaml</code> 中按照已有格式添加或修改资源条目，确保每条链接包含标题、URL 与分类路径，并运行 <code>npm run validate</code> 校验格式正确性。

2. 若新增分类层级，请同步更新 <code>config/default.yaml</code> 中的分类定义，并执行 <code>npm run build</code> 验证生成页面是否正常展示新分类。

3. 提交变更前，请运行 <code>npm run check</code> 对所有已收录链接进行可达性检测，确认无新增失效链接（若确有失效链接，需在提交说明中标注原因）。

4. 发起 Pull Request 时，请填写变更摘要模板，说明本次增删改的资源数量与涉及分类，并附上本地构建成功截图或日志。

5. 如需修改核心解析逻辑或模板渲染引擎，请补充对应的单元测试至 <code>tests/</code> 目录，确保原有功能不受回归影响。

## 常见问题

**问：资源清单中的链接数量达到上千条后，构建速度是否显著下降？**

答：默认配置下，构建过程主要消耗在 HTML 渲染与 JSON 序列化阶段。对于 5000 条以内的资源，构建时间通常不超过 3 秒。若条目超过一万，建议启用分页生成模式（配置 <code>pagination.enable: true</code>），每页 200 条，同时将健康检查独立为定时任务而非构建时执行。

**问：如何自定义索引页面的品牌标识与配色主题？**

答：项目采用 CSS 变量进行主题定制。您可以在 <code>src/assets/theme.css</code> 中覆盖 <code>--primary-color</code>、<code>--header-bg</code> 等变量。如需替换顶部 Logo，请将图片放入 <code>public/</code> 目录并修改 <code>src/generators/html-renderer.js</code> 中的 <code>logoPath</code> 配置项。

**问：能否将资源列表私有化，仅允许内网访问？**

答：NovaIndex 生成的是纯静态 HTML 文件，无内置鉴权机制。您可将其部署于内网 Web 服务器（如 Nginx 或 Apache），并配合基本身份验证（Basic Auth）或 VPN 限制访问范围。项目本身不存储任何敏感凭证，所有链接均为公开可访问的外部地址。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:13
