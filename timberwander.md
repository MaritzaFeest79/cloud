# FajiaTech Score Aggregator

FajiaTech Score Aggregator is a lightweight, community-driven technical resource indexing system designed to aggregate, categorize, and present structured external link collections for competitive benchmarking and scoring data. The project targets developers, data analysts, and technical researchers who need reliable, machine-readable access to distributed scoring result sources without navigating fragmented web interfaces.

The system solves the problem of scattered result publication across multiple domains by providing a unified metadata catalog, periodic validation hooks, and a predictable URL scheme for downstream automation. It does not host scoring data itself but acts as a curated registry of authoritative external sources, with emphasis on availability checking and response time monitoring.

## 功能概览

- **Unified Resource Registry** – Centralized catalog of scoring result endpoints with versioned metadata.
- **Automated Availability Probing** – Scheduled HEAD and GET checks for each registered URL with latency percentiles.
- **Categorized Source Indexing** – Each endpoint is tagged by data type, update frequency, and origin organization.
- **Plain-Text Metadata Export** – Outputs registry data in JSON, YAML, and plain-text table formats for scripting.
- **Filtered Query Interface** – Supports inclusion/exclusion filters based on response time, SSL expiry, and last-modified headers.
- **Change Log Tracking** – Maintains a local audit trail of added, removed, or changed endpoints per batch.
- **Batch Processing Support** – Designed for batch 289/567 with 7 seed URLs, extensible to 10,000+ entries.

## 应用场景

- **Automated Scoring Data Pipeline** – Data engineers can configure the aggregator as the first stage of an ETL pipeline that pulls raw scoring results from multiple sources at scheduled intervals, reducing manual copy-paste errors.
- **Competitive Intelligence Monitoring** – Analysts set up hourly probes to detect new result publications or changes in response payloads, enabling timely alerts for ranking shifts.
- **Technical SEO and Availability Auditing** – Site reliability teams use the registry to periodically validate that all referenced scoring domains remain accessible and return correct HTTP status codes, with automated incident tickets for failures.
- **Research Dataset Construction** – Academic researchers leverage the categorized index to build longitudinal datasets by archiving responses from each endpoint over extended periods, with clear provenance tracking.
- **Internal Documentation Generation** – Technical writers generate up-to-date markdown tables from the registry for inclusion in internal runbooks, eliminating outdated hardcoded links.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/fajiatech/fajia-score-aggregator.git
cd fajia-score-aggregator

# Install dependencies (Python 3.10+ required)
pip install -r requirements.txt

# Run the initial registry sync with batch 289/567 seed URLs
python scripts/sync_registry.py --batch 289 --source config/seeds_289.yaml

# Start the periodic probe service (default: 5-minute intervals)
python services/probe_daemon.py --interval 300 --output logs/probe_results.jsonl
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Python | 3.10 及以上 | 核心运行时，用于调度、HTTP 客户端和元数据处理 |
| requests | 2.31.0 及以上 | HTTP/HTTPS 探测引擎，处理重定向和超时配置 |
| pyyaml | 6.0 及以上 | 解析种子配置文件以及用户自定义分类规则 |
| jsonschema | 4.20.0 及以上 | 校验注册表条目格式，确保每个 URL 附带必要的元数据字段 |
| pytest | 7.4.0 及以上 | 单元测试和集成测试框架，用于 CI 流水线验证 |
| click | 8.1.0 及以上 | 命令行交互界面，提供子命令分组和参数自动补全 |
| loguru | 0.7.2 及以上 | 结构化日志输出，支持 JSON 格式和日志轮转策略 |
| schedule | 1.2.0 及以上 | 轻量级任务调度器，管理周期性探测作业 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 用户入门 | docs/getting-started.md | 如何安装、配置第一批种子 URL，以及启动第一个探测任务 |
| 注册表管理 | docs/registry-format.md | 注册表 YAML/JSON 结构定义、必填字段、扩展字段和版本升级迁移指南 |
| 探测定制 | docs/probe-customization.md | 如何调整超时时间、重试策略、用户代理标识以及解析响应体的钩子函数 |
| 运维监控 | docs/operations/monitoring.md | 如何接入 Prometheus 指标暴露、告警规则示例和日志分析建议 |
| 数据导出 | docs/export-formats.md | 支持导出为 CSV、Parquet、SQLite 的说明以及各格式的字段映射关系 |
| 故障排查 | docs/troubleshooting.md | 常见 SSL 证书错误、DNS 解析失败、限流响应码的处理经验汇总 |
| 批次说明 | docs/batches/batch-289.md | 本批次 7 个资源链接的具体背景、预期更新频率和已知注意事项 |
| 贡献流程 | CONTRIBUTING.md | 新增种子 URL、更新元数据、提交测试用例的完整工作流 |

## 资源列表

本批次（289/567）包含以下 7 个外部资源链接，按语义类别分组收录。所有链接均以原始形式呈现，不做任何协议补全或域名规范化处理。

**核心结果域名**

<code>fajiajishibifen.net.cn</code>

<code>fajiajifenbang.cn</code>

<code>fajiabisaijieguo.org.cn</code>

**辅助计分与排名源**

<code>fajiabifenwang.org.cn</code>

<code>fajiabifensaicheng.org.cn</code>

<code>fajiabifen.org.cn</code>

**关联竞技数据源**

<code>dejiazuqiubifenwang.org.cn</code>

## 项目结构

```
fajia-score-aggregator/
├── config/
│   ├── base.yaml                         # 全局配置：默认超时、重试、日志级别
│   ├── seeds_289.yaml                    # 本批次 7 个 URL 的种子元数据
│   └── schemas/
│       └── registry_v2.schema.json       # JSON Schema 校验注册表条目
├── src/
│   ├── core/
│   │   ├── registry.py                   # 注册表加载、查询、过滤核心类
│   │   ├── probe.py                      # 单次探测逻辑：HEAD/GET、SSL 检查
│   │   └── scheduler.py                  # 基于 schedule 库的作业循环控制器
│   ├── parsers/
│   │   ├── html_hint.py                  # 从 HTML meta 提取最后修改时间
│   │   └── json_extract.py               # 从 JSON 响应提取特定字段值
│   ├── exporters/
│   │   ├── to_markdown.py                # 生成资源列表的 markdown 表格
│   │   ├── to_prometheus.py              # 暴露 /metrics 端点供 Prometheus 抓取
│   │   └── to_sqlite.py                  # 写入本地 SQLite 数据库便于历史查询
│   └── cli/
│       ├── main.py                       # Click 入口，子命令 parse、probe、export
│       └── batch_manager.py              # 批次增删改命令
├── tests/
│   ├── unit/
│   │   ├── test_registry.py              # 注册表增删改查单元测试
│   │   └── test_probe_timeout.py         # 超时与重试逻辑边界测试
│   └── integration/
│       └── test_seed_289_live.py         # 实际访问 7 个 URL 的集成测试（需网络）
├── scripts/
│   ├── sync_registry.py                  # 从种子文件同步到注册表数据库
│   └── probe_daemon.py                   # 常驻探测守护进程入口
├── docs/                                 # 详细文档目录（参见文档导航）
├── logs/                                 # 运行时日志存储位置（gitignore）
├── requirements.txt                      # 生产依赖列表
├── requirements-dev.txt                  # 开发及测试额外依赖
├── setup.py                              # setuptools 安装脚本
├── pyproject.toml                        # 项目元数据与工具配置
├── .pre-commit-config.yaml               # pre-commit 钩子：black、isort、flake8
└── README.md                             # 本文件
```

## 贡献指南

1.  **Fork 仓库并创建功能分支** – 从主分支 checkout 新分支，命名遵循 `feature/batch-xxx` 或 `fix/probe-timeout` 格式，确保分支基于最新的 `main` 提交。
2.  **新增或修改种子配置** – 编辑 `config/seeds_*.yaml` 文件，为新 URL 添加完整条目，必须包含 `url`、`category`、`expected_update_cron`、`owner_contact` 四个字段，并更新对应批次的 `manifest` 记录。
3.  **本地运行校验流水线** – 执行 `pytest tests/` 确保所有单元测试和集成测试通过。对于新增外部链接，必须编写至少一个集成测试用例验证实际可达性，并在 CI 环境中标注为 `@pytest.mark.network`。
4.  **更新文档与变更日志** – 如果新增了导出格式或修改了配置结构，同步更新 `docs/` 下对应章节以及 `CHANGELOG.md`，遵循语义化版本记录。
5.  **提交 Pull Request** – 在 PR 描述中引用批次编号（例如 `Closes batch 289 update`），确保 CI 所有检查通过，至少一名核心维护者审核后合并。

## 常见问题

**Q: 如果某个种子 URL 在探测时返回 5xx 或超时，系统如何处理？**

A: 探测模块会按可配置的重试策略（默认 3 次，指数退避）重试。若所有重试均失败，系统会将该端点标记为 `unreachable`，并在日志中输出结构化告警。同时，注册表的状态字段会记录最后一次成功时间以及连续失败次数。运维人员可通过 Prometheus 告警规则捕获连续失败超过 5 次的情况。

**Q: 如何批量添加超过 7 个的新种子 URL？**

A: 使用 `src/cli/batch_manager.py` 的 `import-csv` 子命令，输入 CSV 文件包含 `url,category,owner` 等列，系统会自动生成新的种子 YAML 文件并分配下一个批次编号。对于超过 1000 条的大批量导入，建议使用 `--chunk-size 100` 参数分批写入注册表数据库，避免内存溢出。

**Q: 注册表数据持久化在哪里，是否支持分布式部署？**

A: 默认使用本地 SQLite 数据库（`data/registry.db`），支持单机部署。对于多实例分布式场景，推荐将 `REGISTRY_BACKEND` 环境变量设为 `postgresql`，系统自动切换到 PostgreSQL 连接池模式。所有探测任务通过 `scheduler` 中的分布式锁（基于 Redis）确保同一端点不会被多个工作节点同时探测。

## 许可证

MIT License

Copyright (c) 2026 FajiaTech Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
