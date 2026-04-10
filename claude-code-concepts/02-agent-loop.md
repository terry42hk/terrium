The most common misconception about Claude Code is treating it as an enhanced chatbot. It is a stateful execution loop that can autonomously run dozens of cycles per user instruction.

## Table of Contents

- [The Loop Structure](#the-loop-structure)
- [Five Properties That Matter](#five-properties-that-matter)
- [Bot vs Agent: The Real Difference](#bot-vs-agent-the-real-difference)
- [Stop Conditions -- Not Just "Model Finished Talking"](#stop-conditions--not-just-model-finished-talking)
- [Common Beginner Misunderstandings](#common-beginner-misunderstandings)
- [Connection to Concept 1 (Harness vs Model)](#connection-to-concept-1-harness-vs-model)

## The Loop Structure

```
User input
    |
+-- Governance (Harness) ----------------------+
|  Load CLAUDE.md                              |
|  Prefetch memory & skills                    |
|  Apply tool result budget, history snip      |
|  Microcompact -> context collapse -> autocompact |
+----------------------------------------------+
    |
+-- Reasoning (Model) -------------------------+
|  Receive cleaned context                     |
|  Think -> decide next action                 |
|  Output: text response OR tool call          |
+----------------------------------------------+
    |
  Tool call present?
    +- Yes -> Execute tool -> Get result -> Back to Governance <- LOOP
    +- No  -> Reply to user -> END
```

> *"Model invocation is just one contraction in the heartbeat. What keeps the system running is the entire loop: how input is gathered, how streams are consumed, how tools are dispatched, how failures recover, when to continue the next turn."*

## Five Properties That Matter

### 1. Stateful -- carries context across turns

Each loop iteration inherits state from the previous one: `messages`, `toolUseContext`, `turnCount`, `autoCompactTracking`. The model knows what it tried last round and won't blindly repeat failures.

### 2. Governance before inference -- clean input, better output

Before the model sees anything, the harness runs 7 layers of input governance:

```
Memory prefetch -> Skill discovery -> Compact boundary ->
Tool result budget -> History snip -> Microcompact ->
Context collapse -> Autocompact
```

> *"Claude Code puts 'context governance' before 'model reasoning'. It doesn't delegate the responsibility of sorting through chaos to the model -- the runtime cleans up first, then hands over cleaner input."*

### 3. Streaming -- act before the model finishes talking

Model output arrives as an event stream (text fragments, tool_use blocks, stop signals), not a single batch. The harness can begin scheduling tool execution while the model is still generating text.

### 4. Interrupt handling -- broken cleanly, not broken silently

When interrupted (Escape), the harness generates synthetic tool results for any in-flight tool calls, ensuring the conversation history stays consistent.

> *"As long as the system has promised an execution, it must settle the account on interruption -- even if settling means recording 'incomplete'."*

### 5. Layered recovery -- not just retry

| Problem | Recovery layers (low cost -> high cost) |
|---------|---------------------------------------|
| Context too long | Context collapse -> Reactive compact -> Notify user |
| Output truncated | Raise token cap -> Meta message to continue from cutoff |

Recovery is a main-path concern, not an exception handler.

## Bot vs Agent: The Real Difference

| | Bot (ChatGPT / Claude.ai) | Agent (Claude Code) |
|--|--|--|
| Prework | Thin -- load system prompt, truncate old messages | Thick -- 7-layer governance pipeline |
| After reasoning | Done -- reply to user | May loop -- execute tools, re-govern, reason again |
| Turns per user message | 1 | 1-30+ |
| Touches real world | No -- text output only | Yes -- reads files, runs commands, edits code |
| Re-governs each turn | No -- one-shot | Yes -- context changes after each tool, requires fresh governance |

**The fundamental difference is not "does it have prework" but "does it loop after prework."**

```
Bot:   prework -> reason -> reply -> END
Agent: prework -> reason -> tool -> prework -> reason -> tool -> ... -> reply -> END
```

## Stop Conditions -- Not Just "Model Finished Talking"

| Condition | Behavior |
|-----------|----------|
| Text output, no tool_use | Normal end -> reply to user |
| Has tool_use | Execute -> continue next turn |
| User interrupts (Escape) | Synthetic results -> stop |
| Prompt too long | Recovery: collapse -> compact |
| Max output tokens | Recovery: raise cap -> continue message |
| Stop hook fires | Hook decides whether to continue |
| API error | Return error directly |

## Common Beginner Misunderstandings

| Misconception | Reality |
|---------------|---------|
| "Claude answers in one shot" | May take 10+ loop iterations with tool calls |
| "Claude is slow" | It's actually reading files, running commands, verifying results |
| "I need to guide it step by step" | State the goal; the loop decomposes steps automatically |
| "It stopped mid-task -- is it broken?" | Likely context compaction or waiting for tool permission |
| "Why did it read 10 files? I only asked to fix one" | Gather-context phase -- it needs full picture for correct changes |

## Connection to Concept 1 (Harness vs Model)

| Loop phase | Responsible party |
|------------|------------------|
| Governance (compress, trim, prefetch) | **Harness** -- deterministic |
| Reasoning (think, decide) | **Model** -- probabilistic |
| Tool execution | **Harness** dispatches, **Model** decides what to call |
| Interrupt handling | **Harness** -- deterministic |
| Recovery | **Harness** -- deterministic |

The Agent Loop is the stage where Model and Harness collaborate. Concept 1 tells you *who does what*; Concept 2 tells you *how they work together*.

## For Non-Technical Users

When you chat with ChatGPT or Claude.ai, you type a message and get one reply -- like texting. Claude Code is different: you give it a task and it works on its own -- reading files, running commands, checking results, and repeating until the job is done. It's like hiring someone to fix your kitchen vs asking a friend for advice over text. The friend replies once; the handyman inspects, works, tests, and repeats until the sink works.

## Reflection Questions

1. You ask Claude to "fix the login bug." It reads 8 files, runs 3 commands, and makes 2 edits before replying. Is this one turn or multiple? What's happening behind the scenes?
2. Why does the harness need to re-govern context after every tool call, not just at the start of the conversation?
3. A friend says "ChatGPT and Claude Code are basically the same, just different UIs." How would you explain the fundamental architectural difference?

---

**Deep Dive:** [Bot vs Agent -- the difference is thick prework and loops](extended/bot-vs-agent.md)
**Workflow:** [Verification Workflow](../workflows/verification-workflow.md)

---

← Previous: [01 Harness vs Model](01-harness-vs-model.md) | [Course Overview](README.md) | Next: [03 Tools](03-tools.md) →
