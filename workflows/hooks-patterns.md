# Hook Patterns — Deterministic Automation

Hooks sit between Permission (hardest control) and CLAUDE.md (soft, advisory) in the control stack. The key property: they execute EVERY time, unconditionally. Claude doesn't decide whether to run them. Claude doesn't even know they exist. They just fire.

Use hooks for anything that must always happen. If you find yourself writing a CLAUDE.md rule that says "always do X after writing a file" — that's a hook, not a rule. Rules are suggestions. Hooks are guarantees.

---

## How Hooks Communicate

Hooks are shell scripts configured in `settings.json`. They communicate through three channels:

| Channel | What sees it | Use for |
|---------|-------------|---------|
| **stdout** | Claude (injected into context) | Adding information Claude should act on |
| **stderr** | Terminal only | Debug logging, progress messages |
| **Exit code 0** | Harness (proceed normally) | Normal completion |
| **Exit code 2** | Harness (block the action) | Preventing dangerous operations |

This is a powerful design. stdout lets you inject context into Claude's awareness without Claude needing to "remember" to check something. stderr lets you debug without polluting Claude's context. Exit code 2 lets you hard-block actions that should never happen.

---

## Hook Events

Hooks can fire at different points in Claude Code's lifecycle:

| Event | When it fires | Common use |
|-------|--------------|------------|
| **PreToolUse** | Before a tool executes | Block dangerous operations, inject context |
| **PostToolUse** | After a tool succeeds | Validate output, auto-format, audit logging |
| **UserPromptSubmit** | Before each user message is processed | Inject metadata (date, branch, identity) |
| **PostCompact** | After context compression | Re-inject critical working memory |
| **Stop** | Claude finishes a response | Post-response checks |
| **SubagentStop** | Subagent completes | Validate subagent output |

You can also filter hooks by tool name. A PostToolUse hook with `"tool": "Write"` only fires after file writes, not after every tool call.

---

## Pattern 1: Auto-Inject Metadata (UserPromptSubmit)

Claude doesn't know today's date. It doesn't know your current git branch. It doesn't know which project you're in unless it reads the directory. This hook solves all three — automatically, every single prompt.

### The Hook

```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "command": "~/.claude/hooks/inject-metadata.sh",
        "timeout": 2000
      }
    ]
  }
}
```

```bash
#!/bin/bash
# ~/.claude/hooks/inject-metadata.sh
# Inject date and git context before every prompt

BRANCH=$(git branch --show-current 2>/dev/null || echo "n/a")
REPO=$(basename "$(git rev-parse --show-toplevel 2>/dev/null)" 2>/dev/null || echo "unknown")

echo "Date: $(date +%Y-%m-%d) | Repo: ${REPO} | Branch: ${BRANCH}"
```

### Why This Matters

Without this hook, you get conversations like:

- "Create a dated filename" → Claude uses a hallucinated date
- "Are we on the feature branch?" → Claude guesses or asks you
- "What repo is this?" → Claude reads the directory name (wasting a tool call)

With this hook, every single prompt starts with accurate metadata. Claude sees `Date: 2026-04-06 | Repo: my-project | Branch: feat/auth` before it even reads your message.

### Variations

Add whatever context you need. Some useful additions:

```bash
# Add Node version for JS projects
echo "Node: $(node -v 2>/dev/null || echo 'n/a')"

# Add active TODO count
TODO_COUNT=$(grep -r "TODO" src/ 2>/dev/null | wc -l | tr -d ' ')
echo "Open TODOs: ${TODO_COUNT}"

# Add last commit summary
LAST_COMMIT=$(git log --oneline -1 2>/dev/null || echo "no commits")
echo "Last commit: ${LAST_COMMIT}"
```

Keep it fast. This runs on every prompt. If it takes more than a second, you'll feel the delay.

---

## Pattern 2: Post-Write Validation (PostToolUse)

Claude sometimes writes files that are structurally correct but miss project conventions. Missing frontmatter, wrong file encoding, incorrect line endings, missing required fields. A post-write hook catches these immediately — before you discover the problem hours later.

### The Hook

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "tool": "Write",
        "command": "~/.claude/hooks/validate-write.sh",
        "timeout": 3000
      }
    ]
  }
}
```

```bash
#!/bin/bash
# ~/.claude/hooks/validate-write.sh
# Validate that written files meet project conventions

# The harness sets this env var to the path of the file Claude just wrote
FILE="$TOOL_INPUT_FILE_PATH"

# --- Check 1: Frontmatter enforcement ---
# Markdown files in notes/ must start with YAML frontmatter ("---")
# Without this, static site generators and Obsidian plugins break silently
if [[ "$FILE" == *"notes/"* && "$FILE" == *.md ]]; then
  FIRST_LINE=$(head -1 "$FILE")
  if [[ "$FIRST_LINE" != "---" ]]; then
    echo "WARNING: File missing YAML frontmatter: $FILE"
    echo "Notes files require frontmatter with at least: title, date, tags"
  fi
fi

# --- Check 2: Executable bit ---
# Shell scripts must be executable, or they fail on first run
if [[ "$FILE" == *.sh ]]; then
  if [[ ! -x "$FILE" ]]; then
    chmod +x "$FILE"
    echo "INFO: Made $FILE executable"
  fi
fi

# --- Check 3: Credential leak detection ---
# Basic regex for common credential patterns (won't catch everything,
# but catches the obvious api_key="sk-..." cases)
if grep -qE '(api_key|token|secret|password)\s*[:=]\s*["\x27][A-Za-z0-9]' "$FILE" 2>/dev/null; then
  echo "WARNING: Possible hardcoded credential detected in $FILE"
  echo "Use environment variables instead of inline secrets"
fi
```

### Why This Matters

The frontmatter check catches a common problem: Claude creates a note that looks perfect in the editor but breaks your static site generator, search indexer, or Obsidian plugin because it's missing required metadata.

The executable check prevents the "why won't this script run" moment that wastes 5 minutes every time.

The credential check is a safety net. It won't catch everything, but it catches the obvious cases — and those are the most common ones.

### Variations

Adapt the validation to your project:

```bash
# Check: Python files should have type hints on functions
if [[ "$FILE" == *.py ]]; then
  UNTYPED=$(grep -n "def " "$FILE" | grep -v "->")
  if [[ -n "$UNTYPED" ]]; then
    echo "WARNING: Functions without return type hints:"
    echo "$UNTYPED"
  fi
fi

# Check: React components should have display names
if [[ "$FILE" == *.tsx && "$FILE" == *"components/"* ]]; then
  if ! grep -q "displayName" "$FILE"; then
    echo "INFO: Component missing displayName — consider adding for debugging"
  fi
fi
```

---

## Pattern 3: Post-Compact Context Recovery (PostCompact)

Context compression (`/compact`) is essential for long sessions, but it's lossy. After compression, Claude loses working memory: what task you're on, which files you're editing, what errors you've seen, what approach you decided on. A PostCompact hook re-injects the critical context automatically.

### The Hook

```json
{
  "hooks": {
    "PostCompact": [
      {
        "command": "~/.claude/hooks/post-compact.sh",
        "timeout": 3000
      }
    ]
  }
}
```

```bash
#!/bin/bash
# ~/.claude/hooks/post-compact.sh
# Re-inject critical context after compression

CONTEXT_FILE=".claude/compact-context.md"

if [[ -f "$CONTEXT_FILE" ]]; then
  echo "=== Post-Compact Context Recovery ==="
  cat "$CONTEXT_FILE"
  echo "=== End Recovery ==="
else
  # Fallback: inject basic project context
  echo "=== Post-Compact Context Recovery ==="
  
  # Current branch and recent commits
  BRANCH=$(git branch --show-current 2>/dev/null)
  if [[ -n "$BRANCH" ]]; then
    echo "Branch: $BRANCH"
    echo "Recent commits:"
    git log --oneline -5 2>/dev/null
  fi
  
  # Recently modified files (likely what we're working on)
  echo ""
  echo "Recently modified files:"
  git diff --name-only HEAD~3 2>/dev/null || find . -name "*.md" -newer .git/HEAD -maxdepth 3 2>/dev/null | head -10
  
  echo "=== End Recovery ==="
fi
```

### The Workflow

The hook above checks for a `.claude/compact-context.md` file. The idea: before you `/compact`, you (or Claude) write a brief summary of current state to that file. After compression, the hook re-injects it.

You can also ask Claude to maintain this file as part of your CLAUDE.md rules:

```
Before any /compact, update .claude/compact-context.md with:
- Current task and progress
- Key file paths being edited
- Error context if debugging
- Decisions made so far
```

Or do it automatically with a pre-compact check. The point is: compression should not mean amnesia.

### What to Include in Recovery Context

Keep it short. The whole point of compacting is to free up space. Don't re-inject 2000 tokens.

Good recovery context (~200 tokens):
- Current task: "Adding pagination to /api/users endpoint"
- Key files: `src/routes/users.ts`, `src/lib/paginate.ts`
- Status: "Pagination logic works, need to add Link headers"
- Blockers: "None"

Bad recovery context (~2000 tokens):
- Full error stack traces
- Complete file contents
- Conversation history summary
- Every decision and alternative considered

---

## When NOT to Use Hooks

Hooks are deterministic. That's their strength and their limitation. Don't use them for:

- **Logic that requires judgment** -- "Only lint if the file is important." What's "important"? That's a judgment call. Put it in CLAUDE.md where Claude can reason about it. Hooks should have clear, mechanical criteria.
- **Complex multi-step workflows** -- If your hook needs to read 5 files, make an API call, and decide between 3 approaches, that's a skill, not a hook. Hooks should be fast, simple, and reliable. A hook that takes 10 seconds or occasionally fails defeats its purpose.
- **One-time actions** -- "For this specific task, check X before writing." Just say it in conversation. Hooks are for things that should always happen, not things that should happen once.
- **Debugging hooks of other hooks** -- If you find yourself writing a PostToolUse hook to validate that your PreToolUse hook worked correctly, step back. Simplify the original hook.

---

## Configuration Reference

Hooks live in your `settings.json`:

```json
{
  "hooks": {
    "EventName": [
      {
        "command": "path/to/script.sh",
        "tool": "ToolName",
        "timeout": 3000
      }
    ]
  }
}
```

- **`command`** -- Shell command or script path. Receives tool context via environment variables.
- **`tool`** -- (Optional) Filter to specific tools. Only applies to PreToolUse and PostToolUse.
- **`timeout`** -- (Optional) Kill the hook if it takes longer than this (milliseconds). Default varies by event.

### Environment Variables by Event

The harness passes context to your hook scripts via environment variables. The most commonly used ones:

| Event | Variable | Contains |
|-------|----------|----------|
| PreToolUse / PostToolUse | `$TOOL_INPUT_FILE_PATH` | Path of the file being read/written |
| PreToolUse / PostToolUse | `$TOOL_INPUT` | Full JSON of the tool's input parameters |
| PostToolUse | `$TOOL_OUTPUT` | The tool's output (e.g., file content, command result) |
| All events | `$SESSION_ID` | Current Claude Code session ID |
| All events | `$HOOK_EVENT` | The event name (e.g., `PostToolUse`) |

For the complete list, run `claude --help hooks` or see the [Claude Code docs on hooks](https://docs.anthropic.com/en/docs/claude-code/hooks).

---

## Getting Started

Start with one hook. The metadata injection hook (Pattern 1) is the easiest to set up and gives immediate value. Run it for a week. Once you trust the mechanism, add validation hooks for your specific project conventions.

The best hooks are the ones you forget exist — they just silently make everything better.
