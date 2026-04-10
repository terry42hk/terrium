# File-Driven Workflow — Why Chat Fails as Memory

The most common mistake in AI-assisted development isn't writing bad prompts. It's putting all your context in the chat window and expecting the AI to remember it.

I've watched this happen repeatedly: developers re-explain their project conventions every session, re-describe their architecture, re-state decisions they made yesterday. The AI forgets. They get frustrated. They conclude the tool isn't reliable.

The tool is fine. The workflow is broken.

---

## The Anti-Pattern: Chat-Only Workflow

You put all your context — conventions, decisions, progress, architectural constraints — into conversation messages. The conversation is the only place this knowledge lives.

Martin Fowler's team named this cleanly: conversations are where thinking happens, not where conclusions are stored.

**Four reasons chat fails as a knowledge layer:**

| Property | Chat | Files |
|:---|:---|:---|
| **Lifespan** | One session | Indefinite |
| **Compression** | Lossy (system decides what to drop) | Lossless |
| **Version control** | None | Git-tracked (diff, blame, review) |
| **Cross-agent** | Locked to one thread | Any agent, any session |

Using chat as memory is like using RAM as a hard drive — it works until you close the window.

---

## The Solution: Three Persistent Files

### 1. Project Context — CLAUDE.md

Every major AI coding tool now supports a persistent context file:

| Tool | File | Auto-loaded |
|:---|:---|:---|
| Claude Code | `CLAUDE.md` | Every turn |
| Cursor | `.cursorrules` | Session start |
| GitHub Copilot | `.github/copilot-instructions.md` | Yes |
| Cross-tool | `AGENTS.md` | 25+ tools |

Write your project conventions once:

```markdown
# CLAUDE.md

## Stack
- TypeScript, Next.js 15, Prisma ORM
- Tests: Vitest + Playwright

## Conventions
- camelCase for variables, PascalCase for components
- API routes in /app/api/, server actions in /app/actions/
- All DB queries through Prisma, no raw SQL
```

Every session loads this automatically. You never re-explain.

See [CLAUDE.md Rules](claude-md-rules.md) for battle-tested rule patterns.

### 2. Task Context — plan.md / spec.md

When you make decisions during a session — architectural choices, API design, implementation approach — write them to a file, not chat.

```markdown
# plan.md — Auth Migration Phase 2

## Completed
- [x] JWT token generation and validation
- [x] /api/v2/login endpoint

## In Progress
- [ ] Client SDK update for new token format

## Decisions
- Refresh token stored in httpOnly cookie (not localStorage)
- /api/v1 remains session-based until Q3 deprecation
```

The file becomes the source of truth. Future sessions read the file, not your conversation history.

Claude Code already writes plan files in plan mode — this pattern is built into the tool. The Cursor community independently discovered the same pattern and calls it "PROJECT_JOURNAL."

### 3. Session Handover — handover.md

At the end of a meaningful session, spend 2 minutes writing:

```markdown
# handover.md — 2026-04-10

## What was done
- Implemented JWT validation middleware
- Added token refresh endpoint with rotation

## What's next
- Client SDK: update TokenManager class
- Add integration test for token expiry

## Watch out for
- /api/v1/legacy-auth still uses sessions — don't touch it
- Redis not configured in staging for token blacklist
```

Next session: "Read handover.md and continue." Zero context loss. Zero re-explanation.

See [Context Window Survival](context-window-survival.md) for when to trigger a handover (the 70% rule).

---

## Before and After

**Chat-only:**
```
Open session → 10 min re-explaining context → work → session ends → everything lost → repeat
```

**File-driven:**
```
Open session → AI reads CLAUDE.md + handover.md → instant context → work → write conclusions to file → seamless continuation
```

The difference isn't that the AI got smarter. It's that your workflow shifted from "disposable sessions" to "cumulative knowledge."

---

## The Decision Tree

```
Is this information needed across sessions?
├─ No → Leave it in conversation (scratch work, thinking aloud)
└─ Yes
    ├─ Project-level convention or constraint?
    │   └─ CLAUDE.md
    ├─ Specific task's plan or progress?
    │   └─ plan.md / spec.md
    └─ Session-end context transfer?
        └─ handover.md
```

---

## Migration Path

```
Step 1: Write CLAUDE.md with project conventions
        (30 min, one-time)

Step 2: Start writing plan.md during planning sessions
        (0 extra time — Claude Code does this in plan mode)

Step 3: Write handover.md at session end
        (2 min per session)
```

The total overhead is roughly 2 minutes per session plus a 30-minute one-time setup. The return compounds with every session you run.

---

## Further Reading

- Martin Fowler — [Context Anchoring](https://martinfowler.com/articles/reduce-friction-ai/context-anchoring.html)
- Martin Fowler — [Context Engineering for Coding Agents](https://martinfowler.com/articles/exploring-gen-ai/context-engineering-coding-agents.html)
- Visual Studio Magazine — [In Agentic AI, It's All About the Markdown](https://visualstudiomagazine.com/articles/2026/02/24/in-agentic-ai-its-all-about-the-markdown.aspx)
- Cursor Forum — [PROJECT_JOURNAL Pattern](https://forum.cursor.com/t/the-project-journal-ai-context-retention-for-complex-projects/140281)
