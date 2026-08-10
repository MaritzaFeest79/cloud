# MeiZhiLian Data Hub

MeiZhiLian Data Hub 是一个面向体育数据分析师、赛事运营团队及篮球爱好者的技术资源聚合平台，专注于提供结构化的赛事数据接口参考、实时比分解析工具链与历史数据追溯方案。项目本身不直接提供数据源，而是通过整理公开可用的数据端点、解析规则和社区维护的映射表，帮助开发者快速构建围绕美职联（MeiZhiLian）赛事的数据展示应用、竞猜系统或历史统计模块。

目标用户包括前端开发者、数据工程师、运维人员以及中小型体育数据服务商。本项目解决的核心问题是赛事数据分散、接口格式不统一、历史对照信息缺失以及快速原型验证时缺乏可靠的测试端点。

## 功能概览

- **实时比分端点映射**：提供标准化的比分查询接口模板，支持按球队、日期、赛季筛选，返回 JSON 格式的结构化数据。
- **赛程数据同步基线**：维护一份社区校正的赛程对照表，包含北京时间与美东时间的自动转换规则，便于定时任务编排。
- **积分榜计算引擎**：内置可配置的积分规则引擎（胜场、负场、胜率、净胜分），支持自定义权重系数，适用于联赛排名展示。
- **历史结果追溯工具**：提供多赛季历史比赛结果的 CSV 样本数据及导入脚本，方便本地测试数据分析管道。
- **比分解析中间件**：针对不同数据源返回的 HTML 或 XML 结构，提供 XPath 与正则表达式抽取模板，降低数据清洗门槛。
- **告警规则配置接口**：支持设置比分变化阈值、分差突变或加时触发等条件，通过 Webhook 输出告警事件。
- **健康检查与状态看板**：内置轻量级状态页，可监控各外部数据端点的响应时间与可用率，辅助运维决策。

## 应用场景

1. **实时赛事看板开发**  
   开发者可利用本项目提供的端点映射规则与解析中间件，快速搭建一个支持多场次同时监控的赛事看板，无需自行摸索各数据源的返回格式差异。

2. **历史数据回溯分析**  
   数据分析师可借助历史结果工具链，将过往赛季的比分数据导入本地数据库，进行胜率趋势、主客场差异或背靠背影响等深度分析。

3. **竞猜系统积分计算**  
   运营团队可复用积分榜计算引擎，将其适配到用户竞猜积分场景中，通过调整权重参数实现不同的排行逻辑，减少重复开发工作。

4. **数据源高可用切换**  
   运维人员可使用健康检查看板对多个备用数据端点进行实时探测，当主端点不可用时自动切换流量，保障业务连续性。

5. **教学演示与原型验证**  
   高校教师或技术博主可将本项目作为 API 网关设计或数据管道教学的案例素材，利用内置的 Mock 数据和解析模板快速验证架构方案。

## 快速开始

以下步骤将指导您在本地环境完成项目的克隆、依赖安装与服务运行。

```bash
# 克隆仓库
git clone https://github.com/meizhilian-dev/data-hub.git
cd data-hub

# 安装依赖（使用 Python 3.9+ 及 pip）
python -m venv venv
source venv/bin/activate  # Windows 使用 venv\Scripts\activate
pip install -r requirements.txt

# 初始化本地配置（复制示例配置并编辑）
cp config/endpoints.example.yaml config/endpoints.yaml
cp config/rules.example.yaml config/rules.yaml

# 运行数据同步测试
python scripts/sync_test.py --source all

# 启动开发服务（默认端口 8080）
python app.py --port 8080
```

执行上述命令后，访问 `http://localhost:8080/status` 可查看服务健康状态，访问 `http://localhost:8080/api/v1/scoreboard` 可获取模拟的实时比分聚合数据。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---|---|---|
| Python | 3.9 - 3.11 | 核心运行环境，推荐使用 3.10 长期支持版 |
| PyYAML | 6.0 以上 | 用于解析 endpoints.yaml 和 rules.yaml 配置文件 |
| requests | 2.28 以上 | 向外发送 HTTP 请求，获取外部数据源响应 |
| lxml | 4.9 以上 | 支持 XPath 解析 HTML/XML 格式的比分页面 |
| pandas | 1.5 以上 | 用于历史数据的加载、清洗与基础统计分析 |
| schedule | 1.2 以上 | 实现定时同步任务与轮询调度 |
| flask | 2.2 以上 | 提供 REST API 服务与状态看板页面 |
| gunicorn | 20.1 以上 | 生产环境推荐使用的 WSGI 服务器（Linux 环境） |
| pytest | 7.2 以上 | 运行单元测试与集成测试（仅开发依赖） |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|---|---|---|
| 入门指南 | docs/getting-started.md | 如何快速理解项目整体架构？第一次运行需要修改哪些配置？ |
| 接口规范 | docs/api-reference.md | 提供了哪些 REST 端点？请求参数与返回字段分别代表什么含义？ |
| 配置手册 | docs/configuration.md | 如何添加新的数据源端点？积分规则中的权重参数如何调整？ |
| 运维手册 | docs/operations.md | 如何部署到生产环境？日志级别如何修改？健康检查阈值如何设置？ |
| 数据字典 | docs/data-dictionary.md | 各数据表字段定义、枚举值含义及关联关系说明 |
| 开发指南 | docs/development.md | 如何贡献代码？测试套件如何运行？PR 提交规范是什么？ |

## 资源列表

本部分收录项目依赖或引用的外部公开资源，所有链接均按用户原始数据原样列出。

数据端点参考

- <code>qiutanbifenjishi.net.cn</code>
- <code>meizhiliansaicheng.org.cn</code>
- <code>meizhilianjishibifen.net.cn</code>
- <code>meizhilianjifenbang.org.cn</code>
- <code>meizhilianbisaijieguo.org.cn</code>
- <code>meizhilianbifen.org.cn</code>
- <code>lanqiujiebaobifen.net.cn</code>

## 项目结构

```
data-hub/
├── app.py                     # 服务入口，初始化 Flask 应用并注册路由
├── requirements.txt           # 生产环境依赖清单
├── config/                    # 所有配置文件存放目录
│   ├── endpoints.yaml         # 外部数据源端点定义（URL 模板、超时、重试策略）
│   ├── endpoints.example.yaml # 端点配置示例文件，供初次安装时复制
│   ├── rules.yaml             # 积分规则、解析规则与告警阈值配置
│   └── logging.yaml           # 日志级别、输出格式与滚动策略配置
├── core/                      # 核心业务逻辑模块
│   ├── fetcher.py             # 通用 HTTP 请求封装，含重试与熔断机制
│   ├── parser.py              # 针对不同数据源的 HTML/XML 解析器工厂
│   ├── scorer.py              # 积分榜计算引擎，支持动态权重调整
│   └── notifier.py            # 告警事件生成与 Webhook 分发器
├── scripts/                   # 运维与开发辅助脚本
│   ├── sync_test.py           # 测试所有已配置数据源的连通性与响应格式
│   ├── import_history.py      # 导入历史 CSV 数据到本地 SQLite 数据库
│   └── mock_server.py         # 启动本地 Mock 服务，模拟外部数据源行为
├── tests/                     # 单元测试与集成测试套件
│   ├── test_fetcher.py        # 测试 HTTP 请求重试、超时与异常处理
│   ├── test_parser.py         # 测试各解析模板对样本数据的抽取准确性
│   └── test_scorer.py         # 测试积分计算逻辑在不同输入下的正确性
├── data/                      # 本地数据存储目录
│   ├── samples/               # 样本数据文件（JSON/CSV），用于离线测试
│   └── cache/                 # 运行时的临时缓存文件（不提交至版本库）
├── docs/                      # 完整项目文档，详见文档导航章节
│   ├── getting-started.md
│   ├── api-reference.md
│   ├── configuration.md
│   ├── operations.md
│   ├── data-dictionary.md
│   └── development.md
└── README.md                  # 项目概览与快速入门（即当前文件）
```

## 贡献指南

我们欢迎并感谢任何形式的贡献，包括但不限于新增数据源适配器、优化解析性能、补充测试用例或改进文档。请遵循以下步骤：

1. 阅读 docs/development.md 了解开发环境配置、代码风格检查工具（flake8 / black）及提交信息格式规范。
2. 从 GitHub Issues 中选择一个未被认领的任务，或新建 Issue 描述您希望解决的问题或新增的功能，等待维护者确认。
3. Fork 本仓库，在您的分支上进行开发，并确保所有现有测试用例通过，新增功能需附带对应的单元测试。
4. 提交 Pull Request 至 main 分支，PR 描述中请注明关联的 Issue 编号以及测试覆盖率变化情况。
5. 等待至少一位维护者进行 Code Review，根据反馈完成修改后，将由维护者合并入主线。

## 常见问题

**问：运行 sync_test.py 时提示部分数据源连接超时，是否会影响服务启动？**

答：不会影响服务启动。本项目设计为弹性加载外部数据源，超时的数据源会被记录为不可用状态，服务仍可正常启动并提供基于缓存或 Mock 数据的降级响应。建议您检查网络环境或配置代理后重新运行测试，也可在 endpoints.yaml 中调整 timeout 和 retry 参数。

**问：积分榜计算引擎默认的胜场权重是 1.0，净胜分权重是 0.5，这个权重配置会影响历史数据吗？**

答：权重配置仅影响实时计算输出，不会修改历史数据源文件。您可以随时修改 rules.yaml 中的权重系数并重启服务，新的计算结果将立即生效于后续的 API 请求。如需对比不同权重下的排名变化，建议使用 scripts/import_history.py 导出历史数据到 CSV 后离线分析。

**问：项目是否支持 PostgreSQL 或 MySQL 作为后端数据库？**

答：当前版本默认使用 SQLite 以降低入门门槛，但 core/storage.py 中已经抽象了数据库适配器接口。您可以根据 docs/development.md 中提供的扩展指南，编写 PostgreSQL 或 MySQL 适配器并替换配置文件中的 dialect 参数。欢迎提交相应的适配器实现供社区复用。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:11
