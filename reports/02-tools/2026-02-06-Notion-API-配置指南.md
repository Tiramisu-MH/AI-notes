# Notion API 配置指南

**日期：** 2026-02-06  
**作者：** 蛋黄 🐱  
**适用场景：** OpenClaw 集成 Notion 数据库操作

---

## 概述

本指南介绍如何在 OpenClaw 中配置 Notion API，实现对 Notion 数据库的读写操作，包括记录支出、查询数据等功能。

---

## 第一步：创建 Notion Integration

### 1.1 访问 Notion 开发者页面

打开浏览器访问：
> https://www.notion.so/my-integrations

### 1.2 创建新集成

1. 点击 **"+ New integration"**
2. 填写基本信息：
   - **Name**：`OpenClaw Integration`（或其他你喜欢的名字）
   - **Associated workspace**：选择你的工作区
   - **Type**：选择 **Internal**（推荐，仅限你自己使用）

3. 点击 **"Submit"**

### 1.3 获取 API Key

创建成功后，进入集成详情页面：

1. 点击 **"Show"** 显示 Secret 令牌
2. 复制这串字符（格式如 `secret_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`）
3. **妥善保存**，关闭页面后将无法再次查看

> ⚠️ **重要**：Internal 类型的集成更安全，仅限你个人使用，不需要 OAuth 流程。

---

## 第二步：保存 API Key 到 OpenClaw

### 2.1 创建配置文件目录

```bash
mkdir -p ~/.config/notion
```

### 2.2 保存 API Key

```bash
echo "你的_Notion_API_Key" > ~/.config/notion/api_key
```

或直接编辑文件：
```bash
nano ~/.config/notion/api_key
# 粘贴 API Key，保存退出
```

### 2.3 验证文件权限

确保文件权限正确：
```bash
chmod 600 ~/.config/notion/api_key
```

---

## 第三步：授权 Notion 页面访问

集成创建后，需要手动授权它访问特定的 Notion 页面/数据库。

### 3.1 授权数据库

1. 打开你想让 OpenClaw 访问的 Notion 数据库页面
2. 点击页面右上角的 **"..."**（更多选项）
3. 选择 **"Connect to"** 或 **"添加连接"**
4. 搜索并选择你刚才创建的 Integration（如 `OpenClaw Integration`）
5. 点击 **"Confirm"** 或 **"确认"**

### 3.2 验证授权

授权成功后，在集成详情页面的 **"Capabilities"** 标签下，应该能看到已连接的页面列表。

---

## 第四步：测试连接

### 4.1 使用 notion skill 测试

在 OpenClaw 中加载 notion skill，然后执行查询：

```
查询我的 Notion 数据库
```

或具体查询某个数据库：

```
列出 Notion 中可访问的数据库
```

### 4.2 添加测试记录

尝试添加一条记录验证写入权限：

```
在"家庭二月开支"数据库中添加一笔记录：
- 金额：¥99
- 类别：软件订阅
- 项目：Kimi-Vip
- 支付方式：平安口袋银行
```

---

## 常见问题

### Q1: 为什么提示 "找不到数据库"？

**原因**：没有给 Integration 授权访问该页面。

**解决**：
1. 打开 Notion 数据库页面
2. 点击 "..." → "Connect to"
3. 选择你的 Integration 并确认

### Q2: API Key 保存在哪里？

**位置**：`~/.config/notion/api_key`

所有 OpenClaw session 共享此配置，只要加载 notion skill 就能使用。

### Q3: 可以访问哪些类型的页面？

- ✅ 数据库（Database）- 支持增删改查
- ✅ 普通页面（Page）- 支持读取和编辑内容
- ❌ 工作区所有页面 - 必须逐个授权

### Q4: Internal 和 Public 集成有什么区别？

| 类型 | 适用场景 | 特点 |
|------|----------|------|
| **Internal** | 个人使用 | 无需审核，仅自己使用，更安全 |
| **Public** | 公开发布 | 需要 OAuth，其他用户可安装 |

对于 OpenClaw 个人使用，**强烈推荐使用 Internal**。

---

## 安全配置检查清单

- [ ] 创建 Internal 类型集成（非 Public）
- [ ] API Key 保存在 `~/.config/notion/api_key`
- [ ] 文件权限设置为 600（仅自己可读）
- [ ] 只授权必要的 Notion 页面
- [ ] 定期轮换 API Key（如泄露风险）

---

## 相关链接

- Notion 开发者中心：https://www.notion.so/my-integrations
- Notion API 文档：https://developers.notion.com/
- OpenClaw Notion Skill 文档：使用 `openclaw skill notion --help` 查看

---

## 附录：示例配置结构

```
~/.config/
└── notion/
    └── api_key          # 存储 Notion API Key
```

---

*报告生成时间: 2026-02-06 12:23 CST Asia/Shanghai*
