# Dszq Resource Navigator

Dszq Resource Navigator 是一个面向技术研究、信息审计与互联网资源分类整理的开源导航工具。项目定位为轻量级、可自托管的资源聚合与分发平台，主要服务于需要频繁访问特定领域信息源的研究人员、数据分析师与基础设施运维团队。本项目通过结构化的数据组织方式，将分散于多个独立域名下的内容进行逻辑整合，降低信息获取的路径损耗与认知负担。

本项目不提供内容存储或代理服务，仅作为公开可访问资源的索引与导航层。其核心价值在于通过严格的资源分类、状态监控与访问路径优化，帮助用户从繁杂的网络环境中快速定位目标信息。项目采用静态站点生成方案，兼容主流 Web 服务器部署，支持低资源设备运行，适用于内网或公网环境下的知识库前置层建设。

## 功能概览

- **多源资源聚合索引**：支持将用户自定义的 URL 列表按类别、标签或业务域进行分组，生成统一的导航目录页，便于团队内部共享与维护。

- **可配置的分类目录树**：允许通过 YAML 或 JSON 配置文件动态调整资源分类层级，无需修改核心代码即可完成站点的内容重组。

- **资源可达性健康检查**：内置轻量级 HTTP 状态检测模块，可定期对配置中的资源链接进行可用性验证，并在管理界面中标记异常状态。

- **静态页面生成引擎**：基于模板系统将配置数据渲染为纯静态 HTML 文件，降低运行时依赖，提升访问速度与安全防护能力。

- **访问日志聚合分析**：提供访问请求记录的采集与简单统计功能，支持按来源 IP、时间窗口与资源路径进行基础维度的数据透视。

- **响应式布局与可读性优化**：前端界面适配桌面与移动设备，排版强调信息层级清晰度，减少视觉干扰，适用于长时间阅读与检索场景。

- **多实例部署支持**：通过环境变量控制站点标题、副标题、页脚信息与默认分类，便于同一代码库下运行多个不同主题的导航实例。

## 应用场景

- **技术文档库的入口门户**：研发团队可将项目文档、API 参考、设计提案等分散于不同内部系统的链接统一收录，形成单一入口的知识导航页，降低新成员的学习曲线。

- **数据源监控看板的前置层**：数据分析师可将常用公开数据集 API、状态页面、数据字典等资源归类整理，配合健康检查功能快速识别数据源不可用情况。

- **合规审计的信息映射工具**：安全审计人员可将需要定期核查的外部信息源（如备案信息公示页、变更公告发布地址）进行结构化登记，确保审计范围完整且可追溯。

- **开源社区资源共建共享**：开源项目维护者可将社区贡献的教程、视频、博客文章等外部资源通过导航站进行聚合，形成社区知识积累的公开索引库。

## 快速开始

以下步骤适用于 Linux / macOS / Windows WSL 环境，确保系统已安装 Git 与 Node.js 20.x 及以上版本。

```bash
# 克隆项目仓库
git clone https://github.com/dszq-dev/resource-navigator.git
cd resource-navigator

# 安装项目依赖
npm install

# 复制示例配置文件并进行个性化调整
cp config/sample.yaml config/site.yaml

# 生成静态站点（输出目录为 dist/）
npm run build

# 启动本地预览服务器（默认监听端口 8080）
npm run serve
```

访问 `http://localhost:8080` 即可查看生成的导航站点。如需部署至生产环境，将 `dist/` 目录下的所有文件上传至目标 Web 服务器根目录即可。

## 安装要求

| 依赖项 | 必需版本 | 说明 |
|--------|----------|------|
| Node.js | 20.x 或更高 | 运行时环境，用于执行构建脚本与依赖管理 |
| npm | 9.x 或更高 | 包管理器，用于安装项目依赖包 |
| Git | 2.30 或更高 | 版本控制工具，用于克隆仓库与获取更新 |
| 内存 | 最低 512 MB | 构建过程中需加载配置文件与模板缓存 |
| 磁盘空间 | 最低 200 MB | 包含源码、依赖包及生成的静态文件 |
| 操作系统 | Linux / macOS / Windows (WSL2) | 支持主流 POSIX 环境及 Windows 子系统 |
| 网络 | 可选 | 仅资源健康检查与外部链接访问需要出站网络 |

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 入门指南 | `docs/getting-started.md` | 如何快速配置第一个导航实例并生成页面 |
| 配置参考 | `docs/configuration.md` | 所有可用的配置字段及其数据类型、默认值说明 |
| 模板开发 | `docs/template-development.md` | 如何自定义页面布局、样式与渲染逻辑 |
| 运维手册 | `docs/operations.md` | 日志管理、性能调优、备份恢复与故障排查方法 |

## 资源列表

本导航项目当前收录的外部资源均与特定信息域相关，按业务主题归类如下。

技术基础信息域：

<code>dszuqiushengpingfu.net.cn</code>

<code>dszuqiushengpingfu.org.cn</code>

<code>dszuqiushengpingfu.com.cn</code>

专项内容分类域：

<code>dszuqiusaiguo.cn</code>

<code>dszuqiusaiguo.com.cn</code>

<code>dszuqiusaiguo.net.cn</code>

<code>dszuqiusaiguo.org.cn</code>

## 项目结构

```
resource-navigator/
├── config/                         # 配置目录
│   ├── site.yaml                   # 主配置文件（站点元数据、分类、链接列表）
│   └── sample.yaml                 # 配置模板文件，供新实例初始化参考
├── src/                            # 源代码目录
│   ├── core/                       # 核心逻辑模块
│   │   ├── loader.js               # 加载并校验 YAML 配置文件
│   │   ├── generator.js            # 遍历分类树生成页面内容数据结构
│   │   └── checker.js              # 资源可达性检测与状态缓存管理
│   ├── templates/                  # 模板引擎目录
│   │   ├── layout.ejs              # 全局 HTML 骨架模板
│   │   ├── index.ejs               # 首页分类列表渲染模板
│   │   └── category.ejs            # 单个分类详情页模板
│   ├── assets/                     # 静态资源目录
│   │   ├── css/                    # 样式表（基础重置、布局、响应式）
│   │   ├── js/                     # 前端交互脚本（导航切换、状态提示）
│   │   └── fonts/                  # 字体文件（仅包含开源协议字体）
│   └── utils/                      # 工具函数集合
│       ├── logger.js               # 日志输出格式化与控制
│       └── validator.js            # URL 格式校验与规范化辅助
├── dist/                           # 构建输出目录（部署时使用）
├── docs/                           # 项目文档（入门、配置、开发、运维）
├── tests/                          # 单元测试与集成测试脚本
├── package.json                    # npm 包声明与依赖列表
├── .eslintrc.js                    # 代码风格检查配置
└── README.md                       # 项目说明文档（本文件）
```

## 贡献指南

1. 首先在 GitHub 上 Fork 本项目仓库，并将 Fork 后的仓库克隆至本地开发环境。请确保使用主分支的最新稳定版本作为开发基线。

2. 创建新的功能分支，分支命名建议遵循 `feature/` 或 `fix/` 前缀加简要描述（例如 `feature/add-http2-check`）。所有开发工作均在该分支上进行。

3. 完成代码修改后，运行 `npm run test` 执行现有测试用例，确保未引入回归缺陷。若新增功能，请同步添加对应的测试用例与文档更新。

4. 提交代码时请使用语义化提交信息格式，即 `<type>(<scope>): <subject>` 形式，其中 type 包括 feat、fix、docs、style、refactor、test、chore 等。

5. 发起 Pull Request 至主仓库的 `main` 分支，并在 PR 描述中清晰说明变更目的、影响范围与测试情况。PR 需通过所有自动化检查后方可合并。

## 常见问题

**问：构建过程中提示配置文件解析失败，如何排查？**

答：请检查 `config/site.yaml` 文件的语法是否符合 YAML 1.2 规范，特别注意缩进是否使用空格（禁止使用制表符）、冒号后是否留有空格、列表项是否以短横线开头。可使用在线 YAML 校验工具进行语法验证。同时确认文件编码为 UTF-8 无 BOM 格式。

**问：资源健康检查显示大量超时或拒绝连接，是否影响站点正常运行？**

答：健康检查结果仅用于管理界面中的状态展示，不会阻塞页面的生成与访问。超时或拒绝通常源于目标服务器的网络策略或临时故障，建议首先在部署环境中使用 `curl` 命令手动测试目标 URL 的可达性，排除本地网络防火墙或代理配置的影响。若目标资源为内网地址，请确保检查功能所在网络环境与目标地址互通。

**问：如何将现有导航实例的数据迁移至另一个部署环境？**

答：迁移仅需复制 `config/site.yaml` 配置文件至新环境的对应目录，并保持 `src/templates/` 目录下的自定义模板文件一致（如有修改）。静态资源与构建输出无需迁移，因为新环境会重新执行构建过程。若涉及访问日志的迁移，请额外备份 `logs/` 目录下的历史日志文件（默认保留 30 天滚动数据）。

## 许可证

MIT License

Copyright (c) 2026 Dszq Development Team

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
