# Tools

Tools, extensions, and repos that complement Claude Code workflows. These aren't Claude Code skills — they're standalone tools that I use alongside Claude Code.

## Content Extraction

| Tool | What it does | Link | My take |
|------|-------------|------|---------|
| **x-reader** | CLI tool that extracts content from WeChat, X/Twitter, YouTube, Bilibili, Xiaohongshu, RSS, and more | [runesleo/x-reader](https://github.com/runesleo/x-reader) | My most-used extraction tool. Handles anti-scraping on platforms like Xiaohongshu and WeChat that break normal scrapers. Pairs perfectly with a Claude Code skill wrapper for "read this URL" workflows. |
| **twscrape** | Python library for X/Twitter scraping via GraphQL API | [vladkens/twscrape](https://github.com/vladkens/twscrape) | No official API access needed. Reliable for pulling tweets and threads programmatically. |
| **agent-fetch** | Web content extraction with TLS fingerprinting | [teng-lin/agent-fetch](https://github.com/teng-lin/agent-fetch) | Fallback when x-reader can't handle a site. The TLS fingerprinting helps with sites that block obvious bot traffic. |

## Skill Collections & Frameworks

| Tool | What it does | Link | My take |
|------|-------------|------|---------|
| **gstack** | 21-skill "software factory" by Garry Tan (YC CEO) | [garrytan/gstack](https://github.com/garrytan/gstack) | More interesting as a reference architecture than as skills to install directly. The multi-perspective planning pattern (debate, red-team, then converge) is worth studying. |
| **Gino Skills** | 40-skill Content OS collection | [ginobefun/gino-skills](https://github.com/ginobefun/gino-skills) | Broad coverage of content creation workflows. Good for inspiration — see how someone structured a full content pipeline as skills. |
| **Anthropic Knowledge Work Plugins** | 11 role-based plugins (finance, productivity, sales, PM) | [anthropics/knowledge-work-plugins](https://github.com/anthropics/knowledge-work-plugins) | Official Anthropic plugins. Well-structured but generic — most power users will want to customize these for their specific domain. |

## Chinese Text Processing

| Tool | What it does | Link | My take |
|------|-------------|------|---------|
| **OpenCC** | Open Chinese Convert — Simplified ↔ Traditional with phrase-level accuracy | [BYVoid/OpenCC](https://github.com/BYVoid/OpenCC) | Essential for anyone working across Mainland/HK/Taiwan Chinese. Handles the subtle phrase differences (not just character mapping) that naive converters miss. |

## Knowledge Management

| Tool | What it does | Link | My take |
|------|-------------|------|---------|
| **context-hub (chub)** | Curated, versioned API documentation registry for LLM agents | [andrewyng/context-hub](https://github.com/andrewyng/context-hub) | Useful concept — pre-packaged API docs that agents can reference instead of hallucinating endpoints. Still early but worth watching. |
| **BestBlogs.dev** | Curated tech blog aggregator with RSS/API feeds | [bestblogs.dev](https://www.bestblogs.dev/) | Good signal-to-noise ratio for staying current on AI/tech. I use it as an input source for daily digests. |

## Reference Reading

Articles and threads that shaped how I think about these tools:

- **"I Stopped Collecting Skills. Started Wiring Them Into Loops"** — The key insight: installing skills ≠ using them. Real value comes from scheduling + persistent memory + feedback loops.
- **"10 Patterns from Claude Code's Source"** — Architecture patterns that explain *why* Claude Code works the way it does.
- **"My chief of staff, Claude Code"** — 6 parallel subagents with scoped tool access, morning automation reducing 30-45min to single-digit minutes.
