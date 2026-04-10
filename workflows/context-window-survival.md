# Context Window Survival — Three Failure Patterns

> Companion to [Context Hygiene](context-hygiene.md), which covers the tools. This guide covers the failure patterns — what goes wrong and when.

The context window is finite. Most developers know this. What they don't know is *how* they're exhausting it. After months of daily Claude Code use and watching others hit the same walls, I've identified three recurring failure patterns — each with a concrete fix.

---

## Pattern 1: Token Burn

**Symptom:** Context hits 70-80% before any real work begins.

**What happened:** You asked Claude to read an entire folder of documents — 20 markdown files, a handful of config files, maybe some logs. Each file is a few KB. In aggregate, that's 30-50K tokens of raw content loaded into your main conversation.

I've seen this most often with people building Karpathy-style LLM wikis — asking Claude Code to read all raw notes and update an index. Claude dutifully reads every file. The context fills up. The session is half-dead before work starts.

**Fix: Delegate to subagents.**

```
# Instead of reading 20 files in the main conversation:
Agent(subagent_type="Explore", prompt="Read all files in docs/ and summarize key conventions")

# Main conversation receives: ~500 words of summary
# Context cost: ~500 tokens instead of ~40,000
```

**My rule:** If a task requires reading more than 5 files, it belongs in a subagent. Main context stays light.

See [Subagent Orchestration](subagent-orchestration.md) for patterns on delegating effectively.

---

## Pattern 2: Mid-Session Amnesia

**Symptom:** Claude contradicts earlier decisions. Forgets file paths. Uses a convention you explicitly rejected.

**What happened:** Context compression. When the conversation gets long, Claude Code automatically compresses older content to make room. Compression is lossy — the system decides what's "unimportant" and drops it. But what it considers unimportant and what you consider unimportant are often different things.

A friend recently pointed out that his sessions consistently lost specific details mid-conversation — not everything, just the precise bits he cared about most. That's compression in action.

**Three countermeasures (light to heavy):**

### TodoWrite — Compression-Resistant State

```
TodoWrite([
  { content: "API uses camelCase, DB uses snake_case", status: "in_progress" },
  { content: "Auth flow: JWT, not session cookies", status: "in_progress" }
])
```

TodoWrite state persists through compression cycles. Use it to anchor critical decisions, not just task tracking.

### Manual /compact — Controlled Compression

Don't wait for autocompact to fire. When you finish a logical phase, trigger `/compact` yourself — optionally with a focus directive:

```
/compact focus on the authentication migration decisions
```

Directed compression preserves what you specify. Automatic compression preserves what the system guesses. See [Context Hygiene](context-hygiene.md) for the 60% rule and PostCompact hook pattern.

### handover.md — Full Context Transfer

When context usage exceeds ~70%, the cleanest move is a handover:

```markdown
# handover.md
## Done
- Migrated auth from session to JWT
- Updated /api/v2/login and /api/v2/refresh endpoints

## Pending
- Update client SDK to use new token format
- Add token rotation logic

## Key Decisions
- JWT expiry: 15min access, 7d refresh (client requirement)
- No breaking changes to /api/v1 (deprecation in Q3)
```

Write this. Start a fresh session. "Read handover.md and continue." Zero information loss. 100% fresh context.

**Decision tree:**

```
Context usage?
├─ < 50% → Continue normally
├─ 50-70% → /compact with focus directive
└─ > 70% → Write handover.md → /clear → new session
```

---

## Pattern 3: Plan-Execute Collision

**Symptom:** After planning, there's not enough context left to implement.

**What happened:** Plan mode reads code, analyzes options, explores trade-offs. By the time planning is done, 60-70% of context is consumed by planning artifacts — file contents, comparisons, decision rationale.

Then you say "OK, let's build it" — and the implementation session is running on fumes.

**Fix: Externalize the plan.**

```
# During planning:
Write plan to plan.md (Claude Code does this automatically in plan mode)

# After planning:
/clear

# New session:
"Read plan.md and execute it."
```

The execution session has 100% of its context available. The plan file lives in the repo — any future session can reference it.

**When to split:**

| Task complexity | Approach |
|:---|:---|
| Single-file change, clear requirements | Plan + execute in same session |
| Multi-file change, research needed | Externalize plan → new session |
| Large refactor, 5+ files | Externalize plan + handover at each phase |

I split at roughly 70% context usage. Below that, same session is fine. Above that, the quality degradation isn't worth the convenience of staying in one session.

---

## Quick Reference

| Pattern | Trigger | Fix |
|:---|:---|:---|
| Token burn | Reading 5+ files into main context | Subagent delegation |
| Mid-session amnesia | AI contradicts earlier decisions | TodoWrite → /compact → handover.md |
| Plan-execute collision | 60%+ context used before implementation | Externalize plan → /clear → new session |

The underlying principle: context window is a finite resource. Treat it like compute budget, not like infinite scratch paper.
