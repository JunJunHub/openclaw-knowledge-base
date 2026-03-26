# Spec-Kit 多 Agent 模式实战

## Claude Code CLI 版本要求

| 版本 | 发布日期 | 特性 |
|------|---------|------|
| **2.1.32** | 2026-02-05 | 首次引入 Agent Teams |
| 2.1.39+ | 当前 | 功能已完善 |

**验证版本**：
```bash
claude --version
```

---

## Agent Teams 核心能力

- 多个 AI Agent 并行协作
- 分工处理不同任务
- 自动协调工作流程
- 适合大型复杂项目

---

## 启用 Agent Teams

```bash
# 查看当前模型
/model

# 切换到 Opus 4.6（推荐用于多 Agent）
/model opus
```

**注意**：Agent Teams 在 Opus 4.6 上效果最佳。

---

## 多 Agent 场景示例

### 场景：同时开发多个独立模块

```bash
# 不使用 Agent Teams（串行）
/speckit.implement

# 使用 Agent Teams（并行）
# 在 plan 阶段，Claude 会自动识别可并行的任务
# 并分配给多个 Agent 同时执行
```

### 实际操作

```bash
# 1. 先创建多个功能的规格
/speckit.specify 功能A：用户管理模块...

# 另开一个分支
/speckit.specify 功能B：订单管理模块...

# 2. 在实现时，Agent Teams 会自动并行处理
```

---

## 推荐工作流

```
┌─────────────────────────────────────────────────────────┐
│                    开发工作流                            │
├─────────────────────────────────────────────────────────┤
│  1. Spec-Kit 定义需求                                   │
│     ↓                                                   │
│  2. Claude Code 生成任务                                │
│     ↓                                                   │
│  ┌─────────────┬─────────────┬─────────────┐           │
│  │ Go Backend  │ C++ Backend │ Qt/QML UI   │           │
│  │ (Agent 1)   │ (Agent 2)   │ (Agent 3)   │           │
│  └─────────────┴─────────────┴─────────────┘           │
│     ↓                                                   │
│  3. 多 Agent 并行实现                                   │
│     ↓                                                   │
│  4. 自动测试 + 集成验证                                 │
└─────────────────────────────────────────────────────────┘
```

---

*来源：workspace-thinker/docs/spec-kit-claude-code-tutorial.md*
*整理日期：2026-03-26*
