# Web Search 搜索配置

## Provider 对比

| Provider | 免费额度 | 特点 | 推荐场景 |
|----------|---------|------|---------|
| **Tavily** | 1,000次/月 | AI 优化结果，结构化 | 默认推荐 |
| **Exa** | 1,000次/月 | 神经网络搜索，语义理解 | 备用 |
| **DuckDuckGo** | 无限 | 免费，无需 Key | 测试/备用 |
| **Brave** | 2,000次/月 | 稳定可靠 | 通用搜索 |

## 配置示例（openclaw.json）

```json
{
  "tools": {
    "web": {
      "search": {
        "enabled": true,
        "provider": "tavily"
      }
    }
  },
  "plugins": {
    "entries": {
      "tavily": {
        "enabled": true,
        "config": {
          "webSearch": {
            "apiKey": "{{TAVILY_API_KEY}}"
          }
        }
      },
      "exa": {
        "enabled": true,
        "config": {
          "webSearch": {
            "apiKey": "{{EXA_API_KEY}}"
          }
        }
      }
    }
  }
}
```

## 环境变量（~/.openclaw/.env）

```bash
TAVILY_API_KEY=tvly-your-tavily-key
EXA_API_KEY=your-exa-key
```

## 可用工具

| 工具 | 说明 |
|------|------|
| `web_search` | 通用搜索（自动使用配置的 provider） |
| `tavily_search` | Tavily 专用（支持深度/话题过滤） |
| `tavily_extract` | URL 内容提取 |
| `exa_search` | Exa 专用（语义搜索） |

## Tavily 高级用法

```bash
# 深度搜索
tavily_search(query="...", search_depth="advanced")

# 新闻搜索
tavily_search(query="...", topic="news")

# 时间范围
tavily_search(query="...", time_range="week")
```

## 获取 API Key

- **Tavily**: https://tavily.com（支持国内网络）
- **Exa**: https://exa.ai

## 更新日期

2026-03-24
