# GitHub Hosts 配置与自动更新

> **分类**：配置指南
> **相关文档**：[[Web-Search搜索配置]]

## 背景

国内访问 GitHub 经常出现连接超时、速度慢等问题。通过配置本地 hosts 文件可以缓解，但 IP 会变化，需要定期更新。

## Hosts 方案的局限

| 问题 | 说明 |
|------|------|
| IP 会变 | GitHub 服务器迁移、CDN 调整，几个月可能换一次 |
| 不是全部域名 | 只覆盖了主要域名，某些资源域名可能漏掉 |
| 需要维护 | 发现访问不了时要重新查 IP 更新 |

## 自动更新脚本

```bash
#!/bin/bash
# ~/scripts/update-github-hosts.sh

HOSTS_FILE="/etc/hosts"
BACKUP="/etc/hosts.backup.$(date +%Y%m%d)"

# 备份原文件
sudo cp $HOSTS_FILE $BACKUP

# 删除旧的 GitHub 条目
sudo sed -i '/# GitHub Hosts/,/# GitHub Hosts End/d' $HOSTS_FILE

# 获取最新 IP（从 public dns 查询）
get_ip() {
  nslookup $1 8.8.8.8 | grep -A1 "Name:" | grep "Address" | tail -1 | awk '{print $2}'
}

GITHUB_IP=$(get_ip github.com)
FASTLY_IP=$(get_ip github.global.ssl.fastly.net)
ASSETS_IP=$(get_ip assets-cdn.github.com)

# 添加新条目
echo -e "\n# GitHub Hosts ($(date +%Y-%m-%d))" | sudo tee -a $HOSTS_FILE
echo "$GITHUB_IP github.com" | sudo tee -a $HOSTS_FILE
echo "$FASTLY_IP github.global.ssl.fastly.net" | sudo tee -a $HOSTS_FILE
echo "$ASSETS_IP assets-cdn.github.com" | sudo tee -a $HOSTS_FILE
echo "# GitHub Hosts End" | sudo tee -a $HOSTS_FILE

# 刷新 DNS
sudo systemd-resolve --flush-caches

echo "✅ GitHub hosts 更新完成"
```

## 定时任务配置

```bash
# 每周日凌晨 6 点自动更新
(crontab -l 2>/dev/null; echo "0 6 * * 0 $HOME/scripts/update-github-hosts.sh") | crontab -
```

## 方案稳定性对比

| 方案 | 稳定性 | 成本 |
|------|--------|------|
| 代理/VPN | ⭐⭐⭐⭐⭐ | 有成本，但最稳 |
| Hosts + 自动更新 | ⭐⭐⭐⭐ | 免费，需维护 |
| 纯 Hosts | ⭐⭐⭐ | 免费，偶尔失效 |

## 建议

- **频繁推送代码、CI/CD 依赖 GitHub**：推荐代理
- **偶尔访问、下载仓库**：Hosts 方案够用

---

*创建时间：2026-04-01*
*来源：workspace/memory/2026-04-01-github-hosts.md*
