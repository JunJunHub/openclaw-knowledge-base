# Qt 开发环境安装指南（国内镜像）

## 推荐版本

- **Qt 6.8 LTS**（支持到 2029 年）

## 镜像源

- 中科大镜像：`https://mirrors.ustc.edu.cn/qtproject`

## 安装步骤

```bash
# 1. 下载安装器
wget https://mirrors.ustc.edu.cn/qtproject/official_releases/online_installers/qt-unified-linux-x64-online.run
chmod +x qt-unified-linux-x64-online.run

# 2. 运行安装器（使用镜像加速）
./qt-unified-linux-x64-online.run --mirror https://mirrors.ustc.edu.cn/qtproject
```

## 编译依赖

```bash
sudo apt install -y \
    libgl1-mesa-dev \
    libxcb1-dev \
    libxkbcommon-x11-dev \
    libx11-xcb-dev \
    libegl1-mesa-dev
```

## 环境变量

```bash
echo 'export PATH="/opt/Qt/6.8.0/gcc_64/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

## 磁盘要求

- 最少 **15GB** 可用空间

---

**来源**：`workspace/memory/2026-04-07.md`
**整理时间**：2026-04-07
