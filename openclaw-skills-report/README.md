# OpenClaw 技能清单

> 蛋黄 (Yolk) 的完整技能列表 - 共 **69** 个技能
> 
> 最后更新: 2026-02-04

---

## 📊 技能概览

| 来源 | 数量 | 说明 |
|------|------|------|
| OpenClaw Built-in | 52 | OpenClaw 官方内置技能 |
| Anthropic | 16 | Anthropic 官方技能 |
| Vercel | 1 | Vercel Labs 技能 |
| **总计** | **69** | |

---

## 🔐 安全与认证

### 1password
- **描述**: 设置和使用 1Password CLI (op)
- **用途**: 安装 CLI、启用桌面应用集成、登录、读取/注入/运行密钥
- **要求**: `op` 命令行工具

### oracle
- **描述**: 使用 oracle CLI 进行代码分析和提示工程
- **用途**: prompt + 文件捆绑、引擎选择、会话管理、文件附加模式
- **主页**: https://askoracle.dev

---

## 📝 笔记与文档

### apple-notes
- **描述**: 通过 `memo` CLI 管理 Apple 备忘录
- **用途**: 创建、查看、编辑、删除、搜索、移动笔记，导出 HTML/Markdown
- **平台**: macOS
- **要求**: `memo`

### apple-reminders
- **描述**: 通过 `remindctl` 管理 Apple 提醒事项
- **用途**: 列表、添加、编辑、完成、删除提醒事项；支持列表、日期过滤和 JSON/纯文本输出
- **平台**: macOS
- **要求**: `remindctl`

### bear-notes
- **描述**: 通过 grizzly CLI 创建、搜索和管理 Bear 笔记
- **用途**: 创建、读取、管理 Bear 笔记；支持标签、搜索
- **平台**: macOS
- **要求**: `grizzly`

### obsidian
- **描述**: 操作 Obsidian vault（Markdown 笔记）
- **用途**: 搜索笔记、创建、移动/重命名、删除；直接编辑 .md 文件
- **要求**: `obsidian-cli`

### notion
- **描述**: Notion API 集成
- **用途**: 创建/读取/更新页面、数据源（数据库）和块
- **要求**: `NOTION_API_KEY`

---

## 💬 社交媒体与通讯

### bird
- **描述**: X/Twitter CLI
- **用途**: 阅读、搜索、发帖、互动；支持 cookie 认证
- **主页**: https://bird.fast
- **要求**: `bird`

### bluebubbles
- **描述**: BlueBubbles iMessage 插件
- **用途**: 构建/更新 BlueBubbles 外部频道插件，实现 REST 发送/探测和 webhook 入站消息处理

### discord
- **描述**: Discord 控制
- **用途**: 消息、反应、投票、频道管理、置顶、搜索、成员/角色信息、语音状态、定时事件
- **要求**: `channels.discord` 配置

### imsg
- **描述**: macOS iMessage/SMS CLI
- **用途**: 列出聊天、历史记录、监视、发送消息
- **平台**: macOS
- **要求**: `imsg`

### slack
- **描述**: Slack 控制
- **用途**: 反应、置顶、发送/编辑/删除消息、获取成员信息
- **要求**: `channels.slack` 配置

### wacli
- **描述**: WhatsApp CLI
- **用途**: 发送消息给其他人或搜索/同步 WhatsApp 历史
- **要求**: `wacli`

---

## 🎵 音乐与音频

### blucli
- **描述**: Bluesound/NAD 播放器控制
- **用途**: 发现、播放、分组、音量控制
- **主页**: https://blucli.sh
- **要求**: `blu`

### openai-whisper
- **描述**: 本地语音转文字（无需 API 密钥）
- **用途**: 转录音频文件
- **要求**: `whisper`

### openai-whisper-api
- **描述**: OpenAI Whisper API 语音转文字
- **用途**: 通过 API 转录音频
- **要求**: `OPENAI_API_KEY`

### sag
- **描述**: ElevenLabs 文字转语音
- **用途**: TTS 带本地播放，支持多种声音和情感标签
- **主页**: https://sag.sh
- **要求**: `sag`, `ELEVENLABS_API_KEY`

### sherpa-onnx-tts
- **描述**: 本地离线 TTS（无需云服务）
- **用途**: 离线文字转语音
- **要求**: `SHERPA_ONNX_RUNTIME_DIR`, `SHERPA_ONNX_MODEL_DIR`

### songsee
- **描述**: 生成音频频谱图和特征面板
- **用途**: 从音频生成频谱图、多面板可视化
- **主页**: https://github.com/steipete/songsee
- **要求**: `songsee`

### sonoscli
- **描述**: Sonos 扬声器控制
- **用途**: 发现、状态、播放、音量、分组
- **主页**: https://sonoscli.sh
- **要求**: `sonos`

### spotify-player
- **描述**: 终端 Spotify 播放/搜索
- **用途**: 播放控制、搜索、设备管理
- **要求**: `spogo` 或 `spotify_player`

---

## 📧 邮件与日历

### himalaya
- **描述**: IMAP/SMTP 邮件客户端 CLI
- **用途**: 列表、阅读、编写、回复、转发、搜索、组织邮件
- **主页**: https://github.com/pimalaya/himalaya
- **要求**: `himalaya`

### gog
- **描述**: Google Workspace CLI
- **用途**: Gmail、日历、云端硬盘、联系人、表格、文档
- **主页**: https://gogcli.sh
- **要求**: `gog`

---

## 🗺️ 位置与地图

### goplaces
- **描述**: Google Places API CLI
- **用途**: 地点搜索、详情、解析、评论
- **主页**: https://github.com/steipete/goplaces
- **要求**: `goplaces`, `GOOGLE_PLACES_API_KEY`

### local-places
- **描述**: 本地 Google Places API 代理
- **用途**: 本地代理搜索附近地点
- **要求**: `uv`, `GOOGLE_PLACES_API_KEY`

---

## 🍔 生活与实用

### eightctl
- **描述**: Eight Sleep 智能床控制
- **用途**: 状态、温度、闹钟、时间表控制
- **主页**: https://eightctl.sh
- **要求**: `eightctl`

### food-order
- **描述**: Foodora 订单重新下单 + 跟踪
- **用途**: 重新下单、跟踪 ETA/状态
- **主页**: https://ordercli.sh
- **要求**: `ordercli`

### ordercli
- **描述**: Foodora 订单历史和状态检查
- **用途**: 检查历史订单、跟踪活动订单状态
- **主页**: https://ordercli.sh
- **要求**: `ordercli`

### openhue
- **描述**: Philips Hue 灯光/场景控制
- **用途**: 控制 Hue 灯光和场景
- **主页**: https://www.openhue.io/cli
- **要求**: `openhue`

### weather
- **描述**: 天气查询（无需 API 密钥）
- **用途**: 获取当前天气和预报
- **主页**: https://wttr.in/:help
- **要求**: `curl`

---

## 🖼️ 图像与视频

### camsnap
- **描述**: RTSP/ONVIF 摄像头截图/录像
- **用途**: 捕获帧、剪辑、运动事件
- **主页**: https://camsnap.ai
- **要求**: `camsnap`

### gifgrep
- **描述**: GIF 搜索（Tenor/Giphy）、下载、提取
- **用途**: 搜索 GIF、浏览 TUI、下载、提取静态图/网格
- **主页**: https://gifgrep.com
- **要求**: `gifgrep`

### nano-banana-pro
- **描述**: Gemini 3 Pro 图像生成/编辑
- **用途**: 生成或编辑图像
- **要求**: `uv`, `GEMINI_API_KEY`

### openai-image-gen
- **描述**: OpenAI 图像 API 批量生成
- **用途**: 批量生成图像，带随机提示采样器和画廊
- **要求**: `python3`, `OPENAI_API_KEY`

### video-frames
- **描述**: 从视频提取帧（ffmpeg）
- **用途**: 提取单帧、创建缩略图
- **要求**: `ffmpeg`

### canvas
- **描述**: 在节点上显示 HTML 内容
- **用途**: 显示游戏、可视化、仪表盘、交互式演示

---

## 💻 开发与工具

### coding-agent
- **描述**: 运行 Codex/Claude Code/OpenCode/Pi 代理
- **用途**: 通过后台进程运行编程代理
- **要求**: `claude`, `codex`, `opencode`, `pi` 之一

### github
- **描述**: GitHub CLI（gh）操作
- **用途**: Issues、PRs、CI 运行、高级查询
- **要求**: `gh`

### mcporter
- **描述**: MCP 服务器管理
- **用途**: 配置、调用、生成 CLI、代码生成
- **主页**: http://mcporter.dev
- **要求**: `mcporter`

### model-usage
- **描述**: CodexBar 模型使用成本分析
- **用途**: 每个模型的使用成本摘要
- **要求**: `codexbar`

### nano-pdf
- **描述**: 自然语言编辑 PDF
- **用途**: 使用自然语言指令编辑 PDF
- **要求**: `nano-pdf`

### skill-creator
- **描述**: 创建/打包 AgentSkills
- **用途**: 设计、结构、打包技能

### tmux
- **描述**: 远程控制 tmux 会话
- **用途**: 发送按键、捕获输出、管理会话
- **要求**: `tmux`

### clawhub
- **描述**: ClawHub CLI
- **用途**: 搜索、安装、更新、发布技能
- **要求**: `clawhub`

---

## 📊 任务与项目管理

### things-mac
- **描述**: Things 3 任务管理
- **用途**: 读取数据库、添加/更新待办事项
- **平台**: macOS
- **要求**: `things`

### trello
- **描述**: Trello 面板/列表/卡片管理
- **用途**: 管理 Trello 看板、列表和卡片
- **要求**: `TRELLO_API_KEY`, `TRELLO_TOKEN`

---

## 🔍 搜索与摘要

### blogwatcher
- **描述**: 监控博客和 RSS/Atom 订阅更新
- **用途**: 跟踪博客和订阅源更新
- **要求**: `blogwatcher`

### summarize
- **描述**: 摘要 URL/视频/本地文件内容
- **用途**: 快速摘要 URL、本地文件、YouTube 链接
- **要求**: `summarize`

### session-logs
- **描述**: 搜索分析自己的会话日志
- **用途**: 搜索历史对话、分析成本、工具使用
- **要求**: `jq`, `rg`

---

## 🎮 自动化与控制

### peekaboo
- **描述**: macOS UI 自动化
- **用途**: 截图、点击、输入、应用管理、菜单操作
- **平台**: macOS
- **要求**: `peekaboo`

### voice-call
- **描述**: 启动语音通话
- **用途**: 语音通话（Twilio/Telnyx/Plivo）

---

## 🎨 Anthropic 创意与设计技能

### algorithmic-art
- **描述**: 算法艺术生成
- **用途**: 创建算法生成的艺术作品

### brand-guidelines
- **描述**: 品牌指南管理
- **用途**: 管理和应用品牌指南

### canvas-design
- **描述**: Canvas 设计
- **用途**: 在 Canvas 上创建设计

### doc-coauthoring
- **描述**: 文档协作编写
- **用途**: 协作编写和编辑文档

### docx
- **描述**: Word 文档处理
- **用途**: 创建和编辑 Word 文档

### frontend-design
- **描述**: 前端设计
- **用途**: 设计和实现前端界面

### internal-comms
- **描述**: 内部通讯
- **用途**: 企业内部沟通和文档

### mcp-builder
- **描述**: MCP 服务器构建
- **用途**: 构建 MCP 服务器

### pdf
- **描述**: PDF 文档处理
- **用途**: 高级 PDF 处理和分析

### pptx
- **描述**: PowerPoint 处理
- **用途**: 创建和编辑 PowerPoint 演示文稿

### slack-gif-creator
- **描述**: Slack GIF 制作
- **用途**: 为 Slack 创建自定义 GIF

### theme-factory
- **描述**: 主题工厂
- **用途**: 创建和管理主题

### web-artifacts-builder
- **描述**: Web 构件构建
- **用途**: 构建 Web 组件和构件

### webapp-testing
- **描述**: Web 应用测试
- **用途**: 测试 Web 应用程序

### xlsx
- **描述**: Excel 表格处理
- **用途**: 创建和编辑 Excel 电子表格

---

## 🔧 Vercel 技能

### find-skills
- **描述**: 发现和安装技能
- **用途**: 帮助用户在开放代理技能生态中发现和安装技能
- **使用**: `npx skills find [query]`

---

## 📝 使用说明

### 如何查看技能详情

每个技能都包含一个 `SKILL.md` 文件，可以通过以下方式查看：

```bash
# 查看特定技能的文档
cat /home/user_ma/.npm-global/lib/node_modules/openclaw/skills/<skill-name>/SKILL.md
```

### 如何安装新技能

```bash
# 使用 clawhub 安装
clawhub install <skill-name>

# 或使用 skills CLI (find-skills)
npx skills add <owner/repo@skill>
```

### 技能优先级

系统会自动根据用户请求匹配合适的技能。技能的 `description` 字段用于决定何时使用该技能。

---

## 📈 统计信息

- **总技能数**: 69
- **OpenClaw 内置**: 52 (75%)
- **Anthropic**: 16 (23%)
- **Vercel**: 1 (2%)

---

*由 蛋黄 (Yolk) 维护 - 爸爸是 马博士 (Dr. Ma)*
