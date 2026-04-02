# Claude Code CLI 完整开发指南

> **相关文档**：[[Superpowers-完整教程与开发流程指南]] | [[AI编码工具对比-Superpowers-vs-SpecKit]]
> 
> **GitHub**: https://github.com/anthropics/claude-code
> **官方文档**: https://docs.anthropic.com/en/docs/claude-code
> **更新日期**: 2026-03-27

Claude Code 是 Anthropic 推出的 **AI 编码代理**，运行在终端中。它不是一个聊天机器人，也不是自动补全工具——而是一个能读取文件、执行命令、浏览网页、调用 API 的**自主代理**。你描述你想要什么，它负责怎么做。

---

## 🎯 核心概念：什么是 Claude Code？

### 不是什么
- ❌ 不是聊天机器人
- ❌ 不是自动补全
- ❌ 不是 IDE 插件

### 是什么
- ✅ **终端里的 `while True` 循环**：接任务 → 选工具 → 运行 → 看结果 → 下一步
- ✅ **真实工具访问**：文件读写、bash、网页搜索、MCP 服务器
- ✅ **无 IDE 中间层**：直接在项目目录中运行

### 定价
| 计划 | 价格 | 适用场景 |
|------|------|---------|
| **Claude Pro** | $20/月 | 有限额，适合试用和学习 |
| **Claude Max** | $100/月 | 无限制，适合日常使用 |

**安装**：需要 Claude 订阅 + `npm install -g @anthropic-ai/claude-code`

---

## 📦 安装与配置

### 安装步骤
```bash
# 1. 确保已安装 Node.js 18+
node --version

# 2. 全局安装 Claude Code
npm install -g @anthropic-ai/claude-code

# 3. 首次运行（需要接受权限）
claude

# 4. 进入项目目录使用
cd your-project
claude
```

### 配置文件结构
```
~/.claude/
├── .mcp.json          # MCP 服务器配置
├── hooks/             # 自动化钩子
│   ├── pre-commit.sh
│   └── post-tool.sh
├── skills/            # 技能文件夹
│   ├── skill-1/
│   │   └── SKILL.md
│   └── skill-2/
│       └── SKILL.md
└── config.json        # 全局配置

项目根目录/
├── CLAUDE.md          # 项目记忆文件（最重要！）
└── .claude/           # 项目级配置
    └── agents/        # 代理定义
```

---

## 💻 常用命令速查表

### 基础命令
| 命令 | 功能 | 示例 |
|------|------|------|
| `claude` | 启动交互式会话 | `claude` |
| `claude "任务描述"` | 直接执行任务 | `claude "添加用户认证"` |
| `claude --help` | 查看帮助 | `claude --help` |
| `claude --version` | 查看版本 | `claude --version` |

### 会话管理
| 命令 | 功能 |
|------|------|
| `/clear` | 清除上下文（开始新任务） |
| `/compact` | 压缩上下文（谨慎使用） |
| `/resume` | 恢复上次会话 |
| `/model` | 切换模型 |

### 工具与集成
| 命令 | 功能 |
|------|------|
| `/mcp` | 管理 MCP 服务器 |
| `/plugin` | 管理插件 |
| `/skill` | 管理技能 |

### 高级功能
| 命令 | 功能 |
|------|------|
| `/agents` | 查看代理团队 |
| `/worktree` | Git 工作树管理 |
| `--worktree` / `-w` | 在隔离工作树中运行 |

### 重要提示
- **使用 `/clear` 开始新任务** — 不要使用 `/compact` 除非真的需要上下文
- **小步迭代** — 完成后立即 `/clear`
- **`CLAUDE.md` 保持精简** — 只包含每个会话都需要的内容

---

## 🔌 插件配置

Claude Code 支持通过插件扩展功能。插件通过 `settings.json` 配置启动脚本，而非直接运行命令。

### claude-hud 插件示例

**配置位置**：`~/.claude/settings.json`

```json
{
  "hooks": {
    "SessionStart": [
      {
        "command": "/home/jun/.claude/plugins/claude-hud/run-hud.sh"
      }
    ]
  }
}
```

**启动脚本内容** (`run-hud.sh`)：
```bash
#!/bin/bash
# Claude HUD wrapper script
PLUGIN_DIR=$(ls -d "${CLAUDE_CONFIG_DIR:-$HOME/.claude}"/plugins/cache/claude-hud/claude-hud/*/ 2>/dev/null | awk -F/ '{ print $(NF-1) "\t" $(0) }' | sort -t. -k1,1n -k2,2n -k3,3n -k4,4n | tail -1 | cut -f2-)
exec "/home/jun/.nvm/versions/node/v22.22.2/bin/node" "${PLUGIN_DIR}dist/index.js"
```

**关键点**：
- 在 `settings.json` 中配置 `hooks.SessionStart` 启动脚本路径
- 脚本会自动查找最新版本的插件并启动
- 使用 Node.js 执行插件的 `dist/index.js`

---

## 🔌 必备 MCP 服务器

MCP (Model Context Protocol) 让 Claude Code 能够与外部服务通信，扩展其能力。

### 核心推荐 MCP

#### 1. **Coolify MCP** — 自动化部署
```json
{
  "mcpServers": {
    "coolify": {
      "command": "npx",
      "args": ["-y", "@masonator/coolify-mcp"],
      "env": {
        "COOLIFY_ACCESS_TOKEN": "${COOLIFY_ACCESS_TOKEN}",
        "COOLIFY_BASE_URL": "${COOLIFY_BASE_URL}"
      }
    }
  }
}
```
**功能**：部署应用、重启服务、检查日志、管理数据库

#### 2. **Telegram MCP** — 消息自动化
```json
{
  "mcpServers": {
    "telegram": {
      "command": "python",
      "args": ["telegram_mcp_proxy.py"],
      "env": {}
    }
  }
}
```
**功能**：读取消息、发送回复、管理频道

#### 3. **GitHub MCP** — 代码仓库管理
**功能**：创建 PR、管理 Issues、代码审查

#### 4. **Codex MCP** — 双模型审查
**功能**：让 Claude 和 OpenAI Codex 交叉检查计划

#### 5. **Chrome MCP** — 浏览器自动化
**功能**：网页抓取、表单填写、截图

### MCP 配置位置
- **全局配置**：`~/.claude/.mcp.json`
- **项目配置**：项目根目录 `.mcp.json`

### MCP 最佳实践
1. **按需配置** — 只添加真正用到的服务器
2. **使用环境变量** — 敏感信息通过 `${VAR_NAME}` 引用
3. **定期更新** — 使用 `npx -y` 确保使用最新版本

---

## 🛠️ 必备 Skills（技能）

Skills 是存储在 `~/.claude/skills/` 中的 Markdown 文件，无需 SDK、无需构建步骤。

### 推荐技能

#### 1. **项目分析技能** — `project-analyzer`
```markdown
# Project Analyzer Skill

## 目的
分析项目结构、依赖关系和代码质量

## 触发条件
- 新项目开始时
- 代码重构前
- 性能优化时

## 执行步骤
1. 读取 package.json / requirements.txt
2. 分析目录结构
3. 识别关键文件
4. 生成项目概览报告
```

#### 2. **数据库查询技能** — `db-query-assistant`
```markdown
# Database Query Assistant

## 目的
生成和优化 SQL 查询

## 功能
- 根据自然语言生成 SQL
- 优化现有查询性能
- 识别索引建议

## 支持的数据库
- PostgreSQL
- MySQL
- SQLite
```

#### 3. **代码审查技能** — `code-reviewer`
```markdown
# Code Reviewer Skill

## 目的
自动化代码审查

## 检查项
- 代码风格一致性
- 潜在 bug
- 性能问题
- 安全漏洞
- 测试覆盖率

## 输出格式
- 问题列表（严重程度排序）
- 修复建议
- 改进示例
```

#### 4. **文档生成技能** — `doc-generator`
```markdown
# Documentation Generator

## 目的
自动生成项目文档

## 生成内容
- README.md
- API 文档
- 架构图
- 使用示例
```

### 技能创建技巧
1. **明确触发条件** — 什么时候使用这个技能
2. **具体执行步骤** — 不要让 Claude 猜测
3. **包含示例** — 提供输入输出示例
4. **避免常见错误** — 参考 [7 Common Mistakes When Writing Claude Code Skills](https://okhlopkov.com/en-write-claude-code-skills-7-mistakes/)

---

## 🧠 记忆管理系统

Claude Code 使用**多层级记忆文件系统**，每次会话开始时自动加载。

### 记忆文件层级（从高到低优先级）

```
~/.claude/CLAUDE.md              # 全局记忆（最高优先级）
项目父目录/CLAUDE.md              # 父级项目记忆
项目根目录/CLAUDE.md              # 项目记忆（最常用）
~/.claude/agents/某代理/CLAUDE.md # 代理级记忆
```

### 核心记忆文件详解

#### 1. **CLAUDE.md** — 项目记忆文件（最重要！）

**位置**：项目根目录
**作用**：在每次会话开始时自动加载，告诉 Claude：
- 项目结构
- 编码规范
- 文件命名约定
- 禁止事项

**最佳实践示例**：
```markdown
# My Obsidian Vault

## Architecture
- **保持文件简短**（<150行）。使用 [[链接]] 导航。
- **AI 永不编辑原始内容**。语音笔记保持原样。

## Voice Messages
收到语音笔记时：
- 如果关于特定项目 → 保存到：projects/X/ai-docs/
- 如果是个人笔记 → 保存到：personal/diary/

然后：
1. 在原始文本下方添加标签和项目链接
2. 提取 TODO → 项目特定 tasks.md
3. 项目事实 → projects/X/overview.md

## Never Do
- 不要修改 .env 文件
- 不要直接推送到 main 分支
- 不要删除备份文件
```

#### 2. **AGENTS.md** — 团队记忆文件

**位置**：项目根目录或 `~/.claude/agents/`
**作用**：定义多个代理的行为规范

**示例结构**：
```markdown
# AGENTS.md - 你的工作空间

## 每次会话前
0. 读取 SHARED_RULES.md
1. 读取 SOUL.md — 这是你是谁
2. 读取 USER.md — 这是你在帮助谁
3. 读取 memory/YYYY-MM-DD.md（今天+昨天）获取最近上下文
4. **如果在主会话中**：也读取 MEMORY.md

## 记忆
你在每个会话开始时都是新的。这些文件是你的连续性：
- **每日笔记**：memory/YYYY-MM-DD.md
- **长期记忆**：MEMORY.md

## 安全
- 不要泄露私人数据
- 运行破坏性命令前询问
- `trash` > `rm`（可恢复 > 永久删除）
```

#### 3. **MEMORY.md** — 长期记忆

**位置**：项目根目录
**作用**：存储跨会话的关键信息

**内容示例**：
```markdown
# MEMORY.md - 长期记忆

## 用户信息
- 姓名: Jun
- 时区: Asia/Shanghai
- GitHub: JunJunHub

## 项目进展
- OpenClaw OPC 项目：月入 ¥2,000+ 目标
- 热点采集脚本：workspace/scripts/hotspot_collector.py
- 知识库：37 篇笔记

## 重要配置
- API Keys: SiliconFlow / Tavily / 飞书
- 浏览器: Google Chrome (.deb)
- 搜索: Tavily (默认)

## 待办事项
- [ ] 配置 XiaohongshuSkills
- [ ] 完善热点采集
- [ ] 百家号、知乎平台注册
```

---

## 💡 高效记忆管理策略

### 策略 1：级联记忆原则
```
全局记忆 (CLAUDE.md)
    ↓
项目记忆 (项目根目录/CLAUDE.md)
    ↓
代理记忆 (agents/某代理/CLAUDE.md)
```

**原则**：
- 高层级文件应该**更精简**
- 低层级文件可以**更具体**
- 避免重复内容

### 策略 2：最小必要原则
**只包含每个会话都需要的内容**：
- ✅ 项目结构
- ✅ 编码规范
- ✅ 禁止事项
- ❌ 临时任务（放到 docs/）
- ❌ 详细文档（引用 @docs/filename.md）

### 策略 3：docs/ 目录引用
不要在 CLAUDE.md 中写详细文档，使用引用：

```markdown
# CLAUDE.md
详细的项目架构见 @docs/architecture.md
API 文档见 @docs/api.md
```

### 策略 4：任务跟踪
使用 Markdown 复选框跟踪任务：

```markdown
## 当前任务
- [x] 完成用户认证
- [ ] 添加密码重置
- [ ] 优化查询性能
```

**不要**使用复杂的记忆系统，简单的复选框足够。

### 策略 5：定期清理
- **每周**：审查 MEMORY.md，移除过时信息
- **每月**：清理 CLAUDE.md，保持精简
- **每季度**：重新组织 docs/ 目录

### 策略 6：上下文卫生
- **新任务 = `/clear`**：不要积累无关上下文
- **完成即清空**：任务完成后立即 `/clear`
- **避免 `/compact`**：除非真的需要保留上下文

---

## 🎣 Hooks（自动化钩子）

Hooks 是在特定事件时自动运行的 shell 脚本。

### 常用 Hooks

#### 1. **Pre-Commit Hook** — 防止提交敏感文件
```bash
# ~/.claude/hooks/pre-commit.sh

# 阻止提交包含凭证文件的提交
if git diff --cached --name-only | grep -qE '\.(env|key|pem)$|creds\.md'; then
  echo "BLOCKED: 尝试提交敏感文件"
  exit 1
fi
```

#### 2. **Post-Tool Hook** — 记录工具调用
```bash
# ~/.claude/hooks/post-tool.sh

# 记录所有工具调用
echo "$(date): $TOOL_NAME" >> ~/.claude/tool-log.txt
```

### Hook 事件
| 事件 | 触发时机 |
|------|---------|
| `PreToolCall` | 工具调用前 |
| `PostToolCall` | 工具调用后 |
| `PreCommit` | Git 提交前 |
| `SessionStart` | 会话开始时 |
| `SessionEnd` | 会话结束时 |

---

## 🤖 Subagents（子代理）与 Teams（团队）

### Subagents
Claude Code 可以生成子代理并行工作：

**示例场景**：
```
用户：分析这个钱包过去 30 天的活动

主代理生成：
├── 数据代理 → 编写并运行 SQL 查询
├── 分析代理 → 检查内部钱包数据库
└── 报告代理 → 整合输出，生成分析报告
```

**优势**：
- 并行处理
- 独立上下文窗口
- 更快的完成速度

### Teams
多个命名代理协作处理共享项目：
- 一个代理收集数据
- 另一个编写章节
- 第三个进行质量审查

---

## 📊 Claude Code vs Cursor

| 场景 | Claude Code | Cursor |
|------|------------|--------|
| **快速内联编辑** | ❌ | ✅ 高亮 → 修改 |
| **探索陌生代码库** | ❌ | ✅ 点击浏览 + AI 解释 |
| **知道确切要改什么** | ❌ | ✅ 指向更快 |
| **需要多种操作** | ✅ bash + API + 编辑 | ❌ |
| **自主工作流** | ✅ 后台代理、定时任务 | ❌ |
| **大规模重构** | ✅ 跨 20 文件 + 测试 | ❌ |
| **外部系统集成** | ✅ MCP 服务器 | ❌ 有限 |

**推荐比例**：90% Claude Code + 10% Cursor（取决于工作类型）

---

## 🚀 最佳实践总结

### 1. CLAUDE.md 编写原则
```markdown
✅ DO:
- 保持精简（<150行）
- 只包含每个会话都需要的内容
- 使用 @docs/ 引用详细文档
- 明确禁止事项

❌ DON'T:
- 写详细教程
- 包含临时任务
- 重复高层级文件的内容
```

### 2. 上下文管理
```
新任务开始 → /clear
小步迭代 → 完成即清空
避免 /compact → 除非真的需要
```

### 3. MCP 服务器选择
```
只添加真正用到的
使用环境变量存储敏感信息
定期更新到最新版本
```

### 4. Skills 创建
```
明确触发条件
提供具体步骤
包含输入输出示例
避免常见错误
```

### 5. Hooks 使用
```
Pre-Commit → 防止提交敏感文件
Post-Tool → 记录工具调用
SessionStart → 自动加载配置
SessionEnd → 清理临时文件
```

---

## 📚 学习资源

### 官方资源
- **GitHub**: https://github.com/anthropics/claude-code
- **官方文档**: https://docs.anthropic.com/en/docs/claude-code
- **社区论坛**: Claude Code Community

### 推荐文章
- [Claude Code Memory Management: The Complete Guide (2026)](https://medium.com/data-science-collective/claude-code-memory-management-the-complete-guide-2026-b0df6300c4e8)
- [My Claude Code Setup: MCP, Hooks, Skills — Real Usage 2026](https://okhlopkov.com/claude-code-setup-mcp-hooks-skills-2026/)
- [7 Common Mistakes When Writing Claude Code Skills](https://okhlopkov.com/en-write-claude-code-skills-7-mistakes/)
- [The Complete Guide to AI Agent Memory Files](https://hackernoon.com/the-complete-guide-to-ai-agent-memory-files-claudemd-agentsmd-and-beyond)

### 实战案例
- **区块链分析**：SQL 查询生成 + 数据可视化 + 报告生成
- **内容创作**：语音笔记 → 结构化文档 → 多平台发布
- **代码重构**：跨文件修改 + 自动测试 + Git 提交

---

## ⚡ 快速开始清单

### 第 1 步：安装（5分钟）
```bash
npm install -g @anthropic-ai/claude-code
claude  # 首次运行接受权限
```

### 第 2 步：创建 CLAUDE.md（10分钟）
```markdown
# My Project

## 项目结构
- src/ - 源代码
- tests/ - 测试文件
- docs/ - 文档

## 编码规范
- 使用 2 空格缩进
- 变量使用 camelCase
- 函数必须有 JSDoc 注释

## Never Do
- 不要修改 .env 文件
- 不要直接推送到 main
```

### 第 3 步：配置基础 MCP（5分钟）
```json
// ~/.claude/.mcp.json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"]
    }
  }
}
```

### 第 4 步：开始使用（立即）
```bash
cd your-project
claude "帮我添加用户认证功能"
```

---

## 🎓 总结：Claude Code 的核心价值

Claude Code 不是工具，而是**工作方式的转变**：

### Before Claude Code
- 15 个浏览器标签：文档、Stack Overflow、GitHub Issues...
- 在 SQL 编辑器、终端、浏览器、Slack 之间切换
- 手动编写代码、运行测试、格式化输出

### After Claude Code
- **1 个终端窗口**：描述需求 → Claude 执行
- **从写代码到审代码**：AI 写，你审查
- **从执行到设计**：你决定"做什么"，AI 负责"怎么做"

**关键洞察**：Claude Code 将 AI 从"有时偏离轨道的助手"转变为"遵循成熟流程的系统化开发伙伴"。

---

*开始你的 Claude Code 之旅：安装、创建 CLAUDE.md、配置 MCP，然后看着你的生产力飞跃。*