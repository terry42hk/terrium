Context is not "the more the better". It is an expensive, inflation-prone budget. Managing it is the difference between productive sessions and frustrating ones.

## Table of Contents

- [What Lives on the "Desk"](#what-lives-on-the-desk)
- [Degradation Is Gradual, Not Sudden](#degradation-is-gradual-not-sudden)
- [Lost in the Middle -- Largely Solved, Still Worth Knowing](#lost-in-the-middle--largely-solved-still-worth-knowing)
- [Five Compression Strategies (Low Cost to High Cost)](#five-compression-strategies-low-cost--high-cost)
- [Compact Is a Controlled Restart, Not a Summary](#compact-is-a-controlled-restart-not-a-summary)
- [Why the 60% Autocompact Setting Works](#why-the-60-autocompact-setting-works)
- [Practical Guidelines](#practical-guidelines)
- [Connection to Previous Concepts](#connection-to-previous-concepts)

> *"Context is not a warehouse where you store things and own them. It is first and foremost an expensive, easily inflated, self-polluting budget."*

## What Lives on the "Desk"

Everything accumulates in the same context window simultaneously:

```
Fixed overhead (loaded every turn):
  System Prompt + CLAUDE.md + Auto-memory + Tool definitions

Accumulated (grows each turn):
  Conversation history + File contents (Read) +
  Command outputs (Bash) + Search results (Grep/Glob) +
  Loaded skills + MCP tool schemas

Context = Fixed overhead + Accumulated - Compression recovery
```

**Size:** 200K tokens (default), up to 1M with extended model.

## Degradation Is Gradual, Not Sudden

```
Usage 30%  -> Normal performance
Usage 60%  -> Early instructions start getting less attention
Usage 80%  -> Noticeable degradation, may forget rules
Usage 95%  -> Triggers prompt-too-long, harness forces compression
Usage 100% -> Cannot continue, requires manual intervention
```

The problem is not "Claude forgets" -- it's that **attention gets diluted** across too much material.

## Lost in the Middle -- Largely Solved, Still Worth Knowing

### The Original Problem (2023)

Liu et al. discovered LLMs exhibit a U-shaped attention curve -- strong attention at the beginning (primacy) and end (recency) of context, weak attention in the middle.

### Current State (2026)

**Substantially mitigated by frontier models through:**
- Long-context training data with information placed at varying positions
- Improved positional encodings (RoPE/YaRN)
- Multi-scale attention (local attention in lower layers, global in higher)
- Curriculum training on increasing context lengths

**Benchmark results:**
| Task type | Position sensitivity (2026) |
|-----------|---------------------------|
| Single-fact retrieval | Essentially none -- NIAH >99% all positions |
| Multi-fact retrieval | Mild degradation |
| Reasoning over scattered facts | Moderate degradation at 256K+ tokens |
| Following mid-context instructions | Minor but measurable |

### Verdict

> **Lost in the Middle in 2026 is not a "bug" but a "physical property" -- like gravity. It won't make you fall, but you still account for it when designing buildings.**

Even without positional attention bias, **signal-to-noise ratio** is a permanent concern. Too much irrelevant material dilutes attention on what matters, regardless of whether the model "sees" it all.

## Five Compression Strategies (Low Cost -> High Cost)

Claude Code uses a layered compression system:

| Strategy | What it does | Analogy |
|----------|-------------|---------|
| **Tool result budget** | Limits tokens per tool result | Each document gets only one page on the desk |
| **History snip** | Trims old conversation turns | Old notes go into the drawer |
| **Microcompact** | Compresses specific fragments | Shorten a long paragraph to a few sentences |
| **Context collapse** | Folds processed context | Finished files get stacked aside |
| **Autocompact** | Full conversation compression | Clear the desk, keep only a summary |

**Key principle: escalate from cheapest to most expensive.** Never clear the desk when tidying would suffice.

## Compact Is a Controlled Restart, Not a Summary

What most people think compact does: "Summarize the conversation."

What compact actually does:

```
Before: 50 turns + 20 files + 30 command outputs = 150K tokens

After:
  Conversation summary (few thousand tokens)
  + Reload CLAUDE.md                    <- back to primacy position
  + Reload plan attachments
  + Reload recently-read files
  + Reload invoked skills (with per-skill token caps)
  + Run PostCompact hooks               <- your identity injection hook
  = ~30K tokens
```

> *"Compact's goal is to re-lay the runtime environment needed to continue working. The summary is an intermediate product, not the final goal."*

### What Compact Preserves vs Loses

| Preserved | May be lost |
|-----------|------------|
| CLAUDE.md (reloaded) | Ad-hoc conversation instructions |
| Current plan | Earlier discussion context |
| Recently-read files | Files read long ago |
| Error & correction records | Trial-and-error process details |
| Skills content | Preferences stated but not written to CLAUDE.md |

**Rule: If an instruction is important enough to survive compression, it belongs in CLAUDE.md, not in conversation.**

## Why the 60% Autocompact Setting Works

Setting `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=60` means compression triggers at 60% usage instead of the higher default.

| Aspect | 60% threshold | Higher threshold (~80-90%) |
|--------|--------------|---------------------------|
| Stability | Better -- always has room | Worse -- degradation before compression kicks in |
| Information retention | Less -- compresses more often | More -- compresses less often |
| Tradeoff | Prioritizes performance consistency | Prioritizes memory completeness |

For most use cases, **stability > memory completeness**. A well-performing Claude with compressed context beats a struggling Claude with full context.

## Practical Guidelines

### When to /clear
```
Use /clear: Switching tasks -- CSS debug done, now writing API
Use /clear: Claude repeats the same mistake twice -- fresh start + better prompt
Use /clear: Something feels "off" but you can't pinpoint why

Don't: Mid-task -- use /compact instead
Don't: Quick unrelated question -- use /btw (doesn't enter history)
```

### Defensive Strategies (Good Engineering Hygiene)

| Strategy | Why it works | Still useful in 2026? |
|----------|-------------|----------------------|
| Key rules in CLAUDE.md (beginning) | Primacy position + survives compression | Always |
| Don't load irrelevant files | Signal-to-noise > raw length | Always |
| Use subagents for bulk reading | Isolates context pollution | Always |
| /clear between tasks | Clean context > bloated context | Always |
| Repeat key instructions mid-session | Counters mid-context attention dip | Insurance, not necessity |
| Split large tasks across sessions | Keeps each session focused | Always |

## Connection to Previous Concepts

| Concept | Relationship |
|---------|-------------|
| **01 Harness vs Model** | Compression timing = Harness (deterministic). Compression content = Model (probabilistic) |
| **02 Agent Loop** | The "governance phase" each turn IS context management -- trim, compress, prefetch |
| **03 Tools** | Every tool result increases context usage |
| **04 Permission** | Minimal context impact, but denied synthetic errors add small overhead |

## For Non-Technical Users

Imagine Claude's memory as a physical desk. Everything Claude needs to work with -- your instructions, files it read, previous messages -- must fit on this desk at the same time. A bigger desk doesn't help if it's covered in irrelevant papers; you'd still struggle to find what matters. That's why keeping the desk tidy (removing old files, compressing past conversations) makes Claude work better, even though the desk is technically large enough.

## Reflection Questions

1. You're at 70% context usage and Claude starts giving inconsistent answers. Should you `/compact` or `/clear`? What factors inform your decision?
2. Why is "the script ran without errors" not valid verification? Give an example where exit code 0 hides a real problem.
3. A colleague says "With 1M tokens, context management doesn't matter anymore." How would you respond?
