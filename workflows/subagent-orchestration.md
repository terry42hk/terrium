# Subagent Orchestration — Parallel Work Without Chaos

Subagents are one of the most powerful features in Claude Code — and one of the easiest to misuse. This guide covers how I actually use them, the discipline rules that prevent chaos, and the patterns that work in practice.

---

## What Are Subagents?

Subagents are isolated Claude instances that run in parallel within your session. They share your prompt cache (so the token overhead is low) but each gets its own context window. The parent agent spawns them, they do their work, and they report back.

The key word is **isolated**. They can't see each other's work. They can't coordinate mid-task. The parent is the only point of coordination.

---

## Three Built-in Types

### Explore (fast, read-only)

The workhorse. Uses a fast model. Can search files, read code, answer questions — but can't modify anything. This is what you reach for 80% of the time.

Use it when:
- You need to understand how something works before changing it
- You're searching across 5+ files for a pattern
- You want to trace a call chain or check test coverage

### Plan (inherits parent model, read-only)

For analysis and planning. Gets the same model as the parent (so it can reason deeply) but still can't change anything. Think of it as a senior engineer reviewing the codebase and proposing an approach.

Use it when:
- You need to design an approach before implementing
- The problem is ambiguous and needs analysis
- You want a second opinion on architecture

### General-purpose (full tools)

The heavy hitter. Can read files, write files, run commands — the full toolkit. Use it for actual implementation work.

Use it when:
- You need to implement changes across multiple files
- The task requires running commands (tests, builds, etc.)
- You're doing multi-step work that involves both reading and writing

---

## The Three Discipline Rules

These aren't theoretical. Each one exists because I've hit the failure mode it prevents.

### Rule 10: Synthesize Before Forwarding

**The failure mode:** You send an Explore agent to research something, then tell a General-purpose agent "based on the research, implement it." The implementation agent gets a wall of text, misses the key detail on line 47, and builds the wrong thing.

**Bad:**
> "Based on your findings, implement the fix."

**Good:**
> "The bug is in `~/.claude/skills/loader.ts` at line 142. The `importSkill()` function doesn't handle the case where the skill directory is a symlink. Change the `fs.existsSync()` check to `fs.lstatSync()` followed by `fs.realpathSync()` if it's a symlink."

**Why it matters:** Subagent output is often 80% noise, 20% signal. File listings, search results, dead ends — most of it is exploration artifacts. The parent's job is to extract the specific file paths, line numbers, and decisions, then write precise instructions.

You're delegating **work**, not **understanding**. The parent must always understand what's happening.

### Rule 11: Dispatch With a Plan

Before launching 2+ parallel subagents, write out:
1. What each agent does
2. What files each agent touches
3. What output format you expect

**Example plan:**
```
Agent A (Explore): Search for all usages of `processPayment()` across the codebase.
  Expected output: List of file paths and line numbers.

Agent B (Explore): Read the test files in `tests/payment/` and summarize coverage gaps.
  Expected output: List of untested scenarios.

Agent C (Explore): Check the API documentation in `docs/api/` for payment endpoint specs.
  Expected output: Expected request/response formats.
```

**Why it matters:** Without a plan, you get duplicated work. Two agents searching for the same thing. Or worse — conflicting conclusions because they looked at different versions of the truth.

### Rule 12: No Overlapping File Scope

Each parallel subagent must have non-overlapping file boundaries. Period.

**Good split:**
- Agent A writes `auth/login.ts` and `auth/signup.ts`
- Agent B writes `api/routes.ts` and `api/middleware.ts`

**Bad split:**
- Agent A refactors `auth/login.ts`
- Agent B adds error handling to `auth/login.ts`

**Why it matters:** Two agents editing the same file means one agent's changes get silently overwritten by the other. There's no merge — the last write wins. This is the most common source of lost work with subagents.

---

## Common Patterns

### Pattern 1: Parallel Exploration (most common)

Launch 2-3 Explore agents with different search focuses. This is the bread and butter.

```
Agent A: Find the function definition for `syncUserData()`
Agent B: Trace the call chain — who calls `syncUserData()` and when?
Agent C: Check test coverage for the sync module
```

All three run simultaneously. When they return, you synthesize:
- Function is in `src/sync/core.ts:89`
- Called from 3 places: the scheduler, the API handler, and the manual trigger
- Tests cover the scheduler path but not the API handler path

Now you know exactly what to change and what tests to add.

### Pattern 2: Research, Plan, Execute Pipeline

Sequential, not parallel. Each step informs the next.

1. **Explore agent** researches the codebase
2. Parent synthesizes findings into a brief
3. **Plan agent** designs the approach (informed by the brief)
4. Parent reviews the plan, adjusts if needed
5. **General-purpose agent** implements (informed by the plan)
6. Parent reviews the implementation

This is slower but much more reliable for complex changes. Each step is a checkpoint where you can course-correct.

### Pattern 3: Parallel Implementation

For independent features or changes that don't touch the same files.

```
Agent A (General-purpose):
  Files: src/auth/login.ts, src/auth/signup.ts, tests/auth/
  Task: Add rate limiting to auth endpoints

Agent B (General-purpose):
  Files: src/api/routes.ts, src/api/middleware.ts, tests/api/
  Task: Add request logging middleware
```

Both run simultaneously. When they finish, the parent reviews all changes before committing. This is where Rule 12 is critical — if these agents shared any files, you'd lose work.

### Pattern 4: Fan-out Research

When you need to understand a broad topic quickly.

```
Agent A (Explore): How does the current caching layer work?
Agent B (Explore): What are the performance bottlenecks in the API?
Agent C (Explore): What do the existing tests tell us about edge cases?
```

Each agent explores one dimension. You get a 360-degree view in the time it would take to explore one dimension manually.

---

## When NOT to Use Subagents

Subagents add coordination overhead. Sometimes the direct approach is faster.

**Don't use subagents when:**

- **Simple changes to 1-2 files.** Just do it directly. The overhead of spawning, waiting, and synthesizing isn't worth it.

- **You need tight iteration.** Subagents can't ask you questions mid-task. If the task requires back-and-forth ("does this look right?" → "no, try this instead"), do it in the main context.

- **The task is inherently sequential.** If step 2 depends entirely on step 1's output, parallel agents won't help. Use the pipeline pattern instead.

- **You're debugging.** Debugging is inherently interactive — you form a hypothesis, test it, adjust. Subagents work best for well-defined tasks with clear boundaries.

---

## Practical Tips

1. **Start with Explore agents.** They're fast, cheap, and can't break anything. Use them liberally.

2. **Keep instructions specific.** "Look at the auth module" is too vague. "Find all places where `validateToken()` is called and list the file paths" is actionable.

3. **Set expected output format.** "Return a bullet list of file paths and line numbers" helps the agent focus and makes synthesis easier.

4. **Review before committing.** After parallel implementation agents finish, always review all changes together. Sometimes independently correct changes create a collectively broken system.

5. **Use `context: fork` in skills.** If a skill needs to read many files, run it in a forked context so the verbose output doesn't pollute your main session. Only the summary comes back.

---

## The Mental Model

Think of subagents like delegating to junior engineers:
- Give them clear, specific tasks (not vague goals)
- Define their scope (what files they own)
- Review their work before shipping
- Coordinate through a single point (you)
- Don't let two people edit the same file simultaneously

The parent agent is the tech lead. It understands the full picture, delegates well-scoped work, and synthesizes results into a coherent whole.
