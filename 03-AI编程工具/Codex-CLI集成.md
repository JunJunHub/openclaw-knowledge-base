# Codex CLI 集成到一键恢复脚本

## 安装信息

- **包名**：`codex-cli`
- **版本**：0.120.0
- **安装方式**：npm 全局安装（使用 npmmirror 镜像加速）

## 集成位置

`scripts/stages/11-dev-tools.sh` - 开发工具阶段

## 安装特性

- ✅ 使用 npmmirror 镜像加速国内安装
- ✅ 自动检测已安装版本，支持重新安装
- ✅ 验证脚本自动检查安装状态

## 认证方式

### 方式 1：ChatGPT 账号登录（推荐）
```bash
codex
```
自动打开浏览器，用 ChatGPT 账号登录。

### 方式 2：API Key 配置
```bash
mkdir -p ~/.codex
echo '{"OPENAI_API_KEY":"你的API密钥"}' > ~/.codex/auth.json
```

## 使用方式

```bash
# 完整安装（包含 Codex CLI）
./scripts/install.sh --all

# 单独安装开发工具
./scripts/install.sh --stage dev-tools
```

---

**来源**：`workspace/memory/2026-04-13-codex-integration.md`
**整理时间**：2026-04-13
