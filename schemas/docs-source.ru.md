---
type: doc
lang: ru
status: draft
updated: 2026-11-06
---

# Схема паспорта внешней документации

Спецификация — [docs-sources](../specifications/docs-sources.ru.md).

Файл `projects/<id>/docs/<папка>/.source.yaml`. Пишется при `doc-hub docs sync`, в git хаба не попадает.

| Поле | Тип | Обяз. | Описание |
|---|---|:-:|---|
| `project` | строка | да | Идентификатор проекта |
| `folder` | строка | да | Имя папки источника |
| `repository` | строка | да | Адрес репозитория |
| `branch` | строка | да | Ветка источника |
| `ref` | строка | да | Ссылка, с которой сделана выгрузка: ветка или значение `--ref` |
| `commit` | строка | да | Коммит выгрузки (полный SHA) |
| `path` | строка | да | Что подключено из репозитория |
| `paths` | массив строк | да | Выгруженные виртуальные пути; уже всей папки, если выгружал агент с ограничением |
| `synced_at` | дата-время | да | Когда выгружено, ISO 8601 в UTC |
| `synced_by` | строка | да | Имя агента |

```yaml
project: portal
folder: tech
repository: git@example.com:portal.git
branch: master
ref: master
commit: 4f2a9c1d8e7b6a5f4e3d2c1b0a9f8e7d6c5b4a39
path: docs
paths: [docs/tech]
synced_at: 2026-11-06T09:30:00Z
synced_by: ramir
```

Имя файла начинается с точки, поэтому в Obsidian паспорт не виден.
