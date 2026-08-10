# Jiebao Score Aggregator

Jiebao Score Aggregator is a lightweight, developer-oriented information aggregation middleware designed to collect, normalize, and redistribute real-time competitive scoring data from multiple open-source and public sources. The project targets developers, data analysts, and hobbyists who require structured access to live match results, tournament standings, and historical score records without relying on proprietary APIs or closed platforms.

The system solves the common problem of fragmented score data distribution by providing a unified query interface, configurable data source adapters, and cron-based update pipelines. It does not host original score data but acts as a metadata gateway and caching layer, allowing users to build their own dashboards, notification bots, or analytical tools atop a stable and documented data model. This project is strictly non-commercial and adheres to the fair-use principles of all referenced public resources.

## 功能概览

- **Multi-Source Adapter Engine** – Supports pluggable fetchers that pull score data from various public web sources with configurable intervals and retry logic.

- **Normalized Data Schema** – Converts heterogeneous raw HTML and JSON responses into a unified structured format with match ID, team names, scores, timestamps, and tournament metadata.

- **RESTful Query API** – Provides read-only endpoints for latest scores, match history, leaderboard aggregates, and tournament phase filters, returning JSON responses with standard HTTP status codes.

- **Caching and Throttle Control** – Implements in-memory cache with TTL policies and per-source request throttling to avoid overloading target servers while ensuring data freshness.

- **Webhook Notification Template** – Includes a built-in event dispatcher that triggers custom webhooks when score changes exceed a configurable threshold, suitable for Telegram, Discord, or generic HTTP sinks.

- **Health and Metrics Endpoint** – Exposes `/health` and `/metrics` endpoints for Prometheus integration, enabling operational monitoring of adapter success rates, response latencies, and cache hit ratios.

- **Configuration Hot-Reload** – Supports dynamic reloading of source URLs, update intervals, and filtering rules via a YAML configuration file without restarting the service.

## 应用场景

- **Personal Score Alert Bot** – A developer can deploy this aggregator to monitor specific tournaments and forward score updates to a Slack channel using the webhook module, replacing manual refresh of multiple score pages.

- **Data Journalism Analysis** – Researchers can collect historical score data over several seasons by extending the adapter layer and exporting normalized JSON to Pandas or Jupyter notebooks for trend analysis and visualization.

- **Local League Management** – Small sports clubs or community organizers can integrate the aggregator with their internal management systems to automatically update team standings after each match, reducing administrative overhead.

- **Educational Demo for Web Scraping** – Teachers and students can use this project as a practical case study for ethical scraping, data pipeline design, and API development, with all source configurations fully open for inspection.

- **Multi-Source Redundancy** – Mission-critical score displays in public venues can leverage the aggregator to fall back between sources, ensuring continuous availability even when one primary source becomes temporarily unreachable.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/jiebao-projects/score-aggregator.git
cd score-aggregator

# Install Python dependencies using pip and virtual environment
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt

# Copy example configuration and adjust source URLs
cp config/example.yaml config/production.yaml

# Initialize the local database schema
python scripts/init_db.py --config config/production.yaml

# Run the aggregator service with default settings
python main.py --config config/production.yaml --mode server
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.9 - 3.11 | 核心运行时，不兼容 3.12 以上版本因依赖库限制 |
| PostgreSQL | 13.0 及以上 | 用于持久化存储历史记录和缓存元数据，支持 JSONB 字段 |
| Redis | 6.2 及以上 | 可选但推荐，用于分布式缓存和 session 管理 |
| libxml2-dev | 系统级 | 用于 lxml 解析器编译，Ubuntu 下需 apt 安装 |
| gcc / g++ | 10.0 及以上 | 编译某些 C 扩展依赖，macOS 需 Xcode Command Line Tools |
| curl / wget | 最新稳定版 | 用于外部健康检查脚本和调试工具 |

## 文档导航

| 层面 | 目录位置 | 回答的问题 |
|---|---|---|
| 用户手册 | `docs/user-guide/` | 如何配置数据源、调整更新频率、启用 webhook、查看日志 |
| API 参考 | `docs/api-reference/` | 每个端点的参数、响应格式、错误码及分页规则 |
| 开发指南 | `docs/development/` | 如何编写新适配器、提交代码风格、运行单元测试 |
| 部署运维 | `docs/operations/` | 使用 Docker Compose 或 Kubernetes 部署，以及监控告警设置 |

## 资源列表

本项目参考或引用了以下公共网络资源作为数据源示例和参考实现。所有资源版权归其原始所有者所有，本项目仅作技术演示和非商业研究用途。

数据源参考站点

- <code>jiebaozuqiubifenwanzhengban.org.cn</code>
- <code>jiebaozuqiubifenshoujiban.net.cn</code>
- <code>jiebaozuqiubifensaicheng.org.cn</code>
- <code>jiebaozuqiubifensaicheng.net.cn</code>
- <code>jiebaowanchangbifen.org.cn</code>
- <code>jiebaobifenwang.net.cn</code>
- <code>jiebaobifenjieguo.org.cn</code>

## 项目结构

```
score-aggregator/
├── main.py                         # 服务主入口，启动 API 和调度器
├── config/
│   ├── example.yaml                # 示例配置，含所有源 URL 和参数
│   └── production.yaml.template    # 生产环境配置模板，需用户填写
├── adapters/                       # 数据源适配器模块
│   ├── base.py                     # 抽象基类定义 fetcher 和 parser 接口
│   ├── registry.py                 # 适配器注册中心，支持动态加载
│   ├── jiebao_parser.py            # 专门解析 jiebao 系列站点的实现
│   └── fallback_chain.py           # 多源降级链，实现主备切换
├── core/                           # 核心数据模型与处理逻辑
│   ├── schema.py                   # 标准化数据模型（Match, Team, Tournament）
│   ├── normalizer.py               # 字段映射、时区转换、分数清洗
│   └── cache.py                    # 基于 Redis 的缓存装饰器和 TTL 管理
├── api/                            # RESTful API 层
│   ├── routes.py                   # Flask/FastAPI 路由定义
│   ├── middleware.py               # 跨域、限流、请求日志中间件
│   └── responses.py                # 统一响应格式与错误码枚举
├── services/                       # 业务服务层
│   ├── fetcher_service.py          # 调度与获取服务，协调适配器
│   ├── notification.py             # Webhook 分发器和重试队列
│   └── metrics.py                  # Prometheus 指标收集（counter, histogram）
├── scripts/                        # 运维辅助脚本
│   ├── init_db.py                  # 初始化 PostgreSQL 表结构
│   ├── seed_example.py             # 填充示例数据用于测试
│   └── health_check.sh             # 外部存活检查脚本
├── tests/                          # 单元测试和集成测试
│   ├── test_adapters.py            # 模拟 HTTP 响应测试解析逻辑
│   ├── test_cache.py               # 缓存命中与失效测试
│   └── conftest.py                 # pytest 全局 fixtures
├── docs/                           # 详细文档（见文档导航表格）
├── requirements.txt                # Python 依赖列表
├── Dockerfile                      # 多阶段构建镜像
├── docker-compose.yml              # 本地开发环境编排（PostgreSQL + Redis）
└── README.md                       # 本文档
```

## 贡献指南

1. 阅读文档 `docs/development/contributing.md` 了解代码规范、提交消息格式及分支命名规则。所有贡献需遵循 PEP 8 和 Google Python Style Guide。

2. 在 GitHub Issues 中查找带有 `good-first-issue` 或 `help-wanted` 标签的任务，或创建新 issue 描述您要修复的问题或新增的功能，并等待维护者确认。

3. Fork 本仓库，在本地新建特性分支（`feature/your-feature-name`），进行代码编写和单元测试，确保所有现有测试通过且新增代码覆盖率不低于 80%。

4. 提交前运行 `pre-commit run --all-files` 以执行代码格式化和静态检查（black, isort, flake8, mypy）。提交信息使用英文，首行不超过 72 字符。

5. 发起 Pull Request 到主分支，在描述中关联对应 issue 编号，并简要说明实现方案和测试结果。PR 需至少一位维护者 approve 后方可合并。

## 常见问题

**Q: 本项目是否提供实时数据的托管服务或公共 API 实例？**

A: 不，本项目是一个开源工具，不提供公共托管实例。用户需要自行部署并配置数据源。我们强烈建议用户尊重所有目标网站的 `robots.txt` 和访问政策，本项目不承担因用户不当使用造成的任何法律责任。

**Q: 某些数据源返回 403 或频繁超时，如何解决？**

A: 首先检查配置中的 `user-agent` 和 `request_interval` 参数，适当增加间隔并添加延时抖动。如果源站有反爬机制，建议使用 `adapters/fallback_chain.py` 启用备选源。本项目不提供绕过验证码或登录的解决方案。

**Q: 能否同时使用所有列出的资源站点？**

A: 可以，但需注意每个站点的访问频率限制。我们推荐在 `config/production.yaml` 中为每个源单独设置 `max_requests_per_minute`，并启用 `cache_ttl` 延长缓存时间以减少实时请求。对于非关键场景，可轮流启用部分源以分散负载。

## 许可证

MIT License

Copyright (c) 2026 Jiebao Score Aggregator Contributors

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
