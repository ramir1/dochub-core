---
type: doc
lang: ru
status: draft
updated: 2026-11-06
---

# Схема конфигурации хаба

`.doc-hub/config.yaml` в корне хаба, хранится в git, меняет администратор хаба. Спецификации: [projects](../specifications/projects.ru.md), [access](../specifications/access.ru.md).

## Поля

| Поле | Тип | Обяз. | Описание |
|---|---|:-:|---|
| `format_version` | целое | да | Сейчас `1`. Единственная версия формата, см. [versioning](../specifications/versioning.ru.md) |
| `hub.id` | строка | да | Идентификатор хаба |
| `hub.name` | строка | да | Название |
| `agents` | словарь | да | Ключ — имя агента |
| `agents.<имя>.description` | строка | да | Назначение: зона, задача |
| `agents.<имя>.person` | строка | нет | Человек, за которым агент закреплён |
| `agents.<имя>.tool` | строка | нет | Инструмент: `claude-code`, `codex`, `human` и т. п. |
| `projects` | словарь | да | Ключ — идентификатор проекта |
| `projects.<id>.name` | строка | да | Название |
| `projects.<id>.docs.<папка>` | словарь | нет | Подключённая внешняя документация, ключ — папка `projects/<id>/docs/<папка>/` |
| `projects.<id>.docs.<папка>.repository` | строка | нет | URL git-репозитория; без него источник — локальная папка |
| `projects.<id>.docs.<папка>.branch` | строка | нет | Ветка, по умолчанию `main` |
| `projects.<id>.docs.<папка>.path` | строка | нет | Что подключить, от корня проекта или клона; без него — весь корень |
| `projects.<id>.owners` | массив строк | да | Владельцы по умолчанию |
| `projects.<id>.areas` | словарь | нет | Ключ — путь зоны |
| `projects.<id>.areas.<путь>.owners` | массив строк | да | Владельцы зоны |
| `projects.<id>.members` | словарь | да | Ключ — имя агента |
| `projects.<id>.members.<агент>.role` | `participant` \| `reader` | да | Роль |
| `projects.<id>.members.<агент>.paths` | массив строк | нет | Ограничение доступа путями |
| `projects.<id>.features.<ключ>.enabled` | логическое | да | `discussions`, `changes`, `claims` |

Идентификаторы и имена: `^[a-z0-9][a-z0-9-]*$`. Пути — без ведущего и завершающего `/`. Владельцы должны быть `participant` проекта.

## Пример

```yaml
format_version: 1

hub:
  id: main
  name: Основной хаб

agents:
  ramir:
    description: Человек, администратор хаба
    person: ramir
    tool: human
  ramir-claude-backend:
    description: Backend портала, API и его документация
    person: ramir
    tool: claude-code
  ramir-codex-frontend:
    description: Frontend портала
    person: ramir
    tool: codex
  mobile-dev:
    description: Агент репозитория мобильного приложения
    tool: claude-code

projects:
  portal:
    name: Портал
    docs:
      tech:
        repository: git@github.com:example/portal.git
        branch: main
        path: docs
    owners: [ramir]
    areas:
      wiki/api:        { owners: [ramir-claude-backend] }
      wiki/ui:         { owners: [ramir-codex-frontend] }
      wiki/mobile-app: { owners: [ramir-claude-backend] }
    members:
      ramir:                { role: participant }
      ramir-claude-backend: { role: participant }
      ramir-codex-frontend: { role: participant }
      mobile-dev:           { role: participant, paths: [wiki/mobile-app] }
    features:
      discussions: { enabled: true }
      changes:     { enabled: true }
      claims:      { enabled: true }
```

Адрес репозитория в примере условный.

## Локальная конфигурация

`.doc-hub/config.local.yaml` — настройки машины, **не в git**.

| Поле | Тип | Описание |
|---|---|---|
| `projects.<id>.docs.<папка>.local_path` | строка | Корень на этой машине: корень проекта для локальной папки, клон для репозитория; `~` раскрывается |

```yaml
projects:
  portal:
    docs:
      tech:
        local_path: ~/projects/portal.loc/www
```

Локальная конфигурация не может менять роли, зоны, функции и пути документации.
