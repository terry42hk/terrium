# Verification — The Highest Leverage Habit

This is the single biggest differentiator between productive Claude Code users and frustrated ones. I've watched people lose hours to bugs that a 30-second verification step would have caught. Three rules form the verification triad:

- **Rule 5:** No verification ability, no change — if you can't verify it's correct, build the verification first
- **Rule 6:** One change, one verification — change one thing, verify, then proceed
- **Rule 7:** Verify output, not exit code — "Script didn't crash" does not mean "Script is correct"

---

## Why This Matters

Claude is confident. It will tell you "Done! All tests pass!" with the same tone whether the tests actually passed or it hallucinated the output. It will refactor your code and say "I've preserved all existing behavior" when it hasn't.

This isn't a flaw — it's the nature of the tool. Verification is how you turn a probabilistic system into a reliable one.

---

## Six Verification Methods

Ranked by practicality. Start at the top and go deeper based on risk.

| # | Method | When to Use | Effort |
|---|--------|-------------|--------|
| 1 | **Look at the result** | Always. Read the actual output. | Low |
| 2 | **Count check** | Data transformations. Input count should match output count. | Low |
| 3 | **Spot check with known answers** | When you know what 3 specific items should look like. | Medium |
| 4 | **"Explain what you did"** | After complex changes. Ask Claude to walk through step by step. | Medium |
| 5 | **Devil's advocate** | Before committing. "Find three problems with this." | Medium |
| 6 | **Fresh session review** | High-stakes deliverables. Start new session, paste output, review. | High |

### Method 1: Look at the Result

This sounds obvious but it's the most commonly skipped step. When Claude says "I've updated the config file," don't just nod — read the file. When it says "here's the refactored function," actually trace through the logic.

The number of times I've caught bugs just by reading the output is embarrassing. Not because the bugs are subtle — because they're obvious once you look.

### Method 2: Count Check

Simple but powerful for data work. If you asked Claude to transform 150 records, count the output. If you asked it to extract all functions from a file that has 23 functions, count the results.

Mismatches reveal:
- Silent filtering (items dropped without explanation)
- Duplication (items counted twice)
- Off-by-one errors (missed the first or last item)

### Method 3: Spot Check with Known Answers

Pick 3 items where you already know the correct answer. Check those specifically.

Example: You ask Claude to convert 200 prices from USD to EUR.
- You know item #1 should be ~92 EUR (it was $100 USD)
- You know item #47 should be ~0.92 EUR (it was $1 USD)
- You know item #200 should be ~4,600 EUR (it was $5,000 USD)

If all three check out, you have reasonable confidence in the batch. If any fail, you know the whole batch is suspect.

### Method 4: "Explain What You Did"

After a complex change, ask Claude to walk through it step by step. This surfaces:
- Steps it thought it did but actually skipped
- Assumptions it made silently
- Logic errors it can catch on reflection

This works because explaining forces re-evaluation. Claude often catches its own mistakes when forced to articulate them.

### Method 5: Devil's Advocate

Before committing, ask: **"Find three problems with this."**

Not "is this correct?" (biased toward yes). Not "review this" (too vague). Specifically ask for problems. Claude is surprisingly good at critiquing its own work when you frame it as criticism rather than confirmation.

Good prompts:
- "What could go wrong with this implementation?"
- "What edge cases did I miss?"
- "If a senior engineer reviewed this, what would they flag?"

### Method 6: Fresh Session Review

The nuclear option for high-stakes work. Start a completely new session (`/clear`), paste the output, and review it cold — without the context of how it was created.

Why this works: In the original session, Claude has access to its entire reasoning chain. This creates anchoring bias — it's biased toward confirming its own work. A fresh session has none of that context and evaluates the output on its own merits.

This is the **Writer-Reviewer separation** pattern. More on this below.

---

## Risk-Based Depth

Not everything needs the same level of verification. Match effort to stakes:

**Low risk, reversible** (draft text, exploratory code)
- Glance at output. Move on.

**Medium risk, reversible** (rename files, refactor code with tests)
- Spot check. Run existing tests.

**Medium risk, hard to reverse** (data transformations, database migrations)
- Thorough check. Count check. Backup first.

**High risk, irreversible** (delete files, send emails, publish content)
- Full review. Devil's advocate. Test with dry-run if possible.

**Critical** (financial transactions, legal documents, medical information)
- Never trust AI alone. Human expert review required. Period.

---

## The "Undo Button" Principle

Before ANY change, ensure you can reverse it.

- `git commit` before refactoring
- Copy the file before transforming
- Export before deleting
- Screenshot before redesigning
- Back up the database before migrating

This isn't paranoia — it's professionalism. The cost of a backup is seconds. The cost of an irreversible mistake is hours or worse.

**Practical workflow:**
```bash
# Before any significant change
git add -A && git commit -m "checkpoint before refactor"

# Now make the change
# ...

# If it went wrong
git diff          # see what changed
git checkout .    # revert everything
```

The `git commit` before starting is the cheapest insurance you'll ever buy.

---

## Writer-Reviewer Session Separation

This is the single most effective quality method I've found. Zero implementation cost. Works for any output type.

### Why Same-Session Review Fails

When you ask Claude to review its own work in the same session, it has access to its entire reasoning chain. It remembers *why* it made each decision. This creates anchoring bias — it's predisposed to confirm that its decisions were correct.

It's like asking a chef to taste-test their own food right after cooking it. They're primed to think it's good.

### The Fix

1. **Create** in Session A — write the code, generate the content, build the thing
2. **`/clear`** — wipe the context
3. **Review** in Session B — paste the output and evaluate it cold

Session B has no memory of Session A. No reasoning chain. No anchoring. It evaluates the work purely on its merits.

### The Right Review Prompts

Framing matters enormously:

| Prompt | Bias | Effectiveness |
|--------|------|---------------|
| "Is this correct?" | Biased toward yes | Low |
| "Review this" | Vague, usually surface-level | Low-Medium |
| "Find problems with this" | Biased toward finding issues | High |
| "What did you assume?" | Surfaces hidden assumptions | High |
| "How would this fail in production?" | Focuses on real-world failure modes | High |

The key insight: **"Is this correct?"** invites confirmation. **"Find problems"** invites genuine analysis. Use the latter.

### When to Use It

- Before shipping anything to production
- Before sending a deliverable to a client
- After any change you can't easily test
- When the stakes justify 2 minutes of extra effort

### When to Skip It

- Quick iterations on draft work
- Changes fully covered by automated tests
- Low-stakes exploratory work

---

## The Common Trap

Claude says: *"Done! All tests pass. The refactoring preserves all existing behavior."*

What actually happened: Claude ran the tests in its head, or ran them but didn't check the output carefully, or the tests pass but don't actually cover the changed code.

**Always:**
1. Ask to see the actual test output (not a summary)
2. Check that the tests actually test the changed code
3. Run the tests yourself if the stakes warrant it

Rule 7 in action: **verify output, not exit code.** A passing test suite means nothing if the tests don't cover what you changed. A successful script execution means nothing if the output file is empty.

---

## One Change, One Verification

Rule 6 exists because batch changes are the enemy of debugging.

**Bad workflow:**
1. Refactor the auth module
2. Add rate limiting
3. Update the API routes
4. Fix the tests
5. Run tests → 3 failures
6. Which change caused which failure? Good luck.

**Good workflow:**
1. Refactor the auth module → run tests → all pass
2. Add rate limiting → run tests → 1 failure → fix → all pass
3. Update the API routes → run tests → all pass
4. Done. Each change verified independently.

This is slower per step but dramatically faster overall. When something breaks, you know exactly which change caused it.

---

## Verification Checklist

Before claiming any task is complete:

- [ ] Did I look at the actual output (not just the success message)?
- [ ] If it's a data transformation, do the counts match?
- [ ] Can I reverse this change if it's wrong?
- [ ] Did I verify with an appropriate method for the risk level?
- [ ] If Claude said "all tests pass," did I check the actual test output?

Build this into muscle memory. It takes 30 seconds and saves hours.

---

## The Bottom Line

Verification isn't about distrusting Claude. It's about building a reliable workflow on top of a probabilistic system. The best Claude Code users verify relentlessly — not because Claude is bad, but because verification is what makes the whole system trustworthy.

Trust, but verify. Every time.
