# Obsidian AppImage 安装配置指南

> 安装日期：2026-03-23
> 系统：Ubuntu 22.04 LTS Desktop
> 安装方式：AppImage
> 版本：v1.8.9

---

## 📋 目录

- [背景](#背景)
- [安装步骤](#安装步骤)
- [问题解决](#问题解决)
- [升级方法](#升级方法)
- [使用方式](#使用方式)
- [相关资源](#相关资源)

---

## 背景

### 为什么选择 AppImage

| 方式 | 优点 | 缺点 |
|------|------|------|
| **AppImage** ✅ | 独立运行、不依赖系统包、版本最新 | 需手动升级、需 FUSE 支持 |
| Flatpak | 沙盒隔离、自动更新 | 占用空间大、启动稍慢 |
| Snap | 系统集成好 | 更新有延迟、AppArmor 限制 |

**选择 AppImage 的原因**：
- Ubuntu 22.04 官方仓库没有 Obsidian
- 想要最新版本
- 不想依赖系统包管理器的更新周期

---

## 安装步骤

### 1. 检查系统环境

```bash
# 确认系统版本
lsb_release -a

# 检查磁盘空间
df -h / && free -h
```

### 2. 下载 AppImage

```bash
# 查看最新版本
curl -s https://api.github.com/repos/obsidianmd/obsidian-releases/releases/latest | grep '"tag_name"'

# 自动检测架构并下载（推荐）
ARCH=$(uname -m)
case $ARCH in
  x86_64)  ASSET="Obsidian-1.8.9.AppImage" ;;
  aarch64) ASSET="Obsidian-1.8.9-arm64.AppImage" ;;
  *)       echo "不支持的架构: $ARCH"; exit 1 ;;
esac

cd ~/Downloads
wget "https://github.com/obsidianmd/obsidian-releases/releases/download/v1.8.9/${ASSET}"

# 或者使用 awk 自动过滤（适用于自动化脚本）
# curl -s https://api.github.com/repos/obsidianmd/obsidian-releases/releases/latest | \
#   awk -F'"' '/browser_download_url.*AppImage/ {print $4}' | \
#   grep -E "$(uname -m|x86_64|aarch64)" | head -1 | xargs wget
```

### 3. 安装到用户目录

```bash
# 创建目录
mkdir -p ~/.local/bin

# 移动并设置权限
mv ~/Downloads/Obsidian-1.8.9.AppImage ~/.local/bin/obsidian.AppImage
chmod +x ~/.local/bin/obsidian.AppImage

# 创建软链接（方便命令行调用）
ln -sf ~/.local/bin/obsidian.AppImage ~/.local/bin/obsidian

# 确保路径在 PATH 中
grep -q '.local/bin' ~/.bashrc || echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### 4. 创建桌面快捷方式

```bash
# 创建 .desktop 文件
mkdir -p ~/.local/share/applications
cat > ~/.local/share/applications/obsidian.desktop << 'EOF'
[Desktop Entry]
Name=Obsidian
Comment=Obsidian Note Taking App
Exec=/home/junjun/.local/bin/obsidian.AppImage %u
Icon=/home/junjun/.local/share/icons/hicolor/512x512/apps/obsidian.png
Type=Application
Categories=Office;Utility;
MimeType=x-scheme-handler/obsidian;
StartupNotify=true
EOF

# 更新桌面数据库
update-desktop-database ~/.local/share/applications
```

### 5. 安装图标

```bash
# 从 AppImage 提取图标
cd /tmp
~/.local/bin/obsidian.AppImage --appimage-extract

# 创建图标目录
for size in 16 32 48 64 128 256 512; do
  mkdir -p ~/.local/share/icons/hicolor/${size}x${size}/apps/
done

# 复制图标
for size in 16 32 48 64 128 256 512; do
  cp "/tmp/squashfs-root/usr/share/icons/hicolor/${size}x${size}/apps/obsidian.png" \
     ~/.local/share/icons/hicolor/${size}x${size}/apps/obsidian.png
done

# 更新图标缓存
gtk-update-icon-cache -t -f ~/.local/share/icons/hicolor

# 清理临时文件
rm -rf /tmp/squashfs-root
```

---

## 问题解决

### 问题 1：缺少 FUSE 支持

**错误信息**：
```
dlopen(): error loading libfuse.so.2
AppImages require FUSE to run.
```

**原因**：Ubuntu 22.04 默认只有 libfuse3，AppImage 需要 libfuse2

**解决方案**：
```bash
# 安装 libfuse2
sudo apt update && sudo apt install libfuse2
```

**备选方案**（无需 FUSE）：
```bash
# 解压 AppImage
cd ~/.local/bin
./obsidian.AppImage --appimage-extract

# 更新软链接指向解压后的可执行文件
ln -sf ~/.local/bin/squashfs-root/AppRun ~/.local/bin/obsidian
```

---

### 问题 2：桌面图标显示不正确

**原因**：系统找不到 Obsidian 的图标文件

**解决方案**：
1. 从 AppImage 提取官方图标
2. 复制到标准图标目录 `~/.local/share/icons/hicolor/`
3. 更新 `.desktop` 文件使用完整路径
4. 刷新图标缓存

详见上文 [安装图标](#5-安装图标) 章节。

---

## 升级方法

### 手动升级（推荐）

```bash
# 1. 检查最新版本
LATEST=$(curl -s https://api.github.com/repos/obsidianmd/obsidian-releases/releases/latest | grep '"tag_name"' | sed 's/.*"v\([^"]*\)".*/\1/')
echo "最新版本: v${LATEST}"

# 2. 下载新版本
cd ~/Downloads
wget "https://github.com/obsidianmd/obsidian-releases/releases/download/v${LATEST}/Obsidian-${LATEST}.AppImage"

# 3. 备份旧版本
mv ~/.local/bin/obsidian.AppImage ~/.local/bin/obsidian-old.AppImage

# 4. 安装新版本
mv "Obsidian-${LATEST}.AppImage" ~/.local/bin/obsidian.AppImage
chmod +x ~/.local/bin/obsidian.AppImage

# 5. 验证
obsidian --version

# 6. 清理备份
rm ~/.local/bin/obsidian-old.AppImage
```

### 自动化升级脚本

创建脚本 `~/.local/bin/upgrade-obsidian.sh`：

```bash
#!/bin/bash
set -e

echo "🔍 检查 Obsidian 最新版本..."
LATEST=$(curl -s https://api.github.com/repos/obsidianmd/obsidian-releases/releases/latest | grep '"tag_name"' | sed 's/.*"v\([^"]*\)".*/\1/')

echo "📦 最新版本: v${LATEST}"
echo "⬇️  下载中..."

cd ~/Downloads
wget -q --show-progress "https://github.com/obsidianmd/obsidian-releases/releases/download/v${LATEST}/Obsidian-${LATEST}.AppImage"

echo "🔄 替换旧版本..."
mv ~/.local/bin/obsidian.AppImage ~/.local/bin/obsidian-old.AppImage 2>/dev/null || true
mv "Obsidian-${LATEST}.AppImage" ~/.local/bin/obsidian.AppImage
chmod +x ~/.local/bin/obsidian.AppImage

echo "✅ 升级完成！新版本: v${LATEST}"
rm ~/.local/bin/obsidian-old.AppImage 2>/dev/null || true
```

---

## 使用方式

### 终端启动

```bash
# 直接运行
obsidian

# 打开指定知识库
obsidian ~/.openclaw/obsidian/

# 后台运行
nohup obsidian > /dev/null 2>&1 &
```

### 图形界面启动

- **应用菜单**：搜索 "Obsidian"
- **文件管理器**：右键 `.md` 文件 → 打开方式 → Obsidian

### 配置 CLI 访问（可选）

在 Obsidian 中启用命令行访问：
1. 打开设置 → 核心插件 → 命令面板
2. 启用命令面板
3. 设置 → 社区插件 → 搜索 "Obsidian CLI"（如有）

---

## 相关资源

### 官方链接
- 官网：https://obsidian.md
- GitHub Releases：https://github.com/obsidianmd/obsidian-releases/releases
- 文档：https://help.obsidian.md

### 知识库信息
- **路径**：`~/.openclaw/obsidian/`
- **Git 仓库**：https://github.com/JunJunHub/openclaw-knowledge-base
- **同步方式**：定时任务（每天 22:00 整理 + 22:30 Git 推送）

### 本机安装位置
| 文件 | 路径 |
|------|------|
| AppImage | `~/.local/bin/obsidian.AppImage` |
| 软链接 | `~/.local/bin/obsidian` |
| 桌面快捷方式 | `~/.local/share/applications/obsidian.desktop` |
| 图标 | `~/.local/share/icons/hicolor/*/apps/obsidian.png` |

---

## 📝 更新日志

| 日期 | 内容 |
|------|------|
| 2026-03-23 | 初始安装，解决 FUSE 和图标问题 |

---

*本文档由 MoltBot 自动生成，记录完整的安装配置过程。*
