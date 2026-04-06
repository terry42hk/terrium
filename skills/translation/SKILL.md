---
name: translation
description: "High-quality translation between English and Chinese (Traditional/Simplified), with domain awareness and cultural adaptation. Use when user asks to translate between English and Chinese."
---

# Translation

> Natural, context-aware translation between English and Chinese.

## When to Use This Skill

- "Translate this to Chinese/English"
- "Help me write this in English" (when source is Chinese)
- "Translate this doc/email/spec"
- User pastes foreign-language content and asks for translation

## Workflow

### Step 1: Detect & Confirm

1. **Detect source language** from the input
2. **Infer target language**:
   - If source is English → target is Traditional Chinese (HK style) unless specified
   - If source is Chinese → target is English unless specified
   - If ambiguous, ask
3. **Detect domain** — technical, business, casual, legal, marketing
4. **Detect tone** — formal, conversational, academic, promotional

### Step 2: Translate

Apply these translation principles:

#### English → Chinese (Traditional, HK)
- Use Hong Kong Traditional Chinese (繁體中文), not Taiwan or Mainland conventions
- Technical terms: keep English for widely-used terms (API, Docker, React), translate conceptual terms
- Sentence structure: restructure for natural Chinese flow, don't mirror English syntax
- Cultural adaptation: localize idioms, don't translate literally
- Numbers/dates: keep Western format unless context requires Chinese format
- Brand names: keep original (don't translate "Claude", "Obsidian", etc.)

#### Chinese → English
- Produce natural, idiomatic English — not translationese
- Preserve the author's voice and emphasis
- Expand compressed Chinese expressions into clear English clauses
- Handle 四字成語/idioms by conveying meaning, not literal translation
- Technical content: use standard English terminology for the domain

### Step 3: Present Output

```markdown
## Translation

{translated text}

---

**Notes:**
- {Any translation choices worth explaining}
- {Terms kept in original language and why}
- {Cultural adaptations made}
```

### Step 4: Format Matching

- **If source is a document** → preserve all formatting (headers, lists, tables, code blocks)
- **If source is a message/email** → match the formality level
- **If source has mixed languages** → translate only the parts in the source language, keep the rest
- **If source is Markdown** → output valid Markdown with same structure

## Modes

### Quick Mode (default)
For short text (< 500 words): translate directly, minimal notes.

### Document Mode
For longer content or formal documents:
1. First pass: translate
2. Second pass: review for consistency (terminology, tone)
3. Present with translation notes

### Glossary Mode
When user provides or references a glossary:
1. Load the glossary (from file or user input)
2. Apply glossary terms consistently
3. Flag any terms not in the glossary that might need adding

## Rules

- **Natural over literal** — the output should read as if originally written in the target language
- **Preserve meaning, adapt expression** — same idea, different cultural packaging
- **Don't over-translate** — technical terms, brand names, and widely-understood English terms stay as-is in Chinese translation
- **HK Chinese by default** — use 繁體中文 with HK conventions (e.g., 軟件 not 軟體, 數據 not 資料 for data)
- **Ask when ambiguous** — if a sentence could mean two things, ask rather than guess
- **No AI-speak** — output should feel human-written, not machine-translated
- **Respect formatting** — if input is Markdown, output is Markdown. If input is plain text, output is plain text
