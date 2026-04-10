Subagents protect your main context by running tasks in isolated loops. Agent Teams go further -- multiple independent sessions that debate, challenge, and coordinate with each other.

## Table of Contents

- [Subagent vs Agent Team](#subagent-vs-agent-team)
- [Three Built-In Subagent Types](#three-built-in-subagent-types)
- [Custom Subagents in `.claude/agents/`](#custom-subagents-in-claudeagents)
- [Seven Quality Improvement Methods (Ranked by Effectiveness)](#seven-quality-improvement-methods-ranked-by-effectiveness)
- [Recommended Combinations by Risk Level](#recommended-combinations-by-risk-level)
- [Self-Audit: What Works and What Doesn't](#self-audit-what-works-and-what-doesnt)
- [Agent Team Details (Experimental, Feb 2026)](#agent-team-details-experimental-feb-2026)
- [Subagent Discipline](#subagent-discipline)
- [Connection to Previous Concepts](#connection-to-previous-concepts)

## Subagent vs Agent Team

| | Subagent | Agent Team |
|--|---------|------------|
| Architecture | Parent spawns child, one-way report back | Multiple independent Claude Code sessions, peer-to-peer messaging |
| Communication | One-way: parent -> child -> result | Bidirectional: agents message each other via file-based mailbox |
| Coordination | Parent manages all work | Shared task list with self-claiming |
| Context | Own isolated window; summary returns to parent | Fully independent context per agent |
| Token cost | Low (shared prompt cache -- 5 agents ~ cost of 1) | High (each agent has own full context) |
| Best for | Focused tasks, research, parallel processing | Complex work requiring discussion and debate |
| Status | Stable, production-ready | Experimental (requires flag) |

**Analogy:**
- **Subagent** = Send assistants to research separately, they bring back reports
- **Agent Team** = Colleagues sit together discussing, challenging each other's ideas

## Three Built-In Subagent Types

| Type | Model | Tools | Best for |
|------|-------|-------|----------|
| **Explore** | Haiku (fast) | Read, Grep, Glob (read-only) | "Find this file", "How does X work?" |
| **Plan** | Inherits parent | All except Edit/Write | System analysis, implementation planning |
| **General-purpose** | Inherits parent | All tools | Complex multi-step tasks with modifications |

## Custom Subagents in `.claude/agents/`

Codify frequently-used personas as agent files for consistency and persistent memory:

```markdown
# ~/.claude/agents/content-reviewer.md
---
name: content-reviewer
description: Reviews written content for clarity, accuracy, and completeness
tools: Read, Grep, Glob
model: opus
memory: user
---
Review content for logical flow, factual accuracy, redundancy,
and missing perspectives. Provide specific references and rewrites.
```

Key frontmatter fields: `name`, `description`, `tools`, `model`, `memory` (user/project/local), `effort`, `isolation` (worktree), `maxTurns`, `skills`, `hooks`.

Setting `memory: user` gives the agent persistent memory across sessions -- it learns your style and standards over time.

## Seven Quality Improvement Methods (Ranked by Effectiveness)

### 1. Writer/Reviewer Session Separation (Most Effective)

```
Session A (Writer): Create content
    -> /clear or new session
Session B (Reviewer): Review with zero bias -- fresh context, never saw the creation process
```

The fresh context eliminates self-bias. Official Anthropic recommendation.

### 2. Custom Reviewer Subagents (Persistent Quality)

Codify your best review personas as `.claude/agents/` files. Benefits: consistent standards, persistent memory (`memory: user` compounds knowledge), shareable via git.

### 3. Plan -> Implement -> Verify Cycle

```
/plan -> Claude researches, proposes approach (read-only, zero side effects)
-> You review and approve
-> Normal mode -> Claude executes
-> Verify output
```

Intercept directional errors before any work is done.

### 4. Devil's Advocate Prompt (Quick Audit)

After any task: *"Find three problems with what you just did. Don't be polite."*

Claude's critique ability > self-confirmation ability. Asking "what's wrong?" yields genuine issues; asking "is this right?" gets biased "yes."

### 5. Adversarial Agent Team (Debate Mode)

Spawn teammates with opposing mandates -- one finds supporting evidence, one finds counterarguments, one moderates. The theory that survives debate is more likely correct.

Note: Experimental feature. High token cost. Best for important research, not daily tasks.

### 6. Multi-Pass Pipeline

Chain specialized subagents: Research -> Draft -> Review -> Polish. Each stage has focused context and clear criteria.

### 7. Parallel Exploration

Three focused subagents investigating different angles simultaneously. Research shows 3 focused agents consistently outperform 1 generalist agent working 3x as long -- because of context isolation.

## Recommended Combinations by Risk Level

```
Daily tasks (low risk):
  Direct execution + Devil's Advocate follow-up

Important content (medium risk):
  Plan first -> Execute -> Writer/Reviewer separation

Deep research (high risk):
  Parallel Exploration -> Adversarial debate -> Fresh session review
```

## Self-Audit: What Works and What Doesn't

| Method | Reliable? | Why |
|--------|-----------|-----|
| Same-conversation "is this right?" | No | Biased toward confirming own work |
| Same-conversation "find problems" | Partial | Better, but still has context bias |
| New session review | Strong | Zero bias -- fresh context |
| Custom reviewer subagent | Yes | Focused lens, persistent memory |
| Agent Team debate | Strong | Adversarial structure fights anchoring |

## Agent Team Details (Experimental, Feb 2026)

- Requires: `CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1`
- Communication: file-based mailbox
- Display modes: "in-process" (one terminal) or "split panes" (tmux/iTerm2)
- Shared task list with dependency tracking and file-lock-based claim prevention
- Hooks available: `TeammateIdle`, `TaskCreated`, `TaskCompleted`

## Subagent Discipline

| Rule | Principle | Prevents |
|------|-----------|----------|
| **Synthesize before forwarding** | Digest subagent results before using | Blindly forwarding unverified research |
| **Dispatch with a plan** | List division of work before launching 2+ subagents | Duplicate work or gaps |
| **No overlapping file scope** | Parallel subagents must not modify same files | Conflicts and overwrites |

## Connection to Previous Concepts

| Concept | Relationship |
|---------|-------------|
| **02 Agent Loop** | Each subagent has its own independent agent loop |
| **04 Permission** | Subagents inherit parent's permission settings |
| **05 Context Window** | Core value = protect main context from pollution |
| **07 Verification** | Parent must verify subagent output before use |

## For Non-Technical Users

Subagents are like sending assistants to do research in separate rooms. Each one works independently, doesn't clutter your main workspace, and brings back a summary. This is useful when a task requires reading lots of files or exploring different angles -- instead of loading everything into one crowded conversation, you send scouts out and review their reports. The most practical tip: when you finish creating something, start a fresh conversation and ask it to review the work with fresh eyes.

## Reflection Questions

1. You just wrote a 2000-word client proposal with Claude. What's the most effective way to catch errors before sending it? Why is a fresh session better than asking "review this" in the same conversation?
2. When would you use an Explore subagent vs a General-purpose subagent? What's the key difference in capability and cost?
3. Why is blindly passing subagent output to the user dangerous?

---

**Workflow:** [Subagent Orchestration](../workflows/subagent-orchestration.md)

---

← Previous: [07 Verification](07-verification.md) | [Course Overview](README.md) | Next: [09 Context Management](09-context-management.md) →
