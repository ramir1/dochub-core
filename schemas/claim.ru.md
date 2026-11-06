---
type: doc
lang: ru
status: draft
updated: 2026-11-06
---

# Схема отметки работы

Спецификация — [claims](../specifications/features/claims.ru.md). Шаблон — [claim](../templates/claim.ru.md).

Файл `projects/<id>/claims/<агент>-<slug>.md`, пишет только сам агент; истёкшую отметку может удалить любой участник.

## Frontmatter

| Поле | Тип | Обяз. | Описание |
|---|---|:-:|---|
| `type` | `claim` | да | |
| `project` | строка | да | Идентификатор проекта |
| `agent` | строка | да | Кто работает; совпадает с началом имени файла |
| `paths` | массив строк | да | Пути от корня репозитория проекта |
| `topic` | строка | да | Что делается |
| `created` | дата-время | да | ISO 8601 в UTC |
| `expires` | дата-время | да | ISO 8601 в UTC |
| `discussion` | строка | нет | Папка или `uid` связанного обсуждения |

Тело — по желанию: план работы в нескольких строках.

## Пример

`projects/portal/claims/ramir-claude-backend-auth-docs.md`:

```markdown
---
type: claim
project: portal
agent: ramir-claude-backend
paths: [wiki/api/auth.md, wiki/mobile-app/03-authorization.md]
topic: Описываю обновление refresh-токенов
created: 2026-11-06T09:00:00Z
expires: 2026-11-06T13:00:00Z
---
```

Формат Markdown выбран, чтобы отметки были видны людям в Obsidian.
