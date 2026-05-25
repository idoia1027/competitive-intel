# HTML视觉规范 · 竞品情报报告

## 核心设计语言

对标baseline：`专题报告_抖音小游戏_2026W20.html`

- **字体**：DM Sans（正文）+ DM Mono（标签/数字/来源）
- **布局**：左侧固定sidebar导航（224px）+ 右侧主内容区
- **背景**：`#f8fafc`（页面）/ `#ffffff`（卡片）
- **风格**：白色卡片、圆角10px、浅灰描边、蓝色accent

---

## CSS变量（必须使用）

```css
:root {
  --blue:#1d4ed8; --blue-l:#eff6ff; --blue-m:#bfdbfe;
  --ink:#0f172a; --ink-2:#334155; --ink-3:#64748b; --ink-4:#94a3b8;
  --line:#e2e8f0; --line-l:#f1f5f9; --bg:#f8fafc; --white:#ffffff;
  --green:#16a34a; --green-l:#f0fdf4;
  --amber:#d97706; --amber-l:#fffbeb;
  --red:#dc2626; --red-l:#fef2f2;
  --teal:#0d9488; --teal-l:#f0fdfa;
  --nav-w:224px;
}
```

---

## 来源Badge体系

每条数据必须紧跟来源badge，badge本身是可点击的`<a>`标签。

```html
<!-- 白皮书/行业报告（经媒体转述）-->
<a href="[URL]" target="_blank" class="src src-wp">白皮书·极光月狐 ↗</a>

<!-- 官方发言/官方报道 -->
<a href="[URL]" target="_blank" class="src src-off">官方发言·新浪财经 ↗</a>

<!-- 第三方数据机构 -->
<a href="[URL]" target="_blank" class="src src-qm">QuestMobile ↗</a>

<!-- 推断（无链接，用span） -->
<span class="src src-inf" title="基于公开信息的综合判断">推断</span>

<!-- 待验证假设 -->
<span class="src src-hyp">待验证假设</span>

<!-- 归因修正 -->
<span class="src src-fix">归因修正</span>
```

```css
.src {
  display:inline-flex; align-items:center;
  font-size:10px; font-weight:700; letter-spacing:.3px;
  padding:1px 7px; border-radius:99px;
  vertical-align:middle; margin:0 2px; border:1px solid;
  text-decoration:none;
}
.src-wp  { background:var(--line-l); color:var(--ink-3); border-color:var(--line); }
.src-off { background:var(--blue-l); color:var(--blue); border-color:var(--blue-m); }
.src-qm  { background:var(--teal-l); color:var(--teal); border-color:#99f6e4; }
.src-inf { background:var(--amber-l); color:var(--amber); border-color:#fde68a; }
.src-hyp { background:var(--green-l); color:var(--green); border-color:#bbf7d0; }
.src-fix { background:var(--red-l); color:var(--red); border-color:#fca5a5; }
```

---

## 核心组件

### Sidebar导航
```html
<nav class="nav">
  <div class="nav-brand">
    <div class="nav-brand-label">竞品专题报告</div>
    <div class="nav-brand-title">[竞品名]<br>深度分析</div>
    <div class="nav-brand-meta">W[周次] · [日期]</div>
  </div>
  <div class="nav-items">
    <div class="nav-section-label">核心结论</div>
    <a class="nav-item active" href="#s0">
      <span class="nav-item-num">↑</span>核心判断
    </a>
    <!-- 更多nav-item... -->
  </div>
</nav>
```

### Metric Tile（关键数字展示）
```html
<div class="metric-tile">
  <div class="metric-label">DAU增速</div>
  <div class="metric-val red">+120%</div>
  <div class="metric-sub">2025年同比 <span class="src src-wp">白皮书 ↗</span></div>
</div>
```

### 判断项（已支撑 vs 待验证）
```html
<!-- 已支撑 -->
<div class="verdict-item">
  <div class="verdict-status ok">✓ 已支撑</div>
  <div class="verdict-text">...</div>
</div>

<!-- 待验证假设 -->
<div class="verdict-item hyp">
  <div class="verdict-status hyp">? 待验证</div>
  <div class="verdict-text">...</div>
</div>
```

### 信号条（第一类威胁）
```html
<div class="signal-item">
  <span class="signal-num">①</span>
  <span class="signal-text">
    <span class="src src-wp">来源 ↗</span>
    具体数据和描述...
  </span>
</div>
```

### 机制卡（06B学习视角）
```html
<div class="mech-card">
  <div class="mech-head">
    <span class="mech-num">机制 01</span>
    <span class="mech-title">机制名称</span>
    <span class="badge badge-blue">对应模块</span>
  </div>
  <div class="mech-body">
    <p>分析内容...</p>
    <div class="mech-prereq">
      <div class="prereq-label">执行前提</div>
      前提说明...
    </div>
  </div>
</div>
```

### 行动优先级表
```html
<table class="action-table">
  <thead>
    <tr><th>建议动作</th><th>紧迫性</th><th>执行能力</th><th>优先级</th><th>备注</th></tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>动作描述</strong></td>
      <td><span class="badge badge-red">高</span></td>
      <td>强·内部数据</td>
      <td><span class="priority p0">P0</span></td>
      <td>备注说明</td>
    </tr>
  </tbody>
</table>
```

优先级样式：
```css
.p0 { background:var(--red-l); color:var(--red); border:1px solid #fca5a5; }
.p1 { background:var(--amber-l); color:var(--amber); border:1px solid #fde68a; }
.p2 { background:var(--line-l); color:var(--ink-3); border:1px solid var(--line); }
```

---

## Sidebar滚动高亮脚本（必须包含）

```javascript
const navLinks = document.querySelectorAll('.nav-item');
const sections = document.querySelectorAll('.section');
const obs = new IntersectionObserver(entries => {
  entries.forEach(e => {
    if (e.isIntersecting) {
      navLinks.forEach(a => a.classList.remove('active'));
      const link = document.querySelector(`.nav-item[href="#${e.target.id}"]`);
      if (link) link.classList.add('active');
    }
  });
}, { rootMargin: '-20% 0px -70% 0px' });
sections.forEach(s => obs.observe(s));
```
