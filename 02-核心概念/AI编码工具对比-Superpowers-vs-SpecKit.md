# AI 编码工具对比：Superpowers vs Spec Kit

> **相关文档**：[[AI编码工具生态全景]] | [[Spec-Kit-完整指南]] | [[Superpowers-功能概览]]

## Superpowers vs GitHub Spec Kit

| 维度 | Superpowers | GitHub Spec Kit |
|------|-------------|-----------------|
| **开发者** | Jesse Vincent (独立) | GitHub 官方 |
| **定位** | AI 编码助手技能框架 | 企业级规格驱动开发工具包 |
| **核心理念** | 子代理驱动开发 + TDD | 六阶段规格工作流 |
| **适用场景** | 中小型项目、快速迭代 | 大型项目、团队协作 |

---

## 功能对比

| 功能 | Superpowers | Spec Kit |
|------|:-----------:|:--------:|
| 轻量灵活 | ✅ | 中等 |
| 子代理协作 | ✅ 独特功能 | 无 |
| TDD 强制执行 | ✅ 强制 | 可选 |
| 企业级工作流 | 中等 | ✅ 完整 |
| Brownfield 支持 | 一般 | ✅ 强 |
| 多代理一致性 | 一般 | ✅ 跨代理模板 |
| Constitution 机制 | 无 | ✅ 项目宪法 |
| Git Worktree | ✅ | 无 |
| 并发子代理 | ✅ | 无 |

---

## 选择建议

| 场景 | 推荐 |
|------|------|
| 个人项目 / 快速原型 | **Superpowers** |
| 重视测试驱动开发 | **Superpowers** |
| 需要并行多任务开发 | **Superpowers** |
| 大型团队 / 企业项目 | **Spec Kit** |
| 扩展现有大型代码库 | **Spec Kit** |
| 需要严格合规 / 审计 | **Spec Kit** |
| 多 AI 工具混合团队 | **Spec Kit** |

---

## 互补使用

两者可以结合使用：
1. 用 **Spec Kit** 定义项目 Constitution 和规格文档
2. 用 **Superpowers** 的 TDD 技能执行具体实现
3. 用 **Superpowers** 的 subagent 功能加速迭代

---

## 使用广度对比

| 工具 | 使用广度 | 说明 |
|------|:--------:|------|
| Cursor | ⭐⭐⭐⭐⭐ | 最大用户群，约 55% AI 编码工具用户 |
| GitHub Copilot | ⭐⭐⭐⭐⭐ | 企业渗透率最高 |
| Claude Code | ⭐⭐⭐⭐ | 快速增长，代理模式强 |
| Spec Kit | ⭐⭐⭐ | GitHub 官方背书，企业采用中 |
| Superpowers | ⭐⭐⭐ | Claude Code 生态内流行 |
| Kiro | ⭐⭐ | AWS 原生环境 |
| Tessl | ⭐ | 前沿实验性 |

---

## 总结

- **Superpowers** 更像"战术工具箱"：轻量灵活、强调执行效率
- **Spec Kit** 更像"战略框架"：结构严谨、适合大规模协作

根据项目规模和团队需求选择即可。
