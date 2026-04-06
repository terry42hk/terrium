# 14 Rules That Govern Every Session

CLAUDE.md is loaded every turn, survives context compression, and sits at the primacy position in the context window. It is the highest-leverage configuration file you have. Most people either leave it empty (generic assistant) or fill it with 500 lines of project documentation (diluted attention). The sweet spot is a tight set of rules that encode your hard-won opinions about how Claude should behave.

These are the 14 rules I run in my global CLAUDE.md. They took months to stabilize — each one exists because I hit a real failure mode without it.

---

## Pre-Task (Rules 1-4)

These fire before Claude writes a single line. They prevent the most expensive mistake: solving the wrong problem.

### Rule 1: Why before how

> First understand the problem. For non-trivial tasks: ask why (5 Whys to find the root need), then plan how, then execute. Don't solve the wrong problem.

**Why this exists:** Claude is eager to help. Give it a task and it immediately starts building. I've had it refactor entire modules before I realized the actual problem was a config typo. Forcing "why before how" catches this early.

**Example:** You say "refactor the auth module to use JWT." Claude should first ask: what's wrong with the current auth? If the answer is "tokens expire too fast," the fix is a config change, not a rewrite.

### Rule 2: Check for existing rules/conventions

> Read relevant existing files (e.g., existing notes in the same folder) to understand formatting patterns before creating new content.

**Why this exists:** Without this, Claude creates files that look nothing like their neighbors. Different heading styles, different frontmatter, different naming conventions. Then you spend time manually harmonizing.

**Example:** You ask Claude to create a new note in a folder where every file has YAML frontmatter with `tags` and `date`. Without this rule, it'll create a bare markdown file. With the rule, it reads a sibling file first and matches the pattern.

### Rule 3: Check installed skills first

> Search `.agent/skills/` for a matching skill BEFORE writing anything from scratch.

**Why this exists:** I've built skills for common workflows — translation, documentation, research. Without this rule, Claude ignores them and writes one-off scripts that are worse than the tested skill.

**Example:** You ask Claude to translate a document. There's a translation skill with tone-matching, glossary support, and quality checks. Without this rule, Claude just runs a naive prompt. With it, Claude loads the skill and follows its workflow.

### Rule 4: Use skills over manual work

> If a skill exists for the task, follow its workflow. Don't reinvent.

**Why this exists:** Even after finding a skill (Rule 3), Claude sometimes reads it and then goes off-script. This rule reinforces: if the skill says "step 1, step 2, step 3," follow steps 1-2-3.

**Example:** A writing skill says to brainstorm before drafting. Claude finds the skill but skips straight to drafting because it's "confident." The output is generic. The brainstorming step exists precisely to prevent generic output.

---

## Code Changes (Rules 5-7)

These govern the edit-verify loop. They prevent the second most expensive mistake: shipping broken code with confidence.

### Rule 5: No verification ability, no change

> If you cannot verify a change is correct (no test, no expected output, no way to diff), build the verification first before making the change.

**Why this exists:** Claude will cheerfully make 15 changes in a row with no way to verify any of them. Then something breaks and you're binary-searching through an undo stack. Building the test first means every change is immediately checkable.

**Example:** You need to change a date parsing function. Before touching the function, write a test that captures current behavior. Then make the change. Then run the test. If it passes, you're done. If not, you know exactly what broke.

### Rule 6: One change, one verification

> Change one thing at a time. Verify its output is correct before making the next change. Never batch multiple features into one unverified pass.

**Why this exists:** Claude loves to batch. "While I'm in here, let me also fix X, Y, and Z." Then the test fails and you don't know which of the four changes caused it. Atomic changes keep the feedback loop tight.

**Example:** You ask Claude to add pagination AND sorting to an API endpoint. Without this rule, it does both at once. The endpoint returns wrong results. Was it the pagination logic? The sorting? The interaction between them? With the rule, it adds pagination first, verifies, then adds sorting.

### Rule 7: Verify output, not exit code

> "Script didn't crash" is not "Script is correct". Always check the actual output content against expected results.

**Why this exists:** Claude's default verification is "I ran it and there were no errors." That's necessary but wildly insufficient. A script can exit 0 while producing completely wrong output. This rule forces Claude to actually read what came out.

**Example:** A data migration script runs without errors. Claude says "Migration complete!" But the output file has 0 rows because the query filter was wrong. With this rule, Claude checks: expected ~10,000 rows, got 0 — something is wrong.

---

## Delivery (Rules 8-9)

These fire right before Claude presents results. They catch the things that slip through the edit-verify loop.

### Rule 8: Self-review before delivery

> Stop and critically review your own work before presenting it. Applies equally to subagents — verify their output before passing it on.

**Why this exists:** Claude has a completion bias. Once it finishes a task, it wants to deliver immediately. This pause catches obvious issues — typos in user-facing text, missing edge cases, files that weren't saved.

**Example:** Claude writes a function, tests pass, and it's about to present the result. The self-review catches that the function has a hardcoded timezone that should be configurable. Small catch, big impact.

### Rule 9: Never store credentials in settings files

> Use environment variables. No API keys, tokens, or passwords in any settings or config files.

**Why this exists:** Claude will happily write `"api_key": "sk-abc123"` into a JSON config file that gets committed to git. This rule is a hard prohibition. Every credential goes through environment variables.

**Example:** You ask Claude to configure a Telegram bot. Without this rule, it writes the bot token directly into `settings.json`. With this rule, it sets up `$TELEGRAM_BOT_TOKEN` in your shell profile and references the variable.

---

## Subagent Discipline (Rules 10-12)

These govern multi-agent workflows. Subagents are powerful but introduce coordination failure modes that don't exist in single-agent work.

### Rule 10: Synthesize before forwarding

> Subagent research results must be digested by the main agent before use. Write specific instructions with file paths and line numbers, never "based on your findings, implement it."

**Why this exists:** A research subagent returns a wall of findings. Without synthesis, you pass the raw dump to an implementation subagent. The implementation agent picks the wrong finding, misinterprets context, or gets overwhelmed. The main agent must read, filter, and write precise instructions.

**Example:** A research agent finds 3 possible approaches to implement caching. The main agent reads all three, decides approach #2 fits best, and tells the implementation agent: "Use Redis with TTL=300s. Add the client in `src/cache.ts`. Cache the response from the function at `src/api.ts:47`." Not: "Based on the research, add caching."

### Rule 11: Dispatch with a plan

> Before launching 2+ parallel subagents, list the division of work (who does what, expected output) and confirm the plan.

**Why this exists:** Without explicit planning, parallel agents duplicate work, miss gaps, or make conflicting changes. The plan forces you to think through the decomposition before burning tokens.

**Example:** You need to add a feature that touches the API, the frontend, and the tests. Before dispatching three agents, you write: "Agent 1: API endpoint in `src/routes/`. Agent 2: Frontend component in `src/components/`. Agent 3: Tests in `tests/`. No agent touches another's directory." Then you dispatch.

### Rule 12: No overlapping file scope

> Parallel subagents must not modify the same set of files. Assign non-overlapping file boundaries before dispatch.

**Why this exists:** Two agents editing the same file create merge conflicts at best and silent overwrites at worst. Claude has no built-in conflict resolution for parallel edits. The only safe approach is prevention.

**Example:** Agent A is told to update `config.ts` with new settings. Agent B is told to refactor `config.ts` for readability. Agent B finishes last and overwrites Agent A's changes. With non-overlapping scope, this can't happen.

---

## Context Hygiene (Rules 13-14)

These manage the context window — the finite resource that degrades everything when exhausted.

### Rule 13: Use subagents for bulk exploration

> When a task requires reading 5+ files, delegate to an Explore subagent instead of reading everything into main context.

**Why this exists:** Reading 10 files into the main context burns ~30% of your window on exploration that may only yield 2 relevant facts. A subagent reads them in its own context and returns a concise summary. Main context stays clean.

**Example:** You need to understand how error handling works across the codebase. Instead of reading 15 files into the main session, dispatch a subagent: "Read all files in `src/errors/` and `src/middleware/`. Summarize the error handling patterns, naming conventions, and any inconsistencies." The summary comes back as 20 lines instead of 500.

### Rule 14: Suggest reset for topic changes

> When the user starts a clearly different task, suggest /compact or /clear to keep signal-to-noise ratio high.

**Why this exists:** After debugging an auth issue for 30 minutes, the context is full of stack traces, failed attempts, and red herrings. If you then say "now build a dashboard," all that auth noise is still in context, competing for attention and degrading output quality.

**Example:** You've been working on database migrations. You say "now let's work on the email templates." Claude suggests: "This is a different topic — I'd recommend `/compact` to compress the migration context before we start on email templates."

---

## How to Write Your Own Rules

Start with this golden test for every rule:

> **"If I delete this rule, will Claude make mistakes?"**
> Yes = keep it. No = delete it.

The sweet spot is **10-20 rules**. Here's why:

- **0 rules** = Claude is a generic assistant. It doesn't know your standards.
- **10-20 rules** = Claude follows your specific workflow. Each rule gets attention.
- **100+ rules** = Diluted attention. Claude tries to follow all of them, follows none of them well.

Rules should encode **decisions and prohibitions**, not describe code structure. Claude can read your code — it doesn't need you to explain what `src/utils.ts` does. But it can't infer that you want one-change-one-verify, or that credentials must never be in config files. Those are judgment calls that live in rules.

**The practical workflow:** Start with the failures. Every time Claude does something you have to undo, ask: "Could a rule have prevented this?" If yes, write the rule. If no, it was a one-off — just correct it in conversation and move on.
