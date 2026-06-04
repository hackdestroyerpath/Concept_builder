# Загрузка контекста

[← Назад к README](../../README.md)

Связанные файлы:
- [Протокол запуска](startup.md)
- [Обновление state](state_update.md)
- [Схема state](../../State/state_schema.md)
- [Граф ссылок](../../Repository/link_graph.md)

## Назначение

Протокол удерживает агента на минимальном рабочем фокусе. Агент не читает весь репозиторий “для уверенности”; так появляется не уверенность, а контекстный болотный газ.

## Пакет фокуса

Перед содержательной работой агент внутренне фиксирует:

```text
mode: service | execution
current_focus: repository | concept | issue | requirement | solution | output | export
current_entity_id: ...
parent_anchor: ...
focus_stack: верхний уровень -> текущий уровень
active_state_files: ...
active_protocols: ...
allowed_context_files: ...
blocked_context_files: ...
state_revision_loaded: ...
reload_reason: startup | focus_changed | state_changed | dependency_needed | confidence_low | user_requested | not_needed
context_confidence: high | medium | low
next_expected_step: ...
```

## Минимальный набор

Агент читает только:
1. ближайшую точку входа текущего уровня;
2. relevant state текущего режима;
3. active protocol из state;
4. файлы текущей сущности: issue, concept page, requirements, solution или contract;
5. parent summary, если без него теряется смысл.

## Контекст по запросу

Дополнительно можно читать parent issue, linked output, manifest, graph, file index или protocol schema, если это нужно для текущей фазы.

## Запрещённый контекст по умолчанию

Не читать без причины: весь репозиторий, все концепции, все issue folders, закрытые output без dependency link, временные файлы разработки и ТЗ-архив реализации.

## Потеря фокуса

Если агент не может назвать режим, фокус, текущую сущность, parent anchor и следующий шаг, он останавливается и восстанавливается через state.
