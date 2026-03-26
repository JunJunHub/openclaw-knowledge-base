# Spec-Kit Brownfield 最佳实践

> Brownfield = 在已有项目中使用 Spec-Kit

---

## Step 1: 让 AI 理解现有代码

```bash
# 在 Claude Code 中
请分析这个 Go 项目的结构，总结：
1. 目录结构和模块划分
2. 主要技术栈和框架
3. 现有的代码风格和约定
4. 测试覆盖情况
```

---

## Step 2: 创建针对性的 Constitution

```bash
/speckit.constitution 基于对现有代码的分析，创建项目原则：
1. 保持与现有代码风格一致
2. 复用现有的基础组件（如日志、配置、数据库连接）
3. 新增代码遵循项目已有的目录结构
4. 不破坏现有接口的向后兼容性
```

---

## Step 3: 小步快跑

```bash
# 每次只添加一个小功能
/speckit.specify 添加一个简单的健康检查接口：GET /health

/speckit.plan 复用现有的 Gin 路由结构

/speckit.tasks

/speckit.implement
```

---

## 学习路径推荐

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

*来源：workspace-thinker/docs/spec-kit-claude-code-tutorial.md*
*整理日期：2026-03-26*
