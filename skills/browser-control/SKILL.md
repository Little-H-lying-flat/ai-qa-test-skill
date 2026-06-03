---
name: browser-control
description: 使用 browser-harness 通过真实浏览器进行 UI、功能、部署和可视化验证。
---

# Browser Control

用于在 Claude Code 中通过 `browser-harness` 控制真实浏览器，完成页面观察、截图、点击、DOM 指标读取、网络/资源检查和 UI/功能验证。

## 适用场景

用户表达以下意图时使用：

- “打开页面看看”
- “帮我跑一下前端页面”
- “截图验证这个 UI”
- “点击按钮确认功能”
- “检查页面是否空白 / 图片是否加载 / 地图是否显示”
- “对比本地和线上页面”
- “验证修复后的真实浏览器行为”

## 核心原则

1. **真实浏览器优先**：UI 和功能结论必须来自真实页面观察，而不是只读代码或只跑测试。
2. **截图驱动探索**：先截图理解页面，再决定点击、输入、读取 DOM 或抓网络。
3. **首次导航用新标签页**：首次打开页面使用 `new_tab(url)`，避免覆盖用户当前标签页。
4. **操作后必须复验**：每次关键点击、输入、导航后重新截图或读取指标。
5. **坐标点击优先**：可见元素优先用截图坐标点击；隐藏元素或不可见状态再考虑 DOM。
6. **环境分开记录**：本地、预览、线上、移动 viewport 的结论不要混淆。
7. **遇到登录墙停止**：不要输入用户凭据；标记为 BLOCKED 并说明需要用户登录或提供测试环境。

## 基本调用

多行命令使用 heredoc，避免 shell 引号转义问题。

```bash
browser-harness <<'PY'
new_tab('http://localhost:3000/')
wait_for_load()
print(page_info())
print(capture_screenshot())
PY
```

## 页面健康检查

用于判断页面是否空白、应用是否挂载、图片是否损坏、地图或关键容器是否存在。

```bash
browser-harness <<'PY'
new_tab('http://localhost:3000/')
wait_for_load()
print(page_info())
print(js('''(() => {
  const imgs = [...document.images];
  const broken = imgs.filter(img => img.complete && img.naturalWidth === 0);
  const root = document.querySelector('#root, #app, main');
  return {
    href: location.href,
    title: document.title,
    bodyText: document.body.innerText.slice(0, 300),
    rootChildren: root?.children.length ?? null,
    imgsCount: imgs.length,
    brokenCount: broken.length,
    broken: broken.slice(0, 10).map(img => img.currentSrc || img.src),
    buttons: document.querySelectorAll('button').length,
    links: document.querySelectorAll('a').length
  };
})()'''))
print(capture_screenshot())
PY
```

## 交互验证

先截图确认目标位置，再用坐标点击，点击后重新截图确认状态。

```bash
browser-harness <<'PY'
new_tab('http://localhost:3000/')
wait_for_load()
print(capture_screenshot())

click_at_xy(800, 420)
wait_for_load()
print(page_info())
print(capture_screenshot())
PY
```

## 本地/线上对比

用于验证部署路径、静态资源、图片、地图瓦片、路由 fallback 等问题。

```bash
browser-harness <<'PY'
import time

pages = [
  ('local', 'http://localhost:3000/'),
  ('online', 'https://example.com/'),
]

for name, url in pages:
    new_tab(url)
    wait_for_load()
    time.sleep(2)
    print('\nPAGE', name, page_info())
    print(js('''(() => {
      const imgs = [...document.images];
      const broken = imgs.filter(img => img.complete && img.naturalWidth === 0);
      return {
        href: location.href,
        title: document.title,
        text: document.body.innerText.slice(0, 200),
        imgsCount: imgs.length,
        brokenCount: broken.length,
        scripts: [...document.scripts].map(s => s.src).filter(Boolean).slice(0, 10),
        styles: [...document.querySelectorAll('link[rel="stylesheet"]')].map(l => l.href).slice(0, 10)
      };
    })()'''))
    print(capture_screenshot())
PY
```

## 移动端/响应式快检

如 browser-harness 环境支持 viewport 设置，先切换 viewport，再验证核心页面和交互。重点检查横向滚动、遮挡、触控目标、固定定位和弹窗。

```bash
browser-harness <<'PY'
new_tab('http://localhost:3000/')
wait_for_load()
print(js('''(() => ({
  href: location.href,
  viewport: { w: innerWidth, h: innerHeight, dpr: devicePixelRatio },
  scrollWidth: document.documentElement.scrollWidth,
  clientWidth: document.documentElement.clientWidth,
  hasHorizontalOverflow: document.documentElement.scrollWidth > document.documentElement.clientWidth
}))()'''))
print(capture_screenshot())
PY
```

## 证据记录

每次验证至少保留一种证据：

- 截图路径；
- `page_info()` 输出；
- DOM 指标 JSON；
- HTTP 状态码和响应头；
- 控制台或网络错误摘要。

## 阻塞判断

- **登录墙**：停止，不输入凭据，说明需要已登录浏览器或测试账号。
- **证书/权限错误**：记录错误页截图和 URL。
- **服务未启动**：说明需要启动命令或端口。
- **浏览器连接失败**：先确认 browser-harness 是否可用；不要改动无关项目代码。

## 不要做什么

- 不要只读代码就声称 UI 正常。
- 不要只跑类型检查或单元测试就下浏览器功能结论。
- 不要覆盖用户当前浏览器标签页作为首次导航。
- 不要在截图或报告中泄露 token、Cookie、手机号、订单号等敏感信息。
- 不要输入或保存用户凭据。
