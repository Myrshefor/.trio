---
name: trio-sync
description: Synchronizes a project's context across Claude chat, Claude Code, and Cowork through a shared .trio/ folder (STATUS.md, LEDGER.md, TO_CODE.md, FROM_CODE.md, DIALOGUE.md), so each surface can see what the others already decided, built, asked, or answered. Applies to any project that has, or should have, a .trio/ folder. Use at the start of substantial work on such a project; when asked to catch up, check status, or summarize recent cross-tool activity; after finishing a meaningful piece of work, making a decision, or producing something the other surfaces should know about; when asking Code a direct question or reporting an answer back to Code; or when asked to set up, initialize, or "connect" trio sync on a project.
---

# Trio sync

Three Claude surfaces often work on the same project without seeing each other: Claude chat (thinking partner and sanity-checker), Claude Code (hands-on implementer), and Cowork (lead strategist and evaluator). This skill gives them a shared, file-based handoff so each one can catch up on what the others did in under a minute, without the user re-explaining anything.

The shared state lives in a `.trio/` folder at the project root:

- `.trio/STATUS.md` — a living snapshot: current focus, recent decisions, open questions, next actions. Overwrite this in place; it is not a log.
- `.trio/LEDGER.md` — an append-only history, newest entry first, of what each surface did. Keep entries short — a summary, not the full exchange (see `DIALOGUE.md` for the verbatim version).
- `.trio/TO_CODE.md` — open asks/questions directed **at Code**, written by Cowork or Chat. Code is the only reader that needs to act on it.
- `.trio/FROM_CODE.md` — open answers/reports directed **back at Cowork/Chat**, written by Code. Cowork (and, via the Project mirror, Chat) is the reader that needs to act on it.
- `.trio/DIALOGUE.md` — append-only, verbatim, **never trimmed or summarized**: the permanent transcript of what actually got asked and answered through `TO_CODE.md`/`FROM_CODE.md`. This is the one file in the protocol nothing ever shortens. Only the user clears it, and only if they explicitly ask.

`TO_CODE.md` and `FROM_CODE.md` are inboxes — current, open items waiting on the other side's attention. `LEDGER.md` is a terse index of what happened. `DIALOGUE.md` is the durable, word-for-word record. Three different jobs; don't collapse them into one file.

## ⚠ `.trio/*.md` is the only handoff channel — never `CLAUDE.md`

`CLAUDE.md` (or any other project-instructions file Claude Code reads on startup) is **not** part of this protocol. It holds persistent instructions *for* Code, not messages *between* surfaces. Writing an answer, a report, or a status update into `CLAUDE.md` instead of `.trio/FROM_CODE.md` (or `LEDGER.md`/`STATUS.md`) means the other surfaces will never see it — they don't read `CLAUDE.md` as part of trio-sync, and it silently breaks the handoff. If you are Code and asked to report something back through trio-sync, the answer belongs in `.trio/FROM_CODE.md` (and, if it's meaningful, `.trio/LEDGER.md` and `.trio/DIALOGUE.md`) — never in `CLAUDE.md`. If you notice `CLAUDE.md` has accumulated content that looks like a trio handoff message rather than a standing instruction, flag it to the user rather than silently leaving it there.

## Identify which surface you are

Work this out before reading or writing anything, since ledger entries are labeled by role:

- **Code** — direct shell/git access to the project's files on the user's own machine (a Claude Code terminal session).
- **Cowork** — an autonomous agent working across files, the web, and possibly other tools in a sandboxed or desktop task, without a human directing every step.
- **Chat** — a live back-and-forth conversation with the user, typically with limited or no direct file access.

If it's genuinely ambiguous, ask the user once ("Should I log this as Code, Cowork, or Chat?") instead of guessing wrong.

## If `.trio/` doesn't exist yet

When asked to set up, initialize, or "connect" trio sync on a project — or when a project would clearly benefit and the user agrees — create the folder with these five files. Substitute the real project name and the current date/time.

`.trio/STATUS.md`:

```markdown
# <Project Name> — Trio Status
_Living snapshot, not a history — see LEDGER.md for that. Last updated: <date time> by <Chat|Code|Cowork>._

## Roles
- **Chat** — thinking partner, sounding board, sanity-checker
- **Code** — hands-on implementer
- **Cowork** — lead strategist, evaluates progress and plans ahead

## Current focus
_(what's being worked on right now)_

## Recent decisions
_(none yet)_

## Open questions
_(none yet)_

## Next actions
_(none yet)_
```

`.trio/LEDGER.md`:

```markdown
# <Project Name> — Trio Ledger
_Newest entry at the top. Keep entries short — a few lines, not a transcript. See DIALOGUE.md for the verbatim exchange._
```

`.trio/TO_CODE.md`:

```markdown
# <Project Name> — Open asks for Code
_Cowork/Chat write here; Code reads and clears. Newest at the top. Empty = nothing outstanding._
```

`.trio/FROM_CODE.md`:

```markdown
# <Project Name> — Open answers from Code
_Code writes here; Cowork/Chat read and clear. Newest at the top. Empty = nothing outstanding._
```

`.trio/DIALOGUE.md`:

```markdown
# <Project Name> — Trio Dialogue (verbatim, never trimmed)
_Full text of everything exchanged through TO_CODE.md / FROM_CODE.md, newest first. This file is only ever appended to. It is cleared only if the user explicitly asks._
```

Tell the user in plain language what was set up and that the other two surfaces can now read it too — this only works once trio-sync is installed or enabled on those surfaces as well.

## At the start of substantial work

If `.trio/` exists, before diving into the user's request:

1. Read `.trio/STATUS.md` in full.
2. Read the newest 5-10 entries of `.trio/LEDGER.md` (it's newest-first, so this is just the top of the file).
3. Check `.trio/TO_CODE.md` (if you are Code) or `.trio/FROM_CODE.md` (if you are Cowork/Chat) for anything addressed to you that's still open.
4. In 2-4 sentences, tell the user what the other surfaces have done recently, and flag anything in "Open questions," "Next actions," or your inbox file addressed to your role. Then proceed with their actual request.

Skip this for a quick one-off question unrelated to the project's ongoing work.

## When asked to catch up, check status, or summarize activity

Read `.trio/STATUS.md`, the newest `LEDGER.md` entries, and both inbox files, and summarize them directly. This doesn't require doing any new work first.

## Asking Code something, or answering Cowork/Chat back

Use this when a surface needs a direct answer from another, not just a general status update:

1. **Cowork or Chat asking Code something:** append the question to the top of `.trio/TO_CODE.md` (newest first), with a date and what's being asked. Chat has no file access, so Cowork writes on Chat's behalf — Chat tells Cowork (or the user relays it) what to ask.
2. **Code answering:** read `.trio/TO_CODE.md`, address the open item, then append the answer to the top of `.trio/FROM_CODE.md` (newest first). Remove or mark resolved the corresponding entry in `TO_CODE.md` once answered, so that file only ever shows what's still outstanding.
3. **Cowork/Chat picking up the answer:** read `.trio/FROM_CODE.md`, then remove or mark resolved the entry once it's been seen and acted on.
4. **Either side, once an exchange is resolved:** append the full verbatim question-and-answer pair to the top of `.trio/DIALOGUE.md`. This is the only place the complete, unabridged text lives permanently — never summarize or trim it. Reference this dialogue entry from `LEDGER.md` with a one-line summary rather than duplicating the full text there.

Keep `TO_CODE.md` and `FROM_CODE.md` limited to genuinely open items — once resolved and copied into `DIALOGUE.md`, remove the resolved entry from the inbox file so it stays a quick "what's outstanding" glance for whoever checks it next.

## After finishing meaningful work

A decision, a completed task, a file worth flagging, or a question for one of the other two surfaces all count as meaningful. A one-line clarifying answer does not. When it's meaningful:

1. Prepend a new entry to the top of `.trio/LEDGER.md` (right below the header, above the previous newest entry):

```markdown
---
### <date> <time> — <Chat|Code|Cowork>
**Did:** <one to three sentences>
**Touched:** <file paths, if any — omit this line if none>
**Handoff:** <what the other two should know or do — omit this line if there's nothing>
---
```

2. Update `.trio/STATUS.md` in place: revise "Current focus" if it changed, add to "Recent decisions" if one was made (trim older ones no longer relevant — keep roughly the 5 most recent), and check off or add to "Open questions" and "Next actions."

Keep everything concise. Reference file paths rather than pasting file contents into the ledger — full verbatim content belongs in `DIALOGUE.md`, not `LEDGER.md`.

## Cowork: mirror the status into a Claude Project (if one is attached)

Regular Claude chat has no device/file access — it cannot read `.trio/` directly, even if trio-sync is enabled for it. Claude Projects are the one channel chat *does* get automatically: when the user chats inside a Project, that Project's docs are pulled into context with no manual pasting required.

If this session is attached to a Claude Project (check for an `attachedProject` context block) and you are Cowork:

1. After you update `.trio/STATUS.md` for real work — not on every read-only status check — also write a mirror doc into that Project, e.g. `claude/trio-status-snapshot.md`, via the Projects tool.
2. Whenever `.trio/FROM_CODE.md` has open items addressed to Cowork/Chat, mirror its current contents into the Project too, e.g. `claude/trio-from-code-snapshot.md`, so Chat can see what Code is waiting on a response to, or what Code just reported, without needing to be told.
3. Whenever you append to `.trio/DIALOGUE.md`, also append the same new entries to a Project doc mirror, e.g. `claude/trio-dialogue-mirror.md` — mirror only what's new since the last sync, not the whole file every time, but never drop or summarize entries: this mirror must stay verbatim and cumulative, just like the source. This is the durable "hot memory" of the Cowork/Code (and Chat) correspondence that the user can come back to without disturbing their current conversation — never trim it unless the user explicitly asks.
4. Keep the `STATUS.md` mirror to the current snapshot only (current focus, recent decisions, standing requirements, next actions) — don't duplicate the full `LEDGER.md` history into it; point back to `.trio/LEDGER.md` on disk instead, so there's one place that can go stale, not two.
5. Note at the top of each mirror doc when it was last synced and by whom, so the user (or chat) can tell if it's fresh.
6. Tell the user once that for chat to see this automatically, they need to actually be chatting inside that Project — not a stray conversation outside it.

Code and Chat don't have a Projects tool, so this step is Cowork-only. If no Project is attached, skip this section entirely.

## General principles

- Never delete another surface's `LEDGER.md` entries or `DIALOGUE.md` entries — both are shared, permanent history. `LEDGER.md` entries may be trimmed for length by their own author if genuinely no longer relevant; `DIALOGUE.md` entries are never trimmed by any surface, only by explicit user request.
- `TO_CODE.md` and `FROM_CODE.md` entries *are* meant to be removed once resolved (after their content is preserved in `DIALOGUE.md`) — they're inboxes, not history.
- If `STATUS.md` and `LEDGER.md` disagree, `LEDGER.md` is the source of truth; fix `STATUS.md` to match.
- If `.trio/*.md` and `CLAUDE.md` seem to disagree about what Code reported, `.trio/*.md` is the source of truth for trio-sync purposes — `CLAUDE.md` was never supposed to carry that information (see the warning above).
- This protocol only works when all three surfaces actually have trio-sync installed or enabled. If asked why another surface "didn't see" something, check whether trio-sync is set up there too, and whether it wrote to `.trio/*.md` rather than some other file — see this plugin's README.
- Reusing trio-sync on a new project doesn't require reinstalling anything — the plugin/skill is already active for every project on this account. Just say "set up trio sync here" on the new project to create its `.trio/` folder.
