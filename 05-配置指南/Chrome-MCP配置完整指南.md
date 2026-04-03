# Chrome MCP 配置完整指南

## 概述
Chrome MCP 允许 OpenClaw 连接你真实使用的 Chrome 浏览器，使用已登录的所有账号。

---

## 两种浏览器模式

| 模式 | Profile | 用途 | 场景 |
|------|---------|------|------|
| OpenClaw 管理 | `clawd` | 独立浏览器，隔离环境 | 测试、匿名操作 |
| Chrome MCP | `user` | 连接真实 Chrome，使用已登录账号 | 日常自动化、已登录服务 |

## 配置步骤

### 1. 修改 openclaw.json

```json
{
  "browser": {
    "defaultProfile": "user",
    "profiles": {
      "user": {
        "driver": "existing-session",
        "attachOnly": true,
        "color": "#00AA00"
      },
      "clawd": {
        "driver": "cdp"
      }
    }
  }
}
```

### 2. 启用 Chrome 远程调试

1. 打开 Chrome
2. 访问 `chrome://inspect/#remote-debugging`
3. 启用远程调试端口（默认 9222）

或者通过命令行启动 Chrome：
```bash
google-chrome-stable --remote-debugging-port=9222
```

### 3. 重启 Gateway

```bash
openclaw gateway restart
```

## 使用方式

```bash
# 默认使用真实 Chrome（user profile）
openclaw browser snapshot

# 明确指定 profile
openclaw browser --browser-profile user snapshot

# 使用独立浏览器
openclaw browser --browser-profile clawd snapshot
```

## 注意事项

- `chrome://` 协议页面无法自动化访问
- SVG 图标按钮（如千问发送按钮）可能无法点击
- 需手动允许 Chrome 连接确认弹窗
- 首次连接时 Chrome 会弹出确认窗口

## 一键恢复脚本更新

已更新 `openclaw-recovery` 配置模板：
- 默认 profile 从 `clawd` 改为 `user`
- 添加两种浏览器 profiles 配置

---

**来源**：`workspace/memory/2026-04-02-chrome-mcp-config.md`
**整理时间**：2026-04-03
