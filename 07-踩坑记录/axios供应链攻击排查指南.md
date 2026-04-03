# axios 供应链攻击排查指南

## 事件概述
2026年3月31日，axios npm 包被投毒：
- 恶意版本：**1.14.1** 和 **0.30.4**
- 攻击方式：通过 `plain-crypto-js` 依赖注入远控木马
- 影响范围：周下载量上亿

## 攻击特征

| 检查项 | 恶意特征 |
|--------|---------|
| 版本号 | axios 1.14.1 或 0.30.4 |
| 依赖包 | plain-crypto-js@4.2.1 |
| 临时文件 | /tmp、/Library/Caches、ProgramData 中的可疑脚本 |
| 网络连接 | 连接 sfrclak.com (C2 域名) |

## 排查命令

```bash
# 检查 axios 版本
npm list axios

# 检查是否有恶意依赖
npm list plain-crypto-js

# 搜索 lock 文件中的恶意版本
grep -r "axios.*1.14.1\|axios.*0.30.4\|plain-crypto-js" package-lock.json yarn.lock pnpm-lock.yaml

# 检查临时目录（Linux）
ls -la /tmp/.???* 2>/dev/null

# 检查网络连接
ss -tunap | grep sfrclak
```

## 安全加固措施

1. **锁版本**：`package.json` 写死具体版本，不用 `^` 或 `latest`
2. **开 2FA**：npm 账号开启两步验证
3. **定期审计**：`npm audit` 或用 Snyk/Socket 扫描
4. **最小化信任**：生产环境用内网镜像或自建 registry
5. **事后自救**：中招后重装系统 + 轮换所有密钥

## 本机检查结果示例

| 检查项 | 状态 |
|--------|------|
| axios 版本 | 1.13.6, 1.14.0 ✅ |
| plain-crypto-js | 未发现 ✅ |
| 临时目录 | 干净 ✅ |
| 网络连接 | 正常 ✅ |

---

*来源：workspace/memory/2026-04-03-axios-attack.md*
*创建时间：2026-04-03*
