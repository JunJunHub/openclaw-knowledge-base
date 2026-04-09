# Qt Creator 启动踩坑

## 问题1：缺少 xcb-cursor 依赖

**错误信息**：
```
From 6.5.0, xcb-cursor0 or libxcb-cursor0 is needed to load the Qt xcb platform plugin.
Could not load the Qt platform plugin "xcb" in "" even though it was found.
```

**解决方案**：
```bash
sudo apt-get install libxcb-cursor0
```

**注意**：`libxcb-cursor0` 与 `libxcursor1` 是不同的包，容易遗漏。

## 问题2：重复的桌面快捷方式

Qt 安装器自动创建 `org.qt-project.qtcreator.desktop`，脚本又创建 `qtcreator.desktop`，导致重复。

**解决方案**：
```bash
rm ~/.local/share/applications/qtcreator.desktop
```

**脚本修复**：检查 `~/.local/share/applications/` 是否已存在 Qt Creator 快捷方式，避免重复创建。

## 问题3：MaintenanceTool 镜像加速

Qt MaintenanceTool 启动时可指定国内镜像：

```bash
# 修改 desktop 文件
sed -i 's|Exec=/home/jun/Qt/MaintenanceTool|Exec=/home/jun/Qt/MaintenanceTool --mirror https://mirrors.ustc.edu.cn/qtproject|' ~/.local/share/applications/Qt-MaintenanceTool.desktop
```

---

**来源**：`workspace/memory/2026-04-09-0241.md`
**整理时间**：2026-04-09
