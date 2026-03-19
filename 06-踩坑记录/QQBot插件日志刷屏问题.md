---
title: QQBot 插件日志刷屏问题
date: 2026-03-20
tags: [openclaw, 插件, qqbot, 日志]
category: 踩坑记录
source: memory/2026-03-19.md
status: final
---

# 问题

QQBot 插件产生大量日志，导致日志刷屏，影响其他日志查看。

# 解决

在 `~/.openclaw/openclaw.json` 中禁用 qqbot 插件：

```json
{
  "plugins": {
    "entries": {
      "qqbot": {
        "enabled": false
      }
    }
  }
}
```

# 效果

禁用后日志刷屏问题解决，系统运行正常。

# 相关

- [[OpenClaw 插件配置]]
- [[日志管理]]
