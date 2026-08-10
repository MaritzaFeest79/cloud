# Nexus Resource Gateway

Nexus Resource Gateway is a lightweight, community-driven technical resource aggregation and navigation system designed for developers, researchers, and open-source contributors who need to rapidly locate, categorize, and access domain-specific online materials. Unlike general-purpose search engines or bookmark managers, Nexus Resource Gateway applies structured metadata tagging, availability probing, and usage-context tagging to each registered resource, enabling users to filter by content type, region availability, and topical relevance. The project targets technical teams that maintain curated external link collections, internal documentation hubs, or educational resource indexes, and it solves the problem of link rot, contextual ambiguity, and discovery overhead through automated health checks and semantic categorization.

The system operates as a static-site generation pipeline combined with a lightweight Python-based crawler that validates resource responsiveness and extracts basic page metadata. Users can manage resource lists via YAML manifests, generate browsable HTML indexes with search and filter capabilities, and output machine-readable JSON feeds for integration into other tools. Nexus Resource Gateway does not host or proxy any third-party content; it acts solely as a structured metadata layer over user-provided URL lists. The project is production-ready for teams with 5 to 500 curated links, and it includes built-in support for custom tag taxonomies, expiration reminders, and multi-format export.

## 功能概览

- **URL Manifest Management** – Define resource collections in YAML with fields for URL, category, tags, region, and optional expiry date.
- **Automated Availability Probing** – Perform periodic HEAD and GET requests to verify resource reachability and record HTTP status codes.
- **Metadata Extraction** – Retrieve page titles, description meta tags, and content-language hints without full page scraping.
- **Tag-Based Filtering and Search** – Generate static HTML pages with client-side filtering by tag, category, and status (active/unreachable).
- **Multi-Format Export** – Output resource lists as JSON, CSV, or Markdown tables for integration with documentation pipelines.
- **Health Report Generation** – Produce weekly summary reports showing active, unreachable, and newly added resources.
- **Custom Field Support** – Attach arbitrary key-value notes to each resource, such as internal remarks, access credentials (encrypted), or usage examples.
- **CLI and Python API** – Provide both a command-line interface for batch operations and a programmable Python module for custom scripting.

## 应用场景

- **Internal Technical Documentation Hub** – A development team maintains a private knowledge base that references dozens of external specification documents, API references, and community forums. Nexus Resource Gateway indexes these references, automatically checks for broken links in nightly builds, and generates an internal dashboard that highlights dead links before they reach end-user documentation.

- **Open-Source Project External Resource Page** – An open-source framework maintains a "Community Resources" page linking to tutorials, video series, and third-party tools. The project maintainers use Nexus Resource Gateway to manage the link list via Git, validate each link on pull request, and publish a static resources page that shows last-verified timestamps and status badges.

- **Educational Course Material Curation** – A university lecturer curates a list of supplementary reading materials, online simulators, and reference datasets for a computer networking course. With Nexus Resource Gateway, the lecturer organizes resources by weekly topic, sets automatic expiry reminders for temporary lab environments, and exports a student-facing HTML page that remains up-to-date without manual editing.

- **Regional Content Mirror Selection** – A research group collects multiple regional mirrors for scientific datasets. They use Nexus Resource Gateway to track mirror availability and response latency, generating a ranked list that automatically suggests the fastest accessible mirror for each geographic region.

- **Compliance-Focused Link Auditing** – A fintech compliance team maintains references to regulatory announcements published on various official websites. The team configures Nexus Resource Gateway to check these URLs daily and flag any status code changes, providing an audit trail of resource availability for regulatory inspections.

## 快速开始

```bash
# Clone the repository
git clone https://github.com/nexus-resource-gateway/nrg-core.git
cd nrg-core

# Create and activate a Python virtual environment
python3 -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Initialize default configuration and sample manifest
nrg init --sample

# Run health check on all resources defined in the manifest
nrg check --manifest resources.yaml

# Generate static HTML site in ./output directory
nrg build --manifest resources.yaml --output ./output

# Start local development server to preview the generated site
python -m http.server --directory ./output 8000
```

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|----------|----------|------|
| Python | 3.9 – 3.12 | 核心运行时，支持类型提示和异步 I/O |
| PyYAML | 6.0+ | 用于解析和写入 YAML 清单文件 |
| aiohttp | 3.9+ | 异步 HTTP 客户端，用于并发资源探测 |
| beautifulsoup4 | 4.12+ | 用于解析 HTML 元数据，不依赖外部浏览器 |
| lxml | 4.9+ | 作为 beautifulsoup4 的解析器后端，提供更快的 HTML 处理 |
| jinja2 | 3.1+ | 模板引擎，用于生成静态 HTML 页面和报告 |
| click | 8.1+ | 构建命令行界面，提供子命令和参数解析 |
| pytest | 7.4+ | 仅开发测试依赖，生产环境可不安装 |
| black | 23.0+ | 仅开发代码格式化依赖，生产环境可不安装 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | docs/user/manual.md | 如何安装、配置清单格式、运行检查与生成站点、自定义标签体系 |
| 运维指南 | docs/operator/deployment.md | 如何部署为定时任务、配置邮件报告、调整探测超时和并发数、备份策略 |
| 开发者文档 | docs/developer/api.md | Python API 模块结构、如何编写自定义过滤器、扩展元数据提取器、贡献插件 |
| 设计说明 | docs/design/architecture.md | 系统整体架构图、数据流、存储设计、异步任务调度原理、安全边界 |
| 常见工作流 | docs/recipes/github-actions.md | 如何在 GitHub Actions 中集成每日自动检查并提交状态 PR |
| 示例集合 | docs/examples/advanced-filtering.md | 针对多标签组合、正则匹配、状态排序的高级查询写法 |

## 资源列表

### 影视字幕与媒体资源

- <code>tingtingzhongwenzimu.org.cn</code>
- <code>jiqingsifang.org.cn</code>
- <code>guochansanjizipai.org.cn</code>
- <code>jiujiumi.org.cn</code>
- <code>jiujiuyiren.org.cn</code>
- <code>tayepa.org.cn</code>
- <code>guochanjiqingzipai.org.cn</code>

## 项目结构

```
nrg-core/
├── src/
│   ├── nrg/
│   │   ├── __init__.py                # 包入口，导出主要 API 类
│   │   ├── cli.py                     # Click 命令行入口，定义 nrg 子命令
│   │   ├── checker.py                 # 异步资源探测逻辑，含重试和状态记录
│   │   ├── parser.py                  # YAML 清单解析与验证，支持自定义字段
│   │   ├── metadata.py                # 基于 aiohttp 和 beautifulsoup4 的元数据提取
│   │   ├── renderer.py                # Jinja2 渲染引擎，生成 HTML 和 Markdown
│   │   ├── exporter.py                # JSON / CSV 导出器，支持流式写入大清单
│   │   ├── scheduler.py               # 定时任务包装器，用于 cron 集成
│   │   └── utils.py                   # 通用工具函数（日志、配置加载、路径处理）
│   └── nrg.egg-info/                  # 打包元数据（自动生成）
├── tests/
│   ├── unit/                          # 单元测试，覆盖解析器、检查器、元数据提取
│   ├── integration/                   # 集成测试，使用本地 mock HTTP 服务
│   └── fixtures/                      # 样本 YAML 清单和预期输出
├── docs/                              # 完整文档，含用户手册、运维指南、API 参考
├── templates/                         # Jinja2 HTML 模板（索引页、详情页、报告页）
├── examples/                          # 示例清单文件和自定义模板
├── scripts/                           # 辅助脚本（如迁移工具、初始数据生成）
├── requirements.txt                   # 生产环境依赖锁定
├── requirements-dev.txt               # 开发环境额外依赖
├── setup.py                           # setuptools 安装脚本
├── pyproject.toml                     # 项目元数据、black/isort/pytest 配置
├── .github/
│   └── workflows/                     # GitHub Actions 工作流（CI 测试、自动发布）
└── README.md                          # 项目概览与快速入门（本文件）
```

## 贡献指南

1. **阅读设计说明** – 在提交任何实质性更改之前，请先阅读 `docs/design/architecture.md` 以理解系统边界、异步模型和扩展点，避免与现有设计范式冲突。

2. **创建议题讨论** – 对于新功能、重大重构或破坏性变更，请先在 GitHub Issues 中创建一个议题，并标注 `proposal` 标签，等待核心维护者反馈后再着手编码。

3. **编写测试用例** – 所有新功能或错误修复必须附带对应的单元测试或集成测试，测试覆盖率不应低于 85%。测试文件放置于 `tests/unit/` 或 `tests/integration/` 下，命名遵循 `test_*.py` 规范。

4. **代码风格与提交规范** – 使用 `black` 和 `isort` 进行自动格式化，提交消息遵循 Conventional Commits 规范（如 `feat: add timeout override flag`、`fix: handle empty manifest gracefully`）。每个拉取请求应包含清晰的变更描述和测试结果截图（如适用）。

5. **文档同步更新** – 若更改影响用户可见行为、配置格式或 CLI 参数，必须同步更新对应的文档章节和示例文件。文档使用 Markdown 编写，位于 `docs/` 目录下。

## 常见问题

**Q: 资源探测是否会过度访问目标服务器，导致我被封禁或列入黑名单？**

A: 默认配置下，Nexus Resource Gateway 仅发送轻量级 HEAD 请求，并且每个目标 URL 在单次运行中只探测一次。并发数默认为 10，且探测间隔可配置 `--delay` 参数（单位秒）以降低请求频率。对于频繁运行（如每小时一次）的场景，建议设置 `--delay 2` 并启用 `--respect-robots` 标志，该标志会尝试解析目标域的 robots.txt 并遵循 Crawl-delay 指令。用户应自行评估目标服务器的承受能力，并遵守相关网站的使用条款。

**Q: 我如何将现有的浏览器书签或 CSV 文件导入到 Nexus Resource Gateway 的 YAML 清单格式？**

A: 项目提供了一个辅助转换脚本 `scripts/import_bookmarks.py`，支持从 Firefox/Chrome 导出的 HTML 书签文件、标准 CSV（列标题为 URL, title, tags）以及简单的每行一个 URL 的文本文件。运行 `python scripts/import_bookmarks.py --input bookmarks.html --output resources.yaml --format html` 即可完成转换。转换过程中会自动尝试提取页面标题作为默认描述，并允许用户通过 `--tag-mapping` 参数指定将书签文件夹映射为标签。

**Q: 生成的静态站点是否可以部署到 GitHub Pages 或任何纯静态托管服务？**

A: 完全可以。`nrg build` 命令生成的是完全静态的 HTML、CSS 和 JavaScript 文件，不依赖任何后端服务。所有过滤和搜索逻辑都在客户端浏览器中执行（基于预生成的 JSON 索引）。用户可以将 `./output` 目录下的全部内容推送到 GitHub Pages 分支、Netlify、Vercel 或任何 S3 兼容的对象存储中。我们官方文档的 `docs/recipes/github-actions.md` 章节提供了一个完整的 GitHub Actions 工作流示例，可实现每次推送清单变更后自动构建并部署到 GitHub Pages。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
