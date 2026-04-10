Tools are what turn Claude from a chatbot into an agent. But they are not "the model's hands" -- they are managed execution interfaces governed by the harness.

## Table of Contents

- [The Fundamental Shift](#the-fundamental-shift)
- [Tool Categories](#tool-categories)
- [Tools Are Not Model Extensions -- They Are Managed Interfaces](#tools-are-not-model-extensions--they-are-managed-interfaces)
- [The Real Execution Pipeline](#the-real-execution-pipeline)
- [Concurrency: Fast but Ordered](#concurrency-fast-but-ordered)
- [Why Dedicated Tools Instead of Just Bash?](#why-dedicated-tools-instead-of-just-bash)
- [Bash: The Risk Amplifier](#bash-the-risk-amplifier)
- [Connection to Previous Concepts](#connection-to-previous-concepts)

## The Fundamental Shift

> *"A model that only outputs text, when wrong, mainly increases communication cost. But once a model starts calling tools, the nature of the problem changes. Tools are not opinions -- tools are actions. Actions leave consequences in the real world."*

## Tool Categories

### File Operations
| Tool | Purpose |
|------|---------|
| **Read** | Read file contents |
| **Edit** | Precise string replacement in files |
| **Write** | Create new files or full overwrite |

### Search
| Tool | Purpose |
|------|---------|
| **Grep** | Regex search across file **contents** |
| **Glob** | Pattern match across file **names** |

### Execution
| Tool | Purpose |
|------|---------|
| **Bash** | Run any shell command -- most powerful and most dangerous |

### Web
| Tool | Purpose |
|------|---------|
| **WebSearch** | Search the internet |
| **WebFetch** | Fetch a specific URL |

### Agent
| Tool | Purpose |
|------|---------|
| **Agent** | Spawn an isolated subagent with its own context |

## Tools Are Not Model Extensions -- They Are Managed Interfaces

Wrong mental model:
```
Model wants to act -> Model acts -> Done
(Model is master, tool is arm)
```

Correct mental model:
```
Model proposes tool_use request
    |
Harness checks permission -> allow / deny / ask
    |
Harness schedules execution (concurrent? sequential?)
    |
Harness executes tool
    |
Harness handles result (success? failure? interrupted?)
    |
Result returned to model
```

> *"Tools should not be modeled as 'extensions of model capability', but as 'external capabilities whose risk the runtime must manage on behalf of the caller'."*

## The Real Execution Pipeline

What you think happens: `Model calls Read -> file is read -> content returned`

What actually happens:

```
1. Permission check -> deny / ask / allow
2. PreToolUse hooks -> may block execution
3. Actual tool execution
4. PostToolUse hooks -> automated post-actions (e.g., formatter)
5. Context modifier -> update conversation context
6. Result returned to model -> enters next loop turn
```

Every step is harness-controlled. The model only handles step 0 (propose) and the end (receive result).

## Concurrency: Fast but Ordered

Harness partitions tool calls by concurrency safety:

- **Safe to parallelize** (e.g., 3 independent `Read` calls) -> run concurrently
- **Causally dependent** (e.g., `Edit` then `Bash: npm test`) -> run sequentially

Even when running concurrently, context modifications replay in **original block order**, preserving causal consistency.

> *"Concurrency can improve throughput, but must not break causal order."*

## Why Dedicated Tools Instead of Just Bash?

| Aspect | Bash (`cat`, `sed`, `grep`) | Dedicated tool (Read, Edit, Grep) |
|--------|---------------------------|----------------------------------|
| Safety | `cat` + redirect can overwrite files | Read cannot modify anything |
| Auditability | Must parse shell command to understand | `Edit: auth.ts line 42` is self-documenting |
| Permission granularity | All-or-nothing Bash permission | Allow Read but ask for Bash |
| Hook precision | Hard to match `sed` calls | PostToolUse can target `Edit` specifically |
| Context efficiency | Shell metadata overhead | Returns only relevant content |

Dedicated tools = universal capability split into bounded, governed units.

## Bash: The Risk Amplifier

> *"Bash is not an ordinary tool -- it is more like a risk amplifier. The more universal an interface, the harder it is to constrain by domain knowledge."*

Claude Code applies special high-density constraints to Bash:
- Detailed rules for git operations (no force push, no skip hooks, no `git add .`)
- Subcommand count limits for compound commands
- Dangerous commands (`rm -rf`, `git reset --hard`) default to ask permission

**Principle: The more powerful the capability, the tighter the governance.**

## Connection to Previous Concepts

| Concept | Relationship to Tools |
|---------|----------------------|
| **01 Harness vs Model** | Model proposes tool calls; Harness decides permission, scheduling, execution |
| **02 Agent Loop** | Tools are the "act" step in the loop. Without tools, the loop can only output text |

## For Non-Technical Users

Tools are how Claude interacts with your computer -- reading files, editing code, running commands. But Claude doesn't directly control them; it *requests* to use them, and the system decides whether to allow it. It's like an employee filling out a form to check out equipment: they specify what they need, and the office manager decides whether to approve it, deny it, or ask you first.

## Reflection Questions

1. Why does Claude Code have both a `Read` tool and `Bash(cat)`? What's the advantage of dedicated tools over a single general-purpose shell?
2. When Claude proposes a tool call, what happens between "Claude decides to edit a file" and "the file is actually edited"? List the steps.
3. Why is Bash described as a "risk amplifier"? Can you think of a Bash command that looks harmless but could cause serious damage?

---

**Workflow:** [Verification Workflow](../workflows/verification-workflow.md)

---

← Previous: [02 Agent Loop](02-agent-loop.md) | [Course Overview](README.md) | Next: [04 Permission Model](04-permission-model.md) →
