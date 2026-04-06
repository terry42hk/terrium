# Context Hygiene — Signal Over Noise

The context window is a budget, not a warehouse. Every token of irrelevant context dilutes Claude's attention on what matters. I've watched sessions degrade from sharp and focused to vague and confused — not because Claude got dumber, but because the context filled up with noise.

Context hygiene is about maintaining a high signal-to-noise ratio throughout a session. It's the difference between a clean desk and a cluttered one — the tools are the same, but the clarity isn't.

---

## The Problem

As you work, your context accumulates:
- File contents you've read
- Failed approaches you've abandoned
- Debugging output from resolved issues
- Exploratory searches that led nowhere
- Previous task context that's no longer relevant

All of this stays in the context window, competing for attention with what you're actually working on right now. Claude doesn't forget — it just gets distracted.

The symptoms:
- Responses become less focused
- Claude references outdated information from earlier in the session
- Instructions need to be repeated
- Quality of analysis degrades noticeably

---

## The Tools

### `/compact` — Compress Without Losing

Compresses conversation history while preserving key information. Think of it as cleaning your desk without throwing away important papers.

**When to use:**
- Context is getting large and you notice quality degrading
- You've finished a sub-task and are moving to the next
- Autocompact triggers (I set mine to 60% — more on this below)
- After a long debugging session full of failed attempts

**Watch out:**
- Important details can be lost in compression. The algorithm preserves what it thinks matters, but it doesn't always agree with you.
- Working memory of specific file contents, line numbers, and error messages is often the first to go.

**Tip:** Before compacting, make sure your current task state is captured somewhere durable — a plan file, a todo list, a commit message, or at minimum a mental note of what you're working on.

### `/clear` — Start Fresh

Nuclear option. Wipes conversation history entirely. Your `CLAUDE.md` survives (it's loaded fresh each time), but everything else is gone.

**When to use:**
- Switching to a completely different task
- Starting a fresh review session (writer-reviewer separation)
- The session is so polluted that compression won't help
- You want a clean evaluation of something without prior context bias

**The key insight:** `/clear` isn't a failure — it's a tool. Starting fresh is often faster than trying to course-correct a degraded session.

### `context: fork` in Skills

When a skill specifies `context: fork`, it runs in a subagent with its own context. The skill can read dozens of files and produce verbose output, but only the summary returns to your main context.

**When to use it (in your skill definitions):**
- Skills that read many files (research, exploration)
- Skills that produce verbose intermediate output
- Skills where only the final result matters to the parent session

**Example:** A skill that scans your entire test suite for coverage gaps might read 50 test files internally, but returns a 10-line summary of gaps found. Without forking, those 50 files would bloat your main context.

### Subagent Delegation

Route exploration to Explore agents instead of reading files directly in your main context.

**Rule of thumb:** If a task requires reading 5+ files, delegate to an Explore subagent.

**Why the math is stark:**

- **Direct reads:** 5 files x 3,000-5,000 tokens each = **15,000-25,000 tokens** of raw content in your main context
- **Explore agent:** reads all 5, synthesizes findings, returns **200-500 tokens** of actionable summary
- Your main context stays clean, and the information quality is actually **higher** because it's been synthesized

---

## When to Reset

Knowing when to compact or clear is a skill in itself. Here are the triggers I watch for.

### Topic Change

Starting a clearly different task? Suggest `/compact` or `/clear`.

If you've been debugging a payment bug and now want to add a new feature to the user profile, those are different tasks. The payment debugging context is pure noise for the profile feature work.

- Same domain, different task → `/compact`
- Completely different domain → `/clear`

### Post-Debugging

After a long debugging session, your context is full of:
- Stack traces from failed attempts
- File contents you read while investigating
- Hypotheses that were wrong
- Commands that produced errors

All of this is useless once the bug is fixed. `/compact` aggressively.

### Post-Implementation

After completing a feature, before starting the next one. The implementation details of Feature A are irrelevant to Feature B. Clean the slate.

### Quality Degradation

If you notice Claude's responses becoming:
- Less focused (addressing things you didn't ask about)
- Repetitive (re-explaining things already established)
- Confused (mixing up details from different parts of the session)

These are symptoms of context pollution. Compact immediately.

---

## The PostCompact Hook Pattern

When autocompact fires, Claude loses working memory. It's like waking up with amnesia — you know who you are (CLAUDE.md) but you've forgotten what you were doing.

A PostCompact hook solves this by re-injecting critical context after every compaction.

**What to re-inject:**
- Current date and git branch
- Active task description
- Key file paths being worked on

**How it works:** The hook runs automatically after any compaction event. It reads from a durable source (environment variables, a state file, git status) and injects that information into the fresh context.

Think of it as leaving yourself a note before going to sleep. When you "wake up" after compaction, the note tells you where you are and what you were doing.

**Example hook output:**
```
Date: 2025-03-15
Branch: feature/payment-retry
Working on: Adding retry logic to payment processor
Key files: src/payments/processor.ts, tests/payments/retry.test.ts
```

This is roughly 50 tokens. Tiny cost, massive benefit. Without it, Claude might need 2-3 exchanges just to re-orient after compaction.

---

## Compact Preservation Checklist

Before any compaction (manual or auto), ensure these are captured somewhere durable:

- [ ] **Current task list and progress** — What's done, what's next
- [ ] **Active plan file path** — If you're following a plan, where is it?
- [ ] **File paths being edited** — What files are you actively changing?
- [ ] **Error context if debugging** — The exact error message and file
- [ ] **User's original request** — Verbatim, not paraphrased
- [ ] **Git branch and recent commits** — Where you are in the codebase

For manual compaction, you can verify these before running `/compact`. For autocompact, the PostCompact hook should handle re-injection automatically.

**Practical tip:** If you're in the middle of complex work and worried about compaction, create a quick checkpoint:
```bash
# Save your state to a file
echo "Task: Add retry logic to payment processor
Files: src/payments/processor.ts, tests/payments/retry.test.ts  
Status: Implemented retry, need to add tests
Branch: feature/payment-retry" > .claude/current-task.md
```

Crude but effective. The PostCompact hook can read this file and re-inject the context.

---

## The 60% Rule

I set `CLAUDE_AUTOCOMPACT_PCT_OVERRIDE` to 60%. Here's why.

**Default behavior:** Claude autocompacts when the context window is nearly full. By that point, there's a lot of accumulated noise, and the compaction has to be aggressive — which means more information loss.

**At 60%:** Compaction triggers earlier, when there's less noise to compress. Each compaction is gentler — less information is lost because there's less to compress. The trade-off is more frequent compactions, but each one is less destructive.

**The analogy:** It's like cleaning your desk every evening vs. once a month. The daily clean takes 2 minutes and you never lose anything important. The monthly clean takes an hour and you might accidentally throw away that note you needed.

**How to set it:**
```bash
# In your shell profile (~/.zshrc or ~/.bashrc)
export CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=60
```

You can experiment with the number. Lower = more frequent but gentler compactions. Higher = less frequent but more aggressive. I've found 60% to be a good balance for my workflow.

---

## Context-Efficient Patterns

### Pattern 1: Summarize, Don't Dump

Instead of reading a file into context and then analyzing it, ask Claude to read and summarize in one step.

**Wasteful:** "Read this file" → (entire file in context) → "Now analyze it"

**Efficient:** "Read this file and tell me which functions handle authentication" → (only the relevant summary in context)

The file content still enters Claude's context temporarily, but the response (which persists) is focused.

### Pattern 2: Work in Phases

Structure long sessions into phases with natural compaction points:

1. **Research phase** → Read files, explore codebase → `/compact`
2. **Planning phase** → Design approach, write plan → `/compact`
3. **Implementation phase** → Write code, run tests → `/compact`
4. **Review phase** → Verify, clean up → Done

Each phase starts relatively clean. The research noise doesn't pollute implementation. The implementation details don't pollute review.

### Pattern 3: Offload to Files

When you and Claude develop a plan, write it to a file rather than keeping it only in conversation context.

**Why:** A plan in conversation context is ephemeral — it gets compressed or lost. A plan in a file is durable — Claude can re-read it after compaction.

```bash
# Write the plan to a file
# Then reference it: "Follow the plan in .claude/plan.md"
```

After compaction, Claude can re-read the file and pick up exactly where it left off. The file survives; the conversation doesn't.

### Pattern 4: Keep Instructions in CLAUDE.md

Anything you find yourself repeating across sessions belongs in `CLAUDE.md`. It's loaded fresh every time and survives both `/compact` and `/clear`.

- Coding conventions
- Project-specific rules
- Preferred verification methods
- Common gotchas

This is the one piece of context that's truly durable. Use it.

---

## The Mental Model

Think of your context window as a whiteboard in a meeting room:

| State | Whiteboard Analogy |
|-------|-------------------|
| **Fresh session** | Clean whiteboard. Maximum clarity. |
| **Mid-session** | Useful diagrams and notes. Productive. |
| **Polluted session** | Covered edge to edge. Can't find what matters. |
| **`/compact`** | Photograph it, erase it, pin the photo to the wall. |
| **`/clear`** | Erase everything. Only the room's permanent signs (CLAUDE.md) remain. |

The goal isn't to never use the whiteboard — it's to erase it before it becomes useless.

---

## Quick Reference

| Situation | Action |
|-----------|--------|
| Starting a different task | `/compact` or `/clear` |
| After fixing a bug | `/compact` |
| After completing a feature | `/compact` |
| Quality is degrading | `/compact` immediately |
| Need an unbiased review | `/clear` then review |
| Reading 5+ files | Delegate to Explore subagent |
| Skill reads many files | Use `context: fork` |
| Worried about losing state | Write to a file first, then compact |
| Setting up autocompact | `export CLAUDE_AUTOCOMPACT_PCT_OVERRIDE=60` |
