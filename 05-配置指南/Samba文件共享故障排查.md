# Samba 文件共享故障排查指南

> 来源：多次问题排查经验整理（2026-04-03 / 2026-04-16）
> 场景：Ubuntu 虚拟机与 Windows 11 文件共享

---

## 🎯 故障现象速查表

| 现象 | 可能原因 | 解决章节 |
|------|---------|---------|
| Windows 提示用户名或密码错误 | Samba 用户不存在 | [第一步](#第一步检查-samba-用户是否存在) |
| 安全策略阻止来宾访问 | Win10/11 默认策略 | [第二步](#第二步检查-windows-安全策略) |
| 无权限访问共享目录 | 文件夹权限问题 | [第三步](#第三步检查文件夹权限) |
| 文件归属 nobody | `force user` 配置问题 | [权限配置优化](#权限配置优化) |
| 能访问但无法写入 | 目录权限或 mask 设置 | [第三步](#第三步检查文件夹权限) |

---

## 💡 核心概念

### Samba 的"双重身份"机制

问题根源：混淆了 **Linux 系统用户** 和 **Samba 服务用户**。

| 类型 | 存储位置 | 用途 | 查看命令 |
|------|---------|------|---------|
| Linux 系统用户 | `/etc/passwd` | 系统登录、文件权限 | `id <用户名>` |
| Samba 服务用户 | `passdb.tdb` | 网络共享认证 | `sudo pdbedit -L` |

**关键点**：
- Samba 用户必须基于已存在的 Linux 系统用户创建
- Samba 用户拥有独立的密码（可与系统密码不同）
- 两者缺一不可，否则 Windows 无法通过验证

### 路径权限问题

共享目录的**父目录**权限也很关键：
- `/home/jun` 默认权限 700 → Windows 访问被拦截
- `/home` 默认权限 755 → Windows 可正常访问

**建议**：共享目录放在 `/home/Share` 而非 `/home/jun/Share`

---

## 📋 标准排查清单

### 第一步：检查 Samba 用户是否存在（Ubuntu 端）

```bash
# 查看 Samba 用户列表
sudo pdbedit -L

# 如果没有你的用户名，添加 Samba 账户
sudo smbpasswd -a <用户名>

# 重启服务
sudo systemctl restart smbd
```

**判断标准**：
- ✅ 列表中有你的用户名 → Samba 账户正常
- ❌ 列表为空或没有你的用户名 → 需要添加 Samba 账户

### 第二步：检查 Windows 安全策略（Windows 端）

**现象**：报错"安全策略阻止未经身份验证的来宾访问"

**解决**：
1. 打开组策略编辑器：`Win + R` → 输入 `gpedit.msc`
2. 导航到：`计算机配置` → `管理模板` → `网络` → `Lanman 工作站`
3. 找到：`启用不安全的来宾登录`
4. 设置为：**已启用**

### 第三步：检查文件夹权限（Ubuntu 端）

```bash
# 查看共享文件夹权限
ls -la /home/Share

# 确保用户有读写权限
sudo chown -R <用户名>:<用户名> /home/Share
chmod 777 /home/Share
```

### 第四步：重启服务

```bash
sudo systemctl restart smbd
```

---

## 🔧 权限配置优化

### `force user` 问题

**问题现象**：
- 文件归属显示 `nobody`
- 普通用户无法写入 nobody 创建的目录

**原因**：`smb.conf` 中配置了 `force user = nobody`

**解决方案**：移除 force user 配置

```bash
# 修复现有权限
sudo chown -R <用户名>:<用户名> /home/Share

# 移除强制用户配置
sudo sed -i '/force user = nobody/d' /etc/samba/smb.conf
sudo sed -i '/force group = nogroup/d' /etc/samba/smb.conf

# 重启 Samba
sudo systemctl restart smbd
```

### 推荐配置

```ini
[Share]
   path = /home/Share
   browseable = yes
   read only = no
   create mask = 0777
   directory mask = 0777
   public = yes
   writable = yes
   guest ok = yes
   # 不使用 force user，文件归属实际访问用户
```

---

## 📁 访问共享目录

### 从 Windows 访问 Ubuntu 共享

```
\\<虚拟机IP>\Share
```

示例：`\\192.168.1.100\Share`

### 认证方式

| 方式 | 用户名 | 密码 |
|------|--------|------|
| 用户认证 | Linux 用户名 | Samba 密码（`smbpasswd` 设置的） |
| 来宾访问 | guest | 无需密码（需 `guest ok = yes`） |

---

## ⚠️ 常见错误

| 错误信息 | 原因 | 解决 |
|---------|------|------|
| 用户名或密码不正确 | Samba 用户不存在 | `sudo smbpasswd -a <用户名>` |
| 安全策略阻止来宾访问 | Win10/11 默认策略 | 修改组策略（见第二步） |
| 无权限访问 | 文件夹权限不足 | `chown` 修改所有者 |
| 连接超时 | 防火墙拦截 | `sudo ufw allow samba` |
| 文件归属 nobody | `force user` 配置 | 移除 force user 配置 |

---

## 🔍 验证命令

```bash
# 检查 Samba 配置语法
testparm -s

# 查看共享配置
testparm -s | grep -A 10 "\[Share\]"

# 查看 Samba 用户
sudo pdbedit -L

# 检查目录权限
ls -la /home/Share

# 检查服务状态
sudo systemctl status smbd
```

---

## 相关文档

- [Samba 官方文档](https://www.samba.org/)
- [Ubuntu Samba 配置指南](https://ubuntu.com/tutorials/install-and-configure-samba)
