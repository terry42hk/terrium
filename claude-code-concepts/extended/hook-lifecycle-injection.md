# Hook is lifecycle injection, not workflow orchestration

Hooks inject scripts at specific lifecycle points — they do not chain tools, orchestrate multi-step workflows, or replace the model's reasoning. Think "event listener," not "workflow engine."

## What Hooks Do

A hook fires at a **single point** in the tool execution lifecycle:
- **Before** a tool runs (PreToolUse)
- **After** a tool runs (PostToolUse)
- **When** a session event occurs (SessionStart, Stop, PostCompact, etc.)

It runs a shell command, produces output, and is done. It does not "call the next hook" or "trigger a sequence."

## What Hooks Do NOT Do

| Expectation | Reality |
|-------------|---------|
| "Chain hooks into a pipeline" | Each hook is independent. No hook-to-hook communication. |
| "Orchestrate a multi-step process" | Hooks fire on events, not on schedules or sequences. |
| "Replace CLAUDE.md rules" | Hooks are mechanical; they cannot make judgment calls. |
| "Run complex logic with conditionals" | A hook is a single shell command. Keep it simple. |

## The Right Mental Model

**Hooks = Event Listeners in web development.**

```javascript
// Web: addEventListener fires on a DOM event
document.addEventListener('click', handler)

// Claude Code: Hook fires on a lifecycle event
"PostToolUse": [{"matcher": "Write", "hooks": [{"type": "command", "command": "..."}]}]
```

Both are:
- **Reactive** — triggered by events, not called explicitly
- **Scoped** — bound to a specific event type
- **Side-effect producers** — they do something, then return control
- **Independent** — one listener doesn't control another

## Correct Uses of Hooks

| Pattern | Hook event | Purpose |
|---------|-----------|---------|
| Auto-lint after file edit | PostToolUse(Write) | Quality gate |
| Inject date into every prompt | UserPromptSubmit | Context enrichment |
| Block edits to protected files | PreToolUse(Edit) | Safety boundary |
| Re-inject identity after compression | PostCompact | Context preservation |
| Notify user when task completes | Notification / Stop | Awareness |

Each is a **single action at a single point** — not a workflow.

## When You Actually Need Orchestration

If you need multi-step automation (research → draft → review → publish), use:
- **Skills** — reusable prompt templates that guide Claude through a workflow
- **Custom agents** — subagents with specific mandates and tool restrictions
- **Plan mode** — structured plan → approve → execute cycle

These are the right tools for orchestration. Hooks are not.

## For Non-Technical Users

Think of hooks as **automatic responses** — like your phone auto-replying "I'm driving" when you're in Do Not Disturb mode. Your phone doesn't orchestrate a conversation; it reacts to one event with one action. Hooks work the same way.
