# 自动化强制检查建议

AI 规则指导模型，但不能独立防止违规；关键规则应在 CI、构建、代码仓库权限和审批流程中强制执行。

## 后端 P0
- Maven Wrapper 和固定 JDK 17 环境。
- Maven Enforcer 检查 Java 版本与依赖收敛（规则按项目情况确认）。
- `mvnw verify` 执行编译与测试。
- Spotless 或 Checkstyle 统一代码风格。
- ArchUnit 检查分层依赖和禁止包引用。
- 依赖漏洞扫描、secret scanning。
- 保护主分支，合并前必须通过必要检查。

## 前端 P0
- 固定 Node 版本和单一包管理器。
- 提交 lockfile，CI 使用锁定安装；例如采用 npm 后使用 `npm ci`。
- 配置 lint、采用 TypeScript 时的 type-check、单元测试和生产构建。
- Vue 项目可评估 Vitest/Vue Test Utils；端到端测试可评估 Playwright。具体版本和运行环境需先批准。
- 不得用 `eslint-disable`、`@ts-ignore` 或忽略检查掩盖错误；例外需限制范围并记录理由。

## Maven Enforcer 示例
版本属性需先在父 POM/插件管理中批准并定义；下面是配置思路，未在具体项目中验证：

```xml
<plugin>
  <groupId>org.apache.maven.plugins</groupId>
  <artifactId>maven-enforcer-plugin</artifactId>
  <version>${maven-enforcer-plugin.version}</version>
  <executions>
    <execution>
      <id>enforce-build-baseline</id>
      <goals><goal>enforce</goal></goals>
      <configuration>
        <rules>
          <requireJavaVersion><version>[17,18)</version></requireJavaVersion>
          <dependencyConvergence/>
        </rules>
        <fail>true</fail>
      </configuration>
    </execution>
  </executions>
</plugin>
```

Java 编译目标示例：

```xml
<properties>
  <maven.compiler.release>17</maven.compiler.release>
</properties>
```

## ArchUnit 思路示例
按实际包名替换，并确认与正式分层一致：

```java
@AnalyzeClasses(packages = "com.example")
class ArchitectureTest {
    @ArchTest
    static final ArchRule interfaces_must_not_depend_on_mappers =
        noClasses().that().resideInAPackage("..interfaces..")
            .should().dependOnClassesThat()
            .resideInAPackage("..infrastructure.persistence.mapper..");
}
```

## 前端视觉自动化
可自动检查 lint、类型检查、构建、关键交互端到端流程、指定视口是否存在控制台错误，以及经人工审核的截图视觉回归。基础可访问性扫描不能替代键盘与屏幕阅读器人工检查。

信息层次、业务文案、数据密度和产品风格仍应在 PR 中人工审查。

## 门禁治理
不允许未经审批关闭门禁。误报需有证据、批准人、补偿措施和到期日。规则与技术基线本身的变更也必须审查。
