# HyperLink Nexus

HyperLink Nexus 是一个面向技术内容创作者、站长及数据分析师的开源外链资源聚合与导航系统。该项目旨在解决分散在网络各处的优质技术文档、数据源及工具入口难以统一管理与快速检索的问题，通过结构化的资源索引机制，帮助用户高效定位特定领域的信息节点。目标用户包括个人博客维护者、开源社区文档贡献者以及需要定期采集特定垂直领域公开数据的研发团队。

## 功能概览

- **批量资源导入与校验** 支持通过文本文件或标准输入流批量导入 URL 列表，系统自动执行协议格式校验与可达性预检，剔除无效条目并生成导入日志。

- **自定义分类标签系统** 允许用户为每个外链资源分配多个层级标签（如“数据源”、“体育资讯”、“实时比分”），并基于标签组合生成动态聚合视图。

- **定时健康检查** 内置轻量级调度器，可配置周期（每小时/每日）对已收录的 URL 执行 HEAD 请求检查，自动标记不可用资源并触发告警通知。

- **全文本搜索与过滤** 基于倒排索引实现对资源标题、描述、标签及域名关键词的快速检索，支持布尔表达式（AND/OR/NOT）过滤搜索结果。

- **外链关系图谱可视化** 解析资源页面中的外部链接引用关系，生成力导向关系图，帮助用户识别信息传播链路与核心枢纽节点。

- **数据导出接口** 提供 RESTful API 与命令行工具，支持将资源列表导出为 JSON、CSV 及 Markdown 表格格式，便于集成至其他文档流水线。

- **访问统计与热度排序** 记录每个外链资源的点击次数与最后访问时间，支持按热度、新增时间或域名字母顺序动态排序展示。

## 应用场景

- **垂直领域资讯聚合** 技术博客编辑可使用 HyperLink Nexus 集中管理多个体育赛事数据源链接，每日自动检查数据接口可用性，确保资讯更新任务不因链接失效而中断。

- **开源项目文档外链维护** 开源项目维护者可将项目 README 中引用的所有参考链接纳入系统管理，利用标签区分“API 文档”、“社区论坛”、“镜像站”等类别，当链接结构变更时快速定位并更新。

- **学术研究数据溯源** 从事网络信息传播研究的学者可利用本系统收集特定域名列表，通过健康检查日志分析站点存活率变化趋势，并导出关系图谱用于论文配图。

- **个人书签库迁移与去重** 需要从浏览器书签管理器迁移至更灵活索引方案的用户，可批量导入书签导出文件，利用系统的去重检测和标签重组织功能重建分类体系。

## 快速开始

以下指令适用于 Linux/macOS 及 Windows WSL 环境，基于 Python 3.10+ 运行。

```bash
# 克隆项目仓库
git clone https://github.com/nexus-dev/hyperlink-nexus.git
cd hyperlink-nexus

# 创建并激活 Python 虚拟环境
python3 -m venv venv
source venv/bin/activate   # Windows 下使用 venv\Scripts\activate

# 安装核心依赖及开发依赖
pip install --upgrade pip
pip install -r requirements.txt
pip install -r dev-requirements.txt  # 可选，用于运行测试

# 初始化本地数据库（SQLite）
python scripts/init_db.py --config config/default.yaml

# 启动开发服务器（默认监听 127.0.0.1:8000）
python app.py serve --port 8000
```

访问 `http://127.0.0.1:8000` 进入 Web 管理界面，首次运行将自动创建管理员账户（提示信息见控制台输出）。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.10 - 3.12 | 核心运行环境，低于 3.10 将无法使用 match/case 语法及类型提示新特性 |
| SQLite | 3.35.0+ | 默认内嵌数据库，用于存储资源元数据、标签和访问日志，生产环境可切换至 PostgreSQL |
| Redis | 6.2+ | 可选，用于缓存搜索结果与分布式任务锁，未安装时回退至内存缓存（单机模式） |
| curl | 7.68+ | 健康检查模块依赖系统 curl 命令执行实际 HTTP 探测，需确保 PATH 可用 |
| Node.js | 18.x+ | 仅用于前端资源构建（Web UI），若仅使用 API 模式可跳过 |
| git | 2.25+ | 用于版本管理与补丁应用，开发环境必需 |
| make | 3.81+ | 辅助执行常见任务（如格式化、测试），非强制但推荐 |

## 文档导航

| 层面 | 目录位置 | 回答的问题 |
|------|----------|-----------|
| 用户手册 | `docs/user/guide.md` | 如何添加第一批资源？如何配置自动健康检查？Web 界面各面板功能是什么？ |
| 运维参考 | `docs/ops/deployment.md` | 如何部署至生产服务器？如何切换 PostgreSQL 数据库？日志轮转策略如何设置？ |
| API 参考 | `docs/api/endpoints.md` | 有哪些可用的 REST 端点？如何通过 Token 鉴权？批量导入的请求体结构是什么？ |
| 开发者指南 | `docs/dev/contributing.md` | 代码风格规范是什么？如何编写新的健康检查探针？提交 PR 的流程步骤？ |
| 架构设计 | `docs/arch/overview.md` | 系统模块划分与数据流向是怎样的？为什么选择 SQLite 作为默认存储？扩展性如何保证？ |

## 资源列表

本系统初始索引库覆盖多个体育数据聚合站点，所有条目均来自公开可访问的域名。以下按域名特征分组列出：

体育赛事比分类

<code>bajiabisaijieguo.asia</code>

<code>aodaliyazuqiuchaojiliansai.asia</code>

赛事分析与推荐类

<code>bajiafenxi.asia</code>

<code>aochaotuijian.asia</code>

<code>aochaosheshoubang.asia</code>

实时直播与赛程类

<code>aochaozhibogw.asia</code>

<code>aochaosaicheng.asia</code>

## 项目结构

```
hyperlink-nexus/
├── app/                                # 应用核心包
│   ├── __init__.py
│   ├── main.py                         # 启动入口（FastAPI 应用工厂）
│   ├── config/                         # 配置模块（支持 yaml/env 覆盖）
│   │   ├── default.yaml                # 默认配置（数据库、缓存、调度参数）
│   │   └── validator.py                # 配置项类型校验器
│   ├── models/                         # 数据模型（SQLAlchemy ORM + Pydantic Schema）
│   │   ├── resource.py                 # Resource 表定义（URL、标题、描述、标签多对多）
│   │   ├── check_log.py                # 健康检查日志模型（记录状态码、响应时间）
│   │   └── tag.py                      # 标签模型（树形结构支持）
│   ├── services/                       # 业务逻辑层
│   │   ├── importer.py                 # 批量导入服务（支持 txt/csv 解析与去重）
│   │   ├── checker.py                  # 健康检查调度器（依赖 apscheduler 和 httpx）
│   │   ├── searcher.py                 # 搜索索引构建与查询执行（Whoosh 引擎）
│   │   └── exporter.py                 # 导出服务（生成 JSON/CSV/Markdown 格式）
│   ├── api/                            # RESTful 路由层
│   │   ├── v1/                         # API 版本 v1
│   │   │   ├── resources.py            # /api/v1/resources 增删改查端点
│   │   │   ├── checks.py               # /api/v1/checks 手动触发检查端点
│   │   │   └── tags.py                 # /api/v1/tags 标签管理端点
│   │   └── deps.py                     # 依赖注入（数据库会话、缓存客户端）
│   ├── static/                         # 前端静态资源（经 Webpack 构建）
│   │   ├── css/                        # 样式表（基于 Tailwind 定制）
│   │   └── js/                         # 页面逻辑（Vue 3 单页应用）
│   └── templates/                      # Jinja2 服务端模板（用于管理后台）
│       ├── dashboard.html              # 总览面板
│       └── resource_list.html          # 资源列表页
├── scripts/                            # 运维与开发辅助脚本
│   ├── init_db.py                      # 数据库初始化（创建表与默认标签）
│   ├── seed_demo.py                    # 填充示例数据（用于演示和集成测试）
│   └── health_check_cli.py             # 命令行手动触发健康检查
├── tests/                              # 单元测试与集成测试
│   ├── unit/                           # 单测（services 与 models 覆盖）
│   └── integration/                    # 集成测试（API 端点与数据库交互）
├── docs/                               # 完整文档（详见文档导航章节）
├── requirements.txt                    # 生产运行时依赖
├── dev-requirements.txt                # 开发与测试阶段额外依赖（pytest, black, mypy）
├── Dockerfile                          # 多阶段构建镜像（基于 alpine 轻量化）
├── docker-compose.yml                  # 本地开发环境编排（含 Redis 与 PostgreSQL 可选）
├── Makefile                            # 常用任务快捷命令（如 make test, make lint）
└── README.md                           # 本文件
```

## 贡献指南

1. 阅读 `docs/dev/contributing.md` 了解代码风格（Black + Flake8）及提交信息规范（Conventional Commits），确保所有新代码通过 CI 流水线中的 lint 与测试步骤。

2. 在 Issue 列表中选择未被指派的待办任务，或提交新 Issue 描述你发现的问题或建议的新特性，等待维护者确认后开始实现。

3. Fork 本仓库，创建以 `feature/` 或 `fix/` 为前缀的本地分支，开发过程中保持每项逻辑变更对应一个原子提交，并编写相应的单元测试覆盖新增逻辑。

4. 完成开发后运行 `make test` 确保全部测试通过（包括原有回归测试），使用 `make format` 自动格式化代码，随后推送分支并发起 Pull Request，在 PR 描述中关联对应的 Issue 编号并提供测试结果截图或日志。

5. 维护者将在 3 个工作日内进行 Code Review，如有修改意见将通过 PR 评论反馈，调整完成后由维护者执行合并或关闭操作。

## 常见问题

**问：健康检查模块是否会对外部站点造成过多请求压力？**

答：系统默认检查间隔为 24 小时，且所有请求均携带 `User-Agent: HyperLinkNexus-HealthCheck/1.0` 标识，超时时间设置为 5 秒，并发数限制为 10 个连接。对于单个域名的连续检查失败次数超过 3 次后，系统会自动将该资源标记为“待人工确认”并暂停后续检查，避免对不可达站点重复无效请求。

**问：是否支持 Windows 原生环境（非 WSL）运行？**

答：核心 Python 代码完全兼容 Windows，但健康检查模块默认依赖系统 curl 命令。在 Windows 环境下，建议安装 Git Bash 或 Cygwin 以提供 curl 环境，或修改配置将检查器切换为纯 Python 实现的 `httpx` 客户端模式（需在 `config/default.yaml` 中设置 `checker.use_system_curl: false`）。前端构建工具链基于 Node.js，在 Windows 下同样正常工作。

**问：数据库从 SQLite 迁移至 PostgreSQL 时，现有数据如何保留？**

答：项目提供 `scripts/migrate_db.py` 脚本，通过读取 SQLite 数据文件并生成 PostgreSQL 兼容的 INSERT 语句完成迁移。执行前请在配置中正确填写 PostgreSQL 连接字符串，并确保目标库已创建所需的表结构（可通过 `init_db.py --engine postgresql` 自动生成）。迁移过程不会修改原 SQLite 文件，支持多次试运行验证数据一致性。

## 许可证

MIT License

Copyright (c) 2026 HyperLink Nexus Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
