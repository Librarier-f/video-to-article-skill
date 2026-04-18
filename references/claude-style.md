---
name: claude-style
description: Claude 经典设计风格 — 精致优雅的中文学术文章排版
---

# Claude 经典设计风格

此风格基于实际文章优化后的设计系统，呈现精致、优雅、编辑感强的视觉效果。

## 核心 Design Tokens

```css
--ink-black: #1a1a1a;      /* 主文字 */
--ink-gray: #4a4a4a;        /* 次要文字 */
--ink-light: #8a8a8a;       /* 辅助文字 */
--paper: #faf8f5;           /* 背景色-米白 */
--paper-warm: #f5f0e8;      /* 背景色-暖白 */
--accent-purple: #c96442;   /* 强调色-赭石红 */
--accent-teal: #0d9488;     /* 强调色-青色 */
--shadow-soft: 0 4px 30px rgba(0,0,0,0.06);
--shadow-medium: 0 8px 40px rgba(0,0,0,0.1);
```

## 字体规范

- **标题字体**：`Noto Serif SC`（Google Fonts 中文衬线字体）
- **正文字体**：`Noto Sans SC`（Google Fonts 中文无衬线字体）
- **字重**：标题 600-700，正文 300-500

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Noto+Serif+SC:wght@300;400;500;600;700&family=Noto+Sans+SC:wght@300;400;500;700&display=swap" rel="stylesheet">
```

## 关键样式特征

### 1. 噪点纹理背景
为页面添加细腻的纸质纹理感：
```css
body::before {
  content: '';
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  opacity: 0.025;
  z-index: 1000;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 200 200' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.8' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%' height='100%' filter='url(%23noise)'/%3E%3C/svg%3E");
}
```

### 2. Hero 全屏布局
```css
.hero {
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  text-align: center;
  padding: 4rem 2rem;
  background: linear-gradient(180deg, var(--paper-warm) 0%, var(--paper) 100%);
}

.hero h1 {
  font-family: 'Noto Serif SC', serif;
  font-size: clamp(2.2rem, 7vw, 4.5rem);
  font-weight: 700;
  line-height: 1.2;
  letter-spacing: -0.02em;
}

.hero h1 .highlight {
  color: var(--accent-purple);
}
```

### 3. 首字下沉
```css
.intro p:first-of-type::first-letter {
  font-family: 'Noto Serif SC', serif;
  font-size: 3.2rem;
  float: left;
  line-height: 1;
  margin-right: 0.5rem;
  margin-top: 0.15rem;
  color: var(--accent-purple);
}
```

### 4. 方法卡片样式
带有大号数字水印和图标：
```css
.method {
  background: white;
  border-radius: 8px;
  padding: 2.5rem;
  box-shadow: var(--shadow-soft);
  margin: 2.5rem 0;
  position: relative;
  overflow: hidden;
  transition: transform 0.3s ease, box-shadow 0.3s ease;
}

.method:hover {
  transform: translateY(-4px);
  box-shadow: var(--shadow-medium);
}

.method-number {
  position: absolute;
  top: -0.5rem;
  right: 1.5rem;
  font-family: 'Noto Serif SC', serif;
  font-size: 6rem;
  font-weight: 700;
  color: var(--accent-purple);
  opacity: 0.06;
  line-height: 1;
}

.method-icon {
  width: 48px;
  height: 48px;
  background: linear-gradient(135deg, var(--accent-purple), #d4826a);
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
}
```

### 5. Highlight 强调框
```css
.highlight-box {
  background: linear-gradient(135deg, var(--paper-warm), var(--paper));
  border-left: 3px solid var(--accent-purple);
  padding: 1.25rem 1.5rem;
  margin: 1.5rem 0;
  border-radius: 0 6px 6px 0;
}
```

### 6. 引用块样式
```css
.quote-block {
  background: linear-gradient(135deg, var(--paper-warm) 0%, var(--paper) 100%);
  padding: 3rem 2.5rem;
  margin: 4rem 0;
  position: relative;
  text-align: center;
  border-left: 4px solid var(--accent-teal);
}

.quote-block blockquote {
  font-family: 'Noto Serif SC', serif;
  font-size: clamp(1.1rem, 3vw, 1.4rem);
  font-weight: 500;
  line-height: 1.7;
  color: var(--ink-black);
  font-style: normal;
}
```

### 7. 深色洞察区域
```css
.key-insight {
  background: linear-gradient(135deg, #1a1a1a, #2d2d2d);
  color: white;
  padding: 4rem;
  margin: 5rem -2rem;
  text-align: center;
}

.key-insight h3 {
  font-family: 'Noto Serif SC', serif;
  font-size: clamp(1.3rem, 3vw, 1.8rem);
  font-weight: 600;
  line-height: 1.5;
}
```

### 8. 滚动渐显动画
```css
.reveal {
  opacity: 0;
  transform: translateY(40px);
  transition: opacity 0.7s ease, transform 0.7s ease;
}

.reveal.visible {
  opacity: 1;
  transform: translateY(0);
}
```

### 9. 章节标题样式
```css
.section-label {
  font-size: 0.7rem;
  letter-spacing: 0.3em;
  color: var(--accent-purple);
  margin-bottom: 1rem;
  display: block;
}

.section-title {
  font-family: 'Noto Serif SC', serif;
  font-size: clamp(1.5rem, 4vw, 2rem);
  font-weight: 600;
  line-height: 1.3;
  position: relative;
}

.section-title::after {
  content: '';
  position: absolute;
  bottom: -8px;
  left: 0;
  width: 50px;
  height: 3px;
  background: var(--accent-purple);
}
```

### 10. 入场动画
```css
@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(30px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes fadeIn { to { opacity: 1; } }

@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(8px); }
}
```

## 页面结构模板

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>文章标题</title>
  <!-- Google Fonts -->
  <style>
    /* Design Tokens + 所有样式 */
  </style>
</head>
<body>
  <!-- Hero 区域 -->
  <header class="hero">
    <span class="overline">分类标签</span>
    <h1>文章标题<span class="highlight">高亮关键词</span></h1>
    <p class="hero-subtitle">副标题描述</p>
    <div class="scroll-hint">
      <span>开始阅读</span>
      <svg>...</svg>
    </div>
  </header>

  <article class="container">
    <!-- 引言 -->
    <section class="intro reveal">
      <p>首段内容，首字会自动下沉...</p>
    </section>

    <!-- 引用块 -->
    <div class="quote-block reveal">
      <blockquote>核心引述</blockquote>
      <cite>— 来源</cite>
    </div>

    <!-- 方法章节 -->
    <section class="reveal">
      <div class="section-header">
        <span class="section-label">第X种</span>
        <h2 class="section-title">章节标题</h2>
      </div>
      <div class="method">
        <span class="method-number">01</span>
        <div class="method-header">
          <div class="method-icon">
            <svg>...</svg>
          </div>
          <h3>小标题</h3>
        </div>
        <p>正文内容...</p>
        <div class="highlight-box">
          <p>强调内容...</p>
        </div>
      </div>
    </section>

    <!-- 深色洞察区域 -->
    <div class="key-insight reveal">
      <span class="key-insight-label">核心洞察</span>
      <h3>洞察标题</h3>
      <p>洞察描述</p>
    </div>
  </article>

  <!-- 总结区域 -->
  <section class="summary reveal">
    <h2>总结标题</h2>
    <p>总结内容...</p>
    <span class="takeaway">核心要点</span>
  </section>

  <!-- Footer -->
  <footer class="footer">
    <p>内容基于来源整理</p>
  </footer>

  <script>
    // 滚动渐显逻辑
    const revealElements = document.querySelectorAll('.reveal');
    const revealOnScroll = () => {
      const windowHeight = window.innerHeight;
      revealElements.forEach(el => {
        const elementTop = el.getBoundingClientRect().top;
        const revealPoint = 120;
        if (elementTop < windowHeight - revealPoint) {
          el.classList.add('visible');
        }
      });
    };
    window.addEventListener('scroll', revealOnScroll);
    window.addEventListener('load', revealOnScroll);
  </script>
</body>
</html>
```

## 经典配色组合

| 元素 | 色值 | 用途 |
|------|------|------|
| 主背景 | `#faf8f5` | 米白色，温润的纸质感 |
| 暖白背景 | `#f5f0e8` | Hero渐变、卡片背景 |
| 主文字 | `#1a1a1a` | 正文、标题 |
| 次要文字 | `#4a4a4a` | 描述文字 |
| 辅助文字 | `#8a8a8a` | 标签、注释 |
| 强调色 | `#c96442` | 标题数字、高亮边框、图标背景、首字下沉 |
| 辅助强调 | `#0d9488` | 引用块边框 |
| 深色背景 | `#1a1a1a` | 洞察区域 |

## 设计特点总结

1. **温润纸质感** - 米白背景 + 噪点纹理，营造纸质阅读体验
2. **衬线标题优雅** - Noto Serif SC 作为标题字体，传递智识感
3. **首字下沉** - 引言段落首字放大并着色，增强编辑感
4. **大号数字水印** - 方法卡片右上角的淡色大数字，层次分明
5. **图标+标题组合** - 渐变背景圆角图标配合标题，视觉焦点明确
6. **深浅交替** - 浅色正文区与深色洞察区域交替，节奏感强
7. **微动效** - Hover 上浮、滚动渐显、入场动画，精致而不喧宾夺主
8. **响应式** - clamp() 函数实现流畅的字号过渡

## 使用场景

当用户说「Claude风格」「输出claude风格的文章」时，使用此规范生成 HTML。
