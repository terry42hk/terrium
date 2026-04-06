# Bot vs Agent — the difference is thick prework and loops

Both bots and agents do prework before reasoning. The difference is not whether prework exists, but whether the system loops after it — and how thick that prework is.

## The Common Misconception

Many explanations frame the difference as:
- Bot = no preparation, just answer
- Agent = has preparation (tools, context loading)

This is wrong. ChatGPT loads system prompts, truncates history, and applies safety filters before responding. That IS prework. The distinction lies elsewhere.

## The Real Difference: Looping

```
Bot:   prework → reason → reply → END
Agent: prework → reason → tool → prework → reason → tool → ... → reply → END
```

A bot reasons once and replies. An agent reasons, acts, observes the result, re-governs its context, reasons again, and may repeat this cycle dozens of times per user message.

## Thick vs Thin Prework

| | Bot (ChatGPT, Claude.ai) | Agent (Claude Code) |
|--|--|--|
| Prework | Thin — load system prompt, truncate old messages | Thick — 7-layer governance pipeline (memory prefetch, skill discovery, compaction, budget trimming, history snip, microcompact, context collapse) |
| After reasoning | Done | May loop — execute tools, re-govern, reason again |
| Turns per user message | 1 | 1-30+ |
| Re-governs each turn | No | Yes — context changes after each tool, requires fresh governance |

## Why Looping Changes Everything

The loop creates a **feedback mechanism**. Each cycle:
1. The agent acts (tool call)
2. Observes the result (tool output)
3. Re-governs (context management)
4. Decides whether to continue or stop

This is what enables autonomous task completion — the agent can decompose a goal into steps, execute each one, handle errors, and adjust its approach. A bot can only suggest steps; an agent can execute them.

## Why Re-Governance Matters

After each tool call, the context has changed — new file contents, command outputs, or search results are now in the window. The harness must re-govern:
- Is context too long? → Compress
- Are old tool results still relevant? → Trim
- Should memory or skills be refreshed? → Prefetch

Without re-governance, the agent would degrade with each cycle as context fills up. Re-governance is what makes long multi-step tasks possible.
