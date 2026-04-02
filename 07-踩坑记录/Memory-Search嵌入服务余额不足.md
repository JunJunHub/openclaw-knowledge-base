# Memory Search 嵌入服务余额不足

> **分类**：踩坑记录
> **相关文档**：[[Memory-Search远程嵌入配置]]

## 问题

Memory Search 功能报错：

```
openai embeddings failed: 403 account balance insufficient
```

## 原因

SiliconFlow 账户余额不足，导致嵌入服务无法调用。

## 影响

- Memory Search 功能暂时不可用
- 无法搜索历史记忆和上下文

## 解决方案

### 短期方案

充值 SiliconFlow 账户：

1. 登录 SiliconFlow 控制台
2. 进入账户充值页面
3. 充值至少 $1 以恢复服务

### 长期方案

考虑配置本地嵌入服务（无需 API 费用）：

```json
{
  "memory": {
    "embeddings": {
      "provider": "local",
      "model": "all-MiniLM-L6-v2"
    }
  }
}
```

## 检查余额

```bash
# SiliconFlow API 查询余额
curl -H "Authorization: Bearer $SILICONFLOW_API_KEY" https://api.siliconflow.cn/v1/user/info
```

## 替代方案

如果不需要语义搜索，可以使用关键词搜索：

```bash
grep -r "关键词" ~/.openclaw/workspace/memory/
```

---

*来源：2026-04-02.md*
