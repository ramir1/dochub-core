---
type: doc
lang: ru
status: draft
updated: 2026-11-07
---

# Функции хаба

**Функция** — возможность хаба, которую можно подключить к проекту. Без функций у проекта есть только документация: wiki и docs (случай C, см. [use-cases](use-cases.ru.md)).

## Общие свойства

- У функции есть спецификация в [`specifications/features/`](../specifications/features/) и схема файлов в [`schemas/`](../schemas/).
- Включается для проекта в конфигурации хаба: `projects.<id>.features.<ключ>`, см. [hub-config](../schemas/hub-config.ru.md).
- Данные — в хабе: `projects/<id>/<ключ>/`. В репозиторий проекта функции ничего не пишут.
- Работа — по уровню: через реализацию или по скилу; люди — в Obsidian.
- Права — по ролям и владельцам зон, см. [access](../specifications/access.ru.md).

## Список

| Функция | Ключ | Что даёт | Спецификация |
|---|---|---|---|
| Обсуждения | `discussions` | Канал и общая память: вопросы, предложения, уведомления, итоги | [discussions](../specifications/features/discussions.ru.md) |
| Изменения | `changes` | Что изменилось в документации с последнего просмотра агента | [changes](../specifications/features/changes.ru.md) |
| Отметки работы | `claims` | Кто над чем сейчас работает | [claims](../specifications/features/claims.ru.md) |
| Доска | `board` | Задачи проекта: что сделать и кто взялся. Необязательна, включается по выбору | [board](../specifications/features/board.ru.md) |

Новые функции добавляются, когда появляется реальный случай использования.
