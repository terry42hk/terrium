# Skills

Claude Code skills are reusable workflow instructions that live in `~/.claude/skills/`. When you invoke a skill (e.g., `/translation`), Claude loads the `SKILL.md` file and follows its workflow — like giving Claude a playbook for a specific task.

## How to install a skill

```bash
# Clone or copy the skill folder into your skills directory
cp -r path/to/skill ~/.claude/skills/

# Or symlink if you want to keep it updated
ln -s path/to/skill ~/.claude/skills/skill-name
```

Each skill folder needs at minimum a `SKILL.md` file with YAML frontmatter (`name`, `description`) and markdown instructions.

## Authored

Skills I wrote and use daily.

| Skill | What it does |
|-------|-------------|
| [translation](translation/) | EN↔Chinese translation with HK Traditional Chinese conventions, domain awareness, and cultural adaptation |

## Curated

Recommended skills from others — see [curated.md](curated.md) for the full list with commentary.
