# Жизненный цикл issue

[← Назад к README](../../README.md)

Связанные файлы:
- [Обновление state](state_update.md)
- [Загрузка контекста](context_loading.md)
- [Связанные issue](linked_issues.md)
- [Issues](../../Issues/README.md)
- [Inbox](../../Inbox/README.md)

## Назначение

Единый жизненный цикл service issue и concept issue: входные данные, registry, reason, QA, requirements, requalification, plan, solution, contract, output и закрытие.

## Входные данные

Агент отделяет точку входа от вложений. Текст пользователя или явный entry-файл сохраняется как `Inbox/<input_id>/entry.md`; вложения фиксируются в `input_manifest.json`.

## Registry

Service issue пишутся в `Issues/registry.jsonl`. Concept issue пишутся в `Concepts/<concept_slug>/Issues/registry.jsonl`.

Минимальная строка содержит `issue_id`, `scope`, `concept_slug`, `title`, `status`, `type`, `parent_id`, `child_ids`, связи, source files, пути к `reason.md`, `state.json`, `requirements.md`, `solution.md`, `contract.md`, `output/`, timestamps и `next_expected_step`.

## Команды решения

| Команда | Действие |
|---|---|
| `утверждаю всё` | approved для всех proposed issue |
| `утверждаю: ID...` | approved для перечисленных issue |
| `отклоняю: ID...` | rejected и tombstone-policy |
| `обсудить: ID...` | статус discuss |
| `отложить: ID...` | статус deferred |
| `изменить: ID...` | правка полей после обсуждения |
| `добавить: <title + reason>` | новый proposed issue; reason обязателен |
| `фокус: ID` | выбор одного issue |

## QA

QA обязателен, если intent допускает разные решения, нет критериев приёмки, изменение рискованное, затрагивает несколько файлов или есть parent, child, blocker, dependency. QA можно пропустить только для низкорисковой механической задачи; `requirements.md` всё равно обязателен.

## Requirements

`requirements.md` создаётся всегда до solution. Пользователь утверждает, меняет, удаляет или добавляет requirements. Переход к solution без approved requirements запрещён.

## Requalification

После утверждения требований агент проверяет тип задачи: `simple` или `complex`. Если тип меняется, state получает `requalification_proposed`, а агент ждёт решения пользователя.

## Simple issue

Порядок строгий:

```text
requirements approved
→ readiness gate
→ plan.md
→ solution.md
→ contract.md
→ user approval
→ execute
→ output/report.md
→ closure checklist
```

## Closure

Issue закрывается только если requirements, solution и contract approved, обязательные проверки пройдены, `output/report.md` сохранён, state и registry обновлены, а зависимости обработаны.
