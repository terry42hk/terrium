The most common mistake new Claude Code users make is putting the wrong rule in the wrong layer. Understanding which behaviors come from the **model** (probabilistic) versus the **harness** (deterministic) is the first concept to internalize.

## Table of Contents

- [Two Roles, One System](#two-roles-one-system)
- [The Four-Layer Control Stack (Hard to Soft)](#the-four-layer-control-stack-hard--soft)
  - [Decision tree: where does a rule belong?](#decision-tree-where-does-a-rule-belong)
  - [Examples](#examples)
- [CLAUDE.md Solves Memory, Not Execution](#claudemd-solves-memory-not-execution)
- [The Core Principle from Harness Engineering](#the-core-principle-from-harness-engineering)
- [Common Beginner Mistakes](#common-beginner-mistakes)

## Two Roles, One System

| | Model (Claude) | Harness (Claude Code) |
|--|--|--|
| Identity | A brilliant but occasionally careless employee | The office's access control and SOPs |
| Does what | Thinks, reasons, understands code, decides next steps | Executes tools, manages permissions, compresses context, runs hooks |
| Nature | **Probabilistic** -- mostly correct, but forgets, cuts corners, confidently errs | **Deterministic** -- rules execute every time, no mood swings |
| Controlled by | CLAUDE.md, prompts, conversation instructions | settings.json, hooks, permissions, CLI flags |
| Failure mode | Ignores instructions, hallucinates, overconfident | Misconfiguration, rule conflicts |

## The Four-Layer Control Stack (Hard -> Soft)

```
Permission  -> Security boundary (what's forbidden)         <- Hardest
Hook        -> Automated process (what must always happen)   <- Hard
CLAUDE.md   -> Judgment guidance (how things should be done) <- Soft but persistent
Conversation-> Ad-hoc requests (do it this way this time)    <- Softest, lost on compaction
```

### Decision tree: where does a rule belong?

```
Does this rule require AI to understand context before acting?
|
+- No (mechanical rule)
|   +- "Never allowed"  -> Permission (deny)
|   +- "Always do this" -> Hook
|
+- Yes (judgment-based guidance)
|
    +- CLAUDE.md
```

### Examples

| Rule | Needs judgment? | Where |
|------|----------------|-------|
| Run prettier after every edit | No -- run every time | **Hook** (PostToolUse) |
| Never force push | No -- always forbidden | **Permission** (deny) |
| Use ES modules, not require() | Yes -- model must understand JS context | **CLAUDE.md** |
| Update docs after API changes | Yes -- model must judge what to update | **CLAUDE.md** |

## CLAUDE.md Solves Memory, Not Execution

A common confusion: CLAUDE.md is loaded every turn (survives compaction), but the model still might not follow it 100%.

| | CLAUDE.md | Conversation instruction |
|--|--|--|
| Survives compaction? | Yes -- reloaded every turn | No -- may be summarized away |
| 100% obeyed? | No -- still probabilistic | No -- also probabilistic |

Analogy:
- **CLAUDE.md** = A sign posted on the office wall. Visible every day (won't disappear), but employees sometimes rush past it (not guaranteed to obey)
- **Conversation instruction** = A verbal reminder. Forgotten in two hours (disappears), and equally not guaranteed
- **Hook** = An automated gate. The employee doesn't need to remember; the system enforces it (100%)

## The Core Principle from Harness Engineering

> *"An agent system's key capability is constrained execution."*
> *"A system cannot rely on 'smartness' to maintain order -- only structure can. Structure isn't as glamorous as intelligence, but it's usually more reliable."*

The five layers of harness in Claude Code's architecture:

1. **System Prompt** -> Control plane, not personality decoration
2. **Query Loop** -> Stateful continuous loop, not single-turn Q&A
3. **Tool Orchestration** -> Scheduling discipline, not free-for-all execution
4. **Bash Constraints** -> Highest-risk tool gets highest-density rules
5. **Error as Main Path** -> Failure is structural, not exceptional

All five are **harness** responsibilities. The model only handles the top layer -- thinking and reasoning.

## Common Beginner Mistakes

| Mistake | Root cause | Fix |
|---------|-----------|-----|
| Put all rules in CLAUDE.md | Thinks CLAUDE.md is omnipotent | Deterministic rules -> Hook/Permission |
| Thinks Claude "deliberately" disobeys | Confuses probabilistic with intentional | Understand occasional misses are normal |
| Blames Claude for dangerous actions | Didn't set permissions | Set permission boundaries before starting |
| Expects Claude to remember everything | Confuses model memory with harness memory | Persistent info -> CLAUDE.md or memory files |

## For Non-Technical Users

Think of Claude Code as having two parts: a **smart employee** (the model) and a **building security system** (the harness). The employee is brilliant but sometimes forgetful -- they might skip a step or cut corners. The security system doesn't think, but it never forgets to lock the door. When you set up Claude Code, you're deciding which rules go on the employee's desk (CLAUDE.md -- reminders) vs which rules are enforced by the building itself (permissions and hooks -- automatic).

## Reflection Questions

1. You want Claude to always run `npm test` after editing any `.ts` file. Where does this rule belong -- CLAUDE.md, Hook, or Permission? Why?
2. A colleague puts "Never delete production databases" in CLAUDE.md. What's the risk, and where should this rule actually live?
3. Think of a rule in your own workflow that you currently enforce through conversation instructions. Should it be in CLAUDE.md, a Hook, or a Permission instead?
