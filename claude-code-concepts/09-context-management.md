In 2026, context management is no longer about "fitting things in" -- it's about maintaining a high signal-to-noise ratio. A focused 100K context outperforms a noisy 500K context.

## Table of Contents

- [Concept 05 vs 09](#concept-05-vs-09)
- [The 2026 Toolkit (Light to Heavy)](#the-2026-toolkit-light--heavy)
- [Key Strategies](#key-strategies)
- [The Kitchen Sink Anti-Pattern](#the-kitchen-sink-anti-pattern)
- [Why Context Management Still Matters in 2026](#why-context-management-still-matters-in-2026)
- [Systemizing Context Management (Beyond Manual Habits)](#systemizing-context-management-beyond-manual-habits)
- [Connection to Previous Concepts](#connection-to-previous-concepts)

## Concept 05 vs 09

- **05 Context Window** = Understanding the constraint (what context is, how it fills, how compression works)
- **09 Context Management** = Actively managing it (your operational strategy)

> *"Most best practices are based on one constraint: Claude's context window fills up fast, and performance degrades as it fills."*
> -- Claude Code Official Best Practices

## The 2026 Toolkit (Light -> Heavy)

| Tool | What it does | Context impact | When to use |
|------|-------------|---------------|-------------|
| **`/btw`** | Ask a quick question | **Zero** -- never enters history | Unrelated side questions |
| **`/compact [focus]`** | Compress conversation with optional focus | Major reduction, preserves specified focus | Mid-session, need space |
| **`/clear`** | Full context reset | Back to zero | Task switch, performance degradation |
| **`/branch`** | Fork session into independent branch | Main session unaffected | Explore alternative approach safely |
| **Subagent** | Run task in isolated context | Only summary returns | Research, bulk file reading |
| **Effort level** | Control model thinking depth | `max` uses more tokens for reasoning | Important tasks |

## Key Strategies

### `/btw` -- Zero-Cost Side Questions
If a question is unrelated to the main task, `/btw` keeps it completely out of conversation history. No context pollution.

### `/compact [focus]` -- Directed Compression
```
/compact                              -> General compression
/compact focus on the API migration   -> Preserve API context, compress rest
/compact keep the error analysis      -> Preserve debug context
```

### `/clear` -- One Session = One Task
```
Use /clear: switching tasks, Claude repeating same mistake, session > 30-40 min
Don't: mid-task (use /compact), quick questions (use /btw)
```

### `/branch` -- Risk-Free Exploration
Fork the entire session to try an alternative approach. Unlike subagents (fresh context with just your instructions), `/branch` carries full conversation history into the fork.

## The Kitchen Sink Anti-Pattern

```
Bad:  One session: debug CSS -> write API -> review PR -> random question -> continue API
      -> Context full of unrelated material -> Claude performance degrades across all tasks

Good: Separate sessions per task, /btw for side questions
```

## Why Context Management Still Matters in 2026

| Old reason (2024) | Current reason (2026) |
|-------------------|----------------------|
| Context window too small | Largely solved (1M tokens) |
| Lost in the middle | Largely mitigated, not eliminated |
| -- | **Signal-to-noise ratio** -- irrelevant material dilutes attention |
| -- | **Reasoning quality** -- clean context = better inference |
| -- | **Cost** -- larger context = more expensive API calls |
| -- | **Speed** -- larger context = slower responses |

> **Old framing:** "I can't fit everything" -> must manage.
> **New framing:** "I can fit everything, but too much makes it worse" -> still must manage.

## Systemizing Context Management (Beyond Manual Habits)

### What you can automate

| Behavior | Manual | Systemized |
|----------|--------|-----------|
| Use subagent for research | Remember to ask | CLAUDE.md rule: "Use subagents for exploration when reading 5+ files" |
| /clear on task switch | Remember to do | CLAUDE.md rule: "Suggest /clear when user starts a different task" |
| Control subagent return size | No control | Custom agent with `maxTurns` + word limit |
| Isolate heavy skills | Per-skill decision | Add `context: fork` to research/analysis skills |

### CLAUDE.md Context Hygiene Rules (Example)
```
- Use subagents for exploration -- 5+ files -> delegate, don't load into main context
- Suggest /clear on task switch -- if user starts a different task, suggest clearing
- Warn at high context -- proactively suggest /compact with focus hint
```

These are judgment-based guidelines (appropriate for CLAUDE.md), making Claude an active participant in context management rather than relying entirely on user discipline.

## Connection to Previous Concepts

| Concept | How Context Management applies |
|---------|-------------------------------|
| **01 Harness vs Model** | Auto-compact timing = Harness. Manual /compact = your choice. Both matter. |
| **02 Agent Loop** | Governance phase each turn IS context management |
| **05 Context Window** | Theory -> 09 is the practice |
| **06 CLAUDE.md** | Keep it lean = save context every turn |
| **08 Subagents** | Primary tool for context isolation |

## For Non-Technical Users

Think of each conversation with Claude as a work session at a desk. When you switch from task A to task B, clear the desk first (`/clear`). If you're mid-task but things feel slow, tidy the desk without starting over (`/compact`). If you have a quick unrelated question, use a sticky note that doesn't touch the desk at all (`/btw`). The key habit: one task per session. Mixing tasks is like cooking dinner while doing taxes on the same counter -- technically possible, but everything suffers.

## Reflection Questions

1. You've been debugging for 40 minutes in one session -- CSS fixes, API changes, and a random Slack question via `/btw`. Claude starts giving weird answers. What happened, and what should you do?
2. What's the difference between `/compact` and `/clear`? When would you choose one over the other?
3. How would you explain the "Kitchen Sink Anti-Pattern" to a new Claude Code user in one sentence?
