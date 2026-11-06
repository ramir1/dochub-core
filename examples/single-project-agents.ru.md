---
type: doc
lang: ru
status: draft
updated: 2026-11-06
---

# Пример: несколько агентов в одном проекте

Случай A из [use-cases](../concepts/use-cases.ru.md#a-несколько-агентов-в-одном-проекте). Проект `portal`: своя документация в wiki, техническая документация репозитория подключена как `docs/tech`.

## Участники

| Агент | Инструмент | Машина | Зона |
|---|---|---|---|
| `ramir-claude-backend` | Claude Code | ноутбук | `wiki/api` (владелец) |
| `ramir-codex-frontend` | Codex | ноутбук, тот же репозиторий | `wiki/ui` (владелец) |
| `ramir-claude-tests` | Claude Code | сервер разработки | — (участник) |
| `ramir` | человек, Obsidian | ноутбук | владелец проекта по умолчанию |

Конфигурация хаба — как в примере [hub-config](../schemas/hub-config.ru.md#пример), плюс агент `ramir-claude-tests` (`role: participant`).

## Настройка

**Репозиторий портала** — `.doc-hub.json`:

```json
{
  "hub": { "id": "main", "path": "~/projects/hub" },
  "projects": ["portal"],
  "agent": "ramir-claude-backend"
}
```

**Имя агента** по умолчанию — из манифеста: `ramir-claude-backend` для сессии Claude Code на backend. У Codex другие права, поэтому его имя задано полностью: `DOC_HUB_AGENT=ramir-codex-frontend`. Манифест локальный, на сервере разработки свой. Оба работают в одном репозитории, но в хабе это разные агенты.

**Хаб** — клон на ноутбуке и на сервере разработки, общий origin. `config.local.yaml` на каждой машине — свой путь к клону портала. Выгрузку `docs/tech` каждая машина обновляет сама; `ramir` читает всё в Obsidian.

## Сценарий

1. **Backend меняет формат ошибок.** `ramir-claude-backend` проверяет отметки работы на `wiki/api/errors.md` (пусто), ставит свою, меняет API в репозитории и документ в wiki.
2. **Уведомление.** Создаёт обсуждение `note` «Новый формат ошибок API» с `to: [ramir-codex-frontend]` и `refs: [portal:wiki/api/errors.md]`. Снимает отметку работы. Пушит хаб. Файлы — как в примере [schemas/discussion](../schemas/discussion.ru.md#пример).
3. **Frontend узнаёт.** Новая сессия Codex начинается с проверки входящих: видит `note`, читает `wiki/api/errors.md`, обновляет клиент, закрывает обсуждение с итогом.
4. **Тесты на другой машине.** `ramir-claude-tests` начинает сессию на сервере: `git pull` хаба, проверка изменений — видит изменение `wiki/api/errors.md`; по `refs` находит обсуждение и итог, понимает, почему изменилось, и обновляет тесты.
5. **Через месяц** агент, впервые работающий с `wiki/api/errors.md`, ищет обсуждения по этому документу и находит итог — без вопросов человеку.

## Вопрос другой зоне

`ramir-claude-tests` считает, что в `wiki/ui/forms.md` описано не то поведение. Это зона `ramir-codex-frontend`, поэтому тесты не правят документ, а создают `proposal` с `refs: [portal:wiki/ui/forms.md]`. Владелец зоны принимает, правит документ в wiki и отмечает `applied` с коммитом хаба.
