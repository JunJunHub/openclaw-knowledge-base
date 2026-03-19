---
title: 飞书多Agent路由配置
date: 2026-03-20
tags: [openclaw, 飞书, agent, binding, 路由]
category: 配置指南
source: memory/2026-03-19.md
status: final
---

# 概述

通过 Binding 机制，实现一个飞书机器人连接多个 Agent，每个群组对应不同的 Agent。

# 配置步骤

## 1. 定义多个 Agent

在 `~/.openclaw/openclaw.json` 中：

```json
{
  "agents": {
    "list": [
      {
        "id": "main",
        "name": "MoltBot",
        "default": true
      },
      {
        "id": "thinker",
        "name": "沉思"
      },
      {
        "id": "media",
        "name": "墨客"
      },
      {
        "id": "monitor",
        "name": "哨兵"
      },
      {
        "id": "coder",
        "name": "极客"
      }
    ]
  }
}
```

## 2. 配置群组路由

```json
{
  "bindings": [
    {
      "agentId": "thinker",
      "match": {
        "channel": "feishu",
        "peer": {
          "kind": "group",
          "id": "oc_xxx1"
        }
      }
    },
    {
      "agentId": "media",
      "match": {
        "channel": "feishu",
        "peer": {
          "kind": "group",
          "id": "oc_xxx2"
        }
      }
    }
  ]
}
```

## 3. 配置群组自动回复

```json
{
  "channels": {
    "feishu": {
      "groupPolicy": "open",
      "groups": {
        "oc_xxx1": {
          "requireMention": false
        },
        "oc_xxx2": {
          "requireMention": false
        }
      }
    }
  }
}
```

# 关键配置说明

| 配置项 | 作用 |
|--------|------|
| `groupPolicy: "open"` | 允许所有群组 |
| `requireMention: false` | 无需 @ 即可自动回复 |
| `bindings` | 将群组路由到指定 Agent |

# 获取群组 ID

方法一：通过飞书群设置 → 群信息 → 复制群链接，从链接中提取

方法二：让 MoltBot 在群里发送消息，从日志中查看 `chat_id`

# 权限要求

飞书应用需要以下权限：
- `im:message` - 消息管理
- `im:message.group_msg` - 群组消息
- `im:message.p2p_msg` - 私聊消息

# 测试验证

```bash
# 在各群组发送消息
测试消息

# 检查是否由对应 Agent 回复
```

# 相关

- [[Agent架构详解]]
- [[飞书群组自动回复配置]]
