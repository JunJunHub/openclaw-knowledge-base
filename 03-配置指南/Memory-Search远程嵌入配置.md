# Memory Search 远程嵌入配置

## 问题背景

虚拟机环境运行本地嵌入模型（node-llama-cpp）性能不足，导致：
- 模型下载超时（300秒）
- 嵌入生成缓慢

## 解决方案

使用 SiliconFlow 远程嵌入服务替代本地模型。

## 配置示例（openclaw.json）

```json
{
  "memorySearch": {
    "enabled": true,
    "provider": "openai",
    "model": "BAAI/bge-m3",
    "mode": "hybrid",
    "remote": {
      "baseUrl": "https://api.siliconflow.cn/v1",
      "apiKey": "{{SILICONFLOW_API_KEY}}"
    }
  }
}
```

## 模型选择

| 模型 | Max Tokens | 特点 |
|------|-----------|------|
| `BAAI/bge-large-zh-v1.5` | 512 | ❌ 中文，但 tokens 不足 |
| `BAAI/bge-m3` | 8192 | ✅ 推荐，支持长文本 |

## 常用命令

```bash
# 检查索引状态
openclaw memory status

# 重建索引
openclaw memory index --force

# 测试搜索
openclaw memory search "OpenClaw 配置"
```

## 踩坑记录

1. **bge-large-zh-v1.5 只支持 512 tokens**
   - 错误: `must have less than 512 tokens`
   - 解决: 改用 `BAAI/bge-m3`（8192 tokens）

2. **node-llama-cpp 本地嵌入超时**
   - 原因: 虚拟机性能不足
   - 解决: 改用远程嵌入服务

## 更新日期

2026-03-24
