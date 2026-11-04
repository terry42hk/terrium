# Workflows

Practical patterns from daily practice. The Claude Code guides are the coding core (the course itself lives in [claude-code-concepts](../claude-code-concepts/)). The fleet foundation is for friends running more than one desk, including Grok Bot, with public rules instead of a tour of a private vault.

## Contents

- [Fleet foundation](#fleet-foundation)
- [Claude Code practice](#claude-code-practice)
- [Philosophy](#philosophy)

## Fleet foundation

Read in this order if you are new to multi-agent desks.

| Guide | What it covers |
|-------|---------------|
| [Fleet Architecture](architecture.md) | Diagrams of Soft CoS routing and the board loop, plus a plain-text picture |
| [Principles](principles.md) | The public checklist: one job, silence, human notes, public-safe examples, PROOF |
| [Soft Chief of Staff](soft-cos.md) | One-job desks, anti-jobs, the one-line redirect, and a single GM door |
| [Fleet Board, Empty Wake, PROOF](fleet-board-empty-wake-proof.md) | Shared board with start / finish / block, silence on an empty wake, four-field PROOF |
| [Mac-first Vault (IPARAG)](mac-first-obsidian-iparag.md) | Public folder roles and local vault → git bridge → remote fallback |
| [Resources and Credits](resources.md) | Where to read next, and who to credit |

## Claude Code practice

Nine guides. Concepts explain *what*. These show how the work actually runs.

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

The fleet pages share that thread at a different scale. A group of desks is brilliant and noisy. One job per desk, a board with three verbs, and silence when nothing changed are how the noise stays out of tomorrow's session.
