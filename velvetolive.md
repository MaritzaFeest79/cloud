# TechNav Resource Aggregator

TechNav is a lightweight, developer-oriented technical resource navigation and data aggregation toolkit designed for open-source maintainers, technical writers, and community managers who need to curate, validate, and present external reference links in a structured manner. The project addresses the common pain point of managing scattered third-party URLs across documentation, ensuring consistent formatting, automated link health checks, and standardized presentation layers for technical content.

Target users include documentation engineers, DevOps leads, and open-source project maintainers who frequently embed external references in README files, wiki pages, or technical blogs. TechNav provides a set of Python-based utilities and Markdown templates to enforce URL formatting rules, generate resource tables, and validate protocol consistency across large link inventories.

## 功能概览

- **URL 规范校验引擎** – 自动检测并修正裸域名、协议前缀、尾部斜杠及大小写不一致问题，支持批量链接清洗。

- **Markdown 目录树生成器** – 基于项目实际文件结构，递归生成带注释的 ASCII 目录树，便于文档同步更新。

- **资源清单表格化输出** – 将任意 URL 列表按类别分组，自动生成符合开源项目规范的 Markdown 表格或列表区块。

- **依赖环境嗅探模块** – 扫描当前系统 Python 版本、包管理器状态及网络连通性，生成安装要求表格。

- **场景化模板引擎** – 内置 6 套常见开源项目 README 章节模板（快速开始、贡献指南、FAQ 等），支持变量插值。

- **链接生命周期追踪** – 记录每个外部 URL 的首次引入时间、最近检查状态及响应码，输出健康报告。

- **批量协议规范化工具** – 针对混用 http/https/www/裸域名的链接库，一键统一为标准格式（不改变用户原始输入）。

## 应用场景

1. 开源项目文档维护：当项目 README 需要引用大量外部数据源（如镜像站、API 端点、参考文档）时，TechNav 可自动生成资源列表并校验每个链接的可达性，避免文档中出现过期或格式错误的 URL。

2. 技术博客批量整理：技术写作者在汇总多篇博文的外部引用时，可使用本工具将所有链接提取为统一格式的 Markdown 列表，并按域名或主题分类，显著减少手工校对时间。

3. 企业内部门户导航构建：运维团队需要维护一套内部技术工具链接页，TechNav 的模板引擎可根据不同部门（开发、测试、运维）生成差异化展示，同时保证所有链接遵循企业规定的协议与域名格式。

4. 开源社区资源聚合：社区经理在发布每月技术周刊或活动汇总时，通过本工具快速将数十个活动报名页、文档入口、视频回放链接整理为结构清晰的章节，提升读者体验。

5. 自动化 CI 流水线集成：将 TechNav 作为 CI 阶段的一个检查步骤，每次代码提交时自动扫描项目内所有 Markdown 文件中的外部链接，发现问题即中断构建并输出报告。

## 快速开始

```bash
# 克隆项目仓库
git clone https://github.com/technav-org/technav-core.git
cd technav-core

# 安装依赖（推荐使用虚拟环境）
python -m venv venv
source venv/bin/activate  # Windows 下使用 venv\Scripts\activate
pip install -r requirements.txt

# 运行完整资源校验（示例）
python technav.py --scan ./docs --output report.md

# 生成 README 资源章节模板
python technav.py --generate-resource-list --input urls.txt --output resources.md
```

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.8 及以上 | 核心运行环境，低于此版本将导致类型注解解析失败 |
| requests | 2.25.0+ | 用于外部链接可达性检查，支持超时与重试机制 |
| pyyaml | 5.4.0+ | 解析配置文件（.technav.yml），用于自定义校验规则 |
| markdown | 3.3.0+ | 提供 Markdown 语法树解析，辅助链接提取与位置定位 |
| pytest | 6.2.0+ | 仅开发测试需要，生产环境可不安装 |
| flake8 | 3.9.0+ | 代码风格检查工具，用于 CI 阶段静态分析 |
| virtualenv | 20.0.0+ | 推荐用于创建隔离的 Python 环境，避免包冲突 |
| git | 2.25.0+ | 版本控制工具，用于 clone 仓库及提交钩子 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | docs/user-guide/ | 如何安装、配置、运行 TechNav 的基本命令与参数详解 |
| 开发指南 | docs/developer-guide/ | 如何扩展校验器、添加自定义模板、提交 Pull Request |
| 配置参考 | docs/configuration/ | .technav.yml 中每个字段的含义、默认值及示例场景 |
| API 文档 | docs/api/ | 核心模块（validator, parser, generator）的函数签名与异常类型 |
| 常见问题 | docs/faq/ | 网络超时处理、URL 编码问题、大文件扫描性能优化等 |
| 变更日志 | CHANGELOG.md | 每个版本的新增功能、破坏性变更及已修复缺陷 |

## 资源列表

本部分汇总当前项目所引用的所有外部数据源与官方信息入口，按类别分组展示。所有链接均严格按照用户提供原始格式原样呈现，不添加任何协议前缀或路径修饰。

官方信息入口

<code>500jishibifen.net.cn</code>

<code>500zuqiubisaijieguo.net.cn</code>

赛事数据参考

<code>zuqiujishibifenwanchangbifen.org.cn</code>

比分官方渠道

<code>bifenguanfang.cn</code>

<code>bifenguanwang.net.cn</code>

<code>bifenguanfang.net.cn</code>

<code>bifenguanwang.org.cn</code>

## 项目结构

```
technav-core/
├── technav.py                 # 主入口 CLI，整合扫描、校验、生成三大流程
├── requirements.txt           # 生产环境依赖锁定文件（pip freeze 输出）
├── .technav.yml               # 全局配置文件，定义校验规则、超时阈值、输出格式
├── README.md                  # 项目总览文档（即本文档）
├── CHANGELOG.md               # 版本变更历史，按语义化版本记录
├── src/                       # 核心源代码目录
│   ├── validator/             # URL 校验引擎子模块
│   │   ├── __init__.py        # 暴露 validate_url, normalize_protocol 接口
│   │   ├── syntax.py          # 正则表达式与 URL 解析逻辑
│   │   └── network.py         # 基于 requests 的存活检测与重试策略
│   ├── parser/                # Markdown 解析与 AST 遍历模块
│   │   ├── __init__.py        # 提供 extract_links_from_md 函数
│   │   ├── md_ast.py          # 使用 markdown 库构建语法树
│   │   └── link_filter.py     # 按域名、协议、路径模式过滤链接
│   ├── generator/             # 输出生成器（表格、列表、目录树）
│   │   ├── __init__.py        # 暴露 generate_table, generate_tree, generate_list
│   │   ├── table_builder.py   # 根据列名与数据生成对齐的 Markdown 表格
│   │   └── tree_walker.py     # 递归遍历文件系统生成 ASCII 目录树
│   └── templates/             # 内置 README 章节模板（Jinja2 格式）
│       ├── quickstart.j2      # 快速开始章节模板，含变量占位符
│       ├── faq.j2             # 常见问题模板，支持多组 Q&A 循环
│       └── contributing.j2    # 贡献指南模板，包含 PR 流程指引
├── tests/                     # 单元测试与集成测试用例
│   ├── test_validator.py      # 覆盖裸域名、带协议、带路径等 30+ 场景
│   ├── test_parser.py         # 验证 AST 提取链接的准确性与边界情况
│   └── fixtures/              # 测试用的示例 Markdown 文件与预期输出
├── docs/                      # 完整文档站点源码（用于生成 HTML 文档）
│   ├── user-guide/            # 分章节的用户手册（安装、配置、命令行）
│   ├── developer-guide/       # 面向贡献者的开发环境搭建与测试指南
│   └── api/                   # 由 Sphinx 自动生成的 API 引用（未编译）
└── scripts/                   # 辅助运维脚本（非核心功能）
    ├── pre-commit-hook.sh     # Git pre-commit 钩子，自动运行链接检查
    └── weekly-report.py       # 定时生成链接健康周报并发送邮件（示例）
```

## 贡献指南

1.  fork 本仓库至个人账户，然后克隆到本地开发环境。请确保使用 Python 3.8+ 并创建独立的虚拟环境。运行 `make setup-dev`（或手动执行 `pip install -r requirements-dev.txt`）安装所有开发依赖。

2.  在提交代码前，请先阅读 `docs/developer-guide/architecture.md` 了解核心模块的设计思路与数据流向。所有新增功能需对应添加单元测试（位于 `tests/` 目录），且测试覆盖率不得低于 85%。

3.  提交 Pull Request 时，请遵循 Conventional Commits 规范（如 `feat:`, `fix:`, `docs:`）。PR 描述中需清晰说明解决的问题、实现方案以及手动测试步骤。若涉及外部链接校验规则的变更，必须提供至少 5 个真实 URL 示例作为验证依据。

4.  接受 PR 后，项目维护者将合并至 `main` 分支，并触发 CI 流水线（包含 lint, test, build）。发布新版本时，会同步更新 `CHANGELOG.md` 并打上 Git tag。

## 常见问题

**Q: 为什么校验某些裸域名（如 abc.com）时会提示协议缺失，但规范要求不能自动补全 http://？**

A: TechNav 的设计原则是「保留用户原始输入，但发出警告」。校验引擎会检测到缺少协议，并在日志中输出 WARNING 级别信息，同时将链接标记为 `needs_review` 状态。用户可根据项目文档规范自行决定是否添加协议前缀。如需强制统一协议，可在 `.technav.yml` 中设置 `strict_protocol: true`，此时未带协议的链接将被视为校验失败。

**Q: 如何处理链接中包含中文或特殊空格字符的情况？**

A: 解析器会自动对链接文本进行百分号编码（UTF-8），但不会修改原始显示文本。对于 Markdown 链接语法 `[text](url)`，我们仅提取 `url` 部分进行校验，`text` 部分保留原样。若校验时遇到无效编码，工具会尝试使用 `repr()` 输出原始字节并跳过该条目，同时生成一份单独的错误报告文件。

**Q: 项目是否支持对 GitHub、GitLab 等平台的内网或私有仓库链接进行验证？**

A: 网络校验模块默认仅对公网地址（判定依据为是否包含内网 IP 段或保留域名）执行 HTTP 请求。对于 `localhost`、`127.0.0.1`、`*.local` 等内网地址，校验器会跳过网络检查并标记为 `skip` 状态。用户可通过 `--allow-private` 参数强制启用内网探测，但需自行确保网络可达。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:20
