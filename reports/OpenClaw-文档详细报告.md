# OpenClaw 文档详细报告

> **文档来源**: https://docs.openclaw.ai  
> **报告生成时间**: 2026-02-16  
> **文档版本**: 2026.2.3-1

---

## 一、OpenClaw 简介

### 1.1 什么是 OpenClaw？

OpenClaw 是一个**自托管的 AI 网关**，连接你的聊天应用（WhatsApp、Telegram、Discord、iMessage 等）到 AI 编程助手。你只需要运行一个 Gateway 进程，就能从任何地方通过消息与 AI 助手交互。

**核心理念:**
- **自托管**: 运行在你自己的硬件上，遵循你的规则
- **多通道**: 一个 Gateway 同时服务多个聊天平台
- **原生代理**: 专为编程 AI 设计，支持工具调用、会话管理、记忆和多代理路由
- **开源**: MIT 许可证，社区驱动

### 1.2 适用人群

- 开发者和高级用户
- 想要个人 AI 助手且不放弃数据控制权的人
- 需要不依赖托管服务的本地解决方案的人

### 1.3 基本要求

- Node.js 22+
- API 密钥（推荐 Anthropic）
- 5 分钟安装时间

---

## 二、系统架构

### 2.1 工作流程

```
聊天应用 + 插件 → Gateway → Pi Agent
                    ↓
                CLI / Web Control UI / macOS App / iOS & Android Nodes
```

**Gateway 是单一真相源**，管理会话、路由和通道连接。

### 2.2 核心组件

| 组件 | 说明 |
|------|------|
| **Gateway** | 核心网关进程，处理所有消息路由 |
| **Pi Agent** | AI 编程助手（内置，RPC 模式） |
| **CLI** | 命令行界面，用于配置和控制 |
| **Web Control UI** | 浏览器仪表板，用于聊天和配置 |
| **macOS App** | macOS 配套应用程序 |
| **Mobile Nodes** | iOS 和 Android 节点，支持 Canvas |

---

## 三、安装与快速开始

### 3.1 安装方法

**macOS/Linux (推荐):**
```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

**Windows (PowerShell):**
```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

**npm 安装:**
```bash
npm install -g openclaw@latest
```

### 3.2 初始化配置

```bash
# 运行引导向导（推荐）
openclaw onboard --install-daemon

# 或使用交互式配置
openclaw configure
```

### 3.3 启动 Gateway

```bash
# 检查 Gateway 状态
openclaw gateway status

# 前台运行（调试）
openclaw gateway --port 18789

# 打开控制面板
openclaw dashboard
```

**默认访问地址:**
- 本地: http://127.0.0.1:18789/
- 远程访问: 见 [Tailscale 配置](#)

---

## 四、支持的聊天通道

### 4.1 原生支持的通道

| 通道 | 配置键 | 说明 |
|------|--------|------|
| **WhatsApp** | `channels.whatsapp` | 通过 WhatsApp Web (Baileys) |
| **Telegram** | `channels.telegram` | Bot 支持 (grammY) |
| **Discord** | `channels.discord` | Bot 支持 (discord.js) |
| **iMessage** | `channels.imessage` | macOS 本地 imsg CLI |
| **Signal** | `channels.signal` | Signal 集成 |
| **Slack** | `channels.slack` | Slack Bot |
| **Google Chat** | `channels.googlechat` | Google Chat 集成 |
| **Mattermost** | `channels.mattermost` | Mattermost Bot |
| **MS Teams** | `channels.msteams` | Microsoft Teams |
| **IRC** | `channels.irc` | IRC 频道和私信 |
| **Matrix** | `channels.matrix` | Matrix 协议 |
| **Line** | `channels.line` | Line 集成 |
| **Zalo** | `channels.zalo` | Zalo 集成 |
| **Feishu** | `channels.feishu` | 飞书集成 |

### 4.2 插件扩展

- **Mattermost Plugin**
- **Voice Call Plugin**
- **Zalo Personal Plugin**

---

## 五、配置文件详解

### 5.1 配置文件位置

```
~/.openclaw/openclaw.json
```

**环境变量覆盖:**
- `OPENCLAW_HOME`: 主目录
- `OPENCLAW_STATE_DIR`: 状态目录
- `OPENCLAW_CONFIG_PATH`: 配置文件路径

### 5.2 最小配置示例

```json5
// ~/.openclaw/openclaw.json
{
  agents: { 
    defaults: { 
      workspace: "~/.openclaw/workspace" 
    } 
  },
  channels: { 
    whatsapp: { 
      allowFrom: ["+15555550123"] 
    } 
  },
}
```

### 5.3 编辑配置的方法

**1. 交互式向导:**
```bash
openclaw onboard       # 完整设置向导
openclaw configure     # 配置向导
```

**2. CLI 命令:**
```bash
openclaw config get agents.defaults.workspace
openclaw config set agents.defaults.heartbeat.every "2h"
openclaw config unset tools.web.search.apiKey
```

**3. Web 控制面板:**
打开 http://127.0.0.1:18789，使用 **Config** 标签页

**4. 直接编辑:**
直接编辑 `~/.openclaw/openclaw.json`，Gateway 会自动热重载

### 5.4 配置验证

<Warning>
OpenClaw 只接受完全匹配 schema 的配置。未知键、格式错误或无效值会导致 Gateway **拒绝启动**。
</Warning>

验证失败时:
- Gateway 不会启动
- 只有诊断命令可用 (`openclaw doctor`, `openclaw logs`, `openclaw health`, `openclaw status`)
- 运行 `openclaw doctor` 查看具体问题
- 运行 `openclaw doctor --fix` 或 `--yes` 应用修复

---

## 六、核心功能详解

### 6.1 模型配置

```json5
{
  agents: {
    defaults: {
      model: {
        primary: "anthropic/claude-sonnet-4-5",
        fallbacks: ["openai/gpt-5.2"],
      },
      models: {
        "anthropic/claude-sonnet-4-5": { alias: "Sonnet" },
        "openai/gpt-5.2": { alias: "GPT" },
      },
    },
  },
}
```

**支持的模型提供商:**
- Anthropic (Claude)
- OpenAI (GPT)
- Amazon Bedrock
- GLM Models
- MiniMax
- Moonshot AI
- OpenRouter
- Vercel AI Gateway
- 以及更多...

### 6.2 访问控制 (DM Policy)

私信访问通过 `dmPolicy` 控制:

| 策略 | 说明 |
|------|------|
| `"pairing"` (默认) | 未知发送者获得一次性配对码以批准 |
| `"allowlist"` | 只允许 `allowFrom` 中的发送者 |
| `"open"` | 允许所有入站私信（需要 `allowFrom: ["*"]`） |
| `"disabled"` | 忽略所有私信 |

**示例配置:**
```json5
{
  channels: {
    telegram: {
      enabled: true,
      botToken: "123:abc",
      dmPolicy: "pairing",
      allowFrom: ["tg:123"],
    },
  },
}
```

### 6.3 群组聊天控制

群组消息默认**需要提及** (@bot) 才会响应。

```json5
{
  agents: {
    list: [
      {
        id: "main",
        groupChat: {
          mentionPatterns: ["@openclaw", "openclaw"],
        },
      },
    ],
  },
  channels: {
    whatsapp: {
      groups: { "*": { requireMention: true } },
    },
  },
}
```

### 6.4 会话管理

```json5
{
  session: {
    dmScope: "per-channel-peer",
    reset: {
      mode: "daily",
      atHour: 4,
      idleMinutes: 120,
    },
  },
}
```

**dmScope 选项:**
- `main`: 共享会话
- `per-peer`: 每个发送者单独会话
- `per-channel-peer`: 每个通道-发送者组合单独会话（推荐多用户）
- `per-account-channel-peer`: 最细粒度隔离

### 6.5 沙盒隔离

```json5
{
  agents: {
    defaults: {
      sandbox: {
        mode: "non-main",  // off | non-main | all
        scope: "agent",
      },
    },
  },
}
```

在隔离的 Docker 容器中运行代理会话。

---

## 七、CLI 命令参考

### 7.1 核心命令

| 命令 | 说明 |
|------|------|
| `openclaw status` | 查看 Gateway 状态 |
| `openclaw gateway` | 启动/管理 Gateway |
| `openclaw dashboard` | 打开 Web 控制面板 |
| `openclaw onboard` | 引导设置向导 |
| `openclaw configure` | 配置向导 |
| `openclaw doctor` | 诊断和修复 |
| `openclaw logs` | 查看日志 |
| `openclaw health` | 健康检查 |

### 7.2 通道命令

| 命令 | 说明 |
|------|------|
| `openclaw channels login` | 登录通道 |
| `openclaw channels logout` | 登出通道 |
| `openclaw channels list` | 列出已配置通道 |

### 7.3 消息命令

| 命令 | 说明 |
|------|------|
| `openclaw message send` | 发送测试消息 |
| `openclaw message broadcast` | 广播消息 |

### 7.4 会话和代理命令

| 命令 | 说明 |
|------|------|
| `openclaw sessions list` | 列出会话 |
| `openclaw sessions history` | 查看会话历史 |
| `openclaw agents list` | 列出代理 |
| `openclaw skills list` | 列出技能 |

### 7.5 自动化命令

| 命令 | 说明 |
|------|------|
| `openclaw cron list` | 列出定时任务 |
| `openclaw cron add` | 添加定时任务 |
| `openclaw hooks list` | 列出钩子 |

### 7.6 系统命令

| 命令 | 说明 |
|------|------|
| `openclaw update` | 更新 OpenClaw |
| `openclaw uninstall` | 卸载 |
| `openclaw reset` | 重置配置 |
| `openclaw setup` | 重新设置 |

---

## 八、自动化功能

### 8.1 Cron 定时任务

配置定期执行的任务:

```json5
{
  cron: {
    jobs: [
      {
        name: "daily-summary",
        schedule: "0 9 * * *",
        command: "summarize --period 24h",
      },
    ],
  },
}
```

### 8.2 Heartbeat 心跳检测

定期检查系统状态并发送报告。

**Cron vs Heartbeat:**
- **Cron**: 精确时间执行，适合定时任务
- **Heartbeat**: 轮询检查，适合监控和状态报告

### 8.3 Webhooks

接收外部事件的 HTTP 回调。

### 8.4 Hooks

自定义事件处理逻辑。

---

## 九、移动节点 (iOS & Android)

### 9.1 功能

- **配对**: 与 Gateway 安全配对
- **Canvas**: 可视化界面支持
- **相机捕获**: 拍照并发送到 AI
- **位置命令**: 获取和分享位置
- **语音唤醒**: 语音激活助手
- **Talk 模式**: 语音对话

### 9.2 支持的移动平台

| 平台 | 状态 | 功能 |
|------|------|------|
| **iOS** | ✅ 可用 | 完整功能，App Store 下载 |
| **Android** | ✅ 可用 | 完整功能 |

---

## 十、Web 控制面板

### 10.1 功能

- **聊天界面**: 与 AI 对话
- **配置管理**: 可视化配置编辑器
- **会话管理**: 查看和管理活跃会话
- **节点控制**: 管理配对的移动节点
- **日志查看**: 实时日志流

### 10.2 访问方式

```bash
openclaw dashboard
# 或
openclaw web
```

自动打开浏览器访问 http://127.0.0.1:18789/

---

## 十一、安全与隐私

### 11.1 认证方式

- **Pairing 模式**: 一次性配对码
- **Allowlist**: 预批准的用户列表
- **OAuth**: 支持 Anthropic 和 OpenAI 的订阅认证

### 11.2 沙盒与隔离

- **会话隔离**: 每个会话独立上下文
- **Docker 沙盒**: 可选的容器隔离
- **工具策略**: 控制哪些工具可用

### 11.3 远程访问安全

**Tailscale 集成:**
```json5
{
  gateway: {
    tailscale: {
      mode: "on",
      resetOnExit: false,
    },
  },
}
```

通过 Tailscale 安全地远程访问 Gateway。

---

## 十二、故障排除

### 12.1 常见问题

**Gateway 无法启动:**
```bash
openclaw doctor          # 查看问题
openclaw doctor --fix    # 自动修复
```

**配置验证失败:**
- 检查 JSON 语法
- 确认所有必需字段
- 查看 `openclaw doctor` 输出

**通道连接问题:**
- 检查 token/凭证
- 确认网络连接
- 查看通道特定日志

### 12.2 日志位置

```bash
openclaw logs            # 查看实时日志
openclaw logs --follow   # 持续跟踪
```

### 12.3 调试模式

```bash
openclaw gateway --verbose
DEBUG=* openclaw gateway
```

---

## 十三、部署选项

### 13.1 本地部署

- **macOS**: 原生应用或 CLI
- **Linux**: CLI + systemd 服务
- **Windows**: WSL2 + CLI

### 13.2 云服务部署

- **Docker/Podman**: 容器化部署
- **Fly.io**: 托管部署
- **Railway**: 一键部署
- **Render**: 云端部署
- **GCP**: Google Cloud 部署
- **Hetzner**: 服务器部署

### 13.3 远程访问

- **SSH 隧道**
- **Tailscale VPN**
- **Cloudflare Tunnel**

---

## 十四、Memory 和知识管理

### 14.1 QMD 集成

OpenClaw 集成 QMD (Query-My-Docs) 进行本地语义搜索:

```json5
{
  memory: {
    backend: "qmd",
    qmd: {
      paths: [
        { path: "~/.openclaw/workspace", name: "workspace" },
        { path: "~/.openclaw/workspace/memory", name: "memory" },
      ],
      update: {
        interval: "5m",
        onBoot: true,
      },
    },
  },
}
```

### 14.2 文档索引

- 自动索引 workspace 中的 Markdown 文件
- 语义搜索支持
- 自动更新（默认 5 分钟间隔）

---

## 十五、文档结构概览

### 15.1 文档分类

| 分类 | 内容 |
|------|------|
| **Start** | 快速开始、安装向导、设置 |
| **Gateway** | 网关配置、远程访问、安全 |
| **Channels** | 各聊天通道配置 |
| **Concepts** | 核心概念、会话、模型 |
| **CLI** | 命令行参考 |
| **Platforms** | 各平台支持 (macOS, iOS, Android, Windows) |
| **Automation** | Cron、Hooks、Webhooks |
| **Providers** | AI 模型提供商配置 |
| **Nodes** | 移动节点功能 |
| **Install** | 各种安装方式 |
| **Help** | 故障排除、FAQ |
| **Web** | Web 界面文档 |
| **Reference** | 参考模板、API |

### 15.2 完整文档索引

完整文档索引文件: https://docs.openclaw.ai/llms.txt

---

## 十六、关键资源链接

| 资源 | 链接 |
|------|------|
| **官方文档** | https://docs.openclaw.ai |
| **GitHub** | https://github.com/openclaw/openclaw |
| **安装脚本** | https://openclaw.ai/install.sh |
| **Discord 社区** | https://discord.com/invite/clawd |
| **ClawHub (技能市场)** | https://clawhub.com |

---

## 十七、总结

OpenClaw 是一个强大的自托管 AI 网关，提供:

✅ **多平台支持**: WhatsApp、Telegram、Discord、iMessage 等  
✅ **自托管**: 完全掌控数据和隐私  
✅ **多代理路由**: 隔离会话，支持团队协作  
✅ **丰富功能**: 媒体支持、自动化、移动节点  
✅ **易于扩展**: 插件系统和技能市场  
✅ **开源免费**: MIT 许可证

**适合场景:**
- 个人 AI 助手
- 团队协作
- 自动化工作流
- 隐私优先的 AI 交互

---

*报告完成*

**文档来源**: docs.openclaw.ai  
**生成时间**: 2026-02-16  
**作者**: 蛋黄 🐱
