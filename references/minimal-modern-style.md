# 极简现代风格设计规范

> 版本 1.0 · 基于 Exaggerated Minimalism + Editorial Typography

## 设计理念

极简现代风格追求**极致的留白与纯粹的排版**。去除所有不必要的装饰，让内容成为唯一的主角。灵感来源于瑞士平面设计、日本极简主义与高端杂志排版。

### 核心原则

| 原则 | 说明 |
|------|------|
| **留白即设计** | 大量负空间，内容呼吸感极强 |
| **字体即装饰** | 字体本身就是视觉焦点 |
| **克制配色** | 单色或双色，微妙的层次变化 |
| **数字改中文** | 「一、二、三」替代「01、02」更优雅 |

## 色彩系统

### 主色板

| 用途 | HEX | CSS 变量 |
|------|-----|----------|
| 墨色（主文字） | `#1a1a1a` | `--ink` |
| 墨淡（次文字） | `#6b6b6b` | `--ink-light` |
| 墨隐（辅助） | `#999999` | `--ink-muted` |
| 纸张（背景） | `#fefefe` | `--paper` |
| 纸暖（卡片背景） | `#f9f7f5` | `--paper-warm` |

### 强调色

| 用途 | HEX | CSS 变量 |
|------|-----|----------|
| 主强调 | `#2d5a4a` | `--accent` |
| 强调亮 | `#3d7a6a` | `--accent-light` |

> 💡 **配色说明**：使用深绿色而非常见的蓝紫色作为强调色，更显沉稳优雅。

## 字体系统

### 字体族

```css
/* 标题：优雅衬线体 */
--serif-display: 'Cormorant Garamond', 'Noto Serif SC', Georgia, serif;

/* 正文：中文优化衬线 */
--serif: 'Noto Serif SC', 'Cormorant Garamond', Georgia, serif;
```

### Google Fonts 引入

```html
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,400;0,500;0,600;1,400;1,500&family=Noto+Serif+SC:wght@400;500;600&display=swap" rel="stylesheet">
```

### 字号规范

| 元素 | 字号 | 行高 | 说明 |
|------|------|------|------|
| Hero 标题 | `clamp(2.5rem, 8vw, 5rem)` | `1.15` | 超大标题 |
| 章节标题 | `2rem` | `1.3` | 衬线体 |
| 卡片小标题 | `1.25rem` | `1.3` | |
| 正文 | `1.05rem` | `2` | 舒适阅读 |
| 引用文字 | `1.35rem` | `1.6` | 斜体 |
| 标签/注释 | `0.7rem` | `1.5` | 大写字母间距 |

## 间距系统

```css
:root {
  /* 基于文章阅读的舒适间距 */
  --section-gap: 5rem;      /* 章节间距 */
  --card-padding: 2.5rem 3rem; /* 卡片内边距 */
  --paragraph-gap: 1.75em;  /* 段落间距 */
}
```

## 内容宽度

```css
article {
  max-width: 720px;  /* 最佳阅读宽度 */
  margin: 0 auto;
  padding: 0 2rem;
}

@media (max-width: 640px) {
  article { padding: 0 1.5rem; }
}
```

## 设计元素

### Hero 区域

- 全屏高度，内容垂直居中
- 顶部装饰线（120px 细线，渐隐效果）
- 底部滚动提示（脉冲动画）

### 分隔线

```html
<div class="divider"><span>标签文字</span></div>
```

两侧各 80px 细线，中间文字，优雅分隔内容区块。

### 引用块

- 米色背景卡片
- 顶部 3px 强调色条
- 衬线斜体文字居中

### 卡片样式

- 左侧 3px 强调色条
- 米色背景 `#f9f7f5`
- 无阴影、无圆角（保持纯粹）

### 首字下沉

```css
.prose p:first-of-type::first-letter {
  font-family: var(--serif-display);
  font-size: 4rem;
  float: left;
  line-height: 0.85;
  margin-right: 0.3rem;
  color: var(--accent);
}
```

### 数字改中文

| 原始 | 优化后 |
|------|--------|
| 01, 02, 03 | 一、二、三 |
| 1. 2. 3. | 一、二、三 |

中文字符更符合中文排版美学，也更易阅读。

## 完整 HTML 模板

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>文章标题</title>
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,400;0,500;0,600;1,400;1,500&family=Noto+Serif+SC:wght@400;500;600&display=swap" rel="stylesheet">
  <style>
    :root {
      --ink: #1a1a1a;
      --ink-light: #6b6b6b;
      --ink-muted: #999;
      --paper: #fefefe;
      --paper-warm: #f9f7f5;
      --accent: #2d5a4a;
      --accent-light: #3d7a6a;
      --serif: 'Noto Serif SC', 'Cormorant Garamond', Georgia, serif;
      --serif-display: 'Cormorant Garamond', 'Noto Serif SC', Georgia, serif;
    }

    * { margin: 0; padding: 0; box-sizing: border-box; }

    html {
      scroll-behavior: smooth;
      font-size: 19px;
    }

    body {
      font-family: var(--serif);
      background: var(--paper);
      color: var(--ink);
      line-height: 1.8;
      -webkit-font-smoothing: antialiased;
    }

    ::selection {
      background: var(--accent);
      color: white;
    }

    /* Hero */
    .hero {
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      text-align: center;
      padding: 3rem 2rem;
      position: relative;
    }

    .hero::before {
      content: '';
      position: absolute;
      top: 0;
      left: 50%;
      transform: translateX(-50%);
      width: 1px;
      height: 120px;
      background: linear-gradient(to bottom, transparent, var(--ink-muted));
    }

    .overline {
      font-size: 0.7rem;
      letter-spacing: 0.35em;
      text-transform: uppercase;
      color: var(--ink-muted);
      margin-bottom: 3rem;
    }

    .hero h1 {
      font-family: var(--serif-display);
      font-size: clamp(2.5rem, 8vw, 5rem);
      font-weight: 400;
      line-height: 1.15;
      max-width: 14ch;
      margin-bottom: 2rem;
    }

    .hero h1 em {
      font-style: italic;
      color: var(--accent);
    }

    .hero-subtitle {
      font-size: 1.1rem;
      color: var(--ink-light);
      max-width: 38ch;
      line-height: 1.7;
    }

    .scroll-indicator {
      position: absolute;
      bottom: 3rem;
      left: 50%;
      transform: translateX(-50%);
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 1rem;
      color: var(--ink-muted);
      font-size: 0.65rem;
      letter-spacing: 0.3em;
      text-transform: uppercase;
    }

    .scroll-indicator::after {
      content: '';
      width: 1px;
      height: 50px;
      background: var(--ink-muted);
      animation: pulse 2s ease-in-out infinite;
    }

    @keyframes pulse {
      0%, 100% { opacity: 0.3; }
      50% { opacity: 1; }
    }

    /* Article */
    article {
      max-width: 720px;
      margin: 0 auto;
      padding: 0 2rem;
    }

    .prose {
      padding: 6rem 0;
    }

    .prose p {
      font-size: 1.05rem;
      line-height: 2;
      color: var(--ink);
      margin-bottom: 1.75em;
    }

    .prose p:first-of-type::first-letter {
      font-family: var(--serif-display);
      font-size: 4rem;
      float: left;
      line-height: 0.85;
      margin-right: 0.3rem;
      margin-top: 0.15rem;
      color: var(--accent);
    }

    /* Divider */
    .divider {
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 4rem 0;
    }

    .divider::before,
    .divider::after {
      content: '';
      flex: 1;
      max-width: 80px;
      height: 1px;
      background: var(--ink-muted);
      opacity: 0.3;
    }

    .divider span {
      padding: 0 1.5rem;
      color: var(--ink-muted);
      font-size: 0.75rem;
      letter-spacing: 0.2em;
    }

    /* Blockquote */
    blockquote {
      margin: 4rem 0;
      padding: 3rem;
      background: var(--paper-warm);
      text-align: center;
    }

    blockquote::before {
      content: '';
      position: absolute;
      top: 0;
      left: 0;
      right: 0;
      height: 3px;
      background: var(--accent);
    }

    blockquote p {
      font-family: var(--serif-display);
      font-size: 1.35rem;
      font-style: italic;
      line-height: 1.6;
      color: var(--ink);
    }

    blockquote cite {
      display: block;
      margin-top: 1.5rem;
      font-size: 0.75rem;
      font-style: normal;
      letter-spacing: 0.15em;
      color: var(--ink-muted);
      text-transform: uppercase;
    }

    /* Section */
    .section {
      padding: 5rem 0;
      border-top: 1px solid rgba(0,0,0,0.06);
    }

    .section-header {
      margin-bottom: 3rem;
    }

    .section-number {
      display: inline-block;
      font-family: var(--serif-display);
      font-size: 0.85rem;
      color: var(--accent);
      margin-bottom: 0.75rem;
    }

    .section-title {
      font-family: var(--serif-display);
      font-size: 2rem;
      font-weight: 400;
      color: var(--ink);
    }

    /* Card */
    .card {
      background: var(--paper-warm);
      padding: 2.5rem 3rem;
      margin: 2rem 0;
      position: relative;
    }

    .card::before {
      content: '';
      position: absolute;
      left: 0;
      top: 0;
      bottom: 0;
      width: 3px;
      background: var(--accent);
    }

    .card h3 {
      font-family: var(--serif-display);
      font-size: 1.25rem;
      font-weight: 500;
      margin-bottom: 1.25rem;
    }

    .card p {
      font-size: 1rem;
      line-height: 1.9;
      color: var(--ink-light);
      margin-bottom: 1.25rem;
    }

    .highlight-box {
      background: var(--paper);
      padding: 1.5rem 2rem;
      margin: 1.5rem 0;
      border-left: 2px solid var(--accent);
    }

    .highlight-box p {
      font-size: 0.95rem;
      color: var(--ink);
      margin: 0;
      font-style: italic;
    }

    /* Insight */
    .insight {
      background: var(--ink);
      color: white;
      padding: 4rem 3rem;
      margin: 5rem -2rem;
      text-align: center;
    }

    .insight-label {
      font-size: 0.65rem;
      letter-spacing: 0.3em;
      text-transform: uppercase;
      color: var(--accent-light);
      margin-bottom: 1.5rem;
    }

    .insight h3 {
      font-family: var(--serif-display);
      font-size: 1.5rem;
      font-weight: 400;
      line-height: 1.5;
      margin-bottom: 1.25rem;
    }

    .insight p {
      font-size: 0.9rem;
      color: rgba(255,255,255,0.5);
      font-style: italic;
    }

    /* Summary */
    .summary {
      padding: 5rem 0;
      border-top: 1px solid rgba(0,0,0,0.06);
    }

    .summary h2 {
      font-family: var(--serif-display);
      font-size: 1.75rem;
      font-weight: 400;
      text-align: center;
      margin-bottom: 3rem;
    }

    .summary-grid {
      display: grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 0;
    }

    .summary-item {
      padding: 1.5rem 0;
      border-bottom: 1px solid rgba(0,0,0,0.06);
      font-size: 0.95rem;
      line-height: 1.7;
      color: var(--ink-light);
    }

    .summary-item:nth-child(odd) {
      padding-right: 2rem;
      border-right: 1px solid rgba(0,0,0,0.06);
    }

    .summary-item:nth-child(even) {
      padding-left: 2rem;
    }

    .summary-item strong {
      color: var(--ink);
      font-weight: 500;
    }

    .summary-item .num {
      display: inline-block;
      font-family: var(--serif-display);
      font-size: 0.8rem;
      color: var(--accent);
      margin-right: 0.5rem;
    }

    .cta-box {
      text-align: center;
      margin-top: 4rem;
    }

    .cta {
      display: inline-block;
      padding: 1rem 2.5rem;
      background: var(--ink);
      color: white;
      font-size: 0.8rem;
      letter-spacing: 0.15em;
      text-transform: uppercase;
      text-decoration: none;
      transition: background 0.3s ease;
    }

    .cta:hover {
      background: var(--accent);
    }

    /* Footer */
    footer {
      padding: 4rem 2rem;
      text-align: center;
      border-top: 1px solid rgba(0,0,0,0.06);
    }

    footer p {
      font-size: 0.8rem;
      color: var(--ink-muted);
    }

    /* Animation */
    .reveal {
      opacity: 0;
      transform: translateY(30px);
      transition: opacity 0.8s ease, transform 0.8s ease;
    }

    .reveal.visible {
      opacity: 1;
      transform: translateY(0);
    }

    /* Responsive */
    @media (max-width: 768px) {
      .summary-grid {
        grid-template-columns: 1fr;
      }
      .summary-item:nth-child(odd) {
        padding-right: 0;
        border-right: none;
      }
      .summary-item:nth-child(even) {
        padding-left: 0;
      }
    }

    @media (max-width: 640px) {
      html { font-size: 17px; }
      article { padding: 0 1.5rem; }
      .insight {
        margin-left: -1.5rem;
        margin-right: -1.5rem;
        padding: 3rem 1.5rem;
      }
      .card { padding: 2rem; }
      .prose p:first-of-type::first-letter { font-size: 3rem; }
    }

    @media (prefers-reduced-motion: reduce) {
      * { animation: none !important; transition: none !important; }
      html { scroll-behavior: auto; }
    }
  </style>
</head>
<body>
  <header class="hero">
    <span class="overline">分类标签</span>
    <h1>文章<em>标题</em></h1>
    <p class="hero-subtitle">副标题描述文字</p>
    <div class="scroll-indicator">阅读</div>
  </header>

  <article>
    <section class="prose reveal">
      <p>首段正文内容，首字会自动放大下沉...</p>
      <p>第二段内容...</p>
    </section>

    <div class="divider"><span>分隔</span></div>

    <blockquote class="reveal">
      <p>引用内容文字</p>
      <cite>— 来源</cite>
    </blockquote>

    <section class="section reveal">
      <div class="section-header">
        <span class="section-number">一</span>
        <h2 class="section-title">章节标题</h2>
      </div>
      <div class="card">
        <h3>卡片小标题</h3>
        <p>卡片正文内容...</p>
        <div class="highlight-box">
          <p>重点强调内容</p>
        </div>
        <p><strong>实践方法：</strong>行动建议...</p>
      </div>
    </section>

    <div class="insight reveal">
      <span class="insight-label">核心洞察</span>
      <h3>重要观点<br>分两行显示</h3>
      <p>— 来源</p>
    </div>

    <section class="summary reveal">
      <h2>总结</h2>
      <div class="summary-grid">
        <div class="summary-item">
          <span class="num">一</span><strong>要点一</strong> — 描述文字
        </div>
        <div class="summary-item">
          <span class="num">二</span><strong>要点二</strong> — 描述文字
        </div>
      </div>
      <div class="cta-box">
        <a href="#" class="cta">行动号召</a>
      </div>
    </section>
  </article>

  <footer>
    <p>内容基于视频字幕翻译整理</p>
  </footer>

  <script>
    const revealElements = document.querySelectorAll('.reveal');
    const observer = new IntersectionObserver(
      (entries) => {
        entries.forEach((entry) => {
          if (entry.isIntersecting) {
            entry.target.classList.add('visible');
          }
        });
      },
      { threshold: 0.1, rootMargin: '0px 0px -60px 0px' }
    );
    revealElements.forEach((el) => observer.observe(el));
  </script>
</body>
</html>
```

## 与其他风格的区别

| 特点 | 极简现代 | 蓝白商务 | Notion深色 |
|------|---------|---------|-----------|
| 背景 | 纯白 `#fefefe` | 浅灰蓝 `#F8FAFC` | 深灰 `#191919` |
| 强调色 | 深绿 `#2d5a4a` | 专业蓝 `#0369A1` | 彩色标注系统 |
| 字体 | 全衬线 | 衬线标题+无衬线正文 | 衬线标题+无衬线正文 |
| 装饰 | 几乎无 | 阴影、渐变 | 无 |
| 卡片 | 左侧色条 | 阴影圆角 | 无边框 |
| 数字 | 中文「一、二」 | 阿拉伯「01」 | 无 |
