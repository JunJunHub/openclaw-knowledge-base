# Spec-Kit 完整指南

> **分类**：AI 编程工具
> **相关文档**：[[AI编码工具生态全景]] | [[AI编码工具对比-Superpowers-vs-SpecKit]] | [[Superpowers-功能概览]] | [[Spec-Kit-多Agent模式]] | [[Spec-Kit-技术栈工具]]

---

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
    ↓
Validate（验证）
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

## 安装与初始化

### 安装 Spec Kit CLI

```bash
# 使用 uv 安装 (推荐)
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git

# 或使用 uvx 一次性运行
uvx --from git+https://github.com/github/spec-kit.git specify --help
```

### 初始化新项目

```bash
specify init my-project --ai claude
```

### Brownfield：在现有项目中初始化

```bash
# 安装 Brownfield 扩展 (关键!)
git clone https://github.com/wcpaxx/spec-kit-brownfield-extensions
cp -r spec-kit-brownfield-extensions/extension/* ~/.claude/extensions/

# 在项目中初始化
cd /path/to/your/existing-project
/speckit.brownfield-bootstrap.init
```

---

## 阶段详解

### Step 1: 建立项目宪法

```bash
/speckit.constitution 这是一个 Go 语言后端项目，使用 Gin 框架。请创建以下原则：
1. 代码风格：遵循 Go 官方规范，使用 gofmt 格式化
2. 测试标准：每个新功能必须有单元测试，覆盖率 ≥ 70%
3. API 设计：RESTful 风格，使用 OpenAPI 规范
4. 错误处理：统一错误码体系，日志结构化
5. 数据库：使用 GORM，迁移文件纳入版本控制
```

**输出**：`.specify/memory/constitution.md`

**关键原则**：Constitution 必须是可验证的

```markdown
❌ 错误示例 (太模糊):
"写高质量的代码"

✅ 正确示例 (可验证):
"单元测试覆盖率 > 80%"
"所有公共方法必须有 Javadoc 注释"
```

---

### Step 2: 创建需求规格

```bash
/speckit.specify 为用户管理模块添加以下功能：
1. 用户注册：支持邮箱和手机号两种注册方式
2. 用户登录：支持密码登录和验证码登录
3. 用户信息修改：头像、昵称、密码
4. 用户状态管理：启用/禁用用户

背景：这是一个企业内部管理系统，用户数据需要与企业组织架构关联。
安全性要求较高，需要操作审计日志。
```

**注意**：只描述"做什么"和"为什么"，不涉及技术实现细节。

**输出**：`specs/001-user-management/spec.md`

---

### Step 3: 需求澄清（可选但推荐）

```bash
/speckit.clarify
```

AI 会自动识别规格中的模糊点，逐个询问澄清。

---

### Step 4: 创建技术实现计划

```bash
/speckit.plan 这个 Go 项目使用以下技术栈：
- Web 框架：Gin
- ORM：GORM + PostgreSQL
- 认证：JWT + Redis 缓存
- 日志：Zap
- 配置：Viper

请基于现有代码结构设计实现方案，尽量复用现有组件。
```

**输出**：
- `specs/001-user-management/plan.md` - 实现计划
- `specs/001-user-management/data-model.md` - 数据模型
- `specs/001-user-management/research.md` - 技术调研

---

### Step 5: 生成任务分解

```bash
/speckit.tasks
```

**输出**：`specs/001-user-management/tasks.md`

```markdown
## Phase 1: 数据模型
- [ ] 创建 User 模型 (models/user.go)
- [ ] 创建数据库迁移文件
- [P] 创建 UserRepository 接口

## Phase 2: 业务逻辑
- [ ] 实现 UserService
- [P] 实现 AuthService
```

`[P]` 标记表示可并行执行的任务。

---

### Step 6: 执行实现

```bash
/speckit.implement

# 或分批执行 (推荐用于大型任务)
/speckit.implement --batch 1-3
```

AI 会按任务顺序逐一实现，遵循 TDD 流程。

---

## Brownfield 最佳实践

### 先让 AI 理解现有代码

```bash
请分析这个 Go 项目的结构，总结：
1. 目录结构和模块划分
2. 主要技术栈和框架
3. 现有的代码风格和约定
4. 测试覆盖情况
```

### 小步快跑

```bash
# 每次只添加一个小功能
/speckit.specify 添加一个简单的健康检查接口：GET /health
/speckit.plan 复用现有的 Gin 路由结构
/speckit.tasks
/speckit.implement
```

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

## Greenfield vs Brownfield

| 方面 | Greenfield | Brownfield |
|------|------------|------------|
| **起点** | 空项目 | 现有代码库 |
| **技术栈** | 你选择 | 自动检测 |
| **架构** | 你设计 | 逆向工程 |
| **模板** | 通用 | 项目特定 |
| **Constitution** | 最小化 | 从代码分析生成 |

---

## 常用命令速查

| 命令 | 用途 |
|------|------|
| `specify init . --ai claude` | 初始化项目 |
| `specify check` | 检查依赖 |
| `/speckit.constitution` | 创建项目原则 |
| `/speckit.specify` | 创建需求规格 |
| `/speckit.clarify` | 澄清模糊需求 |
| `/speckit.plan` | 创建技术计划 |
| `/speckit.tasks` | 分解任务 |
| `/speckit.implement` | 执行实现 |
| `/speckit.analyze` | 一致性分析 |

---

## 学习路径

```
第1天：基础概念
├── 阅读 spec-kit README
├── 理解 SDD 核心理念
└── 安装配置环境

第2-3天：简单练习
├── 在测试项目中练习完整流程
├── 熟悉各命令的输出
└── 理解 generated 文件结构

第4-7天：实战应用
├── 在实际项目中初始化
├── 从小功能开始
├── 逐步增加复杂度
└── 尝试多 Agent 模式

第2周+：高级用法
├── 自定义模板
├── Extensions 和 Presets
└── 集成 CI/CD
```

---

## 参考资源

| 资源 | 链接 |
|------|------|
| Spec-Kit 官方仓库 | https://github.com/github/spec-kit |
| 官方文档 | https://github.github.io/spec-kit/ |
| Brownfield 扩展 | https://github.com/wcpaxx/spec-kit-brownfield-extensions |
| Go Brownfield 示例 | https://github.com/mnriem/spec-kit-go-brownfield-demo |

---

*整理日期：2026-03-26*
*来源：workspace-thinker/docs/spec-kit-claude-code-tutorial.md + 知识库合并*
