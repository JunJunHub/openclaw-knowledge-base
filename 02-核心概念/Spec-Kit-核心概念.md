# Spec-Kit 核心概念

## 什么是 Spec-Kit

Spec-Kit 是 GitHub 开源的 **Spec-Driven Development（规格驱动开发）** 工具包。

### 传统开发 vs SDD

| 传统开发 | Spec-Driven Development |
|---------|------------------------|
| 代码为王，规格只是临时文档 | 规格是可执行的，直接生成实现 |
| 随意编码，后期难以维护 | 有序流程，规格驱动每一步 |
| AI 瞎猜实现方式 | AI 按规格精确实现 |

---

## 核心流程

```
Constitution（项目原则）
    ↓
Specify（需求规格）
    ↓
Plan（技术计划）
    ↓
Tasks（任务分解）
    ↓
Implement（实现）
```

### 各阶段说明

| 阶段 | 命令 | 输出 |
|------|------|------|
| Constitution | `/speckit.constitution` | 项目"宪法"文件 |
| Specify | `/speckit.specify` | 需求规格文档 |
| Clarify | `/speckit.clarify` | 澄清后的需求 |
| Plan | `/speckit.plan` | 技术计划 + 数据模型 |
| Tasks | `/speckit.tasks` | 任务分解清单 |
| Implement | `/speckit.implement` | 实现代码 |

---

## 关键特点

1. **规格即文档**：规格文件直接驱动实现，不会过时
2. **AI 友好**：让 AI 有明确的目标和约束
3. **可追溯**：每个功能都有完整的规格链
4. **渐进式**：可以只在部分功能使用

---

## 参考资源

- GitHub: https://github.com/github/spec-kit
- Claude Code 文档: https://code.claude.com/docs
- Go Brownfield 示例: https://github.com/mnriem/spec-kit-go-brownfield-demo

---

*来源：workspace-thinker/docs/spec-kit-claude-code-tutorial.md*
*整理日期：2026-03-26*
