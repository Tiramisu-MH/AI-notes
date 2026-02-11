---
title: WSL2 安装 OpenClaw 完整指南
date: 2026-02-04 12:49:00
tags:
  - 笔记
  - OpenClaw
  - WSL2
categories:
  - 项目
---

<!-- 目录由 Markdown 渲染器自动生成，或在 VS Code 用插件生成 -->

# 目录

- [1. WSL2 安装](#1-wsl2-安装)
  - [1.1 为什么要装这个](#11-为什么要装这个)
  - [1.2 安装 Ubuntu 24.04](#12-安装-ubuntu-2404)
  - [1.3 网络配置](#13-网络配置)
- [2. Discord 配置（前置准备）](#2-discord-配置前置准备)
  - [2.1 注册 Discord](#21-注册-discord)
  - [2.2 创建 Bot 并获取 Token](#22-创建-bot-并获取-token)
- [3. OpenClaw 安装](#3-openclaw-安装)
  - [3.1 准备工作](#31-准备工作)
  - [3.2 更新系统](#32-更新系统)
  - [3.3 安装 Node.js 22](#33-安装-nodejs-22)
  - [3.4 配置 npm](#34-配置-npm)
  - [3.5 安装 OpenClaw](#35-安装-openclaw)
  - [3.6 运行配置向导](#36-运行配置向导)
- [4. 后续配置](#4-后续配置)
  - [4.1 链接 GitHub](#41-链接-github)
  - [4.2 安装 Anthropic 技能包](#42-安装-anthropic-技能包)
  - [4.3 使用 Bird 浏览推特](#43-使用-bird-浏览推特)
  - [4.4 安装 Chromium 浏览器](#44-安装-chromium-浏览器)
  - [4.5 记忆文件配置](#45-记忆文件配置)
  - [4.6 升级 OpenClaw](#46-升级-openclaw)

---

# 1. WSL2 安装

本文记录如何在 Windows 11 上通过 WSL2 搭建 OpenClaw 运行环境，并接入 Discord 实现 AI 助手功能。

## 1.1 为什么要装这个

| 方案 | 问题 |
|------|------|
| 腾讯云服务器 | 内存太小，经常爆掉，后台也登不进 |
| Mac mini | 手头没有 |
| Jetson NX Orin | 里面有重要环境，担心操作不当系统崩溃 |

**结论**：WSL2 是最稳妥的选择，轻量、隔离、随时可重置。

## 1.2 安装 Ubuntu 24.04

官网：https://releases.ubuntu.com/

安装版本：**Ubuntu 24.04.3 LTS (Noble Numbat)**

下载 `ubuntu-24.04.3-wsl-amd64.wsl` 文件，双击即开始自动安装。

> ⚠️ **血的教训**
>
> 我手贱点了一下 .wsl 发行包，立刻开始安装。虽然秒按 `Ctrl+C` 停掉，但之前的系统已无法启动。
>
> **原因**：这一下生成了一个幽灵 GUID，导致 WSL 识别不到原来的系统。
>
> **解决**：在注册表里手动删除该 GUID 后恢复。ChatGPT 问了半天没解决，Gemini 两句话搞定。

安装完成后，启动 WSL2：

```bash
wsl -d Ubuntu-24.04
```

按提示设置用户名和密码即可。

## 1.3 网络配置

> 💡 **提示**
>
> 宿主机（Windows）开启代理软件的 **TUN 模式**，选择规则或全局模式即可。
>
> **不要折腾手动走代理**，配置复杂且容易连不上。

---

# 2. Discord 配置（前置准备）

> 📱 **建议**：以下步骤在电脑上操作更方便。

## 2.1 注册 Discord

官网：https://discord.com/

QQ 邮箱或 +86 手机号均可注册。

## 2.2 创建 Bot 并获取 Token

1. 进入 Discord 开发者平台：https://discord.com/developers/applications

2. 点击右上角 **New Application**，输入 Bot 名称（后续可修改），同意条款后创建。

3. 左侧菜单点击 **Bot** → **Add Bot**，确认创建。

4. 点击 **Reset Token**，复制保存（后续会用到）。

5. **开启关键权限**（往下滑找到 Privileged Gateway Intents）：
   - ✅ **MESSAGE CONTENT INTENT**（必须）
   - ✅ **SERVER MEMBERS INTENT**（可选）
   
   > 记得点击底部弹出的 **Save Changes** 保存。

6. 左侧点击 **OAuth2** → **URL Generator**：
   - **Scopes** 勾选：`bot` 和 `applications.commands`
   - **Bot Permissions** 勾选：`Send Messages`、`Read Message History`、`Embed Links`、`Use Slash Commands`

7. 复制生成的 **InviteLink**，在浏览器打开，将 Bot 邀请到你的服务器并授权。

8. 回到 OpenClaw 配置向导，粘贴 Token。

> ⚠️ **重要**
>
> Token 粘贴后，**Discord Channels Access** 选择 **Open**，不要选 **Allowlist**。
>
> Allowlist 虽然标了 recommended，但后续添加多频道会非常麻烦，只能手动一个个配置。

---

# 3. OpenClaw 安装

## 3.1 准备工作

检查 Node.js 和 npm 版本：

```bash
node --version  # 需 >= v22.x.x
npm --version   # 需 >= 10.x.x
```

若版本不满足，没关系，继续看下面。

## 3.2 更新系统

```bash
sudo apt update
sudo apt upgrade -y
```

⏱️ 预计用时：2~5 分钟

## 3.3 安装 Node.js 22

由于网络问题（前面踩的坑，用 TUN 解决就好），curl 方式一直失败，最终采用离线安装：

**Windows 下载** → **传至 WSL2** → **解压安装**

下载地址：https://nodejs.org/dist/v22.13.1/node-v22.13.1-linux-x64.tar.xz

```bash
# 解压到指定目录
tar -xf node-v22.13.1-linux-x64.tar.xz -C ~/.local/

# 添加到 PATH（根据实际路径调整）
echo 'export PATH="$HOME/.local/node-v22.13.1-linux-x64/bin:$PATH"' >> ~/.bashrc

source ~/.bashrc
```

> 💡 网络正常的话，直接用官方脚本更简单：
> ```bash
> curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -
> sudo apt-get install -y nodejs
> ```

安装后再次检查版本确认成功。

## 3.4 配置 npm

解决全局安装时的权限问题：

```bash
# 创建专用全局目录
mkdir -p ~/.npm-global

# 配置 npm 前缀
npm config set prefix '~/.npm-global'

# 添加到 PATH
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc

source ~/.bashrc
```

## 3.5 安装 OpenClaw

```bash
npm install -g openclaw@latest
```

⏱️ 预计用时：2~5 分钟

## 3.6 运行配置向导

```bash
# 引导配置 + 安装开机自启服务
openclaw onboard --install-daemon
```

常用 daemon 命令：
```bash
openclaw daemon start    # 启动
openclaw daemon status   # 查看状态
openclaw daemon restart  # 重启
```

> 💡 **daemon 的作用**：让 OpenClaw 后台运行，关闭终端后仍能响应消息。

**配置向导流程**：

| 步骤 | 选项 | 说明 |
|------|------|------|
| 1 | 免责声明 | 输入 `Yes` |
| 2 | 配置模式 | **Quick Start**（快速入门） |
| 3 | 处理方式 | **Reset**（首次安装） |
| 4 | 大模型 | **Moonshot AI** → **Kimi Code** → `k2p5` |
| 5 | Channel | **Discord**（多频道 = 多 session） |
| 6 | 安装 Skills | **No**（后续手动装） |
| 7 | 启用 Hooks | 只选 **Session Memory**（空格选，回车确认） |
| 8 | 服务已安装 | **Restart** |
| 9 | Hatch 方式 | **Hatch in TUI** |

进入 TUI 后，进行初始化人设设置：

> 你叫蛋黄，是我的 AI 助手。我是你爸爸。你被设计成比较健谈和主动的风格，不是那种严肃的工具型 AI。你会结合之前的对话历史和记忆文件来回复，主动分析问题并提供建议，而不只是被动回答。你喜欢用 emoji，解释思路过程，并给出多个角度的信息。

---

# 4. 后续配置（装修毛坯房）

以下是 OpenClaw 安装完成后可进一步配置的功能：

## 4.1 链接 GitHub

配置 GitHub Token，实现代码仓库的自动推送/拉取。

## 4.2 安装 Anthropic 技能包

安装官方提供的文档处理、设计、开发等增强技能。

## 4.3 使用 Bird 浏览推特

通过 `bird` 技能在命令行浏览和操作 Twitter/X。

## 4.4 安装 Chromium 浏览器

配置 headless 浏览器，实现网页截图、自动化测试等功能。

## 4.5 记忆文件配置

设置 MEMORY.md 和每日记忆文件，让 AI 保持长期上下文。

## 4.6 升级 OpenClaw

后续版本更新方法和注意事项。
