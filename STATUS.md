# trio — Status
_Living snapshot, not a history — see LEDGER.md for that. Last updated: 2026-07-30 by Cowork._

## Roles
- **Chat** — thinking partner, sounding board, sanity-checker
- **Code** — hands-on implementer
- **Cowork** — lead strategist, evaluates progress and plans ahead

## Current focus
Только что выделили trio-sync в собственный проект/репозиторий (`github.com/Myrshefor/.trio`), отдельно от Finanon, где протокол изначально родился. Задача этого этапа: убедиться, что репозиторий реально создан и запушен на GitHub, что весь контекст передан (см. `HANDOFF.md`, `docs/`), и что дальнейшая разработка самого trio-sync (не Finanon) ведётся здесь, а не смешивается снова с чьим-то ещё проектом.

## Recent decisions
- trio-sync выделен из проекта Finanon в собственную среду разработки — этот репозиторий. Finanon получает свой отдельный, независимый `.trio/` внутри своего настоящего git-репозитория (`Разработка Finanon\Finanon\.trio\`), не пересекающийся с историей trio. (chyits + Cowork, 2026-07-30)
- История протокола до 2026-07-30 (v0.1 → v0.2.0 → v0.3.0) — общая для этого репозитория и для Finanon, потому что физически создавалась в одном месте до разделения. Полная копия сохранена в `LEDGER.md` обоих проектов как «общая предыстория» — дальше истории расходятся. Подробный разбор — `docs/history-and-incidents.md`.
- trio-sync v0.3.0: добавлены `TO_CODE.md`/`FROM_CODE.md` (направленные инбоксы вместо текстового поля `Handoff:`) и `DIALOGUE.md` (permanent verbatim «хот-память», чистится только по явной просьбе пользователя). (Cowork, 2026-07-30)
- chyits хочет trio-sync как постоянную привычку на все свои будущие проекты, не только на Finanon — это одна из причин, почему у самого протокола теперь есть собственный дом. (chyits, зафиксировано в персональной памяти Claude)

## Open questions
- Репозиторий `github.com/Myrshefor/.trio` создан на GitHub и запушен? (нужно действие пользователя/Code — у Cowork нет прав писать в его GitHub)
- Нужен ли лимит объёма для `DIALOGUE.md` (архивировать раз в N записей), или он реально бесконечный? Открыто с этапа проектирования v0.3.0.
- Нужна ли более строгая конвенция «resolved» для `TO_CODE.md`/`FROM_CODE.md`, кроме простого «убрать запись, когда обработано»?
- Локальный плагин trio-sync в Claude Code на машине chyits — обновлён ли до v0.3.0? (тот же вопрос стоит и в Finanon's `.trio/TO_CODE.md`)

## Standing requirements
_Держать в фокусе на любом этапе:_
- Протокол не должен требовать внешних сервисов — только файлы.
- `CLAUDE.md` (или любой аналог) — никогда не канал хендоффа, только `.trio/*.md`.
- `DIALOGUE.md` — никогда не тримится никем, кроме явной просьбы пользователя.

## Next actions
- [ ] `git init` + первый push в `github.com/Myrshefor/.trio` — owner: chyits/Code (нужен реальный доступ к GitHub-аккаунту)
- [ ] Обновить локальный плагин trio-sync в Claude Code до v0.3.0 — owner: chyits/Code
- [ ] Первый настоящий цикл `TO_CODE.md` → `FROM_CODE.md` → `DIALOGUE.md` — попробовать вживую хотя бы раз (пока нигде не проверено на реальном обмене)
- [ ] Решить два открытых вопроса выше (лимит DIALOGUE.md, конвенция resolved)
