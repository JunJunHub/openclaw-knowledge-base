# VMWare Tools 与 FUSE 冲突解决方案

> 问题日期：2026-03-23
> 解决日期：2026-03-23
> 系统：Ubuntu 22.04 LTS Desktop (VMware 虚拟机)

---

## 问题描述

在 VMware 虚拟机中安装 `libfuse2` 后，VMWare Tools 的共享文件夹和剪贴板同步功能失效。

### 错误现象
- ❌ 共享文件夹无法访问
- ❌ 剪贴板同步失效
- ❌ 文件拖放不工作

### 原因分析
VMWare Tools 专有版本与 `libfuse2` 存在底层 FUSE 接口冲突：
- VMWare Tools 依赖特定的 FUSE 实现
- `libfuse2` 安装后可能覆盖了关键库文件
- 两个应用竞争同一个 FUSE 设备 `/dev/fuse`

---

## 解决方案

### 最终采用方案：迁移到 open-vm-tools

```bash
# 1. 安装 open-vm-tools（开源版本）
sudo apt install -y open-vm-tools open-vm-tools-desktop

# 2. 安装 fuse3（替代 fuse2，但 libfuse2 库文件会保留）
sudo apt install -y fuse3

# 3. 重启虚拟机
systemctl reboot
```

### 验证结果

```bash
# 检查服务状态
systemctl status open-vm-tools

# 检查共享文件夹
ls /mnt/hgfs/

# 检查 FUSE 库
dpkg -l | grep -E "fuse|fuse3|libfuse"

# 测试 AppImage
obsidian --version
```

---

## 技术细节

### FUSE 版本关系

| 组件 | 版本 | 用途 |
|------|------|------|
| `fuse3` | 3.10.5 | 现代 FUSE 实现 |
| `libfuse3-3` | 3.10.5 | fuse3 运行库 |
| `libfuse2` | 2.9.9 | **AppImage 依赖** |

**关键发现**：`libfuse2` 和 `fuse3` **可以共存**，不会冲突！

### 为什么 libfuse2 会保留？

安装 `fuse3` 时，系统会移除 `fuse` 包（旧版 FUSE 工具），但保留 `libfuse2` 库文件，因为：
- 很多旧应用依赖 `libfuse2`
- `libfuse2` 是库文件，`fuse3` 是工具和运行库
- 两者 API 不同，互不干扰

### open-vm-tools vs VMWare Tools

| 特性 | VMWare Tools (专有) | open-vm-tools (开源) |
|------|---------------------|---------------------|
| 维护者 | VMWare 专有 | VMware 开源 + 社区 |
| 集成度 | 单独安装包 | Ubuntu 官方仓库 |
| 更新频率 | 随 VMWare 版本 | 随系统更新 |
| FUSE 兼容性 | ⚠️ 可能冲突 | ✅ 兼容 fuse3 |
| 推荐度 | ❌ 旧版遗留 | ✅ **官方推荐** |

---

## 验证清单

安装 open-vm-tools 后，验证以下功能：

### 1. 服务状态
```bash
systemctl status open-vm-tools
# 应显示 Active: active (running)
```

### 2. 共享文件夹
```bash
ls /mnt/hgfs/
# 应显示在 VMWare 中配置的共享文件夹
```

### 3. 剪贴板同步
- 宿主机复制 → 虚拟机粘贴 ✅
- 虚拟机复制 → 宿主机粘贴 ✅

### 4. AppImage 运行
```bash
obsidian --version
# 应正常显示版本号
```

### 5. 内核模块
```bash
lsmod | grep vmw
# 应包含 vmw_vmci, vmw_vsock_vmci_transport 等
```

---

## 其他尝试过的方案（未采用）

### 方案 A：卸载 libfuse2 + AppImage 解压运行
```bash
sudo apt remove libfuse2
./obsidian.AppImage --appimage-extract
ln -sf squashfs-root/AppRun ~/.local/bin/obsidian
```
**缺点**：需要额外磁盘空间，升级时需重新解压

### 方案 B：尝试双兼容修复
```bash
sudo apt install --reinstall open-vm-tools open-vm-tools-desktop
sudo ldconfig
sudo systemctl restart vmware-tools
```
**结果**：未尝试，直接采用迁移 open-vm-tools 方案

---

## 经验教训

1. **优先选择开源版本**：open-vm-tools 是官方推荐，兼容性更好
2. **FUSE 版本共存是可能的**：libfuse2 和 fuse3 可以同时存在
3. **虚拟机环境特殊**：需要考虑宿主机与虚拟机的交互组件
4. **重启很重要**：某些系统服务更改需要重启才能生效

---

## 相关文件

- Obsidian AppImage 安装指南：`01-安装部署/Obsidian-AppImage安装配置指南.md`
- FUSE 官方文档：https://fuse.sourceforge.net/
- open-vm-tools 项目：http://open-vm-tools.sourceforge.net/

---

## 更新日志

| 日期 | 内容 |
|------|------|
| 2026-03-23 | 问题发现、排查、解决 |

---

*本文档记录了 VMWare 虚拟机环境下 FUSE 冲突的完整解决过程。*
