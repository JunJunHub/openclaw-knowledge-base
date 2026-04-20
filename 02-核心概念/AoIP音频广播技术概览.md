---
title: AoIP音频广播技术概览
created: 2026-04-20
tags:
  - AoIP
  - 音频广播
  - 网络协议
  - 专业音频
source: workspace/memory/2026-04-20-aoip-learning.md
priority: 🟢 低优先
---

# AoIP音频广播技术概览

AoIP (Audio over IP) 是通过IP网络传输专业音频的技术，广泛应用于广播电台、演播室、现场演出等领域。

## 核心协议标准

| 协议 | 全称 | 说明 |
|------|------|------|
| **AES67** | Audio over IP互操作性标准 | 定义不同厂商设备间音频传输的互操作规范 |
| **SMPTE ST 2110** | 专业媒体IP传输标准套件 | 覆盖视频、音频、数据的IP化传输 |
| **SAP** | Session Announcement Protocol | 会话公告协议，用于组播会话通知 |
| **SDP** | Session Description Protocol | 会话描述协议，描述媒体会话参数 |
| **RTP** | Real-time Transport Protocol | 实时传输协议，承载实际音频数据 |
| **PTP** | Precision Time Protocol | 精确时间协议，用于多设备时钟同步 |

## 关键技术概念

### 组播 (Multicast)
- 一对多的网络通信方式
- 数据发送到组播地址
- 所有订阅该组播的接收者都能收到
- 适合广播场景，节省带宽

### 音频计量

| 类型 | 说明 |
|------|------|
| **VU Meter** | 音量单位表，显示音频信号平均电平 |
| **PPM** | Peak Programme Meter，峰值节目表，显示瞬时峰值 |

## 应用场景

1. **广播电台** - 节目传输、演播室互联
2. **演播室** - 音频矩阵、混音台联网
3. **现场演出** - 多舞台音频分发
4. **体育场馆** - 大规模音频覆盖

## 验证与测试

公开AoIP信号验证通常需要：
- 支持AES67/SMPTE ST 2110的接收设备或软件
- 网络接入组播流
- SDP文件或SAP公告获取会话信息

---
*相关链接*: [[SMPTE ST 2110]] | [[RTP协议]] | [[PTP时钟同步]]
