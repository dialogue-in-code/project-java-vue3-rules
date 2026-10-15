# 技术决策待确认清单

未确认项不得由 AI 自行选型。决策完成后更新本文、构建配置、锁文件和规则。

| 项目 | 状态 | 需确定的内容 |
|---|---|---|
| JDK | Java 17 已确认 | 发行版、补丁更新策略、CI 镜像 |
| Spring Boot | 框架已确认，版本 TBD | 精确版本、支持周期和兼容性 |
| MyBatis Starter | TBD | 与 Spring Boot 版本匹配的精确版本 |
| PostgreSQL | 类型已确认，版本 TBD | 主版本、托管/自建、备份恢复策略 |
| Maven | 建议默认，待确认 | Wrapper、Compiler/Enforcer/Surefire/Failsafe 插件版本 |
| Vue 3 | 已确认大版本 | 精确版本和现有依赖情况 |
| TypeScript | 建议采用，待确认 | 是否强制使用及迁移策略 |
| Node.js | TBD | 版本和 CI 环境 |
| 包管理器 | TBD | npm/pnpm/yarn；确认后只使用一种并提交 lockfile |
| 前端构建 | TBD | Vite 等版本 |
| UI 组件库 | TBD | 选择单一主库或明确自研方案 |
| CSS 方案 | TBD | CSS variables、SCSS、CSS Modules、Tailwind 等 |
| 路由/状态管理 | TBD | Vue Router、Pinia 等 |
| API 规范 | TBD | REST、统一响应、错误码、分页、OpenAPI |
| 服务通信/网关/注册发现 | TBD | 技术、超时、重试、熔断与部署 |
| 认证授权 | TBD | OAuth2/OIDC、JWT/session、RBAC、数据权限 |
| 数据库迁移 | TBD | Flyway/Liquibase/其他批准工具 |
| 日志/指标/追踪 | TBD | 组件、格式、告警与保留策略 |
| 缓存/消息队列 | 按需 TBD | 业务必要性、可靠性与运维成本 |
| 部署 | TBD | Docker/Kubernetes/其他平台 |
| 测试/质量门禁 | TBD | 测试框架、覆盖率基线、安全阈值 |
| 代码风格工具 | TBD | Spotless/Checkstyle、ESLint/Prettier 等 |
| 视觉主题 | 提案，待批准 | 主色、语义色、字号、间距、组件库主题 |

## 建议顺序
1. 先定 Spring Boot，再据兼容矩阵选择 MyBatis Starter。
2. 固定 JDK/Maven/PostgreSQL 和 CI 构建基线。
3. 确定 TypeScript、Node、包管理器、组件库、CSS 方案和视觉 tokens。
4. 确定 API、认证、通信、迁移和可观测性。
5. 将批准结果固化到 POM、lockfile、主题文件、CI 与规则。
