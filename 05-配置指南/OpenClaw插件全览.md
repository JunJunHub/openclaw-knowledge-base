# OpenClaw 插件全览

## 概述
OpenClaw 内置 **85 个插件**，按需加载。当前已加载 **44 个**。

---

## 📦 已加载插件 (44个)

### 🤖 模型 Provider（自动按需加载）

| 插件 | 作用 |
|------|------|
| anthropic | Claude 模型支持 |
| anthropic-vertex | Claude on Google Cloud |
| byteplus | 字节跳动火山引擎 |
| chutes | Chutes.ai 模型服务 |
| cloudflare-ai-gateway | Cloudflare AI 网关 |
| copilot-proxy | VS Code Copilot 本地代理 |
| deepseek | DeepSeek 模型 |
| google | Google Gemini 模型 |
| huggingface | Hugging Face 推理 |
| kilocode | Kilo Gateway 模型服务 |
| kimi | Moonshot Kimi 模型 |
| litellm | LiteLLM 统一代理 |
| microsoft-foundry | Azure AI Foundry |
| minimax | MiniMax 模型 |
| mistral | Mistral 模型 |
| modelstudio | 阿里云百炼 |
| moonshot | Moonshot 模型 |
| nvidia | NVIDIA NIM 服务 |
| ollama | 本地 Ollama 模型 |
| openai | OpenAI GPT 模型 |
| opencode | OpenCode Zen 模型 |
| opencode-go | OpenCode Go 模型 |
| openrouter | OpenRouter 聚合服务 |
| qianfan | 百度千帆 |
| sglang | SGLang 推理引擎 |
| synthetic | Synthetic 模型 |
| together | Together.ai |
| venice | Venice 模型 |
| vercel-ai-gateway | Vercel AI 网关 |
| vllm | vLLM 推理引擎 |
| volcengine | 火山引擎 |
| xai | xAI Grok 模型 |
| xiaomi | 小米模型 |
| zai | Z.AI 模型 |

### 🔧 工具类（已启用）

| 插件 | 作用 |
|------|------|
| **browser** | 浏览器自动化 |
| **exa** | 语义搜索 |
| **tavily** | AI 网页搜索 |
| **feishu** | 飞书即时通讯 |
| **memory-core** | 记忆搜索工具 |
| device-pair | 设备配对（手机/电脑）|
| phone-control | 手机自动化控制 |
| talk-voice | 语音通话配置 |

---

## 🚫 已禁用插件 (41个)

### 用户配置禁用

| 插件 | 作用 | 建议 |
|------|------|------|
| **amazon-bedrock** | AWS Bedrock 模型 | 需要 AWS 凭证 |
| **bluebubbles** | BlueBubbles iMessage | macOS 专用 |
| **brave** | Brave 搜索 | 国内网络需要代理 |
| **duckduckgo** | DuckDuckGo 搜索 | 国内网络需要代理 |
| **perplexity** | Perplexity AI 搜索 | 需要 API Key |

### 通讯渠道插件（按需启用）

| 插件 | 作用 | 启用条件 |
|------|------|---------|
| discord | Discord 机器人 | 需要 Bot Token |
| telegram | Telegram 机器人 | 需要 Bot Token |
| whatsapp | WhatsApp Business | 需要配置 Meta 应用 |
| slack | Slack 机器人 | 需要 OAuth 配置 |
| signal | Signal 消息 | 需要配置 |
| irc | IRC 频道 | 传统 IRC |
| line | LINE 机器人 | 日本/台湾常用 |
| matrix | Matrix 协议 | Element 等客户端 |
| msteams | Microsoft Teams | 企业 Teams |
| mattermost | Mattermost | 开源团队协作 |
| googlechat | Google Chat | 企业 Google |
| zalo | Zalo 消息 | 越南常用 |
| nostr | Nostr 协议 | 去中心化社交 |
| synology-chat | 群晖 Chat | NAS 用户 |
| nextcloud-talk | Nextcloud 通话 | 自建私有云 |
| twitch | Twitch 直播 | 游戏直播平台 |
| qqbot | QQ 机器人 | 国内用户（已禁用）|

### 功能扩展插件（推荐启用）

| 插件 | 作用 | 推荐理由 |
|------|------|---------|
| **elevenlabs** | 语音合成 TTS | 高质量语音输出 |
| **deepgram** | 语音识别 STT | 高精度语音转文字 |
| **microsoft-speech** | 微软语音 | 中文语音支持好 |
| **firecrawl** | 网页抓取 | 深度爬取网页内容 |
| **searxng** | 自建搜索引擎 | 隐私友好 |
| **voice-call** | 语音通话 | 电话自动化 |
| **acpx** | ACP 编码代理 | 开发任务委派 |
| **lobster** | 工作流引擎 | 自动化审批流程 |
| **llm-task** | LLM 任务工具 | 结构化任务调用 |
| **diffs** | 代码差异查看 | PR/代码审查 |
| **open-prose** | Prose 命令 | 长文写作 |
| **memory-lancedb** | LanceDB 记忆 | 高性能向量存储 |
| **diagnostics-otel** | OpenTelemetry | 性能监控导出 |
| **thread-ownership** | Slack 线程独占 | 防止多 Agent 冲突 |
| **openshell** | 沙盒执行 | 安全命令执行 |

---

## 启用插件命令

```bash
# 启用单个插件
openclaw config set plugins.entries.<插件名>.enabled true

# 批量启用
openclaw config set plugins.entries.elevenlabs.enabled true
openclaw config set plugins.entries.deepgram.enabled true
openclaw config set plugins.entries.firecrawl.enabled true
openclaw config set plugins.entries.acpx.enabled true

# 重启生效
openclaw gateway restart
```

---

**来源**：`workspace/memory/2026-04-02-plugin-review.md`
**整理时间**：2026-04-03
