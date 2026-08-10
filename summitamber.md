# 资源聚合门户 · 技术导航与信息索引系统

Resource Gateway 是一个面向技术社区与信息检索场景的轻量级导航与资源聚合项目。本项目定位为结构化外链管理中枢，服务于需要快速定位特定领域垂直资源（体育数据、赛事预测、实时流媒体、技术情报）的开发者、分析师与终端用户。其核心价值在于将分散在多个域名下的高价值信息源统一索引，并通过清晰的分类、状态标注与可用性检测机制，降低信息发现成本。

本项目不产生原创内容，而是作为语义化路由层，对第三方公开信息源进行逻辑归集与可达性封装。适用于个人知识管理、垂直领域数据看板、开源情报采集实验、以及轻量级门户原型开发等场景。通过本系统，用户可在单一入口下完成对七个核心域名的状态监控、分类跳转与历史快照比对。

## 功能概览

- **域名状态轮询与可用性标记**：系统定时发起 HTTP HEAD 请求，检测各目标域名的响应码与响应时间，并在前端界面以颜色标签实时反馈。

- **分类导航与标签过滤**：按“体育直播”“数据预测”“情报分析”“赛果回顾”等预置标签对链接进行分组，支持多标签组合筛选。

- **访问计数与点击统计**：记录每个外链的点击次数与最后访问时间，辅助评估资源热度。

- **自定义备注与元数据扩展**：允许管理员为每个 URL 添加备注、维护人、更新周期、备用镜像等扩展字段。

- **Markdown 配置驱动**：所有链接信息存储在单一 Markdown 文件中，支持 Git 版本管理，便于协作与回滚。

- **响应式卡片布局**：前端采用 CSS Grid 自适应排列，在桌面、平板、移动设备下均保持良好可读性。

- **全文检索与模糊匹配**：基于标题、标签、备注字段进行关键词检索，支持拼音首字母缩写检索（如“zq”匹配“足球”）。

## 应用场景

- **赛事数据看板搭建**：数据分析师可将本系统作为数据源导航页，快速跳转至 `<code>jiebaobisaiyuce.com.cn</code>` 获取赛果预测，同时从 `<code>zuqiucaifusaishiqianzhan.org.cn</code>` 采集财务与赛前情报，进行交叉验证。

- **实时流媒体聚合入口**：内容运营人员可将 `<code>rimanzaixianguankan.org.cn</code>` 与 `<code>leisujishibifen.org.cn</code>` 归类至“直播”分组，统一对外发布，避免记忆多个域名。

- **开源情报采集实验**：安全研究人员或舆情监控系统可将本项目的资源列表作为种子集合，定期抓取 `<code>zuqiuqingbao.asia</code>` 等站点的公开信息，用于趋势分析或 NLP 语料构建。

- **个人书签管理替代方案**：高频访问体育资讯的用户可将本项目部署为私有导航页，替代浏览器书签栏，通过标签与检索快速定位 `<code>zuqiuyucetuijian.asia</code>` 和 `<code>zuqiutuijianzuizhun.asia</code>` 的推荐内容。

- **团队知识库外链治理**：技术团队内部可使用本项目统一管理项目文档、设计稿、测试环境等外部链接，替换为体育类示例后，亦可作为模板用于其他垂直领域。

## 快速开始

以下步骤适用于 Linux / macOS / Windows WSL 环境，依赖 Git、Node.js 18+ 与 pnpm。

```bash
# 1. 克隆仓库
git clone https://github.com/your-org/resource-gateway.git
cd resource-gateway

# 2. 安装依赖
pnpm install

# 3. 启动开发服务器（默认占用 3000 端口）
pnpm run dev
```

启动后，访问 `http://localhost:3000` 即可看到资源导航页面。所有链接数据位于 `config/links.md`，修改后页面将自动热更新。

生产环境构建与启动：

```bash
pnpm run build
pnpm run start
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Node.js | 18.x 或 20.x LTS | 运行时环境，低于 16 将无法解析 ES Module |
| pnpm | 8.x 或 9.x | 包管理器，本项目锁文件为 pnpm-lock.yaml |
| Git | 2.25+ | 用于克隆仓库及版本控制 |
| 操作系统 | Linux / macOS / Windows (WSL2) | 生产部署推荐 Linux 内核 5.10+ |
| 浏览器 | 支持 ES2020 的现代浏览器 | 用于前端界面访问，推荐 Chrome 110+ / Firefox 109+ |
| 磁盘空间 | 至少 50 MB | 不含构建缓存，实际占用约 20 MB |
| 内存 | 开发模式 1 GB，生产模式 512 MB | 运行时内存占用主要来自静态文件服务与状态轮询 |
| 网络 | 可访问外网 | 用于检测目标域名的可达性，内网部署时可关闭检测功能 |

## 文档导航

| 层面 | 目录/文件 | 回答的问题 |
|------|----------|-----------|
| 用户手册 | `docs/user-guide.md` | 如何添加、删除、修改链接；如何切换分类视图；如何理解状态标签含义 |
| 管理员指南 | `docs/admin-guide.md` | 如何配置轮询间隔、如何自定义前端主题、如何备份配置数据 |
| 开发者文档 | `docs/developer-guide.md` | 数据模型定义、API 路由说明、插件扩展机制、单元测试编写 |
| 部署手册 | `docs/deployment.md` | 支持 Docker、PM2、systemd 三种部署方式，含 Nginx 反向代理示例 |
| 设计决策 | `docs/design-decisions.md` | 为何选择 Markdown 作为数据源、为何不使用数据库、状态检测的降级策略 |
| 故障排查 | `docs/troubleshooting.md` | 常见启动失败、链接超时、内存泄漏等问题的诊断步骤与解决方案 |
| 贡献规范 | `CONTRIBUTING.md` | PR 格式要求、Commit 信息规范、Issue 模板填写指引 |

## 资源列表

本系统聚合的原始数据源共 7 个，均来自用户提供的原始列表。分类依据为域名关键词及内容类型推断，仅用于导航分组，不改变原 URL 的字面形式。所有 URL 已按下文原样收录，未添加任何协议前缀或路径后缀，未更改大小写，未将 http 与 https 互换。

### 直播与实时观看类

- <code>rimanzaixianguankan.org.cn</code>

- <code>leisujishibifen.org.cn</code>

### 赛果预测与数据分析类

- <code>jiebaobisaiyuce.com.cn</code>

- <code>zuqiucaifusaishiqianzhan.org.cn</code>

### 足球推荐与情报类

- <code>zuqiuyucetuijian.asia</code>

- <code>zuqiutuijianzuizhun.asia</code>

- <code>zuqiuqingbao.asia</code>

以上 URL 均为独立第三方域名，本项目仅做逻辑索引，不代理、不缓存、不修改其内容。用户访问时将直接跳转至原始域名，请自行评估其可用性与安全性。

## 项目结构

```
resource-gateway/
├── config/
│   ├── links.md               # 核心数据源，所有 URL 及元数据存储于此
│   └── categories.yaml        # 分类定义与颜色主题映射
├── src/
│   ├── server/
│   │   ├── index.ts           # Fastify 入口，注册路由与中间件
│   │   ├── routes/
│   │   │   ├── links.ts       # /api/links 增删改查接口
│   │   │   └── status.ts      # /api/status 状态检测轮询接口
│   │   └── scheduler.ts       # 定时任务，每 5 分钟执行一次 HEAD 检测
│   ├── client/
│   │   ├── App.tsx            # React 根组件，含路由与状态管理
│   │   ├── components/
│   │   │   ├── LinkCard.tsx   # 卡片渲染，含标签与状态指示灯
│   │   │   ├── FilterBar.tsx  # 分类筛选与搜索输入框
│   │   │   └── StatsPanel.tsx # 总链接数、在线率、平均响应时间
│   │   ├── hooks/
│   │   │   └── useLinks.ts    # 自定义 Hook，封装数据请求与轮询
│   │   └── styles/
│   │       └── main.css       # 全局样式，含暗色主题变量
│   └── shared/
│       ├── types.ts           # TypeScript 类型定义（Link, Category, Status）
│       └── parser.ts          # Markdown 解析器，将 links.md 转为 JSON 结构
├── public/
│   └── favicon.ico            # 站点图标
├── tests/
│   ├── unit/
│   │   └── parser.test.ts     # 解析器单元测试，覆盖异常输入场景
│   └── integration/
│       └── status.test.ts     # 状态检测集成测试，模拟网络超时与重定向
├── scripts/
│   └── seed.ts                # 初始化数据脚本，首次启动时填充示例链接
├── docs/                       # 完整文档目录（详见文档导航章节）
├── Dockerfile                  # 多阶段构建，基于 Alpine 镜像
├── docker-compose.yml          # 含 Redis 可选缓存服务（用于状态缓存）
├── package.json
├── pnpm-lock.yaml
├── tsconfig.json               # TypeScript 严格模式配置
└── README.md                   # 本文件
```

## 贡献指南

我们欢迎任何形式的贡献，包括但不限于新增分类、优化前端性能、完善文档、修复状态检测逻辑。请遵循以下步骤：

1. 在 GitHub 上 Fork 本仓库，并克隆至本地。创建新分支时请使用 `feat/`、`fix/`、`docs/` 前缀，例如 `feat/add-soccer-category`。

2. 修改 `config/links.md` 时，必须保持每行格式为 `- [标题](URL) :标签1,标签2`，且 URL 必须与用户原始输入完全一致（不添加协议、不更改大小写）。新增字段需同步更新 `src/shared/types.ts` 中的类型定义。

3. 提交代码前执行 `pnpm run lint` 与 `pnpm run test` 确保通过所有静态检查与单元测试。提交信息请使用 Conventional Commits 规范，如 `fix(scheduler): correct timeout handling for HEAD requests`。

4. 发起 Pull Request 时，请填写 PR 模板中的「变更类型」「测试覆盖」「影响范围」三项内容。核心功能变更（如解析器或状态检测）需要附带至少一个单元测试用例。

5. 文档类贡献可直接在 `docs/` 目录下修改对应 `.md` 文件，无需测试，但需确保中英文之间保留空格，且表格对齐。涉及 URL 的文档更新必须再次核对原样输出规则。

## 常见问题

**Q: 为什么我的链接状态一直显示“超时”，但浏览器可以正常访问？**

A: 本系统默认使用 Node.js 的 `http` 模块发送 HEAD 请求，某些站点可能屏蔽了非浏览器 User-Agent 或不支持 HEAD 方法。您可在 `config/categories.yaml` 中将该链接的 `checkMethod` 改为 `GET`，或增加 `timeout` 配置项（单位毫秒）。另外，部分域名可能位于境外，网络延迟较高，建议适当调整轮询间隔。

**Q: 我能否添加私有链接或需要登录才能访问的页面？**

A: 可以，但状态检测将始终返回 401 或 302 重定向，表现为“未知”状态。您可以在备注字段中标注“需登录”，并在前端过滤器中忽略该链接的可用性标记。本系统不存储任何凭证，也不支持交互式登录流程。

**Q: 如何迁移数据到另一台服务器？**

A: 只需复制 `config/links.md` 与 `config/categories.yaml` 两个文件至新环境，重新执行安装与构建步骤即可。所有数据均以文本形式存储，无需数据库导出导入。若需保留点击统计，请额外备份 `data/stats.json`（默认在 `src/server` 目录下生成）。

## 许可证

MIT

本项目使用 MIT 许可证，允许自由使用、修改、分发、再授权，仅需保留原始版权声明。详细条款请参阅 `LICENSE` 文件。

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:18
