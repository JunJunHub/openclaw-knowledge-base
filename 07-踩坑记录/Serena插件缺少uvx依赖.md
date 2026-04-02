# Serena 插件缺少 uvx 依赖

> **分类**：踩坑记录
> **相关文档**：[[Claude-Code-完整开发指南]]

## 问题

启动 Claude Code 时报错：
```
failed to reconnect to plugin:serena:serena
```

## 原因

`serena` 是一个 Python MCP 服务器，需要 `uvx`（uv 的工具运行器）来启动：

```json
{
  "serena": {
    "command": "uvx",
    "args": ["--from", "git+https://github.com/oraios/serena", "serena", "start-mcp-server"]
  }
}
```

## 解决方案

### 方案 1：禁用插件（推荐）

在 `~/.claude/settings.json` 中：

```json
"enabledPlugins": {
  "serena@claude-plugins-official": false
}
```

### 方案 2：安装 uv

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

安装后重启终端，`uvx` 就可用了。

## 背景

- **serena** 是来自 `claude-plugins-official` 市场的第三方插件
- 功能：语义代码分析 MCP 服务器，提供智能代码理解、重构建议、代码导航
- 如果已有其他 LSP 插件（如 `clangd-lsp`），可以禁用 serena

---

*来源：2026-04-02-serena-plugin-error.md*
