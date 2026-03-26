# Obsidian CLI + AI Agent 集成前沿策略

> 调研时间：2026-03-26
> 来源：社区最佳实践 + GitHub 项目

---

## 一、三大集成路径

### 路径 1：Obsidian CLI + Claude Code Skills（推荐）

**原理**：Claude Code 在 Vault 目录运行，通过 Skills 赋予操作 Vault 的能力。

**核心组件**：
- `obsidian-cli` skill — 教 Claude 如何使用 Obsidian CLI
- `CLAUDE.md` — Vault 结构说明 + 操作约定
- Custom Skills — 自定义工作流

**前置条件**：
1. Obsidian 设置中启用 CLI（Settings → Core → Obsidian CLI）
2. 在 Vault 目录启动 Claude Code

**CLAUDE.md 生成流程**：
```
1. 让 Claude 探索现有 Vault 结构
2. 理解每个文件夹用途和命名约定
3. 自动生成结构化 CLAUDE.md
4. 包含写作风格和常用交互方式
```

**典型工作流**：
- 周反思：Claude 询问问题 → 遍历本周笔记 → 生成周报
- 内容创作：读取灵感库 → 生成初稿 → 更新状态
- 知识重组：分析笔记关联 → 建议合并/拆分

---

### 路径 2：Obsidian MCP Server（Claude Desktop / Cursor）

**原理**：通过 Model Context Protocol 暴露 Vault 操作工具。

**主流 MCP Server 对比**：

| 项目 | 特点 | 工具数 |
|------|------|--------|
| `StevenStavrakis/obsidian-mcp` | 简单易用，支持多 Vault | 11 个工具 |
| `cyanheads/obsidian-mcp-server` | 功能全面，支持缓存和容错 | 15+ 工具 |

**可用工具**：
```
read-note        # 读取笔记
create-note      # 创建笔记
edit-note        # 编辑笔记（append/prepend/overwrite）
delete-note      # 删除笔记
search-vault     # 全局搜索（文本/正则）
add-tags         # 添加标签
rename-tag       # 批量重命名标签
list-available-vaults  # 多 Vault 支持
```

**安装配置（Claude Desktop）**：
```json
// ~/Library/Application Support/Claude/claude_desktop_config.json
{
  "mcpServers": {
    "obsidian": {
      "command": "npx",
      "args": ["-y", "obsidian-mcp", "/path/to/vault"]
    }
  }
}
```

**优势**：
- 与 Claude Desktop / Cursor / Windsurf 无缝集成
- 可组合其他 MCP（Todoist、Calendar 等）形成完整生产力系统

---

### 路径 3：Obsidian 插件内嵌 AI CLI

**主流插件**：

| 插件 | 支持的 AI CLI | 特点 |
|------|--------------|------|
| **Obsidian AI CLI** | Claude Code, Gemini CLI, Codex, Qwen | 侧边栏面板，`@file` 引用语法 |
| **Agent Client** | Claude Code, Codex, Gemini CLI | 多线程对话，任务状态指示 |

**核心功能**：
- 自动传递当前文件上下文
- 选中文字直接发送给 AI
- `@filename.md` 引用其他笔记
- 分屏显示 AI 输出

**安装方式**：
1. BRAT 插件市场搜索安装
2. 或手动复制到 `.obsidian/plugins/`

---

## 二、前沿使用场景

### 场景 1：快速写作流（90 分钟 2000 字）

**工作流**：
```
┌─────────────────┐
│ 1. 问题列表写入   │ → Obsidian 笔记
│    Obsidian     │
└────────┬────────┘
         ↓
┌─────────────────┐
│ 2. Claude + MCP │ → "能看到这个文件吗？"
│    逐题提问     │    "问我每个问题"
└────────┬────────┘
         ↓
┌─────────────────┐
│ 3. SuperWhisper │ → 语音回答，避免编辑焦虑
│    语音输入     │
└────────┬────────┘
         ↓
┌─────────────────┐
│ 4. Claude 编辑  │ → 清理口语化、生成结构化段落
│    内联更新     │
└────────┬────────┘
         ↓
┌─────────────────┐
│ 5. 人工精修     │ → 最终润色
└─────────────────┘
```

**关键洞察**：
- 聊天框限制反而成为优势（避免边写边改）
- 语音输入 + AI 清理 = 流畅输出
- AI 先做初稿清理，人工最后润色

---

### 场景 2：自动化内容系统（创作者视角）

**三层文件夹结构**：
```
01_Inspiration/    # 原始素材、链接、趋势分析
02_Production/     # 活跃脚本、分镜、内容
03_Systems/        # 品牌风格指南、Prompt 库
```

**Claude Code 自动化能力**：
| 工作流阶段 | 自动化程度 |
|-----------|-----------|
| 研究与拆解 | 高（Agent 驱动分析）|
| 知识管理 | 中（人工策划）|
| 脚本转分镜 | 完全自动 |
| 视觉增强 | 高（生成填充）|

**Style Guide Prompt Library**：
- 存储在 `03_Systems/`
- 确保跨内容品牌一致性
- Claude 自动应用风格指南

---

### 场景 3：Obsidian Bases + Claude Code（无文件夹系统）

**核心理念**：
> 没有文件夹。每个笔记创建时自动组织自己。

**架构**：
- Categories（类别）代替文件夹
- Subjects（主题）自动关联
- Templates（模板）驱动捕获

**Claude Code 角色**：
- 管理整个系统运行
- 自动读取模板生成内容
- 更新数据库状态字段

**示例**：
```
"写一篇关于 AI 的周报，参考我之前的风格，
完成后更新状态为 ready"
→ Claude 自动找到参考、生成内容、更新字段
```

---

### 场景 4：双 Vault 策略（保护主库）

**痛点**：AI 生成的文本可能污染个人笔记

**解决方案**：
```
主 Vault（个人）  ←── AI 只读
      │
      └──→ 镜像 Vault（AI 沙盒）
           └── 存放摘要、索引、分析
```

**典型用例**：
- AI 在镜像 Vault 生成周总结
- 主 Vault 保持纯净
- 需要时人工挑选有价值内容合并

---

## 三、与 OpenClaw 的结合点

### 已有能力

OpenClaw 已具备：
- ✅ Obsidian vault 路径：`~/.openclaw/obsidian/`
- ✅ Git 自动同步（22:30 推送）
- ✅ Memory Search（语义搜索笔记）
- ✅ Heartbeat 机制（定期整理）

### 可增强方向

| 方向 | 实现方式 | 价值 |
|------|---------|------|
| **CLI Skill** | 创建 `obsidian-vault` skill | 赋予 Agent 直接操作 Vault 能力 |
| **MCP 集成** | 配置 obsidian-mcp 到 OpenClaw | 标准 MCP 工具调用 |
| **自动整理** | Heartbeat + obsidian-cli | 定期重组、打标签、建索引 |
| **内容生成** | 结合热点采集 + 模板 | 自动生成初稿存入 Vault |

### 推荐实现路径

```
Phase 1: 配置 obsidian-mcp
         └── 让 OpenClaw 能读写 Vault

Phase 2: 创建 obsidian-vault skill
         └── 封装常用操作（周总结、标签管理等）

Phase 3: 心跳任务增强
         └── 定期执行知识库维护任务

Phase 4: 内容流水线
         └── 热点采集 → 生成初稿 → 存入 Vault
```

---

## 四、资源链接

### MCP Server
- https://github.com/StevenStavrakis/obsidian-mcp
- https://github.com/cyanheads/obsidian-mcp-server

### Obsidian 插件
- https://github.com/blackdragonbe/obsidian-ai-cli
- Obsidian 社区插件市场：Agent Client

### 教程文章
- [Turning Obsidian into an AI-Native Knowledge System](https://medium.com/@martk/turning-obsidian-into-an-ai-native-knowledge-system-with-claude-code-27cb224404cf)
- [Writing 2,000 words in 90 minutes with Obsidian + MCP + Claude](https://www.haihai.ai/obsidian-mcp/)
- [Claude + Obsidian Got a Level Up](https://www.eleanorkonik.com/p/claude-obsidian-got-a-level-up)

### 视频
- [My Entire Second Brain System: Obsidian Bases + Claude Code](https://www.youtube.com/watch?v=r4nea7QCkfQ)

---

## 五、关键洞察

1. **Obsidian 1.12 原生 CLI 支持** — 不再需要 REST API 插件
2. **Claude Code Skills 是未来方向** — 比 MCP 更深度集成
3. **语音 + AI 编辑 = 写作加速器** — 聊天框限制反而成为优势
4. **双 Vault 策略保护主库** — AI 沙盒 + 人工精选
5. **风格指南库保证一致性** — 存在 Vault 中让 AI 自动应用

---

*整理自社区最佳实践，持续更新*
