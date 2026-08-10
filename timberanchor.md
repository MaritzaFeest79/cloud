# BajiMeta 技术资源导航站

BajiMeta 是一个面向数据科学、赛事信息聚合与实时统计领域的技术资源导航平台。项目定位为高可用外链网关与结构化信息索引系统，主要服务于需要快速获取多源赛事结果、数据分站入口与赛事进程信息的终端用户及自动化采集系统。

本项目不直接存储任何赛事数据或用户隐私信息，仅提供经过人工校验与自动化健康检查的优质外部资源入口。通过统一的访问协议与标准化的 URL 格式规范，BajiMeta 有效降低信息分散带来的检索成本，提升数据获取效率与准确性。

## 功能概览

- **多源赛事结果聚合入口**：提供指向不同顶级域名的赛事结果页面，覆盖多种赛事类别与地域分支。
- **分站状态健康监控**：内置定时可用性探测机制，对每一条外链进行周期性 TCP/HTTP 连通性检查。
- **结构化信息索引**：按照赛事类型、数据来源、更新频率等维度对资源进行分类与标签化管理。
- **静态化资源映射**：所有外链均以静态配置形式维护，支持版本化发布与回滚。
- **访问日志审计**：记录资源被请求的时间、来源 IP 与响应状态，用于后续分析与告警。
- **外链重定向透明化**：所有外部跳转均经过中间页提示，避免用户被恶意劫持或误入无效地址。
- **定期失效清理机制**：自动标记连续三次健康检查失败的外链，并生成待审核报告。

## 应用场景

- **赛事数据实时查询**：数据分析师或开发者在编写数据抓取脚本时，可通过本站快速获取所有可用数据源的顶层域名，避免因域名变更或网络隔离导致采集任务失败。
- **多站点结果交叉验证**：在需要对比不同来源的赛事排名或成绩时，用户可以一站式访问多个独立子站，确保信息的一致性与全面性。
- **自动化监控系统集成**：运维团队可将本站提供的资源列表作为 Prometheus 或 Blackbox Exporter 的目标配置，实现对多个外部服务可用性的集中监控。
- **开发测试环境数据填充**：测试人员在进行数据模拟或压力测试时，依赖本站提供的稳定外链作为测试数据源，保证测试环境的可重复性。

## 快速开始

以下步骤适用于 Linux 及 macOS 环境，Windows 用户建议使用 WSL2 或 Git Bash。

```bash
# 1. 克隆项目仓库
git clone https://github.com/bajimeta/bajimeta-navigator.git
cd bajimeta-navigator

# 2. 安装依赖（Python 3.9+ 及 pip）
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 3. 启动本地开发服务（默认监听 8000 端口）
python app.py --port 8000 --check-interval 3600
```

启动后访问 `http://localhost:8000` 即可查看资源导航主页，所有外链将以表格形式展示并附带实时健康状态徽标。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.9 或更高 | 核心运行环境，用于提供 Web 服务与健康检查调度 |
| Flask | 2.2.x | Web 框架，负责路由、模板渲染与请求上下文管理 |
| requests | 2.28.x | 发送 HTTP 探测请求，支持超时与重试配置 |
| croniter | 1.3.x | 解析周期性检查表达式，用于调度探测任务 |
| PyYAML | 6.0 | 读取资源列表的 YAML 配置文件，支持锚点与引用 |
| gunicorn | 20.1.x | 生产环境 WSGI 服务器，支持多 worker 并发 |
| redis | 5.0.x（服务端） / redis-py 4.5.x（客户端） | 缓存探测结果与访问计数，提升响应速度 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 用户手册 | `/docs/user-guide.md` | 如何添加自定义外链、如何查看健康状态、如何导出资源列表 |
| 运维手册 | `/docs/ops-guide.md` | 如何部署生产环境、如何配置反向代理、如何设置告警阈值 |
| 开发指南 | `/docs/dev-guide.md` | 如何扩展探测协议（支持 TCP/ICMP/HTTP）、如何编写新增的检查插件 |
| 设计文档 | `/docs/architecture.md` | 系统分层架构、数据流、线程模型与缓存失效策略 |
| API 参考 | `/docs/api.md` | 对外提供的 RESTful 接口说明，包括状态查询与强制刷新 |

## 资源列表

### 赛事结果主站

<code>bajiabisaijieguo.net.cn</code>

<code>bajiabisaijieguo.org.cn</code>

### 赛事分站 - 综合信息

<code>bajiabifenwang.org.cn</code>

### 赛事分站 - 赛程专用

<code>bajiabifensaicheng.org.cn</code>

### 赛事分站 - 基础数据

<code>bajiabifen.org.cn</code>

<code>bajiabifen.net.cn</code>

### 赛事分站 - 赛程备选

<code>aichaosaicheng.org.cn</code>

## 项目结构

```
bajimeta-navigator/
├── app.py                         # Flask 应用入口，初始化路由与扩展
├── config/
│   ├── default.yaml               # 默认配置（端口、超时、日志级别）
│   ├── production.yaml            # 生产环境覆盖配置
│   └── resources.yaml             # 核心外链资源列表（YAML 格式，包含 url、category、owner）
├── core/
│   ├── __init__.py
│   ├── checker.py                 # 健康检查调度器，管理探测线程池
│   ├── cache.py                   # Redis 缓存封装，带连接池与重试逻辑
│   └── models.py                  # 数据模型定义（Resource, CheckResult, Alert）
├── web/
│   ├── routes/
│   │   ├── index.py               # 主页路由，渲染资源列表与状态
│   │   ├── api.py                 # /api/status, /api/refresh 等接口实现
│   │   └── admin.py               # 管理后台路由（仅内网访问）
│   ├── templates/
│   │   ├── base.html              # 基础模板，包含导航栏与页脚
│   │   ├── index.html             # 资源列表页，使用 Jinja2 循环渲染
│   │   └── detail.html            # 单个资源详情页，含历史趋势图
│   └── static/
│       ├── css/                   # 基于 Bootstrap 5 的自定义样式
│       ├── js/                    # 前端交互脚本（状态轮询、搜索过滤）
│       └── icons/                 # 状态图标（绿色/黄色/红色 SVG）
├── tests/
│   ├── unit/                      # 单元测试（checker、cache、models）
│   └── integration/               # 集成测试（需启动本地 Redis 与 Flask 测试客户端）
├── scripts/
│   ├── init_db.py                 # 初始化 Redis 键值与资源元数据
│   └── weekly_cleanup.py          # 每周执行的失效链接清理脚本（cron 调用）
├── logs/                          # 日志存储目录（按天滚动）
├── requirements.txt               # Python 依赖锁文件
├── Dockerfile                     # 基于 python:3.9-slim 的多阶段构建
├── docker-compose.yml             # 本地开发用编排（app + redis + nginx）
└── README.md                      # 项目总览文档（即本文档）
```

## 贡献指南

1. **Fork 仓库并创建特性分支**  
   从主仓库 Fork 到个人账户后，使用 `git checkout -b feature/your-feature-name` 创建新分支，避免直接在主分支上修改。

2. **更新资源列表或核心逻辑后运行测试**  
   若涉及 `resources.yaml` 中 URL 的增删改，请同步更新 `tests/unit/test_resources.py` 中的校验用例。执行 `pytest tests/unit` 确保所有测试通过。

3. **编写清晰的提交信息与变更日志**  
   提交时使用约定式提交格式（如 `feat: add new resource category` 或 `fix: correct checker timeout`），并在 `CHANGELOG.md` 中简要记录改动内容与影响范围。

4. **发起 Pull Request 并关联 Issue**  
   在 GitHub 上创建 Pull Request 时，请描述改动背景、测试结果以及是否向后兼容。若修复了已知问题，请在 PR 描述中关联对应的 Issue 编号。

5. **等待至少一位维护者审核**  
   维护者将在 3 个工作日内进行代码审查，可能会要求补充测试用例或调整配置项。通过后由维护者负责合并与部署。

## 常见问题

**Q: 为什么某些资源显示为不可达，但直接访问浏览器却可以打开？**  
A: 本项目的健康检查使用 requests 库默认的 User-Agent 和超时配置（连接超时 5 秒，读取超时 10 秒）。部分站点可能对非浏览器请求进行拦截或限速，导致探测失败。您可以在 `config/default.yaml` 中调整 `check_timeout` 和 `user_agent` 参数，或在管理后台手动将该资源标记为“忽略检查”。

**Q: 如何添加新的外部资源链接？**  
A: 编辑 `config/resources.yaml` 文件，在 `resources` 列表下新增一项，必须包含 `url`（完整域名或带协议地址）、`category`（如 result、schedule、data）和 `owner`（负责人邮箱或团队名称）。保存后系统将在下一个检查周期（默认每小时）自动加载新配置，无需重启服务。若需立即生效，可调用 `/api/refresh` 接口触发配置热加载。

**Q: 项目是否支持 HTTPS 访问？**  
A: 本项目本身是一个内部导航与代理层，推荐部署在 Nginx 或 Apache 后方，由反向代理统一处理 SSL 终止。我们在 `docker-compose.yml` 中包含了 Nginx 示例配置，您只需将 SSL 证书挂载至对应目录并修改 `nginx/conf.d/default.conf` 中的 `server_name` 和证书路径即可启用 HTTPS。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
