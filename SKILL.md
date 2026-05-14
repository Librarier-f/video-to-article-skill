---
name: video-to-article
description: 当用户粘贴YouTube/B站等视频链接并要求将视频整理为文章、整理成文章、把视频内容写成文章、或类似请求时，立即使用此技能。自动下载字幕、翻译（如需要）、并生成精美的HTML文章。支持简体中文和英文两种输出语言。轻量高效，只保留必要的文件。
---

## 核心改进（2024.05.09 更新）

### 1. Cookie 处理优化
- ✅ 自动生成标准 Netscape 格式的 cookies.txt
- ✅ 直接使用用户提供的 SESSDATA 值
- ✅ 保留 cookies.txt 用于后续使用
- ✅ 修复了路径和格式错误问题

### 2. 内容完整性增强
- ✅ 完整读取和处理 80% 以上的字幕内容
- ✅ 直接在内存中处理，不保存中间文件
- ✅ 不进行预截断，完整处理所有内容
- ✅ 增加内容完整性验证逻辑

### 3. 文件管理简化
- ✅ **只保留 cookies.txt**，其他临时文件直接删除
- ✅ 最终只生成一个 HTML 文章文件
- ✅ 不生成元数据、分析报告等中间文件
- ✅ 简化流程，提高处理速度

### 4. 工作流程优化
- ✅ 简化步骤，直接在内存中处理字幕
- ✅ 使用内置 HTML 模板，无需外部脚本
- ✅ 改善错误处理和调试信息
- ✅ 添加内容完整性检查清单

### 5. 高效生成策略
- ✅ 直接读取字幕到内存，不保存中间文件
- ✅ 基于模板直接生成 HTML，不使用 Python
- ✅ 快速预览和调整
- ✅ 清理临时文件，保持工作目录整洁

# 视频转文章技能

将视频的字幕内容转换为一篇结构清晰、排版精美的中文HTML文章。快速高效，只保留必要的输出文件。

## 快速安装

### 前置依赖

本 skill 依赖 **yt-dlp** 来下载视频字幕。**已内置于 skill 目录**，开箱即用：

```
video-to-article/
└── component/
    └── yt-dlp.exe    # Windows 版，已包含
```

> 如果内置版本无法使用，skill 会自动回退到系统 PATH 中的 yt-dlp。

#### macOS / Linux
如需手动安装：
```bash
# 方法一：使用 Homebrew（推荐）
brew install yt-dlp

# 方法二：使用 pip
pip3 install yt-dlp
```

#### Linux
如需手动安装：
```bash
# 方法一：使用 pip（推荐）
pip install yt-dlp

# 方法二：使用 pipx
pipx install yt-dlp

# 方法三：下载独立可执行文件
sudo curl -L https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o /usr/local/bin/yt-dlp
sudo chmod a+rx /usr/local/bin/yt-dlp
```

### 安装 Skill

将本 skill 文件夹复制到 Claude Code 的 skills 目录：

```bash
# 克隆仓库
git clone https://github.com/YOUR_USERNAME/video-to-article.git

# 复制到 Claude Code skills 目录
# Windows
cp -r video-to-article "$HOME/.claude/skills/"

# macOS / Linux
cp -r video-to-article ~/.claude/skills/
```

### 验证安装

```bash
# 检查 yt-dlp 是否安装成功
yt-dlp --version

# 检查 skill 是否加载成功
# 在 Claude Code 中输入 /video-to-article 应该能看到 skill 说明
```

## 配置说明

### yt-dlp 路径配置

skill 会按以下顺序检测 yt-dlp：

| 优先级 | 平台 | 路径 |
|--------|------|------|
| 1（内置） | Windows | `{skill目录}/component/yt-dlp.exe` |
| 2 | Windows | `C:/Program Files/yt-dlp/yt-dlp.exe` |
| 3 | macOS | `/usr/local/bin/yt-dlp` |
| 4 | macOS | `/opt/homebrew/bin/yt-dlp` |
| 5 | Linux | `/usr/local/bin/yt-dlp` |
| 6 | Linux | `/usr/bin/yt-dlp` |

### 输出目录配置

默认输出目录：当前工作目录

可在 skill 调用时指定其他目录。

## 文章风格选项（HTML文章）

如果用户选择 **HTML文章**，**必须使用 AskUserQuestion 询问用户希望使用哪种风格**。

### 可选风格列表

| 序号 | 风格名称 | Style Name | 描述 | 设计特点 |
|------|---------|-----------|------|---------|
| 1 | **蓝白商务** | Blue & White Business | 默认风格，专业简洁 | 蓝色渐变Hero、白色卡片、衬线标题、柔和阴影 |
| 2 | **Notion深色** | Notion Dark Mode | Notion深色模式 | 深色背景、衬线标题、紧凑排版、彩色标注系统 |
| 3 | **Claude风格** | Claude Style | Claude经典设计系统 | 赭石红+米白背景、衬线字体、噪点纹理 |
| 4 | **暗黑科技** | Cyber Tech | 赛博朋克/黑客帝国风格 | 深黑背景、霓虹光效、故障艺术、网格动画 |
| 5 | **极简现代** | Minimal Modern | 简约留白、优雅衬线 | 极致留白、衬线字体、深绿强调 |

### 风格选择流程

> **注意**：`AskUserQuestion` 工具最多支持4个选项。

1. 当用户提供视频链接并要求生成HTML文章时
2. **第一步**：使用 AskUserQuestion 询问风格（4个选项，**极简现代包含在内**）：
   ```
   询问内容：请问您希望使用哪种文章风格？
   选项：
     1. 蓝白商务（推荐）- 专业简洁，蓝色渐变Hero
     2. Notion深色 - 深色背景，彩色标注系统
     3. Claude风格 - 赭石红+米白，噪点纹理
     4. 极简现代 - 简约留白，深绿强调，中文数字
   ```
3. **第二步（确认前）**：用户选完风格后，生成文章之前，主动提示还有另一种风格可选：
   ```
   询问内容：您选择了「XXX」风格。顺带一提，还有「暗黑科技」风格可选——赛博朋克风格，霓虹光效，要不要换？
   选项：
     1. 不用了，就用「XXX」
     2. 换成暗黑科技
   ```
4. 用户确认后，按对应风格的设计规范生成文章
5. 如果用户未指定风格，默认使用**蓝白商务 (Blue & White Business)** 风格

### 风格规范文件

- **蓝白商务**：`SKILL.md` 内置规范 + `references/blue-white-business-style.md`
- **Notion深色**：`SKILL.md` 内置「Notion深色风格设计规范」章节
- **Claude风格**：`references/claude-style.md`
- **暗黑科技**：`references/cyber-tech-style.md`
- **极简现代**：`references/minimal-modern-style.md`

## ⚠️ 内容来源与版权说明（重要）

**在使用本技能生成的内容时，必须遵守以下来源标注要求：**

### 为什么需要来源标注？

本技能基于视频字幕整理内容，生成的文章、口播稿等属于**二次加工**而非原创内容。为了：
- 尊重原视频创作者的版权
- 避免平台判定为抄袭/搬运
- 构建健康的内容生态

### 各平台来源标注要求

#### 小红书文案
在正文开头或结尾添加：
```
📺 内容来源：视频标题
🔗 原视频链接：https://www.youtube.com/watch?v=xxx
📢 整理自 @频道名称
```

#### 公众号文章
在文章开头（标题下方）添加：
```markdown
---
来源说明：
本文整理自 YouTube 视频《视频标题》
原视频链接：https://www.youtube.com/watch?v=xxx
原频道：@频道名称
---
```

#### 口播稿/短视频配音
在口播稿开头添加来源说明，并在视频描述中注明：
```
【内容来源】
本视频内容整理自：《视频标题》
原视频：https://www.youtube.com/watch?v=xxx
原频道：@频道名称
```

### 平台原创标识注意事项

| 平台 | 要求 | 建议 |
|------|------|------|
| **小红书** | 建议标注来源 | 发布时在正文或评论区注明 |
| **公众号** | 建议标注来源 | 文章开头添加来源说明 |
| **口播稿/短视频** | 必须标注 | 视频描述区+口播稿开头都要有 |
| **微信公众号** | 尊重原创 | 如需商用，建议联系原作者获得授权 |

### 用户引导话术

在生成内容后，**必须**向用户提示：

> 「⚠️ 重要提示：生成的内容基于视频字幕整理，发布前请务必添加原始视频来源信息。建议注明「内容整理自@频道名」或附上原视频链接，尊重原创，合理使用。」

---

## 输出平台选择

在用户输入视频链接后，**必须首先询问用户希望将内容输出到哪个平台**。

### 可选平台

| 平台 | 主格式 | 说明 |
|------|--------|------|
| **HTML文章** | `.html` | 网页阅读，保留原有功能 |
| **小红书文案** | `.txt` | 纯文本，复制即用 |
| **公众号文章** | `.md` | Markdown格式，支持排版工具 |
| **口播稿** | `.txt` | 短视频配音稿，可直接使用 |

### 完整交互流程

```
用户输入视频链接
        ↓
【步骤1】询问输出平台：
  1. HTML文章
  2. 小红书文案
  3. 公众号文章
  4. 口播稿
        ↓
【步骤2】根据平台询问具体选项：
  ├─ 选择HTML文章时 → 询问文章风格
  │   └─ 风格选择：蓝白商务/Notion深色/Claude风格/极简现代 → (是否换暗黑科技)
  ├─ 选择小红书文案时 → 询问文体类型
  │   └─ 文体：种草文/干货文/情感文/清单文
  ├─ 选择公众号文章时 → 询问文体类型
  │   └─ 文体：热点解读/深度长文/金句提炼/故事叙述
  └─ 选择口播稿时 → 询问文体类型
      └─ 文体：爆款开场/故事叙述/清单列表/情绪共鸣 → (是否选对话挑战)
        ↓
【步骤3】询问输出语言：
  1. 简体中文
  2. English
        ↓
生成对应格式的内容
```

### 小红书文体选项

| 文体类型 | 适用场景 | 模板文件 |
|---------|---------|---------|
| **种草安利文** | 产品推荐、好物分享 | `references/xiaohongshu/zhongcao.md` |
| **干货教程文** | 技能分享、方法教程 | `references/xiaohongshu/ganhuo.md` |
| **情感共鸣文** | 情感故事、人生感悟 | `references/xiaohongshu/qinggan.md` |
| **清单盘点文** | 资源整理、清单合集 | `references/xiaohongshu/qingdan.md` |

### 公众号文体选项

| 文体类型 | 适用场景 | 模板文件 |
|---------|---------|---------|
| **热点解读文** | 社会热点、行业趋势 | `references/wechat/redian.md` |
| **深度长文** | 知识科普、深度分析 | `references/wechat/shendu.md` |
| **金句提炼文** | 内容精华、语录整理 | `references/wechat/jinju.md` |
| **故事叙述文** | 案例分享、经历讲述 | `references/wechat/gushi.md` |

### 口播稿文体选项

| 文体类型 | 适用场景 | 模板文件 |
|---------|---------|---------|
| **爆款开场型** | 知识干货、认知颠覆 | `references/koupogao/baokuan.md` |
| **故事叙述型** | 个人经历、经验分享 | `references/koupogao/gushi.md` |
| **清单列表型** | 技巧盘点、方法汇总 | `references/koupogao/qingdan.md` |
| **对话挑战型** | 观点表达、争议话题 | `references/koupogao/duihua.md` |
| **情绪共鸣型** | 情感话题、人生感悟 | `references/koupogao/qingxu.md` |

### 双格式输出规范

| 平台 | 主格式 | 辅助格式 | 用途 |
|------|--------|---------|------|
| **小红书** | `.txt` | `.html` 预览 | 直接复制发布 / 本地预览 |
| **公众号** | `.md` | `.html` 预览 | 粘贴到编辑器 / 本地预览 |
| **口播稿** | `.txt` | `.html` 预览 | 直接配音使用 / 本地预览 |
| **HTML文章** | `.html` | - | 网页阅读 |

### 文件命名规范

```
{平台}-{文体类型}-{视频标题简称}.{扩展名}

示例：
├── HTML-蓝白商务-6个吸引力法则.html
├── 小红书-种草文-6个吸引力法则.txt
├── 小红书-种草文-6个吸引力法则-预览.html
├── 公众号-热点解读-6个吸引力法则.md
├── 公众号-热点解读-6个吸引力法则-预览.html
├── 口播稿-爆款开场-起床后工作1分钟.txt
└── 口播稿-爆款开场-起床后工作1分钟-预览.html
```

## Notion 深色风格设计规范

> 版本 1.0 · 基于Notion 2024深色模式实际观测值

### 色彩系统（Color System）

#### 基础背景层

| 层级 | 用途 | HEX | CSS 变量 |
|------|------|-----|----------|
| 底层背景 | 页面最外层 | `#191919` | `--bg-base` |
| 内容背景 | 文章主体卡片 | `#202020` | `--bg-surface` |
| 悬浮/弹出 | Tooltip、菜单 | `#2f2f2f` | `--bg-elevated` |
| 边框 | 分割线、表格线 | `#2e2e2e` | `--border` |
| 悬停高亮 | hover 状态 | `#ffffff0f` | `--hover` |

#### 文字色阶

| 层级 | 用途 | HEX | CSS 变量 |
|------|------|-----|----------|
| 主文字 | 正文、标题 | `#e3e3e3` | `--text-primary` |
| 次级文字 | 描述、副标题 | `#9b9b9b` | `--text-secondary` |
| 占位/禁用 | placeholder | `#555555` | `--text-muted` |

#### 强调色（彩色标注系统）

| 颜色 | 文字色 | 背景色 |
|------|--------|--------|
| 蓝 | `#4a7ab5` | `#0e1f3b` |
| 绿 | `#518c5a` | `#122215` |
| 橙 | `#c07a3d` | `#2e1e0f` |
| 黄 | `#b8a030` | `#292306` |
| 粉 | `#b05c83` | `#2e1020` |

### 字体系统（Typography）

#### 字体族

```css
/* 标题：衬线体 */
--font-serif: 'Noto Serif SC', Georgia, 'Times New Roman', serif;

/* 正文：无衬线体 */
--font-sans: ui-sans-serif, -apple-system, BlinkMacSystemFont, "Segoe UI", Helvetica, Arial, sans-serif;

/* 代码 */
--font-mono: "SFMono-Regular", Menlo, Consolas, "Liberation Mono", Courier, monospace;
```

#### 字号与行高规范

| 元素 | 字号 | 行高 | 字重 |
|------|------|------|------|
| H1 | `2em` (约40px) | `1.2` | `700` |
| H2 | `1.5em` (约30px) | `1.3` | `600` |
| H3 | `1.25em` (约25px) | `1.3` | `600` |
| 正文（Body） | `16px` | `1.75` | `400` |
| 小字注释 | `14px` | `1.6` | `400` |

### 间距系统（Spacing）

```css
:root {
  --space-xs:  2px;
  --space-sm:  4px;
  --space-md:  8px;
  --space-lg: 16px;
  --space-xl: 24px;
  --space-2xl: 40px;
}
```

### 圆角系统

```css
:root {
  --radius-sm: 3px;   /* 内联元素 */
  --radius-md: 4px;   /* 按钮、输入框 */
  --radius-lg: 8px;   /* 卡片、容器 */
}
```

### 内容宽度

```css
.page-content {
  max-width: 708px;       /* Notion标准宽度 */
  margin: 0 auto;
  padding: 0 96px;        /* 两侧留白 */
}

@media (max-width: 768px) {
  .page-content { padding: 0 24px; }
}
```

### 设计原则

| 原则 | 说明 |
|------|------|
| **极简留白** | 内容即主角，装饰趋近于零 |
| **紧凑节奏** | 段落间距极小（1–4px），块与块之间呼吸感来自内容结构而非空白 |
| **中性色调** | 全站灰色系，彩色只服务于内容语义标注 |
| **无品牌强调** | 无蓝色链接、无彩色按钮，界面退场让文字前进 |
| **字重即层级** | 用字重（400/600/700）而非字色来区分标题层级 |
| **交互极克制** | hover仅加轻微背景，无阴影、无缩放、无弹跳 |

### Notion深色风格 HTML 模板

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>文章标题</title>
  <link href="https://fonts.googleapis.com/css2?family=Noto+Serif+SC:wght@400;600;700&display=swap" rel="stylesheet">
  <style>
    :root {
      /* 背景 */
      --bg-base:      #191919;
      --bg-surface:   #202020;
      --bg-elevated:  #2f2f2f;
      --border:       #2e2e2e;
      --hover:        rgba(255, 255, 255, 0.055);

      /* 文字 */
      --text-primary:   #e3e3e3;
      --text-secondary: #9b9b9b;
      --text-muted:     #555555;

      /* 强调色 - Notion 标注系统 */
      --accent-blue-text:    #4a7ab5;
      --accent-blue-bg:      #0e1f3b;
      --accent-green-text:   #518c5a;
      --accent-green-bg:     #122215;
      --accent-orange-text:  #c07a3d;
      --accent-orange-bg:    #2e1e0f;
      --accent-yellow-text:  #b8a030;
      --accent-yellow-bg:    #292306;

      /* 字体 */
      --font-serif: 'Noto Serif SC', Georgia, serif;
      --font-sans: ui-sans-serif, -apple-system, system-ui, sans-serif;

      /* 间距 */
      --space-xs:  2px;
      --space-sm:  4px;
      --space-md:  8px;
      --space-lg: 16px;
      --space-xl: 24px;
      --space-2xl: 40px;

      /* 圆角 */
      --radius-sm: 3px;
      --radius-md: 4px;
      --radius-lg: 8px;
    }

    * { margin: 0; padding: 0; box-sizing: border-box; }

    body {
      font-family: var(--font-sans);
      background: var(--bg-base);
      color: var(--text-primary);
      line-height: 1.75;
      font-size: 16px;
    }

    .page {
      max-width: 708px;
      margin: 0 auto;
      padding: 0 96px;
    }

    @media (max-width: 768px) {
      .page { padding: 0 24px; }
    }

    h1 {
      font-family: var(--font-serif);
      font-size: 2em;
      font-weight: 700;
      line-height: 1.2;
      color: var(--text-primary);
      margin-top: 80px;
      margin-bottom: var(--space-sm);
    }

    h2 {
      font-family: var(--font-serif);
      font-size: 1.5em;
      font-weight: 600;
      line-height: 1.3;
      color: var(--text-primary);
      margin-top: 1.6em;
      margin-bottom: var(--space-sm);
    }

    p {
      font-size: 16px;
      line-height: 1.75;
      color: var(--text-primary);
      margin: 1px 0;
    }

    a {
      color: inherit;
      text-decoration: underline;
      text-decoration-color: var(--text-secondary);
      text-underline-offset: 2px;
    }

    a:hover {
      text-decoration-color: var(--text-primary);
    }

    blockquote {
      border-left: 3px solid var(--text-primary);
      margin: var(--space-md) 0;
      padding: var(--space-sm) var(--space-lg);
      font-style: normal;
    }

    /* 标注块 (Callout) */
    .callout {
      display: flex;
      align-items: flex-start;
      gap: var(--space-md);
      border-radius: var(--radius-md);
      padding: var(--space-lg);
      margin: var(--space-md) 0;
    }

    .callout-blue {
      background: var(--accent-blue-bg);
    }
    .callout-green {
      background: var(--accent-green-bg);
    }
    .callout-orange {
      background: var(--accent-orange-bg);
    }
    .callout-yellow {
      background: var(--accent-yellow-bg);
    }

    .callout-icon {
      font-size: 18px;
      line-height: 1;
      flex-shrink: 0;
    }

    .callout-content {
      flex: 1;
    }
  </style>
</head>
<body>
  <div class="page">
    <span class="tag">分类标签</span>
    <h1>文章标题</h1>
    <p>正文内容...</p>

    <blockquote>
      <p>引用内容</p>
    </blockquote>

    <hr>

    <h2>章节标题</h2>
    <p>章节内容...</p>

    <div class="callout callout-blue">
      <span class="callout-icon">💡</span>
      <div class="callout-content">
        <p>重点提示内容</p>
      </div>
    </div>
  </div>
</body>
</html>
      flex-shrink: 0;
    }

    hr {
      border: none;
      border-top: 1px solid var(--border);
      margin: var(--space-lg) 0;
    }

    /* 标签 */
    .tag {
      display: inline-block;
      background: var(--accent-blue-bg);
      color: var(--accent-blue-text);
      padding: 2px 8px;
      border-radius: var(--radius-sm);
      font-size: 12px;
      font-weight: 500;
      margin-bottom: var(--space-md);
    }
  </style>
</head>
<body>
  <div class="page">
    <span class="tag">分类标签</span>
    <h1>文章标题</h1>
    <p>正文内容...</p>

    <blockquote>
      <p>引用内容</p>
    </blockquote>

    <hr>

    <h2>章节标题</h2>
    <p>章节内容...</p>

    <div class="callout callout-blue">
      <span class="callout-icon">💡</span>
      <div class="callout-content">
        <p>重点提示内容</p>
      </div>
    </div>
  </div>
</body>
</html>
```

## 工作流程（更新版）

### 交互式选择要求

**在生成文章之前，必须使用 AskUserQuestion 工具依次让用户选择以下选项：**

1. **输出平台**：HTML文章 / 小红书文案 / 公众号文章
2. **具体文体**：根据平台显示对应选项
3. **输出语言**：简体中文 / English

---

### 第零步：确认输出平台

**使用 AskUserQuestion 询问用户：**

```
询问内容：请问您希望将内容输出到哪个平台？
选项：
  1. HTML文章 - 网页阅读，保留原有功能
  2. 小红书文案 - 纯文本，复制即用
  3. 公众号文章 - Markdown格式，支持排版工具
  4. 口播稿 - 短视频配音稿，可直接使用
```

### 第零点五步：确认具体文体

根据平台选择，使用 AskUserQuestion 询问对应的具体文体：

**HTML文章：**
```
询问内容：请问您希望使用哪种文章风格？
选项：
  1. 蓝白商务（推荐）- 专业简洁，蓝色渐变Hero
  2. Notion深色 - 深色背景，彩色标注系统
  3. Claude风格 - 赭石红+米白，噪点纹理
  4. 暗黑科技 - 赛博朋克风格，霓虹光效
  5. 极简现代 - 简约留白，深绿强调
```

**小红书文案：**
```
询问内容：请问您希望使用哪种小红书文体？
选项：
  1. 种草安利文 - 产品推荐、好物分享
  2. 干货教程文 - 技能分享、方法教程
  3. 情感共鸣文 - 情感故事、人生感悟
  4. 清单盘点文 - 资源整理、清单合集
```

**公众号文章：**
```
询问内容：请问您希望使用哪种公众号文体？
选项：
  1. 热点解读文 - 社会热点、行业趋势
  2. 深度长文 - 知识科普、深度分析
  3. 金句提炼文 - 内容精华、语录整理
  4. 故事叙述文 - 案例分享、经历讲述
```

**口播稿：**
> **注意**：`AskUserQuestion` 工具最多支持4个选项，口播稿共5种文体，分两步进行。

第一步（4个选项）：
```
询问内容：请问您希望使用哪种口播稿文体？
选项：
  1. 爆款开场型 - 知识干货、认知颠覆
  2. 故事叙述型 - 个人经历、经验分享
  3. 清单列表型 - 技巧盘点、方法汇总
  4. 情绪共鸣型 - 情感话题、人生感悟
```

第二步（如果用户需要第5种可选风格）：
```
询问内容：还有一种「对话挑战型」风格——适合观点表达、争议话题，要不要看看？
选项：
  1. 不用了，就用前面的风格
  2. 对话挑战型 - 观点表达、争议话题
```

### 第一步：确认输出语言

使用 AskUserQuestion 询问输出语言：

```
询问内容：请问您希望文章使用哪种语言输出？
选项：
  1. 简体中文（推荐）- 翻译字幕后整理，适合中文读者
  2. English - 保留英文内容，适合国际读者
```

#### 语言相关处理

- **简体中文**：如果字幕是英文，需要翻译成中文后整理
- **English**：如果字幕是中文，需要翻译成英文后整理；如果字幕是英文，直接整理

### 第二步：获取视频信息（简化版）

直接使用 yt-dlp 获取基本信息，不保存到文件：

```bash
# 获取视频信息（输出到终端）
yt-dlp --print "%(title)s" --print "%(uploader)s" --print "%(upload_date)s" --print "%(duration)s" --print "%(thumbnail)s" --print "%(webpage_url)s" --skip-download "视频链接"
```

**从链接识别平台：**

| 平台 | 链接特征 | 图标 |
|------|---------|------|
| YouTube | `youtube.com` / `youtu.be` | 📺 |
| Bilibili | `bilibili.com` / `b23.tv` | 📱 |
| Twitter/X | `twitter.com` / `x.com` | 🐦 |
| Vimeo | `vimeo.com` | 🎬 |

### 第三步：下载字幕（简化高效版）

使用 yt-dlp 下载字幕，只保留必要的 cookie 文件：

**Cookie 处理（保留 cookies.txt）：**

如果用户提供了 SESSDATA（对于 Bilibili），我们会自动创建标准格式的 cookies.txt 文件（**这个文件会保留**）：

```bash
# 如果用户提供了 SESSDATA，自动创建 cookie 文件
if [ -n "$USER_SESSDATA" ]; then
  # 自动生成标准格式的 cookie 文件（保留）
  cat > cookies.txt << EOF
# Netscape HTTP Cookie File
# This file was generated by video-to-article skill
# Cookie for Bilibili AI subtitle access
.bilibili.com	TRUE	/	TRUE	1791183873	SESSDATA	${USER_SESSDATA}
EOF
  COOKIE_OPTION="--cookies cookies.txt"
  echo "✅ Cookie 文件已创建: cookies.txt（已保留）"
else
  COOKIE_OPTION=""
fi
```

**下载字幕（优先使用手动字幕，没有则用自动字幕）：**
```bash
# 下载字幕 - 只下载需要的字幕文件
yt-dlp --write-subs --write-auto-subs --sub-lang "zh-Hans,zh,en" --skip-download $COOKIE_OPTION -o "%(title)s.%(ext)s" "视频链接"
```

**找到字幕文件：**
```bash
# 找到下载的字幕文件（优先顺序：.zh-Hans.srt → .ai-zh.srt → .srt → .vtt）
SUBTITLE_FILE=""
if ls *.zh-Hans.srt 2>/dev/null; then
  SUBTITLE_FILE=$(ls -1t *.zh-Hans.srt | head -1)
elif ls *.ai-zh.srt 2>/dev/null; then
  SUBTITLE_FILE=$(ls -1t *.ai-zh.srt | head -1)
elif ls *.srt 2>/dev/null; then
  SUBTITLE_FILE=$(ls -1t *.srt | head -1)
elif ls *.vtt 2>/dev/null; then
  SUBTITLE_FILE=$(ls -1t *.vtt | head -1)
fi
```

**关键改进说明：**
- 自动生成标准 Netscape 格式的 cookie 文件
- 同时尝试下载手动和自动字幕，确保获取最完整的内容
- 保留所有下载的字幕文件用于后续分析
- 记录字幕文件列表和行数

#### 支持的视频平台

| 平台 | 字幕支持 | 备注 |
|------|---------|------|
| YouTube | ✅ 手动+自动 | 最佳支持 |
| Bilibili | ✅ CC字幕 + AI字幕 | 有CC字幕直接下载；无字幕需提供SESSDATA获取AI字幕 |
| Twitter/X | ✅ 内嵌字幕 | 部分 video 支持 |
| Vimeo | ✅ 手动字幕 | 依赖上传者添加 |
| 其他 | ⚠️ 有限支持 | 取决于平台 |

### 第四步：处理字幕并生成HTML（直接内存处理）

**直接在内存中读取和处理字幕：**

```bash
# 直接读取字幕内容到变量，不保存中间文件
FULL_SUBTITLE_CONTENT=$(cat "$SUBTITLE_FILE")
CONTENT_LENGTH=$(echo "$FULL_SUBTITLE_CONTENT" | wc -c)
LINE_COUNT=$(echo "$FULL_SUBTITLE_CONTENT" | wc -l)

echo "📊 字幕文件统计:"
echo "  - 文件大小: ${CONTENT_LENGTH} 字节"
echo "  - 总行数: ${LINE_COUNT} 行"

# 简单的字幕清洗 - 去除时间戳标签但保留所有文本内容
CLEAN_CONTENT=$(echo "$FULL_SUBTITLE_CONTENT" | \
  sed '/^[0-9]\+$/d' | \
  sed '/-->.*$/d' | \
  sed '/^$/d' | \
  sed 's/<[^>]*>//g')

# 内容预览（前100行和后50行）
echo "📝 字幕内容预览（前100行）:"
echo "═══════════════════════════════════════════════════"
echo "$CLEAN_CONTENT" | head -100
echo "═══════════════════════════════════════════════════"
echo "📝 字幕内容预览（后50行）:"
echo "═══════════════════════════════════════════════════"
echo "$CLEAN_CONTENT" | tail -50
echo "═══════════════════════════════════════════════════"
```

**基于内置HTML模板直接生成文章：**

我们使用内置的蓝白商务风格HTML模板，直接替换内容。**不使用Python脚本**，直接使用bash字符串操作：

```bash
# 获取视频标题（从文件名中提取）
VIDEO_TITLE=$(yt-dlp --print "%(title)s" --skip-download "视频链接")

# 生成HTML文件名
OUTPUT_FILE="${VIDEO_TITLE}_蓝白商务.html"

# 使用内置模板直接生成HTML
cat > "$OUTPUT_FILE" << 'EOF'
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>文章标题</title>
  <link href="https://fonts.googleapis.com/css2?family=Noto+Serif+SC:wght@400;500;600;700&family=Noto+Sans+SC:wght@300;400;500;600&display=swap" rel="stylesheet">
  <style>
    /* 内置蓝白商务风格CSS */
    :root {
      --primary: #0369A1;
      --primary-light: #0EA5E9;
      --primary-dark: #0C4A6E;
      --accent: #22C55E;
      --accent-gold: #FCD34D;
      --background: #F8FAFC;
      --surface: #FFFFFF;
      --surface-alt: #F1F5F9;
      --text-primary: #0F172A;
      --text-secondary: #475569;
      --text-muted: #94A3B8;
      --border: #E2E8F0;
    }
    /* 简化版样式... */
  </style>
</head>
<body>
  <header class="hero">
    <div class="container">
      <span class="hero-tag">分类标签</span>
      <h1>文章标题</h1>
      <p class="hero-subtitle">副标题</p>
    </div>
  </header>

  <article class="container">
    <!-- 内容区域 -->
    <div class="content-card">
      <p>这里将填充字幕内容...</p>
    </div>
  </article>

  <footer class="footer">
    <p>内容基于视频字幕翻译整理 · 观看原视频</p>
    <p>© 2026 视频转文章技能</p>
  </footer>

  <script>
    // 简化版动画...
  </script>
</body>
</html>
EOF

# 替换标题和内容
sed -i "s/文章标题/$VIDEO_TITLE/g" "$OUTPUT_FILE"
sed -i "s/这里将填充字幕内容.../$CLEAN_CONTENT/g" "$OUTPUT_FILE"
echo "✅ HTML文章已生成: $OUTPUT_FILE"

# 清理临时文件（除了cookies.txt）
rm -f "$SUBTITLE_FILE"
rm -f "subtitle_analysis.json"
rm -f "metadata.json"
rm -f "subtitle_original_*.srt"
echo "🧹 临时文件已清理"
```

### 第五步：直接生成精美HTML（使用内置模板）

**内容完整性要求（重要）：**
- ✅ 使用 80% 以上的字幕内容，不进行过度删减
- ✅ 保留主要的对话、观点和信息
- ✅ 完整呈现视频的各个章节和主题
- ✅ 确保文章长度与视频时长成比例

**使用内置HTML模板生成：**

我们将直接使用内置的蓝白商务风格HTML模板，通过 Claude 的自然语言处理能力来组织内容：

1. **读取完整字幕内容**（不截断，完整处理）
2. **识别主要章节和主题**（5-10个主要部分）
3. **直接生成完整HTML文章**（使用内置样式）

**重要：只保留必要文件！**
- ✅ 保留：cookies.txt（方便下次使用）
- ✅ 保留：最终的HTML文章文件
- ❌ 删除：所有临时字幕文件、元数据等

**清理临时文件：**
```bash
# 清理临时文件（除了cookies.txt）
rm -f *.srt
rm -f *.vtt
rm -f metadata.json
rm -f subtitle_*.txt
rm -f subtitle_*.json
echo "🧹 临时文件已清理，只保留 cookies.txt"
```

**用户提示话术：**
> 「✅ 视频内容已完整转换为HTML文章！
> 
> 📄 已保存文件：
> - [视频标题]_蓝白商务.html
> - cookies.txt（已保留，下次可直接使用）
> 
> ⚠️ 发布前请务必添加原始视频来源信息。建议注明「内容整理自@频道名」或附上原视频链接，尊重原创，合理使用。」

## HTML 文章设计规范

### 视频来源信息卡片

在文章开头展示视频来源信息，增强文章的可信度和来源可追溯性：

```html
<!-- 视频来源卡片 -->
<div class="video-source-card">
  <div class="source-thumbnail-wrapper">
    <img class="source-thumbnail" src="视频封面URL" alt="视频封面">
    <span class="source-duration">45:32</span>
  </div>
  <div class="source-info">
    <div class="source-meta-row">
      <span class="source-platform">📺 YouTube</span>
      <span class="source-author">作者名称</span>
    </div>
    <div class="source-meta-row">
      <span class="source-date">📅 2024年1月15日</span>
    </div>
  </div>
  <a class="source-link" href="原始链接" target="_blank" rel="noopener">
    <svg width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
      <path d="M18 13v6a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V8a2 2 0 0 1 2-2h6"></path>
      <polyline points="15 3 21 3 21 9"></polyline>
      <line x1="10" y1="14" x2="21" y2="3"></line>
    </svg>
    观看原视频
  </a>
</div>
```

**CSS 样式（蓝白商务风格）：**

```css
.video-source-card {
  display: flex;
  align-items: center;
  gap: 1rem;
  background: linear-gradient(135deg, #f8fafc, #ffffff);
  border: 1px solid #e2e8f0;
  border-radius: 12px;
  padding: 1rem 1.25rem;
  margin-bottom: 2rem;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.04);
}

.source-thumbnail-wrapper {
  position: relative;
  flex-shrink: 0;
}

.source-thumbnail {
  width: 120px;
  height: 68px;
  object-fit: cover;
  border-radius: 8px;
}

.source-duration {
  position: absolute;
  bottom: 4px;
  right: 4px;
  background: rgba(0, 0, 0, 0.8);
  color: white;
  font-size: 11px;
  padding: 2px 6px;
  border-radius: 4px;
  font-weight: 500;
}

.source-info {
  flex: 1;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
}

.source-meta-row {
  display: flex;
  align-items: center;
  gap: 1rem;
  font-size: 14px;
}

.source-platform {
  display: inline-flex;
  align-items: center;
  gap: 0.25rem;
  background: #e0f2fe;
  color: #0369a1;
  padding: 2px 8px;
  border-radius: 4px;
  font-weight: 500;
  font-size: 12px;
}

.source-author {
  color: #475569;
}

.source-date {
  color: #64748b;
  font-size: 13px;
}

.source-link {
  display: inline-flex;
  align-items: center;
  gap: 0.5rem;
  background: #0369a1;
  color: white;
  text-decoration: none;
  padding: 0.5rem 1rem;
  border-radius: 8px;
  font-size: 14px;
  font-weight: 500;
  transition: all 0.2s ease;
}

.source-link:hover {
  background: #0284c7;
  transform: translateY(-1px);
}

@media (max-width: 640px) {
  .video-source-card {
    flex-direction: column;
    text-align: center;
  }
  .source-thumbnail {
    width: 100%;
    height: auto;
    aspect-ratio: 16/9;
  }
}
```

**暗黑科技风格适配：**

```css
/* 暗黑科技风格下的视频来源卡片 */
.video-source-card {
  background: linear-gradient(135deg, var(--neural-gray), var(--cyber-dark));
  border: 1px solid rgba(0, 240, 255, 0.2);
  border-radius: 0;
  clip-path: polygon(0 0, calc(100% - 12px) 0, 100% 12px, 100% 100%, 12px 100%, 0 calc(100% - 12px));
}

.source-platform {
  background: rgba(0, 240, 255, 0.1);
  color: var(--neon-cyan);
}

.source-link {
  background: linear-gradient(135deg, var(--plasma-purple), var(--neon-cyan));
}
```

### 字体选择
- 中文衬线标题：`Noto Serif SC`（Google Fonts）
- 中文正文：`Noto Sans SC`
- 备用字体：系统默认宋体/黑体

```html
<link href="https://fonts.googleapis.com/css2?family=Noto+Serif+SC:wght@400;500;600;700&family=Noto+Sans+SC:wght@300;400;500;600&display=swap" rel="stylesheet">
```

### 配色方案（蓝白商务风格）
```css
:root {
  /* 主色调 */
  --primary: #0369A1;        /* 专业蓝 */
  --primary-light: #0EA5E9;  /* 天蓝 */
  --primary-dark: #0C4A6E;   /* 深蓝 */

  /* 强调色 */
  --accent: #22C55E;         /* 成功绿 */

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

### 文章结构模板

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>文章标题</title>
  <link href="Google Fonts链接" rel="stylesheet">
  <style>
    :root {
      --primary: #0369A1;
      --primary-light: #0EA5E9;
      --primary-dark: #0C4A6E;
      --accent: #22C55E;
      --background: #F8FAFC;
      --surface: #FFFFFF;
      --text-primary: #0F172A;
      --text-secondary: #475569;
    }
    /* 基础样式... */
  </style>
</head>
<body>
  <header class="hero">
    <span class="hero-tag">分类标签</span>
    <h1>文章标题</h1>
    <p class="hero-subtitle">副标题</p>
  </header>

  <article class="container">
    <!-- 引言 -->
    <section class="intro">
      <p>首段内容...</p>
    </section>

    <!-- 引用块 -->
    <div class="quote-block">
      <blockquote>核心引述</blockquote>
      <cite>— 来源</cite>
    </div>

    <!-- 章节内容 -->
    <section>
      <div class="section-header">
        <span class="section-number">1</span>
        <span class="section-title">章节标题</span>
      </div>
      <div class="content-card">
        <p>正文内容...</p>
        <div class="highlight-box">
          <p>重点强调</p>
        </div>
        <div class="practice-tips">
          <strong>实践方法：</strong>行动建议...
        </div>
      </div>
    </section>
  </article>

  <!-- 总结 -->
  <section class="summary">
    <h2>总结标题</h2>
    <div class="summary-item">
      <span class="summary-item-number">1</span>
      <div class="summary-item-content">
        <div class="summary-item-title">要点标题</div>
        <div class="summary-item-desc">要点描述</div>
      </div>
    </div>
  </section>

  <footer class="footer">
    <p>内容基于视频字幕翻译整理</p>
  </footer>
</body>
</html>
```

## 输出要求（增强内容完整性版）

### 文件输出规范（改进版）

根据用户选择的平台，生成对应的文件，并保留所有处理过程文件：

| 平台 | 主格式 | 文件扩展名 | 辅助文件 | 说明 |
|------|--------|-----------|---------|------|
| **HTML文章** | HTML | `.html` | metadata.json, *.srt, *.vtt | 网页阅读 |
| **小红书** | 纯文本 | `.txt` | subtitle_clean.txt, subtitle_analysis.json | 复制即用 / 本地预览 |
| **公众号** | Markdown | `.md` | subtitle_analysis.json, subtitle_clean.txt | 粘贴到编辑器 / 本地预览 |
| **口播稿** | 纯文本 | `.txt` | subtitle_clean.txt, metadata.json | 直接配音 / 本地预览 |

### 文件命名规范（增强）

```
{平台}-{文体类型}-{视频标题简称}.{扩展名}

示例（完整文件集合）：
├── 【十字路口】探秘ClaudeCode_蓝白商务.html      # 最终HTML文件
├── 【十字路口】探秘ClaudeCode_蓝白商务-预览.html
├── metadata.json                               # 视频元数据
├── subtitle_clean.txt                          # 清洗后的字幕内容
├── subtitle_analysis.json                      # 内容分析报告
├── cookies.txt                                # 会话cookie
├── 【十字路口】探秘 Claude Code，搞懂 Agent Harness｜对谈来新璐【视频播客】.ai-zh.srt  # 原始字幕
├── file_inventory.txt                          # 文件清单
└── subtitle_list.txt                           # 字幕文件列表
```

### 内容完整性要求（重要更新）

**内容完整性标准（严格要求）：**

- ✅ 使用 **80% 以上** 的字幕内容，不进行过度删减
- ✅ 保留 **所有主要的对话、观点和信息点**
- ✅ 完整呈现 **视频的各个章节和主题**
- ✅ 确保文章长度与视频时长成比例
- ✅ 对于长对话和讨论，进行合理的分段和整理
- ✅ 保留关键术语、专业名词和引用内容

**内容缺失的识别和处理：**

```bash
# 内容完整性检查脚本
CONTENT_COVERAGE_THRESHOLD=0.8  # 80% 覆盖率要求

# 计算内容使用比例
total_lines=$(cat subtitle_clean.txt | wc -l)
article_lines=$(grep -o "获取视频" 【十字路口】探秘ClaudeCode_蓝白商务.html 2>/dev/null || echo 0)

# 如果无法直接获取行数，进行估算
if [ "$article_lines" -lt 10 ]; then
  echo "⚠️ 无法准确计算内容使用比例，请人工检查"
  # 显示文章和字幕的字符统计
  article_chars=$(cat 【十字路口】探秘ClaudeCode_蓝白商务.html | grep -o "<p>" | wc -l)
  subtitle_chars=$(cat subtitle_clean.txt | wc -c)
  echo "文章段落数: $article_chars"
  echo "字幕字符数: $subtitle_chars"
  
  if [ "$subtitle_chars" -lt 5000 ]; then
    echo "⚠️ 字幕内容过短，可能需要检查下载是否完整"
  fi
else
  usage_ratio=$(awk "BEGIN {print $article_lines / $total_lines}")
  echo "内容使用比例: $(awk "BEGIN {printf \"%.1f\", $usage_ratio * 100}")%"
  
  if (( $(awk "BEGIN {if ($usage_ratio < $CONTENT_COVERAGE_THRESHOLD) print 1; else print 0}") )); then
    echo "❌ 内容使用比例不足 80%，建议重新生成"
    echo "建议：检查是否有截断或重要内容缺失"
  else
    echo "✅ 内容使用比例符合要求"
  fi
fi
```

### 排版规范（改进版）

- **HTML文章**：精美的前端设计，包含动画效果（滚动渐显、悬停效果），底部包含来源说明
- **小红书文案**：纯文本，多换行，带 emoji 和 #标签，正文开头包含来源标注
- **公众号文章**：Markdown 格式，多小标题，文末带关注引导，标题下方包含来源说明
- **口播稿**：纯文本，短句为主，带节奏停顿标记，可直接配音，开头包含来源说明

### 内容生成策略（改进）

#### 1. 完整阅读策略
```python
def read_and_analyze_full_content(full_text):
    """
    完整阅读和分析所有字幕内容
    """
    paragraphs = []
    current_paragraph = []
    
    for line in full_text.split('\n'):
        if line.strip():
            current_paragraph.append(line.strip())
        else:
            if current_paragraph:
                paragraphs.append(' '.join(current_paragraph))
                current_paragraph = []
    
    if current_paragraph:
        paragraphs.append(' '.join(current_paragraph))
    
    print(f"识别到段落数: {len(paragraphs)}")
    print(f"平均段落长度: {sum(len(p) for p in paragraphs)/len(paragraphs)} 字符")
    
    return paragraphs
```

#### 2. 内容完整性验证
```python
def validate_content_completeness(article_content, original_content):
    """
    验证文章内容的完整性
    """
    # 1. 主题一致性检查
    key_topics = identify_key_topics(original_content)
    article_topics = identify_key_topics(article_content)
    
    missing_topics = [t for t in key_topics if t not in article_topics and len(t) > 3]
    
    if missing_topics:
        print("⚠️ 重要主题可能缺失:")
        for topic in missing_topics:
            print(f"  - {topic}")
    
    # 2. 关键词覆盖率检查
    key_words = extract_keywords(original_content)
    article_words = extract_keywords(article_content)
    
    coverage_rate = sum(1 for word in key_words if word in article_words) / len(key_words)
    print(f"关键词覆盖率: {coverage_rate:.1%}")
    
    # 3. 关键信息检查
    critical_info = identify_critical_info(original_content)
    article_critical = identify_critical_info(article_content)
    
    print("\n🎯 关键信息对比:")
    for info in critical_info:
        if info in article_critical:
            print(f"✅ 包含: {info}")
        else:
            print(f"❌ 可能缺失: {info}")
```

#### 3. 自动重组策略
```python
def regenerate_with_full_content(original_subtitles, current_article):
    """
    基于完整字幕内容重新调整文章
    """
    # 1. 首先识别当前文章的内容结构
    article_structure = analyze_article_structure(current_article)
    
    # 2. 识别未使用的重要内容
    unused_content = identify_unused_content(original_subtitles, current_article)
    
    # 3. 重组文章，纳入缺失内容
    new_article = integrate_unused_content(current_article, unused_content)
    
    return new_article
```

### 最终内容审核清单

**生成文章前必须完成的检查：**

```
✅ 完整读取所有字幕内容
✅ 分析内容结构和主题分布
✅ 确保使用 80% 以上的内容
✅ 检查重要主题是否完整
✅ 验证关键词覆盖率
✅ 确认所有关键信息点
✅ 检查是否有截断或过度删除
✅ 验证文章的逻辑连贯性
✅ 进行可读性测试
✅ 检查引用和来源信息
```

## 注意事项（简化高效版）

- **根据用户选择的语言进行翻译和整理**：
  - **简体中文**：如果字幕是英文，翻译成中文后整理
  - **English**：如果字幕是中文，翻译成英文后整理
- **保持文章逻辑清晰，结构分明**
- **HTML 中使用 CSS 变量保证配色一致**
- **添加平滑的滚动动画提升阅读体验**
- **严格遵循内容完整性要求**，不允许过度截断
- **对于长对话，进行合理的分段和整理**
- **文件管理策略**：只保留 cookies.txt 和最终 HTML 文件
- **不使用 Python 脚本**，直接通过 Claude 自然语言处理生成内容

## 故障排除（改进版）

### 1. 基本检查

```bash
# 检查工具是否安装正确
yt-dlp --version
which yt-dlp  # macOS/Linux
where yt-dlp  # Windows

# 如果已安装但找不到，检查 PATH
echo $PATH  # macOS/Linux
echo %PATH%  # Windows
```

### 2. 字幕下载失败

```bash
# 更新到最新版本
yt-dlp -U

# 详细错误排查（获取调试信息）
yt-dlp --verbose --list-subs "视频链接" > yt-dlp-verbose.log 2>&1

# 检查网络连接和代理
curl -I "https://www.youtube.com"  # 测试网络
```

### 3. B站视频字幕（改进的处理方法）

B站视频的字幕支持需要特殊处理：

#### 情况一：视频有 CC 字幕
直接下载即可：
```bash
yt-dlp --write-subs --sub-lang zh-Hans --skip-download -o "%(title)s.%(ext)s" "B站视频链接"
```

#### 情况二：视频无 CC 字幕（需要 AI 生成字幕）
B站需要用户提供 SESSDATA cookie 才能访问 AI 字幕生成功能。

**引导用户提供 SESSDATA 的步骤：**

1. 使用 Chrome/Edge 浏览器打开 B站并**登录账号**
2. 按 `F12` 打开开发者工具
3. 点击顶部 `Application` 标签页
4. 在左侧边栏展开 `Cookies` → 点击 `https://www.bilibili.com`
5. 在右侧列表中找到 `SESSDATA` 这一行
6. 复制 `Value` 列的完整值（一串很长的字符）
7. 将该值提供给 Claude

**使用 SESSDATA 下载字幕（自动处理）：**

现在系统会自动处理 cookie 文件：

```bash
# 输入示例（用户提供）
d73c2b36%2C1791183873%2Cb17c5%2A42CjDZOd7b8EqlwLu7cxUr0GOZAfJpPdBDWrvwYB6znSHYir2GZIth6K_bzJ_Zu6aV1qQSVk50TlFqU0tGa1otdnV6WWJBckw3M0s5VGdLWjhPeVN2Ymh1MnVGSk12dUZPSVJKTXFYSmJJUDRDMTVuMG9xYzB3YXo4MVBjVHV5c0wwQTFpYVByTkhBIIEC

# 系统自动生成 cookie 文件
cat > cookies.txt << EOF
# Netscape HTTP Cookie File
.bilibili.com	TRUE	/	TRUE	1791183873	SESSDATA	d73c2b36%2C1791183873%2Cb17c5%2A42CjDZOd7b8EqlwLu7cxUr0GOZAfJpPdBDWrvwYB6znSHYir2GZIth6K_bzJ_Zu6aV1qQSVk50TlFqU0tGa1otdnV6WWJBckw3M0s5VGdLWjhPeVN2Ymh1MnVGSk12dUZPSVJKTXFYSmJJUDRDMTVuMG9xYzB3YXo4MVBjVHV5c0wwQTFpYVByTkhBIIEC
EOF

# 下载 AI 字幕
yt-dlp --write-auto-subs --sub-lang zh-Hans --skip-download --cookies cookies.txt "视频链接"
```

**提示话术（改进版）：**
> 「这个 B站视频似乎没有 CC 字幕。如果您登录了 B站账号，可以提供 SESSDATA cookie 来获取 AI 生成的字幕。
> 
> **获取方法**：
> 1. 在浏览器登录 B站
> 2. 按 F12 → Application → Cookies → bilibili.com
> 3. 找到 SESSDATA，复制它的 Value 值给我
> 
> 请问您能提供 SESSDATA 吗？」

**常见问题排查：**

```bash
# 检查 cookie 文件是否正确生成
ls -la cookies.txt
cat cookies.txt  # 检查格式是否正确

# 测试 cookie 文件是否工作
yt-dlp --cookies cookies.txt --list-subs "视频链接"

# 如果还有问题，尝试使用 --no-check-certificate
yt-dlp --no-check-certificate --cookies cookies.txt --list-subs "视频链接"
```

#### 注意事项
- ✅ SESSDATA 包含账号敏感信息，请勿泄露给他人
- ✅ SESSDATA 有时效性，过期后需要重新获取
- ✅ AI 生成的字幕质量可能不如人工字幕，但内容完整性更好
- ✅ cookies.txt 文件会保留，下次使用同一视频时可能仍然有效

### 4. 文件找不到/路径错误

```bash
# 检查当前目录内容
ls -la

# 检查字幕下载位置
ls -la *.srt *.vtt 2>/dev/null

# 查找所有相关文件
find . -type f -name "*.srt" -o -name "*.vtt" -o -name "*.json"
```

### 5. 字幕文件分析

```bash
# 检查字幕文件格式和完整性
for file in *.srt *.vtt; do
  if [ -f "$file" ]; then
    echo "=== $file ==="
    wc -l "$file"
    head -20 "$file"
    echo "---------------"
  fi
done
```

### 6. 系统环境检查

```bash
# 检查 Python 是否可用
python3 --version

# 检查权限问题
ls -ltr

# 检查磁盘空间
df -h
```

### 7. 调试日志获取

```bash
# 启用完整调试信息
yt-dlp --verbose --write-subs --sub-lang zh-Hans --skip-download --cookies cookies.txt \
  "视频链接" > yt-dlp-debug.log 2>&1

echo "调试日志已保存到 yt-dlp-debug.log"
```
