# Friend Starter — How It Presents

A note for a BOT Creator desk: the one job of designing bots and staging a starter a friend can import.

This page is the presentation contract. The template body is not in this repo. A friend receives a small folder they can copy onto Grok Bot or any other host. That host's own menus stay in the host's docs.

Read [Principles](../workflows/11-principles.md) before you fill a starter in. The picture of the system the starter is introducing is [Fleet Architecture](../workflows/10-architecture.md).

---

## Contents

- [What the friend should see first](#what-the-friend-should-see-first)
- [A folder they can copy](#a-folder-they-can-copy)
- [What stays out of the folder](#what-stays-out-of-the-folder)
- [After import](#after-import)

---

## What the friend should see first

The first file is a single page, not a copy of someone else's vault. On that page, in this order:

1. **The one job** of this desk, in one sentence.
2. **The anti-jobs**, so "helpfulness" has a boundary.
3. **The redirect line**, with the owner left as a name they can change. Example: `Shop owns this. Stopping here.`
4. **The board path**, relative to the starter, such as `board.md`.
5. **The four PROOF fields**: task, done-when, evidence, don't.
6. **A link** to Terrium's [principles](../workflows/11-principles.md) and [Soft CoS](../workflows/12-soft-cos.md), for the long version.

A friend who reads only that page should know who they talk to (the GM), what this desk will refuse, and where a finish gets written.

---

## A folder they can copy

Import, in this note, means the friend receives a small tree and puts it where their desks can read it. The tree below is the presentation, with blanks instead of a finished pack.

```
friend-starter/
  README.md       # the first page: job, anti-jobs, redirect, board, PROOF
  board.md        # verbs only, no live rows
  desks/
    gm.md         # routes only
    writer.md     # one example specialist
```

`board.md` starts empty of real work:

```markdown
# Board

## Now

## Done
```

`gm.md` states the routing job and points at [Soft Chief of Staff](../workflows/12-soft-cos.md). `writer.md` states one fictional job and its anti-jobs. Add a desk by copying that shape. One file, one job.

People who want a whole vault, not a single desk, use the [IPARAG skeleton](../workflows/14-mac-first-obsidian-iparag.md). The starter can live inside `your-vault/Projects/fleet/` later. It does not have to arrive as a vault.

---

## What stays out of the folder

- Home-folder paths, account names, and machine-specific sync locations
- Agent ids, keys, tokens, and private endpoints
- Daily notes, live board rows, and unpublished personal setup instructions
- A second copy of the twelve Claude Code lessons

The starter points at Terrium for the lessons and the fleet essays. The lessons stay in the course.

---

## After import

The friend fills three blanks: their desk names, the path of their board, and the GM they will actually talk to. Then they run one real task through start → finish with PROOF, or through a wrong-desk redirect, and they see whether the first page told the truth.

Until that rehearsal, the starter is a proposal. The [board guide](../workflows/13-fleet-board-empty-wake-proof.md) is the rehearsal script.

---

## Related

- [Tools](README.md)
- [Principles](../workflows/11-principles.md)
- [Soft Chief of Staff](../workflows/12-soft-cos.md)
- [Mac-first vault](../workflows/14-mac-first-obsidian-iparag.md)
