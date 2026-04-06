# Non-Tech verification — six practical methods without writing code

You don't need to write tests or read code to verify Claude's work. These six methods work for anyone — from project managers to content creators — and catch the majority of errors.

## The Six Methods

### 1. Look at the Result

The simplest and most overlooked method. Actually read the output, open the file, check the page. "Claude said it's done" is not verification — seeing the result with your own eyes is.

**Works for:** Documents, emails, file organization, visual outputs.

### 2. Count Check

```
Before: 150 rows in the spreadsheet
After:  How many entries in the output?
150 = correct    148 = two rows lost somewhere
```

Quantity in should equal quantity out. If Claude processed 20 files, count 20 results. If it merged 3 documents, check all 3 are represented.

**Works for:** Data transformations, batch operations, file processing.

### 3. Spot Check with Known Answers

Pick 2-3 cases where you already know the correct answer. Check those specifically. If Claude gets your known cases right, confidence in the unknown cases goes up significantly.

**Works for:** Translations (check phrases you know), calculations (verify a few by hand), data lookups (confirm against sources you trust).

### 4. "Explain What You Did"

Ask Claude: *"Explain what you just did step by step, as if I'm not technical."*

Compare the explanation against your intent. If Claude says "I deleted duplicate entries" but you wanted "I flagged duplicate entries," that's a catch.

**Works for:** Any task where intent alignment matters. Especially useful when you can't directly inspect the output.

### 5. Devil's Advocate

Ask: *"What could go wrong with what you just did? What edge cases might fail? What did you assume?"*

Claude's **critique ability is stronger than its self-confirmation ability**. Asking "is this right?" gets a biased "yes." Asking "what's wrong?" surfaces genuine problems.

**Works for:** Important decisions, complex operations, anything with edge cases.

### 6. Second Opinion (Most Reliable)

Open a **new conversation** (critical: no shared context bias), paste the output, and ask for a critical review.

Why new conversation? The reviewing Claude has no memory of creating the work — it approaches it with fresh eyes. Same-conversation review inherits the reasoning bias that produced the original output.

**Works for:** High-stakes content, important emails, anything where errors are costly.

## When to Use Which

| Risk level | What to verify | Methods |
|-----------|---------------|---------|
| Low (reversible) | Draft text, brainstorming | 1. Look |
| Medium (reversible) | File renaming, formatting | 2-3. Count + Spot check |
| Medium (hard to reverse) | Data transformation, batch ops | 2-3-4 + backup first |
| High (irreversible) | Send email, publish, delete | All methods + 6. Second opinion |
| Critical | Money, legal, medical | **Never trust AI alone** — domain expert review |

## The Golden Rule

> **Verification effort should be proportional to the cost of failure.**

A typo in a draft? Glance at it. A batch delete of 500 files? Full review with backup. Scale your effort to the stakes.
