---
title: ibus-qt4 在 Ubuntu 22.04 已废弃
date: 2026-03-24
tags: [踩坑, Ubuntu, 输入法, 安装]
priority: 高
status: 已解决
---

## 问题描述

在 Ubuntu 22.04 上安装中文输入法时，`ibus-qt4` 包已不存在于官方仓库中。

## 问题原因

- **Ubuntu 22.04** 已移除 `ibus-qt4` 包
- Qt4 相关的包在新版本 Ubuntu 中已被废弃
- 应使用 `ibus-qt5` 或其他替代方案

## 解决方案

### 方案 1：使用 ibus-qt5（推荐）

```bash
sudo apt install ibus-qt5
```

### 方案 2：使用 fcitx 输入法框架

```bash
sudo apt install fcitx fcitx-googlepinyin fcitx-table-wbpy
im-config -n fcitx
```

### 方案 3：使用系统自带的 IBus

Ubuntu 22.04 默认已安装 IBus，只需配置中文输入法：

1. 打开 **设置 → 区域与语言**
2. 点击 **管理安装的语言**
3. 安装 **中文（简体）**
4. 添加 **中文（Intelligent Pinyin）** 输入源

## 在 openclaw-recovery 脚本中的修复

在 `scripts/stages/01-system.sh` 中移除了 `ibus-qt4` 的安装：

```bash
# 旧版本（已移除）
# apt install ibus-qt4

# 新版本（无需额外安装，使用系统默认）
# 或安装 ibus-qt5
apt install ibus-qt5 2>/dev/null || true
```

## 参考资料

- [Ubuntu 22.04 Release Notes](https://discourse.ubuntu.com/t/jammy-jellyfish-release-notes/24668)
- [IBus Documentation](https://github.com/ibus/ibus/wiki)

## 相关文档

- [[OpenClaw-Ubuntu安装部署]]
