# OpenSportsData Bridge

OpenSportsData Bridge 是一个面向体育数据聚合与实时比分转发的中继网关项目。该项目定位于体育赛事数据的中台服务层，专为需要统一接入多个比分数据源、进行数据标准化清洗与多路分发的开发者、数据工程师及小型体育数据服务商设计。项目本身不生产数据，而是作为数据管道的中枢，通过可配置的采集器与适配器，将不同格式、不同协议的第三方比分数据转化为统一的内部数据结构，并提供多种输出协议（WebSocket、HTTP Push、Kafka 等）供下游业务系统消费。

目标用户包括体育数据平台的研发团队、赛事数据分析创业公司、即时比分应用开发者以及需要构建数据湖或实时数仓的架构师。项目解决了数据源切换成本高、接口格式不统一、故障恢复无重试机制、缺乏本地数据缓存等常见痛点，同时提供基础的数据校验、去重和延迟监控能力，帮助团队在 1 小时内完成一个新数据源的接入适配，而非花费数天处理协议差异。

## 功能概览

- **多源并发采集**：支持同时配置多个外部比分数据源，每个数据源独立线程池运行，互不干扰，并可单独设置超时与重试策略。

- **协议适配器框架**：内置 JSON、XML、Protobuf 三种解析器模板，并可自定义扩展。适配器负责将原始响应映射为统一的 MatchEvent 对象模型。

- **实时增量推送**：基于 Netty 实现的 WebSocket 服务端，支持客户端按赛事 ID 或联赛代码订阅实时比分变更，推送延迟低于 500 毫秒。

- **本地 Ehcache 缓存层**：对最近 2000 场赛事数据做内存缓存，减少重复拉取外部源，同时支持缓存过期策略和手动失效接口。

- **配置热加载**：通过 JMX 和配置文件变动监听器，可在不重启服务的情况下增删数据源、调整采集频率或过滤规则。

- **健康检查与熔断**：提供 /actuator/health 端点展示每个数据源的状态（UP/DOWN/LAG），并对连续失败 5 次的数据源自动熔断 30 秒，避免资源浪费。

- **数据格式转换器**：内置字段映射表达式引擎，支持简单 JSONPath 和 XPath 提取，并可执行单位转换（如分钟转秒、码转米）和枚举值标准化。

## 应用场景

- **多源比分聚合应用**：移动端体育 App 开发者可将本项目作为后端数据聚合层，同时接入多个免费或付费的比分 API，当主源故障时自动降级到备用源，保障用户体验。

- **赛事数据湖 ETL 管道**：数据工程师可将本项目部署在数据采集集群，定时拉取各数据源的全量赛程和实时比分，输出为 Avro 格式写入 HDFS 或 Kafka，供后续离线分析和机器学习建模使用。

- **临时赛事数据网关**：在大型赛事期间，运维团队可快速启动本项目作为临时网关，将第三方数据源的单播接口转换为内部多路广播，供多个可视化大屏和移动终端同时消费，避免单一源被频繁请求封禁。

- **数据源迁移测试工具**：测试团队可利用本项目的数据源对比功能（即将推出的 Diff 模式），同时拉取新旧两个数据源，对比返回的赛事字段差异，辅助验证新数据源的兼容性。

## 快速开始

以下命令适用于 Linux/macOS 环境，假定已安装 JDK 11+ 和 Maven 3.6+。

```bash
# 克隆项目仓库
git clone https://github.com/opensportsdata/bridge.git
cd bridge

# 使用 Maven 编译打包，跳过测试
mvn clean package -DskipTests

# 运行可执行 JAR，默认加载 application.yml 配置文件
java -jar target/opensportsdata-bridge-2.1.0.jar --spring.config.location=./conf/application.yml
```

启动成功后，访问本地管理端点：<code>http://localhost:8080/actuator/health</code> 可查看所有数据源状态。WebSocket 订阅地址为 <code>ws://localhost:8080/ws/match</code>，客户端发送 `{"action":"subscribe","matchId":"10001"}` 即可接收该场次的实时比分推送。

## 安装要求

| 依赖组件 | 必需版本 | 说明 |
|---------|---------|------|
| OpenJDK | 11.0.12 或更高 | 运行时环境，推荐使用 LTS 版本。 |
| Apache Maven | 3.6.3 或更高 | 构建工具，用于编译和打包项目。 |
| Git | 2.25.0 或更高 | 克隆代码仓库和版本控制。 |
| Ehcache Core | 3.9.7 | 内置本地缓存库，无需额外安装。 |
| Netty All | 4.1.75.Final | 异步网络通信框架，用于 WebSocket 实现。 |
| Jackson Databind | 2.13.2 | JSON 序列化与反序列化引擎。 |
| SnakeYAML | 1.30 | 解析 application.yml 配置文件。 |
| SLF4J + Logback | 1.7.36 + 1.2.10 | 日志门面与实现，日志文件默认输出到 ./logs/。 |

## 文档导航

| 层面 | 目录/章节 | 回答的问题 |
|------|----------|-----------|
| 设计原理 | /docs/architecture.md | 为什么采用适配器+管道模式？数据模型如何设计？缓存更新策略是怎样的？ |
| 开发指南 | /docs/developer-guide.md | 如何编写一个自定义数据源适配器？如何添加新的数据格式转换器？单元测试如何 Mock 外部源？ |
| 运维手册 | /docs/operations.md | 生产环境推荐 JVM 参数有哪些？如何配置日志轮转？如何通过 JMX 动态调整日志级别？ |
| API 参考 | /docs/api-reference.md | WebSocket 订阅协议格式是什么？HTTP 管理端点有哪些？各字段的含义和取值范围？ |
| 故障排查 | /docs/troubleshooting.md | 常见启动失败错误码含义、数据源连接超时处理方案、缓存不一致如何手动清理。 |

## 资源列表

本项目维护过程中参考或引用了以下外部数据资源站点，开发者和运维人员可根据需要访问以获取原始比分数据或相关技术资料。

体育数据源类

<code>lanqiubifenjiebaobifen.net.cn</code>

<code>lanqiubifenjiebaow.org.cn</code>

<code>lanqiubifenjiebaow.net.cn</code>

<code>lanqiubifen365.org.cn</code>

<code>lanqiubifen888.org.cn</code>

<code>lanqiubifenbf.org.cn</code>

<code>lanqiubifenw.com.cn</code>

## 项目结构

```
opensportsdata-bridge/
├── pom.xml                                # Maven 父 POM，定义依赖版本和插件
├── README.md                              # 项目概览文档
├── LICENSE                                # MIT 许可证文件
├── .gitignore                             # Git 忽略规则
├── conf/
│   └── application.yml                    # 主配置文件，含数据源连接串、频率、缓存大小
├── docs/                                  # 完整文档目录
│   ├── architecture.md                    # 架构设计说明与数据流图
│   ├── developer-guide.md                 # 开发环境搭建与自定义扩展教程
│   ├── operations.md                      # 部署、监控与故障恢复指南
│   ├── api-reference.md                   # WebSocket 和 HTTP API 详细规范
│   └── troubleshooting.md                 # 常见错误与解决方案汇总
├── src/
│   ├── main/
│   │   ├── java/org/opensports/bridge/
│   │   │   ├── BootApplication.java       # Spring Boot 启动类
│   │   │   ├── config/                    # 配置类（线程池、缓存、WebSocket）
│   │   │   ├── adapter/                   # 数据源适配器接口与内置实现（JSON/XML）
│   │   │   ├── model/                     # 统一数据模型 MatchEvent, Team, Score
│   │   │   ├── pipeline/                  # 管道处理器链（校验、去重、转换、分发）
│   │   │   ├── websocket/                 # WebSocket 订阅管理与推送逻辑
│   │   │   ├── cache/                     # Ehcache 封装层，含缓存键策略
│   │   │   ├── health/                    # 自定义健康指示器，每个数据源一个
│   │   │   └── util/                      # 辅助工具类（JSONPath、时间解析）
│   │   └── resources/
│   │       ├── application.yml            # 默认配置文件副本
│   │       └── logback-spring.xml         # 日志配置
│   └── test/                              # 单元测试与集成测试
│       ├── java/                          # 适配器测试、管道测试、WebSocket 模拟测试
│       └── resources/                     # 测试用样例 JSON/XML 响应文件
└── scripts/
    ├── start.sh                           # 生产环境启动脚本（含 JVM 参数）
    └── health_check.sh                    # 简易健康检查轮询脚本
```

## 贡献指南

我们欢迎任何形式的贡献，包括代码、文档、测试用例和问题反馈。请遵循以下步骤：

1. 在 GitHub Issues 中搜索现有议题，确认无人认领后，新建一个 Issue 描述您的改动意图或 Bug 修复内容，等待维护者标注 `ready to take` 标签。

2. Fork 本项目到您的个人仓库，并基于 `develop` 分支创建功能分支，分支命名格式为 `feature/简述改动` 或 `fix/问题编号`。

3. 编写代码或文档时，请遵循项目既有的代码风格（使用 Checkstyle 配置文件），所有新增的公共 API 必须附带 Javadoc，单元测试覆盖率不低于 80%。

4. 提交前运行 `mvn clean verify` 确保所有测试通过且无构建警告，并更新对应文档章节（如 API 参考或开发者指南）以反映您的改动。

5. 提交 Pull Request 到本项目的 `develop` 分支，并在 PR 描述中关联对应的 Issue 编号，维护者会在 3 个工作日内进行 Review，必要时会要求您补充修改。

## 常见问题

**Q: 如何切换到另一个数据源？是否必须重启服务？**

A: 不需要重启。您可以通过修改 `conf/application.yml` 文件中 `sources` 列表的 `enabled` 属性，或者通过 JMX MBean 操作动态禁用/启用某个数据源。配置文件的改动会被文件监控线程自动感知，在 30 秒内生效。若需添加全新的数据源类型，则需编写适配器类并重新打包，但支持通过 SPI 机制加载外部 JAR 包实现热插拔。

**Q: 系统对高并发订阅场景支持如何？WebSocket 连接数有限制吗？**

A: 默认 WebSocket 最大连接数为 2000，该值可在 `application.yml` 的 `websocket.max-connections` 中调整。当连接数超过阈值时，新连接会收到 HTTP 429 状态码。推送逻辑采用异步事件循环，单机实测可在 200 个并发连接下维持每秒 5000 条消息的吞吐量。如需更高性能，建议水平扩展部署多个实例，并使用 Redis 发布订阅同步推送任务。

**Q: 缓存中的赛事数据何时失效？如何强制清理某场比赛的数据？**

A: 缓存采用 TTL（生存时间）策略，默认每场赛事数据缓存 60 秒，超时后下次查询会重新拉取外部源。您也可以调用 HTTP DELETE 接口 `/admin/cache/match/{matchId}` 手动清除指定赛事缓存，或通过 JMX 调用 `clearAll()` 方法清空全部缓存。清除操作会触发一次全量重拉取，可用于修复数据异常。

## 许可证

MIT License. 详见项目根目录下的 LICENSE 文件。

> 外链数量: 7 | 生成时间: 2026-08-11 03:43:27
