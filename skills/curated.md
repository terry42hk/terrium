# Curated Skills

Skills I've installed, tested, and can recommend. Each entry includes what I actually think after using it — not just the README pitch.

## Skill Collections

### [lijigang/ljg-skills](https://github.com/lijigang/ljg-skills)

A thoughtfully designed set of learning and writing skills. The "ljg" prefix stands for the author 李继刚.

| Skill | What it does | My take |
|-------|-------------|---------|
| **ljg-card** | Transforms content into visual PNG cards (6 formats: long card, infograph, multi-card, sketchnote, comic, whiteboard) | The most creative skill I've seen — turns any note into a shareable visual. The multi-card format is great for breaking down complex topics. |
| **ljg-learn** | Deconstructs any concept through 8 philosophical dimensions, compresses into an epiphany | Genuinely useful for deep learning. Forces you to examine a concept from angles you wouldn't naturally consider. |
| **ljg-writes** | "Think by writing" engine — start with a thesis, develop it through structured long-form writing | Opinionated in a good way. The constraint of starting with one thesis prevents the rambling that AI writing usually produces. |
| **ljg-roundtable** | Structured debate — a moderator invites representative figures for multi-perspective discussion | Fun and surprisingly insightful. Best used when you're stuck between options and want to stress-test your thinking. |
| **ljg-word** | Deep English word mastery — deconstructs semantics and builds intuition | Great for bilingual learners who want to go beyond dictionary definitions. |

### [Anthropic Education Content Collection](https://docs.anthropic.com/en/docs/claude-code/skills)

Workflow patterns from Anthropic's documentation and community. Not a single repo — these emerged from best practices.

| Skill | What it does | My take |
|-------|-------------|---------|
| **systematic-debugging** | Structured debugging: reproduce → isolate → diagnose → fix. Forces you to understand before patching. | Prevents the "try random things until it works" anti-pattern. Worth installing just for the discipline it enforces. |
| **verification-before-completion** | Requires running verification commands and confirming output before claiming work is done. | "Evidence before assertions." Simple rule, massive impact. Catches the cases where Claude says "done!" but the test actually fails. |
| **writing-plans** | Converts specs/requirements into detailed implementation plans before touching code. | Best used for multi-file changes. The plan becomes a contract you can review before Claude starts editing. |
| **test-driven-development** | Write tests first, watch them fail, then implement. Classic TDD enforced as a workflow. | Works well for features with clear expected behavior. Less useful for exploratory/prototyping work. |

### [Humanizer-zh](https://github.com/nicekate/Humanizer-zh-main)

Adapted from [blader/humanizer](https://github.com/blader/humanizer) + [hardikpandya/stop-slop](https://github.com/hardikpandya/stop-slop).

| Skill | What it does | My take |
|-------|-------------|---------|
| **Humanizer-zh-main** | Detects and removes AI-generated patterns from Chinese text | Based on Wikipedia's AI writing signs database. Essential if you publish Chinese content — catches the telltale patterns (首先/其次 structures, 總結來說, unnecessary 值得注意的是) that scream "AI wrote this." |

### [pbakaus/impeccable](https://github.com/pbakaus/impeccable)

A 17-skill design suite for Claude Code by Paul Bakaus.

| Skill | What it does | My take |
|-------|-------------|---------|
| **impeccable** (suite) | critique, polish, audit, normalize, harden, delight, distill, adapt, animate, bolder, quieter, colorize, extract, onboard, optimize, clarify | Comprehensive UI/UX toolkit. The individual skills are well-scoped — `critique` for evaluation, `polish` for final pass, `harden` for edge cases. Best used as a design review pipeline rather than picking random skills. |
