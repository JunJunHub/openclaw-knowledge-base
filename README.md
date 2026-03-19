# OpenClaw 知识库

> **创建**: 2026-03-20
> **维护**: Jun + MoltBot 🐾

---

## 目录说明

| 目录 | 用途 | 内容来源 |
|------|------|---------|
| **00-Inbox** | 收件箱，临时笔记待整理 | 随手记录 |
| **01-安装部署** | OpenClaw 安装配置 | 部署经验 |
| **02-核心概念** | 架构、Agent、Session 等 | 学习笔记 |
| **03-配置指南** | openclaw.json 配置详解 | 配置经验 |
| **04-三大专栏** | 热点/实战/创业日记方法论 | 内容创作 |
| **05-自动化流程** | Cron、采集、发布自动化 | 流程文档 |
| **06-踩坑记录** | 问题解决、故障排查 | troubleshooting |
| **07-收益分析** | 月度报告、平台数据 | 数据统计 |
| **08-资源索引** | 快速查找、命令速查 | 参考资料 |
| **09-模板** | 文章模板、笔记模板 | 模板库 |
| **10-Archive** | 归档过时内容 | 历史 |

---

## 笔记规范

### YAML Frontmatter

```yaml
---
title: 笔记标题
date: 2026-03-20
tags: [tag1, tag2]
category: 安装部署
source: workspace-coder/troubleshooting/xxx.md
status: draft | final
---
```

### 标签体系

- `#openclaw` - 所有 OpenClaw 相关
- `#安装` `#配置` `#踩坑` - 分类标签
- `#热点` `#实战` `#创业` - 专栏标签
- `#agent` `#session` `#binding` - 概念标签

---

## 同步规则

各工作空间的"已完成"内容会自动同步到本知识库：

```
workspace-media/content/     →  obsidian/04-三大专栏/
workspace-coder/troubleshooting/  →  obsidian/06-踩坑记录/
workspace-thinker/research/  →  obsidian/04-三大专栏/
```

同步由 Cron 任务执行（每天 22:00）。
