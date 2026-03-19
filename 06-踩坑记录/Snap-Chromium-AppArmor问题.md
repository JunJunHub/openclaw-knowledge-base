---
title: Snap Chromium AppArmor 限制导致浏览器自动化失败
date: 2026-03-20
tags: [openclaw, 踩坑, 浏览器, chromium, apparmor]
category: 踩坑记录
source: workspace-coder/troubleshooting/browser-automation-fix-2026-03-20.md
status: final
---

# 问题

OpenClaw 浏览器控制无法启动，状态始终为 `running: false`。

# 根因

Snap 版 Chromium (`/snap/bin/chromium`) 受 AppArmor 安全策略限制，阻止第三方进程启动/监控浏览器。

# 解决

安装 Google Chrome `.deb` 版本：

```bash
wget -O /tmp/google-chrome.deb https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo dpkg -i /tmp/google-chrome.deb
```

更新配置 `~/.openclaw/openclaw.json`：

```json
{
  "browser": {
    "executablePath": "/usr/bin/google-chrome-stable"
  }
}
```

# 经验

- 避免使用 Snap 版浏览器做自动化
- `.deb` 安装的浏览器无 AppArmor 限制
- 国内可直接访问 `dl.google.com`

# 相关文档

- [[浏览器配置]]
- [[OpenClaw 安装]]
