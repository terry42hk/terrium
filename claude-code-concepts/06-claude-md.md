CLAUDE.md is the highest-leverage configuration in Claude Code. It is loaded every single turn, survives compression, and sits at the primacy position in context. Most people either leave it empty or overload it.

## Table of Contents

- [Three Privileges That Make CLAUDE.md Special](#three-privileges-that-make-claudemd-special)
- [Four-Layer Hierarchy](#four-layer-hierarchy)
- [What to Write (The Golden Test)](#what-to-write-the-golden-test)
- [CLAUDE.md vs Other Persistence Mechanisms](#claudemd-vs-other-persistence-mechanisms)
- [Position in the Four-Layer Control Stack](#position-in-the-four-layer-control-stack)
- [Common Mistakes](#common-mistakes)
- [Latest Features (2026)](#latest-features-2026)

## Three Privileges That Make CLAUDE.md Special

| Property | Conversation instruction | CLAUDE.md |
|----------|------------------------|-----------|
| When loaded | Once, when you say it | **Every single turn** |
| After compression | May be summarized away | **Reloaded intact** |
| Context position | Middle (easily overlooked) | **Beginning (primacy effect)** |

Most persistent + most visible + most stable = the single most effective instruction channel.

## Four-Layer Hierarchy

```
~/.claude/CLAUDE.md           -> Global (all projects) -- personal style, preferences
  ./CLAUDE.md                 -> Project (team-shared, committed to git)
    .claude/rules/*.md        -> Modular rules (conditional loading via globs)
      CLAUDE.local.md         -> Private local (gitignored, personal project prefs)
```

Rules closer to the current working directory have higher priority. Rules files support `globs` frontmatter for conditional loading (e.g., only load credential rules when editing settings files).

## What to Write (The Golden Test)

> **For each line, ask: "If I delete this line, will Claude make mistakes?"**
> Yes -> keep. No -> remove.

### Write these (Claude cannot infer from code)
| Type | Example |
|------|---------|
| Decisions & conventions | "Use ESM, not CommonJS" |
| Team norms | "Commit messages use conventional commits" |
| Prohibitions | "Never mock the database in integration tests" |
| Language/style preferences | "Chinese discussion, English documents" |
| Skill routing | "Debug -> systematic-debugging skill" |
| Verification standards | "Verify output, not exit code" |

### Don't write these (Claude can figure out)
| Type | Why not |
|------|---------|
| File structure descriptions | Claude can `ls` / `Glob` |
| "You are a helpful assistant" | Already in system prompt |
| Code style (if linter exists) | ESLint/Prettier config is authoritative |
| API documentation | The code itself is documentation |

## CLAUDE.md vs Other Persistence Mechanisms

| | CLAUDE.md | Memory (MEMORY.md) | Session Memory | Conversation |
|--|-----------|-------------------|----------------|-------------|
| Persistence | Permanent -- file-based | Permanent -- cross-session | Single session | Single session, lost on compact |
| Loaded when | Every turn | Session start | Extracted on compact | Immediate |
| Content type | Rules, norms, routing | Learned facts, preferences | Current task progress | Ad-hoc instructions |
| Who writes | **You** | Claude auto + you | Claude auto | You |
| Analogy | Employee handbook | Personal notebook | Today's to-do list | Verbal instruction |

## Position in the Four-Layer Control Stack

```
Permission  -> May you do this?           <- Hard, deterministic
Hook        -> What else happens?          <- Hard, deterministic
CLAUDE.md   -> How should you do it?       <- Soft but PERSISTENT
Conversation-> Do it this way this time    <- Soft, lost on compaction
```

CLAUDE.md is the only layer that is both **persistent** (survives compression) and **judgment-based** (requires model understanding). Permission/Hook are persistent but mechanical. Conversation is judgment-based but ephemeral.

## Common Mistakes

| Mistake | Consequence | Fix |
|---------|------------|-----|
| Empty CLAUDE.md | Claude is a generic assistant | Write at least 10-20 lines of core rules |
| 500-line CLAUDE.md | Attention diluted, rules ignored | Keep core to 20-50 lines, split detail to rules/ |
| Write once, never update | Rules go stale | Review when Claude misbehaves; monthly pruning |
| Put deterministic rules here | Occasionally disobeyed | Deterministic rules -> Hook or Permission |
| Duplicate info across layers | Wastes context budget | Single source of truth per rule |

## Latest Features (2026)

| Feature | Status | Notes |
|---------|--------|-------|
| `globs` frontmatter in rules | Available | Conditionally load rules based on file patterns |
| `@include` / `@path` imports | Available | Reference other files from CLAUDE.md |
| `.claude/rules/*.md` modular rules | Available | Split rules into focused files |
| 40K character budget | Current limit | Plenty for well-structured rules |

## For Non-Technical Users

CLAUDE.md is like a set of house rules posted on the fridge -- everyone sees it every morning. It tells Claude your preferences, your team's conventions, and how you like things done. It's the single most impactful thing you can set up, because Claude reads it every single time it thinks. But remember: house rules are reminders, not locks. If you need something absolutely enforced (like "never delete this folder"), that's a lock (Permission), not a fridge note.

## Reflection Questions

1. Apply the Golden Test to your own CLAUDE.md (or imagine one): pick any 3 lines and ask "If I delete this, will Claude make mistakes?" What did you find?
2. You have a 300-line CLAUDE.md and Claude is ignoring some rules. What's likely wrong, and how would you fix it?
3. Why does "Never force push to main" belong in Permission, not CLAUDE.md, even though CLAUDE.md is loaded every turn?
