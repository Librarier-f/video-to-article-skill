# 暗黑科技风格规范

一种赛博朋克/黑客帝国风格的暗黑科技设计，适合技术类、教育类、方法论类文章。

## 核心设计理念

- **视觉氛围**：深邃的黑色背景 + 霓虹光效
- **情感基调**：科技感、未来感、专业感
- **动态效果**：网格动画、扫描线、故障艺术(Glitch)

## 字体选择

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;500;600;700;800;900&family=Rajdhani:wght@300;400;500;600;700&family=Noto+Sans+SC:wght@300;400;500;700&display=swap" rel="stylesheet">
```

- **标题字体**：`Orbitron` — 科技感十足的几何无衬线字体
- **正文字体**：`Rajdhani` + `Noto Sans SC` — 现代感中文支持

## 配色方案

```css
:root {
  /* 背景层次 */
  --void-black: #050508;      /* 最深背景 */
  --deep-space: #0a0a0f;      /* 深空背景 */
  --neural-gray: #12121a;     /* 神经灰 */
  --cyber-dark: #1a1a2e;      /* 赛博暗色 */

  /* 强调色 */
  --matrix-green: #00ff41;    /* 矩阵绿 */
  --neon-cyan: #00f0ff;       /* 霓虹青 */
  --plasma-purple: #b24bf3;   /* 等离子紫 */
  --alert-red: #ff0055;       /* 警报红 */

  /* 文字色 */
  --data-white: #e0e0e0;      /* 数据白 */
  --muted-gray: #6b6b7b;      /* 柔和灰 */

  /* 发光效果 */
  --glow-cyan: 0 0 20px rgba(0, 240, 255, 0.5);
  --glow-green: 0 0 20px rgba(0, 255, 65, 0.5);
  --glow-purple: 0 0 20px rgba(178, 75, 243, 0.5);

  /* 网格线 */
  --grid-line: rgba(0, 240, 255, 0.08);
}
```

## 核心视觉效果

### 1. 动态网格背景

```css
/* 网格背景 */
body::before {
  content: '';
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 0;
  background:
    linear-gradient(90deg, var(--grid-line) 1px, transparent 1px),
    linear-gradient(var(--grid-line) 1px, transparent 1px);
  background-size: 50px 50px;
  animation: gridMove 20s linear infinite;
}

@keyframes gridMove {
  0% { transform: perspective(500px) rotateX(60deg) translateY(0); }
  100% { transform: perspective(500px) rotateX(60deg) translateY(50px); }
}
```

### 2. 扫描线效果

```css
/* 扫描线 */
body::after {
  content: '';
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  pointer-events: none;
  z-index: 1000;
  background: repeating-linear-gradient(
    0deg,
    rgba(0, 0, 0, 0.15),
    rgba(0, 0, 0, 0.15) 1px,
    transparent 1px,
    transparent 2px
  );
  animation: scanline 10s linear infinite;
}

@keyframes scanline {
  0% { transform: translateY(0); }
  100% { transform: translateY(100%); }
}
```

### 3. 故障艺术标题 (Glitch Effect)

```css
.hero h1 .highlight {
  color: var(--neon-cyan);
  text-shadow: var(--glow-cyan);
  position: relative;
}

.hero h1 .highlight::before {
  content: 'MIT学者';
  position: absolute;
  left: 2px;
  top: 0;
  color: var(--alert-red);
  clip-path: inset(0 0 50% 0);
  animation: glitchTop 2s infinite linear alternate-reverse;
}

.hero h1 .highlight::after {
  content: 'MIT学者';
  position: absolute;
  left: -2px;
  top: 0;
  color: var(--plasma-purple);
  clip-path: inset(50% 0 0 0);
  animation: glitchBottom 3s infinite linear alternate-reverse;
}

@keyframes glitchTop {
  0% { transform: translateX(0); }
  20% { transform: translateX(-2px); }
  40% { transform: translateX(2px); }
  60% { transform: translateX(-1px); }
  80% { transform: translateX(1px); }
  100% { transform: translateX(0); }
}

@keyframes glitchBottom {
  0% { transform: translateX(0); }
  20% { transform: translateX(2px); }
  40% { transform: translateX(-2px); }
  60% { transform: translateX(1px); }
  80% { transform: translateX(-1px); }
  100% { transform: translateX(0); }
}
```

## 文章结构模板

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>文章标题 | CYBER://TAG</title>
  <!-- Google Fonts -->
  <style>
    /* CSS变量定义 */
    /* 网格背景 + 扫描线 */
    /* 各组件样式 */
  </style>
</head>
<body>
  <!-- 浮动装饰元素 -->
  <div class="float-element hex1"></div>
  <div class="float-element hex2"></div>
  <div class="float-element circle"></div>

  <!-- 英雄区 -->
  <header class="hero">
    <span class="overline">TAG_01 // TAG_02 // TAG_03</span>
    <div class="glitch-container">
      <h1>像<span class="highlight">关键词</span>一样<br>副标题</h1>
    </div>
    <p class="hero-subtitle">副标题描述<span>高亮词</span></p>
    <div class="scroll-hint">
      <span>INITIALIZE</span>
      <div class="arrow"></div>
    </div>
  </header>

  <article class="container">
    <!-- 引言 -->
    <section class="intro reveal">
      <p>引言内容...</p>
    </section>

    <!-- 引用块 -->
    <div class="quote-block reveal">
      <blockquote>核心引述内容</blockquote>
      <cite>// SOURCE_LABEL</cite>
    </div>

    <!-- 章节 -->
    <section class="reveal">
      <div class="section-header">
        <span class="section-label">PROTOCOL_01</span>
        <h2 class="section-title">章节标题</h2>
      </div>

      <div class="method">
        <span class="method-number">01</span>
        <div class="method-header">
          <div class="method-icon">
            <svg viewBox="0 0 24 24" stroke-width="2">
              <!-- SVG路径 -->
            </svg>
          </div>
          <h3>小标题 [ENGLISH_LABEL]</h3>
        </div>
        <p>正文内容...</p>

        <div class="highlight-box">
          <p>重点强调内容</p>
        </div>

        <p><strong>实践方法：</strong>行动建议...</p>
      </div>
    </section>

    <!-- 关键洞察 -->
    <div class="key-insight reveal">
      <span class="key-insight-label">// CORE_INSIGHT</span>
      <h3>核心洞察标题</h3>
      <p>补充说明</p>
    </div>

    <!-- 更多章节... -->
  </article>

  <!-- 总结 -->
  <section class="summary">
    <h2>// SYSTEM_SUMMARY: 总结标题</h2>
    <p><strong>要点一</strong> — 说明...</p>
    <p><strong>要点二</strong> — 说明...</p>
    <span class="takeaway">EXECUTE: ACTION_LABEL</span>
  </section>

  <!-- 页脚 -->
  <footer class="footer">
    <p><span>DATA_SOURCE:</span> 视频来源说明</p>
  </footer>

  <script>
    // 滚动渐显效果
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

## 组件样式详解

### 1. 方法卡片 (Method Card)

```css
.method {
  background: linear-gradient(135deg, var(--neural-gray), var(--cyber-dark));
  border: 1px solid rgba(0, 240, 255, 0.2);
  border-radius: 0;
  padding: 2.5rem;
  margin: 2.5rem 0;
  position: relative;
  overflow: hidden;
  transition: all 0.4s ease;
  /* 切角效果 */
  clip-path: polygon(0 0, calc(100% - 20px) 0, 100% 20px, 100% 100%, 20px 100%, 0 calc(100% - 20px));
}

.method:hover {
  border-color: var(--neon-cyan);
  box-shadow: var(--glow-cyan), inset 0 0 30px rgba(0, 240, 255, 0.05);
  transform: translateX(10px);
}

.method-number {
  position: absolute;
  top: -1rem;
  right: 1rem;
  font-family: 'Orbitron', monospace;
  font-size: 7rem;
  font-weight: 900;
  color: var(--neon-cyan);
  opacity: 0.08;
  line-height: 1;
  text-shadow: var(--glow-cyan);
}

.method-icon {
  width: 50px;
  height: 50px;
  background: linear-gradient(135deg, var(--plasma-purple), var(--neon-cyan));
  display: flex;
  align-items: center;
  justify-content: center;
  /* 六边形 */
  clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
}
```

### 2. 高亮框 (Highlight Box)

```css
.method .highlight-box {
  background: rgba(0, 255, 65, 0.05);
  border-left: 3px solid var(--matrix-green);
  padding: 1.25rem 1.5rem;
  margin: 1.5rem 0;
  position: relative;
}

.method .highlight-box::before {
  content: '>';
  position: absolute;
  top: -10px;
  left: -10px;
  font-family: 'Orbitron', monospace;
  font-size: 1.5rem;
  color: var(--matrix-green);
  text-shadow: var(--glow-green);
}
```

### 3. 引用块 (Quote Block)

```css
.quote-block {
  background: linear-gradient(135deg, var(--deep-space), var(--neural-gray));
  border: 1px solid var(--plasma-purple);
  padding: 3rem 2.5rem;
  margin: 4rem 0;
  position: relative;
  text-align: center;
  /* 切角 */
  clip-path: polygon(20px 0, 100% 0, 100% calc(100% - 20px), calc(100% - 20px) 100%, 0 100%, 0 20px);
}

.quote-block::before,
.quote-block::after {
  content: '';
  position: absolute;
  width: 40px;
  height: 40px;
  border: 2px solid var(--plasma-purple);
}

.quote-block::before {
  top: 10px;
  left: 10px;
  border-right: none;
  border-bottom: none;
}

.quote-block::after {
  bottom: 10px;
  right: 10px;
  border-left: none;
  border-top: none;
}

.quote-block blockquote {
  font-family: 'Orbitron', monospace;
  font-size: clamp(1rem, 3vw, 1.3rem);
  color: var(--plasma-purple);
  text-shadow: var(--glow-purple);
  font-style: normal;
}
```

### 4. 关键洞察 (Key Insight)

```css
.key-insight {
  background: linear-gradient(135deg, var(--cyber-dark), var(--neural-gray));
  border-top: 2px solid var(--alert-red);
  border-bottom: 2px solid var(--alert-red);
  color: white;
  padding: 4rem 2rem;
  margin: 5rem 0;
  text-align: center;
  position: relative;
  overflow: hidden;
}

.key-insight::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 2px;
  background: linear-gradient(90deg, transparent, var(--alert-red), var(--neon-cyan), var(--alert-red), transparent);
  animation: scanLine 3s linear infinite;
}

@keyframes scanLine {
  0% { transform: translateX(-100%); }
  100% { transform: translateX(100%); }
}
```

### 5. 浮动装饰元素

```css
.float-element {
  position: fixed;
  pointer-events: none;
  z-index: 0;
  opacity: 0.3;
}

.float-element.hex1 {
  top: 20%;
  left: 5%;
  width: 100px;
  height: 100px;
  border: 1px solid var(--neon-cyan);
  clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
  animation: float 6s ease-in-out infinite;
}

.float-element.hex2 {
  top: 60%;
  right: 8%;
  width: 60px;
  height: 60px;
  border: 1px solid var(--plasma-purple);
  clip-path: polygon(50% 0%, 100% 25%, 100% 75%, 50% 100%, 0% 75%, 0% 25%);
  animation: float 8s ease-in-out infinite reverse;
}

.float-element.circle {
  bottom: 30%;
  left: 10%;
  width: 80px;
  height: 80px;
  border: 1px dashed var(--matrix-green);
  border-radius: 50%;
  animation: rotate 20s linear infinite;
}

@keyframes float {
  0%, 100% { transform: translateY(0) rotate(0deg); }
  50% { transform: translateY(-20px) rotate(10deg); }
}

@keyframes rotate {
  from { transform: rotate(0deg); }
  to { transform: rotate(360deg); }
}
```

### 6. 滚动渐显效果

```css
.reveal {
  opacity: 0;
  transform: translateY(40px);
  transition: opacity 0.8s ease, transform 0.8s ease;
}

.reveal.visible {
  opacity: 1;
  transform: translateY(0);
}
```

## 文案风格指南

### 标签格式

使用全大写英文 + 下划线格式，增加科技感：

- `PROTOCOL_01` - 第一个协议
- `CORE_INSIGHT` - 核心洞察
- `SYSTEM_SUMMARY` - 系统总结
- `DATA_SOURCE` - 数据来源
- `INITIALIZE` - 初始化

### 章节命名

- 使用 `[中文] [ENGLISH_LABEL]` 格式
- 例：`MIT校训：手脑并用 [MENS ET MANUS]`

### 强调方式

- `<strong>` 用于实践建议开头
- `.highlight` 用于标题关键词
- `.highlight-box` 用于核心观点

## 适用场景

✅ 技术类文章
✅ 方法论/框架类文章
✅ 编程/科学主题
✅ 黑客文化/极客风格内容
✅ 未来主义/科幻主题

## 不适用场景

❌ 温馨情感类文章
❌ 传统文化主题
❌ 医疗健康类（颜色太过冷硬）
❌ 儿童教育类
