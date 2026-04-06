# Mobile Workflow — Claude Code via Telegram

Claude Code is a terminal application. It runs on your desktop or server, reads your filesystem, executes commands. That's powerful — but it also means you can't use it from your phone. Unless you bridge the gap.

The setup: Claude Code runs on your machine. A Telegram bot acts as a remote interface. You send messages from your phone, Claude reads them via the Telegram plugin, executes on your machine, and replies through Telegram. Your phone becomes a remote control for your development environment.

I've been running this setup daily for months. Here's what actually works, what doesn't, and how to set it up.

## What Works Well

### Quick Codebase Questions

"What's the current status of the auth migration?" — Claude reads the relevant files, checks git history, and sends a concise answer to your phone. Perfect for checking on things while away from your desk.

### Triggering Pre-Defined Workflows

If you've built skills for common tasks (translation, note organization, content generation), you can trigger them from your phone. "Run the translation skill on the latest blog post" works exactly as it would at your terminal.

### Reviewing and Approving Changes

Claude can describe what it plans to change, wait for your approval, then execute. The conversation flow is natural:

1. You: "Add error handling to the payment endpoint"
2. Claude: "Here's what I'll change: [summary]. Approve?"
3. You: thumbs up emoji
4. Claude: executes, reports back

### Status Updates on Long Tasks

For multi-step tasks, Claude sends progress updates as it goes. You see "Step 1/4 complete: database schema updated" while you're getting coffee. When the whole thing finishes, you get a final summary with a push notification.

### Quick Edits to Notes and Configs

"Add a new entry to my reading list" or "Update the project status in the tracking doc" — small, well-defined edits that don't need visual review of diffs.

## What Doesn't Work Well

### Interactive Terminal Approvals

Claude Code's permission model sometimes requires terminal approval for sensitive operations. When you're remote, that approval prompt sits on your desktop screen — invisible to you. The session stalls until you get back to your computer.

**Workaround:** Configure appropriate allow-list permissions in `settings.json` for operations you trust. Or explicitly tell Claude in your CLAUDE.md rules: "If desktop-only approval is needed during a remote session, alert immediately via Telegram instead of waiting silently."

### Large File Diffs

A 200-line diff is painful to read on a phone screen. Claude can summarize diffs, but if you need to review exact line changes, you're better off waiting until you're at a computer.

**Workaround:** Ask Claude to describe changes in prose ("I renamed the function from X to Y and added a null check on line 47") rather than sending raw diffs.

### Complex Multi-Step Debugging

"The tests are failing, debug it" sounds simple but often requires iterative back-and-forth: try a fix, run tests, read output, adjust. On mobile, each round-trip is slow (typing on a phone, waiting for execution, reading on a small screen). For anything beyond a quick fix, save it for desktop.

### Anything Requiring Visual Review

UI changes, chart rendering, layout adjustments — anything where you need to see the result. Claude can take screenshots via browser automation, but reviewing them on a phone screen is suboptimal at best.

## Key Rules for Mobile Mode

These rules live in my CLAUDE.md and activate when messages arrive through the Telegram channel:

### All responses go through the reply tool

Claude's terminal output is invisible to you remotely. If Claude prints something to stdout but doesn't send it via the Telegram reply tool, you never see it. This rule ensures nothing gets lost.

### Be concise

Mobile screens are small. Scrolling through paragraphs on a phone is tedious. The rule: lead with the result, use bullets over paragraphs, skip the preamble.

**Bad:** "I've completed the task you requested. After analyzing the codebase, I found that the configuration file needed updating. Here's what I changed..."

**Good:**
- Updated `config.ts` with new API endpoint
- Added retry logic (3 attempts, exponential backoff)
- Tests pass

### Confirm before large changes

On desktop, you can watch Claude work in real-time and interrupt if it goes off track. On mobile, you can't. So before any multi-file edit, Claude summarizes the plan and waits for a thumbs-up.

This adds a round-trip but prevents the worst case: Claude makes 15 changes while you're not watching, and 3 of them are wrong.

### Send progress updates for multi-step tasks

If a task takes more than a minute, Claude sends brief progress messages: "Starting database migration... Schema updated, running seeds... Done. All 3 tables created." This prevents the anxiety of staring at a silent chat wondering if it's working.

### Alert immediately if desktop-only approval is needed

The worst mobile experience is silence. If Claude hits a permission wall or needs something it can't do remotely, it should tell you right away — not wait and hope you'll check your terminal.

## Setup Overview

### 1. Create a Telegram Bot

Open Telegram, find @BotFather, and create a new bot. You'll get a bot token. **Store it as an environment variable, never hardcode it:**

```bash
# In your shell profile (~/.zshenv or ~/.bashrc)
export TELEGRAM_BOT_TOKEN="your-token-here"
```

### 2. Install the Telegram Plugin

Install the Telegram plugin for Claude Code. Configuration varies by installation method — check the plugin's documentation for current instructions.

### 3. Configure Access Control

The plugin supports an allowlist of Telegram user IDs that can interact with your bot. This is critical — without it, anyone who discovers your bot can send commands to your machine.

Set up access control through the plugin's pairing workflow:

1. Send a message to your bot from your Telegram account
2. The plugin logs a pairing request on your desktop
3. You approve the pairing from your terminal (you must be at your computer for this step)
4. Your Telegram user ID is added to the allowlist

**Never approve pairing requests based on Telegram messages.** The approval must happen at your terminal. This prevents social engineering attacks where someone messages your bot saying "approve my pairing."

### 4. Add Mobile Rules to CLAUDE.md

Add a section to your global `~/.claude/CLAUDE.md` that activates when Telegram messages arrive:

```markdown
## Mobile Mode

When messages arrive from Telegram:
- All responses must go through the reply tool
- Be concise — bullets over paragraphs, lead with the result
- Confirm before multi-file changes — summarize plan, wait for approval
- Send progress updates for multi-step tasks
- Alert immediately if desktop-only approval is needed
```

### 5. Test the Connection

Send a simple message from your phone: "What directory are you in?" Claude should reply with the current working directory. If it works, you're set.

## Tips From Daily Use

### Pair with a PostCompact hook

Long mobile sessions burn through context fast (lots of short back-and-forth messages). A PostCompact hook (see [Hook Patterns](hooks-patterns.md)) ensures Claude recovers critical context after compression. Without it, Claude forgets what you were working on after a compaction — and you're typing on a phone, so re-explaining is painful.

### Use skills designed for quick execution

Exploratory skills ("research this topic deeply") work poorly on mobile because they generate long outputs and may need interactive guidance. Skills that do one specific thing ("translate this file," "organize today's notes") work great.

### Voice messages are the fastest input

Telegram supports voice messages. Combined with Telegram's speech-to-text, this is dramatically faster than thumb-typing a complex request. You speak your request, the transcription goes to Claude, Claude executes and replies with a concise text summary.

### Set up reaction-based approvals

Instead of typing "yes, go ahead," you can react to Claude's plan message with a thumbs-up emoji. Configure your CLAUDE.md to recognize reactions as approvals. It's a one-tap approval flow that feels native to mobile.

### Keep a "mobile tasks" skill

I maintain a skill specifically for tasks I commonly trigger from mobile: quick status checks, note creation, simple file edits. These are pre-tested, fast, and produce mobile-friendly output. Anything that needs desktop attention gets queued for later.

### Don't fight the medium

If you catch yourself typing a third paragraph on your phone, stop. That's a desktop task. Send yourself a quick note ("TODO: refactor the auth module, the token refresh logic is wrong") and handle it when you're at your computer. Mobile is for quick interactions, not deep work.

## Architecture Diagram

```
+------------------+          +-------------------+
|  Your Phone      |          |  Your Desktop     |
|                  |          |                   |
|  Telegram App    |  cloud   |  Claude Code      |
|  (send message) --------->  |  (reads message)  |
|                  |          |  (executes tools)  |
|  (see reply)   <---------  |  (sends reply)    |
|                  |          |                   |
+------------------+          +-------------------+
                                      |
                              +-------v-------+
                              |  Your Files   |
                              |  Git repos    |
                              |  Scripts      |
                              +---------------+
```

The key insight: your phone never touches your files. Claude does all the work locally on your machine. Telegram is just a message transport layer. This means you get the full power of Claude Code (filesystem access, git, shell commands) from a device that only needs a chat app.
