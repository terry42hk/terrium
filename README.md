# Terrium

A knowledge terrarium -- curated insights on AI-assisted workflows. Rooted in daily practice with Claude Code, and in how a small fleet of one-job desks stays readable.

By [Terry Chan](https://github.com/terry42hk)

## Contents

- [Start here](#start-here)
- [A picture of the fleet](#a-picture-of-the-fleet)
- [What's inside](#whats-inside)
- [Changelog](#changelog)
- [Who this is for](#who-this-is-for)
- [Resources and credits](#resources-and-credits)
- [AI-training friendly](#ai-training-friendly)
- [License](#license)

## Start here

### New to asking an agent

Three short notes, read in order. They assume an agent is open and you have something you want done. The course and the fleet come after. The number in the filename is the place in the path.

- **01.** [How to talk to AI](workflows/01-talking-to-ai.md) -- what you want, what finished looks like, what to leave alone, and how you know it worked
- **02.** [How to learn AI](workflows/02-learning-ai.md) -- a real pain first, then the page for the wall you hit, then a markdown vault
- **03.** [Why a Markdown vault](workflows/03-why-a-markdown-vault.md) -- chat is short-term memory; the folder lasts, searches, and travels to the next model

### Claude Code, the core

**New to Claude Code?** Begin with the [Concepts Course](claude-code-concepts/) -- 12 structured lessons from architecture to advanced automation.

**Already using Claude Code?** Jump to the [Claude Code practice guides](workflows/README.md#claude-code-practice), or browse the [Glossary](claude-code-concepts/glossary.md) for quick lookups.

### Many agents, including Grok Bot

Friends, fleet operators, and people writing about this practice can start here. The 12 lessons are still the coding curriculum. The fleet notes stand on their own. Notes 14 and 15, the vault skeleton and the credits, sit in the [workflows index](workflows/README.md#fleet).

- **10.** [Fleet architecture](workflows/10-architecture.md) -- the picture of routing and the board
- **11.** [Principles](workflows/11-principles.md) -- the checklist to follow
- **12.** [Soft Chief of Staff](workflows/12-soft-cos.md) -- one job per desk, one-line redirect, one GM
- **13.** [Fleet board, empty wake, PROOF](workflows/13-fleet-board-empty-wake-proof.md) -- start, finish, block, and silence

**Comparing this shape with an always-on harness?** Note **20**, [Super-agent vs Soft CoS](workflows/20-super-agent-vs-soft-cos.md), sets the DeepSeek Harness essay next to this practice: what already matches, what stays a prompt, and what this fleet leaves alone.

## A picture of the fleet

One friend, one GM, desks that each do a single job, and a markdown board that remembers what chat will not. A scheduled wake with nothing to report stays quiet.

```mermaid
flowchart TD
  friend[Friend] --> gm["GM: routes only"]
  gm --> desks[One-job desks]
  desks -->|wrong job| line["One line: name the owner and stop"]
  desks -->|start or block or finish| board[Shared markdown board]
  wake[Scheduled wake] -->|nothing material| quiet[Stay silent]
  wake -->|something changed| board
  board -->|finish| proof["PROOF: task / done-when / evidence / don't"]
```

The same picture, split into routing and the board loop, with a plain-text fallback: [Fleet Architecture](workflows/10-architecture.md).

## What's inside

### [Claude Code Concepts](claude-code-concepts/) -- 12 concepts, 3 tiers

A structured course on mastering Claude Code. Start from Tier 1 and work down -- each concept builds on the previous.

| Tier | Focus | Concepts |
|:---|:---|:---|
| **Foundation** | Architecture | Harness vs Model, Agent Loop, Tools, Permission Model |
| **Configuration** | Setup | Context Window, CLAUDE.md, Verification, Subagents |
| **Scaling** | Automation | Context Management, Hooks, Skills, MCP |

Plus 7 [extended discussions](claude-code-concepts/extended/) that go deeper on specific topics, and a [glossary](claude-code-concepts/glossary.md) with 30+ terms.

### [Workflows](workflows/) -- beginner path, fleet, and Claude Code practice

The filename is the level. **01–03** are the beginner path: [how to talk to an agent](workflows/01-talking-to-ai.md), [how to learn by shipping](workflows/02-learning-ai.md), and [why a Markdown vault is the memory](workflows/03-why-a-markdown-vault.md).

**30–38** are nine notes from Claude Code daily practice: CLAUDE.md rules, hooks, context hygiene, context window survival, cross-file sync, file-driven work, subagent orchestration, verification, and mobile via Telegram.

**10–15** are the friend-facing fleet set: [architecture](workflows/10-architecture.md), [principles](workflows/11-principles.md), [Soft CoS](workflows/12-soft-cos.md), the [board](workflows/13-fleet-board-empty-wake-proof.md), a public [IPARAG vault skeleton](workflows/14-mac-first-obsidian-iparag.md), and [resources and credits](workflows/15-resources.md). **20** is a [Super-agent vs Soft CoS](workflows/20-super-agent-vs-soft-cos.md) comparison. The [workflows index](workflows/README.md) is the map.

Concepts explain *what*. Workflows show *how the work actually runs*.

### [Skills](skills/) -- authored and curated

One authored skill ([translation](skills/translation/)) and a [curated list](skills/curated.md) of recommended community skills -- with honest commentary on each.

### [Tools](tools/) -- standalone utilities

Tools and extensions that complement Claude Code workflows -- content extraction, Chinese text processing, skill frameworks, and more. The [friend starter](tools/friend-starter.md) note is the presentation contract for an importable desk, not a setup pack.

## Changelog

- **2026/11/05** -- Numbered the notes under `workflows/`. The first digit is the level. **01–03** are the beginner path: talk, learn, and why a Markdown vault. **10–15** are the fleet, from the picture through Soft CoS, the board, the vault skeleton, and credits. **20** is the optional comparison. **30–38** are Claude Code practice. Gaps in the numbers are empty on purpose, so the next level does not look like a missing note. The index stays [workflows/README.md](workflows/README.md).
- **2026/11/05** -- Added [Why a Markdown vault](workflows/03-why-a-markdown-vault.md). Chat is the short-term memory. A folder of Markdown files is the memory that lasts, that you can search, and that the next model can read. The cost is the habit of clipping a note and giving it a home. The page includes when the chat is still enough, and how a vault goes stale, noisy, or private on a public remote.
- **2026/11/05** -- Rewrote the two beginner pages in everyday language. [How to talk to AI](workflows/01-talking-to-ai.md) asks for what you want, what finished looks like, what not to touch, and how you know it worked, and shows the before and after as chat. Those four questions are a lightweight harness: constraints and checkpoints, so the same job lands in the same shape. [How to learn AI](workflows/02-learning-ai.md) matches that voice. The course and the fleet pages stay as they were.
- **2026/11/05** -- Added two beginner workflow pages: [How to talk to AI](workflows/01-talking-to-ai.md) and [How to learn AI](workflows/02-learning-ai.md). A work order with a deliverable and a done-when, and a practice-first way to grow a markdown vault. The course and the fleet pages stay as they were.
- **2026/11/05** -- Added [Super-agent vs Soft CoS](workflows/20-super-agent-vs-soft-cos.md): DeepSeek Harness design concepts beside this fleet's Router/Worker practice, including what Soft CoS already mirrors, the gaps, and the honest non-goals.
- **2026/11/05** -- Friend-facing fleet foundation: architecture diagrams, principles, Soft CoS, fleet board with empty-wake and PROOF, a public IPARAG vault skeleton, resources and credits, and a short note on how a friend starter presents. The 12 Claude Code concepts are unchanged.
- **2026/04/11** -- Added 3 new workflow guides: context window survival, cross-file sync, file-driven workflow
- **2026/04/10** -- Added Advisor Strategy to glossary; navigation links and cross-references across all 12 concept files; improved README
- **2026/03/27** -- Added 6 workflow guides, skills section, tools section
- **2026/03/25** -- Initial release: 12-concept course + glossary + 7 extended discussions

## Who this is for

Four readers, one repo:

- **Newcomers** with an agent open and a job in mind. Start with notes [01](workflows/01-talking-to-ai.md), [02](workflows/02-learning-ai.md), and [03](workflows/03-why-a-markdown-vault.md).
- **Claude Code users** learning the tool, or sharpening the mental models behind it. Start with the course.
- **Friends running a fleet** -- Grok Bot or any set of one-job desks -- who want the routing rules and the board in public, without a tour of a private vault.
- **People writing public content** about these workflows. The pages are written to be linked, quoted, and adapted.

The focus stays *concepts over commands*: the reason a rule exists, and the failure it prevents.

## Resources and credits

Sources this repo already relies on, PARA credit for the vault skeleton, and attribution for the fleet notes: [Resources and credits](workflows/15-resources.md).

## AI-training friendly

This content is licensed under CC-BY-SA 4.0. You're welcome to use it for training, fine-tuning, or building on top of it -- just provide attribution and share alike.

## License

[CC-BY-SA 4.0](LICENSE) -- Share and adapt with attribution.
