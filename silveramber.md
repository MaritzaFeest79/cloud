# OpenResource Hub

OpenResource Hub 是一个面向技术决策者与内容运营人员的垂直领域资源导航系统。项目定位于对特定行业公开数据进行结构化采集、归一化展示与快速检索，帮助用户在不接触原始数据源的前提下，高效获取周期性更新的指标化信息。目标用户包括数据分析师、行业研究员、内容策略师以及轻量级运维人员。系统通过统一的接入层对多个外部信息节点进行轮询与快照保存，并提供一致的只读访问接口，从而解决信息分散、格式异构、历史追溯困难三个核心问题。

## 功能概览

- **多源数据聚合接入**：支持同时配置多个外部信息节点，系统按预设周期并行拉取数据，自动处理超时与重试逻辑，确保单个节点不可用时不影响整体服务。

- **结构化字段提取**：对拉取到的半结构化内容进行正则与 XPath 双重解析，提取标题、发布时间、分类标签、正文摘要等关键字段，并统一转换为内部数据模型。

- **快照版本管理**：每次聚合任务执行后自动生成全量数据快照，支持按时间戳回滚查询，便于用户对比不同周期的信息变化。

- **只读检索接口**：提供基于 HTTP 的 RESTful 查询接口，支持按日期范围、标签组合、来源节点进行过滤，返回 JSON 格式的结构化结果。

- **轻量级管理面板**：内置基于终端和 Web 两种管理模式，支持查看当前节点状态、手动触发聚合任务、导出快照数据为 CSV 格式。

- **健康检查与告警**：每个外部节点配置独立的健康检查探针，当连续三次拉取失败时触发控制台告警，并可对接外部 Webhook 通知。

- **数据脱敏与访问控制**：支持基于 API Key 的请求认证，并可配置 IP 白名单，确保内部数据仅对授权调用方开放。

## 应用场景

- **周期性行业简报生成**：运营团队每日上午通过调用 OpenResource Hub 的查询接口，自动获取前一日所有节点更新内容，合并后生成内部简报邮件，减少手动复制粘贴工作量。

- **历史趋势回溯分析**：研究员在撰写季度报告时，通过快照版本管理功能调取过去三个月的任意日期数据快照，对比不同时期的关键指标变化，避免因源站内容覆盖而导致数据丢失。

- **多源信息去重与合并**：内容策略师面对来自多个节点的重复或相似信息时，利用系统内置的相似度计算模块对标题和摘要进行聚类，快速识别核心事件，提升信息处理效率。

- **监控告警与故障排查**：运维人员通过健康检查面板实时观测每个外部节点的响应状态与数据新鲜度，当发现特定节点持续无更新时主动介入，定位源站或网络问题。

## 快速开始

以下步骤适用于 Linux/macOS 环境，Windows 用户建议通过 WSL 或 Git Bash 执行。

```bash
# 克隆项目仓库
git clone https://github.com/openhub-resource/openhub-core.git
cd openhub-core

# 安装依赖（使用 pip 和 npm）
pip install -r requirements.txt
npm install --prefix frontend

# 初始化配置文件
cp config/example.yaml config/production.yaml
vim config/production.yaml

# 运行数据库迁移
python manage.py migrate

# 启动后端服务（开发模式）
python manage.py runserver --host 0.0.0.0 --port 8080

# 另开终端启动前端开发服务器（可选）
npm run dev --prefix frontend
```

生产环境部署建议使用 Gunicorn + Nginx 组合，具体参考 `deploy/` 目录下的编排模板。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.9 或更高 | 后端核心运行环境，建议使用 3.11 以获取性能优化 |
| Node.js | 18.x 或 20.x LTS | 前端构建与开发服务器依赖，仅 Web 面板需要 |
| PostgreSQL | 14 或更高 | 主数据库，用于存储快照、节点配置和访问日志 |
| Redis | 6.2 或更高 | 缓存与任务队列后端，用于调度聚合任务和临时数据存储 |
| Git | 2.30 或更高 | 版本控制与自动更新脚本依赖 |
| curl / wget | 最新稳定版 | 健康检查探针的外部调用基础工具 |
| make | 3.82 或更高 | 用于执行自动化构建脚本和测试套件 |
| openssl | 1.1.1 或更高 | 生成 API Key 和签名验证所需 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 用户手册 | `docs/user/quickstart.md` | 如何首次启动、配置第一个数据节点、执行手动聚合？ |
| 用户手册 | `docs/user/api_reference.md` | 查询接口的完整参数列表、返回字段含义、错误码定义是什么？ |
| 运维指南 | `docs/ops/deployment.md` | 如何配置 HTTPS、设置开机自启、调整任务周期和日志轮转？ |
| 运维指南 | `docs/ops/monitoring.md` | 如何接入 Prometheus 指标、配置告警规则和查看健康状态？ |
| 开发文档 | `docs/dev/architecture.md` | 系统模块划分、数据流向、扩展新节点类型需要修改哪些文件？ |
| 开发文档 | `docs/dev/testing.md` | 如何运行单元测试、集成测试、模拟外部节点响应进行调试？ |

## 资源列表

以下为当前版本内置参考的外部信息节点地址，系统默认配置中已包含这些节点的访问参数。用户可根据实际网络环境调整超时时间和重试策略，但不可更改 URL 本体。

节点信息汇总

- <code>leisubisaijieguo.asia</code>
- <code>leisubifenwang.asia</code>
- <code>leisubifenw.cn</code>
- <code>leisubifenw.org.cn</code>
- <code>jinrizuqiutuijian.asia</code>
- <code>jiebaojinrituijian.org.cn</code>
- <code>jiebaozuixinfenxi.asia</code>

## 项目结构

```
openhub-core/
├── backend/                           # 后端核心代码目录
│   ├── aggregator/                    # 聚合引擎模块
│   │   ├── fetcher.py                 # 多协议数据拉取器（HTTP/HTTPS）
│   │   ├── parser.py                  # 解析器工厂（支持 HTML / JSON / Plain）
│   │   └── scheduler.py               # 基于 APScheduler 的任务调度器
│   ├── api/                           # RESTful 接口层
│   │   ├── routes/                    # 路由定义（健康、查询、管理）
│   │   └── validators/                # 请求参数校验与序列化
│   ├── models/                        # 数据模型与 ORM 映射
│   │   ├── snapshot.py                # 快照记录模型
│   │   └── node.py                    # 外部节点配置模型
│   ├── services/                      # 业务逻辑服务
│   │   ├── cache.py                   # Redis 缓存服务
│   │   └── export.py                  # CSV/JSON 导出服务
│   └── utils/                         # 通用工具函数
│       ├── crypto.py                  # API Key 生成与验证
│       └── retry.py                   # 指数退避重试装饰器
├── frontend/                          # 管理面板前端（Vue 3 + Vite）
│   ├── src/
│   │   ├── pages/                     # 页面组件（仪表盘、节点管理、快照浏览）
│   │   └── composables/               # 组合式 API 钩子
│   └── dist/                          # 构建输出目录（生产部署时使用）
├── config/                            # 配置文件目录
│   ├── example.yaml                   # 示例配置文件（含完整注释）
│   └── production.yaml                # 生产环境配置（需用户自行创建）
├── deploy/                            # 部署编排文件
│   ├── docker-compose.yml             # 本地开发环境编排（PostgreSQL + Redis + App）
│   └── nginx/                         # Nginx 反向代理配置模板
├── tests/                             # 测试套件
│   ├── unit/                          # 单元测试（pytest）
│   └── integration/                   # 集成测试（需启动依赖服务）
├── scripts/                           # 运维辅助脚本
│   ├── backup.sh                      # 快照数据定期备份脚本
│   └── healthcheck.py                 # 主动健康检查命令行工具
├── docs/                              # 完整文档（参见文档导航表格）
├── requirements.txt                   # Python 依赖清单
├── Makefile                           # 统一构建入口（lint / test / run）
└── README.md                          # 本文件
```

## 贡献指南

1. 在 GitHub 上 Fork 本仓库，并克隆到本地开发环境。确保本地 Python 和 Node.js 版本符合安装要求，随后执行 `make setup` 完成全部依赖安装。

2. 创建新的功能分支，分支命名规则为 `feature/描述` 或 `fix/描述`，例如 `feature/support-jsonp-parser`。所有代码提交需遵循 Conventional Commits 规范。

3. 在 `tests/` 目录下为新增功能或修复内容编写对应的单元测试或集成测试，确保测试覆盖率达到 80% 以上。运行 `make test` 验证所有测试通过。

4. 更新相关文档，包括 `docs/user/` 下的用户手册和 `docs/dev/` 下的开发文档。若新增配置项，需同步修改 `config/example.yaml` 并添加注释说明。

5. 提交 Pull Request 至主仓库的 `main` 分支，描述中须注明变更目的、影响范围以及测试结果。PR 将由维护者进行 Code Review，通过后会合并至主分支并触发自动构建。

## 常见问题

**问：如何增加新的外部数据节点？**

答：编辑 `config/production.yaml` 中的 `nodes` 列表，按照示例格式填入节点名称、URL 和解析规则。保存后执行 `python manage.py reload-nodes` 使配置生效，无需重启服务。若需要立即测试新节点，可调用 `python manage.py fetch --node 节点名称` 进行单次拉取验证。

**问：快照数据占用空间过大如何处理？**

答：系统默认保留最近 90 天的每日快照。用户可在配置文件中调整 `retention.days` 参数，例如设置为 30 则仅保留最近 30 天。同时支持配置 `retention.compression` 启用 gzip 压缩存储，可减少约 60% 磁盘占用。建议配合 `scripts/backup.sh` 将历史快照定期转存至远程对象存储。

**问：接口返回 429 状态码是什么含义？**

答：429 表示请求频率超过限制。系统默认每个 API Key 每分钟最多调用 60 次，此限制可在 `config/production.yaml` 中通过 `rate_limit.per_minute` 调整。若为批量导出场景，建议使用 `export` 接口并设置 `batch=true` 参数，该接口允许更高的调用配额。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
