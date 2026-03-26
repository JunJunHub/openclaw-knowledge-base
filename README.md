# OpenClaw 知识库

> **创建**: 2026-03-20
> **维护**: Jun + MoltBot 🐾
> **仓库**: https://github.com/JunJunHub/openclaw-knowledge-base

---

## 目录结构

```
obsidian/
├── 00-Inbox/           # 收件箱，临时笔记待整理
├── 01-安装部署/         # OpenClaw/Obsidian 安装配置
├── 02-核心概念/         # OpenClaw 核心概念（Agent/Session/Binding）
├── 03-AI编程工具/       # AI 编程工具（Spec-Kit/Superpowers/Claude Code）
├── 04-自动化流程/       # OpenClaw 自动化脚本工具
├── 05-配置指南/         # OpenClaw 各组件配置
├── 06-OPC运营/          # 一人公司运营（内容创作/接单平台）
├── 07-踩坑记录/         # 问题解决、故障排查
├── 08-收益分析/         # OPC 收益追踪
├── 09-资源索引/         # 链接、资源汇总
├── 10-模板/            # 笔记模板
└── 11-Archive/         # 归档
```

---

## 目录说明

| 目录 | 数量 | 用途 |
|------|------|------|
| **01-安装部署** | 3 | OpenClaw、Obsidian 安装配置指南 |
| **02-核心概念** | 2 | OpenClaw Agent、Session、Binding 等核心概念 |
| **03-AI编程工具** | 7 | Spec-Kit、Superpowers、Claude Code 等 AI 编程工具 |
| **04-自动化流程** | 3 | openclaw-recovery 等自动化脚本 |
| **05-配置指南** | 5 | Memory Search、Web Search、飞书等配置 |
| **06-OPC运营** | 2 | 小红书发布、接单平台等运营内容 |
| **07-踩坑记录** | 8 | 问题解决、故障排查 |

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
2. **反向链接面板**：每个文档底部显示 Backlinks

### 当前知识网络

```
[[AI编码工具生态全景]]
    ├── [[AI编码工具对比-Superpowers-vs-SpecKit]]
    │       ├── [[Superpowers-功能概览]]
    │       └── [[Spec-Kit-完整指南]]
    │               ├── [[Spec-Kit-多Agent模式]]
    │               └── [[Spec-Kit-技术栈工具]]

[[多Agent架构详解]]
    └── [[多Agent消息互通指南]]
```

---

## 知识库整理原则

### 1. 主题分离

- OpenClaw 相关文档与 AI 编程工具分开
- 技术文档与运营文档分开
- 每个目录职责单一

### 2. 去重合并

- 同一主题的多个文档合并为一篇
- 保留最详细、最新的版本
- 删除过时或重复的内容

### 3. 添加双链

每个文档开头添加分类和相关文档：

```markdown
# 文档标题

> **分类**：目录名
> **相关文档**：[[相关文档1]] | [[相关文档2]]

内容...
```

---

## 自动同步

### 数据来源

```
workspace/memory/           →  整理后入库
workspace-thinker/research/ →  03-AI编程工具/
workspace-coder/            →  07-踩坑记录/
workspace-media/content/    →  06-OPC运营/
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

1. 确定分类目录（参考目录说明）
2. 使用 `write` 工具创建文档
3. 添加分类和相关文档双链
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

## 最近更新

**2026-03-26 知识库重组**：
- ✅ 创建 `03-AI编程工具/` 目录（7篇）
- ✅ 创建 `06-OPC运营/` 目录（2篇）
- ✅ 删除重复文档（2篇）
- ✅ 添加 Obsidian 双链支持

---

*最后更新：2026-03-26*
