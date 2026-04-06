# Writer-Reviewer session separation — the most effective self-audit

The single most effective way to catch errors in AI-generated work is to review it in a fresh session. A reviewer with zero shared context has zero shared bias.

## Why Same-Session Review Fails

When Claude creates something and then reviews it in the same conversation, it has access to the entire reasoning chain that produced the output. This creates **anchoring bias** — the model is biased toward confirming its own work because it "remembers" why it made each decision.

| Review type | Reliability | Why |
|------------|------------|-----|
| Same-session "Is this correct?" | Low | Biased toward "yes" |
| Same-session "Find problems" | Medium | Better, but still anchored to original reasoning |
| **Fresh session review** | **High** | No shared reasoning — approaches work as a stranger |

## The Method

```
Session A (Writer):
  → Claude creates the content / code / analysis
  → Save the output

/clear or start a new session

Session B (Reviewer):
  → Paste the output
  → "Review this critically. Find errors, gaps, and assumptions."
  → No context about how it was created
```

The key insight: the **absence of creation context** is the feature, not a limitation. The reviewer doesn't know what shortcuts were taken, what alternatives were considered, or what the creator was "trying" to do — it evaluates the output purely on its merits.

## Why This Is Ranked #1

Among quality improvement methods, Writer/Reviewer separation ranks highest because:

1. **Zero implementation cost** — just `/clear` and paste
2. **Works for any output type** — code, prose, data, analysis
3. **Catches the hardest-to-find errors** — logical gaps and unstated assumptions that same-session review misses
4. **Officially recommended** by Anthropic's own best practices

## When to Use It

- **Always use** for high-stakes deliverables (client-facing docs, published content, production code)
- **Consider using** for medium-stakes work (internal reports, data transformations)
- **Skip** for low-stakes, easily reversible tasks (brainstorming, quick notes)

## Variations

| Variation | How | Best for |
|-----------|-----|----------|
| Manual `/clear` + paste | Simplest | Quick review |
| Custom reviewer subagent | Define a reviewer agent with specific standards | Consistent standards, persistent memory |
| Agent Team debate | Multiple agents with opposing mandates | Deep research, important decisions |

## The Principle Behind It

This method works because it exploits a fundamental property of LLMs: **context determines behavior**. An LLM with creation context behaves as a defender of its work. An LLM without creation context behaves as a neutral evaluator. Same model, different context, dramatically different quality of review.
