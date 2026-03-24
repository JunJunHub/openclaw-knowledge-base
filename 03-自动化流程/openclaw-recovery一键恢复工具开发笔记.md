# OpenClaw Recovery 一键恢复工具开发笔记

> 创建时间: 2026-03-24
> 项目地址: https://github.com/JunJunHub/openclaw-recovery

## 项目背景

在虚拟机环境中部署 OpenClaw 需要安装多个依赖、配置环境、恢复工作空间。每次重新部署都需要重复相同的步骤，耗时且容易遗漏。

**目标**: 创建一个一键恢复脚本，在全新 Ubuntu 虚拟机中快速部署完整的 OpenClaw 开发环境。

---

## 架构设计

### SpecKit 规格驱动开发

采用 **先规格后实现** 的开发模式：

```
specs/           # 规格文档（先写）
├── 00-overview.md
├── 01-system-deps.md
├── 02-openclaw-install.md
└── ...

scripts/         # 实现脚本（后写）
├── install.sh   # 主入口
├── lib/common.sh
└── stages/      # 分阶段脚本
```

**优点**：
- 规格与实现分离，便于维护
- 规格文档即项目文档
- 新功能先设计规格，再实现

### 分阶段设计

将安装过程拆分为独立阶段，支持单独执行：

| 阶段 | 脚本 | 功能 |
|------|------|------|
| 01 | `01-system.sh` | 系统依赖 + SSH |
| 02 | `02-node.sh` | NVM + Node.js |
| 03 | `03-chrome.sh` | Google Chrome |
| 04 | `04-openclaw.sh` | OpenClaw 安装 |
| 05 | `05-config.sh` | 配置恢复 |
| 06 | `06-workspaces.sh` | 工作空间初始化 |
| 07 | `07-verify.sh` | 安装验证 |
| 08 | `08-dev-tools.sh` | 开发工具 |
| 09 | `09-file-sharing.sh` | 文件共享 |
| 10 | `10-obsidian.sh` | Obsidian 笔记 |

---

## 功能演进

### v0.1.0 - 基础功能

- 系统依赖安装
- Node.js 环境配置
- Chrome 浏览器安装
- OpenClaw 安装
- 配置文件恢复
- 工作空间初始化

### v0.2.0 - 功能增强

**新增版本选择**：
```bash
# 社区版（默认）
./scripts/install.sh --all --version cn

# 原版
./scripts/install.sh --all --version original
```

**新增系统工具**：
- `vim` - 文本编辑器
- `net-tools` - 网络配置
- `htop` - 进程监控
- `tmux` - 终端复用

**新增 SSH 服务**：
- `openssh-server` 自动安装并启用

### v0.3.0 - 保护机制

**问题**: 在已有环境中误执行脚本会覆盖配置

**解决方案**: 多层保护机制

#### 1. 环境检测模式
```bash
./scripts/install.sh --check
```
仅检测环境，不执行安装。

#### 2. 执行计划预览
显示将执行的所有阶段和潜在风险。

#### 3. 配置变更预览
覆盖前显示 diff 对比，需要用户确认。

#### 4. 自动备份
- `openclaw.json` → `openclaw.json.bak.时间戳`
- `smb.conf` → `smb.conf.bak`

#### 5. 交互式确认
关键操作需要 `y/N` 确认。

---

## 核心设计决策

### 1. 为什么用 Shell 而不是 Python？

- **依赖最小化**: Shell 是 Linux 原生，无需额外安装
- **运维友好**: 系统管理员熟悉 Shell
- **轻量级**: 单文件脚本，无需虚拟环境

### 2. 为什么用 AppImage 安装 Obsidian？

- **无需 root**: 安装到用户目录
- **版本独立**: 不影响系统包管理
- **易于更新**: 直接替换 AppImage 文件

### 3. 为什么社区版改为 @latest？

原问题：
```bash
OPENCLAW_CN_VERSION="openclaw-cn@0.1.8-fix.3"  # 硬编码版本
```

改进：
```bash
OPENCLAW_CN_VERSION="openclaw-cn@latest"  # 自动最新
```

**原因**：
- 社区版定期同步原版，版本号会变化
- 硬编码版本需要手动更新脚本
- 用户期望安装最新版

---

## 使用场景

### ✅ 推荐场景

1. **新虚拟机环境**
   - 刚安装 Ubuntu
   - 需要快速部署开发环境

2. **系统重装后恢复**
   - 虚拟机快照恢复
   - 系统重装后一键恢复

### ❌ 不推荐场景

1. **已配置好的生产环境**
   - 可能覆盖现有配置
   - 建议先用 `--check` 检测

2. **不确定当前环境状态**
   - 建议先用 `--check` 检测
   - 查看执行计划后再决定

---

## 常见问题处理

### NVM 安装失败

**原因**: GitHub 访问慢

**解决**: 使用国内镜像
```bash
export NVM_NODEJS_ORG_MIRROR="https://npmmirror.com/mirrors/node"
```

### Chrome 下载超时

**解决**: 手动下载
```bash
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
sudo apt install -y ./google-chrome-stable_current_amd64.deb
```

---

## 后续优化方向

### 功能增强
- [ ] 支持自定义阶段组合
- [ ] 添加卸载/回滚功能
- [ ] 支持配置导入导出

### 用户体验
- [ ] 添加进度条显示
- [ ] 支持日志级别控制
- [ ] 添加彩色输出选项

### 兼容性
- [ ] 支持 Debian
- [ ] 支持 WSL
- [ ] 支持 macOS (部分阶段)

---

## 文件结构

```
openclaw-recovery/
├── README.md                 # 项目文档
├── DO_NOT_TEST_HERE.md       # 生产环境警告
├── specs/                    # 规格文档
│   ├── 00-overview.md
│   ├── 01-system-deps.md
│   ├── 02-openclaw-install.md
│   ├── 03-config-restore.md
│   ├── 04-workspaces.md
│   ├── 05-verification.md
│   ├── 06-dev-tools.md
│   └── 07-obsidian.md
├── scripts/
│   ├── install.sh            # 主入口
│   ├── lib/
│   │   └── common.sh         # 公共函数
│   └── stages/
│       ├── 01-system.sh
│       ├── 02-node.sh
│       ├── 03-chrome.sh
│       ├── 04-openclaw.sh
│       ├── 05-config.sh
│       ├── 06-workspaces.sh
│       ├── 07-verify.sh
│       ├── 08-dev-tools.sh
│       ├── 09-file-sharing.sh
│       └── 10-obsidian.sh
└── config/
    ├── openclaw.json.template
    ├── secrets.env.example
    └── secrets.env           # (需创建，不提交)
```

---

## 相关资源

- **GitHub**: https://github.com/JunJunHub/openclaw-recovery
- **OpenClaw 官方文档**: https://docs.openclaw.ai
- **OpenClaw 中文社区**: https://clawd.org.cn
- **SpecKit 方法论**: 先规格后实现
