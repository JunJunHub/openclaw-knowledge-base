# GitHub Hosts 更新脚本踩坑

## 问题现象

执行 `update-github-hosts.sh` 脚本后，hosts 文件没有添加新的配置信息。

## 问题原因

1. **`set -e` 导致提前退出**：当某些域名无法解析时，`dig` 命令返回非零状态，导致脚本提前退出

2. **域名列表包含失效域名**：`assets-cdn.github.com` 已失效，无法解析

## 解决方案

### 1. 移除失效域名

```bash
DOMAINS=(
    "github.com"
    "github.global.ssl.fastly.net"
    "codeload.github.com"
    "api.github.com"
    "gist.github.com"
    # 移除: "assets-cdn.github.com" - 已失效
)
```

### 2. 添加容错处理

在 `get_ip` 函数调用处添加 `|| true`：

```bash
get_ip() {
    local domain=$1
    dig +short A "$domain" | grep -E '^[0-9]+\.[0-9]+\.[0-9]+\.[0-9]+$' | head -1
}

# 使用时添加 || true
for domain in "${DOMAINS[@]}"; do
    ip=$(get_ip "$domain" || true)
    if [ -n "$ip" ]; then
        echo "$ip $domain"
    fi
done
```

### 3. 手动添加 hosts（备选）

如果脚本仍然失败，可以手动执行：

```bash
sudo bash -c 'cat >> /etc/hosts << EOF

# GitHub Hosts (2026-04-07)
20.205.243.166 github.com
199.59.149.202 github.global.ssl.fastly.net
20.205.243.165 codeload.github.com
20.205.243.168 api.github.com
159.106.121.75 gist.github.com
# GitHub Hosts End
EOF'
```

### 4. 刷新 DNS 缓存

```bash
sudo resolvectl flush-caches
```

## 验证

```bash
ping -c 2 github.com
```

---

**来源**：`workspace/memory/2026-04-07-github-hosts-fix.md`
**整理时间**：2026-04-07
