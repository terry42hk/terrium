# Workflows

Practical patterns for working with Claude Code effectively. These aren't concepts (see [claude-code-concepts](../claude-code-concepts/) for that) — they're the actual workflows I use daily.

## Guides

| Guide | What it covers |
|-------|---------------|
| [CLAUDE.md Rules](claude-md-rules.md) | 14 battle-tested rules that govern every Claude Code session |
| [Hook Patterns](hooks-patterns.md) | Real hook examples: auto-inject metadata, post-write validation, context recovery |
| [Context Hygiene](context-hygiene.md) | Signal over noise: /compact, /clear, subagent delegation, the 60% rule |
| [Context Window Survival](context-window-survival.md) | Three failure patterns: token burn, mid-session amnesia, plan-execute collision |
| [Cross-File Sync](cross-file-sync.md) | Making related documents discoverable with YAML tags, wikilinks, and MOCs |
| [File-Driven Workflow](file-driven-workflow.md) | Why chat fails as memory, and the three persistent files that fix it |
| [Subagent Orchestration](subagent-orchestration.md) | Patterns for delegating work to subagents effectively |
| [Verification Workflow](verification-workflow.md) | How to verify AI-generated changes before committing |
| [Mobile Workflow](mobile-workflow.md) | Running Claude Code remotely via Telegram |

## Philosophy

These workflows share a common thread: **deterministic discipline around a probabilistic model**. Claude is brilliant but forgetful. The workflows exist to catch what it misses and enforce what it can't guarantee on its own.
