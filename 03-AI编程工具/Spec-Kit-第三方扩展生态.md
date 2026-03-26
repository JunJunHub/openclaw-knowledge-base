# Spec-Kit 第三方扩展生态

> **分类**：AI 编程工具  
> **相关文档**：[[Spec-Kit-完整指南]] | [[AI编码工具生态全景]] | [[SpecSwarm-完整指南]]

---

## 调研日期
2026-03-26

## 核心发现

### 高活跃度、高质量第三方扩展列表

#### 1. MartyBonacci/spec-kit-extensions
- **工作流**：5个（bugfix, modify, refactor, hotfix, deprecate）
- **定位**：解决"spec-kit只支持25%开发工作"问题，覆盖完整开发周期
- **活跃度**：文档完整，有官方贡献计划
- **特点**：Agent-agnostic，支持多 AI 工具

#### 2. pradeepmouli/spec-kit-extensions
- **工作流**：9个（包含 enhance, modify, refactor, hotfix 等）
- **特色**：
  - GitHub Actions 集成
  - 代码审查强制执行
  - 企业合规友好
- **活跃度**：生产就绪，完整 GitHub 工作流集成

#### 3. IBM/iac-spec-kit
- **领域**：基础设施即代码（IaC）专用扩展
- **工作流**：`/iac.specify`、`/iac.plan` 等
- **优势**：企业级支持，IBM 官方维护
- **适合**：云基础设施团队，Terraform/Ansible 项目

#### 4. polarizertech/spec-kit-extensions
- **定位**：社区扩展孵化器，实验性质
- **特色**：简单安装器，标准结构
- **活跃度**：与官方讨论区互动，中等活跃
- **适合**：想尝鲜的开发者，学习扩展开发

#### 5. wcpaxx/spec-kit-brownfield-extensions
- **定位**：专门针对棕地项目（Brownfield）
- **活跃度**：星标较少（19个），仍在活跃维护
- **场景**：现有项目适配，小众但专业

### 扩展生态特点

| 特点 | 说明 |
|------|------|
| **早期阶段** | 大部分扩展在 2025年9-10月诞生 |
| **分化严重** | 同功能多实现，标准不统一 |
| **官方整合中** | 很多扩展计划提交到官方目录 |
| **专业化分工** | 工作流扩展、领域扩展、集成扩展 |

### 选择建议矩阵

| 诞生 |
| **分化严重** | 同功能多实现，标准不统一 |
| **官方整合中** | 很多扩展计划提交到官方目录（Issue #892, PR #723） |
| **专业化分工** | 工作流扩展、领域扩展、集成扩展 |

### 选择建议矩阵

| 团队类型 | 推荐扩展 | 理由 |
|----------|----------|------|
| **综合开发团队** | MartyBonacci 或 pradeepmouli | 功能全面，生产就绪 |
| **基础设施团队** | IBM/iac-spec-kit | 领域专用，企业支持 |
| **企业合规团队** | pradeepmouli | GitHub 集成 + 代码审查强制 |
| **教育/学习用途** | AIDE 扩展演示 | 完整学习示例 |
| **棕地项目** | wcpaxx + 其他扩展 | 现有项目适配 |

### 风险评估

**可立即采用**：
- **MartyBonacci/spec-kit-extensions** - 准备提交官方，质量高
- **pradeepmouli/spec-kit-extensions** - 生产集成完整，企业级
- **IBM/iac-spec-kit** - 企业支持，领域专精

**谨慎评估**：
- **polarizertech/spec-kit-extensions** - 实验性质，适合尝鲜
- **wcpaxx/spec-kit-brownfield-extensions** - 小众场景，活跃度不高

---

## 生态演进趋势

### 时间线
```
2025.09-10: 扩展生态诞生
    多个社区扩展涌现，标准不一
2025.12: 官方开始讨论扩展目录
    GitHub Discussions #746，Issue #892
2026.03: 生态分化与专业化
    出现工作流扩展、领域扩展、集成扩展
2026下半年（预计）：生态成熟
    官方扩展目录成形，标准化统一
```

### 扩展类型分类

| 类型 | 代表项目 | 目标用户 |
|------|----------|----------|
| **工作流扩展** | MartyBonacci, pradeepmouli | 需要完整开发周期的团队 |
| **领域扩展** | IBM/iac-spec-kit | 特定技术栈团队 |
| **集成扩展** | GitHub Actions 集成 | 需要自动化流程的团队 |
| **学习/演示扩展** | AIDE 扩展演示 | 新手、教育用途 |

---

## 调研方法

### 数据收集策略
1. **并行搜索** - 使用不同关键词组合 Tavily 搜索
2. **官方渠道验证** - GitHub Discussions + Issues 分析
3. **质量评估** - 文档完整性、代码结构、社区互动

### 关键指标
- **活跃度**：最近提交、Issue/PR 响应时间
- **质量**：文档完整性、示例项目、测试覆盖
- **社区**：星标数、Fork 数、讨论区活跃度

---

## 后续关注重点

1. **官方扩展目录进展**（Issue #892 + PR #723）
2. **扩展标准化** - 期待统一结构和安装方式
3. **生态成熟度** - 预计 2026 下半年更成熟的市场
4. **企业采用** - 大型公司的生产环境反馈

---

## 相关链接

| 扩展 | GitHub 仓库 | 文档 |
|------|-------------|------|
| MartyBonacci/spec-kit-extensions | https://github.com/MartyBonacci/spec-kit-extensions | README + EXAMPLES.md |
| pradeepmouli/spec-kit-extensions | https://github.com/pradeepmouli/spec-kit-extensions | README + workflows/ |
| IBM/iac-spec-kit | https://github.com/IBM/iac-spec-kit | README + examples/ |
| polarizertech/spec-kit-extensions | https://github.com/polarizertech/spec-kit-extensions | README + quickstart/ |
| wcpaxx/spec-kit-brownfield-extensions | https://github.com/wcpaxx/spec-kit-brownfield-extensions | README + installation/ |

---

**来源**：workspace-thinker/memory/2026-03-26.md（调研时间：2026-03-26 16:23）  
**同步时间**：2026-03-26 17:00  
**分类**：03-AI编程工具 → Spec-Kit 生态