---
name: ai-test
description: 根据项目自动判断 UI/功能/接口/部署测试类型，生成测试用例并执行真实验证，输出证据驱动的测试报告。
---

# AI-Test

面向项目的 AI 测试编排技能：根据用户目标和项目结构，判断应做需求分析、测试策略、测试用例设计、UI/功能测试、接口测试、部署/资源测试、性能体验测试、可访问性测试、移动端测试、自动化测试辅助、缺陷上报、测试修复验证还是混合测试；自动生成测试用例；通过真实运行界面、浏览器、HTTP 请求和必要的静态分析执行验证；最后输出可复现的证据驱动测试报告。

## 适用场景

用户表达以下意图时使用：

- “帮我测试这个项目/页面/功能”
- “生成测试用例并执行”
- “检查某些缺陷是否存在”
- “验证线上/本地功能是否正常”
- “做 UI 测试 / 功能测试 / 接口测试”
- “对比本地和线上行为”
- “回归测试这几个问题”
- “根据项目生成测试用例”
- “全站测试 / 冒烟测试 / 回归测试 / 部署验证”
- “分析需求并设计测试策略”
- “写测试用例 / 评审测试用例 / 生成组合测试用例”
- “做发布测试 / 迭代测试 / 日常测试”
- “帮我定位失败测试 / 修复后回归验证”
- “输出缺陷报告 / 测试报告”

## QA Skill 体系映射

参考通用 QA skills 分类，`/ai-test` 是路由型入口，不替代所有测试专家技能，而是把用户目标映射到合适的测试工作流和测试类型。

### 工作流维度

- **discover-testing**：目标不清楚、首次接触项目、需要识别测试范围时，先发现系统入口、风险点和测试类型。
- **daily-testing-workflow**：日常变更验证、缺陷复测、小范围功能确认，优先最小闭环和快速证据。
- **sprint-testing-workflow**：迭代内功能交付，覆盖需求分析、用例设计、核心路径、异常路径和回归点。
- **release-testing-workflow**：发布前验证，覆盖冒烟、部署资源、关键业务路径、兼容性、性能体验和阻塞缺陷。

### 测试类型维度

- **requirements-analysis**：从需求、README、页面文案、接口定义中提炼可测点、隐含规则和风险。
- **test-strategy**：确定范围、优先级、环境、数据、风险、准入/准出标准。
- **test-case-writing**：生成结构化用例、边界用例、异常用例和组合测试用例。
- **test-case-reviewer**：检查用例是否遗漏需求、边界、状态迁移、权限、数据一致性和可验证证据。
- **functional-testing**：验证用户流程和业务规则。
- **manual-testing / exploratory-testing**：在没有明确脚本时按风险探索，记录观察、假设和新缺陷。
- **api-testing**：验证接口状态码、schema、参数、鉴权、错误处理、缓存和 CORS。
- **automation-testing**：根据项目已有框架补充或运行自动化测试；自动化结果只作为证据之一。
- **performance-testing**：观察加载、交互延迟、布局抖动、资源体积和接口耗时。
- **accessibility-testing**：做轻量 A11Y 快检，必要时扩展为系统检查。
- **mobile-testing**：覆盖移动视口、触控交互、响应式布局、移动端资源和横竖屏风险。
- **security-testing**：仅在明确授权上下文中做非破坏性安全测试；未授权时只给防御性建议或标记需要授权。
- **bug-reporting**：把失败结论整理为可复现缺陷，包含环境、步骤、期望、实际、证据和严重性。
- **test-reporting**：输出 PASS/FAIL/BLOCKED/PARTIAL、覆盖范围、证据和后续回归点。
- **test-fixing / regression-verification**：定位失败原因、验证修复是否生效，并补充回归用例。
- **TDD 辅助**：用户明确要求 TDD 时，先把需求转为失败测试，再实现最小修复，最后运行测试和真实表面验证。


1. **先分类，再测试**：不要一上来跑测试框架。先判断测试对象属于 UI、功能流、接口、部署资源、性能还是混合问题。
2. **真实表面优先**：UI 功能通过浏览器实际操作验证；接口通过真实 HTTP 请求验证；部署问题通过线上资源、构建产物和网络响应验证。
3. **测试用例要来自项目**：从路由、页面、组件、接口、配置、README、package scripts、历史缺陷中提炼用例。
4. **证据驱动结论**：每个结论必须附截图、DOM 指标、响应体、状态码、日志或可复现步骤。
5. **区分环境**：本地、预览、线上、GitHub Pages、Vercel 等环境要分开记录，不混淆。
6. **先最小闭环，再扩展覆盖**：先验证用户关心的问题，再补充邻近功能和回归点。
7. **不以 CI 替代人工验证**：测试框架结果可以作为补充，但 UI/功能结论必须来自真实运行表面。

## 执行入口

收到 `/ai-test <目标>` 后，先给出简短测试卡片，再按深度执行。不要把所有任务都升级成完整测试计划。

1. **测试目标**：用户要验证什么。
2. **工作流判定**：discover / daily / sprint / release / regression。
3. **测试类型判定**：需求分析 / 策略 / 用例 / UI / 功能 / 接口 / 部署资源 / 性能 / A11Y / 移动端 / 自动化 / 缺陷报告 / 修复回归 / 混合。
4. **测试深度**：smoke / standard / regression / exhaustive。
5. **目标环境**：本地 / 预览 / 线上 / GitHub Pages / Vercel / API / 多环境对比。
6. **初始测试用例表**：列出 ID、类型、步骤、预期、证据。
7. **证据类型**：截图、DOM 指标、HTTP 响应、日志、构建输出。

执行策略：

- **smoke / daily 小任务**：直接执行，先用 3-6 个核心用例形成最小闭环，最后给短报告。
- **standard**：先输出简短测试卡片和用例表，然后执行核心路径、常见异常和关键证据采集。
- **regression**：围绕用户指定缺陷或历史缺陷复现、验证修复、补充邻近回归点。
- **exhaustive / release**：先输出分批测试矩阵；范围较大或可能影响外部系统时，先请求用户确认。
- **只要求用例/策略/评审**：不启动应用，输出可执行用例、风险矩阵或评审意见；用户要求执行时再运行验证。

如果用户没有指定深度，默认使用 `standard`。如果用户只问“是否存在某缺陷”，默认使用 `regression`。

## 测试路由器

按用户描述自动路由：

- 提到“页面、按钮、图片、地图、布局、动画、卡顿、跳变、响应式” → **UI + 功能测试**。
- 提到“点击、切换、搜索、筛选、详情、返回、流程” → **功能测试**。
- 提到“需求、规则、验收标准、用户故事、PRD、测试点” → **需求分析 + 测试策略/用例设计**。
- 提到“测试策略、测试计划、范围、优先级、风险、准入、准出” → **测试策略**。
- 提到“测试用例、用例评审、边界、等价类、组合、pairwise、PyPICT” → **测试用例编写/评审 + 组合测试**。
- 提到“接口、API、请求、返回、状态码、参数、鉴权、schema” → **接口测试**。
- 提到“本地正常线上异常、部署、资源 404、图片不见、JS/CSS 不加载、LFS、base path、Vercel、GitHub Pages” → **部署/资源测试**。
- 提到“卡顿、慢、首屏、动画、先小后大、layout shift、性能、耗时、加载” → **性能/体验测试**。
- 提到“无障碍、a11y、键盘、读屏、aria、alt、对比度” → **可访问性测试**。
- 提到“手机、移动端、响应式、横屏、竖屏、触摸、viewport” → **移动端测试**。
- 提到“自动化、Playwright、Cypress、pytest、k6、Gatling、Bruno、Supertest、Rest Assured” → **自动化测试辅助 + 真实验证补充**。
- 提到“缺陷报告、bug report、复现步骤、严重性、优先级” → **缺陷上报**。
- 提到“测试失败、修测试、修复后验证、回归这个 bug” → **测试修复分析 + 回归验证**。
- 提到“TDD、先写测试、红绿重构” → **TDD 辅助**。
- 提到“全站、回归、完整测试、发布前、上线前” → **release/exhaustive 测试矩阵，按深度分批执行**。

## 测试深度

- **smoke**：验证核心页面/接口是否可打开、无空白、无明显 broken 资源；适合日常快速检查。
- **standard**：核心路径 + 常见异常 + 关键 DOM/HTTP 指标 + 截图证据；默认深度。
- **regression**：围绕历史缺陷和用户指定缺陷做复现/修复验证，补充邻近回归点。
- **release**：发布前覆盖冒烟、部署资源、关键业务路径、接口健康、移动端/A11Y 快检、性能体验和阻塞缺陷。
- **exhaustive**：页面矩阵、交互矩阵、接口边界、组合用例、部署资源扫描、性能观察、可访问性快检。

用户可显式指定：

```text
/ai-test smoke 测试首页
/ai-test standard 测试地图页面
/ai-test regression 回归图片和地图比例问题
/ai-test exhaustive 全站测试
```

## 项目测试画像

首次测试一个项目时，先生成项目测试画像：

```markdown
### Project Test Profile

- 技术栈：
- 启动命令：
- 主要路由：
- 关键页面：
- API 来源：
- 静态资源目录：
- 部署平台：
- 高风险点：
- 推荐回归用例：
- 需求/验收来源：
- 现有测试框架：
- 自动化可运行命令：
- 测试数据/账号需求：
- 移动端/兼容性风险：
```

优先读取：

- `package.json`
- `README.md`
- `vite.config.*` / `next.config.*` / `webpack.config.*`
- 路由入口，如 `src/App.tsx`、`src/main.tsx`
- API/server 入口，如 `server.*`、`app.*`、`routes/`、`pages/api/`
- `.github/workflows/`、`vercel.json`、部署文档

## 测试类型判定

### 需求分析与测试策略

满足任一条件：

- 用户要求从需求、PRD、README、用户故事或验收标准中提炼测试点。
- 用户要求测试计划、测试范围、优先级、风险评估、准入/准出标准。
- 项目较大或目标不清楚，需要先发现测试面。

常用手段：

- 提炼显性需求、隐含规则、状态迁移、权限边界、数据依赖和异常路径。
- 输出风险矩阵：影响面、发生概率、可观察证据、测试优先级。
- 明确不测范围、阻塞条件和需要用户提供的数据/账号/环境。

### 测试用例编写与评审

满足任一条件：

- 用户要求生成测试用例、评审测试用例、补充边界用例。
- 用户提到等价类、边界值、判定表、状态迁移、组合测试、pairwise、PyPICT。

常用手段：

- 用 Given / When / Then 或步骤-预期格式输出可执行用例。
- 覆盖正向、反向、边界、状态迁移、权限、兼容性、数据一致性。
- 参数组合较多时，先列因子和值，再生成最小覆盖组合；不要暴力枚举无意义组合。
- 评审时标出遗漏、重复、不可验证、预期不清、测试数据缺失和证据不足。

### UI 测试

满足任一条件：

- 用户关心页面显示、布局、图片、动画、地图、按钮、跳转、卡顿、响应式。
- 缺陷描述来自截图或肉眼观感。
- 项目是前端应用，主要入口是浏览器页面。

常用手段：

- 启动本地服务。
- 用 browser-harness 打开页面。
- 截图、读取 DOM、统计图片/按钮/链接/地图容器。
- 坐标点击、导航、等待加载、再次截图。

### 功能测试

满足任一条件：

- 用户关心完整用户流程：进入页面、切换 tab、点击项目、搜索、筛选、查看详情、返回。
- 需要验证多个 UI 步骤串联后的状态。

常用手段：

- 设计 Given / When / Then 测试用例。
- 按真实用户路径操作。
- 记录每步 URL、页面文本、关键 DOM、截图。

### 接口测试

满足任一条件：

- 项目有后端 API、server routes、fetch/XHR、GraphQL、WebSocket。
- 用户明确说“接口测试”“API 测试”。
- 页面数据来自网络请求且问题可能在响应层。

常用手段：

- 搜索 API 路由、fetch、axios、OpenAPI、server 入口。
- 用 curl / HTTP 客户端请求接口。
- 验证状态码、响应 schema、错误处理、鉴权、边界输入。
- 如涉及安全测试，按授权安全研究流程处理。

### 部署/资源测试

满足任一条件：

- 本地正常、线上异常。
- 图片/CSS/JS/音频/地图瓦片不加载。
- GitHub Pages、Vercel、base path、LFS、缓存、404、证书、CORS 问题。

常用手段：

- 抓取线上 HTML，检查 script/link/img URL。
- 检查资源 HTTP 状态码、Content-Type、Content-Length。
- 判断是否返回 Git LFS pointer、HTML 404、错误证书页。
- 对比本地构建产物和线上产物。

### 移动端与响应式测试

满足任一条件：

- 用户关心手机、平板、触摸操作、横竖屏、移动端布局或响应式断点。
- 页面在桌面正常但移动端异常。

常用手段：

- 用不同 viewport 验证核心路径，至少覆盖窄屏、常规手机、桌面。
- 检查触控目标尺寸、横向滚动、折行、固定定位、弹窗遮挡、虚拟键盘影响。
- 对关键页面记录截图、布局尺寸、可点击元素数量和是否出现溢出。

### 自动化测试辅助

满足任一条件：

- 用户要求运行或编写 Playwright、Cypress、pytest、k6、Gatling、Bruno、Supertest、Rest Assured 等测试。
- 项目已有测试框架，需要补充脚本或定位失败原因。

常用手段：

- 先发现现有测试命令和框架，不引入新框架，除非用户明确要求。
- 自动化测试覆盖稳定断言；真实浏览器/HTTP 验证覆盖用户可见行为。
- 失败测试先定位是产品缺陷、测试数据问题、环境问题还是测试脚本脆弱。

### 缺陷上报与测试修复验证

满足任一条件：

- 用户要求输出 bug report、复现步骤、严重性或优先级。
- 用户要求分析失败测试、确认修复是否生效、做回归验证。

常用手段：

- 缺陷报告必须包含环境、前置条件、复现步骤、期望结果、实际结果、影响范围、证据和严重性建议。
- 修复验证必须先复现或确认旧问题，再验证修复路径和邻近回归点。
- 如果无法复现，标记为“未复现”并说明覆盖路径、环境和证据，不直接说问题不存在。


## 测试用例生成

用例格式：

```markdown
| ID | 类型 | 场景 | 前置条件 | 操作步骤 | 预期结果 | 证据 |
|---|---|---|---|---|---|---|
| UI-001 | UI | 图片加载 | 本地服务启动 | 打开 /collection | 图片显示，brokenCount=0 | 截图 + DOM |
| FUNC-001 | 功能 | 展厅切换 | 打开 /exhibition | 点击“漫步展厅” | URL=/hall，列表出现 | 截图 + URL |
| API-001 | 接口 | 获取列表 | API 可访问 | GET /api/items | 200，schema 正确 | 响应体 |
```

测试类型命名：

- `REQ-*`：需求分析和可测点
- `STRAT-*`：测试策略和风险
- `CASE-*`：测试用例设计/评审
- `UI-*`：视觉和布局
- `FUNC-*`：用户流程
- `API-*`：接口
- `DEPLOY-*`：部署和资源
- `REG-*`：回归缺陷
- `PERF-*`：性能/卡顿观察
- `A11Y-*`：可访问性快检
- `MOBILE-*`：移动端和响应式
- `AUTO-*`：自动化测试辅助
- `BUG-*`：缺陷报告
- `FIX-*`：测试失败定位和修复验证

## 浏览器能力集成

AI-Test 本身是测试流程说明，不是独立浏览器进程；但当 `/ai-test` 被调用后，执行者应优先使用 `browser-harness` 通过真实浏览器完成 UI/功能验证。只要当前 Claude Code 会话可用 Bash，就可以调用浏览器。

### browser-harness 基本规则

- 多行命令统一使用 heredoc，避免引号转义问题。
- 首次打开页面使用 `new_tab(url)`，不要用 `goto_url(url)` 覆盖用户当前标签页。
- 每个关键操作后都重新截图，不凭记忆判断页面状态。
- 优先通过截图观察页面；需要结构化指标时再用 `js(...)` 读取 DOM。
- 点击优先使用截图坐标 + `click_at_xy(x, y)`；只有隐藏元素、无可见几何时才用 DOM。
- 如果遇到登录页、证书页、权限页，记录为 BLOCKED 或环境问题，不要输入用户凭据。

### 常用浏览器启动/观察模板

```bash
browser-harness <<'PY'
new_tab('http://localhost:3000/')
wait_for_load()
print(page_info())
print(capture_screenshot())
PY
```

### 页面健康检查模板

用于判断页面是否空白、React 是否挂载、图片是否损坏、地图是否存在。

```bash
browser-harness <<'PY'
new_tab('http://localhost:3000/map')
wait_for_load()
print(page_info())
print(js('''(() => {
  const imgs = [...document.images];
  const broken = imgs.filter(img => img.complete && img.naturalWidth === 0);
  const map = document.querySelector('.leaflet-container');
  const tile = document.querySelector('.leaflet-tile');
  return {
    href: location.href,
    title: document.title,
    rootChildren: document.querySelector('#root')?.children.length ?? null,
    bodyText: document.body.innerText.slice(0, 300),
    imgsCount: imgs.length,
    brokenCount: broken.length,
    broken: broken.slice(0, 10).map(img => img.currentSrc || img.src),
    hasMap: !!map,
    mapRect: map ? (() => {
      const r = map.getBoundingClientRect();
      return { x: Math.round(r.x), y: Math.round(r.y), w: Math.round(r.width), h: Math.round(r.height) };
    })() : null,
    firstTile: tile?.currentSrc || tile?.src || ''
  };
})()'''))
print(capture_screenshot())
PY
```

### 本地/线上对比模板

用于发现“本地正常、线上异常”的部署问题、资源路径问题、地图比例问题。

```bash
browser-harness <<'PY'
import time

pages = [
  ('local', 'http://localhost:3000/map'),
  ('online', 'https://example.com/map'),
]

for name, url in pages:
    new_tab(url)
    wait_for_load()
    time.sleep(3)
    print('\nPAGE', name, page_info())
    print(js('''(() => {
      const imgs = [...document.images];
      const broken = imgs.filter(img => img.complete && img.naturalWidth === 0);
      const map = document.querySelector('.leaflet-container');
      const tile = document.querySelector('.leaflet-tile');
      return {
        href: location.href,
        imgsCount: imgs.length,
        brokenCount: broken.length,
        hasMap: !!map,
        mapRect: map ? (() => {
          const r = map.getBoundingClientRect();
          return { x: Math.round(r.x), y: Math.round(r.y), w: Math.round(r.width), h: Math.round(r.height) };
        })() : null,
        firstTile: tile?.currentSrc || tile?.src || ''
      };
    })()'''))
    print(capture_screenshot())
PY
```

### 交互功能测试模板

用于点击按钮、导航、切换 tab、展开弹窗等真实用户路径。

```bash
browser-harness <<'PY'
new_tab('http://localhost:3000/exhibition')
wait_for_load()
print(capture_screenshot())

# 根据截图坐标点击目标；点击后必须重新截图验证
click_at_xy(800, 420)
wait_for_load()
print(page_info())
print(capture_screenshot())
PY
```

## 接口发现策略

搜索以下信号：

- `fetch(`
- `axios`
- `XMLHttpRequest`
- `new WebSocket`
- `EventSource`
- `/api/`
- `graphql`
- `openapi`
- `swagger`
- `routes`
- `server`

输出接口清单：

```markdown
| Method | URL | 来源文件 | 参数 | 鉴权 | 测试优先级 |
|---|---|---|---|---|---|
```

接口测试至少覆盖：

- 正常请求；
- 缺少必填参数；
- 非法类型；
- 边界值；
- 未授权；
- 越权/跨对象访问（仅在授权上下文内）；
- 缓存和 CORS 头。

## 资源响应检查模板

浏览器确认异常后，用 HTTP 进一步判断资源是否 404、是否返回 HTML、是否返回 Git LFS pointer。

```bash
python - <<'PY'
import urllib.request
for url in [
    'https://example.com/assets/index.js',
    'https://example.com/import/picture1.jpg',
]:
    try:
        r = urllib.request.urlopen(url, timeout=20)
        data = r.read(120)
        print(url, r.status, r.getheader('content-type'), r.getheader('content-length'))
        print(data[:80])
    except Exception as e:
        print(url, type(e).__name__, e)
PY
```

## UI 可访问性快检

前端页面在 standard 及以上深度建议加入轻量 A11Y 检查：

- 图片是否有有效 `alt`。
- 按钮/链接是否有文本或 `aria-label`。
- 表单控件是否有关联 label。
- 关键交互是否可键盘触达。
- 弹窗是否能关闭、是否有焦点陷阱风险。
- 颜色对比是否存在明显不可读问题。

## 性能/卡顿观察

用户提到卡顿、先小后大、突然放大、慢加载时启用：

- 记录页面首屏可见时间。
- 记录图片 `complete` / `naturalWidth` 变化。
- 路由切换时按 50ms 间隔采样 DOM 快照。
- 观察布局尺寸变化、图片占位变化、动画时长。
- 标记 `duration > 500ms` 的过长入场动画。

路由切换采样示例：

```bash
browser-harness <<'PY'
new_tab('http://localhost:3000/exhibition')
wait_for_load()
print(js('''(async () => {
  const frames = [];
  const start = performance.now();
  const snap = () => {
    const img = [...document.images].find(i => i.getBoundingClientRect().width > 20);
    const r = img?.getBoundingClientRect();
    frames.push({
      t: Math.round(performance.now() - start),
      path: location.pathname,
      img: img ? { complete: img.complete, nw: img.naturalWidth, w: Math.round(r.width), h: Math.round(r.height) } : null,
      text: document.body.innerText.slice(0, 80)
    });
  };
  snap();
  document.querySelector('a[href="/hall"]')?.click();
  for (let i = 0; i < 25; i++) {
    await new Promise(r => setTimeout(r, 50));
    snap();
  }
  return frames;
})()'''))
PY
```

## 部署平台专用检查

### GitHub Pages

检查：

- Vite/前端 `base` 是否为仓库子路径。
- SPA 是否生成 `404.html`。
- HTML 中 script/link 是否指向正确子路径。
- LFS 图片是否被 checkout 成实体文件。
- 图片响应是否是真 JPEG/PNG，而不是 LFS pointer。
- 线上路由直接访问是否正常。

### Vercel

检查：

- `vercel.json` 或平台 rewrite 是否支持 SPA。
- 生产域名和预览域名是否一致。
- 环境变量是否存在。
- 图片路径是否以根路径访问。
- 自定义域名/证书是否正确。
- 本地和 Vercel 构建产物是否一致。

## 失败决策树

### 页面空白

1. 检查 `#root` children 数量。
2. 检查 HTML script/link 路径。
3. 检查 JS/CSS 是否 404。
4. 检查 base path / SPA fallback。
5. 检查浏览器控制台错误。

### 图片损坏

1. 检查 `img.src`。
2. 检查 `naturalWidth` / `complete`。
3. HTTP 请求图片 URL。
4. 判断是否 404、HTML、LFS pointer、错误 Content-Type。
5. 检查 public 路径、部署 artifact、LFS checkout。

### 地图比例异常

1. 检查 `.leaflet-container` 的 `mapRect`。
2. 检查第一张瓦片 URL 的 zoom，如 `/15/`、`/16/`。
3. 对比本地与线上 viewport、容器尺寸、DPR。
4. 检查 `getBoundsZoom`、`ZOOM_OFFSET`、min/max zoom。
5. 检查地图初始化时机和容器是否在加载后尺寸变化。

### 接口异常

1. 检查请求 URL、method、headers、body。
2. 检查状态码和响应体。
3. 检查鉴权、CORS、CSRF、缓存。
4. 对比本地和线上 API base URL。
5. 用边界输入复测。

## 证据保存规范

优先保存到：

```text
C:/Users/H/AppData/Local/Temp/claude/ai-test/<project-or-target>/
```

推荐命名：

- `local-map-before.png`
- `online-map-after.png`
- `collection-broken-images.png`
- `api-items-response.txt`
- `deploy-html-assets.txt`

报告中至少保留一种证据：

- 截图路径；
- `page_info()` 输出；
- DOM 指标 JSON；
- HTTP 状态码和响应头；
- 控制台/网络错误摘要。

## 缺陷判断规则

- **存在**：真实运行中可复现，且有截图/响应/日志证据。
- **不存在**：按用户路径和邻近路径测试未复现，证据支持正常。
- **部分存在**：主问题修复或不复现，但发现相关子问题。
- **阻塞**：环境、权限、网络、证书、依赖导致无法到达测试表面。
- **待二次验证**：修复已部署但 CDN/缓存/异步任务未完全生效。

## 报告策略

默认输出短报告，只有在 release / exhaustive、用户要求“完整报告”、或失败较多时输出完整报告。

### 短报告模板

```markdown
**Verdict:** PASS | FAIL | BLOCKED | PARTIAL
**范围:** <页面、接口、功能流>
**环境:** <本地/预览/线上/API>

- 通过：<关键通过项>
- 失败/阻塞：<关键失败项或无>
- 证据：<截图路径 / DOM 指标 / HTTP 状态码 / 日志摘要>
- 后续：<建议复测点或修复建议>
```

### 完整报告模板

```markdown
## AI-Test Report: <测试目标>

**Verdict:** PASS | FAIL | BLOCKED | PARTIAL
**测试深度:** smoke | standard | regression | exhaustive
**环境:** 本地 / GitHub Pages / Vercel / API
**范围:** <页面、接口、功能流>

### Summary

- Tested:
- Passed:
- Failed:
- Blocked:

### Environment

- URL:
- Browser viewport:
- Commit / Build:
- Time:

### Project Test Profile

- 技术栈：
- 启动命令：
- 主要路由：
- API 来源：
- 部署平台：
- 高风险点：

### 测试用例

| ID | 类型 | 场景 | 结果 | 证据 |
|---|---|---|---|---|
| UI-001 | UI | 打开地图页 | PASS | screenshot path + DOM metrics |

### 关键证据

```json
{
  "imgsCount": 34,
  "brokenCount": 0,
  "hasMap": true,
  "firstTile": "/16/..."
}
```

### Defects

| ID | Severity | Title | Repro Steps | Expected | Actual | Evidence |
|---|---|---|---|---|---|---|

### 修复建议

1. <建议>

### 回归点

- <后续应重复验证的点>
```

### 缺陷报告模板

```markdown
## Bug Report: <缺陷标题>

**Severity:** Critical | High | Medium | Low
**Priority:** P0 | P1 | P2 | P3
**Environment:** <环境、URL、浏览器、版本、时间>

### Steps to Reproduce
1. <步骤>

### Expected
<期望结果>

### Actual
<实际结果>

### Evidence
- Screenshot: <路径>
- DOM/HTTP/Log: <摘要>

### Impact
<影响范围和风险>

### Regression Scope
- <修复后必须回归的邻近路径>
```

## 不要做什么

- 不要只跑单元测试或类型检查就下 UI/功能结论。
- 不要只读代码就声称视觉正常。
- 不要没有截图或 DOM/HTTP 证据就判断缺陷不存在。
- 不要混淆本地和线上结论。
- 不要把浏览器错误页当成业务页面。
- 不要在未确认授权时做破坏性或越权接口测试。
- 不要在测试中输入用户凭据；遇到登录墙应标记 BLOCKED 并说明。
- 不要把敏感响应体、token、手机号、订单号、Cookie 原样写入报告；需要脱敏后引用。
- 不要为了自动化测试随意引入新框架；优先使用项目已有测试工具。
- 不要把测试脚本通过等同于真实用户体验通过，关键 UI/功能仍要真实表面验证。

## 操作注意事项

- 测试结束后如启动了 Vite 后台任务，记得停止。
- 若 GitHub HTTPS push 网络失败，可使用 GitHub REST API 创建 tree/commit/update ref 绕过普通 git transport，但必须确认父提交和本地改动一致。
- 线上资源问题优先看 HTML 里的 script/link 路径、图片响应内容和 Content-Type。
