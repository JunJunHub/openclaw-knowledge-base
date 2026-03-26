# Spec-Kit 开发流程详解

## Step 1: 建立项目宪法

在 Claude Code CLI 中执行：

```bash
/speckit.constitution 这是一个 Go 语言后端项目，使用 Gin 框架。请创建以下原则：
1. 代码风格：遵循 Go 官方规范，使用 gofmt 格式化
2. 测试标准：每个新功能必须有单元测试，覆盖率 ≥ 70%
3. API 设计：RESTful 风格，使用 OpenAPI 规范
4. 错误处理：统一错误码体系，日志结构化
5. 数据库：使用 GORM，迁移文件纳入版本控制
```

**输出**：`.specify/memory/constitution.md`

---

## Step 2: 创建需求规格

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

## Step 3: 需求澄清（可选但推荐）

```bash
/speckit.clarify
```

AI 会自动识别规格中的模糊点，逐个询问澄清。

---

## Step 4: 创建技术实现计划

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
- `specs/001-user-management/plan.md`
- `specs/001-user-management/data-model.md`
- `specs/001-user-management/research.md`

---

## Step 5: 生成任务分解

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

## Step 6: 执行实现

```bash
/speckit.implement
```

AI 会按任务顺序逐一实现，遵循 TDD 流程。

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

*来源：workspace-thinker/docs/spec-kit-claude-code-tutorial.md*
*整理日期：2026-03-26*
