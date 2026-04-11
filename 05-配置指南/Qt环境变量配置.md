# Qt 环境变量配置

## Qt6_DIR 设置

一键恢复工具安装 Qt 后自动设置环境变量：

```bash
export Qt6_DIR=/home/jun/Qt/6.11.0/gcc_64/lib/cmake/Qt6
```

写入 `~/.bashrc`，CMake 可以直接 `find_package(Qt6)`。

## Qt5 配置

```bash
# Qt5 用户
export Qt5_DIR="$HOME/Qt/5.15.2/gcc_64/lib/cmake/Qt5"
```

Qt5 和 Qt6 环境变量可以共存，CMake 根据需要自动选择。

## 配置场景

| 场景 | 建议 |
|------|------|
| 虚拟机单版本 | 全局配置 |
| 多版本共存 | 项目单独配置 |
| CI/CD 环境 | 项目单独配置 |

---

**来源**：`workspace/memory/2026-04-11-qt-env-config.md`
**整理时间**：2026-04-11
