Skills are reusable prompt templates packaged as /slash-commands. Write once, use forever, share with team. They sit between "a single CLAUDE.md rule" and "a full custom agent" in complexity.

## Table of Contents

- [Skill Structure](#skill-structure)
- [Key Frontmatter Fields (2026)](#key-frontmatter-fields-2026)
- [How Skills Load (Context-Efficient)](#how-skills-load-context-efficient)
- [Critical Frontmatter: `context: fork`](#critical-frontmatter-context-fork)
- [Critical Frontmatter: `disable-model-invocation: true`](#critical-frontmatter-disable-model-invocation-true)
- [Description Quality Matters](#description-quality-matters)
- [Skills vs Other Mechanisms](#skills-vs-other-mechanisms)

## Skill Structure

```
.claude/skills/
  +-- review-pr/
      +-- SKILL.md        <- Required (prompt + frontmatter)
      +-- template.md     <- Optional (output template)
      +-- examples/       <- Optional (reference files)
```

## Key Frontmatter Fields (2026)

| Field | Purpose | Example |
|-------|---------|---------|
| `name` | Slash command name | `review-pr` -> `/review-pr` |
| `description` | Claude reads this to decide when to auto-load (250 char limit) | "Review PRs for code quality and security" |
| `context: fork` | Run in isolated subagent context | Prevents context pollution |
| `agent` | Subagent type to use | `Explore`, `Plan`, `general-purpose` |
| `model` | Override model | `opus`, `sonnet`, `haiku` |
| `disable-model-invocation: true` | Manual-only, Claude cannot auto-trigger | For skills with side effects |
| `allowed-tools` | Restrict available tools | `Read, Grep, Glob` |
| `disallowed-tools` | Block specific tools | `Bash, Edit` |

## How Skills Load (Context-Efficient)

1. Session start: Claude sees **descriptions only** (~250 chars each) for all installed skills
2. When triggered (by `/command` or description match): **full SKILL.md content** loads into context
3. If `context: fork`: skill runs in subagent, full content never enters main context

This is much more context-efficient than putting everything in CLAUDE.md.

## Critical Frontmatter: `context: fork`

Any skill that reads many files (research, analysis, review) should have:
```yaml
context: fork
```
This auto-delegates to a subagent -- the main context only receives the summary.

## Critical Frontmatter: `disable-model-invocation: true`

Skills with side effects must not be auto-triggered:

| Should disable auto-invocation | Reason |
|-------------------------------|--------|
| git-conventions | Git operations |
| batch-processing skills | Batch operations, chains other skills |
| file-modifying skills | Modifies files |

| Can keep auto-invocation | Reason |
|-------------------------|--------|
| systematic-debugging | Useful when Claude hits a bug |
| knowledge-capture | Should happen naturally |
| translation | Easy for Claude to detect need |

## Description Quality Matters

Claude routes to skills based on description matching. Poor descriptions = skills never get used.

```yaml
# Bad -- too vague, Claude won't know when to trigger
description: Help with writing

# Good -- clear triggers, Claude knows exactly when to use
description: >
  Review and standardize web clippings in your notes.
  Trigger when user asks to format, organize, or clean up
  newly clipped web articles or Twitter threads.
```

## Skills vs Other Mechanisms

| | Skills | CLAUDE.md | Hooks | Custom Agents |
|--|--------|-----------|-------|---------------|
| Loaded when | On demand (description match or `/command`) | Every turn | Every tool call | When Agent tool invoked |
| Context cost | ~250 chars always; full text on demand | Every turn | Near zero | Isolated context |
| Best for | Multi-step workflows | Short rules and preferences | Mechanical automation | Independent roles (reviewer) |

**Rule of thumb:**
- **One rule** -> CLAUDE.md
- **One workflow** -> Skill
- **One action** -> Hook
- **One role** -> Custom Agent

## For Non-Technical Users

```
1. Type / then tab -> see all available skills
2. /skill-name -> trigger a skill
3. Claude sometimes auto-triggers skills (if description matches)
4. Ask Claude to create new skills: "Create a skill that does X"
```

## Reflection Questions

1. You have a research skill that reads 20+ files. Should you add `context: fork` to its frontmatter? What happens if you don't?
2. What makes a good skill description? Why does "Helps with code" fail while "Use when debugging test failures -- runs failing test, reads stack trace, proposes fix" works?
3. When would you choose to create a Skill vs a Custom Agent? What's the key difference in capability?
