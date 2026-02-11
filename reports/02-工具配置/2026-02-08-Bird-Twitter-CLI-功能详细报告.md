# Bird (X/Twitter CLI) 功能详细报告

> Twitter/X 命令行客户端完整功能清单与使用限制

---

## 1. 账号与认证

| 命令 | 功能 | 限制/备注 |
|------|------|----------|
| `bird whoami` | 显示当前登录账号信息 | 需先配置 cookies |
| `bird check` | 检查认证状态和数据源 | 显示可用的 cookie 来源 |
| `bird query-ids --fresh` | 刷新 GraphQL Query ID 缓存 | 遇到 404 错误时执行 |

**⚠️ 认证要求**：必须使用 `--auth-token` 和 `--ct0` 参数，或配置 `--cookie-source` 从浏览器获取 cookies。

---

## 2. 阅读推文

| 命令 | 功能 | 限制/备注 |
|------|------|----------|
| `bird read <url-or-id>` | 读取单条推文详情 | 支持链接或推文ID |
| `bird <url-or-id>` | `read` 的简写形式 | 同上 |
| `bird thread <url-or-id>` | 获取完整对话线程 | 支持分页 |
| `bird replies <url-or-id>` | 列出推文的回复 | 支持分页，可能较多 |

---

## 3. 时间线

| 命令 | 功能 | 限制/备注 |
|------|------|----------|
| `bird home` | For You 主页时间线 | 默认约 10-20 条 |
| `bird home --following` | 关注的人时间线 | 同上 |
| `bird user-tweets @handle -n 20` | 指定用户的时间线 | 支持分页 |
| `bird mentions` | 提到你的推文 | 支持分页 |
| `bird mentions --user @handle` | 提到指定用户的推文 | 同上 |

---

## 4. 搜索功能

| 命令 | 功能 | 限制/备注 |
|------|------|----------|
| `bird search "query" -n 10` | 关键词搜索 | 支持分页 |
| `bird search "from:username" --all --max-pages 3` | 搜索特定用户推文 | `--max-pages` 限制页数 |

---

## 5. 新闻与趋势

| 命令 | 功能 | 限制/备注 |
|------|------|----------|
| `bird news -n 10` | AI 精选新闻（Explore 标签） | 聚合多标签内容 |
| `bird news --ai-only` | 仅显示 AI 精选内容 | 过滤其他内容 |
| `bird news --sports` | 体育标签内容 | 特定分类 |
| `bird news --with-tweets` | 包含相关推文 | 内容更丰富 |
| `bird trending` | 热门趋势 | `news` 的别名 |

---

## 6. 列表功能

| 命令 | 功能 | 限制/备注 |
|------|------|----------|
| `bird lists` | 查看你创建的列表 | 无 |
| `bird lists --member-of` | 查看你加入的列表 | 无 |
| `bird list-timeline <id> -n 20` | 查看列表的时间线 | 支持分页 |

---

## 7. 书签与点赞

| 命令 | 功能 | 限制/备注 |
|------|------|----------|
| `bird bookmarks -n 10` | 查看你的书签 | 支持分页 |
| `bird bookmarks --folder-id <id>` | 查看指定书签文件夹 | 需文件夹ID |
| `bird bookmarks --include-parent` | 包含父推文 | 显示上下文 |
| `bird bookmarks --author-chain` | 显示作者自回复链 | 适合看线程 |
| `bird bookmarks --full-chain-only` | 仅显示完整回复链 | 过滤单条 |
| `bird unbookmark <url-or-id>` | 取消书签 | 单条操作 |
| `bird likes -n 10` | 查看你点赞的推文 | 支持分页 |

---

## 8. 社交关系

| 命令 | 功能 | 限制/备注 |
|------|------|----------|
| `bird following -n 20` | 查看你关注的人 | 支持分页 |
| `bird followers -n 20` | 查看你的粉丝 | 支持分页 |
| `bird following --user <id>` | 查看指定用户的关注 | 支持分页 |
| `bird about @handle` | 查看账号信息（位置、简介等） | 无 |
| `bird follow @handle` | 关注用户 | 有操作频率限制 |
| `bird unfollow @handle` | 取消关注 | 同上 |

---

## 9. 发推与互动 ⚠️

| 命令 | 功能 | 限制/备注 |
|------|------|----------|
| `bird tweet "内容"` | 发布推文 | **⚠️ 容易被限流** |
| `bird reply <url-or-id> "内容"` | 回复推文 | **⚠️ 容易被限流** |
| `bird tweet "内容" --media img.png` | 带图片发推 | 最多 4 张图 |
| `bird tweet "内容" --media clip.mp4` | 带视频发推 | 仅 1 个视频 |
| `--alt "描述"` | 图片替代文本 | 无障碍支持 |

**⚠️ 重要警告**：发推/回复操作更容易触发 Twitter 的速率限制，如果被 block 了，建议改用浏览器工具。

---

## 10. 分页控制

支持分页的命令：`replies`, `thread`, `search`, `bookmarks`, `likes`, `list-timeline`, `following`, `followers`, `user-tweets`

| 参数 | 功能 | 示例 |
|------|------|------|
| `--all` | 获取所有页面 | `bird bookmarks --all` |
| `--max-pages 3` | 限制最大页数 | `bird search "xxx" --max-pages 3` |
| `--cursor <cursor>` | 从指定位置继续 | 用于恢复获取 |
| `--delay 1000` | 页面间延迟(毫秒) | 防 rate limit |

---

## 11. 输出格式选项

| 参数 | 功能 |
|------|------|
| `--json` | JSON 格式输出（适合程序处理） |
| `--json-full` | JSON + 原始 API 响应 |
| `--plain` | 纯文本，无颜色/emoji（节省字符） |
| `--no-emoji` | 禁用 emoji |
| `--no-color` | 禁用 ANSI 颜色 |
| `--quote-depth n` | 引用推文最大深度（默认 1） |

---

## 12. 全局参数

| 参数 | 功能 |
|------|------|
| `--auth-token <token>` | 设置 auth_token cookie |
| `--ct0 <token>` | 设置 ct0 cookie |
| `--cookie-source <source>` | Cookie 来源（可重复） |
| `--chrome-profile <name>` | Chrome 配置文件名 |
| `--chrome-profile-dir <path>` | Chrome 配置目录路径 |
| `--firefox-profile <name>` | Firefox 配置文件名 |
| `--timeout <ms>` | 请求超时时间 |
| `--cookie-timeout <ms>` | Cookie 提取超时 |

---

## 13. 配置方式

**环境变量**：
```bash
export AUTH_TOKEN="..."
export CT0="..."
```

**配置文件**（`~/.config/bird/config.json5`）：
```json5
{
  authToken: "...",
  ct0: "...",
  cookieSource: ["chrome"],
  timeoutMs: 20000,
}
```

**环境变量覆盖**：
- `BIRD_TIMEOUT_MS`
- `BIRD_COOKIE_TIMEOUT_MS`
- `BIRD_QUOTE_DEPTH`

---

## 14. 使用限制总结

| 限制类型 | 说明 |
|----------|------|
| **认证** | 必须提供有效的 Twitter cookies |
| **发帖限制** | 发推/回复更容易被 rate limit |
| **Cookie 过期** | 需要定期更新 cookies |
| **Query ID 过期** | 出现 404 时运行 `bird query-ids --fresh` |
| **浏览器兼容** | Arc/Brave 需指定 `--chrome-profile-dir` |
| **分页延迟** | 大量获取时建议加 `--delay` 参数 |

---

## 15. 故障排除

### 15.1 Query ID 过期（404错误）
```bash
bird query-ids --fresh
```

### 15.2 认证失败
- 检查 cookies 是否过期
- 重新从浏览器获取 auth_token 和 ct0
- 确保环境变量已正确设置

### 15.3 Cookie 提取失败
- 检查浏览器是否已登录 Twitter
- 尝试不同的 `--cookie-source`
- Arc/Brave 用户需使用 `--chrome-profile-dir`

---

*报告生成时间: 2026-02-08 21:27 CST Asia/Shanghai*
