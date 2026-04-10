Hooks inject your scripts into the tool execution lifecycle. They are deterministic -- always execute, regardless of what the model "decides". Claude doesn't even know they exist.

## Table of Contents

- [What Hooks Are](#what-hooks-are)
- [Position in the Four-Layer Control Stack](#position-in-the-four-layer-control-stack)
- [Full Event Table (2026)](#full-event-table-2026)
- [Matcher Syntax](#matcher-syntax)
- [Communication Channels](#communication-channels)
- [High-Value Hook Patterns](#high-value-hook-patterns)

## What Hooks Are

Hooks are shell commands that run at specific points in Claude Code's lifecycle. Unlike CLAUDE.md (advisory, probabilistic), hooks are **guaranteed to execute**. They are configured in `settings.json`, not in conversation.

## Position in the Four-Layer Control Stack

```
Permission  -> May you do this?           <- Hardest, deterministic
Hook        -> What happens before/after?  <- Hard, deterministic
CLAUDE.md   -> How should you do it?       <- Soft but persistent
Conversation-> Do it this way this time    <- Softest, ephemeral
```

Hook is the only layer that can **simultaneously affect Claude AND the external world**: stdout injects into Claude's context, commands affect the filesystem, exit codes control tool execution flow.

## Full Event Table (2026)

| Event | When | Typical use |
|-------|------|-------------|
| **PreToolUse** | Before tool executes | Block dangerous operations, check file locks |
| **PostToolUse** | After tool succeeds | Auto-format, auto-lint, audit logging |
| **Notification** | Claude needs attention | Desktop notifications |
| **Stop** | Claude finishes responding | Post-response verification reminders |
| **SessionStart** | Session starts/resumes/compacts | Load context, inject identity |
| **SessionEnd** | Session ends | Logging, cleanup |
| **PostCompact** | After context compression | Re-inject identity and key context |
| **ConfigChange** | Settings files change | Auto-validate configuration |
| **FileChanged** | Watched files change externally | Trigger on external tool edits |
| **SubagentStart/Stop** | Subagent lifecycle | Track subagent activity |
| **UserPromptSubmit** | Before your prompt is sent | Auto-inject date, git branch, context |

## Matcher Syntax

```json
"matcher": ""           -> All events of this type
"matcher": "Edit"       -> Only Edit tool
"matcher": "Edit|Write" -> Edit or Write
"matcher": "resume"     -> SessionStart resume events only
```

## Communication Channels

| Channel | Who sees it | Use for |
|---------|------------|---------|
| **stdout** | Claude (injected into context) | Warnings, context, reminders |
| **stderr** | You only (terminal) | Debug info, user-facing messages |
| **exit code 0** | -- | Proceed normally |
| **exit code 2** | -- | Block tool execution |

## High-Value Hook Patterns

### Stop Hook -- Verification Reminder
```json
"Stop": [{"hooks": [{"type": "command",
  "command": "echo 'Before delivering: Did you verify output? Did you self-review?'"}]}]
```
Harness-enforced reinforcement of CLAUDE.md soft rules.

### UserPromptSubmit -- Auto-Inject Context
```json
"UserPromptSubmit": [{"hooks": [{"type": "command",
  "command": "echo \"Date: $(date +%Y-%m-%d). Git branch: $(git branch --show-current 2>/dev/null || echo 'n/a')\""}]}]
```
Claude always knows today's date and current branch.

### PostToolUse(Write) -- File Validation
```json
"PostToolUse": [{"matcher": "Write", "hooks": [{"type": "command",
  "command": "head -1 \"$CLAUDE_FILE_PATH\" | grep -q '^---' || echo 'WARNING: File missing frontmatter'"}]}]
```
Auto-warns when files lack expected structure.

## For Non-Technical Users

```
You need to USE:       Permission (your trust boundary)
You need to KNOW:      Hook exists (deterministic automation)
You need HELP to SET:  Hook commands (ask Claude or an engineer)
```

You don't write hooks from scratch -- use examples, ask Claude to generate them, or use configuration skills.

## Reflection Questions

1. You want Claude to always check for frontmatter when writing markdown files. Should this be a CLAUDE.md rule, a Hook, or both? What's the tradeoff?
2. A hook outputs a warning to stdout. Who sees it -- you, Claude, or both? What if it outputs to stderr instead?
3. Someone asks you to build a "hook pipeline" where Hook A triggers Hook B triggers Hook C. Why doesn't this work, and what should they use instead?

---

**Deep Dive:** [Hook is lifecycle injection, not workflow orchestration](extended/hook-lifecycle-injection.md)
**Workflow:** [Hooks Patterns](../workflows/hooks-patterns.md)

---

← Previous: [09 Context Management](09-context-management.md) | [Course Overview](README.md) | Next: [11 Skills](11-skills.md) →
