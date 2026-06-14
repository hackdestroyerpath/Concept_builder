# Инструкция проекта: Concept Builder Service Mode

[Назад к README](../README.md)

## Назначение

Короткая инструкция проекта для `Service Mode` (сервисного режима). Полные правила живут в рабочих файлах GitHub.

## Связанные файлы

- [README](../README.md)
- [Состояние service](../State/service_state.json)
- [Протокол запуска](../Protocols/common/startup.md)
- [Service Mode](../Protocols/service/service_mode.md)

## Запуск

1. Используй GitHub Connector для чтения и записи.
2. Открой `README.md`.
3. Открой `State/service_state.json`.
4. Открой `Repository/file_index.jsonl` и `Repository/link_graph.md`.
5. Выполни `Protocols/common/startup.md`.
6. Проверь `pending_user_action`; если он не `null`, сначала покажи ожидаемое действие.
7. Загрузи только `active_protocols` из состояния и файлы фокуса из пакета фокуса.

## Рабочий режим

Используй `Protocols/service/service_mode.md` для обслуживания системы. Для входных материалов и registry используй `Protocols/service/input_registry.md`. Для служебных задач используй `Protocols/issue/issue_lifecycle.md` и при необходимости `Protocols/issue/complex_linked.md`.

## Проверка изменения системных файлов

Не меняй системные файлы без утверждённой служебной задачи, утверждённых требований, решения и договора или записанного аварийного исключения. Список затронутых файлов должен быть известен до записи.

## Восстановление

Если хэш состояния неверен, фокус потерян, реестр конфликтует с состоянием или уверенность контекста низкая, останови обычную работу и выполни восстановление по `Protocols/common/focus_packet.md`.

## Сохранение

Перед ответом сохраняй рабочие файлы, реестр, индекс, карту и нужное состояние. `persisted=yes` допустимо только после фактической записи через GitHub Connector.

## Маркер здоровья

```text
mode=service; focus=<focus>; phase=<phase>; persisted=<yes|no>; next=<next>; context_confidence=<high|medium|low>
```
