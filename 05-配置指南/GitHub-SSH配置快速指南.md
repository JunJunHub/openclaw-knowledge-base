# GitHub SSH 配置快速指南

> **分类**：配置指南
> **相关文档**：[[GitHub-Hosts配置与自动更新]]

## 快速配置步骤

### 1. 生成 SSH 密钥

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
```

### 2. 查看公钥

```bash
cat ~/.ssh/id_ed25519.pub
```

### 3. 添加到 GitHub

1. 打开 https://github.com/settings/ssh/new
2. Title 填写设备名（如 `ubuntu24`）
3. Key 粘贴公钥内容
4. 点击 "Add SSH key"

### 4. 添加主机密钥

```bash
ssh-keyscan github.com >> ~/.ssh/known_hosts
```

### 5. 测试连接

```bash
ssh -T git@github.com
```

## GitHub CLI 登录

```bash
# 使用 Token 登录
echo 'export GITHUB_PERSONAL_ACCESS_TOKEN="ghp_xxx"' >> ~/.bashrc
source ~/.bashrc
echo $GITHUB_PERSONAL_ACCESS_TOKEN | gh auth login --with-token
```

## 常见问题

| 问题 | 解决方案 |
|------|---------|
| Permission denied | 检查公钥是否添加到 GitHub |
| Host key verification failed | 执行 `ssh-keyscan github.com >> ~/.ssh/known_hosts` |
| Connection refused | 检查网络/代理设置 |

---

*来源：2026-04-02-github-ssh-fix.md*
