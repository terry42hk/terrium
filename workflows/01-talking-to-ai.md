# 01. How to Talk to AI

Beginner path, first note. Next is [how to learn AI](02-learning-ai.md).

A page for the first time you ask an agent to do work.

The point is a result you can get again. The same job tomorrow should land in the same shape.

Say what you want, what finished looks like, what not to touch, and how we know it worked. Put them in the message. You can read that message again tomorrow without the chat, and the agent has less room to guess.

Four things, every time:

| In the message | What you say | It answers |
|:---|:---|:---|
| **What you want** | One job, and who it is for | What are we doing? |
| **What finished looks like** | The file, the section, the link, or the reply | What do I open? |
| **What not to touch** | What stays as it is, and when to stop | What do we leave alone? |
| **How we know it worked** | A check someone outside this chat can do | When are we done? |

Also name the notes, files, and screens that already have the answer. If you skip them, the agent invents them.

You can say each one in a normal sentence. "You are a world-class expert" still needs a file name and a way to tell it worked.

License for this page: [CC-BY-SA 4.0](../LICENSE). Attribute Terry Chan / Terrium.

---

## Contents

- [Why this beats ask and hope](#why-this-beats-ask-and-hope)
- [The message](#the-message)
- [What finished looks like](#what-finished-looks-like)
- [What not to touch](#what-not-to-touch)
- [How we know it worked](#how-we-know-it-worked)
- [When someone has to see it later](#when-someone-has-to-see-it-later)
- [Before and after](#before-and-after)
- [Say this, skip that](#say-this-skip-that)
- [One ask, one job](#one-ask-one-job)

---

## Why this beats ask and hope

"Make it clearer" can work on a good day. The next day it rewrites a file you meant to keep, fills a blank with a guess, and says it is done. You have no way to tell the good day from the bad one.

The same four questions are a lightweight harness. A harness, here, is constraints plus checkpoints.

Constraints are the edges you set before it starts. What you want. What not to touch. The notes and screens that already have the answer.

Checkpoints are how you can tell it finished. What finished looks like. How we know it worked. You open that thing and look. The agent does not get to call it done.

Put them in the message and the next time has the same edges and the same check. The model can still miss. A miss sits on a line you can point at, and the job stays open until that line holds. That is a stable outcome.

The version where a rule runs even when the model forgets it lives in the course, under [Harness vs Model](../claude-code-concepts/01-harness-vs-model.md). This page is the harness you can write in chat today.

---

## The message

Write it the way you would message a teammate.

> Can you write the refund window so a clerk can follow it? New file, `your-vault/Projects/shop/refund-window.md`. Start from the notes in `your-vault/Projects/shop/notes.md` and leave those lines as I wrote them. Three short sections: the window, the exceptions, and who approves an exception. Leave `pricing.md` alone. Don't publish. If a number isn't in the notes, stop and tell me which one.

That message already says:

- What you want: a clerk can follow the refund window.
- What finished looks like: a new file with three short sections.
- What not to touch: pricing, publishing, and the lines you already wrote.
- How we know it worked: a clerk who was not in this chat can follow the three sections. A missing number means stop.

`your-vault/` is a placeholder, as in the [vault skeleton](14-mac-first-obsidian-iparag.md). Public examples stop there.

Name the page, the note, and the screen that already hold the answer, so the agent does not invent them.

[Soft Chief of Staff](12-soft-cos.md) is the same kind of note, for when several agents split the work: who it is for, what finished looks like, and what to leave alone.

"Help me with refunds" names a topic. The shape of the reply, the finish, and the files that must stay put are still open, so the agent picks them.

When the wish is still vague, write the message before any building. Then let the agent keep going until the check you named is true.

If a test already covers the line, say to leave the test alone. The agent changes the work until the test passes. If the job is a page, the check is the section you open and read.

---

## What finished looks like

Say the thing you expect to open.

| You expect back | Say it like this |
|:---|:---|
| A new note | "New file: `your-vault/Projects/shop/refund-window.md`." |
| An edit to one section | "Add a section on the existing page, under Refunds. Leave the rest." |
| A link a person can open | "Send a link to the draft. The description lists the files." |
| A decision, still in chat | "Three options in the reply, and the one you recommend. Don't write a file." |

Say when you want the answer in the reply. A helpful agent will otherwise create a file you did not ask for.

Something that has to survive the conversation belongs in a file. [Why a Markdown vault](03-why-a-markdown-vault.md) is why that file is worth keeping, and when the reply can stay in the chat. [File-driven workflow](35-file-driven-workflow.md) is the longer version for a coding session: the chat is where you talk, and the file is where the conclusion stays.

Before the agent starts, name two things: what you will open, and what you will check. If either one is hard to name, the job is still a wish. Narrow it until both are obvious.

---

## What not to touch

Say what stays as it is, and when the agent should stop and ask.

Leave a decision you already made. "Leave this note as written" keeps your sentence in the file. "Leave the test alone" keeps the check honest. "Don't publish" and "don't edit pricing" say where this job stops.

If a fact is missing, the message says to stop and name the gap. That pause is how a guess stays out of the file.

Anything you did not name needs its own yes, in a second message. [Soft CoS](12-soft-cos.md) gives those two messages two owners when more than one agent is around. On your own, you still send two messages.

Keep the list short by splitting two kinds of lines:

- **Every job.** A few lines live with the project. In this repo that is [CLAUDE.md rules](30-claude-md-rules.md) and the [principles](11-principles.md): human notes stay as written, public pages stay public, and a finish points at something you can open.
- **This job.** The lines in this message. They name the window, the exception, the screen, the file.

---

## How we know it worked

"The page has three sections a clerk can follow" is a check. "The empty cart shows the empty-cart message" is a check. "Clearer" has no failed version you can point at. The agent will treat a compliment as success.

What you left alone is part of finished. A refund note that also rewrites pricing did not finish.

A job that misses one line is still in progress. For each line, the reply says where you can look. [Verification](37-verification-workflow.md) is you actually looking. "Saved" waits until you open the file.

---

## When someone has to see it later

The four things above are enough for one conversation.

When you need the ending tomorrow, or someone else does, ask for a short recap:

- what the job was
- what finished looked like
- where to look: the path, the link, or the test
- what you left alone

You can ask in the same message: "When you finish, tell me what the job was, what finished looked like, where to look, and what you left alone."

A [fleet board](13-fleet-board-empty-wake-proof.md) writes that recap under four names: task, done-when, evidence, and don't. Together they are called PROOF. Ask for the recap in the words above. The names can wait until you have a board. A missing line means it is still open. The four questions work in a single chat, before any fleet exists.

---

## Before and after

The first message is the one we usually type. The second is the same ask, written so a teammate could start.

### A page

Before:

> Can you improve our refund explanation? Make it clearer and more professional.

The agent can rewrite any file, invent a policy, or answer in the chat.

After:

> Can you write the refund window so a clerk can follow it?
>
> New file, `your-vault/Projects/shop/refund-window.md`. Start from the notes in `your-vault/Projects/shop/notes.md` and leave those lines as I wrote them. Three short sections: the window, the exceptions, and who approves an exception. A clerk who was not in this chat should be able to follow them.
>
> Leave the pricing page alone. Don't publish. If a number isn't in the notes, stop and tell me which one.

### A fix

Before:

> The cart feels broken. Look around and fix stuff.

After:

> The empty cart throws. It should show the empty-cart message.
>
> Put the fix on a branch, and don't merge. Look at the empty-cart screen, and at the test that covers it if we have one.
>
> We're done when an empty cart shows that message. Send the test that was failing and now passes, or tell me which screen to open.
>
> Don't restyle checkout. Don't change shipping rules. If a test already covers this, leave the test alone.

The agent can keep going until the empty cart shows the message. Checkout styling and shipping stay put.

### A note you will reread

Before:

> Teach me about agents.

After:

> I got stuck on one term on the shop page today. Can you explain just that term?
>
> Put it in `your-vault/Knowledge/agents-one-term.md`. Say what I was trying to do, define the term in my words, and link the page where I hit it.
>
> Don't outline a course. Don't create a second file.

That third message is the start of [How to learn AI](02-learning-ai.md). The job comes first. The note comes out of the job.

---

## Say this, skip that

Say these in a normal sentence. Each one fails if you never said what you want back.

### Say this

| You can say | What it saves you from |
|:---|:---|
| "I need a note a clerk can follow." | A topic with no job |
| "Put it in this file." or "Reply in chat. Don't make a file." | A surprise document |
| "We're done when I can open this and see that." | A finish nobody can check |
| "Read these files first." | A source the agent made up |
| "Leave this note as written." | A decision you already made, rewritten |
| "If a fact is missing, stop and name it." | A gap filled in with a guess |
| "One file." | A pile you will not review |
| "Reply with the path." or "Reply with the link." | A "done" you cannot open |

### Skip this

| You might say | What you get back |
|:---|:---|
| "Make it better." | A style the agent picked, and no finish |
| "You know what I mean." | A guess at a taste you never said |
| "While you are there, also …" | A second job inside the first |
| "Handle everything related." | Work with no edge |
| "Act as a world-class …" | A costume, and still no file name |
| "Keep going until it feels perfect." | A loop with no stop |

"While you are there" is how one ask becomes two. Send the second ask on its own.

---

## One ask, one job

One sentence of work. Finding the sources is one message. The page that uses those sources is the next message.

[Principles](11-principles.md) put the same limit on a fleet: one bot, one job. In a single chat, you pick the job. Tomorrow you should be able to point at the file.

---

## Related

- [How to Learn AI](02-learning-ai.md)
- [Why a Markdown Vault](03-why-a-markdown-vault.md)
- [Fleet board, empty wake, PROOF](13-fleet-board-empty-wake-proof.md)
- [Principles](11-principles.md)
- [Soft Chief of Staff](12-soft-cos.md)
- [File-Driven Workflow](35-file-driven-workflow.md)
- [Verification](37-verification-workflow.md)
- [Mac-first vault](14-mac-first-obsidian-iparag.md)
- [Resources and credits](15-resources.md)
