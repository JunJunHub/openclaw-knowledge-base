# Samba 共享目录权限修复

## 问题描述

| 现象 | 原因 |
|------|------|
| Windows 无法访问共享目录 | 家目录 `/home/jun` 默认权限 700，父目录拦截 |
| 文件归属 nobody | Samba 配置 `force user = nobody` |
| 普通用户无法写入 | nobody 创建的目录，其他用户无权限 |

## 问题根源

### 路径问题
- 共享目录放在 `/home/jun/Share`
- `/home/jun` 权限为 700，Windows 访问被父目录权限拦截

### 配置问题
```ini
[Share]
   force user = nobody    # 导致文件归属混乱
   force group = nogroup  # 导致权限问题
```

## 解决方案

### 方案 1：修改路径（推荐）

将共享目录从 `/home/jun/Share` 改为 `/home/Share`：
- `/home` 默认权限 755，Windows 可访问

### 方案 2：移除 force user

```bash
# 修复现有权限
sudo chown -R jun:jun /home/Share

# 移除强制用户配置
sudo sed -i '/force user = nobody/d' /etc/samba/smb.conf
sudo sed -i '/force group = nogroup/d' /etc/samba/smb.conf

# 重启 Samba
sudo systemctl restart smbd
```

## 最终配置

```ini
[Share]
   path = /home/Share
   browseable = yes
   read only = no
   create mask = 0755
   directory mask = 0755
   # 不使用 force user，文件归属实际访问用户
```

## 验证命令

```bash
# 检查配置
testparm -s | grep -A 10 "\[Share\]"

# 检查权限
ls -la /home/Share
```

---

*来源：workspace/memory/2026-04-03-samba-permissions-fix.md*
*创建时间：2026-04-03*
