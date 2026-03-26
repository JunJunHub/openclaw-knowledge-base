# Spec-Kit 技术栈工具推荐

> **相关文档**：[[Spec-Kit-完整指南]] | [[AI编码工具生态全景]]

---

## 技术栈画像

| 层级 | 技术 | 特点 |
|------|------|------|
| **后端** | Golang / C++ | 高性能、跨平台、系统级编程 |
| **客户端** | Qt + QWidget + QML | 跨平台桌面应用、声明式UI |
| **数据库** | MySQL / SQLite3 | 关系型、嵌入式/服务端双轨 |

---

## Golang 工具链

```powershell
# 核心工具
go install golang.org/x/tools/gopls@latest      # LSP
go install github.com/go-delve/delve/cmd/dlv@latest  # 调试器
go install github.com/golangci/golangci-lint/cmd/golangci-lint@latest  # 静态分析

# 代码生成
go install github.com/sqlc-dev/sqlc/cmd/sqlc@latest  # SQL → Go

# 测试覆盖率
go install github.com/axw/gocov/gocov@latest
go install github.com/AlekSi/gocov-xml@latest

# API 文档
go install github.com/swaggo/swag/cmd/swag@latest
```

---

## C++ 工具链

```powershell
# LSP 和代码分析
pip install compiledb          # 生成 compile_commands.json
pip install cpplint           # Google C++ 风格检查
pip install cmake-language-server  # CMake LSP

# 包管理 (可选)
# vcpkg: choco install vcpkg
# 或 Conan: pip install conan
```

---

## Qt/QML 工具

```powershell
# QML 工具
pip install qmllint-wrapper   # QML 语法检查
pip install qt6-tools         # Qt 工具集

# 代码格式化
pip install qmlformat         # QML 格式化
```

---

## 数据库工具

```powershell
# 迁移工具
go install -tags 'mysql sqlite3' github.com/golang-migrate/migrate/v4/cmd/migrate@latest

# ORM 代码生成
go install gorm.io/gen/tools/gentool@latest
```

**GUI 工具**：
- MySQL: MySQL Workbench / DBeaver
- SQLite: DB Browser for SQLite

---

## Claude Code 技能推荐

### Go 后端技能

```yaml
# ~/.claude/skills/go-backend/SKILL.md
name: go-backend
description: Go 后端开发最佳实践

## Go 代码规范
- 错误处理：errors.Wrap() 包装上下文
- API 设计：RESTful，统一响应格式
- 数据库：GORM + 软删除
- 测试：表驱动 + testify
```

### Qt/QML 技能

```yaml
# ~/.claude/skills/qt-qml/SKILL.md
name: qt-qml
description: Qt/QML 跨平台开发指南

## Qt 开发规范
- QWidget：传统桌面 UI
- QML：现代声明式 UI
- 信号槽：新式 connect 语法
- 模型视图：QAbstractListModel
```

### C++ 技能

```yaml
# ~/.claude/skills/cpp-modern/SKILL.md
name: cpp-modern
description: 现代 C++ (C++17/20) 最佳实践

## 内存管理
- 智能指针：unique_ptr, shared_ptr
- RAII 原则
- Qt 对象生命周期管理
```

### 数据库技能

```yaml
# ~/.claude/skills/db-design/SKILL.md
name: db-design
description: 数据库设计规范

## 通用原则
- 主键自增 ID，软删除 deleted_at
- 字符集 UTF-8mb4
- 索引遵循最左前缀原则
```

---

## 一键安装清单

```powershell
# === Claude Code 技能 ===
mkdir -p ~/.claude/skills/go-backend
mkdir -p ~/.claude/skills/qt-qml
mkdir -p ~/.claude/skills/cpp-modern
mkdir -p ~/.claude/skills/db-design

# === Go 工具链 ===
go install golang.org/x/tools/gopls@latest
go install github.com/go-delve/delve/cmd/dlv@latest
go install github.com/golangci/golangci-lint/cmd/golangci-lint@latest

# === C++ 工具链 ===
pip install compiledb cpplint cmake-language-server
```

---

*整理日期：2026-03-26*
