# Протокол запуска

[← Назад к README](../../README.md)

Связанные файлы:
- [Схема state](../../State/state_schema.md)
- [Загрузка контекста](context_loading.md)
- [Обновление state](state_update.md)
- [Service Mode](../service/service_mode.md)
- [Execution Mode](../execution/execution_mode.md)

## Назначение

Протокол задаёт запуск `Concept Builder` в новом чате. Цель запуска: восстановить режим, state, фокус, отложенное действие и следующий безопасный шаг.

## Порядок запуска

1. Определи режим проекта: `Service Mode` или `Execution Mode`.
2. Открой корневой `README.md`.
3. Прочитай верхний state выбранного режима.
4. Загрузи только файлы из `active_protocols`.
5. Собери локальный пакет фокуса по `context_loading.md`.
6. Проверь `pending_user_action` и `next_expected_step`.
7. Обнови state только если запуск меняет состояние.
8. Ответь коротко и покажи `Health marker`.

## Шаблон ответа

```md
Режим: Service Mode | Execution Mode
State загружен: да/нет
Текущий фокус: <system|concept|issue_id|none>
Активная фаза: <phase>
Следующий шаг: <step>
Health marker: mode=<...>; focus=<...>; phase=<...>; persisted=<yes|no>; next=<...>; context_confidence=<high|medium|low>
```

## Pending action имеет приоритет

Если state содержит `pending_user_action`, агент не показывает обычное меню. Он показывает ожидаемое действие, допустимый формат ответа и ближайший безопасный шаг.
