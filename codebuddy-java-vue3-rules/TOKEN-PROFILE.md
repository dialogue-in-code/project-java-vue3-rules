# Token 配置概况（字符数估算）

- `.mdc` 规则文件：14 条。
- 常驻规则：3 条（`00-project-core`、`10-tech-stack`、`30-ai-development-workflow`）。
- 微服务架构与其他专题规则：通过 `paths` 条件触发。
- 3 条常驻规则原文总字符数：2,867。
- 根目录 `CODEBUDDY.md` 字符数：400。
- 常驻规则 + `CODEBUDDY.md` 的粗略文本合计：3,267 字符。
- 字符数不等于 Token 数；实际开销取决于模型分词、CodeBuddy 加载方式、任务相关规则及模型调用轮次。
- 优化重点是避免无关专题规则常驻加载与过宽触发，不以删除关键工程约束为代价。

## 统计口径
字符数 = 去除换行符（`\r\n`/`\n`）后的 UTF-16 字符数，含 frontmatter 与正文。数值为 2026-10-15 内容去重与 paths 收窄后重新统计。

## 常驻规则计数
- `00-project-core`：841 字符。
- `10-tech-stack`：1,021 字符。
- `30-ai-development-workflow`：1,005 字符。

## 触发策略变化
- `20-microservices-architecture` 与 `43-security` 的 `paths` 已收窄，不再对所有 `*.java`/`*.vue` 文件触发，改为仅对架构边界与安全敏感文件触发，降低专题规则被无关加载的概率。
