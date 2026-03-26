# Spec Kit 维护现有大型工程完整流程教程

> 适用于 Brownfield（现有代码库）项目采用规格驱动开发

---

## 整体流程图

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    Brownfield 项目 SDD 工作流                                  │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  [现有项目]                                                                   │
│      │                                                                      │
│      ▼                                                                      │
│  ┌──────────────────────────────────┐                                       │
│  │ 阶段 0: Brownfield Bootstrap     │  ◀── 首次采用 Spec Kit               │
│  │ /speckit.brownfield-bootstrap    │                                       │
│  └──────────────┬───────────────────┘                                       │
│                 ▼                                                           │
│  ┌──────────────────────────────────┐                                       │
│  │ 阶段 1: Constitution (宪法)      │  ◀── 项目核心原则                      │
│  │ .specify/memory/constitution.md  │                                       │
│  └──────────────┬───────────────────┘                                       │
│                 ▼                                                           │
│  ┌──────────────────────────────────┐                                       │
│  │ 阶段 2: Specify (规格)           │  ◀── 新需求规格                        │
│  │ /speckit.specify                 │                                       │
│  └──────────────┬───────────────────┘                                       │
│                 ▼                                                           │
│  ┌──────────────────────────────────┐                                       │
│  │ 阶段 3: Plan (计划)              │  ◀── 技术实现方案                      │
│  │ /speckit.plan                    │                                       │
│  └──────────────┬───────────────────┘                                       │
│                 ▼                                                           │
│  ┌──────────────────────────────────┐                                       │
│  │ 阶段 4: Tasks (任务)             │  ◀── 可执行任务列表                    │
│  │ /speckit.tasks                   │                                       │
│  └──────────────┬───────────────────┘                                       │
│                 ▼                                                           │
│  ┌──────────────────────────────────┐                                       │
│  │ 阶段 5: Implement (实现)         │  ◀── AI 执行编码                       │
│  │ /speckit.implement               │                                       │
│  └──────────────────────────────────┘                                       │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 阶段 0: 安装与初始化

### 0.1 安装 Spec Kit CLI

```bash
# 使用 uv 安装 (推荐)
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git

# 或使用 uvx 一次性运行
uvx --from git+https://github.com/github/spec-kit.git specify --help
```

### 0.2 安装 Brownfield 扩展 (关键!)

**为什么需要这个扩展？**
- 标准 `specify init` 只生成通用模板
- Brownfield 扩展会**自动分析现有代码库**，生成项目特定的 constitution 和模板

```bash
# 克隆扩展仓库
git clone https://github.com/wcpaxx/spec-kit-brownfield-extensions

# 复制扩展到你的 AI 工具目录
cp -r spec-kit-brownfield-extensions/extension/* ~/.claude/extensions/
# 或根据你的 AI 工具选择对应目录
```

### 0.3 在现有项目中初始化

```bash
cd /path/to/your/existing-project

# 运行 brownfield bootstrap 命令
# 在 Claude Code 中执行:
/speckit.brownfield-bootstrap.init

# 可选：聚焦特定模块
/speckit.brownfield-bootstrap.init Focus on the user authentication module
```

---

## 阶段 1: 自动生成 Constitution (宪法)

### 1.1 执行命令后 AI 会做什么

Brownfield Bootstrap 扩展会：

```
┌─────────────────────────────────────────────────────────────┐
│  🔍 Automatic Project Discovery (自动项目发现)              │
│  ├── 扫描 package.json / pom.xml / build.gradle / Cargo.toml│
│  ├── 检测技术栈框架 (React/Vue/Spring/Express/Django)       │
│  ├── 分析项目结构 (monorepo/microservice/layered)           │
│  └── 识别命名约定和代码风格                                  │
│                                                             │
│  📋 Multi-Module Support (多模块支持)                        │
│  ├── 识别模块边界                                           │
│  ├── 映射依赖关系                                           │
│  └── 生成模块级 constitution                                │
│                                                             │
│  📄 Customized Templates (定制模板)                          │
│  ├── spec-template.md (规格模板)                            │
│  ├── plan-template.md (计划模板)                            │
│  └── tasks-template.md (任务模板)                           │
│                                                             │
│  🧪 TDD Enforcement (TDD 强制)                              │
│  └── 从现有测试推断测试策略                                  │
└─────────────────────────────────────────────────────────────┘
```

### 1.2 生成的文件结构

```
your-project/
├── .specify/
│   ├── memory/
│   │   └── constitution.md    ◀── 项目宪法 (最重要!)
│   ├── scripts/
│   │   ├── check-prerequisites.sh
│   │   ├── create-new-feature.sh
│   │   ├── setup-plan.sh
│   │   └── update-claude-md.sh
│   ├── templates/
│   │   ├── spec-template.md   ◀── 规格模板
│   │   ├── plan-template.md   ◀── 计划模板
│   │   └── tasks-template.md  ◀── 任务模板
│   └── specs/
│       └── (后续生成的规格文件)
└── .github/
    └── prompts/
        └── (AI 提示模板)
```

### 1.3 Constitution 示例内容

AI 会根据代码库自动生成类似以下内容：

```markdown
# Constitution: 项目宪法

## 1. 技术栈锁定
- 后端: Spring Boot 3.2 + Java 21
- 前端: React 18 + TypeScript 5.0
- 数据库: PostgreSQL 15
- 缓存: Redis 7.0

## 2. 架构原则
- 遵循领域驱动设计 (DDD)
- API 必须符合 RESTful 规范
- 所有服务必须是无状态的
- 数据库事务必须在 Service 层管理

## 3. 代码规范
- 类命名: PascalCase
- 方法命名: camelCase
- 常量命名: UPPER_SNAKE_CASE
- 包结构: com.company.module.layer

## 4. 测试策略
- 单元测试覆盖率 > 80%
- 集成测试覆盖核心业务流程
- 使用 Testcontainers 进行数据库测试

## 5. 安全要求
- 所有 API 必须有 JWT 认证
- 敏感数据必须加密存储
- SQL 查询必须使用参数化

## 6. 性能要求
- API 响应时间 < 200ms (P95)
- 数据库查询必须有索引覆盖
```

### 1.4 审查与调整 Constitution

**关键原则：Constitution 必须是可验证的**

```markdown
❌ 错误示例 (太模糊):
"写高质量的代码"
"API 要快"

✅ 正确示例 (可验证):
"API 响应结构必须遵循 JSON:API 规范"
"所有公共方法必须有 Javadoc 注释"
"单元测试覆盖率 > 80%"
```

---

## 阶段 2: 编写新需求规格

### 2.1 使用 Specify 命令

```bash
# 创建新功能规格
/speckit.specify

# AI 会提示你输入需求，例如:
"添加用户阅读列表功能，用户可以保存文章到列表，支持分类和标签"
```

### 2.2 AI 生成的规格结构

AI 会基于 constitution 和模板生成：

```markdown
# specs/003-reading-list/spec.md

## 功能概述
用户阅读列表功能，允许用户保存、组织和管理文章。

## 用户故事

### US-1: 创建阅读列表
作为用户，我想创建阅读列表，以便组织我要阅读的文章。

**验收标准:**
- [ ] 用户可以创建新列表
- [ ] 列表必须有名称 (1-50 字符)
- [ ] 列表可以选择性添加描述

### US-2: 添加文章到列表
作为用户，我想将文章添加到列表，以便稍后阅读。

**验收标准:**
- [ ] 支持单个添加和批量添加
- [ ] 文章去重 (同一列表不重复)
- [ ] 显示添加时间

## 非功能性需求
- 性能: 列表加载 < 500ms
- 安全: 仅列表所有者可修改
- 可访问性: 符合 WCAG 2.1 AA

## 技术约束
(基于 Constitution 自动引用)
- API 遵循 RESTful 规范
- 使用现有认证系统
- 数据存储在 PostgreSQL
```

### 2.3 规格澄清流程 (可选但推荐)

```bash
# 运行澄清工作流
/speckit.clarify

# AI 会提问细化需求:
"阅读列表是否支持公开分享?"
"是否需要导入/导出功能?"
"标签是预设还是用户自定义?"
```

---

## 阶段 3: 技术实现计划

### 3.1 生成实现计划

```bash
/speckit.plan
```

### 3.2 AI 生成的计划示例

```markdown
# specs/003-reading-list/plan.md

## 技术方案

### 1. 数据模型

CREATE TABLE reading_lists (
    id UUID PRIMARY KEY,
    user_id UUID NOT NULL REFERENCES users(id),
    name VARCHAR(50) NOT NULL,
    description TEXT,
    created_at TIMESTAMP DEFAULT NOW()
);

CREATE TABLE reading_list_items (
    id UUID PRIMARY KEY,
    list_id UUID NOT NULL REFERENCES reading_lists(id),
    article_id UUID NOT NULL REFERENCES articles(id),
    added_at TIMESTAMP DEFAULT NOW(),
    UNIQUE(list_id, article_id)
);

### 2. API 设计

POST   /api/v1/reading-lists           # 创建列表
GET    /api/v1/reading-lists           # 获取用户列表
GET    /api/v1/reading-lists/{id}      # 获取列表详情
PUT    /api/v1/reading-lists/{id}      # 更新列表
DELETE /api/v1/reading-lists/{id}      # 删除列表

POST   /api/v1/reading-lists/{id}/items    # 添加文章
DELETE /api/v1/reading-lists/{id}/items/{itemId}  # 移除文章

### 3. 依赖分析

| 依赖 | 用途 | 版本 |
|------|------|------|
| 现有 | User 服务 | - |
| 现有 | Article 服务 | - |
| 新增 | 无 | - |

### 4. 宪法合规检查

| 宪法条款 | 本计划如何满足 |
|----------|----------------|
| RESTful API | ✅ 使用标准 HTTP 方法和资源命名 |
| JWT 认证 | ✅ 所有 API 需要 Authorization header |
| 测试覆盖 | ✅ 计划包含单元测试和集成测试 |
```

---

## 阶段 4: 任务分解

### 4.1 生成任务列表

```bash
/speckit.tasks
```

### 4.2 AI 生成的任务示例

```markdown
# specs/003-reading-list/tasks.md

## 任务列表

### Task-1: 创建数据模型 [预计 15min]
- [ ] 创建 ReadingList 实体类
- [ ] 创建 ReadingListItem 实体类
- [ ] 创建 JPA Repository
- [ ] 编写数据库迁移脚本
- **验证**: 运行迁移，检查表创建成功

### Task-2: 实现创建列表 API [预计 20min]
- [ ] 创建 ReadingListController
- [ ] 实现 POST /reading-lists 端点
- [ ] 添加请求验证
- [ ] 编写单元测试
- **验证**: curl 测试创建成功返回 201

### Task-3: 实现获取列表 API [预计 15min]
- [ ] 实现 GET /reading-lists 端点
- [ ] 添加分页支持
- [ ] 编写单元测试
- **验证**: curl 测试返回用户列表

### Task-4: 实现添加文章功能 [预计 20min]
- [ ] 实现 POST /reading-lists/{id}/items 端点
- [ ] 添加去重逻辑
- [ ] 编写单元测试
- **验证**: 测试重复添加被拒绝

### Task-5: 编写集成测试 [预计 25min]
- [ ] 使用 Testcontainers 测试完整流程
- [ ] 测试用户权限隔离
- [ ] 测试并发场景
- **验证**: 所有测试通过

### Task-6: 更新 API 文档 [预计 10min]
- [ ] 更新 OpenAPI 规范
- [ ] 添加示例请求/响应
- **验证**: 文档渲染正确
```

---

## 阶段 5: 执行实现

### 5.1 执行任务

```bash
# 执行所有任务
/speckit.implement

# 或分批执行 (推荐用于大型任务)
/speckit.implement --batch 1-3  # 先执行任务 1-3
```

### 5.2 AI 执行过程

```
┌─────────────────────────────────────────────────────────────┐
│  AI 执行流程                                                 │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  Task-1: 创建数据模型                                        │
│  ├── 读取 constitution.md 确认命名规范                       │
│  ├── 读取现有实体类参考代码风格                              │
│  ├── 生成 ReadingList.java                                  │
│  ├── 生成 ReadingListItem.java                              │
│  ├── 生成 V003__reading_lists.sql                           │
│  └── 运行迁移验证 ✅                                         │
│                                                             │
│  Task-2: 实现创建列表 API                                    │
│  ├── 读取现有 Controller 参考结构                            │
│  ├── 生成 ReadingListController.java                         │
│  ├── 生成 ReadingListService.java                           │
│  ├── 生成 ReadingListRequest DTO                            │
│  ├── 编写 ReadingListControllerTest.java                     │
│  └── 运行测试 ✅                                             │
│                                                             │
│  ... (继续执行后续任务)                                       │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

### 5.3 检查点与人工审核

每完成一批任务后，AI 会暂停让你审核：

```
✅ Batch 1 完成 (Tasks 1-3)

生成文件:
- src/main/java/com/app/entity/ReadingList.java
- src/main/java/com/app/entity/ReadingListItem.java
- src/main/java/com/app/controller/ReadingListController.java
- src/test/java/com/app/controller/ReadingListControllerTest.java

测试结果: 12/12 通过

继续执行下一批? [Y/n]
```

---

## Greenfield vs Brownfield 对比

| 方面 | Greenfield (`specify init`) | Brownfield (扩展) |
|------|-----------------------------|-------------------|
| **起点** | 空项目 | 现有代码库 |
| **技术栈** | 你选择 | 自动检测并锁定 |
| **架构** | 你设计 | 逆向工程 |
| **模板** | 通用 | 项目特定 |
| **Constitution** | 最小化 | 从代码分析生成完整版 |
| **多模块** | 基础 | 完整模块映射 |

---

## 最佳实践总结

| 阶段 | 关键要点 |
|------|----------|
| Bootstrap | 使用 brownfield 扩展，让 AI 分析现有代码 |
| Constitution | 必须具体、可验证，避免模糊描述 |
| Specify | 先澄清需求再生成规格 |
| Plan | 每个决策要引用 Constitution 条款 |
| Tasks | 任务粒度控制在 2-5 分钟可完成 |
| Implement | 分批执行，每批人工审核 |

---

## 相关资源

### 官方文档
- **Spec Kit 仓库**: https://github.com/github/spec-kit
- **官方文档**: https://github.github.io/spec-kit/

### Brownfield 扩展
- **扩展仓库**: https://github.com/wcpaxx/spec-kit-brownfield-extensions
- 支持中英文双语

### 视频教程
- **Den Delimarsky 教程**: https://www.youtube.com/watch?v=SGHIQTsPzuY
  - 演示如何在现有项目中添加阅读列表功能
