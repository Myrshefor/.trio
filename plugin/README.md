# duo-sync

Keeps the Claude app (chat/Cowork) and Claude Code aware of each other's work on the same project, through a plain folder of markdown files: `.trio/` inside the project.

## Files

- **`STATE.md`** — current state in three rotating slots (`СЕЙЧАС`, `ПРОШЛАЯ ЗАПИСЬ`, `ПОЗАПРОШЛАЯ ЗАПИСЬ`) plus standing requirements. A new entry pushes the others down; what falls out lands in `LEDGER.md`; anything still open is carried forward.
- **`INBOX.md`** — one inbox for asks and answers between Claude and Code, with addressee and status (`open` → `answered` → closed). Open items plus the last two closed; older ones move verbatim to `DIALOGUE.md`.
- **`DIALOGUE.md`** — verbatim, never-trimmed record of closed exchanges; rotates into `DIALOGUE-archive-<year>.md` at ~200 exchanges / ~300 KB.
- **`LEDGER.md`** — overflow from `STATE.md`, append-only.

No services, databases or hooks — a skill and a folder convention. `.trio/` keeps its old name so existing projects keep working.

⚠ `CLAUDE.md` is never a hand-off channel. Reports and answers go to `.trio/`.

## Install

**Claude app:** open the `.plugin` file (or drag it into a chat) and accept the prompt. Manage later in Customize.

**Claude Code** (all projects on the machine):

```
/plugin marketplace add "<path to the plugin folder>"
/plugin install duo-sync@duo-sync-marketplace
/reload-plugins
```

Disable any old `trio-sync` / `trio-sync-2` so both don't fire.

## Day to day

Start work on a project that has `.trio/` → the session reads `STATE.md` and `INBOX.md` and tells you in a few lines what the other side did. After meaningful work it updates `STATE.md` itself.

| Phrase | What happens |
|---|---|
| «настрой duo-sync здесь» | Creates `.trio/` on a new project |
| «какой статус» / «догони меня» | Reads `STATE.md` + `INBOX.md`, recaps |
| «передай Code: …» / «передай Claude: …» | New item in `INBOX.md` |
| «что ответил Code» | Reads `INBOX.md`, relays the answer |
| «залогируй это» | Updates `СЕЙЧАС` now, with rotation |
| «перенеси проект на duo-sync» | Follows `docs/migration-v0.5.md` |

## Files in this plugin

```
duo-sync/
├── .claude-plugin/plugin.json
├── skills/duo-sync/SKILL.md
└── README.md
```
