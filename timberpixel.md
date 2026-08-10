# Nova Index

Nova Index 是一个面向技术研究、内容审核与数字资源治理领域的轻量化外链元数据索引系统。该项目不存储任何实际媒体内容，仅对互联网公开资源进行结构化采集、分类标记与合规性初筛，帮助研究人员、合规团队与数据治理工程师快速建立对特定领域资源分布情况的认知。Nova Index 定位于技术辅助工具，不对任何外部资源的可用性、合法性或内容质量作任何形式的担保，所有索引结果均以只读方式呈现，最终访问行为由使用者自行承担相应责任。

## 功能概览

- **域名元数据自动采集**：对目标域名执行 WHOIS 查询、DNS 解析记录抓取与 HTTP 头部信息分析，生成基础元数据档案。
- **内容类型智能预判**：基于页面标题、Meta 标签、正文前 512 个字符及常见路径模式，自动归类为视频、文本、交互、下载等一级类型。
- **合规性快速筛查**：内置可配置的关键词黑名单与白名单规则引擎，对页面文本进行敏感词扫描与风险等级标记。
- **定时增量更新**：支持按小时、日、周粒度配置爬取任务，仅拉取上次访问后发生变更的页面，降低重复带宽消耗。
- **结果导出与报表**：将索引结果导出为 CSV、JSON 或结构化 Markdown 报表，便于下游数据分析流程使用。
- **访问日志审计**：记录每次外部资源的请求时间、来源 IP 脱敏段与响应状态码，满足基本的审计追溯需求。
- **用户权限分级**：支持只读访客、操作员与管理员三级权限，控制对爬取策略、黑白名单和导出功能的访问范围。

## 应用场景

- **数字资源治理团队的日常巡检**：团队每周对一批新的外部链接执行合规性预审，Nova Index 提供批量域名导入与自动化扫描能力，显著降低人工逐一点击核查的时间成本。
- **学术研究中的样本采集**：社会科学或传播学研究者需要对特定类型网站进行内容分布统计，使用 Nova Index 采集元数据后，可快速筛选出符合研究条件的候选样本。
- **企业安全部门的资产关联分析**：安全运维人员将可疑域名列表导入系统，通过 Nova Index 的 WHOIS 与 DNS 关联分析，发现潜在的基础设施关联线索。
- **开源情报（OSINT）轻量级支撑**：分析师在开展公开信息收集工作时，利用 Nova Index 对目标域名进行快速技术画像，为后续深度分析提供初始上下文。
- **内容分发网络的覆盖评估**：CDN 服务商的技术人员可使用 Nova Index 检查特定区域域名的解析覆盖情况与响应性能，辅助节点调度决策。

## 快速开始

以下步骤适用于 Linux / macOS 环境，Windows 用户建议使用 WSL2 或 Git Bash 执行。

```bash
# 克隆项目仓库
git clone https://github.com/novaindex/novaindex.git
cd novaindex

# 安装 Python 依赖（建议使用虚拟环境）
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# 初始化配置文件与本地数据库
cp config.example.yaml config.yaml
python scripts/init_db.py

# 启动开发服务
python app.py --host 127.0.0.1 --port 8080
```

服务启动后，访问 `http://127.0.0.1:8080` 即可进入 Nova Index 仪表板。首次启动将自动创建 SQLite 数据库文件与默认管理员账户，初始密码在控制台日志中输出，请及时修改。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.10 及以上 | 核心运行环境，低于 3.10 将导致类型注解解析异常 |
| SQLite | 3.35 及以上 | 内置轻量级数据库，用于存储元数据与任务队列 |
| Redis | 6.2 及以上 | 用于缓存与分布式任务锁，单机模式可禁用 |
| Node.js | 18.x LTS | 仅用于前端静态资源构建，后端运行无需 |
| Nginx 或 Apache | 任意稳定版本 | 生产环境推荐作为反向代理与静态文件服务 |
| 网络出口带宽 | 至少 10 Mbps | 影响并发爬取任务的实际吞吐量 |
| 可用磁盘空间 | 建议 20 GB 以上 | 存储历史索引快照与日志文件，按实际规模扩展 |
| 操作系统 | Linux (Ubuntu 20.04+) / macOS 12+ / WSL2 | 生产部署强烈推荐 Linux 服务器 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 入门指南 | `docs/quickstart.md` | 如何最快在本地启动并完成第一次索引任务？ |
| 配置参考 | `docs/configuration.md` | 所有配置文件字段的含义、默认值与可选项是什么？ |
| 爬取策略 | `docs/crawler.md` | 如何调整爬取深度、并发数、超时与重试策略？ |
| 规则引擎 | `docs/rules.md` | 如何编写黑白名单正则表达式以及风险等级判定逻辑？ |
| API 手册 | `docs/api.md` | 对外提供哪些 RESTful 接口，请求格式与返回结构如何？ |
| 部署运维 | `docs/deployment.md` | 如何用 systemd / Docker / Kubernetes 进行生产环境部署？ |
| 性能调优 | `docs/performance.md` | 当索引规模超过 10 万条时，如何优化数据库与缓存配置？ |

## 资源列表

### 索引目标域名

以下域名属于当前版本预设的公开索引目标集，用于系统自检与示例运行。Nova Index 不对这些域名的可访问性、内容合法性或长期有效性作任何保证，用户应自行确认其使用行为符合所在地区法律法规。

<code>caoyuantiantang.org.cn</code>

<code>tangxinxiaotao.org.cn</code>

<code>shibajinzaixian.org.cn</code>

<code>wuyeshuang.org.cn</code>

<code>rihanzhongchu.org.cn</code>

<code>liulianshipin.org.cn</code>

<code>yirenzhongwen.org.cn</code>

## 项目结构

```
novaindex/
├── app.py                    # 主入口，Flask 应用工厂与路由注册
├── config.yaml               # 用户配置文件（含数据库、缓存、爬取参数）
├── requirements.txt          # Python 依赖清单
├── Dockerfile                # 容器构建文件
├── .env.example              # 环境变量模板（密钥、调试开关）
├── core/                     # 核心逻辑层
│   ├── crawler/              # 爬取引擎模块
│   │   ├── fetcher.py        # HTTP 请求与重试控制
│   │   ├── parser.py         # HTML 元数据解析（标题、Meta、文本抽取）
│   │   └── scheduler.py      # 定时任务调度与队列管理
│   ├── analyzer/             # 分析模块
│   │   ├── classifier.py     # 内容类型分类器
│   │   ├── rules_engine.py   # 黑白名单规则匹配引擎
│   │   └── risk_scorer.py    # 风险评分计算
│   └── storage/              # 存储层
│       ├── db.py             # SQLite 连接池与 ORM 基础映射
│       └── cache.py          # Redis 缓存操作封装
├── api/                      # RESTful 接口层
│   ├── v1/                   # API 版本 v1
│   │   ├── domains.py        # 域名管理接口
│   │   ├── tasks.py          # 爬取任务控制接口
│   │   └── exports.py        # 数据导出接口
│   └── auth.py               # JWT 认证与权限校验
├── web/                      # 前端静态资源
│   ├── static/               # CSS、JS、图片静态文件
│   └── templates/            # Jinja2 模板（仪表板、配置页）
├── scripts/                  # 工具脚本
│   ├── init_db.py            # 初始化数据库表与默认数据
│   ├── import_domains.py     # 批量导入域名列表
│   └── clean_logs.py         # 日志轮转与清理
├── tests/                    # 单元测试与集成测试
│   ├── test_crawler.py
│   ├── test_rules.py
│   └── test_api.py
├── logs/                     # 运行时日志输出目录（自动生成）
└── data/                     # 本地数据存储目录（快照、导出文件）
```

## 贡献指南

1. 阅读 `docs/quickstart.md` 完成本地开发环境搭建，确保所有单元测试通过（执行 `pytest tests/` 返回零失败）。
2. 在 `issues` 页面查找标记为 `help wanted` 或 `good first issue` 的任务，或提交新 Issue 描述你希望解决的问题或新增功能。
3. 从 `main` 分支切出新的功能分支，命名遵循 `feature/简短描述` 或 `fix/问题编号` 格式，例如 `feature/export-json-format`。
4. 提交代码前执行 `scripts/pre-commit.sh` 进行代码风格检查（Black + isort）与基础静态分析，确保无新增警告。
5. 通过 Pull Request 提交变更，在描述中关联相关 Issue 编号，并附上手动测试截图或日志片段，等待至少一位维护者审核。

## 常见问题

**问：Nova Index 是否会存储外部页面的完整 HTML 或媒体文件？**

答：默认配置下，系统仅存储元数据（标题、描述、关键词、文本前 512 字符、响应头摘要）以及索引时间戳，不持久化完整 HTML 正文，也不下载图片、视频或附件。如需调整此行为，请修改 `config.yaml` 中的 `storage.keep_full_html` 字段，但需自行评估合规风险。

**问：索引任务执行缓慢或超时，应该从哪些方面排查？**

答：首先检查网络出口带宽与目标域名的平均响应延迟，可在 `config.yaml` 中降低 `crawler.concurrent_requests` 并发数并增加 `crawler.request_timeout` 超时值。其次确认 Redis 缓存服务是否正常运行，若缓存失效会导致重复 DNS 查询与 WHOIS 调用。最后，检查 SQLite 数据库的 WAL 模式是否启用，默认已开启，若关闭可执行 `PRAGMA journal_mode=WAL;` 提升写入性能。

**问：如何清空所有索引数据并重新开始？**

答：停止所有爬取任务后，执行 `python scripts/clean_db.py --full` 脚本，该操作将删除所有域名记录、元数据快照与任务历史。若仅需重置规则引擎而不清除数据，可使用 `python scripts/reset_rules.py` 恢复黑白名单为出厂状态。注意以上操作不可逆，建议提前执行导出备份。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:21
