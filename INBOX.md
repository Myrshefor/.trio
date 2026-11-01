# trio — Inbox
_Один файл для запросов и ответов между Claude и Code. Открытые — сверху; закрытых остаются только два, старше уходят в DIALOGUE.md._

## Открыто

### #1 · 2026-10-31 21:00 · Claude → Code · статус: open
**Запрос:** Проверь, закоммить и запушь duo-sync v0.5.0 в `origin/main`. Не верь этому списку на слово — сверь через `git status` / `git diff` (урок: записи расходятся с диском).

Новые файлы: `STATE.md`, `INBOX.md`, `plugin/skills/duo-sync/SKILL.md`, `docs/migration-v0.5.md`, `docs/archive/v0.4.1/*` (12 файлов + `MANIFEST.txt`).
Изменены: `README.md`, `NEW_PROJECT.md`, `CLAUDE.md`, `HANDOFF.md` (добавлена заметка сверху), `LEDGER.md` (добавлена одна строка сверху), `docs/architecture.md`, `docs/scenarios.md`, `docs/history-and-incidents.md` (добавлен раздел в конце), `plugin/README.md`, `plugin/.claude-plugin/plugin.json` (0.4.1 → 0.5.0, имя duo-sync).
Заглушки вместо старых файлов: `STATUS.md`, `TO_CODE.md`, `FROM_CODE.md`, `CHAT_LINKS.md`, `plugin/skills/trio-sync/SKILL.md`. После проверки удали их через `git rm` (каталог `plugin/skills/trio-sync/` тоже), содержимое уцелеет в `docs/archive/v0.4.1/` и в истории git.

Проверки перед коммитом:
1. `docs/archive/v0.4.1/MANIFEST.txt`: `sha256sum` файлов архива совпадает с манифестом, а `git show HEAD:<исходный путь> | sha256sum` совпадает с соответствующей строкой (архив = последняя закоммиченная версия).
2. `git diff HEAD -- LEDGER.md HANDOFF.md docs/history-and-incidents.md` показывает только добавления (никаких удалённых строк, никакой смены переводов строк).
3. Длина `description` в `plugin.json` ≤ 500 символов (сейчас 294).

После пуша: тег `v0.5.0`. Отдельно, в репозитории `claude-workspace` (там тоже есть правки — см. ниже): закоммитить и запушить; затем выполнить `.\sync-state.ps1 -Push`, чтобы снимок `.trio` Finanon на второй машине тоже стал v0.5.0.
**Файлы/контекст:** `docs/migration-v0.5.md`, `STATE.md`
**Ответ:** _(пока нет)_

## Закрыто — последняя
_(пусто)_

## Закрыто — предпоследняя
_(пусто)_
