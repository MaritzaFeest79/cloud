# WebLink Navigator

WebLink Navigator 是一个面向技术社区的开源外链资源聚合与管理平台，专为开发者、技术博主及开源项目维护者设计，用于高效收集、分类、验证和展示外部技术资源。该平台解决了个体在维护大量书签或友情链接时面临的分散管理、链接失效、分类混乱等问题，提供了一套结构清晰、可自动化校验的资源目录生成方案。目标用户包括个人技术博客作者、开源项目文档维护者、技术资讯站点运营人员以及企业内部知识库管理者。

## 功能概览

- **多级分类资源目录**：支持按技术领域、资源类型、语言版本等维度建立无限层级的分类树，便于组织大规模链接集合。
- **自动化链接存活检测**：内置定时任务与手动触发机制，对收录的每一枚外链进行 HTTP 状态码检查，自动标记异常链接并生成报告。
- **自定义元数据模板**：允许为每个资源条目附加标题、描述、标签、维护人、添加日期、更新周期等自定义字段，满足不同场景的元数据管理需求。
- **批量导入与导出**：支持从 CSV、JSON、OPML 及浏览器书签 HTML 文件批量导入链接，同时支持导出为标准格式用于迁移或备份。
- **公开嵌入与展示组件**：提供可嵌入其他页面的小部件（Widget），以卡片、列表或表格形式展示指定分类下的资源集合，适用于项目 README 或网站侧边栏。
- **链接变更追踪日志**：记录每个外链的每一次状态变化（新增、失效、恢复、元数据修改），支持按时间轴回溯，便于审计与协作。
- **用户权限与审核工作流**：支持多用户环境，区分管理员、编辑者、普通访客角色，编辑提交的链接需经审核后方可公开显示，保障资源质量。

## 应用场景

- **开源项目文档站的外链附录**：开源项目维护者可将项目依赖的文档、工具、教程、社区论坛等外部链接通过 WebLink Navigator 统一管理，并在项目 README 或官网文档中嵌入动态更新的资源列表，避免手动维护过时链接。
- **技术博客的友情链接与推荐阅读**：技术博主可使用该平台分类整理友链、写作参考资料、推荐工具集，通过导出的静态数据或嵌入组件，在博客侧边栏或独立页面呈现有序的资源导航，提升读者体验。
- **企业内部技术知识库的资产登记**：企业架构或运维团队可将常用的内部系统地址、云平台控制台、监控面板、代码仓库、CI/CD 工具等链接统一登记，并设置负责人与校验周期，确保关键入口的可访问性。
- **技术社区或导航站的资源共建**：技术社区运营方可开放编辑权限，邀请成员共同贡献优质外链，通过审核流程控制质量，形成领域性的技术资源门户，降低新手查找优质内容的门槛。

## 快速开始

以下步骤帮助您在本地环境快速启动 WebLink Navigator 服务。

```bash
# 1. 克隆仓库
git clone https://github.com/weblink-navigator/weblink-navigator.git
cd weblink-navigator

# 2. 安装项目依赖（使用 pip 与虚拟环境）
python -m venv venv
source venv/bin/activate  # Windows 下使用 venv\Scripts\activate
pip install -r requirements.txt

# 3. 初始化数据库并创建管理员账户
python manage.py migrate
python manage.py createsuperuser

# 4. 启动开发服务器
python manage.py runserver 0.0.0.0:8000
```

访问 <code>http://localhost:8000</code> 即可进入管理后台开始添加链接资源。生产环境部署请参考后续文档导航中的运维指南。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.9 - 3.11 | 核心运行环境，推荐使用 3.10 长期支持版本 |
| PostgreSQL | 13.0 及以上 | 主数据库，用于存储链接元数据、用户信息及操作日志 |
| Redis | 6.2 及以上 | 缓存与任务队列后端，用于链接存活检测的异步任务 |
| Node.js | 16.0 及以上 | 仅用于构建前端静态资源，若使用预编译包可免去 |
| Nginx | 1.20 及以上 | 生产环境推荐的反向代理与静态文件服务器 |
| Supervisor | 4.0 及以上 | 用于进程守护，保证 Celery 工作进程及 Django 服务稳定性 |

## 文档导航

| 层面 | 目录/章节 | 回答的问题 |
|------|----------|-----------|
| 基础使用 | 用户手册 - 快速入门 | 如何添加第一条链接、创建分类、分配标签？ |
| 基础使用 | 用户手册 - 元数据配置 | 自定义字段怎么设置？导入模板格式是什么？ |
| 运维管理 | 运维指南 - 部署与配置 | 生产环境如何配置 Nginx + Gunicorn？环境变量有哪些？ |
| 运维管理 | 运维指南 - 巡检与告警 | 如何调整链接检测频率？异常通知如何接入企业微信或邮件？ |
| 开发扩展 | 开发文档 - API 接口 | 是否提供 RESTful API 供第三方调用？认证方式是什么？ |
| 开发扩展 | 开发文档 - 插件机制 | 如何开发一个自定义校验器或导出格式扩展？ |
| 社区贡献 | 贡献者指引 - 代码规范 | 提交 PR 需要遵循哪些代码风格与测试要求？ |

## 资源列表

### 体育竞技数据分析类资源

<code>qiutanzuqiubifen777.org.cn</code>

<code>qiutanzuqiubifen500.org.cn</code>

<code>qiutanzuqiubifengw.org.cn</code>

<code>qiutanzuqiubifenwz.org.cn</code>

<code>qiutanzuqiubifengf.org.cn</code>

<code>qiutanzuqiuw.net.cn</code>

<code>qiutanlanqiubifenwz.org.cn</code>

## 项目结构

```
weblink-navigator/
├── manage.py                      # Django 项目管理入口
├── requirements.txt               # Python 后端依赖清单
├── package.json                   # 前端构建工具配置
├── .env.example                   # 环境变量模板（含数据库、Redis 等）
├── docker-compose.yml             # 本地开发与测试用容器编排
│
├── backend/                       # 后端核心代码目录
│   ├── settings/                  # 多环境配置（base, dev, prod）
│   ├── apps/                      # 所有 Django 应用
│   │   ├── links/                 # 链接资源管理应用（模型、视图、序列化器）
│   │   ├── checks/                # 链接存活检测应用（Celery 任务、检测策略）
│   │   ├── users/                 # 用户与权限管理（自定义用户模型、角色）
│   │   └── widgets/               # 嵌入组件渲染引擎（卡片/列表/表格）
│   ├── core/                      # 公共核心模块（缓存、日志、异常处理）
│   └── urls.py                    # 主路由配置
│
├── frontend/                      # 前端资源（基于 Vue 3 + Vite）
│   ├── src/                       # 源码目录（组件、页面、状态管理）
│   ├── public/                    # 静态资源（favicon、robots.txt）
│   └── dist/                      # 构建输出目录（由 CI 生成，不纳入版本库）
│
├── docs/                          # 项目文档（Markdown 与 PDF 版手册）
│   ├── user_guide/                # 用户手册（分章节）
│   ├── ops_guide/                 # 运维部署指南
│   └── dev_guide/                 # 开发者文档（API 参考、插件编写）
│
├── scripts/                       # 运维与工具脚本
│   ├── init_db.py                 # 数据库初始化与测试数据填充
│   ├── check_all_links.py         # 手动触发全量链接检测
│   └── backup_metadata.py         # 元数据定时备份脚本
│
└── tests/                         # 单元测试与集成测试
    ├── test_models.py             # 数据模型测试
    ├── test_checks.py             # 链接检测逻辑测试
    └── test_api.py                # RESTful API 接口测试
```

## 贡献指南

1. 阅读项目行为准则与贡献者许可协议，确认您的内容与代码贡献遵循 MIT 许可条款，并在 Issue 列表中查找或新建与您修改方向相关的问题，确保工作不会重复。
2. 从主仓库 Fork 代码至个人账户，并将个人仓库克隆至本地，建议在开发分支（如 feature/xxx 或 fix/xxx）上进行所有修改，避免直接操作主干分支。
3. 确保本地环境通过全部现有单元测试，并在新增功能或修复缺陷时补充对应的测试用例，代码覆盖率不得低于 85%。
4. 完成代码修改后，提交清晰的 commit 信息（遵循 Conventional Commits 规范），推送到个人远程分支，然后向主仓库的 develop 分支提交 Pull Request，等待核心维护者进行代码审查与讨论。
5. 若 Pull Request 涉及文档、资源列表或配置模板的变更，请同步更新 docs 目录下的对应手册，并确保所有 URL 示例有效。

## 常见问题

**问：部署后添加的第一个链接，存活检测状态一直显示“待检测”，如何处理？**

答：请检查 Redis 服务是否正常启动，并且 Celery Worker 进程是否已正确运行。您可以通过 <code>python manage.py shell</code> 进入 Django shell，执行 <code>from checks.tasks import check_single_link; check_single_link.delay(link_id)</code> 手动触发一次检测任务，同时观察 Celery 日志输出。若任务仍未被消费，请确认 <code>CELERY_BROKER_URL</code> 环境变量配置正确。

**问：从浏览器导出的书签 HTML 文件导入时，部分链接分类丢失或错乱，是什么原因？**

答：浏览器书签导出的 HTML 结构不同浏览器间存在细微差异（尤其 Netscape 格式的注释与层级解析）。建议在导入前使用前端界面的“预览映射”功能，手动调整导入的文件夹与系统分类的对应关系。若仍存在问题，可先将书签文件转换为 CSV 格式（保留标题、URL、文件夹路径三列），再通过 CSV 模板导入，此方式兼容性更高。

**问：生产环境下如何更新前端静态资源而不中断服务？**

答：项目采用蓝绿部署策略处理前端资源更新。首先在新目录中拉取代码并执行 <code>npm run build</code> 生成新版本 dist 目录，然后修改 Nginx 配置中的 root 路径指向新目录，执行 <code>nginx -t</code> 校验配置无误后，执行 <code>nginx -s reload</code> 完成平滑切换。若需回滚，只需恢复 Nginx 配置至旧路径并重新加载即可。静态资源均使用内容哈希命名，无需担心浏览器缓存问题。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:17
