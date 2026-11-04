# Workflows

The filename is the reading order. The first digit is the level. A gap in the numbers is a new level, and the folder still sorts.

| Level | Notes | Read them when |
|:---|:---|:---|
| **01–03** | Beginner path | An agent is open and you have a job to do. Read these in order. |
| **10–15** | Fleet | You have more than one desk, including on Grok Bot. Read these in order. |
| **20** | A comparison | You want an outside design set next to Soft CoS. Optional. |
| **30–38** | Claude Code practice | That coding tool is the one you are using. This track does not wait on the fleet notes. |

Nothing sits at 04–09, 16–19, or 21–29. Those gaps keep the next note in a level from looking like a missing page. This file stays `README.md`. It is the index, so it has no number.

The course itself lives in [claude-code-concepts](../claude-code-concepts/). Start at [01](01-talking-to-ai.md) if you are about to ask an agent for work.

## Contents

- [Beginner path](#beginner-path)
- [Fleet](#fleet)
- [A comparison](#a-comparison)
- [Claude Code practice](#claude-code-practice)
- [Philosophy](#philosophy)

## Beginner path

01, then 02, then 03. The third note is the one to open when a chat has already forgotten something you meant to keep.

| Note | What it covers |
|:---|:---|
| [01. How to Talk to AI](01-talking-to-ai.md) | What you want, what finished looks like, what not to touch, and how you know it worked. |
| [02. How to Learn AI](02-learning-ai.md) | A real pain first, then the page that names the wall, then a markdown vault. |
| [03. Why a Markdown Vault](03-why-a-markdown-vault.md) | Chat is short-term memory. The folder lasts, searches, and moves with you. The cost is clip, then organize. |

## Fleet

10 through 15, in order, when the work is more than one desk. Public rules, not a tour of a private vault.

| Note | What it covers |
|:---|:---|
| [10. Fleet Architecture](10-architecture.md) | Diagrams of Soft CoS routing and the board loop, plus a plain-text picture |
| [11. Principles](11-principles.md) | The public checklist: one job, silence, human notes, public-safe examples, PROOF |
| [12. Soft Chief of Staff](12-soft-cos.md) | One-job desks, anti-jobs, the one-line redirect, and a single GM door |
| [13. Fleet Board, Empty Wake, PROOF](13-fleet-board-empty-wake-proof.md) | Shared board with start / finish / block, silence on an empty wake, four-field PROOF |
| [14. Mac-first Vault (IPARAG)](14-mac-first-obsidian-iparag.md) | Public folder roles and local vault → git bridge → remote fallback |
| [15. Resources and Credits](15-resources.md) | Where to read next, and who to credit |

## A comparison

Optional. Read it after the fleet notes.

| Note | What it covers |
|:---|:---|
| [20. Super-agent vs Soft CoS](20-super-agent-vs-soft-cos.md) | DeepSeek Harness Super-agent traits and Router/Worker design, what this fleet already mirrors, the gaps, and the non-goals |

## Claude Code practice

Nine notes, 30 through 38. Concepts explain *what*. These show how the work actually runs.

| Note | What it covers |
|:---|:---|
| [30. CLAUDE.md Rules](30-claude-md-rules.md) | 14 battle-tested rules that govern every Claude Code session |
| [31. Hook Patterns](31-hooks-patterns.md) | Real hook examples: auto-inject metadata, post-write validation, context recovery |
| [32. Context Hygiene](32-context-hygiene.md) | Signal over noise: /compact, /clear, subagent delegation, the 60% rule |
| [33. Context Window Survival](33-context-window-survival.md) | Three failure patterns: token burn, mid-session amnesia, plan-execute collision |
| [34. Cross-File Sync](34-cross-file-sync.md) | Making related documents discoverable with YAML tags, wikilinks, and MOCs |
| [35. File-Driven Workflow](35-file-driven-workflow.md) | Why chat fails as memory, and the three persistent files that fix it |
| [36. Subagent Orchestration](36-subagent-orchestration.md) | Patterns for delegating work to subagents effectively |
| [37. Verification Workflow](37-verification-workflow.md) | How to verify AI-generated changes before committing |
| [38. Mobile Workflow](38-mobile-workflow.md) | Running Claude Code remotely via Telegram |

## Philosophy

These workflows share a common thread: **deterministic discipline around a probabilistic model**. Claude is brilliant but forgetful. The workflows exist to catch what it misses and enforce what it can't guarantee on its own.

The fleet notes share that thread at a different scale. A group of desks is brilliant and noisy. One job per desk, a board with three verbs, and silence when nothing changed are how the noise stays out of tomorrow's session.

The beginner notes use the same thread at the size of one ask. Say what finished looks like, and how you will know. A vault gives tomorrow's session the note the chat will not keep. [03. Why a Markdown vault](03-why-a-markdown-vault.md) is that split: what lasts, what changes, and when the chat is still enough.
