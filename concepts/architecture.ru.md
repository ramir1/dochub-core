---
type: doc
lang: ru
status: draft
updated: 2026-11-08
---

# Архитектура

Слои и уровни работы — [levels](levels.ru.md).

## Составные части

Термины — [glossary](glossary.ru.md).

| Часть | Где описано |
|---|---|
| **Core** | [README](../README.ru.md) |
| **Хаб** | [hub-storage](../specifications/hub-storage.ru.md) |
| **Wiki** | [wiki](../specifications/wiki.ru.md) |
| **Docs** и репозитории проектов | [docs-sources](../specifications/docs-sources.ru.md) |
| **Функции** | [features](features.ru.md) |
| **Реализации** | [levels](levels.ru.md), [protocol](../specifications/protocol.ru.md) |
| **Агенты** | [access](../specifications/access.ru.md) |

## Схема

```
  Машина 1                                  Машина 2
  ┌──────────────────────────────┐          ┌──────────────────────────────┐
  │ portal/ (репозиторий)        │          │ mobile/ (репозиторий)        │
  │   docs/                      │          │   .doc-hub.yaml              │
  │   .doc-hub.yaml              │          │                              │
  │ агенты: backend (Claude),    │          │ агент: mobile-dev            │
  │         frontend (Codex)     │          │                              │
  │ хаб/ (клон) ◄ агент/Obsidian │          │ хаб/ (клон) ◄ агент          │
  │   projects/portal/docs/tech/ │          │                              │
  │     ◄─ выгрузка из portal/   │          │                              │
  └──────────────┬───────────────┘          └──────────────┬───────────────┘
                 │ git pull / push                         │ git pull / push
                 ▼                                         ▼
            ┌────────────────── origin хаба ──────────────────┐
            │ .doc-hub/config.yaml  — проекты, агенты, права  │
            │ projects/portal/wiki/ — своя документация       │
            │ projects/portal/discussions/ claims/ changes/   │
            └─────────────────────────────────────────────────┘
  projects/*/docs/ на каждой машине свои и в origin хаба не попадают.
```

## Потоки

- **Wiki** правится в хабе: человек — в Obsidian (на уровнях 1 и 2 — без утилиты или без веб-интерфейса, см. [уровни](levels.ru.md)), агент — через реализацию или по скилу.
- **Docs** правится там, где лежит; явная выгрузка копирует её в хаб, в git хаба она не попадает.
- **Функции:** агент работает в своём репозитории, а файлы пишет в клон хаба, коммитит и отправляет в origin.
- **Синхронизация:** `git pull` перед чтением, `commit` и `push` после записи; столкновения — в [hub-storage](../specifications/hub-storage.ru.md#столкновения-записи).

## Что вне Core

Сервис с API, Web, индексатор и поиск, развёртывание (Docker и т. п.), аутентификация — это устройство реализаций, см. [levels](levels.ru.md).
