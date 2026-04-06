# System-level context management — automate via rules and agents

Context management should not depend entirely on user discipline. By encoding hygiene rules in CLAUDE.md and leveraging hooks, custom agents, and skill design, you can make Claude an active participant in keeping its own context clean.

## The Problem

Manual context management relies on the user remembering to:
- Use subagents for bulk research
- `/clear` between tasks
- `/compact` when context gets heavy
- Avoid loading irrelevant files

This works when you're disciplined. It fails when you're deep in a problem and not thinking about context health. The solution: **systemize it**.

## Three Layers of Automation

### Layer 1: CLAUDE.md Rules (Judgment-Based)

These make Claude actively suggest context hygiene rather than waiting for the user:

```markdown
# Context hygiene (CLAUDE.md)
- Use subagents for bulk exploration — when reading 5+ files, delegate to an Explore subagent
- Suggest context reset for topic changes — when user starts a different task, suggest /compact or /clear
- Warn at high context — proactively suggest /compact with focus hint
```

These are **soft rules** — Claude uses judgment about when to apply them. They belong in CLAUDE.md because they require understanding context (e.g., "is this a different task?").

### Layer 2: Hooks (Deterministic)

For behaviors that must always happen, regardless of model judgment:

| Hook | Purpose |
|------|---------|
| PostCompact | Re-inject identity, key context, and current task state |
| SessionStart | Load environment context (date, branch, project state) |
| UserPromptSubmit | Auto-inject date and git branch into every prompt |

These run automatically — the model cannot skip them.

### Layer 3: Agent and Skill Design

Build context isolation into your tools:

| Mechanism | How it helps |
|-----------|-------------|
| Custom agents with `maxTurns` | Prevents subagents from running indefinitely and producing huge outputs |
| Word limits in agent prompts | "Report in under 200 words" controls what returns to main context |
| `context: fork` in skill frontmatter | Skill runs in isolated context, only result returns |
| Explore subagent type | Uses a fast model, read-only tools, naturally lightweight |

## The Shift

```
Manual discipline:  "I should remember to /clear"
System-level:       "Claude suggests /clear, hooks preserve identity, 
                     subagents isolate research, skills fork context"
```

You move from relying on **user memory** to relying on **system design**. The user still makes the final call (these are suggestions, not blocks), but the system makes it easy to do the right thing.

## What Cannot Be Automated

Some context management decisions still require human judgment:
- Whether a subtask is truly independent (safe to subagent) or needs main context
- When a task is truly "done" and context can be cleared
- What focus to preserve during `/compact`

The goal is not full automation — it's reducing the burden so the user only makes the decisions that genuinely require judgment.
