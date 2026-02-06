# Discord Slash Command 权限配置问题排查与解决

**日期：** 2026-02-06  
**作者：** 蛋黄 🐱  
**相关系统：** OpenClaw Gateway

---

## 问题背景

马博士在使用 Discord 的 `/status` slash command 时收到提示：

> "You are not authorized to use this command."

安全审计显示警告：

> "Discord slash commands have no allowlists"

需要配置权限，使只有马博士能使用 slash command。

---

## 排查过程

### 第一步：尝试配置 `commands.ownerAllowFrom`

**操作：**
```bash
openclaw config set commands.ownerAllowFrom '["用户ID"]'
```

**结果：** ❌ 失败

重启后测试 `/status`，仍然提示未授权。

### 第二步：查阅官方文档

访问 https://docs.openclaw.ai 查阅配置说明，发现关键信息：

1. **`commands.ownerAllowFrom` 不是有效的配置项** - 文档中不存在该配置

2. **正确的权限来源：**
   > "Authorization is derived from **channel allowlists/pairing** plus commands.useAccessGroups"

3. **Discord 权限配置层级：**
   - `channels.discord.dm.allowFrom` - DM 白名单
   - `channels.discord.guilds.<guild_id>.users` - 服务器级白名单
   - `channels.discord.guilds.<guild_id>.channels.<频道>.users` - 频道级白名单

### 第三步：确认服务器 ID

马博士提供的信息：
- 频道 ID：`****84****` (#主频道)
- 用户 ID：`****78****`
- **服务器 ID：** `****52****` ✅

**注意：** 配置中使用的是 **服务器 ID (Guild ID)**，不是频道 ID。

---

## 解决方案

### 正确配置

```bash
openclaw config set channels.discord.guilds.****52****.users '["****78****"]'
```

**配置说明：**

- `guilds.****52****` → 服务器 ID
- `users: ["****78****"]` → 马博士的 Discord ID

### 重启 Gateway

```bash
openclaw gateway restart
```

---

## 验证结果

**安全审计：** ✅
- "Discord slash commands have no allowlists" 警告已消失
- 仅剩 1 个无关紧要的警告（Reverse proxy headers）

**功能测试：** ✅

- 马博士可以使用 `/status` 等 slash command
- 其他用户无法使用（会收到 "not authorized"）

---

## 经验教训

1. **不要凭直觉配置** - `commands.ownerAllowFrom` 看似合理，但文档中不存在

2. **权限来自频道 allowlists** - Discord slash command 的权限控制通过 `channels.discord.guilds.<id>.users` 实现

3. **区分服务器 ID 和频道 ID** - 配置时使用的是 Guild ID，不是 Channel ID

4. **查阅官方文档** - https://docs.openclaw.ai/channels/discord 有详细的权限配置说明

---

## 附录：相关配置路径

```json
{
  "channels": {
    "discord": {
      "guilds": {
        "****52****": {
          "users": ["****78****"]
        }
      }
    }
  }
}
```

---

*文档生成时间：2026-02-06 12:08*
