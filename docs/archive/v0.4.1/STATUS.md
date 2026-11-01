# trio — Status
_Living snapshot, not a history — see LEDGER.md for that. Last updated: 2026-07-30 by Cowork._

## Roles
- **Chat** — thinking partner, sounding board, sanity-checker
- **Code** — hands-on implementer
- **Cowork** — lead strategist, evaluates progress and plans ahead

## Current focus
trio-sync v0.4.1 проверен Code и запушен в `github.com/Myrshefor/.trio`: `NEW_PROJECT.md` (самодостаточный бутстрап для новых проектов), исправлен устаревший раздел `HANDOFF.md`, Cowork теперь сначала ищет папку проекта на устройстве сам (`device_list_dir`/`device_request_folder_access`), а не сразу просит пользователя ввести путь текстом. chyits всё ещё нужно переустановить обновлённый плагин локально (пакет доставлен в чат) — это единственное, что осталось из этого батча.

## Recent decisions
- trio-sync v0.4.1: Cowork теперь ищет папку проекта на устройстве через `device_list_dir`/`device_request_folder_access` (skeleton-листинг вероятных родителей по имени проекта) вместо того, чтобы сразу просить пользователя ввести путь текстом. (chyits + Cowork, 2026-07-30)
- trio-sync v0.4.0: формализована ротация `DIALOGUE.md` (200 обменов/~300 КБ, что раньше) и конвенция «resolved» для TO_CODE/FROM_CODE; добавлен канал видимости для Chat через публичный GitHub (`CHAT_LINKS.md`); написан `docs/scenarios.md` (шесть сценариев, по черновику Chat `trio-scenarios-addendum-draft.md`); зафиксирована таблица фраз-триггеров. (chyits + Chat + Cowork, 2026-07-30)
- trio-sync v0.3.0: добавлены `TO_CODE.md`/`FROM_CODE.md` (направленные инбоксы вместо текстового поля `Handoff:`) и `DIALOGUE.md` (permanent verbatim «хот-память», чистится только по явной просьбе пользователя). (Cowork, 2026-07-30)
- trio-sync выделен из проекта Finanon в собственную среду разработки — этот репозиторий. Finanon получает свой отдельный, независимый `.trio/` внутри своего настоящего git-репозитория (`Разработка Finanon\Finanon\.trio\`), не пересекающийся с историей trio. (chyits + Cowork, 2026-07-30)
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
- [x] Code: закоммитить и запушить правку `HANDOFF.md` + `NEW_PROJECT.md` + v0.4.1 (device-bridge discovery) — done 2026-07-30 by Code
- [ ] chyits: переустановить trio-sync **v0.4.1** в Cowork и Claude Code — пакет (`trio-sync-v0.4.1.plugin` / `trio-sync-v0.4.1-marketplace.zip`) отправлен Cowork в чат; первая версия не ставилась («description must be at most 500 characters», 518 символов) — пересобран с сокращённым `description` (446 символов), переотправлен; без переустановки установленные копии остаются на v0.3.0 и новое поведение не применяется
- [ ] Следить за инцидентом с откатом верхнеуровневого CLAUDE.md — не повторится ли (по своей природе не закрывается «сейчас», нужен только новый факт наблюдения)
