# Bash 脚本中 $HOME 变量展开问题

## 问题描述

在某些上下文中，`$HOME` 变量未正确展开，导致路径错误。

## 场景

```bash
# 脚本开头定义变量
WIN_MOUNT_POINT="$HOME/mnt/win"
SHARE_DIR="$HOME/Share"

# 某些情况下执行时
mkdir: cannot create directory '/home/Share': Permission denied
```

## 根因

`$HOME` 变量在某些执行上下文（如 sudo、cron、子进程）中可能未定义或指向错误路径。

## 解决方案

使用 `$USER` 变量构建路径：

```bash
# ❌ 可能出问题
WIN_MOUNT_POINT="$HOME/mnt/win"
SHARE_DIR="$HOME/Share"

# ✅ 更可靠
WIN_MOUNT_POINT="/home/$USER/mnt/win"
SHARE_DIR="/home/$USER/Share"
```

## 其他替代方案

```bash
# 使用 ~ 展开需要 eval
SHARE_DIR=$(eval echo "~$USER/Share")

# 或者明确使用用户名
SHARE_DIR="/home/$(whoami)/Share"
```

## 相关文件

- `openclaw-recovery/scripts/stages/09-file-sharing.sh`
- 提交：`6392dd2`

---

*来源：openclaw-recovery 一键恢复脚本调试，2026-04-01*
