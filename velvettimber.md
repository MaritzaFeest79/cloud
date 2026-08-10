# OSS-Directory

Open Source Software Directory 是一个专注于收录、索引和归档高质量互联网资源与开源项目外链的元目录系统。本项目面向开发者、技术研究人员以及开源爱好者，旨在通过人工筛选与社区贡献相结合的方式，构建一个结构化、可维护且长期稳定的技术资源导航体系。

本项目不直接托管任何第三方代码或文件，而是作为资源定位与分类索引层存在。通过严格的资源审核流程、定期的可用性检查以及版本化的目录更新记录，OSS-Directory 致力于解决技术资源散落、链接失效、分类混乱等长期困扰开发者的信息管理问题。

## 功能概览

- 多维度资源分类索引：按技术栈、应用场景、许可证类型、语言区域等维度对资源进行精细化标签管理。

- 外链存活与状态监控：系统定期对收录的 URL 进行 HTTP 状态检查，自动标记异常链接，并在目录更新日志中记录变动。

- 结构化元数据导出：支持将目录内容导出为 JSON、YAML 或 CSV 格式，便于其他工具或脚本进行二次处理。

- 社区贡献与审核流程：注册用户可提交新资源或更新现有条目，所有变更需经过维护者审核后合并。

- 全文检索与快速过滤：基于标题、描述、标签及域名进行实时搜索，支持正则表达式与布尔逻辑查询。

- 版本化目录历史：每次目录更新均生成快照，支持回溯任意历史版本，便于对比资源变动。

- 轻量级静态生成：项目构建输出为纯静态 HTML 及 Markdown 文件，可部署在任何 Web 服务器或对象存储上。

## 应用场景

- 技术团队内部知识库建设：团队可将 OSS-Directory 作为基础框架，定制化收录内部使用的工具、文档和依赖镜像源，形成统一的入口。

- 开源项目依赖与生态索引：开源维护者可以使用本目录记录项目所依赖的第三方库、服务地址以及相关社区论坛，方便新贡献者快速了解外围生态。

- 离线文档与资源镜像规划：系统管理员可基于导出的元数据文件，规划需要镜像或缓存的外部资源清单，用于内网环境或受限网络条件下的开发工作。

- 技术博客与教程的外部链接管理：技术写作者可以利用本目录的结构化管理方式，集中维护文章中的引用链接，避免因链接失效导致内容质量下降。

- 学术研究中的软件工具有效性验证：研究人员可通过版本化目录功能，准确引用特定时间点的资源集合，增强实验可复现性。

## 快速开始

以下命令演示了如何从代码仓库克隆项目、安装依赖并启动本地开发服务。请确保您的系统已安装 Git、Node.js 及 npm。

```bash
git clone https://github.com/oss-directory/oss-directory.git
cd oss-directory
npm install
npm run build
npm start
```

执行上述命令后，本地服务将默认监听 8080 端口。您可以通过浏览器访问 `http://127.0.0.1:8080` 查看生成的目录首页。若需自定义端口或构建输出路径，请参考项目根目录下的 `config.yaml` 配置文件。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Node.js | 18.0.0 或更高 | 项目运行时环境，用于执行构建脚本与本地服务 |
| npm | 9.0.0 或更高 | Node.js 包管理器，用于安装项目依赖 |
| Git | 2.30.0 或更高 | 版本控制工具，用于克隆仓库及管理提交历史 |
| Python | 3.9 或更高（可选） | 仅在运行额外的链接检测脚本时需要，主流程不依赖 |
| curl | 7.68.0 或更高（可选） | 用于外部资源状态检查的备用工具，非强制 |
| 磁盘空间 | 至少 200 MB | 用于存放源代码、构建输出及缓存数据 |
| 内存 | 建议 1 GB 以上 | 构建大型目录时，内存影响索引排序及搜索性能 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 入门指南 | `docs/getting-started/` | 如何快速部署、配置第一个资源条目以及理解目录结构 |
| 维护手册 | `docs/maintenance/` | 如何执行链接检查、更新索引、处理失效资源以及版本发布流程 |
| API 参考 | `docs/api/` | 如何通过 HTTP 接口查询目录数据、获取元数据导出文件以及状态码含义 |
| 贡献规范 | `docs/contributing/` | 提交新资源的标准、审核周期、标签使用规则以及提交信息格式 |

## 资源列表

以下为本目录当前收录的外部资源链接。所有 URL 均按原始形式原样列出，未做任何协议补全或域名规范化处理。

技术资源与社区

<code>oumeidiyishipin.org.cn</code>

<code>zhongwenzimuzipai.org.cn</code>

<code>ribendaxiangjiao.org.cn</code>

媒体与工具索引

<code>liumangruanjianxiazai.org.cn</code>

<code>shoujizaixianguankannidongde.org.cn</code>

<code>jiqingshipinwangzhi.org.cn</code>

<code>oumeijingpinzipai.org.cn</code>

## 项目结构

```
oss-directory/
├── config/                         # 全局配置文件目录
│   ├── categories.yaml             # 资源分类标签与层级定义
│   ├── mirrors.yaml                # 外部镜像源与代理列表
│   └── validator-rules.json        # 资源字段校验规则（正则、长度、枚举）
│
├── src/                            # 核心源码目录
│   ├── core/                       # 索引引擎、缓存管理与事件调度
│   │   ├── indexer.js              # 资源索引构建主逻辑
│   │   ├── cache.js                # LRU 缓存实现，用于加速检索
│   │   └── scheduler.js            # 定时任务调度器（链接检查、快照生成）
│   ├── parsers/                    # 多种格式解析器
│   │   ├── yaml-loader.js          # YAML 资源条目加载器
│   │   ├── csv-importer.js         # 批量导入 CSV 格式资源列表
│   │   └── json-exporter.js        # 目录数据导出为 JSON 流
│   └── web/                        # 静态站点生成与路由
│       ├── generator.js            # Markdown / HTML 页面生成器
│       ├── router.js               # 简易路由映射，用于开发预览
│       └── themes/                 # 默认主题样式与模板引擎
│
├── data/                           # 资源数据存储（非 git 追踪，由构建生成）
│   ├── raw/                        # 社区提交的原始资源文件（待审核）
│   ├── curated/                    # 已审核通过的目录条目（主数据源）
│   └── snapshots/                  # 版本化快照，按日期归档
│
├── scripts/                        # 辅助运维脚本
│   ├── check-links.sh              # 基于 curl 的外部链接批量检测脚本
│   ├── generate-sitemap.py         # 生成站点地图的 Python 脚本
│   └── migrate-v1-to-v2.js         # 数据版本迁移工具
│
├── tests/                          # 单元测试与集成测试
│   ├── unit/                       # 针对核心模块的单元测试（Jest）
│   └── integration/                # 端到端构建与导出测试
│
├── docs/                           # 项目文档源码（英文与中文）
├── .github/                        # GitHub 工作流定义
│   └── workflows/                  # CI 流水线配置（构建、测试、部署）
│
├── package.json                    # npm 项目描述文件
├── README.md                       # 项目入口文档（即本文件）
└── LICENSE                         # MIT 许可证文本
```

## 贡献指南

1. 复刻项目仓库，并在本地创建新的功能分支。分支命名建议采用 `feature/资源类别-简述` 或 `fix/问题编号` 格式，便于维护者识别。

2. 在 `data/raw/` 目录下按照模板格式创建新的资源条目文件。模板文件位于 `config/template.yaml`，请完整填写标题、描述、URL、标签及提交人信息。所有必填字段缺失将导致审核不通过。

3. 提交代码前，请运行 `npm run lint` 和 `npm test` 确保代码风格一致且所有单元测试通过。若新增功能或修改核心逻辑，需同步编写或更新对应的测试用例。

4. 发起 Pull Request 至主仓库的 `main` 分支。PR 描述中需清晰说明新增资源的来源、用途以及选择收录的理由。维护者会在 3 个工作日内进行审核。

5. 审核通过后，维护者将资源从 `raw` 移动至 `curated` 目录，并更新索引快照。贡献者将获得项目贡献者列表中的署名，并可参与后续版本规划讨论。

## 常见问题

Q: 本目录与普通书签管理器或导航网站有何区别？

A: OSS-Directory 并非面向最终用户的浏览导航，而是面向开发者和维护者的结构化数据管理工具。其核心产出为机器可读的元数据文件，而非仅提供点击跳转。版本化、状态监控和标准化导出是本项目区别于常规书签工具的三个主要特性。

Q: 如果收录的资源链接失效，项目会如何处理？

A: 项目内置的调度器会每天执行一次全局链接状态检查。对于连续 7 天返回 4xx 或 5xx 状态码的资源，系统将自动在目录中标记为「待确认」，并发送通知给最近更新该条目的维护者或提交者。若超过 30 天仍未恢复，该条目将被移至 `data/archived/` 并排除在主索引之外，但历史快照中仍保留其记录。

Q: 我能否在不运行本地服务的情况下直接使用导出的数据？

A: 可以。每次构建完成后，`dist/` 目录下会生成 `index.json`、`index.yaml` 以及按分类拆分的多个 CSV 文件。您可以直接将这些文件用于静态站点生成、文档工具链集成或导入至数据库。无需启动任何 Web 服务即可消费这些数据。

## 许可证

MIT License

Copyright (c) 2026 OSS-Directory Contributors

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
