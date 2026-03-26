# Superpowers 仓库功能概览

> **GitHub**: https://github.com/obra/superpowers
> **开发者**: Jesse Vincent

Superpowers 是一个 **AI 编码助手技能框架**，为 Claude Code、Cursor、Codex 等 AI 编程工具提供完整的软件开发工作流。

---

## 核心功能

### 1. 开发流程自动化
- **brainstorming** — 编码前先头脑风暴，通过提问细化想法，探索替代方案
- **writing-plans** — 将工作拆分成 2-5 分钟的小任务，包含精确文件路径和验证步骤
- **executing-plans** — 批量执行计划，设置人工检查点

### 2. 测试驱动开发 (TDD)
- **test-driven-development** — 强制执行 RED-GREEN-REFACTOR 循环：
  - 先写失败测试 → 看它失败 → 写最小代码 → 看它通过 → 提交

### 3. 子代理协作
- **subagent-driven-development** — 为每个任务派发独立子代理，两阶段审查（规格合规性 + 代码质量）
- **dispatching-parallel-agents** — 并发子代理工作流

### 4. Git 工作流
- **using-git-worktrees** — 在新分支创建隔离工作区
- **finishing-a-development-branch** — 验证测试后提供合并/PR/保留/丢弃选项

### 5. 代码审查
- **requesting-code-review** — 任务间审查，按严重程度报告问题
- **receiving-code-review** — 响应反馈

### 6. 元技能
- **writing-skills** — 创建新技能的最佳实践指南

---

## 设计哲学

| 原则 | 说明 |
|------|------|
| 测试驱动 | 永远先写测试 |
| 系统化 | 流程优于猜测 |
| 降低复杂度 | 简洁为首要目标 |
| 证据导向 | 验证后再宣布成功 |

---

## 支持平台

- Claude Code（官方插件市场）
- Cursor
- Codex
- OpenCode
- Gemini CLI

---

## 安装方式

### Claude Code
```
/plugin install superpowers@claude-plugins-official
```

### Codex
按照 https://raw.githubusercontent.com/obra/superpowers/refs/heads/main/.codex/INSTALL.md 操作

### Gemini CLI
```
gemini extensions install https://github.com/obra/superpowers
```

---

## 相关链接

- **GitHub**: https://github.com/obra/superpowers
- **Marketplace**: https://github.com/obra/superpowers-marketplace
- **Lab (实验性技能)**: https://github.com/obra/superpowers-lab
