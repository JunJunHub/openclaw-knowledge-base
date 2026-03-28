# Wails - Go 桌面应用框架

> Electron 的轻量替代品，用 Go + Web 技术构建跨平台桌面应用

## 核心定位

| 特性 | Wails | Electron |
|------|-------|----------|
| 打包体积 | ~20 MB | ~150 MB+ |
| 内存占用 | 低（系统 WebView） | 高（嵌入 Chromium） |
| 后端语言 | Go | Node.js |
| 通信机制 | 内存桥接（同进程） | IPC（跨进程） |
| 启动速度 | < 0.5s | 2-3s |

---

## 渲染架构

```
┌─────────────────────────────────────────────────┐
│                  Wails 应用                      │
├─────────────────────────────────────────────────┤
│  ┌─────────────┐      ┌─────────────────────┐  │
│  │  Go 后端    │◄────►│  绑定层 (Bindings)   │  │
│  │ (业务逻辑)   │      │  内存桥接 < 1ms     │  │
│  └─────────────┘      └─────────────────────┘  │
│                                ▲                │
│  ┌─────────────────────────────┴───────────┐   │
│  │          原生 WebView (系统提供)          │   │
│  │   ┌─────────────────────────────────┐   │   │
│  │   │  HTML / CSS / JavaScript 前端   │   │   │
│  │   └─────────────────────────────────┘   │   │
│  └─────────────────────────────────────────┘   │
└─────────────────────────────────────────────────┘
```

### 渲染引擎（按平台）

| 平台 | 渲染引擎 | 说明 |
|------|---------|------|
| Windows | WebView2 | 基于 Chromium（Edge 浏览器内核） |
| macOS | WebKit | Safari 的渲染引擎 |
| Linux | WebKitGTK | GTK 移植版 WebKit |

**关键点**：不嵌入浏览器，复用系统原生 WebView 组件。

---

## Go ↔ JavaScript 通信

**内存直接桥接，非 HTTP / IPC：**

```go
// Go 后端
func (a *App) Greet(name string) string {
    return "Hello " + name
}
```

```javascript
// JS 前端（自动生成绑定）
import { Greet } from '../wailsjs/go/main/App'
Greet("Jun").then(result => console.log(result))
```

**工作流程**：
1. Go 定义方法 → `wails dev` 自动生成 JS/TS 绑定
2. 前端直接调用 → 零网络开销，纯内存通信

---

## 快速开始

```bash
# 安装 CLI
go install github.com/wailsapp/wails/v2/cmd/wails@latest

# 创建项目
wails init -n my-app -t react

# 开发模式
cd my-app && wails dev

# 构建发布
wails build
```

---

## 适用场景

### ✅ 适合

| 场景 | 典型应用 |
|------|---------|
| 工具类桌面应用 | 文件处理、格式转换、批量重命名 |
| 内部企业工具 | 数据迁移、配置管理、日志分析 |
| 游戏相关工具 | 游戏启动器、Mod 管理器、存档编辑器 |
| 本地 AI 应用 | Ollama + Go 集成、文档摘要、图片识别 |
| 系统级应用 | 密码管理器、备份工具、系统监控 |

### ❌ 不适合

- 复杂多媒体编辑（WebView 渲染性能有限）
- 3D 游戏（不适合实时渲染）
- 大规模企业应用（生态相对小）

---

## 实际案例

### 1. 游戏启动器（Ibrahim Okić）

```
功能：
├── 下载游戏文件
├── 管理 Mod
├── 游戏内资料和通知
├── 统计面板
├── 登录/注册/密码重置
├── Discord Rich Presence
├── 自动安装依赖
└── 自更新功能
```

### 2. GitHub 桌面客户端（Twilio 教程）

- 查看 GitHub 公开仓库
- 浏览用户 Gist
- React + Ant Design 前端

### 3. 密码管理器（Enrique Marín）

- 本地加密存储
- Svelte + TypeScript 前端

### 4. varly（官网展示）

- 原生菜单、半透明效果
- 现代 UI 设计

---

## Wails v3 新特性（Alpha）

- **多窗口支持** — 终于可以开多个窗口
- **系统托盘** — 带菜单、图标自适应
- **改进的绑定生成** — `wails3 generate bindings`
- **Wails Markup Language (wml)** — 类似 htmx

---

## 开发模式 vs 生产模式

| 模式 | 前端资源 | 调试 |
|------|---------|------|
| `wails dev` | 从磁盘加载 | Vite 热更新、DevTools |
| `wails build` | `go:embed` 嵌入 | 单文件可执行 |

---

## 与其他框架对比

| 需求 | 推荐 |
|------|------|
| 会 Go，不想学 Rust | ✅ **Wails** |
| 会 Rust，追求极致性能 | Tauri |
| 只会 JS/TS，团队大 | Electron |
| 需要原生 UI 控件多 | Flutter / Qt |

---

## 参考资源

- 官网：https://wails.io
- 文档：https://wails.io/docs/introduction
- GitHub：https://github.com/wailsapp/wails
- Awesome Wails：https://github.com/wailsapp/awesome-wails（200+ 案例）

---

## 关联笔记

- [[Claude-Code-完整开发指南]]
- [[Superpowers-完整教程与开发流程指南]]
- [[AI编码工具对比-Superpowers-vs-SpecKit]]

---

*创建时间：2026-03-28*
*来源：workspace/memory/2026-03-28.md*
