# OpenClaw 定时任务调研报告

## 调研日期
2026-03-21

## 一、核心机制概述

OpenClaw 提供两种主要的定时任务机制：

### 1. Cron Jobs (Gateway 调度器)
- **运行位置**: Gateway 进程内部
- **持久化**: `~/.openclaw/cron/jobs.json`
- **三种调度类型**: `at`(一次性)、`every`(间隔)、`cron`(表达式)
- **两种执行模式**: 主会话(共享上下文)、隔离会话(独立运行)

### 2. Heartbeat (心跳)
- **运行位置**: 主会话
- **默认间隔**: 30 分钟
- **特点**: 批量检查、上下文感知、成本优化
- **配置文件**: `HEARTBEAT.md`

---

## 二、热门定时任务类型与案例

### 📊 类型一：每日/每周例行任务

#### 1. 晨间简报 (Morning Briefing)
```bash
openclaw cron add \
  --name "Morning brief" \
  --cron "0 7 * * *" \
  --tz "Asia/Shanghai" \
  --session isolated \
  --message "Summarize overnight emails, calendar for today, and top 3 AI news stories." \
  --announce \
  --channel telegram
```

**用途**: 每天早上7点自动汇总邮件、日历事件、AI新闻

#### 2. 每日天气提醒
```json
{
  "name": "daily-weather",
  "schedule": { "kind": "cron", "expr": "0 7 * * *", "tz": "Asia/Shanghai" },
  "payload": { "kind": "agentTurn", "message": "查询今天的天气，并发送给我" },
  "sessionTarget": "isolated"
}
```

#### 3. 工作日站会提醒
```json
{
  "name": "standup-reminder",
  "schedule": { "kind": "cron", "expr": "50 9 * * 1-5", "tz": "Asia/Shanghai" },
  "payload": { "kind": "agentTurn", "message": "提醒：10分钟后站会" },
  "sessionTarget": "isolated"
}
```

#### 4. 每日复盘 (End-of-day review)
```bash
openclaw cron add \
  --name "Daily review" \
  --cron "0 18 * * *" \
  --session isolated \
  --message "Recap tasks completed and flag pending items." \
  --announce
```

#### 5. 每周报告
```bash
openclaw cron add \
  --name "Weekly review" \
  --cron "0 9 * * 1" \
  --session isolated \
  --message "Generate a summary of the past week's activities." \
  --model opus
```

---

### 🔄 类型二：自动化工作流

#### 1. 科技新闻聚合
- **来源**: 109+ 渠道（RSS、Twitter/X、GitHub、网络搜索）
- **处理**: 自动聚合、去重、质量评分
- **推送**: Discord、Email、Telegram
- **配置示例**:
```
Install tech-news-digest from ClawHub. Set up a daily tech digest at 9am to Discord #tech-news channel.
```

#### 2. Reddit 每日摘要
```bash
openclaw cron add \
  --name "Reddit digest" \
  --cron "0 17 * * *" \
  --session isolated \
  --message "Get top performing posts from subscribed subreddits." \
  --announce \
  --channel telegram
```

#### 3. 财报追踪器
- **周日预览**: 扫描即将发布的财报日历
- **自动提醒**: 为关注公司安排一次性定时任务
- **结果摘要**: 搜索财报结果，生成详细报告
```
Every Sunday at 6 PM, run a cron job to:
1. Search for upcoming week's earnings calendar for tech and AI companies
2. Filter for companies I care about
3. Post the list to Telegram "earnings" topic
```

#### 4. 竞品监控
```bash
openclaw cron add \
  --name "Competitor monitoring" \
  --cron "0 */4 * * *" \
  --session isolated \
  --message "Check competitor websites for pricing or feature changes." \
  --announce
```

---

### ✍️ 类型三：内容创作辅助

#### 1. YouTube 内容管道
- 自动寻找视频创意
- 研究与跟踪
- 多智能体协作（研究、写作、缩略图制作）

#### 2. 多智能体内容工厂
- Discord 中运行多智能体
- 专用频道协作
- 自动发布到社交媒体

#### 3. 博客转多平台内容
- 一篇博客 → 8个Twitter线程 + 3个LinkedIn帖子 + 2个新闻简报
- 根据平台格式自动调整

---

### 💾 类型四：数据同步和备份

#### 1. 记忆整理 (Memory flush)
```bash
openclaw cron add \
  --name "Memory consolidation" \
  --cron "0 23 * * *" \
  --session isolated \
  --message "Run a pre-sleep memory consolidation to prevent context loss overnight." \
  --announce
```

#### 2. 个人知识库 (RAG)
- 自动导入 URL 内容
- 语义搜索
- 与其他工作流集成

#### 3. Git 自动备份
- 定期提交代码
- 推送到远程仓库

---

### 🔔 类型五：监控和提醒

#### 1. Heartbeat 检查清单
```markdown
# HEARTBEAT.md 示例
- 检查邮件中的紧急消息
- 查看未来 2 小时的日历事件
- 如果闲置超过 8 小时，发送简短问候
- 检查项目状态
```

#### 2. 服务器监控告警
- CPU 过热自动重启服务
- 微信/Telegram 通知
- 日志监控

#### 3. 价格监控报警
```bash
openclaw cron add \
  --name "BTC price monitor" \
  --cron "*/5 * * * *" \
  --session isolated \
  --message "Check BTC price and alert if above threshold." \
  --announce \
  --channel telegram
```

#### 4. 系统健康检查
```bash
openclaw cron add \
  --name "Health check" \
  --cron "0 */6 * * *" \
  --session isolated \
  --message "Check server health, disk usage, and running services." \
  --announce
```

---

## 三、Cron vs Heartbeat 选择指南

| 使用场景 | 推荐方式 | 原因 |
|---------|---------|------|
| 每 30 分钟检查收件箱 | Heartbeat | 批量合并，上下文感知 |
| 每天早上 9 点准时发送报告 | Cron (隔离) | 需要精确时间 |
| 监控日历中的即将发生的事件 | Heartbeat | 自然契合周期性感知 |
| 运行每周深度分析 | Cron (隔离) | 独立任务，可使用不同模型 |
| 20 分钟后提醒我 | Cron (--at) | 精确时间的一次性提醒 |
| 后台项目健康检查 | Heartbeat | 附加在现有周期中 |

---

## 四、最佳实践

### 成本优化
1. 使用 `isolatedSession: true` 避免发送完整对话历史
2. 使用 `lightContext: true` 限制 bootstrap 文件
3. 为定时任务使用更便宜的模型
4. 保持 `HEARTBEAT.md` 简短

### 安全建议
1. 不要在 `HEARTBEAT.md` 中放置敏感信息
2. 审查技能源代码
3. 检查请求的权限
4. 使用 `full` 模式时需谨慎

### 配置建议
1. 使用 `activeHours` 限制活跃时间
2. 为隔离任务设置适当的 `delivery`
3. 定期检查 `openclaw cron runs` 日志
4. 使用 `--delete-after-run` 清理一次性任务

---

## 五、热门社区资源

### 官方文档
- [Cron Jobs 文档](https://docs.openclaw.ai/zh-CN/automation/cron-jobs)
- [Heartbeat 文档](https://docs.openclaw.ai/gateway/heartbeat)
- [Cron vs Heartbeat](https://docs.openclaw.ai/automation/cron-vs-heartbeat)

### 社区案例库
- [awesome-openclaw-usecases](https://github.com/hesamsheikh/awesome-openclaw-usecases) - 30+ 真实用例
- [openclaw-tutorial](https://github.com/datawhalechina/openclaw-tutorial) - Datawhale 教程

### 技能市场
- [ClawHub](https://clawhub.ai) - 官方技能市场
- tech-news-digest - 科技新闻聚合
- reddit-readonly - Reddit 摘要

---

## 六、总结

OpenClaw 定时任务最受欢迎的应用场景：

1. **晨间简报** - 每日邮件、日历、新闻汇总
2. **新闻推送** - AI/科技/财经新闻自动推送
3. **服务器监控** - 自动告警、健康检查
4. **工作提醒** - 会议、站会、周报提醒
5. **竞品/价格监控** - 自动检测变化并通知
6. **内容创作** - 多平台内容自动生成与发布

核心建议：批量检查用 Heartbeat，精确时间用 Cron，复杂工作流用 Lobster。