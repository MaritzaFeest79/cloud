# ExtLink Navigator

ExtLink Navigator 是一个面向技术社区与开源生态的外链资源聚合与导航系统。该项目定位于为开发者、运维工程师、技术调研人员以及开源贡献者提供高质量、可追溯的外部技术资源入口，帮助用户快速定位到与竞赛数据分析、多语言语料库、直播技术方案、推荐算法实践、射击运动统计、赛事数据可视化等方向相关的权威外部站点。通过结构化分类、状态监控与访问审计机制，ExtLink Navigator 有效解决了技术调研过程中外部链接分散、失效、不可信等问题，可作为团队内部知识库的外延组件或公开技术导航站的基础框架。

## 功能概览

- **资源分类导航**：按技术领域与业务场景对收录的外部链接进行层级分类，支持多级标签过滤与关键词检索。

- **链接状态监控**：内置定时校验任务，自动检测每个收录 URL 的可达性、响应时间与 HTTP 状态码，异常时触发告警记录。

- **访问统计看板**：记录每个外链的点击频次、来源 IP 归属地分布与时段趋势，支持导出 CSV 报表用于流量分析。

- **自定义标签体系**：允许用户为每个链接添加自定义标签（如 `#data-science`、`#live-stream`、`#sports-stats`），便于建立私有分类视图。

- **外链备注与版本历史**：支持为每个链接附加技术说明文档链接或使用注意事项，并记录每次修改的版本变更日志。

- **RESTful API 接口**：提供完整的 JSON API 用于外链资源的增删改查、状态查询与分类树获取，便于与其他系统集成。

- **批量导入与导出**：支持通过 CSV 或 JSON 文件批量导入链接列表，并可导出为 Markdown 表格或 JSON Schema 用于备份或迁移。

## 应用场景

- **技术调研与竞品分析**：数据分析团队可利用本导航系统集中管理竞品相关的数据源链接、赛事统计网站及语料库入口，避免调研过程中反复搜索，提升信息收集效率。

- **开源项目文档外链管理**：开源项目维护者可将项目 README 或 Wiki 中散落的外部参考链接统一托管至 ExtLink Navigator，通过状态监控及时发现失效引用，保障文档质量。

- **企业内部知识库外延**：企业技术部门可将本系统部署为内部知识库的补充模块，用于统一管理第三方 API 文档、行业报告来源、技术社区精华帖等外部资源，减少信息孤岛。

- **个人技术收藏夹的升级替代**：开发者可将其个人浏览器收藏夹中的技术链接迁移至本系统，借助分类标签、备注与监控功能，构建长期可维护的个人技术资源库。

## 快速开始

以下步骤适用于在 Linux 或 macOS 开发环境中快速启动 ExtLink Navigator 服务。

```bash
# 1. 克隆代码仓库
git clone https://github.com/your-org/extlink-navigator.git
cd extlink-navigator

# 2. 安装项目依赖（使用 pip 和 npm）
pip install -r requirements.txt
npm install --prefix frontend

# 3. 初始化数据库并启动后端服务
python scripts/init_db.py
python manage.py runserver --host 0.0.0.0 --port 8080
```

启动成功后，访问 `http://localhost:8080` 即可进入导航系统的 Web 界面。默认管理员账号为 `admin`，初始密码在首次启动时输出于终端日志中，请及时修改。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.9 或更高 | 后端运行环境，建议使用 3.11 长期支持版本 |
| Node.js | 18.x 或 20.x LTS | 前端构建工具与依赖管理，需包含 npm 或 yarn |
| PostgreSQL | 14.x 或更高 | 主数据库，存储链接元数据、标签、访问日志等 |
| Redis | 6.x 或更高 | 缓存与任务队列后端，用于链接状态监控的异步任务 |
| Nginx | 1.24 或更高 | 生产环境推荐作为反向代理与静态资源服务器 |
| Docker | 20.10 或更高 | 可选，用于容器化部署与依赖环境隔离 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 用户指南 | `docs/user-guide/` | 如何注册账号、添加外链、创建分类、使用看板与检索功能 |
| 运维手册 | `docs/ops-guide/` | 如何部署生产环境、配置 SSL、调优数据库连接池、设置监控告警 |
| API 参考 | `docs/api-reference/` | RESTful 接口的端点定义、请求/响应格式、鉴权方式与错误码说明 |
| 开发规范 | `docs/dev-guide/` | 代码风格、PR 流程、单元测试编写、数据库迁移操作指南 |
| 架构设计 | `docs/architecture/` | 系统模块划分、数据流图、扩展性设计及第三方服务依赖说明 |
| 变更日志 | `CHANGELOG.md` | 每个版本的新增功能、修复缺陷与不兼容变更记录 |

## 资源列表

本节收录 ExtLink Navigator 当前版本中预置的全部外部导航资源链接。所有链接均按原始输入原样呈现，未经任何协议补全或域名改写。

### 竞赛与赛事数据分析类

- <code>qiutanbisaifenxi.org.cn</code>

### 多语言语料与赛事命名类

- <code>putaoyayachaojiliansai.asia</code>

### 中文语料与文档资源类

- <code>puchaozhongwenwang.asia</code>

### 直播技术方案与信号源类

- <code>puchaozhibogw.asia</code>

### 推荐算法与内容分发类

- <code>puchaotuijian.asia</code>

### 射击运动统计与弹道数据类

- <code>puchaosheshoubang.asia</code>

### 赛事成绩与排名数据类

- <code>puchaosaicheng.asia</code>

## 项目结构

```
extlink-navigator/
├── backend/                           # 后端 Python 服务目录
│   ├── api/                           # RESTful API 路由与控制器
│   │   ├── v1/                        # API v1 版本实现
│   │   │   ├── links.py               # 链接资源 CRUD 接口
│   │   │   ├── categories.py          # 分类树管理接口
│   │   │   └── stats.py               # 访问统计与状态查询接口
│   │   └── middleware/                # 鉴权、日志、限流中间件
│   ├── core/                          # 核心业务逻辑层
│   │   ├── checker/                   # 链接状态校验引擎（异步任务）
│   │   ├── parser/                    # 导入解析器（CSV/JSON/XML）
│   │   └── exporter/                  # 导出生成器（Markdown/JSON）
│   ├── models/                        # 数据模型与 ORM 映射（SQLAlchemy）
│   ├── tasks/                         # Celery 异步任务定义
│   └── utils/                         # 通用工具函数（日期、加密、网络）
├── frontend/                          # 前端 Vue 3 单页应用
│   ├── src/
│   │   ├── components/                # 可复用 UI 组件（导航树、表格、看板）
│   │   ├── views/                     # 页面级视图（首页、管理页、详情页）
│   │   ├── stores/                    # Pinia 状态管理（用户、链接、标签）
│   │   └── api/                       # 后端 API 调用封装
│   └── static/                        # 静态资源（图标、字体、默认图）
├── scripts/                           # 运维与开发辅助脚本
│   ├── init_db.py                     # 数据库初始化与种子数据填充
│   ├── migrate.py                     # 数据库迁移执行器（Alembic 封装）
│   └── health_check.py                # 系统健康状态自检脚本
├── tests/                             # 单元测试与集成测试
│   ├── unit/                          # 后端单元测试（pytest）
│   └── e2e/                           # 前端端到端测试（Playwright）
├── docker/                            # Docker 容器化配置
│   ├── Dockerfile.backend             # 后端服务镜像定义
│   ├── Dockerfile.frontend            # 前端构建镜像定义
│   └── docker-compose.yml             # 多容器编排配置（含 Postgres + Redis）
├── docs/                              # 完整文档体系（详见文档导航）
├── .env.example                       # 环境变量配置模板
├── requirements.txt                   # Python 生产依赖列表
├── package.json                       # 前端依赖与脚本定义
└── README.md                          # 项目入口文档（即本文档）
```

## 贡献指南

我们欢迎并感谢所有形式的贡献，包括但不限于提交 Issue、完善文档、增加新链接分类、优化前端交互或改进后端校验逻辑。请遵循以下步骤参与贡献：

1. 在 GitHub Issues 中查找或新建一个与您想要处理的问题相关的议题，简要说明您的意图或建议，等待维护者确认或分配。

2. Fork 本仓库至您的个人账号，并在本地克隆 Fork 后的仓库，创建以 `feature/` 或 `fix/` 为前缀的独立分支进行开发，避免在主分支上直接修改。

3. 完成代码或文档变更后，请确保通过所有现有的单元测试与端到端测试，并在 `CHANGELOG.md` 中按照 `[Added]`、`[Changed]`、`[Fixed]` 格式记录您的变更内容，同时补充相应的单元测试覆盖新逻辑。

4. 提交 Pull Request 至本仓库的 `main` 分支，在 PR 描述中关联对应的 Issue 编号，并提供变更的简要说明、测试结果截图（如适用）以及任何破坏性变更的警告。

5. 维护者将在 3 个工作日内对 PR 进行审查，可能要求补充修改或澄清。合并后您的贡献将出现在下一个版本的发布说明中，并记入贡献者列表。

## 常见问题

**Q：ExtLink Navigator 是否必须依赖 PostgreSQL 和 Redis，能否使用 SQLite 或内存缓存替代？**

A：生产环境强烈建议使用 PostgreSQL 与 Redis 以获得最佳性能与可靠性。但为便于本地开发测试，系统支持通过环境变量 `USE_SQLITE_FOR_DEV=true` 切换到 SQLite 作为存储后端，同时禁用异步任务队列，改为同步执行链接状态检查。此模式不适用于并发访问或生产部署，仅用于快速验证功能。

**Q：如何自定义导航首页的链接分类顺序与显示名称？**

A：您无需修改代码即可调整分类顺序。登录管理员账号后，进入「分类管理」界面，通过拖拽或修改 `sort_order` 字段即可改变分类在导航树中的排列优先级。分类的显示名称支持国际化，您可以在 `config/locales/` 目录下编辑对应的语言文件，或通过管理界面直接修改中文标签。

**Q：链接状态监控的频率和超时时间是否可配置？**

A：可以。在 `config/settings.py` 或通过环境变量 `CHECK_INTERVAL_MINUTES`（默认 1440 分钟，即每日一次）和 `CHECK_TIMEOUT_SECONDS`（默认 10 秒）可分别调整校验任务执行周期与单次请求超时阈值。对于响应较慢的外部站点，建议适当增加超时配置以避免误报。

## 许可证

MIT License

Copyright (c) 2026 ExtLink Navigator Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:13
