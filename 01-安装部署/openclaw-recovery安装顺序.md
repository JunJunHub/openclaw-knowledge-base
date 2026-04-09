# openclaw-recovery 安装顺序重构

## 新安装顺序（按依赖分层）

```
基础层:   system → github-hosts → docker → node → python → golang
OpenClaw: chrome → openclaw → config → workspaces
工具层:   dev-tools
应用层:   file-sharing → obsidian → n8n → qt
验证层:   verify
```

## 关键优化

| 优化项 | 说明 |
|--------|------|
| Docker 提前 | 第 3 阶段（N8N 依赖）|
| Qt 放最后 | 耗时最长，不阻塞其他功能 |
| CC Switch | 改用 deb 包（更稳定）|
| Docker 镜像 | 换成 IPv4 兼容源 |

## 阶段清单

| 编号 | 阶段 | 说明 |
|------|------|------|
| 01 | system | 系统基础依赖 |
| 02 | github-hosts | GitHub 访问优化 |
| 03 | docker | Docker 容器环境 |
| 04 | node | Node.js 环境 |
| 05 | python | Python 工具 |
| 06 | golang | Go 环境 |
| 07 | chrome | Chrome 浏览器 |
| 08 | openclaw | OpenClaw 核心 |
| 09 | config | 配置恢复 |
| 10 | workspaces | 工作空间 |
| 11 | dev-tools | 开发工具 |
| 12 | file-sharing | 文件共享 |
| 13 | obsidian | Obsidian |
| 14 | n8n | N8N 工作流平台 |
| 15 | qt | Qt 开发环境 |
| 16 | verify | 全量验证 |

---

**来源**：`workspace/memory/2026-04-08-recovery-tool-refactor.md`
**整理时间**：2026-04-08
