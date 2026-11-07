---
type: doc
lang: ru
status: draft
updated: 2026-11-07
---

# Схема карточки

Спецификация — [board](../specifications/features/board.ru.md). Шаблон — [card](../templates/card.ru.md).

Файл `projects/<id>/board/<slug>.md`. Если имя занято, к `slug` добавляется `-2`, `-3` и так далее.

## Frontmatter

| Поле | Тип | Обяз. | Описание |
|---|---|:-:|---|
| `uid` | строка | да | ULID |
| `type` | `card` | да | |
| `project` | строка | да | Идентификатор проекта |
| `title` | строка | да | Что сделать, одной строкой |
| `author` | строка | да | Кто создал |
| `assignee` | строка | нет | Исполнитель; обязателен для `doing` |
| `status` | `backlog` \| `todo` \| `doing` \| `done` \| `closed` | да | Колонка |
| `priority` | `low` \| `normal` \| `high` | да | Приоритет |
| `due` | дата | нет | Срок, `YYYY-MM-DD` |
| `refs` | массив строк | нет | Документы: `проект:путь` |
| `discussion` | строка | нет | Имя файла без расширения или `uid` связанного обсуждения |
| `outcome` | строка | для `done`, `closed` | Итог в 1–3 предложения |
| `created` | дата-время | да | ISO 8601 в UTC |

## Тело

Описание задачи в Markdown, затем раздел `## History`. В истории по одной метке `[время агент → статус]` на смену статуса, метки разделены пустой строкой. Заголовок раздела одинаков на всех языках: по нему реализация находит историю.

## Пример

`projects/portal/board/refresh-token-docs.md`:

```markdown
---
uid: 01KA2Y7Q8Z3M4N5P6R7S8T9V0W
type: card
project: portal
title: Описать обновление refresh-токенов
author: ramir
assignee: ramir-claude-backend
status: doing
priority: high
due: 2026-11-14
refs: [portal:wiki/api/auth.md]
discussion:
outcome:
created: 2026-11-07T09:00:00Z
---

Описать срок жизни токена и ошибки обновления.

## History

[2026-11-07T09:00:00Z ramir → todo]

[2026-11-07T10:30:00Z ramir-claude-backend → doing]
```
