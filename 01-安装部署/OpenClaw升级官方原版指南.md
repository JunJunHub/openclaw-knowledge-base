# OpenClaw 升级官方原版指南

## 版本对比

| 项目 | 社区版 | 官方原版 |
|------|--------|----------|
| 包名 | `openclaw-cn` | `openclaw` |
| 版本 | `0.1.8-fix.3` | `2026.3.23-2` |
| Chrome MCP | 仅 headless | 支持 existing-session |
| Serper 插件 | ✅ 可用 | ❌ 不兼容 |

## 升级步骤

```bash
# 1. 卸载社区版
npm uninstall -g openclaw-cn

# 2. 安装官方原版
npm install -g openclaw

# 3. 验证版本
openclaw --version
# 输出: OpenClaw 2026.3.23-2 (7ffe7e4)
```

## 兼容性注意事项

### 需要调整的配置

1. **Serper 插件** → 改用 Tavily/Exa 搜索
2. **Chrome MCP** → 配置 `existing-session` 或 `user`
3. **Plugin SDK** → 使用 `openclaw/plugin-sdk/*` 新路径

### 保持兼容的配置

- 飞书插件 ✅
- 百度千帆模型 ✅
- SiliconFlow 模型 ✅
- Memory Search ✅

## 相关链接

- 官方文档: https://docs.openclaw.ai
- GitHub: https://github.com/openclaw/openclaw
- 社区: https://discord.com/invite/clawd

## 更新日期

2026-03-24
