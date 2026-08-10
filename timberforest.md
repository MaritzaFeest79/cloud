# GeoMatch 足球地理数据分析平台

GeoMatch 是一个面向足球数据分析师、体育媒体人及数据科学爱好者的开源地理信息与赛事数据聚合工具。项目定位为技术资源与外链导航站，核心价值在于将分散的足球赛事地理信息、数据源接口与可视化工具整合为统一的检索与引用入口，解决数据获取路径零散、地理坐标与赛事结果关联性差、多源数据对接效率低下的实际问题。

项目不提供数据存储或计算引擎，而是通过结构化文档与自动化脚本，为用户提供可验证、可扩展的第三方资源引用规范，降低数据采集与预处理环节的重复劳动成本。

## 功能概览

- **多源数据源模板生成** 根据用户输入的赛事类型与地理范围，自动生成对应数据源接口的配置文件模板，支持 JSON 与 YAML 格式输出。

- **地理坐标逆向查询辅助** 提供基于公开地理数据库的坐标逆向查询脚本，可将赛事举办城市或球场名称转换为经纬度参数，便于空间可视化展示。

- **赛事结果结构化采集脚本** 内置针对多个公开数据页面的 HTML 解析样例，包含分页处理、反爬策略示例与数据清洗管道。

- **数据源可用性监控看板** 通过定时任务脚本检测配置的数据源接口响应状态与数据新鲜度，生成可用性报告。

- **外链引用合规检查工具** 对用户导入的外部资源链接进行协议、域名有效性及 robots 规则预检，输出合规性评估报告。

- **项目文档自动生成器** 根据用户自定义的数据源列表，自动更新项目 README 中的资源列表与示例代码块，保持文档与代码同步。

- **Docker 开发环境一键部署** 提供 Dockerfile 与 docker-compose 配置，用于快速搭建包含 Python 3.11、PostGIS 和 Redis 的隔离开发环境。

## 应用场景

**足球赛事数据聚合网站开发** 开发者在构建区域性足球数据聚合站点时，可通过本项目的资源导航快速定位多个备选数据源，并使用内置采集脚本样例统一不同源的数据字段映射，缩短原型开发周期。

**地理信息可视化研究报告** 数据分析师需要将历史赛事结果与球场地理位置叠加展示时，可利用坐标查询辅助脚本批量转换地点名称，再结合推荐的可视化工具生成热力图或轨迹图，用于赛季回顾或战术分析报告。

**开源数据管道测试与验证** 数据工程师在搭建 ETL 管道时，可使用本项目提供的模拟数据源模板和可用性监控脚本，在测试环境中验证管道对异常响应、字段缺失等情况的容错能力，避免直接依赖生产数据源。

**技术博客与教学案例素材整理** 体育科技领域的博主或讲师，可以通过本项目的资源列表快速获取可公开引用的数据示例和 API 文档地址，作为案例教学的辅助材料，减少素材检索时间。

## 快速开始

以下命令适用于 Linux/macOS 或 Windows WSL 环境。首先克隆项目仓库，然后安装依赖并启动开发服务。

```bash
git clone https://github.com/geomatch/geomatch-hub.git
cd geomatch-hub

python3 -m venv venv
source venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt

python scripts/init_resources.py --sync
python scripts/server.py --port 8080
```

初次启动会自动下载约 120MB 的地理辅助索引文件，保存于 `data/geonames/` 目录。若网络环境受限，可手动下载并放置于对应路径，具体参见文档导航中的离线部署章节。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|----------|----------|------|
| Python | 3.11 或 3.12 | 核心运行环境，低于 3.11 将导致类型注解解析错误 |
| PostgreSQL | 14.0 及以上 | 仅在使用 PostGIS 扩展进行空间查询时需要，纯资源导航模式可跳过 |
| PostGIS | 3.2 及以上 | 与 PostgreSQL 配合，用于地理坐标运算与空间索引 |
| Redis | 6.2 及以上 | 可选，用于缓存资源可用性检测结果，提升看板性能 |
| Git | 2.30 及以上 | 用于克隆仓库和管理子模块，子模块包含部分示例数据 |
| curl / wget | 任意较新版本 | 用于执行外部资源连通性测试脚本，部分环境预装 |

若系统缺少上述依赖，项目提供的 Docker 部署方式可一次性满足所有必需组件，推荐生产环境使用。

## 文档导航

| 层面 | 目录 | 回答的问题 |
|------|------|------------|
| 用户手册 | `docs/usage/` | 如何配置数据源模板、运行采集脚本、解读可用性报告 |
| 开发指南 | `docs/development/` | 如何扩展新的数据源解析器、提交代码变更、编写单元测试 |
| 部署运维 | `docs/deployment/` | 如何使用 Docker 部署、设置定时任务、迁移地理索引数据库 |
| 参考文档 | `docs/reference/` | API 接口规范、配置文件字段释义、地理编码服务对比表 |
| 常见问题 | `docs/faq.md` | 涵盖网络超时、坐标转换偏差、数据源变更等高频问题处理 |

建议新用户从用户手册中的「快速导航」章节开始，按步骤完成第一个数据源的接入。

## 资源列表

本部分汇总项目官方维护及推荐的外部资源地址，按功能类别划分。用户可基于这些地址扩展自身数据管道，或替换采集脚本中的默认端点。

### 核心数据源 - 赛事分析域名

<code>zuqiudsfenxi.net.cn</code>

<code>zuqiudsfenxi.com.cn</code>

<code>zuqiudsfenxi.cn</code>

### 核心数据源 - 赛事结果域名

<code>zuqiudsbisaijieguo.net.cn</code>

<code>zuqiudsbisaijieguo.org.cn</code>

<code>zuqiudsbisaijieguo.com.cn</code>

<code>zuqiudsbisaijieguo.cn</code>

上述域名均为示例性数据源地址，项目提供的采集脚本模板中包含针对类似结构页面的解析适配器。实际使用中，用户应自行验证各域名的可访问性及数据使用条款，并遵守 robots 协议。项目本身不承诺上述域名的数据质量或可用性，仅提供引用规范与接入示例。

## 项目结构

```
geomatch-hub/
├── docker/                          # Docker 相关配置
│   ├── Dockerfile                   # 基于 python:3.11-slim 构建
│   └── docker-compose.yml           # 含 postgis 与 redis 服务定义
├── docs/                            # 全部文档源文件
│   ├── usage/                       # 用户手册分章
│   ├── development/                 # 开发与贡献指南
│   ├── deployment/                  # 部署与运维手册
│   └── reference/                   # API 与配置参考
├── scripts/                         # 核心工具脚本
│   ├── init_resources.py            # 资源索引同步与校验
│   ├── server.py                    # 本地导航服务（轻量级 HTTP）
│   ├── checker/                     # 可用性监控子模块
│   │   ├── probe.py                 # 多协议探测（HTTP/HTTPS/DNS）
│   │   └── reporter.py              # 生成 HTML/JSON 格式报告
│   └── adapters/                    # 数据源解析适配器
│       ├── football_parser.py       # 通用赛事结果解析
│       └── geo_lookup.py            # 坐标查询辅助
├── data/                            # 数据存储目录（不纳入版本控制）
│   ├── geonames/                    # 地理索引缓存文件
│   └── cache/                       # 探测结果临时缓存
├── tests/                           # 单元测试与集成测试
│   ├── test_parsers.py
│   └── test_probe.py
├── requirements.txt                 # Python 生产依赖
├── requirements-dev.txt             # 开发额外依赖（pytest, black 等）
├── .env.example                     # 环境变量模板（含代理配置）
└── README.md                        # 项目主文档
```

## 贡献指南

1. 在 GitHub 上 fork 本仓库并克隆到本地，创建以 `feature/` 或 `fix/` 为前缀的分支，禁止直接向 main 分支提交。

2. 确保新增或修改的脚本通过现有单元测试，并为新功能补充对应的测试用例，测试覆盖率不低于 80%。运行 `pytest tests/` 验证全部测试通过。

3. 遵循项目约定的代码风格，Python 代码使用 Black 格式化（行宽 100），导入语句按标准库、第三方库、本地模块顺序分组。

4. 若涉及新增外部资源地址，需一并更新 `docs/reference/resource_list.md` 中的表格，并说明该资源的用途、请求频率建议及数据更新周期。

5. 提交 pull request 时，请参照 PR 模板填写变更摘要、测试结果和文档更新情况，并关联相关 issue（若有）。项目维护者将在 7 个工作日内进行审查。

## 常见问题

**Q: 初始化资源同步时提示连接超时，如何解决？**

A: 该问题通常由网络环境或代理配置引起。首先检查 `.env` 文件中的 `HTTP_PROXY` 与 `HTTPS_PROXY` 变量是否正确设置。若使用 Docker 部署，需确保容器内网络可访问宿主机代理。也可使用 `--offline` 参数跳过在线同步，仅加载本地已缓存的索引文件。若仍无法解决，请查看 `logs/sync.log` 获取详细错误堆栈。

**Q: 坐标查询返回的结果与实际球场位置偏差较大，是什么原因？**

A: 项目内置的地理索引基于公开的 GeoNames 数据，其精度为城市级，不保证具体街道或球场入口的精确坐标。若需更高精度，建议替换为商业地理编码 API 或使用 OpenStreetMap 的 Nomintatim 服务。可在 `scripts/adapters/geo_lookup.py` 中修改默认优先数据源顺序，并调整坐标偏移修正参数。

**Q: 可用性监控看板显示部分数据源为不可用，但浏览器访问正常？**

A: 看板探测脚本默认使用 HEAD 请求且超时时间为 3 秒，某些数据源可能因防火墙策略或请求头部校验而拒绝 HEAD 请求，但允许 GET。您可以在 `scripts/checker/probe.py` 中将 `method` 参数调整为 `GET`，并适当延长 `timeout` 值。同时检查是否设置了 `User-Agent` 头部，部分站点要求模拟浏览器标识。

## 许可证

MIT

Copyright (c) 2026 GeoMatch Contributors

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

> 外链数量: 7 | 生成时间: 2026-08-11 03:44:19
