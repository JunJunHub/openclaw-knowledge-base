# AoIP 音频广播技术概览

> 来源：2026-04-20 学习笔记

## 核心概念

AoIP (Audio over IP) 是将音频信号通过 IP 网络传输的技术，广泛应用于专业音频广播、演播室、现场演出等领域。

## 关键协议标准

| 协议 | 说明 |
|------|------|
| **AES67** | 音频 over IP 互操作性标准，定义不同厂商设备间音频传输的互操作规范 |
| **SMPTE ST 2110** | 专业媒体通过 IP 网络传输的标准套件 |
| **SAP** | Session Announcement Protocol，会话公告协议 |
| **SDP** | Session Description Protocol，会话描述协议 |
| **RTP** | Real-time Transport Protocol，实时传输协议 |
| **PTP** | Precision Time Protocol，精确时间协议，用于音频同步 |

## 核心技术

### 组播 (Multicast)
一对多的网络通信方式，数据发送到组播地址，所有订阅该组播的接收者都能收到。

### 音频电平测量
- **VU Meter**：音量单位表，用于显示音频信号电平
- **PPM**：Peak Programme Meter，峰值节目表

## 验证公开广播信号

可通过互联网验证对接公开的 AES67/SMPTE ST 2110 测试信号，了解协议实际运行方式。

---

## 相关资源

- [AES67 标准](https://www.aes.org/publications/standards/search.cfm?docID=96)
- [SMPTE ST 2110](https://www.smpte.org/)

---
*整理时间：2026-05-22*
