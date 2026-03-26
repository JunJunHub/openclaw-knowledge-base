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

> **适用场景**：现有代码库采用 Spec-Kit 工作流
> **扩展仓库**：https://github.com/wcpaxx/spec-kit-brownfield-extensions

#### 双组件架构

Brownfield 扩展包含两个独立组件：

```
spec-kit-brownfield-extensions/
├── extension/               # spec-kit Extension
│   └── commands/
│       ├── init.md          # 英文初始化命令
│       └── init-cn.md       # 中文中文初始化命令
└── skills/                  # Claude Code Skills
    ├── brownfield-skills/   # 开发者专长生成器
    └── brownfield-ears/     # EARS 需求转换器
```

#### 组件用途

| 组件 | 类型 | 用途 | 何时使用 |
|------|------|------|----------|
| `/speckit.brownfield-bootstrap.init` | Extension 命令 | 初始化现有项目的 SDD 工作流 | 首次在现有项目中使用 Spec Kit |
| `brownfield-skills` | Claude Code Skill | 生成资深开发者专长 | 让 AI 理解项目约定、技术栈、架构 |
| `brownfield-ears` | Claude Code Skill | 需求 → EARS 格式转换 | 复杂需求规格化（`/speckit.specify` 前） |

#### 安装步骤

```bash
# 1. 克隆仓库
git clone https://github.com/wcpaxx/spec-kit-brownfield-extensions
cd spec-kit-brownfield-extensions

# 2. 全局安装 Extension 命令（所有项目共享）
mkdir -p ~/.claude/commands
cp extension/commands/init.md ~/.claude/commands/speckit.brownfield-bootstrap.init.md
cp extension/commands/init-cn.md ~/.claude/commands/speckit.brownfield-bootstrap.init-cn.md

# 3. 全局安装 EARS 技能（通用工具）
mkdir -p ~/.claude/skills
cp -r skills/brownfield-ears ~/.claude/skills/

# 4. 项目级安装 brownfield-skills（在目标项目中执行）
cd /path/to/your-project
mkdir -p .claude/skills
cp -r /path/to/spec-kit-brownfield-extensions/skills/brownfield-skills .claude/skills/
```

#### 全局 vs 项目级安装

| 组件 | 推荐位置 | 原因 |
|------|----------|------|
| Extension 命令 | **全局** `~/.claude/commands/` | 初始化命令是通用工具 |
| brownfield-skills | **项目级** `.claude/skills/` | 运行后生成项目特定文档 |
| brownfield-ears | **全局** `~/.claude/skills/` | EARS 格式转换是通用技能 |

#### 初始化现有项目

```bash
cd /path/to/your/existing-project

# 英文版
/speckit.brownfield-bootstrap.init

# 中文版
/speckit.brownfield-bootstrap.init-cn

# 可选：聚焦特定模块
/speckit.brownfield-bootstrap.init Focus on the user authentication module
```

#### 命令别名（向后兼容）

| 旧命令 | 新命令 |
|--------|--------|
| `speckit.brownfield.bootstrap` | `speckit.brownfield-bootstrap.init` |
| `speckit.brownfield.bootstrap-cn` | `speckit.brownfield-bootstrap.init-cn` |

### Spec Kit CLI 升级与维护

> **当前最新版本**：v0.4.2（截至 2026-03-26）
> **版本检查**：`specify --version 2>/dev/null || echo "未安装"`

#### 🔍 检查当前版本

```bash
# 检查是否已安装
specify --version 2>/dev/null || echo "Specify CLI 未安装"

# 查看 uv 已安装工具
uv tool list 2>/dev/null | grep specify

# 查看 GitHub 最新版本
curl -s https://api.github.com/repos/github/spec-kit/releases | grep -o '"tag_name": "[^"]*"' | head -1
```

#### ⚡ 升级方式

##### 方法 1: uv 重新安装（推荐）

```bash
# 重新安装到最新版本
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git --force

# 或指定特定版本
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git@v0.4.2
```

##### 方法 2: 使用 uvx 一次性运行（无需安装）

```bash
# 使用最新版本（每次从 GitHub 拉取）
uvx --from git+https://github.com/github/spec-kit.git specify --version
```

#### 🎯 是否需要升级？

| **情况** | **建议** |
|----------|----------|
| 首次安装 | 直接安装最新版 `v0.4.2` |
| 现有版本低于 `v0.4.0` | **推荐升级**（可能有重大变更） |
| 现有版本 `v0.4.x` | 按需升级（修复 bug、新功能） |
| 生产环境 | 先测试再升级，注意向后兼容性 |

#### 📝 升级后验证

```bash
# 1. 验证版本
specify --version

# 2. 验证基本命令
specify init test-project --dry-run

# 3. 验证 Brownfield 扩展兼容性
echo "/speckit.brownfield-bootstrap.init" | claude
```

#### ⚠️ 注意事项

1. **Brownfield 扩展兼容性**：
   - Spec Kit CLI 升级通常不影响 Brownfield 扩展
   - Extension 命令 (`/speckit.brownfield-bootstrap.init`) 独立于 CLI
   - 技能 (`brownfield-skills`/`brownfield-ears`) 独立于 CLI

2. **向后兼容性**：
   - Spec Kit 还在快速迭代阶段 (`v0.x`)
   - 主要版本变更 (`v0.4.x → v0.5.x`) 可能有重大变更
   - 建议阅读 [CHANGELOG](https://github.com/github/spec-kit/blob/main/CHANGELOG.md)

3. **安装位置**：
   - uv 默认安装到 `~/.local/bin/` 或 uv 工具目录
   - 确保 `$PATH` 包含 uv 工具路径

4. **完整升级流程**：
   ```bash
   # 1. 备份现有项目配置
   cp -r .specify/ .specify.backup/
   
   # 2. 升级 Spec Kit CLI
   uv tool install specify-cli --from git+https://github.com/github/spec-kit.git --force
   
   # 3. 验证新版本
   specify --version
   
   # 4. 测试现有功能
   specify check
   
   # 5. 如有问题恢复备份
   mv .specify.backup/ .specify/
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

## spec-kit 生态系统

### 演进图谱
```
GitHub spec-kit (官方核心)
├── Brownfield 扩展（GitHub 社区）
│   ├── wcpaxx/spec-kit-brownfield-extensions
│   └── 多个语言示例项目
└── Marty Bonacci 项目线
    ├── spec-kit-extensions (v1.0) → Agent-agnostic 扩展
    │   ├── 5个额外工作流（bugfix/modify/refactor/hotfix/deprecate）
    │   └── 支持多 AI 工具（Claude Code/Copilot/Cursor/Windsurf）
    └── SpecSwarm (v5.0) → Claude Code 专用插件
        ├── 一体化开发体验
        ├── 自然语言优先
        └── 质量评分系统（0-100分）
```

### 1. 核心项目
| 项目 | 类型 | 开发者 | 状态 |
|------|------|--------|------|
| **GitHub spec-kit** | CLI 工具包 | GitHub 官方 | 持续维护 |
| **Brownfield 扩展** | 适配扩展 | wcpaxx 社区 | 活跃 |
| **spec-kit-extensions** | 工作流扩展 | Marty Bonacci | **开发重点已转向 SpecSwarm** |
| **SpecSwarm** | Claude Code 插件 | Marty Bonacci | 主要项目 |

### 2. spec-kit-extensions 详解

**定位**：Agent-agnostic（与 AI 代理无关）的扩展，增加 5 个关键工作流

**解决的问题**：
- 原版 spec-kit 只覆盖全新功能开发（约 25% 工作量）
- 缺少对现有代码库的完整生命周期支持

**5 个额外工作流**：
| 工作流 | 命令 | 核心特性 |
|--------|------|----------|
| Bug 修复 | `/speckit.bugfix "description"` | 回归测试优先 |
| 功能修改 | `/speckit.modify NNN "description"` | 自动影响分析 |
| 代码重构 | `/speckit.refactor "description"` | 基准指标对比 |
| 生产紧急修复 | `/speckit.hotfix "incident"` | 事后分析强制 |
| 功能弃用 | `/speckit.deprecate NNN "reason"` | 3阶段日落计划 |

**关键特性**：
- ✅ **检查点式工作流** - 用户在每个阶段评审和调整
- ✅ **自动影响分析** - `/modify` 自动扫描相关文件
- ✅ **嵌套修改记录** - 修改组织在父功能目录下
- ✅ **多 AI 工具支持** - Claude Code、Copilot、Cursor、Windsurf 等

### 3. 与 SpecSwarm 的关系

**时间线**：
- **早期**：spec-kit-extensions（通用扩展，支持多工具）
- **现在**：SpecSwarm（Claude Code 专用，深度集成）

**作者建议**：
> "🚀 Claude Code users: Check out [SpecSwarm](https://github.com/MartyBonacci/specswarm)"

**技术路线**：
```
spec-kit-extensions (通用) → SpecSwarm (Claude 专用)
    多工具支持                    深度集成
    Agent-agnostic               Claude Code 原生
    配置较复杂                   安装即用
    检查点工作流                 自然语言工作流
```

### 4. 选择建议

| 使用场景 | 推荐方案 | 理由 |
|----------|----------|------|
| **单一 Claude Code 用户** | **SpecSwarm** | 一体化体验，自然语言优先 |
| **需要多 AI 工具支持** | **spec-kit-extensions** | Agent-agnostic 设计 |
| **现有项目增强 spec-kit** | **两者皆可** | 根据使用的 AI 工具选择 |
| **学习 spec-kit 概念** | **spec-kit-extensions** | 更接*近原版理念 |

---

## 参考资源

| 资源 | 链接 | 说明 |
|------|------|------|
| Spec-Kit 官方仓库 | https://github.com/github/spec-kit | GitHub 官方 SDD 工具包 |
| 官方文档 | https://github.github.io/spec-kit/ | 完整使用文档 |
| Brownfield 扩展 | https://github.com/wcpaxx/spec-kit-brownfield-extensions | 现有项目适配扩展 |
| Go Brownfield 示例 | https://github.com/mnriem/spec-kit-go-brownfield-demo | Go 项目示例 |
| **spec-kit-extensions** | **https://github.com/MartyBonacci/spec-kit-extensions** | **5个关键工作流扩展** |
| **SpecSwarm** | **https://github.com/MartyBonacci/specswarm** | **Claude Code 专用插件** |
| **第三方扩展生态** | **[[Spec-Kit-第三方扩展生态]]** | **完整第三方扩展调研报告** |

## 相关工具

| 工具 | GitHub | 类型 | 与 Spec-Kit 关系 |
|------|--------|------|------------------|
| **SpecSwarm** | https://github.com/MartyBonacci/specswarm | Claude Code 插件 | Spec-Kit 理念的 Claude 插件实现 |
| **spec-kit-extensions** | https://github.com/MartyBonacci/spec-kit-extensions | Agent-agnostic 扩展 | 5个关键工作流（bugfix/modify/refactor/hotfix/deprecate） |
| **Superpowers** | https://github.com/jvns/awesome-claude-code#superpowers | Claude Code 技能 | 互补工具，侧重 TDD + 子代理 |
| **AI编码工具对比** | 见 [[AI编码工具对比-Superpowers-vs-SpecKit-vs-SpecSwarm]] | 对比指南 | 详细功能对比与选择建议 |

---

*整理日期：2026-03-26*
*来源：workspace-thinker/docs/spec-kit-claude-code-tutorial.md + 知识库合并*
