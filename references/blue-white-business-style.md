# 蓝白商务风格规范

一种专业、简洁、现代的商务设计风格，适合知识类、教育类、方法论类文章。

## 核心设计理念

- **视觉氛围**：清爽的蓝白配色 + 专业感
- **情感基调**：专业、可信、现代、简洁
- **动态效果**：微妙的滚动渐显、悬停效果

## 字体选择

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Noto+Serif+SC:wght@400;500;600;700&family=Noto+Sans+SC:wght@300;400;500;600&display=swap" rel="stylesheet">
```

- **标题字体**：`Noto Serif SC` — 优雅的中文衬线字体，用于标题和引用
- **正文字体**：`Noto Sans SC` — 清晰的中文无衬线字体，用于正文

## 配色方案

```css
:root {
  /* 主色调 */
  --primary: #0369A1;        /* 专业蓝 */
  --primary-light: #0EA5E9;  /* 天蓝 */
  --primary-dark: #0C4A6E;   /* 深蓝 */

  /* 强调色 */
  --accent: #22C55E;         /* 成功绿 */
  --accent-gold: #FCD34D;    /* 金色强调 */

  /* 背景 */
  --background: #F8FAFC;     /* 页面背景 */
  --surface: #FFFFFF;        /* 卡片背景 */
  --surface-alt: #F1F5F9;    /* 交替背景 */

  /* 文字 */
  --text-primary: #0F172A;   /* 主文字 */
  --text-secondary: #475569; /* 次要文字 */
  --text-muted: #94A3B8;     /* 辅助文字 */

  /* 边框 */
  --border: #E2E8F0;
  --border-light: #F1F5F9;
}
```

## 文章结构模板

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>文章标题</title>
  <!-- Google Fonts -->
  <style>
    :root {
      --primary: #0369A1;
      --primary-light: #0EA5E9;
      --primary-dark: #0C4A6E;
      --accent: #22C55E;
      --background: #F8FAFC;
      --surface: #FFFFFF;
      --surface-alt: #F1F5F9;
      --text-primary: #0F172A;
      --text-secondary: #475569;
      --text-muted: #94A3B8;
      --border: #E2E8F0;
    }
    /* 基础样式 */
  </style>
</head>
<body>
  <!-- Hero区域 -->
  <header class="hero">
    <div class="hero-content">
      <span class="hero-tag">分类标签</span>
      <h1>文章标题</h1>
      <p class="hero-subtitle">副标题描述</p>
      <div class="hero-meta">
        <div class="hero-meta-item">元信息</div>
      </div>
    </div>
  </header>

  <!-- 引言 -->
  <section class="intro">
    <div class="intro-content">
      <p>引言内容...</p>
    </div>
  </section>

  <!-- 引用块 -->
  <section class="section">
    <div class="container">
      <div class="quote-block">
        <blockquote>核心引述</blockquote>
        <cite>— 来源</cite>
      </div>
    </div>
  </section>

  <!-- 章节 -->
  <section class="section section-alt">
    <div class="container">
      <div class="section-header">
        <span class="section-number">1</span>
        <span class="section-title">章节标题</span>
        <span class="section-subtitle">副标题</span>
      </div>
      <div class="content-card">
        <p>正文内容...</p>
        <div class="highlight-box">
          <p>重点强调内容</p>
        </div>
        <div class="practice-tips">
          <strong>实践方法</strong>
          <p>行动建议...</p>
        </div>
      </div>
    </div>
  </section>

  <!-- 核心洞察 -->
  <section class="section section-alt">
    <div class="container">
      <div class="key-insight">
        <span class="key-insight-label">核心洞察</span>
        <h3>洞察标题</h3>
        <p>补充说明</p>
      </div>
    </div>
  </section>

  <!-- 总结 -->
  <section class="summary">
    <div class="container">
      <h2 class="summary-title">总结标题</h2>
      <div class="summary-list">
        <div class="summary-item">
          <span class="summary-item-number">1</span>
          <div class="summary-item-content">
            <div class="summary-item-title">要点标题</div>
            <div class="summary-item-desc">要点描述</div>
          </div>
        </div>
      </div>
    </div>
  </section>

  <!-- 页脚 -->
  <footer class="footer">
    <p>内容来源说明</p>
  </footer>
</body>
</html>
```

## 组件样式详解

### 1. Hero区域

```css
.hero {
  background: linear-gradient(135deg, var(--primary-dark) 0%, var(--primary) 50%, var(--primary-light) 100%);
  padding: 6rem 2rem;
  text-align: center;
  position: relative;
  overflow: hidden;
}

.hero::before {
  content: '';
  position: absolute;
  inset: 0;
  background: url("data:image/svg+xml,%3Csvg width='60' height='60' viewBox='0 0 60 60' xmlns='http://www.w3.org/2000/svg'%3E%3Cg fill='none' fill-rule='evenodd'%3E%3Cg fill='%23ffffff' fill-opacity='0.05'%3E%3Ccircle cx='30' cy='30' r='2'/%3E%3C/g%3E%3C/g%3E%3C/svg%3E");
  pointer-events: none;
}

.hero-tag {
  display: inline-block;
  background: rgba(255, 255, 255, 0.15);
  color: white;
  padding: 0.5rem 1.25rem;
  border-radius: 100px;
  font-size: 0.85rem;
  font-weight: 500;
  letter-spacing: 0.05em;
  margin-bottom: 1.5rem;
  backdrop-filter: blur(10px);
}

.hero h1 {
  font-family: 'Noto Serif SC', serif;
  font-size: clamp(2rem, 6vw, 3.5rem);
  font-weight: 700;
  color: white;
  line-height: 1.3;
  margin-bottom: 1.25rem;
}

.hero h1 .highlight {
  background: linear-gradient(135deg, #FCD34D, #FBBF24);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.hero-subtitle {
  font-size: 1.15rem;
  color: rgba(255, 255, 255, 0.9);
  max-width: 600px;
  margin: 0 auto;
  line-height: 1.8;
}
```

### 2. 章节标题

```css
.section-header {
  margin-bottom: 2rem;
  padding-bottom: 1rem;
  border-bottom: 2px solid var(--primary);
  display: inline-block;
}

.section-number {
  display: inline-block;
  background: var(--primary);
  color: white;
  width: 32px;
  height: 32px;
  border-radius: 50%;
  text-align: center;
  line-height: 32px;
  font-weight: 600;
  font-size: 0.9rem;
  margin-right: 0.75rem;
  vertical-align: middle;
}

.section-title {
  font-family: 'Noto Serif SC', serif;
  font-size: 1.5rem;
  font-weight: 600;
  color: var(--text-primary);
  vertical-align: middle;
}

.section-subtitle {
  display: block;
  margin-top: 0.5rem;
  margin-left: 2.5rem;
  font-size: 0.95rem;
  color: var(--text-secondary);
  font-weight: 400;
}
```

### 3. 内容卡片

```css
.content-card {
  background: var(--surface);
  border-radius: 12px;
  padding: 2rem;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.05);
  border: 1px solid var(--border);
  margin-bottom: 1.5rem;
  transition: box-shadow 0.3s ease, transform 0.3s ease;
}

.content-card:hover {
  box-shadow: 0 4px 12px rgba(3, 105, 161, 0.1);
  transform: translateY(-2px);
}
```

### 4. 高亮框

```css
.highlight-box {
  background: linear-gradient(135deg, #EFF6FF, #DBEAFE);
  border-left: 4px solid var(--primary);
  padding: 1.25rem 1.5rem;
  margin: 1.5rem 0;
  border-radius: 0 8px 8px 0;
}

.highlight-box p {
  color: var(--primary-dark);
  margin: 0;
  font-weight: 500;
}
```

### 5. 引用块

```css
.quote-block {
  background: var(--surface-alt);
  padding: 2.5rem;
  margin: 2rem 0;
  border-radius: 12px;
  position: relative;
  text-align: center;
}

.quote-block::before {
  content: '"';
  font-family: 'Noto Serif SC', serif;
  font-size: 4rem;
  color: var(--primary-light);
  position: absolute;
  top: 0.5rem;
  left: 1.5rem;
  line-height: 1;
  opacity: 0.5;
}

.quote-block blockquote {
  font-family: 'Noto Serif SC', serif;
  font-size: 1.2rem;
  font-weight: 500;
  color: var(--text-primary);
  line-height: 1.8;
  font-style: normal;
  max-width: 600px;
  margin: 0 auto;
}

.quote-block cite {
  display: block;
  margin-top: 1rem;
  font-size: 0.9rem;
  color: var(--text-muted);
  font-style: normal;
}
```

### 6. 核心洞察

```css
.key-insight {
  background: linear-gradient(135deg, var(--primary-dark), var(--primary));
  padding: 3rem 2rem;
  margin: 3rem 0;
  text-align: center;
  border-radius: 16px;
  color: white;
}

.key-insight-label {
  display: inline-block;
  background: rgba(255, 255, 255, 0.2);
  padding: 0.35rem 1rem;
  border-radius: 100px;
  font-size: 0.8rem;
  font-weight: 500;
  letter-spacing: 0.05em;
  margin-bottom: 1rem;
}

.key-insight h3 {
  font-family: 'Noto Serif SC', serif;
  font-size: 1.35rem;
  font-weight: 600;
  line-height: 1.6;
  margin-bottom: 1rem;
  max-width: 600px;
  margin-left: auto;
  margin-right: auto;
}

.key-insight p {
  color: rgba(255, 255, 255, 0.9);
  max-width: 500px;
  margin: 0 auto;
  line-height: 1.8;
}
```

### 7. 实践提示

```css
.practice-tips {
  background: linear-gradient(135deg, #ECFDF5, #D1FAE5);
  border: 1px solid #A7F3D0;
  border-radius: 12px;
  padding: 1.5rem;
  margin-top: 1.5rem;
}

.practice-tips-header {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 0.75rem;
}

.practice-tips-header strong {
  color: #065F46;
  font-weight: 600;
}

.practice-tips p {
  color: #047857;
  margin: 0;
}
```

### 8. 总结区域

```css
.summary {
  background: var(--primary-dark);
  padding: 4rem 2rem;
  color: white;
}

.summary-title {
  font-family: 'Noto Serif SC', serif;
  font-size: 1.5rem;
  font-weight: 600;
  margin-bottom: 2rem;
  text-align: center;
}

.summary-item {
  display: flex;
  gap: 1rem;
  margin-bottom: 1.5rem;
  align-items: flex-start;
}

.summary-item-number {
  flex-shrink: 0;
  width: 28px;
  height: 28px;
  background: rgba(255, 255, 255, 0.15);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 600;
  font-size: 0.85rem;
}

.summary-item-title {
  font-weight: 600;
  margin-bottom: 0.25rem;
  color: #FCD34D;
}

.summary-item-desc {
  color: rgba(255, 255, 255, 0.85);
  font-size: 0.95rem;
  line-height: 1.7;
}
```

### 9. 滚动动画

```css
.fade-in {
  opacity: 0;
  transform: translateY(20px);
  transition: opacity 0.6s ease, transform 0.6s ease;
}

.fade-in.visible {
  opacity: 1;
  transform: translateY(0);
}

@media (prefers-reduced-motion: reduce) {
  .fade-in {
    opacity: 1;
    transform: none;
    transition: none;
  }
}
```

```javascript
// Scroll Reveal Animation
const observerOptions = {
  threshold: 0.1,
  rootMargin: '0px 0px -50px 0px'
};

const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('visible');
    }
  });
}, observerOptions);

document.querySelectorAll('.fade-in').forEach(el => {
  observer.observe(el);
});

// Respect reduced motion preference
if (window.matchMedia('(prefers-reduced-motion: reduce)').matches) {
  document.querySelectorAll('.fade-in').forEach(el => {
    el.classList.add('visible');
  });
}
```

## 适用场景

✅ 知识类文章
✅ 教育/学习方法类
✅ 商务/专业内容
✅ 企业培训材料
✅ 方法论/框架类文章

## 不适用场景

❌ 娱乐/休闲类文章
❌ 艺术创作主题
❌ 儿童教育类
❌ 需要"酷炫"效果的科技内容
