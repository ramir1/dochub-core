---
type: doc
lang: ru
status: draft
updated: 2026-11-06
---

# Схема манифеста `.doc-hub.json`

> **Статус: предложение.** Спецификация — [project-manifest](../specifications/project-manifest.ru.md).

## Поля

| Поле | Тип | Обяз. | Описание |
|---|---|:-:|---|
| `hub.id` | строка | да | Идентификатор хаба |
| `hub.path` | строка | одно из двух | Путь к локальному клону хаба; `~` раскрывается; переопределяется `DOC_HUB_PATH` |
| `hub.url` | строка | одно из двух | Адрес сервиса; смысл определяет реализация уровня 3 |
| `projects` | массив строк | да | Проекты хаба, которые и есть этот репозиторий (может быть пустым) |
| `access` | массив строк | нет | Другие проекты хаба, с которыми работают агенты этого репозитория; подсказка агентам, а не выдача прав |

Имени агента, прав и секретов в манифесте нет.

## JSON Schema

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "DocHub project manifest",
  "type": "object",
  "required": ["hub", "projects"],
  "properties": {
    "hub": {
      "type": "object",
      "required": ["id"],
      "properties": {
        "id":   { "type": "string", "pattern": "^[a-z0-9][a-z0-9-]*$" },
        "path": { "type": "string" },
        "url":  { "type": "string" }
      },
      "anyOf": [ { "required": ["path"] }, { "required": ["url"] } ]
    },
    "projects": { "type": "array", "items": { "type": "string", "pattern": "^[a-z0-9][a-z0-9-]*$" }, "uniqueItems": true },
    "access":   { "type": "array", "items": { "type": "string", "pattern": "^[a-z0-9][a-z0-9-]*$" }, "uniqueItems": true }
  },
  "additionalProperties": true
}
```
