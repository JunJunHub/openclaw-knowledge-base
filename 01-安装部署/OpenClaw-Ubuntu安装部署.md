---
title: OpenClaw Ubuntu 安装部署
date: 2026-03-20
tags: [openclaw, 安装, ubuntu, 部署]
category: 安装部署
source: memory/2026-03-12.md
status: final
---

# 系统要求

- Ubuntu 22.04 LTS
- Node.js 18+ (推荐 24.x)
- Python 3.8+
- 至少 2GB 内存

# 安装步骤

## 1. 安装 Node.js

```bash
# 使用 nvm 安装
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash
source ~/.bashrc
nvm install 24
nvm use 24
```

## 2. 安装 OpenClaw

```bash
npm install -g openclaw-cn
# 或
npm install -g openclaw
```

## 3. 初始化配置

```bash
openclaw onboard
```

按提示完成基础配置。

## 4. 启动 Gateway

```bash
openclaw gateway start
```

# 中文输入法

虚拟机 Ubuntu 默认无中文输入，需手动安装：

```bash
# 安装 IBus 智能拼音
sudo apt install ibus-libpinyin

# 重启 IBus
ibus restart
```

配置步骤：
1. 右键托盘 → 首选项 → 输入法 → 添加
2. 选择「中文」→「智能拼音」
3. 切换快捷键：`Super+Space`

# 开发工具

## Claude Code CLI

Anthropic 官方编程助手：

```bash
npm install -g @anthropic-ai/claude-code
```

版本信息：
- 版本：2.1.81
- 路径：`/home/junjun/.nvm/versions/node/v24.14.0/bin/claude`

注意事项：
- 国内需配置代理（api.anthropic.com 被墙）
- 首次运行需登录 Anthropic 账号
- 官网：https://www.anthropic.com

# 关键配置文件

| 文件 | 用途 |
|------|------|
| `~/.openclaw/openclaw.json` | 主配置文件 |
| `~/.openclaw/.env` | 环境变量（API Key） |
| `~/.openclaw/workspace/` | 工作空间 |

# 常用命令

```bash
# 查看状态
openclaw status

# 查看日志
openclaw logs

# 重启服务
openclaw gateway restart

# 配置检查
openclaw doctor
```

# Python 依赖

内容采集和分析需要：

```bash
pip3 install --user requests beautifulsoup4 lxml openpyxl
```

# 浏览器自动化

```bash
# 安装 Playwright
npx playwright install chromium
npx playwright install-deps chromium
```

⚠️ 注意：避免使用 Snap 版 Chromium，推荐 Google Chrome `.deb` 版本

# 相关

- [[浏览器配置]]
- [[飞书插件路径冲突问题]]
- [[openclaw.json配置详解]]
