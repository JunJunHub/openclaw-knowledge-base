# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Purpose

This is an **Obsidian knowledge base** for the OpenClaw project — a multi-agent AI system. It stores documentation, guides, and notes (not executable code). Files are Markdown using Obsidian's `[[wikilink]]` syntax for cross-linking.

## Directory Structure

```
00-Inbox/          # Temporary notes pending organization
01-安装部署/        # OpenClaw/Obsidian installation guides
02-核心概念/        # Core concepts (Agent/Session/Binding)
03-AI编程工具/      # AI programming tools (Spec-Kit/Superpowers/Claude Code)
04-自动化流程/      # Automation scripts and workflows
05-配置指南/        # Configuration guides (Memory Search, Web Search, Feishu)
06-OPC运营/         # One-person company operations (content creation, platforms)
07-踩坑记录/        # Troubleshooting and problem-solving records
08-收益分析/        # Revenue tracking and analysis
09-资源索引/        # Resource links and indexes
10-模板/           # Note templates
11-Archive/        # Archived content
```

## Document Conventions

### Frontmatter
Each document should include YAML frontmatter:
```yaml
---
title: 文档标题
date: YYYY-MM-DD
tags: [tag1, tag2]
category: 分类名
source: memory/YYYY-MM-DD.md  # if derived from workspace memory
status: draft | final
---
```

### Cross-Links
Use Obsidian's wikilink syntax:
- `[[文件名]]` — basic link
- `[[文件名|显示文本]]` — custom display text

### Document Header
Start each document with classification and related docs:
```markdown
> **分类**：目录名
> **相关文档**：[[相关文档1]] | [[相关文档2]]
```

## Sync Workflow

### Data Sources
Notes are automatically synced from OpenClaw workspace directories:
- `workspace/memory/` → processed into knowledge base
- `workspace-thinker/research/` → `03-AI编程工具/`
- `workspace-coder/` → `07-踩坑记录/`
- `workspace-media/content/` → `06-OPC运营/`

### Sync State
Track processed files in `.sync-state.json`. When adding new notes from workspaces, update `processedFiles` array and `stats`.

### Manual Sync
```bash
git add . && git commit -m "docs: 更新笔记" && git push
```

## Knowledge Base Maintenance

### Adding New Notes
1. Determine appropriate category directory
2. Create document with proper frontmatter
3. Add classification header and cross-links
4. Update `.sync-state.json`
5. Git commit and push

### Merging Duplicates
1. Identify duplicate content on same topic
2. Select most complete version as base
3. Merge content from other documents
4. Add cross-links
5. Delete redundant documents
6. Update `.sync-state.json`

## Files to Avoid Editing

- `.obsidian/` — Obsidian app configuration
- `.sync-state.json` — sync state (update deliberately, never overwrite)
- `00-Inbox/` — temporary holding area

## OpenClaw Context

OpenClaw uses a multi-agent architecture:
- **Main Agent** (`workspace/`) — handles private messages, coordinates other agents
- **Isolated Agents** — independent sessions for scheduled tasks
- **Sub-agents** — spawned by main agent for async tasks

Each workspace contains `AGENTS.md`, `SOUL.md`, `USER.md`, `MEMORY.md`, `HEARTBEAT.md`.
