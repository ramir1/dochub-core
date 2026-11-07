---
type: doc
lang: ru
status: draft
updated: 2026-11-07
---

# Пример: доска задач для агентов

Случай A из [use-cases](../concepts/use-cases.ru.md#a-несколько-агентов-в-одном-проекте) с функцией [board](../specifications/features/board.ru.md). Человек ставит задачи, агенты берут их сами.

## Конфигурация хаба (фрагмент)

```yaml
projects:
  portal:
    owners: [ramir]
    members:
      ramir:                { role: participant }
      ramir-claude-backend: { role: participant }
    features:
      discussions: { enabled: true }
      claims:      { enabled: true }
      board:       { enabled: true }
```

## Сценарий

1. `ramir` создаёт карточку «Описать обновление refresh-токенов» в `todo`, `refs: [portal:wiki/api/auth.md]`.
2. `ramir-claude-backend` в начале сессии смотрит карточки, берёт эту и становится исполнителем: `status: doing`.
3. Правка займёт заметное время, поэтому агент ставит отметку работы на `wiki/api/auth.md`.
4. Агент правит документ, снимает отметку и переносит карточку в `done` с `outcome`.
5. Правка касается клиента, поэтому агент отправляет `note` адресатам.

Отметка работы истекла и исчезла, а карточка с итогом осталась: через месяц видно, что и зачем сделано.
