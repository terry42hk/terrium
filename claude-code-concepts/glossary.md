# Claude Code Concepts -- Glossary

Key terms for beginners working through the Claude Code Concepts Course.

## A

**Agent Loop**
The continuous cycle of governance -> reasoning -> tool execution -> re-governance that makes Claude Code an agent, not a chatbot. Each cycle is one "heartbeat." See [02 Agent Loop](02-agent-loop.md).

**Autocompact**
Automatic context compression triggered when usage exceeds a threshold (default ~80%, recommended 60%). The harness compresses the conversation and reloads critical context. See [05 Context Window](05-context-window.md).

## C

**CLAUDE.md**
The primary configuration file for Claude Code. Loaded at the beginning of every turn, survives compression. Contains judgment-based guidance -- preferences, conventions, routing rules. See [06 CLAUDE.md](06-claude-md.md).

**Compact / /compact**
Manual context compression command. Optionally takes a focus hint (e.g., `/compact focus on the API migration`). Preserves CLAUDE.md, current plan, and recent files. See [09 Context Management](09-context-management.md).

**Context Window**
The total amount of text (measured in tokens) Claude can "see" at once. Think of it as a desk -- everything must fit on it simultaneously. Default 200K tokens, up to 1M extended. See [05 Context Window](05-context-window.md).

## D

**Deny (Permission)**
A permission result that blocks a tool call unconditionally. The model receives a synthetic error. Used for irreversible or dangerous operations. See [04 Permission Model](04-permission-model.md).

**Deterministic**
Always produces the same result given the same input. Hooks, Permissions, and harness operations are deterministic -- they execute every time, no exceptions. Opposite of probabilistic.

## F

**Frontmatter**
YAML metadata at the top of a markdown file, enclosed in `---`. Used in skill files and agent definitions to specify properties like title, tags, and configuration.

## G

**Governance**
The harness preprocessing phase before each model reasoning step. Includes loading CLAUDE.md, prefetching memory, trimming context, and running compression. Happens every loop turn, not just at session start.

## H

**Harness**
The shell/runtime that wraps the AI model. In Claude Code, the harness handles permissions, hooks, context management, tool execution, and error recovery. It is deterministic. See [01 Harness vs Model](01-harness-vs-model.md).

**Hook**
A shell command that fires at a specific lifecycle event (e.g., before a tool runs, after compression, when a session starts). Configured in `settings.json`. Deterministic -- always executes. See [10 Hooks](10-hooks.md).

## L

**Lost in the Middle**
A 2023 research finding that LLMs attend less to information in the middle of their context. Largely mitigated in 2026 frontier models, but signal-to-noise still matters. See [05 Context Window](05-context-window.md).

## M

**Matcher**
A pattern in hook configuration that specifies which tool or event triggers the hook (e.g., `"matcher": "Edit"` fires only for Edit tool calls).

**MCP (Model Context Protocol)**
An open standard that lets Claude connect to external services (GitHub, Trello, browsers, databases) through standardized tool interfaces. See [12 MCP](12-mcp.md).

**Model**
The AI (Claude) that thinks, reasons, and decides. Probabilistic -- usually correct, but can forget, hallucinate, or ignore instructions. Controlled by CLAUDE.md and conversation context.

## P

**Permission**
The harness layer that decides whether a tool call is allowed, denied, or requires user approval. The hardest (most reliable) control layer. See [04 Permission Model](04-permission-model.md).

**Plan Mode**
A permission mode where Claude can only read -- no edits, no commands. Used for safe exploration of unfamiliar codebases. Activated with Shift+Tab.

**Primacy Effect**
The tendency for information at the beginning of a text to receive more attention. CLAUDE.md benefits from this by being loaded at the start of context.

**Probabilistic**
May produce different results or occasionally fail. The model's behavior is probabilistic -- it usually follows CLAUDE.md rules, but not 100% of the time. Opposite of deterministic.

## R

**Re-governance**
The harness re-running its governance pipeline after each tool call. Necessary because context has changed (new file contents, command outputs) and may need compression or trimming.

## S

**settings.json**
The configuration file for harness-level controls: permissions (allow/deny lists), hooks, and MCP server connections. Located at `.claude/settings.json` (project) or `~/.claude/settings.json` (global).

**Signal-to-Noise Ratio**
The proportion of useful information vs irrelevant content in the context window. High S/N = better Claude performance. The primary reason context management still matters in 2026.

**Skill**
A reusable prompt template packaged as a `/slash-command`. Sits between a CLAUDE.md rule and a custom agent in complexity. See [11 Skills](11-skills.md).

**Subagent**
An isolated Claude instance spawned by the main agent to handle a specific task. Has its own context window. Results return as a summary to the parent. See [08 Subagents](08-subagents.md).

## T

**Token**
The basic unit of text that LLMs process. Roughly 0.75 words per token (English). Context window sizes are measured in tokens.

**Tool**
A managed execution interface that lets Claude interact with the world -- read files, edit code, run commands, search the web. Each tool call goes through the permission and hook pipeline. See [03 Tools](03-tools.md).

## V

**Verification**
The practice of checking Claude's output for correctness before accepting it. The highest-leverage habit for AI agent users. See [07 Verification](07-verification.md).
