---
type: doc
lang: ru
status: draft
updated: 2026-11-07
---

# DocHub Core

**DocHub** — единое место документации проектов и слой координации ИИ-агентов (и людей) вокруг неё. У проекта есть своя документация — **wiki**, для которой хаб источник истины, и подключённая внешняя — **docs**, только для чтения. Вокруг них хаб даёт агентам общий канал (обсуждения), общую память (итоги, привязанные к документам), информирование об изменениях и отметки «кто над чем работает».

**Core** — этот репозиторий: концепция, протокол, правила и скил. Кода здесь нет: реализации (CLI, Web, свои инструменты) работают по протоколу и могут быть переписаны.

> **Статус:** черновик; первая версия CLI есть в репозитории `cli`. **«Предложение»** — не согласовано окончательно; **«открытый вопрос»** — не решено.

## С чего начать

1. [Обзор](concepts/overview.ru.md) — суть, принципы, этапы.
2. [Случаи использования](concepts/use-cases.ru.md) — для чего это нужно на практике.
3. [Уровни работы](concepts/levels.ru.md) — концепция, протокол, реализации.
4. [Архитектура](concepts/architecture.ru.md) — из чего состоит DocHub.
5. [Подключение](rules/integration.ru.md) — как создать хаб и подключить проект.
6. [Правила для агентов](rules/agents.ru.md) и [работа через Obsidian](rules/obsidian.ru.md) — как пользоваться хабом сейчас.

## Карта файлов

[AGENTS](AGENTS.ru.md) — инструкции для агентов, которые редактируют сам Core.

### concepts/ — понятия

| Файл | О чём |
|---|---|
| [overview](concepts/overview.ru.md) | Суть, принципы, этапы, открытые вопросы |
| [use-cases](concepts/use-cases.ru.md) | Несколько агентов в одном проекте; межпроектный контракт; чтение чужой документации |
| [levels](concepts/levels.ru.md) | Слои, уровни работы, хранение хаба |
| [architecture](concepts/architecture.ru.md) | Репозитории, хаб, реализации, агенты, синхронизация |
| [features](concepts/features.ru.md) | Функции хаба: обсуждения, изменения, отметки работы, доска |
| [glossary](concepts/glossary.ru.md) | Термины |

### specifications/ — спецификации

| Файл | О чём |
|---|---|
| [protocol](specifications/protocol.ru.md) | Что входит в протокол, что обязательно для реализации |
| [projects](specifications/projects.ru.md) | Проект, wiki и docs, зоны и владельцы |
| [access](specifications/access.ru.md) | Агенты, идентичность, роли, ограничение путями |
| [project-manifest](specifications/project-manifest.ru.md) | Файл `.doc-hub.yaml` в репозитории |
| [hub-storage](specifications/hub-storage.ru.md) | Хранилище хаба и синхронизация между машинами |
| [wiki](specifications/wiki.ru.md) | Своя документация проекта |
| [docs-sources](specifications/docs-sources.ru.md) | Внешняя документация, только чтение |
| [documents](specifications/documents.ru.md) | Идентификация документов и ссылки |
| [versioning](specifications/versioning.ru.md) | Версии форматов |
| [cli-contract](specifications/cli-contract.ru.md) | Команды эталонной реализации `doc-hub`, не часть протокола |
| [features/discussions](specifications/features/discussions.ru.md) | Обсуждения: вопросы, предложения, уведомления |
| [features/changes](specifications/features/changes.ru.md) | Изменения документации с последнего просмотра |
| [features/claims](specifications/features/claims.ru.md) | Отметки «работаю над …» |
| [features/board](specifications/features/board.ru.md) | Доска задач: карточки, статусы, исполнители |

### schemas/ — форматы

| Файл | О чём |
|---|---|
| [hub-config](schemas/hub-config.ru.md) | `.doc-hub/config.yaml` и `config.local.yaml` |
| [project-manifest](schemas/project-manifest.ru.md) | `.doc-hub.yaml` |
| [discussion](schemas/discussion.ru.md) | Файл обсуждения и метки сообщений |
| [changes](schemas/changes.ru.md) | Отметка просмотра изменений |
| [claim](schemas/claim.ru.md) | Отметка работы |
| [card](schemas/card.ru.md) | Карточка доски |
| [docs-source](schemas/docs-source.ru.md) | Паспорт внешней документации |

### rules/ — правила

| Файл | О чём |
|---|---|
| [integration](rules/integration.ru.md) | Создание хаба и подключение проекта |
| [agents](rules/agents.ru.md) | Порядок работы ИИ-агента с хабом |
| [obsidian](rules/obsidian.ru.md) | Настройка и работа через Obsidian |
| [default-rules](rules/default-rules.ru.md) | Требования к wiki |
| [naming](rules/naming.ru.md) | Имена файлов, папок, агентов |
| [writing](rules/writing.ru.md) | Стиль и достоверность |
| [translation](rules/translation.ru.md) | Перевод ru → en |

### skills/ — скилы

| Файл | О чём |
|---|---|
| [dochub](skills/dochub/SKILL.md) | Работа агента с файлами хаба без утилиты (уровень 1) |

### examples/ и templates/

- Примеры: [несколько агентов в одном проекте](examples/single-project-agents.ru.md), [межпроектный контракт](examples/contract-zone.ru.md), [доска задач для агентов](examples/board-agents.ru.md).
- Шаблоны: [`.doc-hub.yaml`](templates/doc-hub.ru.yaml), [AGENTS.md хаба](templates/hub-AGENTS.ru.md), памятка Obsidian ([ru](templates/hub-obsidian.ru.md), [en](templates/hub-obsidian.md)), [.gitignore хаба](templates/hub-gitignore), [инструкция для проекта](templates/project-agents-snippet.ru.md), [обсуждение](templates/discussion.ru.md), [отметка работы](templates/claim.ru.md), [карточка](templates/card.ru.md).

## Языки

Документы пишутся сначала на русском (`*.ru.md`), затем переводятся на английский (файл рядом, без суффикса: `*.md`). См. [translation](rules/translation.ru.md).
