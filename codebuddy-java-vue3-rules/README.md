# CodeBuddy Java + Vue 3 项目规则包

一套面向 **Java 17 + Spring Boot + MyBatis + PostgreSQL 微服务 + Vue 3** 技术栈的 CodeBuddy AI 开发规则包。通过「常驻规则 + 路径条件触发规则」的双层加载策略，在约束 AI 开发行为的同时控制 Token 开销，让 AI 生成的代码符合项目的架构、编码、安全与交付标准。

> 本包为规则与文档模板，不包含业务代码。技术版本（Spring Boot、MyBatis 等）以 `docs/TECH-DECISIONS.md` 与项目实际构建文件为准，未决项为 `TBD`，不表示允许 AI 自行选型。

## 目录

- [特性](#特性)
- [技术栈基线](#技术栈基线)
- [前提条件](#前提条件)
- [目录结构](#目录结构)
- [规则总览](#规则总览)
- [安装（快速开始）](#安装快速开始)
- [详细使用步骤](#详细使用步骤)
- [Token 优化原则](#token-优化原则)
- [常见问题（FAQ）](#常见问题faq)
- [注意事项](#注意事项)
- [许可证](#许可证)

## 特性

- **14 条中文规则**：3 条常驻 + 11 条按文件路径条件触发，覆盖架构、编码、API、数据访问、安全、前端工程、视觉、测试与交付全链路。
- **双层加载策略**：常驻规则只保留核心约束与强制工作流，专题规则仅在匹配文件时加载，避免无关规则常驻消耗 Token。
- **单一权威来源**：重复约束只在一处完整定义，其余文件用指针引用，消除冗余、便于维护。
- **技术决策框架**：提供 ADR 模板与 `TECH-DECISIONS.md` 待确认清单，把版本、组件、平台选型纳入受控审批。
- **自动化门禁建议**：`automation/` 给出 Maven Enforcer、ArchUnit、前端 lint/type-check 等 CI 落地建议。

## 技术栈基线

| 项目 | 状态 |
|---|---|
| Java | 17（已确认，不得使用更高版本语言特性） |
| Spring Boot | 框架已确认，精确版本 TBD |
| MyBatis | 精确版本 TBD |
| PostgreSQL | 类型已确认，精确版本 TBD |
| 架构 | 微服务 |
| Vue | 3（已确认） |
| 构建 | Maven（建议默认，Wrapper 与插件版本 TBD） |

其余（TypeScript、Node、包管理器、UI 组件库、CSS 方案、路由/状态管理、服务通信、认证授权、数据库迁移、可观测性等）见 `docs/TECH-DECISIONS.md`。

## 前提条件

- 已安装 **CodeBuddy IDE**。
- 规则仅在**会话开始时**加载：创建或修改规则后，需要新建对话会话才能生效。
- 建议 Git 管理：`.codebuddy/`、`CODEBUDDY.md`、`docs/` 应提交仓库，供团队共享。

## 目录结构

```
.
├── .codebuddy/
│   └── rules/                          # 14 条规则（每目录一个 RULE.mdc）
│       ├── 00-project-core/            # 常驻：项目核心约束
│       ├── 10-tech-stack/              # 常驻：技术栈与依赖基线
│       ├── 20-microservices-architecture/
│       ├── 30-ai-development-workflow/ # 常驻：AI 开发流程
│       ├── 40-java-coding/
│       ├── 41-api-design/
│       ├── 42-mybatis-postgresql/
│       ├── 43-security/
│       ├── 50-vue3-engineering/
│       ├── 51-frontend-visual-style/
│       ├── 52-frontend-components-and-data-ui/
│       ├── 53-frontend-api-state/
│       ├── 60-testing-quality-gates/
│       └── 70-engineering-and-delivery/
├── docs/
│   ├── TECH-DECISIONS.md               # 技术决策待确认清单
│   ├── FRONTEND-STYLE-GUIDE.md         # 前端视觉与交互设计指南
│   └── ADR-TEMPLATE.md                 # 架构决策记录（ADR）模板
├── automation/
│   └── README.md                       # 自动化强制检查建议
├── CODEBUDDY.md                        # 项目 AI 入口（精简）
├── TOKEN-PROFILE.md                    # Token 配置概况与字符数统计
└── README.md
```

## 规则总览

| 编号 | 规则 | 加载方式 | 触发范围（paths） |
|---|---|---|---|
| 00 | 项目核心约束 | 常驻 | — |
| 10 | 技术栈与依赖基线 | 常驻 | — |
| 20 | 微服务架构 | 路径触发 | `**/*Controller.java`、`**/*Client.java`、`**/*Adapter.java`、`**/*Application.java`、`**/pom.xml`、`**/application*.yml`、`**/bootstrap*.yml` |
| 30 | AI 开发流程 | 常驻 | — |
| 40 | Java 编码规范 | 路径触发 | `**/*.java` |
| 41 | API 设计规范 | 路径触发 | `**/*Controller.java`、`**/*Request.java`、`**/*Response.java`、`**/*DTO.java`、`**/*Dto.java`、`**/api/**`、`**/*.vue`、`**/*.ts` |
| 42 | MyBatis/PostgreSQL | 路径触发 | `**/*Mapper.java`、`**/*.xml`、`**/*.sql`、`**/db/**`、`**/migration*/**` |
| 43 | 安全规则 | 路径触发 | `**/*Security*.java`、`**/*Config.java`、`**/*Controller.java`、`**/application*.yml`、`**/application*.yaml`、`**/*.properties` |
| 50 | Vue 3 前端工程 | 路径触发 | `**/*.vue`、`**/*.ts`、`**/*.js` |
| 51 | 前端视觉风格 | 路径触发 | `**/*.vue`、`**/*.css`、`**/*.scss`、`**/*.less` |
| 52 | 业务 UI 组件规范 | 路径触发 | `**/*.vue`、`**/*.css`、`**/*.scss` |
| 53 | 前后端联调与状态 | 路径触发 | `**/*.vue`、`**/*.ts`、`**/*.js`、`**/api/**` |
| 60 | 测试与质量门禁 | 路径触发 | `**/*.java`、`**/*.vue`、`**/*.ts`、`**/*Test.java`、`**/*.spec.ts`、`**/*.test.ts`、`**/pom.xml`、`**/package.json` |
| 70 | 工程与交付 | 路径触发 | `**/pom.xml`、`**/package.json`、`**/*lock*`、`Dockerfile`、`**/*.yml`、`**/*.yaml`、`**/*.properties`、`.gitignore` |

## 安装（快速开始）

1. 将本仓库内容合并到目标项目根目录，并提交 `.codebuddy/`、`CODEBUDDY.md`、`docs/` 与 `automation/`。
2. 在 CodeBuddy IDE 中确认 3 条核心规则为「总是应用」：`00-project-core`、`10-tech-stack`、`30-ai-development-workflow`。
3. 其余专题规则通过各自 `paths` 在处理匹配文件时自动触发，无需额外配置。
4. 新建对话验证规则加载结果。

## 详细使用步骤

### 第 1 步：安装到项目

把本包内容复制到项目根目录。不要只复制 `.codebuddy/`——`CODEBUDDY.md`、`docs/`、`automation/` 也要一并提交，它们共同构成完整约束。

### 第 2 步：确认规则加载方式

- 常驻规则（`00/10/30`）的 `alwaysApply: true` 使其每次会话都加载原文。
- 其余规则 `alwaysApply: false` + `paths`，仅在编辑匹配文件时触发。
- 在 IDE 设置页「规则」中核对每条规则的「类型」与「启用」状态是否符合预期。

### 第 3 步：完成技术决策

1. 打开 `docs/TECH-DECISIONS.md`，逐项确认 `TBD` 项（Spring Boot、MyBatis、PostgreSQL、Node、包管理器、UI 库等）。
2. 需要选型时，用 `docs/ADR-TEMPLATE.md` 起草决策记录：≥2 个候选方案、优缺点、成本、许可证、漏洞与传递依赖。
3. 批准后更新 `TECH-DECISIONS.md`，并把版本固化到父 POM 的 `dependencyManagement`/`pluginManagement` 与前端 lockfile。

### 第 4 步：验证规则加载

新建对话（规则仅在会话开始加载），让 CodeBuddy 在不修改文件的情况下列出：

- 当前常驻规则；
- 与指定任务匹配的专题规则；
- 所有 `TBD` 项；
- 尚未配置的自动化检查。

### 第 5 步：落地自动化门禁

按 `automation/README.md` 配置后端（Maven Wrapper、Enforcer、ArchUnit、漏洞/secret 扫描）与前端（固定 Node/包管理器、`npm ci`、lint/type-check/测试）门禁。规则文件不能取代 CI，关键约束应在门禁中强制执行。

### 第 6 步：日常协作约定

- 让 AI 开工前先读 `CODEBUDDY.md` 与相关规则；前端任务遵守 `docs/FRONTEND-STYLE-GUIDE.md`。
- 要求 AI 完成后输出交付报告（实现结果、修改文件、影响、实际验证命令与结果、未运行检查、风险）。
- 修改规则或技术基线本身同样需要审查。

## Token 优化原则

- 常驻规则只保留核心约束、已确认技术和强制工作流（当前 3 条，字符数见 `TOKEN-PROFILE.md`）。
- Java、API、MyBatis/PostgreSQL、安全、Vue、视觉样式、测试及工程细则通过路径条件触发。
- `CODEBUDDY.md` 保持精简，详细说明放在对应规则或 `docs/` 中。
- 不为节省 Token 删除关键约束；通过相关性加载而非降低标准来控制上下文。

## 常见问题（FAQ）

**Q：修改规则后为什么不生效？**
A：规则只在会话开始时加载。修改规则后需要新建对话会话；已有会话不会自动更新规则上下文。

**Q：如何确认当前会话加载了哪些规则？**
A：在对话中直接询问 AI「当前应用了哪些规则」，它会列出本次会话加载的全部规则。

**Q：路径触发规则没有生效？**
A：检查 `paths` 的 glob 是否匹配实际文件路径。匹配深层目录需要 `**/` 前缀（如 `src/**/*.ts`），只写 `src/*.ts` 不会匹配子目录。

**Q：`TBD` 项可以让 AI 直接选一个版本吗？**
A：不可以。`TBD` 表示尚未批准。需要该选择才能继续时，应提出备选方案、影响和风险并请求人工决策（流程见「详细使用步骤」第 3 步）。

## 注意事项

- 规则文件不能取代自动化门禁。依赖版本、测试工具、前端包管理器、CI 平台和基础设施必须按 `docs/TECH-DECISIONS.md` 经批准后锁定。
- 修改规则或技术基线本身也需要审查，规则文件的变更应与代码变更一样纳入 PR 流程。

## 许可证

尚未指定。请在正式发布前补充 `LICENSE` 文件并在此声明。
