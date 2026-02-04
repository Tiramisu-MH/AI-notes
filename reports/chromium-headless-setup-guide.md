# Chromium 无头浏览器配置完全指南

> 基于 OpenClaw 框架的无头浏览器部署实践报告  
> 作者：蛋黄 (Yolk) 🐱  
> 生成时间：2026-02-04  
> 适用平台：Linux (WSL2) 

---

## 📋 目录

1. [概述](#概述)
2. [安装过程](#安装过程)
3. [配置详解](#配置详解)
4. [踩坑记录](#踩坑记录)
5. [崩溃教训](#崩溃教训)
6. [最佳实践](#最佳实践)
7. [故障排查速查表](#故障排查速查表)

---

## 概述

### 什么是无头浏览器？

无头浏览器（Headless Browser）是一种没有图形用户界面的浏览器，在后台运行，通过编程接口（如 CDP - Chrome DevTools Protocol）进行控制。适用于：

- 🤖 自动化测试
- 🕷️ 网页爬取
- 📸 页面截图/PDF 生成
- 🔍 SEO 分析
- 🤖 AI Agent 网页交互

### OpenClaw 的浏览器方案

OpenClaw 提供两种浏览器控制模式：

| 模式 | 说明 | 适用场景 |
|------|------|----------|
| **openclaw** | 独立管理的 Chromium 实例 | 需要完全隔离、无头模式 |
| **chrome** | 通过扩展控制现有 Chrome | 需要与日常浏览器共用 |

---

## 安装过程

### 1. 系统要求检查

```bash
# 检查系统架构
uname -m  # x86_64 或 arm64

# 检查内存（建议至少 2GB）
free -h

# 检查磁盘空间（建议至少 1GB 可用）
df -h
```

### 2. Chromium 安装方式

#### Linux (Ubuntu/WSL2)

```bash
# 方式 1：通过 apt 安装（推荐，稳定版）
sudo apt update
sudo apt install -y chromium-browser

# 验证安装
chromium-browser --version

# 方式 2：通过 snap 安装（较新版本）
sudo snap install chromium

# Warning：不要安装 snap 版本的 Chromium！！！ 

# 方式 3：Playwright 自带 Chromium
Playwright 会自动下载兼容的 Chromium 版本
```

### 3. 依赖安装（Linux 关键步骤）

**⚠️ 这是最容易出错的环节！**

Chromium 在 Linux 上需要大量共享库，缺少任何一个都会导致启动失败：

```bash
# Ubuntu/Debian 完整依赖
sudo apt install -y \
    libnss3 \
    libatk-bridge2.0-0 \
    libxcomposite1 \
    libxdamage1 \
    libxrandr2 \
    libgbm1 \
    libasound2 \
    libpangocairo-1.0-0 \
    libgtk-3-0
```

---

## 配置详解

### OpenClaw 配置文件

配置文件路径：`~/.openclaw/openclaw.json`

### 基础配置（无头模式）

```json
{
  "browser": {
    "enabled": true,
    "headless": true,
    "defaultProfile": "openclaw",
    "color": "#FF4500",
    "noSandbox": true,
    "attachOnly": false,
    "executablePath": "/usr/bin/chromium-browser",
    "remoteCdpTimeoutMs": 1500,
    "remoteCdpHandshakeTimeoutMs": 3000,
    "profiles": {
      "openclaw": {
        "cdpPort": 18800,
        "color": "#FF4500"
      }
    }
  }
}
```

### 配置项详解

| 配置项 | 类型 | 默认值 | 说明 |
|--------|------|--------|------|
| `enabled` | boolean | true | 启用浏览器控制服务 |
| `headless` | boolean | true | **无头模式开关** - true = 无界面运行 |
| `defaultProfile` | string | "openclaw" | 默认使用的配置文件 |
| `executablePath` | string | 自动检测 | Chromium 可执行文件路径 |
| `noSandbox` | boolean | true | 禁用沙盒（容器环境常用）|
| `attachOnly` | boolean | false | 只连接已运行的浏览器 |
| `cdpPort` | number | 18800+ | Chrome DevTools Protocol 端口 |

### 多配置文件示例

```json
{
  "browser": {
    "enabled": true,
    "headless": true,
    "defaultProfile": "production",
    "profiles": {
      "development": {
        "cdpPort": 18800,
        "color": "#00AA00",
        "headless": false
      },
      "production": {
        "cdpPort": 18801,
        "color": "#FF4500",
        "headless": true
      },
      "remote": {
        "cdpUrl": "http://10.0.0.42:9222",
        "color": "#0066CC"
      }
    }
  }
}
```

### 常用命令

```bash
# 查看浏览器状态
openclaw browser --browser-profile openclaw status

# 启动浏览器
openclaw browser --browser-profile openclaw start

# 打开网页
openclaw browser --browser-profile openclaw open https://example.com

# 获取页面快照
openclaw browser --browser-profile openclaw snapshot

# 截图
openclaw browser --browser-profile openclaw screenshot

# 生成 PDF
openclaw browser --browser-profile openclaw pdf --target-url https://example.com

# 停止浏览器
openclaw browser --browser-profile openclaw stop
```

---

## 踩坑记录

### 🕳️ 坑 1：缺少共享库（最常见）

**现象：**
```
error while loading shared libraries: libnss3.so: cannot open shared object file
```

**解决：**
```bash
# Ubuntu/Debian
sudo apt install -y libnss3 libatk-bridge2.0-0 libxcomposite1

# 使用 ldd 检查缺失的库
ldd $(which chromium-browser) | grep not
```

---

### 🕳️ 坑 2：沙盒权限问题

**现象：**
```
[ERROR:zygote_linux.cc(661)] write: Broken pipe (32)
[ERROR:broker_posix.cc(43)] Invalid node channel message
```

**解决：**

方式 1 - 禁用沙盒（容器/无特权环境）：
```json
{
  "browser": {
    "noSandbox": true
  }
}
```

方式 2 - 正确配置用户命名空间：
```bash
# 启用用户命名空间
sudo sysctl -w kernel.unprivileged_userns_clone=1

# 永久生效
echo 'kernel.unprivileged_userns_clone=1' | sudo tee /etc/sysctl.d/00-local-userns.conf
```

---

### 🕳️ 坑 3：WSL2 显示问题

**现象：**
```
[ERROR:gpu_init.cc(457)] Passthrough is not supported, GL is disabled
```

**解决：**
```bash
# 安装缺失的库
sudo apt install -y libgl1-mesa-glx libglib2.0-0

# 如果不需要 GPU 加速，无头模式可忽略此警告
# 或在配置中禁用 GPU
```

---

### 🕳️ 坑 4：端口冲突

**现象：**
```
Error: listen EADDRINUSE: address already in use :::18800
```

**解决：**
```bash
# 查找占用端口的进程
sudo lsof -i :18800

# 或更换配置文件中的 cdpPort
# "cdpPort": 18801
```

---

### 🕳️ 坑 5：Playwright 版本不匹配

**现象：**
```
browserType.launch: Executable doesn't exist at /path/to/chromium
```

**解决：**
```bash
# 重新安装 Playwright 浏览器
playwright install --with-deps chromium

# 或手动指定正确的路径
which chromium-browser
# 然后将路径写入配置
```

---

### 🕳️ 坑 6：中文显示为方块

**现象：**
截图中的中文显示为 `□` 或乱码

**解决：**
```bash
# 安装中文字体
sudo apt install -y fonts-wqy-zenhei fonts-wqy-microhei

# 或安装完整字体包
sudo apt install -y ttf-wqy-zenhei xfonts-wqy
```

---

### 🕳️ 坑 7：时区问题

**现象：**
浏览器显示的日期时间不正确

**解决：**
```bash
# 设置系统时区
sudo timedatectl set-timezone Asia/Shanghai

# 或在启动参数中添加
--timezone=Asia/Shanghai
```

---

## 最佳实践

### ✅ 配置建议

```json
{
  "browser": {
    "enabled": true,
    "headless": true,
    "noSandbox": true,
    "defaultProfile": "production",
    "executablePath": "/usr/bin/chromium-browser",
    "launchOptions": {
      "args": [
        "--disable-dev-shm-usage",
        "--disable-gpu",
        "--no-first-run",
        "--no-zygote",
        "--disable-background-networking",
        "--disable-background-timer-throttling",
        "--disable-backgrounding-occluded-windows",
        "--disable-breakpad",
        "--disable-client-side-phishing-detection",
        "--disable-component-update",
        "--disable-default-apps",
        "--disable-features=TranslateUI",
        "--disable-hang-monitor",
        "--disable-ipc-flooding-protection",
        "--disable-popup-blocking",
        "--disable-prompt-on-repost",
        "--disable-renderer-backgrounding",
        "--force-color-profile=srgb",
        "--metrics-recording-only",
        "--safebrowsing-disable-auto-update"
      ]
    },
    "profiles": {
      "production": {
        "cdpPort": 18800,
        "color": "#FF4500"
      }
    }
  }
}
```

### ✅ 启动检查清单

- [ ] Chromium 已正确安装并可执行
- [ ] 所有依赖库已安装（使用 `ldd` 检查）
- [ ] 配置文件路径正确且 JSON 格式有效
- [ ] 端口未被占用
- [ ] 有足够的内存（>512MB）
- [ ] 有足够的磁盘空间（>1GB）
- [ ] 系统时区设置正确
- [ ] 中文字体已安装（如需显示中文）

### ✅ 性能优化

1. **复用浏览器实例**：避免频繁启动/关闭
2. **使用无头模式**：减少资源消耗
3. **禁用不必要的功能**：如 GPU、沙盒（在受控环境）
4. **限制并发页面数**：避免内存溢出
5. **定期重启**：防止内存泄漏

---

## 故障排查速查表

| 问题 | 检查项 | 解决方案 |
|------|--------|----------|
| 无法启动 | 依赖库 | `ldd` 检查并安装缺失库 |
| 权限错误 | 沙盒 | 设置 `noSandbox: true` |
| 端口占用 | 端口冲突 | 更换端口或杀死占用进程 |
| 中文乱码 | 字体 | 安装中文字体包 |
| 内存不足 | 内存 | 添加 `--disable-dev-shm-usage` |
| 页面崩溃 | GPU | 添加 `--disable-gpu` |
| 证书错误 | SSL | 设置 `ignoreHTTPSErrors: true` |
| 进程堆积 | 僵尸进程 | 使用 `tini` 或定期清理 |

---

## 参考链接

- [OpenClaw 官方文档 - Browser](https://docs.openclaw.ai/tools/browser)
- [Chromium 官方文档](https://www.chromium.org/developers/)
- [Playwright 文档](https://playwright.dev/)
- [Puppeteer 故障排查](https://pptr.dev/troubleshooting)

---

> 🐱 **蛋黄的话**：无头浏览器配置虽然坑多，但只要按部就班、仔细检查依赖，大部分问题都能迎刃而解。记住：**ldd 是你的好朋友，沙盒是你的双刃剑**。

---

*报告生成时间：2026-02-04*  
*适用 OpenClaw 版本：latest*  
*最后更新：2026-02-04*
