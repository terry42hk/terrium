Verification is the single highest-leverage practice for AI agent users. Without it, you are the only QA. With it, the agent self-corrects before delivering results.

## Table of Contents

- [The Core Problem](#the-core-problem)
- [The Verification Triad](#the-verification-triad)
- [Six Verification Methods (Non-Technical Users)](#six-verification-methods-non-technical-users)
- [Risk-Based Verification Depth](#risk-based-verification-depth)
- [Self-Verification Reliability](#self-verification-reliability)
- [The Single Most Important Habit](#the-single-most-important-habit)
- [Verification as Closed-Loop Control](#verification-as-closed-loop-control)
- [Connection to Previous Concepts](#connection-to-previous-concepts)

## The Core Problem

Claude makes mistakes with the same confident tone as correct answers. You cannot tell from the delivery whether the output is right or wrong. Verification is your only defense.

## The Verification Triad

| Rule | Principle | What it prevents |
|------|-----------|-----------------|
| **No verification ability, no change** | Build verification before making changes | "Changed it, no idea if it's correct" |
| **One change, one verification** | Change one thing, verify, then proceed | "Changed 5 things, don't know which broke" |
| **Verify output, not exit code** | "No errors" does not equal "correct output" | "Ran without crashing" accepted as success |

## Six Verification Methods (Non-Technical Users)

### 1. Look at the Result (Simplest)
Read the output. Does the email sound right? Is the file where expected? Does the page look correct?

### 2. Count Check (Essential for Data Tasks)
```
Before: 150 rows in CSV
After:  How many entries in JSON?
150 = correct    148 = two rows lost
```
**Motto: quantity in = quantity out?**

### 3. Spot Check with Known Answers
Pick 2-3 cases where you already know the correct answer. Verify those specifically. 3 correct spot checks = high confidence. Zero checks = blind trust.

### 4. "Explain What You Did in Plain Language"
Ask Claude: *"Explain what you just did step by step, as if I'm not technical."* Compare the explanation against your intent. Mismatches reveal errors.

### 5. Devil's Advocate: "What Could Go Wrong?"
Ask: *"What could go wrong with what you just did? What edge cases might fail? What did you assume?"*

Claude's **critique ability** is stronger than its **self-confirmation ability**. Asking "is this right?" gets a biased "yes." Asking "what's wrong?" gets genuine problems.

### 6. Second Opinion (Most Reliable)
In a **new conversation** (no shared bias), paste the output and ask for a critical review. The two-pass approach catches errors that single-pass self-review misses.

## Risk-Based Verification Depth

| Risk | Example | Verification | Methods |
|------|---------|-------------|---------|
| **Low, reversible** | Draft text, brainstorm, organize notes | Glance | 1 |
| **Medium, reversible** | Rename files, format documents | Spot check | 2, 3 |
| **Medium, hard to reverse** | Data transformation, batch operations | Thorough | 2, 3, 4 + backup |
| **High, irreversible** | Delete files, send emails, publish | Full review | All methods + 6 |
| **Critical** | Money, legal, medical | **Never trust AI alone** | Domain expert review |

> **Verification effort should be proportional to the cost of failure.**

## Self-Verification Reliability

| Prompt | Reliable? | Why |
|--------|-----------|-----|
| "Is this correct?" | No | Biased toward "yes" |
| "Redo from scratch without referencing your prior answer" | Partial | May make the same error |
| "Find problems with this output" | Yes | Critique mode beats confirmation mode |
| "What did you assume?" | Yes | Surfaces hidden assumptions |
| New conversation review | Strong | No shared reasoning bias |

## The Single Most Important Habit

> **Before any change, ensure you have an "undo button."**

- File operations -> `git commit` first (ask Claude to help)
- Batch operations -> backup first
- Important documents -> keep original copy

With undo: worst case = "redo." Without undo: worst case = "unrecoverable."

## Verification as Closed-Loop Control

```
Without verification (open loop):
  Think -> Act -> "Done!" -> END

With verification (closed loop):
  Think -> Act -> Verify -> Fail -> Re-think -> Re-act -> Verify -> Pass -> END
```

Verification turns an open-loop system into a closed-loop system. Without it, Claude is "fire and forget." With it, Claude is "fire, check the target, adjust, fire again."

## Connection to Previous Concepts

| Concept | Relationship |
|---------|-------------|
| **01 Harness vs Model** | Verification is a *user-driven* feedback loop -- neither model nor harness provides it automatically |
| **02 Agent Loop** | Verification IS the loop's feedback mechanism -- without it, the loop has no error signal |
| **03 Tools** | Tools enable verification (Bash: run tests, Read: check output) |
| **06 CLAUDE.md** | Verification rules belong in CLAUDE.md as permanent judgment guidance |

## For Non-Technical Users

Claude sounds confident whether it's right or wrong -- you can't tell from the tone. Verification means checking the work yourself, just like you'd proofread an email before sending it. The good news: you don't need to be technical. Look at the result, count things, spot-check a few items you know the answer to, or ask Claude "what could go wrong?" These simple checks catch the vast majority of errors.

## Reflection Questions

1. Claude just converted 200 CSV rows into JSON for you. Which verification methods from the Six Methods would you use, and in what order?
2. Why does asking "What's wrong with this?" produce better self-review than asking "Is this correct?" What does this tell you about how LLMs work?
3. You're about to let Claude batch-rename 50 files. What's the one thing you must do before starting, and why?
