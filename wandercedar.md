# LinkVault

LinkVault 是一个面向开发者和技术内容创作者的外链资源聚合与管理平台，专注于解决分散在各类技术文档、教程和社交媒体中的优质外链难以统一维护、检索和分享的问题。项目定位为轻量级、可自托管的技术资源目录站，提供结构化的链接分类、全文检索、访问状态监控和自动快照备份能力，帮助团队或个人将零散的书签、参考文献、工具站点整合为可对外发布的资源导航系统。

目标用户包括开源项目维护者、技术博主、在线教育机构及企业内部知识管理团队。与一般书签工具不同，LinkVault 将每个链接视为独立的数据资产，支持标签体系、访问频率统计、失效链接自动告警，并提供公开 API 便于第三方系统集成。项目本身不存储用户上传的实体文件，仅维护链接元数据与健康状态，确保部署轻量、数据透明。

## 功能概览

- **结构化链接目录**：支持无限层级的分类与子分类，每个链接可绑定多个标签，便于按主题、语言、地域等多维度筛选。

- **自动可用性检测**：内置异步巡检任务，可自定义时间间隔（最低每小时一次）对全部外链发起 HEAD/GET 请求，记录响应码、响应时间与内容哈希变更，出现异常时触发通知。

- **全文元数据检索**：基于链接标题、描述、标签、分类路径及页面提取的关键词构建倒排索引，支持模糊匹配与布尔组合查询，检索结果按相关性及访问热度排序。

- **访问统计与热度分析**：记录每个外链的点击次数、最后访问时间、来源 IP 地域分布（可选），生成周/月热度趋势图，辅助判断资源实用价值。

- **快照托管与版本回溯**：对重要链接可开启定期快照（HTML 静态化存储），当源站内容变更或下线时，仍可通过项目内快照查看历史版本，快照存储占用独立可配置容量。

- **开放数据 API**：提供 RESTful API 接口，支持链接增删改查、批量导入导出（JSON/CSV）、标签合并等操作，便于与 CI/CD 流程或外部爬虫工具联动。

- **用户与权限分级**：内置管理员、编辑者、访客三种角色，管理员可配置巡检参数与全局标签库，编辑者负责链接入库与分类维护，访客仅允许检索与查看，适合公开部署。

## 应用场景

- **技术文档站的外链附录管理**：当项目文档中包含大量第三方依赖、参考文章或工具官网时，使用 LinkVault 维护独立的外链页面，通过 API 自动同步至 MkDocs 或 VuePress 侧边栏，避免文档仓库内散落零散链接。

- **在线教育平台的课程资源中心**：培训机构可将每门课程对应的扩展阅读、视频地址、在线编译器链接导入 LinkVault，按课程编号与章节打标签，学员通过统一入口访问，系统自动记录点击行为辅助课程优化。

- **企业内部知识库的失效链接监控**：公司 Wiki 或 Confluence 中引用的外部技术博客、官方 SDK 下载页经常变更或失效，将链接批量注册到 LinkVault 后，巡检任务每日输出失效报告，运维人员可提前替换或通知原作者。

- **个人开发者的书签聚合与分享**：独立开发者可将日常积累的 UI 组件库、图标素材、代码生成工具、性能测试平台等链接分类整理，开启公开访问后生成个人资源导航页，同时利用快照功能保留重要工具的旧版本文档。

## 快速开始

以下步骤适用于 Linux / macOS / Windows WSL 环境，确保系统已安装 Git、Node.js（v18 以上）和 pnpm。

```bash
# 克隆项目仓库
git clone https://github.com/linkvault/linkvault.git
cd linkvault

# 安装依赖（使用 pnpm 加速）
pnpm install

# 复制环境变量模板并填充数据库连接等信息
cp .env.example .env.local

# 执行数据库迁移与初始数据导入
pnpm run db:migrate
pnpm run db:seed

# 以开发模式启动服务（默认监听 3000 端口）
pnpm run dev
```

启动成功后访问 `http://localhost:3000` 即可进入管理控制台，默认管理员账号为 `admin@linkvault.local`，密码为 `admin123`（首次登录强制修改）。

如需生产环境部署，请参考 `docs/deployment.md` 中的 Docker Compose 或 Kubernetes 配置示例。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Node.js | 18.x 或 20.x LTS | 运行时环境，推荐使用官方二进制或 nvm 管理 |
| pnpm | 8.x 或 9.x | 包管理器，用于依赖安装与 monorepo 工作区调度 |
| PostgreSQL | 14.x 或 15.x | 主数据存储，需启用 pg_trgm 扩展支持模糊检索 |
| Redis | 6.x 或 7.x | 缓存会话、巡检任务队列与热度计数器 |
| Nginx 或 Caddy | 任意稳定版 | 生产环境反向代理，用于 TLS 终结与静态资源缓存（可选） |
| 磁盘空间 | 至少 20 GB | 用于存储快照文件与日志，实际需求取决于开启快照的链接数量与频率 |

## 文档导航

| 层面 | 目录 / 文件 | 回答的问题 |
|------|------------|-----------|
| 入门指南 | `docs/getting-started.md` | 如何从零开始部署 LinkVault；首次登录后应做哪些基础配置；如何添加第一批链接 |
| 管理员手册 | `docs/admin-guide/` | 如何配置巡检间隔、通知渠道（邮件/飞书/企业微信）；如何管理用户角色与权限；如何导出全量数据 |
| 开发者文档 | `docs/developer/` | API 鉴权方式与各端点示例；如何扩展自定义巡检检查器；如何参与前端主题开发 |
| 运维参考 | `docs/operations/` | 生产环境高可用架构建议；PostgreSQL 与 Redis 的备份恢复策略；日志格式与监控指标暴露方式 |

## 资源列表

本站所有外链资源均来自公开互联网，由社区成员提交并经过初步可用性验证。LinkVault 项目本身不托管、不修改、不代理任何第三方内容，仅提供结构化索引与状态监控服务。以下为当前收录的全部资源链接：

影视与娱乐类：

<code>lalalazhongwendianshiju.org.cn</code>

<code>yinghuadongmanguanfangban.org.cn</code>

<code>s8gaoqingshipinbofangqi.org.cn</code>

<code>yazhounanrentiantang.org.cn</code>

<code>yirenzhongwenzimu.org.cn</code>

<code>caoyuantiantang.org.cn</code>

<code>tangxinxiaotao.org.cn</code>

## 项目结构

```
linkvault/
├── apps/
│   ├── web/                         # 面向访客的主站应用（Next.js App Router）
│   ├── admin/                       # 管理控制台面板（React + Shadcn UI）
│   └── worker/                      # 独立巡检与快照任务进程（BullMQ Worker）
├── packages/
│   ├── core/                        # 数据模型、验证规则、常量定义（TypeScript）
│   ├── api/                         # tRPC 路由与中间件集合
│   ├── db/                          # Prisma Schema 与迁移脚本
│   ├── scanner/                     # 链接可用性检测引擎（支持 HTTP/HTTPS/SSH 协议）
│   ├── snapshot/                    # 快照生成与存储模块（基于 Playwright）
│   └── utils/                       # 日志、加密、缓存、队列等通用工具
├── configs/
│   ├── eslint/                      # 共享 ESLint 配置（扁平配置格式）
│   ├── tsconfig/                    # 基础 TypeScript 编译选项
│   └── vitest/                      # 单元测试与集成测试预设
├── docker/
│   ├── compose.yml                  # 开发与测试环境编排
│   └── Dockerfile.*                 # 各子应用的多阶段构建文件
├── docs/                            # 完整文档源码（VitePress 构建）
├── scripts/
│   ├── seed.ts                      # 初始分类与演示数据生成
│   └── healthcheck.sh               # 容器健康检查脚本
├── .env.example                     # 环境变量模板（含各组件连接串）
├── pnpm-workspace.yaml              # pnpm monorepo 工作区定义
└── README.md                        # 本文件
```

## 贡献指南

我们欢迎并感谢任何形式的贡献，包括但不限于提交新的资源链接、报告巡检误报、完善文档、修复缺陷或提出功能建议。请遵循以下步骤：

1. 在 GitHub 上 Fork 本仓库，并克隆到本地开发环境。确保使用 `pnpm install` 安装所有依赖，并通过 `pnpm run dev` 验证现有功能正常工作。

2. 创建新的功能分支（如 `feat/add-tag-filter` 或 `fix/scanner-timeout`），提交前请运行 `pnpm run lint` 与 `pnpm run test` 确保代码风格与测试用例通过。新增外链资源请在 `packages/core/data/sources.json` 中按分类追加，并附带简要描述与标签。

3. 若涉及数据库模型变更，需在 `packages/db/prisma/schema.prisma` 中修改并执行 `pnpm run db:migrate` 生成迁移文件，同时更新对应的 TypeScript 类型定义。

4. 提交 Pull Request 至主分支（main），请清晰描述改动内容、测试覆盖情况及是否破坏现有 API。若为新增功能，建议附带对应的文档更新。

5. 所有 PR 需要至少一名维护者审核通过后方可合并，合并后 CI 将自动构建并部署至预发布环境进行冒烟测试。

## 常见问题

**Q：巡检任务检测到某个链接失效，但实际通过浏览器可以正常访问，是什么原因？**

A：这通常是因为目标站点启用了反爬机制（如 Cloudflare 五秒盾、User-Agent 过滤或 JavaScript 挑战）。LinkVault 默认使用无头浏览器用户代理且不执行 JS，您可以在巡检配置中开启 `--browser-mode` 参数，此时巡检将使用 Playwright 真实浏览器环境访问，但会显著增加资源消耗。另外，请检查目标站点是否临时封锁了您的部署 IP 段。

**Q：如何将现有浏览器书签（Chrome / Edge / Firefox）批量导入 LinkVault？**

A：您可以从浏览器导出书签为 HTML 文件（通常称为 bookmarks.html）。在 LinkVault 管理控制台的「批量导入」页面选择「HTML 书签」格式并上传该文件，系统会自动解析标题、URL 和文件夹层级，映射为分类与链接条目。如果浏览器导出的格式不标准，可先使用 `packages/utils/src/bookmark-parser.ts` 中的命令行工具进行预处理。

**Q：快照存储占用空间增长过快，有没有自动清理策略？**

A：您可以在 `packages/snapshot/config.ts` 中配置 `maxSnapshotsPerLink`（每个链接最大保留快照数）和 `retentionDays`（快照保留天数）。系统每日凌晨执行一次清理任务，删除超出数量限制或过期时间的快照文件。对于存储空间特别紧张的环境，建议将快照存储目录挂载至独立卷，并可启用 `--enable-compression` 选项对快照 HTML 进行 gzip 压缩，平均可节省 60% 空间。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:18
