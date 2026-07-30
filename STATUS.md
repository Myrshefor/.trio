# trio — Status
_Living snapshot, not a history — see LEDGER.md for that. Last updated: 2026-07-30 by Cowork._

## Roles
- **Chat** — thinking partner, sounding board, sanity-checker
- **Code** — hands-on implementer
- **Cowork** — lead strategist, evaluates progress and plans ahead

## Current focus
trio-sync v0.4.0 (ротация `DIALOGUE.md`, конвенция «resolved», канал видимости для Chat через публичный GitHub — `CHAT_LINKS.md`, `docs/scenarios.md`, таблица фраз-триггеров) проверен Code и запушен в `github.com/Myrshefor/.trio`. Второй настоящий цикл TO_CODE/FROM_CODE/DIALOGUE состоялся.

## Recent decisions
- trio-sync v0.4.0: формализована ротация `DIALOGUE.md` (200 обменов/~300 КБ, что раньше) и конвенция «resolved» для TO_CODE/FROM_CODE; добавлен канал видимости для Chat через публичный GitHub (`CHAT_LINKS.md`); написан `docs/scenarios.md` (шесть сценариев, по черновику Chat `trio-scenarios-addendum-draft.md`); зафиксирована таблица фраз-триггеров. (chyits + Chat + Cowork, 2026-07-30)
- trio-sync v0.3.0: добавлены `TO_CODE.md`/`FROM_CODE.md` (направленные инбоксы вместо текстового поля `Handoff:`) и `DIALOGUE.md` (permanent verbatim «хот-память», чистится только по явной просьбе пользователя). (Cowork, 2026-07-30)
- trio-sync выделен из проекта Finanon в собственную среду разработки — этот репозиторий. Finanon получает свой отдельный, независимый `.trio/` внутри своего настоящего git-репозитория (`Разработка Finanon\Finanon\.trio\`), не пересекающийся с историей trio. (chyits + Cowork, 2026-07-30)
- История протокола до 2026-07-30 (v0.1 → v0.2.0 → v0.3.0) — общая для этого репозитория и для Finanon, потому что физически создавалась в одном месте до разделения. Подробный разбор — `docs/history-and-incidents.md`.
- chyits хочет trio-sync как постоянную привычку на все свои будущие проекты, не только на Finanon. (chyits, зафиксировано в персональной памяти Claude)

## Open questions
- Инцидент с временным откатом верхнеуровневого `CLAUDE.md` (см. `LEDGER.md`, запись Code от 2026-07-30) — причина не подтверждена технически; следить, повторится ли.

## Standing requirements
_Держать в фокусе на любом этапе:_
- Протокол не должен требовать внешних сервисов — только файлы.
- `CLAUDE.md` (или любой аналог) — никогда не канал хендоффа, только `.trio/*.md`.
- `DIALOGUE.md` — никогда не тримится никем, кроме явной просьбы пользователя (ротация в архив — не тримминг, см. `docs/architecture.md` §2).

## Next actions
- [x] `git init` + первый push в `github.com/Myrshefor/.trio` — done 2026-07-30 by Code (репозиторий на GitHub уже существовал с авто-README, перезаписан force-push'ем с согласия chyits)
- [x] Первый настоящий цикл `TO_CODE.md` → `FROM_CODE.md` → `DIALOGUE.md` — done 2026-07-30 by Code
- [x] Решить два открытых вопроса (лимит DIALOGUE.md, конвенция resolved) — done 2026-07-30 by Cowork, см. `docs/architecture.md` §2
- [x] Code: проверить, закоммитить и запушить пакет v0.4.0 — done 2026-07-30 by Code
- [ ] Следить за инцидентом с откатом верхнеуровневого CLAUDE.md — не повторится ли
