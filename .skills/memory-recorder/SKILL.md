---
name: memory-recorder
description: Automatically maintains a project memory file (memory.md) by recording important changes, session history, and mode/skill usage. Use when working on a project that has a memory.md file, or when the user asks to record something to memory. Triggers on: important code changes, new features, config changes, starting plans (brainstorming/writing-plans), context compression warnings, or the user explicitly asking to update memory.
---

# Memory Recorder

Maintain and update the project's `memory.md` file with the following rules.

## Rules

### Rule 1: Auto-record on important changes

After completing any of the following, update `memory.md` immediately and push:

- New feature added (new section, new functionality)
- Configuration changes (deployment, CI/CD, hosting)
- Starting a plan/spec (brainstorming, writing-plans)
- Using a new skill or workflow mode
- Data changes (new links added to defaultData)

Update the **会话记录** table (or create it) with: date, mode/skill used, and a one-line change summary.

### Rule 2: Ask before context compression

Before the session ends or context is about to be compressed, ask the user:

> "会话即将结束/压缩，需要更新 memory.md 吗？"

Only skip if the user has explicitly said no in the current session.

### Rule 3: Record mode/skill usage

When $brainstorming, $writing-plans, $using-superpowers, or Plan Mode is activated, note it in the session record.

### Update format

Every change to `memory.md` must:

1. Update 会话记录 table with new row
2. Update any outdated stats (category count, line count)
3. Mark milestones complete if applicable
4. Commit and push with message like "更新记忆文档: <summary>"

### memory.md required sections

The file must contain:

- `## 代理规则` — the three rules above
- `## 目标` — project goal
- `## 关键决策` — architecture/styling/hosting decisions
- `## 当前数据` — what content exists in each category
- `## 文件清单` — key project files
- `## 在线地址` + `## GitHub` — URLs
- `## 已完成的里程碑` — checkboxes
- `## 待办` — remaining tasks
- `## 会话记录` — dated table of sessions and changes
