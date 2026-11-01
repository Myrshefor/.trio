# trio-sync

Keeps Claude chat, Claude Code, and Cowork aware of each other's work on the same project — without any of them needing a shared "memory" that doesn't exist yet.

## The problem this solves

Claude chat, Claude Code, and Cowork don't share context or files with each other today. Each one starts from zero, even when they're all working on the same project. `trio-sync` works around that with a plain folder of markdown files, `.trio/`, that lives inside your project:

- **`STATUS.md`** — a short, current snapshot: what's being worked on, recent decisions, open questions, who owes what.
- **`LEDGER.md`** — a running history, newest first, of what each surface did (kept terse — a summary, not a transcript).
- **`TO_CODE.md`** — open asks/questions directed at Code, written by Cowork or Chat. Cleared once Code answers.
- **`FROM_CODE.md`** — open answers/reports directed back at Cowork/Chat, written by Code. Cleared once picked up.
- **`DIALOGUE.md`** — the permanent, word-for-word record of everything asked and answered through `TO_CODE.md`/`FROM_CODE.md`. Nothing ever trims this file automatically — it's the "hot memory" of the cross-surface conversation, and only clears if you explicitly ask for that.

Any of the three surfaces reads this folder before starting work and updates it after finishing something worth flagging. That's the whole mechanism — no external services, no database, nothing to keep running.

**One pitfall this version guards against:** `CLAUDE.md` (Claude Code's own persistent instructions file) is *not* part of this handoff. If Code answers a trio-sync question by writing into `CLAUDE.md` instead of `.trio/FROM_CODE.md`, the other surfaces will never see it — they don't read `CLAUDE.md` as part of this protocol. v0.3.0 adds an explicit warning about this to the skill after exactly this mix-up happened in practice.

## Install in Cowork

Open the `trio-sync.plugin` file sent alongside this one (or drag it into a Cowork chat) and accept the install prompt. Cowork will then have the `trio-sync` skill available in every task.

You can also manage it later from **Customize** in the Claude Desktop app sidebar, or from the skills settings on claude.ai.

## Install in Claude Code

This gets you the plugin on every project on this machine (not just one repo):

```
/plugin marketplace add "<path to the trio-sync-marketplace folder>"
/plugin install trio-sync@trio-sync-marketplace
/reload-plugins
```

Unzip `trio-sync-marketplace.zip` somewhere permanent first (not a temp folder), then use that path in the first command — for example:

```
/plugin marketplace add "C:\Users\Myrsheftor\claude-plugins\trio-sync-marketplace"
/plugin install trio-sync@trio-sync-marketplace
```

## Regular chat (not Cowork, not Code)

Plain chat conversations — in the Claude Desktop app or on claude.ai — don't automatically read files on your computer the way Cowork and Claude Code do, and that's a hard product limit, not a permissions setting — no amount of folder or device access changes it. Two ways to bridge that:

1. **Best option — Claude Projects (semi-automatic).** If the project has a Claude Project on claude.ai attached, Cowork mirrors `.trio/STATUS.md` into a doc there (`claude/trio-status-snapshot.md`) every time it does real trio-sync work — built into the skill since v0.2.0. As of v0.3.0, Cowork also mirrors `.trio/FROM_CODE.md` (`claude/trio-from-code-snapshot.md`) and new `.trio/DIALOGUE.md` entries (`claude/trio-dialogue-mirror.md`), so Chat can see what Code just reported and the full verbatim exchange history too. As long as you chat *inside* that Project (not a stray conversation), all of these mirrors are pulled into context automatically, no pasting needed. `STATUS.md`'s mirror only covers the current snapshot; the dialogue mirror is cumulative and never trimmed — see `.trio/LEDGER.md` on disk for the terse full history either way.
2. **As of v0.4.0, if `.trio/` lives in a public GitHub repo:** Cowork also mirrors `claude/trio-chat-links.md` — direct links to every protocol file and doc. Because it's mirrored the same way as the other Project docs, Chat gets it automatically inside the Project too, but it lets Chat pull the *live* current version of any file via a web fetch instead of whatever Cowork last copied into a snapshot. One real limitation: Chat can only follow a link that's already literally present in the conversation — it can't browse the repo tree or guess a file's URL from just the repo root. See `docs/architecture.md` §5.
3. **Always works, no setup required:** at the start of a chat, paste or attach `.trio/STATUS.md` (or the Project's mirror doc). At the end, ask Claude to write a short summary and paste it into `LEDGER.md` yourself (or hand it to Cowork/Claude Code next time and ask them to log it for you).

Chat's role here — thinking partner and sanity-checker — doesn't need deep file access to be useful; it mainly needs the gist, which either option above gives it.

## Using it day to day

You don't need to remember commands. Once installed:

- Starting work on a project that has a `.trio/` folder → the surface reads it automatically and briefly tells you what the others have been up to.
- Ask "catch me up" / "what's the status" / "what did Code do" any time.
- After something worth flagging happens, the surface updates the ledger and status on its own. You can also just say "log that" or "update the trio status" to force it.
- To turn this on for a new project: say "set up trio sync here" (or "connect the trio here") and the skill creates the `.trio/` folder for you — including the GitHub/`CHAT_LINKS.md` question from v0.4.0.

### Quick trigger phrases (v0.4.0)

Finalized so you don't have to re-explain context each time — say these to any surface, in Russian, roughly as written:

| Phrase (as you'd actually say it) | What happens |
|---|---|
| «настрой trio-sync здесь» | Creates `.trio/` on a new project, including the GitHub/`CHAT_LINKS.md` step |
| «какой статус» / «догони меня» | Any surface reads `STATUS.md` + the latest `LEDGER.md` entries and briefly recaps |
| «передай Code: <текст>» | The question goes into `TO_CODE.md` (Cowork writes it on your behalf, or you paste it into Code yourself) |
| «что ответил Code» / «покажи FROM_CODE» | Reads `FROM_CODE.md` and relays the answer |
| «подумай с Chat над: <текст>» (to Cowork) | Cowork drafts the question; you bring it to Chat yourself |
| «передай Cowork: <текст>» (to Chat) | Chat hands you a ready-made phrasing; you paste it into Cowork |
| «залогируй это» | Forces a `LEDGER.md`/`STATUS.md` write right now, instead of waiting for the usual trigger |
| «обнови ссылки» | Regenerates `CHAT_LINKS.md` / `claude/trio-chat-links.md` if new files have shown up in the repo |

## Files in this plugin

```
trio-sync/
├── .claude-plugin/plugin.json
├── skills/trio-sync/SKILL.md
└── README.md
```

No MCP servers, hooks, or external services — just a skill and a folder convention. That was a deliberate choice for reliability over cleverness: fancier options (a shared knowledge-graph MCP server like the open-source Kairn project, for example) exist if you want deeper automation later, but they add a moving part you have to keep running. Start here; upgrade only if this stops being enough.
