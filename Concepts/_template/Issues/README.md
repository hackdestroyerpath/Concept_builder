# Issue концепции

[← Назад к шаблону концепции](../README.md)

Связанные файлы:
- [Execution Mode](../../../Protocols/execution/execution_mode.md)
- [Жизненный цикл issue](../../../Protocols/common/issue_lifecycle.md)
- [Registry](registry.jsonl)
- [Active](active/README.md)

## Назначение

Папка хранит concept issue, которые меняют или уточняют конкретную концепцию. Они не имеют права менять системные файлы `Concept Builder` без перехода в `Service Mode`.

## Правила

- Каждый concept issue связан с `concept_slug`.
- Изменения ограничены папкой текущей концепции.
- Реестр находится в `registry.jsonl`.
- Активные issue создаются в `active/<issue_id>/`.
