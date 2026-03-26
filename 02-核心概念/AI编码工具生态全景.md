# AI 编码开发规范生态全景

## 四层架构

```
┌─────────────────────────────────────────────────────────────┐
│  1️⃣ Spec Frameworks (规格框架层)                              │
│  Spec Kit • OpenSpec • BMAD • Intent • cc-sdd               │
├─────────────────────────────────────────────────────────────┤
│  2️⃣ Planning & Task Systems (规划任务层)                     │
│  Taskmaster • Agent OS • Beads • Feature-Driven-Flow        │
├─────────────────────────────────────────────────────────────┤
│  3️⃣ Execution Agents (执行代理层)                             │
│  GSD • Devika • OpenDevin • CrewAI • LangGraph • AutoGen    │
├─────────────────────────────────────────────────────────────┤
│  4️⃣ AI IDEs (集成开发环境层)                                  │
│  Cursor • Claude Code • Kiro • Windsurf                     │
└─────────────────────────────────────────────────────────────┘
```

---

## 1️⃣ 规格框架层

与 Spec Kit 同层，定义需求规格和架构工件。

| 工具 | 特点 | 来源 |
|------|------|------|
| **OpenSpec** | 开放规格标准 | 社区 |
| **BMAD-METHOD** | 多代理编排框架，覆盖多领域 | 企业级 |
| **Intent** | 活规格 + 多代理协调，适合跨服务 | 企业级 |
| **cc-sdd** | Claude Code 专用 SDD 工作流 | 社区 |

生成产物：
```
SPEC.md
ARCHITECTURE.md
TASKS.md
```

---

## 2️⃣ 规划任务层

将规格转换为任务图，AI 代理可执行。

| 工具 | 说明 |
|------|------|
| **Taskmaster** | 任务管理 |
| **Agent OS** | 代理操作系统 |
| **Beads** | 任务编排 |
| **Feature-Driven-Flow** | 功能驱动流程 |

本质是 AI 项目管理工具。

---

## 3️⃣ 执行代理层

实际编写和修改代码的系统。

| 工具 | 特点 | 适用场景 |
|------|------|----------|
| **GSD** | 自主软件工程师代理 | 完整项目开发 |
| **Devika** | 开源 AI 软件工程师 | 端到端开发 |
| **OpenDevin** | 开源自主开发代理 | 研究/实验 |
| **CrewAI** | 多代理协作框架 | 复杂任务编排 |
| **LangGraph** | 状态图驱动的代理工作流 | 可定制流程 |
| **AutoGen** | 微软多代理对话框架 | 企业应用 |

能力：
- 读取仓库
- 修改文件
- 运行测试
- 提交更改

---

## 4️⃣ AI IDE 层

集成规划和执行到开发工具。

| 工具 | 特点 | 平台 |
|------|------|------|
| **Cursor** | `.cursor/rules` 项目规则，最轻量入口 | VS Code 系 |
| **Claude Code** | 内置插件市场，支持 Superpowers 等 | 终端/IDE |
| **Kiro** | AWS 原生，规格驱动前置 | VS Code 系 |
| **Windsurf** | Cascade 代理模式 | VS Code 系 |
| **Sweep AI** | 自动 PR 生成 | GitHub |

---

## 激进派：Spec-as-Source

规格是主要工件，代码是自动生成产物。

| 工具 | 理念 |
|------|------|
| **Tessl** | 规格即源码，代码是编译产物 |
| **Intent 平台** | 类似 SQL → 查询计划，Terraform → 基础设施 |

```
Spec = source code
Code = compiled artifact
```

---

## 选择矩阵

| 你的情况 | 推荐组合 |
|----------|----------|
| 个人/小团队快速迭代 | Cursor + Superpowers |
| 企业级合规开发 | Spec Kit + GitHub Copilot |
| 多代理复杂协作 | CrewAI / LangGraph |
| AWS 云原生项目 | Kiro |
| 跨服务大规模系统 | Intent / BMAD |
| 规格优先理念 | Tessl (实验性) |

---

## 相关资源

- **生态地图**: https://medium.com/@visrow/spec-driven-development-is-eating-software-engineering-a-map-of-30-agentic-coding-frameworks-6ac0b5e2b484
- **工具排名**: https://blog.logrocket.com/ai-dev-tool-power-rankings/
