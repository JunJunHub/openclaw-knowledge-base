---
title: 小红书Cookie过期与浏览器配置问题
date: 2026-03-21
tags: [openclaw, 小红书, cookie, 浏览器, 踩坑]
category: 踩坑记录
source: memory/2026-03-21.md
status: final
---

# 问题

测试小红书自动发布时遇到两个问题：

## 1. Cookie 过期

用户提供 Cookie 后，访问小红书创作者平台显示未登录。

### 原因
- Cookie 可能已过期
- 小红书检测到异常登录

### 解决方案
改用手动登录方式：
```json
// openclaw.json
"browser": {
  "headless": false  // 显示浏览器窗口，手动登录
}
```

登录状态保存在：`~/.openclaw/browser/clawd/user-data`

---

## 2. `userDataDir` 配置不支持

尝试共享用户 Chrome 配置时遇到：
```
Invalid config at /home/junjun/.openclaw/openclaw.json:
- browser: Unrecognized key: "userDataDir"
```

### 原因
社区版版本落后，部分配置项不支持。

| 版本 | 支持情况 |
|------|----------|
| 原版 2026.3.13 | 可能支持 |
| 社区版 0.1.8-fix.3 | ❌ 不支持 |

### 解决方案
使用 `headless: false` + 手动登录，或使用 Chrome 扩展连接已登录标签页。

---

# 相关

- [[openclaw-cn版本差异问题]]
- [[浏览器配置]]
- [[Snap Chromium AppArmor限制]]
