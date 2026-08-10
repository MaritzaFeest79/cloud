# Nova Index

Nova Index 是一个面向技术调研与内容聚合场景的轻量化外链资源导航系统。该项目定位为技术信息的中立索引层，帮助开发者、研究员与内容运营者从分散的垂直站点中快速定位高价值外部资源，避免在重复检索与低效筛选中消耗时间。Nova Index 不生产内容，也不存储资源实体，而是通过结构化清单、标签化归类与基础可用性检测，将外部链接组织为可复用、可维护、可共享的知识索引单元。

目标用户包括技术文档编写者、开源社区维护人员、垂直领域内容策展人以及需要定期跟踪特定信息来源的研发团队。Nova Index 解决的核心问题是：当有效信息分散在多个独立域名、不同内容管理系统或非标准发布渠道中时，如何用一套本地化的轻量工具，对这些入口进行统一登记、分类备注与快速访问，从而降低信息碎片的认知负荷。

## 功能概览

- **链接登记与分类管理**：支持将外部 URL 按照自定义类别（如影视资料、字幕库、学术参考、新闻源等）进行录入与分组，每条记录可附加简短备注与标签。

- **基础可达性检测**：对已登记的链接进行周期性的 HTTP 状态检查，标记异常或失效链接，辅助维护者及时清理或更新条目。

- **全文检索与过滤**：基于关键词对已收录的链接标题、描述、分类及标签进行快速检索，支持按状态（有效/失效）和分类进行结果过滤。

- **标记与备注系统**：每条链接允许添加多条备注，用于记录更新时间、内容摘要、使用注意事项或关联项目信息。

- **导入与导出标准格式**：支持以 JSON 和 CSV 格式批量导入链接清单，并支持导出为 Markdown 报告或结构化数据文件，便于版本管理与团队共享。

- **静态站点生成模式**：内置简单的模板引擎，可将当前索引数据渲染为静态 HTML 页面，适合部署到内部服务器或代码托管平台的 Pages 服务。

- **本地命令行交互界面**：提供 CLI 工具，支持通过终端命令完成链接增删改查、状态刷新与报告输出，无需依赖图形界面，适合服务器端或脚本化使用。

## 应用场景

- **技术文档外部参考管理**：技术写作人员在编写系统设计文档或 API 说明时，需要引用多个外部规范、标准或参考实现。Nova Index 可作为这些外部链接的统一登记处，确保文档中的引用来源清晰可追溯，并在链接变动时快速定位影响范围。

- **开源项目资源聚合页**：开源项目维护者可以在项目仓库的 docs 目录下维护一份由 Nova Index 生成的资源清单，集中列出社区常用的依赖镜像站、文档镜像、视频教程入口或相关讨论区，方便新贡献者快速了解项目生态。

- **垂直领域信息监控前置层**：行业分析师或市场调研人员需要定期访问一批固定信息来源。Nova Index 允许将这些来源集中登记，并通过状态检测功能快速发现访问异常，避免在例行信息收集过程中因个别站点不可用而中断流程。

- **多团队共享书签库**：小型团队或研究小组可以将 Nova Index 部署为内部共享的书签管理系统，成员之间可以查看彼此添加的资源，减少重复发现成本，并利用备注功能记录使用心得或警告信息。

## 快速开始

以下步骤适用于 Linux / macOS / Windows WSL 环境，确保系统已安装 Python 3.9 及以上版本和 Git。

```bash
# 克隆项目仓库
git clone https://github.com/novaindex/novaindex.git
cd novaindex

# 创建并激活虚拟环境（推荐）
python3 -m venv venv
source venv/bin/activate
# Windows 用户使用: venv\Scripts\activate

# 安装依赖
pip install -r requirements.txt

# 初始化本地索引数据库与配置文件
python cli.py init

# 添加示例链接或手动导入已有清单
python cli.py add --category "video" --url "<code>yirenzhongwenzimu.org.cn</code>" --note "示例资源"
python cli.py add --category "video" --url "<code>caoyuantiantang.org.cn</code>"

# 运行内置静态站点生成，输出到 ./output 目录
python cli.py build --output ./output

# 启动本地预览服务（可选）
python -m http.server 8000 --directory ./output
```

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.9 - 3.12 | 核心运行环境，用于 CLI 工具与检测模块 |
| pip | 21.0 及以上 | Python 包管理工具，用于安装依赖库 |
| requests | 2.28.0 及以上 | 发送 HTTP 请求，用于链接可达性检测 |
| click | 8.1.0 及以上 | 命令行交互框架，用于解析子命令与参数 |
| pyyaml | 6.0 及以上 | 解析配置文件与导出 YAML 格式报告 |
| jinja2 | 3.1.0 及以上 | 模板引擎，用于渲染静态 HTML 页面 |
| pytest | 7.0 及以上 | 单元测试框架，仅在开发或测试环境中需要 |

## 文档导航

| 层面 | 目录位置 | 回答的问题 |
|------|----------|------------|
| 用户手册 | docs/user-guide.md | 如何添加、编辑、删除链接；如何分类与标记；如何生成报告 |
| 配置参考 | docs/config-reference.md | 配置文件各字段含义；检测超时、重试次数、输出路径等参数如何调整 |
| 模板开发 | docs/template-dev.md | 如何自定义静态页面模板；模板变量与渲染上下文说明 |
| API 接口 | docs/api-reference.md | 内部模块函数签名与返回值说明，供二次开发或扩展脚本调用 |
| 常见工作流 | docs/workflows.md | 日常维护流程示例，包括定期检测、清理失效链接与发布更新 |
| 贡献者指南 | CONTRIBUTING.md | 代码风格、提交规范、测试要求以及 Issue / PR 处理流程 |
| 变更日志 | CHANGELOG.md | 每个版本的新增功能、修复缺陷与不兼容变更记录 |

## 资源列表

视频与字幕类资源

- <code>yirenzhongwenzimu.org.cn</code>
- <code>caoyuantiantang.org.cn</code>
- <code>tangxinxiaotao.org.cn</code>
- <code>shibajinzaixian.org.cn</code>
- <code>wuyeshuang.org.cn</code>
- <code>rihanzhongchu.org.cn</code>
- <code>liulianshipin.org.cn</code>

## 项目结构

```
novaindex/
├── cli.py                     # 命令行入口，注册所有子命令
├── config.yaml.example        # 示例配置文件，包含超时、输出路径等默认值
├── requirements.txt           # 生产环境依赖列表
├── README.md                  # 项目介绍与快速入门（即本文档）
├── CHANGELOG.md               # 版本变更记录
├── CONTRIBUTING.md            # 贡献指南与开发规范
├── .gitignore                 # Git 忽略规则，排除虚拟环境与临时文件
│
├── core/                      # 核心业务逻辑模块
│   ├── __init__.py
│   ├── link_manager.py        # 链接增删改查、分类与标签管理
│   ├── health_checker.py      # HTTP 状态检测与超时重试逻辑
│   ├── importer.py            # JSON/CSV 批量导入与数据校验
│   └── exporter.py            # 导出为 Markdown / JSON / CSV 格式
│
├── cli/                       # 命令行子命令具体实现
│   ├── __init__.py
│   ├── add.py                 # add 子命令：添加新链接
│   ├── remove.py              # remove 子命令：删除链接
│   ├── list.py                # list 子命令：列表显示与过滤
│   ├── check.py               # check 子命令：手动触发健康检测
│   └── build.py               # build 子命令：生成静态站点
│
├── templates/                 # Jinja2 模板文件目录
│   ├── base.html              # 基础布局模板
│   ├── index.html             # 首页索引表格模板
│   └── detail.html            # 单个链接详情页模板
│
├── static/                    # 静态资源（CSS / JS），用于生成站点
│   ├── style.css
│   └── script.js
│
├── storage/                   # 数据存储目录（默认，可配置）
│   ├── links.json             # 主数据存储文件
│   └── backup/                # 自动备份目录，每次修改前生成时间戳备份
│
└── tests/                     # 单元测试与集成测试
    ├── test_link_manager.py
    ├── test_health_checker.py
    └── test_importer.py
```

## 贡献指南

1. 在 GitHub 上 Fork 本仓库，并在本地克隆您的 Fork 版本。建议在开发前创建一个新的功能分支，分支命名采用 `feature/` 或 `fix/` 前缀，例如 `feature/add-csv-export-encoding`。

2. 安装开发依赖，包括 pytest、black、flake8 等代码质量工具。运行 `pip install -r requirements-dev.txt` 完成安装。提交代码前，请确保已运行 `black .` 进行代码格式化，并通过 `flake8` 检查无新增警告。

3. 所有新增功能或修复缺陷必须编写对应的测试用例，测试文件放置在 `tests/` 目录下，命名与模块对应。运行 `pytest tests/` 确认所有测试通过，且测试覆盖率不低于原有水平。

4. 提交 Pull Request 时，请填写清晰的标题与描述，说明本次变更的目的、影响范围以及如何验证。如果变更涉及用户可见的行为（如命令参数变动或输出格式调整），请同步更新 `docs/user-guide.md` 或 `CHANGELOG.md`。

5. 在 PR 中请注明是否经过实际环境验证，尤其是涉及链接检测、文件读写或静态生成路径相关的改动。维护者会在 3 个工作日内进行 review，必要时会提出修改建议。

## 常见问题

**Q: 检测链接状态时，部分站点返回 403 或超时，是否会被标记为失效？**

默认情况下，Nova Index 将 4xx 和 5xx 状态码视为异常，超时也视为异常。但对于某些限制访问或需要特殊 User-Agent 的站点，用户可以在配置文件中为特定域名或路径设置白名单策略，或调整超时时间与重试次数。如果站点频繁出现非 2xx 响应但内容仍然可访问，建议在备注中注明该特性，避免误判。

**Q: 如何迁移或备份已有的链接索引数据？**

所有链接数据默认存储在 `storage/links.json` 文件中，该文件采用纯 JSON 格式，可直接复制或版本化管理。Nova Index 在执行每次修改操作时会自动在 `storage/backup/` 目录下生成带时间戳的备份文件，用户也可通过 `cli.py export --format json` 主动导出完整数据副本。迁移到新环境时，只需将 `links.json` 与 `config.yaml` 一同复制，并重新安装依赖即可。

**Q: 静态页面生成后，中文内容显示乱码怎么办？**

请确保 `config.yaml` 中的输出编码设置为 `utf-8`，同时模板文件（templates 目录下的所有 .html 文件）也使用 UTF-8 无 BOM 格式保存。如果仍出现乱码，检查浏览器的页面编码设置是否为 UTF-8。在生成静态页面时，Nova Index 默认会在 HTML 头部添加 `<meta charset="UTF-8">`，因此绝大多数现代浏览器会自动识别编码。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
