# OpenClaw 知识库

> **创建**: 2026-03-20
> **维护**: Jun + MoltBot 🐾
> **仓库**: https://github.com/JunJunHub/openclaw-knowledge-base

---

## 目录结构

```
obsidian/
├── 00-Inbox/          # 收件箱，临时笔记待整理
├── 01-安装部署/        # OpenClaw 安装配置相关
├── 02-核心概念/        # 架构、Agent、Session、AI工具等
├── 03-自动化流程/      # Cron、采集、发布自动化
├── 04-配置指南/        # openclaw.json 及各组件配置
├── 05-三大专栏/        # 热点/实战/创业日记方法论
├── 06-踩坑记录/        # 问题解决、故障排查
├── 07-收益分析/        # 月度报告、平台数据
├── 08-资源索引/        # 快速查找、命令速查
├── 09-模板/           # 文章模板、笔记模板
├── 10-Archive/        # 归档过时内容
└── .sync-state.json   # 同步状态记录
```

---

## Obsidian 双链机制

### 什么是双链

Obsidian 使用 `[[文件名]]` 语法建立文档间的双向链接，形成知识网络。

**语法**：
```markdown
[[文件名]]           # 基础链接
[[文件名|显示文本]]   # 自定义显示文本
```

### 如何查看关联

1. **关系图谱**：`Ctrl+G`（或点击左侧图谱图标）
   - 可视化显示所有文档的关联关系
   - 节点大小表示连接数量

2. **反向链接面板**：每个文档底部
   - 显示哪些文档链接到当前文档
   - 显示当前文档链接的其他文档

### 当前知识网络

```
[[AI编码工具生态全景]]
    ├── [[AI编码工具对比-Superpowers-vs-SpecKit]]
    │       ├── [[Superpowers-功能概览]]
    │       └── [[Spec-Kit-完整指南]]
    │               ├── [[Spec-Kit-多Agent模式]]
    │               └── [[Spec-Kit-技术栈工具]]
    └── 其他工具...

[[多Agent架构详解]]
    └── [[多Agent消息互通指南]]
```

---

## 知识库整理原则

### 1. 去重合并

- 同一主题的多个文档合并为一篇完整文档
- 保留最详细、最新的版本作为基础
- 避免信息碎片化

**示例**：7 篇 Spec-Kit 文档 → 合并为 3 篇核心文档

### 2. 添加双链

每个文档开头添加相关文档链接：

```markdown
# 文档标题

> **相关文档**：[[相关文档1]] | [[相关文档2]]

内容...
```

### 3. 状态文件追踪

`.sync-state.json` 记录：
- 已处理的 memory 日志文件
- 新增/删除的笔记
- 各分类统计

---

## 笔记规范

### YAML Frontmatter（可选）

```yaml
---
title: 笔记标题
date: 2026-03-20
tags: [tag1, tag2]
source: workspace-coder/troubleshooting/xxx.md
---
```

### 标签体系

| 标签前缀 | 用途 | 示例 |
|---------|------|------|
| `#openclaw/` | OpenClaw 相关 | `#openclaw/安装` `#openclaw/配置` |
| `#ai/` | AI 工具相关 | `#ai/spec-kit` `#ai/claude-code` |
| `#opc/` | 一人公司相关 | `#opc/热点` `#opc/收益` |

### 命名规范

- 文件名使用中文，清晰描述内容
- 多个词用 `-` 连接：`Spec-Kit-完整指南.md`
- 避免特殊字符

---

## 自动同步

### 数据来源

```
workspace/memory/           →  整理后入库
workspace-thinker/research/ →  02-核心概念/
workspace-coder/            →  06-踩坑记录/
workspace-media/content/    →  05-三大专栏/
```

### 同步时间

| 时间 | 任务 |
|------|------|
| 22:00 | 知识库整理（从 memory 提取笔记）|
| 22:30 | Git 推送到 GitHub |

### 手动同步

```bash
cd ~/.openclaw/obsidian
git add . && git commit -m "docs: 更新笔记" && git push
```

---

## 维护指南

### 新增笔记流程

1. 确定分类目录
2. 使用 `write` 工具创建文档
3. 添加相关文档双链
4. 更新 `.sync-state.json`
5. Git 提交推送

### 整理重复文档流程

1. 分析重复内容
2. 选择最完整版本作为基础
3. 合并其他文档内容
4. 添加双链
5. 删除重复文档
6. 更新状态文件

---

## 相关资源

- **Obsidian 官方文档**: https://help.obsidian.md
- **双链教程**: https://help.obsidian.md/Linking+notes+and+files/Internal+links
- **关系图谱**: https://help.obsidian.md/Plugins/Graph+view

---

*最后更新：2026-03-26*
