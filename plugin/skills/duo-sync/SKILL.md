---
name: duo-sync
description: File-based hand-off between the Claude app (chat/Cowork) and Claude Code on one project, through a shared .trio/ folder (STATE.md, INBOX.md, DIALOGUE.md, LEDGER.md). Use at the start of substantial work on a project that has .trio/, to catch up or report status, after meaningful work or a decision, to pass a task or answer between Claude and Code, or to set up, connect or migrate duo-sync (formerly trio-sync).
---

# Duo sync

Two Claude surfaces work on the same project without seeing each other's conversations:

- **Claude** — the Claude app (what used to be separate "chat" and "Cowork"). Thinks, plans, researches, reviews; can read and write project files through the device bridge, a connected repo, or a Project.
- **Code** — a Claude Code session in a terminal. Implements, runs, commits.

`.trio/` is the shared folder that lets either one pick up where the other stopped. (The folder keeps its old name on purpose: existing projects, links and repos keep working. The protocol is called duo-sync since v0.5.0.)

| File | Job | Rule |
|---|---|---|
| `STATE.md` | Living state: `СЕЙЧАС` → `ПРОШЛАЯ ЗАПИСЬ` → `ПОЗАПРОШЛАЯ ЗАПИСЬ`, plus standing requirements | Rotating, see below |
| `INBOX.md` | The one place for asks and answers between surfaces | Open items + the last 2 closed |
| `DIALOGUE.md` | Verbatim record of every closed inbox exchange | Never trimmed; rotates into archive files |
| `LEDGER.md` | Overflow: state blocks that rotated out of `STATE.md` | Append-only, newest first |

## ⚠ `.trio/*.md` is the only hand-off channel — never `CLAUDE.md`

`CLAUDE.md` holds standing instructions *for* Code, not messages *between* surfaces. A report written there is invisible to the other side. Answers, findings and status go to `.trio/`. If you find hand-off-looking content in `CLAUDE.md`, tell the user instead of leaving it.

## Which surface are you

Label every entry `Claude` or `Code`. Old entries labeled Chat or Cowork were written by Claude. If you truly can't tell, ask once.

## Start of substantial work

If `.trio/` exists: (1) read `STATE.md`; (2) read `INBOX.md` and note items addressed to you; (3) read `LEDGER.md` only when you need older context. Then tell the user in 2–4 sentences what the other side did and what waits on you, and continue. Skip this for a quick unrelated question. "Catch me up" / "what's the status" = the same reading, then answer — no new work required.

## STATE.md — three slots, rotation instead of editing history

Template:

```markdown
# <Project> — State
_Last updated: <date time> by <Claude|Code>. Rotation: СЕЙЧАС → ПРОШЛАЯ ЗАПИСЬ → ПОЗАПРОШЛАЯ ЗАПИСЬ → LEDGER.md_

## СЕЙЧАС
### <date time> — <Claude|Code>
**Фокус:** what is being worked on
**Сделано:** what was finished (files, commits)
**Решения:** decisions made, with who made them
**Открыто:** unresolved questions
**Дальше:** next actions (owner in brackets)

## ПРОШЛАЯ ЗАПИСЬ
_(пусто)_

## ПОЗАПРОШЛАЯ ЗАПИСЬ
_(пусто)_

## Постоянные требования
- ...
```

**When** work is meaningful (a task finished, a decision made, focus changed, something other side must know) **write a new `СЕЙЧАС`**. Same session, same focus → edit your own `СЕЙЧАС` in place; don't rotate on every small step.

**Rotation, in this order** (so an interruption can only duplicate, never lose):
1. If `ПОЗАПРОШЛАЯ ЗАПИСЬ` has content, copy that block **verbatim** to the top of `LEDGER.md` (right under its header).
2. `ПРОШЛАЯ` → `ПОЗАПРОШЛАЯ`.
3. `СЕЙЧАС` → `ПРОШЛАЯ`.
4. Write the new `СЕЙЧАС`. **Carry forward** every item still open in the old `Открыто` and unchecked in `Дальше` — restate it or mark it done. Nothing open may fall into `LEDGER.md` silently.

`Постоянные требования` never rotate; they change only when the user changes them. Keep decisions that must stay in force there, not only in a slot.

## INBOX.md — one inbox, two directions

```markdown
# <Project> — Inbox
_Open: waiting on someone. Closed: only the last two stay here; older ones go to DIALOGUE.md._

## Открыто
### #<n> · <date time> · <Claude|Code> → <Claude|Code> · статус: open|answered
**Запрос:** ...
**Файлы/контекст:** ...
**Ответ:** _(пока нет)_ / text + who + when

## Закрыто — последняя
## Закрыто — предпоследняя
```

- **Ask:** append a new item at the top of `Открыто`, status `open`, addressee = the other surface. Ids are sequential; take max id seen in `INBOX.md` and `DIALOGUE.md` plus one.
- **Answer:** the addressee fills `Ответ`, sets status `answered`. The item stays in `Открыто` until the asker has seen it.
- **Close:** the asker, after reading and acting, moves the item to `Закрыто — последняя`. Before that: the old `последняя` → `предпоследняя`, and the old `предпоследняя` is appended **verbatim** to the top of `DIALOGUE.md`, *then* removed from the inbox (DIALOGUE first).
- **Don't create a round-trip for nothing.** If you can do or look up something yourself (open the file, read the repo, search the web), do it. `INBOX.md` is for what only the other surface can do or decide, or for work that must survive between sessions.
- The user can always paste text into either surface; that doesn't replace writing the item.

## DIALOGUE.md and rotation

Verbatim record of closed exchanges, newest first: `---` / `### <date closed> — #<n> <from> → <to> (opened <date>)` / **Запрос:** … / **Ответ:** … / `---`. **Never trimmed or summarised by anyone**; only the user may clear it, on explicit request. At ~200 exchanges or ~300 KB (whichever first), whoever just appended moves the *oldest whole block* unchanged into `DIALOGUE-archive-<year of that block>.md` (same rules) and leaves one line under the header: `_Archive older than <date> — see DIALOGUE-archive-<year>.md._`. Check after appending.

## LEDGER.md

Where `STATE.md` blocks land when they rotate out. Append-only. Older hand-written entries (labels Chat/Cowork/Code, `Resolved:` lines) stay as they are. Nobody deletes another surface's entries.

## Finding the project folder (Claude app)

Don't open with "type the path". (1) Check folders already connected. (2) If the project isn't there, list a plausible parent (home, Desktop, Documents, a sibling of a connected folder); a names-only listing works even without access — walk down a level at a time to the folder named like the project and confirm `.trio` is inside. (3) Request access to that exact path (one click for the user). (4) Ask the user to type or attach only if discovery found nothing. If a shell on the computer is available, use it for work inside connected folders. If the project is a GitHub repo, `add_repo` or a Project's synced source also works for reading.

If no file access exists at all (a bare Project conversation), say so, ask the user to attach `STATE.md` and `INBOX.md`, and don't pretend to know their content.

## Set up a new project

Create `.trio/` in the project root (substitute name and date): `STATE.md` (template above, `СЕЙЧАС` = "duo-sync connected"), `INBOX.md` (template above), `LEDGER.md` (`# <Project> — Ledger` + `_Newest first. Blocks rotated out of STATE.md._`), `DIALOGUE.md` (`# <Project> — Dialogue (verbatim, never trimmed)` + `_Newest first. Only appended to._`). Ask only what you can't find out: project name, where the folder is, and whether `.trio/` should be in git (some projects keep it out on purpose — check `.gitignore` first and respect it). Add a short block to the project's `CLAUDE.md` pointing at `.trio/` (see `NEW_PROJECT.md`). Tell the user in plain words what was created.

## Migrating a project from trio-sync v0.4

Follow `docs/migration-v0.5.md` in the duo-sync repo (`github.com/Myrshefor/.trio`). Never overwrite a project's old files before they are copied to `.trio/_legacy-v0.4/` and open items are carried into `STATE.md` / `INBOX.md`. Don't migrate while another session is actively writing to that `.trio/`.

## Weekly review (optional scheduled task)

Once a week a scheduled task reads each project's `.trio/` and reports, without changing anything but `INBOX.md`: `СЕЙЧАС` older than 14 days; items in `Открыто` older than 7 days; `DIALOGUE.md` near the rotation threshold; anything open that exists in `LEDGER.md` but not in `STATE.md`; hand-off content found in `CLAUDE.md`; a leftover v0.4 file in use next to a v0.5 `STATE.md`.

## Principles

- One channel you actually follow beats two you can't check. Don't add side channels (gists, chat-only notes): a finding that exists only in a conversation did not happen for the other side.
- State files describe the work; the files on disk are the work. If `STATE.md`/`LEDGER.md` disagree with what's actually on disk, trust the disk and fix the record. Two real incidents (an unlogged finding, a logged edit that never happened) are why.
- Don't conclude data is lost because you can't see it. Check directly first (a moved folder, an unconnected folder and a gitignored `.trio/` all look like "missing").
- A file absent from a synced repo is usually about where the folder sits relative to the git root, not a sync bug.
- The protocol needs only files — no services, databases or running processes.
- `STATE.md` rotation and `DIALOGUE.md` never-trim are what make "nothing lost" true. Don't shortcut them.
- If an old copy of the skill (`trio-sync`) is still installed next to this one, tell the user to disable it, or both will fire.
