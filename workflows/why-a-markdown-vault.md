# Why a Markdown Vault

A page for the week a chat forgets a decision you already made.

The chat is short-term memory. It holds this conversation, while you are in it. A folder of Markdown files is the long-term memory. The files last. You can search them. If you keep the folder in git, you can open what a note said last month. That folder is a vault.

The same notes still open next month, in another model, on another bot. An agent can file a new clip and point at the note you wrote. You pay for that with a habit: clip what matters, then give it a home.

License for this page: [CC-BY-SA 4.0](../LICENSE). Attribute Terry Chan / Terrium.

---

## Contents

- [The chat holds this conversation](#the-chat-holds-this-conversation)
- [What the vault keeps](#what-the-vault-keeps)
- [What changes once the note is a file](#what-changes-once-the-note-is-a-file)
- [The same file, any model](#the-same-file-any-model)
- [File it, then cite it](#file-it-then-cite-it)
- [The cost is the habit](#the-cost-is-the-habit)
- [When the chat is enough](#when-the-chat-is-enough)
- [What goes wrong](#what-goes-wrong)
- [One shop, two Thursdays](#one-shop-two-thursdays)

---

## The chat holds this conversation

You and the agent agree on the shop's refund window. Fourteen days. Opened food stays sold. A clerk can approve one exception, and the reason goes in the order note.

That agreement lives in the thread. Close the laptop, start a new chat, or switch to another model, and Thursday's answer can say thirty days, because thirty days sounds like a shop. The Tuesday lines were thinned out, or they sit in a conversation this one cannot see.

A conversation works as a temporary desk. You try a sentence, follow a question, and throw away a bad draft. A long thread keeps the recent lines and lets the early ones go. That is the short-term memory doing its job.

The decision you would hate to re-argue needs a place that is still there when the thread ends.

[How to learn AI](learning-ai.md) starts from a job on your desk and keeps the note that comes out of it. This page is why that note is a file.

---

## What the vault keeps

A vault is a folder of plain text files. Obsidian is a comfortable way to read them. Any editor that opens Markdown is enough. The folder is the memory. The app is a reader.

| | This chat | A Markdown file |
|:---|:---|:---|
| **Lasts** | While this conversation is the one you have open. A long thread thins out the early lines. | Until you delete the file. |
| **Findable** | You scroll, or you remember which thread had the sentence. | You search your title and your words. "14 days" finds the note. |
| **Earlier versions** | The thread is one stream. A sentence you replaced is gone. | If the folder is in git, you can open last month's wording beside today's. |
| **Who can read it** | This model, in this thread. | Any editor, any model, any bot that can open the folder. |

Those four rows are the claim. The notes last, you can search them, you can keep earlier versions, and the next tool can read them.

The first two show up on day one: a folder, and a few notes with titles. Earlier versions are the git history, once a note should survive the laptop. [Mac-first vault](mac-first-obsidian-iparag.md) is that pattern in one line. The local folder is the copy you write in, git is the slice you mean to keep, and a remote is what another person reads when the laptop is closed. The current file is already useful before the first commit. Share the remote when there is something you mean other people to read.

The file stores the conclusion. A useful note is short:

```markdown
# Refund window

Unopened goods: 14 days. Opened food: no refund. A clerk can approve one exception, and the reason goes in the order note.

Why I kept this: a new chat invented 30 days. This is the line we ship.
```

Put it at `your-vault/Projects/shop/refund-window.md` while the shop page is unfinished. `your-vault/` is a placeholder, same as the [vault skeleton](mac-first-obsidian-iparag.md). If you would still want the rule after the page ships, Knowledge is the later home. One home. A link can point at it from anywhere else.

Folder roles live on that skeleton page: Inbox for a capture that has no home yet, Projects for the thing you are finishing, Knowledge for a note that should outlive the project. PARA credit for the four classic roles is in [Resources](resources.md).

---

## What changes once the note is a file

Three changes show up in ordinary weeks.

**You stop re-teaching the decision.** Thursday's message is "Read `your-vault/Projects/shop/refund-window.md` and answer the clerk." You send a path. Tuesday's thread can stay closed.

**A correction happens once.** Two chats can each invent a different window. Two chats that both read the file can still miss, and the miss is in one place. You fix the file. The next reader, a person or a bot, gets the correction. The month agrees with itself.

**New clips have somewhere to land.** A link about exceptions is useful because the refund note already exists. The agent can suggest putting it next to the refund window, because it can see that note. An empty folder gives the agent nothing to attach a clip to. [How to learn AI](learning-ai.md) is the capture habit that fills the first notes.

The file remembers what you last wrote. When the policy changes, you change the file. A lasting copy of an old rule will keep teaching the old rule. Git helps on the day you change your mind: the history can show that March said 30 days and May says 14. The current file is the one to cite. The history is how you can see that you changed it.

A coding session has a sharper version of the same idea. [File-driven workflow](file-driven-workflow.md) names three files a project keeps so the next session can start. This page is the folder those files live in, together with the notes that are not code.

---

## The same file, any model

Markdown is text. The model that helped you write the note can be gone. The note opens anyway.

That is the part that lasts across months and across products. March's refund window still answers a question in June. Switching assistants means pointing the new one at the folder. The notes come along as files.

A second bot can do a different job on the same file. One reads it to answer a clerk. Another files a clip beside it. They share the note because they share the folder.

If filing clips is worth its own helper, give that helper one sentence, the way [Soft CoS](soft-cos.md) writes a desk: file captures and suggest a folder. Beside it, what that helper leaves alone: rewriting your line, publishing, and starting the project. One chat can do the filing too. The folder is what the next helper inherits.

---

## File it, then cite it

Once the note exists, the useful agent work is clerical.

- Suggest a folder.
- Add a few lines on what the note is saying, under your line.
- Point at a project you already have.
- Answer a later question with the path, so you can open the sentence it used.

You decide. [Human notes stay as written](principles.md). The agent appends. Your sentence stays.

A citation you can use next month is a path:

> The window is 14 days. Source: `your-vault/Projects/shop/refund-window.md`.

"We said so on Tuesday" depends on a thread staying open. The path is a file you, or another bot, can open in June.

Ask for the filing the way [How to talk to AI](talking-to-ai.md) asks for any job:

> I dropped a clip in `your-vault/Inbox/exceptions-clip.md`. Suggest a folder and append it under my line. Leave my sentence as I wrote it. Don't publish, and don't make a second copy.
>
> We're done when I can open the note and see my line unchanged, plus the path you suggest.

Then you open the note. The suggestion is a proposal until you accept it.

---

## The cost is the habit

The vault stays empty until you do two small things.

**Clip.** When a decision, a link, or a sentence should outlive the afternoon, drop it in Inbox while you still remember why. A line and a link are enough. A bookmark stays inside one app. A Markdown note is a file the next agent can open.

**Organize.** Give the note a title you would search for. Add one line in your words: why you kept it, and which job it touches. Then give it a home. Inbox is the door. A rule you will need on Thursday wants a home you can name.

A clip takes about a minute. Filing takes a few minutes, or one pass by an agent that you then check. The second time you would have retyped the decision, the minute has paid for itself. By the tenth time, the folder is what the new model reads first.

A handful of notes is a real start. A finished cabinet of folders can wait until the handful feels cramped. [How to learn AI](learning-ai.md) calls a week of this enough: one painful job, and a short note when something earns a keep.

Keep the conclusion you would want quoted back to you. A transcript of every chat buries that conclusion. A note for every stray sentence buries it the other way. The habit is choosing.

---

## When the chat is enough

The chat is the right place for a lot of real work.

- You are trying sentences, and this draft can disappear.
- The answer is a one-time look: what this error means, or a paragraph you will paste somewhere else yourself.
- You asked for the reply in chat, and tomorrow does not need it. [How to talk to AI](talking-to-ai.md) includes that on purpose: "Reply in chat. Don't make a file."
- The fact already lives in a file, and this conversation is using it.
- You are listing options you will drop after you pick one. Keep the pick when you will need the pick again. The rejected options can go with the thread.

A practical test: if losing the thread would lose the conclusion, write the conclusion down. If losing the thread would lose the detours, the chat was the right room.

Scratch work in the thread keeps the vault small enough to search. The vault is for the lines a stranger, or a future model, could follow with you out of the room.

---

## What goes wrong

**The transcript.** Pasting the whole Tuesday chat into a file feels like a save. In June the window is buried in the attempts. Save the conclusion, the reason you kept it, and the link. One idea, one title, the way [How to learn AI](learning-ai.md) describes a note.

**Two homes.** The same 14 days live in the project note and again on a knowledge page. You update one. A later agent cites the other. Give the rule one home, and link to it from the second place. The skeleton page says a note has one home.

**A cleaner sentence.** The agent rewrites your line, and the decision you made leaves with the old wording. Ask it to append. Then open the file and check that your line is still the line you wrote.

**Untitled captures.** Inbox fills with names like "Untitled 14" and "paste.md". Search has nothing to find. The title is the organize step. A nameless file is fine for a day. It is a poor name for a rule you will cite.

**A faithful old rule.** The file will keep teaching 30 days after the shop moved to 14, for as long as 30 is what the file says. Change the file when the decision changes. If the folder is in git, the old line stays in the history, and the current file is the one to cite.

**A private note on a public remote.** The local vault can hold the real shop, the real names, and the drafts. The remote gets the slice you mean other people to read. Public examples in this repo stay on `your-vault/` for that reason. The checklist is [Principles](principles.md).

**Waiting on an app.** The notes are already files. You can begin in the editor you have. A reader with a sidebar can come later. The month of notes is the part that costs more the longer you wait.

---

## One shop, two Thursdays

Monday you are writing the refund page with an agent. The thread tries three wordings. You keep one sentence: 14 days, no refund on opened food, one clerk exception with a reason in the order note. You drop that sentence in `your-vault/Inbox/refund-window.md`, plus a line on why you kept it.

You ask for a folder. The suggestion comes back as Projects, because the page is unfinished. You move the note to `your-vault/Projects/shop/refund-window.md`. Your sentence stays. The agent's line sits under it.

Thursday a different model gets a clerk question. You point it at the file. The reply says 14 days and names the path. Monday's thread stays closed.

A week later a clip about food returns lands in Inbox. The filing pass suggests the shop project, next to the window, because that note is already there. You accept the suggestion, or you correct it. Either way the proposal is on the page where you can see it.

June, another bot, same folder. The window is still 14 days, unless you changed the file. The chats from Monday and Thursday can be gone. The note is what the shop still agrees on.

That is the impact. Clip what should last, and give it a home. After that, the models share a memory you can search, correct, and hand to the next one.

---

## Related

- [How to Learn AI](learning-ai.md)
- [How to Talk to AI](talking-to-ai.md)
- [Mac-first vault](mac-first-obsidian-iparag.md)
- [File-Driven Workflow](file-driven-workflow.md)
- [Soft Chief of Staff](soft-cos.md)
- [Principles](principles.md)
- [Cross-File Sync](cross-file-sync.md)
- [Resources and credits](resources.md)
