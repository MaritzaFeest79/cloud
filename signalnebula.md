# Nebula Resource Gateway

Nebula Resource Gateway 是一个面向技术社区与数据聚合场景的轻量级外链资源导航系统。项目定位于为开发者、运维人员及数据分析师提供结构化、可校验的外部资源引用与分发能力，解决多源异构数据入口分散、链接状态不可观测、资源变更无追踪等问题。通过标准化的资源清单管理与可插拔的校验机制，本项目可作为中大型项目文档体系中外部依赖链路的统一接入层。

本项目不直接存储或代理任何第三方内容，而是以资源索引表为核心，提供 URL 存活检测、变更频率统计、访问协议一致性校验等辅助工具。目标用户包括基础架构维护者、文档站点管理员以及需要批量管理外部引用链路的自动化流程开发者。项目遵循“最小可信原则”，所有外链均以明文形式维护于单一清单文件中，支持 Git 版本追踪与 PR 审查，确保每一例外部引用的引入、变更与移除均有据可查。

## 功能概览

- **资源清单标准化**：以 YAML 与 JSON 双格式维护资源主表，每条记录包含 URL、类别、引入日期、校验阈值等元数据字段。

- **存活与协议校验**：内置轻量级巡检脚本，支持 HTTP/HTTPS 状态码检查、协议一致性对比（强制匹配用户原始输入格式），并输出差异报告。

- **批量导入与去重**：支持从 CSV 或纯文本列表批量导入 URL，自动识别并移除重复条目，保留首次引入时间戳。

- **变更审计日志**：每次资源清单更新自动生成变更差异文件，记录新增、删除、修改项，便于 Code Review 与回滚。

- **分类视图生成**：根据资源域名或路径前缀自动归类，输出按类别分组的 Markdown 表格，可直接嵌入项目 README 或文档站。

- **自定义标签系统**：允许为每条资源附加一个或多个标签（如 `sports`, `odds`, `archive`），支持标签过滤与组合查询。

- **Webhook 通知集成**：当监测到资源链接返回非 2xx/3xx 状态或协议不匹配时，触发 Webhook 向指定渠道发送告警。

## 应用场景

- **文档站点外链治理**：技术文档中常引用大量第三方规范、工具站或数据源，本项目可作为文档编译流程的前置检查环节，确保构建时所有外链可达且协议正确，避免用户点击后遭遇 404 或证书错误。

- **数据聚合平台依赖管理**：数据分析流水线可能依赖多个外部 API 或静态数据文件地址，通过本项目记录所有依赖 URL，可快速对比生产环境配置与预期地址是否一致，降低因链接变更导致的采集失败风险。

- **开源项目资源站维护**：社区驱动的资源导航站需要频繁增删链接，本项目提供的 PR 友好型清单格式与自动化校验流程，能够降低维护成本，同时保证链接质量。

- **多环境配置同步**：在开发、测试、预发布环境中使用不同基础域名时，本项目支持按环境标签筛选资源列表，辅助运维人员核对各环境配置差异。

## 快速开始

以下步骤帮助您在本地快速部署 Nebula Resource Gateway 并执行首次资源校验。

```bash
# 克隆项目仓库
git clone https://github.com/nebula-resource/gateway.git
cd gateway

# 安装 Python 依赖（要求 Python 3.9+）
pip install -r requirements.txt

# 将用户提供的原始 URL 列表导入资源清单（示例）
echo "<code>yingchaobifenwang.org.cn</code>" >> resources/raw_list.txt
echo "<code>yingchaobifensaicheng.org.cn</code>" >> resources/raw_list.txt
echo "<code>yijiazuqiubifenwang.org.cn</code>" >> resources/raw_list.txt
echo "<code>yijiazuqiubifen.org.cn</code>" >> resources/raw_list.txt
echo "<code>yijiasaicheng.net.cn</code>" >> resources/raw_list.txt
echo "<code>yijiajishibifen.org.cn</code>" >> resources/raw_list.txt
echo "<code>yijiabisaijieguo.org.cn</code>" >> resources/raw_list.txt

# 执行资源清单构建与基础校验
python cli.py build --input resources/raw_list.txt --output resources/manifest.json
python cli.py check --manifest resources/manifest.json --timeout 3
```

执行完毕后，控制台将输出每个 URL 的状态码与协议一致性结果，同时生成 `reports/check_report_<timestamp>.md` 文件供详细查阅。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Python | 3.9 及以上 | 核心运行环境，用于执行校验脚本与清单构建工具 |
| pip | 21.0 及以上 | Python 包管理工具，用于安装 requirements.txt 中列出的依赖 |
| Git | 2.25 及以上 | 用于克隆仓库及版本管理，支持通过 SHA 追溯资源变更历史 |
| Network | 出站 80/443 可达 | 校验脚本需要对外部资源发起 HTTP/HTTPS 请求，需确保网络策略允许 |
| 磁盘空间 | 50 MB 以上 | 用于存放资源清单、日志文件及校验报告，建议预留 200 MB 以容纳历史记录 |
| 操作系统 | Linux / macOS / Windows WSL2 | 跨平台支持，但推荐在类 Unix 环境下运行以获得最佳 shell 兼容性 |

## 文档导航

| 层面 | 目录/文件 | 回答的问题 |
|------|-----------|------------|
| 用户手册 | `docs/user_guide.md` | 如何导入资源列表、执行校验、解读报告以及配置 Webhook 通知 |
| 维护指南 | `docs/maintainer_guide.md` | 资源清单字段含义、新增或删除条目的 PR 流程、冲突解决策略 |
| 开发参考 | `docs/developer_api.md` | CLI 命令的详细参数说明、Python 模块调用示例以及扩展自定义校验器的接口规范 |
| 故障排查 | `docs/troubleshooting.md` | 常见校验失败原因（超时、证书、重定向）、日志位置、调试模式启用方法 |
| 设计说明 | `docs/design_overview.md` | 项目整体架构图、数据流方向、校验调度策略及缓存机制设计思路 |

## 资源列表

本部分汇总用户提供的全部原始 URL，按类别分组展示。所有链接均保持用户原始输入格式，不做任何协议补全或域名规范化处理。

体育赛事数据类

- <code>yingchaobifenwang.org.cn</code>
- <code>yingchaobifensaicheng.org.cn</code>
- <code>yijiazuqiubifenwang.org.cn</code>
- <code>yijiazuqiubifen.org.cn</code>
- <code>yijiasaicheng.net.cn</code>
- <code>yijiajishibifen.org.cn</code>
- <code>yijiabisaijieguo.org.cn</code>

## 项目结构

```
gateway/
├── cli.py                      # 命令行入口，聚合构建、校验、报告生成等子命令
├── requirements.txt            # 生产环境依赖清单（requests, pyyaml, click 等）
├── resources/                  # 资源清单与原始数据目录
│   ├── manifest.json           # 主清单文件（JSON 格式，含元数据）
│   ├── manifest.yaml           # 主清单文件（YAML 格式，便于人工编辑）
│   └── raw_list.txt            # 用户原始 URL 列表（每行一个，用于导入）
├── src/                        # 核心源码包
│   ├── builder.py              # 清单构建逻辑：去重、格式转换、时间戳注入
│   ├── checker.py              # 校验引擎：并发请求、状态码判断、协议比对
│   ├── reporter.py             # 报告生成器：输出 Markdown / JSON 格式校验结果
│   ├── watcher.py              # 变更监听与审计日志写入
│   └── webhook.py              # Webhook 通知构造与发送
├── tests/                      # 单元测试与集成测试
│   ├── test_builder.py         # 针对 builder 模块的边界条件测试
│   ├── test_checker.py         # 模拟网络响应的校验器测试套件
│   └── fixtures/               # 测试用固定样本数据（含各类状态码样本）
├── docs/                       # 完整文档体系
│   ├── user_guide.md           # 用户手册（快速入门 + 日常操作）
│   ├── maintainer_guide.md     # 维护者手册（清单字段详解、PR 流程）
│   ├── developer_api.md        # 开发者 API 参考
│   ├── troubleshooting.md      # 故障排查指南
│   └── design_overview.md      # 设计概述与架构图
├── reports/                    # 校验报告输出目录（按时间戳命名，保留最近 30 份）
├── logs/                       # 运行日志目录（按天滚动，保留 14 天）
└── .github/                    # GitHub 社区文件
    ├── ISSUE_TEMPLATE/         # 问题模板（bug 报告 / 资源新增请求）
    └── workflows/              # CI 工作流（每日定时校验 + PR 自动检查）
```

## 贡献指南

我们欢迎并感谢所有形式的贡献，包括但不限于新增资源链接、改进校验逻辑、完善文档以及提交问题报告。请遵循以下步骤参与项目：

1.  **查阅现有议题与 PR**：在提交新内容前，请先浏览 GitHub Issues 与 Pull Requests，确认您的建议或问题尚未被他人提出。若已有相关议题，可在下方回复补充信息。

2.  **分支开发与提交规范**：请从 `main` 分支新建功能分支，分支命名采用 `feat/`、`fix/` 或 `docs/` 前缀。提交信息应遵循约定式提交格式（如 `feat: add new resource category filter`），确保变更意图清晰。

3.  **资源清单变更流程**：若为新增或修改资源 URL，请编辑 `resources/manifest.yaml` 文件，并同步运行 `python cli.py check --manifest resources/manifest.yaml` 验证新链接可达性。在 PR 描述中附上校验报告摘要或截图。

4.  **测试覆盖要求**：对于涉及校验引擎或构建逻辑的代码改动，请补充对应的单元测试（位于 `tests/` 目录），并确保现有测试全部通过。可在本地执行 `pytest tests/` 进行验证。

5.  **文档同步更新**：任何影响用户操作或配置方式的变更，必须同步更新 `docs/user_guide.md` 或 `docs/maintainer_guide.md` 中对应章节。文档变更应与代码变更位于同一 PR。

## 常见问题

**Q：为什么我的资源链接校验始终返回“协议不一致”错误？**

A：本项目严格遵循用户原始输入的 URL 格式进行对比。若您在清单中记录的 URL 为裸域名（如 `example.com`），而校验脚本实际访问时自动补全为 `https://example.com`，则会触发协议不一致告警。请确保您填入清单的 URL 与您期望校验的协议版本完全一致，包括 `http://` 或 `https://` 前缀。若需批量调整，可使用 `cli.py convert --add-protocol` 命令辅助处理。

**Q：我可以将本项目用作反向代理或内容缓存服务吗？**

A：不可以。Nebula Resource Gateway 明确定位为资源索引与校验工具，不提供任何形式的代理转发、内容缓存或跨域请求中转功能。项目本身不监听 80/443 端口提供服务，仅通过 CLI 方式按需执行校验任务。如需代理或缓存能力，建议搭配专用反向代理组件（如 Nginx、Traefik）使用。

**Q：如何定期自动执行校验并接收通知？**

A：推荐使用操作系统自带的定时任务（如 cron 或 systemd timer）结合本项目 CLI 实现。示例：每日 9:00 执行 `python cli.py check --manifest resources/manifest.yaml --webhook-config config/webhook.json`，并将 `--webhook-config` 指向包含目标渠道（如钉钉、飞书、Slack）的配置文件。项目也提供了 GitHub Actions 示例工作流（`.github/workflows/daily_check.yml`），可直接复用。

## 许可证

MIT

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
