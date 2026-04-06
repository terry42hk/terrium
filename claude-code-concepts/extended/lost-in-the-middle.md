# Lost in the Middle (2026) — largely solved, still matters for complex reasoning

The 2023 "Lost in the Middle" problem — where LLMs pay less attention to information in the middle of their context — has been largely solved for simple tasks. But for complex multi-hop reasoning at 256K+ tokens, positional effects remain measurable.

## The Original Problem (2023)

Liu et al. discovered a U-shaped attention curve in LLMs: strong attention at the **beginning** (primacy) and **end** (recency) of context, weak attention in the middle. Information placed mid-context was frequently missed.

## What Changed (2024-2026)

Frontier models addressed this through:
- **Long-context training data** with information at varying positions
- **Improved positional encodings** (RoPE/YaRN)
- **Multi-scale attention** — local attention in lower layers, global in higher layers
- **Curriculum training** on increasing context lengths

## Current Benchmark Results (2026)

| Task type | Position sensitivity |
|-----------|---------------------|
| Single-fact retrieval (NIAH) | Essentially none — >99% accuracy at all positions |
| Multi-fact retrieval | Mild degradation |
| Reasoning over scattered facts | Moderate degradation at 256K+ tokens |
| Following mid-context instructions | Minor but measurable |

## The Verdict

> **Lost in the Middle in 2026 is not a "bug" but a "physical property" — like gravity. It won't make you fall, but you still account for it when designing buildings.**

The positional bias is no longer the primary concern. But **signal-to-noise ratio** is permanent. Even if the model "sees" everything in its context equally well, too much irrelevant material dilutes attention on what matters.

## Practical Implications

**What you no longer need to worry about:**
- Strategically placing important information at the start/end of prompts
- Reordering context to game position effects

**What still matters (and always will):**
- Keeping context lean — remove irrelevant material
- Using subagents to isolate research from main context
- Compacting when signal-to-noise degrades
- Storing critical rules in CLAUDE.md (primacy position is still a bonus, not a necessity)

## The Reframing

```
2023: "Put important stuff at the start because the model can't see the middle"
2026: "Keep context clean because noise hurts performance regardless of position"
```

The advice changed from **positional strategy** to **hygiene discipline**. The latter is actually harder because there's no simple trick — it requires ongoing judgment about what belongs in context and what doesn't.
