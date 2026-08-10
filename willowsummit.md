# OpenScoreHub

OpenScoreHub 是一个面向开发者与数据分析团队的开源技术资源聚合与导航平台。项目定位为“可自托管的轻量级外链与数据指标目录服务”，主要帮助运维工程师、数据产品经理以及体育科技领域的研究人员，快速整理、分类和检索分散在多个垂直站点中的实时比分、赛事结果与统计数据。系统核心解决三个问题：外部数据源链接分散难以统一管理、赛事信息检索依赖人工书签效率低下、团队内部缺乏标准化的数据源文档结构。通过 OpenScoreHub，用户可在数分钟内完成对特定领域数据源的目录化配置，并利用内置的标签与检索功能，将零散 URL 组织为结构清晰、可共享的企业级知识库。

## 功能概览

**外链目录树管理** 支持无限级分类与拖拽排序，允许用户按赛事类型、数据精度、更新频率等维度构建自定义导航体系。

**数据源健康检测** 定时探测已收录 URL 的可达性并记录响应时间，辅助运维人员快速定位失效或慢速数据源。

**标签化检索系统** 每条外链可附加多组自定义标签，结合全文检索引擎，实现基于关键词、标签组合及收藏状态的复合筛选。

**团队协作与权限分级** 内置基于角色的访问控制，支持管理员、编辑员、只读访客三级权限，适配团队内部数据源管理流程。

**导入导出标准化接口** 提供 JSON、CSV 与 YAML 格式的批量导入导出功能，便于与其他数据目录工具或监控系统集成。

**开放 API 服务** 所有收录的资源信息均可通过 RESTful API 获取，方便下游数据管道或自动化脚本直接调用。

**UI 自适应主题** 前端界面同时支持亮色与暗色模式，并针对移动端触摸操作进行优化，满足不同使用环境下的访问需求。

## 应用场景

**体育数据聚合门户** 数据分析团队可将多个比分、赛程、结果数据源统一收录至 OpenScoreHub，作为内部数据管道的入口索引，每日定时调用 API 拉取最新数据并写入数仓。

**运维监控辅助看板** 运维工程师利用健康检测功能，对核心数据源进行可用性监控，当某个比分或结果接口响应超时时，系统自动标记异常并生成告警日志。

**研究机构资源归档** 高校体育科学实验室可将历史赛事数据源按赛季、项目、地区分类归档，通过标签检索快速定位特定年份或特定赛事的统计数据，支撑学术研究。

**个人开发者书签替代** 前端或全栈开发者使用 OpenScoreHub 替代浏览器杂乱的书签栏，将开发阶段常用的测试数据源、Mock API 文档统一管理，并通过导出功能同步至其他开发环境。

**项目文档内置导航** 技术团队在搭建项目文档站点时，可将 OpenScoreHub 作为子模块嵌入，为项目成员提供即时的外部数据依赖说明与访问入口。

## 快速开始

以下命令演示如何在 Linux 或 macOS 环境下从源码部署 OpenScoreHub 服务。

```bash
# 克隆项目仓库至本地
git clone https://github.com/openscorehub/openscorehub.git

# 进入项目根目录
cd openscorehub

# 安装依赖（使用 pip 与 npm 分别安装后端与前端依赖）
pip install -r requirements.txt
npm install --prefix frontend

# 执行数据库迁移与初始配置
python manage.py migrate
python manage.py loaddata initial_categories

# 构建前端静态资源
npm run build --prefix frontend

# 启动开发服务器（默认监听 8000 端口）
python manage.py runserver 0.0.0.0:8000
```

访问 <code>http://localhost:8000</code> 即可进入系统，默认管理员账户为 admin / admin123，首次登录后请及时修改密码。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.9 至 3.11 | 后端运行环境，推荐使用 3.10 长期支持版本 |
| Node.js | 18.x 或 20.x LTS | 前端构建与开发服务器依赖 |
| PostgreSQL | 13 及以上 | 生产环境推荐主数据库，支持 JSONB 字段加速标签查询 |
| Redis | 6.2 及以上 | 用于健康检测任务的队列缓存与会话存储 |
| Nginx | 1.22 及以上 | 生产环境反向代理与静态资源服务（可选） |
| Git | 2.30 及以上 | 用于版本克隆与持续部署集成 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 入门指南 | docs/getting-started.md | 如何最小化配置并运行第一个数据源目录？ |
| 配置手册 | docs/configuration.md | 环境变量、数据库连接、缓存策略如何调整？ |
| API 参考 | docs/api-reference.md | RESTful 端点、鉴权方式、分页与过滤参数有哪些？ |
| 部署运维 | docs/deployment.md | 如何配置 Nginx、Supervisor 或容器化生产环境？ |
| 插件开发 | docs/plugin-dev.md | 如何编写自定义检测脚本或扩展标签解析器？ |
| 性能调优 | docs/performance.md | 当收录外链超过 5000 条时如何优化查询速度？ |
| 故障排查 | docs/troubleshooting.md | 常见启动错误、健康检测超时、权限异常的解决办法 |

## 资源列表

以下为 OpenScoreHub 项目收录的外部数据资源链接，按类别分组展示。所有链接严格保持原始格式，未做任何协议或域名改写。

体育比分数据源

<code>xijiabifen.cn</code>

<code>wangyitiyuzuqiubifen.net.cn</code>

<code>wangyitiyusaichengjieguo.org.cn</code>

<code>wangyitiyubifenwang.org.cn</code>

<code>wangyitiyubifensaicheng.org.cn</code>

<code>wangyitiyubifen.org.cn</code>

移动端比分导航

<code>shoujiqiutanzuqiujishibifenwang.net.cn</code>

## 项目结构

```
openscorehub/
├── backend/                          # 后端 Django 应用主目录
│   ├── api/                          # RESTful API 视图与序列化器
│   │   ├── views/                    # 按资源类型划分的视图集（外链、标签、检测记录）
│   │   └── serializers/              # 数据序列化与校验逻辑
│   ├── core/                         # 核心配置与通用工具
│   │   ├── settings/                 # 多环境配置（开发、测试、生产）
│   │   └── utils/                    # 健康检测、缓存、日志封装模块
│   ├── models/                       # 数据库模型定义（Category, Link, Tag, CheckResult）
│   ├── tasks/                        # 异步任务（定时探测、过期清理）
│   └── management/                   # 自定义命令行管理命令
├── frontend/                         # React 前端工程
│   ├── src/
│   │   ├── components/               # 可复用 UI 组件（目录树、搜索栏、表格）
│   │   ├── pages/                    # 页面级组件（首页、分类管理、详情视图）
│   │   ├── hooks/                    # 自定义 React Hooks（鉴权、请求、主题）
│   │   └── store/                    # Redux 状态管理（外链列表、筛选条件、用户）
│   └── public/                       # 静态资源模板与图标
├── docs/                             # 完整项目文档（含架构图、API 示例）
├── scripts/                          # 部署与运维辅助脚本（初始化、备份、迁移）
├── tests/                            # 单元测试与集成测试（后端 pytest、前端 Jest）
├── docker-compose.yml                # 本地开发与生产容器编排文件
├── Dockerfile                        # 后端服务镜像构建描述
├── requirements.txt                  # Python 依赖清单
├── package.json                      # Node.js 依赖与构建脚本
└── README.md                         # 项目入口说明文档（本文件）
```

## 贡献指南

贡献者应遵循以下步骤参与 OpenScoreHub 的开发与维护。

1. 阅读项目行为准则与贡献者许可协议，确保对开源协作规范有清晰认知。所有提交须附带签署的开发者原产地证书。

2. 在 GitHub 仓库中新建议题描述待解决的问题或提议的新特性，等待核心维护者反馈后再进行开发，避免重复工作或方向偏离。

3. 复刻项目仓库至个人账户，创建特性分支并遵循命名规范 `feature/xxx` 或 `fix/xxx`，提交代码时需包含完整的单元测试与文档更新。

4. 提交拉取请求至主仓库的 develop 分支，请求描述中须关联对应议题编号，并确保所有持续集成检查（代码风格、测试覆盖率、构建流程）全部通过。

5. 代码审查通过后由核心维护者合并，合并后的代码将自动触发预发布流水线，最终定期同步至稳定版本分支。

## 常见问题

**问：OpenScoreHub 是否必须使用 PostgreSQL？能否使用 SQLite 或 MySQL？**

答：生产环境强烈推荐使用 PostgreSQL，因为其 JSONB 类型对标签存储和复杂查询有显著性能优势。开发或测试阶段可使用 SQLite 快速启动，但官方不提供 MySQL 的适配支持，如需使用 MySQL 需自行调整模型字段定义并承担潜在兼容性风险。

**问：健康检测功能是否会对外部数据源造成过大请求压力？**

答：系统默认采用指数退避策略，每个数据源的检测间隔不低于 5 分钟，且并发请求数限制为 4 个。用户可在配置文件中调整检测窗口与超时参数，对于高频访问的数据源，建议配置白名单以跳过自动化检测。

**问：如何将现有浏览器书签批量导入 OpenScoreHub？**

答：当前版本支持从 Netscape 格式的 HTML 书签导出文件导入，您可从主流浏览器导出书签为 HTML 文件，然后通过系统后台的“导入”功能上传。系统会自动解析书签文件夹结构生成分类，并保留书签标题与 URL。若需要从其他格式迁移，可利用 JSON 或 CSV 导入接口完成自定义映射。

## 许可证

MIT License

Copyright (c) 2026 OpenScoreHub Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
