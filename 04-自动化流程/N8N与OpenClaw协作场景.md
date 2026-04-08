# N8N + OpenClaw 协作场景

## 核心定位

| 工具 | 定位 | 擅长 |
|------|------|------|
| **N8N** | 工作流自动化 | 定时任务、数据流转、API 集成、条件分支 |
| **OpenClaw** | AI 智能体 | 内容创作、对话理解、智能决策、浏览器操作 |

## 结合场景示例

### 1️⃣ 内容发布自动化

```
N8N 定时触发 → 调用 OpenClaw API → 生成内容 → 发布到平台
```

- N8N 每周一/三/五 20:00 定时触发
- 调用 OpenClaw 生成小红书笔记内容
- N8N 调用小红书 API 或浏览器自动化发布

### 2️⃣ 数据采集 + 智能分析

```
N8N 采集数据 → OpenClaw 分析 → 生成报告 → 推送通知
```

- N8N 定时抓取热点新闻、GitHub Trending
- 调用 OpenClaw 进行内容分析、摘要生成
- N8N 推送到飞书/Telegram 通知

### 3️⃣ 多平台同步

```
用户发布 → N8N 监听 → OpenClaw 改写 → 同步到多平台
```

## 技术对接方式

| 方式 | 说明 |
|------|------|
| HTTP Webhook | OpenClaw 暴露 HTTP 端点，N8N 通过 HTTP Request 调用 |
| 消息队列 | N8N 发送消息到飞书/Telegram，OpenClaw 监听处理 |
| 共享存储 | N8N 写入任务到共享目录，OpenClaw 定时检查处理 |

## 小红书自动化工作流

```
定时触发(20:00 Mon/Wed/Fri)
       ↓
  读取状态文件
       ↓
  解析待发布笔记
       ↓
生成配图(Pollinations AI)
       ↓
 发布到小红书
       ↓
  更新状态文件
       ↓
 发送飞书通知
```

---

**来源**：`workspace/memory/2026-04-08-n8n-docker-setup.md`
**整理时间**：2026-04-08
