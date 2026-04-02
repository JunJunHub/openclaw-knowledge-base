# Chrome MCP 浏览器配置

> **分类**：配置指南
> **相关文档**：[[Claude-Code-完整开发指南]]

## 配置日期
2026-04-02

## 配置内容

### 1. 禁用未使用的插件

```bash
openclaw config set plugins.entries.amazon-bedrock.enabled false
```

原因：未配置 AWS 凭证，占用插件槽位

### 2. 配置 Chrome DevTools MCP

**添加 `user` profile**（连接真实 Chrome 浏览器）：

```json
"browser": {
  "enabled": true,
  "executablePath": "/usr/bin/google-chrome-stable",
  "headless": false,
  "noSandbox": true,
  "defaultProfile": "clawd",
  "profiles": {
    "user": {
      "driver": "existing-session",
      "attachOnly": true,
      "color": "#00AA00"
    }
  }
}
```

### 3. Chrome 端配置

1. 打开 Chrome 浏览器
2. 访问：`chrome://inspect/#remote-debugging`
3. 勾选 **"Enable remote debugging for this browser instance"**

### 4. 验证命令

```bash
# 检查 user profile 状态
openclaw browser --browser-profile user status

# 查看当前打开的标签页
openclaw browser --browser-profile user tabs

# 截图
openclaw browser --browser-profile user screenshot
```

## 两种浏览器模式

| 模式 | Profile | 用途 |
|------|---------|------|
| OpenClaw 管理 | `clawd` 或 `openclaw` | 独立浏览器，隔离环境 |
| Chrome MCP | `user` | 连接真实 Chrome，使用已登录账号 |

## Agent 使用方式

在 browser 工具中指定 `profile="user"` 即可使用 Chrome MCP 模式：

```
browser action=snapshot profile=user
```

## 注意事项

- Chrome 需要版本 144+ 支持 `--autoConnect`
- 连接时 Chrome 会弹出确认对话框，需手动点击允许
- `chrome://` 协议页面无法通过自动化访问（安全限制）
- SVG 图标按钮可能无法通过常规自动化点击

---

*来源：2026-04-02.md*
