<div align="center">

# 🎬 Video-to-Article.skill

**将视频一键转换为精美文章的 agent skill**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Claude Code](https://img.shields.io/badge/Claude%20Code-Skill-blue)](https://claude.ai/code)
[![yt-dlp](https://img.shields.io/badge/Powered%20by-yt--dlp-green)](https://github.com/yt-dlp/yt-dlp)

[English](#english) · [简体中文](#简体中文)

</div>

---

## 简体中文

### 😫 你是否遇到过这些问题？

> **"这个 YouTube 视频没有中文字幕，看不懂..."**
>
> **"45分钟的视频太长了，没时间看完，但内容很重要..."**
>
> **"看了一堆教程视频，想整理成笔记，但手动记录太累..."**
>
> **“想给我的自媒体快速寻找一些灵感，但懒得看视频”**
>
> **“想把看过的视频整理成知识库保存着防止遗忘又懒得记笔记”**

如果以上任何一条戳中了你，这个 skill 就是为你准备的。

### ✨ Video to Article 能帮你做什么？

只需发送一条视频链接，Claude 就能自动：

- 📥 **下载视频字幕**（支持 YouTube、B站、Twitter 等主流平台）
- 🌐 **智能翻译**（英文视频自动翻译成流畅中文，或中文视频翻译成英文）
- ✍️ **整理成文章**（结构清晰、逻辑分明，不是简单的字幕堆砌）
- 🎨 **生成精美 HTML**（5种专业设计风格，支持手机/电脑阅读）
- 📋 **输出多平台文案**（小红书、公众号、口播稿，配好图/直接能用）
- 📎 **保留来源信息**（视频封面、作者、时长、原链接一应俱全）

### 🎯 使用场景

| 场景 | 示例 |
|------|------|
| 📚 **学习笔记** | 把 1 小时的技术讲座整理成可反复阅读的文章 |
| 🌍 **跨语言学习** | 英文视频自动翻译成中文，或中文视频翻译成英文 |
| 📝 **内容创作** | 快速整理视频要点，作为写作素材 |
| 💼 **工作汇报** | 把培训视频变成文档，方便分享和存档 |
| 🎧 **知识管理** | 建立「视频→文章」的知识库 |

### 🚀 快速开始

#### 1. 安装 Skill

```bash
# 克隆仓库
git clone https://github.com/Librarier-f/video-to-article-skill.git

# 复制到 Claude Code skills 目录
# Windows
cp -r video-to-article "$HOME/.claude/skills/"

# macOS / Linux
cp -r video-to-article ~/.claude/skills/
```

#### 2. 开始使用

在 Claude Code 中发送视频链接：

```
请把这个视频整理成文章：https://www.youtube.com/watch?v=xxxxx
```

Claude 会自动：
1. 询问输出平台（HTML文章 / 小红书 / 公众号 / 口播稿）
2. 询问具体风格/文体
3. 下载字幕并生成内容

### 🎨 5 种精美设计风格

| 风格 | 特点 | 适合场景 |
|------|------|----------|
| **蓝白商务** 💼 | 专业简洁、蓝色渐变、衬线标题 | 商务报告、学习笔记、通用文章 |
| **Notion 深色** 🌙 | Notion 风格、紧凑排版、彩色标注 | 技术文档、知识库、夜间阅读 |
| **Claude 风格** 🎨 | 米白背景、赭石红强调、纸质感 | 优雅文章、人文内容、深度阅读 |
| **暗黑科技** ⚡ | 赛博朋克、霓虹光效、故障艺术 | 技术教程、极客风格、编程内容 |
| **极简现代** 📄 | 极致留白、衬线字体、深绿强调 | 优雅阅读、杂志排版、深度内容 |



### 📺 支持的视频平台

| 平台 | 字幕支持 | 备注 |
|------|---------|------|
| **YouTube** | ✅ 手动字幕 + AI 字幕 | 最佳支持 |
| **Bilibili** | ✅ CC字幕 + AI字幕 | AI字幕需提供 SESSDATA cookie |
| **Twitter/X** | ✅ 内嵌字幕 | 部分视频支持 |
| **Vimeo** | ✅ 手动字幕 | 依赖上传者添加 |
| **其他** | ⚠️ 有限支持 | 取决于平台 |

### 🌐 语言支持

- **简体中文输出**：英文视频自动翻译成中文
- **English 输出**：中文视频自动翻译成英文

### 📋 输出平台

支持 4 种输出格式，满足不同发布需求：

| 平台 | 输出格式 | 特点 |
|------|---------|------|
| **HTML文章** | `.html` | 精美网页，5种风格可选 |
| **小红书文案** | `.txt` + `.html`预览 | 适配平台风格，配图建议 |
| **公众号文章** | `.md` + `.html`预览 | Markdown格式，复制即用 |
| **口播稿** | `.txt` + `.html`预览 | 短视频配音稿，可直接使用 |

每种平台都有专属的文案模板，确保生成的内容符合目标平台的特点。

### 📁 项目结构

```
video-to-article/
├── SKILL.md                      # Skill 主文件
├── README.md                     # 项目说明
├── LICENSE                       # MIT 许可证
├── .gitignore                    # Git 忽略文件
├── component/
│   └── yt-dlp.exe               # 内置 yt-dlp（Windows）
├── references/
│   ├── blue-white-business-style.md  # 蓝白商务风格规范
│   ├── claude-style.md           # Claude 风格规范
│   ├── cyber-tech-style.md       # 暗黑科技风格规范
│   ├── minimal-modern-style.md    # 极简现代风格规范
│   ├── hashtags.md                # 话题标签参考
│   ├── interaction-phrases.md     # 互动话术参考
│   ├── koupogao/                  # 口播稿模板
│   │   ├── baokuan.md            # 爆款开场型
│   │   ├── gushi.md             # 故事叙述型
│   │   ├── qingdan.md           # 清单列表型
│   │   ├── duihua.md            # 对话挑战型
│   │   └── qingxu.md           # 情绪共鸣型
│   ├── xiaohongshu/              # 小红书文案模板
│   │   ├── zhongcao.md          # 种草安利文
│   │   ├── ganhuo.md           # 干货教程文
│   │   ├── qinggan.md          # 情感共鸣文
│   │   └── qingdan.md          # 清单盘点文
│   └── wechat/                   # 公众号文章模板
│       ├── redian.md            # 热点解读文
│       ├── shendu.md           # 深度长文
│       ├── jinju.md            # 金句提炼文
│       └── gushi.md            # 故事叙述文
└── examples/
    ├── sample-output-blue-white.html   # 蓝白商务示例
    ├── sample-output-notion-dark.html  # Notion深色示例
    ├── sample-output-claude.html      # Claude风格示例
    ├── sample-output-cyber-tech.html  # 暗黑科技示例
    └── sample-output-minimal-modern.html # 极简现代示例
```

### 🎨 示例预览

点击下方链接查看各风格的实际效果：

| 风格 | 示例文件 |
|------|---------|
| **蓝白商务** 💼 | [sample-output-blue-white.html](./examples/sample-output-blue-white.html) |
| **Notion 深色** 🌙 | [sample-output-notion-dark.html](./examples/sample-output-notion-dark.html) |
| **Claude 风格** 🎨 | [sample-output-claude.html](./examples/sample-output-claude.html) |
| **暗黑科技** ⚡ | [sample-output-cyber-tech.html](./examples/sample-output-cyber-tech.html) |
| **极简现代** 📄 | [sample-output-minimal-modern.html](./examples/sample-output-minimal-modern.html) |

### 🔧 高级配置

<details>
<summary><b>yt-dlp 路径配置</b></summary>

如果 yt-dlp 不在系统 PATH 中，skill 会自动检测以下路径：

| 平台 | 默认检测路径 |
|------|-------------|
| Windows（内置） | `{skill目录}/component/yt-dlp.exe` |
| Windows | `D:yt-dlp.exe`、`C:/Program Files/yt-dlp/yt-dlp.exe` |
| macOS | `/usr/local/bin/yt-dlp`、`/opt/homebrew/bin/yt-dlp` |
| Linux | `/usr/local/bin/yt-dlp`、`/usr/bin/yt-dlp` |

</details>

<details>
<summary><b>B站视频 AI 字幕获取</b></summary>

B站视频如果没有 CC 字幕，需要提供 SESSDATA cookie 来获取 AI 生成的字幕：

1. 在浏览器登录 B站
2. 按 `F12` → `Application` → `Cookies` → `bilibili.com`
3. 找到 `SESSDATA`，复制它的 Value 值
4. 提供给 Claude

**注意**：SESSDATA 包含账号敏感信息，请勿泄露给他人。

</details>

### 🐛 故障排除

<details>
<summary><b>yt-dlp 命令找不到</b></summary>

```bash
# 检查安装位置
which yt-dlp   # macOS/Linux
where yt-dlp   # Windows

# 更新 yt-dlp
yt-dlp -U
```

</details>

<details>
<summary><b>字幕下载失败</b></summary>

```bash
# 查看可用字幕
yt-dlp --list-subs "视频链接"

# 查看详细错误
yt-dlp --verbose --list-subs "视频链接"
```

</details>

### 🤝 贡献

欢迎提交 Issue 和 Pull Request！

### 📄 许可证

本项目采用 MIT 许可证 - 详见 [LICENSE](LICENSE) 文件

### 🙏 致谢

- [yt-dlp](https://github.com/yt-dlp/yt-dlp) - 强大的视频下载工具
- [Claude Code](https://claude.ai/code) - AI 编程助手
- [Google Fonts](https://fonts.google.com/) - Noto Serif SC / Noto Sans SC 字体

---

## English

### 😫 Do These Problems Sound Familiar?

> **"This YouTube video has no English subtitles, I can't understand..."**
>
> **"The video is 45 minutes long, I don't have time to watch it all..."**
>
> **"I want to take notes from tutorial videos, but manual transcription is tedious..."**
>
> **"I want to share video content with friends, but asking them to watch is too much..."**

If any of these resonate with you, this tool is for you.

### ✨ What Can Video to Article Do?

Just send a video link, and Claude will automatically:

- 📥 **Download video subtitles** (YouTube, Bilibili, Twitter, and more)
- 🌐 **Smart translation** (English→Chinese or Chinese→English)
- ✍️ **Organize into articles** (Clear structure, logical flow—not just subtitle dumps)
- 🎨 **Generate beautiful HTML** (5 professional design styles, mobile/desktop friendly)
- 📎 **Preserve source info** (Thumbnail, author, duration, original link included)

### 🎯 Use Cases

| Scenario | Example |
|----------|---------|
| 📚 **Study Notes** | Convert a 1-hour tech talk into a readable article |
| 🌍 **Cross-language Learning** | Auto-translate English videos to Chinese or vice versa |
| 📝 **Content Creation** | Quickly extract key points as writing material |
| 💼 **Work Reports** | Turn training videos into shareable documents |
| 🎧 **Knowledge Management** | Build a "video→article" knowledge base |

### 🚀 Quick Start

#### 1. Install Skill

```bash
# Clone the repository
git clone https://github.com/YOUR_USERNAME/video-to-article.git

# Copy to Claude Code skills directory
# Windows
cp -r video-to-article "$HOME/.claude/skills/"

# macOS / Linux
cp -r video-to-article ~/.claude/skills/
```

#### 2. Start Using

Send a video link in Claude Code:

```
Please convert this video to an article: https://www.youtube.com/watch?v=xxxxx
```

Claude will automatically:
1. Ask for your preferred output platform (HTML / 小红书 / WeChat / 口播稿)
2. Ask for specific style or format
3. Download subtitles and generate content

### 🎨 5 Beautiful Design Styles

| Style | Features | Best For |
|-------|----------|----------|
| **Blue & White Business** 💼 | Professional, blue gradients, serif titles | Business reports, study notes |
| **Notion Dark** 🌙 | Notion-style, compact layout, color highlights | Tech docs, knowledge base |
| **Claude Style** 🎨 | Cream background, terracotta accents, paper texture | Elegant articles, humanities |
| **Cyber Tech** ⚡ | Cyberpunk, neon effects, glitch art | Tech tutorials, programming |
| **Minimal Modern** 📄 | Extreme whitespace, serif typography, deep green accent | Elegant reading, magazine layout |

### 📺 Supported Platforms

| Platform | Subtitle Support | Notes |
|----------|------------------|-------|
| **YouTube** | ✅ Manual + AI subtitles | Best support |
| **Bilibili** | ✅ CC + AI subtitles | AI subtitles require SESSDATA cookie |
| **Twitter/X** | ✅ Embedded subtitles | Partial support |
| **Vimeo** | ✅ Manual subtitles | Depends on uploader |
| **Others** | ⚠️ Limited support | Platform dependent |

### 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

### 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

<div align="center">

**⭐ If this project helps you, please give it a star! ⭐**

Made with ❤️ for Claude Code users

</div>
