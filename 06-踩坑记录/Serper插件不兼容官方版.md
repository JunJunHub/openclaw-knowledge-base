# Serper 插件不兼容官方版 OpenClaw

## 问题描述

升级到官方原版 OpenClaw 后，Serper 插件无法加载：

```
[plugins] serper failed to load: Error: Cannot find module 'openclaw/plugin-sdk'
```

## 原因分析

1. 官方原版使用新的 Plugin SDK 路径：`openclaw/plugin-sdk/*`
2. 社区版 Serper 插件依赖旧的 SDK 路径
3. 官方 OpenClaw 内置不支持 Serper provider

## 解决方案

改用官方支持的搜索 Provider：**Tavily** 或 **Exa**

### 配置 Tavily

```bash
# 1. 获取 API Key
# https://tavily.com

# 2. 配置环境变量
echo 'TAVILY_API_KEY=your-key' >> ~/.openclaw/.env

# 3. 配置 openclaw.json
# 见 Web-Search搜索配置.md
```

### 清理 Serper

```bash
# 移除 Serper 扩展
rm -rf ~/.openclaw/extensions/serper
```

## 替代方案对比

| Provider | 免费额度 | 官方支持 |
|----------|---------|---------|
| Tavily | 1,000次/月 | ✅ |
| Exa | 1,000次/月 | ✅ |
| DuckDuckGo | 无限 | ✅ |
| Serper | 2,500次/月 | ❌ 社区版 |

## 更新日期

2026-03-24
