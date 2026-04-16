# Python 环境管理：uv 工作流指南

> 从 pyenv 迁移到 uv 的完整经验总结
> 记录日期：2026-04-16

## 🚀 核心决策：弃用 pyenv，全面拥抱 uv

### 为什么选择 uv？

| 特性 | pyenv | uv |
|------|-------|-----|
| 速度 | 慢（编译安装） | 极快（Rust 编写） |
| Windows 支持 | pyenv-win 配置繁琐 | 开箱即用 |
| 功能范围 | 仅管理 Python 版本 | 版本 + 虚拟环境 + 依赖包 |
| 常见问题 | "应用执行别名"导致失效 | 无此问题 |

### uv 的优势
- **一站式管理**：Python 版本、虚拟环境、依赖包统一管理
- **极速安装**：Rust 编写，比 pip 快 10-100 倍
- **项目隔离**：自动锁定依赖，确保环境一致性

---

## ⚠️ 关键风险：Python 版本选择

### 当前状态
- 已安装：Python 3.14.4（非常新的版本）
- **风险**：第三方库（特别是 AI/数据科学类如 PyTorch, NumPy）可能尚未适配 3.14

### 推荐操作
```powershell
# 安装稳定版本
uv python install 3.12

# 设置为全局默认
uv python pin --global 3.12
```

**建议**：生产环境使用 Python 3.11 或 3.12，避免使用最新版本。

---

## 🛠️ 最佳实践工作流

### 模式一：项目开发（有文件夹/项目）

适用于有 `pyproject.toml` 的正式项目：

```powershell
# 1. 在项目目录下，指定使用 Python 3.12
uv python pin 3.12

# 2. 创建虚拟环境（自动生成 .venv）
uv venv

# 3. 添加依赖（自动更新 pyproject.toml 和 uv.lock）
uv add requests

# 4. 运行脚本（自动激活环境，无需手动操作）
uv run main.py
```

**优点**：
- 自动生成 `pyproject.toml` 和 `uv.lock`
- 环境可复现，团队协作友好
- 无需手动激活虚拟环境

### 模式二：脚本工具（单文件/无项目）

适用于临时脚本、快速验证：

```powershell
# 仅执行一次，设定全局默认版本
uv python pin --global 3.12

# 之后在任何目录直接运行
uv run script.py
```

---

## 🔑 常用命令速查表

| 需求 | 传统方式 (pip/venv) | uv 方式 (推荐) |
|------|---------------------|----------------|
| 安装 Python | 官网下载安装包 / pyenv install | `uv python install 3.12` |
| 创建虚拟环境 | `python -m venv .venv` | `uv venv` |
| 安装包 | `pip install requests` | `uv add requests` (项目内) |
| 安装包（全局） | `pip install requests` | `uv pip install requests` |
| 运行脚本 | 需先 activate 虚拟环境 | `uv run script.py` |
| 查看版本 | `python --version` | `uv python list` |
| 锁定项目版本 | 无 | `uv python pin 3.12` |
| 全局默认版本 | 无 | `uv python pin --global 3.12` |

---

## 📁 文件结构说明

### 项目级配置
```
my-project/
├── .venv/           # 虚拟环境（自动生成）
├── .python-version  # Python 版本锁定（uv python pin）
├── pyproject.toml   # 项目配置 + 依赖声明
└── uv.lock          # 依赖锁定文件
```

### 全局配置
- 位置：`~/.config/uv/python-version`（Linux/macOS）
- 作用：所有无项目配置的脚本默认使用此版本

---

## 💡 使用建议

1. **新项目**：始终使用 `uv python pin 3.12` 锁定版本
2. **临时脚本**：设置全局默认版本后直接 `uv run`
3. **依赖管理**：使用 `uv add` 而非 `pip install`，确保 `uv.lock` 更新
4. **团队协作**：提交 `pyproject.toml` 和 `uv.lock`，忽略 `.venv/`

---

## 🔗 相关链接

- [uv 官方文档](https://docs.astral.sh/uv/)
- [uv GitHub](https://github.com/astral-sh/uv)
- [从 pip 迁移到 uv](https://docs.astral.sh/uv/guides/integration/pip/)

---

## 📝 更新记录

- 2026-04-16：从 pyenv 迁移到 uv，整理工作流指南
