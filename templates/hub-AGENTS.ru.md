# DocHub — инструкции для ИИ-агентов

Это хаб DocHub: единое место документации проектов и координации агентов разных инструментов, сессий и машин.
Копируется в корень хаба как `AGENTS.md`.

## Работа

Агенты в хабе не запускаются: они работают в своих репозиториях.

**Есть команда `doc-hub`** — работай только через неё, файлы вручную не пиши:

- начало сессии: `doc-hub status`;
- обсуждения: `doc-hub discussion …`, входящие: `doc-hub inbox`;
- отметки работы: `doc-hub claim …`, изменения: `doc-hub changes`;
- своя документация: `doc-hub wiki …`, запись только в своей зоне;
- внешняя документация: `doc-hub docs …`, только чтение.

**Команды нет** — работай с файлами сам по скилу `skills/dochub/SKILL.md` из DocHub Core.

## Где что лежит

- `projects/<id>/discussions/`, `changes/`, `claims/` — данные функций.
- `.doc-hub/config.yaml` — проекты, зоны, агенты, роли. Не менять.
- `projects/<id>/wiki/` — своя документация.
- `projects/<id>/docs/<папка>/` — внешняя, только чтение, не в git.

## Спецификации

DocHub Core: `rules/agents`, `specifications/cli-contract`, `specifications/access`, `specifications/features/*`.
<!-- Укажите путь или ссылку на DocHub Core, используемый этим хабом. -->
