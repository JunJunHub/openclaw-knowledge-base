# Bash Source 脚本中的 Exit 问题

## 问题描述

在使用 `source` 加载的脚本中使用 `exit` 会终止整个父进程。

## 场景

```bash
# install.sh 主脚本
run_stage() {
  local stage_script="$SCRIPT_DIR/stages/${stage_name}.sh"
  source "$stage_script"  # 使用 source 加载
}

# stages/03-chrome.sh 阶段脚本
if command -v google-chrome &> /dev/null; then
  log_info "Chrome 已安装: $(google-chrome --version)"
  exit 0  # ❌ 会终止整个 install.sh
fi
```

## 根因

`source` 在当前 shell 环境中执行脚本，`exit` 会直接退出当前 shell（即父进程）。

## 解决方案

使用 `return` 替代 `exit`：

```bash
# ✅ 正确做法
if command -v google-chrome &> /dev/null; then
  log_info "Chrome 已安装: $(google-chrome --version)"
  return 0  # 仅结束当前函数/脚本
fi
```

## 最佳实践

| 场景 | 使用 |
|------|------|
| 独立执行脚本 | `exit` |
| source 加载脚本 | `return` |
| 不确定时 | 使用函数封装，return 退出函数 |

## 相关文件

- `openclaw-recovery/scripts/stages/*.sh`
- 提交：`41c3f86`

---

*来源：openclaw-recovery 一键恢复脚本调试，2026-04-01*
