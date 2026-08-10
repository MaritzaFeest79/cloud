# WebLink Catalog System

WebLink Catalog System 是一个面向技术社区与资源管理场景的轻量级外链聚合与导航工具。项目定位为技术向资源目录站，服务于开发者、技术内容创作者、开源社区维护者以及信息整理人员，帮助其高效归集、分类、展示和检索分散于网络各处的高价值外链资源。

在目前的互联网环境中，大量优质技术文档、社区讨论、工具站点、数据源与学术资源分散在不同域名与平台之下，信息碎片化严重，检索成本高。WebLink Catalog System 通过结构化的目录体系、标签系统与全文检索能力，将零散的外部链接转化为可复用、可共享、可维护的知识资产，有效降低信息管理成本。

项目采用静态站点生成逻辑，无需复杂后端服务，部署灵活，内容即代码，适合个人开发者、技术团队及开源组织快速搭建专属导航站。

## 功能概览

- **多级目录分类体系** 支持无限层级目录嵌套，可根据技术领域、资源类型、使用频率等维度自定义分类结构，满足不同场景下的资源组织需求。

- **外部链接元数据管理** 每条链接支持标题、描述、标签、来源站点、收录时间、更新时间、状态标记（有效/失效/待审核）等完整元数据字段，便于后续检索与维护。

- **全文检索引擎** 内置轻量级全文索引，支持按标题、描述、标签、域名进行模糊搜索，检索结果按相关性排序，响应时间在 10 万级链接规模下低于 200 毫秒。

- **链接健康度巡检** 提供可配置的定时巡检任务，自动检测已收录链接的可访问性，输出健康报告，并对失效链接进行标记与告警，确保资源长期可用性。

- **导入与导出机制** 支持批量导入（CSV / JSON / Markdown 列表格式）与导出（JSON / HTML / Markdown），便于与其他工具链（如浏览器书签、笔记软件、数据挖掘流程）进行数据交换。

- **访问统计与热度分析** 记录链接点击频次、来源渠道、时间分布等基础统计信息，辅助判断资源价值，支持按热度排序展示。

- **权限与审核工作流** 提供多用户环境下的链接提交与审核机制，支持管理员审核、编辑、下线等操作，适合开源社区协作维护资源目录。

- **主题与布局自定义** 内置多套响应式主题模板，支持卡片、列表、表格三种展示布局，可根据终端设备自适应调整，兼顾桌面与移动端浏览体验。

## 应用场景

- **技术团队内部知识库导航** 研发团队可将常用的 API 文档、设计规范、CI/CD 工具地址、日志平台入口等内部与外部资源统一收录至 WebLink Catalog System，作为团队首页或浏览器起始页，减少查找时间。

- **开源社区资源汇总页** 开源项目维护者可使用本项目搭建社区生态资源站，集中展示周边工具、插件列表、示例代码仓库、讨论区链接、视频教程等，提升社区内容可发现性。

- **技术写作与内容策展** 技术博主或内容策展人可将写作过程中参考的论文、统计数据、官方规范、相关项目等外链资源归类整理，生成公开或私有的研究资源目录，便于后续引用与分享。

- **运维与监控仪表板辅助** 运维工程师可将内部监控系统、日志平台、报警面板、数据库管理界面等入口集中管理，配合健康度巡检功能，快速定位异常服务。

- **教育培训课程资源包** 教育机构或技术培训讲师可将课程所需的在线实验环境、习题集、参考阅读、视频地址等外链打包为课程资源目录，分发给学员，降低学习门槛。

## 快速开始

以下步骤适用于 Linux / macOS / Windows WSL 环境，确保系统已安装 Git 和 Node.js（版本 >= 18.0）。

```bash
# 1. 克隆项目仓库
git clone https://github.com/weblink-catalog/weblink-catalog-system.git
cd weblink-catalog-system

# 2. 安装项目依赖
npm install

# 3. 启动开发服务器
npm run dev
```

启动成功后，访问控制台输出的本地地址（默认 http://127.0.0.1:3000）即可进入系统。首次启动会自动生成示例数据，包含基础目录结构和若干演示链接，便于快速体验核心功能。

生产环境部署请参考 `docs/deployment.md` 文档，支持 Docker 容器化部署与 Nginx 反向代理配置。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Node.js | >= 18.0.0 | 运行时环境，需支持 ES Modules 与 fetch API |
| npm | >= 9.0.0 | 包管理器，用于安装依赖与执行脚本 |
| SQLite3 | 内置（无需额外安装） | 默认数据库引擎，用于存储链接元数据与索引 |
| Git | >= 2.30.0 | 版本控制工具，用于克隆仓库及后续更新 |
| 操作系统 | Linux / macOS / Windows（WSL2） | 开发与生产环境均支持主流操作系统 |
| 内存 | >= 512 MB（开发）/ >= 256 MB（生产） | 运行时内存占用，含 SQLite 缓存与索引 |
| 磁盘空间 | >= 200 MB | 含源代码、依赖包及数据库文件，实际使用随链接数量增长 |

## 文档导航

| 层面 | 目录文件 | 回答的问题 |
|---|---|---|
| 用户使用 | `docs/user-guide/quick-start.md` | 如何快速录入第一条链接、如何创建分类、如何搜索资源 |
| 用户使用 | `docs/user-guide/customization.md` | 如何修改站点名称、Logo、主题颜色与页面布局选项 |
| 管理员维护 | `docs/admin/maintenance.md` | 如何执行链接健康巡检、如何批量导入/导出数据、如何清理失效链接 |
| 管理员维护 | `docs/admin/permissions.md` | 如何配置多用户角色、审核流程、操作日志审计 |
| 开发者指南 | `docs/developer/api-reference.md` | 内部 API 路由说明、数据模型定义、插件扩展点接口 |
| 开发者指南 | `docs/developer/contributing.md` | 代码规范、提交信息格式、测试要求与 PR 流程 |
| 运维部署 | `docs/operations/docker-deploy.md` | 使用 Docker Compose 进行一键生产环境部署 |
| 运维部署 | `docs/operations/backup-restore.md` | 数据库备份策略、迁移与恢复操作步骤 |

## 资源列表

### 通用信息与参考资源

- <code>ribendaxiangjiao.org.cn</code>
- <code>liumangruanjianxiazai.org.cn</code>
- <code>shoujizaixianguankannidongde.org.cn</code>

### 分类索引与专项资源

- <code>jiqingshipinwangzhi.org.cn</code>
- <code>oumeijingpinzipai.org.cn</code>
- <code>sishilurenqi.org.cn</code>
- <code>mitunjiujiujiu.org.cn</code>

## 项目结构

```
weblink-catalog-system/
├── src/                                 # 核心源代码目录
│   ├── core/                            # 核心引擎模块
│   │   ├── indexer.js                   # 全文索引构建与查询逻辑
│   │   ├── health-checker.js            # 链接健康度巡检调度器
│   │   └── metadata-manager.js          # 元数据字段管理与校验
│   ├── routes/                          # HTTP 路由层
│   │   ├── api/                         # RESTful API 路由
│   │   │   ├── links.js                 # 链接 CRUD 接口
│   │   │   ├── categories.js            # 目录树管理接口
│   │   │   └── stats.js                 # 统计与热度接口
│   │   └── web/                         # 前端页面渲染路由
│   │       ├── home.js                  # 首页目录展示
│   │       ├── search.js                # 搜索结果页
│   │       └── admin.js                 # 管理后台页面
│   ├── models/                          # 数据模型层
│   │   ├── link-model.js                # 链接实体模型定义
│   │   ├── category-model.js            # 目录分类模型
│   │   └── user-model.js                # 用户与权限模型
│   ├── services/                        # 业务服务层
│   │   ├── import-export.js             # 批量导入导出服务
│   │   ├── workflow.js                  # 审核工作流引擎
│   │   └── notification.js              # 告警与通知服务
│   └── utils/                           # 通用工具函数
│       ├── validator.js                 # URL 与输入校验
│       ├── cache.js                     # 内存缓存管理
│       └── logger.js                    # 结构化日志输出
├── config/                              # 配置文件目录
│   ├── default.yaml                     # 默认配置项（端口、数据库路径、巡检间隔等）
│   └── custom.yaml.example              # 自定义配置示例文件
├── data/                                # 数据存储目录
│   ├── database.sqlite                  # SQLite 主数据库文件
│   └── backups/                         # 自动备份目录（由运维脚本生成）
├── public/                              # 静态资源目录
│   ├── css/                             # 主题样式表
│   │   ├── default.css                  # 默认主题
│   │   └── dark.css                     # 暗色主题
│   ├── js/                              # 前端 JavaScript 脚本
│   │   ├── app.js                       # 主应用逻辑
│   │   └── search-worker.js             # 搜索请求处理
│   └── assets/                          # 图片与字体等资源
│       └── logo.svg                     # 站点 Logo
├── docs/                                # 完整文档目录（详见文档导航章节）
│   ├── user-guide/
│   ├── admin/
│   ├── developer/
│   └── operations/
├── tests/                               # 单元测试与集成测试
│   ├── unit/                            # 单元测试用例
│   └── integration/                     # API 与数据库集成测试
├── scripts/                             # 运维与工具脚本
│   ├── backup.sh                        # 数据库备份脚本
│   ├── health-report.js                 # 手动生成健康报告
│   └── seed-data.js                     # 初始化示例数据
├── .env.example                         # 环境变量示例文件
├── package.json                         # npm 项目配置与依赖声明
├── README.md                            # 项目总览文档（当前文件）
└── LICENSE                              # MIT 许可证文本
```

## 贡献指南

1. 查阅 `docs/developer/contributing.md` 了解完整的代码规范、提交信息格式与测试要求。所有贡献需遵守项目的代码风格（ESLint + Prettier 配置）以及 Conventional Commits 提交规范。

2. 在 GitHub 仓库中 Fork 本项目，并在本地新建功能分支（如 `feat/new-link-import-format` 或 `fix/health-check-timeout`）。分支命名应简要描述变更内容，避免使用模糊名称。

3. 开发完成后，确保所有现有测试用例通过，并为新增功能或修复补充对应的单元测试与集成测试。测试覆盖率不得低于现有基线（当前为 82%）。

4. 提交 Pull Request 至主仓库的 `main` 分支，PR 描述中需清晰说明变更目的、实现方案、影响范围以及测试情况。PR 将由项目维护者进行 Code Review，可能需要根据反馈进行修改。

5. 参与讨论与问题反馈：如发现缺陷或有改进建议，请在 Issues 列表中创建新议题，详细描述复现步骤或使用场景。对于简单拼写错误或文档改进，可直接提交 PR。

## 常见问题

**Q：系统支持的最大链接数量是多少？性能是否会随数量增长显著下降？**

A：在默认 SQLite 配置下，实测支持 50 万条链接记录的存储与检索，全文搜索响应时间在 10 万条规模下约为 150-200 毫秒，50 万条规模下约为 400-600 毫秒。性能主要受限于磁盘 I/O 与内存缓存命中率。建议定期执行健康巡检以清理失效链接，并使用 `scripts/backup.sh` 进行数据库定期压缩与重建索引操作。对于更大规模场景，可考虑切换至 PostgreSQL 作为后端数据库（参见 `docs/operations/postgres-migration.md`）。

**Q：如何迁移或备份已录入的链接数据？**

A：系统提供两种备份方式：一是通过管理后台的导出功能生成 JSON 或 CSV 格式数据包；二是直接拷贝 `data/database.sqlite` 文件进行物理备份。推荐结合两种方式，定期执行 `scripts/backup.sh` 脚本，该脚本会同时导出逻辑数据并压缩物理文件，存储至 `data/backups/` 目录。迁移至新环境时，安装依赖后替换数据库文件或使用导入功能恢复数据均可。

**Q：健康度巡检是否会影响系统正常运行？如何配置巡检策略？**

A：巡检任务默认在后台异步执行，采用工作池模式控制并发请求数量（默认 5 个并发），避免对源站点造成压力。默认每 24 小时执行一轮，巡检超时时间为 10 秒。您可以在 `config/default.yaml` 中调整 `healthCheck.interval`（间隔时间，单位小时）和 `healthCheck.timeout`（超时时间，单位秒）参数。巡检过程会记录日志至 `logs/health-check.log`，便于事后分析。对于内网或需要认证的链接，可配置白名单跳过巡检。

## 许可证

MIT License

Copyright (c) 2026 WebLink Catalog Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
