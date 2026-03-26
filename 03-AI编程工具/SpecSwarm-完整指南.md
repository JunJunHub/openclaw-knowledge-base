# SpecSwarm 完整指南

> **分类**：AI 编程工具
> **相关文档**：[[AI编码工具生态全景]] | [[AI编码工具对比-Superpowers-vs-SpecKit]] | [[Spec-Kit-完整指南]]
> **官方仓库**：https://github.com/MartyBonacci/specswarm
> **版本**：v5.1.1（2026-03-26）

---

## 什么是 SpecSwarm

SpecSwarm 是 **Claude Code 插件**，为完整的软件开发生命周期提供统一的工作流管理。

**核心定位**：
- ✅ **规格驱动开发**：灵感来自 GitHub spec-kit，但更全面
- 🐛 **Bug 管理**：系统化的修复与回归测试
- 🔧 **代码维护**：重构和功能修改
- 📊 **质量保证**：自动化验证（0-100 分评分）
- 🗣️ **自然语言**：像与队友交流一样使用自然语言

**一句话总结**：
> 构建、修复、维护、分析你的整个软件项目，用一个统一的插件。

---

## 与 GitHub Spec-Kit 的关系

### 灵感来源
```
原作：GitHub spec-kit (规格驱动开发方法论)
│
改编：SpecKit 插件 by Marty Bonacci (2025)
│
增强：SpecSwarm v5.0.0 by Marty Bonacci & Claude Code (2025-2026)
```

### 关键区别
| 方面 | GitHub Spec-Kit | SpecSwarm |
|------|----------------|-----------|
| **分发形式** | CLI 工具链 | Claude Code 插件 |
| **集成深度** | 独立工具 | 深度集成到 Claude Code |
| **命令数量** | 6 个核心阶段 | 10 个可视命令 + 11 个内部命令 |
| **自然语言** | 有限支持 | ⭐⭐⭐⭐⭐ 核心功能 |
| **质量验证** | 基础 | 自动化评分系统（0-100分） |
| **应用场景** | 企业/大型项目 | 全场景，更适合中小项目快速迭代 |

---

## 安装与配置

### 1. 从 Marketplace 安装（推荐）

```bash
# 1. 添加 marketplace（首次需要）
/plugin marketplace add MartyBonacci/specswarm

# 2. 安装主插件
/plugin install specswarm@specswarm-marketplace

# 3. 可选：安装 /ss: 快捷命令
/plugin install ss@specswarm-marketplace
```

**注意**：安装后需要重启 Claude Code 才能激活插件。

### 2. 便携式安装（已废弃 ⚠️）

```bash
# 不推荐 - 官方已废弃此方式
curl -sSL https://raw.githubusercontent.com/MartyBonacci/specswarm/main/portable/install.sh | bash
```

**为什么不推荐**：
- 使用不同的命令前缀 (`/sw:` vs `/specswarm:`)
- 不再维护，功能可能落后
- 缺少自动更新

---

## 快速开始

### Step 1: 项目初始化

```bash
# 在项目根目录执行
/specswarm:init
```

这会创建 `.specswarm/` 目录，包含：
- `tech-stack.md` - 技术栈文档，防止技术漂移
- `quality-standards.md` - 质量标准和预算
- `constitution.md` - 项目治理规则

### Step 2: 开始开发

**方法一：自然语言**
```bash
# 无需记忆命令，直接描述需求
"Build user authentication with JWT"
"Fix the login button on mobile"
"Change authentication from session to JWT"
"Ship this feature"
```

**方法二：使用具体命令**
```bash
# 构建新功能
/specswarm:build "feature description"

# 修复 bug
/specswarm:fix "bug description"

# 修改现有功能
/specswarm:modify "change description"

# 发布功能
/specswarm:ship
```

---

## 核心命令详解

### 1. `/specswarm:init` - 项目初始化
```bash
/specswarm:init
```

**作用**：创建项目的 `.specswarm/` 配置目录。

**配置文件**：
- `tech-stack.md` - 锁定技术栈，防止意外引入新技术
- `quality-standards.md` - 质量门槛和预算
- `constitution.md` - 项目核心原则

### 2. `/specswarm:build` - 构建新功能
```bash
/specswarm:build "用户认证功能，支持 JWT"

# 小任务快速模式
/specswarm:build "添加健康检查 API" --quick
```

**完整工作流程**：
1. **规格** - 创建功能规格文档
2. **澄清** - AI 询问模糊点
3. **计划** - 技术实现方案
4. **任务** - 可执行的任务列表
5. **实现** - AI 执行编码
6. **验证** - 质量评分

### 3. `/specswarm:fix` - 修复 Bug
```bash
/specswarm:fix "登录按钮点击无效"
# 或者
/specswarm:fix "修复图片加载失败问题"
```

**特点**：
- 自动回归测试
- 支持多 bug 协调调试 (`--coordinate`)
- 自动重试机制

### 4. `/specswarm:modify` - 修改现有功能
```bash
/specswarm:modify "将认证从 session 改为 JWT"
/specswarm:modify "在用户列表 API 中添加分页"
```

**标志选项**：
- `--refactor` - 行为保持的重构
- `--deprecate` - 分阶段的弃用功能
- `--analyze-only` - 仅分析影响，不执行实现

### 5. `/specswarm:ship` - 发布功能
```bash
/specswarm:ship
```

⚠️ **安全保护**：
- 总需要明确的 "yes" 确认
- 无法自动执行破坏性操作
- 合并前运行质量验证

**可选标志**：
- `--security-audit` - 发布前运行全面安全扫描

---

## 其他重要命令

| 命令 | 用途 | 示例 |
|------|------|------|
| `/specswarm:release` | 版本更新 + 变更日志 + 标签发布 | `/specswarm:release` |
| `/specswarm:upgrade` | 依赖/框架升级 + 兼容性分析 | `/specswarm:upgrade` |
| `/specswarm:rollback` | 安全回滚失败的功能 | `/specswarm:rollback` |
| `/specswarm:status` | 检查后台会话进度 | `/specswarm:status` |
| `/specswarm:metrics` | 功能分析仪表板 | `/specswarm:metrics --export` |

### `/ss:` 快捷命令

> **迁移提示**：从 `/specswarm:` 迁移到 `/ss:`，两者当前功能相同

```bash
# 安装快捷命令插件
/plugin install ss@specswarm-marketplace

# 使用快捷命令
/ss:build "feature"
/ss:fix "bug"
/ss:ship
```

| 快捷命令 | 完整命令 |
|----------|-----------|
| `/ss:build` | `/specswarm:build` |
| `/ss:fix` | `/specswarm:fix` |
| `/ss:modify` | `/specswarm:modify` |
| `/ss:ship` | `/specswarm:ship` |
| `/ss:init` | `/specswarm:init` |
| `/ss:release` | `/specswarm:release` |
| `/ss:upgrade` | `/specswarm:upgrade` |
| `/ss:rollback` | `/specswarm:rollback` |
| `/ss:status` | `/specswarm:status` |
| `/ss:metrics` | `/specswarm:metrics` |

---

## 关键特性

### 1. 质量验证系统（0-100 分）

**评分维度**：
- **单元测试**（30 分） - 基于通过率
- **代码覆盖率**（30 分） - 基于覆盖率百分比
- **集成测试**（20 分） - API/服务测试
- **浏览器测试**（20 分） - E2E 用户流测试

**质量门槛**：建议保持 >80 分

### 2. 技术栈管理

**技术栈锁定**示例：
```markdown
# .specswarm/tech-stack.md
## 核心技术
- React Router v7
- PostgreSQL 17.x

## 批准库
- Zod v4+ (验证)

## 禁止
- ❌ Redux (使用 React Router loader/actions)
```

**防止技术漂移**：在规划、任务和执行阶段自动验证

### 3. 语言无关性

- 支持 Claude 能读取的任何语言或框架
- 无特定语言工具 - Claude 处理代码理解和生成
- 质量分析支持常见测试框架识别（Vitest、Jest、Pytest、go test 等）

---

## 内部命令（高级）

**背景**：SpecSwarm 有 11 个内部命令用于重新运行各个步骤

| 内部命令 | 对应阶段 |
|----------|----------|
| `specify` | 规格创建 |
| `clarify` | 需求澄清 |
| `plan` | 技术规划 |
| `tasks` | 任务分解 |
| `implement` | 代码实现 |
| `validate` | 质量验证 |
| `analyze-quality` | 质量分析 |
| `bugfix` | Bug 修复 |
| `hotfix` | 热修复 |
| `complete` | 功能完成 |
| `constitution` | 宪法管理 |

**访问方式**：这些命令在主要列表中隐藏，但可直接执行

---

## 最佳实践

### 1. 初始化优先
```bash
# 任何项目开始前先运行
/specswarm:init
```

### 2. 定义技术栈
尽早创建 `tech-stack.md`，防止技术漂移

### 3. 启用质量门槛
保持 >80% 质量分数，确保代码质量

### 4. 发布前质量分析
```bash
# 发布前运行完整质量分析
/specswarm:build "feature" --analyze
```

### 5. 控制包大小
保持包大小 <500KB，性能很重要

### 6. 使用自然语言
描述需求比记忆命令更直观

---

## 故障排除

### 问题 1：质量验证未运行
**原因**：缺少 `quality-standards.md` 文件

**解决**：
```bash
# 重新初始化
/specswarm:init
# 或
echo "# Quality Standards" > .specswarm/quality-standards.md
```

### 问题 2：命令前缀不匹配
**现象**：安装了便携式版本（废弃）导致命令前缀不同

**解决**：卸载便携版，重新安装 Marketplace 版本
- 便携版：`/sw:build`
- Marketplace：`/specswarm:build`

### 问题 3：插件未显示
**解决**：重启 Claude Code

---

## 版本历史

### v5.1.1 (2026-03-24)
- **修复**：分支创建失败问题
- **新增**：独立 `/ss:` 快捷命令插件
- **改进**：分支验证关口

### v5.1.0 (2026-03-22)
- **修复**：硬编码路径问题
- **新增**：`--quick` 标志用于小任务
- **清理**：移除未实现功能

### v5.0.0 (2026-03-20)
- **主要**：全新自然语言工作流
- **新增**：5 项新技能
- **增强**：智能路由意图

---

## 场景推荐

| 场景 | 推荐工具 | 理由 |
|------|----------|------|
| **个人项目/原型** | **SpecSwarm** | 自然语言更快，适合独立开发 |
| **团队协作项目** | **GitHub Spec-Kit** | 官方标准，企业使用模式 |
| **快速 Bug 修复** | **SpecSwarm** | 自动回归测试，修复更可靠 |
| **大规模重构** | **GitHub Spec-Kit** | 更强的架构管理能力 |
| **学习规格开发** | **两者皆可** | SpecSwarm 上手更简单 |

---

## 总结

### SpecSwarm 优势
1. **一体化体验** - 所有开发阶段在一个插件中
2. **自然语言优先** - 降低学习曲线
3. **智能路由** - AI 自动选择正确工作流
4. **质量驱动** - 自动化评分系统强制执行标准
5. **Claude 深度集成** - 充分利用 Claude Code 能力

### 与 GitHub Spec-Kit 定位差异
- **Spec-Kit**：企业级、标准化、多工具集成
- **SpecSwarm**：个人/团队、一体化、自然语言驱动

**如果你想要**：
- ✅ 快速启动，无需复杂配置 → **SpecSwarm**
- ✅ 严格的企业级流程 → **GitHub Spec-Kit**
- ✅ Claude Code 原生体验 → **SpecSwarm**
- ✅ 跨 AI 工具的一致性 → **GitHub Spec-Kit**

---

*整理时间：2026-03-26 16:30*
*来源：GitHub specswarm 仓库 README + 文档分析*
*关联文档：[[03-AI编程工具/Spec-Kit-完整指南]]*