# OpenClaw 一键恢复脚本开发指南

> 来源: 2026-03-23 记忆日志
> 分类: 自动化流程

## 📋 项目概述

**目标**: 在新 Ubuntu 虚拟机环境中，基于国内网络，一键恢复当前 OpenClaw 所有安装配置。

**方法论**: **SpecKit 规格驱动开发** - 先写 specs/，再实现脚本。

## 🏗️ 项目结构

```
openclaw-recovery/
├── specs/                     # 规格文档
│   ├── 00-overview.md        # 项目概述
│   ├── 01-system-deps.md     # 系统依赖
│   ├── 02-openclaw-install.md # OpenClaw安装
│   ├── 03-config-restore.md  # 配置恢复
│   ├── 04-workspaces.md      # Workspace恢复
│   └── 05-verification.md    # 验证测试
├── scripts/                   # 安装脚本
│   ├── install.sh            # 主脚本
│   ├── lib/common.sh         # 公共函数库
│   └── stages/               # 7个阶段脚本
├── config/                   # 配置模板
│   ├── openclaw.json.template
│   └── secrets.env.example
└── README.md
```

## 🎯 核心设计原则

### 1. 国内网络优化
- **npm 镜像**: `https://registry.npmmirror.com`
- **NVM 镜像**: `https://npmmirror.com/mirrors/node`

### 2. 敏感信息安全
使用占位符模板，通过环境变量注入：

| 占位符 | 环境变量 |
|--------|----------|
| `{{SILICONFLOW_API_KEY}}` | `SILICONFLOW_API_KEY` |
| `{{BAIDU_QIANFAN_API_KEY}}` | `BAIDU_QIANFAN_API_KEY` |
| `{{FEISHU_APP_ID}}` | `FEISHU_APP_ID` |
| `{{FEISHU_APP_SECRET}}` | `FEISHU_APP_SECRET` |
| `{{GATEWAY_TOKEN}}` | `GATEWAY_TOKEN` |

### 3. 幂等性设计
- 可重复执行
- 已安装组件自动跳过
- 避免重复安装和配置冲突

### 4. 分阶段执行
支持 `--stage` 参数灵活控制执行范围：
- `system` - 系统依赖
- `node` - Node.js 安装
- `chrome` - Chrome 安装
- `openclaw` - OpenClaw 安装
- `config` - 配置恢复
- `workspaces` - Workspace 创建
- `verify` - 验证测试

## 💡 关键决策

| 决策点 | 选择 | 原因 |
|--------|------|------|
| 脚本语言 | Bash | 更简单，依赖更少 |
| 执行方式 | 分阶段 | 更灵活，便于调试 |
| 配置方式 | 占位符模板 | 安全性更高 |

## 🚀 使用方法

```bash
# 进入项目目录
cd ~/.openclaw/workspace-coder/openclaw-recovery

# 完整安装
./scripts/install.sh --all

# 分阶段安装
./scripts/install.sh --stage system
./scripts/install.sh --stage node

# 交互式输入敏感信息
./scripts/install.sh --all --interactive
```

## ⚠️ 重要提醒

> **此脚本仅供新虚拟机环境使用！**
>
> 在已配置好的环境中运行可能：
> - 覆盖现有配置
> - 破坏系统环境
> - 导致不可预期的问题

## 🔗 相关资源

- **项目位置**: `~/.openclaw/workspace-coder/openclaw-recovery/`
- **知识库**: OpenClaw OPC 项目
- **开发模式**: SpecKit 规格驱动开发

---

**创建时间**: 2026-03-23
**标签**: #自动化 #脚本开发 #OpenClaw #SpecKit
