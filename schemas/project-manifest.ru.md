---
type: doc
lang: ru
status: draft
updated: 2026-11-06
---

# Схема манифеста `.doc-hub.yaml`

Спецификация — [project-manifest](../specifications/project-manifest.ru.md).

## Поля

| Поле | Тип | Обяз. | Описание |
|---|---|:-:|---|
| `hubs` | словарь | да | Разделы по хабам, ключ — `hub.id`. Первый раздел — хаб по умолчанию |
| `hubs.<id>.path` | строка | одно из двух | Путь к локальному клону хаба; `~` раскрывается; переопределяется `DOC_HUB_PATH` |
| `hubs.<id>.url` | строка | одно из двух | Адрес сервиса; смысл определяет реализация уровня 3 |
| `hubs.<id>.projects` | массив строк | да | Проекты хаба, которые и есть этот репозиторий (может быть пустым) |
| `hubs.<id>.agent` | строка | нет | Имя агента по умолчанию, см. [access](../specifications/access.ru.md#идентичность) |
| `hubs.<id>.access` | массив строк | нет | Другие проекты хаба, с которыми работают агенты этого репозитория; подсказка агентам, а не выдача прав |

Формат файла — YAML. Прав и секретов в манифесте нет. Файл свой на каждой машине и не хранится в git.

## JSON Schema

Схема проверяет содержимое файла после разбора YAML.

```json
{
  "$schema": "https://json-schema.org/draft/2020-12/schema",
  "title": "DocHub project manifest",
  "type": "object",
  "required": ["hubs"],
  "properties": {
    "hubs": {
      "type": "object",
      "minProperties": 1,
      "propertyNames": { "pattern": "^[a-z0-9][a-z0-9-]*$" },
      "additionalProperties": {
        "type": "object",
        "required": ["projects"],
        "properties": {
          "path":     { "type": "string" },
          "url":      { "type": "string" },
          "projects": { "type": "array", "items": { "type": "string", "pattern": "^[a-z0-9][a-z0-9-]*$" }, "uniqueItems": true },
          "agent":    { "type": "string", "pattern": "^[a-z0-9][a-z0-9-]*$" },
          "access":   { "type": "array", "items": { "type": "string", "pattern": "^[a-z0-9][a-z0-9-]*$" }, "uniqueItems": true }
        },
        "anyOf": [ { "required": ["path"] }, { "required": ["url"] } ]
      }
    }
  },
  "additionalProperties": true
}
```
