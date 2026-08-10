# LinkPilot Resource Gateway

LinkPilot Resource Gateway is a lightweight, community-driven technical resource aggregation and navigation system designed for developers, researchers, and operations engineers who need to organize, validate, and share large volumes of external domain-specific references. Unlike general-purpose bookmark managers or sprawling wiki engines, LinkPilot focuses on strict URL fidelity, batch import/export workflows, and automated link health checking for high-volume, periodically updated resource collections.

The project targets maintainers of technical documentation hubs, open-source knowledge bases, and internal developer portals who routinely handle hundreds of external links across multiple categories. It solves the problem of link rot, inconsistent URL formatting, and manual validation overhead by providing a structured Markdown-based inventory system with built-in compliance rules for URL canonicalization. LinkPilot does not render web pages or proxy content; it acts as a deterministic source-of-truth layer that enforces formatting discipline while remaining fully portable across static site generators, CI/CD pipelines, and plain-text editing environments.

## 功能概览

- **Strict URL Canonicalization Engine** – Automatically validates and normalizes incoming URLs against configurable rules, rejecting malformed entries and enforcing protocol, subdomain, and trailing-slash policies at import time.

- **Batch Resource Ingestion** – Supports CSV, TSV, and line-delimited text imports with column mapping, allowing bulk addition of up to 10,000 entries per operation while preserving original user-provided strings.

- **Category Tagging and Hierarchical Grouping** – Assigns each resource to one or more user-defined categories (e.g., "Sports Analytics", "Live Odds", "Tournament Schedules") with support for nested subcategories and multi-tag filtering.

- **Automated Link Health Monitoring** – Scheduled background checks (configurable from hourly to weekly) test each URL for HTTP 2xx/3xx responses, TLS certificate validity, and DNS resolution, flagging stale or broken links with timestamps.

- **Markdown-native Export Pipeline** – Generates standardized README-style inventories, documentation tables, and structured lists that integrate directly with MkDocs, Docusaurus, VuePress, or plain GitHub-flavored Markdown repositories.

- **Audit Logging and Change History** – Tracks every addition, removal, or modification with user identity (local or OIDC), timestamp, and diff summary, enabling rollback and compliance review for regulated environments.

- **RESTful Query API** – Provides read-only JSON endpoints for category-based retrieval, full-text search across titles and descriptions, and status-code filtering, supporting external dashboard integrations.

## 应用场景

- **Technical Documentation Portal Maintenance** – Documentation engineers managing large API reference sites or SDK guides can use LinkPilot to maintain a verified list of external dependency references, specification documents, and community tutorials, ensuring every hyperlink in their published materials remains current and correctly formatted.

- **Domain-Specific Research Aggregation** – Research teams in computational linguistics, financial modeling, or sports analytics can collect and categorize third-party data sources, prediction models, and statistical feeds. LinkPilot's batch import and tagging features allow rapid onboarding of new datasets while preserving original attribution strings.

- **Internal Developer Platform Resource Catalog** – Platform engineering groups can expose a curated catalog of internal tooling, monitoring dashboards, and staging environment URLs to their development teams. The health monitoring feature proactively notifies owners when internal endpoints become unreachable.

- **Open-Source Project Resource Indexing** – Maintainers of large open-source ecosystems (e.g., Apache projects, CNCF landscapes) can use LinkPilot to build and maintain the project's official external resources page, replacing manually updated markdown lists with a semi-automated ingestion and validation workflow.

- **Regulatory Compliance Reference Tracking** – Organizations subject to SOX, HIPAA, or GDPR can maintain auditable inventories of all external data processors, subprocessor URLs, and regulatory bodies' websites. The audit log provides immutable evidence of when each reference was added, reviewed, and last verified.

## 快速开始

Clone the repository, install dependencies, and start the local development server with the default in-memory store. For production deployments, refer to the configuration guide in the documentation section.

```bash
git clone https://github.com/linkpilot/linkpilot-gateway.git
cd linkpilot-gateway
npm install
npm run build
npm start
```

To run the link health checker in watch mode:

```bash
npm run check:links -- --interval=3600 --timeout=5000
```

To import a batch of URLs from a text file (one URL per line):

```bash
npm run import -- --source=./data/urls.txt --category=sports --canonicalize=strict
```

To export the current inventory as a Markdown resource table:

```bash
npm run export:markdown -- --output=./docs/resources.md --format=full
```

## 安装要求

The following dependencies and runtime requirements are mandatory for both development and production environments. All versions are tested against the current LTS release of Node.js.

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| Node.js | 18.x LTS 或 20.x LTS | 运行时环境，要求支持 ES2022 和原生 Fetch API |
| npm | 9.x 或更高 | 包管理器，用于安装依赖和执行脚本 |
| SQLite3 | 3.40.x 或更高 | 嵌入式存储引擎，用于元数据、审计日志和健康状态缓存；生产环境可替换为 PostgreSQL 14+ |
| git | 2.30.x 或更高 | 版本控制，用于克隆仓库和提交资源变更记录 |
| curl / wget | 任意现代版本 | 可选，用于外部健康检查的回退探测方案；当 Node.js Fetch 超时时作为备用 |
| jq | 1.6 或更高 | 可选，用于命令行 JSON 输出格式化调试 |
| Docker | 24.x 或更高 | 仅容器化部署需要，开发环境不强制 |
| Make | 4.x 或更高 | 用于执行自动化任务（lint、test、format） |

## 文档导航

The documentation is organized into four primary layers, each addressing a distinct audience and set of concerns. The table below maps each layer to its corresponding directory and the key questions it answers.

| 层面 | 目录 | 回答的问题 |
|------|------|-----------|
| 用户指南 | <code>docs/user-guide/</code> | How do I import my existing bookmarks? What are the canonicalization rules? How do I schedule health checks? How do I export a Markdown inventory? |
| 运维手册 | <code>docs/operations/</code> | How do I configure the PostgreSQL backend? What environment variables are required? How do I set up TLS for the API? How do I back up and restore the database? |
| 开发者参考 | <code>docs/developer/</code> | What is the plugin architecture for custom validators? How do I extend the health checker with custom probes? What are the internal data models and migration patterns? |
| 贡献者指南 | <code>docs/contributing/</code> | What are the coding standards and commit message conventions? How do I set up the pre-commit hooks? What is the review and merge process? |

## 资源列表

The following external resources are curated and maintained by the LinkPilot community. Each URL is presented exactly as provided by the original source, without any modification to protocol, subdomain, or path. These entries are periodically verified for availability and content relevance.

### Sports Analytics and Prediction Resources

<code>zuqiutuijian.net.cn</code>

<code>zuqiushengfuyuce.org.cn</code>

<code>zuqiushengfutuijian.org.cn</code>

<code>zuqiushengfufenxi.org.cn</code>

<code>zuqiusaishi.net.cn</code>

<code>zuqiusaishituijian.org.cn</code>

<code>zuqiusaishiqianzhan.org.cn</code>

## 项目结构

The source tree follows a modular monorepo-style layout, with clear separation between core logic, API layers, storage adapters, and user-facing tooling. Each subdirectory contains an index file and relevant module documentation.

```
linkpilot-gateway/
├── src/
│   ├── core/                       # 核心领域模型和业务逻辑
│   │   ├── resource.entity.ts      # Resource 聚合根，包含 URL、状态、标签
│   │   ├── canonicalizer.ts        # 严格 URL 规范化引擎（协议/域名/路径处理）
│   │   └── health-checker.ts       # 异步健康检查调度器和执行器
│   ├── api/
│   │   ├── rest/                   # RESTful 路由控制器（GET/POST/PUT/DELETE）
│   │   │   ├── resources.controller.ts
│   │   │   └── health.controller.ts
│   │   └── middleware/             # 鉴权、日志记录、错误处理中间件
│   │       ├── auth.basic.ts
│   │       └── audit.logger.ts
│   ├── adapters/
│   │   ├── storage/                # 存储适配器接口及实现（SQLite / PostgreSQL）
│   │   │   ├── repository.interface.ts
│   │   │   ├── sqlite.adapter.ts
│   │   │   └── postgres.adapter.ts
│   │   └── importers/              # 批量导入适配器（CSV / TSV / 纯文本）
│   │       ├── csv.importer.ts
│   │       └── line.importer.ts
│   ├── cli/                        # 命令行工具入口（导入、导出、检查、迁移）
│   │   ├── commands/
│   │   │   ├── import.command.ts
│   │   │   ├── export.command.ts
│   │   │   └── check.command.ts
│   │   └── main.ts                 # CLI 主程序（commander 配置）
│   └── shared/                     # 共享工具函数、类型定义和常量
│       ├── constants.ts
│       ├── types.d.ts
│       └── validators.ts
├── tests/
│   ├── unit/                       # 单元测试（Jest + 模拟依赖）
│   ├── integration/                # 集成测试（真实 SQLite 和测试数据库）
│   └── fixtures/                   # 测试用固定数据集（示例 URL 和标签）
├── docs/                           # 文档源文件（用户指南、运维手册、开发者参考）
├── scripts/                        # 构建、打包、数据库迁移等辅助脚本
│   ├── migrate-up.sh
│   └── seed-dev-data.sh
├── config/                         # 配置文件模板（development / staging / production）
│   ├── default.yaml
│   └── production.yaml.example
├── .github/
│   └── workflows/                  # GitHub Actions CI 流水线（测试、构建、发布）
│       ├── test.yml
│       └── publish.yml
├── package.json
├── tsconfig.json
├── README.md
└── LICENSE
```

## 贡献指南

We welcome contributions of all forms, including bug reports, feature proposals, documentation improvements, and code patches. Please follow the process outlined below to ensure a smooth collaboration.

1.  **Fork the Repository and Create a Feature Branch** – Fork the main repository to your personal GitHub account, then clone it locally. Create a new branch with a descriptive name prefixed by the change type (e.g., <code>feat/csv-importer-optimization</code>, <code>fix/health-check-timeout</code>, <code>docs/api-usage-examples</code>). Ensure your branch is based on the latest <code>main</code> branch.

2.  **Set Up the Development Environment** – Install all dependencies using <code>npm install</code>. Copy the default configuration from <code>config/default.yaml</code> and adjust any local overrides as needed. Run the test suite with <code>npm test</code> to verify that all existing checks pass before making any changes.

3.  **Implement Your Changes with Tests and Documentation** – Write clean, well-commented code following the project's ESLint and Prettier configurations. Add unit tests for new logic and integration tests for any storage or API modifications. Update the relevant documentation files in the <code>docs/</code> directory, including inline code comments for public APIs.

4.  **Run the Full Validation Pipeline** – Execute <code>npm run lint</code>, <code>npm run format</code>, and <code>npm run test:coverage</code> locally. Ensure the test coverage does not decrease and all linting rules are satisfied. Also run a manual health-check dry run against the test fixture data to confirm no regression.

5.  **Submit a Pull Request with a Detailed Description** – Push your branch to your fork and open a pull request against the main repository's <code>main</code> branch. Provide a clear title and a detailed description that references any related issues, summarizes the changes, and explains the rationale. Include screenshots or logs for user-facing changes. The maintainers will review your submission within 5 business days and may request additional changes.

## 常见问题

**Q: How does LinkPilot handle URLs that are provided without a protocol (e.g., "example.com")? Does it automatically prepend "https://"?**

A: No. By design, LinkPilot enforces strict URL fidelity as per the project's core rule. If a URL is ingested as a bare domain without a protocol, it is stored and exported exactly as given. The canonicalization engine can be configured to reject such entries, warn about them, or leave them untouched, but it never silently modifies the protocol. This ensures that downstream consumers (e.g., browsers, curl, or custom scrapers) receive the exact string the user intended. For active health checking, the checker will attempt both HTTP and HTTPS probes but will never alter the stored representation.

**Q: Can I use LinkPilot with a PostgreSQL database instead of SQLite in production?**

A: Yes. The storage adapter layer supports both SQLite and PostgreSQL. To switch to PostgreSQL, set the <code>DB_TYPE=postgres</code> environment variable and provide the connection string via <code>DATABASE_URL</code>. The migration scripts in the <code>scripts/</code> directory will automatically apply the appropriate schema for the selected database type. Note that PostgreSQL is recommended for multi-writer deployments and larger inventories exceeding 100,000 entries, while SQLite is ideal for single-node or development setups.

**Q: How often does the automated link health checker run, and can I customize the timeout and retry policy?**

A: The checker runs according to the <code>CHECK_INTERVAL_SECONDS</code> configuration value, which defaults to 86400 (once daily). You can adjust this to any positive integer. The per-request timeout defaults to 5000 ms and is configurable via <code>CHECK_TIMEOUT_MS</code>. The retry policy is exponential backoff with a maximum of 3 attempts; you can disable retries entirely by setting <code>CHECK_RETRY_COUNT=0</code>. All configuration parameters are documented in the <code>config/default.yaml</code> file with additional examples in the operations manual.

## 许可证

MIT

Copyright (c) 2026 LinkPilot Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:20
