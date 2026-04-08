# N8N 工作流自动化平台

## 概述

- **类型**：开源工作流自动化平台（类似 Zapier/Make）
- **特点**：可视化拖拽编排，支持 400+ 预置节点
- **优势**：支持自托管，数据完全掌控

## 与 OpenClaw 的关系

| 平台 | 定位 | 适用场景 |
|------|------|---------|
| N8N | 稳定流程 | 定时任务、数据流转 |
| OpenClaw | 智能决策 | 内容创作、对话交互 |

可以结合使用。

## Docker 安装

### 1. 配置国内镜像

```bash
# /etc/docker/daemon.json
{
  "registry-mirrors": [
    "https://docker.1ms.run",
    "https://docker.xuanyuan.me"
  ]
}
```

### 2. 启动 N8N

```bash
docker run -d \
  --name n8n \
  --restart unless-stopped \
  -p 5678:5678 \
  -e TZ=Asia/Shanghai \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
```

### 3. 访问

- 地址：http://localhost:5678

## npm 安装问题

官方 npm 包存在 `n8n-editor-ui` 版本缺失 bug，推荐使用 Docker 方式。

---

**来源**：`workspace/memory/2026-04-08.md`
**整理时间**：2026-04-08
