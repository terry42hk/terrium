# How to Talk to AI

A page for the first time you ask an agent to do work.

A loose prompt is a wish. What you send is a **work order**: a small contract the agent can carry out, and that you can read again tomorrow without the chat. The order carries its own facts. The agent has less room to guess.

Five parts, every time:

| Part | What you write | The question it closes |
|:---|:---|:---|
| **Goal** | One job, including who it is for | What is this ask? |
| **Deliverable** | The file, the section, the link, or the reply | What do I get back? |
| **Interfaces** | The files, screens, and notes that already exist | Where is the truth? |
| **Rules** | The don't list, and when to stop | What stays untouched? |
| **Done-when** | The condition a stranger can check | When is it finished? |

Done-when is the acceptance line. On a [fleet board](fleet-board-empty-wake-proof.md) it is the same field inside PROOF.

These are sentence patterns. A costume such as "you are a world-class expert" still needs a file name and a finish line.

License for this page: [CC-BY-SA 4.0](../LICENSE). Attribute Terry Chan / Terrium.

---

## Contents

- [The work order](#the-work-order)
- [Name the deliverable](#name-the-deliverable)
- [Done-when](#done-when)
- [PROOF, when the finish has to last](#proof-when-the-finish-has-to-last)
- [Before and after](#before-and-after)
- [Patterns, not spells](#patterns-not-spells)
- [One ask, one job](#one-ask-one-job)

---

## The work order

Labels help while the habit is new. Prose is fine once the same five parts are always there.

```markdown
Goal: State the shop's refund window for a clerk.
Deliverable: a new file, your-vault/Projects/shop/refund-window.md
Interfaces: the human notes in your-vault/Projects/shop/notes.md
Done-when: the file has three short sections a clerk can follow: the window, the exceptions, and who approves an exception.
Rules: leave pricing.md unchanged. Do not publish. Leave the human lines as written.
Stop: if a number is missing, stop and name the number.
```

`your-vault/` is a placeholder, as in the [vault skeleton](mac-first-obsidian-iparag.md). Public examples stop there.

**Interfaces** are the page, the note, and the screen that already hold the truth. Name them. An unnamed interface gets invented.

**Rules** are the don't list. [Soft Chief of Staff](soft-cos.md) hands a desk the same brief: who it is for, the done-when, and the don't. The work order names the task. The deliverable is what comes back from it.

A wish leaves a part blank. "Help me with refunds" names a topic. The form of the reply, the finish line, and the files that must stay put are still open, so the agent picks them.

When the wish is still vague, write the short spec before any building. Goal, deliverable, done-when, rules. Then let the agent iterate until the done-when holds.

If a test already covers that line, leave the test alone. The agent changes the work until the test passes. If the job is a page, the check is the section you open and read. Same work order. The evidence is whatever you can open.

---

## Name the deliverable

The deliverable is the object you expect back.

| You expect back | Say it as |
|:---|:---|
| A new note | `Deliverable: your-vault/Projects/shop/refund-window.md` |
| An edit to one section | `Deliverable: a new section in the existing page, under "Refunds"` |
| A link a person can open | `Deliverable: a draft review link. The description lists the files.` |
| A decision, still in chat | `Deliverable: three options in the reply, one recommended. Write no file.` |

Say so when the deliverable is a reply. Otherwise a helpful agent creates a file you did not ask for.

A result that has to survive the conversation belongs in a file. [File-driven workflow](file-driven-workflow.md) is the longer version: chat is where the talking happens, and the file is where the conclusion stays.

Before the agent starts, name two things: the object you will open, and the check you will perform on it. If either one is hard to name, the job is still a wish. Narrow it until both are obvious. The rules line says what is allowed to change, and what needs a separate yes.

---

## Done-when

Done-when is an observable condition. "Looks good" is a mood. "The page has three sections a clerk can follow" is a condition. "The empty cart shows the empty-cart message" is a condition.

A done-when has a failed state you can point at. "Clearer" has none. An agent will treat a compliment as success.

Keep two levels, so the list stays short:

- **Every job.** Standing rules live with the project. In this repo that is [CLAUDE.md rules](claude-md-rules.md) and the [principles](principles.md): human notes stay as written, public pages stay public, a finish can point at evidence.
- **This job.** The lines live in the work order. They name the window, the exception, the screen, the file.

A job that misses one line is still in progress. For each line, the reply says where you can look. [Verification](verification-workflow.md) is you actually looking. A message that says "saved" waits until you open the file.

---

## PROOF, when the finish has to last

Goal, deliverable, done-when, and rules are enough for one conversation.

When you tomorrow, or another desk, must see how the job ended, use the four fields from [Fleet board, empty wake, PROOF](fleet-board-empty-wake-proof.md):

| Field | In the work order | You can check it by |
|:---|:---|:---|
| **task** | The goal, one line | Reading it |
| **done-when** | The acceptance line | Doing the check |
| **evidence** | The path, link, or test | Opening it |
| **don't** | The rules | Seeing that change is absent |

You can ask for the report in the same order: "When you finish, reply with task, done-when, evidence, and don't."

A missing field means the row is still open. The board page shows the spot-check. The four words work in a single chat, before any fleet exists.

---

## Before and after

### A page

Wish:

> Can you improve our refund explanation? Make it clearer and more professional.

The agent can rewrite any file, invent a policy, or answer in chat.

Work order:

> Goal: State the refund window for a clerk.
> Deliverable: a new file, `your-vault/Projects/shop/refund-window.md`.
> Interfaces: `your-vault/Projects/shop/notes.md`. Leave those human lines as written.
> Done-when: three short sections, the window, the exceptions, and who approves an exception. A clerk who was not in this chat can follow them.
> Rules: do not edit the pricing page. Do not publish. If a number is missing, stop and name it.

### A fix

Wish:

> The cart feels broken. Look around and fix stuff.

Work order:

> Goal: The empty cart throws. It should show the empty-cart message.
> Deliverable: a branch with that fix. Do not merge.
> Interfaces: the empty-cart screen, and the test that covers it if one exists.
> Done-when: an empty cart shows the empty-cart message.
> Evidence: the test that failed before and passes after, or the screen I can open.
> Rules: do not restyle checkout. Do not change shipping rules. If a test already covers this, leave the test alone.

The second version is a short spec. The agent can iterate toward the done-when. The rules stay put.

### A note you will reread

Wish:

> Teach me about agents.

Work order:

> Goal: Explain the one term that blocked the shop page today.
> Deliverable: `your-vault/Knowledge/agents-one-term.md`.
> Done-when: the note names the goal it serves, defines the one term in my words, and links the page where I hit it.
> Rules: do not outline a course. Do not create a second file.

That third order is the start of [How to learn AI](learning-ai.md). The goal comes first. The note is a byproduct.

---

## Patterns, not spells

Say these in a normal sentence. Each one fails if the deliverable is still missing.

### Do

| Pattern | What it fixes |
|:---|:---|
| "The work order is …" | A wish with no contract |
| "The deliverable is …" | The form of the reply |
| "Done when I can open … and check …" | The acceptance line |
| "Read these files first." | The interfaces |
| "Leave this note as written." | A decision you already made |
| "If a fact is missing, stop and name it." | A gap filled by invention |
| "One file." | A review you can finish |
| "Reply with the path" or "reply with the link." | Evidence attached to the claim |

### Don't

| Pattern | What you get instead |
|:---|:---|
| "Make it better." | A style the agent chose, and no finish line |
| "You know what I mean." | A guess at taste you never stated |
| "While you are there, also …" | A second job inside the first |
| "Handle everything related." | A scope with no edge |
| "Act as a world-class …" | A costume in front of a missing file name |
| "Keep going until it feels perfect." | A loop with no stop |

"While you are there" is how one order becomes two. Split it. [Soft CoS](soft-cos.md) gives the two pieces two owners when more than one desk exists. Alone, you still send two work orders.

---

## One ask, one job

One sentence of work. Research that ends in a list of sources is one order. A page that uses that list is the next order.

[Principles](principles.md) put the same limit on a fleet: one bot, one job. In a single chat you are the router. The agent is the desk. The deliverable is the row you can point at tomorrow.

---

## Related

- [How to Learn AI](learning-ai.md)
- [Fleet board, empty wake, PROOF](fleet-board-empty-wake-proof.md)
- [Principles](principles.md)
- [Soft Chief of Staff](soft-cos.md)
- [File-Driven Workflow](file-driven-workflow.md)
- [Verification](verification-workflow.md)
- [Mac-first vault](mac-first-obsidian-iparag.md)
- [Resources and credits](resources.md)
