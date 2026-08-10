# Nexus Index

Nexus Index 是一个面向技术社区与终端用户的开源外链资源整理与导航系统。项目定位于解决信息分散、资源链接失效快、可信来源难以甄别等问题，通过结构化索引与周期性检测机制，为开发者、内容创作者及普通用户提供稳定、可验证的公开资源入口。Nexus Index 本身不存储或分发任何第三方内容，仅作为公开超链接的元数据聚合与可用性验证工具，适用于个人部署、团队内部分享或公益性质的导航站点搭建。

项目目标用户包括：需要频繁检索特定领域公开资源的开发者、希望减少重复搜索行为的运维人员、以及搭建轻量级导航站点的站长。Nexus Index 通过标准化的链接描述字段、状态标记与分类标签，帮助用户在短时间内定位到目标资源，同时降低因链接失效或域名变更带来的信息获取成本。

## 功能概览

- **公开链接聚合管理**：支持手动录入与批量导入公开超链接，每条记录包含标题、描述、类别、添加时间与最后检测状态。
- **链接可用性周期性检测**：内置定时任务模块，可对已收录链接发起 HEAD 请求，标记异常状态并生成报告。
- **多级分类与标签体系**：允许用户自定义分类层级与标签，便于按主题、语种、地区或文件类型筛选资源。
- **全文本字段检索**：支持按标题、描述、域名关键词快速检索已收录链接，返回高亮匹配结果。
- **链接状态历史记录**：保留每次检测的状态快照，支持查看单个链接的可用性变化曲线，辅助判断源站稳定性。
- **数据导入导出**：支持 JSON 与 CSV 格式的链接数据导入导出，便于迁移、备份或与其他工具集成。
- **用户可配置检测策略**：允许调整检测超时时间、重试次数、检测频率白名单等参数，适应不同网络环境。
- **只读前端展示模板**：提供一套轻量级只读展示页面，可直接部署为静态导航站，无需依赖后端渲染。

## 应用场景

- **个人知识管理辅助**：研究者或工程师可将日常积累的文档站、工具站、数据源链接统一收录，通过 Nexus Index 快速检索与状态监控，避免书签栏杂乱无章。
- **团队内部资源导航**：中小型开发团队可使用 Nexus Index 搭建内部公共链接库，集中存放开发文档、设计规范、测试环境入口等，减少新人上手时的信息询问成本。
- **公益或社区导航站点**：非盈利组织或兴趣社区可基于 Nexus Index 构建垂直领域的资源导航页，例如学术数据库、开源镜像站、本地便民服务入口，并利用可用性检测功能定期清理失效链接。
- **运维监控辅助工具**：运维人员可将 Nexus Index 作为辅助监控面板，集中收录业务依赖的外部 API 文档、状态页、镜像源地址，通过检测日志快速发现外部服务异常。

## 快速开始

以下步骤适用于首次部署 Nexus Index 实例。默认使用 SQLite 作为数据存储，无需额外安装数据库服务。

```bash
# 克隆项目仓库
git clone https://github.com/nexus-index/core.git nexus-index

# 进入项目目录
cd nexus-index

# 安装 Python 依赖（推荐使用虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 初始化数据库表结构与基础配置
python manage.py initdb
python manage.py migrate

# 以开发模式启动内置 Web 服务
python manage.py runserver --host 0.0.0.0 --port 8080
```

启动后，访问 `http://localhost:8080` 可进入管理控制台，默认管理员账号为 `admin`，初始密码在首次启动时由控制台输出。生产环境部署建议参考 `docs/deployment.md` 文档配置 WSGI 服务与反向代理。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.9 - 3.11 | 核心运行环境，推荐使用 3.10 稳定版 |
| SQLite | 3.31 及以上 | 内置数据库，用于存储链接数据与检测记录 |
| requests | 2.28.0 及以上 | 用于发起链接可用性检测 HTTP 请求 |
| jinja2 | 3.1.0 及以上 | 模板渲染引擎，用于生成管理界面与展示页面 |
| click | 8.1.0 及以上 | 命令行交互框架，提供 manage.py 子命令支持 |
| pytest | 7.2.0 及以上 | 仅开发与测试环境需要，用于运行单元测试 |
| gunicorn | 20.1.0 及以上 | 生产环境推荐部署的 WSGI 服务器 |
| python-dotenv | 0.21.0 及以上 | 用于加载 `.env` 配置文件中的环境变量 |

## 文档导航

| 层面 | 目录 / 文档 | 回答的问题 |
|------|-------------|------------|
| 用户手册 | `docs/user-guide/quick-start.md` | 如何安装、登录、添加第一条链接与进行首次检测 |
| 用户手册 | `docs/user-guide/link-management.md` | 如何批量导入链接、编辑分类、设置检测策略 |
| 运维参考 | `docs/ops/deployment.md` | 如何配置 HTTPS 反向代理、系统服务与日志轮转 |
| 运维参考 | `docs/ops/health-check.md` | 如何解读链接检测状态码、配置告警通知阈值 |
| 开发指南 | `docs/dev/architecture.md` | 项目整体模块划分、数据库表设计、扩展点说明 |
| 开发指南 | `docs/dev/api.md` | 内部 RESTful API 规范与示例，用于二次开发或集成 |
| 贡献指南 | `CONTRIBUTING.md` | 代码风格、提交规范、测试要求与 PR 流程详情 |

## 资源列表

本列表收录项目相关或项目构建过程中涉及的全部公开资源链接，按类别分组展示。所有链接严格保留用户提供的原始格式。

基础资源入口

- <code>guochanyirenwang.org.cn</code>
- <code>zhongwenzimuzaixianguankan.org.cn</code>
- <code>dianyingtiantangzaixianbofang.org.cn</code>
- <code>mianfeidongmandaquan.org.cn</code>
- <code>zuixindianshijuzaixianguankan.org.cn</code>
- <code>mianfeizhuijuwangzhan.org.cn</code>
- <code>dongmanzaixianguankanquanji.org.cn</code>

## 项目结构

项目根目录下各主要子目录与核心文件说明如下。带有注释的行描述了该目录或文件的核心职责。

```
nexus-index/
├── manage.py                 # 命令行入口，包含 initdb、migrate、runserver 等子命令
├── requirements.txt          # 生产环境与开发环境统一的 pip 依赖清单
├── .env.example              # 环境变量配置模板，含 SECRET_KEY、检测超时等参数
├── nexus/
│   ├── __init__.py           # 主应用工厂函数，负责创建 Flask / FastAPI 实例
│   ├── settings.py           # 配置类定义，支持开发、测试、生产三套配置
│   ├── models.py             # SQLAlchemy / Peewee ORM 模型定义，包含 Link、CheckRecord、Category
│   ├── schemas.py            # Pydantic / Marshmallow 序列化与校验模式
│   ├── routes/               # 路由层，按功能拆分 api_v1、admin、frontend
│   │   ├── __init__.py
│   │   ├── api_v1.py         # 对外 RESTful 接口，支持增删改查与批量操作
│   │   ├── admin.py          # 管理后台路由，提供链接管理、检测触发、配置编辑
│   │   └── frontend.py       # 只读展示页路由，供外部访客浏览公开链接列表
│   ├── services/             # 业务逻辑层，封装链接检、分类合并、数据导入导出
│   │   ├── checker.py        # 异步或定时链接检测逻辑，含重试与超时控制
│   │   ├── importer.py       # JSON/CSV 解析与数据批量写入
│   │   └── stats.py          # 链接状态统计、分类计数、检测报告生成
│   ├── templates/            # Jinja2 模板文件，用于管理端与前端展示
│   │   ├── base.html
│   │   ├── dashboard.html
│   │   ├── link_list.html
│   │   └── link_detail.html
│   ├── static/               # CSS、JavaScript、图标资源，提供基础 UI 样式
│   │   ├── css/
│   │   ├── js/
│   │   └── images/
│   └── utils/                # 通用工具函数，包括日志初始化、时间格式化、域名解析辅助
│       ├── logger.py
│       ├── validators.py
│       └── network.py
├── tests/                    # 单元测试与集成测试用例，覆盖模型、路由、检测服务
│   ├── conftest.py
│   ├── test_models.py
│   ├── test_checker.py
│   └── test_api.py
├── docs/                     # 完整文档目录，包括用户手册、运维参考与开发指南
│   ├── user-guide/
│   ├── ops/
│   └── dev/
└── scripts/                  # 辅助脚本，如批量初始化数据、迁移历史数据、生成检测报告
    ├── seed_data.py
    └── export_report.py
```

## 贡献指南

Nexus Index 遵循开源社区协作规范，欢迎各类形式的贡献，包括但不限于代码、文档、翻译与问题反馈。

1. 在 GitHub 上 Fork 本仓库，并克隆到本地开发环境。确保使用 Python 3.10 虚拟环境，并安装开发依赖组 `pip install -r requirements-dev.txt`。
2. 新建功能分支或修复分支，命名规范为 `feature/xxx` 或 `fix/xxx`。所有代码变更需保持与现有模块风格一致，并补充必要的单元测试覆盖。
3. 提交代码前运行完整测试套件 `pytest tests/`，确保无回归性错误。同时使用 `flake8` 和 `black` 进行代码格式检查与自动格式化。
4. 编写或更新对应的文档说明，包括但不限于配置变更、新增接口、使用示例。文档需放置在 `docs/` 目录下对应子目录中。
5. 发起 Pull Request 到主仓库的 `main` 分支，并在 PR 描述中明确说明变更动机、影响范围及测试情况。PR 需获得至少一位维护者审阅后方可合并。

## 常见问题

**问：链接检测功能是否会频繁触发源站限流或被视为恶意行为？**

答：Nexus Index 默认检测策略为每个链接每 24 小时仅发起一次 HEAD 请求，且不下载响应体。用户可在配置中调整检测间隔为更长周期，或设置检测白名单以排除特定域名。检测请求携带标准 User-Agent 头，标识为 Nexus Index 可用性监测，源站可通过该标识进行过滤。项目方建议用户遵守目标站点的 robots.txt 规则，并对非公开或敏感链接手动关闭检测。

**问：我能否将 Nexus Index 部署为纯静态导航站点，而不使用后端检测服务？**

答：可以。Nexus Index 支持导出当前链接数据为静态 JSON 文件，并利用内置的 `frontend` 模板生成只读 HTML 页面。用户可运行 `python manage.py export --static` 命令，将输出目录内容直接部署到任何支持静态文件托管的服务上。此时链接可用性检测功能将不可用，但所有链接数据与分类信息完整保留。

**问：数据库中的链接数据如何备份与恢复？**

答：若使用默认 SQLite 数据库，直接复制项目根目录下的 `nexus.db` 文件即可完成全量备份。恢复时停止服务，将备份文件覆盖回原路径并重启即可。若使用 PostgreSQL 或 MySQL，推荐使用对应数据库的原生 pg_dump 或 mysqldump 工具。项目同时提供 `python manage.py export --format csv` 命令，可将链接与分类数据导出为独立 CSV 文件，作为冗余备份手段。

## 许可证

Nexus Index 采用 MIT 许可证。完整许可证文本请参见项目根目录下的 LICENSE 文件。该许可证允许任何个人或组织自由使用、修改、分发本软件，包括用于商业目的，但需保留原始版权声明与许可声明。本软件按“现状”提供，不附带任何明示或暗示的担保，包括但不限于适销性、特定用途适用性及非侵权担保。详情请查阅 MIT 许可证全文。

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
