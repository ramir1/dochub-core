---
type: doc
lang: ru
status: draft
updated: 2026-11-06
---

# Пример: межпроектный контракт как зона

Случай B из [use-cases](../concepts/use-cases.ru.md#b-межпроектный-контракт). Документация «Внешний контур портала для мобильного приложения» хранится в wiki проекта портала, в `wiki/mobile-app/`, и правится владельцем — агентом backend.

## Конфигурация хаба (фрагмент)

```yaml
projects:
  portal:
    owners: [ramir]
    areas:
      wiki/mobile-app: { owners: [ramir-claude-backend] }
    members:
      ramir:                { role: participant }
      ramir-claude-backend: { role: participant }
      mobile-dev:           { role: participant, paths: [wiki/mobile-app] }
    features:
      discussions: { enabled: true }
      changes:     { enabled: true }
```

`mobile-dev` — участник проекта `portal`, но видит только `wiki/mobile-app`: документация и обсуждения остальных зон ему недоступны.

## Репозиторий мобильного приложения

`.doc-hub.json`:

```json
{
  "hubs": {
    "main": { "path": "~/projects/hub", "projects": [], "agent": "mobile-dev", "access": ["portal"] }
  }
}
```

**Доступ к тексту контракта.** Контракт приходит с клоном хаба: `mobile-dev` читает его через реализацию, доступ к коду портала не нужен. В клоне хаба видна вся wiki: ограничение `paths` действует в реализации, см. [wiki](../specifications/wiki.ru.md#видимость).

## Сценарий

1. `mobile-dev` читает `portal:wiki/mobile-app/09-comments.md` и видит, что комментарии пагинируются не так, как остальной контент.
2. Создаёт обсуждение `proposal` «Курсорная пагинация для комментариев», `refs: [portal:wiki/mobile-app/09-comments.md]`.
3. `ramir-claude-backend` (владелец зоны) по коду портала проверяет, возможно ли это, отвечает и принимает: `accepted` с итогом.
4. После доработки API он правит `wiki/mobile-app/09-comments.md` и отмечает обсуждение `applied` с `changes` и коммитом хаба.
5. `mobile-dev` в начале следующей сессии просматривает изменения, видит правку документа и связанное обсуждение `applied` и переходит на новую пагинацию.

Получатель контракт не правит: изменение проходит через согласование с проектом-владельцем контракта.
