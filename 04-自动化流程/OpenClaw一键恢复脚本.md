# OpenClaw 一键恢复脚本

> 创建时间：2026-03-23
> 来源：memory/2026-03-23.md

---

## 项目概述

**目标**：在新 Ubuntu 虚拟机环境中，基于国内网络，一键恢复当前 OpenClaw 所有安装配置

**方法论**：SpecKit 规格驱动开发（先写 specs/，再实现脚本）

**项目位置**：`~/.openclaw/workspace-coder/openclaw-recovery/`

---

## 核心特性

| 特性 | 说明 |
|------|------|
| 国内网络优化 | 淘宝镜像、NVM 镜像自动配置 |
| 敏感信息安全 | 环境变量注入，不硬编码 |
| 幂等性设计 | 可重复执行，已安装自动跳过 |
| 分阶段执行 | 支持 `--stage` 参数灵活控制 |

---

## 使用方法

```bash
cd ~/.openclaw/workspace-coder/openclaw-recovery

./scripts/install.sh --all                  # 完整安装
./scripts/install.sh --stage system         # 仅系统依赖
./scripts/install.sh --all --interactive    # 交互式输入敏感信息
```

---

## 安装阶段

| 阶段 | 脚本 | 内容 |
|------|------|------|
| 01 | `01-system.sh` | 系统依赖安装 |
| 02 | `02-node.sh` | Node.js v24.14.0 安装 |
| 03 | `03-chrome.sh` | Chrome .deb 版本安装 |
| 04 | `04-openclaw.sh` | OpenClaw 安装 |
| 05 | `05-config.sh` | 配置恢复（占位符替换）|
| 06 | `06-workspaces.sh` | 5个 Workspace 创建 |
| 07 | `07-verify.sh` | 验证测试 |

---

## 敏感配置占位符

| 占位符 | 环境变量 |
|--------|----------|
| `{{SILICONFLOW_API_KEY}}` | `SILICONFLOW_API_KEY` |
| `{{BAIDU_QIANFAN_API_KEY}}` | `BAIDU_QIANFAN_API_KEY` |
| `{{FEISHU_APP_ID}}` | `FEISHU_APP_ID` |
| `{{FEISHU_APP_SECRET}}` | `FEISHU_APP_SECRET` |
| `{{GATEWAY_TOKEN}}` | `GATEWAY_TOKEN` |

---

## 关键决策

1. **Bash 脚本而非 Python** — 更简单，依赖更少
2. **分阶段执行而非单一脚本** — 更灵活，便于调试
3. **占位符模板而非直接修改配置** — 安全性更高

---

## 技术细节

| 组件 | 版本/来源 |
|------|----------|
| Node.js | v24.14.0（通过 NVM）|
| OpenClaw | openclaw-cn@0.1.8-fix.3 |
| Chrome | Google Chrome 146.0.7680.153（.deb 版本）|
| npm 镜像 | https://registry.npmmirror.com |
| NVM 镜像 | https://npmmirror.com/mirrors/node |

---

## GitHub

https://github.com/JunJunHub/openclaw-recovery

---

*整理自 memory/2026-03-23.md*
