# Cross-File Sync — Making Related Documents Discoverable

Claude Code doesn't maintain relational links between files. When you modify one document, related documents are not automatically updated. This is the most common pain point I hear from people using Claude Code with Obsidian or multi-file documentation projects.

The fix isn't a better prompt. It's better file structure.

---

## The Problem

Claude Code discovers files through two mechanisms:

1. **Explicit mention** — You name the file in the conversation
2. **Search** — Claude uses Grep or Glob to find files matching a pattern

Neither mechanism understands *relationships*. When you say "update the project timeline," Claude finds `timeline.md` but has no way to know that `budget.md`, `task-list.md`, and `meeting-notes.md` are related documents that may need corresponding updates.

**What this looks like in practice:**
- You say "update project status" → Claude modifies 1 file → you catch 3 missed files → remind Claude → it fixes one more → you check again → another miss
- You change a deadline in the spec → the task list still shows the old date → the meeting notes reference the wrong milestone

The AI isn't being lazy. It literally doesn't know those files exist or are related.

---

## The Solution: Three-Layer Discoverability

### Layer 1: YAML Frontmatter

Add metadata at the top of every document:

```yaml
---
created: 2026-04-10
tags:
  - project/pepsi-campaign
  - type/timeline
  - status/active
---
```

**How it helps:** Claude can now search for `project/pepsi-campaign` to find all files in a project.

```
# What Claude does internally:
Grep(pattern="project/pepsi-campaign", glob="*.md")
# Returns: timeline.md, task-list.md, budget.md, meeting-notes.md, moc.md
```

**Tag taxonomy I use:**

| Prefix | Purpose | Example |
|:---|:---|:---|
| `project/` | Project association | `project/pepsi-campaign` |
| `type/` | Document type | `type/timeline`, `type/spec`, `type/meeting-notes` |
| `status/` | Current state | `status/active`, `status/draft`, `status/archived` |

**Implementation time:** 2 minutes per existing file. For new files, bake it into your template.

### Layer 2: Wikilinks

Inside your documents, reference related files:

```markdown
Deployment moved to April 15 per [[Meeting Notes 0408]].
Budget impact tracked in [[Budget Tracker]].
```

When Claude reads a file and encounters a wikilink, it knows there's a related document worth checking. This creates a traversable web of connections.

Wikilinks are Obsidian syntax. For non-Obsidian setups, use relative markdown links: `[Meeting Notes](./meeting-notes-0408.md)`.

**Implementation time:** Add as you write naturally. No bulk migration needed.

### Layer 3: MOC (Map of Content)

For each project or domain, create a single index file:

```markdown
# Pepsi Campaign MOC

## Core Documents
- [[Task List]] — Deliverables and status
- [[Timeline]] — Key dates and milestones
- [[Budget Tracker]] — Spend tracking

## Meeting Notes
- [[Meeting Notes 0408]] — Client kickoff
- [[Meeting Notes 0415]] — Mid-campaign review
```

The MOC is the single file Claude needs to read to understand the full scope. Instead of relying on search (which may miss files with inconsistent tagging), the MOC provides an authoritative list.

**Implementation time:** 5 minutes per project. Update as new files are created.

---

## Wiring It Together

### CLAUDE.md Integration

Add a rule to your project's CLAUDE.md:

```markdown
When modifying any project document, first read the project's MOC
to identify all related files that may need updates.
```

This is guidance, not enforcement — Claude will follow it most of the time but may skip it under context pressure. For guaranteed cross-file behavior, use a hook (see [Hook Patterns](hooks-patterns.md)).

### The Updated Flow

```
User: "Update Pepsi project status"

Claude's workflow:
1. Search project/pepsi-campaign → find MOC
2. Read MOC → enumerate all project files
3. Read each relevant file → understand current state
4. Apply updates across all files that reference the changed data
```

**Without structure:** Modify 1 file. User catches missed updates manually.
**With structure:** Discover all related files through tags + MOC. Update in one pass.

---

## Implementation Checklist

- [ ] Add YAML frontmatter to all project documents (tags, created, modified)
- [ ] Adopt a consistent tag taxonomy (`project/`, `type/`, `status/`)
- [ ] Add wikilinks when referencing other documents
- [ ] Create one MOC per project or domain area
- [ ] Add a CLAUDE.md rule: "Read MOC before modifying project documents"

---

## Limitations

- This pattern is guidance-based. Claude will usually follow it, but may skip steps under context pressure. For deterministic cross-file actions, use [Hooks](hooks-patterns.md).
- MOCs require manual maintenance. When you create a new project file, add it to the MOC. This is a small discipline cost for a large coordination benefit.
- The pattern scales well to ~20-30 files per project. Beyond that, consider splitting into sub-MOCs.
