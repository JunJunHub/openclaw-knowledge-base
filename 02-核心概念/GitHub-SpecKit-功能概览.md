# GitHub Spec Kit 功能概览

> **GitHub**: https://github.com/github/spec-kit
> **开发者**: GitHub 官方

GitHub 官方开源的规格驱动开发工具包，提供 CLI、模板和提示，将工作流程化。

---

## 核心理念

**规格驱动开发 (SDD)**：规格是真理之源，代码是实现产物。

```
Spec → Plan → Tasks → Code
```

---

## 六阶段工作流

```
Constitution → Specify → Plan → Tasks → Implement → Validate
```

### 1. Constitution (宪法)

项目最高指导原则，定义：
- 技术栈锁定
- 架构原则
- 代码规范
- 测试策略
- 安全要求
- 性能要求

**位置**: `.specify/memory/constitution.md`

**命令**: `/speckit.constitution`

### 2. Specify (规格)

定义"做什么"和"为什么"，包含：
- 用户故事
- 验收标准
- 非功能性需求

**命令**: `/speckit.specify`

### 3. Plan (计划)

技术实现方案，包含：
- 数据模型
- API 设计
- 依赖分析
- 验证策略

**命令**: `/speckit.plan`

### 4. Tasks (任务)

可执行任务列表：
- 每个任务 2-5 分钟可完成
- 包含具体文件路径
- 包含验证步骤

**命令**: `/speckit.tasks`

### 5. Implement (实现)

AI 执行编码：
- 读取 constitution 遵循规范
- 参考现有代码风格
- 自动生成测试

**命令**: `/speckit.implement`

---

## 文件结构

```
project/
├── .specify/
│   ├── memory/
│   │   └── constitution.md    # 项目宪法
│   ├── scripts/
│   │   ├── check-prerequisites.sh
│   │   ├── create-new-feature.sh
│   │   └── setup-plan.sh
│   ├── templates/
│   │   ├── spec-template.md
│   │   ├── plan-template.md
│   │   └── tasks-template.md
│   └── specs/
│       └── 001-feature/
│           ├── spec.md
│           ├── plan.md
│           └── tasks.md
└── .github/
    └── prompts/
```

---

## 安装

```bash
# 使用 uv 安装
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git

# 初始化新项目
uvx --from git+https://github.com/github/spec-kit.git specify init my-project

# 指定 AI 平台
specify init my-project --ai claude
specify init my-project --ai cursor-agent
specify init my-project --ai copilot
specify init my-project --ai gemini
```

---

## 支持的 AI 编码助手

- GitHub Copilot
- Claude Code
- Gemini CLI
- Cursor Agent
- Windsurf
- Amp

---

## 相关链接

- **GitHub**: https://github.com/github/spec-kit
- **官方文档**: https://github.github.io/spec-kit/
- **官方博客**: https://github.blog/ai-and-ml/generative-ai/spec-driven-development-with-ai-get-started-with-a-new-open-source-toolkit/
