# 软件下载资源汇总

> 记录时间：2026-03-24
> 用途：虚拟机环境搭建与系统部署参考

---

## 一、Windows 11 官方镜像

### 当前最新版本
- **版本号**：Windows 11 26H1（2026年3月更新）
- **构建号**：Build 28000.1719
- **发布周期**：半年频道（Semi-Annual Channel）

### 推荐下载渠道（按速度排序）

| 站点 | 地址 | 特点 |
|------|------|------|
| 异次元软件站 | https://www.iplaysoft.com/windows11.html | 完整哈希校验、多版本、绕过TPM教程 |
| MSDN镜像站 | https://msdn.sjjzm.com/win11.html | 纯净镜像、无广告、版本规范 |
| sysin博客 | https://www.cnblogs.com/sysin/p/19623649 | 24H2/25H2/26H1多版本对比 |
| 微软官方 | https://www.microsoft.com/zh-cn/software-download/windows11 | 最安全，国内速度较慢 |

### 版本选择建议

**商业版 (Business Editions)** - 推荐
- 包含：专业版 + 企业版 + 教育版 + 专业工作站版
- 优势：支持BitLocker加密、远程桌面、域加入
- 适用：企业环境、自动化系统、远程运维

**消费者版 (Consumer Editions)**
- 包含：家庭版 + 专业版 + 教育版
- 适用：个人使用、临时测试环境

### 文件验证（26H1 商业版示例）
```
文件名：zh-cn_windows_11_business_editions_version_26h1_updated_march_2026_x64_dvd_d6bda57b.iso
大小：7.18GB
SHA256：6E623972E323AB7A61C45A42E50A18201D0F351788B44691B0C55804A8BBC0F2
```

### 安装注意事项
- 使用 Rufus 或 Ventoy 制作启动盘可自动绕过 TPM 2.0 限制
- 虚拟机建议配置：4核心 / 8GB RAM / 100GB 存储

---

## 二、Ubuntu LTS 长期支持版

### 当前最新版本
- **版本号**：Ubuntu 24.04 LTS "Noble Numbat"
- **发布日期**：2024年4月25日
- **支持周期**：5年标准支持（至2029年4月）+ 可扩展ESM至2034年

### 下一个LTS版本
- **版本号**：Ubuntu 26.04 LTS "Resolute Raccoon"
- **发布日期**：2026年4月23日
- **支持周期**：10年（5年标准 + 5年ESM）

### 官方下载地址
- **桌面版**：https://cn.ubuntu.com/download/desktop
- **服务器版**：https://cn.ubuntu.com/download/server

### 国内镜像站（推荐）
- 清华大学镜像站：https://mirrors.tuna.tsinghua.edu.cn/ubuntu-releases/
- 阿里云镜像站：https://mirrors.aliyun.com/ubuntu-releases/

### 技术规格（24.04 LTS）
- 内核版本：Linux 6.8
- 桌面环境：GNOME 46
- Python版本：3.12（默认）
- 工具链：GCC 13.2, LLVM 18, Rust 1.70+

### 版本选择策略

| 场景 | 推荐版本 | 理由 |
|------|---------|------|
| 生产环境 | Ubuntu 24.04 LTS | 稳定性验证充分，生态成熟 |
| 技术探索 | Ubuntu 25.10 | 最新特性，支持至2026年7月 |
| 长期规划 | Ubuntu 26.04 LTS | 2026年4月发布，10年支持 |

---

## 三、VMware Workstation Pro

### 重要变化（2024年11月起）
- **免费开放**：向所有用户（商业/教育/个人）免费
- **产品合并**：原VMware Workstation Player功能已整合至Pro版本
- **收购方**：Broadcom（博通）

### 当前最新版本
- **版本号**：VMware Workstation Pro 25H2u1
- **发布日期**：2026年2月26日
- **构建号**：Build 25219725

### 版本命名规则
- **25H2**：2025年下半年初始版本
- **25H2u1**：更新版本（Update 1），包含安全修复和bug修复

### 国内下载问题（2026-03）

⚠️ **出口管制限制**
- 国内访问 Broadcom 官网下载时可能提示：
  > Account verification is Pending. Please try after some time.
- 原因：博通对中国地区实施出口管制，账户验证无法通过

### 国内替代下载渠道

**推荐站点：WinningPC**
- 地址：https://winningpc.com/download-vmware-workstation-pro-for-personal-use/
- 特点：提供个人使用版下载，无需 Broadcom 账户

### 官方下载步骤（非中国地区）

1. **注册Broadcom账户**
   ```
   地址：https://www.broadcom.com/
   点击右上角「Support Portal」→「Register」
   要求：真实邮箱验证，密码需含大小写+数字+特殊符号
   ```

2. **登录下载**
   ```
   登录后 → 左侧菜单「My Downloads」→ 点击「HERE」
   选择：VMware Workstation Pro
   平台：Windows / Linux
   ```

3. **首次下载验证**
   - 勾选「Terms and Conditions」
   - 填写身份验证信息（一次性）

### 25H2u1 关键修复
- ✅ 重新启用"检查更新"功能
- ✅ 修复Hyper-V兼容性问题
- ✅ 修复Windows主机响应延迟
- ✅ 修复USB设备直通故障
- ✅ 修复多显示器鼠标消失问题
- ✅ 修复Vulkan呈现模式bug

### 避坑指南
- ⚠️ 不要同时安装Hyper-V与旧版VMware（可能导致蓝屏）
- ⚠️ 国内用户无法通过 Broadcom 官网下载（出口管制）
- ⚠️ 推荐使用 WinningPC 等可信第三方站点
- ⚠️ 警惕来源不明的"破解版"安装包

### 相关资源
- 官方发布说明：https://techdocs.broadcom.com/us/en/vmware-cis/desktop-hypervisors/workstation-pro/25H2/release-notes/
- 教程参考：https://cloud.tencent.com/developer/news/3210901

---

## 四、虚拟机环境搭建建议

### 推荐组合
- **宿主机**：Windows 11 26H1 专业版
- **虚拟化平台**：VMware Workstation Pro 25H2u1
- **Linux虚拟机**：Ubuntu 24.04 LTS Server（无头环境）/ Desktop（开发环境）

### 标准化模板策略
1. 创建基础VM模板（含Python、Node.js、Docker）
2. 制作快照作为"标准化环境基线"
3. 后续Agent环境通过克隆+个性化配置快速部署

### 资源分配建议
| 用途 | CPU | 内存 | 存储 |
|------|-----|------|------|
| 轻量级Agent | 2核 | 4GB | 50GB |
| 内容创作工具 | 4核 | 8-16GB | 100GB |
| AI推理环境 | 8核 | 16-32GB | 200GB+ |

---

## 五、验证工具

### 哈希校验工具
- **OpenHashTab**：Windows右键菜单集成（推荐）
- **命令行**：`certutil -hashfile 文件名 SHA256`

### 启动盘制作工具
- **Ventoy**：多合一启动盘，支持多个ISO
- **Rufus**：单ISO启动盘制作，支持绕过TPM限制

---

*最后更新：2026-03-30*
*维护者：沉思助手*
