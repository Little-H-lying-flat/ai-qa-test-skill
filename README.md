# AI QA Test Skill

面向 Claude Code 的 QA 测试技能仓库，包含项目级测试编排和真实浏览器验证相关 skills。

## Skills

| Skill | 路径 | 用途 |
|---|---|---|
| `ai-test` | `skills/ai-test/SKILL.md` | 根据项目和目标自动路由 UI、功能、接口、部署、性能、A11Y、移动端、自动化、缺陷报告和回归验证，并输出证据驱动测试报告。 |
| `browser-control` | `skills/browser-control/SKILL.md` | 使用 `browser-harness` 控制真实浏览器，完成截图、点击、DOM 指标、页面健康检查和本地/线上对比验证。 |

## 适用场景

- 根据项目生成测试用例并执行真实验证。
- 做 Web UI、功能流、API、部署资源、移动端、可访问性和性能体验测试。
- 回归验证某个 bug 是否修复。
- 输出可复现的 bug report 或测试报告。
- 使用真实浏览器确认页面显示、交互和资源加载状态。

## 安装到 Claude Code

将需要的 skill 目录复制到 Claude Code 的 skills 目录中，例如：

```bash
cp -r skills/ai-test ~/.claude/skills/
cp -r skills/browser-control ~/.claude/skills/
```

Windows Git Bash 示例：

```bash
cp -r skills/ai-test "C:/Users/<USER>/.claude/skills/"
cp -r skills/browser-control "C:/Users/<USER>/.claude/skills/"
```

然后在 Claude Code 中使用：

```text
/ai-test standard 测试首页和核心功能
```

## `ai-test` 工作方式

`ai-test` 是路由型 QA skill，会根据用户目标选择合适的测试工作流和测试类型。

### 工作流

- `discover-testing`：首次接触项目或目标不清楚时，发现测试范围和风险点。
- `daily-testing-workflow`：日常变更、小范围验证、缺陷复测。
- `sprint-testing-workflow`：迭代交付，覆盖需求、用例、核心路径和异常路径。
- `release-testing-workflow`：发布前验证，覆盖冒烟、部署资源、关键路径和阻塞缺陷。

### 测试类型

- 需求分析与测试策略
- 测试用例编写与评审
- UI / 功能测试
- API 测试
- 部署 / 静态资源测试
- 性能体验测试
- 可访问性测试
- 移动端 / 响应式测试
- 自动化测试辅助
- 缺陷上报
- 测试失败定位和修复后回归验证
- TDD 辅助

## `browser-control` 工作方式

`browser-control` 使用 `browser-harness` 调用真实浏览器，适合做 UI 和功能验证。

基本示例：

```bash
browser-harness <<'PY'
new_tab('http://localhost:3000/')
wait_for_load()
print(page_info())
print(capture_screenshot())
PY
```

核心原则：

- 首次导航使用 `new_tab(url)`。
- 每次关键操作后重新截图或读取 DOM 指标。
- 可见元素优先通过截图坐标点击。
- 遇到登录墙、证书页、权限页时停止并标记 BLOCKED。
- 不输入或保存用户凭据。

## 输出报告

默认输出短报告：

```markdown
**Verdict:** PASS | FAIL | BLOCKED | PARTIAL
**范围:** <页面、接口、功能流>
**环境:** <本地/预览/线上/API>

- 通过：<关键通过项>
- 失败/阻塞：<关键失败项或无>
- 证据：<截图路径 / DOM 指标 / HTTP 状态码 / 日志摘要>
- 后续：<建议复测点或修复建议>
```

完整测试或发布验证会输出测试矩阵、缺陷表和回归点。

## 安全与隐私

- 不要把私有项目说明、账号、token、Cookie 或敏感响应体上传到公开仓库。
- API 和安全相关测试必须确认授权范围。
- 对线上生产环境默认只做非破坏性验证。
- 报告中的手机号、订单号、token、Cookie 等信息需要脱敏。

## License

未指定许可证。使用前请根据你的发布需求补充 LICENSE。
