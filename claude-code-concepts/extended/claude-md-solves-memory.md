# CLAUDE.md solves memory, not execution

CLAUDE.md is persistent and loaded every turn — but it is probabilistic, not deterministic. It solves the problem of "Claude forgetting what you told it," not the problem of "Claude must always do X."

## The Distinction

| Need | Example | Solution |
|------|---------|----------|
| "Claude should know my preferences" | "I prefer Chinese discussion, English docs" | CLAUDE.md (memory) |
| "Claude must never push to main" | Security boundary | Permission (execution) |
| "Claude must always lint after editing" | Guaranteed automation | Hook (execution) |

CLAUDE.md is **guidance** — the model reads it, understands it, and *usually* follows it. But "usually" is not "always." A model under pressure (long context, complex task, competing instructions) may deprioritize a CLAUDE.md rule.

## Why This Matters

The most common configuration mistake is putting deterministic rules in CLAUDE.md:

```
# CLAUDE.md (wrong place for this rule)
Never edit files in /production/
```

This works 95% of the time. The 5% failure is the one that costs you. The correct placement:

```json
// settings.json — Permission deny (100% reliable)
"permissions": {"deny": ["Edit:/production/**"]}
```

## The Decision Tree

```
Is this rule about what Claude should KNOW or what Claude must DO?

├─ KNOW (preferences, conventions, style, routing)
│   → CLAUDE.md ✅
│
└─ DO (must always / must never)
    ├─ "Must never" → Permission deny
    └─ "Must always" → Hook
```

## The Memory Analogy

Think of CLAUDE.md as an **employee handbook** — it tells Claude how things work here, what's expected, and what the team norms are. An employee who read the handbook will usually follow it. But if you need a rule that cannot be violated even by a well-meaning employee, you implement it as a **locked door** (Permission) or an **automated process** (Hook), not as a handbook entry.
