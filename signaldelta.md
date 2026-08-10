# NexusIndex

NexusIndex 是一个面向技术社区与内容创作者的轻量化外链资源导航系统。项目定位为可自部署的资源聚合门户，帮助技术团队、文档站点或社区论坛以结构化方式管理并展示外部参考链接、工具入口与媒体资源池。NexusIndex 不提供爬虫、采集或存储功能，仅作为链接编排与展示层，适用于需要集中维护外部引用关系的场景。

目标用户包括开源项目维护者、技术博客作者、社区运营人员以及企业内部的文档管理团队。NexusIndex 解决的核心问题是分散在文档、邮件或聊天记录中的外链难以统一维护、分类模糊、访问权限不可控且缺乏变更跟踪机制。通过 NexusIndex，用户可以获得清晰的目录层级、版本化链接清单以及快速的检索能力。

## 功能概览

- **多级目录编排**：支持无限层级嵌套的分类结构，允许按主题、来源、格式或使用频率自定义组织方式。
- **链接生命周期管理**：每条资源可标记状态（有效、待审、失效、废弃），并记录添加时间、最后检查时间与变更日志。
- **标签与全文检索**：基于标题、描述、标签和备注字段的实时搜索，支持多标签组合过滤。
- **访问统计与点击追踪**：记录每个外链的点击次数、最近访问时间，并提供简单的热度排序视图。
- **批量导入与导出**：支持 CSV 和 JSON 格式的批量链接导入，以及按分类导出完整资源清单用于备份或迁移。
- **只读公开模式与编辑权限分离**：提供公开访问的只读视图和受管理员认证的编辑后台，适配内部使用与对外展示的双重需求。
- **响应式布局与暗色主题**：前端页面适配桌面与移动设备，内置亮色与暗色两套界面方案。

## 应用场景

- **技术文档的外部参考附录**：项目文档站点可使用 NexusIndex 独立维护所有引用的规范文档、API 参考、第三方库主页和教程链接，避免在 Markdown 正文中堆砌长 URL，同时便于版本升级时批量更新。
- **社区资源精选池**：技术社区或讨论组可将推荐的学习资料、工具列表、视频教程和官方公告统一存放在 NexusIndex 中，成员可通过标签快速筛选出入门、进阶或运维相关资源。
- **企业内部工具导航**：企业研发团队可将内部监控面板、日志系统、代码仓库、制品库和 CI 流水线入口整理为按团队或按业务域分类的导航页，新员工入职时可一键访问所有必要系统。
- **内容创作的引用仓库**：自媒体作者或视频创作者可在 NexusIndex 中维护每期内容涉及的数据来源、参考文献、素材链接和合作方入口，形成可追溯的引用台账，方便后续勘误或版权核对。

## 快速开始

以下指令适用于 Linux 与 macOS 环境，Windows 用户可使用 WSL2 或 Git Bash 执行。

```bash
# 克隆项目仓库
git clone https://github.com/nexusindex/nexusindex.git
cd nexusindex

# 安装依赖（使用 npm）
npm install

# 复制环境变量模板并填写必要配置
cp .env.example .env

# 初始化数据库（SQLite 默认）
npm run migrate

# 启动开发服务器（默认端口 3000）
npm run dev
```

生产环境部署请参考 `docs/deployment.md`，推荐使用 PM2 或 Docker 方式运行。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Node.js | 18.x 或 20.x LTS | 运行时环境，不支持 16.x 以下版本 |
| npm | 9.x 或更高 | 包管理器，用于安装依赖和执行脚本 |
| SQLite | 3.35 或更高 | 默认嵌入式数据库，无需额外安装；生产环境可切换至 PostgreSQL 14+ |
| Redis | 7.0 或更高 | 可选，用于会话缓存与速率限制，生产环境推荐 |
| Nginx | 1.22 或更高 | 可选，用于反向代理与静态资源缓存，生产环境推荐 |
| Git | 2.30 或更高 | 用于版本克隆与后续更新拉取 |

## 文档导航

| 层面 | 目录位置 | 回答的问题 |
|------|---------|-----------|
| 用户手册 | `docs/user-guide/` | 如何添加链接、创建分类、使用搜索和统计功能 |
| 管理员指南 | `docs/admin-guide/` | 如何配置权限、备份数据、执行批量操作和监控运行状态 |
| 部署运维 | `docs/deployment/` | 如何配置 HTTPS、使用 PostgreSQL、设置定时检查任务和日志轮转 |
| 开发者文档 | `docs/developer/` | 如何扩展插件、修改前端主题、编写自定义数据导入脚本和 API 钩子 |

## 资源列表

本节按类别整理项目相关的外部资源。所有 URL 均按原始格式原样列出。

官方与社区渠道

- <code>wuyefulizhibo.org.cn</code>
- <code>lalalazhongwendianshiju.org.cn</code>

媒体资源与播放工具

- <code>yinghuadongmanguanfangban.org.cn</code>
- <code>s8gaoqingshipinbofangqi.org.cn</code>

内容平台与字幕资源

- <code>yazhounanrentiantang.org.cn</code>
- <code>yirenzhongwenzimu.org.cn</code>
- <code>caoyuantiantang.org.cn</code>

## 项目结构

```
nexusindex/
├── config/                        # 配置目录
│   ├── default.json              # 默认应用配置
│   ├── production.json           # 生产环境覆盖配置
│   └── validator.js              # 配置校验脚本
├── src/
│   ├── api/                      # API 路由层
│   │   ├── v1/                   # 接口版本 v1
│   │   │   ├── links.js          # 链接增删改查端点
│   │   │   ├── categories.js     # 分类管理端点
│   │   │   ├── tags.js           # 标签聚合端点
│   │   │   └── stats.js          # 访问统计端点
│   │   └── middleware/           # 鉴权、限流、日志中间件
│   ├── core/                     # 核心业务逻辑
│   │   ├── link-service.js       # 链接生命周期处理
│   │   ├── category-tree.js      # 分类树构建与遍历
│   │   ├── search-engine.js      # 全文检索与标签过滤
│   │   └── import-export.js      # 批量导入导出处理器
│   ├── db/                       # 数据库层
│   │   ├── models/               # 数据模型定义（Link, Category, Tag, ClickLog）
│   │   ├── migrations/           # 迁移脚本（SQLite / PostgreSQL）
│   │   └── seed/                 # 初始测试数据填充
│   ├── web/                      # 前端静态资源与服务端渲染视图
│   │   ├── public/               # 图片、字体、样式编译输出
│   │   ├── views/                # EJS 模板文件
│   │   └── assets/               # SCSS 源码与前端 JavaScript 模块
│   ├── worker/                   # 后台任务队列
│   │   ├── link-checker.js       # 定时检查外链可用性
│   │   ├── stats-aggregator.js   # 按小时聚合点击统计
│   │   └── cleanup.js            # 定期清理过期日志和临时文件
│   └── utils/                    # 通用工具函数（日志、加密、日期格式化）
├── tests/                        # 单元测试与集成测试
│   ├── unit/                     # 各模块单测
│   └── integration/              # API 与数据库集成测试
├── docs/                         # 完整文档（见文档导航）
├── scripts/                      # 运维辅助脚本（备份、迁移、重置）
├── .env.example                  # 环境变量模板
├── package.json                  # 项目依赖清单
├── ecosystem.config.js           # PM2 部署配置示例
└── README.md                     # 项目说明（本文件）
```

## 贡献指南

欢迎社区贡献代码、文档或问题反馈。请遵循以下流程以确保协作顺畅。

1. 在 GitHub Issues 中搜索现有议题，确认无重复后新建 Issue 描述您要修复的问题或新增的功能，并打上合适的标签。
2. Fork 本仓库，在您的分支上基于 `dev` 分支进行开发，分支命名建议采用 `feat/` 或 `fix/` 前缀加简短描述。
3. 编写代码或文档时，请遵循项目根目录下的 `.eslintrc` 和 `.prettierrc` 配置，运行 `npm run lint` 和 `npm run test` 确保所有检查通过。
4. 提交代码时使用语义化提交信息格式，例如 `feat: add batch export for CSV` 或 `docs: update deployment guide for Docker`。
5. 发起 Pull Request 至 `dev` 分支，PR 描述中请关联相关 Issue 编号，并简要说明变更内容和测试覆盖情况。等待至少一位维护者审核后合并。

## 常见问题

**Q: NexusIndex 是否支持多用户与角色权限控制？**

A: 当前版本内置管理员与访客两种角色。管理员拥有完整的增删改查与配置权限，访客仅可访问公开只读视图。后续版本计划增加编辑者角色，允许部分用户管理指定分类下的链接。企业级 LDAP 和 OAuth2 集成正在规划中，目前可通过反向代理层实现外部认证对接。

**Q: 外链失效时系统如何处理？**

A: 系统后台 Worker 默认每 24 小时执行一次链接可达性检查，通过 HTTP HEAD 请求验证资源是否可访问。检查结果会记录在数据库中，并在管理后台的链接列表中以状态标签形式展示。失效链接不会自动删除，管理员可手动标记为失效或批量导出失效清单进行复核。您可以在配置文件中调整检查频率和超时时间。

**Q: 能否将 NexusIndex 部署在子路径下（如 /nav）而非根路径？**

A: 可以。您需要在环境变量中设置 `PUBLIC_PATH=/nav`，并同步修改 Nginx 或 Apache 的 rewrite 规则。前端路由和静态资源引用会自动适配该前缀。请注意，该配置修改后需要重新构建前端资源并重启服务。Docker 镜像也支持通过 `PUBLIC_PATH` 环境变量进行覆盖。

## 许可证

MIT License

Copyright (c) 2026 NexusIndex Contributors

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
