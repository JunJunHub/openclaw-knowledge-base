# Obsidian AppImage 在虚拟机中启动失败

## 问题描述

Obsidian AppImage 在 Ubuntu 虚拟机中启动失败，出现两个错误。

## 错误 1：缺少 FUSE 库

```
dlopen(): error loading libfuse.so.2

AppImages require FUSE to run. 
You might still be able to extract the contents of this AppImage 
if you run it with the --appimage-extract option. 
See FUSE for more information
```

### 解决方案

```bash
sudo apt update && sudo apt install libfuse2
```

## 错误 2：SUID Sandbox 问题

```
[6756:0401/143927.113784:FATAL:sandbox/linux/suid/client/setuid_sandbox_host.cc:166] 
The SUID sandbox helper binary was found, but is not configured correctly. 
Rather than run without sandboxing I'm aborting now. 
You need to make sure that /tmp/.mount_ObsidiOOKneU/chrome-sandbox 
is owned by root and has mode 4755.
Trace/breakpoint trap (core dumped)
```

### 解决方案

添加 `--no-sandbox` 参数：

```bash
./Obsidian.AppImage --no-sandbox
```

## 完整修复

创建启动脚本：

```bash
#!/bin/bash
OBSIDIAN_APP="$HOME/Applications/Obsidian.AppImage"

# 虚拟机环境需要 --no-sandbox
nohup "$OBSIDIAN_APP" --no-sandbox "$@" > /dev/null 2>&1 &
```

创建桌面快捷方式：

```ini
[Desktop Entry]
Name=Obsidian
Exec=/home/user/Applications/Obsidian.AppImage --no-sandbox %U
Terminal=false
Type=Application
```

## 相关文件

- `openclaw-recovery/scripts/stages/10-obsidian.sh`
- 提交：`6392dd2`

---

*来源：openclaw-recovery 一键恢复脚本调试，2026-04-01*
